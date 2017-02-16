<properties
    pageTitle="Hyper-V virtuelle Computer in VMM Wolken zu einer sekundären VMM-Website mithilfe der PowerShell (Ressourcenmanager) repliziert | Microsoft Azure"
    description="Beschreibt das Bereitstellen von Azure Website Wiederherstellung zum Koordinieren von Replikation, Failover- und Hyper-V virtueller Computer in VMM Wolken zu einer sekundären VMM-Website mithilfe der PowerShell (Ressourcen-Manager)"
    services="site-recovery"
    documentationCenter=""
    authors="sujaytalasila"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="sutalasi"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a>Repliziert Hyper-V virtuelle Computer in VMM Wolken zu einer sekundären VMM-Website mithilfe der PowerShell (Ressourcen-Manager)

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmm-to-vmm.md)
- [Klassische Portal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell - Ressourcenmanager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Willkommen bei Azure Website Wiederherstellung! Verwenden Sie in diesem Artikel, wenn soll lokalen Hyper-V-virtuellen Computern repliziert in System Center virtuellen Computern Manager (VMM) Wolken an einem sekundären Standort verwaltet. 

In diesem Artikel wird gezeigt, wie Sie PowerShell verwenden, um allgemeine Aufgaben automatisieren, die Sie beim Einrichten von Azure Website Wiederherstellung Hyper-V-virtuellen Computern in System Center VMM Wolken auf System Center VMM Wolken im sekundären repliziert ausführen müssen.

Im Artikel enthält erforderliche Komponenten für das Szenario und zeigt Ihnen 

- Informationen zum Einrichten einer Wiederherstellung Services Tresor
- Installieren Sie den Azure-Anbieter für Websites Wiederherstellung auf die Quelle VMM-Server und dem Ziel VMM-server
- Registrieren der VMM-Server im Tresor
- Konfigurieren der Replikationsrichtlinie für die Cloud VMM. Die Replikation Einstellungen für die Richtlinie werden auf alle geschützten virtuellen Computern angewendet 
- Aktivieren des Schutzes für die virtuellen Computer. 
- Testen Sie das Failover von virtuellen Maschinen, einzeln oder als Teil eines Plans Wiederherstellung, um sicherzustellen, dass alles wie erwartet funktioniert.
- Ausführen eines geplanten oder einer ungeplanten Failover von virtuellen Maschinen, einzeln oder als Teil eines Plans Wiederherstellung, um sicherzustellen, dass alles wie erwartet funktioniert.

Wenn Sie Probleme beim Einrichten dieses Szenario auftreten, stellen Sie Ihre Fragen im [Azure Wiederherstellung Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure weist zwei verschiedenen [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Arbeiten mit Ressourcen: Azure Ressourcenmanager und Classic. Azure verfügt auch über zwei communityportalen – Azure klassischen Portals, die das Bereitstellungsmodell klassischen unterstützt, und der Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle. Dieser Artikel behandelt das Modell zur Bereitstellung von Ressourcenmanager.



## <a name="on-premises-prerequisites"></a>Lokale erforderliche Komponenten

Hier ist, was in der primären und sekundären lokalen Websites müssen Sie dieses Szenario bereitstellen:

**Erforderliche Komponenten** | **Details** 
--- | ---
**VMM** | Es empfiehlt sich, dass Sie eine VMM-Server in der primären Standort und einem VMM-Server in der sekundäre Standort bereitstellen.<br/><br/> Sie können auch die [Replikation zwischen Wolken auf einem einzelnen VMM-Server](site-recovery-single-vmm.md). Dazu benötigen Sie mindestens zwei Wolken auf dem Server VMM konfiguriert.<br/><br/> VMM Server sollte mindestens ausführen System Center 2012 SP1 mit den neuesten Updates.<br/><br/> Jede VMM-Server muss in eine oder mehrere Wolken konfiguriert und alle Wolken das Hyper-V Kapazität Profil festlegen müssen. <br/><br/>Wolken müssen eine oder mehrere VMM Hostgruppen enthalten.<br/><br/>Weitere Informationen zum Einrichten von VMM Wolken in [der Cloud-Textur VMM konfigurieren](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), und [Exemplarische Vorgehensweise: Erstellen von privaten Wolken mit System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM-Servern sollte Zugriff auf das Internet haben. 
**Hyper-V** | Hyper-V Server müssen mindestens ausführen Windows Server 2012 mit Hyper-V-Rolle und die neuesten Updates installiert haben.<br/><br/> Ein Hyper-V-Server sollte eine oder mehrere virtuelle Computer enthalten.<br/><br/>  Hyper-V-Hostservern sollte in Hostgruppen in der primären und sekundären VMM Wolken befinden.<br/><br/> Wenn Sie Hyper-V in einem Cluster in Windows Server 2012 R2 ausführen sollten Sie [Aktualisieren 2961977](https://support.microsoft.com/kb/2961977) installieren<br/><br/> Wenn Sie Hyper-V in einem Cluster unter Windows Server 2012 Notiz dieser Bank Cluster ausführen wird nicht automatisch erstellt, wenn Sie einen statischen IP-Adresse-basierten Cluster haben. Sie müssen der Cluster Makler manuell konfigurieren. [Weitere Informationen finden](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Anbieter** | Während der Wiederherstellung von Website-Bereitstellung installieren Sie den Azure-Anbieter für Websites Wiederherstellung auf VMM-Servern. Der Anbieter kommuniziert mit Website Wiederherstellung über HTTPS 443 zu koordinieren Replikation. Datenreplikation tritt zwischen der primären und sekundären Hyper-V-Servern über das LAN oder eine VPN-Verbindung.<br/><br/> Der Anbieter ausgeführt wird, auf dem Server VMM benötigt Zugriff auf diese URLs: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Darüber hinaus lässt Firewall Kommunikation von den Servern VMM für die [IP-Adressbereiche Azure Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) und das Protokoll HTTPS (443).

### <a name="network-mapping-prerequisites"></a>Voraussetzungen für Netzwerk-Zuordnung
Netzwerk-Zuordnung Karten zwischen VMM VM Netzwerken auf der primären und sekundären VMM-Server zu:

- Platzieren Sie optimal Replikat virtuellen Computern auf sekundäre Hyper-V-Hosts nach Failover.
- Herstellen einer Verbindung entsprechenden virtuellen Computer Netzwerken mit Replikat virtuellen Computern.
- Wenn Sie nicht Netzwerk konfigurieren wird nicht Replikat Zuordnung virtuellen Computern mit jedes Netzwerk nach Failover verbunden sein.
- Wenn Sie Netzwerk einrichten möchten ist Zuordnung während der Wiederherstellung Sites Bereitstellung hier Sie müssen:

    - Stellen Sie sicher, dass virtuelle Computer, auf dem Quelle Hyper-V-Host-Server mit einem VMM VM-Netzwerk verbunden sind. Diesem Netzwerk sollte ein logisches Netzwerk verknüpft werden, die mit der Cloud verknüpft ist.
    - Stellen Sie sicher, dass die sekundäre Cloud, die für die Wiederherstellung verwendet werden, ein entsprechender virtueller Computer Netzwerk konfiguriert ist. Die virtuellen Computer Netzwerk mit einem logischen Netzwerk verknüpft werden soll, die der sekundäre Cloud zugeordnet ist.


Weitere Informationen zum Konfigurieren von VMM Netzwerken in den unter Artikel

- [Konfigurieren von logische Netzwerke in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Konfigurieren von virtuellen Computer Netzwerke und Gateways in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)

[Erfahren Sie mehr](site-recovery-network-mapping.md) über die Funktionsweise der Netzwerk-Zuordnung.

###<a name="powershell-prerequisites"></a>PowerShell erforderliche Komponenten
Stellen Sie sicher, dass stehen Ihnen Azure PowerShell erstellt haben. Wenn Sie bereits PowerShell verwenden, müssen Sie auf Version 0.8.10 oder höher. Informationen zum Einrichten von PowerShell finden Sie den [Leitfaden zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Sobald Sie einrichten und PowerShell konfiguriert haben, können Sie alle verfügbaren Cmdlets für den Dienst anzeigen [können](https://msdn.microsoft.com/library/dn850420.aspx). 

Wenn Sie wissen, dass Sie verwenden die Cmdlets, z. B., wie Parameterwerte, Eingaben und Ausgaben in der Regel in Azure PowerShell behandelt werden Tipps, die Ihnen helfen können finden Sie im [Leitfaden für erste Schritte mit Azure-Cmdlets](https://msdn.microsoft.com/library/azure/jj554332.aspx).

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

1. Erstellen einer Ressourcengruppe Azure Ressourcenmanager aus, wenn Sie eine bereits besitzen

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Erstellen Sie einer neuen Wiederherstellung Services Tresor, und speichern Sie das erstellte ASR Tresor Objekt in einer Variablen (wird später verwendet werden). Sie können auch das ASR Tresor Objekt Beitrag erstellen, die mithilfe des Cmdlets Get-AzureRMRecoveryServicesVault abrufen:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location 

## <a name="step-3-set-the-recovery-services-vault-context"></a>Schritt 3: Einrichten des Wiederherstellung Services Tresor Kontexts

1.  Wenn Sie eine Tresor bereits erstellt haben, führen Sie die unter Befehl aus, um den Tresor zu erhalten.

        $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname

2.  Festlegen des Kontexts Tresor durch Ausführen der folgenden Befehl.

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


## <a name="step-5-create-and-associate-a-replication-policy"></a>Schritt 5: Erstellen und Zuordnen einer Replikationsrichtlinie

1.  Erstellen Sie eine Hyper-V 2012 R2 Replikation-Richtlinie, indem Sie den folgenden Befehl ausführen:

    
        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod 

    > [AZURE.NOTE] Die Cloud VMM kann Hyper-V-Hosts mit verschiedenen Versionen von Windows Server (wie die Hyper-V Vorkenntnisse angegeben) enthalten, jedoch die Replikationsrichtlinie ist bestimmte Betriebssystemversion. Wenn Sie verschiedene Hosts für das Betriebssystem von verschiedenen Versionen ausgeführt haben, erstellen Sie dann separate Replikation Richtlinien für jede Art von Version des Betriebssystems. Für z. B.: Wenn Sie fünf Hosts für Windows Server 2012 und drei auf Windows Server 2012 R2 ausgeführt haben, erstellen Sie zwei Replikation Richtlinien – eine für die einzelnen Versionen des Betriebssystems.

2.  Abrufen der primären Schutz Container (primäre VMM Cloud) und Wiederherstellung Schutz Container (Wiederherstellung VMM Cloud) durch Ausführen der folgenden Befehle:
    
        $PrimaryCloud = "testprimarycloud"
        $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

        $RecoveryCloud = "testrecoverycloud"
        $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
  
3.  Abrufen der Richtlinie, die Sie in Schritt 1 mit der angezeigte Name der Richtlinie erstellt

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Beginnen Sie mit der Replikationsrichtlinie die Zuordnung des Containers Schutz (VMM Cloud):

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer

5.  Warten Sie die Richtlinie Association Auftrag abzuschließen. Sie können prüfen, ob der Job abgeschlossen ist, verwenden den folgenden PowerShell-Codeausschnitt.
   
        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
            $isJobLeftForProcessing = $true;
        }

    Nachdem Sie der Auftrag Verarbeitung abgeschlossen ist, führen Sie den folgenden Befehl aus:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)


Um den Abschluss des Vorgangs zu überprüfen, folgen Sie den Schritten [Monitor Aktivität](#monitor)aus.

## <a name="step-5-configure-network-mapping"></a>Schritt 5: Konfigurieren von Netzwerk-Zuordnung

1. Der erste Befehl ruft Servern für den aktuellen Azure Website Wiederherstellung Tresor. Der Befehl speichert Microsoft Azure Website wiederherstellen-Servern in der Variablen $Servers Matrix an.

        $Servers = Get-AzureRmSiteRecoveryServer

2. Die erhalten Sie unter Befehle des Website Wiederherstellung Netzwerks für die Quelle VMM-Server und dem Ziel VMM-Server.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    
    > [AZURE.NOTE] Der Quelle VMM-Server kann der ersten Phase oder der zweiten Zeile in der Matrix Servers. Überprüfen Sie die Namen der Server VMM und rufen Sie die Netzwerke ordnungsgemäß


4. Das Cmdlet "abgeschlossen" erstellt eine Zuordnung zwischen der primären und der Wiederherstellung Netzwerk. Das Cmdlet gibt das primäre Netzwerk als das erste Element der $PrimaryNetworks und der Wiederherstellung-Netzwerk, wie das erste Element der $RecoveryNetworks an.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-6-configure-storage-mapping"></a>Schritt 6: Konfigurieren von Speicher-Zuordnung

1. Die unter Befehl ruft Sie die Liste der Klassifizierung von Speicher in $storageclassifications Variable.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification


2. Die erhalten Sie unter Befehle der Quelle Einstufung in $SourceClassificaion Variable und Zielwebsites Klassifizierung in $TargetClassification Variable. 

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    
    > [AZURE.NOTE] Die Quell- und Zielwebsites Einstufung können jedes Element in der Matrix sein. Schlagen Sie in der Ausgabe der unter Befehl aus, um den Index der Quell- und Zielwebsites Klassifizierung in der Matrix $storageclassifications ermitteln. 
    
    > Get-AzureRmSiteRecoveryStorageClassification | Select-Object - Eigenschaft FriendlyName, Id | Tabelle formatieren


3. Die unter Cmdlet erstellt eine Zuordnung zwischen der Quelle Klassifizierung und die gewünschte Klassifizierung. 

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-7-enable-protection-for-virtual-machines"></a>Schritt 7: Aktivieren des Schutzes für virtuellen Computern

Nachdem der Server, Wolken und Netzwerke ordnungsgemäß konfiguriert sind, können Sie den Schutz für virtuellen Computern in der Cloud aktivieren. 


  1. Zum Schutz aktivieren möchten, führen Sie den folgenden Befehl aus, um den Schutz Container zu erhalten:
    
            $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
    
  2. Erhalten Sie die Schutz Entität (virtueller Computer), indem Sie den folgenden Befehl ausführen:
    
            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
        
  3. Aktivieren Sie Replikation für den virtuellen Computer, indem Sie den folgenden Befehl ausführen:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy


## <a name="test-your-deployment"></a>Testen der bereitstellungs

Zum Ausführen eines Failovers Test für einen einzelnen virtuellen Computer, oder erstellen einen Wiederherstellungsplan aus mehreren virtuellen Computern und Ausführen ein Failovers Test für den Plan, Testen der Bereitstellung. Test Failover simuliert Ihrer Failover und Wiederherstellung Verfahren in einem Netzwerk isoliert. 

> [AZURE.NOTE] Sie können einen Wiederherstellungsplan Azure-Portal für eine Anwendung erstellen.

Um den Abschluss des Vorgangs zu überprüfen, folgen Sie den Schritten [Monitor Aktivität](#monitor)aus.


### <a name="run-a-test-failover"></a>Ausführen eines Failovers testen

1.  Führen Sie die unter Cmdlets abzurufenden virtueller Computer Netzwerk Sie Failover testen möchten Ihre virtuelle Computer.

        $Servers = Get-AzureRmSiteRecoveryServer
        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2.  Führen Sie ein Test-Failover eines virtuellen Computers folgendermaßen aus:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1] 

2.  Führen Sie ein Test-Failover für einen Wiederherstellungsplan folgendermaßen aus:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1] 

### <a name="run-a-planned-failover"></a>Ausführen eines geplanten Failovers

1. Ausführen eines geplanten Failovers eines virtuellen Computers mit den folgenden Schritten:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. Ausführen eines geplanten Failovers eines Plans Wiederherstellung mit den folgenden Schritten:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Ausführen eines ungeplanten Failovers

1. Ausführen eines ungeplanten Failovers eines virtuellen Computers mit den folgenden Schritten:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 

2.Klicken Ausführen eines ungeplanten Failovers eines Plans Wiederherstellung mit den folgenden Schritten:
        
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 
    
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

