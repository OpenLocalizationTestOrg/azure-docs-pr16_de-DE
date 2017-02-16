<properties
    pageTitle="Erste Schritte mit Azure AD-iOS | Microsoft Azure"
    description="So erstellen Sie eine iOS-Anwendung, die für die Anmeldung Azure AD integriert und Azure AD-Anrufe geschützt APIs OAuth verwenden."
    services="active-directory"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-ios-app"></a>Integrieren von Azure AD in einer App für iOS

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Azure AD bietet die Active Directory-Authentifizierungsbibliothek oder ADAL, für iOS-Clients, die Zugriff auf geschützte Ressourcen benötigen.  ADAL des alleinige Zweck im Leben ist für Ihre app abzurufenden Zugriffstoken zu erleichtern.  Um zu veranschaulichen, wie einfach es ist, wird hier wir eine Ziel C Vorgangsliste Anwendung, die zu erstellen:

-   Ruft Token für das Anrufen von der Azure AD Graph-API mit dem [OAuth 2.0-Authentifizierung-Protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx)zugegriffen werden.
-   Sucht in einem Verzeichnis für Benutzer mit einer angegebenen Alias.

Wenn Sie um die gesamte Arbeit der Anwendung zu erstellen, müssen Sie:

2. Registrieren Sie Ihrer Anwendung Azure AD.
3. Installieren und Konfigurieren von ADAL.
5. Verwenden Sie ADAL, um die Token von Azure AD zu gelangen.

Um zu beginnen, [Laden Sie das app-Gerüst](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) oder [Laden Sie das Beispiel fertige](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  Sie benötigen auch einem Azure AD-Mandanten, in dem Sie Benutzer erstellen und Registrieren einer Anwendung.  Wenn Sie noch keinen Mandanten, [erfahren Sie, wie Sie eins zu erhalten](active-directory-howto-tenant.md)haben.

> [AZURE.TIP] Probieren Sie die Vorschau des unserer neuen [Entwicklerportal](https://identity.microsoft.com/Docs/iOS) , die Ihnen den Einstieg und bei Azure Active Directory in wenigen Minuten erhalten helfen!  Die Entwicklerportal führt Sie durch das Verfahren zum Registrieren einer app und Azure AD in Ihren Code zu integrieren.  Wenn Sie fertig sind, haben Sie eine einfache Anwendung, die Benutzer in Ihrem Mandanten und einer Back-End-authentifizieren können, die Token akzeptieren und Validierung ausführen können. 

## <a name="1-determine-what-your-redirect-uri-will-be-for-ios"></a>*1. ermitteln Sie für iOS gehört Ihre URI umleiten*

Damit Ihre Programme in bestimmten Szenarien SSO sicher starten erforderlich ist, dass Sie einen **URI umleiten** in einem bestimmten Format erstellen. Ein umleiten URI wird verwendet, um sicherzustellen, dass die Token an die richtige Anwendung zurück, die für sie aufgefordert werden.

Das iOS-Format für einen URI umleiten lautet:

```
<app-scheme>://<bundle-id>
```

-   **Aap-Farbschema** - dies im Projekt XCode registriert ist. Es ist wie andere Programme aufgerufen werden können. Sie finden diese unter "Info.plist" -> URL-Typen-URL-Bezeichner >. Wenn Sie bereits eine oder mehrere konfiguriert haben, sollten Sie eine erstellen.
-   **Paket-Id** – Dies ist der Paket Bezeichner finden Sie unter "Identität" heben Sie die Markierung der Project-Einstellungen in XCode.

Ein Beispiel für diesen Code Schnellstart wäre: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>*2. die Anwendung DirectorySearcher zu registrieren*
Wenn Ihre app Token abrufen aktivieren möchten, müssen Sie zuerst in Ihrem Azure AD-Mandanten zu registrieren, und gewähren sie über die Berechtigung zum Zugreifen auf der Azure AD Graph-API:

-   Melden Sie sich bei der Azure-Verwaltungsportal
-   Klicken Sie im linken Navigationsbereich auf **Active Directory**
-   Wählen Sie einen Mandanten, in dem die Anwendung registrieren aus.
-   Klicken Sie auf die Registerkarte **Applications** aus, und klicken Sie in der unteren Schublade auf **Hinzufügen** .
-   Folgen Sie den Anweisungen, und erstellen Sie eine neue **Native Client-Anwendung**.
    -   Den **Namen** der Anwendung wird eine Anwendung an Endbenutzer beschrieben.
    -   **Umleiten Uri** ist eine Zeichenfolge und Farbschema Kombination, mit denen Azure AD token Antworten zurückgeben.  Geben Sie einen bestimmten Wert an Ihrer Anwendung anhand der obigen Informationen an.
-   Nachdem Sie die Registrierung abgeschlossen haben, zuweisen AAD Ihre app eine eindeutige Client-ID.  Sie benötigen diesen Wert in den nächsten Abschnitten, also kopieren Sie sie von der Registerkarte **Konfigurieren** .
- Registerkarte **Konfigurieren** , suchen Sie auch im Abschnitt "Berechtigungen zu anderen Applications".  Fügen Sie die Berechtigung **Directory Access Ihrer Organisation** , klicken Sie unter **Delegierte Berechtigungen**für die Anwendung "Azure Active Directory" hinzu.  Dadurch wird eine Anwendung Abfragen die Graph-API für Benutzer aktiviert werden.

## <a name="3-install--configure-adal"></a>*3: Installieren und Konfigurieren von ADAL*
Jetzt, da Sie eine Anwendung in Azure AD haben, können Sie ADAL installieren und Schreiben von Code Identität Zusammenhang.  In der Reihenfolge für ADAL mit Azure AD kommunizieren können müssen Sie es einige Informationen über Ihre app-Registrierung zur Verfügung zu stellen.
-   Durch Hinzufügen von ADAL zum Verwenden von Cocapods DirectorySearcher Projekt beginnen.

```
$ vi Podfile
```
Fügen Sie zu diesem Podfile Folgendes ein:

```
source 'https://github.com/CocoaPods/Specs.git'
link_with ['QuickStart']
xcodeproj 'QuickStart'

pod 'ADALiOS'
```

Laden Sie jetzt die Podfile Cocoapods verwenden. Dadurch wird ein neuer XCode Arbeitsbereich erstellt, Sie laden werden.

```
$ pod install
...
$ open QuickStart.xcworkspace
```

-   Öffnen Sie in das Projekt Schnellstart Plist Datei `settings.plist`.  Ersetzen Sie die Werte der Elemente im Abschnitt zu den Werten, die Sie in der Azure-Portal eingeben.  Code werden diese Werte verwiesen werden, wenn es ADAL verwendet.
    -   Die `tenant` ist die Domäne Ihrer Azure AD-Mandanten, z. B. contoso.onmicrosoft.com
    -   Die `clientId` ClientId Ihrer Anwendung, die Sie kopiert, auf dem Portal haben ist.
    -   Die `redirectUri` ist die Umleitung Url, die Sie im Portal registriert.

## <a name="4--use-adal-to-get-tokens-from-aad"></a>*4 verwenden Sie die ADAL Token aus AAD abrufen*
Das grundlegende Prinzip hinter ADAL ist, wenn Ihre app ein Access-Token benötigt, einfach eine CompletionBlock aufgerufen `+(void) getToken : `, und ADAL erledigt den Rest.  

-   In der `QuickStart` geöffneten Projekt `GraphAPICaller.m` , und suchen Sie die `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` Kommentar im oberen Bereich.  Dies ist, wo Sie ADAL übergeben die Koordinaten durch eine CompletionBlock zur Kommunikation mit Azure AD- und feststellen, wie Token zwischengespeichert.

```ObjC
+(void) getToken : (BOOL) clearCache
           parent:(UIViewController*) parent
completionHandler:(void (^) (NSString*, NSError*))completionBlock;
{
    AppData* data = [AppData getInstance];
    if(data.userItem){
        completionBlock(data.userItem.accessToken, nil);
        return;
    }

    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:data.resourceId
                                 clientId:data.clientId
                              redirectUri:redirectUri
                           promptBehavior:AD_PROMPT_AUTO
                                   userId:data.userItem.userInformation.userId
                     extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                          completionBlock:^(ADAuthenticationResult *result) {

                              if (result.status != AD_SUCCEEDED)
                              {
                                  completionBlock(nil, result.error);
                              }
                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                              }
                          }];
}

```

- Jetzt müssen wir dieses Token verwenden, um für Benutzer im Diagramm zu suchen. Suchen nach der `// TODO: implement SearchUsersList` CommentThis Methode macht eine GET-Anforderung der Azure AD Graph-API Abfrage für Benutzer, dessen UPN mit der angegebenen Suchbegriff beginnt.  Um die Graph-API Abfragen, Sie müssen jedoch eine Access_token in können die `Authorization` Kopfzeile der Anfrage – hier kommt ADAL in.

```ObjC
+(void) searchUserList:(NSString*)searchString
                parent:(UIViewController*) parent
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];


    [self craftRequest:[self.class trimString:graphURL]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

         if (error != nil)
         {
             completionBlock(nil, error);
         }
         else
         {

             NSOperationQueue *queue = [[NSOperationQueue alloc]init];

             [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                 if (error == nil && data != nil){

                     NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                     // We can grab the top most JSON node to get our graph data.
                     NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                     // Don't be thrown off by the key name being "value". It really is the name of the
                     // first node. :-)

                     //each object is a key value pair
                     NSDictionary *keyValuePairs;
                     NSMutableArray* Users = [[NSMutableArray alloc]init];

                     for(int i =0; i < graphDataArray.count; i++)
                     {
                         keyValuePairs = [graphDataArray objectAtIndex:i];

                         User *s = [[User alloc]init];
                         s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                         s.name =[keyValuePairs valueForKey:@"givenName"];

                         [Users addObject:s];
                     }

                     completionBlock(Users, nil);
                 }
                 else
                 {
                     completionBlock(nil, error);
                 }

             }];
         }
     }];

}

```
- Wenn Ihre app ein Token fordert, indem er `getToken(...)`, ADAL versucht, ein Token zurück, ohne Bestätigung für Anmeldeinformationen.  ADAL fest, dass es sich bei der Benutzer muss sich anmelden, ein Token abzurufen, wird es ein Anmeldedialogfeld angezeigt, die Anmeldeinformationen des Benutzers sammeln und Token nach der erfolgreichen Authentifizierung zurückgibt.  Wenn ADAL aus irgendeinem Grund ein Token zurück kann, löst eine `AdalException`.
- Beachten Sie, dass die `AuthenticationResult` -Objekt enthält eine `tokenCacheStoreItem` Objekt, das zum Sammeln von Informationen möglicherweise Sie Ihre app müssen verwendet werden kann.  Klicken Sie in der Schnellstart `tokenCacheStoreItem` wird verwendet, um festzustellen, ob Authenitcation bereits aufgetreten ist.


## <a name="step-5-build-and-run-the-application"></a>Schritt 5: Erstellen Sie, und führen Sie die Anwendung



Herzlichen Glückwunsch! Nun verfügen über eine arbeiten iOS-Anwendung, die die Möglichkeit zum Authentifizieren von Benutzern, sicheres Aufrufen von Web-APIs mit OAuth 2.0 hat und Abrufen grundlegender Informationen über den Benutzer.  Wenn Sie noch nicht geschehen ist, ist jetzt die Zeit in Ihrem Mandanten für einige Benutzer gefüllt wird.  Führen Sie die app Schnellstart, und melden Sie sich mit einem dieser Benutzer.  Suchen Sie nach anderen Benutzern basierend auf ihrem Benutzerprinzipalnamen.  Schließen Sie die app, und führen Sie erneut aus.  Beachten Sie, wie die Sitzung des Benutzers intakt bleibt.

ADAL erleichtert die all diese allgemeine Identität Features in Ihrer Anwendung zu integrieren.  Es übernimmt alle Prozesse laufen für Sie - Cache-Verwaltung, OAuth Protokoll Support Vorführung des Benutzers mit einer Login UI, Aktualisieren von abgelaufenen Token und vieles mehr.  Sie wirklich wissen müssen lediglich einen einzelnen API Anruf `getToken`.

Als Referenz die fertige Stichprobe (ohne die Konfigurationswerte) zur Verfügung gestellt wird [hier](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="additional-scenarios"></a>Zusätzliche Szenarien
Sie können jetzt in zusätzliche Szenarien verschieben, klicken Sie auf.  Möglicherweise möchten versuchen:

- [Sichern einer Node.JS Web-API mit Azure AD](active-directory-devquickstarts-webapi-nodejs.md)
- Informationen Sie [zum Aktivieren von Cross-app SSO unter iOS ADAL verwenden](active-directory-sso-ios.md)  

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
