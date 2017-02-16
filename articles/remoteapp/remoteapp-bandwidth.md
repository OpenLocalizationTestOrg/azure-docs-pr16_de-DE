
<properties 
    pageTitle="Schätzen Azure RemoteApp Netzwerk Bandbreite Verwendung | Microsoft Azure"
    description="Informationen Sie zu den Netzwerk Bandbreite Anforderungen für Ihre Azure RemoteApp Websitesammlungen und apps."
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

# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Schätzen Sie Azure RemoteApp Netzwerk Bandbreite Verwendung 

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp verwendet die (Remotedesktopprotokoll) für die Kommunikation zwischen der Anwendung, die in der Cloud Azure und Ihre Benutzer ausgeführt. Dieser Artikel enthält einige grundlegenden Richtlinien, die Sie zum schätzen die Netzwerkverwendung und potenziell auswerten Netzwerk Bandbreite Verwendung pro Benutzer Azure RemoteApp verwenden können.

Schätzen der Bandbreite Verwendung pro Benutzer ist sehr komplex und mehrere gleichzeitig ausgeführte in Multitasking Szenarien, wo möglicherweise Applikationen beeinflussen, gegenseitig Leistung basierend auf deren Demand für Netzwerk-Bandbreite erfordert. Auch der Typ des Remote Desktop-Client (z. B. Mac-Client im Vergleich zu HTML5-Client) kann zu anderen Bandbreite Ergebnissen führen. Damit Sie dies durcharbeiten können, wird wir die Szenarios für die Verwendung in unterschiedliche die allgemeinen Kategorien realen Szenarios repliziert Teile aufzuteilen. (Wobei das realen Szenario natürlich eine Mischung aus Kategorien ist und unterscheidet sich nach Benutzer je.)

Beachten Sie, dass es sich bei angenommen RDP bietet einen guten hervorragende-Benutzeroberfläche für die meisten Szenarios für die Verwendung in Netzwerken mit Wartezeiten unter 120 ms und Bandbreite über 5 MB – dieser Wert basiert auf RDPs Möglichkeit, dynamisch anhand der verfügbaren Bandbreite anzupassen und die geschätzte Anwendung Bandbreite muss, bevor wir - fortfahren. In diesem Artikel geht über die hinausgehen "die meisten Verwendungsszenarien" auf den Rand, prüfen Sie, wo Szenarien entladen beginnen und Benutzerfunktionalität zu beeinträchtigen beginnt.

Sehen Sie sich jetzt den folgenden Artikeln, um die Details, einschließlich Faktoren berücksichtigen, geplante Empfehlungen und was wir nicht unsere schätzt hinzugefügt haben.

- [Wie treten Bandbreite und die Qualität des zusammen Arbeit auf?](remoteapp-bandwidthexperience.md)
- [Testen Ihr Netzwerk Bandbreite Verwendung mit anhand einiger allgemeinen Szenarien](remoteapp-bandwidthtests.md)
- [Schnelle Richtlinien, wenn Sie die Zeit oder die Möglichkeit zum Testen besitzen](remoteapp-bandwidthguidelines.md)


## <a name="what-are-we-not-including"></a>Was sind wir nicht eingeschlossen?

Wenn Sie die vorgeschlagenen Tests und unsere Empfehlungen für insgesamt (und zugegebenermaßen generischen) überprüfen, achten Sie darauf, dass es gibt verschiedene Faktoren, die wir nicht berücksichtigt werden. Beispielsweise herunterladen die Benutzer Erfahrung Komplikationen durch die asymmetrische Art der Upload im Vergleich zu bereitgestellten Bandbreite an. Asymmetrische Art der meisten WLAN wirkt sich außerdem auf die Leistung und der Sicht-Oberfläche auf Benutzer aus. Für interaktive Szenarien möglicherweise der untergeordnete Datenverkehr niedriger als vor-, Prioritäten zugewiesen, die möglicherweise erhöhen die Anzahl der verloren Video- oder Audioclip Rahmen und daher beeinflussen, der Sicht Benutzer auf das streaming Erfahrung. Sie können eigene Versuche, um herauszufinden, was gut für Ihre bestimmte Anwendungsfall- und Netzwerk ist ausführen.

Obwohl wir Gerät Umleitung behandelt, wir keine Auswirkung auf die Bandbreite von angeschlossenen Geräten, wie Speicher, Drucker, Scanner, Webcams und andere USB-Geräte verursacht Netzwerkverkehr berücksichtigen. Der Effekt der Geräte in der Regel die Bandbreite Anforderungen, vorübergehend Spitzen und verschwinden, wenn der Vorgang abgeschlossen ist. Aber wenn Sie häufig, Fertig, dass die Bandbreite bei Bedarf ganz auffällig werden konnte.

Darüber hinaus nicht erläutert wie einen Benutzer können andere Benutzer auf demselben Netzwerk auswirken. Beispielsweise möglicherweise einen Benutzer Verarbeitung von 4 K Video in einem 100 MB/s-Netzwerk erheblich beeinflussen, andere Benutzer die gleichen Netzwerk versuchen, dieselbe Aufgabe auszuführen. Leider wird es bis sehr schwer zu bestimmen, den Einfluss der gleichzeitigen Verwendung erteilen eine allgemeine oder umfassenden Empfehlungen darüber, wie das System am Aggregat durchführt. Wir sagen können lediglich, dass die zugrunde liegende Protokoll Technologie machen der verfügbaren Bandbreite optimal nutzen, sie hat aber ihre Grenzen.