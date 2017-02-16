<properties 
    pageTitle="Das Debuggen SAML-basierten einmaliges Anmelden auf Anwendungen in Azure Active Directory | Microsoft Azure" 
    description="Erfahren Sie, wie Debuggen SAML-basierten einmaliges Anmelden auf Anwendungen in Azure Active Directory " 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>So debuggen SAML-basierten einmaliges Anmelden auf Anwendungen in Azure Active Directory

Wenn Sie eine SAML-basierten Anwendungsintegration beseitigen, empfiehlt es sich häufig mithilfe ein Tools wie [Fiddler](http://www.telerik.com/fiddler) finden Sie unter der SAML-Anforderung, die Antwort SAML und der tatsächlichen SAML-Token, das mit der Anwendung ausgestellt wurde. Untersuchen des SAML-Tokens, können Sie sicherstellen, dass alle erforderlichen Attribute, der Benutzername in den SAML-Betreff und den Herausgeber URI bis vertraut sind, wie erwartet.

![][1]

Die Antwort aus dem Azure Active Directory, die das SAML-Token enthält ist in der Regel das Schema aus, das nach einer HTTP 302 Umleiten von https://login.windows.net und wird gesendet, um die konfigurierten **Antworten URL** der Anwendung auftritt. 
 
Sie können das SAML-Token, indem diese Zeile dann auswählen und Anzeigen der **Inspektoren > Web Forms** Registerkarte im rechten Bereich. Klicken Sie hier mit der rechten Maustaste in des Werts **SAMLResponse** , und wählen Sie **Senden an TextWizard**. Wählen Sie dann **Aus Base64** aus dem Menü **Transformieren** , um das Token entschlüsseln und deren Inhalt angezeigt.
 
**Hinweis**: um den Inhalt dieser HTTP-Anforderung anzuzeigen, Fiddler fordert Sie möglicherweise so konfigurieren Sie Entschlüsseln des HTTPS-Datenverkehr, die Sie ausführen müssen.

## <a name="related-articles"></a>Verwandte Artikel

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
- [Konfigurieren von einmaliges Anmelden auf Anwendungen, die nicht in der Galerie Azure Active Directory sind](active-directory-saas-custom-apps.md)
- [Wie Sie in der SAML-Token für integrierte Apps ausgestellt Ansprüche anpassen](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png
