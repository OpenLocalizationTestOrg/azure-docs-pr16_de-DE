
<properties
    pageTitle="Informationen für eine VNET in Azure RemoteApp Ziehpunkt | Microsoft Azure"
    description="Erfahren Sie mehr über die IP-Adressanforderungen für Azure RemoteApp mit einer VNET ausgeführt"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a>Informationen für eine VNET in Azure RemoteApp Ziehpunkt

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Wenn Sie Azure RemoteApp virtual Network (VNET) verwenden, verwendet RemoteApp IP-Adressen im Subnetz aus. Basierend auf die Skalierung der Ihrem Dienst RemoteApp, müssen Sie sicherstellen, dass Ihr Subnetz genügend IP-Adressen für RemoteApp virtuellen Computern verfügbar ist. Während dieser Leitfaden Ziehpunkts perfekten nicht angegeben, wie läuft RemoteApp dynamisch und nach unten virtuellen Computern innerhalb einer Websitesammlung zu drehen, wird es Sie Ihrem Subnetzbereich schätzen hilfreich sein. Dies ist besonders wichtig, da, nachdem ein RemoteApp-Dienst in einem VNET platziert wurde, Sie Subnetz vergrößern können nicht ohne RemoteApp entfernen.

Für jede RemoteApp-Auflistung, die Sie bei maximale Kapazität ausführen möchten, sollten Sie 100 verfügbaren IP-Adressen verfügen. Wenn Sie eine RemoteApp-Sammlung in den Standard-Plan haben und die maximale 500 Benutzer verfügen möchten, sollten Sie beispielsweise 100 IP-Adressen für die Websitesammlung verfügen. Auf ähnliche Weise benötigen Sie 100 IP-Adressen für eine Websitesammlung RemoteApp im grundlegende Plan, die 800 Benutzer verfügt über ein. Wenn Sie beabsichtigen, die weniger Benutzer (kleiner als der Maximalwert sein) haben, können Sie die IP-Adressen pro Websitesammlung erforderlich reduzieren. Die minimale Subnetz Größe Anforderung ist 30 IP-Adressen (/ 27).

Überprüfen Sie die folgenden Informationen, um sicherzustellen, dass Ihre VNET ist konfiguriert und Arbeitszeiten Propertly:

- [Migrieren von einer persönlichen VNET zu einer Azure VNET](remoteapp-migratevnet.md)
- [Überprüfen der Azure-VNET zur Verwendung mit Azure RemoteApp](remoteapp-vnet.md)
