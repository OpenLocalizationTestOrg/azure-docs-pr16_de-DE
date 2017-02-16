<properties
    pageTitle="Konfigurieren Sie einen benutzerdefinierten Domänennamen für eine Web-app in Azure-App-Dienst, den Datenverkehr-Manager für den Lastenausgleich verwendet."
    description="Verwenden eines benutzerdefinierten Domänennamens für eine eine Web-app in Azure-App-Dienst, den Datenverkehr-Manager für den Lastenausgleich enthält."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Konfigurieren einen benutzerdefinierten Domänennamen für eine Web-app in Azure-App-Verwaltungsdienst mit den Datenverkehr-Manager

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[AZURE.INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Dieser Artikel enthält die allgemeine Anweisungen für einen benutzerdefinierten Domänennamen mit Azure-App-Dienst verwenden, die Datenverkehr-Manager für den Lastenausgleich verwenden.

[AZURE.INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>
## <a name="understanding-dns-records"></a>Grundlegendes zu DNS-Einträge

[AZURE.INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>
## <a name="configure-your-web-apps-for-standard-mode"></a>Konfigurieren von Web apps für Standardmodus

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>
## <a name="add-a-dns-record-for-your-custom-domain"></a>Hinzufügen eines DNS-Eintrags für Ihre benutzerdefinierte Domäne

> [AZURE.NOTE] Wenn Sie die Domäne durch Azure App Dienst Web Apps erworben haben überspringen vor, und finden Sie im letzten Schritt des Artikels [Kaufen Domäne für Web Apps](custom-dns-web-site-buydomains-web-app.md) .

Um eine Web app im App-Verwaltungsdienst Azure Ihrer benutzerdefinierte Domäne zuzuordnen, müssen Sie hinzufügen ein neues Eintrags in der DNS-Tabelle für Ihre benutzerdefinierte Domäne mithilfe von Tools von der domänenregistrierungsstelle, die Sie Ihren Domänennamen aus erworben. Gehen Sie folgendermaßen vor, suchen und verwenden Sie die DNS-Tools.

1. Melden Sie sich bei Ihrem Konto bei Ihrer domänenregistrierungsstelle, und suchen Sie nach einer Seite für die Verwaltung von DNS-Einträge. Suchen Sie nach Links oder Bereiche der Website mit **Domänennamen**, **DNS**oder **Name Server Management**bezeichnet. Häufig ein Link zu dieser Seite finden Sie Ihre Kontoinformationen anzeigen, und suchen Sie nach einem Link wie **My Domains**.

1. Nachdem Sie die Seite "Verwaltung" für Ihren Domänennamen gefunden haben, suchen Sie einen Link, der Sie die DNS-Einträge bearbeiten kann. Möglicherweise als **Zonendatei**, **DNS-Einträge**, oder als Link Konfiguration **Erweitert** aufgeführt sein.

    * Die Seite kann einige Datensätze, die bereits erstellt haben, wie etwa ein Eintrag wird in den meisten Fällen sind '**@**'oder'\*' mit einer Seite 'Domäne Parken'. Sie können auch Einträge für häufige Unterdomänen wie **"www"**enthalten.
    * Die Seite erwähnen **CNAME-Einträge**, oder Bereitstellen eine Dropdownliste um einen Datensatz vom Typ auszuwählen. Sie können auch anderen Datensätzen wie **eine Datensätze** und **MX-Einträge**erwähnen. In einigen Fällen werden CNAME-Einträge von anderen Namen wie eines **Aliasressourceneintrags**aufgerufen werden.
    * Die Seite außerdem Felder, die Sie zu einem anderen Domänennamen auf **Karte** aus einem **Hostname** oder **Domänennamen** zulassen.

1. Während die Einzelheiten jeder Registrierungsstelle unterschiedlich sein, im Allgemeinen Sie zuordnen *aus* Ihren benutzerdefinierten Domänennamen (beispielsweise **"contoso.com"**) *auf* den Datenverkehr Manager Domänennamen (**contoso.trafficmanager.net**), die für Ihre Web-app verwendet wird.

    > [AZURE.NOTE] Wenn ein Datensatz bereits verwendet wird, und Sie Ihre apps präemptiv gebunden müssen, können Sie alternativ einen zusätzlichen CNAME-Eintrag erstellen. Beispielsweise präemptiv erstellen Sie **www.contoso.com** bei der Web-app zum Binden einen CNAME-Eintrag aus **awverify.www** zu **contoso.trafficmanager.net**. Sie können "www.contoso.com" klicken Sie dann auf Web App hinzufügen, ohne "Www"-CNAME-Eintrag ändern. Weitere Informationen finden Sie unter [Erstellen von DNS-Einträge für eine Web-app in einer benutzerdefinierten Domäne][CREATEDNS].

1. Nachdem Sie das Hinzufügen oder Ändern von DNS-Einträge bei Ihrer Registrierungsstelle abgeschlossen haben, speichern Sie die Änderungen vor.

<a name="enabledomain"></a>
## <a name="enable-traffic-manager"></a>Aktivieren Sie den Datenverkehr-Manager

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter der [Node.js Developer Center](/develop/nodejs/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
