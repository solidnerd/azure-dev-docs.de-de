---
title: Schreiben von serverlosem Node.js-Code mit Azure Functions
description: In diesem Artikel wird die Verwendung von Azure Functions zum Erstellen und Bereitstellen von serverlosem Code erläutert.
ms.topic: article
ms.date: 08/19/2019
ms.custom: seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: ae6a4cebef39976af4d9a30534d394d37d86a0c8
ms.sourcegitcommit: 756e4873f904db954a56c20ebb2f1f5116ee4596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "77002494"
---
# <a name="use-azure-functions-to-write-serverless-nodejs-code-on-azure"></a>Verwenden von Azure Functions zum Schreiben von serverlosem Node.js-Code in Azure

Das serverlose Angebot in Azure heißt Azure Functions. Mit serverlosem Code können Sie dynamische, bedarfsgesteuerte Endpunkte im Internet erstellen, ohne sich Gedanken über die Bereitstellung und Verwaltung der Infrastruktur machen zu müssen. Serverloser Code besteht aus Skripts und anderen Codeausschnitten, die als Reaktion auf verschiedene Ereignisse ausgeführt werden. 

Beginnen Sie mit dem folgenden Artikel:

- [Erstellen Ihrer ersten Funktion mit Visual Studio Code](/azure/azure-functions/functions-create-first-function-vs-code). In diesem Artikel erhalten Sie eine Einführung in Azure Functions im Kontext von Visual Studio Code, wodurch viele Details vereinfacht werden.

Eignen Sie sich danach weiteres Wissen über die Möglichkeiten mit Azure Functions anhand der folgenden Artikel an:

- In der [Einführung in Azure Functions](/azure/azure-functions/functions-overview) werden die Vorteile der serverlosen Entwicklung, die Kosten und die verschiedenen Trigger beschrieben, die Sie zum Ausführen von serverlosem Code verwenden können.

- [Konzepte für Azure Functions-Trigger und -Bindungen](/azure/azure-functions/functions-triggers-bindings)

- [Azure Functions-Entwicklerhandbuch](/azure/azure-functions/functions-reference) gefolgt vom [JavaScript-Entwicklerhandbuch für Azure Functions](/azure/azure-functions/functions-reference-node)

- Wenn Sie *zustandsbehaftete* Funktionen in einer serverlosen Umgebung schreiben möchten, sehen Sie sich die Artikel [Was ist Durable Functions?](/azure/azure-functions/durable/durable-functions-overview) und [Erstellen Ihrer ersten dauerhaften Funktion in JavaScript](/azure/azure-functions/durable/quickstart-js-vscode) an.

Anschließend können Sie eine Vielzahl von Ressourcen nutzen, um mehr über serverlosen Code zu erfahren:

- Microsoft Learn-Modul: [Aktivieren automatischer Updates in einer Web-App mithilfe von Azure Functions und SignalR Service](https://docs.microsoft.com/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr/)

- Erfahren Sie mehr über die Verwendung verschiedener Trigger zum Ausführen von serverlosem Code:

  - [Ausführen von Code mit einem Timer](/azure/azure-functions/functions-create-scheduled-function)
  - [Tutorial: Hochladen von Bilddaten in die Cloud mit Azure Storage](/azure/storage/blobs/storage-upload-process-images?tabs=nodejsv10)
  - [Ausführen von Code, wenn eine Nachricht in Azure Queue Storage geschrieben wird](/azure/azure-functions/functions-create-storage-queue-triggered-function)

- [Speichern von unstrukturierten Daten mit Azure Functions und Azure Cosmos DB](/azure/azure-functions/functions-integrate-store-unstructured-data-cosmosdb?tabs=javascript) Informationen zu anderen Datenbanken finden Sie unter [Integrieren von Azure-Datenbanken in Node.js-Apps](node-howto-integrate-databases.md).

- [Lokales Codieren und Testen von Azure Functions](/azure/azure-functions/functions-develop-local)

- [Strategien zum Testen Ihres Codes in Azure Functions](/azure/azure-functions/functions-test-a-function) und [Azure Functions – Fehlerbehandlung](/azure/azure-functions/functions-bindings-error-pages)

- [Konfigurieren der Authentifizierung mit Azure Active Directory](/azure/app-service/configure-authentication-provider-aad?toc=%2fazure%2fazure-functions%2ftoc.json)

- [Einrichten von Continuous Deployment in Azure Pipelines](/azure/azure-functions/functions-how-to-azure-devops)

- [Beispiele für Node.js und Azure Functions](/samples/browse/?languages=javascript%2Cnodejs&products=azure-functions)
