<properties
   pageTitle="Übersicht über Azure DNS | Microsoft Azure"
   description="Übersicht über die Hostingdienste auf Microsoft Azure Azure-DNS. Hosten Sie Ihrer Domäne auf Microsoft Azure."
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="azure-dns-overview"></a>Azure DNS (Übersicht)


Das Domain Name System oder DNS, zum Übersetzen von verantwortlich ist (oder beheben) eine Website oder einen bestimmten Dienst Namen seiner IP-Adresse. Azure DNS ist ein Hostdienst für DNS-Domänen, mit einer namensauflösung von mithilfe von Microsoft Azure Infrastruktur bereitstellen. Durch das Hosten von Domänen in Azure, können Sie Ihre DNS-Einträge verwenden dieselben Anmeldeinformationen, APIs, Tools und Abrechnung als anderer Azure Dienste verwalten.


DNS-Domänen in Azure DNS werden in den Azure globale Netzwerk der DNS-Namenserver gehostet werden. Wir verwenden Anycast-Netzwerke, damit jeder DNS-Abfrage durch den nächsten verfügbaren DNS-Server beantwortet wird. Dies stellt sowohl schnelle Leistung und hohe Verfügbarkeit für Ihre Domäne aus.

Der Azure DNS-Dienst basiert auf Azure Ressource Manager (Cloud). Als solche können die Vorteile von Cloud-Features wie rollenbasierte Access-Steuerelement, Überwachungsprotokolle und Ressourcen sperren. Ihre Domänen und Einträge können über Azure-Portal, Azure-PowerShell-Cmdlets und die Plattformen Azure CLI verwaltet werden. Mit dem Dienst über den REST-API und SDKs können mit Anforderung der Automatisches DNS Management Applications integrieren.

Azure DNS unterstützt erwerben von Domänennamen derzeit nicht. Wenn Sie Domänen erwerben möchten, müssen Sie eine Drittanbieter-Domänennamen-Registrierungsstelle verwenden. Die Registrierungsstelle wird in der Regel eine kleine jährliche Gebühr. Die Domänen können dann in Azure DNS für die Verwaltung von DNS-Einträgen gehostet werden. Details finden Sie unter [Stellvertretung einer Domäne zu Azure DNS](dns-domain-delegation.md) .


## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer DNS-zone](dns-getstarted-create-dnszone-portal.md)





