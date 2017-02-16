<properties
    pageTitle="Erste Schritte-Azure AD-Windows Store | Microsoft Azure"
    description="So erstellen Sie eine Windows Store-Anwendung, die für die Anmeldung Azure AD integriert und Azure AD-Anrufe geschützt APIs OAuth verwenden."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-with-a-windows-store-app"></a>Integrieren von Azure AD mit einer Windows Store-App

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Wenn Sie eine app für Windows Store entwickeln, macht Azure AD einfach und unkompliziert für Sie Ihre Benutzer mit ihren Active Directory-Konten authentifizieren.  Darüber hinaus können die Anwendung zu einem beliebigen Web-API von Azure AD, wie die Office 365-APIs oder der Azure-API geschützten sicher nutzen.

Für Windows Store-desktop-apps, die Zugriff auf geschützte Ressourcen benötigen, bietet Azure AD an den Active Directory-Authentifizierungsbibliothek oder ADAL.  ADAL des alleinige Zweck im Leben ist für Ihre app abzurufenden Zugriffstoken zu erleichtern.  Um veranschaulichen, wie einfach es ist, wird hier wir einer "Verzeichnissuche" Windows Store-app, die erstellen:

-   Ruft Token für das Anrufen von der Azure AD Graph-API mit dem [OAuth 2.0-Authentifizierung-Protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx)zugegriffen werden.
-   Sucht in einem Verzeichnis für Benutzer mit einer angegebenen UPN.
-   Vorzeichen Benutzer ab.

Wenn Sie um die gesamte Arbeit der Anwendung zu erstellen, müssen Sie:

2. Registrieren Sie Ihrer Anwendung Azure AD.
3. Installieren und Konfigurieren von ADAL.
5. Verwenden Sie ADAL, um die Token von Azure AD zu gelangen.

Um zu beginnen, [herunterladen ein Projekts Gerüst](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip) oder [die fertige Stichprobe herunterladen](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Jeder ist eine Lösung für Visual Studio 2015.  Sie benötigen auch einem Azure AD-Mandanten, in dem Sie Benutzer erstellen und Registrieren einer Anwendung.  Wenn Sie noch keinen Mandanten, [erfahren Sie, wie Sie eins zu erhalten](active-directory-howto-tenant.md)haben.

## <a name="1-register-the-directory-searcher-application"></a>*1. die Directory Searcher Anwendung zu registrieren*
Wenn Ihre app Token abrufen aktivieren möchten, müssen Sie zuerst in Ihrem Azure AD-Mandanten zu registrieren, und gewähren sie über die Berechtigung zum Zugreifen auf der Azure AD Graph-API:

-   Melden Sie sich bei der [Azure-Verwaltungsportal](https://manage.windowsazure.com)
-   Klicken Sie im linken Navigationsbereich auf **Active Directory**
-   Wählen Sie einen Mandanten, in dem die Anwendung registrieren aus.
-   Klicken Sie auf die Registerkarte **Applications** aus, und klicken Sie in der unteren Schublade auf **Hinzufügen** .
-   Folgen Sie den Anweisungen, und erstellen Sie eine neue **Native Client-Anwendung**.
    -   Den **Namen** der Anwendung wird eine Anwendung an Endbenutzer beschrieben.
    -   **Umleiten Uri** ist eine Zeichenfolge und Farbschema Kombination, mit denen Azure AD token Antworten zurückgeben.  Geben Sie einen Platzhalterwert jetzt z. b `http://DirectorySearcher`.  Wir werden diesen Wert später ersetzen.
-   Nachdem Sie die Registrierung abgeschlossen haben, zuweisen AAD Ihre app eine eindeutige Client-ID.  Sie benötigen diesen Wert in den nächsten Abschnitten, also kopieren Sie sie von der Registerkarte **Konfigurieren** .
- Registerkarte **Konfigurieren** , suchen Sie auch im Abschnitt "Berechtigungen zu anderen Applications".  Fügen Sie die Zugriffsberechtigung **Verzeichnis als der Benutzer angemeldet** , klicken Sie unter **Delegierte Berechtigungen**für die Anwendung "Azure Active Directory" hinzu.  Dadurch wird eine Anwendung Abfragen die Graph-API für Benutzer aktiviert werden.

## <a name="2-install--configure-adal"></a>*2. installieren und Konfigurieren von ADAL*
Jetzt, da Sie eine Anwendung in Azure AD haben, können Sie ADAL installieren und Schreiben von Code Identität Zusammenhang.  In der Reihenfolge für ADAL mit Azure AD kommunizieren können müssen Sie es einige Informationen über Ihre app-Registrierung zur Verfügung zu stellen.
-   Durch Hinzufügen von ADAL zum Verwenden der Paket-Manager-Konsole DirectorySearcher Projekt beginnen.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Öffnen Sie im Projekt DirectorySearcher `MainPage.xaml.cs`.  Ersetzen Sie die Werte in den `Config Values` Region zu den Werten, die Sie in einer Azure-Portal eingeben.  Code werden diese Werte verwiesen werden, wenn es ADAL verwendet.
    -   Die `tenant` ist die Domäne Ihrer Azure AD-Mandanten, z. B. contoso.onmicrosoft.com
    -   Die `clientId` ClientId Ihrer Anwendung, die Sie kopiert, auf dem Portal haben ist.
-   Sie müssen jetzt den Rückruf Uri für Ihre Windows Store-app zu ermitteln.  Festlegen einer fortzuschreiten in dieser Zeile in der `MainPage` Methode:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- Erstellen Sie die Lösung, sicherzustellen, dass alle Paketverweise wiederhergestellt werden.  Wenn Pakete fehlen, von der Nuget-Paket-Manager zu öffnen und die Pakete wiederherstellen.
- Führen Sie die app, und kopieren Sie den Wert von reservieren `redirectUri` Wenn die fortzuschreiten erreicht ist.  Es sollte ungefähr wie folgt aussehen

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

- Klicken Sie auf die Registerkarte **Konfigurieren** der Anwendung im Verwaltungsportal Azure ersetzen Sie den Wert für die **RedirectUri** mit dem folgenden Wert ein.  

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. ADAL abzurufenden Token aus AAD verwenden*
Das grundlegende Prinzip hinter ADAL ist, wenn Ihre app ein Access-Token benötigt, sie einfach ruft `authContext.AcquireToken(…)`, und ADAL erledigt den Rest.  

-   Dieser erste Schritt besteht Ihre app Initialisierung `AuthenticationContext` -ADAL primäre Klasse ist.  Dies ist, wo Sie ADAL übergeben die Koordinaten Azure AD Vermittlung und feststellen, wie Token zwischengespeichert werden muss.

```C#
public MainPage()
{
    ...

    authContext = new AuthenticationContext(authority);
}
```

- Suchen Sie jetzt die `Search(...)` Methode, die aufgerufen wird, wenn der Benutzer auf die Schaltfläche "Suchen" des app Benutzeroberfläche klickt.  Diese Methode stellt eine GET-Anforderung der Azure AD Graph-API Abfrage für Benutzer, dessen UPN mit der angegebenen Suchbegriff beginnt.  Um die Graph-API Abfragen, Sie müssen jedoch eine Access_token in können die `Authorization` Kopfzeile der Anfrage – hier kommt ADAL in.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
        }
        return;
    }
    ...
}
```
- Wenn Ihre app ein Token fordert, indem er `AcquireTokenAsync(...)`, ADAL versucht, ein Token zurück, ohne Bestätigung für Anmeldeinformationen.  ADAL fest, dass es sich bei der Benutzer muss sich anmelden, ein Token abzurufen, wird es ein Anmeldedialogfeld angezeigt, die Anmeldeinformationen des Benutzers sammeln und Token nach der erfolgreichen Authentifizierung zurückgibt.  Ist ADAL zurückzugebenden ein Token aus irgendeinem Grund die `AuthenticationResult` Status wird ein Fehler sein.

- Heute ist es Zeit zu verwenden, die die gerade von Ihnen erworbenen Access_token.  Auch in der `Search(...)` Methode, fügen Sie das Token an die Graph-API GET-Anforderung in der Kopfzeile Autorisierung:

```C#
// Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

```
- Sie können auch die `AuthenticationResult` Objekt für die Anzeige von Informationen über den Benutzer in Ihrer app, z. B.-Id des Benutzers:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- ADAL können Sie schließlich den Benutzer als auch die Anwendung melden.  Wenn der Benutzer auf die Schaltfläche "Abmelden" klickt, möchten wir sicherstellen, dass auf den nächsten Anruf an `AcquireTokenAsync(...)` bei der Anmeldung wird in der Ansicht angezeigt.  Mit ADAL ist dies so einfach wie das Löschen des Caches token:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Herzlichen Glückwunsch! Jetzt haben ein Windows Store-app, die die Möglichkeit zum Authentifizieren von Benutzern, sicheres Aufrufen von Web-APIs mit OAuth 2.0 enthält nicht funktioniert und Abrufen grundlegender Informationen über den Benutzer.  Wenn Sie noch nicht geschehen ist, ist jetzt die Zeit in Ihrem Mandanten für einige Benutzer gefüllt wird.  Führen Sie die DirectorySearcher-app, und melden Sie sich mit einem dieser Benutzer.  Suchen Sie nach anderen Benutzern basierend auf ihrem Benutzerprinzipalnamen.  Schließen Sie die app, und führen Sie erneut aus.  Beachten Sie, wie die Sitzung des Benutzers intakt bleibt.  Melden Sie sich ab (indem Sie mit der rechten Maustaste, um der unteren Leiste anzeigen), und wieder als ein anderer Benutzer anmelden.

ADAL erleichtert die all diese allgemeine Identität Features in Ihrer Anwendung zu integrieren.  Es übernimmt alle Prozesse laufen für Sie - Cache-Verwaltung, OAuth Protokoll Support Vorführung des Benutzers mit einer Login UI, Aktualisieren von abgelaufenen Token und vieles mehr.  Sie wirklich wissen müssen lediglich einen einzelnen API Anruf `authContext.AcquireToken*(…)`.

Als Referenz die fertige Stichprobe (ohne die Konfigurationswerte) zur Verfügung gestellt wird [hier](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Sie können jetzt auf zusätzliche Identität Szenarien verschieben, klicken Sie auf.  Möglicherweise möchten versuchen:

[Sichern eine .NET Web-API mit Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
