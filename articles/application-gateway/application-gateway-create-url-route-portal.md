<properties
   pageTitle="Erstellen eine Path-basierten Regel für ein Gateway mit dem Portal | Microsoft Azure"
   description="Erfahren Sie, wie eine Regel Pfad-basierten für ein Gateway mit dem Portal erstellen"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a>Erstellen einer Path-basierten Regel für ein Gateway mit dem portal

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-url-route-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-url-route-arm-ps.md)

URL-Pfad-basierten routing ermöglicht es Ihnen, basierend auf den URL-Pfad der HTTP-Anforderung leitet zugeordnet werden soll. Überprüft, ob es stellt einen Weg in einer Back-End-Ressourcenpool so konfiguriert, dass für die URL-Listen in das Feld Gateway-Anwendung, und senden den Datenverkehr an den definierten Back-End-Pool. Eine häufige Verwendung für das routing URL-basierten besteht darin Saldo Besprechungsanfragen für verschiedene Inhaltstypen in anderen Back-End-Serverpools zu laden.

URL-basierten routing führt einen neuen Regeltyp in Anwendungsgateway ein. Anwendungsgateway weist zwei Regeltypen: Standard- und Path-basierte Regeln. Grundlegende Regeltyp bietet Round-Robert Dienstleistungen für die Back-End-Pools während der Pfad-basierte Regeln sowie die Funktion RUNDEN Robert Verteilung, auch berücksichtigt Pfad Muster der Anforderung URL während den Back-End-Pool auswählen.

## <a name="scenario"></a>Szenario

Das folgende Szenario durchläuft ein Gateway der vorhandenen eine Path-basierte Regel erstellen.
Das Szenario setzt voraus, dass Sie die Schritte zum [Erstellen eines Gateways Anwendung](application-gateway-create-gateway-portal.md)bereits befolgt haben.

![URL-Routing][scenario]

## <a name="a-namecreateruleacreate-the-path-based-rule"></a><a name="createrule"></a>Die Pfad-basierte Regel erstellen

Eine Path-basierten Regel erfordert eine eigene Zuhörer möchten, bevor Sie sicher werden die Regel erstellen müssen Sie eine verfügbare Zuhörer verwenden.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu http://portal.azure.com, und wählen Sie eine vorhandene Anwendungsgateway. Klicken Sie auf **Regeln**

![Übersicht über die Anwendung Gateway][1]

### <a name="step-2"></a>Schritt 2

Klicken Sie auf **Path-basierten** Schaltfläche zum Hinzufügen einer neuen Regel auf Pfad basieren.

### <a name="step-3"></a>Schritt 3

Das Blade **Pfad-basierten Regel hinzufügen** besteht aus zwei Abschnitten. Der erste Abschnitt ist, in dem Sie die Zuhörer, den Namen der Regel und die Standardeinstellungen für den Pfad definiert. Die Standardeinstellungen für den Pfad sind für weitergeleitet, die nicht unter den benutzerdefinierten Pfad-basierten Routing fallen. Der zweite Abschnitt des Blades **Pfad-basierten Regel hinzufügen** ist, in dem Sie die Pfad-basierte Regeln selbst definieren.

**Grundlegende Einstellungen**

- **Name** - hier einen Anzeigenamen für die Regel, die im Portal zugegriffen werden kann.
- **Zuhörer** – Dies ist die Zuhörer, die für die Regel verwendet wird.
- **Standard-Back-End-Pool** - diese Einstellung ist die Einstellung, die definiert die Back-End für die Standardregel verwendet werden soll
- **Standard-HTTP-Einstellungen** - diese Einstellung ist die Einstellung, die definiert die HTTP-Einstellungen für die Standardregel verwendet werden soll.

**Path-basierte Regeln**

- **Name** – dies einen Anzeigenamen, Pfad-basierten Regel ist.
- **Pfade** - diese Einstellung definiert den Pfad, die, den für die Regel aussehen wird, wenn die Weiterleitung des Datenverkehrs
- **Back-End-Ressourcenpool** – diese Einstellung wird die Einstellung, die definiert die Back-End für die Regel verwendet werden soll
- **HTTP-Einstellung** - diese Einstellung ist die Einstellung, die definiert die HTTP-Einstellungen für die Regel verwendet werden soll.

>[AZURE.IMPORTANT] Pfade: Die Liste der Pfad Muster zu entsprechen. Jede muss mit beginnen / und die einzige Stelle einer "\*" ist zulässig befindet sich am Ende. Beispiele für gültige sind /xyz, /xyz* oder /xyz/*.  

![Hinzufügen von Regel Pfad-basierten Blade mit Informationen ausgefüllt][2]

Hinzufügen einer Path-basierten Regel zu einer vorhandenen Anwendungsgateway ist ein einfacher Vorgang über das Portal. Nachdem Sie eine Regel für Pfad erstellt wurde, können sie zum Hinzufügen von weiteren Regeln einfach bearbeitet werden. 

![Hinzufügen von zusätzlichen Pfad-basierte Regeln][3]

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren von SSL Verschiebung mit Azure Application Gateway finden Sie unter [Konfigurieren von SSL Auslagern](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png