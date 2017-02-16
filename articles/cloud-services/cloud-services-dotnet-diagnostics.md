<properties
    pageTitle="Verwendung von Azure Diagnose (.NET) mit Cloud Services | Microsoft Azure"
    description="Verwenden von Azure Diagnose zum Sammeln von Daten aus Azure Cloud Services für Debuggen, Messen der Leistung, Überwachung, Analyse des Datenverkehrs und vieles mehr."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="01/25/2016"
    ms.author="robb"/>



# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Azure Diagnose Azure-Cloud-Dienste zu aktivieren

Einen Hintergrund auf Azure-Diagnose finden Sie unter [Übersicht über Azure-Diagnose](../azure-diagnostics.md) .


## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>Aktivieren der Diagnose in eine Worker-Rolle

Diese Durchgang durch beschrieben, wie eine Azure Worker-Rolle implementiert wird, die werden Daten mithilfe der Klasse .NET Quelle Gibt aus. Azure Diagnose wird verwendet, um die werden Daten sammeln und speichern Sie es in ein Konto Azure-Speicher. Beim Erstellen einer Worker-Rolle aktiviert Diagnose 1.0 als Teil der Lösung in Azure SDKs für .NET 2.4 und früher Visual Studio automatisch. Die folgenden Anweisungen beschreiben Sie den Vorgang zum Erstellen der Worker-Rolle Diagnose 1.0 aus der Lösung und Bereitstellen von Diagnose 1.2 oder 1.3 für Ihre Rolle Worker deaktivieren.

### <a name="pre-requisites"></a>Erforderliche Komponenten
In diesem Artikel wird vorausgesetzt, Sie haben ein Azure-Abonnement und Visual Studio 2013 mit dem Azure SDK verwenden. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie für die [Kostenlose Testversion][]registrieren. Stellen Sie sicher, [Installieren und Konfigurieren von Azure PowerShell Version 0.8.7 oder höher][].

### <a name="step-1-create-a-worker-role"></a>Schritt 1: Erstellen einer Worker-Rolle
1.  Starten Sie **Visual Studio 2013**.
2.  Erstellen eines neuen Projekts von **Azure-Cloud-Dienst** aus der **Cloud** -Vorlage, das auf .NET Framework 4.5.  Nennen Sie das Projekt "WadExample", und klicken Sie auf Ok.
3.  Wählen Sie **Worker-Rolle** aus, und klicken Sie auf Ok. Das Projekt wird erstellt.
4.  **Lösung-Explorer**Doppelklicken Sie auf die Datei **WorkerRole1** Eigenschaften.
5.  In der **Konfiguration** der Registerkarte heben Sie die Markierung **Diagnose aktivieren** aus, um die Diagnose 1.0 (Azure SDK 2.4 und Eariler) zu deaktivieren.
6.  Erstellen Sie Ihre Lösung, um sicherzustellen, dass Sie keine Fehler aufweisen.

### <a name="step-2-instrument-your-code"></a>Schritt 2: Instrumentieren von code
Ersetzen Sie den Inhalt der WorkerRole.cs mit den folgenden Code ein. Die Klasse SampleEventSourceWriter, von der [Quelle Class][], geerbt implementiert vier Protokollierungsmethoden: **SendEnums**, **MessageMethod**, **SetOther** und **HighFreq**. Erste Parameter der Methode **WriteEvent** definiert die ID für den jeweiligen Ereignis. Die Run-Methode implementiert eine unbegrenzte Schleife, die jeder der Protokollierungsmethoden implementiert in der Klasse **SampleEventSourceWriter** alle 10 Sekunden ruft an.

    using Microsoft.WindowsAzure.ServiceRuntime;
    using System;
    using System.Diagnostics;
    using System.Diagnostics.Tracing;
    using System.Net;
    using System.Threading;

    namespace WorkerRole1
    {
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
    }


### <a name="step-3-deploy-your-worker-role"></a>Schritt 3: Bereitstellen von Ihrer Rolle Worker
1.  Stellen Sie Ihre Arbeitskollegen Rolle in Azure aus in Visual Studio, indem Sie das Projekt **WadExample** in der Lösung-Explorer, und drücken Sie dann **Veröffentlichen** über das Menü **Erstellen** auswählen.
2.  Wählen Sie Ihr Abonnement.
3.  Wählen Sie im Dialogfeld **Microsoft Azure veröffentlichen Einstellungen** **Neu erstellen...**ein.
4.  Geben Sie im Dialogfeld **Cloud-Dienst erstellen und Speicher-Konto** einen **Namen** (beispielsweise "WadExample"), und wählen Sie eine Region oder die Zugehörigkeit Gruppe.
5.  Einrichten der **Umgebung** zu **Staging**.
6.  Ändern Sie andere **Einstellungen** nach Bedarf, und klicken Sie auf **Veröffentlichen**.
7.  Nach Abschluss der Bereitstellung im klassischen Azure-Portal überprüfen Sie, ob Ihre Cloud-Dienst in einem Zustand **ausgeführt** wird.

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>Schritt 4: Erstellen Sie Ihrer Konfigurationsdatei Diagnose und installieren Sie die Erweiterung
1.  Herunterladen der öffentlichen Konfiguration Datei Schemadefinition durch den folgenden PowerShell-Befehl ausführen:
2.
        (Get-AzureServiceAvailableExtension-.-Kennung Erweiterungsname 'PaaSDiagnostics' - ProviderNamespace 'Microsoft.Azure.Diagnostics'). PublicConfigurationSchema | Out-File-Codierung utf8 - Dateipfad 'WadConfig.xsd'

2.  Hinzufügen eine XML-Datei zu einem Projekt **WorkerRole1** mit der rechten Maustaste auf das Projekt **WorkerRole1** , und wählen Sie **Hinzufügen** -> **Neues Element...**  ->  **C#-Elemente** -> **Daten** -> **XML-Datei**. Benennen Sie die Datei "WadExample.xml" ein.

    ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)

3.  Ordnen Sie die WadConfig.xsd der Konfigurationsdatei. Stellen Sie sicher, dass das WadExample.xml-Editor-Fenster das aktive Fenster ist. Drücken Sie **F4** , um das Fenster **Eigenschaften** zu öffnen. Klicken Sie auf die Eigenschaft **mithilfe von Schemas** im Fenster **Eigenschaften** . Klicken Sie auf die **...** in der Eigenschaft **mithilfe von Schemas** . Klicken Sie auf die **hinzufügen...** Schaltfläche, und navigieren Sie zu dem Speicherort, an dem Sie die XSD-Datei gespeichert, und wählen Sie die Datei WadConfig.xsd. Klicken Sie auf **OK**.
4.  Ersetzen Sie den Inhalt der Konfigurationsdatei WadExample.xml mit den folgenden XML-Code ein, und speichern Sie die Datei. Diese Konfigurationsdatei definiert einige Leistungsindikatoren zu sammeln: eine für die CPU-Auslastung und eine für die arbeitsspeichernutzung. Klicken Sie dann definiert die Konfiguration der vier Ereignisse, die Methoden in der Klasse SampleEventSourceWriter entspricht.

```
        <?xml version="1.0" encoding="utf-8"?>
        <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
            <WadCfg>
                <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
                <PerformanceCounters scheduledTransferPeriod="PT1M">
                    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
                    <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
                    </PerformanceCounters>
                    <EtwProviders>
                        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
                            <Event id="1" eventDestination="EnumsTable"/>
                            <Event id="2" eventDestination="MessageTable"/>
                            <Event id="3" eventDestination="SetOtherTable"/>
                            <Event id="4" eventDestination="HighFreqTable"/>
                            <DefaultEvents eventDestination="DefaultTable" />
                        </EtwEventSourceProviderConfiguration>
                    </EtwProviders>
                </DiagnosticMonitorConfiguration>
            </WadCfg>
        </PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>Schritt 5: Installieren Sie Diagnose auf Ihre Worker-Rolle
Sind die PowerShell-Cmdlets für die Verwaltung von Diagnose auf einer Website oder Arbeitskollegen Rolle: Set-AzureServiceDiagnosticsExtension und Get-AzureServiceDiagnosticsExtension-AzureServiceDiagnosticsExtension entfernen.

1.  Azure PowerShell zu öffnen.
2.  Führen Sie das Skript zum Installieren der Diagnose auf Ihre Arbeitskollegen Rolle (mit dem Speicher kontoschlüssel für Ihr Wadexample Speicherkonto ersetzen *StorageAccountKey* ):

```
    $storage_name = "wadexample"
    $key = "<StorageAccountKey>"
    $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
    $service_name="wadexample"
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Schritt 6: Prüfen Sie Ihre Daten werden
Navigieren Sie in der Visual Studio- **Server-Explorer** mit dem Wadexample Speicher-Konto. Nach der Cloud wurde Dienst ausgeführt etwa 5 Minuten Sie Tabellen **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** und **WADSetOtherTable**erhalten sollen. Doppelklicken Sie auf eine der Tabellen im telemetrieprotokoll anzeigen möchten, die gesammelt wurden.
    ![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)


## <a name="configuration-file-schema"></a>Konfiguration File Schema

Die Diagnose Konfigurationsdatei definiert Werte, die Einstellungen diagnostic Konfiguration Initialisierung beim Start des Diagnose-Agents verwendet werden. Finden Sie die [neuesten Schema verweisen](https://msdn.microsoft.com/library/azure/mt634524.aspx) gültige Werte und Beispiele für.

## <a name="troubleshooting"></a>Behandlung von Problemen

Wenn Sie Probleme haben, finden Sie unter [Problembehandlung Azure-Diagnose](../azure-diagnostics-troubleshooting.md) Hilfe häufig auftretende Probleme.

## <a name="next-steps"></a>Nächste Schritte
So ändern Sie die Daten, die Sie sammeln, [eine Liste von virtuellen Computern finden Sie unter Verwandte Azure Diagnose Artikel](azure-diagnostics.md#cloud-services) Behandeln von Problemen oder erfahren Sie mehr über Diagnose im Allgemeinen.


[Quelle Klasse]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Kostenlose Testversion]: http://azure.microsoft.com/pricing/free-trial/
[Installieren und Konfigurieren von Azure PowerShell Version 0.8.7 oder höher]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
