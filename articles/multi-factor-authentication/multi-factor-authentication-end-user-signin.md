<properties
    pageTitle="Azure MFA Anmeldefenster Erfahrung mit Azure kombinierte Authentifizierung"
    description="Auf dieser Seite wird Sie Anleitungen auf, wo Sie die verschiedenen Anmeldefenster Methoden mit Azure MFA verfügbare Funktionen finden Sie unter bereitstellen."
    keywords="Benutzerauthentifizierung, Anmeldeverhalten, Anmeldung mit Mobiltelefon mit Rufnummer anmelden"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>Geben Sie im Erfahrung mit Azure kombinierte Authentifizierung
> [AZURE.NOTE]  Der folgende Dokumentation auf dieser Seite wird eine typische Anmeldeverhalten.  Hilfe bei der Anmeldung finden Sie unter [haben Sie Probleme mit Azure kombinierte Authentifizierung](multi-factor-authentication-end-user-manage-settings.md)



## <a name="what-will-your-sign-in-experience-be"></a>Was wird der Anmeldevorgang sein?
Je nachdem wie anmelden und mehrstufige Authentifizierung verwenden werden Ihre Erfahrungen variieren.  In diesem Abschnitt werden wir Informationen bereitstellen, auf was Sie erwartet bei der Anmeldung.  Wählen Sie das Schema, das am besten beschreibt vorgehen:


Was machst du?|Beschreibung
:------------- | :------------- |
[Anmelden mit Mobil- oder Office-Telefon](#signing-in-with-mobile-or-office-phone) | Dies ist, was Sie über Ihr Telefon mobil oder Office Anmeldung verhindern erwarten können.
[Melden Sie sich mit der Microsoft-Authenticator-app mit einer Benachrichtigung](#signing-in-with-the-microsoft-authenticator-app-using-notification) | Dies ist, was Sie erwarten können mit Microsoft Authenticator app mit Benachrichtigungen.
[Melden Sie sich mit der Überprüfungscode mit Microsoft Authenticator-app](#signing-in-with-the-microsoft-authenticator-app-using-verification-code)|Dies ist, was Sie erwarten können mithilfe der Microsoft Authenticator Thapp mit einem Überprüfungscode.
[Melden Sie sich mit einer alternativen Methode](#signing-in-with-an-alternate-method)|Dies wird gezeigt, was Sie erwartet, wenn Sie eine andere Methode verwenden möchten.

## <a name="signing-in-with-mobile-or-office-phone"></a>Anmelden mit Mobil- oder Office-Telefon

Die folgende Informationen werden zur Darstellung von kombinierte Authentifizierung mit Ihrem Smartphone Mobil oder Office mit beschrieben.

### <a name="to-sign-in-with-a-call-to-your-office-or-mobile-phone"></a>Anmelden mit einem Anruf an Ihr Mobiltelefon oder office

- Melden Sie sich an eine Anwendung oder den Dienst, wie Office 365 mit Ihren Benutzernamen und Ihr Kennwort ein.
- Microsoft ruft Sie auf.

![Microsoft-Anrufe](./media/multi-factor-authentication-end-user-signin-phone/call.png)

- Telefonanruf ein, und drücken Sie die #-Taste.

![Antwort](./media/multi-factor-authentication-end-user-signin-phone/phone.png)

- Sie sollten jetzt angemeldet sein.</li>

## <a name="signing-in-with-the-microsoft-authenticator-app-using-notification"></a>Melden Sie sich mit der Microsoft-Authenticator-app mit einer Benachrichtigung

Die folgende Informationen beschreiben Sie zur Darstellung von kombinierte Authentifizierung mit der Microsoft-Authenticator-app verwenden, wenn Sie eine Benachrichtigung gesendet werden.

### <a name="to-sign-in-with-a-notification-sent-the-microsoft-authenticator-app"></a>Für die Anmeldung an einer Benachrichtigung gesendet Microsoft Authenticator-app

- Melden Sie sich an eine Anwendung oder den Dienst, wie Office 365 mit Ihren Benutzernamen und Ihr Kennwort ein.
- Microsoft wird eine Benachrichtigung gesendet.

![Microsoft sendet eine Benachrichtigung](./media/multi-factor-authentication-end-user-signin-app-notify/notify.png)


- Telefonanruf ein, und drücken Sie die Taste überprüfen.  Wenn Ihr Unternehmen eine PIN erfordert werden Sie hier aufgefordert werden.

![Vergewissern Sie sich](./media/multi-factor-authentication-end-user-signin-app-notify/phone.png)

![Setup](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

- Sie sollten jetzt angemeldet sein.


## <a name="signing-in-with-the-microsoft-authenticator-app-using-verification-code"></a>Melden Sie sich mit der Überprüfungscode mit Microsoft Authenticator-app

Die folgende Informationen beschreiben Sie zur Darstellung von kombinierte Authentifizierung mit der Microsoft-Authenticator-app verwenden, wenn Sie es mit einem Überprüfungscode verwenden.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Anmelden mit einer Überprüfungscode mit der Microsoft-Authenticator-app

- Melden Sie sich an eine Anwendung oder den Dienst, wie Office 365 mit Ihren Benutzernamen und Ihr Kennwort ein.
- Microsoft fordert Sie für eine Überprüfungscode.

![Geben Sie ein Überprüfung](./media/multi-factor-authentication-end-user-signin-app-verify/verify.png)

- Öffnen Sie die app Microsoft Authenticator auf Ihrem Smartphone aus, und geben Sie den Code in das Feld, in dem Sie Anmeldung sind.

![Abrufen von code](./media/multi-factor-authentication-end-user-signin-app-verify/phone.png)



- Sie sollten jetzt angemeldet sein.


## <a name="signing-in-with-an-alternate-method"></a>Melden Sie sich mit einer alternativen Methode


Im folgende Abschnitt wird gezeigt, wie Sie sich mit an eine andere Methode, wenn die bevorzugte Methode möglicherweise nicht verfügbar.

### <a name="to-sign-in-with-an-alternate-method"></a>Anmelden mit einer alternativen Methode

- Melden Sie sich an eine Anwendung oder den Dienst, wie Office 365 mit Ihren Benutzernamen und Ihr Kennwort ein.
- Wählen Sie eine andere Überprüfung Option verwenden.  Sie können mit der anderen Optionen eine der Auswahlmöglichkeiten vorhanden sein. Die Zahl wird wird auf der Grundlage wie viele Setup haben.

![Verwenden Sie alternative-Methode](./media/multi-factor-authentication-end-user-signin-alt/alt.png)

- Wählen Sie eine andere Methode aus, und melden Sie sich an.
