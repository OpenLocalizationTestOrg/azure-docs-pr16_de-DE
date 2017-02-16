<properties
    pageTitle="Hyper-V virtuelle Computer in VMM Wolken mit Azure Website Wiederherstellung und PowerShell repliziert | Microsoft Azure"
    description="Erfahren Sie, wie die Replikation von Hyper-V virtuelle Computer in VMM Wolken mithilfe von Website-Wiederherstellung und PowerShell automatisieren."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell---classic"></a>Repliziert Hyper-V virtuelle Computer in VMM Wolken in Azure mithilfe der Powershell - klassisch

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmm-to-azure.md)
- [PowerShell - Ressourcenmanager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klassische-Portal](site-recovery-vmm-to-azure-classic.md)
- [PowerShell - klassisch](site-recovery-deploy-with-powershell.md)


## <a name="overview"></a>(Übersicht)

Azure Website Wiederherstellung beiträgt zu Ihrer Business Continuity- und Disaster Wiederherstellung (BCDR) Strategie durch Replikation, Failover und Wiederherstellung von virtuellen Computern in einer Reihe von Szenarien für die Bereitstellung orchestriert. Eine vollständige Liste der Bereitstellungsszenarien finden Sie unter der [Azure Website Wiederherstellung (Übersicht)](site-recovery-overview.md).

In diesem Artikel wird gezeigt, wie Sie PowerShell verwenden, um allgemeine Aufgaben automatisieren, die Sie beim Einrichten von Azure Website Wiederherstellung Hyper-V-virtuellen Computern in System Center VMM Wolken auf Azure-Speicher repliziert ausführen müssen.

Im Artikel enthält erforderliche Komponenten für das Szenario, und zeigt, wie Sie das Einrichten einer Website Wiederherstellung Tresor, den Azure-Anbieter für Websites Wiederherstellung serverseitig VMM Quelle installieren, Registrieren des Servers im Tresor, Azure-Speicherkonto hinzufügen, den Agent Azure Wiederherstellung Services auf Hyper-V Hostservern installieren, konfigurieren Schutz Einstellungen für VMM Wolken, die alle geschützten virtuellen Computern angewendet wird , und klicken Sie dann Aktivieren des Schutzes für diese virtuelle Computer. Einrichtung abzuschließen Sie, indem Sie das Failover, um sicherzustellen, dass alles wie erwartet funktioniert.

Wenn Sie Probleme beim Einrichten dieses Szenario auftreten, stellen Sie Ihre Fragen im [Azure Wiederherstellung Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle für das Erstellen und Arbeiten mit Ressourcen: [Ressourcenmanager und Classic](../resource-manager-deployment-model.md). Dieser Artikel behandelt die Bereitstellung Klassisch verwenden.



## <a name="before-you-start"></a>Bevor Sie beginnen

Stellen Sie sicher, dass Sie diese erforderlichen Komponenten angeordnet haben:

### <a name="azure-prerequisites"></a>Azure erforderliche Komponenten

- Benötigen Sie ein [Microsoft Azure](https://azure.microsoft.com/) -Konto ein. Sie können mit einer [kostenlosen Testversion](https://azure.microsoft.com/pricing/free-trial/)beginnen.
- Sie benötigen ein Konto Azure-Speicher replizierte Daten gespeichert. Das Konto benötigt Geo-Replikation aktiviert. Es sollte in derselben Region als der Azure-Website Wiederherstellung Tresor und mit dem gleichen Abonnement verknüpft werden. [Weitere Informationen zum Azure-Speicher](../storage/storage-introduction.md).
- Sie müssen, um sicherzustellen, dass die Einhaltung der [Azure-virtuellen Computern Vorkenntnisse](site-recovery-best-practices.md#virtual-machines)virtuellen Computern, die Sie schützen möchten.

### <a name="vmm-prerequisites"></a>VMM erforderliche Komponenten
- Sie benötigen VMM-Server ausgeführt wird, klicken Sie auf System Center 2012 R2.
- Sie benötigen mindestens eine Cloud auf dem Server VMM, die Sie schützen möchten. Die Cloud sollte enthalten:
    - Eine oder mehrere VMM Hostgruppen.
    - Eine oder mehrere Hyper-V-Host-Server oder Cluster in jeder Hostgruppe.
    - Eine oder mehrere virtuelle Computer auf dem Quellserver Hyper-V

### <a name="hyper-v-prerequisites"></a>Voraussetzungen für die Hyper-V

- Die Host Hyper-V Server müssen mindestens ausgeführt werden **Windows Server 2012** mit Hyper-V-Rolle oder **Microsoft Hyper-V Server 2012** und die neuesten Updates installiert haben.
- Wenn Sie Hyper-V in einer Notiz Cluster dieser Bank Cluster ausführen wird nicht automatisch erstellt, wenn Sie einen statischen IP-Adresse-basierten Cluster haben. Sie müssen der Cluster Makler manuell konfigurieren. In den Server-Manager dazu > Failovercluster-Manager, mit dem Cluster verbinden, klicken Sie auf **Rolle konfigurieren** und wählen Sie **Hyper-V Replikat Bank** **Rolle wählen Sie** Bildschirm des Assistenten hohen Verfügbarkeit.
- Alle Hyper-V Hostserver oder Cluster für die Sie zum Schutz verwalten möchten, muss in der Cloud VMM enthalten sein.

### <a name="network-mapping-prerequisites"></a>Voraussetzungen für Netzwerk-Zuordnung
Wenn Sie virtuellen Computern Azure Netzwerk Zuordnung Karten zwischen Netzwerken virtueller Computer, auf dem Server der Quelle VMM schützen und Adressieren Azure Netzwerke, um Folgendes zu aktivieren:

- Allen Computern, die über die gleichen Netzwerk fehlschlägt, können miteinander, eine Verbindung herstellen, unabhängig von der Wiederherstellung, denen planen sie sind.
- Ist ein Netzwerk-Gateway am Ziel Azure Netzwerk einrichten, können virtuellen Computern mit anderen lokalen virtuellen Computern verbinden.
- Wenn Sie nicht Netzwerk Zuordnung nur virtuellen Computern, die nicht konfigurieren über im gleichen Wiederherstellungsplan werden können nach Failover auf Azure miteinander verbunden.

Wenn Sie Netzwerk-Zuordnung bereitstellen möchten benötigen Sie Folgendes:

- Den virtuellen Computern, die Sie auf die Quelle VMM-Server schützen möchten, sollten mit einem virtueller Computer-Netzwerk verbunden werden. Diesem Netzwerk sollte ein logisches Netzwerk verknüpft werden, die mit der Cloud verknüpft ist.
- Ein Azure-Netzwerk mit dem repliziert virtuellen Computern nach Failover verbinden können. Dieses Netzwerk wählen Sie zum Zeitpunkt der Failover. Das Netzwerk sollte in derselben Region als Ihr Abonnement Azure Website Wiederherstellung sein.
- [Weitere](site-recovery-network-mapping.md) Informationen zum Netzwerk-Zuordnung:

###<a name="powershell-prerequisites"></a>PowerShell erforderliche Komponenten
Stellen Sie sicher, dass stehen Ihnen Azure PowerShell erstellt haben. Wenn Sie bereits PowerShell verwenden, müssen Sie auf Version 0.8.10 oder höher. Informationen zum Einrichten von PowerShell finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md). Sobald Sie einrichten und PowerShell konfiguriert haben, können Sie alle verfügbaren Cmdlets für den Dienst anzeigen [können](https://msdn.microsoft.com/library/dn850420.aspx).

Wenn Sie wissen, dass Sie verwenden die Cmdlets, wie etwa wie Parameterwerte, Eingaben und Ausgaben in der Regel in Azure PowerShell verarbeitet werden Tipps, die Ihnen helfen können finden Sie unter [Erste Schritte mit Azure-Cmdlets](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Schritt 1: Festlegen des Abonnements

Führen Sie in der PowerShell dieser Cmdlets ein:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

Ersetzen Sie die Elemente innerhalb der "< >" durch Ihre spezifischen Informationen ein.

## <a name="step-2-create-a-site-recovery-vault"></a>Schritt 2: Erstellen einer Website Wiederherstellung Tresor

Ersetzen Sie in PowerShell die Elemente innerhalb der "< >" durch eigene bestimmte Informationen zu, und führen Sie diese Befehle:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>Schritt 3: Generieren Sie einen Tresor Registrierungsschlüssel

Erstellen Sie einen Registrierungsschlüssel im Tresor aus. Nachdem Sie den Anbieter für Azure Websites Wiederherstellung herunterladen und installieren es auf dem Server VMM, verwenden Sie diesen Schlüssel, um den VMM-Server im Tresor registrieren.

1.  Rufen Sie der Tresor-Einstellungsdatei ab und festlegen Sie den Kontext:

    ```

    $VaultName = "<testvault123>"
    $VaultGeo  = "<Southeast Asia>"
    $OutputPathForSettingsFile = "<c:\>"

    $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

    ```

2.  Festlegen des Kontexts Tresor durch Ausführen der folgenden Befehle:

    ```

    $VaultSettingFilePath = $vaultSetingsFile.FilePath
    $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

    ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Schritt 4: Installieren des Wiederherstellung-Anbieters Azure-Website

1.  Erstellen Sie ein Verzeichnis auf dem Computer VMM indem Sie den folgenden Befehl ausführen:

    ```

    pushd C:\ASR\

    ```

2.  Extrahieren Sie die Dateien mit dem heruntergeladenen Anbieter durch den folgenden Befehl ausführen

    ```

    AzureSiteRecoveryProvider.exe /x:. /q

    ```

3.  Installieren Sie den Anbieter mithilfe der folgenden Befehle:

    ```

    .\SetupDr.exe /i

    ```

    ```

    $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
    do
    {
        $isNotInstalled = $true;
        if(Test-Path $installationRegPath)
        {
            $isNotInstalled = $false;
        }
    }While($isNotInstalled)

    ```

    Warten Sie für die Installation auf Fertig stellen.

4.  Registrieren Sie den Server im Tresor mit dem folgenden Befehl aus:

    ```

    $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
    pushd $BinPath
    $encryptionFilePath = "C:\temp\"
    .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

    ```

## <a name="step-5-create-an-azure-storage-account"></a>Schritt 5: Erstellen Sie ein Konto Azure-Speicher

Wenn Sie ein Azure-Speicher-Konto besitzen, erstellen Sie ein Konto Geo-Replikation aktiviert, indem Sie den folgenden Befehl ausführen:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Beachten Sie, dass das Speicherkonto muss in der gleichen Region wie der Dienst Azure Website Wiederherstellung und mit dem gleichen Abonnement verknüpft werden.


## <a name="step-6-install-the-azure-recovery-services-agent"></a>Schritt 6: Installieren des Wiederherstellung Azure-Agents Services

Installieren Sie aus dem Azure-Portal den Azure Wiederherstellung Services-Agent auf jedem Hyper-V-Host-Server befindet sich in der VMM Wolken, die Sie schützen möchten.

Führen Sie den folgenden Befehl auf allen VMM Hosts aus:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>Schritt 7: Konfigurieren von Cloud Schutz-Einstellungen

1.  Erstellen Sie ein Cloud Schutz-Profil in Azure, indem Sie den folgenden Befehl ausführen:

    ```

    $ReplicationFrequencyInSeconds = "300";
    $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds  $ReplicationFrequencyInSeconds;

    ```

2.  Abrufen eines Containers Schutz durch Ausführen der folgenden Befehle:

    ```

    $PrimaryCloud = "testcloud"
    $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

    ```

3.  Starten Sie die Zuordnung des Containers Schutz mit der cloud

    ```

    $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

    ```

4.  Wenn das Projekt abgeschlossen wurde, führen Sie den folgenden Befehl aus:


        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


5.  Nachdem Sie der Auftrag Verarbeitung abgeschlossen ist, führen Sie den folgenden Befehl aus:


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



Um den Abschluss des Vorgangs zu überprüfen, folgen Sie den Schritten [Monitor Aktivität](#monitor)aus.

## <a name="step-8-configure-network-mapping"></a>Schritt 8: Konfigurieren von Netzwerk-Zuordnung
Vorbemerkung Netzwerk Zuordnung überprüfen, ob virtuellen Computern auf die Quelle VMM-Server mit einem virtuellen Computer-Netzwerk verbunden sind. Darüber hinaus erstellen Sie ein oder mehrere Azure virtuelle Netzwerke. Beachten Sie, dass mehrere virtueller Computer Netzwerke mit einem einzelnen Azure Netzwerk zugeordnet werden können.

Beachten Sie, dass, wenn das Zielnetzwerk verfügt über mehrere Subnetze von diesen Subnetzen verfügt und denselben Namen wie Subnetz Grundlage der Quelle virtuellen Computern befindet, und klicken Sie dann Replikat virtuellen Computers wird mit diesem Ziel Subnetz nach Failover verbunden sein. Wenn Sie kein Ziel Subnetz mit einem übereinstimmenden Namen vorhanden ist, wird mit dem ersten Subnetz im Netzwerk des virtuellen Computers verbunden.

Der erste Befehl ruft Servern für den aktuellen Azure Website Wiederherstellung Tresor. Der Befehl speichert Microsoft Azure-Website wiederherstellen-Servern in der Variablen $Servers Matrix.

    $Servers = Get-AzureSiteRecoveryServer


Der zweite Befehl erhält das Standort Wiederherstellung-Netzwerk für den ersten Server in der Matrix $Servers. Der Befehl speichert die Netzwerke in der Variablen $Networks.


    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

Der dritte Befehl erhält Ihre Azure-Abonnements mithilfe des Cmdlets Get-AzureSubscription, und klicken Sie dann in der Variablen $Subscriptions gespeichert.

    $Subscriptions = Get-AzureSubscription



Der vierte Befehl erhält Azure virtuelle Netzwerke, verwenden das Cmdlet "Get-AzureVNetSite", und klicken Sie dann diesen Wert in der Variablen $AzureVmNetworks.


    $AzureVmNetworks = Get-AzureVNetSite



Das Cmdlet "abgeschlossen" erstellt eine Zuordnung zwischen der primären und Azure-virtuellen Computernetzwerk. Das Cmdlet gibt das primäre Netzwerk als das erste Element der $Networks an. Das Cmdlet gibt ein virtuellen Computernetzwerk als das erste Element der $AzureVmNetworks an, mit deren ID an. Der Befehl schließt Ihrer Azure-Abonnement-ID an.


    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Schritt 9: Aktivieren des Schutzes für virtuellen Computern

Nach dem Server, Wolken und Netzwerke ordnungsgemäß konfiguriert sind, können Sie den Schutz für virtuellen Computern in der Cloud aktivieren. Beachten Sie Folgendes:

Virtuellen Computern müssen [Azure-virtuellen Computern erforderliche Komponenten](site-recovery-best-practices.md#virtual-machines)entsprechen.

Zum Schutz des Betriebssystems und Betriebssystem aktivieren müssen Datenträgereigenschaften des virtuellen Computers festgelegt werden. Beim Erstellen eines virtuellen Computers in VMM mithilfe einer Vorlage virtuellen Computern können Sie die Eigenschaft festlegen. Sie können diese Eigenschaften für vorhandenen virtuellen Computern auch auf den Registerkarten **Allgemein** und **Hardwarekonfiguration** des virtuellen Computereigenschaften festlegen. Wenn Sie keinen dieser Eigenschaften in VMM festlegen werden Sie diese im Portal Azure Website Wiederherstellung konfiguriert sein.



1.  Zum Schutz aktivieren möchten, führen Sie den folgenden Befehl aus, um den Schutz Container zu erhalten:

        $ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName



2. Erhalten Sie die Schutz Entität (virtueller Computer), indem Sie den folgenden Befehl ausführen:


        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



3. Aktivieren Sie die DR für den virtuellen Computer, indem Sie den folgenden Befehl ausführen:



        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity  -Protection Enable -Force



## <a name="test-your-deployment"></a>Testen der bereitstellungs

Zum Ausführen eines Failovers Test für einen einzelnen virtuellen Computer, oder erstellen einen Wiederherstellungsplan aus mehreren virtuellen Computern und Ausführen ein Failovers Test für den Plan, Testen der Bereitstellung. Test Failover simuliert Ihrer Failover und Wiederherstellung Verfahren in einem Netzwerk isoliert. Beachten Sie Folgendes:

- Wenn Sie die Verbindung des virtuellen Computers in Azure mithilfe von Remotedesktop nach dem Failover möchten, aktivieren Sie Remote Desktop-Verbindung des virtuellen Computers vor dem Ausführen des Failovers testen.
- Nach einem Failover verwenden Sie eine öffentliche IP-Adresse Verbindung zu den virtuellen Computern in Azure mithilfe von Remotedesktop. Wenn Sie dies tun möchten, stellen Sie sicher, dass Sie keine Domänenrichtlinien besitzen, die verhindern, dass Sie eine Verbindung zu einer virtuellen Computern mithilfe einer öffentlichen Adresse.

Um den Abschluss des Vorgangs zu überprüfen, folgen Sie den Schritten [Monitor Aktivität](#monitor)aus.

### <a name="create-a-recovery-plan"></a>Erstellen Sie einen Wiederherstellungsplan

1. Erstellen einer XML-Datei als Vorlage für Ihre Wiederherstellungsplan mit den folgenden Daten ein, und speichern Sie es dann als "C:\RPTemplatePath.xml".
2. Ändern der RecoveryPlan Knoten-Id, Name, PrimaryServerId und SecondaryServerId.
3. Ändern Sie den Knoten ProtectionEntity PrimaryProtectionEntityId (Vmid von VMM).
4. Sie können weitere virtuellen Computern durch Hinzufügen von weiteren ProtectionEntity Knoten hinzufügen.



        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"  PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"  SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03- cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"  Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"  After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



4. Füllen Sie die Daten in der Vorlage ein:


        $TemplatePath = "C:\RPTemplatePath.xml";



5. Erstellen der RecoveryPlan:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;



### <a name="run-a-test-failover"></a>Ausführen eines Failovers testen

1.  Rufen Sie das Objekt RecoveryPlan, indem Sie den folgenden Befehl ausführen:

        $RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;


2.  Starten Sie das Test-Failover durch den folgenden Befehl ausführen:


        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


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

[Weitere Informationen finden Sie](https://msdn.microsoft.com/library/dn850420.aspx) Informationen zum Azure Website Wiederherstellung PowerShell-Cmdlets. </a>.
