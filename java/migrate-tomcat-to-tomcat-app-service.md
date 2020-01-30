---
title: Migrieren von Tomcat-Anwendungen zu Tomcat unter Azure App Service
description: In diesem Leitfaden wird beschrieben, was Sie beachten sollten, wenn Sie eine vorhandene Tomcat-Anwendung für die Ausführung unter Azure App Service mit Tomcat migrieren möchten.
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.openlocfilehash: ce1c54f0f4b28c5c0a2e11f4afc53f1dd59899c5
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288599"
---
# <a name="migrate-tomcat-applications-to-tomcat-on-azure-app-service"></a>Migrieren von Tomcat-Anwendungen zu Tomcat unter Azure App Service

In diesem Leitfaden wird beschrieben, was Sie beachten sollten, wenn Sie eine vorhandene Tomcat-Anwendung für die Ausführung unter Azure App Service mit Tomcat 8.5 oder 9.0 migrieren möchten.

## <a name="before-you-start"></a>Vorbereitung

Falls Sie keine Anforderungen der Migrationsvorbereitung erfüllen können, helfen Ihnen die Informationen in den folgenden relevanten Migrationsleitfäden weiter:

* [Migrieren von Tomcat-Anwendungen zu Containern unter Azure Kubernetes Service](migrate-tomcat-to-containers-on-azure-kubernetes-service.md)
* Migrieren von Tomcat-Anwendungen zu Azure Virtual Machines (in Kürze verfügbar)

## <a name="pre-migration-steps"></a>Schritte zur Migrationsvorbereitung

* [Wechseln zu einer unterstützten Plattform](#switch-to-a-supported-platform)
* [Bestand: Externe Ressourcen](#inventory-external-resources)
* [Bestand: Geheimnisse](#inventory-secrets)
* [Bestand: Dauerhafte Nutzung](#inventory-persistence-usage)
* [Sonderfälle](#special-cases)

### <a name="switch-to-a-supported-platform"></a>Wechseln zu einer unterstützten Plattform

App Service verfügt über bestimmte Versionen von Tomcat für bestimmte Versionen von Java. Migrieren Sie Ihre Anwendung zur Sicherstellung der Kompatibilität zu einer der unterstützten Versionen von Tomcat und Java in der aktuellen Umgebung, bevor Sie mit der Ausführung der restlichen Schritte beginnen. Achten Sie darauf, dass Sie die sich ergebende Konfiguration umfassend testen. Verwenden Sie für diese Tests [Red Hat Enterprise Linux 8](https://portal.azure.com/#create/RedHat.RedHatEnterpriseLinux80-ARM) als Betriebssystem.

#### <a name="java"></a>Java

> [!NOTE]
> Diese Überprüfung ist besonders wichtig, wenn Ihr aktueller Server auf einem nicht unterstützten JDK (z. B. Oracle JDK oder IBM OpenJ9) ausgeführt wird.

Melden Sie sich an Ihrem Produktionsserver an, und führen Sie den folgenden Befehl aus, um Ihre aktuelle Java-Version zu ermitteln:

```bash
java -version
```

Laden Sie zum Ermitteln der aktuellen Version, die von Azure App Service verwendet wird, [Zulu 8](https://www.azul.com/downloads/zulu-community/?&version=java-8-lts&os=&os=linux&architecture=x86-64-bit&package=jdk) (bei Nutzung der Java 8 Runtime) bzw. [Zulu 11](https://www.azul.com/downloads/zulu-community/?&version=java-11-lts&os=&os=linux&architecture=x86-64-bit&package=jdk) (bei Nutzung der Java 11 Runtime) herunter.

#### <a name="tomcat"></a>Tomcat

Melden Sie sich an Ihrem Produktionsserver an, und führen Sie den folgenden Befehl aus, um Ihre aktuelle Tomcat-Version zu ermitteln:

```bash
${CATALINA_HOME}/bin/version.sh
```

Laden Sie [Tomcat 8.5](https://tomcat.apache.org/download-80.cgi#8.5.50) oder [Tomcat 9](https://tomcat.apache.org/download-90.cgi) herunter, um die von Azure App Service verwendete aktuelle Version zu ermitteln. Dies hängt davon ab, welche Version Sie in Azure App Service nutzen möchten.

[!INCLUDE [inventory-external-resources](includes/migration/inventory-external-resources.md)]

[!INCLUDE [inventory-secrets](includes/migration/inventory-secrets.md)]

[!INCLUDE [inventory-persistence-usage](includes/migration/inventory-persistence-usage.md)]

<!-- App-Service-specific addendum to inventory-persistence-usage -->
#### <a name="dynamic-or-internal-content"></a>Dynamischer oder interner Inhalt

Für Dateien, für die von Ihrer Anwendung häufige Schreib- und Lesevorgänge durchgeführt werden (z. B. temporäre Datendateien), oder für statische Dateien, die nur für Ihre Anwendung sichtbar sind, können Sie Azure Storage im App Service-Dateisystem bereitstellen. Weitere Informationen finden Sie unter [Bereitstellen von Inhalt aus Azure Storage in App Service unter Linux](/azure/app-service/containers/how-to-serve-content-from-azure-storage).

### <a name="identify-session-persistence-mechanism"></a>Identifizieren eines Mechanismus für Sitzungspersistenz

Untersuchen Sie die *context.xml*-Dateien in Ihrer Anwendung und der Tomcat-Konfiguration, um den verwendeten Manager für Sitzungspersistenz zu ermitteln. Suchen Sie nach dem `<Manager>`-Element, und notieren Sie sich den Wert des `className`-Attributs.

Die integrierten [PersistentManager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html)-Implementierungen von Tomcat, z. B. [StandardManager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html#Standard_Implementation) oder [FileStore](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html#Nested_Components), sind nicht für die Nutzung mit einer verteilten skalierten Plattform wie App Service konzipiert. Da App Service ggf. einen Lastenausgleich zwischen verschiedenen Instanzen vornimmt und für Instanzen jederzeit einen transparenten Neustart durchführen kann, ist das dauerhafte Speichern eines veränderlichen Zustands in einem Dateisystem nicht zu empfehlen.

Falls das Erzielen von Sitzungspersistenz erforderlich ist, müssen Sie eine andere `PersistentManager`-Implementierung verwenden, bei der in einen externen Datenspeicher geschrieben wird, z. B. Pivotal-Sitzungs-Manager mit Redis Cache. Weitere Informationen finden Sie unter [Verwenden von Redis als Sitzungscache mit Tomcat](/azure/app-service/containers/configure-language-java#use-redis-as-a-session-cache-with-tomcat).

### <a name="special-cases"></a>Spezialfälle

Für bestimmte Produktionsszenarien sind unter Umständen zusätzliche Änderungen erforderlich, oder es gelten zusätzliche Einschränkungen. Szenarien dieser Art treten zwar meist nicht sehr häufig auf, aber Sie sollten trotzdem sicherstellen, dass sie für Ihre Anwendung entweder nicht zutreffen oder korrekt behoben werden.

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>Ermitteln, ob für die Anwendung geplante Aufträge benötigt werden

Geplante Aufträge, z. B. Quartz Scheduler-Aufgaben oder cron-Aufträge, können mit App Service nicht verwendet werden. App Service hindert Sie nicht an der Bereitstellung einer Anwendung, die intern geplante Aufgaben enthält. Wenn Ihre Anwendung aber horizontal hochskaliert wird, wird derselbe geplante Auftrag unter Umständen mehrmals pro geplantem Zeitraum ausgeführt. Diese Situation kann unerwünschte Konsequenzen haben.

Inventarisieren Sie alle geplanten Aufträge innerhalb oder außerhalb des Anwendungsservers.

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>Ermitteln, ob Ihre Anwendung betriebssystemspezifischen Code enthält

Wenn Ihre Anwendung Code mit Abhängigkeiten vom Hostbetriebssystem enthält, müssen Sie ihn umgestalten, um diese Abhängigkeiten zu beseitigen. Beispielsweise müssen Sie ggf. alle Vorkommen von `/` oder `\` in Dateisystempfaden durch [`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) oder [`Paths.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-) ersetzen.

#### <a name="determine-whether-tomcat-clustering-is-used"></a>Ermitteln, ob das Tomcat-Clustering genutzt wird

Das [Tomcat-Clustering](https://tomcat.apache.org/tomcat-8.5-doc/cluster-howto.html) wird für Azure App Service nicht unterstützt. Stattdessen können Sie die Skalierung und den Lastenausgleich mit Azure App Service und ohne Tomcat-spezifische Funktionen konfigurieren und verwalten. Sie können den Sitzungszustand an einem alternativen Speicherort speichern, um ihn für Replikate übergreifend verfügbar zu machen. Weitere Informationen finden Sie unter [Identifizieren eines Mechanismus für Sitzungspersistenz](#identify-session-persistence-mechanism).

Suchen Sie für die Ermittlung, ob für Ihre Anwendung das Clustering verwendet wird, in der *server.xml*-Datei in den Elementen `<Host>` oder `<Engine>` nach dem `<Cluster>`-Element.

#### <a name="identify-all-outside-processesdaemons-running-on-the-production-servers"></a>Ermitteln aller externen Prozesse/Daemons, die auf den Produktionsservern ausgeführt werden

Sie müssen die Migration zu einem anderen Ort durchführen oder alle Prozesse beseitigen, die außerhalb des Anwendungsservers ausgeführt werden, z. B. die Überwachung von Daemons.

#### <a name="determine-whether-non-http-connectors-are-used"></a>Ermitteln, ob Connectors ohne HTTP genutzt werden

App Service unterstützt nur jeweils einen HTTP-Connector. Wenn für Ihre Anwendung zusätzliche Connectors erforderlich sind, z. B. der AJP-Connector, sollten Sie App Service nicht verwenden.

Suchen Sie in der Datei *server.xml* Ihrer Tomcat-Konfiguration nach `<Connector>`-Elementen, um die von Ihrer Anwendung genutzten HTTP-Connectors zu ermitteln.

#### <a name="determine-whether-memoryrealm-is-used"></a>Ermitteln, ob MemoryRealm genutzt wird

Für [MemoryRealm](https://tomcat.apache.org/tomcat-8.5-doc/api/org/apache/catalina/realm/MemoryRealm.html) wird eine persistente XML-Datei benötigt. Unter Azure App Service müssen Sie diese Datei in das Verzeichnis */home* bzw. ein zugehöriges Unterverzeichnis oder in bereitgestellten Speicher hochladen. Hierbei müssen Sie den Parameter `pathName` entsprechend ändern.

Gehen Sie wie folgt vor, um zu ermitteln, ob `MemoryRealm` derzeit verwendet wird: Untersuchen Sie Ihre *server.xml*- und *context.xml*-Dateien, und suchen Sie nach `<Realm>`-Elementen, für die das `className`-Attribut auf `org.apache.catalina.realm.MemoryRealm` festgelegt ist.

#### <a name="determine-whether-ssl-session-tracking-is-used"></a>Ermitteln, ob die SSL-Sitzungsverfolgung genutzt wird

App Service führt die Sitzungsabladung außerhalb der Tomcat-Runtime durch. Aus diesem Grund können Sie die [SSL-Sitzungsverfolgung](https://tomcat.apache.org/tomcat-8.5-doc/servletapi/javax/servlet/SessionTrackingMode.html#SSL) nicht verwenden. Verwenden Sie stattdessen einen anderen Modus für die Sitzungsverfolgung (`COOKIE` oder `URL`). Vermeiden Sie die Verwendung von App Service, wenn Sie die SSL-Sitzungsverfolgung benötigen.

#### <a name="determine-whether-accesslogvalve-is-used"></a>Ermitteln, ob AccessLogValve genutzt wird

Bei Verwendung von [AccessLogValve](https://tomcat.apache.org/tomcat-8.5-doc/api/org/apache/catalina/valves/AccessLogValve.html) sollten Sie den Parameter `directory` auf `/home/LogFiles` oder ein zugehöriges Unterverzeichnis festlegen.

## <a name="migration"></a>Migration

### <a name="parametrize-the-configuration"></a>Parametrisieren der Konfiguration

Bei der Migrationsvorbereitung haben Sie in *server.xml*- und *context.xml*-Dateien ggf. Geheimnisse und externe Abhängigkeiten identifiziert, z. B. Datenquellen. Ersetzen Sie für alle auf diese Weise identifizierten Elemente den Benutzernamen, das Kennwort, die Verbindungszeichenfolge und die URL durch eine Umgebungsvariable.

Angenommen, die *context.xml*-Datei enthält das folgende Element:

```xml
<Resource
    name="jdbc/dbconnection"
    type="javax.sql.DataSource"
    url="jdbc:postgresql://postgresdb.contoso.com/wickedsecret?ssl=true"
    driverClassName="org.postgresql.Driver"
    username="postgres"
    password="t00secure2gue$$"
/>
```

In diesem Fall können Sie die Änderungen vornehmen, die im folgenden Beispiel angegeben sind:

```xml
<Resource
    name="jdbc/dbconnection"
    type="javax.sql.DataSource"
    url="${postgresdb.connectionString}"
    driverClassName="org.postgresql.Driver"
    username="${postgresdb.username}"
    password="${postgresdb.password}"
/>
```

### <a name="provision-an-app-service-plan"></a>Bereitstellen eines App Service-Plans

Wählen Sie unter [App Service – Preise](https://azure.microsoft.com/pricing/details/app-service/linux/) in der Liste mit den verfügbaren App Service-Plänen die Option aus, bei der die Spezifikationen die Vorgaben der aktuellen Produktionshardware erfüllen bzw. übererfüllen.

> [!NOTE]
> Wenn Sie die Ausführung von Staging- bzw. Canarybereitstellungen planen oder Bereitstellungsslots nutzen möchten, muss der App Service-Plan diese zusätzliche Kapazität enthalten. Wir empfehlen Ihnen die Verwendung eines Premium-Plans (oder höher) für Java-Anwendungen. Weitere Informationen finden Sie unter [Einrichten von Stagingumgebungen in Azure App Service](/azure/app-service/deploy-staging-slots).

Erstellen Sie anschließend den App Service Plan. Weitere Informationen finden Sie unter [Verwalten eines App Service-Plans in Azure](/azure/app-service/app-service-plan-manage).

### <a name="create-and-deploy-web-apps"></a>Erstellen und Bereitstellen von Web-Apps

Sie müssen für jede WAR-Datei, die auf Ihrem Tomcat-Server bereitgestellt wird, unter Ihrem App Service-Plan eine Web-App erstellen.

> [!NOTE]
> Es ist zwar möglich, für eine Web-App mehrere WAR-Dateien bereitzustellen, aber dies entspricht nicht der empfohlenen Vorgehensweise. Die Bereitstellung mehrerer WAR-Dateien für eine Web-App verhindert, dass jede Anwendung gemäß ihren jeweiligen Nutzungsanforderungen skaliert werden kann. Darüber hinaus wird hierdurch die Komplexität nachfolgender Bereitstellungspipelines erhöht. Wenn mehrere Anwendungen unter einer einzelnen URL verfügbar sein sollen, sollten Sie den Einsatz einer Routinglösung, z. B. [Azure Application Gateway](/azure/application-gateway/), erwägen.

#### <a name="maven-applications"></a>Maven-Anwendungen

Wenn Ihre Anwendung über eine Maven-POM-Datei erstellt wurde, sollten Sie [das Web-App-Plug-In für Maven verwenden](/azure/app-service/containers/quickstart-java#configure-the-maven-plugin), um die Web-App zu erstellen und Ihre Anwendung bereitzustellen.

#### <a name="non-maven-applications"></a>Nicht auf Maven basierende Anwendungen

Falls Sie das Maven-Plug-In nicht verwenden können, müssen Sie die Web-App über andere Mechanismen bereitstellen, z. B.:

* [Azure portal](https://portal.azure.com/#create/Microsoft.WebSite)
* [Azure-Befehlszeilenschnittstelle](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)
* [Azure PowerShell](/powershell/module/az.websites/new-azwebapp)

Verwenden Sie nach der Erstellung der Web-App einen der [verfügbaren Bereitstellungsmechanismen](/azure/app-service/deploy-zip), um Ihre Anwendung bereitzustellen.

### <a name="migrate-jvm-runtime-options"></a>Migrieren von JVM-Runtimeoptionen

Wenn für Ihre Anwendung bestimmte Runtimeoptionen benötigt werden, sollten Sie [zum Angeben den am besten geeigneten Mechanismus verwenden](/azure/app-service/containers/configure-language-java#set-java-runtime-options).

### <a name="populate-secrets"></a>Einfügen von Geheimnissen

Verwenden Sie die Anwendungseinstellungen, um die spezifischen Geheimnisse für Ihre Anwendung zu speichern. Falls Sie die gleichen Geheimnisse für mehrere Anwendungen nutzen möchten oder differenzierte Zugriffsrichtlinien und Überwachungsfunktionen benötigen, sollten Sie stattdessen [Azure Key Vault nutzen](/azure/app-service/containers/configure-language-java#use-keyvault-references).

### <a name="configure-custom-domain-and-ssl"></a>Konfigurieren der benutzerdefinierten Domäne und der Nutzung von SSL

Wenn Ihre Anwendung in einer benutzerdefinierten Domäne sichtbar ist, müssen Sie Ihre [Webanwendung zuordnen](/azure/app-service/app-service-web-tutorial-custom-domain).

Anschließend müssen Sie [das SSL-Zertifikat für diese Domäne an Ihre App Service-Web-App binden](/azure/app-service/app-service-web-tutorial-custom-ssl).

### <a name="migrate-data-sources-libraries-and-jndi-resources"></a>Migrieren von Datenquellen, Bibliotheken und JNDI-Ressourcen

Führen Sie [diese Schritte zum Migrieren von Datenquellen](/azure/app-service/containers/configure-language-java#tomcat) aus.

Migrieren Sie alle zusätzlichen Klassenpfadabhängigkeiten auf Serverebene, indem Sie [die gleichen Schritte wie für die JAR-Dateien der Datenquelle](/azure/app-service/containers/configure-language-java#finalize-configuration) ausführen.

Migrieren Sie alle zusätzlichen [freigegebenen JDNI-Ressourcen auf Serverebene](/azure/app-service/containers/configure-language-java#shared-server-level-resources).

> [!NOTE]
> Wenn Sie sich an die empfohlene Architektur mit einer WAR-Datei pro Web-App halten, sollten Sie erwägen, Klassenpfadbibliotheken auf Serverebene und JNDI-Ressourcen zu Ihrer Anwendung zu migrieren. Bereiche wie Governance und Change Management für Komponenten werden so erheblich vereinfacht.

### <a name="migrate-remaining-configuration"></a>Migrieren der restlichen Konfiguration

Wenn Sie den vorherigen Abschnitt abgeschlossen haben, sollten Sie unter */home/tomcat/conf* über Ihre anpassbare Serverkonfiguration verfügen.

Schließen Sie die Migration ab, indem Sie alle zusätzlichen Konfigurationen kopieren (z. B. [realms](https://tomcat.apache.org/tomcat-8.5-doc/config/realm.html), [JASPIC](https://tomcat.apache.org/tomcat-8.5-doc/config/jaspic.html)).

### <a name="migrate-scheduled-jobs"></a>Migrieren von geplanten Aufträgen

Erwägen Sie für die Ausführung von geplanten Aufträgen in Azure die Verwendung von [Azure Functions mit einem Zeitgebertrigger](/azure/azure-functions/functions-bindings-timer). Hierbei ist es nicht erforderlich, den eigentlichen Auftragscode in eine Funktion zu migrieren. Für die Funktion kann einfach eine URL in Ihrer Anwendung aufgerufen werden, um den Auftrag auszulösen.

Alternativ können Sie eine [Logik-App](/azure/logic-apps/logic-apps-overview) mit einem [Wiederholungstrigger](/azure/logic-apps/tutorial-build-schedule-recurring-logic-app-workflow#add-the-recurrence-trigger) erstellen, um die URL aufzurufen, ohne außerhalb Ihrer Anwendung Code schreiben zu müssen.

> [!NOTE]
> Zur Verhinderung einer missbräuchlichen Nutzung müssen Sie ggf. sicherstellen, dass für den Endpunkt zum Aufrufen des Auftrags Anmeldeinformationen benötigt werden. In diesem Fall müssen die Anmeldeinformationen von der Triggerfunktion bereitgestellt werden.

### <a name="restart-and-smoke-test"></a>Neustarten und Durchführen des Buildüberprüfungstests

Abschließend müssen Sie Ihre Web-App neu starten, um alle Konfigurationsänderungen anzuwenden. Vergewissern Sie sich nach Abschluss des Neustarts, dass Ihre Anwendung korrekt ausgeführt wird.

## <a name="post-migration-steps"></a>Schritte nach der Migration

Nachdem Sie Ihre Anwendung nun zu Azure App Service migriert haben, sollten Sie überprüfen, ob sie wie erwartet funktioniert. Als Nächstes können Sie sich dann über unsere Empfehlungen informieren, mit denen Sie Ihre Anwendung cloudnativer gestalten können.

### <a name="recommendations"></a>Empfehlungen

1. Wenn Sie sich für die Verwendung des Verzeichnisses */home* zum Speichern der Dateien entschieden haben, können Sie erwägen, dafür die [Umstellung auf Azure Storage](/azure/app-service/containers/how-to-serve-content-from-azure-storage) durchzuführen.

1. Falls Sie über Konfigurationsdaten im Verzeichnis */home* mit Verbindungszeichenfolgen, SSL-Schlüsseln und anderen Geheimnisinformationen verfügen, können Sie, falls möglich, eine Kombination aus [Azure Key Vault](/azure/app-service/app-service-key-vault-references) und [Parametereinfügung mit Anwendungseinstellungen](/azure/app-service/configure-common#configure-app-settings) verwenden.

1. Erwägen Sie die [Verwendung von Bereitstellungsslots](/azure/app-service/deploy-staging-slots), um zuverlässige Bereitstellungen ohne jegliche Ausfallzeiten zu erzielen.

1. Entwerfen und implementieren Sie eine DevOps-Strategie. Sie können [Bereitstellungen automatisieren und mit Azure Pipelines testen](/azure/devops/pipelines/ecosystems/java-webapp), um die Zuverlässigkeit sicherzustellen, während gleichzeitig die Entwicklungsgeschwindigkeit erhöht wird. Bei Verwendung von Bereitstellungsslots können Sie nicht nur die [Bereitstellung für einen Slot automatisieren](/azure/devops/pipelines/targets/webapp?view=azure-devops&tabs=yaml#deploy-to-a-slot), sondern auch den nachfolgenden Slotaustausch.

1. Entwerfen und implementieren Sie eine Strategie für Geschäftskontinuität und Notfallwiederherstellung. Bei unternehmenskritischen Anwendungen sollten Sie erwägen, eine [Bereitstellungsarchitektur mit mehreren Regionen](/azure/architecture/reference-architectures/app-service-web-app/multi-region) zu verwenden.