<properties
   pageTitle="Zum Skalieren Azure Funktionen | Microsoft Azure"
   description="Grundlegendes zu Azure Funktionen wie skalieren, um den Bedürfnissen Ihrer Auslastung ereignisgesteuerten aus."
   services="functions"
   documentationCenter="na"
   authors="dariagrigoriu"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure-Funktionen, Funktionen, Ereignisse zu verarbeiten, Webhooks, dynamische berechnen, ohne Server Architektur"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="reference"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="dariagrigoriu"/>

# <a name="how-to-scale-azure-functions"></a>Zum Skalieren Azure-Funktionen

## <a name="introduction"></a>Einführung

Ein Vorteil der Azure-Funktionen ist, dass nur berechnen Ressourcen verbraucht werden, wenn erforderlich. Dies bedeutet, dass Sie nicht im Leerlauf virtuellen Computern bezahlen oder Kapazität für, wenn Sie es benötigen möglicherweise reservieren müssen. Stattdessen weist die Plattform berechnen Power, wenn Code wird ausgeführt, von Bedarf laden verarbeitet skaliert und dann nach unten skaliert, wenn der Code nicht ausgeführt wird.

Für diese neue Funktion wird der dynamischen Serviceplan.  

Dieser Artikel enthält eine Übersicht über die Funktionsweise der dynamischen Serviceplan und wie die Plattform bei Bedarf zum Ausführen des Codes skaliert.

Wenn Sie nicht mit Azure-Funktionen vertraut sind, stellen Sie sicher, prüfen Sie im Artikel [Übersicht über Funktionen Azure](functions-overview.md) , um seine Funktionen besser zu verstehen.

## <a name="configure-azure-functions"></a>Konfigurieren von Azure-Funktionen

Zwei Hauptfenster Einstellungen beziehen sich auf dieselbe Skalierung:

* [Azure App-Serviceplan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) oder einen bestimmten Dienst dynamische plan
* Größe des Speichers für die Ausführung-Umgebung

Die Kosten einer Funktion ändert sich je nach der Serviceplan, die Sie auswählen. Mit einem dynamischen Serviceplan sind die Kosten Funktion der Ausführung dauert, Arbeitsspeichergröße und Anzahl der Ausführungen. Gebühren fällig nur, wenn der Code tatsächlich ausgeführt wird.

Eine App Serviceplan hostet die Funktionen auf vorhandenen virtuellen Computern, die auch verwendet werden können, um anderen Code ausgeführt werden. Nachdem Sie für diese virtuelle Computer monatlich Zahlen, besteht keine zusätzliche Gebühr für die von Ausführungsfunktionen wurden.

## <a name="choose-a-service-plan"></a>Wählen Sie einen Serviceplan

Bei der Erstellung von Funktionen können Sie auswählen, um diese auf einem dynamischen Serviceplan oder ein [App-Serviceplan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)auszuführen.
Ihre Funktionen werden in der App-Serviceplan eine dedizierte virtuellen Computers, genau wie Web apps Arbeit heute für Basic, Standard oder Premium SKUs ausgeführt.
Diesem dedizierten virtuellen Computer apps und Funktionen zugeordnet, und es ist immer verfügbar, ob Code aktiv oder nicht ausgeführt wird. Dies ist eine gute Wahl, wenn Sie vorhandene, Auslastung virtuellen Computern haben, die bereits anderen Code ausgeführt werden, oder wenn Sie vermuten, Funktionen kontinuierlich oder fast kontinuierlich ausführen. Ein virtueller Computer trennt Kosten aus sowohl Laufzeit und Arbeitsspeicher Größe. Daher können Sie die Kosten vieler langer-Funktionen, um die Kosten für die eine oder mehrere virtuellen Computern einschränken, die sie auf ausgeführt werden.

[AZURE.INCLUDE [Dynamic Service plan](../../includes/functions-dynamic-service-plan.md)]
