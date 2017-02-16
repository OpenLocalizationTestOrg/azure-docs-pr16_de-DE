<properties
    pageTitle="Erstellen Sie eine Web-app aus dem Azure Marketplace | Microsoft Azure"
    description="Erfahren Sie, wie Sie eine neue WordPress Web app aus dem Azure Marketplace mithilfe von Azure-Portal zu erstellen."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

# <a name="create-a-web-app-from-the-azure-marketplace"></a>Erstellen Sie eine Web-app aus dem Azure Marketplace

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Die Azure Marketplace stellt eine Vielzahl von gängige Web apps, entwickelt von Microsoft, Drittanbieter Unternehmen und open-Source-Software Initiativen zur Verfügung. Beispielsweise WordPress, Umbraco CMS, Drupal, usw. Diese Web apps basieren auf einer Vielzahl von beliebte Framework, z. B. von [PHP] in dieser WordPress Beispiel, [.NET], [Node.js], [Java]und [Python], um ein paar zu nennen. Um eine Web app aus dem Azure Marketplace die einzige Software zu erstellen, die Sie benötigen wird im Browser, den Sie für das [Azure-Portal]zu verwenden.

In diesem Lernprogramm erfahren Sie, wie:

* Suchen und Erstellen von Web-app in Azure-App-Dienst, die auf einer Azure Marketplace-Vorlage basiert.
* Konfigurieren der App-Verwaltungsdienst Azure-Einstellungen für das neue Web app.
* Starten und Verwalten von Web app.

Stellen Sie innerhalb dieses Lernprogramms eine Blogwebsite WordPress aus dem Azure Marketplace bereit. Wenn Sie die Schritte in diesem Lernprogramm abgeschlossen haben, müssen Sie Ihrer eigenen Website WordPress nach oben und in der Cloud ausgeführt werden.

![Beispiel für WordPress Wep app dashboard][WordPressDashboard1]

Die WordPress-Website, die Sie in diesem Lernprogramm bereitstellen werden verwendet MySQL für die Datenbank ein. Wenn Sie stattdessen die SQL-Datenbank für die Datenbank verwenden möchten, finden Sie unter [Project Nami], die auch über die Azure Marketplace verfügbar.

> [AZURE.NOTE]
> Damit dieses Lernprogramm abgeschlossen, benötigen Sie ein Microsoft Azure-Konto an. Wenn Sie kein Konto haben, können Sie [die Vorteile Ihres Visual Studio-Abonnent aktivieren] [ activate] , oder [Melden Sie sich für eine kostenlose Testversion][free trial].
>
> Wenn Sie mit Azure-App-Verwaltungsdienst anzufangen, bevor Sie für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen]. Sie können von dort aus sofort eine kurzlebige Starter Web app erstellen, im App-Dienst – keine Kreditkarte erforderlich ist, und es werden keine Zusagen.

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Suchen Sie und erstellen Sie eine Web-App in Azure App-Verwaltungsdienst

1. Melden Sie sich bei der [Azure-Portal].

1. Klicken Sie auf **neu**.
    
    ![Erstellen einer neuen Azure Ressource][MarketplaceStart]
    
1. Suchen Sie nach **WordPress**, und klicken Sie dann auf **WordPress**. (Wenn Sie anstelle von MySQL SQL-Datenbank verwenden möchten, suchen Sie nach **Project Nami**.)

    ![Suchen nach WordPress auf der Marketplace][MarketplaceSearch]
    
1. Nach dem Lesen der Beschreibung der app WordPress, klicken Sie auf **Erstellen**.

    ![Erstellen Sie WordPress Web app][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Konfigurieren von Einstellungen zur Azure App-Verwaltungsdienst für Ihre neue Web App

1. Nachdem Sie eine neue Web app erstellt haben, wird das WordPress Einstellungen Blade angezeigt werden die verwenden Sie die folgenden Schritte ausführen:

    ![Konfigurieren von WordPress Web app-Einstellungen][ConfigStart]

1. Geben Sie einen Namen für das Web app im **Web app** -Feld ein.

    Dieser Name muss in der Domäne azurewebsites.net eindeutig sein, da die URL des Web app *{Name}*ist. azurewebsites.net. Wenn der eingegebene Name nicht eindeutig ist, wird Sie in das Textfeld ein rotes Ausrufezeichen angezeigt.

    ![Konfigurieren der WordPress Web app-name][ConfigAppName]

1. Wenn Sie mehr als ein Abonnement besitzen, wählen Sie das Element, das Sie verwenden möchten. 

    ![Konfigurieren Sie das Abonnement für das Web app][ConfigSubscription]

1. Wählen Sie eine **Ressourcengruppe** oder erstellen Sie einen neuen.

    Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht Azure Ressourcenmanager][ResourceGroups].

    ![Konfigurieren der Ressourcengruppe für das Web app][ConfigResourceGroup]

1. Wählen Sie eine **App-Dienst Plan/Speicherort** aus, oder Erstellen eines neuen Kontos.

    Weitere Informationen zur App-Service-Pläne, finden Sie unter [Übersicht über die App-Verwaltungsdienst Azure Pläne][AzureAppServicePlans]. 

    ![Konfigurieren des Serviceplans für das Web app][ConfigServicePlan]

1. Klicken Sie auf die **Datenbank**, und geben Sie dann in das **Neue MySQL-Datenbank** Blade die erforderlichen Werte für das Konfigurieren der MySQL-Datenbank.

    ein. Geben Sie einen neuen Namen ein, oder lassen Sie den Standardnamen.

    b. Lassen Sie die **Datenbanktyp** **freigegeben**.

    c. Wählen Sie am selben Speicherort wie das Element, das Sie für das Web app ausgewählt haben.

    d. Wählen Sie eine Preisgestaltung Stufe aus. In diesem Lernprogramm ist in **Quecksilber** - also mit minimalen Verbindungen und Speicherplatz kostenlosen - Ordnung.

    e. In das **Neue MySQL-Datenbank** Blade annehmen der Vertragsbedingungen, und klicken Sie dann auf **OK**. 

    ![Konfigurieren Sie die Datenbank-Einstellungen für das Web app][ConfigDatabase]

1. Klicken Sie in das Blade **WordPress** annehmen der Vertragsbedingungen, und klicken Sie dann auf **Erstellen**. 

    ![Beenden Sie die Web app-Einstellungen, und klicken Sie auf OK][ConfigFinished]

    App-Verwaltungsdienst Azure erstellt das Web-app in der Regel in weniger als einer Minute. Sie können den Fortschritt überwachen, durch Klicken auf das Glockensymbol am oberen Rand der Portalseite.

    ![Statusanzeige][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Starten und Verwalten von WordPress Web app
    
1. Wenn das Web app erstellen abgeschlossen ist, navigieren Sie im Portal Azure der Ressourcengruppe, in dem Sie die Anwendung erstellt haben, und Sie können sehen, das Web app und die Datenbank.

    Die zusätzliche Ressource mit dem Symbol Glühbirne ist [Anwendung Einsichten][ApplicationInsights], die Überwachung Services für Ihre Web app bereitstellt.

1. Klicken Sie in das Blade **Ressourcengruppe** auf die Web app Linie.

    ![Wählen Sie Ihre WordPress Web app][WordPressSelect]

1. Klicken Sie in das Web app-Blade auf **Durchsuchen**.

    ![Navigieren Sie zu Ihrem WordPress Web app][WordPressBrowse]

1. Wenn Sie aufgefordert werden, wählen Sie die Sprache für Ihren Blog WordPress, wählen Sie die gewünschte Sprache aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie die Sprache für Ihre WordPress Web app][WordPressLanguage]

1. WordPress **Willkommen** auf der Seite Geben Sie die Konfigurationsinformationen, WordPress erforderlich machen, und klicken Sie dann auf **WordPress installieren**.

    ![Konfigurieren Sie die Einstellungen WordPress Web app][WordPressConfigure]

1. Melden Sie sich mit den Anmeldeinformationen, die Sie auf der Seite **Willkommen** erstellt haben.  

1. Ihre Website Dashboardseite wird geöffnet und zeigt die von Ihnen bereitgestellten Informationen.    

    ![Anzeigen des Dashboards WordPress][WordPressDashboard2]

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie erfahren, wie das Erstellen und Bereitstellen einer Beispiel Web app aus dem Azure Marketplace angezeigt werden.

Weitere Informationen zum Arbeiten mit App-Dienst Web Apps finden Sie unter den Links auf der linken Seite der Seite (für große Browserfenster) oder am oberen Rand der Seite (für schmale Browserfenster).

Weitere Informationen zur Entwicklung von WordPress Web apps auf Azure [Entwickeln WordPress Azure-App-Dienst]finden Sie unter[WordPressOnAzure]. 

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[Wiederholen Sie den App-Dienst]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Azure-Portal]: https://portal.azure.com/
[Project-Nami]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png
