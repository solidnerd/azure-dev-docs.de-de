---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 5527a38151afd531cbd256428bd729b7aba8fe45
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897588"
---
### <a name="provision-a-public-ip-address"></a>Bereitstellen einer öffentlichen IP-Adresse

Wenn für Ihre Anwendung der Zugriff von außerhalb Ihrer internen oder virtuellen Netzwerke möglich sein soll, benötigen Sie eine öffentliche statische IP-Adresse. Sie sollten diese IP-Adresse in der Knotenressourcengruppe Ihres Clusters bereitstellen. Dies wird im folgenden Beispiel veranschaulicht:

```bash
nodeResourceGroup=$(az aks show -g $resourceGroup -n $aksName --query 'nodeResourceGroup' -o tsv)
publicIp=$(az network public-ip create -g $nodeResourceGroup -n applicationIp --sku Standard --allocation-method Static --query 'publicIp.ipAddress' -o tsv)
echo "Your public IP address is ${publicIp}."
```