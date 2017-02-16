<properties 
    pageTitle="Durch die Anwendung Einsichten verwendeten IP-Adressen | Microsoft Azure"
    description="Server-Firewallausnahmen durch die Anwendung Einsichten erforderlich" 
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="ip-addresses-used-by-application-insights"></a>Durch die Anwendung Einsichten verwendeten IP-Adressen

Der [Visual Studio-Anwendung Einsichten](app-insights-overview.md) -Dienst verwendet eine Reihe von IP-Adressen. Möglicherweise müssen Sie diese Adressen wissen, wenn die app, die Sie für die Überwachung sind eine Firewall gehostet wird.

> [AZURE.NOTE] Zwar diese Adressen statisch sind, ist es möglich, dass sie von Zeit zu Zeit geändert werden muss.


## <a name="outgoing-ports"></a>Ausgehende ports

Sie müssen einige ausgehenden Ports in des Servers Firewall an, damit die Anwendung Einsichten SDK und/oder den Status Monitor zum Senden von Daten mit dem Portal zu öffnen:

|Zweck|URL|IP|Ports
|---|---|---|---
| Werden|DC.Services.VisualStudio.com<br/>DC.applicationinsights.Microsoft.com| 40.114.241.141<br/>104.45.136.42<br/>40.84.189.107<br/>168.63.242.221|443
|LiveStream|RT.Services.VisualStudio.com<br/>RT.applicationinsights.Microsoft.com |Variable|443



+ Status Monitor Konfigurations - erforderlich nur, wenn Sie Änderungen vornehmen:
 -  `management.core.windows.net:443` 
 -  `management.azure.com:443`
 -  `login.windows.net:443`
 -  `login.microsoftonline.com:443`
 -  `secure.aadcdn.microsoftonline-p.com:443`
 -  `auth.gfx.ms:443`
 -  `login.live.com:443`
+ Status Monitor Installation:
 +  `packages.nuget.org:443`

Diese Liste möglicherweise von Zeit zu Zeit ändern.

## <a name="availability-tests"></a>Verfügbarkeit von tests

Dies ist die Liste der Adressen aus der [Verfügbarkeit Webtests](app-insights-monitor-web-app-availability.md) ausgeführt werden. Wenn Webtests für Ihre app ausgeführt werden soll, aber Ihre Webserver zum Erstellen von bestimmter Clients beschränkt ist, müssen Sie eingehende Datenverkehr aus unserem Verfügbarkeit Testservern werden kann.

Öffnen Sie Ports 80 (http) und 443 (Https) für eingehenden Datenverkehr von diesen Adressen aus:

```

157.55.14.43
157.55.14.44
157.55.14.47
157.55.14.49
157.55.14.50
157.55.14.60
157.55.14.61
157.55.14.62
157.55.14.64
157.55.14.65
202.89.228.57
202.89.228.67
202.89.228.68
202.89.228.69
207.46.14.51
207.46.14.52
207.46.14.55
207.46.14.56
207.46.14.60
207.46.14.61
207.46.14.62
207.46.14.63
207.46.14.64
207.46.14.65
207.46.14.67
207.46.14.68
207.46.56.57
207.46.56.58
207.46.56.59
207.46.56.61
207.46.56.62
207.46.56.63
207.46.56.64
207.46.56.67
207.46.71.37
207.46.71.38
207.46.71.51
207.46.71.52
207.46.71.54
207.46.71.55
207.46.71.57
207.46.71.58
207.46.98.152
207.46.98.153
207.46.98.156
207.46.98.157
207.46.98.158
207.46.98.159
207.46.98.160
207.46.98.162
207.46.98.169
207.46.98.170
207.46.98.171
207.46.98.172
213.199.178.54
213.199.178.55
213.199.178.56
213.199.178.57
213.199.178.58
213.199.178.59
213.199.178.60
213.199.178.61
213.199.178.63
213.199.178.64
65.54.66.56
65.54.66.57
65.54.66.58
65.54.66.61
65.54.78.49
65.54.78.50
65.54.78.51
65.54.78.54
65.54.78.57
65.54.78.58
65.55.244.15
65.55.244.16
65.55.244.17
65.55.244.18
65.55.244.37
65.55.244.40
65.55.244.42
65.55.244.44
65.55.244.46
65.55.244.47
65.55.82.77
65.55.82.78
65.55.82.81
65.55.82.84
65.55.82.85
65.55.82.86
65.55.82.87
65.55.82.88
65.55.82.89
65.55.82.90
65.55.82.91
65.55.82.92
70.37.147.43
70.37.147.44
70.37.147.45
70.37.147.48
94.245.66.43
94.245.66.44
94.245.66.45
94.245.66.48
94.245.72.44
94.245.72.45
94.245.72.46
94.245.72.49
94.245.72.52
94.245.72.53
94.245.78.40
94.245.78.41
94.245.78.42
94.245.78.45
94.245.82.32
94.245.82.33
94.245.82.37
94.245.82.38


```  

## <a name="data-access-api"></a>Zugreifen auf Daten API



|URI|IP|Ports
|---|---|---
|API.applicationinsights.IO<br/>API1.applicationinsights.IO<br/>API2.applicationinsights.IO<br/>api3.applicationinsights.IO<br/>api4.applicationinsights.IO<br/>api5.applicationinsights.IO|13.82.26.252<br/>40.76.213.73|80,443
|dev.applicationinsights.IO<br/>dev.applicationinsights.Microsoft.com<br/>dev.aisvc.VisualStudio.com<br/>www.applicationinsights.IO<br/>www.applicationinsights.Microsoft.com<br/>www.aisvc.VisualStudio.com|13.82.24.149<br/>40.114.82.10|80,443





 
