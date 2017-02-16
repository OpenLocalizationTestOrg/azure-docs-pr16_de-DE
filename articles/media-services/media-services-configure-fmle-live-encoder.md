<properties 
    pageTitle="Konfigurieren den FMLE Encoder zum Senden eines einzelnen Bitrate live Streams | Microsoft Azure" 
    description="In diesem Thema wird gezeigt, wie konfigurieren den Encoder Flash Medien Live Encoder (FMLE) um einen einzelnen Bitrate Stream an AMS Kanäle zu senden, die für die live-Codierung aktiviert sind." 
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
    ms.author="juliako;cenkdin;anilmur"/>

#<a name="use-the-fmle-encoder-to-send-a-single-bitrate-live-stream"></a>Verwenden der FMLE Encoder zu einen einzelnen Bitrate live Stream senden

> [AZURE.SELECTOR]
- [FMLE](media-services-configure-fmle-live-encoder.md)
- [Live Elementen](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)

In diesem Thema wird gezeigt, wie konfigurieren den Encoder [Live Media-Encoder Flash](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) um einen einzelnen Bitrate Stream an AMS Kanäle zu senden, die für die live-Codierung aktiviert sind. Weitere Informationen finden Sie unter [Arbeiten mit Kanäle, die zum Ausführen mit Azure Media Services Codierung Live aktiviert sind](media-services-manage-live-encoder-enabled-channels.md).

In diesem Lernprogramm erfahren zum Verwalten von Azure Media Services (AMS) mit dem Tool Azure Media Services Explorer (AMSE). Dieses Tool kann nur auf Windows-Computer ausgeführt werden. Wenn Sie Mac oder Linux sind, verwenden Sie das Azure-Portal [Kanäle](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) und [Programme](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program)zu erstellen.

Beachten Sie, dass dieses Lernprogramms beschreibt das AAC verwenden. FMLE unterstützt nicht jedoch AAC standardmäßig. Sie müssen ein Plug-In für AAC Codierung eine solche von MainConcept kaufen: [AAC-Plug-Ins](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)

##<a name="prerequisites"></a>Erforderliche Komponenten

- [Erstellen Sie ein Konto Azure Media Services](media-services-portal-create-account.md)
- Stellen Sie sicher, es ist eine Streaming-Endpunkt mit mindestens eine streaming Einheit zugeordnet wird ausgeführt. Weitere Informationen finden Sie unter [Verwalten von Streaming Endpunkte in einer Media Services-Kontos](media-services-portal-manage-streaming-endpoints.md)
- Installieren Sie die neueste Version des Tools [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) .
- Starten Sie das Tool, und Verbinden mit Ihrem Konto AMS.

##<a name="tips"></a>Tipps

- Verwenden Sie nach Möglichkeit eine Verbindung zum Internet gehört.
- Eine gute Faustregel, wenn die Bandbreite Anforderungen bestimmen besteht darin, das streaming eine Bitrate doppelklicken. Während Sie dies nicht obligatorisch erforderlich ist, wird es hilfreich sein, den Einfluss der Überlastung des Netzwerks zu verringern.
- Bei der Verwendung von Software Grundlage Encoder, schließen Sie alle nicht benötigten Programme.

## <a name="create-a-channel"></a>Erstellen Sie einen Kanal

1.  Klicken Sie im Tool AMSE navigieren Sie zur Registerkarte **Live** , und klicken Sie mit der rechten Maustaste in den Bereich Channel. Wählen Sie **Erstellen Kanal...** aus dem Menü aus.

![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. Geben Sie einen Channel an das Beschreibungsfeld ist optional. Kanal aktivieren Sie unter Einstellungen **Standard** für die Codierung Live die Option, mit der Eingabe-Protokoll auf **RTMP**festgelegt. Lassen Sie andere Einstellungen wie ist ein.


Stellen Sie sicher, dass die **Starten Sie den neuen Kanal jetzt** ausgewählt ist.

3. Klicken Sie auf **Channel erstellen**.
![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

>[AZURE.NOTE] Der Kanal kann bis zu 20 Minuten zum Starten dauern.


Beim Start von des Kanals können Sie [den Encoder konfigurieren](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).

>[AZURE.IMPORTANT] Beachten Sie, dass Abrechnung wird gestartet, sobald Channel in einen bereiten Zustand wechselt. Weitere Informationen finden Sie unter [den Channel Staaten](media-services-manage-live-encoder-enabled-channels.md#states).

##<a name="a-idconfigurefmlertmpaconfigure-the-fmle-encoder"></a><a id=configure_fmle_rtmp></a>Konfigurieren des FMLE Encoders

In diesem Lernprogramm werden die folgenden ausgabeeinstellungen verwendet. Im weiteren Verlauf dieses Abschnitts werden Konfigurationsschritte ausführlicher beschrieben. 

**Video**:
 
- Codec: h. 264. 
- Profil: Hoch (Ebene 4.0) 
- Bitrate: 5000/s 
- Schlüsselbild: 2 Sekunden (60 Sekunden) 
- Frame Rate: 30
 
**Audio**:

- Codec: AAC (LC) 
- Bitrate: 192/s 
- Sample-Rate: 44,1 kHz


###<a name="configuration-steps"></a>Konfigurationsschritte

1. Navigieren Sie zu der Flash Media Live Encoder des (FMLE) Schnittstelle auf dem Computer verwendet wird.

    Die Benutzeroberfläche ist eine Hauptseite Einstellungen. Beachten Sie bitte die folgenden empfohlenen Einstellungen Schritte bei streaming FMLE verwenden.
    
    - Format: H. 264 Frame-Rate: 30,00 
    - Eingabe Größe: 1280 x 720 
    - Bitrate: 5000/s (können basierend auf Netzwerk Einschränkungen angepasst werden)  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

    Wenn Quellen mit Zeilensprünge möglich, melden Sie Häkchen die Option ""Zusammenfügen ""

2. Wählen Sie das Schraubenschlüsselsymbol neben Format, diese zusätzlichen Einstellungen sollten:

    - Profil: Main
    - Ebene: 4.0
    - Keyframe Häufigkeit: 2 Sekunden 
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)

3. Legen Sie die folgende wichtige audio-Einstellung:
    
    - Format: AAC. 
    - Sample-Rate: 44100 Hz
    - Bitrate: 192/s
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)

6. Erhalten einer URL im Kanal um die FMLEs **RTMP Endpunkt**zugewiesen.
    
    Navigieren Sie zurück zu den AMSE-Tool, und klicken Sie auf den Kanalstatus aktivieren. Sobald der Zustand zum **Ausführen**von **Felder starten** geändert hat, können Sie die eingegebene URL abrufen.
      
    Wenn der Kanal ausgeführt wird, klicken Sie mit der rechten Maustaste auf der Channelname, navigieren Sie nach unten bis zum Hover über **Eingabe-URL in Zwischenablage kopieren** , und wählen Sie **Primäre Eingabe URL**aus.  
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)

7. Fügen Sie diese Informationen im Feld **FMS-URL** des Abschnitts Ausgabe, und weisen Sie einen Namen für die Stream. 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    Wiederholen Sie diese Schritte mit der sekundäre Eingabe URL, zusätzliche Redundanzgründen.
8. Wählen Sie **eine Verbindung herstellen**.

>[AZURE.IMPORTANT]Bevor Sie **Verbinden**geklickt haben, sicherstellen **müssen** Sie, dass der Kanal bereit. 
>Stellen Sie außerdem sicher nicht zu den Kanal Status bereit ohne eine Eingabe Beitrag länger als 15 Minuten > feed zu verlassen.

##<a name="test-playback"></a>Der Testwiedergabe
  
1. Navigieren Sie zu dem Tool AMSE, und klicken Sie mit der rechten Maustaste den Kanal getestet werden. Wählen Sie im Menü zeigen Sie auf **Wiedergabe der Vorschau** , und wählen Sie **mit Azure Media Player**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

Wenn Streams im Player angezeigt wird, hat der Encoder ordnungsgemäß konfiguriert wurde, zum Verbinden mit AMS. 

Wenn eine Fehlermeldung angezeigt wird, wird der Kanal zurückgesetzt werden müssen, und Encoder Einstellungen angepasst. Finden Sie im Thema [zur Problembehandlung](media-services-troubleshooting-live-streaming.md) für Anleitungen.  

##<a name="create-a-program"></a>Erstellen Sie ein Programm

1. Sobald Channel Wiedergabe bestätigt ist, erstellen Sie ein Programm aus. Klicken Sie auf der Registerkarte **Live** im Tool AMSE klicken Sie mit der rechten Maustaste in den Bereich Programm, und wählen Sie **Neues Programm erstellen**aus.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)

2. Benennen Sie die Anwendung und, falls erforderlich, passen Sie die **Länge der Archiv-Fenster** (die 4 Stunden standardmäßig verwendet). Sie können auch Geben Sie einen Speicherort oder als Standard lassen.  
3. Aktivieren Sie das Kontrollkästchen **Starten Sie das Programm jetzt** ein.
4. Klicken Sie auf **Programm erstellen**.  
  
    Hinweis: Die Erstellung des weniger Zeit als Channel erstellen.    
 
5. Nachdem das Programm ausgeführt wird, bestätigen Sie Wiedergabe von rechten Maustaste auf das Programm und navigieren zu der **Wiedergabe der Programme** aus, und klicken Sie dann **mit Azure Media Player**auswählen.  
6. Nachdem bestätigt, nach rechts klicken Sie auf das Programm erneut und wählen Sie aus **den Ausgabe-URL in Zwischenablage kopieren** (oder rufen Sie diese Informationen über die Option **Programminformationen und Einstellungen** im Menü ab). 

Der Stream ist jetzt bereit sind, werden in einem Player eingebettet oder an einer Zielgruppe, für die Anzeige im live verteilt.  


## <a name="troubleshooting"></a>Behandlung von Problemen

Finden Sie im Thema [zur Problembehandlung](media-services-troubleshooting-live-streaming.md) für Anleitungen. 


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
