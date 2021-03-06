---
title: Verwenden von Spring Data-JDBC mit Azure PostgreSQL
description: Erfahren Sie, wie Sie Spring Data-JDBC (Java Database Connectivity) mit einer Azure PostgreSQL-Datenbank verwenden.
services: postgresql
documentationcenter: java
ms.date: 12/26/2019
ms.service: postgresql
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: f259ac4b169f3533c0f5eabd179dd79cc45b708b
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "81669626"
---
# <a name="how-to-use-spring-data-jdbc-with-azure-postgresql"></a>Verwenden von Spring Data-JDBC mit Azure PostgreSQL

In diesem Artikel wird die Erstellung einer Beispielanwendung veranschaulicht, die [Spring Data] verwendet, um Informationen in einer Azure [PostgreSQL](https://www.postgresql.org/)-Datenbank mithilfe von [Java Database Connectivity (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) zu speichern und abzurufen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung der Schritte in diesem Artikel müssen folgende Voraussetzungen erfüllt sein:

* Ein Azure-Abonnement – wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile] anwenden oder sich für ein [Kostenloses Azure-Konto] registrieren
* Ein unterstütztes Java Development Kit (JDK). Weitere Informationen zu den für die Entwicklung in Azure verfügbaren JDKs finden Sie unter <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), Version 3.0 oder höher
* [cURL](https://curl.haxx.se/) oder ein ähnliches HTTP-Hilfsprogramm zum Testen der Funktionalität
* Das Befehlszeilenprogramm [psql](https://www.postgresql.org/docs/current/app-psql.html)
* Einen [Git-Client](https://git-scm.com/downloads)

## <a name="create-a-postgresql-database-for-azure"></a>Erstellen einer PostgreSQL-Datenbank für Azure

### <a name="create-a-postgresql-database-server-using-the-azure-portal"></a>Erstellen eines PostgreSQL-Datenbankservers im Azure-Portal

> [!NOTE]
> 
> Ausführlichere Informationen zum Erstellen von PostgreSQL-Datenbanken finden Sie unter [Erstellen eines Azure Database for PostgreSQL-Servers im Azure-Portal](/azure/postgresql/quickstart-create-server-database-portal).

1. Navigieren Sie zum Azure-Portal unter <https://portal.azure.com/>, und melden Sie sich an.

1. Klicken Sie auf **+ Ressource erstellen**, auf **Datenbanken** und dann auf **Azure Database for PostgreSQL**.

   ![Erstellen einer PostgreSQL-Datenbank][POSTGRESQL01]

1. Geben Sie Folgendes ein:

   - **Ressourcengruppe**: Erstellen Sie eine neue Ressourcengruppe, oder wählen Sie eine vorhandene Ressourcengruppe aus.
   - **Abonnement**: Geben Sie das Azure-Abonnement an, das Sie verwenden möchten.
   - **Servername**: Wählen Sie einen eindeutigen Namen für Ihren PostgreSQL-Server aus. Dieser Name wird zum Erstellen eines vollqualifizierten Domänennamens (z. B. *wingtiptoyspostgresql.postgres.database.azure.com*) verwendet.
   - **Quelle auswählen**: Wählen Sie für dieses Tutorial `None` aus, um eine neue Datenbank zu erstellen.
   - **Serveradministratoranmeldung**: Geben Sie den Namen des Datenbankadministrators an.
   - **Kennwort** und **Kennwort bestätigen**: Geben Sie das Kennwort für den Datenbankadministrator an.
   - **Standort**: Geben Sie die nächstgelegene geografische Region für Ihre Datenbank an.
   - **Version**: Geben Sie aktuelle Datenbankversion an.

   ![Festlegen der Eigenschaften der PostgreSQL-Datenbank][POSTGRESQL02]

1. Nachdem Sie alle oben genannten Informationen eingegeben haben, klicken Sie auf **Bewerten und erstellen**.

1. Überprüfen Sie die Angaben, und klicken Sie auf **Erstellen**.

### <a name="configure-a-firewall-rule-for-your-postgresql-database-server-using-the-azure-portal"></a>Konfigurieren einer Firewallregel für den PostgreSQL-Datenbankserver im Azure-Portal

1. Navigieren Sie zum Azure-Portal unter <https://portal.azure.com/>, und melden Sie sich an.

1. Klicken Sie auf **Alle Ressourcen** und anschließend auf die PostgreSQL-Datenbank, die Sie gerade erstellt haben.

1. Klicken Sie auf **Verbindungssicherheit**, und erstellen Sie im Abschnitt **Firewallregeln** eine neue Regel, indem Sie einen eindeutigen Namen für die Regel angeben, den Bereich der IP-Adressen eingeben, die Zugriff auf Ihre Datenbank benötigen, und anschließend auf **Speichern** klicken. (In dieser Übung wird die IP-Adresse Ihres Entwicklungscomputers verwendet, d. h. des Clients.  Sie können sie sowohl für **Start-IP-Adresse** als auch für **End-IP-Adresse** verwenden.)

   ![Konfigurieren der Verbindungssicherheit][POSTGRESQL03]

### <a name="retrieve-the-connection-string-for-your-postgresql-server-using-the-azure-portal"></a>Abrufen der Verbindungszeichenfolge für den PostgreSQL-Server im Azure-Portal

1. Navigieren Sie zum Azure-Portal unter <https://portal.azure.com/>, und melden Sie sich an.

1. Klicken Sie auf **Alle Ressourcen** und anschließend auf die PostgreSQL-Datenbank, die Sie gerade erstellt haben.

1. Klicken Sie auf **Verbindungszeichenfolgen**, und kopieren Sie den Wert im Textfeld **JDBC**.

   ![Abrufen der JDBC-Verbindungszeichenfolge][POSTGRESQL05]

### <a name="create-postgresql-database-using-the-psql-command-line-utility"></a>Erstellen der PostgreSQL-Datenbank mit dem Befehlszeilenprogramm `psql`

1. Öffnen Sie eine Befehlsshell, und stellen Sie wie im folgenden Beispiel gezeigt eine Verbindung mit Ihrem PostgreSQL-Server her, indem Sie einen `psql`-Befehl eingeben:

   ```shell
   psql --host=wingtiptoyspostgresql.postgres.database.azure.com --port=5432 --username=wingtiptoysuser@wingtiptoyspostgresql --dbname=postgres
   ```
   Hierbei gilt:

   | Parameter | BESCHREIBUNG |
   |---|---|
   | `host` | Der vollqualifizierte PostgreSQL-Servername, den Sie weiter oben in diesem Artikel festgelegt haben |
   | `host` | Der PostgreSQL-Serverport (Standardwert: `5432`) |
   | `username` | Der PostgreSQL-Administratorname, den Sie weiter oben in diesem Artikel festgelegt haben, mit angefügtem gekürzten Servernamen |
   | `dbname` | Gibt an, dass Sie vorerst die `postgres`-Standarddatenbank verwenden möchten |

   Ihr PostgreSQL-Server sollte mit einer Anzeige antworten, die dem folgenden Beispiel ähnelt:

   ```shell
   psql (9.3.24, server 10.5)
   SSL connection (cipher: ECDHE-RSA-AES256-SHA384, bits: 256)
   Type "help" for help.
   
   postgres=>
   ```
   > Hinweis: Wenn Sie eine Fehlermeldung erhalten, dass der Server diese IP-Adresse nicht erkennt, wird die von Ihrem Client verwendete IP-Adresse in der Fehlermeldung angezeigt.  Gehen Sie zurück, und weisen Sie sie wie zuvor beschrieben zu: *Konfigurieren einer Firewallregel für Ihren Server im Azure-Portal*.

1. Erstellen Sie eine Datenbank mit dem Namen *mypgsqldb*, indem Sie wie im folgenden Beispiel gezeigt einen `psql`-Befehl eingeben:

   ```SQL
   CREATE DATABASE mypgsqldb;
   ```

   Ihr PostgreSQL-Server sollte mit einer Anzeige antworten, die dem folgenden Beispiel ähnelt:

   ```shell
   CREATE DATABASE
   ```

1. OPTIONAL: Sie können überprüfen, ob die Datenbank erstellt wurde, indem Sie `\l` im Befehlszeilenprogramm `psql` eingeben. Ihr PostgreSQL-Server sollte mit einer Ausgabe antworten, die dem folgenden Beispiel ähnelt:

   ```shell
                   List of databases
          Name        |      Owner      | Encoding
   -------------------+-----------------+----------
    azure_maintenance | azure_superuser | UTF8
    azure_sys         | azure_superuser | UTF8
    mypgsqldb         | wingtiptoysuser | UTF8
    postgres          | azure_superuser | UTF8
    template0         | azure_superuser | UTF8
    template1         | azure_superuser | UTF8
   (6 rows)
   ```

1. Geben Sie `\q` ein, um das Befehlszeilenprogramm `psql` zu beenden.

## <a name="configure-the-sample-application"></a>Konfigurieren der Beispielanwendung

1. Öffnen Sie eine Befehlsshell, und klonen Sie das Beispielprojekt wie im folgenden Beispiel gezeigt mit einem Git-Befehl:

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. Suchen Sie im Verzeichnis *resources* des Beispielprojekts nach der Datei *application.properties*, oder erstellen Sie diese Datei, wenn sie noch nicht vorhanden ist.

1. Öffnen Sie die Datei *application.properties* in einem Text-Editor, und fügen Sie die folgenden Zeilen in der Datei hinzu, bzw. konfigurieren Sie diese Zeilen. Ersetzen Sie dabei die Beispielwerte durch die entsprechenden Werte, die Sie zuvor festgelegt haben:

   ```yaml
   spring.datasource.url=jdbc:postgresql://wingtiptoyspostgresql.postgres.database.azure.com:5432/mypgsqldb?ssl=true&sslmode=prefer
   spring.datasource.username=wingtiptoysuser@wingtiptoyspostgresql
   spring.datasource.password=********
    ```
   Hierbei gilt:

   | Parameter | BESCHREIBUNG |
   |---|---|
   | `spring.datasource.url` | Die PostgreSQL-JDBC-Zeichenfolge, die Sie weiter oben in diesem Artikel kopiert haben |
   | `spring.datasource.username` | Der PostgreSQL-Administratorname, den Sie weiter oben in diesem Artikel festgelegt haben, mit angefügtem gekürzten Servernamen |
   | `spring.datasource.password` | Das PostgreSQL-Administratorkennwort, das Sie weiter oben in diesem Artikel festgelegt haben |

1. Speichern und schließen Sie die Datei *application.properties*.

## <a name="package-and-test-the-sample-application"></a>Verpacken und Testen der Beispielanwendung 

1. Erstellen Sie die Beispielanwendung mit Maven. Beispiel:

   ```shell
   mvn clean package -P postgresql
   ```

1. Starten Sie die Beispielanwendung. Beispiel:

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. Erstellen Sie wie in den folgenden Beispielen dargestellt über eine Eingabeaufforderung mit `curl` neue Datensätze:

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
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

In diesem Tutorial haben Sie eine Java-Beispielanwendung erstellt, die Spring Data verwendet, um Informationen in einer Azure PostgreSQL-Datenbank mithilfe von JDBC zu speichern und abzurufen.

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

[POSTGRESQL01]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-01.png
[POSTGRESQL02]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-02.png
[POSTGRESQL03]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-03.png
[POSTGRESQL04]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-04.png
[POSTGRESQL05]: media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-05.png