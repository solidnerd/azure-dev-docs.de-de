---
title: Ändern des App-Codes und erneutes Bereitstellen in Azure
description: 'Teil 6 des Tutorials: Vornehmen von Änderungen und erneutes Bereitstellen'
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: f656e7414d6b7b6cecc28d69f108bfe92eb82894
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686128"
---
# <a name="make-changes-and-redeploy"></a>Vornehmen von Änderungen und erneutes Bereitstellen

[Vorheriger Schritt: Streaming von Protokollen](tutorial-vscode-azure-cli-node-05.md)

In diesem Schritt nehmen Sie Änderungen an Ihrem App-Code vor, committen die Änderungen in das lokale Git-Repository, und anschließend stellen Sie Ihre Website erneut bereit, indem Sie sie in Azure pushen.

1. Öffnen Sie im Ordner `myExpressApp` die Datei *views/index.pug*, und ändern Sie die Meldung in Zeile 5 in `p Welcome to Azure!`.

    ![Bearbeiten der Datei „index.pug“](media/azure-cli/editpugfile.png)

1. Speichern Sie die Datei .

1. Führen Sie an einem Terminal oder einer Eingabeaufforderung den folgenden Befehl aus, um die Änderungen in Git zu committen:

    ```bash
    git commit -a -m "Edited message"
    ```

1. Pushen Sie die Änderungen in den Git-Remotespeicherort „Azure“, den Sie zuvor erstellt haben:

    ```bash
    git push azure master
    ```

1. Da App Service bereits mit dem Git-Repository verbunden ist, zeigt die Ausgabe des Befehls, dass Änderungen automatisch in Azure veröffentlicht werden: 

    ```output
    Enumerating objects: 7, done.
    Counting objects: 100% (7/7), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (4/4), done.
    Writing objects: 100% (4/4), 405 bytes | 202.00 KiB/s, done.
    Total 4 (delta 2), reused 0 (delta 0)
    remote: Updating branch 'master'.
    remote: Updating submodules.
    remote: Preparing deployment for commit id '9fd89a25b6'.
    remote: Generating deployment script.
    remote: Running deployment command...
    remote: Handling node.js deployment.
    remote: Creating app_offline.htm
    remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
    remote: Copying file: 'views\index.pug'
    remote: Deleting app_offline.htm
    remote: Using start-up script bin/www from package.json.
    remote: Generated web.config.
    remote: The package.json file does not specify node.js engine version constraints.
    remote: The node.js application will run with the default node.js version 6.9.5.
    remote: Selected npm version 3.10.10
    remote: ..
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git
       a98bad8..9fd89a2  master -> master
    ```

1. Aktualisieren Sie die App im Browser, um die Änderungen anzuzeigen:

    ![Im Browser angezeigte veröffentlichte Änderungen](media/azure-cli/remote-app-changes.png)

> [!div class="nextstepaction"]
> [Ich sehe meine Änderungen.](tutorial-vscode-azure-cli-node-07.md) [Es ist ein Problem aufgetreten.](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=publishing-changes)