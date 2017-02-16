<properties
    pageTitle="Installieren von Symantec Endpunkt Schutz auf einen virtuellen | Microsoft Azure"
    description="Informationen Sie zum Installieren und konfigurieren die Erweiterung Symantec Endpunkt Schutz Sicherheit auf einen neuen oder vorhandenen Azure-virtuellen mit dem Bereitstellungsmodell klassischen erstellt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="iainfou"/>

# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>So installieren und Konfigurieren von Symantec Endpunkt Schutz auf einen Windows-virtuellen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

In diesem Artikel wird das Installieren und Konfigurieren von dem Client Symantec Endpunkt Schutz auf vorhandenen virtuellen Computer (virtueller Computer) unter Windows Server veranschaulicht. Dies ist der vollständige Client, wozu auch Dienste wie Virus und Spyware-Schutz, Firewall und Intrusionsprävention. Der Client wird als Erweiterung Sicherheit mithilfe des virtuellen Computer-Agents installiert.

Wenn Sie ein vorhandenes Abonnement von Symantec für eine lokale Lösung verfügen, können Sie es zum Schutz Ihrer Azure-virtuellen Computern verwenden. Wenn Sie einen Kunden nicht noch befinden, können Sie bei einem Testabonnement anmelden. Weitere Informationen zu dieser Lösung, finden Sie unter [Symantec Endpunkt Schutz auf Microsoft Azure-Plattform][Symantec]. Diese Seite enthält Links zu Informationen zur Lizenzierung und Anweisungen zum Installieren des Clients aus, wenn Sie bereits eine Symantec-Kunde sind.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Installieren von Symantec Endpunkt Schutz eines vorhandenen virtuellen Computers

Bevor Sie beginnen, benötigen Sie Folgendes:

- Die Azure PowerShell-Modul, Version 0.8.2 oder höher, auf Ihrem Arbeitscomputer. Sie können die Version von Azure PowerShell, die Sie installiert haben, Überprüfen der **Get-Modul Azure | Tabelle formatieren Version** Befehl. Anweisungen und einen Link auf die neueste Version, finden Sie unter [So installieren und Konfigurieren von Azure PowerShell][PS]. Melden Sie sich bei der Verwendung von Ihrem Abonnement Azure `Add-AzureAccount`.

- Der virtueller Computer-Agent Ausführen des Azure virtuellen Computers.

Überprüfen Sie zunächst, dass die virtuellen Computer Agent bereits auf dem virtuellen Computer installiert ist. Füllen Sie in der Cloud-Dienstnamen und der Name des virtuellen Computers, und führen Sie die folgenden Befehle an ein Administrator Ebene Azure PowerShell-Eingabeaufforderung. Ersetzen Sie alles innerhalb der Anführungszeichen, einschließlich der < und > Zeichen.

> [AZURE.TIP] Wenn Sie die Cloud-Dienst und virtuellen Computernamen nicht kennen, führen Sie **Get-AzureVM** , um die Namen für alle virtuellen Computer in Ihres aktuellen Abonnements aufzulisten.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Wenn Sie **der Befehl **Schreiben-Host** angezeigt wird,**wird der virtuellen Computer-Agent installiert. Wenn **falsch**angezeigt wird, finden Sie unter den Anweisungen und einen Link zur Downloadwebsite im Blog Azure [virtueller Computer-Agents und Erweiterungen - Teil 2]Posten[Agent].

Wenn der Agent virtuellen Computer installiert ist, führen Sie diese Befehle zum Installieren des Symantec Endpunkt Schutz-Agents aus.

    $Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

    Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection -VM $vm | Update-AzureVM

So überprüfen, dass die Symantec Sicherheit Erweiterung installiert wurde und auf dem neuesten Stand ist:

1.  Melden Sie sich am virtuellen Computer an. Anweisungen finden Sie unter [So melden Sie sich bei einem virtuellen Computern ausführen von Windows Server][Logon].
2.  Klicken Sie auf Windows Server 2008 R2, **starten > Symantec Endpunkt Schutz**. Geben Sie für Windows Server 2012 oder Windows Server 2012 R2 vom Startbildschirm **Symantec**, und klicken Sie dann auf **Symantec Endpunkt Schutz**.
3.  Der Registerkarte **Status** des Fensters **Status Symantec Endpunkt Schutz** Updates anwenden, oder starten Sie bei Bedarf erneut.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[So melden Sie sich bei einem virtuellen Computern unter WindowsServer][Logon]

[Azure-virtuellen Computer Extensions und Features][Ext]


<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Portal]: http://manage.windowsazure.com

[Create]: virtual-machines-windows-classic-tutorial.md

[PS]: ../powershell-install-configure.md

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]: virtual-machines-windows-classic-connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493