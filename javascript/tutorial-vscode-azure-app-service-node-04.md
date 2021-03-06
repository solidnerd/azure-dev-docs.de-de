---
title: Streamen von Protokollen aus Azure App Service in Visual Studio Code
description: 'Teil 4 des Tutorials: Anzeigen oder Anfügen von Protokollen.'
ms.topic: conceptual
ms.date: 03/04/2020
ms.openlocfilehash: e6c1f4f87a28b33dac51d6ffc59c0cd9dfdc429d
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "78893528"
---
# <a name="stream-logs-from-azure-app-service"></a>Streamen von Protokollen aus Azure App Service

[Vorheriger Schritt: Bereitstellen der Website](tutorial-vscode-azure-app-service-node-03.md)

In diesem Schritt erfahren Sie, wie Sie eine von der ausgeführten App generierte Ausgabe durch Aufrufe von `console.log` anzeigen oder „am Ende anfügen“. Diese Ausgabe wird im Fenster **Ausgabe** in Visual Studio Code angezeigt.

1. Klicken Sie im **Azure App Service**-Explorer mit der rechten Maustaste auf den Knoten der App, und wählen Sie **Start streaming logs** (Streamen der Protokolle starten) aus.

    ![Anzeigen von Streamingprotokollen](media/deploy-azure/start-streaming-logs.png)

1. Wenn Sie dazu aufgefordert werden, aktivieren Sie die Protokollierung, und starten Sie die Anwendung neu.

    ![Eingabeaufforderung zum Aktivieren der Protokollierung und Neustarten](media/deploy-azure/enable-restart.png)

1. Nachdem die App neu gestartet wurde, wird das Fenster **Ausgabe** in VS Code mit einer Verbindung mit dem Protokollstream geöffnet, der die Ausgabe zeigt.

    <pre>
    Connecting to log stream...
    2020-03-04T19:29:44  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours.
    Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).
    </pre>

1. Aktualisieren Sie die Webseite im Browser einige Male, um die zusätzliche Protokollausgabe anzuzeigen.

> [!div class="nextstepaction"]
> [Ich sehe die Protokolle.](tutorial-vscode-azure-app-service-node-05.md) [Es ist ein Problem aufgetreten.](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=tailing-logs)
