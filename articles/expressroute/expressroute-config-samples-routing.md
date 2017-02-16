<properties
   pageTitle="Beispiele für ExpressRoute Kunden Router Konfiguration | Microsoft Azure"
   description="Diese Seite enthält Router Config Beispiele für Cisco und Juniper Routern."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="router-configuration-samples-to-setup-and-manage-routing"></a>Router Konfiguration Beispiele zum Einrichten und Verwalten von routing

Diese Seite enthält Benutzeroberfläche und routing Konfiguration Beispiele für Cisco IOS-XE und Juniper MX Reihe Routern. Diese Beispiele dienen nur werden sollen und dürfen nicht verwendet werden, denn damit ist. Sie können mit Ihrem Lieferanten im Zusammenhang mit dem entsprechenden Konfigurationen für Ihr Netzwerk arbeiten. 

>[AZURE.IMPORTANT] Die Beispiele in dieser Seite sollen rein für Anleitungen sein. Sie müssen mit Ihres Herstellers sales / technischen Team und Netzwerke Team im Zusammenhang mit geeigneten Konfigurationen an Ihre Bedürfnisse arbeiten. Probleme bei der auf dieser Seite aufgeführten Konfigurationen wird von Microsoft nicht unterstützt. Sie müssen Ihre Gerätehersteller Supportthemen wenden.

Router Konfiguration Beispielen darunter gelten für alle Peerings. Überprüfen Sie [ExpressRoute Peerings](expressroute-circuit-peerings.md) und [ExpressRoute routing Anforderungen](expressroute-routing.md) detaillierte Informationen zur Weiterleitung an.

## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS-XE-basierten Router

In den Beispielen in diesem Abschnitt gelten für jeden Router die Familie IOS-XE OS ausgeführt.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. konfigurieren und untergeordnete Schnittstellen

Benötigen Sie eine Sub-Benutzeroberfläche pro peering in jedem Router Herstellen einer Verbindung mit Microsoft. Eine Sub-Benutzeroberfläche kann mit einem VLAN-ID oder ein gestapeltes Paar von VLAN-IDs und eine IP-Adresse identifiziert werden.

#### <a name="dot1q-interface-definition"></a>Definition der Dot1Q-Benutzeroberfläche

In diesem Beispiel enthält die untergeordnete Benutzeroberflächen-Definition für eine untergeordnete Schnittstelle mit einer einzelnen VLAN-ID Die VLAN-ID wird nur einmal peering. Das letzte Oktett Ihrer IPv4-Adresse wird immer eine ungerade Zahl sein.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

#### <a name="qinq-interface-definition"></a>Definition der QinQ-Benutzeroberfläche

Dieses Beispiel stellt die untergeordnete Benutzeroberflächen-Definition für eine untergeordnete Schnittstelle mit einer zwei VLAN-IDs. Die äußere VLAN-ID (s-Tag), wenn verwendet bleibt unverändert über alle die Peerings. Innere VLAN-ID (c-Tag) wird nur einmal peering. Das letzte Oktett Ihrer IPv4-Adresse wird immer eine ungerade Zahl sein.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>
    
### <a name="2-setting-up-ebgp-sessions"></a>2. einrichten eBGP Sitzungen

Richten Sie eine Sitzung BGP mit Microsoft für jede peering. Im folgenden Beispiel können Sie für die Einrichtung einer Sitzung BGP mit Microsoft. Wenn die IPv4-Adresse, die Sie für Ihre Sub-Oberfläche verwendet a.b.c.d wurde, werden die IP-Adresse der benachbarten BGP (Microsoft) a.b.c.d+1. Das letzte Oktett der BGP-benachbarten IPv4-Adresse wird immer eine gerade Zahl sein.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3 einrichten Präfixe über die Sitzung BGP angekündigt wird.

Sie können Ihre Router um select Präfixe an Microsoft ankündigen konfigurieren. Sie können mit dem folgenden Beispiel vorgehen.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. ordnet Routing

Routing-Karten können und Präfix Listen auf Filter Präfixe in Ihrem Netzwerk verteilt. Im folgenden Beispiel können Sie die Aufgabe auszuführen. Stellen Sie sicher, dass Sie die entsprechenden Präfix Listen Setup verfügen.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a>Juniper MX Reihe Routern 

In den Beispielen in diesem Abschnitt gelten für alle Juniper MX Reihe Routern.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. konfigurieren und untergeordnete Schnittstellen

#### <a name="dot1q-interface-definition"></a>Definition der Dot1Q-Benutzeroberfläche

In diesem Beispiel enthält die untergeordnete Benutzeroberflächen-Definition für eine untergeordnete Schnittstelle mit einer einzelnen VLAN-ID Die VLAN-ID wird nur einmal peering. Das letzte Oktett Ihrer IPv4-Adresse wird immer eine ungerade Zahl sein.

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


#### <a name="qinq-interface-definition"></a>Definition der QinQ-Benutzeroberfläche

Dieses Beispiel stellt die untergeordnete Benutzeroberflächen-Definition für eine untergeordnete Schnittstelle mit einer zwei VLAN-IDs. Die äußere VLAN-ID (s-Tag), wenn verwendet bleibt unverändert über alle die Peerings. Innere VLAN-ID (c-Tag) wird nur einmal peering. Das letzte Oktett Ihrer IPv4-Adresse wird immer eine ungerade Zahl sein.

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a>2. einrichten eBGP Sitzungen

Richten Sie eine Sitzung BGP mit Microsoft für jede peering. Im folgenden Beispiel können Sie für die Einrichtung einer Sitzung BGP mit Microsoft. Wenn die IPv4-Adresse, die Sie für Ihre Sub-Oberfläche verwendet a.b.c.d wurde, werden die IP-Adresse der benachbarten BGP (Microsoft) a.b.c.d+1. Das letzte Oktett der BGP-benachbarten IPv4-Adresse wird immer eine gerade Zahl sein.

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3 einrichten Präfixe über die Sitzung BGP angekündigt wird.

Sie können Ihre Router um select Präfixe an Microsoft ankündigen konfigurieren. Sie können mit dem folgenden Beispiel vorgehen.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a>4. ordnet Routing

Routing-Karten können und Präfix Listen auf Filter Präfixe in Ihrem Netzwerk verteilt. Im folgenden Beispiel können Sie die Aufgabe auszuführen. Stellen Sie sicher, dass Sie die entsprechenden Präfix Listen Setup verfügen.

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a>Nächste Schritte

Finden Sie weitere Details der [ExpressRoute häufig gestellte Fragen](expressroute-faqs.md) .
