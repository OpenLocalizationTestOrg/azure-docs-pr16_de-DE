<properties
 pageTitle="HPC Pack Cluster für Excel und SOA | Microsoft Azure"
 description="Erste Schritte umfangreiche Excel und SOA Auslastung für eine HPC Pack Cluster in Azure ausgeführt"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/25/2016"
 ms.author="danlep"/>

# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Erste Schritte ausführen von Excel und SOA Auslastung auf einem HPC Pack Cluster in Azure

In diesem Artikel wird gezeigt, wie einen Microsoft HPC Pack Cluster auf Azure-virtuellen Computern mithilfe einer Vorlage Azure Schnellstart oder optional ein Bereitstellungsskript Azure PowerShell bereitgestellt werden kann. Der Cluster verwendet Azure Marketplace virtueller Computer Bilder Microsoft Excel- oder Service-orientierte Architektur (SOA) Auslastung mit HPC Pack ausgeführt werden sollen. Cluster können Sie einfache Excel HPC und SOA-Dienste aus einer lokalen Clientcomputer ausführen. Excel HPC Dienste gehören Excel Arbeitsmappe Verschiebung und benutzerdefinierte Funktionen von Excel oder zu UDFs.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Auf hoher Ebene zeigt das folgende Diagramm HPC Pack Cluster Sie erstellen.

![HPC Cluster mit Knoten Excel Auslastung ausgeführt][scenario]

## <a name="prerequisites"></a>Erforderliche Komponenten

*   **Clientcomputer** - benötigten auf einem Windows-basierten Clientcomputer Stichprobe Excel und SOA Aufträge zum Cluster senden. Sie benötigen außerdem einen Windows-Computer das Azure PowerShell Cluster Bereitstellungsskript ausführen (Wenn Sie diese Methode der Bereitstellung auswählen) und

*   **Azure-Abonnement** – Wenn Sie ein Azure-Abonnement besitzen, können Sie ein [Konto frei](https://azure.microsoft.com/pricing/free-trial/) in nur ein paar Minuten erstellen.

*   **Kerne Kontingent** - möglicherweise müssen Sie das Kontingent der Kerne, erhöhen insbesondere dann, wenn Sie mehrere Clusterknoten mit Multikernprozessoren virtueller Computer Maßen bereitstellen. Wenn Sie eine Vorlage Azure Schnellstart verwenden, ist das Kontingent Kerne in Ressourcenmanager pro Azure Region. In diesem Fall müssen Sie das Kontingent in einem bestimmten Bereich zu vergrößern. Finden Sie unter [Grenzwerte Azure-Abonnement, Kontingente, und Einschränkungen](../azure-subscription-service-limits.md). Um ein Kontingent, [Öffnen Sie eine Supportanfrage online Kunden](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) kostenlos zu erhöhen.

*   **Microsoft Office-Lizenz** – Wenn Sie bereitstellen berechnen, verwenden ein Bild Marketplace HPC Pack virtueller Computer mit Microsoft Excel-Knoten eine 30-Tage-Testversion von Microsoft Excel Professional Plus 2013 installiert ist. Nach dem Punkt Auswertung müssen Sie eine gültige Lizenz für Microsoft Office Excel weiterhin ausgeführt, Auslastung Onlinediensten bereitstellen. Weiter unten in diesem Artikel finden Sie in der [Excel-Aktivierung](#excel-activation) . 


## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Schritt 1. Richten Sie eine HPC Pack Cluster in Azure

Wir zwei Optionen zum Einrichten des Clusters anzeigen: zunächst mithilfe einer Vorlage Azure Schnellstart und Azure-Portal; und die Sekunde, verwenden ein Bereitstellungsskript Azure PowerShell.


### <a name="option-1-use-a-quickstart-template"></a>Option 1. Verwenden einer Vorlage Schnellstart
Verwenden einer Vorlage Azure Schnellstart schnell und einfach bereitstellen einen HPC Pack Cluster im Azure-Portal an. Wenn Sie die Vorlage im Portal öffnen, erhalten Sie eine einfache Benutzeroberfläche, wo Sie die Einstellungen für Ihren Cluster ein. Hier sind die Schritte aus. 

>[AZURE.TIP]Wenn Sie möchten, verwenden Sie eine [Azure Marketplace-Vorlage](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) , die einen ähnlichen Cluster speziell für Excel-Auslastung erstellt wird. Die Schritte unterscheiden sich geringfügig von der folgenden.

1.  Finden Sie auf der [Vorlagenseite erstellen HPC Cluster GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Wenn Sie möchten, lesen Sie Informationen zu der Vorlage und des Quellcodes aus.

2.  Klicken Sie auf **Bereitstellen in Azure** , um eine Bereitstellung mit der Vorlage im Azure-Portal zu starten.

    ![Bereitstellen von Vorlagen für Azure][github]

3.  Im Portal folgendermaßen Sie vor, um die Parameter für die HPC Cluster Vorlage eingeben.

    ein. Klicken Sie auf der Seite **Parameter** Geben Sie ein, oder ändern Sie die Werte für die Vorlagenparameter. (Klicken Sie auf das Symbol neben jeder Einstellung für Hilfeinformationen.) Beispielwerte sind in der folgenden Abbildung gezeigt. In diesem Beispiel wird einen Cluster mit dem Namen *hpc01* in der *hpc.local* -Domäne, die aus einem Knoten am erstellt und 2 Knoten zu berechnen. Die Datenverarbeitungsknoten werden von einem Bild HPC Pack virtueller Computer erstellt, die Microsoft Excel enthält.

    ![Geben Sie Parameter][parameters]

    >[AZURE.NOTE]Die am Knoten virtueller Computer wird automatisch aus dem [neuesten Marketplace-Bild](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) von HPC Pack 2012 R2 unter Windows Server 2012 R2 erstellt. Aktuell basiert das Bild auf HPC Pack 2012 R2 Update 3.
    >
    >Berechnen Sie, dass Knoten virtuellen Computern aus dem letzten Bild der ausgewählten berechnen Knoten Familie erstellt werden. Wählen Sie die Option **ComputeNodeWithExcel** für das neueste HPC Pack berechnen Knotenbild, das eine Testversion von Microsoft Excel Professional Plus 2013 enthält. Um einen Cluster für allgemeine SOA Sitzungen oder Excel UDFs Verschiebung bereitstellen, wählen Sie die Option **ComputeNode** (ohne Excel installiert haben) aus.

    b. Wählen Sie das Abonnement aus.

    c. Erstellen Sie eine Ressourcengruppe für den Cluster, wie z. B. *hpc01RG*ein.

    d. Wählen Sie einen Speicherort für die Ressourcengruppe, z. B. zentralen US ein.

    e. Überprüfen Sie auf der Seite **Vertragsbedingungen** der Ausdrücke ein. Wenn Sie zustimmen, klicken Sie auf **kaufen**. Wenn Sie die Werte für die Vorlage festgelegt haben, klicken Sie dann auf **Erstellen**.

4.  Wenn die Bereitstellung abgeschlossen ist (in der Regel dauert ungefähr 30 Minuten), exportieren Sie die Zertifikatsdatei Cluster aus am Cluster-Knoten. In einem späteren Schritt importieren Sie dieses öffentliche Zertifikat auf dem Clientcomputer die serverseitige Authentifizierung für sichere HTTP-Bindung bereitzustellen.

    ein. Verbinden Sie vom Azure-Portal auf den am Knoten Remotedesktop.

     ![Verbinden Sie mit dem am Knoten][connect]

    b. Verwenden Sie Standardverfahren in Certificate Manager, um das Zertifikat am Knoten ohne den privaten Schlüssel (befindet sich unter Zertifikat: \LocalMachine\My) exportieren. In diesem Beispiel exportieren *CN = hpc01.eastus.cloudapp.azure.com*.

    ![Exportieren des Zertifikats][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>Option 2. Verwenden Sie das Skript HPC Pack IaaS-Bereitstellung

Das Bereitstellungsskript HPC Pack IaaS bietet eine andere vielseitige Möglichkeit einen HPC Pack Cluster bereitstellen. Er erstellt einen Cluster im Bereitstellungsmodell klassischen während die Vorlage im Modell zur Bereitstellung von Azure Ressourcenmanager verwendet. Darüber hinaus ist das Skript ein Abonnement in der globalen Azure oder China Azure Service kompatibel.

**Zusätzliche erforderliche Komponenten**

* **Azure PowerShell** - [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) (Version 0.8.10 oder höher) auf dem Clientcomputer.

* **Bereitstellungsskript HPC Pack IaaS** - herunterladen und Entpacken die neueste Version des Skripts vom [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Überprüfen Sie die Version des Skripts durch Ausführen `New-HPCIaaSCluster.ps1 –Version`. Dieser Artikel basiert auf Version 4.5.0 oder höher des Skripts.

**Erstellen der Konfigurationsdatei**

 Das Bereitstellungsskript HPC Pack IaaS verwendet eine XML-Konfigurationsdatei als Eingabe, die die Infrastruktur des HPC-Cluster beschrieben. Ersetzen durch die Werte für Ihre Umgebung in der folgenden Beispieldatei Konfiguration, zum Bereitstellen von eines Clusters mit ein am und 18 berechnen Knoten aus dem berechnen Knotenbild, das Microsoft Excel erstellt. Weitere Informationen zur Konfigurationsdatei finden Sie unter der Manual.rtf-Datei im Skriptordner und [Erstellen einer HPC Cluster mit HPC Pack IaaS Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Hinweise zu der Konfigurationsdatei**

* Die **VMName** des am Knotens **muss** die **ServiceName**identisch sein oder SOA Einzelvorgänge nicht ausgeführt.

* Stellen Sie sicher, dass Sie **EnableWebPortal** angeben, dass das Zertifikat am Knoten generiert und exportiert wird.

* Die Datei gibt ein nach der Konfiguration PowerShell-Skript PostConfig.ps1, die auf dem am Knoten ausgeführt wird. Das folgende Beispielskript konfiguriert die Verbindungszeichenfolge Azure-Speicher, die berechnen Knoten Rolle aus dem am Knoten entfernt, und wenn sie bereitgestellt werden, damit die Teilnehmer alle Knoten online. 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Führen Sie das Skript**

1.  Öffnen Sie die PowerShell-Konsole auf dem Clientcomputer als Administrator an.

2.  Wechseln Sie zu dem Skriptordner (in diesem Beispiel E:\IaaSClusterScript).

    ```
    cd E:\IaaSClusterScript
    ```
    
3.  Um den HPC Pack Cluster bereitstellen zu können, führen Sie den folgenden Befehl ein. In diesem Beispiel wird davon ausgegangen, dass die Konfigurationsdatei in E:\HPCDemoConfig.xml befindet.

    ```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```

Für einige Zeit, wird das Bereitstellungsskript HPC Pack ausgeführt. Eine Sache, das Skript bedeutet, besteht darin, exportieren und das Zertifikat Cluster herunterladen und speichern es im Ordner "Dokumente" des Benutzers auf dem Clientcomputer. Das Skript generiert eine ähnlich wie der folgende Meldung an. In einem der folgenden Schritte importieren Sie das Zertifikat im entsprechenden Zertifikat Store.    
    
    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Schritt 2. Verschieben von Excel-Arbeitsmappen, und führen Sie UDFs aus einem lokalen client

### <a name="excel-activation"></a>Excel-Aktivierung

Wenn Sie das Bild ComputeNodeWithExcel VM für Produktionsarbeitslasten verwenden zu können, müssen Sie einen gültigen Microsoft Office-Lizenz Key zum Aktivieren von Excel auf die Datenverarbeitungsknoten bereitstellen. Andernfalls die Testversion von Excel wird nach 30 Tagen und Ausführen von Excel-Arbeitsmappen mit COMException (0x800AC472) fehl. 

Sie können Excel weitere 30 Tage lang Zeit Auswertung rearm: Melden Sie sich an den am Knoten und Clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` Knoten über HPC Cluster Manager auf alle Excel zu berechnen. Sie können bis zu zwei Mal rearm. Anschließend müssen Sie einen gültigen Office-Lizenz Key angeben.

Office Professional Plus 2013 installiert haben, klicken Sie auf das Bild virtueller Computer ist eine Edition Lautstärke mit einer generische Lautstärke Lizenz Schlüssel (GVLK). Sie können sie über Schlüsselverwaltungsdienst (KMS) aktivieren / Active Directory-Based Aktivierung (AD-BA) oder mehrere des Mehrfachaktivierungsschlüssels (MAK). 

    * KMS/AD-BA verwenden, vorhandenen KMS Server verwenden oder einen neuen mithilfe von Microsoft Office 2013 Lautstärke Lizenz Pack einrichten. (Wenn Sie möchten, richten Sie den Server auf die am Knoten.) Aktivieren Sie dann KMS Host-Taste über das Internet oder per Telefon an. Klicken Sie dann Clusrun `ospp.vbs` die Excel berechnet zu die KMS-Server und den Port einrichten und Aktivieren von Office auf allen Knoten. 
    
    * Mit MAK, ersten Clusrun `ospp.vbs` die Excel berechnet zu die Taste Eingabe, und aktivieren Sie dann alle Knoten über das Internet oder per Telefon. 

>[AZURE.NOTE]Mit diesem virtuellen Computer Bild können Retail Product Keys für Office Professional Plus 2013 verwendet werden. Wenn Sie gültigen Schlüssel und Installationsmedien für Office oder Excel-Editionen als dieser Office Professional Plus 2013 Lautstärke Edition verfügen, können Sie diese verwenden. Klicken Sie zuerst deinstallieren Sie dieses Volume Edition, und installieren Sie die Edition, die Sie besitzen. Neu installierte Knoten der Excel-berechnen kann als Bild zur Verwendung in einer Bereitstellung bei angepassten virtuellen Computer erfasst werden.

### <a name="offload-excel-workbooks"></a>Verschieben von Excel-Arbeitsmappen

Wie folgt vor, um eine Excel-Arbeitsmappe zu verschieben, damit es auf dem HPC Pack Cluster in Azure ausgeführt wird. Dazu müssen Sie Excel 2010 oder 2013 bereits auf dem Clientcomputer installiert.

1. Verwenden Sie eine der Optionen in Schritt 1, um eine HPC Pack Cluster mit der Excel bereitstellen berechnen Knotenbild. Erhalten Sie die Cluster-Zertifikatsdatei (CER) und Cluster-Benutzernamen und Ihr Kennwort ein.

2. Importieren Sie das Zertifikat Cluster unter Zertifikat: \CurrentUser\Root auf dem Clientcomputer.

3. Stellen Sie sicher, dass Excel installiert ist. Erstellen Sie eine Datei Excel.exe.config mit dem folgenden Inhalt in denselben Ordner wie Excel.exe auf dem Clientcomputer. Dieser Schritt ist sichergestellt, dass die HPC Pack 2012 R2 Excel COM-add-ins erfolgreich geladen.

    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
    
4.  Richten Sie den Kunden Aufträge mit dem HPC Pack Cluster senden aus. Eine Möglichkeit ist, um die vollständige [Installation HPC Pack 2012 R2 Update 3](http://www.microsoft.com/download/details.aspx?id=49922) herunterladen und Installieren des HPC Pack-Clients. Alternativ herunterladen Sie und installieren Sie die [HPC Pack 2012 R2 Update 3-Clienttools](https://www.microsoft.com/download/details.aspx?id=49923) und die entsprechenden Visual C++ 2010 redistributable für Ihren Computer ([X64](http://www.microsoft.com/download/details.aspx?id=14632), [X86](https://www.microsoft.com/download/details.aspx?id=5555)).

5.  In diesem Beispiel verwenden wir eine Stichprobe Excel-Arbeitsmappe mit dem Namen ConvertiblePricing_Complete.xlsb. Sie können es herunterladen [können](https://www.microsoft.com/en-us/download/details.aspx?id=2939).

6.  Kopieren Sie die Excel-Arbeitsmappe in einen Arbeitsordner, z. B. D:\Excel\Run ein.

7.  Öffnen Sie die Excel-Arbeitsmappe. Klicken Sie auf dem Menüband **Entwicklung** klicken Sie auf **COM-Add-Ins** , und bestätigen Sie, dass die HPC Pack Excel COM-add-ins erfolgreich geladen wird.

    ![Excel-add-in für HPC Pack][addin]

8.  Bearbeiten Sie die VBA-Makros HPCControlMacros in Excel durch Ändern der Kommentarzeilen, wie im folgenden Skript dargestellt. Ersetzen durch die entsprechenden Werte für Ihre Umgebung.

    ![Excel-Makros für HPC Pack][macro]

    ```
    'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
    Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"

    'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
    Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.Initialize ActiveWorkbook
    HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles

    'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
    HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
    HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
```

9.  Kopieren Sie die Excel-Arbeitsmappe auf ein Verzeichnis hochladen, z. B. D:\Excel\Upload ein. Dieses Verzeichnis in die Konstante HPC_DependsFiles in die VBA-Makros angegeben.

10. Wenn die Arbeitsmappe auf dem Cluster in Azure ausführen möchten, klicken Sie auf die Schaltfläche **Cluster** auf dem Arbeitsblatt.

### <a name="run-excel-udfs"></a>Ausführen von Excel UDFs

Um Excel UDFs ausführen zu können, führen Sie die vorherigen Schritte 1 bis 3 auf dem Clientcomputer einrichten. Für Excel UDFs brauchen Sie die Excel-Anwendung, die auf Datenverarbeitungsknoten installiert sein. Ja, beim Erstellen des Clusters Knoten zu berechnen, können Sie auswählen eine normale anstelle der berechnen Knotenbild mit Excel zu berechnen.

>[AZURE.NOTE] Es gibt maximal 34 Zeichen in der Excel 2010 und 2013 Cluster Connector-Dialogfeld ein. Verwenden Sie dieses Dialogfeld Cluster angeben, der die UDFs ausgeführt wird. Wenn der Clustername des vollständigen mehr enthält (z. B. hpcexcelhn01.southeastasia.cloudapp.azure.com), passt nicht in das Dialogfeld. Zum Festlegen einer Variablen maschinellen organisationsweite wie *CCP_IAASHN* mit dem Wert des Clusternamens lange kann umgangen werden. Geben Sie *"% CCP_IAASHN"* klicken Sie dann im Dialogfeld als Cluster am Knotennamen. 

Nachdem der Cluster erfolgreich bereitgestellt wurde, die folgenden Schritte zum Ausführen einer Stichprobe integrierten fortzusetzen Excel UDFs. Angepasste Excel UDFs finden Sie unter diese [Ressourcen](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) der XLLs erstellen und diese auf die IaaS Cluster bereitstellen.

1.  Öffnen Sie eine neue Excel-Arbeitsmappe. Klicken Sie im Menüband **Entwicklung** auf **Add-Ins**. Klicken Sie dann im Dialogfeld klicken Sie auf **Durchsuchen**, navigieren Sie zum Ordner %CCP_HOME%Bin\XLL32, und wählen Sie aus der Stichprobe ClusterUDF32.xll. Wenn die ClusterUDF32 auf dem Clientcomputer nicht vorhanden ist, kopieren Sie sie aus dem Ordner %CCP_HOME%Bin\XLL32 auf dem am Knoten ein.

    ![Wählen Sie UDFs][udf]

2.  Klicken Sie auf **Datei** > **Optionen** > **Erweitert**. Aktivieren Sie unter **Formeln** **Zulassen User defined XLL Funktionen zu einem berechnungscluster ausführen**. Klicken Sie dann auf **Optionen** , und geben Sie den Clusternamen der vollständigen in **Cluster am Knoten ein**. (Wie bereits erwähnt ist zuvor dieses Eingabefeld auf 34 Zeichen beschränkt, damit eine lange Clustername nicht passt möglicherweise. Sie können hier eine maschinelle organisationsweite Variable für einen Clusternamen lange verwenden.)

    ![Konfigurieren des UDFs][options]

3.  Um die Berechnung UDFs Cluster ausgeführt werden, klicken Sie auf die Zelle mit dem Wert =XllGetComputerNameC(), und drücken Sie die EINGABETASTE. Die Funktion ruft einfach den Namen des Knotens berechnen für den UDFs ausgeführt wird. Fordert für den ersten Ausführen klicken Sie im Dialogfeld Anmeldeinformationen für den Benutzernamen und das Kennwort für die Verbindung mit dem Cluster IaaS.

    ![Ausführen von UDFs][run]

    Wenn es viele Zellen gibt zu berechnen, drücken Sie Alt-Umschalt-Strg + F9 zum Ausführen der Berechnung auf alle Zellen aus.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>Schritt 3. Ausführen einer SOA Arbeitsbelastung aus einem lokalen client

Führen Sie allgemeine SOA-Anwendungen auf dem HPC Pack IaaS Cluster zuerst mit einer der Methoden in Schritt 1 Cluster bereitstellen. Geben Sie eine generische Knotenbild in diesem Fall berechnen, da Sie nicht Excel auf die Datenverarbeitungsknoten benötigen. Klicken Sie dann wie folgt vor.

1. Importieren Sie nach Erhalt des Zertifikats Cluster es auf dem Clientcomputer unter Zertifikat: \CurrentUser\Root aus.

2. Installieren Sie die [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) und [HPC Pack 2012 R2 Update 3-Clienttools](https://www.microsoft.com/download/details.aspx?id=49923). Diese Tools ermöglichen Sie entwickeln und SOA Clientanwendungen auszuführen.

3. Laden Sie die HelloWorldR2 [Beispiel-Code](https://www.microsoft.com/download/details.aspx?id=41633)ein. Öffnen Sie die HelloWorldR2.sln in Visual Studio 2010 oder 2012.

4. Erstellen Sie zuerst das Projekt EchoService. Bereitstellen von den Dienst zum Cluster IaaS klicken Sie dann auf die gleiche Weise, die Sie zu einem lokalen Cluster bereitstellen. Die detaillierten Schritte finden Sie in der Infodatei in HelloWordR2. Ändern Sie, und erstellen Sie die HellWorldR2 und anderen Projekten wie im folgenden Abschnitt beschrieben, um den SOA-Clientanwendungen generieren, die auf einem Cluster Azure IaaS ausgeführt werden.

### <a name="use-http-binding-with-azure-storage-queue"></a>Verwenden von HTTP-Bindung mit Warteschlange Azure-Speicher

Nehmen Sie HTTP-Bindung zur Verwendung mit einer Warteschlange Azure-Speicher ein paar Änderungen an den Muster-Code ein.

* Aktualisieren Sie den Clusternamen ein.

    ```
// Before
const string headnode = "[headnode]";
// After e.g.
const string headnode = "hpc01.eastus.cloudapp.azure.com";
or
const string headnode = "hpc01.cloudapp.net";
```

* Optional, verwenden Sie den Standardwert TransportScheme in SessionStartInfo, oder legen Sie ihn explizit auf Http.

```
    info.TransportScheme = TransportScheme.Http;
```

* Verwenden Sie standardmäßige Bindung, für die BrokerClient.

    ```
// Before
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
// After
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
```

    Oder explizit mithilfe der BasicHttpBinding festgelegt.

    ```
BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
```

* Legen Sie gegebenenfalls die Kennzeichnung UseAzureQueue true in SessionStartInfo ein. Wenn nicht festgelegt, es festgelegt wird Wenn der Cluster Azure Domänensuffixe enthält die TransportScheme Http standardmäßig auf true.

    ```
    info.UseAzureQueue = true;
```

###<a name="use-http-binding-without-azure-storage-queue"></a>Verwenden von HTTP-Bindung ohne Warteschlange Azure-Speicher

HTTP-Bindung, ohne eine Warteschlange Azure-Speicher explizit verwenden die Kennzeichnung UseAzureQueue auf False wird in der SessionStartInfo eingestellt.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>Verwenden Sie NetTcp Bindung

Um NetTcp Bindung verwenden zu können, ist die Konfiguration zum Herstellen einer Verbindung mit einer lokalen Cluster ähnlich. Sie müssen ein paar Endpunkte auf dem am Knoten virtueller Computer zu öffnen. Wenn Sie das Bereitstellungsskript HPC Pack IaaS verwendet, um den Cluster zu erstellen, beispielsweise, legen Sie die Endpunkte in der klassischen Azure-Portal wie folgt.


1. Beenden Sie den virtuellen Computer an.

2. Hinzufügen der TCP-9090, 9087, 9091, 9094 für die Sitzung, Broker, Broker Worker und Datendienste Hilfethemas

    ![Konfigurieren von Endpunkten][endpoint]

3. Starten Sie den virtuellen Computer an.

Die SOA-Clientanwendung erfordert keine Änderungen mit Ausnahme der Änderung des am Namens auf den IaaS Cluster vollständigen Namen an.

## <a name="next-steps"></a>Nächste Schritte

* Finden Sie unter [folgenden Ressourcen](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) für Weitere Informationen zu Excel-Auslastung mit HPC Pack ausgeführt.

* Finden Sie unter [Verwalten von SOA-Dienste in Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) für Weitere Informationen zum Bereitstellen und Verwalten von SOA-Dienste mit HPC Pack ein.

<!--Image references-->
[scenario]: ./media/virtual-machines-windows-excel-cluster-hpcpack/scenario.png
[github]: ./media/virtual-machines-windows-excel-cluster-hpcpack/github.png
[template]: ./media/virtual-machines-windows-excel-cluster-hpcpack/template.png
[parameters]: ./media/virtual-machines-windows-excel-cluster-hpcpack/parameters.png
[create]: ./media/virtual-machines-windows-excel-cluster-hpcpack/create.png
[connect]: ./media/virtual-machines-windows-excel-cluster-hpcpack/connect.png
[cert]: ./media/virtual-machines-windows-excel-cluster-hpcpack/cert.png
[addin]: ./media/virtual-machines-windows-excel-cluster-hpcpack/addin.png
[macro]: ./media/virtual-machines-windows-excel-cluster-hpcpack/macro.png
[options]: ./media/virtual-machines-windows-excel-cluster-hpcpack/options.png
[run]: ./media/virtual-machines-windows-excel-cluster-hpcpack/run.png
[endpoint]: ./media/virtual-machines-windows-excel-cluster-hpcpack/endpoint.png
[udf]: ./media/virtual-machines-windows-excel-cluster-hpcpack/udf.png
