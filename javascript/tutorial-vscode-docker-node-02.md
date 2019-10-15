---
title: Verwenden einer Containerregistrierung in Visual Studio Code
description: 'Teil 2 des Tutorials: Verwenden einer Containerregistrierung'
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 790267333cadc1208b6a750e487f0e459e87185d
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686250"
---
# <a name="use-a-container-registry"></a>Verwenden einer Containerregistrierung

[Vorheriger Schritt: Einführung und Voraussetzungen](tutorial-vscode-docker-node-01.md)

In diesem Schritt richten Sie eine geeignete Containerregistrierung für Ihr App-Image ein. Containerfähige Hostingdienste wie Azure App Service pullen dann die Images aus der Registrierung.

In diesem Tutorial wird [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry/) (ACR) verwendet, eine private, sichere, gehostete Registrierung für Ihre Images. Die hier gezeigten Tools und Prozesse funktionieren jedoch auch mit anderen Registrierungen wie [Docker Hub](https://hub.docker.com/).

## <a name="create-an-azure-container-registry"></a>Erstellen einer Azure-Containerregistrierung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und wählen Sie dann **Ressource erstellen** > **Container** > **Containerregistrierung** aus.

    ![Erstellen einer Containerregistrierung im Azure-Portal](media/deploy-containers/portal-01.png)

1. Geben Sie im angezeigten Formular **Containerregistrierung erstellen** die entsprechenden Werte ein:

    - Der **Registrierungsname** muss innerhalb von Azure eindeutig sein und aus 5 bis 50 alphanumerischen Zeichen bestehen.
    - Wählen Sie unter **Abonnement** Ihr Abonnement aus.
    - Die **Ressourcengruppe** muss nur innerhalb Ihres Abonnements eindeutig sein.
    - Wählen Sie unter **Standort** eine Region in Ihrer Nähe aus.
    - Wählen Sie unter **Administratorbenutzer** die Option **Aktivieren** aus.
    - Wählen Sie unter **SKU** die Option **Basic** aus.

    ![Werte für das Containerregistrierungsformular](media/deploy-containers/portal-02.png)

1. Wählen Sie **Erstellen** aus, um die Registrierung zu erstellen.

1. Nachdem die Registrierung erstellt wurde, öffnen Sie die Benachrichtigungen im Portal, und wählen Sie **Zu Ressource wechseln** für die Registrierung aus:

    ![Öffnen der neu erstellten Registrierungsressource](media/deploy-containers/portal-03.png)

1. Wählen Sie auf der Registrierungsseite die Option **Zugriffsschlüssel** aus, und notieren Sie die Administratoranmeldeinformationen:

    ![Anmeldeinformationen für die Registrierung im Azure-Portal](media/deploy-containers/portal-04.png)

1. Melden Sie sich an einer Eingabeaufforderung oder einem Terminal mit dem folgenden Befehl bei Docker an. Ersetzen Sie dabei `<registry_name>` durch den Namen Ihrer Registrierung sowie `<username>` und `<password>` durch die Werte, die im Azure-Portal für den Administratorbenutzer angezeigt werden:

    ```bash
    docker login <registry_name>.azurecr.io -u <username> -p <password>
    ```

1. Öffnen Sie in Visual Studio Code den **Docker**-Explorer, und vergewissern Sie sich, dass der Registrierungsendpunkt, den Sie gerade eingerichtet haben, unter **Registrierungen** angezeigt wird:

    ![Überprüfen, ob die Registrierung im Docker-Explorer angezeigt wird](media/deploy-containers/registries.png)

> [!div class="nextstepaction"]
> [Ich habe eine Registrierung erstellt.](tutorial-vscode-docker-node-03.md) [Es ist ein Problem aufgetreten.](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=create-registry)