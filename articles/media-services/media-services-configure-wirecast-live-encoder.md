<properties 
    pageTitle="Konfigurieren den Encoder Telestream Wirecast zum Senden eines einzelnen Bitrate live Streams | Microsoft Azure" 
    description="In diesem Thema wird gezeigt, wie konfigurieren den Encoder Wirecast live, um einen einzelnen Bitrate Stream an AMS Kanäle zu senden, die für die live-Codierung aktiviert sind. " 
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

#<a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a>Verwenden der Wirecast Encoder zu einen einzelnen Bitrate live Stream senden

> [AZURE.SELECTOR]
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [Live Elementen](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

In diesem Thema wird gezeigt, wie konfigurieren den Encoder [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) live, um einen einzelnen Bitrate Stream an AMS Kanäle zu senden, die für die live-Codierung aktiviert sind.  Weitere Informationen finden Sie unter [Arbeiten mit Kanäle, die zum Ausführen mit Azure Media Services Codierung Live aktiviert sind](media-services-manage-live-encoder-enabled-channels.md).

In diesem Lernprogramm erfahren zum Verwalten von Azure Media Services (AMS) mit dem Tool Azure Media Services Explorer (AMSE). Dieses Tool kann nur auf Windows-Computer ausgeführt werden. Wenn Sie Mac oder Linux sind, verwenden Sie das Azure-Portal [Kanäle](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) und [Programme](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program)zu erstellen.


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

![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Geben Sie einen Channel an das Beschreibungsfeld ist optional. Kanal aktivieren Sie unter Einstellungen **Standard** für die Codierung Live die Option, mit der Eingabe-Protokoll auf **RTMP**festgelegt. Lassen Sie andere Einstellungen wie ist ein.


Stellen Sie sicher, dass die **Starten Sie den neuen Kanal jetzt** ausgewählt ist.

3. Klicken Sie auf **Channel erstellen**.
![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

>[AZURE.NOTE] Der Kanal kann bis zu 20 Minuten zum Starten dauern.

Beim Start von des Kanals können Sie [den Encoder konfigurieren](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).

>[AZURE.IMPORTANT] Beachten Sie, dass Abrechnung wird gestartet, sobald Channel in einen bereiten Zustand wechselt. Weitere Informationen finden Sie unter [den Channel Staaten](media-services-manage-live-encoder-enabled-channels.md#states).

##<a name="a-idconfigurewirecastrtmpaconfigure-the-telestream-wirecast-encoder"></a><a id=configure_wirecast_rtmp></a>Konfigurieren des Telestream Wirecast Encoders

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

1. Öffnen Sie die Telestream Wirecast Anwendung auf dem Computer zu verwendet und für das streaming RTMP einrichten.
2. Konfigurieren Sie die Ausgabe, indem Sie auf der Registerkarte **Ausgabe** Navigation und Auswahl von **Ausgabeeinstellungen...**.
    
    Stellen Sie sicher, dass das **Ziel der Ausgabe** **RTMP-Server**festgelegt ist.
3. Klicken Sie auf **OK**.
4. Legen Sie auf der Einstellungsseite **im Zielfeld **Azure Media Services**sein** .
 
    Das Profil Codierung ist aktiviert **Azure h. 264 720 p 16:9 (1280 x 720)**. Um diese Einstellungen anpassen, wählen Sie das Zahnradsymbol rechts neben der Dropdownliste aus, nach unten, und wählen Sie dann auf **Neue voreingestellte**.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)

5. Konfigurieren von Encoder Voreinstellungen.

    Benennen Sie die Vorgabe, und überprüfen Sie die folgenden empfohlenen Einstellungen:

    **Video**
    
    - Encoder: MainConcept h. 264.
    - Bilder pro Sekunde: 30
    - Durchschnitt: 5000 Empfang Kbits/s (angepasst werden kann basierend auf Netzwerk Einschränkungen)
    - Profil: Main
    - Key Frame jeder: 60 Frames

    **Audio**

    - Ziel Bitrate: 192 Empfang Kbits/s
    - Sample-Rate: 44,100 kHz
     
    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)

6. Drücken Sie auf **Speichern**.

    Das Feld Codierung verfügt jetzt über den neu erstellten Profil zur Auswahl zur Verfügung. 

    Stellen Sie sicher, dass neue Profil ausgewählt ist.

7. Abrufen einer URL im Kanal um den Wirecast **RTMP Endpunkt**zugewiesen.
    
    Navigieren Sie zurück zu den AMSE-Tool, und klicken Sie auf den Kanalstatus aktivieren. Sobald der Zustand zum **Ausführen**von **Felder starten** geändert hat, können Sie die eingegebene URL abrufen.
      
    Wenn der Kanal ausgeführt wird, klicken Sie mit der rechten Maustaste auf der Channelname, navigieren Sie nach unten bis zum Hover über **Eingabe-URL in Zwischenablage kopieren** , und wählen Sie **Primäre Eingabe URL**aus.  
    
    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)

8. Klicken Sie im Fenster Wirecast **Ausgabeeinstellungen** fügen Sie diese Informationen in **das Adressfeld des Abschnitts Ausgabe** , und weisen Sie einen Namen für die Streams. 


    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

9. Wählen Sie **OK**aus.

10. Bestätigen Sie auf dem Hauptbildschirm **Wirecast** Eingabewerte Quellen für Video und Audio bereit sind, und drücken Sie dann die **Stream** in der oberen linken Ecke aus.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

>[AZURE.IMPORTANT]Bevor Sie **Stream**geklickt haben, sicherstellen **müssen** Sie, dass der Kanal bereit. 
>Stellen Sie außerdem sicher nicht zu den Kanal Status bereit ohne eine Eingabe Beitrag länger als 15 Minuten > feed zu verlassen.

##<a name="test-playback"></a>Der Testwiedergabe
  
1. Navigieren Sie zu dem Tool AMSE, und klicken Sie mit der rechten Maustaste den Kanal getestet werden. Wählen Sie im Menü zeigen Sie auf **Wiedergabe der Vorschau** , und wählen Sie **mit Azure Media Player**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Wenn Streams im Player angezeigt wird, hat der Encoder ordnungsgemäß konfiguriert wurde, zum Verbinden mit AMS. 

Wenn eine Fehlermeldung angezeigt wird, wird der Kanal zurückgesetzt werden müssen, und Encoder Einstellungen angepasst. Finden Sie im Thema [zur Problembehandlung](media-services-troubleshooting-live-streaming.md) für Anleitungen.  

##<a name="create-a-program"></a>Erstellen Sie ein Programm

1. Sobald Channel Wiedergabe bestätigt ist, erstellen Sie ein Programm aus. Klicken Sie auf der Registerkarte **Live** im Tool AMSE klicken Sie mit der rechten Maustaste in den Bereich Programm, und wählen Sie **Neues Programm erstellen**aus.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)

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
