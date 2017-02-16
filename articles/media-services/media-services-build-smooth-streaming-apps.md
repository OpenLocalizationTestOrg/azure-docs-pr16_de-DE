<properties 
    pageTitle="Kontinuierliche Streaming Windows Store-App-Lernprogramm | Microsoft Azure" 
    description="Informationen Sie zum Azure Media-Dienste verwenden, um eine C#-Windows Store-Anwendung mit einer XML-MediaElement-Steuerelement auf Wiedergabe interpolierten Stream Inhalt zu erstellen." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>



#<a name="how-to-build-a-smooth-streaming-windows-store-application"></a>So erstellen Sie eine Windows Store-Anwendung Streaming Glätten

Die interpolierten Streaming Client SDK für Windows 8 ermöglicht Entwicklern das Erstellen von Windows Store-Apps, die bei Bedarf und live interpolierten Streaming-Inhalte wiedergeben können. Zusätzlich zu den grundlegenden Wiedergabe von interpolierten Inhalte das SDK enthält auch leistungsfähigen Features wie Microsoft PlayReady Schutz, Ebene Einschränkung, Live DVR Audioqualität streamen umsteigen, warten auf statusaktualisierungen (z. B. Qualität Änderungen auf Websiteebene vorzunehmen) und Fehlerereignisse usw.. Weitere Informationen zu den unterstützten Features finden Sie unter der [Versionsinformationen](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). Weitere Informationen finden Sie unter [Player Framework für Windows 8](http://playerframework.codeplex.com/). 

In diesem Lernprogramm enthält vier Lektionen:

1. Erstellen einer einfachen interpolierten Streaming-Store-Anwendungs
2. Hinzufügen eines Schieberegler, um zu steuern, den Fortschritt Medien
3. Wählen Sie interpolierten Streaming Streams
4. Wählen Sie Spuren interpolierten Streaming

##<a name="prerequisites"></a>Erforderliche Komponenten

- Windows 8 32-Bit- oder 64-Bit. Sie können [Windows 8 Enterprise Auswertung](http://msdn.microsoft.com/evalcenter/jj554510.aspx) von MSDN erhalten.
- Visual Studio 2012 oder Visual Studio Express 2012 (oder höher). Sie können die Testversion von [hier](http://www.microsoft.com/visualstudio/11/downloads)abrufen.
- [Microsoft optimierten Streaming Client SDK für Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).


Die fertige Lösung für jede Lektion kann von MSDN Developer Codebeispielen (Code Gallery) heruntergeladen werden: 

- [Lektion 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) - eine einfache Windows 8 optimierten Streaming MediaPlayer 
- [Lektion 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) - einen einfachen Windows 8 interpolierten Streaming MediaPlayer mit einem Schieberegler steuern, 
- [Lektion 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - ein Windows 8 optimierten Streaming MediaPlayer mit Streamauswahl  
- [Lektion 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) - ein Windows 8 optimierten Streaming MediaPlayer mit Auswahl nachverfolgen.

##<a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Lektion 1: Erstellen einer einfachen interpolierten Streaming-Store-Anwendungs

In dieser Lektion erstellt Sie Windows Store-Anwendung mit einem MediaElement-Steuerelement für die Wiedergabe interpolierten Stream Inhalt.  Die ausgeführte Anwendung sieht wie folgt aus:

![Beispiel für optimierten Streaming Windows Store-Anwendung][PlayerApplication]
 
Weitere Informationen zur Entwicklung von Windows Store-Anwendung finden Sie unter [entwickeln hervorragende Apps für Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx). Diese Lektion enthält eine die folgenden Verfahren:

1.  Erstellen eines Projekts Windows Store
2.  Entwerfen der Benutzeroberfläche (XAML)
3.  Ändern der CodeBehind-Datei
4.  Kompilieren und Testen der Anwendungs

**So erstellen Sie eine Windows Store-Projekt**

1.  Visual Studio 2012 oder höher ausführen.
2.  Im Menü **Datei** klicken Sie auf **neu**, und klicken Sie dann auf **Projekt**.
3.  Wählen Sie im Dialogfeld Neues Projekt geben Sie ein, oder wählen Sie die folgenden Werte aus:

Namen|Wert
---|---
Vorlagengruppe|Installiert/Vorlagen/Visual c# / Windows speichern
Vorlage|Leere App (XAML)
Namen|SSPlayer
Speicherort|C:\SSTutorials
Lösungsnamen|SSPlayer
Verzeichnis für Lösung erstellen|(aktiviert)

4.  Klicken Sie auf **OK**.

**Einen Verweis auf die interpolierten Streaming Client SDK hinzufügen**

1.  Lösung-Explorer mit der rechten Maustaste **SSPlayer**, und klicken Sie dann auf **Verweis hinzufügen**.
2.  Geben Sie ein, oder wählen Sie die folgenden Werte aus:

Namen|Wert
---|---
Referenzgruppe|Windows-Erweiterungen
Bezug|Wählen Sie Microsoft optimierten Streaming Client SDK für Windows 8 und Microsoft Visual C++ Runtime-Paket aus.
    
3.  Klicken Sie auf **OK**. 

Nach dem Hinzufügen der Verweise, müssen Sie die gezielte Plattform (X64 oder X86) auswählen, Hinzufügen von Verweisen werden bei der Konfiguration der Any CPU-Plattform nicht funktionieren.  Im Explorer Lösung sehen Sie sich, dass die gelbe Warnung Markierung für diese Verweise hinzugefügt.

**Entwerfen die Benutzeroberfläche player**

1.  Lösung-Explorer klicken Sie **MainPage.xaml** , um es in der Entwurfsansicht öffnen.
2.  Suchen Sie nach der ** &lt;Raster&gt; ** und ** &lt;/Grid&gt; ** tags die XAML-Datei, und fügen Sie den folgenden Code zwischen den beiden Tags:

        <Grid.RowDefinitions>
            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
        </Grid.RowDefinitions>
        
        <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
        </StackPanel>

        <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
        </StackPanel>

        <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
        </StackPanel>

    Das MediaElement-Steuerelement wird zur Medienwiedergabe verwendet. Das Schieberegler-Steuerelement mit dem Namen SliderProgress wird in der nächsten Lektion um zu steuern, den Fortschritt Medien verwendet werden.

3.  Drücken Sie **STRG + S** , um die Datei zu speichern.

Das MediaElement-Steuerelement unterstützt keine interpolierten Streaming Inhalt Out-of-Box. Klicken Sie zum Aktivieren des interpolierten Streaming Supports müssen Sie den Ereignishandler, indem Sie Dateinamenerweiterung und MIME-Typ interpolierten Streaming Byte-Stream registrieren.  Um zu registrieren, verwenden Sie die Methode MediaExtensionManager.RegisterByteStremHandler des Namespace Windows.Media.

In diese XAML-Datei sind die Steuerelemente zugeordnet einige Ereignishandler.  Sie müssen diese Ereignishandler definieren.

**So ändern Sie die CodeBehind-Datei**

1.  Lösung-Explorer mit der rechten Maustaste **MainPage.xaml**, und klicken Sie dann auf **Code anzeigen**.
2.  Fügen Sie am oberen Rand der Datei, die folgende Anweisung verwenden:

        using Windows.Media;

3.  Fügen Sie am Anfang der **MainPage** -Klasse, die folgenden Daten Mitglied hinzu:

        private MediaExtensionManager extensions = new MediaExtensionManager();

4.  Fügen Sie am Ende des Konstruktors **MainPage** die folgenden zwei Zeilen ein:

        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
        
5.  Am Ende der **MainPage** -Klasse hinter den folgenden Code:

        #region UI Button Click Events
        private void btnPlay_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Play();
            txtStatus.Text = "MediaElement is playing ...";
        }
        
        private void btnPause_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Pause();
            txtStatus.Text = "MediaElement is paused";
        }
        
        private void btnSetSource_Click(object sender, RoutedEventArgs e)
        {
            sliderProgress.Value = 0;
            mediaElement.Source = new Uri(txtMediaSource.Text);
        
            if (chkAutoPlay.IsChecked == true)
            {
                txtStatus.Text = "MediaElement is playing ...";
            }
            else
            {
                txtStatus.Text = "Click the Play button to play the media source.";
            }
        }
        
        private void btnStop_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Stop();
            txtStatus.Text = "MediaElement is stopped";
        }
        
        private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            txtStatus.Text = "Seek to position " + sliderProgress.Value;
            mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
        }
        #endregion

    Hier wird der SliderProgress_PointerPressed Ereignishandler festgelegt.  Es gibt mehrere Works zu tun ist damit dies funktioniert, die in der nächsten Lektion dieses Lernprogramms behandelt werden.
6.  Drücken Sie **STRG + S** , um die Datei zu speichern.

Die fertige CodeBehind-Datei wird wie folgt aussehen:

![CodeView in Visual Studio der interpolierten Streaming Windows Store-Anwendung][CodeViewPic]

**Kompilieren und Testen Sie die Anwendung**

1.  Klicken Sie im Menü **Erstellen** auf **Konfigurations-Manager**.
2.  Ändern Sie die **Lösung für die aktive Plattform** für Ihre Entwicklungsplattform entsprechen.
3.  Drücken Sie **F6** , um das Projekt zu kompilieren. 
4.  Drücken Sie **F5** , um die Anwendung ausführen.
5.  Am oberen Rand der Anwendung können Sie entweder verwenden Sie den Standardwert interpolierten Streaming URL oder eine andere eingeben. 
6.  Klicken Sie auf **Quelle festlegen**. Da **Automatische Wiedergabe** standardmäßig aktiviert ist, werden die Medien automatisch wiedergegeben.  Sie können das Medium mit dem **Wiedergabe**, **Pause** und **Beenden der** Schaltflächen steuern.  Sie können die Medien Lautstärke mit dem Schieberegler vertikale steuern.  Jedoch der horizontale Schieberegler für den Fortschritt Medien steuern vollständig noch nicht verfügbar ist. 

Lesson1 abgeschlossen haben.  In dieser Lektion verwenden Sie ein MediaElement-Steuerelement zur Wiedergabe interpolierten Streaming-Inhalte an.  In der nächsten Lektion fügen Sie einen Schieberegler, um den Fortschritt des Inhalts interpolierten Streaming steuern.


##<a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>Lektion 2: Hinzufügen eines Schieberegler, um zu steuern, den Fortschritt Medien
In Lektion 1 haben Sie eine Windows Store-Anwendung mit einem Steuerelement MediaElement XAML zur Wiedergabe von Medieninhalten interpolierten Streaming erstellt.  Einige grundlegende Medienfunktionen wie starten, beenden und Anhalten vertrauen.  In dieser Lektion fügen Sie einen Schieberegler-Steuerelements zur Anwendung hinzu.

In diesem Lernprogramm werden wir einen Zeitgeber verwenden, um die Schiebereglerposition basierend auf der aktuellen Position des MediaElement-Steuerelements zu aktualisieren.  Schieberegler für Anfang und Ende Anzeigedauer auch bei live-Inhalte aktualisiert werden müssen.  Dies kann besser in das adaptive Quelle Update Ereignis behandelt werden.

Medienquellen sind Objekte, die Media-Daten zu generieren.  Die Quelle Auflösung wird ein Stream URL oder Byte-Zeichen und die entsprechenden Medienquelle für den betreffenden Inhalt erstellt.  Die Quelle Auflösung ist das Standardverfahren für die Applikationen Medienquellen erstellen. 

Diese Lektion enthält eine die folgenden Verfahren:

1.  Registrieren Sie sich den interpolierten Streaming Ereignishandler 
2.  Fügen Sie die Ereignishandler adaptive Quelle Manager Ebene hinzu
3.  Fügen Sie die Ebene Ereignishandler adaptive Quelle hinzu.
4.  Fügen Sie Ereignishandler MediaElement hinzu
5.  Schieberegler für verwandte Code hinzufügen
6.  Kompilieren und Testen der Anwendungs

**Registrieren den interpolierten Streaming Byte-Stream Ereignishandler und übergeben die propertyset**

1.  Klicken Sie mit der rechten Maustaste auf **MainPage.xaml**Lösung-Explorer und klicken Sie dann auf **Code anzeigen**.
2.  Fügen Sie am Anfang der Datei, die folgende Anweisung verwenden:

        using Microsoft.Media.AdaptiveStreaming;

3.  Fügen Sie am Anfang der MainPage-Klasse, die folgenden Datenelemente hinzu:

        private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
        private IAdaptiveSourceManager adaptiveSourceManager;
    
4.  Innerhalb des Konstruktors **MainPage** fügen Sie dem folgenden Code nach der **folgt. Initialisierung Components();** Zeilen- und der Registrierung Codezeilen in der vorherigen Lektion geschrieben:
    
        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
    
5.  Ändern Sie innerhalb des Konstruktors **MainPage** zwei Methoden zum Hinzufügen von RegisterByteStreamHandler das vierte Parameter:

        // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
        // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
        // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
        propertySet);

6.  Drücken Sie **STRG + S** , um die Datei zu speichern.

**Hinzufügen den Ereignishandler adaptive Quelle Manager Ebene**

1.  Klicken Sie mit der rechten Maustaste auf **MainPage.xaml**Lösung-Explorer und klicken Sie dann auf **Code anzeigen**.
2.  Fügen Sie die folgenden Datenelement innerhalb der **MainPage** -Klasse hinzu:

        private AdaptiveSource adaptiveSource = null;

3.  Fügen Sie am Ende der **MainPage** -Klasse den folgenden Ereignishandler hinzu:
    
        #region Adaptive Source Manager Level Events
        private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
        }
        #endregion Adaptive Source Manager Level Events

4.  Fügen Sie am Ende des Konstruktors **MainPage** die folgende Zeile, um das adaptive Quelle open-Ereignis zu abonnieren:
    
    adaptiveSourceManager.AdaptiveSourceOpenedEvent += neue AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);

5.  Drücken Sie **STRG + S** , um die Datei zu speichern.

**Ebene Ereignishandler adaptive Quelle hinzufügen**

1.  Klicken Sie mit der rechten Maustaste auf **MainPage.xaml**Lösung-Explorer und klicken Sie dann auf **Code anzeigen**.
2.  Fügen Sie die folgenden Datenelement innerhalb der **MainPage** -Klasse hinzu:
    
        private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate; 
        private Manifest manifestObject;
    
3.  Fügen Sie am Ende der **MainPage** -Klasse die folgenden Ereignishandler hinzu:

        #region Adaptive Source Level Events
        private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
        }
        
        private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
        {
            adaptiveSourceStatusUpdate = args;
        }
        
        private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
        {
            txtStatus.Text = "Error: " + args.HttpResponse;
        }
        #endregion Adaptive Source Level Events

4.  Fügen Sie am Ende der Methode **MediaElement AdaptiveSourceOpened** den folgenden Code, um die Ereignisse zu abonnieren:
    
        adaptiveSource.ManifestReadyEvent +=
                    mediaElement_ManifestReady;
        adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 
            mediaElement_AdaptiveSourceStatusUpdated;
        adaptiveSource.AdaptiveSourceFailedEvent += 
            mediaElement_AdaptiveSourceFailed;
    
5.  Drücken Sie **STRG + S** , um die Datei zu speichern.

Die gleichen Ereignisse stehen auf Adaptive Datenquellen-Manager Ebene ebenfalls, die für den Umgang mit Funktionen, die alle Medienelemente in der app gemeinsam verwendet werden können. Jede AdaptiveSource enthält eine eigene Ereignisse und alle AdaptiveSource Ereignisse werden unter AdaptiveSourceManager Löschvorgänge werden.

**Ereignishandler Medienelement hinzufügen**

1.  Klicken Sie mit der rechten Maustaste auf **MainPage.xaml**Lösung-Explorer und klicken Sie dann auf **Code anzeigen**.
2.  Fügen Sie am Ende der **MainPage** -Klasse die folgenden Ereignishandler hinzu:
    
        #region Media Element Event Handlers 
        private void MediaOpened(object sender, RoutedEventArgs e)
        {
            txtStatus.Text = "MediaElement opened";
        }
        
        private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
        {
            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
        }
        
        private void MediaEnded(object sender, RoutedEventArgs e)
        {
            txtStatus.Text ="MediaElement ended.";
        }
        #endregion Media Element Event Handlers

3.  Am Ende des Konstruktors **MainPage** fügen Sie den folgenden Code tiefgestellt auf die Ereignisse aus:
    
        mediaElement.MediaOpened += MediaOpened;
        mediaElement.MediaEnded += MediaEnded;
        mediaElement.MediaFailed += MediaFailed;

4.  Drücken Sie **STRG + S** , um die Datei zu speichern.

**Hinzufügen von Schieberegler Zusammenhang code**

1.  Klicken Sie mit der rechten Maustaste auf **MainPage.xaml**Lösung-Explorer und klicken Sie dann auf **Code anzeigen**.
2.  Fügen Sie am Anfang der Datei, die folgende Anweisung verwenden:
    
        using Windows.UI.Core;

3.  Fügen Sie die folgenden Daten Mitglieder innerhalb der **MainPage** -Klasse hinzu:
    
        public static CoreDispatcher _dispatcher;
        private DispatcherTimer sliderPositionUpdateDispatcher;
    
4.  Am Ende des Konstruktors **MainPage** fügen Sie den folgenden Code ein:

        _dispatcher = Window.Current.Dispatcher;
        PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
        sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
    
5.  Fügen Sie am Ende der **MainPage** -Klasse den folgenden Code ein:
    
        #region sliderMediaPlayer
        private double SliderFrequency(TimeSpan timevalue)
        {
            long absvalue = 0;
            double stepfrequency = -1;
        
            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }
        
            TimeSpan totalDVRDuration = new TimeSpan(absvalue);
        
            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }
        
            return stepfrequency;
        }
        
        void updateSliderPositionoNTicks(object sender, object e)
        {
            sliderProgress.Value = mediaElement.Position.TotalSeconds;
        }
        
        public void setupTimer()
        {
            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
        }

        public void startTimer()
        {
            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderStartTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderEndTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
        }
        #endregion sliderMediaPlayer

    **Hinweis:** CoreDispatcher wird verwendet, um die Änderungen an den UI-Thread nicht UI-Thread vornehmen. Bei Engpass auf Verteiler-Thread kann Entwickler von Benutzeroberflächenelemente er / aktualisieren will bereitgestellten Verteiler verwenden auswählen.  Beispiel:
    
        await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 
          timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
        double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 
          sliderProgress.Maximum = absvalue; }); 
        

6.  Fügen Sie am Ende der Methode **MediaElement_AdaptiveSourceStatusUpdated** den folgenden Code ein:
    
        setSliderStartTime(args.StartTime);
        setSliderEndTime(args.EndTime);

7.  Am Ende der Methode **MediaOpened** fügen Sie den folgenden Code ein:
    
    sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan); sliderProgress.Width = mediaElement.Width; setupTimer();

8.  Drücken Sie **STRG + S** , um die Datei zu speichern.

**Kompilieren und Testen Sie die Anwendung**

1. Drücken Sie **F6** , um das Projekt zu kompilieren. 
2.  Drücken Sie **F5** , um die Anwendung ausführen.
3.  Am oberen Rand der Anwendung können Sie entweder verwenden Sie den Standardwert interpolierten Streaming URL oder eine andere eingeben. 
4.  Klicken Sie auf **Quelle festlegen**. 
5.  Testen Sie den Schieberegler ein.

Lektion 2 abgeschlossen haben.  In dieser Lektion haben Sie einen Schieberegler Anwendung hinzugefügt. 

##<a name="lesson-3-select-smooth-streaming-streams"></a>Lektion 3: Wählen Sie interpolierten Streaming Streams
Interpolierten Streaming ist in der Lage, zum Streaming von Inhalten mit mehreren Sprache audio Spuren, die durch die Viewer ausgewählt werden.  Aktivieren Sie in dieser Lektion Leser Streams auswählen. Diese Lektion enthält eine die folgenden Verfahren:

1. Ändern Sie die XAML-Datei
2. Ändern Sie die Code Behand-Datei
3. Kompilieren und Testen der Anwendungs


**So ändern Sie die XAML-Datei**

1. Lösung-Explorer mit der rechten Maustaste **MainPage.xaml**, und klicken Sie dann auf **Ansicht-Designer**.
2. Suchen Sie nach &lt;Grid.RowDefinitions&gt;, und ändern Sie die RowDefinitions, damit er sieht wie folgt aus:

        <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
        </Grid.RowDefinitions>

3. Innerhalb der &lt;Raster&gt;&lt;/Grid&gt; Kategorien, fügen Sie den folgenden Code ein Listenfeldsteuerelement definieren, damit Benutzer finden Sie in der Liste der verfügbaren Streams können, und wählen Streams hinzu:

        <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
        </Grid>

4. Drücken Sie **STRG + S,** um die Änderungen zu speichern.


**So ändern Sie die CodeBehind-Datei**

1. Lösung-Explorer mit der rechten Maustaste **MainPage.xaml**, und klicken Sie dann auf **Code anzeigen**.
2. Fügen Sie eine neue Klasse innerhalb des Namespace SSPlayer: #region Klasse Stream
    
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
    
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
    
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
    
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream

3. Fügen Sie am Anfang der MainPage-Klasse, Variablen die folgenden Definitionen ein:

        private List<Stream> availableStreams;
        private List<Stream> availableAudioStreams;
        private List<Stream> availableTextStreams;
        private List<Stream> availableVideoStreams;

4. Fügen Sie den folgenden Bereich innerhalb der MainPage-Klasse hinzu:

        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
        
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
        
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
        
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
        
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
        
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
        
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
        
            // Select the frist video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
        
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
        
            // Select the frist audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
        
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
        
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection

5. Suchen Sie nach der Methode MediaElement_ManifestReady, hängen Sie den folgenden Code am Ende der Funktion:
    
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();

    Damit MediaElement Manifest bereit ist, der Code eine Liste der verfügbaren Streams erhält, und das Benutzeroberfläche Listenfeld mit der Liste aufgefüllt.

6. Suchen Sie in der Klasse MainPage die Benutzeroberfläche Schaltflächen Ereignisse Region klicken Sie auf, und fügen Sie dann die folgende Funktionsdefinition hinzu:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Kompilieren und Testen Sie die Anwendung**

1. Drücken Sie **F6** , um das Projekt zu kompilieren. 
2.  Drücken Sie **F5** , um die Anwendung ausführen.
3.  Am oberen Rand der Anwendung können Sie entweder verwenden Sie den Standardwert interpolierten Streaming URL oder eine andere eingeben. 
4.  Klicken Sie auf **Quelle festlegen**. 
5.  Die Standardsprache ist Audio_eng. Versuchen Sie, zwischen Audio_eng und Audio_es zu wechseln. Jedem, wählen Sie einen neuen Stream, müssen Sie die Senden-Schaltfläche klicken.

Lektion 3 abgeschlossen haben.  In dieser Lektion fügen Sie die Funktionalität zum Auswählen des Streams.

##<a name="lesson-4-select-smooth-streaming-tracks"></a>Lektion 4: Wählen Sie Spuren interpolierten Streaming
Eine Präsentation mit interpolierten Streaming kann mehrere Videodateien mit anderen Qualität Ebenen (Bit Sätzen) und Lösungen codierte enthalten. In dieser Lektion ermöglicht Benutzern, Spuren auszuwählen. Diese Lektion enthält eine die folgenden Verfahren:

1. Ändern Sie die XAML-Datei
2. Ändern Sie die Code Behand-Datei
3. Kompilieren und Testen der Anwendungs

**So ändern Sie die XAML-Datei**

1. Lösung-Explorer mit der rechten Maustaste **MainPage.xaml**, und klicken Sie dann auf **Ansicht-Designer**.
2. Suchen Sie nach der &lt;Raster&gt; mit dem Namen **GridStreamAndBitrateSelection**kategorisieren, hängen Sie den folgenden Code am Ende der Kategorie:

        <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
        </StackPanel>

3. Drücken Sie **STRG + S** , um He Änderungen zu speichern


**So ändern Sie die CodeBehind-Datei**

1. Lösung-Explorer mit der rechten Maustaste **MainPage.xaml**, und klicken Sie dann auf **Code anzeigen**.
2. Fügen Sie eine neue Klasse innerhalb des Namespace SSPlayer hinzu:
    
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
    
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
    
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
    
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track

3. Fügen Sie am Anfang der MainPage-Klasse, Variablen die folgenden Definitionen ein:
    
        private List<Track> availableTracks;

4. Fügen Sie den folgenden Bereich innerhalb der MainPage-Klasse hinzu:
    
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>

        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();

            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;

            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;

                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }

        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }

        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }

        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }

        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection

5. Suchen Sie nach der Methode MediaElement_ManifestReady, hängen Sie den folgenden Code am Ende der Funktion:

        getTracks(manifestObject);
        refreshAvailableTracksListBoxItemSource();

6. Suchen Sie in der Klasse MainPage die Benutzeroberfläche Schaltflächen Ereignisse Region klicken Sie auf, und fügen Sie dann die folgende Funktionsdefinition hinzu:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Kompilieren und Testen Sie die Anwendung**

1. Drücken Sie **F6** , um das Projekt zu kompilieren. 
2.  Drücken Sie **F5** , um die Anwendung ausführen.
3.  Am oberen Rand der Anwendung können Sie entweder verwenden Sie den Standardwert interpolierten Streaming URL oder eine andere eingeben. 
4.  Klicken Sie auf **Quelle festlegen**. 
5.  Standardmäßig werden alle Spuren des Videostreams aktiviert. Änderungen an der Bit experimentieren möchten, können Sie wählen Sie aus den niedrigsten Bit (Disagio) verfügbar und wählen Sie dann den höchsten Bit (Disagio) verfügbar. Klicken Sie nach jeder Änderung Absenden auf.  Sie können die Videoqualität Änderungen angezeigt.

Lektion 4 abgeschlossen haben.  In dieser Lektion fügen Sie die Funktionalität zum Titel auszuwählen.


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="other-resources"></a>Weitere Ressourcen:
- [So erstellen Sie eine reibungslose Streaming Windows 8 JavaScript-Anwendung mit erweiterten features](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
- [Interpolierten Streaming – technische Übersicht](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png
 
