---
title: 'Schritt 8: Bereinigen der mit dem Python-Code verwendeten Ressourcen in Azure Functions'
description: 'Tutorialschritt 8: Bereinigen von Azure-Ressourcen, um laufende Gebühren zu vermeiden'
ms.topic: conceptual
ms.date: 01/15/2020
ms.custom: seo-python-october2019
ms.openlocfilehash: 1e8735f8cb1a3955fda50365a70274ae6c3c5230
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "80442145"
---
# <a name="8-clean-up-azure-resources-for-azure-functions"></a>8: Bereinigen von Azure-Ressourcen für Azure Functions

[Vorheriger Schritt: Hinzufügen einer Speicherbindung](tutorial-vs-code-serverless-python-07.md)

In diesem Artikel erfahren Sie, wie Sie die in diesem Tutorial erstellten Azure-Ressourcen entfernen. Die mit Visual Studio Code erstellte Azure-Funktions-App enthält Ressourcen, die minimale Kosten verursachen können. (Weitere Informationen finden Sie unter [Azure Functions – Preise](https://azure.microsoft.com/pricing/details/functions/).)

Die beste Möglichkeit zur Bereinigung von Ressourcen ist das Löschen der Ressourcengruppe, die alle einzelnen Ressourcen dieses Tutorials enthält. Zu den Ressourcen gehören die Funktions-App, das Speicherkonto und der zugrunde liegende App Service-Plan.

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

In Visual Studio Code stellen Sie ggf. fest, dass das Kontextmenü der Funktions-App im **Azure Functions**-Explorer über den Befehl **Funktions-App löschen** verfügt. Mit dem Befehl wird aber nur die Funktions-App gelöscht, während die anderen Ressourcen erhalten bleiben. Dies kann dazu führen, dass laufende Kosten anfallen.

## <a name="next-steps"></a>Nächste Schritte

Herzlichen Glückwunsch zum Absolvieren dieser exemplarische Vorgehensweise zum Bereitstellen von Python-Code für Azure Functions! Sie sind jetzt bereit, viele weitere serverlose Funktionen zu erstellen.

Wie bereits erwähnt, können Sie mehr über die Functions-Erweiterung erfahren, indem Sie das zugehörige GitHub-Repository [vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions) besuchen. Beschreibungen von Problemen und sonstige Beiträge sind ebenfalls willkommen.

Lesen Sie die [Einführung in Azure Functions](/azure/azure-functions/functions-overview), um die verschiedenen Trigger zu untersuchen, die Sie verwenden können.

Weitere Informationen zu Azure-Diensten, die Sie von Python aus verwenden können, einschließlich Datenspeicherung sowie KI und Machine Learning-Diensten, finden Sie unter [Azure für Python-Cloudentwickler](/azure/python/?view=azure-python).

Es gibt auch andere Azure-Erweiterungen für Visual Studio Code, die möglicherweise für Sie hilfreich sind. Suchen Sie einfach im Erweiterungen-Explorer nach „Azure“:

![Azure-Erweiterungen für Visual Studio Code](media/tutorial-vs-code-serverless-python/azure-extensions-for-visual-studio-code.png)

Einige beliebte Erweiterungen sind:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). Weitere Informationen finden Sie im [Tutorial: Bereitstellen für App Service](tutorial-deploy-app-service-on-linux-01.md).
- [Azure CLI-Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [Ich bin fertig!](https://docs.microsoft.com/python/azure/?view=azure-python)

[Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=08-clean-up-resources)
