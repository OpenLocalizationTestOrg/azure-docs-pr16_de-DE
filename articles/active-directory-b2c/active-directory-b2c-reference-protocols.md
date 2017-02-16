<properties
    pageTitle="Azure-Active Directory B2C | Microsoft Azure"
    description="So erstellen Sie apps direkt mithilfe von Azure Active Directory B2C unterstützten Protokolle."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-authentication-protocols"></a>Azure AD-B2C: Protokolle für die Authentifizierung

Azure Active Directory (Azure AD) B2C Identität als Dienst für Ihre apps durch die Unterstützung von zwei Protokolle nach Industriestandard bietet: OpenID verbinden und OAuth 2.0. Der Dienst ist Standards kompatiblen, jedoch alle zwei Implementierung dieser Protokolle können raffinierten Unterschiede aufweisen.  Die Informationen in diesem Handbuch wird für Sie hilfreich sein, wenn Sie den Code schreiben, indem Sie direkt senden sowie Behandeln von HTTP-Anfragen und nicht mithilfe der open Source-Bibliothek. Es empfiehlt sich, dass Sie diese Seite lesen, bevor Sie in die Details des jedes spezifische Protokoll kümmern. Aber wenn Sie bereits mit Azure AD B2C vertraut sind, können Sie direkt zu [den Führungslinien Protokoll Bezug](#protocols)wechseln.

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>Die Grundlagen
Jeder app Azure AD B2C verwendet, die muss in Ihrem Verzeichnis B2C im [Azure-Portal](https://portal.azure.com)registriert werden. Die app Registrierung gesammelt, und Ihre app ein paar Werte zugewiesen:

- Eine **ID der Anwendung** , die Ihre app identifiziert.
- Ein **URI umleiten** oder ein **Paket Bezeichner** , direkte Antworten zu Ihrer Anwendung wieder verwendet werden können.
- Ein paar andere Szenario-spezifische Werte. Weitere Informationen Sie [zu Ihrer Anwendung zu registrieren](active-directory-b2c-app-registration.md).

Nachdem Sie Ihre app registriert haben, wird mit Azure AD kommuniziert, per Anfragen an den Endpunkt Version 2.0:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

In nahezu allen OAuth, und verbinden Sie OpenID Zahlungen umfasst vier Parteien im Exchange:

![Rollen OAuth 2.0](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

- Die **Autorisierung Server** ist der Azure AD-Version 2.0-Endpunkt. Ganzer sicher nichts im Zusammenhang mit Benutzerinformationen und Access. Es werden auch die Trust Beziehungen zwischen den Parteien in einen Fluss behandelt. Es ist zum Überprüfen der Identität des Benutzers, erteilen und Widerrufen des Zugriffs auf Ressourcen sowie zum Ausgeben von Token verantwortlich ist. Es ist auch bekannt als Identitätsanbieter.
- Der **Ressourcenbesitzer** ist normalerweise der Endbenutzer. Der Teilnehmer, die die Daten zugegriffen werden kann, und er bietet die Leistung Dritte weiter, um die Daten oder Ressourcen zugreifen dürfen.
- Im **Client OAuth** ist Ihre app. Es wird durch die Anwendung ID identifiziert. Es ist normalerweise der Teilnehmer, die mit Endanwendern interagieren. Sie fordert auch Token vom Server Autorisierung an. Der Ressourcenbesitzer muss die Client Zugriffsberechtigung für die Ressource gewähren.
- Der **Ressourcenserver** ist, wo befindet sich die Ressource oder die Daten. Er vertraut den Autorisierung Server um sicher authentifiziert und autorisiert den Client OAuth. Außerdem wird Person Access Token verwendet, um sicherzustellen, dass der Zugriff auf eine Ressource gewährt werden kann.

## <a name="policies"></a>Richtlinien
Azure AD B2C Richtlinien sind wohl, die wichtigsten Features des Diensts. Azure AD B2C erweitert die standard OAuth 2.0, und verbinden Sie OpenID Protokolle durch Einführung in die Richtlinien. Diese ermöglichen Azure AD B2C viel mehr als einfache Authentifizierung und Autorisierung ausführen. Richtlinien beschreiben vollständig Consumer Identität Erfahrung, einschließlich Anmeldung, Anmeldung und Profil bearbeiten. Richtlinien können in einer administrativen Benutzeroberfläche definiert werden. Sie können mithilfe eines spezielle Abfrageparameters in HTTP-Authentifizierungsanfragen ausgeführt werden. Richtlinien sind nicht Standardfeatures OAuth 2.0 und OpenID zu verbinden, damit Sie die Zeit zu verstehen sie Unternehmen sollten. Weitere Informationen finden Sie unter der [Azure AD B2C Richtlinie Referenzhandbuchs](active-directory-b2c-reference-policies.md).

## <a name="tokens"></a>Token
Die Durchführung Azure AD B2C OAuth 2.0, und verbinden Sie OpenID nutzt von Person Token, einschließlich Token Person, die als JSON Web Token (JWTs) dargestellt werden. Eine Person Token ist ein einfaches Sicherheitstoken, den "Person" den Zugriff auf eine geschützte Ressource gewährt. Die Person ist eine Partei, die das Token präsentieren kann. Azure AD muss zuerst eine Party authentifizieren, bevor sie eine Person Token empfangen kann. Wenn die erforderlichen Schritte zum Token im Übertragung und Speicherung gesichert nicht geöffnet werden, kann jedoch werden abgefangen und durch einen dritten unbeabsichtigte verwendet.

Einige Sicherheitstokens haben eine integrierte Methode, die unbefugten verhindert, dass diese Person Token verfügen nicht über dieses Verfahren Sie müssen in einem Kanal, wie z. B. Transport Layer Security (HTTPS) übertragen werden. Bei der Übertragung einer Person Token außerhalb eines Kanal können bösartige dritte ein Mann in zweiter Angriffen das Token erfassen und verwenden, um unbefugten Zugriff auf eine geschützte Ressource. Diese Sicherheitsprinzipien angewendet werden, wenn die Person Token gespeichert oder zur späteren Verwendung zwischengespeichert werden. Immer Stellen Sie sicher, dass Ihre app übermittelt und Person Token auf sichere Weise speichert.

Weitere Person token Sicherheitsaspekte finden Sie unter [RFC 6750 Abschnitt 5](http://tools.ietf.org/html/rfc6750).

Weitere Details zu den verschiedenen Arten von Azure AD B2C verwendeten Token stehen im [token Azure AD-Bezug](active-directory-b2c-reference-tokens.md).

## <a name="protocols"></a>Protokolle

Wenn Sie bereit sind, einige Beispiel Anfragen zu überprüfen, können Sie mit einem der folgenden Lernprogramme starten. Können Sie jeweils entspricht ein bestimmtes Authentifizierung Szenario. Wenn Sie Hilfe bei der Festlegung welcher Ablauf für Sie richtig ist benötigen, checken Sie [die Typen von apps Sie erstellen können, mithilfe von Azure AD B2C aus](active-directory-b2c-apps.md).

- [Erstellen Sie mobile und systemeigene Applikationen mithilfe von OAuth 2.0](active-directory-b2c-reference-oauth-code.md)
- [Erstellen Sie mithilfe der herstellen OpenID Web apps](active-directory-b2c-reference-oidc.md)
