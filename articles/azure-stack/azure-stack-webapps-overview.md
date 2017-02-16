<properties
    pageTitle="Azure Stapel Web Apps Übersicht | Microsoft Azure"
    description="Übersicht über das Web Apps in Azure Stapel"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="azure-stack-web-apps-overview"></a>Azure Stapel Web Apps (Übersicht)
    
> [AZURE.NOTE] Die folgende Informationen gilt nur für Azure Stapel TP1 Bereitstellungen.

Azure Stapel Web Apps ist das erste Element des Angebots Azure-App-Verwaltungsdienst auf Azure Stapel gebracht. Das Installationsprogramm Azure Stapel Web Apps erstellt eine Instanz der einzelnen fünf Arten erforderliche Rolle und Sie können auch eine Dateiserver erstellen. Obwohl Sie mehrere Instanzen für die einzelnen Arten Rolle hinzufügen können, denken Sie daran, dass es nicht viel Platz für virtuelle Computer im Technical Preview 1 ist. Die aktuellen Funktionen von Azure Stapel Web Apps sind hauptsächlich Foundation-Funktionen, die zum Verwalten der Web apps System und Host erforderlich sind.

![Azure Stapel App Dienst Web Apps in der Azure Stapeln Portal][1]

## <a name="limitations-of-the-technical-preview"></a>Einschränkungen der technische Vorschau

Es gibt keine Unterstützung für die Azure Stapel App-Verwaltungsdienst Preview-Versionen. Setzen Sie Produktionsarbeitslasten zu diesem Release Preview nicht. Es gibt auch keine Upgrade zwischen Azure Stapel App-Verwaltungsdienst Preview-Versionen. Erster Linie diese Preview-Versionen sind, um anzuzeigen, was wir bereit sind und Feedback erhalten. 

## <a name="what-is-an-app-service-plan"></a>Was ist eine App Dienst planen?

Der Azure Stapel Web Apps-Anbieter für Ressourcen verwendet den gleichen Code, den das Feature Azure Web Apps in der App-Verwaltungsdienst Azure verwendet. Daher sind einige allgemeine Konzepte im Wert Beschreibung ein. Im Web Apps heißt der Preisgestaltung Container von Web apps der App-Serviceplan. Es stellt den Satz von dedizierten virtuellen Computern verwendet, um Ihre apps aufzunehmen. Innerhalb eines bestimmten Abonnements haben Sie mehrere Dienstpläne für die App. Dies gilt auch in Azure Stapel Web Apps. 

In Azure gibt es freigegebene und dedizierte Kollegen. Ein freigegebenes Arbeitskollegen unterstützt HD-mandantenfähigen Web app hosten, und es ist nur eine Reihe von freigegebenen Kollegen. Dedizierte Server nur von einem Mandanten verwendet und sind in drei Größen: klein, Mittel und groß. Die Bedürfnissen der lokalen Kunden können nicht immer mithilfe dieser Ausdrücke beschrieben werden. In Azure Stapel Web Apps können Ressourcen Anbieter Administratoren die Ebenen Worker definieren, die sie zur Verfügung stellen, sodass sie mehrere Sätze von freigegebenen Worker oder unterschiedliche Optionssätze dedizierten Worker basierend auf deren eindeutige Web-hosting Anforderungen haben möchten. Verwenden diese Definitionen Worker, können sie dann eigene Preise SKUs definieren.

## <a name="portal-features"></a>Leistungsmerkmale des Portals

Als auch mit dem Back-End wahr ist, wird Azure Stapel Web Apps, die Azure Web Apps verwendet dieselbe Benutzeroberfläche verwendet. Einige Features sind deaktiviert und nicht noch in Azure Stapel funktionsfähig. Dies liegt daran Azure-spezifische erwartet wird oder Dienste, die diese Features erfordern nicht noch Azure Stapel zur Verfügung stehen. 

## <a name="next-steps"></a>Nächste Schritte

- [Bevor Sie mit Azure Stapel Web Apps beginnen](azure-stack-webapps-before-you-get-started.md)
- [Installieren Sie das Web Apps-Ressourcen](azure-stack-webapps-deploy.md)

Sie können auch eine andere [Plattform als eine Service (PaaS) Dienste](azure-stack-tools-paas-services.md), wie die [Ressource-Anbieter für SQL Server](azure-stack-sql-rp-deploy-short.md) und [MySQL-Anbieter für Ressourcen](azure-stack-mysql-rp-deploy-short.md)ausprobieren.

<!--Image references-->
[1]: ./media/azure-stack-webapps-overview/AppService_Portal.png
