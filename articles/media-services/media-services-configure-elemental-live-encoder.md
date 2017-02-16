<properties 
    pageTitle="Konfigurieren den Encoder elementares Live zum Senden eines einzelnen Bitrate live Streams | Microsoft Azure" 
    description="In diesem Thema wird gezeigt, wie konfigurieren den Encoder elementares Live, um einen einzelnen Bitrate Stream an AMS Kanäle zu senden, die für die live-Codierung aktiviert sind." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="cenkdin;anilmur;juliako"/>

#<a name="use-the-elemental-live-encoder-to-send-a-single-bitrate-live-stream"></a>Verwenden der Encoder elementares Live an einen einzelnen Bitrate live Stream senden

> [AZURE.SELECTOR]
- [Live Elementen](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

In diesem Thema wird gezeigt, wie konfigurieren den Encoder [Elementares Live](http://www.elementaltechnologies.com/products/elemental-live) , um einen einzelnen Bitrate Stream an AMS Kanäle zu senden, die für die live-Codierung aktiviert sind.  Weitere Informationen finden Sie unter [Arbeiten mit Kanäle, die zum Ausführen mit Azure Media Services Codierung Live aktiviert sind](media-services-manage-live-encoder-enabled-channels.md).

In diesem Lernprogramm erfahren zum Verwalten von Azure Media Services (AMS) mit dem Tool Azure Media Services Explorer (AMSE). Dieses Tool kann nur auf Windows-Computer ausgeführt werden. Wenn Sie Mac oder Linux sind, verwenden Sie das Azure-Portal [Kanäle](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) und [Programme](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program)zu erstellen.

##<a name="prerequisites"></a>Erforderliche Komponenten

- Sie müssen Kenntnisse in der Schnittstelle elementares Live Web live Ereignisse zu erstellen.
- [Erstellen Sie ein Konto Azure Media Services](media-services-portal-create-account.md)
- Stellen Sie sicher, es ist eine Streaming-Endpunkt mit mindestens eine streaming Einheit zugeordnet wird ausgeführt. Weitere Informationen finden Sie unter [Verwalten von Streaming Endpunkten in einem Media Services-Konto](media-services-portal-manage-streaming-endpoints.md).
- Installieren Sie die neueste Version des Tools [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) .
- Starten Sie das Tool, und Verbinden mit Ihrem Konto AMS.

##<a name="tips"></a>Tipps

- Verwenden Sie nach Möglichkeit eine Verbindung zum Internet gehört.
- Eine gute Faustregel, wenn die Bandbreite Anforderungen bestimmen besteht darin, das streaming eine Bitrate doppelklicken. Während Sie dies nicht obligatorisch erforderlich ist, wird es hilfreich sein, den Einfluss der Überlastung des Netzwerks zu verringern.
- Bei der Verwendung von Software Grundlage Encoder, schließen Sie alle nicht benötigten Programme.

## <a name="elemental-live-with-rtp-ingest"></a>Elementares Live mit RTP Aufnahme

In diesem Abschnitt wird gezeigt, wie den Encoder elementares Live konfiguriert werden, der einen einzelnes Bitrate live Stream über RTP gesendet wird.  Weitere Informationen finden Sie unter [MPEG-Terminaldienste Stream über RTP](media-services-manage-live-encoder-enabled-channels.md#channel).

### <a name="create-a-channel"></a>Erstellen Sie einen Kanal

1.  Klicken Sie im Tool AMSE navigieren Sie zur Registerkarte **Live** , und klicken Sie mit der rechten Maustaste in den Bereich Channel. Wählen Sie **Erstellen Kanal...** aus dem Menü aus.

![Elementares](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. Geben Sie einen Channel an das Beschreibungsfeld ist optional. Kanal aktivieren Sie unter Einstellungen **Standard** für die Codierung Live die Option, mit der Eingabe-Protokoll auf **RTP (MPEG-Terminaldienste)**festgelegt. Lassen Sie andere Einstellungen wie ist ein.


Stellen Sie sicher, dass die **Starten Sie den neuen Kanal jetzt** ausgewählt ist.

3. Klicken Sie auf **Channel erstellen**.
![Elementares](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

>[AZURE.NOTE] Der Kanal kann bis zu 20 Minuten zum Starten dauern.

Beim Start von des Kanals können Sie [den Encoder konfigurieren](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).

>[AZURE.IMPORTANT] Beachten Sie, dass Abrechnung wird gestartet, sobald Channel in einen bereiten Zustand wechselt. Weitere Informationen finden Sie unter [den Channel Staaten](media-services-manage-live-encoder-enabled-channels.md#states).

###<a name="a-idconfigureelementalrtpaconfigure-the-elemental-live-encoder"></a><a id=configure_elemental_rtp></a>Konfigurieren des elementares Live Encoders 

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


####<a name="configuration-steps"></a>Konfigurationsschritte

1. Navigieren Sie zu der **Elementares Live** -Web-Oberfläche, und der Encoder für das streaming **UDP/Terminaldienste** einrichten. 

2. Nachdem ein neues Ereignis erstellt wurde, führen Sie einen Bildlauf nach unten, bis die Ausgabegruppen, und fügen Sie die Gruppe der Ausgabe **UDP/Terminaldienste** . 

3. Erstellen Sie eine neue Ausgabe, indem Sie **neue Stream** , und klicken Sie dann auf **Ausgabe hinzufügen**.  
    
    ![Elementares](./media/media-services-elemental-live-encoder/media-services-elemental13.png)
    
    >[AZURE.NOTE] Es wird empfohlen, dass das Ereignis elementares den Timecode auf "Systemuhr" festgelegt, um den Encoder im Fall eines Fehlers Stream erneut verbinden Hilfe verfügt.

4. Nachdem Sie nun die Ausgabe erstellt wurde, klicken Sie auf **Stream hinzufügen**. Die ausgabeeinstellungen können nun konfiguriert werden. 
5. Führen Sie einen Bildlauf nach unten zu den "Stream 1", die gerade erstellt wurde, klicken Sie auf die Registerkarte **Video** auf der linken Seite und erweitern Sie im Abschnitt **Erweitert** -Einstellungen. 

    ![Elementares](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    Während elementares Live eine Vielzahl von verfügbaren anpassen hat, werden die folgenden Einstellungen für die ersten Schritte mit streaming an AMS empfohlen. 
    
    - Lösung: 1280 x 720 
    - Bildrate: 30 
    - GOP-Größe: 60 frames 
    - Interlace-Modus: schrittweisen 
    - Bitrate: 5000000 Bit/s (diese kann basierend auf Netzwerk Einschränkungen angepasst werden) 
    

    ![Elementares](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

6. Erhalten einer URL im Kanal an.
    
    Navigieren Sie zurück zu den AMSE-Tool, und klicken Sie auf den Kanalstatus aktivieren. Sobald der Zustand zum **Ausführen**von **Felder starten** geändert hat, können Sie die eingegebene URL abrufen.
      
    Wenn der Kanal ausgeführt wird, klicken Sie mit der rechten Maustaste auf der Channelname, navigieren Sie nach unten bis zum Hover über **Eingabe-URL in Zwischenablage kopieren** , und wählen Sie **Primäre Eingabe URL**aus.  
    
    ![Elementares](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
    
1. Fügen Sie diese Informationen im **Primären** Zielfeld von Elementen aus. Alle anderen Einstellungen können die Standardeinstellung bleiben.
    
    ![Elementares](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    Zusätzliche Redundanzgründen wiederholen Sie diese Schritte mit der sekundäre Eingabe URL durch Erstellen einer separaten "Ausgabe" Registerkarte für das Streaming UDP/Terminaldienste ein.
    
7. Klicken Sie auf **Erstellen** (Wenn Sie ein neues Ereignis erstellt wurde) oder **Aktualisieren** (wenn ein bereits vorhandenes Ereignis bearbeiten), und fahren Sie mit den Encoder starten. 

>[AZURE.IMPORTANT]Bevor Sie **beginnen** , klicken Sie auf die Web-Oberfläche elementares Live klicken, sicherstellen **müssen** Sie, dass der Kanal bereit. 
>Stellen Sie außerdem sicher nicht zu den Kanal verlassen, ohne ein Ereignis länger als 15 Minuten > Status bereit.

Wechseln Sie Streams 30 Sekunden ausgeführt wurde, und wieder in die AMSE Tool und Testen der Wiedergabe.  

###<a name="test-playback"></a>Der Testwiedergabe
  
1. Navigieren Sie zu dem Tool AMSE, und klicken Sie mit der rechten Maustaste den Kanal getestet werden. Wählen Sie im Menü zeigen Sie auf **Wiedergabe der Vorschau** , und wählen Sie **mit Azure Media Player**.  

    ![Elementares](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

Wenn Streams im Player angezeigt wird, hat der Encoder ordnungsgemäß konfiguriert wurde, zum Verbinden mit AMS. 

Wenn eine Fehlermeldung angezeigt wird, wird der Kanal zurückgesetzt werden müssen, und Encoder Einstellungen angepasst. Finden Sie im Thema [zur Problembehandlung](media-services-troubleshooting-live-streaming.md) für Anleitungen.   

###<a name="create-a-program"></a>Erstellen Sie ein Programm

1. Sobald Channel Wiedergabe bestätigt ist, erstellen Sie ein Programm aus. Klicken Sie auf der Registerkarte **Live** im Tool AMSE klicken Sie mit der rechten Maustaste in den Bereich Programm, und wählen Sie **Neues Programm erstellen**aus.  

    ![Elementares](./media/media-services-elemental-live-encoder/media-services-elemental9.png)

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
