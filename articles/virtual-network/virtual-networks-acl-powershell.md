<properties
   pageTitle="So Access Control Lists (ACLs) für Endpunkte verwalten, indem Sie mithilfe der PowerShell"
   description="Informationen Sie zum Verwalten von ACLs mit PowerShell"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-manage-access-control-lists-acls-for-endpoints-by-using-powershell"></a>So Access Control Lists (ACLs) für Endpunkte verwalten, indem Sie mithilfe der PowerShell

Sie können erstellen und Verwalten von Network Access Control Lists (ACLs) für Endpunkte mithilfe der PowerShell Azure oder im Verwaltungsportal. In diesem Thema finden Sie Verfahren für häufige Aufgaben ACL, die Sie mithilfe der PowerShell ausführen können. Die Liste der Azure-PowerShell-Cmdlets finden Sie unter [Cmdlets für die Azure-Verwaltung](http://go.microsoft.com/fwlink/?LinkId=317721). Weitere Informationen zu ACLs finden Sie unter [was eine Network Access Control (ACL) ist?](virtual-networks-acl.md). Wenn Sie Ihre ACLs mithilfe der Verwaltungsportal verwalten möchten, finden Sie unter [Festlegen von Endpunkte eines virtuellen Computers](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Verwalten von Netzwerk-Zugriffssteuerungslisten mithilfe von Azure PowerShell

Können Azure PowerShell-Cmdlets zu erstellen, entfernen und konfigurieren (festlegen) Network Access Control Lists (ACLs). Wir haben ein paar Beispiele für einige der Methoden enthalten, die Sie konfigurieren können, eine ACL mithilfe der PowerShell.

Um eine vollständige Liste der ACL PowerShell-Cmdlets abrufen möchten, können Sie eine der folgenden Aktionen aus:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Erstellen Sie eine ACL Netzwerk mit Regeln, die von einem remote-Subnetz Zugriff zulassen

Im folgenden Beispiel wird eine Möglichkeit, eine neue ACL erstellen, die Regeln enthält. Diese ACL wird dann an einen Endpunkt virtuellen Computern angewendet. Die ACL Regeln im folgenden Beispiel werden Access aus einem remote-Subnetz zulassen. Öffnen Sie zum Erstellen einer neuen Netzwerk ACL mit Genehmigung Regeln für ein remote Subnetz eine Azure PowerShell ISE aus. Kopieren Sie und fügen Sie das Skript unter das Skript mit Ihrer eigenen Werten konfigurieren, und führen Sie das Skript.

1. Erstellen Sie das neue Netzwerk ACL Objekt ein.

        $acl1 = New-AzureAclConfig

1. Legen Sie eine Regel, die aus einem remote-Subnetz Zugriff erlaubt. Im folgenden Beispiel legen Sie Regel *100* (in der Regel 200 und höherer Priorität hat), der remote-Subnetz *10.0.0.0/8* auf den Endpunkt des virtuellen Computers zugreifen dürfen. Ersetzen Sie die Werte mit Ihren eigenen Konfiguration Anforderungen an. Der Name "SharePoint ACL Config" sollte mit dem Anzeigenamen ersetzt werden, die Sie mit dieser Regel anrufen möchten.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"

1. Wiederholen Sie nach weiteren Regeln das Cmdlet, ersetzen die Werte mit Ihren eigenen Anforderungen Konfiguration aus. Achten Sie darauf, um die Regel zu ändern Zahl Reihenfolge entsprechend die Reihenfolge, in der die Regeln angewendet werden sollen. Die Anzahl der untere Regel Vorrang vor der höheren Nummer.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"

1. Als Nächstes können entweder erstellen Sie einen neuen Endpunkt (hinzufügen) oder die ACL für einen vorhandenen Endpunkt (Gruppe) festlegen. In diesem Beispiel werden wir einen neuen Namen "Web" virtuellen Computern-Endpunkt hinzufügen und aktualisieren den Endpunkt virtuellen Computern mit den Einstellungen ACL.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM

1. Als Nächstes kombinieren Sie die Cmdlets, und führen Sie das Skript. In diesem Beispiel würde die kombinierte Cmdlets wie folgt aussehen:

        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Entfernen einer Netzwerk ACL-Regel, die aus einem remote-Subnetz Zugriff erlaubt

Im folgenden Beispiel wird eine Möglichkeit, eine Netzwerk ACL Regel zu entfernen.  Öffnen Sie zum Entfernen einer Regel Netzwerk ACL mit Genehmigung Regeln für ein remote Subnetz eine Azure PowerShell ISE aus. Kopieren Sie und fügen Sie das Skript unter das Skript mit Ihrer eigenen Werten konfigurieren, und führen Sie das Skript.

1. Erste Schritt besteht im Netzwerk ACL Objekt für den Endpunkt des virtuellen Computers zu erhalten. Sie erhalten die Regel ACL entfernen. In diesem Fall werden wir sie durch die Regel-ID entfernen Dadurch werden nur die Regel-ID 0 aus der Zugriffssteuerungsliste entfernt. Es wird nicht das Objekt ACL aus den Endpunkt virtuellen Computern entfernt.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1

1. Als Nächstes müssen Sie das Objekt Netzwerk ACL an den Endpunkt virtuellen Computern anwenden und des virtuellen Computers zu aktualisieren.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Entfernen einer ACL Netzwerk von einem Endpunkt virtuellen Computern

In bestimmten Szenarien möglicherweise Sie ein Objekt Netzwerk ACL von einem Endpunkt virtuellen Computern entfernen möchten. Öffnen Sie hierzu eine Azure PowerShell ISE aus. Kopieren Sie und fügen Sie das Skript unter das Skript mit Ihrer eigenen Werten konfigurieren, und führen Sie das Skript.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Nächste Schritte

[Was ist eine Network Access Control (ACL)?](virtual-networks-acl.md)
