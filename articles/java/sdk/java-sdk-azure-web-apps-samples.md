---
title: Azure-Verwaltungsbibliotheken für Java-Web-App-Beispiele
description: Abrufen von Beispielcode zum Erstellen und Aktualisieren von in App Service gehosteten Azure-Web-Apps mit den Azure-Verwaltungsbibliotheken für Java
keywords: Azure, Java, SDK, API, Maven, Gradle, Web-Apps, App Service
author: rloutlaw
ms.date: 04/16/2017
ms.topic: article
ms.service: multiple
ms.assetid: 43633e5c-9fb1-4807-ba63-e24c126754e2
ms.custom: seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 30884958dd4c829cf7b268715de7c79f3a786724
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "81674176"
---
# <a name="azure-management-libraries-for-java---web-app-samples"></a>Azure-Verwaltungsbibliotheken für Java: Beispiele für Web-Apps 

Die folgende Tabelle enthält Links zu Java-Ressourcen, die Sie zum Erstellen und Konfigurieren von Web-Apps verwenden können.

| **Erstellen einer App** ||
|---|---|
| [Erstellen einer Web-App und Bereitstellen über FTP oder GitHub][1] | Bereitstellen von Web-Apps über das lokale Git, FTP und Continuous Integration über GitHub |
| [Erstellen einer Web-App und Verwalten von Bereitstellungsslots][2] | Erstellen einer Web-App und Bereitstellen in Stagingslots sowie Austauschen von Bereitstellungen zwischen Slots |
| **Konfigurieren der App** ||
| [Erstellen einer Web-App und Konfigurieren einer benutzerdefinierten Domäne][3] | Erstellen einer Web-App mit einer benutzerdefinierten Domäne und einem selbstsignierten SSL-Zertifikat |
| **Skalieren von Apps** ||
| [Skalieren einer Web-App mit Hochverfügbarkeit in mehreren Regionen][4] | Skalieren einer Web-App in drei verschiedenen geografischen Regionen und Verfügbarmachen der App mithilfe von Azure Traffic Manager über einen einzelnen Endpunkt | 
| **Verbinden der App mit Ressourcen** ||
| [Verbinden einer Web-App mit einem Speicherkonto][5] | Erstellen eines Azure-Speicherkontos und Hinzufügen der Speicherkonto-Verbindungszeichenfolge zu den App-Einstellungen |
| [Verbinden einer Web-App mit einer SQL-Datenbank][6] | Erstellen einer Web-App und einer SQL-Datenbank und Hinzufügen der SQL Datenbank-Verbindungszeichenfolge zu den App-Einstellungen |

[1]: java-sdk-configure-webapp-sources.md
[2]: https://github.com/Azure-Samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://github.com/Azure-Samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
[5]: https://github.com/Azure-Samples/app-service-java-manage-storage-connections-for-web-apps/
[6]: https://github.com/Azure-Samples/app-service-java-manage-data-connections-for-web-apps/