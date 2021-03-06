---
title: Verwenden von Spring Cloud Azure Stream Binder für Azure Service Bus
description: In diesem Artikel wird die Verwendung von Spring Cloud Azure Stream Binder zum Senden und Empfangen von Nachrichten an und von Azure Service Bus veranschaulicht.
author: seanli1988
manager: kyliel
ms.author: seal
ms.date: 08/21/2019
ms.topic: article
ms.openlocfilehash: 033025b82a493cf701abad7a6a97802611b7e48d
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "81669066"
---
# <a name="how-to-use-spring-cloud-azure-stream-binder-for-azure-service-bus"></a>Verwenden von Spring Cloud Azure Stream Binder für Azure Service Bus

[!INCLUDE [spring-boot-20-note.md](includes/spring-boot-20-note.md)]

Azure stellt eine asynchrone Nachrichtenplattform namens [Azure Service Bus](/azure/service-bus-messaging/service-bus-messaging-overview) (auch „Service Bus“) bereit, die auf dem Standard [AMQP 1.0](http://www.amqp.org/) („Advanced Message Queueing Protocol 1.0“) basiert. Service Bus kann für alle unterstützten Azure-Plattformen verwendet werden.

In diesem Artikel wird veranschaulicht, wie Spring Cloud Stream Binder zum Senden und Empfangen von Nachrichten von `queues` und `topics` von Service Bus verwendet wird.

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Voraussetzungen müssen für diesen Artikel erfüllt sein:

1. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/) registrieren.

1. Sie benötigen ein unterstütztes Java Development Kit (JDK) der Version 8 oder höher. Weitere Informationen zu den für die Entwicklung in Azure verfügbaren JDKs finden Sie unter <https://aka.ms/azure-jdks>.

1. Sie benötigen [Apache Maven](http://maven.apache.org/) Version 3.2 oder höher.

1. Wenn Sie bereits eine Service Bus-Warteschlange oder ein -Thema konfiguriert haben, stellen Sie sicher, dass der Service Bus-Namespace die folgenden Voraussetzungen erfüllt:

    1. Er lässt den Zugriff über alle Netzwerke zu.
    1. Standard (oder höher)
    1. Er verfügt über eine Zugriffsrichtlinie mit Lese-/Schreibberechtigungen für Ihre Warteschlange und Ihr Thema.

1. Wenn Sie noch keine Service Bus-Warteschlange oder -Thema konfiguriert haben, verwenden Sie das Azure-Portal, um [eine Service Bus-Warteschlange zu erstellen](/azure/service-bus-messaging/service-bus-quickstart-portal) oder [ein Service Bus-Thema zu erstellen](/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal). Stellen Sie sicher, dass der Namespace die Anforderungen des vorherigen Schritts erfüllt. Notieren Sie sich auch die Verbindungszeichenfolge im Namespace, da Sie sie für die Test-App dieses Tutorials benötigen.

1. Wenn Sie über keine Spring Boot-Anwendung verfügen, [erstellen Sie mit dem Spring-Initialisierer ein **Maven**-Projekt](https://start.spring.io/). Denken Sie daran, dass Sie **Maven-Projekt** auswählen und unter **Abhängigkeiten** die Abhängigkeit **Web** hinzuzufügen müssen.

## <a name="use-the-spring-cloud-stream-binder-starter"></a>Verwenden des Spring Cloud Stream Binder-Starters

1. Suchen Sie im übergeordneten Verzeichnis Ihrer App nach der Datei *pom.xml*, z. B.:

    `C:\SpringBoot\servicebus\pom.xml`

    Oder

    `/users/example/home/servicebus/pom.xml`

1. Öffnen Sie die Datei *pom.xml* in einem Text-Editor.

1. Fügen Sie den folgenden Codeblock unter **&lt;dependencies>** hinzu, je nachdem, ob Sie eine Service Bus-Warteschlange oder ein -Thema verwenden:

    **Service Bus-Warteschlange**

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-servicebus-queue-stream-binder</artifactId>
        <version>1.1.0.RC5</version>
    </dependency>
    ```

    ![Bearbeiten Sie die Datei „pom.xml“ für die Service Bus-Warteschlange.](media/configure-spring-cloud-stream-binder-java-app-with-service-bus/add-stream-binder-starter-pom-file-dependency-for-service-bus-queue.png)

    **Service Bus-Thema**

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-servicebus-topic-stream-binder</artifactId>
        <version>1.1.0.RC5</version>
    </dependency>
    ```

    ![Bearbeiten Sie die Datei „pom.xml“ für das Service Bus-Thema.](media/configure-spring-cloud-stream-binder-java-app-with-service-bus/add-stream-binder-starter-pom-file-dependency-for-service-bus-topic.png)

1. Speichern und schließen Sie die Datei *pom.xml*.

## <a name="configure-the-app-for-your-service-bus"></a>Konfigurieren der App für Service Bus

Sie können Ihre App entweder basierend auf der Verbindungszeichenfolge oder auf einer Anmeldeinformationsdatei konfigurieren. In diesem Tutorial wird eine Verbindungszeichenfolge verwendet. Weitere Informationen zur Verwendung von Anmeldeinformationsdateien finden Sie im [Codebeispiel für Spring Cloud Azure Stream Binder für Azure Service Bus-Warteschlangen](https://github.com/microsoft/spring-cloud-azure/tree/release/1.1.0.RC4/spring-cloud-azure-samples/servicebus-queue-binder-sample#credential-file-based-usage
) und im [Codebeispiel für Spring Cloud Azure Stream Binder für Azure Service Bus-Themen](https://github.com/microsoft/spring-cloud-azure/tree/release/1.1.0.RC4/spring-cloud-azure-samples/servicebus-topic-binder-sample#credential-file-based-usage).

1. Suchen Sie die Datei *application.properties* im *Ressourcen*-Verzeichnis Ihrer App. Beispiel:

   `C:\SpringBoot\servicebus\src\main\resources\application.properties`

   Oder

   `/users/example/home/servicebus/src/main/resources/application.properties`

1. Öffnen Sie die Datei *application.properties* in einem Text-Editor.

1. Fügen Sie den entsprechenden Code an das Ende der Datei *application.properties* an, je nachdem, ob Sie eine Service Bus-Warteschlange oder ein -Thema verwenden. Verwenden Sie die Tabelle [Feldbeschreibungen](#fd), um die Beispielwerte durch die entsprechenden Eigenschaften für Ihre Service Bus-Instanz zu ersetzen.

    **Service Bus-Warteschlange**

    ```yaml
    spring.cloud.azure.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.cloud.stream.bindings.input.destination=examplequeue
    spring.cloud.stream.bindings.output.destination=examplequeue
    spring.cloud.stream.servicebus.queue.bindings.input.consumer.checkpoint-mode=MANUAL
    ```

    **Service Bus-Thema**

    ```yaml
    spring.cloud.azure.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.cloud.stream.bindings.input.destination=exampletopic
    spring.cloud.stream.bindings.input.group=examplesubscription
    spring.cloud.stream.bindings.output.destination=exampletopic
    spring.cloud.stream.servicebus.topic.bindings.input.consumer.checkpoint-mode=MANUAL
    ```

    **<a name="fd">Feldbeschreibungen</a>**

    |                                        Feld                                   |                                                                                   BESCHREIBUNG                                                                                    |
    |--------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    |               `spring.cloud.azure.servicebus.connection-string`                |                                        Geben Sie die Verbindungszeichenfolge an, die Sie im Service Bus-Namespace vom Azure-Portal erhalten haben.                                   |
    |               `spring.cloud.stream.bindings.input.destination`                 |                            Geben Sie Service Bus-Warteschlange oder das Service Bus-Thema an, das Sie in diesem Tutorial verwendet haben.                         |
    |                  `spring.cloud.stream.bindings.input.group`                    |                                            Wenn Sie ein Service Bus-Thema verwendet haben, geben Sie das Themenabonnement an.                                |
    |               `spring.cloud.stream.bindings.output.destination`                |                               Geben Sie denselben Wert an, den Sie für das Eingabeziel verwendet haben.                        |
    | `spring.cloud.stream.servicebus.queue.bindings.input.consumer.checkpoint-mode` |                                                       Geben Sie `MANUAL` an.                                                   |
    | `spring.cloud.stream.servicebus.topic.bindings.input.consumer.checkpoint-mode` |                                                       Geben Sie `MANUAL` an.                                                   |

1. Speichern und schließen Sie die Datei *application.properties*.

## <a name="implement-basic-service-bus-functionality"></a>Implementieren grundlegender Service Bus-Funktionalität

In diesem Abschnitt erstellen Sie die Java-Klassen, die erforderlich sind, um Nachrichten an Ihre Service Bus-Instanz zu senden.

### <a name="modify-the-main-application-class"></a>Ändern der Hauptanwendungsklasse

1. Suchen Sie die Java-Hauptanwendungsdatei im Paketverzeichnis Ihrer App. Beispiel:

    `C:\SpringBoot\servicebus\src\main\java\com\example\ServiceBusBinderApplication.java`

   Oder

   `/users/example/home/servicebus/src/main/java/com/example/ServiceBusBinderApplication.java`

1. Öffnen Sie die Java-Datei für die Hauptanwendung in einem Text-Editor.

1. Fügen Sie der Datei den folgenden Code hinzu:

    ```java
    package com.example;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class ServiceBusBinderApplication {

        public static void main(String[] args) {
            SpringApplication.run(ServiceBusBinderApplication.class, args);
        }
    }
    ```

1. Speichern und schließen Sie die Datei.

### <a name="create-a-new-class-for-the-source-connector"></a>Erstellen einer neuen Klasse für den Quellconnector

1. Erstellen Sie mithilfe eines Text-Editors eine Java-Datei namens *StreamBinderSource.java* im Paketverzeichnis Ihrer App.

1. Fügen Sie den folgenden Code zur neuen Datei hinzu:

    ```java
    package com.example;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.cloud.stream.annotation.EnableBinding;
    import org.springframework.cloud.stream.messaging.Source;
    import org.springframework.messaging.support.GenericMessage;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;

    @EnableBinding(Source.class)
    @RestController
    public class StreamBinderSource {

        @Autowired
        private Source source;

        @PostMapping("/messages")
        public String postMessage(@RequestParam String message) {
            this.source.output().send(new GenericMessage<>(message));
            return message;
        }
    }
    ```

1. Speichern und schließen Sie die Datei *StreamBinderSource.java*.

### <a name="create-a-new-class-for-the-sink-connector"></a>Erstellen einer neuen Klasse für den Senkenconnector

1. Erstellen Sie mithilfe eines Text-Editors eine Java-Datei namens *StreamBinderSink.java* im Paketverzeichnis Ihrer App.

1. Fügen Sie die folgenden Codezeilen zur neuen Datei hinzu:

    ```java
    package com.example;

    import com.microsoft.azure.spring.integration.core.AzureHeaders;
    import com.microsoft.azure.spring.integration.core.api.Checkpointer;
    import org.springframework.cloud.stream.annotation.EnableBinding;
    import org.springframework.cloud.stream.annotation.StreamListener;
    import org.springframework.cloud.stream.messaging.Sink;
    import org.springframework.messaging.handler.annotation.Header;

    @EnableBinding(Sink.class)
    public class StreamBinderSink {

        @StreamListener(Sink.INPUT)
        public void handleMessage(String message, @Header(AzureHeaders.CHECKPOINTER) Checkpointer checkpointer) {
            System.out.println(String.format("New message received: '%s'", message));
            checkpointer.success().handle((r, ex) -> {
                if (ex == null) {
                    System.out.println(String.format("Message '%s' successfully checkpointed", message));
                }
                return null;
            });
        }
    }
    ```

1. Speichern und schließen Sie die Datei *StreamBinderSink.java*.

## <a name="build-and-test-your-application"></a>Erstellen und Testen der Anwendung

1. Öffnen Sie eine Eingabeaufforderung.

1. Ändern Sie das Verzeichnis in den Speicherort Ihrer Datei *pom.xml*, z. B.:

    `cd C:\SpringBoot\servicebus`

    Oder

    `cd /users/example/home/servicebus`

2. Erstellen Sie Ihre Spring Boot-Anwendung mit Maven, und führen Sie sie aus:

    ```shell
    mvn clean spring-boot:run
    ```

3. Sobald Ihre Anwendung ausgeführt wird, können Sie sie mit *curl* testen:

    ```shell
    curl -X POST localhost:8080/messages?message=hello
    ```

    In Ihrem Anwendungsprotokoll sollte „hello“ angezeigt werden:

    ```shell
    New message received: 'hello'
    Message 'hello' successfully checkpointed
    ```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Verwenden Sie das [Azure-Portal](https://portal.azure.com/), um die in diesem Artikel erstellten Ressourcen zu löschen, wenn Sie sie nicht mehr benötigen, um unerwartete Gebühren zu vermeiden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Spring Framework in Azure](/java/azure/spring-framework)