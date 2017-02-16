<properties 
    pageTitle="Authentifizieren mit lokalen Active Directory in Ihrer app Azure | Microsoft Azure" 
    description="Erfahren Sie mehr über die verschiedenen Optionen für Branchen apps in Azure-App-Verwaltungsdienst Authentifizierung mit lokalen Active Directory" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="web" 
    ms.date="08/31/2016" 
    ms.author="cephalin"/>

# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>Authentifizieren Sie mit lokalen Active Directory in Ihrer Azure-app #

In diesem Artikel wird gezeigt, wie mit lokalen Active Directory (AD) in der [App-Verwaltungsdienst Azure](../app-service/app-service-value-prop-what-is.md)authentifiziert werden können. Eine Azure-app in der Cloud gehostet wird, es gibt jedoch einige Methoden zum Authentifizieren AD-Benutzern sicheren lokalen. 

## <a name="authenticate-through-azure-active-directory"></a>Authentifizierung über Azure-Active Directory
Ein Azure Active Directory-Mandanten kann mit einem lokalen Verzeichnis synchronisiert werden AD. Dieser Ansatz ermöglicht AD-Benutzern den Zugriff auf Ihre App aus dem Internet, und authentifizieren ihre Anmeldeinformationen lokal verwenden. Darüber hinaus bietet Azure-App-Verwaltungsdienst eine [einsatzbereiten Lösung für diese Methode](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Mit wenigen Mausklicks eine Schaltfläche können Sie mit einem Mandanten Directory synchronisiert Authentifizierung für Ihre app Azure aktivieren. Dieser Ansatz weist die folgenden Vorteile:

-   Erfordert eine Authentifizierungscode in Ihrer app keine. Lassen Sie die App-Verwaltungsdienst führen Sie die Authentifizierung für Sie und Ihre Zeit für die Bereitstellung von Funktionalität in Ihrer app.
-   [Azure AD Graph-API](http://msdn.microsoft.com/library/azure/hh974476.aspx) aktiviert den Zugriff auf Directory-Daten aus der Azure-app.
-   Bietet SSO auf [Alle Programme, die von Azure Active Directory unterstützt](/marketplace/active-directory/), einschließlich Office 365, Dynamics CRM Online, Microsoft Intune und Tausende von nicht-Microsoft Cloudanwendungen. 
-   Azure-Active Directory unterstützt rollenbasierte Access-Steuerelement. Sie können das Muster [Authorize(Roles="X")] mit minimalen Änderungen dem Code verwenden.

So schreiben Sie eine Azure Line-of-Business-app, die mit Azure Active Directory authentifiziert finden Sie unter [Erstellen einer Azure - Branchen-app mit Azure-Active Directory-Authentifizierung](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Authentifizierung über einen lokalen STS
Wenn Sie eine lokale secure token Service (STS) wie Active Directory Federation Services (AD FS) verfügen, können Sie die Authentifizierung für Ihre app Azure Föderation verwenden. Dieser Ansatz empfiehlt sich, wenn Firmenrichtlinie verhindert, dass AD-Daten in Azure gespeichert wird. Beachten Sie jedoch Folgendes ein:

-   STS Suchtopologie muss bereitgestellten lokal mit Kosten und Verwaltung Verwaltungsaufwand.
-   Nur AD FS-Administratoren können [sich verlassen Partei-vertraut und Anfordern von Regeln](http://technet.microsoft.com/library/dd807108.aspx), konfigurieren die Optionen des Entwicklers einschränken können. Andererseits, ist es möglich, verwalten und Anpassen von [Ansprüchen](http://technet.microsoft.com/library/ee913571.aspx) auf Basis pro Anwendung.
-   Zugriff auf lokale AD-Daten erfordert eine separate Lösung durch die Firewall Ihres Unternehmens.

So schreiben Sie eine Azure Line-of-Business-app, die authentifiziert mit einem lokalen STS finden Sie unter [Erstellen einer app Branchen Azure, mit dem AD FS-Authentifizierung](web-sites-dotnet-lob-application-adfs.md).
 
