<properties 
    pageTitle="Einbetten eines MPEG-Strich Adaptive Streaming-Videos in einer Anwendung HTML5 mit DASH.js | Microsoft Azure" 
    description="In diesem Thema veranschaulicht, wie eine MPEG-Strich Adaptive Streaming Video in einer Anwendung HTML5 mit DASH.js einbetten." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>Einbetten eines MPEG-Strich Adaptive Streaming-Videos in einer Anwendung HTML5 mit DASH.js

##<a name="overview"></a>(Übersicht)

MPEG-Strich ist ein ISO-Standard für das adaptive streaming Videoinhalt seiner signifikante Vorteile für diejenigen, die die hoher Qualität, anpassbarer Video streaming Ausgabe vorführen möchten. Mit MPEG-Strich wird der Videodatenstrom automatisch zur unteren Definition getrennt, wenn das Netzwerk belastet wird. Reduzieren Sie die Wahrscheinlichkeit des Viewers "angehalten" Video angezeigt, während der Player-downloads für den nächsten Sekunden zur Wiedergabe (QuickInfos Pufferung). Überlastung des Netzwerks verringert, wird der Videoplayer wiederum in einen höheren Qualität Stream zurückzugeben. Die Möglichkeit, die erforderliche Bandbreite anpassen führt außerdem eine schnellere Startzeit für Video. Dies bedeutet, dass die ersten Sekunden in einem Fast-zu-Download niedriger Qualität Segment wiedergegeben werden können, und klicken Sie dann auf einer höheren Qualität Inhalt einmal ausreichend Schritt gepuffert wurde weist.

Dash.js ist eine MPEG-Strich-Videoplayer Quelle öffnen in JavaScript geschrieben. Ihr Ziel ist einen robusten, Plattform-Player bereitstellen, der frei in Clientanwendungen wiederverwendet werden kann, die Videowiedergabe erfordern. Darüber MPEG-Strich Videowiedergabe in einen beliebigen Browser, die heute den W3C Medien Quelle Erweiterungen (MSE) unterstützt, die Chrome, Microsoft Edge und IE11 (andere Browser haben ihre Absicht zur Unterstützung von MSE gekennzeichnet) ist. Weitere Informationen zu DASH.js finden Sie unter Js Repository dash.js GitHub.


##<a name="creating-a-browser-based-streaming-video-player"></a>Erstellen einen browserbasierte streaming Videoplayer

Um eine einfache Webseite zu erstellen, die einen Videoplayer mit dem erwarteten zeigt Steuerelemente solche Wiedergabe, Pause, Zurückspulen usw., müssen Sie:

1. Erstellen Sie eine HTML-Seite
1. Fügen Sie das video tag
1. Hinzufügen des dash.js Players
1. Initialisierung des Players
1. Fügen Sie einige CSS-Format hinzu
1. Anzeigen der Ergebnisse in einem Browser, der MSE implementiert

Initialisierung des Players kann nur ein paar Zeilen JavaScript-Code ausgeführt werden. Verwenden dash.js, ist einfach es wirklich MPEG-Strich Video in Ihrem Browser-basierte Clientanwendungen einbetten.

##<a name="creating-the-html-page"></a>Erstellen der HTML-Seite

Der erste Schritt besteht im Erstellen einer Standardseite der HTML-Code enthält, für das **video** speichern diese Datei als basicPlayer.html, wie im folgende Beispiel veranschaulicht wird:

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

##<a name="adding-the-dashjs-player"></a>Hinzufügen der DASH.js Player

Zum Hinzufügen der Implementierung dash.js Bezug zu der Anwendung müssen Sie die Datei dash.all.js aus der Version 1.0 von Project dash.js umgibt. Dies sollte der Anwendung im JavaScript-Ordner gespeichert werden. Diese Datei ist eine Komfort, der alle notwendigen dash.js-Code zusammen in einer einzigen Datei abruft. Wenn Sie sich um die dash.js Repository haben, wird Sie die einzelnen Dateien zu finden, Testen Code und vieles mehr, aber wenn Sie alle Aufgaben ausführen möchten werden dash.js verwenden und anschließend die dash.all.js-Datei befindet, benötigen Sie.

Um den Clientanwendungen im Player dash.js hinzufügen, fügen Sie eine Skripttag den oberen Abschnitt des basicPlayer.html hinzu:

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


Erstellen Sie dann eine Funktion, um den Player Initialisierung beim Laden der Seite ein. Fügen Sie nach der Zeile, in der Sie dash.all.js Laden Sie, das folgende Skript hinzu:

    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

Diese Funktion erstellt zunächst eine DashContext. Hiermit wird die Anwendung für eine bestimmte Runtime-Umgebung konfigurieren. Vom technischen Gesichtspunkt definiert er die Klassen, die das Abhängigkeit einfügen Framework beim Erstellen der Anwendungs verwendet werden sollen. Verwenden Sie in den meisten Fällen Dash.di.DashContext.

Als Nächstes Instanz Sie die primäre Klasse von dash.js Framework, MediaPlayer erstellt. Diese Klasse enthält das Herzstück Methoden wie erforderlich wiedergeben und anhalten, verwaltet die Beziehung mit dem video Element und verwaltet werden auch die Interpretation der Datei Medien Präsentation Beschreibung (MPD) das beschreibt das Video wiedergegeben werden soll.

Die Funktion startup() der MediaPlayer-Klasse wird aufgerufen, um sicherzustellen, dass der Player zum Wiedergeben von Videos bereit ist. Unter anderem: Diese Funktion damit ist sichergestellt, dass alle notwendigen Klassen (wie im Kontext definiert) geladen wurden. Nachdem der Player fertig ist, können Sie mithilfe der Funktion attachView() das video Element anfügen. Dadurch MediaPlayer den Videodatenstrom in das Element einfügen und die Wiedergabe nach Bedarf auch steuern.

Übergeben Sie die URL der Datei MPD an MediaPlayer, damit es zum Video weiß, dass es zur Wiedergabe erwartet wird. Die soeben erstellte setupVideo()-Funktion müssen ausgeführt werden, nachdem die Seite vollständig geladen wurde. Vornehmen Sie, indem Sie das Ereignis Onload des Textkörperelements. Ändern der <body> Element:

    <body onload="setupVideo()">

Legen Sie schließlich die Größe des Videos mit CSS-Elements an. In einer Umgebung adaptive streaming ist dies besonders wichtig, da die Größe des Videos wiedergegeben werden während der Wiedergabe anpasst zum Ändern von netzwerkbedingungen ändern kann. Erzwingen Sie in diesem einfachen Demo einfach video Elements 80 % des Browserfensters verfügbar sein, indem Sie die folgenden CSS Abschnitt am Rand der Seite hinzufügen:
    
    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

##<a name="playing-a-video"></a>Wiedergeben eines Videos

Wiedergeben eines Videos, zeigen Sie Ihren Browser am basicPlayback.html Datei, und klicken Sie auf Wiedergabe klicken Sie auf der Videoplayer angezeigt.


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Siehe auch

[Entwickeln von Applications Videoplayer](media-services-develop-video-players.md)

[GitHub dash.js repository](https://github.com/Dash-Industry-Forum/dash.js) 
