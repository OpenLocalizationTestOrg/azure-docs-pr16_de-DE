<properties
   pageTitle="Erforderliche Komponenten für ExpressRoute Annahme | Microsoft Azure"
   description="Diese Seite enthält eine Liste der Anforderungen erfüllt sein, bevor Sie eine Verbindung Azure ExpressRoute bestellen können."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>


# <a name="expressroute-prerequisites--checklist"></a>Voraussetzungen für die ExpressRoute und Checkliste  

Die Verbindung zu Microsoft-Cloud-Diensten mit ExpressRoute müssen Sie überprüfen, dass die folgenden Anforderungen aufgeführt, die in den folgenden Abschnitten erfüllt sind.

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Azure-Konto

- Ein gültiges und aktives Microsoft Azure-Konto. Dies ist erforderlich, die Verbindung ExpressRoute einrichten. Schaltkreise ExpressRoute handelt es sich um Ressourcen in Azure-Abonnements. Ein Azure-Abonnement ist eine Vorbedingung, auch wenn Connectivity nicht Azure Microsoft - Cloud-Diensten, wie Office 365-Diensten und CRM online beschränkt ist.
- Ein aktives Office 365-Abonnement, (bei Verwendung von Office 365-Diensten). Finden Sie im Abschnitt [Office 365-spezifischen Anforderungen](#office-365-specific-requirements) in diesem Artikel Weitere Informationen ein.

## <a name="connectivity-provider"></a>Connectivity-Anbieter
- Sie können mit einem [ExpressRoute Connectivity-Partner](expressroute-locations.md#partners) in Verbindung mit der Microsoft-Cloud arbeiten. Sie können eine Verbindung zwischen Ihrem lokalen Netzwerk und Microsoft auf [drei Arten](expressroute-introduction.md#howtoconnect)einrichten. 
- Wenn Ihrem Anbieter keinen ExpressRoute Connectivity-Partner ist, können Sie immer noch in der Microsoft-Cloud über ein [Exchange-Cloudanbieter](expressroute-locations.md#nonpartners)verbinden.

## <a name="network-requirements"></a>Netzwerk-Anforderungen
- **Redundante Connectivity**: Es ist nicht erforderlich Redundanz auf physische Konnektivität zwischen Ihnen und Ihrem Anbieter. Microsoft verlangt redundante BGP Sitzungen eingerichtet werden zwischen Microsoft Routern und den Peeringliste Routern, auch wenn Sie nur [eine physische Verbindung mit einem Exchange Cloud](expressroute-faqs.md#onep2plink)verfügen. 
- **Routing**: je nachdem, wie Sie mit der Microsoft-Cloud verbinden, Sie oder Ihren Anbieter zum Einrichten und Verwalten der BGP Sitzungen für [Domänen routing](expressroute-circuit-peerings.md)erforderlich ist. Einige Ethernet-Konnektivität Anbieter oder Exchange-Cloudanbieter bieten möglicherweise BGP Management als ein Wert Dienst hinzufügen.
- **NAT**: Microsoft nur öffentliche IP-Adressen durch Microsoft peering akzeptiert. Wenn Sie private IP-Adressen in Ihrem lokalen Netzwerk verwenden, Adressen Sie oder Ihre Anbieter müssen die privaten IP-Adressen in den öffentlichen IP-übersetzen [NAT verwenden](expressroute-nat.md).
- **QoS**: Skype für Unternehmen verschiedene Dienste (z. B. Stimme, video, Text) verwendet wird, die erfordern QoS-Behandlung unterschieden. Sie und Ihren Anbieter sollte die [QoS Anforderungen](expressroute-qos.md)folgen.
- **Network Security**: [Netzwerk Sicherheit](../best-practices-network-security.md) sollten bei einer Verbindung mit der Microsoft-Cloud über ExpressRoute.
 
## <a name="office-365"></a>Office 365

Wenn Sie planen der Aktivierung von Office 365 auf ExpressRoute, überprüfen Sie die folgenden Dokumente für Weitere Informationen zu Office 365.


- [Übersicht über ExpressRoute für Office 365](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
- [Routing mit ExpressRoute für Office 365](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
- [Office 365-URLs und IP-Adressbereiche](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
- [Planen von Netzwerk und Leistung optimieren für Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
- [Netzwerk Bandbreite von einsparungen und tools](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
- [Office 365-Integration in lokale Umgebungen](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## <a name="crm-online"></a>CRM Online 
Wenn Sie beabsichtigen, CRM Online auf ExpressRoute aktivieren, überprüfen Sie die folgenden Dokumente Weitere Informationen zu CRM Online

- [CRM Online-URLs](https://support.microsoft.com/kb/2655102) und [IP-Adressbereiche](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu ExpressRoute finden Sie im [ExpressRoute häufig gestellte Fragen](expressroute-faqs.md).
- Suchen nach einer ExpressRoute Connectivity-Anbieter. Finden Sie unter [ExpressRoute Partner und Peeringliste Speicherorte](expressroute-locations.md).
- Anforderungen für [Routing](expressroute-routing.md), [NAT](expressroute-nat.md) und [QoS](expressroute-qos.md)finden Sie unter.
- Konfigurieren Sie die Verbindung ExpressRoute.
    - [Erstellen einer Verbindung ExpressRoute](expressroute-howto-circuit-classic.md)
    - [Konfigurieren der Weiterleitung](expressroute-howto-routing-classic.md)
    - [Verknüpfen eines VNet zu einer ExpressRoute Verbindung](expressroute-howto-linkvnet-classic.md)

