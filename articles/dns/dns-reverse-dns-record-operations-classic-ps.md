<properties
   pageTitle="Verwalten von reverse DNS-Einträge für Ihre Azure (klassisch) Dienstleistungen mithilfe der PowerShell | Microsoft Azure"
   description="Verwalten von reverse-DNS-Einträge oder PTR-Einträge für Azure Services mithilfe der PowerShell im Bereitstellungsmodell klassischen von. "
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-classic-using-azure-powershell"></a>Zum Verwalten von reverse DNS-Einträge für Ihre Azure mithilfe der PowerShell Azure Dienstleistungen (klassisch)

[AZURE.INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [Führen Sie diese Schritte aus, die mithilfe des Modells Ressourcenmanager](dns-reverse-dns-record-operations-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Überprüfung von reverse-DNS-Einträgen
Um sicherzustellen, dass eine Drittanbieter reverse-DNS-Einträge zu Ihren Domänen DNS-Zuordnung erstellen können, lässt Azure nur die Erstellung eines reverse DNS-Eintrags, der eine der folgenden wahr ist:

- Die Umkehrung DNS FQDN ist der Name des Cloud-Dienst für die sie angegeben wurde, oder einen beliebigen Namen Cloud-Dienst innerhalb des gleichen Abonnements z. B., reverse-DNS-"contosoapp1.cloudapp.net.".
- Weiterleiten der reverse DNS FQDN aufgelöst, den Namen oder die IP-Adresse des Cloud-Dienst für die angegeben wurde, oder auf einen beliebigen Namen Cloud-Dienst oder IP-innerhalb des gleichen Abonnements z. B. reverse-DNS-"app1.contoso.com." ist welche einen CName-Alias für contosoapp1.cloudapp.net ist.

Validierungen sind nur ausgeführt werden, wenn die reverse-DNS-Eigenschaft für einen Cloud-Dienst festlegen oder geändert wird. Periodische erneute Validierung wird nicht durchgeführt.

## <a name="add-reverse-dns-to-existing-cloud-services"></a>Hinzufügen von reverse-DNS-Einträge zu vorhandenen Cloud Services
Sie können einer vorhandenen Cloud-Dienst verwenden das Cmdlet "Set-AzureService" einen reverse-DNS-Eintrag hinzufügen:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="create-a-cloud-service-with-reverse-dns"></a>Erstellen Sie einen Cloud-Dienst mit reverse-DNS-
Mit der reverse DNS-Eigenschaft angegeben haben, verwenden das Cmdlet "Set-AzureService" können Sie einen neuen Cloud-Dienst hinzufügen:

    PS C:\> New-AzureService –ServiceName “contosoapp1” –Location “West US” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="view-reverse-dns-for-existing-cloud-services"></a>Ansicht reverse-DNS-für vorhandene Cloud Services
Sie können den konfigurierten Wert für einen vorhandenen Cloud-Dienst verwenden das Cmdlet "Get-AzureService" anzeigen:

    PS C:\> Get-AzureService "contosoapp1"

## <a name="remove-reverse-dns-from-existing-cloud-services"></a>Entfernen von reverse-DNS-aus vorhandenen Cloud Services
Sie können eine reverse-DNS-Eigenschaft aus einer vorhandenen Cloud-Dienst verwenden das Cmdlet "Set-AzureService" entfernen. Dies geschieht, indem Sie den Wert der reverse DNS-Eigenschaft auf leere festlegen:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-asm-include.md)]
