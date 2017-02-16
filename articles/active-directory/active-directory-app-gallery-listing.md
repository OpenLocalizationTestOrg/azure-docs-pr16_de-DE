<properties
   pageTitle="Auflisten von Ihrer Anwendung in der Galerie Azure Active Directory"
   description="Wie eine Anwendung aufgelistet, die im Katalog Azure Active Directory einmaliges Anmelden unterstützt | Microsoft Azure"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>


# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Auflisten von Ihrer Anwendung in der Galerie Azure Active Directory

Wenn Sie eine Anwendung aufzulisten, die im [Katalog Azure AD-](https://azure.microsoft.com/marketplace/active-directory/all/)einmaliges Anmelden mit Azure Active Directory unterstützt, muss die Anwendung eine der folgenden Integration Modi implementieren:

* **Verbinden von OpenID** - direkte Integration mit Azure AD für Authentifizierung und der Zustimmung Azure AD-API für Konfiguration mit OpenID verbinden. Wenn Sie nur eine Integration beginnen und Ihrer Anwendung SAML nicht unterstützt, ist dies der Modus empfohlen.

* **SAML** – die Anwendung bereits weist die Möglichkeit zum Konfigurieren von Drittanbietern Identitätsanbieter mit dem SAML-Protokoll.

Auflistung Anforderungen für jeden Modus sind unter.

##<a name="openid-connect-integration"></a>OpenID verbinden Integration

Eine Anwendung mit Azure AD, folgen den [Anweisungen Entwicklertools](active-directory-authentication-scenarios.md)integriert werden soll. Klicken Sie dann führen Sie die folgenden Fragen und Senden an waadpartners@microsoft.com.

* Bereitstellen von Anmeldeinformationen für ein Konto oder Test-Mandanten mit der Anwendung, die vom Azure AD-Team zum Testen der Integration verwendet werden kann.  

* Bereitstellen von Anweisungen auf wie das Team Azure AD-anmelden kann, und verbinden eine Instanz der Azure AD an Ihrer Anwendung mithilfe der [Azure AD Zustimmung Framework](active-directory-integrating-applications.md#overview-of-the-consent-framework). 

* Bieten Sie etwaigen weiteren Anweisungen für das Team Azure AD-einmaliges Anmelden mit Ihrer Anwendung testen erforderlich ist. 

* Geben Sie die folgenden Informationen ein:

> Name des Unternehmens:
> 
> Website für Unternehmen:
> 
> Name der Anwendung:
> 
> Beschreibung der Anwendung (Beschränkung auf 256 Zeichen):
> 
> Anwendung Website (Information):
> 
> Anwendung technischen Support-Website oder Kontaktinformationen:
> 
> Client-ID der Anwendung, wie die Anwendung Details bei https://manage.windowsazure.com dargestellt:
> 
> Anmeldung URL-Kunden, wechseln Sie zum Anmelden bei Anwendung und und/oder erwerben die Anwendung:
> 
> Wählen Sie bis zu drei Feldkategorien für eine Anwendung (für verfügbare Kategorien finden Sie unter dem Azure Active Directory Marketplace) unter aufgeführt:
> 
> Fügen Sie kleine Anwendungssymbol (PNG-Datei, 45px durch 45px, einfarbige Hintergrundfarbe) an:
> 
> Fügen Sie große Anwendungssymbol (PNG-Datei, 215px durch 215px, einfarbige Hintergrundfarbe) an:
> 
> Fügen Sie die Anwendung Logo (PNG-Datei, 150px durch 122px, transparente Hintergrundfarbe) an:

##<a name="saml-integration"></a>SAML-Integration

Eine beliebige app, die SAML 2.0 unterstützt kann direkt mit einem Azure AD-Mandanten anhand der [folgenden Schritte zum Hinzufügen einer benutzerdefinierten Anwendungs](active-directory-saas-custom-apps.md)integriert werden. Nachdem Sie getestet haben, dass Ihre Anwendungsintegration mit Azure AD funktioniert, senden Sie die folgende Informationen zu <waadpartners@microsoft.com>.

* Bereitstellen von Anmeldeinformationen für ein Konto oder Test-Mandanten mit der Anwendung, die vom Azure AD-Team zum Testen der Integration verwendet werden kann.  

* Geben Sie die SAML anmelden URL, Herausgeber-URL (Entität-ID) und Antwort-URL (Assertion Consumer Service)-Werte für eine Anwendung, als beschrieben [hier](active-directory-saas-custom-apps.md)ein. Wenn Sie diese Werte in der Regel als Teil eines SAML-Metadaten-Datei bereitstellen, dann senden Sie uns bitte, die ebenfalls.

* Geben Sie eine kurze Beschreibung der Azure AD als Identitätsanbieter in Ihrer Anwendung mithilfe von SAML 2.0 konfigurieren. Wenn Ihre Anwendung konfigurieren von Azure AD als Identitätsanbieter über ein administrative Self-service-Portal unterstützt, klicken Sie dann stellen Sie sicher, dass die oben angegebenen Anmeldeinformationen die Möglichkeit zur Einrichtung dieser einschließen.

* Geben Sie die folgenden Informationen ein:

> Name des Unternehmens:
> 
> Website für Unternehmen:
> 
> Name der Anwendung:
> 
> Beschreibung der Anwendung (Beschränkung auf 256 Zeichen):
> 
> Anwendung Website (Information):
> 
> Anwendung technischen Support-Website oder Kontaktinformationen:
> 
> Anmeldung URL-Kunden, wechseln Sie zum Anmelden bei Anwendung und und/oder erwerben die Anwendung:
> 
> Wählen Sie bis zu drei Feldkategorien für eine Anwendung unter (für verfügbare Kategorien finden Sie unter dem [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/)) aufgelistet werden):
> 
> Fügen Sie kleine Anwendungssymbol (PNG-Datei, 45px durch 45px, einfarbige Hintergrundfarbe) an:
> 
> Fügen Sie große Anwendungssymbol (PNG-Datei, 215px durch 215px, einfarbige Hintergrundfarbe) an:
> 
> Fügen Sie die Anwendung Logo (PNG-Datei, 150px durch 122px, transparente Hintergrundfarbe) an:
