<properties 
    pageTitle="Azure RemoteApp - treten wie Bandbreite und die Qualität des Arbeit zusammen auf? | Microsoft Azure"
    description="Erfahren Sie, wie die Bandbreite in Azure RemoteApp des Benutzers für Quality of Experience auswirken kann."
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

# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Azure RemoteApp - treten wie Bandbreite und die Qualität des Arbeit zusammen auf?

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Wenn die [Netzwerkbandbreite für insgesamt](remoteapp-bandwidth.md) für Azure RemoteApp erforderlich angezeigt werden, beachten Sie die folgenden Faktoren berücksichtigen: Dies sind alle Teil eines dynamischen Systems, die der allgemeine Umgang wirkt sich auf. 

- **Verfügbaren Bandbreite und aktuellen netzwerkbedingungen** - einer Reihe von Parametern (Verlust, Wartezeit, jitter) zu einem bestimmten Zeitpunkt im selben Netzwerk können die Anwendung streaming Erfahrung, d. h., eine geringere optimiert beeinflussen. Die Bandbreite in Ihrem Netzwerk verfügbar ist eine Funktion Überlastung, zufällige Verlust, Wartezeit, da alle diese Parameter der Überlastung angibt, wirken sich auf die Steuerelemente wiederum die Übertragung Geschwindigkeit, um Konflikte zu vermeiden.  Beispielsweise wird ein Verlust oder Netzwerk mit langen Wartezeiten den Benutzer die falsche auch in einem Netzwerk mit 1000 MB Bandbreite stellen. Die Verlust und Wartezeit variieren basierend auf der Anzahl der Benutzer, die im gleichen Netzwerk und (z. B. Anzeigen von Videos, herunterladen oder Hochladen große Dateien, Drucken) tun diese Benutzer sind.
- **Praktisches Szenario** - die Oberfläche, hängt davon ab, was der Benutzer als Einzelpersonen und als Gruppe in einem Netzwerk ausführen. Lesen einer Folie erfordert beispielsweise nur ein einzelnes Bild aktualisiert werden; Wenn der Benutzer skims und führt einen über den Inhalt eines Dokuments Text Bildlauf, benötigen sie eine höhere Anzahl von Frames pro Sekunde aktualisiert werden. Die Kommunikation und rückwärts auf dem Server in diesem Szenario wird später weitere Bandbreite nutzen. Erwägen Sie auch ein extrem Beispiel: mehrere Benutzer sind HD-Videos ansehen (wie etwa mit einer Auflösung von 4K), HD-Telefonkonferenzen gedrückt, 3D Video spielen, oder auf CAD-Systemen arbeiten. Alle diese können auch eine wirklich hohe Bandbreite Netzwerk praktisch unbrauchbar machen.
- **Bildschirmauflösung und die Anzahl der Bildschirme** - mehr Netzwerkbandbreite ist erforderlich, um die vollständige Aktualisierung vergrößern Bildschirme als kleinere Bildschirme geeignet ist. Die zugrunde liegende Technologie hat ziemlich gut Codierung und die Übertragung nur Regionen mit Bildschirmen, die aktualisiert wurden, dennoch und wieder, der gesamte Bildschirm aktualisiert werden soll. Wenn der Benutzer einen höheren Auflösung Bildschirm (beispielsweise 4 K Auflösung) enthält, ist das Update mehr als einem Bildschirm mit niedriger Auflösung (wie 1024x768px) Bandbreite erforderlich. Diese dieselbe Logik angewendet wird, wenn Sie mehr als einen Bildschirm für die Umleitung verwenden. Bandbreite muss die Anzahl der Bildschirme zu erhöhen.
- **Zwischenablage und Gerät Umleitung** – Dies ist ein Problem nicht sehr offensichtlich, aber in vielen Fällen, wenn ein Benutzer eine große Datenmenge in die Zwischenablage speichert dauert ein wenig Zeit für diese Informationen aus der Remote Desktop-Client auf dem Server zu übertragen. Mit der untergeordneten kann von der Erfahrung senden den Inhalt der Zwischenablage vor-beeinträchtigt werden. Dasselbe gilt für die Umleitung für Geräte – Wenn Sie einen Scanner oder eine Webcam erzeugt eine große Datenmenge, die auf dem Server übergeordneten gesendet werden muss, oder ein Drucker muss ein großes Dokument, erhalten oder lokaler Speicher zu einer app ausgeführt werden, in der Cloud, eine große Datei kopieren verfügbar sein muss, Benutzer Umständen Frames ausgelassen oder vorübergehend "eingefroren" Video, da die Daten für die Umleitung Gerät benötigt die Netzwerk-Bandbreite zunehmender ist muss. 

Wenn Sie Ihr Netzwerk Bandbreite Anforderungen auswerten, stellen Sie sicher, all diese als System arbeiten Faktoren berücksichtigen.

Jetzt, kehren Sie zu im [Hauptfenster Netzwerk Bandbreite Artikel](remoteapp-bandwidth.md)oder navigieren Sie zum Testen Ihr [Netzwerk-Bandbreite](remoteapp-bandwidthtests.md).

## <a name="learn-more"></a>Weitere Informationen
- [Schätzen Sie Azure RemoteApp Netzwerk Bandbreite Verwendung](remoteapp-bandwidth.md)

- [Azure RemoteApp - testen Ihr Netzwerk Bandbreite Verwendung mit anhand einiger allgemeinen Szenarien](remoteapp-bandwidthtests.md)

- [Azure RemoteApp Netzwerkbandbreite - Richtlinien (Wenn Sie Ihre eigenen überprüfen können)](remoteapp-bandwidthguidelines.md)