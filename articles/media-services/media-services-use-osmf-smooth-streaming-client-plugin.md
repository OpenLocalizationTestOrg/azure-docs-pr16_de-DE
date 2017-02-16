<properties 
    pageTitle="Interpolierten Streaming-Plug-In für Open Source Media Framework" 
    description="Erfahren Sie, wie die Azure Media Services interpolierten Streaming-Plug-In für die Adobe Open Source Media Framework verwendet werden soll." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a>So verwenden Sie den Microsoft-optimierten Streaming-Plug-In für Adobe Open Source Media Framework

##<a name="overview"></a>(Übersicht)


Die Microsoft interpolierten Streaming-Plug-In für Open Quelle Medien Framework 2.0 (für OSMF AA) erweitert die Standardfunktionen von OSMF und OSMF-Player für neue und vorhandene Inhalt Wiedergabe Microsoft interpolierten Streaming hinzugefügt. Das Plug-in fügt auch interpolierten Streaming Wiedergabe Funktionen zu Stroboskop Medien Wiedergabe (SMP).

SS für OSMF umfasst zwei Versionen von-Plug-Ins:

- Statische interpolierten Streaming-Plug-In für OSMF (.swc)

- Dynamische interpolierten Streaming-Plug-In für OSMF (SWF)

Dieses Dokument wird davon ausgegangen, dass der Leser allgemeine Kenntnisse von OSMF und OSMF weist-Plug-ins. Weitere Informationen zu OSMF finden Sie in der Dokumentation auf der [offiziellen OSMF-Website](http://osmf.org/).

###<a name="smooth-streaming-plugin-for-osmf-20"></a>Kontinuierliche Streaming-Plug-In für OSMF 2.0

Das Plug-in unterstützt laden und Wiedergeben von bei Bedarf interpolierten Streaming-Inhalte mit den folgenden Features:

- Bei Bedarf interpolierten Streaming Wiedergabe (Wiedergabe, Pause, Suche, beenden)
- Live interpolierten Streaming Wiedergabe (Wiedergabe)
- Live DVR-Funktionen (Pause, Suche, DVR Wiedergabe, Go-to-Live)
- Unterstützung für video-Codecs - h. 264.
- Unterstützung für Audio-Codecs - AAC.
- Mehrere audio mit integrierten OSMF-APIs Wechseln der Sprache
- Max Wiedergabe Qualität Auswahl mit integrierten OSMF-APIs
- Beiwagen Untertitel mit OSMF Untertitel-Plug-Ins
- Adobe&reg; Flash&reg; Player 11.4 oder höher.
- Diese Version unterstützt nur OSMF 2.0.

## <a name="supported-features-and-known-issues"></a>Unterstützte Features und bekannte Probleme

Eine vollständige Liste der unterstützten Funktionen nicht unterstützte Features und der Liste der bekannten Probleme finden Sie in [diesem Dokument](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).


## <a name="loading-the-plugin"></a>Das Plug-In geladen
OSMF-Plug-Ins statisch (zum Zeitpunkt der Kompilierung) geladen werden kann oder dynamisch (zur Laufzeit). Das Plug-in interpolierten Streaming für OSMF Download enthält dynamische und statische Versionen.

- Statische laden: statisch laden, wenn Sie eine statische Bibliotheksdatei (SWC) erforderlich ist. Statische-Plug-Ins werden als ein Bezug auf die Projekte und Zusammenführen in die endgültige Ausgabe-Datei zum Zeitpunkt der Kompilierung hinzugefügt.

- Dynamisches Laden: zum dynamischen Laden von einer vorkompilierten (SWF-) erforderlich ist. Dynamische-Plug-Ins sind in der Laufzeit geladen und nicht in die Projektausgabe enthalten. (Kompilierte Ausgabe) Dynamic-Plug-Ins können mithilfe der Protokolle HTTP und Datei geladen werden.

Weitere Informationen zu statischen und dynamischen Laden finden Sie unter der offiziellen [OSMF-Plug-Ins Seite](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

###<a name="ss-for-osmf-static-loading"></a>Für OSMF statische Ladevorgang SS
Der folgende Codeausschnitt zeigt So laden das Plugin SS statisch für OSMF und OSMF MediaFactory Klasse verwenden grundlegende Video wiederzugeben. Vor dem einschließlich der SS für OSMF Code, stellen Sie sicher, dass die Projektreferenz, die "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swc" statische-Plug-in enthält.

```
package 
{
    
    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    
    
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        

        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }
    
        private function initMediaPlayer():void
        {
        
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            
            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}
```


###<a name="ss-for-osmf-dynamic-loading"></a>SS für OSMF dynamisches Laden

Der folgenden Codeausschnitt veranschaulicht, wie das Plugin SS für OSMF dynamisch laden und Wiedergeben einer Basic mithilfe der Klasse OSMF MediaFactory video. Vor dem einschließlich der SS für OSMF Code, kopieren Sie der "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swf" dynamische-Plug-Ins in den Projektordner, wenn Sie möchten, laden Sie die Datei Protocol mit oder unter einem Webserver für HTTP laden kopieren. Es ist nicht erforderlich "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swc" in den Projektverweise aufnehmen möchten ein.

 
Paket {}
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;

    
    //Sets the size of the SWF
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        
        
        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }
        
        private function initMediaPlayer():void
        {
            
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}

##<a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a>Stroboskop Medienwiedergabe mit der SS ODMF dynamische-Plug-Ins

Das interpolierten Streaming für dynamische OSMF-Plug-in ist mit [Stroboskop Medien Wiedergabe (SMP)](http://osmf.org/strobe_mediaplayback.html)kompatibel. Die SS für OSMF-Plug-Ins können Sie Inhalte Wiedergabe interpolierten Streaming SMP hinzufügen. Kopieren Sie hierzu "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swf" unter einem Webserver für HTTP laden, verwenden die folgenden Schritte aus:

1.  Durchsuchen Sie die [Einrichtungsseite Stroboskop Medienwiedergabe](http://osmf.org/dev/2.0gm/setup.html). 
2.  Legen Sie die Src auf einer interpolierten Streaming Quelle (z. B. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3.  Nehmen Sie die gewünschte Konfiguration Änderungen vor, und klicken Sie auf Vorschau anzeigen und aktualisieren.
 
    **Notiz** Der Inhalt Webserver benötigt eine gültige crossdomain.xml. 
4.  Kopieren und Einfügen des Codes eine einfache HTML-Seite mit Ihrem bevorzugten Texteditor, wie im folgenden Beispiel:



        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



5. Der Einbettungscode interpolierten Streaming OSMF-Plug-Ins hinzu, und speichern.

        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>


6.  Die HTML-Seite zu speichern und Veröffentlichen auf einem Webserver. Navigieren Sie zu der veröffentlichten Webseite mit Ihrem bevorzugten Flash-&reg; Player aktiviert Internetbrowser (Internet Explorer, Chrome, Firefox, usw.).
7.  Einfacheres interpolierten Streaming-Inhalte in Adobe&reg; Flash&reg; Player.

Weitere Informationen zu allgemeinen OSMF Entwicklung finden Sie unter der offiziellen [OSMF Entwicklung Seite](http://osmf.org/resources.html).

##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Siehe auch

[Microsoft Adaptive Streaming-Plug-In für OSMF aktualisieren](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 
