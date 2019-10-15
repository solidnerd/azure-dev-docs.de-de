---
title: Lokale Ausführung der Azure Functions-Anwendung in Visual Studio Code
description: 'Teil 3 des Tutorials: Lokales Ausführen der App, um sie zu testen.'
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: kraigb
ms.openlocfilehash: 0a2b74d754e8a55afb88f6ef628c5da466e7b2df
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686025"
---
# <a name="test-the-function-locally"></a>Lokales Testen der Funktion

[Vorheriger Schritt: Erstellen der Funktions-App](tutorial-vscode-serverless-node-02.md)

In diesem Schritt führen Sie das Azure Functions-Projekt lokal aus, um es vor der Bereitstellung in Azure zu testen.

Beim Erstellen der Funktions-App wurde dem Projekt von der Azure Functions-Erweiterung automatisch eine VS Code-Startkonfiguration hinzugefügt (Datei *.vscode/launch.json*). Diese Konfiguration führt die gleiche Runtime aus wie Azure. Sie können daher vor der Bereitstellung in der Cloud sicher sein, dass Ihr Quellcode funktioniert.

1. Drücken Sie in Visual Studio Code **F5** (oder verwenden Sie den Menübefehl **Debuggen** > **Debuggen starten**), um den Debugger zu starten und an den Azure Functions-Host anzufügen. (Dieser Befehl verwendet automatisch die einzige von Azure Functions erstellte Debugkonfiguration.)

1. Die Ausgabe der Functions Core Tools wird in VS Code im Bereich **Terminal** angezeigt. Nachdem der Host gestartet wurde, drücken Sie die **STRG-Taste**, und klicken Sie gleichzeitig auf die lokale URL in der Ausgabe, um den Browser zu öffnen und die Funktion auszuführen:

    ![Ausgabe im VS Code-Bereich „Terminal“ beim lokalen Debuggen](media/functions-extension/local-test-output.png)

1. Der von der standardmäßigen HTTP-Triggervorlage erstellte Code analysiert einen `name`-Abfrageparameter, um die Antwort anzupassen. Fügen Sie in Ihrem Browser `?name=<yourname>` zur URL im Browser hinzu, um die Antwortausgabe korrekt anzuzeigen:

    ![Analyse der URL-Parameter durch die HTTP-Triggerfunktion](media/functions-extension/local-test-browser.png)

1. Bei der lokalen Ausführung Ihrer Funktion können Sie Breakpoints für verschiedene Teile des Codes festlegen. (Weitere Informationen zu Breakpoints und zum Debuggen in VS Code finden Sie unter [Debuggen](https://code.visualstudio.com/docs/editor/debugging).) Öffnen Sie die Datei *index.js*, und klicken Sie dann im Editor-Fenster auf den Rand links neben Zeile 11. Es erscheint ein kleiner roter Punkt, der einen Breakpoint anzeigt. Entfernen Sie nun das `?name=`-Argument aus der URL im Browser. Wenn der Browser die Anforderung sendet, hält VS Code den Funktionscode an diesem Breakpoint an:

    ![An einem Breakpoint angehaltene Ausführung in VS Code](media/functions-extension/debugging-breakpoint.png)

> [!div class="nextstepaction"]
> [Ich habe die Funktions-App lokal ausgeführt.](tutorial-vscode-serverless-node-04.md) [Es ist ein Problem aufgetreten.](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=run-app)