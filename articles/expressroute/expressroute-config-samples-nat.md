<properties
   pageTitle="Beispiele für ExpressRoute Kunden Router Konfiguration | Microsoft Azure"
   description="Diese Seite enthält Beispiele für Router-Konfiguration für Cisco und Juniper Routern."
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

# <a name="router-configuration-samples-to-setup-and-manage-nat"></a>Router Konfiguration Beispiele zum Einrichten und Verwalten von NAT

Diese Seite enthält Beispiele für NAT-Konfiguration für ASA Cisco und Juniper SRX Reihe Routern. Diese Beispiele dienen nur werden sollen und dürfen nicht verwendet werden, denn damit ist. Sie können mit Ihrem Lieferanten im Zusammenhang mit dem entsprechenden Konfigurationen für Ihr Netzwerk arbeiten. 

>[AZURE.IMPORTANT] Die Beispiele in dieser Seite sollen rein für Anleitungen sein. Sie müssen mit Ihres Herstellers sales / technischen Team und Netzwerke Team im Zusammenhang mit geeigneten Konfigurationen an Ihre Bedürfnisse arbeiten. Probleme bei der auf dieser Seite aufgeführten Konfigurationen wird von Microsoft nicht unterstützt. Sie müssen Ihre Gerätehersteller Supportthemen wenden.

Router Konfiguration Beispielen darunter gelten für Öffentliche Azure und Microsoft Peerings. Sie müssen nicht NAT für Azure private peering konfigurieren. Überprüfen Sie [ExpressRoute Peerings](expressroute-circuit-peerings.md) und [ExpressRoute NAT Anforderungen](expressroute-nat.md) für weitere Details an.

**Hinweis:** Verwenden Sie für die Verbindung zum Internet und ExpressRoute separaten NAT IP-Adresspools an. Verwenden den gleichen NAT IP-Pool im Internet und ExpressRoute führt asymmetrische routing und Verlust der Konnektivität.

## <a name="cisco-asa-firewalls"></a>Cisco ASA firewalls

### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a>PAT Konfiguration für den Datenverkehr von Kundennetzwerk an Microsoft

    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>
    
    
    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>
    
    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2
    
    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a>PAT Konfiguration für den Datenverkehr von Microsoft Kunden Netzwerk

#### <a name="interfaces-and-direction"></a>Schnittstellen und Richtung:
    Source Interface (where the traffic enters the ASA): inside
    Destination Interface (where the traffic exits the ASA): outside

#### <a name="configuration"></a>Konfiguration:
NAT Ressourcenpool:

    object network outbound-PAT
        host <NAT-IP>

Ziel-Server:

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

Objektgruppe für Kunden IP-Adressen

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>
    
    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

NAT Befehle:

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a>Juniper SRX Reihe Routern 

### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a>1. erstellen Sie redundante Ethernet-Schnittstellen für den cluster

    interfaces {
        reth0 {
            description "To Internal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "To Microsoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "To Microsoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a>2. erstellen Sie zwei Sicherheitszonen

 - Vertrauen Sie Zone für internen Netzwerk und nicht vertrauen Zone für externe Netzwerk gegenüberliegende Kante Routern
 - Weisen Sie die entsprechende Schnittstellen zu Zonen
 - Dienste auf den Schnittstellen zulassen


    Sicherheit {Zonen Sicherheit-Zone Trust {{Host-eingehende-Datenverkehr {System-Dienste {Ping;                  } Protokolle {Bgp;                  Schnittstellen für}} {reth0.100;              }} Sicherheit-Zone nicht vertrauen {Host-eingehende-Datenverkehr {System-Dienste {Ping;                  } Protokolle {Bgp;                  Schnittstellen für}} {reth1.100;              }          }      }  }


### <a name="3-create-security-policies-between-zones"></a>3. Sicherheitsrichtlinien zwischen Zonen erstellen
 
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }


### <a name="4-configure-nat-policies"></a>4 Konfigurieren Sie 4 NAT Richtlinien
 - Zwei NAT Pool anlegen. Eine wird zum NAT-Datenverkehr an Microsoft ausgehende und andere von Microsoft an den Kunden verwendet werden.
 - Erstellen von Regeln zum NAT den entsprechenden Datenverkehr

        security {
            nat {
                source {
                    pool SNAT-To-ExpressRoute {
                        routing-instance {
                            External-ExpressRoute;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    pool SNAT-From-ExpressRoute {
                        routing-instance {
                            Internal;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    rule-set Outbound_NAT {
                        from routing-instance Internal;
                        to routing-instance External-ExpressRoute;
                        rule SNAT-Out {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-To-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                    rule-set Inbound-NAT {
                        from routing-instance External-ExpressRoute;
                        to routing-instance Internal;
                        rule SNAT-In {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-From-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }


### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a>5. konfigurieren Sie BGP zum selektiven Präfixe in jeder Richtung ankündigen

Beispiele in [Routing](expressroute-config-samples-routing.md) -Konfiguration Beispielen finden Sie unter.

### <a name="6-create-policies"></a>6 Richtlinien erstellen

    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }

## <a name="next-steps"></a>Nächste Schritte

Finden Sie weitere Details der [ExpressRoute häufig gestellte Fragen](expressroute-faqs.md) .
