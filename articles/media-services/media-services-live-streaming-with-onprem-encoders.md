<properties 
    pageTitle="Live mit lokal Encoder die senden-Bitrate Streams erstellte streaming | Microsoft Azure" 
    description="In diesem Thema beschrieben, wie ein Kanal einrichten, die einen Multi-Bitrate live Stream aus einer lokalen Encoder empfängt. Streams kann dann übermittelt werden Wiedergabe Clientanwendungen über eine oder mehrere Streaming Endpunkte mithilfe einer der folgenden adaptive streaming Protokolle: HLS, interpolierten Stream, MPEG Gedankenstrich HDS." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="cenkdin;juliako"/>

#<a name="live-streaming-with-on-premise-encoders-that-create-multi-bitrate-streams"></a>Live streaming mit lokal Encoder, die Multi-Bitrate Streams erstellen

##<a name="overview"></a>(Übersicht)

Im Azure Media Services stellt einen **Channel** eine Verkaufspipeline für die Verarbeitung von live streaming von Inhalten. Ein **Kanal** empfängt live Eingabewerte Streams in einem der beiden folgenden Arten:


- Klicken Sie auf eine lokale live Encoder sendet eine Multi-Bitrate **RTMP** oder **Interpolierten Streaming** (MP4 fragmentiert) an den Kanal, die zum Ausführen der live-Codierung mit AMS nicht aktiviert ist. Die Motor angesaugten Streams passieren **Channel**s ohne weitere Verarbeitung. Diese Methode ist **Pass-Through-**aufgerufen. Sie können die folgenden live Encoder, die Multi-Bitrate interpolierten Streaming ausgeben: Elementen, Envivio, Cisco.  Die folgenden live Encoder ausgeben RTMP: Adobe Flash Live, Telestream Wirecast und Tricaster Kodierungsprogramme.  Ein live Encoder kann auch einen einzelnen Bitrate Stream an einen Kanal senden, die für die live-Codierung nicht aktiviert ist, aber, die nicht empfohlen wird. Wenn Sie aufgefordert werden, bietet ein Media-Dienste Streams.

    >[AZURE.NOTE] Mithilfe einer Pass-Through-Methode ist die am häufigsten preisgünstige Möglichkeit zum streaming live.
    
- Ein lokale live Encoder sendet einen einzelnes-Bitrate Stream an den Kanal, die zum Ausführen der live-Codierung mit den Diensten von Medien in einem der folgenden Formate aktiviert ist: RTP (MPEG-Terminaldienste), RTMP oder interpolierten Streaming (MP4 fragmentiert). Klicken Sie dann der Kanal führt live-Codierung des eingehenden einzelnen Bitrate Stream in einem Videodatenstrom Multi-Bitrate (adaptive). Wenn angefordert, bietet ein Media-Dienste Streams.

Ab Release Media Services 2.10, wenn Sie einen Kanal erstellen, können Sie angeben, auf welche Weise Sie für Ihre Channel Eingabe-Stream erhalten möchten und, unabhängig davon, ob Sie für den Kanal live Codieren von Ihrem Stream ausführen möchten. Sie haben zwei Optionen:

- **Keine** – angeben diesen Wert, wenn Sie beabsichtigen, verwenden Sie einen lokale live Encoder welche wird Multi-Bitrate ausgeben übertragen (Pass-Through-Stream). In diesem Fall übergeben ohne beliebiger Codierung der eingehende Stream durch in die Ausgabe ein. Dies ist das Verhalten der einen Kanal vor Version 2.10.  Dieses Thema bietet Informationen zum Arbeiten mit Kanäle dieses Typs.

- **Standard** – dies Wert, wenn Sie beabsichtigen, Media-Dienste zu verwenden, um Ihre einzelnen Bitrate live Stream in Multi-Bitrate Stream codieren auswählen. Achten Sie darauf, dass eine Abrechnung Auswirkung für live Codierung und denken Sie daran, dass einen live codieren Kanal in den Status "Ausführen" verlassen Gebühren anfallen.  Es wird empfohlen, dass Ihre laufenden Kanäle sofort zu beenden, nachdem Ihr live streaming Ereignis zusätzliche stündliche Gebühren zu vermeiden abgeschlossen ist.
Wenn Sie aufgefordert werden, bietet ein Media-Dienste Streams. 

>[AZURE.NOTE]In diesem Thema wird erläutert, Attribute der Kanäle, die nicht aktiviert sind, live Codierung (**keine** Codierung Typ) ausführen. Informationen zum Arbeiten mit Kanäle, die zum Ausführen der live-Codierung aktiviert sind, finden Sie unter [Live streaming mit Azure Media Services um Multi-Bitrate Streams zu erstellen](media-services-manage-live-encoder-enabled-channels.md).

Das folgende Diagramm stellt einen live streaming Workflow, der einen lokalen live Encoder in Ausgabe Multi-Bitrate RTMP oder fragmentiert MP4 (interpolierten Streaming) Streams verwendet.

![Live-workflow][live-overview]

In diesem Thema werden folgende Themen behandelt:

- [Allgemeine live streaming Szenario](media-services-live-streaming-with-onprem-encoders.md#scenario)
- [Beschreibung der einen Kanal und die zugehörigen Komponenten](media-services-live-streaming-with-onprem-encoders.md#channel)
- [Aspekte](media-services-live-streaming-with-onprem-encoders.md#considerations)

##<a name="a-idscenarioacommon-live-streaming-scenario"></a><a id="scenario"></a>Allgemeine live streaming Szenario
Die folgenden Schritte beschreiben die Aufgaben, die im Allgemeinen live streaming Applications erstellen.

1. Herstellen einer Verbindung einem Computer mit einer Videokamera. Starten und Konfigurieren eines lokalen live Encoders, der einen Stream Multi-Bitrate RTMP oder fragmentiert MP4 (interpolierten Streaming) gibt. Weitere Informationen finden Sie unter [Azure Media Services RTMP Support und Live-Encoder](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Diesen Schritt kann auch durchgeführt werden, nachdem Sie Ihre Kanal erstellt haben.

1. Erstellen Sie und starten Sie einen Kanal.
1. Abrufen der Kanal Aufnahme URL. 

    Die URL für die Erfassung wird vom live Encoder zum Streams an den Kanal senden.
1. Abrufen der Channel Vorschau-URL an. 

    Verwenden Sie diese URL, um sicherzustellen, dass Ihre Channel ordnungsgemäß live-Streams empfängt.

3. Erstellen Sie ein Programm. 

    Bei Verwendung das Azure-Portal erstellt ein Programm erstellen auch eine Anlage. 

    Bei Verwendung von .NET SDK oder REST müssen Sie eine Anlage erstellen und angeben, dass diese Anlage verwenden, wenn Sie ein Programm erstellen. 
1. Veröffentlichen Sie die Anlage, die mit dem Programm verknüpft ist.   

    Stellen Sie sicher, dass mindestens eine streaming reservierte Einheit für das streaming Endpunkt von der zum Streaming von Inhalten aus.
1. Starten Sie das Programm, wenn Sie streaming und Archivierung starten möchten.
2. Optional kann der live Encoder signalisiert werden, ist eine Werbeanzeige zu starten. Die Ankündigung wird in der Ausgabestream eingefügt.
1. Beenden Sie das Programm immer, wenn Sie streaming und das Ereignis Archivierung beenden möchten.
1. Löschen Sie des Programms (und optional löschen Sie die Anlage).     

##<a name="a-idchanneladescription-of-a-channel-and-its-related-components"></a><a id="channel"></a>Beschreibung der einen Kanal und die zugehörigen Komponenten

###<a name="a-idchannelinputachannel-input-ingest-configurations"></a><a id="channel_input"></a>Eingabe Channel (Aufnahme) Konfigurationen

####<a name="a-idingestprotocolsaingest-streaming-protocol"></a><a id="ingest_protocols"></a>Aufnahme streaming-Protokoll

Media Services unterstützt Aufnahme live-Feeds mit dem folgenden streaming-Protokoll: 

- **Multi-Bitrate fragmentiert MP4**
 
- **Multi-Bitrate RTMP** 

    Wenn die **RTMP** Aufnahme streaming-Protokoll aktiviert ist, werden zwei ingest(input) Endpunkte für den Kanal erstellt werden: 
    
    **Primäre URL**: Gibt an, die vollständig qualifizierte URL ein, der den Kanal des primären RTMP Aufnahme Endpunkt.

    **Sekundäre URL** (optional): Gibt an, die vollständig qualifizierte URL ein, der den Kanal des sekundären RTMP Aufnahme Endpunkt. 


    Verwenden Sie den sekundären URL, wenn Sie die Zuverlässigkeit und Fehlertoleranz Erfassung Stream als auch Encoder Failover und Fehlertoleranz, insbesondere in den folgenden Szenarien verbessern möchten.

    - Einzelne Encoder doppelten drücken Sie nach der primären und sekundären URLs:
    
        Der Hauptfenster Zweck besteht Weitere Stabilität Netzwerk Fluktuationen und JIT-Compiler bereitstellen. Einige RTMP Encoder nicht behandelt Netzwerk auch getrennt. Wenn ein Netzwerk trennen passiert, einem Encoder möglicherweise nicht mehr Codierung und sendet keine zwischengespeicherten Daten beim Schließen wieder geschieht dies bewirkt, dass Diskontinuitäten und Daten verloren gegangen sind. Netzwerk getrennt können aufgrund einer schlechten Netzwerk- oder eine Wartung Azure auf erfolgen. Primäre/sekundäre URLs verringern die Netzwerkproblemen und auch eine gesteuert Upgradevorgang bereitstellen. Jedes Mal, wenn eine Trennung der Verbindung geplanten Netzwerk geschieht, Media-Dienste verwaltet der primären und sekundären trennen und stellt eine verzögerte zwischen den beiden die verleiht Zeit für Encoder zum Senden von Daten beibehalten und dann erneut trennen. Die Reihenfolge der trennt die Verbindung kann auch zufällig sein, aber es werden immer eine Verzögerung zwischen primärem/sekundärem oder sekundären/Primär. In diesem Szenario ist Encoder weiterhin zum einzelnen Zeitpunkt des Fehlers.
     
    - Zeigen Sie mehrere Encoder jedes Encoder zu dedizierten verschieben:
        
        Dieses Szenario bietet beide Encoder und Aufnahme Redundanz. In diesem Szenario encoder1 verlagert, auf die URL des primären und encoder2 legt sekundäre-URL. Wenn ein Encoder Fehler kann anderen Encoder immer noch senden Daten beibehalten. Datenredundanz kann weiterhin verwaltet werden, da Media-Dienste nicht trennen primären und sekundären zur gleichen Zeit. Dieses Szenario setzt voraus Encoder sind Uhrzeit synchronisieren und genau dieselbe Daten enthält.  
 
    - Mehrere Encoder doppelte drücken Sie nach der primären und sekundären URLs:
    
        In diesem Szenario Pushbenachrichtigungen Encoder Daten auf primären und sekundären URLs an. Dadurch wird die optimale Zuverlässigkeit und Fehlertoleranz sowie Datenredundanz. Es kann beide Encoder Fehlern tolerieren und auch getrennt, auch wenn ein Encoder nicht funktioniert mehr. Dieses Szenario setzt voraus Encoder sind Uhrzeit synchronisieren und genau dieselbe Daten enthält.  

Informationen zu live Encoder RTMP finden Sie unter [Azure Media Services RTMP Support und Live-Encoder](http://go.microsoft.com/fwlink/?LinkId=532824).

Folgendes gilt:

- Stellen Sie sicher, dass Sie ausreichend freier Internet Connectivity zum Senden von Daten an die Erfassung Punkte haben. 
- Sekundäre mit URL Aufnahme zusätzliche Bandbreite erfordert. 
- Eingehende Multi-Bitrate Streams kann bis zu 10 Videoqualität Ebenen (QuickInfos Layer) und bis zu 5 audio Spuren aufweisen.
- Die höchste durchschnittliche Bitrate für die Videoqualität Ebenen oder Ebenen sollte unter 10 MB/s liegen.
- Das Aggregat von den Mittelwert eine Bitrate für alle Video- und Streams sollte unter 25/s liegen.
- Sie können das Eingabewerte Protokoll während der Kanal nicht ändern oder dessen zugeordneten Programme ausgeführt werden. Wenn Sie andere Protokolle benötigen, sollten Sie separate Kanäle für jedes Eingabewerte Protokoll erstellen. 
- Können Sie eine einzelne Bitrate in Ihrem Channel Aufnahme, aber da Streams vom Channel nicht verarbeitet wird, erhalten die Clientanwendungen auch einen einzelnen Bitrate Stream (diese Option wird nicht empfohlen).

####<a name="ingest-urls-endpoints"></a>Aufnahme URLs (Endpunkte) 

Ein Kanal stellt von außen liegenden Tabellenblättern (Aufnahme URL), dass Sie in der live Encoder angeben, damit der Encoder Pushbenachrichtigungen kann Streams auf Ihre Kanäle.   

Wenn Sie den Kanal erstellen, können Sie die Erfassung URLs erhalten. Um diese URLs zu gelangen, verfügt der Kanal nicht in den Zustand **ausgeführt** werden. Wenn Sie zum Senden der Daten in den Kanal starten bereit sind, muss der Kanal in den Zustand **ausgeführt** werden. Nachdem der Kanal Aufnahme Daten startet, können Sie Ihre Stream über die URL der Vorschau Vorschau anzeigen.

Sie haben die Möglichkeit der Aufnahme fragmentiert MP4 live Stream über SSL-Verbindung (interpolierten Streaming). Vergewissern Sie sich die Erfassung URL auf HTTPS aktualisieren Schritte aus, um über SSL Aufnahme. Sie können keine derzeit RTMP über SSL Aufnahme. 

####<a name="a-idkeyframeintervalakeyframe-interval"></a><a id="keyframe_interval"></a>Keyframe Intervall

Wenn Sie einen lokalen live Encoder mit Multi-Bitrate Stream generieren, gibt das Intervall Keyframe GOP Dauer (wie durch die externen Encoder verwendet) an. Sobald diese eingehende Stream vom Channel eingeht, Sie können dann live-Streams an ausliefern Wiedergabe Clientanwendungen in eines der folgenden Formate: interpolierten Streaming, Gedankenstrich und HLS. Wenn live streaming ausführen, wird HLS immer dynamisch verpackt. Standardmäßig berechnet Media-Dienste automatisch HLS Segment Verpackung Verhältnis (Fragmente pro Segment) entsprechend dem Intervall Keyframe auch als Gruppe von Bildern – GOP, die von den live Encoder empfangene bezeichnet. 

Die folgende Tabelle zeigt, wie die Segmentdauer berechnet wird:

Keyframe Intervall|HLS Segment Verpackung Verhältnis (FragmentsPerSegment)|Beispiel
---|---|---
kleiner oder gleich 3 Sekunden|3:1|Wenn die KeyFrameInterval (oder GOP) 2 Sekunden lang ist, werden dieses HLS Segment Verpackung Verhältnis 3-1, um ein 6 Sekunden HLS Segment erstellen.
3 bis 5 Sekunden|2:1|Wenn die KeyFrameInterval (oder GOP) 4 Sekunden lang ist, werden dieses HLS Segment Verpackung Verhältnis 2 1, um ein 8 Sekunden HLS Segment erstellen.
größer als 5 Sekunden|1:1|Ist der KeyFrameInterval (oder GOP) 6 Sekunden lange, werden das standardmäßige HLS Segment Verpackung Verhältnis 1 zu 1, um eine 6 zweite lange HLS Segment erstellen.


Sie können die Fragmente pro Segment Verhältnis ändern, indem Sie Konfigurieren des Channel Ausgabe und FragmentsPerSegment auf ChannelOutputHls festlegen. 

Sie können auch den Keyframewert Intervall ändern, indem Sie die KeyFrameInterval-Eigenschaft auf ChanneInput. 

Wenn Sie explizit die KeyFrameInterval, das Verhältnis zwischen HLS Segment Verpackung, die FragmentsPerSegment mit den festlegen oben beschriebenen Regeln berechnet wird.  

Wenn Sie sowohl die KeyFrameInterval FragmentsPerSegment explizit festlegen, wird Media-Dienste, die von Ihnen festgelegte Werte verwendet. 


####<a name="allowed-ip-addresses"></a>Zugelassene IP-Adressen

Sie können die IP-Adressen definieren, die zum Veröffentlichen von Video zu diesem Channel zulässig ist. Zulässige IP-Adressen können als eine einzelne IP-Adresse (z. B. ' 10.0.0.1'), IP-Bereich verwenden eine IP-Adresse und eine CIDR Subnetzmaske (z. B. ' 10.0.0.1/22'), oder verwenden eine IP-Adresse und eine gepunktete decimal Subnetzmaske IP-Bereich angegeben werden (z. B. ' 10.0.0.1(255.255.252.0)'). 

Wenn keine IP-Adressen angegeben sind, und es keine Regeldefinition ist, wird keine IP-Adresse zulässig. Um eine beliebige IP-Adresse zulassen möchten, erstellen Sie eine Regel und 0.0.0.0/0 festgelegt.

###<a name="channel-preview"></a>Channel preview 

####<a name="preview-urls"></a>Vorschau-URLs

Kanäle bieten einen Endpunkt Vorschau (Preview-URL), den Sie in der Vorschau und überprüfen Ihre Stream, bevor weitere Verarbeitung und Übermittlung verwenden.

Im Vorschau-URL gelangen, wenn Sie den Kanal erstellen. Um die URL zu gelangen, muss der Kanal nicht in den Zustand **ausgeführt** werden. 

Nachdem der Kanal Aufnahme Daten startet, können Sie Ihre Stream Vorschau anzeigen.

Notiz, die aktuell der Vorschau Stream nur in fragmentiert MP4 übermittelt werden kann (interpolierten Streaming)-Format, unabhängig von den angegebenen Eingabewerte Typ. Im Player [http://smf.cloudapp.net/healthmonitor](http://smf.cloudapp.net/healthmonitor) können Sie um den interpolierten Stream zu testen. Einen Player gehostet Azure-Portal können Sie auch Ihre Stream anzeigen.


####<a name="allowed-ip-addresses"></a>Zugelassene IP-Adressen

Sie können die IP-Adressen definieren, die Verbindung zu den Endpunkt Vorschau zulässig ist. Wenn keine IP-Adressen angegeben sind, kann jede beliebige IP-Adresse. Zulässige IP-Adressen können als eine einzelne IP-Adresse (z. B. ' 10.0.0.1'), IP-Bereich verwenden eine IP-Adresse und eine CIDR Subnetzmaske (z. B. ' 10.0.0.1/22'), oder verwenden eine IP-Adresse und eine gepunktete dezimale Subnetzmaske IP-Bereich angegeben werden (z. B. ' 10.0.0.1(255.255.252.0)').

###<a name="channel-output"></a>Channel Ausgabe

Weitere Informationen finden Sie im Abschnitt [Einstellung Keyframe Intervall](#keyframe_interval) aus.


###<a name="channels-programs"></a>Der Channel-Programme

Ein Kanal ist zugeordnet Programme, mit die Sie die Veröffentlichung und Speicherung von Segmente in einem live Stream steuern können. Kanäle verwalten Programme. Die Beziehung Channel und Programm ist sehr ähnlich wie herkömmliche Medien, wo ein Kanal hat einen Konstanten Videodatenstrom von Inhalten und ein Programm ist auf einigen terminierten Ereignis in diesem Kanal ausgelegte.

Sie können angeben, die Anzahl der Stunden, die den aufgezeichneten Inhalt für das Programm durch Festlegen der Länge **Archiv Fenster** beibehalten werden sollen. Dieser Wert kann maximal 25 Stunden aus mindestens 5 Minuten festgelegt werden. Archivieren Fensterlänge bestimmt auch, dass die maximale Größe des Zeit-Clients aus der die aktuelle Position live zeitlich rückwärts anfordern kann. Programme ausgeführt werden können, über den festgelegten Zeitraum, aber Inhalt, der hinter dem Fensterlänge fällt wird kontinuierlich verworfen. Dieser Wert dieser Eigenschaft bestimmt auch an, wie lange die Client-Manifeste wachsen können.

Jedes Programm ist eine Anlage als Stream gesendeten Inhalt gespeichert zugeordnet. Eine Anlage zu einem Container Blob im Speicher Azure-Konto zugeordnet ist, und die Dateien in der Anlage werden als Blobs im Container gespeichert. Um das Programm zu veröffentlichen, damit Ihre Kunden Streams Weg anzeigen können, müssen Sie einen auf-Anforderung Locator für die zugeordnete Anlage erstellen. Haben Sie diese Locator aktivieren Sie streaming URL zu erstellen, die für Kunden bereitgestellt werden können.

Ein Kanal unterstützt bis zu drei gleichzeitig ausgeführte Programme an, sodass Sie mehrere Archiven des gleichen eingehende Streams erstellen können. So können Sie veröffentlichen und anderen Teile eines Ereignisses archivieren, je nach Bedarf. Beispielsweise ist Ihre geschäftliche Anforderung 6 Stunden eines Programms archivieren, sondern nur in den letzten 10 Minuten übertragen. Dazu müssen Sie zwei gleichzeitig ausgeführte Programme zu erstellen. Archivieren von 6 Stunden des Ereignisses ein Programm festgelegt ist, aber das Programm wird nicht veröffentlicht. Das andere Programm für 10 Minuten archivieren festgelegt ist, und dieses Programm veröffentlicht wird.

Vorhandene Programme für neue Ereignisse sollten Sie nicht wieder verwenden. In diesem Fall erstellen und Starten eines neuen Programms für jedes Ereignis, wie im Abschnitt Programming Live Streaming Applications beschrieben.

Starten Sie das Programm, wenn Sie streaming und Archivierung starten möchten. Beenden Sie das Programm immer, wenn Sie streaming und das Ereignis Archivierung beenden möchten. 

Zum Löschen der archivierten Inhalte beenden Sie und löschen Sie das Programm, und löschen Sie die zugeordnete Ressource. Eine Anlage kann gelöscht werden, wenn es von einem Programm verwendet wird. das Programm muss zuerst gelöscht werden. 

Sogar nachdem Sie beenden und löschen das Programm, würde der Benutzer können Ihre archivierten Inhalte als Video on Demand zur übertragen, solange Sie nicht die Anlage löschen.

Wenn Sie möchten archivierte Inhalte beibehalten, aber nicht verfügbar für das streaming, löschen Sie den streaming Locator.

##<a name="a-idstatesachannel-states-and-how-states-map-to-the-billing-mode"></a><a id="states"></a>Channel Staaten und Staaten wie in den Modus Abrechnung zuordnen 

Der aktuelle Status eines Channel. Mögliche Werte sind:

- **Nicht mehr**. Dies ist der anfänglicher Status des den Kanal nach der Erstellung aus. In diesem Zustand die Channel-Eigenschaften können aktualisiert werden, aber streaming ist nicht zulässig.
- **Starten**. Der Kanal wird gestartet. Keine Updates oder streaming während dieser Status zulässig ist. Falls ein Fehler auftritt, gibt der Kanal in den Zustand beendet.
- **Ausgeführt wird**. Der Kanal ist Lage live Streams zu verarbeiten.
- **Beenden**. Der Kanal wird gestoppt. Keine Updates oder streaming während dieser Status zulässig ist.
- **Löschen**. Der Kanal wird gelöscht. Keine Updates oder streaming während dieser Status zulässig ist.

Die folgende Tabelle zeigt die Channel Staaten wie in den Modus Abrechnung zuordnen. 
 
Channel Zustand|Portal Benutzeroberfläche Indikatoren|In Rechnung gestellt?
---|---|---|---
Starten|Starten|Keine (vorübergehenden Zustand)
Ausführen|Sofort (keine ausgeführte Programme)<p>oder<p>Streaming (mindestens ein Programm ausführt)|Ja
Beenden|Beenden|Keine (vorübergehenden Zustand)
Beendet|Beendet|Nein

##<a name="a-idccandadsaclosed-captioning-and-ad-insertion"></a><a id="cc_and_ads"></a>Untertitel und Ad-Einfügemarke 

Die folgende Tabelle zeigt die unterstützten geschlossenen Untertitel und Ad Einfügemarke-Standards.

Standard|Notizen
---|---
CEA-708 und EIA-608 (708/608)|CEA-708 und EIA-608 sind Untertiteln Standards für die USA und Kanada geschlossen.<p><p>Derzeit wird Untertitel nur unterstützt, wenn in der Eingabewerte codierte Stream übertragen. Sie müssen einen live Media Encoder verwenden, der 608 oder 708 Beschriftungen in der codierte Stream einfügen können, die an Media-Dienste gesendet wird. Media-Dienste wird den Inhalt mit eingefügten an die Betrachter vorführen.
TTML in Ismt (interpolierten Streaming Textspuren)|Media-Dienste dynamische Verpackung ermöglicht Kunden zum Streaming von Inhalten in einem der folgenden Formate: MPEG Gedankenstrich, HLS oder interpolierten Streaming. Jedoch, wenn Sie Aufnahme fragmentiert MP4 (interpolierten Streaming) mit Beschriftungen in .ismt (interpolierten Streaming Textspuren), Sie möchten nur möglicherweise Streams interpolierten Streaming Clients vorführen.
SCTE-35|Digitale signalisierendes System zum Positionieren der Einfügemarke Werbung verwendet wird. Das Signal Formular untergeordneten Empfänger mit Werbung in den Stream für die vorgesehene Zeit zusammengeführt werden sollen. SCTE-35 muss als eine gering gefüllte nachverfolgen im Eingabewerte Stream gesendet werden.<p><p>Beachten Sie, dass derzeit nur Eingabe-Stream-Format unterstützt, die Ad-Signale fungiert ist fragmentiert MP4 (interpolierten Streaming). Die einzige unterstützte Ausgabe Format ist ebenfalls mit interpolierten Streaming.


##<a name="a-idconsiderationsaconsiderations"></a><a id="Considerations"></a>Aspekte

Bei Verwendung einen lokalen live Encoder, senden einen Multi-Bitrate Stream in einem Channel gelten die folgenden Einschränkungen:

- Stellen Sie sicher, dass Sie ausreichend freier Internet Connectivity zum Senden von Daten an die Erfassung Punkte haben.
- Eingehende Multi-Bitrate Streams kann einen Höchstwert von 10 Videoqualität Ebenen (10 Ebenen), und klicken Sie auf bis zu 5 audio Spuren verfügen.
- Die höchste durchschnittliche Bitrate für die Videoqualität Ebenen oder Ebenen sollte unter 10 MB/s liegen.
- Das Aggregat von der Mittelwert eine Bitrate für alle Video- und Streams sollte unter 25/s liegen.
- Sie können das Eingabewerte Protokoll während der Kanal nicht ändern oder dessen zugeordneten Programme ausgeführt werden. Wenn Sie andere Protokolle benötigen, sollten Sie separate Kanäle für jedes Eingabewerte Protokoll erstellen.


Andere Aspekte im Zusammenhang mit der Arbeit mit Kanäle und zugehörigen Komponenten:

- Jedes Mal, wenn Sie den live Encoder neu zu konfigurieren, rufen Sie die Methode zum **Zurücksetzen** auf dem Kanal. Bevor Sie den Kanal zurückzusetzen, müssen Sie das Programm beenden. Nachdem Sie den Kanal zurückgesetzt, starten Sie das Programm erneut.
- Ein Kanal kann abgebrochen werden nur, wenn es in den Zustand "aktiv" ist, und alle Programme auf dem Kanal wurde beendet.
- Standardmäßig können Sie nur 5 Kanäle bei Ihrem Konto Media-Dienste hinzufügen. Weitere Informationen finden Sie unter [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md).
- Sie können das Eingabewerte Protokoll während der Kanal nicht ändern oder dessen zugeordneten Programme ausgeführt werden. Wenn Sie andere Protokolle benötigen, sollten Sie separate Kanäle für jedes Eingabewerte Protokoll erstellen.
- Sie werden nur berechnet, wenn Ihre Channel im Zustand **ausgeführt** wird. Weitere Informationen finden Sie in [diesem](media-services-live-streaming-with-onprem-encoders.md#states) Abschnitt.

##<a name="how-to-create-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders"></a>So erstellen Sie Kanäle, die Multi-Bitrate live Stream von lokalen Encoder erhalten

Weitere Informationen zu lokalen live Encoder finden Sie unter [3rd Party Live Encoder mit Azure Media-Diensten verwenden](https://azure.microsoft.com/blog/azure-media-services-rtmp-support-and-live-encoders/).

Wählen Sie **Portal**, **.NET**, **REST-API** , um Informationen zum Erstellen und verwalten Kanäle und Programme aus.

[AZURE.INCLUDE [media-services-selector-manage-channels](../../includes/media-services-selector-manage-channels.md)]



##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="related-topics"></a>Verwandte Themen

[Azure Media Services fragmentierte MP4 Live Aufnahme Spezifikation](media-services-fmp4-live-ingest-overview.md)

[Beim Senden von Live Streaming Ereignissen mit Azure Media-Dienste](media-services-overview.md)

[Media-Dienste (Konzepte)](media-services-concepts.md)

[live-overview]: ./media/media-services-manage-channels-overview/media-services-live-streaming-current.png

