<properties
   pageTitle="QoS-Anforderungen für ExpressRoute | Microsoft Azure"
   description="Diese Seite enthält ausführliche Anforderungen für das Konfigurieren und Verwalten von QoS für ExpressRoute Schaltkreise."
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

# <a name="expressroute-qos-requirements"></a>ExpressRoute QoS Anforderungen

Skype für Unternehmen weist verschiedene Auslastung, die gestaffelter QoS behandelt werden müssen. Wenn Sie beabsichtigen, VoIP-Diensten über ExpressRoute nutzen, sollten Sie die nachfolgend beschriebenen Anforderungen entsprechen.

![](./media/expressroute-qos/expressroute-qos.png)

>[AZURE.NOTE] QoS-Anforderungen anwenden an die Microsoft nur peering DSCP-Werte in der Azure öffentlichen peering und Azure private peering eingehenden Netzwerkverkehr werden auf 0 zurückgesetzt. 

Die folgende Tabelle enthält eine Liste der DSCP Auswahlmöglichkeiten von Skype für Business verwendet wird. Weitere Informationen finden Sie unter [Verwalten von QoS für Skype für Unternehmen](https://technet.microsoft.com/library/gg405409.aspx) .

| **Datenverkehrsklasse** | **Behandlung (DSCP-Markierung)** | **Skype für Business Auslastung** |
|---|---|---|
| **Voicemail** | EF (46) | Skype-Lync-VoIP |
| **Interaktive** | AF41 (34) | Video |
|   | AF21 (18) | App-Freigabe | 
| **Standard** | AF11 STELLT (10) | Dateiübertragung|
|   | CS0 (0) | Irgendetwas anderes| 


- Die Auslastung klassifizieren, und markieren die richtigen DSCP-Werten. Führen Sie die Anleitung [hier](https://technet.microsoft.com/library/gg405409.aspx) zum DSCP Auswahlmöglichkeiten in Ihrem Netzwerk festlegen.

- Sie sollten konfigurieren und unterstützt mehrere QoS Warteschlangen in Ihrem Netzwerk. VoIP muss es sich um einen eigenständigen Klasse und erhalten die EF Behandlung in RFC 3246 angegeben. 

- Sie können die Warteschlangenmechanismus, Überlastung Erkennung Richtlinie und Bandbreite Zuteilung pro Datenverkehrsklasse festlegen. Aber der DSCP-Markierung für Skype für Business Auslastung beibehalten werden muss. Bei Verwendung von DSCP Auswahlmöglichkeiten nicht aufgeführten z. B. AF31 (26), müssen Sie diese DSCP-Wert 0 schreiben Sie vor dem das Paket an Microsoft senden. Microsoft sendet nur Pakete mit dem DSCP-Wert, der in der obigen Tabelle aufgeführten markiert. 

## <a name="next-steps"></a>Nächste Schritte

- Die Anforderungen für das [Routing](expressroute-routing.md) und [NAT](expressroute-nat.md)finden Sie unter.
- Finden Sie unter folgenden Links, um die Verbindung ExpressRoute konfigurieren.

    - [Erstellen einer Verbindung ExpressRoute](expressroute-howto-circuit-classic.md)
    - [Konfigurieren der Weiterleitung](expressroute-howto-routing-classic.md)
    - [Verknüpfen eines VNet zu einer ExpressRoute Verbindung](expressroute-howto-linkvnet-classic.md)
