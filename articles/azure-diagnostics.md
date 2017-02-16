<properties
    pageTitle="Übersicht über Azure Diagnose | Microsoft Azure"
    description="Verwenden Sie für das Debuggen, Messen der Leistung, Überwachung, Analyse des Datenverkehrs in der Cloud Services, virtuellen Computern und Dienst Fabric Azure-Diagnose"
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/02/2016"
    ms.author="robb"/>


# <a name="what-is-microsoft-azure-diagnostics"></a>Was ist Microsoft Azure-Diagnose


Azure-Diagnose ist eine Funktion in Azure, die die Sammlung von Diagnoseprotokollen Daten auf einer bereitgestellten Anwendung ermöglicht. Sie können die Erweiterung Diagnose aus einer Reihe von verschiedenen Quellen verwenden. Aktuell nicht unterstützt werden, Azure Cloud-Dienst Web und Worker-Rollen, Azure virtuellen Computern unter Microsoft Windows und Dienst Fabric. Andere Dienste Azure verfügen über eigene separate Diagnose.

## <a name="data-you-can-collect"></a>Daten, die Sie erfassen können

Azure-Diagnose kann die folgenden Arten von Daten erfassen:

Datenquelle|Beschreibung
---|---
-Datenquellen | Betriebssystem und benutzerdefinierte-Datenquellen
Von Anwendungsprotokollen     | Verfolgen von Nachrichten, die von der Anwendung geschrieben
Windows-Ereignisprotokollen   | An das Windows-Ereignis Protokollierungssystem gesendet werden
.NET Quelle    | Schreiben von Ereignissen mithilfe der Klasse .NET [Quelle](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) Code
IIS-Protokolle             | Informationen zu IIS-Websites
Manifest Grundlage ETW   | Ereignis Tracing for Windows-Ereignisse, die von einem Prozess generiert
Speichert abstürzen          | Informationen über den Status des Prozesses bei einem Absturz der Anwendung
Benutzerdefinierte Fehlerprotokolle    | Erstellt von Ihrer Anwendung oder Dienst Protokolle
Azure Diagnostic Infrastructure Protokolle|Informationen zu Diagnose selbst

Die Erweiterung Azure Diagnose können Sie diese Daten mit einer Firma Azure-Speicher übertragen oder für Dienste wie [Anwendung Einsichten](./application-insights/app-insights-cloudservices.md)zu senden. Sie können die Daten für das Debuggen und Problembehandlung, Messen der Leistung, Überwachung, Ressource: Einsatz hinzu, die Analyse des Datenverkehrs und Kapazität, Planung und Überwachung verwenden.


## <a name="versioning"></a>Versionsverwaltung
[Versionsverlauf Azure-Diagnose](azure-diagnostics-versioning-history.md)finden Sie unter.

## <a name="next-steps"></a>Nächste Schritte
Wählen Sie welche Dienst sammeln Diagnose auf und verwenden die folgenden Artikeln zur Seite Erste Schritte werden soll. Verwenden Sie die allgemeinen Azure Diagnose-Links für bestimmte Vorgänge zu Referenzzwecken.

## <a name="web-apps"></a>Web Apps
Beachten Sie, dass Web Apps Azure-Diagnose nicht verwenden. Suchen Sie die entsprechende Informationen in [Web Apps](./app-service-web/web-sites-enable-diagnostic-log.md)

## <a name="cloud-services-using-azure-diagnostics"></a>Cloud-Diensten mit Azure-Diagnose
- Wenn Visual Studio verwenden zu können, finden Sie unter [Visual Studio verwenden, um eine Cloud Services-Anwendung zu verfolgen](./vs-azure-tools-debug-cloud-services-virtual-machines.md) , um anzufangen. Andernfalls finden Sie unter
- [Zum Überwachen der Cloud-Diensten mit Azure-Diagnose](./cloud-services/cloud-services-how-to-monitor.md)
- [Einrichten von Azure-Diagnose in einer Cloud Services-Anwendung](./cloud-services/cloud-services-dotnet-diagnostics.md)

Erweiterte Themen finden Sie unter

- [Verwenden von Azure Diagnose mit Anwendung Einsichten für Clouddienste](./application-insights/app-insights-cloudservices.md)
- [Verfolgen des Ablaufs eine Cloud Services-Anwendung mit Azure-Diagnose](./cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
- [Verwenden von PowerShell Diagnose auf Cloud Services einrichten](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)


## <a name="virtual-machines-using-azure-diagnostics"></a>Virtuelle Maschinen mit Azure-Diagnose
- Wenn Visual Studio verwenden zu können, finden Sie unter [Mit Visual Studio Spur Azure-virtuellen Computern](./vs-azure-tools-debug-cloud-services-virtual-machines.md) um anzufangen. Andernfalls finden Sie unter
- [Einrichten von Azure-Diagnose auf eine Azure-virtuellen Computern](./virtual-machines-dotnet-diagnostics.md)

Erweiterte Themen finden Sie unter

- [Verwenden von PowerShell Diagnose auf Azure virtuellen Computern einrichten](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)
- [Erstellen Sie einen virtuellen Windows-Computer mit für die Überwachung und Diagnose mit Azure Ressourcenmanager Vorlage](./virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)

## <a name="service-fabric-using-azure-diagnostics"></a>Dienst Fabric mit Azure-Diagnose
Erste Schritte mit [Monitor eine Fabric Service-Anwendung](./service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). In diesem Artikel werden sind viele weitere Dienst Fabric Diagnose Artikel im Navigationsbereich auf der linken Seite verfügbar.

## <a name="general-azure-diagnostics-articles"></a>Artikel Allgemein Azure-Diagnose
- [Konfiguration von Azure Diagnose Schema](https://msdn.microsoft.com/library/azure/mt634524.aspx) - Informationen zum Ändern der Schemadatei zum Sammeln und Diagnosedaten weiterleiten. Beachten Sie, dass Sie Visual Studio auch so ändern Sie die Schemadatei verwenden können.
- [Wie Daten in Azure Storage gespeichert sind Azure Diagnose](./cloud-services/cloud-services-dotnet-diagnostics-storage.md) - wissen die Namen von Tabellen und Blobs, in dem die Diagnose Daten geschrieben ist.
- Informationen Sie zum [Verwenden der Leistungsindikatoren in Azure-Diagnose](./cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
- Sie lernen, wie [Routing Azure Diagnoseinformationen zu Einsichten Anwendung](./azure-diagnostics-configure-applicationinsights.md)
- Wenn Sie Probleme mit Diagnose Anfangs- oder Suchen von Daten in Tabellen Azure-Speicher haben, finden Sie unter [Problembehandlung Azure-Diagnose](./azure-diagnostics-troubleshooting.md)
