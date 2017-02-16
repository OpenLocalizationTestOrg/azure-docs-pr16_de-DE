<properties
   pageTitle="Migrieren von Orchestrator zu Azure Automatisierung | Microsoft Azure"
   description="Beschreibt, wie System Center Orchestrator Runbooks und Integration Packs in Azure Automatisierung migrieren."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />


# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Migrieren von Orchestrator zu Azure Automatisierung (Beta)

Runbooks in [System Center Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) basieren auf Aktivitäten von Integration Packs, die speziell für Orchestrator geschrieben werden, während Runbooks in Azure Automatisierung auf Windows PowerShell basieren.  [Grafisch Runbooks](automation-runbook-types.md#graphical-runbooks) in Azure Automatisierung haben ein ähnliches Erscheinungsbild zu Orchestrator Runbooks, mit deren Aktivitäten, die PowerShell-Cmdlets, untergeordnete Runbooks und Posten darstellt.

Das [System Center Orchestrator Migration Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) enthält Tools, um Sie beim Konvertieren von Runbooks von Orchestrator in Azure Automatisierung unterstützen.  Außer zum Konvertieren von der Runbooks selbst, müssen Sie die Integration Packs mit den Aktivitäten, die die Runbooks verwenden in Integrationsmodule mit Windows PowerShell-Cmdlets konvertieren.  

Im folgenden sehen die Grundlagen zum Konvertieren von Orchestrator Runbooks in Azure Automatisierung.  Jeder dieser Schritte wird in den folgenden Abschnitten ausführlich beschrieben.

1.  Laden Sie das [System Center Orchestrator Migration Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) enthält die Tools und Module in diesem Artikel beschrieben.
2.  Importieren von [Standardaktivitäten Modul](#standard-activities-module) in Azure Automatisierung.  Dies umfasst konvertierte Versionen Orchestrator Standardaktivitäten, die von konvertierte Runbooks verwendet werden können.
3.  Importieren von [System Center Orchestrator Integrationsmodule](#system-center-orchestrator-integration-modules) in Azure Automatisierung für diese Integration Packs untersuchten Ihrer Runbooks, die System Center zugreifen.
4.  Benutzerdefinierte und Drittanbieter Integration Packs in Verbindung mit dem [Integration Pack Konverter](#integration-pack-converter) konvertieren und Importieren in Azure Automatisierung.
5.  Orchestrator Runbooks unter Verwendung des [Runbooks Konverter](#runbook-converter) konvertieren und in Azure Automatisierung installieren.
6.  Manuell erstellen Sie erforderliche Orchestrator Posten Azure Automatisierung, da diese Ressourcen nicht in der Runbooks Konverter konvertiert wird.
7.  Konfigurieren einer [Hybrid Runbooks Worker](#hybrid-runbook-worker) in Ihrem lokalen Datencenter konvertierte Runbooks ausführen, die lokale Ressourcen zugreifen können.

## <a name="service-management-automation"></a>Service Management Automatisierung

[Service Management Automatisierung](http://technet.microsoft.com/library/dn469260.aspx) (SMA) speichert Runbooks in Ihrem lokalen Data Center wie Orchestrator und führt die gleichen Integrationsmodule wird als Azure Automatisierung verwendet. Der [Runbooks Konverter](#runbook-converter) konvertiert Orchestrator Runbooks in grafisch Runbooks durch die SMA nicht unterstützt werden.  Sie können weiterhin die [Standardmodul Aktivitäten](#standard-activities-module) und [System Center Orchestrator Integrationsmodule](#system-center-orchestrator-integration-modules) in SMA installieren, aber Sie müssen manuell [Schreiben Sie Ihre Runbooks](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Hybrid Runbooks Worker

Runbooks in Orchestrator auf einem Datenbankserver gespeichert und Runbooks-Servern, sowohl in Ihrem lokalen Data Center ausgeführt.  Runbooks in Azure Automatisierung in der Cloud Azure gespeichert werden und in Ihrem lokalen Datencenter über einen [Hybrid Runbooks Worker](automation-hybrid-runbook-worker.md)ausgeführt werden kann.  Dies ist, wie Sie normalerweise ausgeführt werden Runbooks aus Orchestrator konvertiert werden, da sie auf den lokalen Servern ausführen ausgelegt sind.

## <a name="integration-pack-converter"></a>Integration Pack Konverter

Die Integration Pack Konverter konvertiert Integration Packs, die erstellt wurden, verwenden die [Orchestrator Integration Toolkit (OIT)](http://technet.microsoft.com/library/hh855853.aspx) zu Integrationsmodule ausgehend von Windows PowerShell, die in Azure Automatisierung oder Dienst Management Automatisierung importiert werden kann.  

Beim Ausführen der Integration Pack Konverter sind Sie ein Assistent angezeigt, mit der Sie eine Datei für die Integration Pack (.oip) auswählen können.  Der Assistent dann Listen die in dieser Integration Pack enthaltenen Aktivitäten und können Sie auswählen, welche migriert werden soll.  Wenn Sie den Assistenten abgeschlossen haben, wird eine Integrationsmodul, das ein entsprechendes Cmdlet für jede der Aktivitäten in das ursprüngliche Integration Pack enthält erstellt.


### <a name="parameters"></a>Parameter

Alle Eigenschaften einer Aktivität im Integration Pack werden in Parameter des entsprechenden Cmdlets im Integrationsmodul konvertiert.  Windows PowerShell-Cmdlets verfügen über eine Reihe von [Allgemeine Parameter](http://technet.microsoft.com/library/hh847884.aspx) , die mit allen Cmdlets verwendet werden kann.  Beispielsweise den ausführlichen Parameter bewirkt, dass ein Cmdlet ausführliche Informationen zu seiner Ausführung ausgegeben.  Keine Cmdlet möglicherweise einen Parameter mit demselben Namen wie eine allgemeine Parameter.  Wenn eine Aktivität eine Eigenschaft mit demselben Namen wie eine allgemeine Parameter verfügt, fordert der Assistent Sie einen anderen Namen für den Parameter bereitstellen.

### <a name="monitor-activities"></a>Überwachen von Aktivitäten

Monitor Runbooks in Orchestrator mit einem [Monitor Aktivität](http://technet.microsoft.com/library/hh403827.aspx) starten und Ausführen durch ein bestimmtes Ereignis aufgerufen werden kontinuierlich warten.  Azure Automatisierung unterstützt keine Monitor Runbooks, damit alle Monitor Aktivitäten im Integration Pack nicht konvertiert werden.  Stattdessen wird ein Platzhalter Cmdlet im Integrationsmodul für die Aktivität Monitor erstellt.  Dieses Cmdlet hat keine Funktion, aber dies zulässt, dass alle konvertierte Runbooks, mit deren Hilfe installiert werden soll.  Diese Runbooks werden nicht in Azure Automatisierung ausführen, jedoch können installiert werden, damit Sie es ändern können.

### <a name="integration-packs-that-cannot-be-converted"></a>Integration Packs, die konvertiert werden kann

Integration Packs, die nicht mit OIT erstellt wurden, können nicht mit der Integration Pack Konverter konvertiert werden. Es gibt auch einige Integration Packs dieses Tool Lieferumfang von Microsoft, die aktuell konvertiert werden kann.  Konvertierte Versionen dieser Integration Packs wurden [zum Download bereitgestellt](#system-center-orchestrator-integration-modules) , damit sie in Azure Automatisierung oder Dienst Management Automatisierung installiert werden können.


## <a name="standard-activities-module"></a>Standardaktivitäten-Modul

Orchestrator enthält eine Reihe von [Standardaktivitäten](http://technet.microsoft.com/library/hh403832.aspx) , die fallen nicht unter eines Sprachpakets Integration von vielen Runbooks verwendet werden.  Das Standardaktivitäten Modul ist eine Integrationsmodul, das ein gleichwertiger Cmdlet für jede dieser Aktivitäten enthält.  Sie müssen dieses Integrationsmodul in Azure Automatisierung installieren, vor dem Importieren alle konvertierte Runbooks, die eine standard-Aktivität verwenden.

Zusätzlich zur Unterstützung von konvertierte Runbooks, kann die Cmdlets im Modul Standardaktivitäten von einer anderen Person mit Orchestrator vertraut erstellen neue Runbooks in Azure Automatisierung verwendet werden.  Während die Funktionalität der alle der Standardaktivitäten mit Cmdlets ausgeführt werden kann, können er anders ausgeführt werden.  Die Cmdlets im Modul konvertierte Standardaktivitäten wird funktionieren genauso wie die entsprechenden Aktivitäten und die gleichen Parameter verwenden.  Damit können Sie den vorhandenen Orchestrator Runbooks Autor in deren Übergang zu Azure Automatisierung Runbooks.

## <a name="system-center-orchestrator-integration-modules"></a>System Center Orchestrator Integrationsmodule

Microsoft bietet [Integration Packs](http://technet.microsoft.com/library/hh295851.aspx) für Runbooks zum Automatisieren von System Center Komponenten und andere Produkte erstellen.  Einige dieser Integration Packs aktuell Grundlage OIT aber nicht aktuell in Integrationsmodule aufgrund bekannte Probleme konvertiert werden.  [System Center Orchestrator Integrationsmodule](https://www.microsoft.com/download/details.aspx?id=49555) enthält konvertierte Versionen dieser Integration Packs, die in Azure-Automatisierung und Service Management Automatisierung importiert werden kann.  

Durch die RTM-Version dieses Tools werden aktualisierte Versionen der Integration Packs basierend auf OIT konvertiert werden kann, die mit der Integration Pack Konverter veröffentlicht werden.  Leitfaden wird auch zur Unterstützung bei der Konvertierung Runbooks mit Aktivitäten aus der Integration Packs nicht basierend auf OIT bereitgestellt werden.

## <a name="runbook-converter"></a>Runbooks Konverter

Der Konverter Runbooks konvertiert Orchestrator Runbooks in [grafisch Runbooks](automation-runbook-types.md#graph-runbooks) , die in Azure Automatisierung importiert werden kann.  

Runbooks Konverter wird als PowerShell-Modul implementiert, mit einem Cmdlet aufgerufen **ConvertFrom-SCORunbook** , der die Konvertierung ausführt.  Wenn Sie das Tool installieren, wird eine Verknüpfung zu einem PowerShell-Sitzung erstellt, die das Cmdlet zu laden.   

Im folgenden sehen die Grundlagen zum Konvertieren einer Orchestrator Runbooks und in Azure Automatisierung importieren.  Die folgenden Abschnitte enthalten weitere Details auf mithilfe des Tools und Arbeiten mit konvertierte Runbooks.

1. Exportieren Sie ein oder mehrere Runbooks Orchestrator.
2. Integrationsmodule für alle Aktivitäten in des Runbooks zu erhalten.
3. Konvertieren Sie die Orchestrator Runbooks in die exportierte Datei ein.
4. Überprüfen Sie die Informationen in Protokolle, um die Konvertierung zu überprüfen und alle erforderlichen manuellen Aufgaben zu bestimmen.
4. Importieren Sie konvertierte Runbooks in Azure Automatisierung.
5. Erstellen Sie alle erforderlichen Anlagen in Azure Automatisierung an.
6. Bearbeiten des Runbooks in Azure Automatisierung, alle erforderlichen Aktivitäten zu ändern.

### <a name="using-runbook-converter"></a>Verwenden von Runbooks Konverter

Die Syntax für **ConvertFrom-SCORunbook** lautet wie folgt aus:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string> 

- RunbookPath - Pfad zur Datei exportieren, enthält die Runbooks zu konvertieren.
- Modul - Trennzeichen getrennte Komma Integrationsmodule, die Aktivitäten in der Runbooks enthält.
- OutputFolder - konvertiert, grafisch Runbooks Pfad zu dem Ordner zu erstellen. 


Befehl im folgenden Beispiel konvertiert die Runbooks in eine Exportdatei **MyRunbooks.ois_export**bezeichnet.  Diese Runbooks verwenden Sie die Active Directory und Data Protection Manager Integration Packs.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks" 


### <a name="log-files"></a>Protokolldateien

Der Konverter Runbooks werden die folgenden Protokolldateien in am selben Speicherort wie die konvertierte Runbooks erstellen.  Wenn die Dateien bereits vorhanden sind, werden sie mit Informationen aus der letzten Konvertierung überschrieben.

| Datei | Inhalt |
|:---|:---|
| Runbooks Konverter - Progress.log | Ausführliche vor der Konvertierung, einschließlich Informationen für jede Aktivität erfolgreich konvertiert und Warnung für jede Aktivität nicht konvertiert. |
| Runbooks Konverter - Summary.log  | Zusammenfassung der letzten Konvertierung einschließlich alle Warnungen und Nachverfolgen von Aufgaben, die Sie ausführen, z. B. eine Variable für die konvertierte Runbooks erforderlichen erstellen müssen.  |

### <a name="exporting-runbooks-from-orchestrator"></a>Exportieren von Runbooks aus Orchestrator

Der Konverter Runbooks funktioniert mit einer Datei exportieren aus Orchestrator, die eine oder mehrere Runbooks enthält.  Es wird eine entsprechende Azure Automatisierung Runbooks für jede Orchestrator Runbooks in die Exportdatei erstellt.  

Um eine Runbooks aus Orchestrator exportieren, mit der rechten Maustaste in des Namens des Runbooks Runbooks-Designer, und dann auf **Exportieren**.  Um alle Runbooks in einem Ordner zu exportieren, mit der rechten Maustaste in des Namens des Ordners und dann auf **Exportieren**.


### <a name="runbook-activities"></a>Runbooks Aktivitäten

Der Konverter Runbooks wandelt jede Aktivität im des Runbooks Orchestrator an eine entsprechende Aktivität im Azure Automatisierung an.  Für die Aktivitäten, die konvertiert werden können, wird eine Aktivität Platzhalter des Runbooks mit Warnungstext erstellt.  Nachdem Sie die konvertierte Runbooks in Azure Automatisierung importiert haben, müssen Sie eine der folgenden Aktivitäten mit gültigen Aktivitäten ersetzen, die die benötigten Funktionen ausführen.

Alle Orchestrator Aktivitäten im [Standardmodul Aktivitäten](#standard-activities-module) werden konvertiert.  Es gibt einige Orchestrator Standardaktivitäten, die nicht in diesem Modul durch und werden nicht konvertiert.  Beispielsweise verfügt über **Senden Plattform Ereignis** keine Azure Automatisierung entspricht, da das Ereignis für Orchestrator spezifisch sind.

[Monitor Aktivitäten](https://technet.microsoft.com/library/hh403827.aspx) werden nicht konvertiert, da es gibt keine Entsprechung zu in Azure Automatisierung.  Die Ausnahme sind Überwachen der Aktivitäten in [Integration Packs konvertiert](#integration-pack-converter) , die auf den Platzhalter Aktivität konvertiert werden.

Alle Aktivitäten einer [Integration Pack konvertiert](#integration-pack-converter) werden konvertiert, wenn Sie den Pfad für das Integrationsmodul mit dem Parameter **Module** bereitstellen.  System Center Integration Packs können Sie die [System Center Orchestrator Integrationsmodule](#system-center-orchestrator-integration-modules).


### <a name="orchestrator-resources"></a>Orchestrator Ressourcen

Der Konverter Runbooks konvertiert nur Runbooks, nicht für andere Orchestrator Ressourcen wie Indikatoren, Variablen oder Verbindungen.  Indikatoren werden nicht in Azure-Automatisierung unterstützt.  Variablen und Verbindungen werden unterstützt, aber Sie müssen sie manuell erstellen.  Die Protokolldateien werden informieren Sie, ob des Runbooks solche Ressourcen benötigt werden, und geben Sie entsprechende Ressourcen, die Sie benötigen, um in Azure Automatisierung für die konvertierte Runbooks ordnungsgemäß zu erstellen.

Beispielsweise möglicherweise eine Runbooks eine Variable verwenden, um einen bestimmten Wert in einer Aktivität zu füllen.  Die konvertierte Runbooks rechnet die Aktivität und eine Variable Anlage in Azure Automatisierung mit demselben Namen wie die Variable Orchestrator angeben.  Dies wird in der Datei **Runbooks Konverter - Summary.log** gekennzeichnet, die nach der Konvertierung erstellt wird.  Sie benötigen diese Variable Anlage in Azure Automatisierung manuell zu erstellen, bevor Sie mithilfe des Runbooks. 

 
### <a name="input-parameters"></a>Eingabeparameter

Runbooks in Orchestrator akzeptieren Eingabeparameter bei der **Initialisierung Daten** Aktivität.  Wenn der zu konvertierenden Runbooks diese Aktivität enthält, wird ein [Eingabeparameter](automation-graphical-authoring-intro.md#runbook-input-and-output) in der Azure Automatisierung Runbooks für jeden Parameter in die Aktivität erstellt.  Eine [Workflow-Skript Steuerelement](automation-graphical-authoring-intro.md#activities) Aktivität wird in die konvertierte Runbooks erstellt, die abgerufen und gibt jeden Parameter.  Alle Aktivitäten in des Runbooks, die Eingabeparameter verwenden, schlagen Sie in der Ausgabe aus dieser Aktivität.

Der Grund, dass diese Strategie verwendet wird besteht darin, die Funktionalität in des Runbooks Orchestrator am besten spiegeln.  Aktivitäten in den neuen grafischen Runbooks sollte direkt, um mit einer Datenquelle Runbooks Eingabeparameter verweisen.


### <a name="invoke-runbook-activity"></a>Aufrufen des Runbooks Aktivität

Runbooks in Orchestrator Starten anderer Runbooks mit der Aktivität **Runbooks aufzurufen** . Wenn der zu konvertierenden Runbooks diese Aktivität enthält, und die Option **Warten auf den Abschluss** festgelegt ist, wird eine Aktivität Runbooks dafür in die konvertierte Runbooks erstellt.  Wenn die Option **Warten auf den Abschluss** nicht festgelegt ist, wird Klicken Sie dann eine Workflowskript Aktivität erstellt, die **Start-AzureAutomationRunbook** , die zum Starten des Runbooks verwendet.  Nachdem Sie die konvertierte Runbooks in Azure Automatisierung importiert haben, müssen Sie diese Aktivität mit den Informationen in der Aktivität angegebenen ändern.




## <a name="related-articles"></a>Verwandte Artikel

- [System Center 2012 - Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
- [Service Management Automatisierung](https://technet.microsoft.com/library/dn469260.aspx)
- [Hybrid Runbooks Worker](automation-hybrid-runbook-worker.md)
- [Orchestrator Standardaktivitäten](http://technet.microsoft.com/library/hh403832.aspx)
- [Download System Center Orchestrator Migration Toolkit](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
 
