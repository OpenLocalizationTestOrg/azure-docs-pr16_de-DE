<properties 
    pageTitle="Versionsinformationen für Anwendung Einsichten für Windows" 
    description="Die neuesten Updates für Windows Store-SDK." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/12/2016" 
    ms.author="joshweb"/>
 
# <a name="release-notes-for-application-insights-sdk-for-windows-phone-and-store"></a>Versionsinformationen für die Anwendung Einsichten SDK für Windows Phone und speichern

Die Anwendung Einsichten SDK sendet werden über Ihre app live an [Anwendung Einblicken](https://azure.microsoft.com/services/application-insights/), wo Sie deren Verwendung und Leistung analysieren können.


## <a name="version-111"></a>Version 1.1.1

### <a name="windows-sdk"></a>Windows SDK

- Beheben Sie ein hängen während Absturz beim Verwenden der Windows Phone-Silverlight-SDK. Nach dieser Änderung alle Absturz, die später als ~ 2 Sekunden nach dem WindowsAppInitialier.InitializeAsync(...) aufrufen geschieht zum beibehalten werden Datenträger und wird gesendet werden, um das nächste Mal die app gestartet wird. Wenn es sich bei ein Absturz vor ~ 2 Sekunden nach den Anruf ausgeführt wird, wird es ignoriert.  
- Legen Sie die NuGet Abhängigkeiten auf einer bestimmten Version von Core und Microsoft.ApplicationInsights.PersistenceChannel (v1.2.3).   

### <a name="core-sdk"></a>Core SDK

- Core wird im Github verwaltet. Zukünftigen Versionsinformationen des Core SDK finden Sie [im github](http://github.com/Microsoft/ApplicationInsights-dotnet/releases)

## <a name="version-11"></a>Version 1.1

### <a name="core-sdk"></a>Core SDK

- SDK führt jetzt neuen werden-ein ```DependencyTelemetry``` der enthält Informationen zur Abhängigkeit Anruf aus einer Anwendung
- Neue Methode ```TelemetryClient.TrackDependency``` ermöglicht, Informationen zu Anrufe Abhängigkeit von Anwendung senden

## <a name="version-100"></a>Version 1.0.0

### <a name="windows-app-sdk"></a>Windows-App SDK

- Neue Initialisierung für Windows-Apps. Neue `WindowsAppInitializer` Klasse mit `InitializeAsync()` Methode für den Neustart Initialisierung SDK Websitesammlung ermöglicht. Diese Änderung ermöglichen eine bessere Kontrolle und erheblichen app Initialisierung leistungsverbesserungen über vorherigen ApplicationInsights.config Methode an.
- DeveloperMode wird nicht mehr automatisch festgelegt werden. So ändern Sie DeveloperMode Verhalten, die im Code angegeben werden muss.
- NuGet-Paket Barcode nicht mehr ApplicationInsights.config. Empfiehlt sich die neuen WindowsAppInitializer verwendet, wenn NuGet-Paket manuell hinzufügen.
- Nur ApplicationInsights.config liest `<InstrumentationKey>`, werden alle anderen Einstellungen zugunsten WindowsAppInitializer Einstellungen ignoriert.
- Store Markt werden automatisch von SDK erfasst.
- Viele Updates, Stabilität und Leistung Erweiterungen.

### <a name="core-sdk"></a>Core SDK

- ApplicationInsights.config Datei ist nicht mehr Requiered. Und nicht durch die NuGet-Paket hinzugefügt. Konfiguration kann vollständig in Code angegeben werden muss.
- NuGet-Paket wird eine Zieledatei nicht mehr zu Ihrer Lösung hinzufügen. Dadurch wird die automatische Einstellung der DeveloperMode während eines Debuggen Builds entfernt. DeveloperMode sollte Code manuell festgelegt werden.

## <a name="version-017"></a>Version 0,17

### <a name="windows-app-sdk"></a>Windows-App SDK

- Windows-App SDK unterstützt jetzt Universal Windows-Apps für Windows-10-Technische Vorschau und im Vergleich mit einer 2015 RC erstellt.

### <a name="core-sdk"></a>Core SDK

- TelemetryClient Standardeinstellungen Initialisierung mit der InMemoryChannel.
- Neue API hinzugefügt, `TelemetryClient.Flush()`. Diese Methode Löschvorgang werden alle an diesen Client angemeldet werden sofort blockierenden hochzuladen auslösen. Dies ermöglicht das manuelle Auslösen des Uploads vor dem Beenden des Prozesses.
- NuGet-Paket hinzugefügt ein Ziel .net 4.5. Dieses Ziel weist keine externen Abhängigkeiten (entfernte BCL und Quelle Abhängigkeiten).

## <a name="version-016"></a>Version 0,16 

Vorschau 2015-04-28

- SDK unterstützt jetzt DNX Zielplattform für die Überwachung von Applications [Core .NET Framework](http://www.dotnetfoundation.org/NETCore5) aktivieren.
- Instanzen von ```TelemetryClient``` Instrumentation Schlüssel nicht mehr Zwischenspeichern. Jetzt, wenn Instrumentation Schlüssel im festgelegt wurde nicht ```TelemetryClient``` explizit ```InstrumentationKey``` null zurück. Es behebt ein Problem beim Festlegen ```TelemetryConfiguration.Active.InstrumentationKey``` nach einige werden wurde bereits gesammelt, Module werden wie Abhängigkeit Collection, Web Anfragen Sammlung und Leistung Indikatoren Datensammlung wird neuen Instrumentation Product Key verwendet.

## <a name="version-015"></a>Version 0,15 verwendet wird.

- Neue Eigenschaft ```Operation.SyntheticSource``` jetzt erhältlich auf ```TelemetryContext```. Jetzt können Sie Ihre Elemente werden als "keiner realen Benutzerdatenverkehr" markieren und angeben, wie dieser Datenverkehr generiert wurde. Als Beispiel durch Festlegen dieser Eigenschaft können Sie den Datenverkehr aus der Automatisierung von laden Test Datenverkehr unterscheiden.
- Channel Logik wurde in das separate NuGet aufgerufen Microsoft.ApplicationInsights.PersistenceChannel verschoben. Standardkanal ist jetzt InMemoryChannel bezeichnet.
- Neue Methode ```TelemetryClient.Flush``` werden Elemente aus dem Puffer geleert werden synchron ermöglicht

## <a name="version-013"></a>Version 0.13

Keine Versionsinformationen für ältere Versionen zur Verfügung. 
