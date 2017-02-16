<properties 
    pageTitle="Mithilfe von Azure Media-Dienste um zu erstellen Multi-Bitrate Streams streaming Live | Microsoft Azure" 
    description="In diesem Thema beschrieben, wie ein Kanal einrichten, die einen einzelnen Bitrate live Stream aus einer lokalen Encoder empfängt, und klicken Sie dann führt live Codierung in adaptive Bitrate Stream mit Media-Dienste. Streams kann dann übermittelt werden Wiedergabe Clientanwendungen über eine oder mehrere Streaming Endpunkte mithilfe einer der folgenden adaptive streaming Protokolle: HLS, interpolierten Stream, MPEG Gedankenstrich HDS." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="juliako;anilmur"/>

#<a name="live-streaming-using-azure-media-services-to-create-multi-bitrate-streams"></a>Live streaming mit Azure Media Services Multi-Bitrate Streams erstellen

##<a name="overview"></a>(Übersicht)

In Azure Media Services (AMS) stellt einen **Channel** eine Verkaufspipeline für die Verarbeitung von live streaming von Inhalten. Ein **Kanal** empfängt live Eingabewerte Streams in einem der beiden folgenden Arten:

- Ein lokale live Encoder sendet einen einzelnes-Bitrate Stream an den Kanal, die zum Ausführen der live-Codierung mit den Diensten von Medien in einem der folgenden Formate aktiviert ist: RTP (MPEG-Terminaldienste), RTMP oder interpolierten Streaming (MP4 fragmentiert). Klicken Sie dann der Kanal führt live-Codierung des eingehenden einzelnen Bitrate Stream in einem Videodatenstrom Multi-Bitrate (adaptive). Wenn Sie aufgefordert werden, bietet ein Media-Dienste Streams.

- Klicken Sie auf eine lokale live Encoder sendet eine Multi-Bitrate **RTMP** oder **Interpolierten Streaming** (MP4 fragmentiert) an den Kanal, die zum Ausführen der live-Codierung mit AMS nicht aktiviert ist. Die Motor angesaugten Streams passieren **Channel**s ohne weitere Verarbeitung. Diese Methode ist **Pass-Through-**aufgerufen. Sie können die folgenden live Encoder, die Multi-Bitrate interpolierten Streaming ausgeben: Elementen, Envivio, Cisco.  Die folgenden live Encoder ausgeben RTMP: Adobe Flash Live, Telestream Wirecast und Tricaster Kodierungsprogramme.  Ein live Encoder kann auch einen einzelnen Bitrate Stream an einen Kanal senden, die für die live-Codierung nicht aktiviert ist, aber, die nicht empfohlen wird. Wenn Sie aufgefordert werden, bietet ein Media-Dienste Streams.

    >[AZURE.NOTE] Mithilfe einer Pass-Through-Methode ist die am häufigsten preisgünstige Möglichkeit zum streaming live.

Ab Release Media Services 2.10, wenn Sie einen Kanal erstellen, können Sie angeben, auf welche Weise Sie für Ihre Channel Eingabe-Stream erhalten möchten und, unabhängig davon, ob Sie für den Kanal live Codieren von Ihrem Stream ausführen möchten. Sie haben zwei Optionen:

- **Keine** – angeben diesen Wert, wenn Sie beabsichtigen, verwenden Sie einen lokale live Encoder welche wird Multi-Bitrate ausgeben übertragen (Pass-Through-Stream). In diesem Fall übergeben ohne beliebiger Codierung der eingehende Stream durch in die Ausgabe ein. Dies ist das Verhalten der einen Kanal vor Version 2.10.  Ausführlichere Informationen zum Arbeiten mit Kanäle dieses Typs finden Sie unter [Live streaming mit lokal Encoder, die Multi-Bitrate Streams erstellen](media-services-live-streaming-with-onprem-encoders.md).

- **Standard** – dies Wert, wenn Sie beabsichtigen, Media-Dienste zu verwenden, um Ihre einzelnen Bitrate live Stream in Multi-Bitrate Stream codieren auswählen. Achten Sie darauf, dass eine Abrechnung Auswirkung für live Codierung und denken Sie daran, dass einen live codieren Kanal in den Status "Ausführen" verlassen Gebühren anfallen.  Es wird empfohlen, dass Ihre laufenden Kanäle sofort zu beenden, nachdem Ihr live streaming Ereignis zusätzliche stündliche Gebühren zu vermeiden abgeschlossen ist.

>[AZURE.NOTE]In diesem Thema wird erläutert, Attribute der Kanäle, die auszuführenden live Codierung (**Standard** Typ Codierung) aktiviert sind. Informationen zum Arbeiten mit Kanäle, die zum Ausführen der live-Codierung deaktiviert sind, finden Sie unter [Live streaming mit lokal Encoder, die Multi-Bitrate Streams erstellen](media-services-live-streaming-with-onprem-encoders.md).
>
>Vergewissern Sie sich im Abschnitt [Aspekte](media-services-manage-live-encoder-enabled-channels.md#Considerations) zu überprüfen.


##<a name="billing-implications"></a>Abrechnung Auswirkungen

Ein live Codierung Kanal beginnt, sobald sie Übergänge zwischen in "Running" über die-API ist Abrechnung.   Sie können auch den Status der Azure-Portal oder im Tool Azure Media Services Explorer (http://aka.ms/amse) anzeigen.

Die folgende Tabelle zeigt, wie Channel Staaten Abrechnung Zuständen im Portal API und Azure zugeordnet. Beachten Sie, dass die Staaten weicht zwischen API und Portal UX. Sobald ein Kanal in den Status "Einstieg" über die-API oder in den Status "Bereit" oder "Streaming" Azure-Portal ist, wird der Abrechnung aktiv sein.
Den Kanal von zur Abrechnung Sie Weiter um zu beenden, müssen Sie den Kanal über die-API oder der Azure-Portal zu beenden.
Sie sind verantwortlich für Ihre Kanäle beenden, wenn Sie mit der live Codierung Kanal fertig sind.  So beenden Sie einen codieren Channel Fehler führt kontinuierliche Abrechnung.

###<a name="a-idstatesachannel-states-and-how-they-map-to-the-billing-mode"></a><a id="states"></a>Channel Staaten und deren Zuordnung in den Modus Abrechnung 

Der aktuelle Status eines Channel. Mögliche Werte sind:

- **Nicht mehr**. Dies ist der anfänglicher Status des den Kanal nach der Erstellung (sofern keine Autostart im Portal ausgewählt wurde). Keine Abrechung in diesem Zustand. In diesem Zustand die Channel-Eigenschaften können aktualisiert werden, aber streaming ist nicht zulässig.
- **Starten**. Der Kanal wird gestartet. Keine Abrechung in diesem Zustand. Keine Updates oder streaming während dieser Status zulässig ist. Falls ein Fehler auftritt, gibt der Kanal in den Zustand beendet.
- **Ausgeführt wird**. Der Kanal ist Lage live Streams zu verarbeiten. Es ist jetzt Verwendung Abrechnung. Sie müssen den Kanal, um zu verhindern, dass weitere Abrechnung beenden. 
- **Beenden**. Der Kanal wird gestoppt. Keine Abrechung in diesem vorübergehenden Zustand. Keine Updates oder streaming während dieser Status zulässig ist.
- **Löschen**. Der Kanal wird gelöscht. Keine Abrechung in diesem vorübergehenden Zustand. Keine Updates oder streaming während dieser Status zulässig ist.

Die folgende Tabelle zeigt die Channel Staaten wie in den Modus Abrechnung zuordnen. 
 
Channel Zustand|Portal Benutzeroberfläche Indikatoren|Handelt es sich um Abrechnung?
---|---|---
Starten|Starten|Keine (vorübergehenden Zustand)
Ausführen|Sofort (keine ausgeführte Programme)<br/>oder<br/>Streaming (mindestens ein Programm ausführt)|JA
Beenden|Beenden|Keine (vorübergehenden Zustand)
Beendet|Beendet|Nein

###<a name="automatic-shut-off-for-unused-channels"></a>Automatische für nicht verwendete Kanäle ausschalten

Beginnend mit dem 25 Januar 2016 Media-Dienste Variationswebsites ein Update, die automatisch einen Kanal (mit live-Codierung aktiviert) beendet wird, nachdem er eine nicht verwendete Zustand über längere Zeiträume ausgeführt worden ist. Dies gilt für Kanäle, die über keine aktive Programme verfügen und die einen Eingaben Beitrag-Feeds für eine längere Zeit nicht erhalten haben.

Der Schwellenwert für eine nicht verwendete Zeitraum ist nominell 12 Stunden, aber Ankündigung geändert.

##<a name="live-encoding-workflow"></a>Codierung Workflow Live
Das folgende Diagramm darstellt, in denen ein Kanal ein einzelnen Bitrate Streams in eines der folgenden Protokolle erhält, einen streaming live-Workflow: RTMP, interpolierten Streaming oder RTP (MPEG-Terminaldienste); Klicken Sie dann codiert es in einen Stream Multi-Bitrate Streams Weg. 

![Live-workflow][live-overview]


##<a name="in-this-topic"></a>In diesem Thema

- Übersicht über ein [gängiges live streaming Szenario](media-services-manage-live-encoder-enabled-channels.md#scenario)
- [Beschreibung der einen Kanal und die zugehörigen Komponenten](media-services-manage-live-encoder-enabled-channels.md#channel)
- [Aspekte](media-services-manage-live-encoder-enabled-channels.md#Considerations)


##<a name="a-idscenarioacommon-live-streaming-scenario"></a><a id="scenario"></a>Allgemeine Live Streaming Szenario

Im folgenden werden die allgemeinen Schritte bei der gemeinsamen live streaming Applications erstellen.

>[AZURE.NOTE] Die maximal zulässige empfohlene Dauer eines Ereignisses live ist aktuell, 8 Stunden. Wenden Sie sich an Amslived unter Microsoft.com, wenn Sie einen Kanal für längere Zeiträume ausführen müssen. Achten Sie darauf, dass eine Abrechnung Auswirkung für live Codierung und denken Sie daran, dass einen live codieren Kanal in den Status "Ausführen" verlassen stündliche Gebühren anfallen.  Es wird empfohlen, dass Ihre laufenden Kanäle sofort zu beenden, nachdem Ihr live streaming Ereignis zusätzliche stündliche Gebühren zu vermeiden abgeschlossen ist. 

1. Herstellen einer Verbindung einem Computer mit einer Videokamera. Starten und konfigurieren einen lokalen live Encoder, die einen **einzigen** Bitrate Stream in eine der folgenden Protokolle ausgeben kann: RTMP, interpolierten Streaming oder RTP (MPEG-Terminaldienste). 
    
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

>[AZURE.NOTE]Es ist sehr wichtig, nicht zu vergessen, einen Live-Kanal Codierung zu beenden. Achten Sie darauf, dass stündlich Einfluss für live Codierung Abrechnung und denken Sie daran, dass einen live codieren Kanal in den Status "Ausführen" verlassen Gebühren anfallen.  Es wird empfohlen, dass Ihre laufenden Kanäle sofort zu beenden, nachdem Ihr live streaming Ereignis zusätzliche stündliche Gebühren zu vermeiden abgeschlossen ist. 


##<a name="a-idchannelachannels-input-ingest-configurations"></a><a id="channel"></a>Channel Eingabe des (Aufnahme) Konfigurationen

###<a name="a-idingestprotocolsaingest-streaming-protocol"></a><a id="Ingest_Protocols"></a>Aufnahme streaming-Protokoll

Wenn Sie den **Typ der Encoder** auf **Standard**festgelegt ist, werden gültige Optionen:

- **RTP** (MPEG-Terminaldienste): MPEG-2 Transportstream über RTP.  
- Einzelne Bitrate **RTMP**
- Einzelne Bitrate **Fragmentiert MP4** (interpolierten Streaming)

####<a name="rtp-mpeg-ts---mpeg-2-transport-stream-over-rtp"></a>MPEG-2 Transportstream über RTP RTP (MPEG-Terminaldienste -).  

Typische Anwendungsfall-: 

Professionelle Sendeanstalten arbeiten in der Regel mit High-End-lokalen live Encoder von Herstellern wie elementares Technologien, Ericsson, Ateme, Imagine oder Envivio, einen Stream zu senden. Häufig verwendet in Verbindung mit einer IT-Abteilung und private Netzwerke.

Aspekte:

- Die Verwendung eines Streams ein einzelnes Programm Transport (SPTS) Eingabemethoden wird dringend empfohlen. 
- Sie können bis zu 8 audio-Streams mithilfe von MPEG-2-Terminaldienste über RTP eingeben. 
- Der Videodatenstrom sollte eine durchschnittliche Bitrate unter 15/s haben.
- Die durchschnittliche aggregieren Bitrate der audio Streams sollte unter 1/s liegen.
- Im folgenden sind die unterstützten Codecs:
    - MPEG-2 / H.262 Video 
        
        - Hauptfenster Profil (4:2:0)
        - Höchst-Profil (4:2:0, 4:2:2)
        - 422 Profil (4:2:0, 4:2:2)

    - MPEG-4 AVC / h. 264-Video  
    
        - Geplante, Primär, hoch Profil (8-Bit-Version 4:2:0)
        - Hohe 10 Profil (10 Bits 4:2:0)
        - Hohe 422 Profil (10 Bits 4:2:2)


    - MPEG-2 AAC-LC Audio 
    
        - Mono, Stereo, Kontrollkästchen (5.1, 7.1)
        - MPEG-2 Formatvorlage ADTS Verpackung

    - Audio für Dolby Digital (IK-3) 

        - Mono, Stereo, Kontrollkästchen (5.1, 7.1)

    - MPEG-Audio (Layer II und III) 
            
        - Mono, Stereo

- Empfohlene übertragen, die Encoder einbeziehen:
    - Ateme AM2102
    - Ericsson AVP2000
    - eVertz 3480
    - Ericsson RX8200
    - Stellen Sie sich vor der Kommunikation Selenio ENC 1
    - Stellen Sie sich vor der Kommunikation Selenio ENC 2
    - AdTec EN-30
    - AdTec EN - 91P
    - AdTec EN-100
    - Harmonische ProStream 1000
    - Thor H-2 m-4HD
    - eVertz 7880 SLKE
    - Cisco Spinnaker
    - Elementen Live

####<a name="a-idsinglebitratertmpasingle-bitrate-rtmp"></a><a id="single_bitrate_RTMP"></a>Einzelne Bitrate RTMP

Aspekte:

- Der eingehende Stream enthalten keine Multi-Bitrate Video.
- Der Videodatenstrom sollte eine durchschnittliche Bitrate unter 15/s haben.
- Der Audiodatenstrom sollte eine durchschnittliche Bitrate unter 1/s haben.
- Im folgenden sind die unterstützten Codecs:

- MPEG-4 AVC / h. 264-Video

- Geplante, Primär, hoch Profil (8-Bit-Version 4:2:0)
- Hohe 10 Profil (10 Bits 4:2:0)
- Hohe 422 Profil (10 Bits 4:2:2)

- MPEG-2 AAC-LC Audio

- Mono, Stereo, Kontrollkästchen (5.1, 7.1)
- Zins 44,1 kHz werden
- MPEG-2 Formatvorlage ADTS Verpackung

- Empfohlene Encoder umfassen:

- Telestream Wirecast
- Live Flash Media-Encoder
- Tricaster

####<a name="single-bitrate-fragmented-mp4-smooth-streaming"></a>Einzelner Bitrate fragmentiert MP4 (interpolierten Streaming)

Typische Anwendungsfall-:

Verwenden lokaler live Encoder von Herstellern wie elementares Technologien, Ericsson, Ateme, Envivio Eingabe-Stream über das Internet öffnen in einem in der Nähe Azure senden Datacenter.

Aspekte:

Denen für [einzelne Bitrate RTMP](media-services-manage-live-encoder-enabled-channels.md#single_bitrate_RTMP)identisch.

####<a name="other-considerations"></a>Weitere Überlegungen

- Sie können das Eingabewerte Protokoll während der Kanal nicht ändern oder dessen zugeordneten Programme ausgeführt werden. Wenn Sie andere Protokolle benötigen, sollten Sie separate Kanäle für jedes Eingabewerte Protokoll erstellen.
- Maximale Auflösung für eingehende Videostreams ist 1920 x 1080, und höchstens 60 Felder/Sekunde Interlaced oder 30 Frames/Sekunde Wenn schrittweisen.


###<a name="ingest-urls-endpoints"></a>Aufnahme URLs (Endpunkte)

Ein Kanal stellt von außen liegenden Tabellenblättern (Aufnahme URL), dass Sie in der live Encoder angeben, damit der Encoder Pushbenachrichtigungen kann Streams auf Ihre Kanäle.

Sie können die Erfassung URLs erhalten, nachdem Sie einen Kanal erstellen. Um diese URLs zu gelangen, muss der Kanal nicht in den Zustand **ausgeführt** werden. Wenn Sie zum Senden der Daten in den Kanal starten bereit sind, müssen sie in den Zustand **ausgeführt** . Nachdem der Kanal Aufnahme Daten startet, können Sie Ihre Stream über die URL der Vorschau Vorschau anzeigen.

Sie haben die Möglichkeit der Aufnahme fragmentiert MP4 live Stream über SSL-Verbindung (interpolierten Streaming). Vergewissern Sie sich die Erfassung URL auf HTTPS aktualisieren Schritte aus, um über SSL Aufnahme.

###<a name="allowed-ip-addresses"></a>Zugelassene IP-Adressen

Sie können die IP-Adressen definieren, die zum Veröffentlichen von Video zu diesem Channel zulässig ist. Zulässige IP-Adressen können als eine einzelne IP-Adresse (z. B. ' 10.0.0.1'), IP-Bereich verwenden eine IP-Adresse und eine CIDR Subnetzmaske (z. B. ' 10.0.0.1/22'), oder verwenden eine IP-Adresse und eine gepunktete dezimale Subnetzmaske IP-Bereich angegeben werden (z. B. ' 10.0.0.1(255.255.252.0)').

Wenn keine IP-Adressen angegeben sind, und es keine Regeldefinition ist wird keine IP-Adresse zulässig. Um eine beliebige IP-Adresse zulassen möchten, erstellen Sie eine Regel und 0.0.0.0/0 festgelegt.


##<a name="channel-preview"></a>Channel preview

###<a name="preview-urls"></a>Vorschau-URLs

Kanäle bieten einen Endpunkt Vorschau (Preview-URL), den Sie in der Vorschau und überprüfen Ihre Stream, bevor weitere Verarbeitung und Übermittlung verwenden.

Im Vorschau-URL gelangen, wenn Sie den Kanal erstellen. Um die URL zu gelangen, muss der Kanal nicht in den Zustand **ausgeführt** werden.

Nachdem der Kanal Aufnahme Daten startet, können Sie Ihre Stream Vorschau anzeigen.

>[AZURE.NOTE]Aktuell Streams Vorschau nur in fragmentiert MP4 übermittelt werden kann (interpolierten Streaming)-Format, unabhängig von den angegebenen Eingabewerte Typ. Im Player [http://smf.cloudapp.net/healthmonitor](http://smf.cloudapp.net/healthmonitor) können Sie um den interpolierten Stream zu testen. Einen Player gehostet Azure-Portal können Sie auch Ihre Stream anzeigen.

###<a name="allowed-ip-addresses"></a>Zugelassene IP-Adressen

Sie können die IP-Adressen definieren, die Verbindung zu den Endpunkt Vorschau zulässig ist. Wenn keine IP-Adressen angegeben sind, kann jede beliebige IP-Adresse. Zulässige IP-Adressen können als eine einzelne IP-Adresse (z. B. ' 10.0.0.1'), IP-Bereich verwenden eine IP-Adresse und eine CIDR Subnetzmaske (z. B. ' 10.0.0.1/22'), oder verwenden eine IP-Adresse und eine gepunktete decimal Subnetzmaske IP-Bereich angegeben werden (z. B. ' 10.0.0.1(255.255.252.0)').

##<a name="live-encoding-settings"></a>Live-Codierung Einstellungen

In diesem Abschnitt beschrieben, wie die Einstellungen für den live Encoder innerhalb der Kanal angepasst werden können, wenn der **Typ codieren** von einem Kanal **Standard**festgelegt ist.

>[AZURE.NOTE]Wenn mehrere Sprachspuren eingeben und live Codierung mit Azure ausführen, wird nur RTP mehrsprachigen Eingaben unterstützt. Sie können bis zu 8 audio-Streams mithilfe von MPEG-2-Terminaldienste über RTP definieren. Mehrere audio Spuren mit RTMP oder interpolierten streaming Aufnahme wird derzeit nicht unterstützt. Beim Ausführen live Codierung mit [lokalen live codiert](media-services-live-streaming-with-onprem-encoders.md), es gibt keine solche Einschränkung, da welchen an AMS gesendet wird durch einen Kanal verläuft, ohne die weitere Verarbeitung.

###<a name="ad-marker-source"></a>AD-Marker-Quelle

Sie können die Quelle für die Ad Datenpunkten Signale angeben. Standardwert ist **Api**, was bedeutet, dass der live Encoder innerhalb der Kanal an eine asynchrone **Ad-Marker-API**überwachen soll.

Die anderen gültige Option ist **Scte35** (nur zulässig, wenn die Erfassung streaming-Protokoll RTP (MPEG-Terminaldienste) festgelegt ist. Wenn Scte35 angegeben ist, wird der live Encoder SCTE-35 Signale aus dem Eingabewerte RTP (MPEG-Terminaldienste) Stream analysiert werden.

###<a name="cea-708-closed-captions"></a>CEA 708 Untertitel

Eine optionalen kennzeichnen, den live Encoder CEA 708 Beschriftungen Daten ignorieren weist, eingebettet in das eingehende Video. Wenn die Kennzeichnung auf falsch (Standard) festgelegt ist, wird der Encoder erkennen und setzen Sie wieder CEA 708 Daten in der Ausgabe Videodatenströme.

###<a name="video-stream"></a>Videodatenstrom

Optional. Beschreibt den Eingabewerte Videodatenstrom an. Wenn dieses Feld nicht angegeben wird, ist der Standardwert verwendet. Diese Einstellung ist nur zulässig, wenn die Eingabe streaming-Protokoll RTP (MPEG-Terminaldienste) festgelegt ist.

####<a name="index"></a>Index

Ein nullbasierter Index, der angibt, welche Eingabewerte Videodatenstrom vom live Encoder innerhalb der Kanal verarbeitet werden sollen. Diese Einstellung gilt nur, wenn Aufnahme streaming-Protokoll RTP (MPEG-Terminaldienste) ist.

Standardwert ist 0 (null). Es wird empfohlen, in ein einzelnes Programm Transportstream (SPTS) zu senden. Eingabe-Stream mehrere Programme enthält, live Encoder analysiert Programm Karte Tabelle (Rmz) in der Eingabe, die Eingaben, deren Stream Typnamen MPEG-2-Video oder h. 264 identifiziert und ordnet diese in der Reihenfolge, in der Zahl angegeben sind Der nullbasierte Index wird dann zum Auswählen von der n-ter Eintrag bei dieser Festlegung.

###<a name="audio-stream"></a>Audiodatenstrom

Optional. Beschreibt die Eingabewerte audio-Streams. Wenn dieses Feld nicht angegeben wird, wenden Sie angegebenen Standardwerte aus. Diese Einstellung ist nur zulässig, wenn die Eingabe streaming-Protokoll RTP (MPEG-Terminaldienste) festgelegt ist.

####<a name="index"></a>Index

Es wird empfohlen, in ein einzelnes Programm Transportstream (SPTS) zu senden. Eingabe-Stream mehrere Programme enthält, der live Encoder innerhalb der Kanal analysiert Programm Karte Tabelle (Rmz) in der Eingabe, identifiziert die Eingaben, deren Stream Typnamen MPEG-2 AAC ADTS oder IK-3-System-A oder IK-3-System-B oder MPEG-2 Private PE-Dateien oder MPEG-1 Audio oder MPEG-2-Audio und ordnet diese in der Reihenfolge, in der Zahl angegeben sind Der nullbasierte Index wird dann zum Auswählen von der n-ter Eintrag bei dieser Festlegung.

####<a name="language"></a>Sprache

Die Sprach-ID den Audiodatenstrom, ISO 639-2, wie z. B. eng entsprechen Wenn Sie nicht vorhanden ist, ist der Standardwert UND (nicht definiert).

Es kann bis zu 8 Audiodatenstrom Sätze angegeben wird, wenn die Eingabe für den Kanal über RTP MPEG-2-Terminaldienste ist. Es kann jedoch keine zwei Einträge mit demselben Wert des Indexes.

###<a name="a-idpresetasystem-preset"></a><a id="preset"></a>System Voreinstellung

Gibt die Vorgabe vom live Encoder innerhalb dieser Kanal verwendet werden. Derzeit ist der einzige zulässige Wert **Default720p** (Standard).

Beachten Sie, dass, wenn Sie benutzerdefinierte Voreinstellungen benötigen, Sie Amslived unter Microsoft.com kontaktieren.

**Default720p** wird das Video in den folgenden 7 Ebenen codiert werden.


####<a name="output-video-stream"></a>Die Ausgabe Videodatenstrom

BitRate|Breite|Höhe|MaxFPS|Profil|Name des Streams Ausgabe
---|---|---|---|---|---
3500|1280|720|30|Hohe|Video_1280x720_3500kbps
2200|960|540|30|Main|Video_960x540_2200kbps
1350|704|396|30|Main|Video_704x396_1350kbps
850|512|288|30|Main|Video_512x288_850kbps
550|384|216|30|Main|Video_384x216_550kbps
350|340|192|30|Geplante|Video_340x192_350kbps
200|340|192|30|Geplantes|Video_340x192_200kbps


####<a name="output-audio-stream"></a>Audiodatenstrom Ausgabe

Audio wird in Stereo AAC-LC bei 64/s, werden Zinsfuß 44,1 kHz codiert.

##<a name="signaling-advertisements"></a>Auslösen der anzeigen

Wenn Ihre Channel Live Codierung aktiviert hat, Sie verfügen über eine Komponente in der Verkaufspipeline, für die Verarbeitung von Video und es bearbeiten. Sie können für den Kanal zum Einfügen von Slate und/oder Werbung in ausgehenden adaptive Bitrate Streams signal an. Slate werden weiterhin Bilder, die Sie verwenden können, um den Eingabewerte live-Feed in bestimmten Fällen (beispielsweise während einer professionellen Seitenumbruch) verdecken. Werbung Signale, sind die Uhrzeit synchronisiert Signale, die Sie in der ausgehenden Stream, um die Videoplayer auf spezielle – beispielsweise bezüglich zum Umschalten auf eine Werbung zum entsprechenden Zeitpunkt feststellen einbetten. Finden Sie in diesem [Blog](https://codesequoia.wordpress.com/2014/02/24/understanding-scte-35/) einen Überblick über die SCTE-35 Signalisierungsmechanismus für diesen Zweck verwendet. Im folgenden finden Sie eine typische Szenario, die Sie in Ihrer live Ereignis implementieren konnte.

1. Haben Sie die Betrachter ein Bild vor dem Ereignis zu erhalten, bevor das Ereignis beginnt.
1. Haben Sie die Betrachter ein Bild nach dem EVENT erhalten, nachdem das Ereignis endet.
1. Haben Sie die Betrachter ein Ereignis Fehler Bild zu erhalten, wenn ein Problem, während das Ereignis (beispielsweise Power Fehler in der Stadion vorliegt).
1. Senden eines Bilds AD-SEITENUMBRUCH das live Ereignis während einer professionellen Seitenumbruch feed ausgeblendet.

Nachfolgend werden die Eigenschaften, die Sie beim Auslösen der Werbung festlegen können. 

###<a name="duration"></a>Dauer

Die Dauer in Sekunden, der die kommerzielle Seitenumbruch. Dies hat einen positiven Wert ungleich NULL sein, um die kommerzielle Pause zu starten. Wenn ein professioneller Umbruch ist, wird ausgeführt, und die Dauer wird mit dem CueId NULL gesetzt Abgleich der fortlaufenden kommerzielle Zeilenumbruch, und klicken Sie dann die Pause storniert werden.

###<a name="cueid"></a>CueId

Eine eindeutige ID für den professionellen Seitenumbruch entsprechenden Aktion(en) Ausführen von untergeordneten Anwendung verwendet werden. Muss eine positive ganze Zahl sein. Legen Sie diesen Wert auf eine zufällige positive ganze Zahl oder einem übergeordneten System zum Nachverfolgen der Hinweis Ids verwenden können. Stellen Sie sicher, dass alle Ids zu positive ganze Zahlen normalisieren vor dem senden, über die API.

###<a name="show-slate"></a>Slate anzeigen

Optional. Zeigt den live Encoder wechseln zu dem Bild [Standard Slate](media-services-manage-live-encoder-enabled-channels.md#default_slate) während einer professionellen Seitenumbruch und Ausblenden des eingehende video-Feeds an. Audio ist auch während Slate stumm geschaltet. Standardmäßig ist " **false**". 
 
Das Bild verwendet wird über die standardmäßigen Blattes Anlage ID-Eigenschaft zum Zeitpunkt der Erstellung Kanal angegeben sein. Die Slate wird für die Anzeige Bildgröße gestreckt werden. 


##<a name="insert-slate--images"></a>Fügen Sie Bilder Slate

Wechseln Sie zu einem Bild Blattes kann live Encoder innerhalb der Kanal signalisierende. Es kann auch signalisiert werden, um eine laufende Slate zu beenden. 

Der live Encoder kann wechseln Sie zu einem Bild Blattes und das eingehende Videosignal in bestimmten Fällen – beispielsweise während einer Ad-Unterbrechung ausblenden konfiguriert werden. Wenn ein solcher Slate nicht so konfiguriert ist, wird von Video während dieser Ad Seitenumbruch nicht maskierte.

###<a name="duration"></a>Dauer

Die Dauer der Slate in Sekunden. Dies hat einen positiven Wert ungleich NULL sein, um die Slate starten. Wenn eine laufende Slate vorhanden ist, und eine Dauer von NULL angegeben ist, wird die fortlaufenden Slate beendet.

###<a name="insert-slate-on-ad-marker"></a>Einfügen von Slate auf Ad-marker

Beim Festlegen auf "True" Diese Einstellung den live Encoder zum Einfügen eines Bilds Blattes während einer Ad-Unterbrechung konfiguriert. Der Standardwert ist wahr. 

###<a name="a-iddefaultslateadefault-slate-asset-id"></a><a id="default_slate"></a>Standard-Blattes Aktiva-Id

Optional. Gibt die Objekt-Id der Anlage Services Medien die das Bild des Blattes enthält. Standardmäßig ist null. 

**Hinweis**: vor dem Erstellen des Kanals, das Blattes Bild mit den folgenden Einschränkungen als eine dedizierte Anlage (keine anderen Dateien sollten in dieser Anlage) hochgeladen werden soll. 

- Höchstens 1920 x 1080 in Auflösung.
- In den meisten 3 MB groß.
- Der Dateiname muss eine *.jpg-Erweiterung haben.
- Das Bild muss in einer Anlage als die einzige AssetFile hochgeladen werden, dieses Objekt und diese AssetFile als die primäre Datei gekennzeichnet werden soll. Die Anlage kann nicht Speicher verschlüsselt werden.

Wenn die **standardmäßige slate Aktiva-Id** nicht angegeben wird, und **Slate auf Ad-Marker einfügen** auf **true**festgelegt ist, wird ein Standardbild für Azure Media Services zum Ausblenden von Videostreams verwendet werden. Audio ist auch während Slate stumm geschaltet. 


##<a name="channels-programs"></a>Der Channel-Programme

Ein Kanal ist zugeordnet Programme, mit die Sie die Veröffentlichung und Speicherung von Segmente in einem live Stream steuern können. Kanäle verwalten Programme. Die Beziehung Channel und Programm ist sehr ähnlich wie herkömmliche Medien, wo ein Kanal hat einen Konstanten Videodatenstrom von Inhalten und ein Programm ist auf einigen terminierten Ereignis in diesem Kanal ausgelegte.

Sie können angeben, die Anzahl der Stunden, die den aufgezeichneten Inhalt für das Programm durch Festlegen der Länge **Archiv Fenster** beibehalten werden sollen. Dieser Wert kann maximal 25 Stunden aus mindestens 5 Minuten festgelegt werden. Archivieren Fensterlänge bestimmt auch, dass die maximale Größe des Zeit-Clients aus der die aktuelle Position live zeitlich rückwärts anfordern kann. Programme ausgeführt werden können, über den festgelegten Zeitraum, aber Inhalt, der hinter dem Fensterlänge fällt wird kontinuierlich verworfen. Dieser Wert dieser Eigenschaft bestimmt auch an, wie lange die Client-Manifeste wachsen können.

Jedes Programm ist eine Anlage als Stream gesendeten Inhalt gespeichert zugeordnet. Eine Anlage zu einem Container Blob im Speicher Azure-Konto zugeordnet ist, und die Dateien in der Anlage werden als Blobs im Container gespeichert. Um das Programm zu veröffentlichen, damit Ihre Kunden Streams Weg anzeigen können, müssen Sie einen auf-Anforderung Locator für die zugeordnete Anlage erstellen. Haben Sie diese Locator aktivieren Sie streaming URL zu erstellen, die für Kunden bereitgestellt werden können.

Ein Kanal unterstützt bis zu drei gleichzeitig ausgeführte Programme an, sodass Sie mehrere Archiven des gleichen eingehende Streams erstellen können. So können Sie veröffentlichen und anderen Teile eines Ereignisses archivieren, je nach Bedarf. Beispielsweise ist Ihre geschäftliche Anforderung 6 Stunden eines Programms archivieren, sondern nur in den letzten 10 Minuten übertragen. Dazu müssen Sie zwei gleichzeitig ausgeführte Programme zu erstellen. Archivieren von 6 Stunden des Ereignisses ein Programm festgelegt ist, aber das Programm wird nicht veröffentlicht. Das andere Programm für 10 Minuten archivieren festgelegt ist, und dieses Programm veröffentlicht wird.

Vorhandene Programme für neue Ereignisse sollten Sie nicht wieder verwenden. In diesem Fall erstellen und Starten eines neuen Programms für jedes Ereignis, wie im Abschnitt Programming Live Streaming Applications beschrieben.

Starten Sie das Programm, wenn Sie streaming und Archivierung starten möchten. Beenden Sie das Programm immer, wenn Sie streaming und das Ereignis Archivierung beenden möchten. 

Zum Löschen der archivierten Inhalte beenden Sie und löschen Sie das Programm, und löschen Sie die zugeordnete Ressource. Eine Anlage kann gelöscht werden, wenn es von einem Programm verwendet wird. das Programm muss zuerst gelöscht werden. 

Sogar nachdem Sie beenden und löschen das Programm, würde der Benutzer können Ihre archivierten Inhalte als Video on Demand zur übertragen, solange Sie nicht die Anlage löschen.

Wenn Sie möchten archivierte Inhalte beibehalten, aber nicht verfügbar für das streaming, löschen Sie den streaming Locator.


##<a name="getting-a-thumbnail-preview-of-a-live-feed"></a>Erste Miniaturansicht der ein Feed live

Wenn Live Codierung aktiviert ist, können Sie nun eine Vorschau des Feeds live erhalten, bei den Kanal eintreffen. Dies kann ein wertvolles Tool prüfen, ob Ihr live Feed tatsächlich den Kanal erreichen ist sein. 

##<a name="a-idstatesachannel-states-and-how-states-map-to-the-billing-mode"></a><a id="states"></a>Channel Staaten und Staaten wie in den Modus Abrechnung zuordnen 

Der aktuelle Status eines Channel. Mögliche Werte sind:

- **Nicht mehr**. Dies ist der anfänglicher Status des den Kanal nach der Erstellung aus. In diesem Zustand die Channel-Eigenschaften können aktualisiert werden, aber streaming ist nicht zulässig.
- **Starten**. Der Kanal wird gestartet. Keine Updates oder streaming während dieser Status zulässig ist. Falls ein Fehler auftritt, gibt der Kanal in den Zustand beendet.
- **Ausgeführt wird**. Der Kanal ist Lage live Streams zu verarbeiten.
- **Beenden**. Der Kanal wird gestoppt. Keine Updates oder streaming während dieser Status zulässig ist.
- **Löschen**. Der Kanal wird gelöscht. Keine Updates oder streaming während dieser Status zulässig ist.

Die folgende Tabelle zeigt die Channel Staaten wie in den Modus Abrechnung zuordnen. 
 
Channel Zustand|Portal Benutzeroberfläche Indikatoren|In Rechnung gestellt?
---|---|---
Starten|Starten|Keine (vorübergehenden Zustand)
Ausführen|Sofort (keine ausgeführte Programme)<br/>oder<br/>Streaming (mindestens ein Programm ausführt)|Ja
Beenden|Beenden|Keine (vorübergehenden Zustand)
Beendet|Beendet|Nein


>[AZURE.NOTE] Aktuell, der Mittelwert der Channel Start etwa 2 Minuten ist, aber manchmal kann bis zu 20 + Minuten dauern. Zurücksetzen von Kennwörtern Channel können bis zu 5 Minuten dauern.


##<a name="a-idconsiderationsaconsiderations"></a><a id="Considerations"></a>Aspekte

- Wenn ein Kanal **Standard** Codierung Typ einen Verlust der Eingabesprache auftritt Quelle/Beitrag-Feeds, es sorgt dafür, durch die Quelle Video oder Audio mit einem Fehler Slate und Pausen ersetzen. Der Kanal weiterhin ein Slate ausgeben, bis der Eingabe/Beitrag Lebensläufe-feed. Es empfiehlt sich, dass ein live-Kanal nicht in einen solchen Status für mehr als 2 Stunden bleiben. Neben diesem Punkt das Verhalten der Kanal am Eingabewerte erneute Verbindung ist nicht gewährleistet, weder ist ihr Verhalten in Antwort auf einen Zurücksetzungsbefehl. Sie müssen den Kanal beenden, löschen und Erstellen eines neuen Kontos.
- Sie können das Eingabewerte Protokoll während der Kanal nicht ändern oder dessen zugeordneten Programme ausgeführt werden. Wenn Sie andere Protokolle benötigen, sollten Sie separate Kanäle für jedes Eingabewerte Protokoll erstellen.
- Jedes Mal, wenn Sie den live Encoder neu zu konfigurieren, rufen Sie die Methode zum **Zurücksetzen** auf dem Kanal. Bevor Sie den Kanal zurückzusetzen, müssen Sie das Programm beenden. Nachdem Sie den Kanal zurückgesetzt, starten Sie das Programm erneut.
- Ein Kanal kann abgebrochen werden nur, wenn es in den Zustand "aktiv" ist, und alle Programme auf dem Kanal wurde beendet.
- Standardmäßig können Sie nur 5 Kanäle bei Ihrem Konto Media-Dienste hinzufügen. Hierbei handelt es sich um eine weiche Kontingent für alle neuen Konten. Weitere Informationen finden Sie unter [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md).
- Sie können das Eingabewerte Protokoll während der Kanal nicht ändern oder dessen zugeordneten Programme ausgeführt werden. Wenn Sie andere Protokolle benötigen, sollten Sie separate Kanäle für jedes Eingabewerte Protokoll erstellen.
- Sie werden nur berechnet, wenn Ihre Channel im Zustand **ausgeführt** wird. Weitere Informationen finden Sie in [diesem](media-services-manage-live-encoder-enabled-channels.md#states) Abschnitt.
- Die maximal zulässige empfohlene Dauer eines Ereignisses live ist aktuell, 8 Stunden. Wenden Sie sich an Amslived unter Microsoft.com, wenn Sie einen Kanal für längere Zeiträume ausführen müssen.
- Stellen Sie sicher, dass mindestens eine streaming reservierte Einheit für das streaming Endpunkt von der zum Streaming von Inhalten aus.
- Wenn mehrere Sprachspuren eingeben und live Codierung mit Azure ausführen, wird nur RTP mehrsprachigen Eingaben unterstützt. Sie können bis zu 8 audio-Streams mithilfe von MPEG-2-Terminaldienste über RTP definieren. Mehrere audio Spuren mit RTMP oder interpolierten streaming Aufnahme wird derzeit nicht unterstützt. Beim Ausführen live Codierung mit [lokalen live codiert](media-services-live-streaming-with-onprem-encoders.md), es gibt keine solche Einschränkung, da welchen an AMS gesendet wird durch einen Kanal verläuft, ohne die weitere Verarbeitung.
- Die Codierung Voreinstellung wird den Begriff "max Frame-Rate" von 30 Frames / verwendet. In diesem Fall ist die Eingabe 60 f/s / 59.97i, die Eingabewerte Frames werden verworfen/Deserialisieren-interlaced auf 30/29,97 fps. Wenn die Eingabe 50 f/s/50i ist, sind die Eingabewerte Frames gelöscht/Deserialisieren-interlaced zu 25 f/s. Wenn die Eingabe 25 f/s ist, bleibt Ausgabe mit 25 fps.
- Vergessen Sie nicht die Option zum Beenden des Kanäle anschließend. Wenn Sie diese nicht wünschen, weiterhin Abrechnung.

##<a name="known-issues"></a>Bekannte Probleme

- Channel starten, um Zeit in einen Mittelwert von 2 Minuten wurde verbessert, doch manchmal erhöhten Bedarf konnte weiterhin Minuten dauern, bis zu 20 +.
- RTP-Unterstützung wird in Richtung professionell Sendeanstalten zur Verfügung gestellt. Überprüfen Sie die Notizen in RTP in [diesem](https://azure.microsoft.com/blog/2015/04/13/an-introduction-to-live-encoding-with-azure-media-services/) Blog aus.
- Blattes Bilder sollte Einschränkungen beschrieben [hier](media-services-manage-live-encoder-enabled-channels.md#default_slate)entsprechen. Wenn Sie versuchen, erstellen Sie einen Kanal mit einer standardmäßigen Slate, die größer als 1920 x 1080 ist, wird die Anfrage später Fehler ausgegeben.
- Erneut... nicht auf die Option zum Beenden des Kanäle vergessen, wenn Sie fertig sind streaming. Wenn Sie diese nicht wünschen, weiterhin Abrechnung.

###<a name="how-to-create-channels-that-perform-live-encoding-from-a-singe-bitrate-to-adaptive-bitrate-stream"></a>So erstellen Sie Kanäle, die aus einem einzigen Bitrate in adaptive Bitrate Stream ausführen live Codierung

Wählen Sie **Portal**, **.NET**, **REST-API** , um Informationen zum Erstellen und verwalten Kanäle und Programme aus.

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET SDK](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST-API](https://msdn.microsoft.com/library/azure/dn783458.aspx)


##<a name="next-step"></a>Als Nächstes

Überprüfen Sie die Pfade learning Media-Dienste.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-topics"></a>Verwandte Themen

[Beim Senden von Live Streaming Ereignissen mit Azure Media-Dienste](media-services-overview.md)

[Media-Dienste (Konzepte)](media-services-concepts.md)

[Azure Media Services fragmentierte MP4 Live Aufnahme Spezifikation](media-services-fmp4-live-ingest-overview.md)

[live-overview]: ./media/media-services-manage-live-encoder-enabled-channels/media-services-live-streaming-new.png

