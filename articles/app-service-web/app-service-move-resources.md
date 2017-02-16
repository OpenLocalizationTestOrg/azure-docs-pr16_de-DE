<properties
    pageTitle="Wechseln Sie in eine andere Ressourcengruppe Web App-Ressourcen"
    description="Beschreibt die Szenarien, wo Sie Web Apps und Services App aus einer Gruppe von Ressourcen zu einem anderen wechseln können."
    services="app-service"
    documentationCenter=""
    authors="ZainRizvi"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/04/2016"
    ms.author="zarizvi"/>
    
# <a name="supported-move-configurations"></a>Unterstützte verschieben Konfigurationen

Sie können mit der [Cloud verschieben Ressourcen Api](../resource-group-move-resources.md)Azure Web App-Ressourcen verschieben.

Azure Web Apps unterstützt derzeit die folgenden Szenarien verschieben:

* Verschieben Sie den gesamten Inhalt einer Ressourcengruppe (Web apps, app-Service-Pläne und Zertifikate) in eine andere Ressourcengruppe 
    * Hinweis: Die Ziel-Ressourcengruppe kann keine Microsoft.Web Ressourcen in diesem Szenario enthalten.
* Verschieben einzelner Web apps zu einer anderen Ressourcengruppe, während sie weiterhin hosten, in deren aktuellen app-Serviceplan (der app-Serviceplan bleibt in der alten Ressourcengruppe)
