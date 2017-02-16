<properties
    pageTitle="Azure Active Directory B2C: Extensible Policy Framework | Microsoft Azure"
    description="Ein Thema extensible Richtlinie Rahmen der Azure Active Directory B2C und zum Erstellen von verschiedenen Richtlinientypen"
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
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-extensible-policy-framework"></a>Azure Active Directory B2C: Extensible Policy framework

## <a name="the-basics"></a>Die Grundlagen

Extensible Richtlinie Rahmen der B2C Azure Active Directory (Azure AD) ist die Stärke Core des Diensts. Richtlinien beschreiben vollständig Consumer Identität Erfahrung wie Anmelde-, Anmeldung oder ein Profil bearbeiten. Beispielsweise können mit eine Anmelde Richtlinie Sie Verhaltensweisen festlegen, indem Sie die folgenden Einstellungen konfigurieren:

- Kontotypen (für soziale Netzwerke wie Facebook-Konten oder lokale Konten wie e-Mail-Adresse), die Nutzer zum Registrieren für die Anwendung verwenden können.
- Attribute (z. B. Vorname, Postleitzahl und Bremsbacken Größe), bei der Anmeldung vom Consumer gesammelt werden sollen.
- Verwendung der kombinierte Authentifizierung.
- Das Aussehen und Verhalten aller Anmeldung Seiten.
- Informationen (die als Ansprüche in einem Token-Manifeste), dass die Anwendung empfängt wann die Richtlinie Beendigung ausführen.

Sie können mehrere Richtlinien unterschiedlichen Typs in Ihrem Mandanten erstellen und Bedarf in Ihrer Anwendung verwenden. Richtlinien können für Webanwendungen wiederverwendet werden. Dies ermöglicht Entwicklern definieren und Consumer Identität Erfahrung mit minimalen oder keine Änderungen an ihren Code ändern.

Richtlinien sind für die Verwendung über eine einfache Entwicklertools Schnittstelle verfügbar. Der Anwendung eine Richtlinie mithilfe einer standard HTTP-Authentifizierung Anforderung (Übergabe eines Parameters Richtlinie in der Besprechungsanfrage) auslöst und empfängt eine angepasste Token als Antwort. Angenommen, der einzige Unterschied zwischen anfordert Aufrufen einer Anmeldung Richtlinie und die Aufrufen einer Richtlinie Anmeldung den Namen der Richtlinie in die Zeichenfolge "p" Abfrageparameter verwendet werden:

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Weitere Details zu den Policy Framework finden Sie unter diesen [Blogbeitrag](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).

## <a name="create-a-sign-up-policy"></a>Erstellen einer Richtlinie Anmeldung

Um Anmeldung auf Ihrer Anwendung zu aktivieren, müssen Sie eine Anmelde Richtlinie zu erstellen. Dieser Richtlinie beschrieben, die Nutzer bei der Anmeldung werden durch Benutzeroberflächen und den Inhalt der Token, die die Anwendung auf erfolgreich Sportteam gewährt werden soll.

1. [Gehen Sie folgendermaßen vor, um das B2C Features Blade Azure-Portal zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Anmeldung Richtlinien**.
3. Klicken Sie auf **+ Add** am oberen Rand der Blade.
4. Der **Name** bestimmt den Namen der Anmeldung Richtlinie, von der Anwendung verwendet. Geben Sie zum Beispiel "SiUp" ein.
5. Klicken Sie auf **Identitätsanbieter** , und wählen Sie "Email beim Registrieren". Optional können Sie auch für soziale Netzwerke Identitätsanbieter, auswählen, wenn Sie bereits konfiguriert ist. Klicken Sie auf **OK**.
6. Klicken Sie auf **Anmeldung Attribute**. Hier wählen Sie die Attribute, die Sie bei der Anmeldung vom Consumer sammeln möchten. Wählen Sie beispielsweise "Land/Region", "Anzeigename" und "Postleitzahl" ein. Klicken Sie auf **OK**.
7. Klicken Sie auf **Anwendung Ansprüche**. Hier wählen Sie Ansprüche, die in der Token zurück zur vorherigen Anwendung nach einer erfolgreichen Anmeldung Erfahrung gesendet zurückgegeben werden soll. Wählen Sie beispielsweise "Anzeigename", "Identitätsanbieter", "Postleitzahl", "Benutzer ist neu" und "Des Benutzers Objekt-ID" ein.
8. Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als "**B2C_1_SiUp**" (die **B2C\_1\_ ** Fragment wird automatisch hinzugefügt) in das Blade **Anmeldung Richtlinien** .
9. Öffnen Sie die Richtlinie, indem Sie auf "**B2C_1_SiUp**".
10. Select "Contoso B2C app" in der Dropdownliste **Applications** und `https://localhost:44321/` in der **Antworten URL / URI umleiten** Dropdown-Liste.
11. Klicken Sie auf **jetzt ausführen**. Eine neue Browserregisterkarte geöffnet, und durch Benutzerfunktionen der anmelden für eine Anwendung ausgeführt werden kann.

    > [AZURE.NOTE]
    Es nimmt in eine Minute um für das Erstellen von Richtlinien und Aktualisierungen wirksam wird.

## <a name="create-a-sign-in-policy"></a>Erstellen einer Richtlinie Anmeldung

Zum Anmelden auf Ihrer Anwendung zu aktivieren, müssen Sie zum Erstellen einer Richtlinie Anmeldung. Dieser Richtlinie werden die Erfahrung, die Nutzer durchlaufen werden während Anmeldung und den Inhalt von Token, die die Anwendung empfangen wird auf dem erfolgreiche Sign-ins.

1. [Gehen Sie folgendermaßen vor, um das B2C Features Blade Azure-Portal zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Anmeldung Richtlinien**.
3. Klicken Sie auf **+ Add** am oberen Rand der Blade.
4. Der **Name** bestimmt den Namen der Anmeldung Richtlinie, von der Anwendung verwendet. Geben Sie zum Beispiel "SiIn" ein.
5. Klicken Sie auf **Identitätsanbieter** , und wählen Sie "Lokale Konto Anmeldefenster". Optional können Sie auch für soziale Netzwerke Identitätsanbieter, auswählen, wenn Sie bereits konfiguriert ist. Klicken Sie auf **OK**.
6. Klicken Sie auf **Anwendung Ansprüche**. Hier wählen Sie Ansprüche, die in der Token zurück zur vorherigen Anwendung nach dem erfolgreiche Anmeldeverhalten gesendet zurückgegeben werden soll. Wählen Sie beispielsweise "Anzeigename", "Identitätsanbieter", "Postleitzahl" und "Des Benutzers Objekt-ID" ein. Klicken Sie auf **OK**.
7. Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als "**B2C_1_SiIn**" (die **B2C\_1\_ ** Fragment wird automatisch hinzugefügt) in das Blade **Anmeldung Richtlinien** .
8. Öffnen Sie die Richtlinie, indem Sie auf "**B2C_1_SiIn**".
9. Select "Contoso B2C app" in der Dropdownliste **Applications** und `https://localhost:44321/` in der **Antworten URL / URI umleiten** Dropdown-Liste.
10. Klicken Sie auf **jetzt ausführen**. Eine neue Browserregisterkarte geöffnet, und durch Benutzerfunktionen in Ihrer Anwendung Signieren ausgeführt werden kann.

    > [AZURE.NOTE]
    Es nimmt in eine Minute um für das Erstellen von Richtlinien und Aktualisierungen wirksam wird.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Erstellen einer Richtlinie Anmeldung oder Anmeldung

Dieser Richtlinie übernimmt sowohl Consumer Anmeldung und Anmeldung Erfahrung mit einer einzelnen Konfiguration an. Nutzer sind unten rechts Pfad (Anmeldung oder Anmeldung) je nach Kontext geführt haben. Es werden auch den Inhalt von Token, die die Anwendung beim erfolgreichen Sportteam oder Sign-ins gesendet wird.  Eine Stichprobe Code für die Anmeldung oder Anmeldung Richtlinie ist [verfügbar sind](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

1. [Gehen Sie folgendermaßen vor, um das B2C Features Blade Azure-Portal zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Anmeldung oder Anmeldung Richtlinien**.
3. Klicken Sie auf **+ Add** am oberen Rand der Blade.
4. Der **Name** bestimmt den Namen der Anmeldung Richtlinie, von der Anwendung verwendet. Geben Sie zum Beispiel "SiUpIn" ein.
5. Klicken Sie auf **Identitätsanbieter** , und wählen Sie "Email beim Registrieren". Optional können Sie auch für soziale Netzwerke Identitätsanbieter, auswählen, wenn Sie bereits konfiguriert ist. Klicken Sie auf **OK**.
6. Klicken Sie auf **Anmeldung Attribute**. Hier wählen Sie die Attribute, die Sie bei der Anmeldung vom Consumer sammeln möchten. Wählen Sie beispielsweise "Land/Region", "Anzeigename" und "Postleitzahl" ein. Klicken Sie auf **OK**.
7. Klicken Sie auf **Anwendung Ansprüche**. Hier wählen Sie Ansprüche, die in der Token zurück zur vorherigen Anwendung nach einer erfolgreichen Anmeldung oder Anmeldung Erfahrung gesendet zurückgegeben werden soll. Wählen Sie beispielsweise "Anzeigename", "Identitätsanbieter", "Postleitzahl", "Benutzer ist neu" und "Des Benutzers Objekt-ID" ein.
8. Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als "**B2C_1_SiUpIn**" (die **B2C\_1\_ ** Fragment wird automatisch hinzugefügt) in der **Anmeldung oder Anmeldung Richtlinien** Blade.
9. Öffnen Sie die Richtlinie, indem Sie auf "**B2C_1_SiUpIn**".
10. Select "Contoso B2C app" in der Dropdownliste **Applications** und `https://localhost:44321/` in der **Antworten URL / URI umleiten** Dropdown-Liste.
11. Klicken Sie auf **jetzt ausführen**. Eine neue Browserregisterkarte geöffnet, und durch Anmeldung oder Anmeldung Benutzerfunktionen ausgeführt werden kann, wie konfiguriert.

    > [AZURE.NOTE]
    Es nimmt in eine Minute um für das Erstellen von Richtlinien und Aktualisierungen wirksam wird.

## <a name="create-a-profile-editing-policy"></a>Erstellen eines Profils Richtlinie bearbeiten

Um Profil bearbeiten auf Ihrer Anwendung zu aktivieren, müssen Sie zum Erstellen eines Profils Richtlinie zu bearbeiten. Dieser Richtlinie werden die Erfahrung, die Nutzer durchlaufen werden, während das Profil bearbeiten und den Inhalt von Token, die die Anwendung auf erfolgreichen Abschluss gewährt werden soll.

1. [Gehen Sie folgendermaßen vor, um das B2C Features Blade Azure-Portal zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Profil Bearbeiten von Richtlinien**.
3. Klicken Sie auf **+ Add** am oberen Rand der Blade.
4. Der **Name** bestimmt das Profil bearbeiten Richtliniennamen, die von der Anwendung verwendet. Geben Sie zum Beispiel "SiPe" ein.
5. Klicken Sie auf **Identitätsanbieter** , und wählen Sie "E-Mail-Adresse". Optional können Sie auch für soziale Netzwerke Identitätsanbieter, auswählen, wenn Sie bereits konfiguriert ist. Klicken Sie auf **OK**.
6. Klicken Sie auf **Profilattribute**. Hier wählen Sie die Attribute, die der Consumer anzeigen und bearbeiten kann. Wählen Sie beispielsweise "Land/Region", "Anzeigename" und "Postleitzahl" ein. Klicken Sie auf **OK**.
7. Klicken Sie auf **Anwendung Ansprüche**. Hier wählen Sie Ansprüche, die in der Token zurück zur vorherigen Anwendung nach dem erfolgreiche Profil bearbeiten Erfahrung gesendet zurückgegeben werden soll. Wählen Sie beispielsweise "Anzeigename" und "Postleitzahl" ein.
8. Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als "**B2C_1_SiPe**" (die **B2C\_1\_ ** Fragment wird automatisch hinzugefügt) in das **Profil Bearbeiten von Richtlinien** Blade.
9. Öffnen Sie die Richtlinie, indem Sie auf "**B2C_1_SiPe**".
10. Select "Contoso B2C app" in der Dropdownliste **Applications** und `https://localhost:44321/` in der **Antworten URL / URI umleiten** Dropdown-Liste.
11. Klicken Sie auf **jetzt ausführen**. Eine neue Browserregisterkarte geöffnet, und durch das Profil bearbeiten Consumer berichtet in Ihrer Anwendung ausgeführt werden kann.

    > [AZURE.NOTE]
    Es nimmt in eine Minute um für das Erstellen von Richtlinien und Aktualisierungen wirksam wird.
    
## <a name="create-a-password-reset-policy"></a>Erstellen einer Richtlinie für Kennwörter zurücksetzen

Um abgestimmte Kennwortrücksetzung auf Ihrer Anwendung zu aktivieren, müssen Sie zum Erstellen einer Richtlinie für Kennwörter zurücksetzen. Beachten Sie, dass das Kennwort des Mandanten organisationsweite angegebene Option zurücksetzen [hier](active-directory-b2c-reference-sspr.md) ist für die Anmeldung Richtlinien behalten ihre Gültigkeit. Dieser Richtlinie werden die Erfahrung, die die Nutzer durchlaufen werden, während das Zurücksetzen von Kennwörtern und den Inhalt der Token, die die Anwendung auf erfolgreichen Abschluss gewährt werden soll.

1. [Gehen Sie folgendermaßen vor, um das B2C Features Blade Azure-Portal zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Kennwort zurücksetzen Richtlinien**.
3. Klicken Sie auf **+ Add** am oberen Rand der Blade.
4. Zurücksetzen des Kennworts Richtliniennamen, die von der Anwendung verwendeten bestimmt den **Namen** . Geben Sie zum Beispiel "SSPR" ein.
5. Klicken Sie auf **Identitätsanbieter** , und wählen Sie "Mit e-Mail-Adresse Kennwort zurücksetzen". Klicken Sie auf **OK**.
6. Klicken Sie auf **Anwendung Ansprüche**. Hier wählen Sie Ansprüche, die in der Token zurück zur vorherigen Anwendung gesendet werden, nachdem ein erfolgreich Kennwort Erfahrung zurücksetzen zurückgegeben werden sollen. Wählen Sie beispielsweise "Des Benutzers Objekt-ID" aus.
7. Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als "**B2C_1_SSPR**" (die **B2C\_1\_ ** Fragment wird automatisch hinzugefügt) in das **Kennwort zurücksetzen Richtlinien** Blade.
8. Öffnen Sie die Richtlinie, indem Sie auf "**B2C_1_SSPR**".
9. Select "Contoso B2C app" in der Dropdownliste **Applications** und `https://localhost:44321/` in der **Antworten URL / URI umleiten** Dropdown-Liste.
10. Klicken Sie auf **jetzt ausführen**. Eine neue Browserregisterkarte geöffnet, und Sie durch ein Kennwort zurücksetzen Benutzerfunktionen in Ihrer Anwendung ausführen können.

    > [AZURE.NOTE]
    Es nimmt in eine Minute um für das Erstellen von Richtlinien und Aktualisierungen wirksam wird.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Token, Sitzung und Konfiguration für einzelne anmelden](active-directory-b2c-token-session-sso.md).
