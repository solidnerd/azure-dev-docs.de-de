---
title: Verwenden der Spring Data-JPA mit Azure SQL-Datenbank
description: Erfahren Sie, wie Sie die Spring Data-JPA (Java-Persistenz-API) mit einer Azure SQL-Datenbank verwenden.
services: sql-database
documentationcenter: java
ms.date: 12/19/2018
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 1694a342089e5bcb6cfd669ef723292fe48b3b55
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "81668466"
---
# <a name="how-to-use-spring-data-jpa-with-azure-sql-database"></a>Verwenden der Spring Data-JPA mit Azure SQL-Datenbank

## <a name="overview"></a>Übersicht

In diesem Artikel wird die Erstellung einer Beispielanwendung veranschaulicht, die [Spring Data] verwendet, um Informationen in einer [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) mithilfe der [Java-Persistenz-API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm) zu speichern und abzurufen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung der Schritte in diesem Artikel müssen folgende Voraussetzungen erfüllt sein:

* Ein Azure-Abonnement – wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile] anwenden oder sich für ein [Kostenloses Azure-Konto] registrieren
* Ein unterstütztes Java Development Kit (JDK). Weitere Informationen zu den für die Entwicklung in Azure verfügbaren JDKs finden Sie unter <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), Version 3.0 oder höher
* [cURL](https://curl.haxx.se/) oder ein ähnliches HTTP-Hilfsprogramm zum Testen der Funktionalität
* Einen [Git-Client](https://git-scm.com/downloads)

## <a name="create-an-azure-sql-database"></a>Erstellen einer Azure-SQL-Datenbank

### <a name="create-a-sql-database-server-using-the-azure-portal"></a>Erstellen eines SQL-Datenbank-Servers im Azure-Portal

> [!NOTE]
> 
> Ausführlichere Informationen zum Erstellen von Azure SQL-Datenbanken finden Sie unter [Erstellen einer Azure SQL-Datenbank im Azure-Portal](/azure/sql-database/sql-database-get-started-portal).

1. Navigieren Sie zum Azure-Portal unter <https://portal.azure.com/>, und melden Sie sich an.

1. Klicken Sie auf **+ Ressource erstellen**, auf **Datenbanken** und dann auf **SQL-Datenbank**.

   ![Erstellen einer SQL-Datenbank][SQL01]

1. Geben Sie die folgenden Informationen an:

   - **Datenbankname**: Wählen Sie einen eindeutigen Namen für Ihre SQL-Datenbank aus. Die Datenbank wird auf dem SQL-Server erstellt, den Sie später angeben.
   - **Abonnement**: Geben Sie das Azure-Abonnement an, das Sie verwenden möchten.
   - **Ressourcengruppe**: Erstellen Sie eine neue Ressourcengruppe, oder wählen Sie eine vorhandene Ressourcengruppe aus.
   - **Quelle auswählen**: Wählen Sie für dieses Tutorial `Blank database` aus, um eine neue Datenbank zu erstellen.

   ![Festlegen der Eigenschaften der SQL-Datenbank][SQL02]
   
1. Klicken Sie auf **Server** und dann auf **Neuen Server erstellen**, und geben Sie die folgenden Informationen an:

   - **Servername**: Wählen Sie einen eindeutigen Namen für Ihren SQL-Server aus. Dieser Name wird zum Erstellen eines vollqualifizierten Domänennamens (z. B. *wingtiptoyssql.database.windows.net*) verwendet.
   - **Serveradministratoranmeldung**: Geben Sie den Namen des Datenbankadministrators an.
   - **Kennwort** und **Kennwort bestätigen**: Geben Sie das Kennwort für den Datenbankadministrator an.
   - **Standort**: Geben Sie die nächstgelegene geografische Region für Ihre Datenbank an.

1. Wenn Sie alle oben genannten Informationen eingegeben haben, klicken Sie auf **OK**.

1. Klicken Sie auf **Überprüfen und erstellen**.

### <a name="configure-a-firewall-rule-for-your-sql-server-using-the-azure-portal"></a>Konfigurieren einer Firewallregel für den SQL-Server im Azure-Portal

Nach der Erstellung der SQL-Datenbank und des SQL-Servers können Sie Sicherheitseinstellungen konfigurieren.

1. Navigieren Sie zum Azure-Portal unter <https://portal.azure.com/>, und melden Sie sich an.

1. Klicken Sie auf **Alle Ressourcen** und anschließend auf den SQL-Server, den Sie gerade erstellt haben.

1. Klicken Sie im Abschnitt **Übersicht** auf **Firewalleinstellungen anzeigen**.

   ![Firewalleinstellungen anzeigen][SQL06]

1. Erstellen Sie im Abschnitt **Firewalls und virtuelle Netzwerke** eine neue Regel, indem Sie einen eindeutigen Namen für die Regel angeben, den Bereich der IP-Adressen eingeben, die Zugriff auf Ihre Datenbank benötigen, und anschließend auf **Speichern** klicken. (In dieser Übung wird die IP-Adresse Ihres Entwicklungscomputers verwendet, d. h. des Clients.  Sie können sie sowohl für **Start-IP-Adresse** als auch für **End-IP-Adresse** verwenden.)

   ![Konfigurieren der Firewalleinstellungen][SQL07]

### <a name="retrieve-the-connection-string-for-your-sql-server-using-the-azure-portal"></a>Abrufen der Verbindungszeichenfolge für den SQL-Server im Azure-Portal

1. Navigieren Sie zum Azure-Portal unter <https://portal.azure.com/>, und melden Sie sich an.

1. Klicken Sie auf **Alle Ressourcen** und anschließend auf die SQL-Datenbank, die Sie gerade erstellt haben.

1. Klicken Sie auf **Verbindungszeichenfolgen**.


   ![Auswählen der SQL-Datenbank][SQL08]

1. Klicken Sie auf **JDBC**, und kopieren Sie den Wert im Textfeld „JDBC“.

   ![Abrufen der JDBC-Verbindungszeichenfolge][SQL09]

## <a name="configure-the-sample-application"></a>Konfigurieren der Beispielanwendung

1. Öffnen Sie eine Befehlsshell, und klonen Sie das Beispielprojekt wie im folgenden Beispiel gezeigt mit einem Git-Befehl:

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. Suchen Sie im Verzeichnis *resources* des Beispielprojekts nach der Datei *application.properties*, oder erstellen Sie diese Datei, wenn sie noch nicht vorhanden ist.

1. Öffnen Sie die Datei *application.properties* in einem Text-Editor, und fügen Sie die folgenden Zeilen in der Datei hinzu, bzw. konfigurieren Sie diese Zeilen. Ersetzen Sie dabei die Beispielwerte durch die entsprechenden Werte, die Sie zuvor festgelegt haben:

   ```yaml
   spring.datasource.url=jdbc:sqlserver://wingtiptoyssql.database.windows.net:1433;database=wingtiptoys;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
   spring.datasource.username=wingtiptoysuser@wingtiptoyssql
   spring.datasource.password=********
    ```
   Hierbei gilt:

   | Parameter | BESCHREIBUNG |
   |---|---|
   | `spring.datasource.url` | Eine bearbeitete Version der SQL-JDBC-Zeichenfolge, die Sie weiter oben in diesem Artikel abgerufen haben |
   | `spring.datasource.username` | Der SQL-Administratorname, den Sie weiter oben in diesem Artikel festgelegt haben, mit angefügtem gekürzten Servernamen |
   | `spring.datasource.password` | Das SQL-Administratorkennwort, das Sie weiter oben in diesem Artikel festgelegt haben |

1. Speichern und schließen Sie die Datei *application.properties*.

> [!NOTE]
> Die erste Eigenschaft der Datei *application.properties* lautet `spring.jpa.hibernate.ddl-auto=create`. Dabei handelt es sich um eine Hibernate-Eigenschaft, die das Datenbankschema beim Anwendungsstart automatisch löscht und neu erstellt.
> Diese Konfiguration ist bei der Entwicklung und beim Testen nützlich, sollte aber nicht in der Produktion verwendet werden.

## <a name="package-and-test-the-sample-application"></a>Verpacken und Testen der Beispielanwendung

1. Erstellen Sie die Beispielanwendung mit Maven. Beispiel:

   ```shell
   mvn clean package -P sql
   ```

1. Starten Sie die Beispielanwendung. Beispiel:

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. Erstellen Sie wie in den folgenden Beispielen dargestellt über eine Eingabeaufforderung mit `curl` neue Datensätze:

   ```shell
   curl -s -d "{\"name\":\"dog\",\"species\":\"canine\"}" -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d "{\"name\":\"cat\",\"species\":\"feline\"}" -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   Ihre Anwendung sollte Werte zurückgeben, die dem folgenden Beispiel ähneln:

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. Rufen Sie wie im folgenden Beispiel dargestellt über eine Eingabeaufforderung mit `curl` alle vorhandenen Datensätze ab:

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   Ihre Anwendung sollte Werte zurückgeben, die dem folgenden Beispiel ähneln:

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie eine Java-Beispielanwendung erstellt, die Spring Data verwendet, um Informationen in einer Azure SQL-Datenbank mithilfe der Java-Persistenz-API (JPA) zu speichern und abzurufen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Spring und Azure finden Sie im Dokumentationscenter zu Spring in Azure.

> [!div class="nextstepaction"]
> [Spring Framework in Azure](/azure/developer/java/spring-framework)

### <a name="additional-resources"></a>Weitere Ressourcen

Weitere Informationen zur Verwendung von Azure mit Java finden Sie unter [Azure für Java-Entwickler] und [Working with Azure DevOps and Java] (Arbeiten mit Azure DevOps und Java).

<!-- URL List -->

[Azure für Java-Entwickler]: /azure/developer/java/
[kostenloses Azure-Konto]: https://azure.microsoft.com/pricing/free-trial/
[Working with Azure DevOps and Java]: /azure/devops/ (Arbeiten mit Azure DevOps und Java)
[MSDN-Abonnentenvorteile]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SQL01]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-01.png
[SQL02]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-02.png
[SQL03]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-03.png
[SQL04]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-04.png
[SQL05]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-05.png
[SQL06]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-06.png
[SQL07]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-07.png
[SQL08]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-08.png
[SQL09]: media/configure-spring-data-jpa-with-azure-sql-server/create-azure-sql-09.png
