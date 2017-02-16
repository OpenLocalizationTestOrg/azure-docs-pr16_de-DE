<properties 
    pageTitle="Azure RemoteApp Netzwerkbandbreite - allgemeine Richtlinien | Microsoft Azure"
    description="Verstehen Sie einige grundlegende Netzwerk Bandbreite Richtlinien für Ihre Azure RemoteApp Websitesammlungen und apps."
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
    
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a>Azure RemoteApp Netzwerkbandbreite - Richtlinien (Wenn Sie Ihre eigenen überprüfen können)

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Wenn Sie nicht die Zeit oder Videofunktionen in Azure RemoteApp [Netzwerk Bandbreite Tests](remoteapp-bandwidthtests.md) ausgeführt haben, sind hier einige sehr allgemeinen Richtlinien, mit denen Sie die Bandbreite pro Benutzer zu ermitteln können.

Wenn Sie eine Kombination aus diesen Szenarien enthalten, nicht empfohlen etwas kleiner als (oder gleich) 10 MB/s als der MINIMALWERT Netzwerk-Bandbreite für moderne Internet verbundenen apps in einer remote-Umgebung. (Zwar wie beschrieben, kein besseres sichergestellt wird als Mittelwert Erfahrungen.)

## <a name="complex-powerpoint-simple-powerpoint"></a>Komplexe PowerPoint einfachen PowerPoint

Azure RemoteApp wird am besten auf 100 MB LAN. Auf das 10 MB/s-Netzwerk-Profil sehen der Benutzer beim Jitter über 120 ms mehr als 5 %, ist eine durchschnittliche Erfahrung. Mit 1 MB/s, die die verschiedenen – beispielsweise in einer Bildschirmpräsentation langen ist möglicherweise der Benutzer einer animierten Übergänge gar nicht anzeigen, da Frames übersprungen werden.

## <a name="internet-explorer-mixed-pdf-pdf-text"></a>Internet Explorer: gemischte PDF-Datei, die PDF-Datei, die Text

10 MB/s-Netzwerk-Profil ist ähnlich LAN in den meisten Aspekte. 1 MB/s stellt eine OK zur Verfügung steht, zur Verfügung, auch wenn es einige Jitter bei einem Bildlauf während Bilder auf dem Bildschirm vorhanden sind.

## <a name="flash-video-youtube"></a>Flash Video (YouTube)

100 MB/s LAN bietet am besten während 10 MB/s zulässigen (d. h.) ist mit der Frame-Rate aufrechterhalten, aber jitter erhöht. Bei 1 MB/s ist Jitter sehr hohe und auffällig.

## <a name="word-typing-word-remote-input"></a>Word-Eingabe (Word remote Eingabe)
Dies ist ein Verwendungsszenario mit niedriger Bandbreite. Bei 256 KB/s stellen wir eine Erfahrung als LAN so gut.

## <a name="learn-more"></a>Weitere Informationen
- [Schätzen Sie Azure RemoteApp Netzwerk Bandbreite Verwendung](remoteapp-bandwidth.md)

- [Azure RemoteApp - treten wie Bandbreite und die Qualität des Arbeit zusammen auf?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp - Tseting Ihr Netzwerk Bandbreite Verwendung mit anhand einiger allgemeinen Szenarien](remoteapp-bandwidthtests.md)