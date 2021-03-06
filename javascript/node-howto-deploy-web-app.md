---
title: Bereitstellen von Node.js-Web-Apps in Azure
description: Erste Schritte mit Azure App Service und anderen Hostingoptionen für Web-Apps, einschließlich progressiver Web-Apps (PWA)
ms.topic: article
ms.date: 08/20/2019
ms.custom: seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: e7cb374b645738140ead924296ef7cef9b1b61d7
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "74467229"
---
# <a name="deploy-nodejs-web-apps-to-azure-app-service"></a>Bereitstellen von Node.js-Web-Apps in Azure App Service

In Azure stehen Ihnen mehrere Optionen zum Bereitstellen und Hosten von Web-Apps zur Verfügung:

- Azure App Service, ein PaaS-Angebot (Platform-as-a-Service), ist die beste Hostingoption für Web-Apps. Steigen Sie zunächst mit einer der folgenden Ressourcen ein:

  - [Erstellen einer Node.js-Web-App in Azure](/azure/app-service/app-service-web-get-started-nodejs)
  - [Testen von Azure App Service: Erstellen einer Express-App aus einer Vorlage](https://code.visualstudio.com/tryappservice/?utm_source=msftdocs&utm_medium=microsoft&utm_campaign=tryappservice)
  - [Microsoft Learn-Modul: Hosten einer Web-App mit Azure App Service](/learn/modules/host-a-web-app-with-azure-app-service/index)
  - [Erstellen einer Node.js- und MongoDB-Web-App in Azure](/azure/app-service/app-service-web-tutorial-nodejs-mongodb-app)
  - [App Service-Beispiele](/samples/browse/?languages=javascript%2Cnodejs&products=azure-app-service)

- Mithilfe von Azure Container Registry und Azure Kubernetes Service können Sie Ihre eigenen Container erstellen und in Azure bereitstellen. Ausführliche Informationen dazu finden Sie unter [Bereitstellen von Node.js-Containern in Azure](node-howto-deploy-containers.md).

- Wenn Sie hauptsächlich mit serverlosem Code arbeiten möchten, finden Sie weitere Informationen unter [Schreiben von serverlosem Node.js-Code in Azure](node-howto-write-serverless-code.md).

- Ausführliche Informationen zum Erstellen einer (statischen) JAMstack-Website finden Sie unter [Erstellen von (statischen) JAMstack-Web-Apps mit Azure](node-howto-create-static-site-jamstack.md).

- Wenn Sie die Infrastruktur steuern möchten, können Sie einfach eine VM (virtueller Computer) verwenden. Befolgen Sie zunächst dem Microsoft Learn-Pfad [Bereitstellen einer Website mit virtuellen Azure-Computern](/learn/paths/deploy-a-website-with-azure-virtual-machines/).

Eine umfassende Übersicht über die verschiedenen Hostingoptionen finden Sie unter [Entscheidungsstruktur für Azure-Computedienste](/azure/architecture/guide/technology-choices/compute-decision-tree) und im Microsoft Learn-Modul [Wichtige Clouddienste: Computeoptionen in Azure](/learn/modules/intro-to-azure-compute/).
