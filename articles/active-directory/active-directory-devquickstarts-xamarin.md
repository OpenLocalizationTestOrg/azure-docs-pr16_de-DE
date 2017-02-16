<properties
    pageTitle="Erste Schritte mit Azure AD-Xamarin | Microsoft Azure"
    description="So erstellen Sie eine Xamarin-Anwendung, die für die Anmeldung Azure AD integriert und Azure AD-Anrufe geschützt APIs OAuth verwenden."
    services="active-directory"
    documentationCenter="xamarin"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-xamarin-app"></a>Integrieren von Azure AD in einer App Xamarin

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Xamarin können Sie mobile-apps in c# schreiben, die auf iOS, Android und Windows (mobile Geräte und PCs) ausgeführt werden können. Wenn Sie eine app mit Xamarin erstellen, macht Azure AD einfach und unkompliziert für Sie Ihre Benutzer mit ihren Active Directory-Konten authentifizieren. Darüber hinaus können die Anwendung zu einem beliebigen Web-API von Azure AD, wie die Office 365-APIs oder der Azure-API geschützten sicher nutzen.

Xamarin-apps, die Zugriff auf geschützte Ressourcen benötigen, bietet Azure AD der Active Directory-Authentifizierungsbibliothek oder ADAL. ADAL des alleinige Zweck im Leben ist für Ihre app abzurufenden Zugriffstoken zu erleichtern. Um zu veranschaulichen, wie einfach es ist, wird hier wir einer app "Verzeichnissuche", die zum Erstellen von:

-   Läuft unter iOS, Android, Windows-Desktop, Windows Phone und Windows Store.
- Verwendet eine einzelne tragbaren Klasse-Bibliothek (PCL) zur Benutzerauthentifizierung und erhalten Informationen zur Azure AD Graph-API Token
-   Sucht in einem Verzeichnis für Benutzer mit einer angegebenen UPN.

Wenn Sie um die gesamte Arbeit der Anwendung zu erstellen, müssen Sie:

2. Richten Sie Ihre Xamarin Entwicklungsumgebung
2. Registrieren Sie Ihrer Anwendung Azure AD.
3. Installieren und Konfigurieren von ADAL.
5. Verwenden Sie ADAL, um die Token von Azure AD zu gelangen.

Um zu beginnen, [herunterladen ein Projekts Gerüst](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip) oder [die fertige Stichprobe herunterladen](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Jeder ist eine Visual Studio-2013-Lösung. Sie benötigen auch einem Azure AD-Mandanten, in dem Sie Benutzer erstellen und Registrieren einer Anwendung. Wenn Sie noch keinen Mandanten, [erfahren Sie, wie Sie eins zu erhalten](active-directory-howto-tenant.md)haben.

## <a name="0-set-up-your-xamarin-development-environment"></a>*0. richten Sie Ihre Xamarin Entwicklungsumgebung*
Da in diesem Lernprogramm Projekte für iOS, Android und Windows umfasst, benötigen Visual Studio Sie und Xamarin zusammen. Zum Erstellen der erforderlichen Umgebung Anweisungen Sie abgeschlossen auf [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) auf MSDN. Diese Anweisungen enthalten Material, die Sie überprüfen können, erfahren Sie mehr darüber Xamarin, während Sie auf den Installer ausführen warten. 

Nachdem Sie das notwendigen Setup abgeschlossen haben, öffnen die Lösung in Visual Studio für den Einstieg. Finden Sie sechs Projekte: fünf Plattform-spezifische Projekte und einem tragbaren Class-Bibliothek, die für alle Plattformen freigegeben werden sollen`DirectorySearcher.cs`

## <a name="1-register-the-directory-searcher-application"></a>*1. die Directory Searcher Anwendung zu registrieren*
Wenn Ihre app Token abrufen aktivieren möchten, müssen Sie zuerst in Ihrem Azure AD-Mandanten zu registrieren, und gewähren sie über die Berechtigung zum Zugreifen auf der Azure AD Graph-API:

-   Melden Sie sich bei der [Azure-Verwaltungsportal](https://manage.windowsazure.com)
-   Klicken Sie im linken Navigationsbereich auf **Active Directory**
-   Wählen Sie einen Mandanten, in dem die Anwendung registrieren aus.
-   Klicken Sie auf die Registerkarte **Applications** aus, und klicken Sie in der unteren Schublade auf **Hinzufügen** .
-   Folgen Sie den Anweisungen, und erstellen Sie eine neue **Native Client-Anwendung**.
    -   Den **Namen** der Anwendung wird eine Anwendung an Endbenutzer beschrieben.
    -   **Umleiten Uri** ist eine Zeichenfolge und Farbschema Kombination, mit denen Azure AD token Antworten zurückgeben. Geben Sie einen Wert, z. B. `http://DirectorySearcher`.
-   Nachdem Sie die Registrierung abgeschlossen haben, zuweisen AAD Ihre app eine eindeutige Client-ID. Sie benötigen diesen Wert in den nächsten Abschnitten, also kopieren Sie sie von der Registerkarte **Konfigurieren** .
- Registerkarte **Konfigurieren** , suchen Sie auch im Abschnitt "Berechtigungen zu anderen Applications". Fügen Sie die Berechtigung **Directory Access Ihrer Organisation** , klicken Sie unter **Delegierte Berechtigungen**für die Anwendung "Azure Active Directory" hinzu. Dadurch wird eine Anwendung Abfragen die Graph-API für Benutzer aktiviert werden.

## <a name="2-install--configure-adal"></a>*2. installieren und Konfigurieren von ADAL*
Jetzt, da Sie eine Anwendung in Azure AD haben, können Sie ADAL installieren und Schreiben von Code Identität Zusammenhang. In der Reihenfolge für ADAL mit Azure AD kommunizieren können müssen Sie es einige Informationen über Ihre app-Registrierung zur Verfügung zu stellen.
-   Beginnen Sie, indem Sie beide Projekte in der Lösung mithilfe der Verwaltungskonsole für Paket ADAL hinzu.

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
`

- Sie sollten feststellen, dass zwei Bibliotheksverweise auf jedes Projekt - hinzugefügt werden, den PCL Teil ADAL und Plattform-spezifische Anteil an.

-   Öffnen Sie das Projekt DirectorySearcherLib `DirectorySearcher.cs`. Ändern Sie die Werte der Klasse entsprechend die Werten, die Sie in der Azure-Portal eingeben. Code werden diese Werte verwiesen werden, wenn es ADAL verwendet.
    -   Die `tenant` ist die Domäne Ihrer Azure AD-Mandanten, z. B. contoso.onmicrosoft.com
    -   Die `clientId` ClientId Ihrer Anwendung, die Sie kopiert, auf dem Portal haben ist.
    - Die `returnUri` ist die eingegebene im Portal, z. B. RedirectUri `http://DirectorySearcher`.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. ADAL abzurufenden Token aus AAD verwenden*
*Nahezu* alle des app Authentifizierungslogik liegt in `DirectorySearcher.SearchByAlias(...)`. In der Plattform-spezifische Projekte benötigt wird lediglich einen kontextbezogenen Parameter zu übergeben die `DirectorySearcher` PCL.

- Öffnen Sie zuerst `DirectorySearcher.cs` , und fügen Sie einen neuen Parameter in der `SearchByAlias(...)` Methode. `IPlatformParameters`ist der kontextbezogene Parameter, der die Plattform-Objekten einschließt, für die ADAL Authentifizierung durchzuführen muss.

```C#
public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
{
```

-   Als Nächstes Initialisierung der `AuthenticationContext` -ADAL primäre Klasse ist. Dies ist, wo Sie ADAL übergeben die Koordinaten zur Kommunikation mit Azure AD werden muss. Rufen Sie anschließend `AcquireTokenAsync(...)`, welche akzeptiert die `IPlatformParameters` Objekt und ruft illustrieren Authentifizierung erforderlich, um ein Token bei der app zurückzugeben.

```C#
...
    AuthenticationResult authResult = null;
    try
    {
        AuthenticationContext authContext = new AuthenticationContext(authority);
        authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
    }
    catch (Exception ee)
    {
        results.Add(new User { error = ee.Message });
        return results;
    }
...
```
- `AcquireTokenAsync(...)`versucht zunächst ein Token für die angeforderten Ressource (in diesem Fall die Graph-API) zurück, ohne dass der Benutzer seine Anwendungsinformationen eingeben (über Zwischenspeichern oder Aktualisieren von alten Token). Falls erforderlich, wird es dem Benutzer die Vorzeichen Azure AD-Seite angezeigt bevor Sie das angeforderte Token.


- Sie können das Zugriffstoken auf die Anforderung Graph-API in der Kopfzeile Autorisierung fügen Sie:

```C#
...
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
...
```

Das ist alles für die `DirectorySearcher` PCL und Ihre app der Identität-bezogene Code.  Übrig bleibt Nummer der `SearchByAlias(...)` Methode in Ihrer Plattform Ansichten, und der erforderlichen Code für den Umgang mit ordnungsgemäß des Lebenszyklus Benutzeroberfläche hinzufügen.

####<a name="android"></a>Android:
- In `MainActivity.cs`, fügen Sie einen Anruf an `SearchByAlias(...)` klicken Sie auf der Schaltfläche Ereignishandler auf:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
```
- Sie müssen auch außer Kraft setzen die `OnActivityResult` Lebenszyklus-Methode, um alle Authentifizierung weiterleiten leitet wieder in die geeignete Methode.  ADAL bietet eine Methode Helper für diese in Android:

```C#
...
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
...
```

####<a name="windows-desktop"></a>Windows-Desktop:
- In `MainWindow.xaml.cs`, einfach tätigen eines Anrufs zu `SearchByAlias(...)` übergeben einer `WindowInteropHelper` in des Desktops des `PlatformParameters` Objekt:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="ios"></a>iOS:
- In `DirSearchClient_iOSViewController.cs`, iOS `PlatformParameters` Objekt übernimmt einfach einen Verweis auf den Controller anzeigen:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="windows-universal"></a>Windows-Dienst:
- Öffnen Sie in Windows-universeller `MainPage.xaml.cs` und Implementieren der `Search` -Methode, die eine Methode Helper in einem freigegebenen Projekt verwendet werden, um die Benutzeroberfläche nach Bedarf aktualisiert werden.

```C#
...
    List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

Herzlichen Glückwunsch! Sie verfügen nun über eine arbeiten Xamarin-app, die bietet die Möglichkeit zur Benutzerauthentifizierung und sicheres Aufrufen von Web-APIs OAuth 2.0 auf fünf verschiedenen Plattformen verwenden. Wenn Sie noch nicht geschehen ist, ist jetzt die Zeit in Ihrem Mandanten für einige Benutzer gefüllt wird. Führen Sie die DirectorySearcher-app, und melden Sie sich mit einem dieser Benutzer. Suchen Sie nach anderen Benutzern basierend auf ihrem Benutzerprinzipalnamen.

ADAL erleichtert die allgemeine Identität Features in der app integrieren. Es übernimmt alle Prozesse laufen für Sie - Cache-Verwaltung, OAuth Protokoll Support Vorführung des Benutzers mit einer Login UI, Aktualisieren von abgelaufenen Token und vieles mehr. Sie wirklich wissen müssen lediglich einen einzelnen API Anruf `authContext.AcquireToken*(…)`.

Als Referenz die fertige Stichprobe (ohne die Konfigurationswerte) zur Verfügung gestellt wird [hier](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Sie können jetzt auf zusätzliche Identität Szenarien verschieben, klicken Sie auf. Möglicherweise möchten versuchen:

[Sichern eine .NET Web-API mit Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
