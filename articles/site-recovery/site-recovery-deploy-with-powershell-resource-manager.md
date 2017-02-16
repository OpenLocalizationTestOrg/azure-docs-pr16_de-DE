<properties
    pageTitle="Schützen Servern in Azure mithilfe von Azure PowerShell Azure Ressourcenmanager | Microsoft Azure"
    description="Automatisieren Sie Schutz der Server in Azure mit Azure Website Wiederherstellung mithilfe von PowerShell und Azure Ressourcenmanager."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>Replikation zwischen lokalen Hyper-V-virtuellen Computern und Azure mithilfe der PowerShell und Azure Ressourcenmanager

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-hyper-v-site-to-azure.md)
- [PowerShell - Ressourcenmanager](site-recovery-deploy-with-powershell-resource-manager.md)
- [Klassische-Portal](site-recovery-hyper-v-site-to-azure-classic.md)



## <a name="overview"></a>(Übersicht)

Azure Website Wiederherstellung beiträgt zu Ihrer Business Continuity- und Disaster Wiederherstellungsstrategie durch Replikation, Failover und Wiederherstellung von virtuellen Computern in einer Reihe von Szenarien für die Bereitstellung orchestriert. Eine vollständige Liste der Szenarien für die Bereitstellung finden Sie unter der [Azure Website Wiederherstellung (Übersicht)](site-recovery-overview.md).

Azure PowerShell ist ein Modul, Cmdlets zum Verwalten von Azure über Windows PowerShell bereitstellt. Sie können zwei Arten von Modulen arbeiten: Modul Azure-Profil oder des Moduls Azure Ressource-Manager.

Website Wiederherstellung PowerShell-Cmdlets, erhältlich Azure PowerShell für Azure Ressourcenmanager, helfen Ihnen schützen und Wiederherstellen von Ihren Servern in Azure.

Dieser Artikel beschreibt, wie Sie mithilfe von Windows PowerShell zusammen mit Azure Ressourcenmanager, Website Wiederherstellung zum Konfigurieren und Server-Schutz in Azure koordinieren bereitstellen. In diesem Artikel verwendeten Beispiel wird gezeigt, wie schützen, über fehl, und Wiederherstellen von virtuellen Computern auf einem Hyper-V-Host auf Azure mithilfe von Azure PowerShell Azure Ressourcenmanager erstellt wird.

> [AZURE.NOTE] Die Website Wiederherstellung PowerShell-Cmdlets aktuell ermöglichen es Ihnen, Folgendes zu konfigurieren: eine virtuellen Computern Manager Website an eine andere, einer Website virtuellen Computern Manager Azure und eine Website Hyper-V Azure.

Sie brauchen eine PowerShell-Experten in diesem Artikel verwendet werden, aber Sie müssen die grundlegende Konzepte, wie Module, Cmdlets und Sitzungen zu verstehen. Weitere Informationen zu Windows PowerShell finden Sie unter [Erste Schritte mit Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).

Sie können auch weitere Informationen zum [Verwenden von Azure PowerShell Azure Ressourcenmanager](../powershell-azure-resource-manager.md).

> [AZURE.NOTE] Microsoft-Partner zur Verfügung, die Teil des Programms Cloud Lösung Provider (CSP) sind können konfigurieren und Verwalten von Schutz ihrer Kunden Server zu ihrer Kunden entsprechenden CSP Abonnements (Abonnements Mandanten).

## <a name="before-you-start"></a>Bevor Sie beginnen

Stellen Sie sicher, dass Sie diese erforderlichen Komponenten angeordnet haben:

- Ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Sie können mit einer [kostenlosen Testversion](https://azure.microsoft.com/pricing/free-trial/)beginnen. Darüber hinaus erhalten Sie Informationen zu [Preise Azure Website Wiederherstellung-Manager](https://azure.microsoft.com/pricing/details/site-recovery/).
- Azure PowerShell 1.0. Weitere Informationen zu dieser Version und wie Sie diese installieren, finden Sie unter [Azure PowerShell 1.0.](https://azure.microsoft.com/)
- Die Module [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) und [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) . Sie können die neuesten Versionen dieser Module aus dem [Katalog PowerShell](https://www.powershellgallery.com/) abrufen.

In diesem Artikel veranschaulicht, wie mithilfe von Azure Powershell Azure Ressourcenmanager konfigurieren und Schutz Ihrer Server verwalten. In diesem Artikel verwendeten Beispiel wird gezeigt, wie Schützen einer virtuellen Computern, die auf einem Hyper-V-Host in Azure ausgeführt werden kann. Die folgenden Vorkenntnisse sind in diesem Beispiel wird. Umfassendere mehrere Anforderungen für die verschiedenen Szenarios für die Website Wiederherstellung finden Sie in der Dokumentation in Verbindung mit diesem Szenario Gruppenrichtlinien.

- Ein Hyper-V-Host unter Windows Server 2012 R2 oder Microsoft Hyper-V Server 2012 R2, die eine oder mehrere virtuelle Computer enthält.
- Hyper-V-Servern verbunden mit dem Internet, direkt oder über einen Proxy.
- Den virtuellen Computern, die Sie schützen möchten, sollte mit [virtuellen Computern erforderliche Komponenten](site-recovery-best-practices.md#virtual-machines)entsprechen.


## <a name="step-1-sign-in-to-your-azure-account"></a>Schritt 1: Melden Sie sich bei Ihrem Konto Azure


1. Öffnen Sie eine PowerShell-Konsole, und führen Sie diesen Befehl zum Anmelden bei Ihrem Konto Azure. Das Cmdlet öffnet eine Webseite anzuzeigen, die Sie für Ihr Kontoanmeldeinformationen aufgefordert werden.

        Login-AzureRmAccount

    Alternativ können Sie Ihre Kontoanmeldeinformationen ein als eines Parameters zur einschließen der `Login-AzureRmAccount` -Cmdlets mithilfe der `-Credential` Parameter.

    Wenn Sie CSP Partner für einen Mandanten arbeiten sind, geben Sie den Kunden als einen Mandanten, mit ihren primären Domänennamen TenantID oder den Mandanten.

        Login-AzureRmAccount -Tenant "fabrikam.com"

2. Ein Konto kann über mehrere Abonnements, verfügen, damit Sie das Abonnement zuordnen soll, die, das Sie mit dem Konto verwenden möchten.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName

3.  Stellen Sie sicher, dass Ihr Abonnement registriert ist, um die Azure-Anbieter für Wiederherstellung Services und Website-Wiederherstellung verwenden, mithilfe der folgenden Befehle:

    - `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
    -  `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

    In der Ausgabe dieser Befehle Wenn die **RegistrationState** **registriert festgelegt ist**, können Sie mit Schritt2 fortfahren. Wenn dies nicht der Fall ist, sollten Sie fehlenden Provider in Ihrem Abonnement registrieren.

    Um den Azure-Anbieter für Website Wiederherstellung und Wiederherstellung-Dienste zu registrieren, führen Sie folgende Befehle aus:

        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

    Stellen Sie sicher, dass der Anbieter erfolgreich registriert, mithilfe der folgenden Befehle: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` und `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.



## <a name="step-2-set-up-the-recovery-services-vault"></a>Schritt 2: Einrichten des Wiederherstellung Services Tresors

1. Erstellen Sie eine Ressourcengruppe Azure Ressourcenmanager, in der werden Sie erstellen den Tresor, oder verwenden eine vorhandene Ressourcengruppe. Sie können eine neue Ressourcengruppe erstellen, indem Sie mit den folgenden Befehl aus:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    Stelle, an der die $ResourceGroupName-Variable enthält den Namen der Ressourcengruppe, die Sie erstellen möchten, und die Variable $Geo enthält den Azure Bereich, in dem die Ressourcengruppe (beispielsweise "Brasilien Süd") zu erstellen.

    Sie können eine Liste der Ressourcengruppen im Rahmen Ihres Abonnements erhalten, indem der `Get-AzureRmResourceGroup` Cmdlet.

2. Erstellen einer neuen Azure Wiederherstellung Services Tresor wie folgt aus:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Sie können eine Liste der vorhandenen +++ abrufen, indem Sie mit der `Get-AzureRmRecoveryServicesVault` Cmdlet.

> [AZURE.NOTE] Wenn Sie Vorgänge auf Website Wiederherstellung Depots erstellt, über die klassischen Portal oder das Azure Service Management PowerShell-Modul ausführen möchten, können Sie eine Liste der solche +++ abrufen, mithilfe der `Get-AzureRmSiteRecoveryVault` Cmdlet. Erstellen Sie eine neue Wiederherstellung Services Tresor für alle neuen Vorgänge. Zuvor erstellten Website Wiederherstellung Depots werden unterstützt, aber Sie verfügen nicht über die neuesten Features.

## <a name="step-3-set-the-recovery-services-vault-context"></a>Schritt 3: Einrichten des Wiederherstellung Services Tresor Kontexts

1.  Legen Sie im Kontext Tresor, indem Sie den folgenden Befehl ausführen:

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a>Schritt 4: Erstellen einer Website Hyper-V und einen neuen Tresor Registrierungsschlüssel für die Website generieren.

1. Erstellen einer neuen Hyper-V-Website wie folgt aus:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    Dieses Cmdlet startet einen Auftrag Website Wiederherstellung, um die Website zu erstellen, und gibt eine Website Wiederherstellung Job-Objekts. Warten Sie ausgefüllt, und überprüfen Sie, ob er den Auftrag wurde erfolgreich abgeschlossen.

    Sie können den Job-Objekts abrufen und überprüfen den aktuellen Status des Projekts, damit auch mithilfe des Cmdlets Get-AzureRmSiteRecoveryJob.
2. Generieren Sie, und Laden Sie einen Registrierungsschlüssel für die Website, wie folgt aus:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    Kopieren Sie die heruntergeladene Taste an den Hyper-V-Host ein. Benötigen Sie die EINGABETASTE, um zu der Website der Hyper-V-Host registrieren.

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>Schritt 5: Installieren der Azure-Website Wiederherstellung Anbieter und Azure Wiederherstellung Services-Agent auf dem Host von Hyper-V

1. Laden Sie das Installationsprogramm für die neueste Version von den Anbieter von [Microsoft](https://aka.ms/downloaddra).

2. Ausführen des Installationsprogramms auf dem Host von Hyper-V und am Ende der Installation fahren Sie mit der Registrierungsschritt.

3. Wenn Sie dazu aufgefordert werden, geben Sie heruntergeladene Website Registrierungsschlüssel und Registrierung abschließen des Hosts Hyper-V zu der Website.

4. Stellen Sie sicher, dass der Hyper-V-Host zu der Website mit dem folgenden Befehl registriert ist:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a>Schritt 6: Erstellen einer Replikationsrichtlinie und den Schutz Container zuordnen

1. Erstellen einer Replikationsrichtlinie wie folgt aus:

        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Überprüfen Sie die zurückgegebenen um sicherzustellen, dass das Erstellen von Richtlinien Replikation erfolgreich ist.

    >[AZURE.IMPORTANT] Das angegebene Speicherkonto sollten in der gleichen Azure Region als Ihrem Tresor Wiederherstellung Services und sollte Geo-Replikation aktiviert haben.
    >
    > - Wenn das angegebene Wiederherstellung Speicherkonto vom Typ Azure-Speicher (klassisch) ist, Failover der geschützten Computer Wiederherstellen des Computers Azure IaaS (klassisch).
    > - Wenn das angegebene Wiederherstellung Speicherkonto vom Typ Azure-Speicher (Azure Ressourcenmanager) ist, Failover der geschützten Computer Wiederherstellen des Computers Azure IaaS (Azure Ressourcenmanager).

2. Erhalten Sie den Schutz Container entspricht, zu der Website, wie folgt:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3.  Beginnen Sie die Zuordnung des Containers Schutz mit der Replikationsrichtlinie wie folgt ein:

        $Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName
        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer

    Warten Sie für die Zuordnung Auftrag abgeschlossen, und stellen Sie sicher, dass sie erfolgreich abgeschlossen wurde.

##<a name="step-7-enable-protection-for-virtual-machines"></a>Schritt 7: Aktivieren des Schutzes für virtuellen Computern

1. Abrufen der Schutz Entität entspricht dem virtuellen Computer zu schützen, wie folgt:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

2. Starten des virtuellen Computers, wie folgt geschützt:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

    >[AZURE.IMPORTANT] Das angegebene Speicherkonto sollten in der gleichen Azure Region als Ihrem Tresor Wiederherstellung Services und sollte Geo-Replikation aktiviert haben.
    >
    > - Wenn das angegebene Wiederherstellung Speicherkonto vom Typ Azure-Speicher (klassisch) ist, Failover der geschützten Computer Wiederherstellen des Computers Azure IaaS (klassisch).
    > - Wenn das angegebene Wiederherstellung Speicherkonto vom Typ Azure-Speicher (Azure Ressourcenmanager) ist, Failover der geschützten Computer Wiederherstellen des Computers Azure IaaS (Azure Ressourcenmanager).

    > Wenn der virtuellen Computer, die Sie schützen möchten mehr als einen Datenträger beigefügt verfügt, geben Sie den Datenträger Betriebssystem mithilfe des Parameters *OSDiskName* ein.

3. Warten Sie den virtuellen Computern zum Erreichen eines geschützten Status nach der anfänglichen Replikation aus. Dies kann abhängig von Faktoren wie der Datenmenge repliziert werden und die übergeordneten Bandbreite zum Azure eine Weile dauern. Die Job-Status und StateDescription werden wie folgt berechnet, bei dem virtuellen Computer erreichen eines geschützten Status aktualisiert.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed

4. Aktualisieren Sie Wiederherstellung Eigenschaften, wie die Rolle virtueller Speicher und die Azure Netzwerk des virtuellen Computers Benutzeroberfläche Karten die bei einem Failover anfügen.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob


        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Schritt 8: Ausführen ein Failovers testen

1. Führen Sie einen Test Failover Auftrag, wie folgt aus:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id

2. Stellen Sie sicher, dass der Test virtueller Computer in Azure erstellt wird. (Der Test-Failover-Auftrag wird nach dem Erstellen des Tests virtueller Computer in Azure angehalten. Auftragsabschluss durch den erstellten Artefakte beim Fortsetzen der Auftrags bereinigen, wie im nächsten Schritt dargestellt.)

3. Führen Sie das Failover Test wie folgt:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob


##<a name="next-steps"></a>Nächste Schritte

[Weitere Informationen finden Sie](https://msdn.microsoft.com/library/azure/mt637930.aspx) Informationen zum Azure Website Wiederherstellung mit Azure Ressourcenmanager PowerShell-Cmdlets.
