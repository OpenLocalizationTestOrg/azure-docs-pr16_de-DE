<properties
   pageTitle="Anwendung Gateway WebSocket Support | Microsoft Azure"
   description="Diese Seite enthält eine Übersicht über die Anwendung Gateway WebSocket-Unterstützung."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/16/2016"
   ms.author="amsriva"/>

# <a name="application-gateway-websocket-support"></a>Anwendung Gateway WebSocket-support

Application Gateway bietet systemeigene Unterstützung für WebSocket über alle Gateway Größen. Es gibt keine konfigurierbare Einstellung Selektives aktivieren oder deaktivieren Sie die WebSocket-Unterstützung. Sie können eine Standardansicht HTTPListener-weiterhin am Anschluss 80/443 WebSocket Datenverkehr empfangen. WebSocket-Datenverkehr wird dann WebSocket aktiviert Back-End-Server mit den entsprechenden Back-End-Pool gemäß Angabe in der Anwendung Gateway Regeln geleitet. In [RFC6455](https://tools.ietf.org/html/rfc6455) standardisiert WebSocket-Protokoll ermöglicht eine vollständige duplex Kommunikation zwischen Server und Client über eine zeitintensive TCP-Verbindung. Dieses Feature ermöglicht eine interaktiver Kommunikation zwischen Webserver und -Client, ohne die Notwendigkeit Umfragen als erforderlich in HTTP-basierte Implementierungen bidirektionaler werden kann.  WebSocket haben Aufwand niedrig im Gegensatz zu HTTP und wiederverwenden dieselbe TCP-Verbindung für mehrere Nachfrage/Antworten, was zu einer effizienteren Nutzung von Ressourcen. WebSocket Protokolle sollen über herkömmliche HTTP-Ports 80 und 443 arbeiten.

Der Back-End-Server muss auf Anwendung Gateway Stichproben, reagieren, die im Abschnitt [Dienststatus Prüfpunkt Übersicht](application-gateway-probe-overview.md) beschrieben werden. Anwendungsprüfpunkte Gateway-Dienststatus sind nur bei HTTP-/HTTPS, dies bedeutet, dass jeder Back-End-Server auf HTTP-Stichproben für Anwendungsgateway für WebSocket Datenverkehr an den Server beantworten muss.

## <a name="listener-configuration-element"></a>Zuhörer Konfigurationselement

Vorhandene HTTPListener kann zur Unterstützung von WebSocket verwendet werden. Es folgt ein Ausschnitt des HttpListeners Elements aus einer Vorlage Beispieldatei. Sie müssten HTTP- und HTTPS-Listener unterstützen WebSocket und schützen WebSocket-Datenverkehr. Auf ähnliche Weise können im [Portal](application-gateway-create-gateway-portal.md) oder [PowerShell](application-gateway-create-gateway-arm.md) So erstellen einen Gateway mit Empfängern am Anschluss 80/443 WebSocket-Datenverkehr zu unterstützen.


    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener",
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
                    }
                },
                {
                    "name": "appGatewayHttpListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                    }
                }
            ],

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>BackendAddressPool, BackendHttpSetting und Routing Regel-Konfiguration

BackendAddressPool sollte zum Definieren einer Back-End-Ressourcenpool mit aktiviert WebSocket-Servern verwendet werden. BackendHttpSetting definiert werden sollen, mit Back-End-port 80/443. Eigenschaften für die Zugehörigkeit Cookie-basierten und RequestTimeouts sind nicht für die WebSocket-Datenverkehr. Es gibt keine Änderung in der Regel routing erforderlich. Routing Regel, dass 'Einfache' fortgesetzt werden soll, um die entsprechenden Zuhörer an den entsprechenden Back-End-Adresspool verbinden verwendet werden soll. 

    "requestRoutingRules": [{
        "name": "<ruleName1>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    }, {
        "name": "<ruleName2>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }

        }
    }]

## <a name="websocket-enabled-backend"></a>WebSocket aktiviert Back-End-

Ihre Back-End-müssen einen HTTP-/HTTPS-Webserver ausgeführt wird, klicken Sie auf den konfigurierten port (in der Regel 80/443) für WebSocket entwickelt. Diese Anforderung ist, da WebSocket-Protokoll der ersten Handshakes mit HTTP mit dem Upgrade zu WebSocket-Protokoll als Feld Kopfzeile werden muss.

    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13

Ein weiterer Grund dafür ist, dass die Anwendung Gateway Back-End-Dienststatus Prüfpunkt nur HTTP-/HTTPS-Protokolle unterstützt. Wenn der Back-End-Server nicht auf HTTP-/HTTPS-Prüfpunkte reagiert, würde von Back-End-Pool und keine Besprechungsanfragen, einschließlich WebSocket Anfragen geöffnet werden, dieses Back-End-zu erreichen.

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie nach dem Kennenlernen der WebSocket Support, erste Schritte mit einer Webanwendung WebSocket aktiviert [einen Gateway erstellen](application-gateway-create-gateway.md) .
