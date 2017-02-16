<properties
    pageTitle="Azure Active Directory B2C: Rufen Sie eine Web-API aus einer Drittanbieter-Bibliotheken mit iOS-Anwendung | Microsoft Azure"
    description="In diesem Artikel wird erläutert, wie Sie eine iOS 'Vorgangsliste'-app zu erstellen, die eine Web Node.js API ruft mithilfe von OAuth 2.0 Person Token mithilfe einer Drittanbieter-Bibliothek"
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

< tags ms.service= "Active Directory b2c" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic= "Hero-Artikel"

    ms.date="07/26/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c--call-a-web-api-from-an-ios-application-using-a-third-party-library"></a>Azure AD-B2C: Rufen eine Web-API einer iOS-Anwendung, die mit einer Drittanbieter-Bibliothek

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Die Microsoft-Identitätsplattform verwendet offene Standards wie OAuth2 und OpenID verbinden. Dies ermöglicht Entwicklern eine Bibliothek nutzen unserer Dienste integriert werden sollen. Entwickler, die sich mit unserer Plattform mit anderen Bibliotheken zur Makrofunktion haben wir einige Exemplarische Vorgehensweisen, wie diese in Demonstate geschrieben Drittanbieter-Bibliotheken, die Verbindung zur Microsoft-Identitätsplattform zu konfigurieren. Die meisten Bibliotheken, die [die RFC6749 OAuth2 Spezifikation](https://tools.ietf.org/html/rfc6749) implementieren werden Verbindung zur Microsoft Identity-Plattform hergestellt.


Wenn Sie OAuth2 oder OpenID verbinden neu ist möglicherweise viele dieser Konfiguration Stichprobe nicht viel Ihnen sinnvoll. Es empfiehlt sich, dass Sie eine kurze [Übersicht über das Protokoll aus, das wir hier dokumentierten haben](active-directory-b2c-reference-protocols.md)betrachten.

> [AZURE.NOTE]
    Einige Features von unserer Plattform, die einen Ausdruck in dieser Standards, wie bedingte Zugriff und Intune Richtlinie Verwaltung verfügen müssen Sie unsere Quelle öffnen Microsoft Azure Identität Bibliotheken verwenden. 
   
Nicht alle Azure Active Directory-Szenarien und Features werden von der B2C-Plattform unterstützt.  Um festzustellen, ob die B2C-Plattform verwendet werden sollen, erfahren Sie [B2C Einschränkungen](active-directory-b2c-limitations.md).


## <a name="get-an-azure-ad-b2c-directory"></a>Abrufen einer Azure AD B2C-Verzeichnis

Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis erstellen, oder den Mandanten. Ein Verzeichnis ist ein Container für alle Benutzer, apps, Gruppen und mehr. Wenn Sie eine besitzen bereits, fortsetzen [Erstellen Sie ein Verzeichnis B2C](active-directory-b2c-get-started.md) , bevor Sie.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Als Nächstes müssen Sie eine app in Ihrem Verzeichnis B2C erstellen. Dadurch Azure AD-Informationen, die sichere Kommunikation mit Ihrem app benötigt werden. Sowohl die app als auch im Web-API werden durch eine einzelne **Anwendung-ID** in diesem Fall dargestellt, weil eine logische app bestehen aus. [Wenn Sie eine app erstellen, gehen Sie wie folgt vor.](active-directory-b2c-app-registration.md) Achten Sie darauf, um:

- Fügen Sie ein **mobiles Gerät** in die Anwendung.
- Kopieren Sie die **ID der Anwendung** , die zu Ihrer Anwendung zugeordnet ist. Sie auch benötigen diese später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen Sie Ihre Richtlinien

In Azure AD B2C wird jede Erfahrungen von einer [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Diese app enthält eine Identität Erfahrung: kombinierte bei der Anmeldung anmelden sowie Anmeldung. Sie müssen dieser Richtlinie jedes Typs zu erstellen, wie in der [Richtlinie Bezug Artikel](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)beschrieben. Wenn Sie die Richtlinie erstellen, müssen Sie unbedingt:

- Wählen Sie den **Anzeigenamen** und die Anmeldung Attribute in Ihrer Richtlinie ein.
- Wählen Sie die Anwendung Ansprüche **Anzeigenamen** und **Objekt-ID** in jeder Richtlinie aus. Sie können auch anderen Ansprüche auswählen.
- Kopieren Sie den **Namen** der einzelnen Richtlinie, nachdem Sie es erstellt haben. Es sollte das Präfix haben `b2c_1_`.  Sie benötigen später den Namen der Richtlinie.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie Ihre Richtlinien erstellt haben, sind Sie bereit sind, Ihre app zu erstellen.


## <a name="download-the-code"></a>Laden Sie den code

Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)verwaltet.  Wenn Sie nachvollziehen möchten, können Sie /archive/master.zip [Herunterladen der app als ein ZIP](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)) oder Klonen Sie es:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Oder nur herunterladen Sie den fertigen Code, und legen Sie sofort Los: 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## <a name="download-the-third-party-library-nxoauth2-and-launch-a-workspace"></a>Herunterladen der Drittanbieter-Bibliothek nxoauth2 und Starten eines Arbeitsbereichs

Für diese exemplarische Vorgehensweise verwenden wir die OAuth2Client aus GitHub, eine Bibliothek OAuth2 für Mac OS X & iOS (Kakao & Kakao berühren). Diese Bibliothek basiert auf Entwurf 10 der Spezifikation OAuth2. Das Profil systemeigene Anwendung implementiert, und den Endbenutzer Autorisierung Endpunkt unterstützt. Dies sind alle Punkte, die wir in Reihenfolge auf zahlreiche mit Microsoft Identitätsplattform müssen.

### <a name="adding-the-library-to-your-project-using-cocoapods"></a>Hinzufügen der Bibliothek zu einem Projekt mit CocoaPods

CocoaPods ist Abhängigkeit Manager für Xcode Projekte. Sie haben die obigen Installationsschritte automatisch verwaltet.

```
$ vi Podfile
```
Fügen Sie zu diesem Podfile Folgendes ein:

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

Laden Sie jetzt die Podfile Cocoapods verwenden. Dadurch wird ein neuer XCode Arbeitsbereich erstellt, Sie laden werden.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## <a name="the-structure-of-the-project"></a>Die Struktur des Projekts

Wir haben die folgende Struktur für unsere Projekt in das Gerüst einrichten:

* Eine **Masteransicht** mit eines Aufgabenbereichs
* Eine **Vorgangsansicht hinzufügen** für die Daten zu ausgewählten Vorgang
* Eine **Anmeldung anzeigen** , die dem Benutzer die app anmelden können.

Wir werden in zu verschiedenen Dateien im Projekt Authentifizierung hinzufügen springen. Andere Teile des Codes wie den visual-Code ist nicht für die Identität von Belang und für Sie vorgesehen sind.

## <a name="create-the-settingsplist-file-for-your-application"></a>Erstellen der `settings.plist` Datei für eine Anwendung

Es ist einfacher zum Konfigurieren der Anwendung, wenn wir einen zentralen Ort zum setzen unsere Konfigurationswerte haben. Darüber hinaus hilft Ihnen zu verstehen, was jede Einstellung in Ihrer Anwendung bedeutet. Wir nutzen Sie die *Liste der Eigenschaft* als eine Möglichkeit, um diese Werte mit der Anwendung bereitzustellen.

* Erstellen/Öffnen der `settings.plist` Datei unter `Supporting Files` in Ihrer Anwendungsarbeitsbereich

* Geben Sie die folgenden Werte (wir werden diese im Detail bald durchgehen)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

Wechseln Sie wir zu diese im Detail.


Für `authURL`, `loginURL`, `bhh`, `tokenURL` sehen Sie in Ihrem Mandanten Namen ausfüllen müssen. Dies ist der Name Mandant Ihrer B2C Mandanten, die Ihnen zugewiesen wurde. Beispielsweise `kidventusb2c.onmicrosoft.com`. Wenn Sie unseren Quelle öffnen Microsoft Azure Identität Bibliotheken verwenden möchten wir diese Daten für über unsere Metadaten-Endpunkt ziehen Sie nach unten. Wir haben die Festplatte Extrahieren der folgenden Werte für Sie erledigt.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Die `keychain` Wert ist der Container, der die Bibliothek NXOAuth2Client zum Erstellen einer Schlüsselbund zum Speichern Ihrer Tokens verwendet werden. Wenn Sie über zahlreiche apps SSO zu erhalten möchten können Sie angeben den gleichen Schlüsselbund in jede Anwendung sowie die Verwendung von dieser Schlüsselbund in Ihrem XCode Entitements anfordern. Dies wird in der Apple-Dokumentation behandelt.

Die `<policy name>` am Ende jeder URL werden an folgenden Stellen, wo, setzen die Richtlinie, die Sie soeben erstellt haben. Die app ruft diese Richtlinien je nach den Datenfluss.

Die `taskAPI` den REST-Endpunkt wir Ruft mit Ihr B2C Token entweder hinzufügen, Aufgaben oder Abfrage vorhandene Vorgänge ist. Dies wurde speziell für dieses Beispiel eingerichtet. Sie brauchen sie für die Stichprobe für die Arbeit ändern.

Die restlichen diese Werte müssen mithilfe der Bibliothek und einfach erstellen, stellen Sie Werte auf den Kontext auszuführen.

Nun wir verfügen die `settings.plist` Datei erstellt haben, benötigen wir Code, um sie zu lesen.

## <a name="set-up-a-appdata-class-to-read-our-settings"></a>Einrichten einer Klasse AppData, lesen unseren Einstellungen

Wir stellen eine einfache Datei, die gerade analysiert unsere `settngs.plist` Datei wir über erstellt, und nehmen Sie diese Einstellungen verfügbaren in der Zukunft an eine beliebige Klasse. Da wir wünschen, erstellen Sie eine neue Instanz von Daten jedes Mal, wenn eine Klasse dafür gefragt werden, wir verwenden Sie ein Muster für einzelne und die gleiche Instanz erstellt jedes Mal, wenn eine Anforderung für die Einstellungen erfolgt nur zurück

* Erstellen einer `AppData.h` Datei:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Erstellen einer `AppData.m` Datei:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Jetzt wir einfach in unserem aufrufen können, indem Sie einfach aufrufen `  AppData *data = [AppData getInstance];` dazu unsere Klassen, wie Sie sehen unter.



## <a name="set-up-the-nxoauth2client-library-in-your-appdelegate"></a>Einrichten von der Bibliothek NXOAuth2Client in Ihrem AppDelegate

Die Bibliothek NXOAuthClient erfordert einige Werte einrichten zu erhalten. Nach dem Abschluss können Sie das Token verwenden, das die REST-API Aufrufen aufgerufenen ist. Da wir wissen, dass die `AppDelegate` jederzeit wir die Anwendung Laden aufgerufen wird es ist sinnvoll, dass wir unsere Konfigurationswerte in dieser Datei setzen.
* Open `AppDelegate.m` Datei

* Importieren Sie einige Header-Dateien, die wir später verwendet wird.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* Hinzufügen der `setupOAuth2AccountStore` Methode in der AppDelegate

Erstellen eine AccountStore und leiten ihn in die Daten, die wir gerade in ab lesen müssen wir die `settings.plist` Datei.

Es gibt einige Aktionen, denen Sie zum B2C-Dienst an diesem Punkt bedenken sollten, die diesen Code verständlicher machen:


1. Azure AD B2C verwendet die *Richtlinie* wie die Abfrageparameter genannt Ihre Anfrage an. Dadurch wird ein Azure Active Directory als einer unabhängigen Dienst nur für eine Anwendung fungieren. Um diese Abfrage zusätzliche Parameter wir bieten müssen Bereitstellen der `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` Methode mit unseren benutzerdefinierte Richtlinienparametern. 

2. Azure AD B2C verwendet Bereiche im Wesentlichen genauso wie anderen OAuth2 Server. Da die Verwendung von B2C so viele Informationen zu Benutzerauthentifizierung, als den Zugriff auf Ressourcen ist sind einige Bereiche jedoch absolut in Reihenfolge für den Flow ordnungsgemäß funktioniert erforderlich. Dies ist die `openid` Bereich. Unsere Microsoft Identität SDKs automatisch an die `openid` Bereich für Sie, damit Sie nicht, die in unseren SDK-Konfiguration sehen. Da wir eine Drittanbieter-Bibliothek verwenden, müssen wir jedoch diesen Bereich angeben.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Im nächsten Schritt stellen Sie sicher, dass Sie in der AppDelegate unter Aufrufen `didFinishLaunchingWithOptions:` Methode. 

```
[self setupOAuth2AccountStore];
```


## <a name="create-a-loginviewcontroller-class-that-we-will-use-to-handle-authentication-requests"></a>Erstellen einer `LoginViewController` Klasse, die wir Authentifizierungsanfragen verarbeitet verwendet wird

Wir verwenden eine Webview für Konto anmelden. Dadurch konnte zur Bestätigung der des Benutzers für zusätzliche Faktoren wie SMS-Textnachricht (falls konfiguriert), oder geben Sie dem Benutzer Fehlermeldungen zurück. Hier werden wir Festlegen der Webview nach oben, und Schreiben Sie dann später den Code, um den Rückruf verarbeitet, die in der WebView aus dem Microsoft-Dienst Identität ausgeführt wird.

* Erstellen einer `LoginViewController.h` Klasse

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

Wir können jede dieser Methoden unten erstellen.

> [AZURE.NOTE] 
    Stellen Sie sicher, dass Sie binden der `loginView` an der tatsächlichen Webview, die innerhalb der Storyboard ist. Andernfalls müssen Sie keine Webview, das Popup können, wenn authentifiziert werden soll.

* Erstellen einer `LoginViewController.m` Klasse

* Fügen Sie einige Variablen Zustand Ereignisinformationen, da wir authentifizieren hinzu

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* Überschreiben Sie die WebView Methoden für die Authentifizierung

Müssen wir die Webview das Verhalten mitgeteilt wird, die, das wir möchten, wenn ein Benutzer muss sich anmelden, wie oben beschrieben. Sie können einfach Ausschneiden und fügen Sie den folgenden Code.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* Schreiben von Code, um das Ergebnis der Anfrage OAuth2 zu behandeln.

Wir benötigen Code, die die RedirectURL behandelt, die wieder aus der WebView stammen. Wenn es nicht erfolgreich war, wird es erneut. In der Zwischenzeit wird die Bibliothek des Fehlers bereitstellen, finden Sie in der Verwaltungskonsole oder Asyncronously helfen können. 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* Richten Sie die Benachrichtigung Factory aus.

Wir erstellen wir in konnten derselben Methode der `AppDelegate` oben, jedoch diesmal werden wir einige hinzufügen `NSNotification`s, um uns Ihre Wünsche mitteilen in unseren Dienst geschieht. Wir richten Sie einen Beobachter, der uns mitteilen wird, wenn etwas mit dem Token geändert wird. Sobald wir das Token abrufen zurückgeben wir den Benutzer wieder in die `masterView`.



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* Hinzufügen von Code, der den Benutzer verarbeitet immer, wenn eine Anforderung für systemeigene Signieren initiiert wird

Erstellen wir eine Methode, die aufgerufen wird, wenn eine Anforderung für die Authentifizierung müssen. Dies wird die Methode, die eine Webview tatsächlich erstellt sein.

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Nennen schließlich alle diese Methoden, die wir über jedes Mal geschrieben haben wir die `LoginViewController` geladen. Dazu wird der folgenden Methoden zum Hinzufügen von unserem `viewDidLoad` Methode Apple (datumuhrzeitschlüssel)

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

Sie sind jetzt fertig mit erstellen wir eine Interaktion mit unserer Anwendung für die Anmeldung wird Hauptfenster verarbeitet. Nachdem wir angemeldet haben, müssen wir unsere Token verwenden, die wir erhalten haben. Für das Erstellen wir einige Helper Code, der REST-APIs für uns mit dieser Bibliothek aufgerufen wird.


## <a name="create-a-graphapicaller-class-to-handle-our-requests-to-a-rest-api"></a>Erstellen einer `GraphAPICaller` Class unsere Anfragen zu einem REST-API verarbeitet

Wir haben eine Konfiguration geladen jedes Mal, wenn wir unsere app zu laden. Jetzt müssen wir etwas mit tun, sobald ein Token vorliegt. 

* Erstellen einer `GraphAPICaller.h` Datei

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

Sie finden Sie unter aus diesem Code, dass wir zwei Methoden erstellen werden: eine die Aufgaben aus einer API und eine weitere Hinzufügen von Aufgaben an die API abzurufenden.

Jetzt, da wir unsere Schnittstelle eingerichtet haben, fügen Sie die eigentliche Implementierung uns hinzu:

* Erstellen einer`GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## <a name="run-the-sample-app"></a>Führen Sie die Beispielapp

Schließlich erstellen Sie, und führen Sie die app in Xcode. Registrieren oder melden Sie sich bei der app, und Erstellen von Aufgaben für einen Benutzer angemeldet. Melden Sie sich ab und Erstellen von Aufgaben für diesen Benutzer wieder als ein anderer Benutzer anmelden.

Beachten Sie, dass die Aufgaben gespeicherten pro Benutzer zur-API, da die API die Identität des Benutzers aus dem Access-Token extrahiert, den sie erhält.


## <a name="next-steps"></a>Nächste Schritte

Sie können nun auf Erweiterte B2C Themen verschieben. Sie können Folgendes versuchen:

[Anrufen einer Node.js Webs API aus einer Node.js Web app]()

[Anpassen einer app B2C bedienen]()
