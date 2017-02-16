<properties
    pageTitle="Azure-Active Directory-B2C: Übersicht | Microsoft Azure"
    description="Entwickeln, Consumer zugängliche Anwendung mit Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications"></a>Azure Active Directory B2C: Registrieren Sie, und melden Sie sich Nutzer in Ihrer Anwendung

Azure Active Directory B2C ist eine umfassende Cloud Identität Lösung für Ihre Consumer zugänglichen Web und Windows-Dienste. Es ist ein hoch verfügbaren globalen Dienst, der von Millionen von Consumer Identitäten skaliert. Auf einem Enterprise-Noten sichere Plattform integriert, bleibt Azure Active Directory B2C der Anwendung, Ihr Unternehmen und Ihre Consumer geschützt.

In der Vergangenheit Anwendungsentwickler zum Registrieren, und melden Sie sich in ihre Programme Nutzer würde eigene Code geschrieben haben. Und sie möchten verwendet haben lokale Datenbanken oder Systeme Benutzernamen und Kennwörter gespeichert. Azure Active Directory B2C ermöglicht Entwicklern ein besseres Consumer Identitätsmanagement in ihre Programme mithilfe von eine sichere und standardisierten Plattform und eine umfangreiche Menge von extensible Richtlinien integriert werden soll. Wenn Sie Azure Active Directory B2C verwenden, können Ihre Consumer für die Anwendung anhand ihrer vorhandenen sozialen Benutzerkonten (Facebook, Google, Amazon, LinkedIn) oder durch Erstellen von neuen Anmeldeinformationen (e-Mail-Adresse und Ihr Kennwort ein, oder Benutzername und Kennwort); anmelden Wir rufen Sie die letzteren "lokalen Konten."

## <a name="get-started"></a>Erste Schritte

Zum Erstellen einer Anwendung, die akzeptiert Consumer Signieren von und anmelden, Sie müssen die Anwendung mit einer Azure Active Directory B2C registrieren zunächst tenant wird. Rufen Sie Ihrer eigenen Mandanten mithilfe der Schritte in [Erstellen einer B2C von Azure AD-Mandanten ab](active-directory-b2c-get-started.md).

Ihrer Anwendung gegen den Azure Active Directory B2C-Dienst können entweder durch Auswahl von Protokollnachrichten direkt senden mithilfe eines [OAuth 2.0](active-directory-b2c-reference-protocols.md#oauth2-authorization-code-flow) oder [Geöffneten ID verbinden](active-directory-b2c-reference-protocols.md#openid-connect-sign-in-flow), oder mit unseren Bibliotheken auf die Arbeit erledigen geschrieben werden. Wählen Sie Ihre bevorzugten Plattform in der folgenden Tabelle aus, und legen Sie los.

[AZURE.INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## <a name="whats-new"></a>Was ist neu

Überprüfen Sie wieder so oft zukünftigen Änderungen an der Azure-Active Directory B2C wissen. Wir werden auch über alle Updates tweet, mithilfe von @AzureAD.

- Erfahren Sie, Informationen zu unseren [extensible Policy Framework](active-directory-b2c-reference-policies.md) und zu den Arten von Richtlinien, die Sie erstellen und in Ihrer Anwendung verwenden können.
- Lesezeichen Sie unserem [Service-Blog](https://blogs.msdn.microsoft.com/azureadb2c/) für Benachrichtigungen auf Nebenversionen Dienstprobleme, Updates, Status und Problembehebungen. Fahren Sie mit sowie dem [Dashboard Azure Status](https://azure.microsoft.com/status/) zu überwachen.
- Aktuelle [diensteinschränkungen, Einschränkungen, und Einschränkungen](active-directory-b2c-limitations.md).
- Schließlich einer [Stichprobe Code](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c) Azure AD B2C und ASP.NET Core verwenden.

## <a name="how-to-articles"></a>Artikel mit Vorgehensweisen

Erfahren Sie, wie Sie Besonderheiten Azure Active Directory B2C verwenden:

- Konfigurieren Sie [Facebook](active-directory-b2c-setup-fb-app.md), [Google +](active-directory-b2c-setup-goog-app.md), [Microsoft-Konto](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md)und [LinkedIn](active-directory-b2c-setup-li-app.md) -Konten für die Verwendung in Ihrer Anwendung Consumer zugänglichen.
- [Verwenden von benutzerdefinierten Attributen zum Sammeln von Informationen über Ihre Nutzer](active-directory-b2c-reference-custom-attr.md).
- [Aktivieren von Azure kombinierte Authentifizierung in Clientanwendungen Consumer zugänglichen](active-directory-b2c-reference-mfa.md).
- [Einrichten von Self-service Kennwortrücksetzung für Ihre Nutzer](active-directory-b2c-reference-sspr.md).
- [Anpassen der Look And Feel melden Sie sich anmelden, nach oben und andere Consumer zeigende Seiten](active-directory-b2c-reference-ui-customization.md) , die Azure Active Directory B2C realisiert werden.
- [Verwenden der Azure Active Directory Graph-API programmgesteuert erstellen, lesen, aktualisieren und Löschen von Nutzer](active-directory-b2c-devquickstarts-graph-dotnet.md) in Ihrem Mandanten Azure Active Directory B2C.

## <a name="next-steps"></a>Nächste Schritte

Diese Links werden für den Dienst ein tieferer Einblick untersuchen hilfreich sein:

- Sehen Sie die [Preisinformationen Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
- Hilfe zum Überlauf Stapel mithilfe des [Azure-Active Directory](http://stackoverflow.com/questions/tagged/azure-active-directory) oder [adal](http://stackoverflow.com/questions/tagged/adal) Kategorien.
- Geben Sie uns Ihre Meinung mit [Benutzer Stimme](https://feedback.azure.com/forums/169401-azure-active-directory/)– wir möchten sie hören! Verwenden Sie den Ausdruck "AzureADB2C:" in den Titel für Ihren Beitrag, damit wir gefunden werden kann.
- Überprüfen Sie den [Azure AD B2C Protokoll Bezug](active-directory-b2c-reference-protocols.md).
- Überprüfen Sie den [Azure AD B2C Token Bezug](active-directory-b2c-reference-tokens.md).
- Der [Azure-Active Directory B2C häufig gestellte Fragen](active-directory-b2c-faqs.md)zu lesen.
- [Datei für Azure Active Directory B2C Anfragen zu unterstützen](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unseren Produkten

Wir empfehlen Ihnen erhalten von Benachrichtigungen Sicherheitsvorfälle auftreten, besuchen [Diese Seite](https://technet.microsoft.com/security/dd252948) und von Ihren Sicherheitshinweisen abonnieren.
