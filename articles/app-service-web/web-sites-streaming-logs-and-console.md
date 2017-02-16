<properties 
    pageTitle="Streaming Protokolle und Konsole" 
    description="Streaming Protokolle und Console (Übersicht)" 
    authors="btardif" 
    manager="wpickett" 
    editor="" 
    services="app-service\web" 
    documentationCenter=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="byvinyal"/>

# <a name="streaming-logs-and-the-console"></a>Streaming Protokolle und der Verwaltungskonsole

## <a name="streaming-logs"></a>Streaming von Protokollen

Der **Azure-Portal** bietet einen integrierte streaming Log-Viewer, der Sie Tracing Ereignisse aus Ihrer apps **App-Dienst** in Echtzeit anzeigen kann.  

Einrichten von diesem Feature sind einige einfache Schritte erforderlich:

- Schreiben von Code auf
- Aktivieren Sie die Anwendung **Diagnoseprotokolle** für Ihre app
- Stream über die integrierte **Protokolle Streaming** Benutzeroberfläche im **Azure-Portal**.

### <a name="how-to-write-traces-in-your-code"></a>So schreiben Sie Spuren im code ###

Schreiben von Spuren im Code ist einfach.  In c# ist so einfach wie das Schreiben des folgenden Codes:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

Die Spur-Klasse befindet sich im System.Diagnostics-Namespace.

In einer app node.js können Sie diesen Code, um das gleiche Ergebnis erzielen schreiben:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a>So aktivieren und das streaming anzeigen ' Protokolle '
![][BrowseSitesScreenshot]Diagnose auf einer Basis pro-app aktiviert sind. Beginnen Sie, indem Sie zu der Website, der Sie auf dieses Feature aktivieren möchten.  
  
![][DiagnosticsLogs]Wählen Sie im Menü Einstellungen führen Sie einen Bildlauf nach unten bis zum Abschnitt **Überwachung** , und klicken Sie auf **Diagnoseprotokolle (1)**. Klicken Sie dann **(2) aktivieren** **Anwendung Protokollierung (Dateisystem)** oder **Anwendung Protokollierung (Blob)** die **Ebene** Option können Sie die Sicherheitsebene zum Erfassen von Spuren zu ändern. Wenn Sie gerade versuchen, um das Feature kennenzulernen, schieben Sie die Ebene **ausführlich** , um sicherzustellen, dass alle Anweisungen Spur erfasst wurden.

Klicken Sie auf **Speichern** , am oberen Rand der Blade und nun Protokolle anzeigen.

>[AZURE.NOTE] Je höher werden der **schwere Ebene** , die die weitere Ressourcen für die Anmeldung und die weitere Spuren verbraucht hergestellt. Stellen Sie sicher, dass **schwere Ebene** an die richtige Ausführlichkeit für eine Herstellung oder hoher Auslastung der Website konfiguriert ist. 

![][StreamingLogsScreenshot]Wenn das **streaming Protokolle** aus innerhalb des Portals Azure anzeigen möchten, klicken Sie auf **(1) Log Stream** auch im Abschnitt **für die Überwachung** im Menü Einstellungen. Wenn Ihre app aktiv Spur Anweisungen schreiben ist, sollten Sie diese in der **Benutzeroberfläche (2) streaming protokolliert werden** nahezu in Echtzeit angezeigt.

## <a name="console"></a>Konsole
Der **Azure-Portal** bietet Console-Zugriff auf Ihre app. Können Sie Ihre app Dateisystem durchsuchen und Powershell/Cmd Skripts ausführen. Sie werden mit dieselben Berechtigungen wie Ihre laufenden app-Code festgelegt werden, wenn Console-Befehle ausgeführt gebunden. Zugriff auf geschützte Verzeichnisse oder Ausführen von Skripts, die erweiterte Berechtigungen erfordern ist gesperrt.  

![][ConsoleScreenshot]Wählen Sie im Menü Einstellungen führen Sie einen Bildlauf nach unten bis zum Abschnitt " **Development Tools** " und klicken Sie auf **Konsole (1)** und **(2) Console** Benutzeroberfläche rechts geöffnet.

Um die **Konsole**kennenzulernen, versuchen Sie grundlegende Befehle wie ein:

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png
