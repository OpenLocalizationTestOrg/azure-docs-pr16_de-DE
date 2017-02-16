<properties
    pageTitle="Verfolgen des Ablaufs eines Cloud Services-Anwendung mit Azure-Diagnose | Microsoft Azure"
    description="Hinzufügen von Tracing Nachrichten zu Azure dieser Anwendung debuggen Messen der Leistung, Überwachung, Analyse des Datenverkehrs und vieles mehr."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Verfolgen des Ablaufs eine Cloud Services-Anwendung mit Azure-Diagnose

Tracing ist eine Möglichkeit zum Überwachen der Ausführung der Anwendung, während der Ausführung. Sie können die [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx)und [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) Klassen verwenden, Informationen zu Fehlern und Ausführung der Anwendung in Protokolle, Textdateien oder anderen Geräten für die spätere Analyse aufzeichnen. Weitere Informationen zum Verfolgen finden Sie unter [Tracing and Instrumenting Applications](https://msdn.microsoft.com/library/zs6s4h68.aspx).


## <a name="use-trace-statements-and-trace-switches"></a>Verwenden von Spur Anweisungen und Spur Schalter

Implementieren Sie gezeichnet, die in der Cloud Services-Anwendung durch Hinzufügen der [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) der Anwendungskonfiguration und Anrufe System.Diagnostics.Trace oder System.Diagnostics.Debug in Ihrer Anwendungscode. Verwenden Sie die Konfiguration Datei *app.config* für Worker-Rollen und *web.config* für Webrollen. Beim Erstellen eines neuen gehosteten Diensts mithilfe einer Vorlage für Visual Studio Azure-Diagnose wird automatisch dem Projekt hinzugefügt, und der DiagnosticMonitorTraceListener wird der entsprechende Konfigurationsdatei für die Rollen, die Sie hinzufügen hinzugefügt.

Informationen zum Platzieren Spur Anweisungen finden Sie unter [wie: Hinzufügen Spur-Anweisungen Anwendungscode](https://msdn.microsoft.com/library/zd83saa2.aspx).

Indem Sie [Spur Schalter](https://msdn.microsoft.com/library/3at424ac.aspx) im Code platzieren, können Sie steuern, ob Tracing tritt auf, und wie umfangreich ist. Auf diese Weise können Sie den Status Ihrer Anwendung in einer Umgebung für die Herstellung zu überwachen. Dies ist besonders wichtig in einem Business-Anwendung, die mehrere Komponenten auf mehreren Computern verwendet. Weitere Informationen finden Sie unter [wie: Konfigurieren von Spur Schalter](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-the-trace-listener-in-an-azure-application"></a>Konfigurieren der Spur Zuhörer in Azure-Anwendung

Spur, Debuggen und TraceSource, ist es erforderlich, die richten "Listener" zu sammeln und Aufzeichnen von Nachrichten, die gesendet werden. Listener sammeln, speichern und Weiterleiten von Nachrichten Tracing. Diese leiten Sie die Tracing Ausgabe an ein entsprechendes Ziel, beispielsweise ein Protokoll, Fenster oder Textdatei. Azure Diagnose verwendet die [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) -Klasse.

Bevor Sie das folgende Verfahren abgeschlossen haben, müssen Sie den Azure diagnostic Monitor Initialisierung. Hierzu finden Sie unter [Aktivieren der Diagnose in Microsoft Azure](cloud-services-dotnet-diagnostics.md).

Beachten Sie, dass, wenn Sie die Vorlagen, die von Visual Studio bereitgestellt werden verwenden, die Konfiguration von der Zuhörer automatisch für Sie hinzugefügt wird.


### <a name="add-a-trace-listener"></a>Hinzufügen einer Spur Zuhörer

1. Öffnen Sie die Datei web.config oder app.config für Ihre Rolle.
2. Fügen Sie den folgenden Code zu der Datei ein. Ändern Sie das Attribut Version, um die Versionsnummer der Assembly verwenden, die Sie verweisen. Die Assemblyversion wird mit jeder Version Azure SDK nicht unbedingt geändert, es sei denn, es gibt Updates darauf.

    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                  <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
    >[AZURE.IMPORTANT] Stellen Sie sicher, dass Sie einen Projektverweis auf die Assembly Microsoft.WindowsAzure.Diagnostics haben. Aktualisieren Sie die Versionsnummer in der obigen Xml entsprechend die Version der Assembly Microsoft.WindowsAzure.Diagnostics verwiesen wird.

3. Speichern Sie die Datei Config.

Weitere Informationen zu Listener finden Sie unter [Ablaufverfolgungslistener](https://msdn.microsoft.com/library/4y5y10s7.aspx).

Nachdem Sie die Schritte zum Hinzufügen der Zuhörer abgeschlossen haben, können Sie dem Code Spur Anweisungen hinzufügen.


### <a name="to-add-trace-statement-to-your-code"></a>So Code Spur-Anweisung hinzu

1. Öffnen Sie eine Quelldatei für eine Anwendung. Angenommen, die <RoleName>cs-Datei für die Worker-Rolle oder Webrolle.
2. Fügen Sie die folgende Anweisung verwenden, wenn es nicht bereits hinzugefügt wurde:
    ```
        using System.Diagnostics;
    ```
3. Nehmen Sie Spur-Anweisung, die Informationen über den Status Ihrer Anwendung erfassen möchten. Eine Vielzahl von Methoden können Sie die Ausgabe der Spur-Anweisung formatieren. Weitere Informationen finden Sie unter [wie: Hinzufügen Spur-Anweisungen Anwendungscode](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Speichern Sie die Quelldatei ein.
