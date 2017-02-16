<properties
    pageTitle="Azure App-Verwaltungsdienst für das Web und Mobile API apps | Microsoft Azure"
    description="Erfahren Sie, wie Azure-App-Verwaltungsdienst hilft Ihnen die entwickeln, bereitstellen und Verwalten von Web und mobile-apps."
    keywords="App-Dienst, Azure app Dienst, app-Verwaltungsdienst Kosten, skalierbare skalieren, app-Bereitstellung, Azure app-Bereitstellung, Paas, Plattform als Dienst, Website, Website, Azure mobile web"
    services="app-service"
    documentationCenter=""
    authors="omarkmsft"
    manager="erikre"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/26/2016"
    ms.author="omark"/>

# <a name="what-is-azure-app-service"></a>Was ist die App-Verwaltungsdienst Azure?

*App-Dienst* ist ein [Plattform-als-Service](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) von Microsoft Azure Geschenk. Erstellen von Web und mobile apps Plattform oder Gerät. Integrieren Ihrer apps SaaS Lösungen, Verbinden mit lokalen Applikationen und Ihre Geschäftsprozesse automatisieren. Azure wird Ihre apps auf vollständig verwalteten virtuellen Computern (virtuelle Computer), mit Ihrer Wahl freigegebenen virtuellen Computer Ressourcen oder dedizierten virtuellen Computern ausgeführt.

App-Dienst einschließt im Web und mobile-Funktionen, die wir zuvor separat als Azure-Websites und Azure Mobile Dienste zur Verfügung. Darüber hinaus neue Funktionen zum Automatisieren von Geschäftsprozessen und Cloud-APIs hosten. Als einen einzelnen integrierten Dienst bietet Ihnen ein App-Dienst Verfassen von verschiedenen Komponenten – Websites, Back-End mobile-app, Rest-APIs und Geschäftsprozesse – zu einer einzigen Lösung.

Das folgende 4-minütiges Video enthält eine kurze Beschreibung der Beziehung der App-Dienst zu früheren Azure Angebote und darin Neuigkeiten.

+[AZURE.VIDEO app-service-history-lesson]

## <a name="why-use-app-service"></a>Gründe für die Verwendung von App-Dienst

Hier sind einige der wichtigsten Features und Funktionen von App-Dienst:

- **Mehrere Sprachen und Framework** - App-Dienst verfügt über erster Klasse Unterstützung für ASP.NET, Node.js, Java, PHP und Python aus. Sie können auch [Windows PowerShell und andere Skripts oder Programmdateien](../app-service-web/web-sites-create-web-jobs.md) auf App-Dienst virtuellen Computern ausführen.

- **Optimierung der DevOps** - richten Sie [fortlaufende Integration und Bereitstellung](../app-service-web/app-service-continuous-deployment.md) mit Visual Studio Team Services, GitHub oder BitBucket. Höherstufen Sie Updates über [Test- und staging-Umgebungen](../app-service-web/web-sites-staged-publishing.md). Führen Sie [A / B testen](../app-service-web/app-service-web-test-in-production-get-start.md). Verwalten Sie Ihre apps im App-Dienst mithilfe der [PowerShell Azure](../powershell-install-configure.md) oder die [Plattformen line Interface (CLI)](../xplat-cli-install.md).

- **Global mit hoher Verfügbarkeit skalieren** - Skala [nach oben](../app-service-web/web-sites-scale.md) oder [Verkleinern](../monitoring-and-diagnostics/insights-how-to-scale.md) manuell oder automatisch. Hosten Ihrer apps an einer beliebigen Stelle in der Microsoft globale Datacenter Infrastruktur und der App-Verwaltungsdienst [Vereinbarung zum SERVICELEVEL](https://azure.microsoft.com/support/legal/sla/app-service/) legt hohen Verfügbarkeit fest.

- **Verbindungen mit SaaS Plattformen und lokaler Daten** - von mehr als 50 [Verbinder](../connectors/apis-list.md) für Enterprise-Systeme (z. B. SAP, Siebel und Oracle), wählen Sie SaaS-Dienste (z. B. Vertrieb und Office 365) und Internet-Dienste (wie etwa Facebook oder Twitter). Zugriff auf lokale Daten mithilfe von [Hybrid Verbindungen](../biztalk-services/integration-hybrid-connection-overview.md) und [Azure-virtuellen Netzwerken](../app-service-web/web-sites-integrate-with-vnet.md).

- **Sicherheits- und Compliance** - App Service ist [ISO, SOC, und PCI-Compliance](https://www.microsoft.com/TrustCenter/).

- **Vorlagen in der Anwendung** – wählen Sie aus einer umfangreichen Liste von Vorlagen in der Anwendung in [Azure Marketplace](https://azure.microsoft.com/marketplace/) , mit denen Sie mithilfe eines Assistenten wie WordPress, Joomla und Drupal beliebte Open Source-Software installieren zu können.

- Die Arbeit der erstellen, bereitstellen und Debuggen **visual Studio Integration** - dedizierten Tools in Visual Studio zu optimieren.

## <a name="app-types-in-app-service"></a>Typen von App im App-Dienst

App Dienst bietet mehrere *Typen von app*, von denen jede eine bestimmte Art von Arbeitsbelastung gehostet werden soll:

- [**Web Apps**](../app-service-web/app-service-web-overview.md) - zum Hosten von Websites und Web Applications.

- [**Mobile-Apps**](../app-service-mobile/app-service-mobile-value-prop.md) Sichern Sie für mobile-app Hostinganbieter endet.

- [**API Apps**](../app-service-api/app-service-api-apps-why-best-platform.md) - zum Rest-APIs hosten.

- [**Logik Apps**](../app-service-logic/app-service-logic-what-are-logic-apps.md) - zum Automatisieren von Geschäftsprozessen und Integration von Systemen und Daten über Wolken ohne Schreiben von Code.

Word *app* Hier bezieht sich auf den Hostinganbieter Ressourcen ausschließlich eine Arbeitsbelastung ausgeführt. Aufnahme "Web app" als Beispiel können Sie wahrscheinlich gewohnten an eine Web app als Ressourcen berechnen und Code der Anwendung, die Funktionalität zusammen an einem Browser zu übermitteln. Jedoch in der App-Dienst eine *Web app* die berechnen Ressourcen, die Azure zum Hosten Ihrer Anwendungscodes enthält. Wenn eine Anwendung besteht, die von einem Web-front-End und eine REST-API back-End, könnten Sie beide mit einem Web-app bereitstellen bereitstellen oder Sie konnte Front-End-Code in einer Web app und Back-End-Code zu einer API-app. Eine Anwendung kann mehrere App Dienst Apps verschiedene Arten bestehen.

## <a name="app-service-plans"></a>App-Service-Pläne

Geben Sie die Art der berechnen Ressourcen, die auf Ihre apps ausgeführt [App Service-Pläne](azure-web-sites-web-hosting-plans-in-depth-overview.md) an. Wenn Sie light Datenverkehr erwarten, können Sie freigegebene virtuellen Computern (**frei** und **freigegebene** Ebenen Preise). Für höhere geladen wird können Sie auf verschiedene Größen dedizierten virtuellen Computern ( **Standard**,**Standard-**und **Premium** Ebenen) auswählen. Mehrere App Dienst apps können derselben Plan freigeben, und diese skalieren nach oben und unten die Preisgestaltung Ebenen zusammen in den Plan.

Wenn Sie weitere Skalierbarkeit und Netzwerkisolation benötigen, können Sie Ihre apps in einer [App-Service-Umgebung](../app-service-web/app-service-app-service-environment-intro.md)ausführen.

## <a name="pricing"></a>Preise

Informationen dazu, wie viel Kosten App-Dienst finden Sie unter [App Preisen](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>Testen Sie die App-Dienst

[Erstellen einer Stichprobe Web app, mobile-app oder Logik app](http://go.microsoft.com/fwlink/?LinkId=523751) und die Wiedergabe mit 1 Stunde, keine Kreditkartendaten erforderlich ist, keine Zusagen, unkompliziert.

Oder öffnen Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/pricing/free-trial/), und führen Sie eine der unsere Lernprogramme erste Schritte:

* [Lernprogramm: Erstellen einer Web app](../app-service-web/app-service-web-get-started.md)
* [Lernprogramm: Erstellen einer mobilen-app](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Lernprogramm: Erstellen einer API-app](../app-service-api/app-service-api-dotnet-get-started.md)
* [Lernprogramm: Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)
