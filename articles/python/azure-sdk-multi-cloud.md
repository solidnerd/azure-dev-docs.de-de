---
title: 'Herstellen einer Verbindung mit allen Regionen: Azure SDK für Python (mehrere Clouds)'
description: Verwenden von Azure in allen Regionen
ms.date: 02/22/2018
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: a8425ffbc58fe0173da4b8b75611332d3f97fa04
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "80441745"
---
# <a name="multi-cloud-connect-to-all-regions-with-the-azure-sdk-for-python"></a>Mehrere Clouds: Herstellen einer Verbindung mit allen Regionen mit dem Azure SDK für Python

Sie können das Azure SDK für Python verwenden, um eine Verbindung mit allen Regionen herzustellen, in denen Azure [verfügbar](https://azure.microsoft.com/regions/services) ist.

Standardmäßig ist das Azure SDK für Python zum Herstellen einer Verbindung mit der globalen Azure-Umgebung konfiguriert.

## <a name="using-predeclared-cloud-definition"></a>Verwenden einer vorab deklarierten Clouddefinition

> [!IMPORTANT]
> Das `msrestazure`-Paket muss für diesen Abschnitt mindestens 0.4.11 entsprechen.

Sie können das `azure_cloud`-Modul von `msrestazure` verwenden.

```python
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD
from msrestazure.azure_active_directory import UserPassCredentials
from azure.mgmt.resource import ResourceManagementClient

credentials = UserPassCredentials(
    login,
    password,
    cloud_environment=AZURE_CHINA_CLOUD
)
client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager
)
```
  
Folgende Clouddefinitionen sind verfügbar:

- AZURE_PUBLIC_CLOUD
- AZURE_CHINA_CLOUD
- AZURE_US_GOV_CLOUD
- AZURE_GERMAN_CLOUD

## <a name="using-your-own-cloud-definition-eg-azure-stack"></a>Verwenden Ihrer eigenen Clouddefinition (z.B. Azure Stack)

Azure Resource Manager verfügt zu Ihrer Unterstützung über einen Metadatenendpunkt:

```python
from msrestazure.azure_cloud import get_cloud_from_metadata_endpoint
from msrestazure.azure_active_directory import UserPassCredentials
from azure.mgmt.resource import ResourceManagementClient

mystack_cloud = get_cloud_from_metadata_endpoint("https://myazurestack-arm-endpoint.com")
credentials = UserPassCredentials(
    login,
    password,
    cloud_environment=mystack_cloud
)
client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=mystack_cloud.endpoints.resource_manager
)
```

## <a name="using-adal"></a>Verwenden von ADAL

Beim Herstellen einer Verbindung mit einer anderen Region müssen einige Punkte berücksichtigt werden:

- An welchem Endpunkt soll nach einem Token (Authentifizierung) gefragt werden?
- An welchem Endpunkt wird dieses Token (Nutzung) verwendet?

Im Folgenden sehen Sie ein allgemeines Beispiel:

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Public Azure - default values
authentication_endpoint = 'https://login.microsoftonline.com/'
azure_endpoint = 'https://management.azure.com/'

context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```

### <a name="azure-government"></a>Azure Government

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Government
authentication_endpoint = 'https://login.microsoftonline.us/'
azure_endpoint = 'https://management.usgovcloudapi.net/'

context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```

### <a name="azure-germany"></a>Azure Deutschland

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Azure Germany
authentication_endpoint = 'https://login.microsoftonline.de/'
azure_endpoint = 'https://management.microsoftazure.de/'

context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```

### <a name="azure-china-21vianet"></a>Azure China 21Vianet

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Azure China
authentication_endpoint = 'https://login.chinacloudapi.cn/'
azure_endpoint = 'https://management.chinacloudapi.cn/'

context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```
