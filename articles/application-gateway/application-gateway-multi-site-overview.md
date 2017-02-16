<properties
   pageTitle="Mehrere Websites auf Application Gateway Hostinganbieter | Microsoft Azure"
   description="Diese Seite enthält eine Übersicht über die Unterstützung der Anwendungsgateway mit mehreren Standorten."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="amsriva"/>

# <a name="application-gateway-multiple-site-hosting"></a>Application Gateway mehreren Websitehost

Mehreren Websitehost, können Sie mehr als eine Webanwendung auf der gleichen Instanz der Anwendung Gateway zu konfigurieren. Dieses Feature können Sie ein effizienter Suchtopologie für die Bereitstellung Ihrer zu konfigurieren, indem Sie eine Anwendungsgateway bis zu 20 Websites hinzufügen. Jede Website kann in einem eigenen Back-End-Pool umgeleitet werden. Im folgenden Beispiel werden die Anwendungsgateway Datenverkehr für contoso.com und fabrikam.com aus zwei Back-End-Serverpools namens ContosoServerPool und FabrikamServerPool übermittelt.

![imageURLroute](./media/application-gateway-multi-site-overview/multisite.png)

Anforderung von http://contoso.com an ContosoServerPool weitergeleitet werden, und http://fabrikam.com an FabrikamServerPool weitergeleitet werden.

Auf ähnliche Weise können zwei Unterdomänen derselben übergeordneten Domäne auf die gleiche Bereitstellung der Anwendung Gateway gehostet werden. Beispiele für mit Unterdomänen konnte http://blog.contoso.com und http://app.contoso.com auf einer einzelnen Anwendung Gateway-Bereitstellung gehostet sind.

## <a name="host-headers-and-server-name-indication-sni"></a>Host Kopf- und Server Namen Angabe (SNI)

Es gibt drei gängigsten Methoden für die Aktivierung von mehreren Websitehost auf dieselbe Infrastruktur aus.

1. Hosten Sie mehrere Webanwendungen jede auf eine eindeutige IP-Adresse ein.
2. Verwenden Sie Hostnamen, um mehrere Webanwendungen auf die IP-Adresse zu hosten.
3. Verwenden Sie andere Ports, um mehrere Webanwendungen auf die IP-Adresse zu hosten.

Ein Gateway wird aktuell eine einzelne öffentliche IP-Adresse, die es für den Datenverkehr überwacht. Unterstützung für mehrere daher, wird jede mit einem eigenen IP-Adresse, derzeit nicht unterstützt. Anwendungsgateway unterstützt mehrere Hostinganbieter Abhören jede andere Ports aber dieses Szenario müssten die Applikationen Datenverkehr auf nicht standardmäßigen Ports akzeptiert und ist oft nicht gewünschte Konfiguration. Application Gateway basiert auf HTTP 1.1 Host Überschriften, um mehr als eine Website auf die gleiche öffentliche IP-Adresse und den Port zu hosten. Die Websites auf Anwendungsgateway gehostet können auch Support SSL Verschiebung mit der Erweiterung TLS Server Namen Angabe (SNI). Dieses Szenario bedeutet, dass die Client-Browser und Back-End-Web-Farm HTTP/1.1 und TLS-Erweiterung unterstützen muss, wie in RFC 6066 definiert sind.

## <a name="listener-configuration-element"></a>Zuhörer Konfigurationselement

Vorhandenes HTTPListener Konfigurationselement wurde verbessert, um Host Name und Server Namen Angabe Elemente, unterstützen, die durch die Anwendungsgateway für die Weiterleitung Verkehr zu entsprechenden Back-End-Pool verwendet wird. Im folgenden Code wird ist der Ausschnitt des HttpListeners Elements aus einer Vorlagendatei.

    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener1",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
                        },
                        "Protocol": "Https",
                        "SslCertificate": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
                        },
                        "HostName": "contoso.com",
                        "RequireServerNameIndication": "true"
                    }
                },
                {
                    "name": "appGatewayHttpListener2",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                        "HostName": "fabrikam.com",
                        "RequireServerNameIndication": "false"
                    }
                }
            ],




Sie können [mit mehreren Websitehost Ressourcenmanager-Vorlage](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) für eine durchgehende Vorlage basierende Bereitstellung besuchen.

## <a name="routing-rule"></a>Weiterleitung Regel

Es gibt keine Änderung in der Regel routing erforderlich. Die Weiterleitung Regel 'Basic' sollte weiterhin ausgewählt werden, um die entsprechende Website Zuhörer an den entsprechenden Back-End-Adresspool zu verbinden.

    "requestRoutingRules": [
    {
        "name": "<ruleName1>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener1')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    },
    {
        "name": "<ruleName2>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener2')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/FabrikamServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    }
    ]

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie nach dem Kennenlernen von mehreren Websitehost, [Erstellen Sie einen Gateway mit mehreren Websitehost](application-gateway-create-multisite-azureresourcemanager-powershell.md) , um ein Gateway mit Möglichkeit zur Unterstützung von mehr als eine Webanwendung zu erstellen.
