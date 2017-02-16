<properties
   pageTitle="URL-basierten Inhalt routing (Übersicht) | Microsoft Azure"
   description="Diese Seite enthält eine Übersicht über die URL der Anwendung Gateway-basierten des inhaltsroutings, UrlPathMap Konfiguration und PathBasedRouting Regel."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>

# <a name="url-path-based-routing-overview"></a>URL-Pfad basierend Routing (Übersicht)

URL-Pfad basierend Routing können Sie für die Weiterleitung Verkehr zu Back-End-Serverpools basierend auf URL-Pfade der Anfrage. Eines der Szenarios ist zum Weiterleiten von Besprechungsanfragen für verschiedene Inhaltstypen in anderen Back-End-Serverpools.
Im folgenden Beispiel Application Gateway ist erstellen Datenverkehr für contoso.com aus drei Back-End-Serverpools zum Beispiel: VideoServerPool, ImageServerPool und DefaultServerPool.

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

Anforderung von http://contoso.com/video* VideoServerPool, und http://contoso.com/images weitergeleitet werden* , werden an ImageServerPool weitergeleitet. DefaultServerPool ausgewählt ist, wenn keine der Pfad Muster entsprechen.

## <a name="urlpathmap-configuration-element"></a>UrlPathMap Konfigurationselement

UrlPathMap Element wird verwendet, um zu Back-End-Server Ressourcenpool Zuordnungen Pfad-Muster anzugeben. Im folgenden Code wird ist der Ausschnitt des UrlPathMap Elements aus einer Vorlagendatei.

    "urlPathMaps": [
    {
    "name": "<urlPathMapName>",
    "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/ urlPathMaps/<urlPathMapName>",
    "properties": {
        "defaultBackendAddressPool": {
            "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendAddressPools/<poolName>"
        },
        "defaultBackendHttpSettings": {
            "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendHttpSettingsList/<settingsName>"
        },
        "pathRules": [
            {
                "paths": [
                    <pathPattern>
                ],
                "backendAddressPool": {
                    "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendAddressPools/<poolName2>"
                },
                "backendHttpsettings": {
                    "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/backendHttpsettingsList/<settingsName2>"
                },

            },

        ],

    }
    }
    

>[AZURE.NOTE] PathPattern: Mit dieser Einstellung wird eine Liste der Pfad Muster zu entsprechen. Jede muss mit beginnen / und die einzige Stelle ein "*" ist zulässig befindet sich am Ende vor einer "/". Die Zeichenfolge "Herd", um den Pfad beschrieben ist Text nicht nach dem ersten enthalten? oder # und diese Zeichen werden hier nicht zulässig. 

Sie können eine [Ressourcenmanager Vorlage mit URL-basierten routing](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing) für Weitere Informationen Auschecken.

## <a name="pathbasedrouting-rule"></a>PathBasedRouting Regel

RequestRoutingRule vom Typ PathBasedRouting wird verwendet, um die einer Zuhörer an eine UrlPathMap gebunden werden soll. Alle Besprechungsanfragen, die für diese Zuhörer erhalten, werden auf Basis der Richtlinie angegebenen UrlPathMap weitergeleitet.
Codeausschnitt PathBasedRouting Regel:

    "requestRoutingRules": [
    {

    "name": "<ruleName>",
    "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/requestRoutingRules/<ruleName>",
    "properties": {
        "ruleType": "PathBasedRouting",
        "httpListener": {
            "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/httpListeners/<listenerName>"
        },
        "urlPathMap": {
            "id": "/subscriptions/<subscriptionId>/../microsoft.network/applicationGateways/<gatewayName>/ urlPathMaps/<urlPathMapName>"
        },

    }
    
## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie nach dem Kennenlernen der URL-basierten des inhaltsroutings, [Erstellen Sie einen Gateway mit URL-basierten routing](application-gateway-create-url-route-portal.md) einen Gateway mit Weiterleitungsregeln URL zu erstellen.
