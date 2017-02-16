<properties
    pageTitle="Verwenden von App-V-apps mit Azure RemoteApp | Microsoft Azure"
    description="Informationen Sie zum Verwenden der App-V-apps in Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="using-app-v-apps-in-azure-remoteapp"></a>Verwenden von App-V-apps in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

App-V Applications können in einer Websitesammlung Azure RemoteApp Hybrid, die beitreten zu einer Domäne erforderlich ist.

Bevor Sie beginnen, stellen Sie sicher, den App-V 5.1-Client mit den neuesten Updates installieren. Sie müssen ein [benutzerdefiniertes Bild](remoteapp-create-custom-image.md) zu erstellen, die der App-V-Client enthält.  

Es ist einfach zu Ihrer vorhandene App-V-Infrastruktur mit Azure RemoteApp verwenden. Da eine Auflistung Hybrid in einer Azure VNET, die Zugriff auf Ihre Domänencontroller hat bereitgestellt wird, und die virtuellen Computern Domäne hinzugefügt sind, können Sie Ihre vorhandenen App-V Infrastruktur und Bereitstellung Methoden zur Easyily Host App-V-Anwendung in Azure RemoteApp nutzen. Hier sind einige Aspekte, die Sie kennen sollten basierend auf den Typ des App-V-Bereitstellung, die Sie aktuell arbeiten:

| Konfigurationsoptionen |                       | Positive                                                               | Negative                                                                                              |
|-----------------------|-----------------------|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Zustellungsart       | Streaming (bei Bedarf) | App wird immer die neuesten und frisch                                     | Ersten Mal Wartezeit                                                                                    |
|                       | Bereitgestellt               | Am schnellsten; App ist bereits vorhanden des virtuellen Computers                              | Aufblasen - hat Bild Speicherplatz (Grenzwert 127 GB)                                                           |
| App-Speicherort-Speicher  | Freigegebene Inhalte        | App wird im Speicher Azure RemoteApp Instanz ausgeführt.                         | Frisst, wo befindet sich die app Arbeitsspeicher und gute Verbindung zum streaming (Dateiserver)                      |
|                       | Datenträger (zwischengespeichert)         | Schnelle Ausführung. App nicht hängt von der Verfügbarkeit der Inhaltsquelle | Aufblasen - hat Bild Speicherplatz (Grenzwert 127 GB)                                                           |
| Als Ziel             | Benutzer                  | Vollständige eigenständigen App-V-Infrastruktur erfordert                          |                                                                                                       |
|                       | Global (Computer)      |  Vor dem Veröffentlichen oder als Ziel für die Veröffentlichung mit Server                         |  Müssen Sie Azure Bild zu aktualisieren, wenn die app (riesigen) aktualisiert werden sollen. Benötigt einige Speicherplatz auf Bild. |

 Nach dem Erstellen Ihrer benutzerdefiniertes Bild und Ihre Sammlung Hybrid veröffentlichen Sie die Anwendung, Zuweisen von Benutzern und genießen Ihrer vorhandenen App-V Applications in Azure RemoteApp an einem beliebigen Gerät an einer beliebigen Stelle übermittelt gehostet wird.
