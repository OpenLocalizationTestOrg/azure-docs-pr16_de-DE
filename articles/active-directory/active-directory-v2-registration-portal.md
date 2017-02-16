<properties
    pageTitle="Hilfethemen der App-Registrierung Portal | Microsoft Azure"
    description="Eine Beschreibung der verschiedenen Features in Microsoft app Registrierung Portal."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="app-registration-reference"></a>App Registrierung Bezug
Dieses Dokument enthält Kontext bereitstellen und eine Beschreibung der verschiedenen Features in der Microsoft-App Registrierung Portal [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)gefunden.

## <a name="my-applications"></a>Meine Programme
Diese Liste enthält alle Anwendungen für die Verwendung mit dem Azure AD-Version 2.0-Endpunkt registriert.  Diese Applikationen haben die Möglichkeit, sich Benutzer mit von Microsoft-Konto und Arbeit/Schule Konten aus Azure Active Directory beide persönlichen Konten anmelden.  Weitere Informationen zu den Azure AD-Version 2.0-Endpunkt finden Sie unter unseren [Version 2.0 (Übersicht)](active-directory-appmodel-v2-overview.md).  Diesen Anwendungen können auch verwendet werden, um mit dem Microsoft-Konto-Authentifizierung Endpunkt integrieren `https://login.live.com`.

## <a name="live-sdk-applications"></a>Live SDK Applikationen
Diese Liste enthält alle Anwendungen für die Verwendung ausschließlich mit Microsoft-Konto registriert.  Sie sind für die Verwendung mit Azure Active Directory überhaupt nicht aktiviert.  Hier finden Sie alle Applikationen mit, die zuvor mit der MSA-Entwicklerportal am registriert wurde hat `https://account.live.com/developers/applications`.  Alle Funktionen, die Sie zuvor durchgeführt `https://account.live.com/developers/applications` kann nun diese neuen Portal ausgeführt werden `https://apps.dev.microsoft.com`.  Wenn Sie Fragen zu Ihrem Microsoft-Konto Applications haben, kontaktieren Sie uns.

## <a name="application-secrets"></a>Anwendung Kennwörter
Anwendung Kennwörter werden Anmeldeinformationen, mit die die Anwendung zuverlässigen [Client-Authentifizierung](http://tools.ietf.org/html/rfc6749#section-2.3) mit Azure AD ausführen können.  OAuth & OpenID verbinden, eine Anwendung Kennwörter häufig genannt wird eine `client_secret`.  In der Version 2.0-Protokoll, eine Anwendung, ein Sicherheitstoken einen Webspeicherort adressiert empfängt (mit einer `https` Farbschema) müssen eine Anwendung geheim verwenden, um sich beim Einlösen von dieser Sicherheitstoken Azure AD identifizieren.  Darüber hinaus eine native Client dieser empfängt Token auf einem Gerät wird verboten aus mithilfe einer Anwendung geheim Client-Authentifizierung, um die Speicherung vertrauliche Informationen in unsichere Umgebungen zu verhindern.

Jeder app kann zwei gültige Anwendung geheimen Informationen zu einem bestimmten Zeitpunkt Zeitpunkt enthalten.  Aufbewahren zwei Kennwörter, müssen Sie die Ablilty periodische Key Rollover über die gesamte Umgebung Ihrer Anwendung ausführen.  Nachdem Sie den gesamten Ihrer Anwendung einen neuen geheimen migriert haben, wählen Sie den alten Schlüssel löschen und Bereitstellen ein neues Kontos.

Zu diesem Zeitpunkt dürfen nur zwei Arten der Anwendung Kennwörter im Portal Registrierung app.  **Neues Kennwort generieren** auswählen generiert und speichert einen geheimen im entsprechenden Datenspeicher, die Sie in Ihrer Anwendung verwenden können.  **Neues Schlüsselpaar generieren** auswählen erstellt eine neuen öffentlichen und privaten Schlüssel, die heruntergeladen und für die Clientauthentifizierung zu Azure AD-verwendet werden können.

## <a name="profile"></a>Profil
Profilabschnitt des Portals Registrierung app kann verwendet werden, auf der Anmeldeseite für eine Anwendung anpassen.  Sie können zu diesem Zeitpunkt der Anmeldung Seite Anwendung Logo, Ausdrücke Service URL und Datenschutzbestimmungen ändern.  Das Logo muss ein transparentes Bild eines 48 x 48 oder 50 x 50 Pixel in einer GIF, PNG- oder JPEG-Datei, die 15 KB ist oder zu verkleinern.  Versuchen Sie die Werte ändern und Anzeigen der resultierenden Signieren Seite!

## <a name="live-sdk-support"></a>Live SDK-Unterstützung
Wenn Sie "Live SDK Support" aktivieren, werden Sie erstellen Anwendung vertrauliche Daten in Azure AD bereitgestellt werden und Microsoft Account Datenspeicher.  Dadurch wird eine Anwendung direkt mit dem Microsoft Account-Dienst (login.live.com) integriert werden soll.  Wenn Sie eine app, die mithilfe von Microsoft Account direkt (anstelle des Azure AD-Version 2.0-Endpunkts) erstellen möchten, sollten Sie sicherstellen, dass Live SDK-Unterstützung aktiviert ist.

Deaktivieren von Live SDK-Unterstützung wird sichergestellt, dass die Anwendung geheim nur in Azure AD-Daten geschrieben wird gespeichert.  Die Daten Azure AD-Store übernimmt unternehmensweite Verbraucher zu verstoßen, mit denen sie bestimmte Standards, wie z. B. FISMA Compliance entsprechen.  Wenn Sie Live SDK-Unterstützung aktivieren, kann Ihrer Anwendung nicht Einhaltung einige dieser Standards erzielen.

Wenn Sie immer nur den Azure AD-Version 2.0-Endpunkt verwenden möchten, können Sie problemlos Live SDK-Unterstützung deaktivieren.

