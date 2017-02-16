<properties
    pageTitle="OAUTH Sicherheit in SaaS Verbindern und API Apps | Azure"
    description="Informationen Sie zu OAUTH Sicherheit Verbindern und API-Apps in Azure-App-Verwaltungsdienst; Microservices Architektur; SaaS"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="dwrede"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="mandia"/>


# <a name="learn-about-oauth-security-in-saas-connectors"></a>Informieren Sie sich über die Sicherheit OAUTH SaaS Verbinder

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2014-12-01-Schema Vorschauversion aus.

Viele der Software als einen Verbinder Service (SaaS) wie Facebook, Twitter, DropBox usw. Benutzern die Authentifizierung über das Protokoll OAUTH erforderlich.  Wenn Sie diese Connectors SaaS aus Logik Apps verwenden, stellen wir eine vereinfachte Benutzerfunktionalität, wo Sie im Designer Logik Apps "Autorisieren" klicken. Wenn Sie **Autorisieren**, werden Sie aufgefordert, melden Sie sich (sofern nicht bereits), und geben Sie die Zustimmung für die Verbindung zum Dienst SaaS in Ihrem Auftrag. Nachdem Sie Zustimmung bereitstellen und autorisieren, können Ihre Apps Logik dann diese SaaS-Dienste zugreifen.

## <a name="create-your-own-saas-app"></a>Erstellen Sie eigener SaaS-app
Diese einfacheres ist möglich, da wir zuvor erstellt und die Anwendung in dieser Dienste SaaS registriert.  In bestimmten Fällen möchten Sie möglicherweise registrieren und Ihre eigene Anwendung verwenden.  Hier erforderlich, beispielsweise wenn Sie diese Connectors SaaS in einer benutzerdefinierten Anwendung verwenden möchten. In diesem Beispiel wird des Verbinders DropBox, aber der Vorgang ist für alle Verbinder, die auf OAUTH aufsetzen.

Auch im Kontext Logik Apps können Sie Ihre eigene Anwendung anstelle von der Standard-Anwendung, die wir erläutern die notwendigen verwenden. Wenn die Schaltfläche "Autorisieren" kann keine Verbindung herstellen, können Sie versuchen, eine eigene app erstellen. Die folgende Liste enthält Schritte für die Twitter-Verbinder aus:

1. Öffnen Sie den Verbinder Twitter Azure Preview-Portal an. Wechseln Sie zur **Suche** > **API Apps**. Wählen Sie den Verbinder Twitter aus:  
    ![][1]

2. Wählen Sie **Einstellungen**aus > **Authentifizierung**:  
    ![][2]

3. Kopieren Sie den Wert für die **URI umleiten** :  
    ![][3]

4. Wechseln Sie zu [Twitter](http://apps.twitter.com) und **Erstellen Sie eine neue App**. Fügen Sie in der Eigenschaft **Rückruf URL** **Umleiten URI** -Werts aus den Verbinder Twitter kopiert haben: ![][4]  
5. Wenn Ihre Twitter-app erstellt wird, wählen Sie **Schlüssel und Access Token**aus. Kopieren Sie die folgenden Werte.
6. Fügen Sie Ihre Einstellungen Twitter-Authentifizierung Verbinder diese Werte in den Eigenschaften **Client-ID** und **Client geheim** :   
    ![][5]  
7. Speichern Sie Ihre verbindereinstellungen.  

Nun sollten Sie den Verbinder aus Logik Apps verwenden können. Wenn Sie diesen Connector von Logik Apps verwenden, wird eine Anwendung anstelle der Standard-Anwendung verwendet.  

> [AZURE.NOTE] Wenn Sie eine app zuvor autorisiert haben, müssen Sie möglicherweise die app autorisieren möchten.


<!--Image references-->
[1]: ./media/app-service-logic-oauth-security/TwitterConnector.png
[2]: ./media/app-service-logic-oauth-security/Authentication.png
[3]: ./media/app-service-logic-oauth-security/RedirectURI.png
[4]: ./media/app-service-logic-oauth-security/TwitterApp.png
[5]: ./media/app-service-logic-oauth-security/TwitterKeys.png
