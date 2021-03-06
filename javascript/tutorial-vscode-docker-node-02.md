---
title: Verwenden einer Containerregistrierung in Visual Studio Code
description: 'Teil 2 des Tutorials: Verwenden einer Containerregistrierung'
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 01e35f12e83a7e53d6d5b78c4fcf156d9a5b53f0
ms.sourcegitcommit: 756e4873f904db954a56c20ebb2f1f5116ee4596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82026173"
---
# <a name="use-a-container-registry"></a>Verwenden einer Containerregistrierung

[Vorheriger Schritt: Einführung und Voraussetzungen](tutorial-vscode-docker-node-01.md)

In diesem Schritt richten Sie eine geeignete Containerregistrierung für Ihr App-Image ein. Containerfähige Hostdienste wie Azure App Service können dann Images per Pull aus der Registrierung abrufen.

In diesem Tutorial wird [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) verwendet, eine private, sichere, gehostete Registrierung für Ihre Images. Die hier gezeigten Tools und Prozesse funktionieren jedoch auch mit anderen Registrierungen wie [Docker Hub](https://hub.docker.com/).

## <a name="create-an-azure-container-registry"></a>Erstellen einer Azure-Containerregistrierung

1. Drücken Sie in Visual Studio Code **F1**, um die Befehlspalette zu öffnen.

1. Geben Sie im Suchfeld **Registrierung** ein. Wählen Sie in den Ergebnissen **Azure Container Registry: Registrierung erstellen** aus.

   ![Der Docker-Explorer in Visual Studio Code](media/deploy-containers/docker-create-registry.jpg)

1. Geben Sie folgende Werte ein bzw. wählen diese aus:

    - Geben Sie in **Registrierungsname** einen Namen ein, der in Azure eindeutig ist und von 5 bis 50 alphanumerische Zeichen enthält.
    - Wählen Sie in **SKU** **Basic** aus.
    - Geben Sie als **Ressourcengruppe** einen Wert ein, der innerhalb Ihres Abonnements eindeutig ist.
    - Wählen Sie unter **Standort** eine Region in Ihrer Nähe aus.

    Visual Studio Code beginnt mit dem Erstellungsprozess der Registrierung in Azure. Nach dem Abschluss des Prozesses wird eine Benachrichtigung wie die folgende angezeigt. Diese Benachrichtigung bestätigt, dass die Registrierung erfolgreich erstellt wurde.

   ![Bestätigung in Visual Studio Code, dass die Registrierung erstellt wurde](media/deploy-containers/registry-created.jpg)

1. Öffnen Sie den **Docker**-Explorer. Vergewissern Sie sich, dass der Registrierungsendpunkt, den Sie soeben eingerichtet haben, unter **Registrierungen** angezeigt wird.

   ![Überprüfung, ob die Registrierung im Docker-Explorer angezeigt wird](media/deploy-containers/docker-explorer-registry.jpg)

## <a name="sign-in-to-azure-container-registry"></a>Anmelden bei Azure Container Registry

Ihre Azure-Registrierungen werden zwar in der Docker-Erweiterung angezeigt, Images können jedoch erst nach der Anmeldung bei Container Registry dorthin gepusht werden.

1. Wählen Sie in Visual Studio **STRG**+ **`** aus, um das integrierte Terminal zu öffnen.

1. Führen Sie den folgenden Befehl an der Azure-Befehlszeilenschnittstelle aus, um sich bei Container Registry anzumelden. Ersetzen Sie `<your-registry-name>` durch den Namen der Registrierung, die Sie zuvor erstellt haben.

    ```bash
    az acr login --name <your-registry-name>
    ```

> [!div class="nextstepaction"]
> [Ich habe eine Registrierung erstellt](tutorial-vscode-docker-node-03.md) [Es ist ein Problem aufgetreten](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=create-registry)
 