<properties
   pageTitle="Verwalten von reverse DNS-Einträge für Ihre Azure Dienstleistungen mithilfe der PowerShell | Microsoft Azure"
   description="Zum Verwalten von reverse-DNS-Einträge oder PTR-Einträge für Azure Services mithilfe der PowerShell in Ressourcenmanager"
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-using-azure-powershell"></a>Zum Verwalten von reverse DNS-Einträge für Ihre Azure Dienstleistungen mithilfe der PowerShell Azure

[AZURE.INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](dns-reverse-dns-record-operations-classic-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Überprüfung von reverse-DNS-Einträgen
Um sicherzustellen, dass eine Drittanbieter reverse-DNS-Einträge zu Ihren Domänen DNS-Zuordnung erstellen können, lässt Azure nur die Erstellung eines reverse DNS-Eintrags, der eine der folgenden wahr ist:

- Die "ReverseFqdn" ist identisch mit den "Fqdn" für die öffentliche IP-Adresse Ressource für die sie angegeben wurde, oder "Fqdn" für eine öffentliche IP-Adresse innerhalb des gleichen Abonnements z. B., "ReverseFqdn" ist "contosoapp1.northus.cloudapp.azure.com.".

- Die "ReverseFqdn" aufgelöst weiterleiten, den Namen oder die IP-Adresse des die öffentliche IP-Adresse aus, für die sie angegeben wurde oder eine öffentliche IP-Adresse "Fqdn" oder die IP-innerhalb des gleichen Abonnements z. B. "ReverseFqdn", "app1.contoso.com." ist welche einen CName-Alias für "contosoapp1.northus.cloudapp.azure.com." ist

Validierungen sind nur ausgeführt werden, wenn die reverse-DNS-Eigenschaft für eine öffentliche IP-Adresse festlegen oder geändert wird. Periodische erneute Validierung wird nicht durchgeführt.

## <a name="add-reverse-dns-to-existing-public-ip-addresses"></a>Hinzufügen von reverse-DNS-zu vorhandenen öffentlichen IP-Adressen
Sie können eine vorhandene öffentliche IP-Adresse mit dem Cmdlet "Set-AzureRmPublicIpAddress" reverse-DNS-hinzufügen:

    PS C:\> $pip = Get-AzureRmPublicIpAddress -Name PublicIP -ResourceGroupName NRP-DemoRG-PS
    PS C:\> $pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
    PS C:\> Set-AzureRmPublicIpAddress -PublicIpAddress $pip

Wenn Sie reverse-DNS-mit einer vorhandenen öffentlichen IP-Adresse hinzufügen, die bereits einen Namen für die DNS-Einträge nicht möchten, müssen Sie auch einen DNS-Namen angeben. Sie können dies mit dem Cmdlet "Set-AzureRmPublicIpAddress" erreichen hinzufügen:

    PS C:\> $pip = Get-AzureRmPublicIpAddress -Name PublicIP -ResourceGroupName NRP-DemoRG-PS
    PS C:\> $pip.DnsSettings = New-Object -TypeName Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings
    PS C:\> $pip.DnsSettings.DomainNameLabel = "contosoapp1"
    PS C:\> $pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
    PS C:\> Set-AzureRmPublicIpAddress -PublicIpAddress $pip

## <a name="create-a-public-ip-address-with-reverse-dns"></a>Erstellen Sie eine öffentliche IP-Adresse mit reverse-DNS-
Sie können eine neue öffentliche IP-Adresse mit der angegebenen mithilfe des Cmdlets "New-AzureRmPublicIpAddress" reverse DNS-Eigenschaft hinzufügen:

    PS C:\> New-AzureRmPublicIpAddress -Name PublicIP2 -ResourceGroupName NRP-DemoRG-PS -Location WestUS -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."

## <a name="view-reverse-dns-for-existing-public-ip-addresses"></a>Anzeigen von reverse DNS für vorhandene öffentliche IP-Adressen
Sie können den konfigurierten Wert für eine vorhandene öffentliche IP-Adresse verwenden das Cmdlet "Get-AzureRmPublicIpAddress" anzeigen:

    PS C:\> Get-AzureRmPublicIpAddress -Name PublicIP2 -ResourceGroupName NRP-DemoRG-PS

## <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Entfernen von reverse-DNS-aus vorhandenen öffentlichen IP-Adressen
Sie können eine reverse-DNS-Eigenschaft aus einer vorhandenen öffentlichen IP-Adresse mit dem Cmdlet "Set-AzureRmPublicIpAddress" entfernen. Dies geschieht, indem Sie den Wert der ReverseFqdn-Eigenschaft auf leere:

    PS C:\> $pip = Get-AzureRmPublicIpAddress -Name PublicIP -ResourceGroupName NRP-DemoRG-PS
    PS C:\> $pip.DnsSettings.ReverseFqdn = ""
    PS C:\> Set-AzureRmPublicIpAddress -PublicIpAddress $pip


[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-arm-include.md)]
