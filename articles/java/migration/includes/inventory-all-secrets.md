---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: a771d0689e83e0b61bf9b8c31e0cbf36949fdc20
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673396"
---
### <a name="inventory-all-secrets"></a>Bestand: Alle Geheimnisse

Vor der Einführung von „Configuration-as-a-Service“-Technologien, z. B. Azure Key Vault, gab es kein gut definiertes Konzept für „Geheimnisse“. Stattdessen haben Sie über viele unterschiedliche Konfigurationseinstellungen verfügt, die quasi als die nun verwendeten „Geheimnisse“ gedient haben. Bei App-Servern, z. B. WebLogic Server, sind diese Geheimnisse in vielen unterschiedlichen Konfigurationsdateien und -speichern enthalten. Überprüfen Sie alle Eigenschaften und Konfigurationsdateien auf den Produktionsservern auf Geheimnisse und Kennwörter. Sehen Sie in der Datei *weblogic.xml* in Ihren WARs nach. Unter Umständen finden Sie in Ihrer Anwendung auch Konfigurationsdateien mit Kennwörtern oder Anmeldeinformationen. Weitere Informationen finden Sie unter [Grundlegende Konzepte von Azure Key Vault](/azure/key-vault/basic-concepts).
