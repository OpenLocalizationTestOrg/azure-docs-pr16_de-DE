<properties
 pageTitle="Automatisch skalieren HPC Pack Clusterknoten | Microsoft Azure"
 description="Automatisch vergrößern Sie und verkleinern Sie die Anzahl der HPC Pack Cluster berechnen Knoten in Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a>Automatisch vergrößern Sie und verkleinern Sie die HPC Pack Clusterressourcen in Azure gemäß der Arbeitsbelastung cluster




Wenn Sie Azure "Burst" Knoten in Ihrem Cluster HPC Pack bereitstellen oder einen HPC Pack Cluster in Azure-virtuellen Computern zu erstellen, sollten Sie eine Möglichkeit, die automatisch vergrößert oder verkleinert wird die Anzahl der Azure berechnen von Ressourcen wie Knoten oder Kerne entsprechend der aktuellen Arbeitsbelastung im Cluster. So können Sie Ihre Azure Ressourcen effizienter verwenden und ihre Kosten kontrollieren.
Richten Sie hierzu die HPC Pack Clustereigenschaft **AutoGrowShrink**aus. Führen Sie alternativ **AzureAutoGrowShrink.ps1** HPC PowerShell Skript, das mit HPC Pack installiert ist.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Darüber hinaus können aktuell nur automatisch vergrößern und verkleinern HPC Pack Datenverarbeitungsknoten, die einem Windows Server-Betriebssystem ausgeführt werden.

## <a name="set-the-autogrowshrink-cluster-property"></a>Legen Sie die Eigenschaft AutoGrowShrink cluster

### <a name="prerequisites"></a>Erforderliche Komponenten

* **HPC Pack 2012 R2 Update 2 oder höher Cluster** - am Cluster-Knoten werden kann entweder lokal bereitgestellt oder in einer Azure-virtuellen Computer. Finden Sie unter Erste Schritte mit einer lokalen am und Azure "Burst" Knoten [einem Hybriden Cluster mit HPC Pack einrichten](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) . Lesen Sie die [HPC Pack IaaS Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) schnell einen HPC Pack Cluster in Azure-virtuellen Computern bereitstellen.


* **Für einen Cluster mit einem am Knoten in Azure** – Wenn Sie das Bereitstellungsskript HPC Pack IaaS Cluster erstellen, aktivieren Sie die **AutoGrowShrink** Cluster-Eigenschaft, indem Sie die Option AutoGrowShrink in der Clusterkonfigurationsdatei verwenden. Weitere Informationen finden Sie unter der Dokumentation öffentlichem das [Skript herunterladen](https://www.microsoft.com/download/details.aspx?id=44949). 

    Aktivieren Sie alternativ die **AutoGrowShrink** Clustereigenschaft nach der Bereitstellung von Cluster mithilfe von HPC PowerShell-Befehlen im folgenden Abschnitt beschrieben. Um dies vorzubereiten, gehen Sie zuerst folgendermaßen vor:
    1. Konfigurieren Sie ein Zertifikat Azure Management aus, auf dem am Knoten und im Azure-Abonnement. Sie können für eine Test-Bereitstellung verwenden das Standard Microsoft HPC Azure selbst signierte Zertifikat, das HPC Pack auf dem am Knoten installiert und einfach, dass das Zertifikat für Ihr Abonnement Azure hochladen. Optionen und Schritte finden Sie in der [TechNet-Bibliothek Anleitungen](https://technet.microsoft.com/library/gg481759.aspx).
    2. Führen Sie auf dem am Knoten **"regedit" ein** , wechseln Sie zu HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo, und fügen Sie einen neuer Zeichenfolgenwert. Legen Sie den Namen des Werts zu "Fingerabdruck", und Daten in den Fingerabdruck des Zertifikats in Schritt 1 Wert.


### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a>HPC PowerShell Befehle zum Festlegen der Eigenschaft AutoGrowShrink

Folgende sind Beispiele für HPC PowerShell-Befehlen, um **AutoGrowShrink** festzulegen und deren Verhalten mit zusätzlichen Parametern abstimmen. Finden Sie weiter unten in diesem Artikel erhalten Sie die vollständige Liste der Einstellungen unter [AutoGrowShrink Parameter](#AutoGrowShrink-parameters) . 

Führen Sie diese Befehle starten Sie HPC PowerShell auf dem Cluster am Knoten als Administrator.

**So aktivieren Sie die Eigenschaft AutoGrowShrink**

    Set-HpcClusterProperty –EnableGrowShrink 1

**So deaktivieren Sie die Eigenschaft AutoGrowShrink**

    Set-HpcClusterProperty –EnableGrowShrink 0

**So ändern Sie das Intervall vergrößern in Minuten**

    Set-HpcClusterProperty –GrowInterval <interval>

**So ändern Sie das Intervall verkleinern in Minuten**

    Set-HpcClusterProperty –ShrinkInterval <interval>

**Zum Anzeigen der aktuellen Konfigurations der AutoGrowShrink**

    Get-HpcClusterProperty –AutoGrowShrink

### <a name="autogrowshrink-parameters"></a>AutoGrowShrink Parameter

Nachfolgend werden AutoGrowShrink Parameter, die Sie mithilfe des Befehls **Set-HpcClusterProperty** ändern können.

* **EnableGrowShrink** - zu aktivieren oder deaktivieren die Eigenschaft **AutoGrowShrink** wechseln.
* **ParamSweepTasksPerCore** - Anzahl der parametrische Zug Vorgänge wächst eine Core. Standardmäßig wird eine Core pro Vorgang wächst. 
 
    >[AZURE.NOTE] HPC Pack QFE KB3134307 wird **ParamSweepTasksPerCore** **TasksPerResourceUnit**. Es basiert auf den Auftrag Ressourcentyp und Knoten, Sockets oder Core werden kann.
    
* **GrowThreshold** - Schwellenwert verzögerte Vorgänge zum automatischen Vergrößerung auslösen. Die Standardeinstellung ist 1, d. h., die mindestens 1 Vorgänge befinden sich in der Warteschlange automatisch Hierarchieknoten zu vergrößern.
* **GrowInterval** - Intervall in Minuten zum automatischen Vergrößerung auslösen. Das standardmäßige Intervall beträgt 5 Minuten.
* **ShrinkInterval** - Intervall in Minuten automatisch verkleinert ausgelöst. Das standardmäßige Intervall beträgt 5 Minuten. |
* **ShrinkIdleTimes** - Anzahl fortlaufender Prüfungen zu verkleinern, um anzugeben, dass die Knoten im Leerlauf befinden. Die Standardeinstellung ist 3 Mal. Ist der **ShrinkInterval** 5 Minuten, überprüft HPC Pack z. B., ob der Knoten im Leerlauf 5 Minuten ist. Wenn die Knoten im Leerlauf sind nach fortlaufender 3 (15 Minuten) überprüft, verkleinert HPC Pack Knotens.
* **ExtraNodesGrowRatio** - zusätzliche Prozentsatz der Knoten mit bereichern für Nachricht übergeben Interface (MPI) stellen. Der Standardwert ist 1, d. h., HPC Pack Knoten 1 % für MPI Aufträge vergrößert wird. 
* **GrowByMin** - wechseln, um anzugeben, ob die Richtlinie automatische Vergrößerung auf den minimalen Ressourcen für den Job erforderlich basiert. Die Standardeinstellung ist falsch, was bedeutet, dass HPC Pack für Aufträge basierend auf der maximalen Ressourcen für die Einzelvorgänge erforderlichen Knoten vergrößert wird.
* **SoaJobGrowThreshold** - Schwellenwert von eingehenden SOA Anfragen zum Auslösen des Automatisches wächst Prozess. Der Standardwert ist 50000.  
    
    >[AZURE.NOTE] Für diesen Parameter ist starten in HPC Pack 2012 R2 Update 3 unterstützt.
    
* **SoaRequestsPerCore** -Anzahl der eingehenden SOA mit einem Core bereichern anfordert. Der Standardwert beläuft sich 20.000 Seiten.  

    >[AZURE.NOTE] Für diesen Parameter ist starten in HPC Pack 2012 R2 Update 3 unterstützt.

### <a name="mpi-example"></a>Beispiel für MPI

Standardmäßig wächst HPC Pack 1 % zusätzlichen Knoten für MPI-Projekte (**ExtraNodesGrowRatio** auf 1 festgelegt ist). Der Grund ist, dass MPI möglicherweise mehrere Knoten erfordern und der Auftrag nur ausgeführt werden, kann wenn alle Knoten bereit sind. Beim Starten von Azure Knoten gelegentlich möglicherweise einen Knoten als andere, die bewirken, dass andere Knoten im Leerlauf sein, während Sie warten auf diesem Knoten zum Vorbereiten starten mehr Zeit benötigen. Wachsende zusätzliche Knoten, HPC Pack reduziert diese Ressource Wartezeit und potenziell speichert Kosten. Führen Sie zum Erhöhen des Prozentsatzes von zusätzlichen Knoten für MPI Einzelvorgänge (beispielsweise um 10 %) Befehl aus, ähnlich wie

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>Beispiel für SOA

Standardmäßig ist **SoaJobGrowThreshold** auf 50000 und **SoaRequestsPerCore** 200000 festgelegt ist. Wenn Sie eine SOA Auftrag mit 70000 Anforderungen senden, eine Aufgabe in der Warteschlange werden und eingehende Anfragen sind 70000. In diesem Fall wächst HPC Pack 1 Core für die Aufgabe in der Warteschlange und für eingehenden Anfragen wächst (70000-50000) / 20000 = 1 core, sodass für insgesamt 2 Kerne für dieses Projekt SOA vergrößert wird.

## <a name="run-the-azureautogrowshrinkps1-script"></a>Führen Sie das Skript AzureAutoGrowShrink.ps1

### <a name="prerequisites"></a>Erforderliche Komponenten

* **HPC Pack 2012 R2 Update 1 oder höher Cluster** - das Skript **AzureAutoGrowShrink.ps1** ist im Ordner Bin CCP_HOME % installiert. Am Cluster-Knoten werden kann entweder lokal bereitgestellt oder in einer Azure-virtuellen Computer. Finden Sie unter Erste Schritte mit einer lokalen am und Azure "Burst" Knoten [einem Hybriden Cluster mit HPC Pack einrichten](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) . Finden Sie unter dem [HPC Pack IaaS Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) schnell einen HPC Pack Cluster in Azure-virtuellen Computern bereitstellen, oder verwenden Sie einer [Vorlage Azure Schnellstart](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).

* **Azure PowerShell 0.8.12** - derzeit das Skript hängt dieser spezifischen Version von Azure PowerShell aus. Wenn Sie eine neuere Version auf dem am Knoten ausgeführt werden, müssen Sie möglicherweise Azure PowerShell auf die [Version 0.8.12](http://az412849.vo.msecnd.net/downloads03/azure-powershell.0.8.12.msi) zum Ausführen des Skripts heruntergestuft. 

* **Für einen Cluster mit Knoten Azure Burst** - führen Sie das Skript auf einem Clientcomputer dem HPC Pack installiert ist, oder klicken Sie auf den am Knoten. Wenn auf einem Clientcomputer ausgeführt wird, stellen Sie sicher, dass Sie die Variable $env: CCP_SCHEDULER ordnungsgemäß zu zeigen Sie auf den am Knoten. Die Knoten Azure "Burst" müssen bereits mit dem Cluster hinzugefügt werden, aber sie können in den Zustand nicht bereitgestellt werden.


* **Für einen Cluster bereitgestellt in Azure-virtuellen Computern** - führen Sie das Skript auf dem am Knoten virtueller Computer, da abhängig von der **Start-HpcIaaSNode.ps1** und **Beenden-HpcIaaSNode.ps1** Skripts, installiert sind vorhanden. Diese Skripts darüber hinaus erfordern ein Zertifikat Azure Management oder veröffentlichen Einstellungsdatei (siehe [Verwalten berechnen Knoten in einem Cluster HPC Pack in Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)). Stellen Sie sicher, dass alle der Berechnungsknoten virtuellen Computern Sie müssen bereits mit dem Cluster hinzugefügt werden. Sie können den Zustand beendet werden.

### <a name="syntax"></a>Syntax

```
AzureAutoGrowShrink.ps1
[[-NodeTemplates] <String[]>] [[-JobTemplates] <String[]>] [[-NodeType] <String>]
[[-NumOfQueuedJobsPerNodeToGrow] <Int32>]
[[-NumOfQueuedJobsToGrowThreshold] <Int32>]
[[-NumOfActiveQueuedTasksPerNodeToGrow] <Int32>]
[[-NumOfActiveQueuedTasksToGrowThreshold] <Int32>]
[[-NumOfInitialNodesToGrow] <Int32>] [[-GrowCheckIntervalMins] <Int32>]
[[-ShrinkCheckIntervalMins] <Int32>] [[-ShrinkCheckIdleTimes] <Int32>]
[-UseLastConfigurations] [[-ArgFile] <String>] [[-LogFilePrefix] <String>]
[<CommonParameters>]

```
### <a name="parameters"></a>Parameter

 * **NodeTemplates** - Namen der Knoten Vorlagen zum Definieren des Bereichs für die Knoten vergrößern und verkleinern. Wenn nicht angegeben (der Standardwert ist @()), alle Knoten in der Gruppe der **AzureNodes** Knoten befinden sich im Bereich, wenn **NodeType** hat den Wert AzureNodes und alle Knoten in der Gruppe der **ComputeNodes** Knoten befinden sich im Bereich, wenn **NodeType** Wert ComputeNodes aufweist.

 * **JobTemplates** - Namen der Position Vorlagen, um den Bereich für die Knoten mit bereichern definieren.

 * **NodeType** - Knoten vergrößern und verkleinern. Unterstützte Werte sind:

     * **AzureNodes** – für Azure PaaS (Burst) Knoten in einer lokalen oder Azure IaaS Cluster.

     * **ComputeNodes** – für berechnen Knoten virtuellen Computern nur in einem Cluster Azure IaaS.

* **NumOfQueuedJobsPerNodeToGrow** - Anzahl der in der Warteschlange Aufträge mit einem Knoten bereichern erforderlich.

* **NumOfQueuedJobsToGrowThreshold** - die Mindestanzahl der Aufträge zum Starten des Prozesses vergrößern.

* **NumOfActiveQueuedTasksPerNodeToGrow** - die Anzahl der aktiven in der Warteschlange Aufgaben, die mit einem Knoten bereichern erforderlich. Wenn **NumOfQueuedJobsPerNodeToGrow** mit einem Wert größer als 0 angegeben ist, wird dieser Parameter ignoriert.

* **NumOfActiveQueuedTasksToGrowThreshold** - die Mindestanzahl der aktiven in der Warteschlange Aufgaben zum Starten des Prozesses vergrößern.

* **NumOfInitialNodesToGrow** - die ursprüngliche Mindestanzahl der Knoten vergrößern alle Knoten im Bereich **weiterspielen (Deallocated)**oder **Nicht-bereitgestellt** werden kann.

* **GrowCheckIntervalMins** – das Intervall in Minuten zwischen Prüfungen zu vergrößern.

* **ShrinkCheckIntervalMins** – das Intervall in Minuten zwischen Prüfungen zu verkleinern.

* **ShrinkCheckIdleTimes** - die Anzahl der fortlaufender verkleinern Prüfungen (durch **ShrinkCheckIntervalMins**getrennt), um anzugeben, dass die Knoten im Leerlauf befinden.

* **UseLastConfigurations** - den vorherigen Konfigurationen in der Argumentdatei gespeichert.

* **ArgFile**– den Namen der Argumentdatei ein verwendet, um speichern und aktualisieren die Konfigurationen, um das Skript auszuführen.

* **LogFilePrefix** – das Präfixname der Protokolldatei. Sie können einen Pfad angeben. Standardmäßig wird das Protokoll in das aktuelle Verzeichnis geschrieben.

### <a name="example-1"></a>Beispiel 1

Im folgende Beispiel wird die Azure Burst-Knoten bereitgestellt, mit der Standardvorlage AzureNode vergrößern und verkleinern automatisch konfiguriert. Wenn alle Knoten zunächst in den Zustand **Nicht bereitgestellt** werden, werden Sie mindestens 3 Knoten gestartet. Wenn die Anzahl der Aufträge in 8 überschreitet, startet das Skript Knoten bis deren das Verhältnis zwischen der Aufträge zu **NumOfQueuedJobsPerNodeToGrow**überschreitet. Wenn ein Knoten im Leerlauf in 3 im Leerlauf hintereinander ist gefunden wird, wird sie abgebrochen.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Beispiel 2

Im folgende Beispiel wird den Azure Berechnungsknoten virtuellen Computern befinden, die mit der Standardvorlage ComputeNode vergrößern und verkleinern automatisch konfiguriert.
Die Projekte, die so konfiguriert ist, indem Sie die Standardvorlage für den Job definieren des Gültigkeitsbereichs von die Arbeitsbelastung im Cluster an. Wenn alle Knoten Anfangs angehalten werden, werden Sie mindestens 5 Knoten gestartet. Wenn die Anzahl der aktiven verzögerte Vorgänge 15 überschreitet, startet das Skript Knoten bis deren das Verhältnis zwischen der aktiven Aufgaben in der Warteschlange **NumOfActiveQueuedTasksPerNodeToGrow**überschreitet. Wenn ein Knoten im Leerlauf in 10 im Leerlauf hintereinander ist gefunden wird, wird sie abgebrochen.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
