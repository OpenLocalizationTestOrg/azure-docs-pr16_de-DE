<properties
   pageTitle="Verwalten von reverse DNS-Einträge für Ihre Azure Dienstleistungen mit Azure CLI | Microsoft Azure"
   description="Zum Verwalten von reverse-DNS-Einträge oder PTR-Einträge bei Azure-Diensten mit Azure CLI in Ressourcenmanager"
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

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-using-the-azure-cli"></a>Zum Verwalten von reverse DNS-Einträge für Ihre Verwendung der CLI Azure Azure-Dienste

[AZURE.INCLUDE [DNS-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
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
Sie können eine vorhandene öffentliche IP-Adresse mithilfe der öffentlichen Ip-Netzwerk Azure reverse-DNS-hinzufügen:

    azure network public-ip set -n PublicIp -g NRP-DemoRG-PS -f contosoapp1.westus.cloudapp.azure.com.

Wenn Sie reverse-DNS-mit einer vorhandenen öffentlichen IP-Adresse hinzufügen, die bereits einen Namen für die DNS-Einträge nicht möchten, müssen Sie auch einen DNS-Namen angeben. Sie können dies mithilfe der öffentlichen Ip-Azure Netzwerk erreichen hinzufügen:

    azure network public-ip set -n PublicIp -g NRP-DemoRG-PS -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.

## <a name="create-a-public-ip-address-with-reverse-dns"></a>Erstellen Sie eine öffentliche IP-Adresse mit reverse-DNS-
Sie können hinzufügen, erstellen eine neue öffentliche IP-Adresse mit der reverse DNS-Eigenschaft mithilfe der Azure Netzwerk öffentlichen Ip-angegeben:

    azure network public-ip create -n PublicIp3 -g NRP-DemoRG-PS -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.

## <a name="view-reverse-dns-for-existing-public-ip-addresses"></a>Anzeigen von reverse DNS für vorhandene öffentliche IP-Adressen
Sie können den konfigurierten Wert für eine vorhandene öffentliche IP-Adresse mithilfe des öffentlichen Ip-Felds Azure Netzwerk anzeigen:

    azure network public-ip show -n PublicIp3 -g NRP-DemoRG-PS

## <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Entfernen von reverse-DNS-aus vorhandenen öffentlichen IP-Adressen
Sie können eine reverse-DNS-Eigenschaft aus einer vorhandenen öffentlichen IP-Adresse mit Azure Netzwerk öffentlichen Ip-Satz entfernen. Dies geschieht, indem Sie den Wert der ReverseFqdn-Eigenschaft auf leere:

    azure network public-ip set -n PublicIp3 -g NRP-DemoRG-PS –f “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-arm-include.md)]
