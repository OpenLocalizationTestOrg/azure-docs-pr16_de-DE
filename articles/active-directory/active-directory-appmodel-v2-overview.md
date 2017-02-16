<properties
    pageTitle="Übersicht über die Version 2.0-Endpunkt | Microsoft Azure"
    description="Eine Einführung in das Erstellen von apps mit Anmeldung sowohl Microsoft Account und Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="dastrock"/>

# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Anmeldung Microsoft Account und Azure AD-Benutzer in einer einzelnen app

In der Vergangenheit war ein app-Entwickler, die sowohl von Microsoft-Konten Azure Active Directory unterstützt wollten erforderlich, um mit zwei separate Systeme integrieren.  Wir haben nun eine neue Version der Authentifizierung-API eingeführt, die Sie sich in der Benutzer, die mit den beiden Typen von Konten mit Azure AD-System ermöglicht.  Dieses Authentifizierungssystem zusammengeführt wird als **den Endpunkt Version 2.0**bezeichnet.  Mit den Endpunkt Version 2.0 ermöglicht eine einfache Integration eine Zielgruppe zu erreichen, die Millionen von Benutzern mit persönlichen und Arbeit/Schule Konten umfasst.

Apps, die den Endpunkt Version 2.0 verwenden, können auch REST-APIs aus dem [Microsoft Graph](https://graph.microsoft.io) und [Office 365](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) verwenden beide Typen von Konten nutzen.

<!-- For a quick introduction to the v2.0 endpoint, please view the [Getting Started with Microsoft Identities: Enterprise Grade Sign In For Your Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/) video. -->

## <a name="getting-started"></a>Erste Schritte
[AZURE.VIDEO build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps]

Wählen Sie Ihre bevorzugte Plattform aus der folgenden Liste, um eine app mit unseren open-Source-Bibliotheken und Framework erstellen.  Alternativ können Sie unseren OAuth 2.0 und verbinden OpenID Protokoll Dokumentation senden und Empfangen von Protokollnachrichten direkt ohne eine autorisierende Bibliothek.

<!-- TODO: Finalize this table  -->
[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Was ist neu
Hier die grundlegende Informationen werden nützlich sein, was ist und was nicht möglich, mit der Version 2.0-Endpunkt.

- Informationen Sie zu den [Arten von apps, die Sie mit der Version 2.0-Endpunkt erstellen können](active-directory-v2-flows.md).
- Grundlegendes zur [Einschränkungen, Einschränkungen, und Einschränkungen](active-directory-v2-limitations.md) mit den Endpunkt Version 2.0.
- Wir haben kürzlich Unterstützung für [Administrator beschränkt Bereiche](active-directory-v2-scopes.md) und dem [OAuth2-Client, die Anmeldeinformationen erteilen](active-directory-v2-protocols-oauth-client-creds.md)hinzugefügt.  Probieren sie!

## <a name="reference"></a>Bezug
Diese Links werden für die Plattform – ein tieferer Einblick untersuchen hilfreich sein:

- Erstellen von 2016: [Erste Schritte mit Microsoft Identitäten: Enterprise Noten anmelden für Ihre Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/)
- Anfordern von Hilfe auf Stapelüberlauf mit [Azure-Active Directory](http://stackoverflow.com/questions/tagged/azure-active-directory) oder [adal](http://stackoverflow.com/questions/tagged/adal) Kategorien.
- [Version 2.0-Protokoll Bezug](active-directory-v2-protocols.md)
- [Version 2.0 Token Bezug](active-directory-v2-tokens.md)
- [Bezug der Version 2.0-Bibliothek](active-directory-v2-libraries.md)
- [Bereiche und Zustimmung in den Endpunkt Version 2.0](active-directory-v2-scopes.md)
- [Die Registrierung von Microsoft App-Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)
- [Office 365 REST-API-Referenz](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)
- [Die Microsoft Graph](https://graph.microsoft.io)