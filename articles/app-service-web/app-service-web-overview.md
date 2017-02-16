<properties
    pageTitle="Web Apps-Übersicht | Microsoft Azure"
    description="Erfahren Sie, wie Azure-App-Verwaltungsdienst können Sie bei der Entwicklung und Host Webanwendungen"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="cephalin"/>

# <a name="web-apps-overview"></a>Web Apps (Übersicht)

*App Dienst Web Apps* ist eine vollständig verwaltete berechnen-Plattform, die für das Hosten von Websites und Web-Applikationen optimiert ist. Dieses Angebot [Plattform als Service](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) von Microsoft Azure ermöglicht konzentrieren Sie Ihre Geschäftslogik während Azure sorgt dafür, dass die Infrastruktur ausführen und Ihre apps skalieren.

Das folgende Video 5 Minuten führt Azure App Dienst Web Apps.

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="what-is-a-web-app-in-app-service"></a>Was ist eine Web app im App-Dienst?

In UTC-App ist eine *Web app* die berechnen-Ressourcen, die Azure bietet für eine Website oder Web-Anwendung zu hosten.  

Die Ressourcen berechnen möglicherweise auf freigegebenen oder dedizierten virtuellen (virtuelle Computer), je nach der Preisgestaltung Ebene, die Sie auswählen. Der Code der Anwendung eines verwalteten virtuellen Computers, die von anderen Kunden isoliert ist wird ausgeführt.

Code kann in allen Sprachen oder Framework, das von der [App-Verwaltungsdienst Azure](../app-service/app-service-value-prop-what-is.md), wie z. B. ASP.NET, Node.js, Java, PHP oder Python unterstützt wird. Sie können auch Skripts ausführen, die [PowerShell und Skript-Sprachen](web-sites-create-web-jobs.md#acceptablefiles) in einer Web app verwenden.

Beispiele für normalen Anwendungsszenarien, denen Sie Web-Apps für verwenden können, finden Sie unter [Web app Szenarien](https://azure.microsoft.com/documentation/scenarios/web-app/) und im Abschnitt **Szenarien und Empfehlungen im Zusammenhang mit** der [App-Verwaltungsdienst Azure, virtuellen Computern, Service-Fabric und Cloud Services-Vergleich](choose-web-site-cloud-service-vm.md#scenarios).

## <a name="why-use-web-apps"></a>Gründe für die Verwendung der Web Apps

Hier sind einige der wichtigen Features der App-Dienst, die auf Web Apps anzuwenden:

- **Mehrere Sprachen und Framework** - App-Dienst verfügt über erster Klasse Unterstützung für ASP.NET, Node.js, Java, PHP und Python aus. Sie können auch [PowerShell und andere Skripts oder Programmdateien](../app-service-web/web-sites-create-web-jobs.md) auf App-Dienst virtuellen Computern ausführen.

- **Optimierung der DevOps** - richten Sie [fortlaufende Integration und Bereitstellung](../app-service-web/app-service-continuous-deployment.md) mit Visual Studio Team Services, GitHub oder BitBucket. Höherstufen Sie Updates über [Test- und staging-Umgebungen](../app-service-web/web-sites-staged-publishing.md). Führen Sie [A / B testen](../app-service-web/app-service-web-test-in-production-get-start.md). Verwalten Sie Ihre apps im App-Dienst mithilfe der [PowerShell Azure](../powershell-install-configure.md) oder die [Plattformen line Interface (CLI)](../xplat-cli-install.md).

- **Global mit hoher Verfügbarkeit skalieren** - Skala [nach oben](../app-service-web/web-sites-scale.md) oder [Verkleinern](../monitoring-and-diagnostics/insights-how-to-scale.md) manuell oder automatisch. Hosten Ihrer apps an einer beliebigen Stelle in der Microsoft globale Datacenter Infrastruktur und der App-Verwaltungsdienst [Vereinbarung zum SERVICELEVEL](https://azure.microsoft.com/support/legal/sla/app-service/) legt hohen Verfügbarkeit fest.

- **Verbindungen mit SaaS Plattformen und lokaler Daten** - von mehr als 50 [Verbinder](../connectors/apis-list.md) für Enterprise-Systeme (z. B. SAP, Siebel und Oracle), wählen Sie SaaS-Dienste (z. B. Vertrieb und Office 365) und Internet-Dienste (wie etwa Facebook oder Twitter). Zugriff auf lokale Daten mithilfe von [Hybrid Verbindungen](../biztalk-services/integration-hybrid-connection-overview.md) und [Azure-virtuellen Netzwerken](../app-service-web/web-sites-integrate-with-vnet.md).

- **Sicherheits- und Compliance** - App Service ist [ISO, SOC, und PCI-Compliance](https://www.microsoft.com/TrustCenter/).

- **Vorlagen in der Anwendung** – wählen Sie aus einer umfangreichen Liste von Vorlagen in der Anwendung in [Azure Marketplace](https://azure.microsoft.com/marketplace/) , mit denen Sie mithilfe eines Assistenten wie WordPress, Joomla und Drupal beliebte Open Source-Software installieren zu können.

- Die Arbeit der erstellen, bereitstellen und Debuggen **visual Studio Integration** - dedizierten Tools in Visual Studio zu optimieren.

Darüber hinaus kann eine Web app nutzen Features Angeboten durch [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (z. B. CORS-Unterstützung) und [Mobile-Apps](../app-service-mobile/app-service-mobile-value-prop.md) (z. B. Pushbenachrichtigungen). Weitere Informationen zu Datentypen der app im App-Dienst finden Sie unter [Übersicht über die App-Verwaltungsdienst Azure](../app-service/app-service-value-prop-what-is.md).

Zusätzlich zu Web Apps in App-Dienst bietet Azure andere Dienste, die für das Hosten von Websites und Web-Applikationen verwendet werden können. In den meisten Fällen ist Web Apps die beste Option dar.  Microservice Architektur erwägen Sie [Dienst Fabric](https://azure.microsoft.com/documentation/services/service-fabric), und wenn Sie mehr Kontrolle über die virtuellen Computern, die von Code ausgeführt wird benötigen, klicken Sie auf, sollten Sie sich mit dem [Azure-virtuellen Computern](https://azure.microsoft.com/documentation/services/virtual-machines/). Weitere Informationen dazu, wie Sie zwischen diesen Azure Services auswählen finden Sie unter [Azure-App-Verwaltungsdienst virtuellen Computern, Fabric Service, Vergleich, und Cloud Services](choose-web-site-cloud-service-vm.md).

## <a name="getting-started"></a>Erste Schritte

Führen Sie zum Beispiel-Code auf eine neue Web app im App-Dienst bereitzustellen anzufangen, des Lernprogramms [Bereitstellen Ihre erste Azure in 5 Minuten Web app](app-service-web-get-started.md) an. Benötigen Sie ein kostenloses Azure-Konto ein.

Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.
