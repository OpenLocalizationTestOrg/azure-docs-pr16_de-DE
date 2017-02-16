<properties
    pageTitle="App-Verwaltungsdienst Azure: Skalierung App Dienstanwendungen"
    description="Erfahren Sie, den Vorteilen und der Skalierung der Anwendung im App-Dienst."
    keywords="App Dienst Azure app Dienst skalieren skalierbare, app-Serviceplan app Dienst Kosten"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-scaling-app-service-applications"></a>App-Verwaltungsdienst Azure: Skalierung App Dienstanwendungen

Applications in Azure-App-Verwaltungsdienst gehostet können [umfangreiche Maßstab erzielen](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).
Skalierung der Anwendung ist jedoch ein komplexes Problem, das nicht über eine Lösung "eine universell" verfügt. Klicken Sie auf ordnungsgemäß Skalieren Ihrer Anwendung es 3 wichtige Bereiche, die für Ihren Erfolg Applikationen beiträgt sind:

1. Grundlegendes zu Ihrer Anwendungsarchitektur und seine Schwächen.
    * Ist der Anwendung Stateful? Statusfreie?
    * Was sind die Komponenten Ihrer Anwendung?
        * Wo sind die Engpässe in der Anwendung ein?
    * Beim Laden zu Ihrer Anwendung angewendet wird, wird was ersten unterbrechen?
2. Grundlegendes zu den erwarteten laden und Leistung Anforderungen aus.
    * Benötigt die Anwendung skaliert Benutzer dienen? oder eine Millionen?
    * Werden Datenverkehr muss von einem geografischen Standort oder global bereitgestellt?
    * Gibt es Variationen saisonale? Datenverkehr Spitzen?
    * Wie schnell sollte die app Antworten? 1 Sekunde? 1 Millisekunden?
3. Grundlegendes zu und nutzen Sie die Hostinganbieter Ihre app-Plattform ordnungsgemäß.
    * Welche Features sollten werden genutzt, um meine Maßstab Ziele zu erreichen?

In diesem Abschnitt hilft Ihnen zu verstehen, alle Faktoren und Hilfe Sie eine Strategie entwickeln, die die notwendigen App Service-Features zum Erreichen Ihrer Ziele Skalierbarkeit nutzt.

[AZURE.INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]
