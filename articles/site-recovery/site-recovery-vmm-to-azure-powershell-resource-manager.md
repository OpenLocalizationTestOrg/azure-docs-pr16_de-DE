<properties
    pageTitle="Repliziert Hyper-V virtuelle Computer in VMM Wolken mit Azure Website Wiederherstellung und PowerShell (Ressourcenmanager) | Microsoft Azure"
    description="Repliziert Hyper-V virtuelle Computer in VMM Wolken mit Azure Website Wiederherstellung und PowerShell"
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="rajanaki"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell-and-azure-resource-manager"></a>Hyper-V virtuelle Computer in VMM Wolken auf Azure mit PowerShell und Azure Ressourcenmanager repliziert

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmm-to-azure.md)
- [PowerShell - Ressourcenmanager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klassische-Portal](site-recovery-vmm-to-azure-classic.md)
- [PowerShell - klassisch](site-recovery-deploy-with-powershell.md)



## <a name="overview"></a>(Übersicht)

Azure Website Wiederherstellung beiträgt zu Ihrer Business Continuity- und Disaster Wiederherstellung (BCDR) Strategie durch Replikation, Failover und Wiederherstellung von virtuellen Computern in einer Reihe von Szenarien für die Bereitstellung orchestriert. Eine vollständige Liste der Bereitstellungsszenarien finden Sie unter der [Azure Website Wiederherstellung (Übersicht)](site-recovery-overview.md).

In diesem Artikel wird gezeigt, wie Sie PowerShell verwenden, um allgemeine Aufgaben automatisieren, die Sie beim Einrichten von Azure Website Wiederherstellung Hyper-V-virtuellen Computern in System Center VMM Wolken auf Azure-Speicher repliziert ausführen müssen.

Im Artikel enthält erforderliche Komponenten für das Szenario und zeigt Ihnen

- Informationen zum Einrichten einer Wiederherstellung Services Tresor
- Installieren Sie den Azure-Anbieter für Websites Wiederherstellung auf die Quelle VMM-server
- Registrieren des Servers im Tresor, zum Hinzufügen eines Kontos Azure-Speicher
- Installieren des Agents Azure Wiederherstellung Services auf Hyper-V-Host-Servern
- Konfigurieren von Schutz Einstellungen für VMM Wolken, die alle geschützten virtuellen Computern angewendet wird
- Aktivieren des Schutzes für diese virtuelle Computer.
- Testen der Fail-Over, um sicherzustellen, dass alles wie erwartet funktioniert.

Wenn Sie Probleme beim Einrichten dieses Szenario auftreten, stellen Sie Ihre Fragen im [Azure Wiederherstellung Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle für das Erstellen und Arbeiten mit Ressourcen: [Ressourcenmanager und Classic](../resource-manager-deployment-model.md). Dieser Artikel behandelt das Modell zur Bereitstellung von Ressourcenmanager verwenden.

## <a name="before-you-start"></a>Bevor Sie beginnen

Stellen Sie sicher, dass Sie diese erforderlichen Komponenten angeordnet haben:

### <a name="azure-prerequisites"></a>Azure erforderliche Komponenten

- Benötigen Sie ein [Microsoft Azure](https://azure.microsoft.com/) -Konto ein. Wenn Sie eine besitzen, beginnen Sie mit einer [kostenlosen Konto](https://azure.microsoft.com/free). Darüber hinaus können Sie über die [Preise Azure Website Wiederherstellung Manager](https://azure.microsoft.com/pricing/details/site-recovery/)lesen.
- Wenn Sie, sich die Replikation zu einem CSP Abonnement Szenario versuchen, benötigen Sie ein Abonnement CSP. Weitere Informationen über das Programm CSP in [wie das Programm CSP anmelden](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).
- Sie benötigen ein Azure Version 2 Speicher (Ressourcenmanager) Konto zum Speichern von Daten auf Azure repliziert. Das Konto benötigt Geo-Replikation aktiviert. Es sollte in derselben Region als Azure Website Wiederherstellung-Dienst werden, und das gleiche Abonnement oder das Abonnement CSP zugeordnet werden. Finden Sie weitere Informationen zum Einrichten von Azure-Speicher finden Sie die [Einführung in Microsoft Azure-Speicher](../storage/storage-introduction.md) für den Verweis ein.
- Sie müssen, um sicherzustellen, dass der [Azure-virtuellen Computern Vorkenntnisse](site-recovery-best-practices.md#azure-virtual-machine-requirements)virtuellen Computern, die Sie schützen möchten einhalten.

> [AZURE.NOTE] Derzeit sind nur virtueller Computer Ebene Vorgänge über Powershell möglich. Unterstützung für Ebene Plan Wiederherstellungsvorgängen wird verfügbar gemacht werden.  Jetzt sind Sie mit der Ausführung der Fail "Overs" nur nach einer einzelnen 'geschützte virtueller Computer' und nicht auf einen Plan für die Wiederherstellung Ebene beschränkt.

### <a name="vmm-prerequisites"></a>VMM erforderliche Komponenten
- Sie benötigen VMM-Server auf System Center 2012 R2 ausführen zu können.
- Alle VMM-Server mit virtuellen Computern, die Sie schützen möchten, muss der Azure-Anbieter für Websites Wiederherstellung ausgeführt werden. Dies ist während der Bereitstellung Azure Website Wiederherstellung installiert.
- Sie benötigen mindestens eine Cloud auf dem Server VMM, die Sie schützen möchten. Die Cloud sollte enthalten:
    - Eine oder mehrere VMM Hostgruppen.
    - Eine oder mehrere Hyper-V-Host-Server oder Cluster in jeder Hostgruppe.
    - Ein oder mehrere virtuelle Computer auf dem Quellserver Hyper-V.
- Weitere Informationen zum Einrichten von VMM Wolken:
    - Weitere Informationen zu privaten VMM Wolken in [VMM 2012 und Wolken](http://go.microsoft.com/fwlink/?LinkId=324956)und [Was ist neu in Private Cloud mit System Center 2012 R2 VMM](http://go.microsoft.com/fwlink/?LinkId=324952) finden.
    - Informationen Sie zum [Konfigurieren der VMM-Cloud-Textur](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)
    - Erfahren Sie, nachdem der Cloud Fabric Elemente angeordnet sind zum Erstellen von privater Wolken in [eine private Cloud in VMM erstellen](http://go.microsoft.com/fwlink/p/?LinkId=324953) und in [Exemplarische Vorgehensweise: Erstellen von privaten Wolken mit System Center 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).


### <a name="hyper-v-prerequisites"></a>Voraussetzungen für die Hyper-V

- Die Host Hyper-V Server müssen mindestens ausgeführt werden **Windows Server 2012** mit Hyper-V-Rolle oder **Microsoft Hyper-V Server 2012** und die neuesten Updates installiert haben.
- Wenn Sie Hyper-V in einer Notiz Cluster dieser Bank Cluster ausführen wird nicht automatisch erstellt, wenn Sie einen statischen IP-Adresse-basierten Cluster haben. Sie müssen der Cluster Makler manuell konfigurieren. Für
- Anweisungen finden Sie unter [Konfigurieren von Hyper-V Replikat Bank](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).
- Alle Hyper-V Hostserver oder Cluster für die Sie zum Schutz verwalten möchten, muss in der Cloud VMM enthalten sein.

### <a name="network-mapping-prerequisites"></a>Voraussetzungen für Netzwerk-Zuordnung
Wenn Sie virtuellen Computern in Azure, im Netzwerk Zuordnung Karten des virtuellen Computers auf dem Server der Quelle VMM Netzwerke schützen und Adressieren Azure Netzwerke, um Folgendes zu aktivieren:

- Allen Computern, die über die gleichen Netzwerk fehlschlägt, können miteinander, eine Verbindung herstellen, unabhängig von der Wiederherstellung, denen planen sie sind.
- Ist ein Netzwerk-Gateway am Ziel Azure Netzwerk einrichten, können virtuellen Computern mit anderen lokalen virtuellen Computern verbinden.
- Wenn Sie Netzwerk-Zuordnung nicht konfiguriert haben, werden nur den virtuellen Computern, die nicht über im gleichen Wiederherstellungsplan können nach Fail-Over in Azure miteinander zu verbinden.

Wenn Sie Netzwerk-Zuordnung bereitstellen möchten benötigen Sie Folgendes:

- Den virtuellen Computern, die Sie auf die Quelle VMM-Server schützen möchten, sollten mit einem virtueller Computer-Netzwerk verbunden werden. Diesem Netzwerk sollte ein logisches Netzwerk verknüpft werden, die mit der Cloud verknüpft ist.
- Ein Azure-Netzwerk mit dem repliziert virtuellen Computern nach Fail-Over verbinden können. Dieses Netzwerk wählen Sie zum Zeitpunkt der Fail-Over. Das Netzwerk sollte in derselben Region als Ihr Abonnement Azure Website Wiederherstellung sein.

Weitere Informationen zum Netzwerk-Zuordnung in

- [Konfigurieren von logische Netzwerke in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Konfigurieren von virtuellen Computer Netzwerke und Gateways in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)
- [So konfigurieren und überwachen virtueller Netzwerke in Azure](https://azure.microsoft.com/documentation/services/virtual-network/)


###<a name="powershell-prerequisites"></a>PowerShell erforderliche Komponenten
Stellen Sie sicher, dass stehen Ihnen Azure PowerShell erstellt haben. Wenn Sie bereits PowerShell verwenden, müssen Sie auf Version 0.8.10 oder höher. Informationen zum Einrichten von PowerShell finden Sie den [Leitfaden zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Sobald Sie einrichten und PowerShell konfiguriert haben, können Sie alle verfügbaren Cmdlets für den Dienst anzeigen [können](https://msdn.microsoft.com/library/dn850420.aspx).

Finden Sie im [Leitfaden für erste Schritte mit Azure-Cmdlets](https://msdn.microsoft.com/library/azure/jj554332.aspx), wenn Sie wissen, dass Sie verwenden die Cmdlets, wie etwa wie Parameterwerte, Eingaben und Ausgaben in der Regel in Azure PowerShell verarbeitet werden Tipps, die Ihnen helfen können.

## <a name="step-1-set-the-subscription"></a>Schritt 1: Festlegen des Abonnements

1. Aus Azure Powershell, melden Sie sich bei Ihrem Konto Azure: verwenden die folgenden Cmdlets

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred


2. Abrufen einer Liste von Ihrer Abonnements. Dies wird auch der SubscriptionIDs für jedes Abonnement angegeben werden. Notieren Sie die SubscriptionID des Abonnements, in dem Sie den Wiederherstellung Services Tresor erstellen möchten

        Get-AzureRmSubscription

3. Legen Sie das Abonnement, in denen ist der Wiederherstellung Services Tresor erstellt werden, indem Sie die Abonnement-ID erwähnen

        Set-AzureRmContext –SubscriptionID <subscriptionId>


## <a name="step-2-create-a-recovery-services-vault"></a>Schritt 2: Erstellen einer Wiederherstellungsdatei Services Tresor

1. Erstellen Sie eine Ressourcengruppe in Azure Ressourcenmanager, wenn Sie eine bereits besitzen

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Erstellen Sie einer neuen Wiederherstellung Services Tresor, und speichern Sie das erstellte ASR Tresor-Objekt in einer Variablen (wird später verwendet werden). Sie können auch das ASR Tresor Objekt Beitrag erstellen, die mithilfe des Cmdlets Get-AzureRMRecoveryServicesVault abrufen:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a>Schritt 3: Einrichten des Wiederherstellung Services Tresor Kontexts

1.  Festlegen des Kontexts Tresor durch Ausführen der folgenden Befehl.

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Schritt 4: Installieren des Wiederherstellung-Anbieters Azure-Website

1.  Erstellen Sie ein Verzeichnis auf dem Computer VMM indem Sie den folgenden Befehl ausführen:

        New-Item c:\ASR -type directory

2.  Extrahieren Sie die Dateien mit dem heruntergeladenen Anbieter durch den folgenden Befehl ausführen

        pushd C:\ASR\
        .\AzureSiteRecoveryProvider.exe /x:. /q


3.  Installieren Sie den Anbieter mithilfe der folgenden Befehle:

        .\SetupDr.exe /i
        $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
        do
        {
                        $isNotInstalled = $true;
                        if(Test-Path $installationRegPath)
                        {
                                        $isNotInstalled = $false;
                        }
        }While($isNotInstalled)

    Warten Sie für die Installation auf Fertig stellen.

4.  Registrieren Sie den Server im Tresor mit dem folgenden Befehl aus:

        $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
        pushd $BinPath
        $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice


## <a name="step-5-create-an-azure-storage-account"></a>Schritt 5: Erstellen Sie ein Konto Azure-Speicher

1. Wenn Sie ein Azure-Speicher-Konto besitzen, erstellen Sie ein Konto Geo-Replikation aktiviert in der gleichen Geo als die Tresor durch den folgenden Befehl ausführen:

        $StorageAccountName = "teststorageacc1" #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"  
        $ResourceGroupName =  “myRG”            #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

Beachten Sie, dass Speicher-Konto muss in der gleichen Region wie der Dienst Azure Website Wiederherstellung und mit dem gleichen Abonnement verknüpft werden.

## <a name="step-6-install-the-azure-recovery-services-agent"></a>Schritt 6: Installieren des Wiederherstellung Azure-Agents Services

1. Herunterladen des Azure Wiederherstellung Services-Agents am [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) , und installieren Sie es auf jedem Hyper-V-Host-Server befindet sich in der VMM Wolken, die Sie schützen möchten.

2. Führen Sie den folgenden Befehl auf allen VMM Hosts:

    marsagentinstaller.exe/q /nu

## <a name="step-7-configure-cloud-protection-settings"></a>Schritt 7: Konfigurieren von Cloud Schutz-Einstellungen

1.  Erstellen Sie eine Replikationsrichtlinie Azure, indem Sie den folgenden Befehl ausführen:


        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

2.  Abrufen eines Containers Schutz durch Ausführen der folgenden Befehle:

        $PrimaryCloud = "testcloud"
        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

3.  Erhalten Sie die Richtliniendetails zu einer Variablen mithilfe der Projekt, das erstellt wurde, und den Namen der Anzeige Richtlinie erwähnen:

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Beginnen Sie mit der Replikationsrichtlinie die Zuordnung des Containers Schutz:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  

5.  Wenn das Projekt abgeschlossen wurde, führen Sie den folgenden Befehl aus:

        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
        $isJobLeftForProcessing = $true;
        }

6.  Nachdem Sie der Auftrag Verarbeitung abgeschlossen ist, führen Sie den folgenden Befehl aus:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)

Um den Abschluss des Vorgangs zu überprüfen, folgen Sie den Schritten [Monitor Aktivität](#monitor)aus.

## <a name="step-8-configure-network-mapping"></a>Schritt 8: Konfigurieren von Netzwerk-Zuordnung

Vorbemerkung Netzwerk Zuordnung überprüfen, ob virtuellen Computern auf die Quelle VMM-Server mit einem virtuellen Computer-Netzwerk verbunden sind. Darüber hinaus erstellen Sie ein oder mehrere Azure virtuelle Netzwerke.

Weitere Informationen zum Erstellen eines virtuellen Netzwerks mithilfe von Azure Ressourcenmanager und PowerShell in [ein virtuelles Netzwerk mit einer Website-zu-Standort VPN-Verbindung mithilfe von Azure Ressourcenmanager und PowerShell erstellen](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

Beachten Sie, dass mehrere virtuellen Computern Netzwerke mit einem einzelnen Azure Netzwerk zugeordnet werden können. Wenn das Zielnetzwerk verfügt über mehrere Subnetze und von diesen Subnetzen denselben Namen wie Subnetz hat, auf dem sich die Quelle virtuellen Computern befindet, wird dann Replikat virtuellen Computers mit diesem Ziel Subnetz nach Fail-Over verbunden sein. Ist kein Ziel Subnetz mit einem übereinstimmenden Namen, wird der virtuellen Computern mit dem ersten Subnetz im Netzwerk verbunden.

1. Der erste Befehl ruft Servern für den aktuellen Azure Website Wiederherstellung Tresor. Der Befehl speichert Microsoft Azure-Website wiederherstellen-Servern in der Variablen $Servers Matrix.

        $Servers = Get-AzureRmSiteRecoveryServer

2. Der zweite Befehl erhält das Standort Wiederherstellung-Netzwerk für den ersten Server in der Matrix $Servers. Der Befehl speichert die Netzwerke in der Variablen $Networks.


        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

3. Der dritte Befehl ruft Azure virtuelle Netzwerke, und klicken Sie dann diesen Wert in der Variablen $AzureVmNetworks.

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork

4. Das Cmdlet "abgeschlossen" erstellt eine Zuordnung zwischen der primären und Azure-virtuellen Computernetzwerk. Das Cmdlet gibt das primäre Netzwerk als das erste Element der $Networks an. Das Cmdlet gibt ein Netzwerk virtuellen Computern als das erste Element der $AzureVmNetworks an.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]


## <a name="step-9-enable-protection-for-virtual-machines"></a>Schritt 9: Aktivieren des Schutzes für virtuellen Computern

Nachdem der Server, Wolken und Netzwerke ordnungsgemäß konfiguriert sind, können Sie den Schutz für virtuelle Computer in der Cloud aktivieren.

 Beachten Sie Folgendes:

 - Virtuellen Computern müssen Azure erfüllen. Aktivieren Sie diese Elemente in [Voraussetzungen und Support](site-recovery-best-practices.md) im Planungshandbuch.

 - Zum Aktivieren von Schutz, das Betriebssystem und das Betriebssystem müssen Datenträgereigenschaften des virtuellen Computers festgelegt werden. Beim Erstellen eines virtuellen Computers in VMM mithilfe einer Vorlage virtuellen Computern können Sie die Eigenschaft festlegen. Sie können diese Eigenschaften für vorhandenen virtuellen Computern auch auf den Registerkarten **Allgemein** und **Hardwarekonfiguration** des virtuellen Computereigenschaften festlegen. Wenn Sie keinen dieser Eigenschaften in VMM festlegen werden Sie diese im Portal Azure Website Wiederherstellung konfiguriert sein.


  1. Zum Schutz aktivieren möchten, führen Sie den folgenden Befehl aus, um den Schutz Container zu erhalten:

            $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName

  2. Erhalten Sie die Schutz Entität (virtueller Computer), indem Sie den folgenden Befehl ausführen:

            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer

  3. Aktivieren Sie die DR für den virtuellen Computer, indem Sie den folgenden Befehl ausführen:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1



## <a name="test-your-deployment"></a>Testen der bereitstellungs

Zum Ausführen einer Test-Fail-Over für einen einzelnen virtuellen Computer, oder erstellen einen Wiederherstellungsplan aus mehreren virtuellen Computern und Ausführen einer Test-Fail-Over für den Plan, Testen der Bereitstellung. Test Fail-Over simuliert Ihrer Fail-Over und Wiederherstellung Verfahren in einem Netzwerk isoliert. Beachten Sie Folgendes:

- Wenn Sie die Verbindung des virtuellen Computers in Azure mithilfe von Remotedesktop nach der Fail-Over möchten, aktivieren Sie Remote Desktop-Verbindung des virtuellen Computers vor dem Ausführen des Failovers testen.
- Nach dem Fail-Over verwenden Sie eine öffentliche IP-Adresse Verbindung zu den virtuellen Computern in Azure mithilfe von Remotedesktop. Wenn Sie dies tun möchten, stellen Sie sicher, dass Sie keine Domänenrichtlinien besitzen, die verhindern, dass Sie eine Verbindung zu einer virtuellen Computern mithilfe einer öffentlichen Adresse.

Um den Abschluss des Vorgangs zu überprüfen, folgen Sie den Schritten [Monitor Aktivität](#monitor)aus.


### <a name="run-a-test-failover"></a>Ausführen eines Failovers testen

1.  Starten Sie das Test-Failover durch den folgenden Befehl ausführen:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a>Ausführen eines geplanten Failovers

1. Starten Sie das geplante Failover durch den folgenden Befehl ausführen:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a>Ausführen eines ungeplanten Failovers

1. Starten Sie das ungeplante Failover, indem Sie den folgenden Befehl ausführen:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  


## <a name="a-namemonitora-monitor-activity"></a><a name=monitor></a>Überwachen der Aktivität

Verwenden Sie die folgenden Befehle, um die Aktivität zu überwachen. Beachten Sie, dass Sie warten zwischen Einzelvorgänge für die Verarbeitung abgeschlossen haben.

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Nächste Schritte

[Weitere Informationen finden Sie](https://msdn.microsoft.com/library/azure/mt637930.aspx) Informationen zum Azure Website Wiederherstellung mit Azure Ressourcenmanager PowerShell-Cmdlets.
