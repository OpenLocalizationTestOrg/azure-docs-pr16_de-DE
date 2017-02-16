<properties
    pageTitle="Azure-Active Directory-Domänendiensten: Administration Guide | Microsoft Azure"
    description="Teilnehmen an einem Windows-Computer zu einer verwalteten Domäne Azure PowerShell und das Bereitstellungsmodell klassischen verwenden."
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="maheshu"/>


# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>Teilnehmen an einer Windows Server virtuellen Computers zu einer verwalteten Domäne mithilfe der PowerShell

> [AZURE.SELECTOR]
- [Azure klassischen Portal – Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

> [AZURE.IMPORTANT] Azure weist zwei verschiedenen Bereitstellungsmodelle für das Erstellen und Arbeiten mit Ressourcen: [Ressourcenmanager und Classic](../resource-manager-deployment-model.md). Dieser Artikel behandelt das Bereitstellungsmodell klassischen verwenden. Azure Active Directory-Domänendiensten unterstützt das Modell Ressourcenmanager zurzeit nicht.

Diese Schritte zeigen, wie eine Reihe von Azure PowerShell-Befehlen anpassen, die erstellen und einem Windows-basierten Azure-virtuellen Computern mithilfe eines Bausteins Ansatzes vorkonfiguriert. Diese Schritte können Sie ein Windows-basiertem Azure-virtuellen Computern erstellen und es zu einer verwalteten Azure Active Directory-Domänendiensten-Domäne hinzufügen.

Diese Schritte eines ausfüllen-in-the-Leerzeichen Ansatzes zum Erstellen von Azure PowerShell-Befehlssätze. Dieser Ansatz kann hilfreich sein, wenn Sie noch neu bei PowerShell sind oder möchten Sie wissen, welche Werte für die erfolgreiche Konfiguration angeben. Fortgeschrittene PowerShell-Benutzer können die Befehle und Ersetzen durch eigene Werte für die Variablen (die Zeilen, die mit "$" beginnen).

Wenn dies nicht erfolgt noch, führen Sie die Anweisungen in [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Azure PowerShell auf Ihrem lokalen Computer installieren. Öffnen Sie dann eine Windows PowerShell-Eingabeaufforderung aus.

## <a name="step-1-add-your-account"></a>Schritt 1: Fügen Sie Ihres Kontos hinzu

1. PowerShell dazu aufgefordert werden Geben Sie **-AzureAccount hinzufügen** , und klicken Sie auf die **EINGABETASTE**.
2. Geben Sie die e-Mail-Adresse, die mit Ihrem Azure-Abonnement verknüpft ist, und klicken Sie auf **Weiter**.
3. Geben Sie das Kennwort für Ihr Konto ein.
4. Klicken Sie auf **Anmelden**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Schritt 2: Legen Sie Ihr Abonnement und Speicher-Konto

Legen Sie Ihre Azure-Abonnement und Speicher-Konto durch Ausführen der folgenden Befehle in der Windows PowerShell-Befehlszeile. Ersetzen Sie alles innerhalb der Anführungszeichen, einschließlich der Zeichen, die mit den richtigen Namen < und >.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Sie können die richtige Abonnementname aus der Eigenschaft SubscriptionName der Ausgabe des Befehls **Get-AzureSubscription** erhalten. Sie können den richtige Speicher Kontonamen aus die Eigenschaft Beschriftung die Ausgabe des Befehls **Get-AzureStorageAccount** abrufen, nachdem Sie den Befehl **Select-AzureSubscription** ausführen.


## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>Schritt 3: Schrittweise Anleitung - Bereitstellung des virtuellen Computers, und fügen Sie es zur verwalteten Domäne
Hier wird der entsprechende Azure PowerShell-Befehlssatz dieses virtuellen Computers, mit leere Zeilen zwischen jeden Zellenblock, um die Lesbarkeit zu erstellen.

Geben Sie Informationen über die Windows-virtuellen Computern bereitgestellt werden.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

Die InstanceSize Werte für D, DS oder G-Serie virtuellen Computern finden Sie unter [virtuellen Computern und Cloud-Dienst Größen für Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Geben Sie Informationen zur verwalteten Domäne.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Geben Sie den Namen des Cloud-Dienst an.

    $svcname="Contoso100-test"

Geben Sie den Namen des virtuellen Netzwerks, der der virtuellen Computer verknüpft werden sollen. Stellen Sie sicher, dass die verwaltete AAD-DS-Domäne in diesem virtuellen Netzwerk verfügbar ist.

    $vnetname="MyPreviewVnet"

Wählen Sie das Bild virtueller Computer verwendet werden, um die virtuellen Computer bereitstellen.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Konfigurieren Sie den virtuellen Computer - Name des virtuellen Computers festlegen, Instanzgröße und Bild verwendet werden.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Anmeldeinformationen für lokale Administratoren für den virtuellen Computer zu erhalten. Wählen Sie ein sicheres lokale Administratorkennwort aus.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

Stellen Sie die Anmeldeinformationen für ein Benutzerkonto 'AAD DC Administratoren' Gruppe virtueller Computer der verwalteten Domäne hinzufügen. Gehen Sie wie folgt nicht angeben Domänenname – beispielsweise in diesem Beispiel, können wir 'Bob' als Benutzernamen ein.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

Konfigurieren den virtuellen Computer - Domäne Verknüpfung Anforderung & erforderlichen Anmeldeinformationen angeben.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Legen Sie ein Subnetz für den virtuellen Computer an.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Optional: Zeigen Sie auf die IP-Adresse der Domäne. Wenn Sie die IP-Adressen der verwalteten Domäne Azure Active Directory-Domänendiensten werden die DNS-Server für das virtuelle Netzwerk festlegen, ist dieser Schritt nicht erforderlich.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Bereitstellung von nun den Domänenverbund virtuellen Windows-Computer.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>Skript zum Bereitstellen eines Windows virtuellen Computers und es automatisch zu einer verwalteten AAD-Domänendienste-Domäne hinzufügen
Diese PowerShell-Befehlssatz erstellt ein virtuellen Computers für eine Line-of-Business-Server, die:

- Windows Server 2012 R2 Datacenter Bild verwendet.
- Ist eine zusätzliche small virtuellen Computern an.
- Enthält den Namen Contoso-Tests zurück.
- Domäne wird automatisch an die verwaltete contoso100 Domäne hinzugefügt werden.
- Wird im gleichen virtuellen Netzwerk als verwalteten Domäne hinzugefügt.

So sieht das vollständige Beispielskript zum Erstellen der Windows-Computer, und fügen Sie ihn automatisch zur Azure-Active Directory-Domänendiensten verwalteten Domäne aus.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Siehe auch
- [Azure Active Directory-Domänendiensten - Leitfaden für erste Schritte](./active-directory-ds-getting-started.md)

- [Verwalten einer verwalteten Azure Active Directory-Domänendiensten-Domäne](./active-directory-ds-admin-guide-administer-domain.md)
