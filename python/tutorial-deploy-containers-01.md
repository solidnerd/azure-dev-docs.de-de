---
title: Bereitstellen von Docker-Containern in Azure App Service mit Visual Studio Code
description: 'Tutorialschritt 1: Einführung und Voraussetzungen'
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 0135761f8294b3fbbb8fe821540b46126c107109
ms.sourcegitcommit: d6575ac86449380b5a9c6c66aa722cb33ed53438
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186181"
---
# <a name="deploy-containers-to-azure-app-service"></a>Bereitstellen von Containern in Azure App Service

In diesem Tutorial erfahren Sie, wie Sie mit Visual Studio Code ein Containerimage aus einer Containerregistrierung in [Azure App Service](https://azure.microsoft.com/services/app-service/containers/) bereitstellen.

Wenn Sie Probleme mit einem der Schritte in diesem Tutorial haben, würden wir uns über nähere Informationen freuen. Verwenden Sie den Link **Ich bin auf ein Problem gestoßen** am Ende jedes Artikels, um Feedback zu geben.

## <a name="prerequisites"></a>Voraussetzungen

- Ein [Azure-Konto](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)
- [Visual Studio Code](https://code.visualstudio.com/)
- Ein geeigneter Container, der in eine Containerregistrierung hochgeladen wurde. Details zum Erstellen eines Containers mit einer Python-Web-App und zum Hinzufügen des Containers zu einer Registrierung sowie weitere Informationen finden Sie unter [Erstellen eines Containers](https://code.visualstudio.com/docs/python/tutorial-create-containers).
- Die [Azure App Service-Erweiterung für VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).
- Die [Docker-Erweiterung für VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [Ich habe mich bei Azure angemeldet.](tutorial-deploy-containers-02.md)

[Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=01-verify-prerequisites)