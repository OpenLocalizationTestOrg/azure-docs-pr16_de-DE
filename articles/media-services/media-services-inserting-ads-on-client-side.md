<properties 
    pageTitle="Einfügen von Werbung clientseitig | Microsoft Azure" 
    description="In diesem Thema wird gezeigt, wie Werbung clientseitig eingefügt wird." 
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


#<a name="inserting-ads-on-the-client-side"></a>Einfügen von Werbung clientseitig

Dieses Thema enthält Informationen zum Einfügen von verschiedenen Arten von anzeigen auf dem Client.

Informationen über die geschlossenen Untertitel und Ad-Unterstützung in Videos streaming Live finden Sie unter [Untertitel unterstützt und die Einfügemarke Standards Ad](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

>[AZURE.NOTE] Azure Media Player unterstützt derzeit nicht anzeigen.

##<a name="a-idinsertadsintomediaainserting-ads-into-your-media"></a><a id="insert_ads_into_media"></a>Einfügen von Werbung in Ihre Medien

Azure Media-Dienste bieten Unterstützung für Werbung über die Windows Media-Plattform: Player Framework. Player Framework mit Active Directory-stehen für Windows 8, Silverlight, Windows Phone 8 und iOS-Geräte. Jedes Framework Player enthält Beispielcode, der Sie wird gezeigt, wie eine Player-Anwendung zu implementieren. Es gibt drei verschiedene Arten von anzeigen, die Sie in die Medien: Liste einfügen können.

- **Linear** – vollständigen Frame anzeigen, die das Hauptfenster Video angehalten.
- **Nonlinear** – Überlagerung anzeigen, die angezeigt werden, wie das Hauptfenster Video wiedergegeben wird, in der Regel ein Logo oder ein anderes statischen Bild innerhalb der Player platziert.
- **Companion** – anzeigen, die außerhalb des Players angezeigt werden.

Anzeigen können an jeder Stelle in das Hauptfenster Video Zeitachse platziert werden. Sie müssen dem Player mitteilen, wann die Anzeige wiedergeben und welche Werbung zur Wiedergabe. Dies wird mithilfe einer Reihe von standard XML-basierten Dateien: Video Ad Service-Vorlage (VAST), digitale Video mehrere Ad Wiedergabeliste (VMAP), Medien abstrakte Abfolge Vorlage (MOTTE) und digitale Video Player Ad-Oberfläche Definition (VPAID). GROßE Dateien angeben, welche Anzeige dargestellt wird. VMAP Dateien festlegen, wann zu verschiedenen Werbung wiedergeben und große XML enthalten. MOTTE Dateien sind eine weitere Möglichkeit zum Sequenz anzeigen, die auch große XML enthalten können. VPAID Dateien definieren Sie eine Schnittstelle zwischen dem video-Player und den Ad oder Ad-Server.

Jedes Framework Player funktioniert anders, und jeder werden in einem eigenen Thema behandelt werden. In diesem Thema wird die grundlegenden Methoden zum Einfügen von Werbung verwendet werden. Videoplayer Applikationen anfordern anzeigen aus einem Ad-Server. Der Ad-Server kann auf verschiedene Arten Antworten:

- Eine große Datei zurück
- Eine VMAP-Datei (mit eingebetteten VAST) zurück
- Verschieben einer Datei MOTTE (mit eingebetteten VAST)
- Eine große Datei mit VPAID Werbung zurück

 
###<a name="using-a-video-ad-service-template-vast-file"></a>Verwenden eine Video Ad-Dienstdatei Vorlage (VAST)

Eine große Datei gibt an, welche Ad oder Anzeige dargestellt. Die folgende XML-Daten ist ein Beispiel für eine große Datei für eine lineare Anzeige:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>
    
Das lineare Ad wird beschrieben, indem Sie die **<Linear>** Element. Es gibt die Dauer der Anzeige an, klicken Sie auf Verlauf Ereignisse durch, klicken Sie auf Überwachung und eine Reihe von **<MediaFile>** Elemente. Verlauf Ereignisse werden angegeben, in der **<TrackingEvents>** Element und zulassen, dass einen Ad-Server verschiedene Ereignisse zu verfolgen, die beim Anzeigen der Anzeige auftreten. In diesem Fall starten, Mittelpunkt, fertig sind, und blenden Sie Ereignisse erfasst werden. Das Start-Ereignis tritt auf, wenn die Anzeige angezeigt wird. Das Ereignis Mittelpunkt tritt auf, wenn Sie mindestens 50 % der Active Directory-Zeitachse angezeigt wurde. Das Ereignis abgeschlossen tritt auf, wenn die Anzeige bis zum Ende ausgeführt wurde. Das Ereignis erweitern tritt auf, wenn der Benutzer den Videoplayer in den Vollbildmodus erweitert. Clickthroughs angegeben sind, mit einem **<ClickThrough>** Element innerhalb einer **<VideoClicks>** Element und gibt einen URI an eine Ressource angezeigt werden, wenn der Benutzer auf die Anzeige klickt. ClickTracking wird angegeben, einer **<ClickTracking>** Element, auch innerhalb der **<VideoClicks>** Element und gibt eine Ressource Verlauf für die Medienwiedergabe verwendet, um anzufordern, wenn der Benutzer auf die Anzeige klickt. Die **<MediaFile>** Elemente geben Sie Informationen zu einer bestimmten Codierung einer Anzeige. Wenn es mehrere gibt **<MediaFile>** Element, der Videoplayer kann wählen Sie die beste Codierung für die Plattform. 

Lineare anzeigen können in einer bestimmten Reihenfolge angezeigt werden. Hierzu hinzufügen zusätzliche <Ad> Elemente, die die VAST, und geben Sie die Reihenfolge, mit dem Attribut Sequenz. Das folgende Beispiel veranschaulicht dies:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>
    
Nichtlinear Werbung in angegeben sind eine <Creative> auch Element. Das folgende Beispiel zeigt eine <Creative> Element, das eine nichtlinear Ad beschreibt.

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>

 
Die **<NonLinearAds>** Element kann eine oder mehrere enthalten **<NonLinear>** Elemente, die jeweils kann eine Ad nichtlinear beschreiben. Die **<NonLinear>** Element gibt die Ressource für die Anzeige nichtlinear an. Die Ressource kann es sich ein **<StaticResouce>**, eine **<IFrameResource>**, oder eine **<HTMLResouce>**.**<StaticResource>** Beschreibt die eine Ressource HTML-Format, und definiert eine CreativeType-Attribut, das angibt, wie die Ressource angezeigt wird:

Bild/Gif, Image/Jpeg, Image/Png – die Ressource wird angezeigt, in einem HTML- **<img>** Kategorie.

Anwendung/X-Javascript – die Ressource wird in einer HTML-<**Script**>-Tag angezeigt.

Anwendung/X-Shockwave-Flash – die Ressource wird in einem Flash Player angezeigt.

**<IFrameResource>**Beschreibt eine HTML-Ressource, die in einem IFrame angezeigt werden können. **<HTMLResource>**Beschreibt, einen Teil der HTML-Code, der in eine Webseite eingefügt werden kann. **<TrackingEvents>**Geben Sie Verlauf Ereignisse und den URI zum anfordern, wenn das Ereignis eintritt. In diesem Beispiel werden die AcceptInvitation und reduzieren Ereignisse erfasst. Weitere Informationen zu den **<NonLinearAds>** Element und deren untergeordnete, finden Sie unter IAB.NET/VAST. Beachten Sie, dass die **<TrackingEvents>** Element befindet sich in der** <NonLinearAds> ** Element statt der **<NonLinear>** Element.

Companion Werbung in definiert sind eine <CompanionAds> Element. Die <CompanionAds> Element kann eine oder mehrere enthalten <Companion> Elemente. Jede <Companion> Element beschrieben, eine Ad Companion und enthalten kann eine <StaticResource>, <IFrameResource>, oder <HTMLResource> die auf die gleiche Weise wie in einer Ad nichtlinear angegeben sind. Eine große Datei kann mehrere Companion Werbung enthalten, und die Anwendung Player kann auswählen, das am besten geeignete Ad angezeigt werden. Weitere Informationen zu VAST finden Sie unter [große 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

###<a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Verwenden eines digitalen Videos mehrere Ad-Wiedergabelistendatei (VMAP)

Eine Datei VMAP ermöglicht es Ihnen, die angeben, wann Ad Umbrüche ausgeführt, wie lange jedes Umbruch ist, Anzahl der anzeigen in einen Umbruch angezeigt werden können und welche Arten von anzeigen möglicherweise während einer Unterbrechung angezeigt. Die folgende in einer Beispiel VMAP-Datei, die eine einzelne Ad Pause definiert:
    
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>
     
Eine Datei VMAP beginnt mit einer <VMAP> Element, das eine oder mehrere enthält <AdBreak> Elemente, die jeder Definieren einer Ad-Seitenumbruch. Jede Ad Seitenumbruch gibt einen Typ des Seitenumbruchs, Seitenumbruch-ID, und Zeitoffset. Das Attribut BreakType gibt die Art der Anzeige, die während der Unterbrechung wiedergegeben werden kann: linearen, nichtlinear, oder anzeigen. Werbung Karte anzeigen für große Companion anzeigen. Mehr als eine Ad-Typ kann in einer Liste von kommagetrennten (ohne Leerzeichen) angegeben werden. Die BreakID ist ein optionaler Bezeichner für die Anzeige an. Die TimeOffset gibt an, wenn die Anzeige angezeigt werden soll. Sie können auf eine der folgenden Arten angegeben werden:

1. Zeit – im hh: mm: oder hh:mm:ss.mmm Format, wobei .mmm Millisekunden ist. Der Wert dieses Attribut gibt die Zeit ab dem Beginn der Zeitachse video an den Anfang der Unterbrechung Ad an.
1. Prozentsatz – im Format n % ist, wobei n der Prozentsatz der Zeitachse video ausprobieren, bevor Sie die Anzeige wiedergegeben
1. Anfangs-/Ende – gibt an, dass eine Ad angezeigt werden soll, vor oder nach das Video angezeigt worden ist
1. Positionieren – die Reihenfolge der Ad Umbrüche gibt an, wenn die Anzeigedauer für die Ad-Umbrüche unbekannt, z. B. live-streaming ist. Die Reihenfolge der einzelnen Ad Seitenumbruch im Format #n angegeben, wobei n eine ganze Zahl 1 oder größer ist. 1 gibt an, die Anzeige wiedergegeben werden sollte bei der ersten Gelegenheit, 2 bedeutet die Anzeige sollte bei der zweiten Gelegenheit usw. wiedergegeben werden.

Innerhalb des Elements <**AdBreak**> kann ein <**AdSource**> Element vorhanden sein. Das Element <**AdSource**> enthält die folgenden Attribute:

1. ID – gibt eine ID für die Ad-Quelle
1. AllowMultipleAds – der boolesche Wert, der angibt, ob mehrere Werbung während der Seitenumbruch Ad angezeigt werden können
1. FollowRedirects – leitet ein Optionaler boolescher Wert, der angibt, wenn der Videoplayer berücksichtigen sollten in einer Ad-Antwort

Das Element <**AdSource**> stellt dem Player eine Ad-Antwort Inline oder ein Verweis auf eine Ad-Antwort. Sie können eine der folgenden Elemente enthalten:

- <VASTAdData>Zeigt an, dass eine große Ad Antwort innerhalb der VMAP Datei eingebettet ist
- <AdTagURI>ein URI, der eine Ad-Antwort von einem anderen System verweist auf
- <CustomAdData>-eine beliebige Zeichenfolge darstellt nicht große Reaktionen

In diesem Beispiel wird eine Antwort Inline-Ad angegeben, mit einer <VASTAdData> Element, das eine große Ad-Antwort enthält. Weitere Informationen zu den anderen Elementen finden Sie unter [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

Das Element <**AdBreak**> kann auch ein <**TrackingEvents**> Element enthalten. Das <**TrackingEvents**> Element ermöglicht das Nachverfolgen am Anfang oder Ende einer Ad-Unterbrechung oder gibt an, ob während der Ad Seitenumbruch ein Fehler aufgetreten ist. Das Element <**TrackingEvents**> enthält ein oder mehrere <**Überwachung**> Elemente, von die jede ein Ereignis Verlauf und einem Verlauf URI angibt. Die möglichen Verlauf Ereignisse sind:

1. BreakStart – verfolgt Anfang einer Ad-Seitenumbruch
1. BreakEnd – vom Abschluss einer Ad-Unterbrechung nachverfolgen
1. Fehler – verfolgt ein Fehlers während der Ad Seitenumbruch

Im folgenden Beispiel wird eine VMAP-Datei, die angibt, Verlauf Ereignisse

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

Weitere Informationen auf das Element <**TrackingEvents**> und deren untergeordnete finden Sie unter http://iab.org/VMAP.pdf

###<a name="using-a-media-abstract-sequencing-template-mast-file"></a>Verwenden einer Medien abstrakte Abfolge Vorlagendatei (MOTTE)

Eine Datei MOTTE können Sie Trigger angeben, die definieren, wann eine Anzeige angezeigt wird. So sieht ein Beispiel MOTTE-Datei, die Trigger für einer Prä rollupzeilen Ad, eine Ad Mid zurücksetzen und eine Ad nach der rollupzeilen enthält.

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' the trigger for the next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>

 

Eine Datei MOTTE beginnt mit einer **<MAST>** Element, das eine enthält **<triggers>** Element. Die <triggers> Element enthält eine oder mehrere **<trigger>** Elemente, die definieren, wenn eine bestimmte Anzeige wiedergegeben werden soll. 

Die **<trigger>** Element enthält eine **<startConditions>** Element die angeben, wann eine Anzeige zur Wiedergabe beginnen soll. Die **<startConditions>** Element enthält eine oder mehrere <condition> Elemente. Wenn jede <condition> ergibt True ein Triggers wird initiiert oder widerrufen, je nachdem, ob die <condition> in enthalten ist ein **< StartConditions**> oder **<endConditions>** Element Hilfethemas. Wenn mehrere <condition> Elemente vorhanden sind, werden diese als eine implizit oder behandelt, eine Bedingung Auswertung True bewirkt, dass des Triggers einleiten. <condition>Elemente können geschachtelt werden. Wenn untergeordnete <condition> Elemente voreingestellte sind, werden diese als eine implizit und behandelt, allen Bedingungen müssen für den Trigger einleiten true ergeben. Die <condition> Element enthält die folgenden Attributen, die die Bedingung definieren: 

1. **Typ** – gibt den Typ der Bedingung, Ereignis oder Eigenschaft
1. **Name** – der Name der Eigenschaft oder des Ereignisses während der Auswertung verwendet werden
1. **Wert** – der Wert, der eine Eigenschaft ausgewertet
1. **Operator** – der Vorgang bei der Auswertung verwenden: EQ (gleich), NEQ (nicht gleich), GTR (größer), GEQ (größer oder gleich), LT (kleiner als), fahrenden Triebfahrzeugs (kleiner oder gleich), Rest (modulo)

**<endConditions>**auch enthalten <condition> Elemente. Wenn eine Bedingung ausgewertet wird, den Trigger true wird zurückgesetzt. Die <trigger> Element auch enthält eine <sources> Element, das eine oder mehrere enthält <source> Elemente. Die <source> Elemente definieren den URI der Antwort Active Directory und den Typ der Antwort Ad. In diesem Beispiel wird ein URI für eine große Antwort angegeben. 


    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>
 

###<a name="using-video-player-ad-interface-definition-vpaid"></a>Verwenden von Video Player-Ad Interface Definition (VPAID)

VPAID ist eine API für das ausführbare Ad Einheiten zur Kommunikation mit einen Videoplayer aktivieren. Dadurch wird ein Ad hochgradig interaktive Erfahrung. Der Benutzer mit der Anzeige interagieren kann, und die Anzeige auf Aktionen, die vom Viewer Antworten kann. Beispielsweise kann eine Ad Schaltflächen angezeigt, mit die den Benutzer können Sie weitere Informationen oder eine längere Fassung der Anzeige anzeigen können. Der Videoplayer muss die VPAID-API unterstützen, und das ausführbare Ad muss die API implementieren. Wenn ein Spieler anfordert, dass eine Anzeige von einer Ad-Server der Server für eine große Antwort reagieren kann, die eine Ad VPAID enthält.

Eine ausführbare Anzeige wird Code erstellt, die ausgeführt werden müssen, in einer Runtime-Umgebung, z. B. Adobe Flash™ oder JavaScript, die in einem Webbrowser ausgeführt werden kann. Wenn ein Ad-Server eine große Antwort mit einer Anzeige VPAID zurückgibt, das Attribut des Werts von der ApiFramework in der <MediaFile> Element darf "VPAID". Dieses Attribut gibt an, dass die enthaltene Anzeige einer VPAID ausführbare Ad ist. Das Type-Attribut muss den MIME-Typ des Programms, wie etwa "Application/X-Shockwave-Flash" oder "Application/X-Javascript" festgelegt werden. Die folgenden XML-Codeausschnitt zeigt die <MediaFile> Element aus eine große Antwort mit einer VPAID ausführbare Anzeige. 

    
    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>
 

Eine ausführbare Anzeige kann Initialisierung mit der <AdParameters> Element innerhalb der <Linear> oder <NonLinear> Elementen in eine große Antwort. Weitere Informationen zu den <AdParameters> Element, finden Sie unter [große 3.0](http://www.iab.net/media/file/VASTv3.0.pdf). Weitere Informationen über die VPAID-API finden Sie unter [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

##<a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Implementieren eines Windows oder Windows Phone 8-Player mit Ad-Support

Die Microsoft Media-Plattform: Player Framework für Windows 8 und Windows Phone 8 enthält eine Zusammenstellung von Applications Stichprobe, die zeigen, wie Sie eine Videoplayer-Anwendung, die mit Framework implementieren. Sie können das Framework Player und in den Beispielen von [Player Framework für Windows 8 und Windows Phone 8](https://playerframework.codeplex.com)herunterladen.

Wenn Sie die Lösung Microsoft.PlayerFramework.Xaml.Samples öffnen, sehen Sie eine Reihe von Ordnern innerhalb des Projekts. Der Ordner Werbung enthält den Muster-Code für das Erstellen eines Videoplayers mit Ad-Support relevant. Innerhalb der Werbung ist Ordner eine Reihe von XAML/Cs-Dateien mit denen Einlegen von Werbung in eine andere Möglichkeit probiert anzeigen. Die folgende Liste werden die einzelnen beschrieben:

- AdPodPage.xaml wird gezeigt, wie eine Ad-Pod anzuzeigen.
- AdSchedulingPage.xaml wird gezeigt, wie Werbung zu planen.
- FreeWheelPage.xaml wird gezeigt, wie die FreeWheel-Plug-in verwenden, um Werbung zu planen.
- MastPage.xaml wird gezeigt, wie mit einer Datei MOTTE Werbung zu planen.
- ProgrammaticAdPage.xaml wird gezeigt, wie Werbung programmgesteuert in ein Video zu planen.
- ScheduleClipPage.xaml wird gezeigt, wie eine Anzeige ohne eine große Datei zu planen.
- VastLinearCompanionPage.xaml zeigt, wie eine lineare eingefügt und Companion Ad.
- VastNonLinearPage.xaml wird gezeigt, wie eine nichtlineare Ad eingefügt wird.
- VmapPage.xaml wird gezeigt, wie Werbung mit einer Datei VMAP angeben.

Jeder der in diesen Beispielen wird die MediaPlayer-Klasse von Player Framework definiert verwendet. Die meisten Beispiele verwenden-Plug-Ins, die Unterstützung für verschiedene Ad Antwortformate hinzufügen. Im Beispiel ProgrammaticAdPage interagiert programmgesteuert mit einer MediaPlayer-Instanz.

###<a name="adpodpage-sample"></a>Beispiel für AdPodPage

In diesem Beispiel verwendet die AdSchedulerPlugin um zu definieren, wann eine Ad angezeigt werden sollen. In diesem Beispiel wird eine Werbung Mid rollupzeilen geplant, nach 5 Sekunden wiedergegeben werden soll. Der Ad-Pod (einer Gruppe von Werbung in der Reihenfolge angezeigt werden) in eine große Datei von einem Ad-Server zurückgegeben angegeben. Angegebene URI auf die große Datei ist der <RemoteAdSource> Element.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
    
        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>
    
                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>
    
                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

Weitere Informationen zu den AdSchedulerPlugin finden Sie unter [Werbung in Framework Player unter Windows 8 und Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

###<a name="adschedulingpage"></a>AdSchedulingPage

In diesem Beispiel wird auch der AdSchedulerPlugin verwendet. Es plant drei anzeigen, eine Ad vor dem Zurücksetzen, eine Ad Mid zurücksetzen und eine rollupzeilen nach der Anzeige. Der URI, der die VAST für jede Anzeige in angegeben ist ein <RemoteAdSource> Element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>
    
                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


###<a name="freewheelpage"></a>FreeWheelPage

In diesem Beispiel wird die FreeWheelPlugin Source-Attribut gibt an, die einem URI Gibt an, die in eine Datei SmartXML verweist, die Ad-Inhalt als auch Ad Zeitplaninformationen angegeben.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="mastpage"></a>MastPage

In diesem Beispiel wird die MastSchedulerPlugin, die Sie eine Datei MOTTE verwenden kann. Das Source-Attribut gibt den Speicherort der Datei MOTTE an.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="programmaticadpage"></a>ProgrammaticAdPage

In diesem Beispiel interagiert programmgesteuert mit MediaPlayer. Die Datei ProgrammaticAdPage.xaml instanziiert MediaPlayer:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

Die Datei ProgrammaticAdPage.xaml.cs erstellt eine AdHandlerPlugin, fügt einen TimelineMarker, um anzugeben, wenn eine Ad angezeigt werden soll, und klicken Sie dann fügt einen Ereignishandler für das Ereignis MarkerReached lädt eine RemoteAdSource angeben eines URI in eine große Datei und dann wiedergegeben wird die Anzeige.
    
    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;
    
            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }
    
            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

###<a name="scheduleclippage"></a>ScheduleClipPage

In diesem Beispiel wird die AdSchedulerPlugin zum Planen einer Ad Mid rollupzeilen durch Angabe einer WMV-Datei, die die Anzeige enthält verwendet.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearcompanionpage"></a>VastLinearCompanionPage

In diesem Beispiel veranschaulicht, wie die AdSchedulerPlugin verwenden, um eine Mid rollupzeilen linearen Ad mit einer Ad Companion zu planen. Die <RemoteAdSource> Element gibt den Speicherort der große Datei.
    
    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage

In diesem Beispiel werden die AdSchedulerPlugin einen linearen Planung und eine nichtlineare Ad verwendet. Speicherort der große Datei wird angegeben, mit der <RemoteAdSource> Element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
                            
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vmappage"></a>VMAPPage

Diese Beispiele verwendet die VmapSchedulerPlugin mithilfe einer VMAP Datei anzeigen zu planen. Der URI, der die Datei VMAP in das Attribut Quelle der angegeben ist die <VmapSchedulerPlugin> Element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

##<a name="implementing-an-ios-video-player-with-ad-support"></a>Implementieren eines iOS Video-Player mit Ad-Support


Die Microsoft Media-Plattform: Player Framework für iOS enthält eine Auflistung von Applications Stichprobe, die zeigen, wie Sie eine Videoplayer-Anwendung, die mit Framework implementieren. Sie können das Framework Player und in den Beispielen aus [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework)herunterladen. Die Seite Github verfügt über einen Link an einem Wiki, die enthält weitere Informationen zum Player Rahmen und eine Einführung in die Stichprobe Player: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).


###<a name="scheduling-ads-with-vmap"></a>Planen der anzeigen mit VMAP

Im folgenden Beispiel wird veranschaulicht, wie mithilfe einer VMAP Datei anzeigen zu planen.

    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest
    
    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

###<a name="scheduling-ads-with-vast"></a>Planen der anzeigen mit VAST

Im folgende Beispiel veranschaulicht, wie eine späte Bindung Ad große planen.
    
    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
         
   Im folgende Beispiel veranschaulicht, wie eine frühe Bindung große Anzeige planen.
Beispiel 4: Planen einer frühen Bindung große Ad //Download der VAST ablegen, wenn (! [ framework.adResolver DownloadManifest: & Manifesten WithURL: [NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[Self LogFrameworkError];} else {adLinearTime.startTime = 7; adLinearTime.duration = 0;
        
        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

Im folgende Beispiel gezeigt, wie eine Ad mit grob Ausschneiden bearbeiten (RCE) einfügen

    //Example:1 How to use RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

Im folgenden Beispiel wird gezeigt, wie eine Ad-Pod planen.

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;
    
    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Im folgenden Beispiel wird zum Planen eines nicht Kurznotizen Mid rollupzeilen Ad. Ein nicht-Kurznotizen Ad ist nur wiedergegeben werden, nachdem alle Suchvorgänge unabhängig von der Viewer führt.
    
    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Im folgenden Beispiel wird zum Planen einer Kurznotizen Mid rollupzeilen Ad. Eine Kurznotizen Ad wird jedes Mal angezeigt, wenn der angegebene Punkt auf der Zeitachse video erreicht wird.

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


Im folgende Beispiel veranschaulicht, wie eine Ad nach der rollupzeilen zu planen.

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Im folgende Beispiel veranschaulicht, wie eine Ad Vorabversion rollupzeilen planen.
    
    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Im folgende Beispiel veranschaulicht, wie eine Überlagerung Mid rollupzeilen Ad planen.
    
    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
##<a name="see-also"></a>Siehe auch

[Entwickeln von Applications Videoplayer](media-services-develop-video-players.md)
