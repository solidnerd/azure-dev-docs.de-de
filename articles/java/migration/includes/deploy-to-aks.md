---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 29ea7925e91b1f42e0f83b88fbc7f7d9a8fd3629
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "81672876"
---
### <a name="deploy-to-aks"></a>Bereitstellen für AKS

Es wird beschrieben, wie Sie Ihre Kubernetes-YAML-Datei(en) erstellen und anwenden. Weitere Informationen finden Sie unter [Quickstart: Bereitstellen eines Azure Kubernetes Service-Clusters über die Azure-Befehlszeilenschnittstelle](/azure/aks/kubernetes-walkthrough#run-the-application). Wenn Sie einen externen Lastenausgleich erstellen (ob für Ihre Anwendung oder einen Eingangscontroller), sollten Sie darauf achten, dass Sie die IP-Adresse aus dem vorherigen Abschnitt als `LoadBalancerIP` angeben.

Binden Sie externalisierte Parameter als Umgebungsvariablen ein. Weitere Informationen finden Sie unter [Definieren von Umgebungsvariablen für einen Container](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/). Vermeiden Sie die Einbindung von Geheimnissen (z. B. Kennwörter, API-Schlüssel und JDBC-Verbindungszeichenfolgen). Diese werden im folgenden Abschnitt beschrieben.

Beachten Sie beim Erstellen Ihrer YAML-Bereitstellungsdatei die Einstellungen für Arbeitsspeicher und CPU, damit Ihre Container die richtige Größe haben.
