<properties
    pageTitle="Problembehandlung bei Azure-Diagnose"
    description="Beheben von Problemen bei der Verwendung von Azure Diagnose in Azure Cloud Services, virtuellen Computern und "
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
    ms.date="10/04/2016"
    ms.author="robb"/>


# <a name="azure-diagnostics-troubleshooting"></a>Problembehandlung bei Azure-Diagnose
Problembehandlung bei der Verwendung von Azure-Diagnose relevante Informationen an. Weitere Informationen zur Azure-Diagnose finden Sie unter [Übersicht über Azure-Diagnose](azure-diagnostics.md#cloud-services).

## <a name="azure-diagnostics-is-not-starting"></a>Azure Diagnose wird nicht gestartet.

Diagnose besteht aus zwei Komponenten: ein Gast Agent-Plug-Ins und überwacht der Agent. Sie können die Protokolldateien **DiagnosticsPluginLauncher.log** und **DiagnosticsPlugin.log** Warum Diagnose kann nicht gestartet werden Informationen zu überprüfen.  
  
In einer Cloud-Dienst befinden sich die Protokolldateien für das Gast Agent-Plug-in: 

``` 
C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.6.3.0\ 
``` 

Ein Azure virtuellen Computern befinden sich Protokolldateien für das Gast Agent-Plug-in: 

``` 
C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.6.3.0\Logs\ 
``` 
 
Die letzte Zeile der Protokolldateien wird den Beendigungscode enthalten.  

``` 
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0 
``` 

Das Plug-in gibt die folgenden Beendigungscodes:

Beenden der Code|Beschreibung
---|---
0|Erfolg.
-1 ab.|Allgemeiner Fehler.
-2 auf.|Laden die Datei Rcf nicht möglich.<p>Dies ist ein interner Fehler, der nur erfolgen soll, wenn das Startprogramm für das von Gast Agent-Plug-Ins manuell, falsch, des virtuellen Computers aufgerufen wird.
-3|Die Diagnose Konfigurationsdatei kann nicht geladen werden.<p><p>Lösung: Dies ist das Ergebnis einer Konfigurationsdatei nicht Schema Überprüfung übergeben. Die Lösung ist eine Konfigurationsdatei, die – Konformität mit dem Schema bereitstellen.
-4|Lokale Ressourcenverzeichnis wird bereits von einer anderen Instanz von Diagnose überwacht der Agent verwendet werden.<p><p>Lösung: Geben Sie einen anderen Wert für **LocalResourceDirectory**.
-6|Das Startprogramm für das von Gast Agent-Plug-in versucht, mit einer ungültigen Befehlszeilenoption Diagnose zu starten.<p><p>Dies ist ein interner Fehler, der nur erfolgen soll, wenn das Startprogramm für das von Gast Agent-Plug-Ins manuell, falsch, des virtuellen Computers aufgerufen wird.
-10|Das Plug-in Diagnose beigetreten sind mit Ausnahmefehler.
-11|Der Gast-Agent konnte den Vorgang verantwortlich für das Starten und überwachen den Agent für die Überwachung erstellen.<p><p>Lösung: Stellen Sie sicher, dass ausreichende Systemressourcen neue Prozesse starten zur Verfügung stehen.<p>
-101|Ungültige Argumente beim Aufrufen der Diagnose-Plug-in.<p><p>Dies ist ein interner Fehler, der nur erfolgen soll, wenn das Startprogramm für das von Gast Agent-Plug-Ins manuell, falsch, des virtuellen Computers aufgerufen wird.
-102|Der Plug-in-Prozess kann nicht selbst Initialisierung.<p><p>Lösung: Stellen Sie sicher, dass ausreichende Systemressourcen neue Prozesse starten zur Verfügung stehen.
-103|Der Plug-in-Prozess kann nicht selbst Initialisierung. Insbesondere ist es das Protokollierungsobjekt kann nicht erstellt werden.<p><p>Lösung: Stellen Sie sicher, dass ausreichende Systemressourcen neue Prozesse starten zur Verfügung stehen.
-104|Laden die bereitgestellte vom Agent Gast Rcf-Datei nicht möglich.<p><p>Dies ist ein interner Fehler, der nur erfolgen soll, wenn das Startprogramm für das von Gast Agent-Plug-Ins manuell, falsch, des virtuellen Computers aufgerufen wird.
-105|Das Plug-in Diagnose kann die Diagnose Konfiguration-Datei nicht öffnen.<p><p>Dies ist ein interner Fehler, der nur erfolgen soll, wenn die Diagnose-Plug-Ins manuell, falsch, des virtuellen Computers aufgerufen wird.
-106|Konfigurationsdatei der Diagnose nicht gelesen werden.<p><p>Lösung: Dies ist das Ergebnis einer Konfigurationsdatei nicht Schema Überprüfung übergeben. Somit ist die Lösung bereitstellen eine Konfigurationsdatei, die – Konformität mit dem Schema aus. Sie können die XML-Daten suchen, die an die Erweiterung Diagnose in den Ordner *%SystemDrive%\WindowsAzure\Config* des virtuellen Computers übermittelt wird. Öffnen Sie die entsprechende XML-Datei, und suchen **Microsoft.Azure.Diagnostics**, und klicken Sie dann für das Feld **XmlCfg** ein. Die Daten sind base64-codierte daher zu [Entschlüsseln es](http://www.bing.com/search?q=base64+decoder) müssen Sie zum Anzeigen der XML-Datei, die von Diagnose geladen wurde.<p>
-107|Der Ressource Directory Durchgang zu überwacht der Agent ist ungültig.<p><p>Dies ist ein interner Fehler, der nur erfolgen soll, wenn der Überwachung Agent manuell, falsch, des virtuellen Computers aufgerufen wird.</p>
-108    |Konvertieren die Diagnose Konfigurationsdatei in die Überwachung Agentenkonfigurationsdatei nicht möglich.<p><p>Dies ist ein interner Fehler, der nur erfolgen soll, wenn das Plug-in Diagnose mit einer ungültigen Konfigurationsdatei manuell aufgerufen wird.
-110|Allgemeine Diagnose Konfiguration zurück.<p><p>Dies ist ein interner Fehler, der nur erfolgen soll, wenn das Plug-in Diagnose mit einer ungültigen Konfigurationsdatei manuell aufgerufen wird.
-111|Kann nicht überwachen den Agent gestartet.<p><p>Lösung: Stellen Sie sicher, dass ausreichend Systemressourcen verfügbar sind.
-112|Allgemeiner Fehler


## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Diagnosedaten sind nicht angemeldet zu Azure-Speicher
Azure Diagnose alle Daten in Azure Storage gespeichert.

Die am häufigsten verwendeten Ereignisdaten fehlen verursacht Kontoinformationen falsch definierten Speicher.

Lösung: Korrigieren Sie Ihre Konfigurationsdatei Diagnose und installieren Sie Diagnose erneut zu.
Wenn das Problem weiterhin besteht, nachdem die Erweiterung Diagnose erneut zu installieren, und klicken Sie dann möglicherweise Sie debuggen müssen weiter Sie nach Durchsicht durch die Überwachung Agentfehlern. Vor dem Ereignisdaten zu Ihrem Speicherkonto hochgeladen werden, wird er in der LocalResourceDirectory gespeichert.

Für die Rolle der Cloud-Dienst ist der LocalResourceDirectory:

    C:\Resources\Directory\<CloudServiceDeploymentID>.<RoleName>.DiagnosticStore\WAD<DiagnosticsMajorandMinorVersion>\Tables

Für virtuelle Computer ist der LocalResourceDirectory:

    C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD<DiagnosticsMajorandMinorVersion>\Tables

Wenn Sie keine Dateien im Ordner LocalResourceDirectory vorhanden sind, ist der Überwachung Agent kann nicht gestartet werden. Dies wird in der Regel durch eine ungültige Konfigurationsdatei eines Ereignisses verursacht, die in der CommandExecution.log gemeldet werden sollen.

Wenn die Überwachung Agent erfolgreich Ereignisdaten sammeln ist sehen Sie .tsf Dateien für jedes Ereignis in Ihrem Konfigurationsdatei definiert. Überwachung-Agent protokolliert seine Fehler in der Datei MaEventTable.tsf. Die Anwendung tabel2csv können Sie zum Prüfen der Inhalt dieser Datei die .tsf-Datei in einer Datei mit durch Kommas getrennten values(.csv) konvertieren:

Klicken Sie auf eine Rolle der Cloud-Dienst:

    %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

*"% Systemlaufwerk"* auf einer Rolle der Cloud-Dienst ist in der Regel D:

Auf einem virtuellen Computer:

    C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

Die oben aufgeführten Befehle generiert der Log-Datei *maeventtable.csv*, das Sie öffnen und prüfen Sie, ob Fehlermeldungen können.    


## <a name="diagnostics-data-tables-not-found"></a>Diagnosedaten Tabellen nicht gefunden
Die Tabellen in Azure-Speicher gedrückt Azure Diagnosedaten werden mit dem Namen mit dem folgenden Code:

        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;

Hier ist ein Beispiel:

        <EtwEventSourceProviderConfiguration provider=”prov1”>
          <Event id=”1” />
          <Event id=”2” eventDestination=”dest1” />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider=”prov2”>
          <DefaultEvents eventDestination=”dest2” />
        </EtwEventSourceProviderConfiguration>

4 Tabellen generiert:

Ereignis|Tabellenname
---|---
Anbieter = "prov1" &lt;Ereignis-Id "1" = /&gt;|WADEvent + MD5("prov1") + "1"
Anbieter = "prov1" &lt;Ereignis-Id = "2" EventDestination = "dest1" /&gt;|WADdest1
Anbieter = "prov1" &lt;DefaultEvents /&gt;|WADDefault+MD5("prov1")
Anbieter = "prov2" &lt;DefaultEvents EventDestination = "dest2" /&gt;|WADdest2
