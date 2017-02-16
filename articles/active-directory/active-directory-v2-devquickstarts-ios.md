<properties
    pageTitle="Azure AD-Version 2.0 iOS App | Microsoft Azure"
    description="So erstellen Sie eine app für iOS, die in der Benutzer mit der beiden persönlichen Microsoft-Konto anmeldet und geschäftlichen oder schulnotizbücher Konten mithilfe von Drittanbieter-Bibliotheken."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="brandwe"/>

# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Hinzufügen von Anmeldung zu einer mit einer Drittanbieter-Bibliothek mit Graph-API mit der Version 2.0-Endpunkt iOS-app

Die Microsoft-Identitätsplattform verwendet offene Standards wie OAuth2 und OpenID verbinden. Jede Bibliothek können Entwickler, die sie aus mit unserer Dienste integriert werden soll. Damit werden Entwickler unserer Plattform mit anderen Bibliotheken verwenden, haben wir ein paar Vorgehensweisen wie diesen Termin, um zu veranschaulichen, wie Drittanbieter-Bibliotheken für die Verbindung zur Microsoft-Identitätsplattform konfigurieren geschrieben. Die meisten Bibliotheken, die [der Spezifikation RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) implementieren können mit der Microsoft-Identitätsplattform verbinden.

Benutzer können mit der Anwendung, die diese exemplarische Vorgehensweise erstellt wird, melden Sie sich bei ihrer Organisation und suchen Sie nach anderen Personen in ihrer Organisation mithilfe der Graph-API.

Wenn Sie OAuth2 oder OpenID Verbinden nicht vertraut sind, möglicherweise viele dieser Konfiguration Stichprobe nicht Ihnen sinnvoll. Es empfiehlt sich, dass Sie [Version 2.0 Protokolle - OAuth 2.0 Autorisierung Code Datenfluss](active-directory-v2-protocols-oauth-code.md) finden Sie unter Hintergrund.


> [AZURE.NOTE]
    Einige Features von unserer Plattform, die einen Ausdruck in den Standards OAuth2 oder OpenID verbinden, wie bedingte Access und Intune Richtlinie Verwaltung verfügen erfordern Sie unseren Quelle öffnen Microsoft Azure Identität Bibliotheken verwenden.

Der Endpunkt Version 2.0 unterstützt nicht alle Azure Active Directory-Szenarien und Features.

> [AZURE.NOTE]
    Um festzustellen, ob den Version 2.0-Endpunkt verwendet werden sollen, erfahren Sie, [Version 2.0 Einschränkungen](active-directory-v2-limitations.md).

## <a name="download-code-from-github"></a>Herunterladen von Code aus GitHub
Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2)verwaltet.  Wenn Sie nachvollziehen möchten, können Sie Sie [Herunterladen des app Gerüst als eine ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) oder das Gerüst Klonen:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

Sie können auch einfach die Stichprobe herunterladen und sofort beginnen:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Registrieren einer app
Erstellen Sie eine neue app bei der [Anwendung Registrierung Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), oder führen Sie die detaillierten Schritte an, [wie eine app mit den Endpunkt Version 2.0 registrieren](active-directory-v2-app-registration.md).  Vergewissern Sie sich an:

- Kopieren Sie die **Id der Anwendung** , die zu Ihrer Anwendung zugeordnet ist, da Sie es bald benötigen.
- Fügen Sie die **Mobile** -Plattform für Ihre app hinzu.
- Kopieren Sie den **URI umleiten** aus dem Portal ein. Sie müssen den Standardwert verwenden `urn:ietf:wg:oauth:2.0:oob`.


## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a>Herunterladen von Drittanbietern NXOAuth2 Bibliothek und Erstellen eines Arbeitsbereichs

Für diese exemplarische Vorgehensweise verwenden Sie die OAuth2Client aus GitHub, also eine Bibliothek OAuth2 für Mac OS X und iOS (Kakao und Kakao berühren). Diese Bibliothek basiert auf Entwurf 10 der Spezifikation OAuth2. Das Profil systemeigene Anwendung implementiert, und den Endpunkt Autorisierung des Benutzers unterstützt. Dies sind alle Punkte, die Sie benötigen, mit der Microsoft-Identitätsplattform integriert werden soll.

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a>Hinzufügen der Bibliothek zu einem Projekt mit CocoaPods

CocoaPods ist Abhängigkeit Manager für Xcode Projekte. Es verwaltet automatisch die vorherigen Installationsschritte.

```
$ vi Podfile
```
1. Fügen Sie zu diesem Podfile Folgendes ein:

    ```
     platform :ios, '8.0'

     target 'QuickStart' do

     pod 'NXOAuth2Client'

     end
    ```

2. Laden Sie die Podfile mithilfe von CocoaPods ein. Dadurch wird einen neuen Xcode Arbeitsbereich erstellt, den Sie laden werden.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a>Untersuchen Sie die Struktur des Projekts

Die folgende Struktur ist für unsere Projekt in das Gerüst eingerichtet:

- Eine Masteransicht mit einer Benutzerprinzipalnamen-Suche
- Eine Detailansicht für die Daten über den ausgewählten Benutzer
- Eine Stelle, an der ein Benutzer bei der app anmelden kann Abfragen im Diagramm Login-Ansicht

Wir werden in verschiedenen Dateien in das Gerüst verschoben, Authentifizierung hinzufügen. Andere Teile des Codes, wie etwa die visuellen Code befassen sich nicht auf Identität jedoch für Sie vorgesehen sind.

## <a name="set-up-the-settingsplst-file-in-the-library"></a>Richten Sie die Datei settings.plst in der Bibliothek

-   Öffnen Sie im Schnellstart Projekt, das `settings.plist` Datei. Ersetzen Sie die Werte der Elemente im Abschnitt zu den Werten, die Sie in der Azure-Portal verwendet. Code wird diese Werte verwiesen werden, wenn es der Active Directory-Authentifizierungsbibliothek verwendet.
    -   Die `clientId` ist die Client-ID der Anwendung, die Sie in das Portal kopiert haben.
    -   Die `redirectUri` ist die URL der Umleitung, die im Portal bereitgestellt.

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a>Einrichten von der Bibliothek NXOAuth2Client in Ihrem LoginViewController

Die Bibliothek NXOAuth2Client erfordert einige Werte einrichten zu erhalten. Nachdem Sie diesen Vorgang abgeschlossen haben, können Sie das erworbene Token verwenden, das Graph-API aufrufen. Da `LoginView` werden als jederzeit Beginn wir authentifizieren müssen bezeichnet, ist es sinnvoll, von Konfigurationswerten in dieser Datei setzen.

- Lassen Sie uns Hinzufügen einiger Werte in der `LoginViewController.m` Datei für Authentifizierung und Autorisierung im Kontext festlegen. Details zu den Werten, folgen Sie den Code.

    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

Details zu den Code wollen.

Die erste Zeichenfolge ist für `scopes`.  Die `User.Read` Wert können Sie das grundlegende Profil der Anmeldung bei Benutzer zu lesen.

Sie können über alle verfügbaren Bereiche in [Microsoft Graph Berechtigung Bereichen](https://graph.microsoft.io/docs/authorization/permission_scopes)informieren.

Für `authURL`, `loginURL`, `bhh`, und `tokenURL`, verwenden Sie die Werte, die zuvor bereitgestellt. Wenn Sie die Quelle öffnen Microsoft Azure Identität Bibliotheken verwenden, ziehen Sie wir diese Daten nach unten für Sie mithilfe von unserem Metadaten-Endpunkt. Wir haben die Festplatte Extrahieren der folgenden Werte für Sie erledigt.

Die `keychain` Wert ist der Container, der die Bibliothek NXOAuth2Client zum Erstellen einer Schlüsselbund zum Speichern Ihrer Tokens verwendet werden. Wenn Sie über zahlreiche apps einmaliges Anmelden (SSO) erhalten möchten, können Sie den gleichen Schlüsselbund in jede Anwendung angeben und die Verwendung von dieser Schlüsselbund in Ihre Ansprüche Xcode anfordern. Dies wird in der Apple-Dokumentation erläutert.

Die restlichen diese Werte sind erforderlich, verwenden Sie die Bibliothek, und erstellen Orte für Sie Werte auf den Kontext ausführen.

### <a name="create-a-url-cache"></a>Erstellen eines URL-Caches

Innerhalb `(void)viewDidLoad()`, das heißt immer nach dem Laden der Ansicht, der folgende Code gleicht einen Cache für unsere Zwecke.

Fügen Sie den folgenden Code ein:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>Erstellen einer WebView-Anmeldung

Eine WebView kann Benutzer für weitere Faktoren wie SMS-Textnachricht (falls konfiguriert) auffordern oder Fehlermeldungen an den Benutzer zurück. Hier erhalten Sie festlegen der WebView nach oben, und Schreiben Sie dann später den Code, um den Rückruf verarbeitet, die in der WebView von der Identität Services ausgeführt wird.

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a>Überschreiben Sie die WebView Methoden für die Authentifizierung

Um die WebView mitteilen, was passiert, wenn ein Benutzer muss sich anmelden, wie zuvor beschrieben, können Sie den folgenden Code einzufügen.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a>Schreiben von Code, um das Ergebnis der Anfrage OAuth2 zu behandeln.

Mit dem folgende Code wird die RedirectURL bearbeiten, die aus der WebView zurückgibt. Wenn Authentifizierung nicht erfolgreich war, wird der Code erneut ausführen. In der Zwischenzeit wird die Bibliothek des Fehlers bereitstellen, die Sie finden Sie in der Verwaltungskonsole oder asynchrone helfen können.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a>Einrichten von OAuth Kontext (Konto Store genannt)

Sie können hier anrufen `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` im Store freigegebenen Konto für jeden Dienst, der die Anwendung zugreifen können soll. Kontotyp ist eine Zeichenfolge, die als Bezeichner für einen bestimmten Dienst verwendet wird. Da Sie die Graph-API zugreifen, der Code verweist darauf als `"myGraphService"`. Sie richten Sie dann einen Beobachter, der informieren wird, wenn etwas mit dem Token geändert wird. Nachdem Sie das Token erhalten, Sie die Informationen der Benutzer zurück zu den `masterView`.



```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a>Einrichten der Folienmasteransicht zu suchen, und zeigen Sie die Benutzer aus der Graph-API

Eine Master-Shape-View-Controller (MVC)-app, die die zurückgegebenen Daten im Raster zeigt ist nicht Gegenstand diese Anleitung, und viele online Lernprogramme erläutert, wie Sie eine erstellen. Mit diesem Code wird in der Datei Gerüst. Jedoch müssen Sie ein paar Punkte, die in dieser Anwendung MVC behandelt:

* Wenn ein Benutzer etwas in das Suchfeld eingibt ACHSENABSCHNITT
* Bereitstellen eines Objekts mit Daten wieder in die MasterView, sodass die Ergebnisse im Raster angezeigt werden kann

Wir werden die folgenden ausführen.

### <a name="add-a-check-to-see-if-youre-logged-in"></a>Fügen Sie eine Prüfung durch, um festzustellen, ob Sie angemeldet sind

Die Anwendung unterstützt kleinen, wenn der Benutzer sich nicht angemeldet ist, dass er smart prüfen, ob im Cache bereits Token ist ist. Wenn dies nicht der Fall ist, Sie leiten Sie an die LoginView für den Benutzer anmelden. Wenn diese nicht zurückgerufen, ist die beste Methode zum Aktionen auszuführen, beim Laden einer Ansicht mit den `viewDidLoad()` Methode, Apple uns bereitstellt.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a>Aktualisieren der Tabellenansicht aus, wenn Daten empfangen werden

Wenn die Graph-API Daten zurückgibt, müssen Sie die Daten anzuzeigen. Zur Vereinfachung wurde hier der gesamte Code in der Tabelle aktualisieren. Sie können nur die richtigen Werte in Ihrem MVC häufig verwendeter Code einfügen.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a>Bieten Sie eine Möglichkeit, die Graph-API aufzurufen, wenn eine Person in das Suchfeld eingibt

Wenn ein Benutzer eine Suche in das Feld eingibt, müssen Sie mit der Graph-API programmiersprachlichen über. Die `GraphAPICaller` Klasse, die Sie in den folgenden Code erstellen werden, trennt die Nachschlagefunktionen von der Präsentation. Schreiben Sie uns jetzt den Code, der alle Suchzeichen der Graph-API feeds. Wir führen können, indem Sie eine Methode namens `lookupInGraph`, der akzeptiert der Zeichenfolge, die wir suchen möchten.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a>Schreiben Sie eine Helper-Klasse, um die Graph-API zugreifen

Dies ist das Herzstück unserer Anwendung. Während der restlichen Code in das standardmäßige MVC Muster aus Apple einfügen wurde, Schreiben Sie hier Code ein, um die Abfrage im Diagramms während der Eingabe, und klicken Sie dann die Daten zurück. Hier finden Sie der Code, und eine ausführliche Erläuterung ergibt sich.

### <a name="create-a-new-objective-c-header-file"></a>Erstellen einer neuen Ziel C Kopfzeile-Datei

Benennen Sie die Datei `GraphAPICaller.h`, und fügen Sie den folgenden Code hinzu.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Hier sehen Sie, dass eine angegebene Methode eine Zeichenfolge akzeptiert und eine CompletionBlock gibt an. Dieser CompletionBlock, wird als Sie vielleicht können, die Tabelle aktualisieren können, indem ein Objekt mit eingetragenen Daten in Echtzeit als die Benutzer suchen.


### <a name="create-a-new-objective-c-file"></a>Erstellen einer neuen Ziel C-Datei

Benennen Sie die Datei `GraphAPICaller.m`, und fügen Sie die folgende Methode hinzu.

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

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
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


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

```

Aufzurufen Sie lassen Sie uns, diese Methode im Detail.

Das Herzstück dieser Code befindet sich in der `NXOAuth2Request`, Methode nimmt die Parameter, die Sie bereits definiert haben in der Datei settings.plist.

Dieser erste Schritt besteht, um den richtigen Graph-API Anruf zu erstellen. Da Sie anrufen `/users`, Sie angeben, indem Sie es, die der Ressource: Grafik-API zusammen mit der Version anfügen. Es ist sinnvoll, um diese Elemente in einer externen Einstellungsdatei setzen, da diese die API Weiterentwicklung ändern können.


```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Als Nächstes müssen Sie Parameter angeben, die auch für den Anruf Graph-API bereitgestellt werden. Es ist *Wichtig* , dass die Parameter nicht da, die für alle nicht-URI übereinstimmenden Zeichen zur Laufzeit bereinigt wird in den Endpunkt Ressource setzen. Alle Abfragecode muss in den Parametern angegeben werden.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Dies ruft Umständen eine `convertParamsToDictionary` Methode, die Sie noch nicht geschrieben haben. Führen wir nun am Ende der Datei:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
Als Nächstes verwenden wir die `NXOAuth2Request` Methode, um Daten aus der-API im JSON-Format zurückzukehren.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Schließlich wollen wie Sie die Daten an die MasterViewController zurückzugeben. Die Daten als seriell gibt und deserialisiert und in ein Objekt, das die MainViewController nutzen kann geladen werden müssen. Zu diesem Zweck das Gerüst weist eine `User.m/h` Datei, die ein Benutzerobjekt erstellt wird. Sie füllen das Objekt Benutzer mit Informationen aus dem Diagramm.

```objc
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
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a>Führen Sie das Beispiel

Wenn Sie das Gerüst verwendet haben, oder zusammen mit der exemplarische Vorgehensweise gefolgt sollten nun eine Anwendung ausführen. Starten Sie den Simulator, und klicken Sie auf **Anmelden** , um die Anwendung zu verwenden.

## <a name="get-security-updates-for-our-product"></a>Abrufen von Sicherheitsupdates für unser Produkt

Wir empfehlen Ihnen Sicherheitsvorfälle auftreten, besuchen die [Sicherheits-TechCenter](https://technet.microsoft.com/security/dd252948) und Abonnieren von Ihren Sicherheitshinweisen Benachrichtigungen erhalten.
