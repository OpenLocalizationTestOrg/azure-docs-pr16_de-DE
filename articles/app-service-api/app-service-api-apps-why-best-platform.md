<properties 
    pageTitle="Einführung API Apps | Microsoft Azure" 
    description="Erfahren Sie, wie App-Verwaltungsdienst Azure Entwickeln, Host, und der Rest-APIs nutzen." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/23/2016" 
    ms.author="rachelap"/>

# <a name="api-apps-overview"></a>Apps-API (Übersicht)

API-apps in Azure-App-Verwaltungsdienst bieten Features, die das Entwickeln, Host, und nutzen APIs in der Cloud und lokale erleichtern. Mit apps API erhalten Sie Enterprise Noten Sicherheit, einfachen Access-Steuerelements, Hybrid Connectivity, automatische SDK Generation und nahtlose Integration mit [Logik Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md).

[App-Verwaltungsdienst Azure](../app-service/app-service-value-prop-what-is.md) ist eine vollständig verwaltete Plattform für Web, mobil, und Integration Szenarien. Apps-API ist eine der vier app Arten von [App-Verwaltungsdienst Azure](../app-service/app-service-value-prop-what-is.md)angeboten.

![App-Typen in Azure-App-Verwaltungsdienst](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Gründe für die Verwendung von Apps-API

Hier sind einige der wichtigen Features von Apps-API aus:

- **Bringen Sie Ihre vorhandene API als-ist** -verfügen Sie nicht über so ändern Sie den Code in Ihrer vorhandenen APIs zu nutzen der Apps-API – nur von Code zu einer API-app bereitstellen. Ihre API können beliebige Sprache oder Framework von App-Verwaltungsdienst, einschließlich ASP.NET und c#, Java, PHP, Node.js und Python unterstützt.

- **Einfache Verbrauch** - integrierte Unterstützung für [Metadaten Swagger API](http://swagger.io/) macht Ihre APIs von einer Vielzahl von Clients einfach angewendet.  Generieren Sie Automatisches Client-Code für Ihre in einer Vielzahl von Sprachen, einschließlich c#, Java und Javascript-APIs. Konfigurieren Sie einfach [CORS](app-service-api-cors-consume-javascript.md) ohne Änderung des Codes. Weitere Informationen finden Sie unter [App Service API Apps Metadaten API Discovery und Code zur Erstellung](app-service-api-metadata.md) und [verbrauchen einer app API JavaScript CORS verwenden](app-service-api-cors-consume-javascript.md). 

- **Einfache Access Steuerelement** - API app nicht authentifizierten Zugriff mit keine Änderungen an Ihrem Code schützen. Integrierte Authentifizierung Services secure APIs für den Zugriff von anderen Diensten oder von Clients, die Benutzer darstellt. Unterstützte Identitätsanbieter einschließen Azure Active Directory, Facebook, Twitter, Google und Microsoft Account. Clients können Active Directory-Authentifizierung Bibliothek (ADAL) oder das Mobile-Apps SDK verwenden. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung für API Apps in Azure-App-Dienst](app-service-api-authentication.md).

- Die Arbeit der erstellen, bereitstellen, Verarbeitung, für das Debuggen und Verwalten von apps API **visual Studio Integration** - dedizierten Tools in Visual Studio zu optimieren. Weitere Informationen finden Sie unter [der Azure SDK für .NET 2.8.1 Ankündigung](/blog/announcing-azure-sdk-2-8-1-for-net/).

- **Integration in Logik Apps** - API apps, die Sie erstellen können von [App Dienst Logik Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md)genutzt werden.  Weitere Informationen finden Sie unter [Verwenden der benutzerdefinierten API auf App-Verwaltungsdienst mit apps Logik gehostet](../app-service-logic/app-service-logic-custom-hosted-api.md) und [Neues Schema Version 2015-08-01-Vorschau](../app-service-logic/app-service-logic-schema-2015-08-01.md).

Darüber hinaus kann eine app API von [Web Apps](../app-service-web/app-service-web-overview.md) und [Mobile-Apps](../app-service-mobile/app-service-mobile-value-prop.md)bereitgestellten Features nutzen. Auch im umgekehrten Fall gilt: Wenn Sie einer Web app oder mobile-app verwenden, um eine API zu hosten, sie können nutzen API Apps-Features wie Swagger Metadaten für Client Code Generation und CORS für den Zugriff über einen Webbrowser die Domain-übergreifende. Der einzige Unterschied zwischen den drei app Typen (API, mobile Web) ist der Name und Symbol für diese Azure-Portal verwendet.

## <a name="whats-the-difference-between-api-apps-and-azure-api-management"></a>Was ist der Unterschied zwischen Apps-API und Azure-API Management?

Apps-API und [Azure-API Management](../api-management/api-management-key-concepts.md) sind ergänzende Dienste:

* Projektmanagement-API wird zum Verwalten von APIs. Sie setzen eine front-End-API Management auf eine API überwachen und Steuerung: Einsatz hinzu, Eingabe- und zu bearbeiten, mehrere APIs in einen Endpunkt konsolidieren, usw. Die verwalteten APIs können an einer beliebigen Stelle gehostet werden.
* Apps-API geht hosting-APIs. Dienst enthält Features, die Entwicklung und in Anspruch nehmen APIs zu erleichtern, aber es keine führen Sie die Arten von Überwachung, begrenzungsebene und Bearbeiten von oder bedeutet, dass Management API konsolidieren. Wenn Sie API Verwaltungsfunktionen benötigen, können Sie APIs in API apps hosten, ohne API Management.

Hier ist ein Diagramm, das API Management für APIs gehostet API apps und an anderer Stelle verwendete veranschaulicht.

![Azure-API Management und API-Apps](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Einige Features von Management-API und API Apps haben ähnliche Funktionen.  Beispielsweise können beide CORS Support automatisieren. Wenn Sie die beiden Dienste gemeinsam verwenden, verwenden Sie-API Management für CORS, da es als front-End für Ihre apps API funktioniert. 

## <a name="getting-started"></a>Erste Schritte

Finden Sie im Lernprogramm für welches Framework Sie es vorziehen, mit API Apps ersten Beispiel-Code auf eine bereitzustellen:

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

Wenn Sie Fragen zu API apps beginnen Sie einen Thread im [API Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)aus. 
