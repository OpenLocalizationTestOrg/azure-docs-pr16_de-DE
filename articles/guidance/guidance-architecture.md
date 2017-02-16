
<properties
   pageTitle="Azure Anleitungen | Muster und Methoden | Microsoft Azure"
   description="Azure Bezug Architekturen"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="christb"/>

# <a name="azure-reference-architectures"></a>Azure Bezug Architekturen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

_Diesen Inhalt befindet sich in der aktiven Entwicklung. Es ist heute, damit wir es für eine Vorschau verfügbar machen sind hilfreich. Wir schätzen Ihr Feedback ein._

Unsere Bezug Architekturen werden nach Szenario mit mehreren verknüpften Architekturen gruppiert werden angeordnet.
Jede einzelne Architektur bietet empfohlene Vorgehensweisen und Schritte sowie eine ausführbare Komponente, die die Empfehlungen enthält.
Viele der Architekturen sind schrittweisen; Erstellen auf vorherige Architekturen, die weniger Anforderungen aufweisen.

## <a name="designing-your-infrastructure-for-resiliency"></a>Entwerfen der Infrastruktur für Stabilität

Dieser Reihe mit empfohlenen Methoden für optimale virtueller Computer Konfiguration beginnt und endet mit einer Bereitstellung mit mehreren Region mit Failover.

- [Ein virtueller Computer von Windows Azure ausgeführt](guidance-compute-single-vm.md)
- [Ein Linux virtueller Computer ausgeführte auf Windows Azure](guidance-compute-single-vm-linux.md)
- [Ausführen von mehreren virtuellen Computern für Skalierbarkeit und Verfügbarkeit](guidance-compute-multi-vm.md)
- [Ausführen von Windows virtuellen Computern für eine mehrstufige Architektur](guidance-compute-n-tier-vm.md)
- [Ausführen von Linux virtuellen Computern für eine mehrstufige Architektur](guidance-compute-n-tier-vm-linux.md)
- [Ausführen von Windows virtuellen Computern in mehreren Regionen hohen Verfügbarkeit](guidance-compute-multiple-datacenters.md)
- [Ausführen von Linux virtuellen Computern in mehreren Regionen hohen Verfügbarkeit](guidance-compute-multiple-datacenters-linux.md)

## <a name="connecting-your-on-premises-network-to-azure"></a>Verbinden von Ihrem lokalen Netzwerk mit Azure

Dieser Reihe wird gestartet, indem die Verfahren zum Verbinden Ihrer vorhandenen Netzwerks in Azure vorgeführt. Dann wird die Größe des Deckblatt Anforderungen für Verfügbarkeit und Sicherheit.

- [Implementieren einer Hybrid Netzwerkarchitektur mit Azure und lokalen VPN](guidance-hybrid-network-vpn.md)
- [Implementieren einer Hybrid Netzwerkarchitektur mit Azure ExpressRoute](guidance-hybrid-network-expressroute.md)
- [Implementieren einer hoch verfügbaren Hybrid Netzwerkarchitektur](guidance-hybrid-network-expressroute-vpn-failover.md)

## <a name="securing-your-hybrid-network"></a>Schützen des Netzwerks hybrid

Dieser Reihe behandelt bewährte Methoden zum Erstellen von DMZ in Azure sichere Verbindungen aus Ihrem lokalen Datacenter und im Internet stammen.

- [Implementierung einer zwischen Azure und Ihrem lokalen datacenter](guidance-iaas-ra-secure-vnet-hybrid.md)
- [Implementierung einer zwischen Azure und im Internet](guidance-iaas-ra-secure-vnet-dmz.md)

## <a name="providing-identity-services"></a>Bereitstellen von Diensten Identität

Dieser Reihe startet vorgeführt, wie mit Azure Active Directory Benutzerauthentifizierung in Azure bereitstellen. Dann wird die Größe des Deckblatt komplexer Szenarien Erweitern Ihrer Infrastruktur addiert in Azure und ADFS für Delegierung verwenden.

- [Implementieren der Azure-Active Directory](./guidance-identity-aad.md)
- [Erweitern Sie in Azure Active Directory-Verzeichnisdienst (ADDS)](./guidance-identity-adds-extend-domain.md)
- [Erstellen einer Ressourcengesamtstruktur Active Directory Directory Services (ADDS) in Azure](./guidance-identity-adds-resource-forest.md)
- [Implementierung von Active Directory Federation Services (ADFS) in Azure](./guidance-identity-adfs.md)

## <a name="architecting-scalable-web-application-using-azure-paas"></a>Skalierbare Webanwendung mit Azure PaaS Architektur

Dieser Reihe behandelt Empfehlungen für bauen skalierbare und hochgradig verfügbaren Web apps. 

- [Grundlegende Webanwendung](guidance-web-apps-basic.md)
- [Verbessern der Skalierbarkeit in einer Webanwendung](guidance-web-apps-scalability.md)
- [Webanwendung mit hoher Verfügbarkeit](guidance-web-apps-multi-region.md)
