<properties 
    pageTitle="IOS WebView mit einer systemeigenen Engagement für Mobile iOS SDK zu schließen" 
    description="So erstellen Sie eine Verbindung zwischen WebView Javascript und der systemeigenen Engagement für Mobile iOS SDK ausgeführt werden"      
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-ios" 
    ms.devlang="objective-c" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a>IOS WebView mit einer systemeigenen Engagement für Mobile iOS SDK zu schließen

> [AZURE.SELECTOR]
- [Android-Brücke](mobile-engagement-bridge-webview-native-android.md)
- [iOS Bridge](mobile-engagement-bridge-webview-native-ios.md)

Einige mobile apps dienen als app Hybrid, in dem die app ähneln mit der Entwicklung einer systemeigenen iOS Ziel-C entwickelt, aber einige oder sogar alle Bildschirme innerhalb einer iOS WebView gerendert werden. Sie können weiterhin Engagement für Mobile iOS SDK in solche apps nutzen und dieses Lernprogramms beschrieben, wie Sie wechseln Sie zu diesem Thema. 

Es gibt zwei Vorgehensweisen, dies zu erreichen, obwohl beide nicht dokumentierten sind:

- Zunächst wird eine beschrieben auf diesen [Link](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) , der bei der Registrierung werden eine `UIWebViewDelegate` auf dem Webanzeige und Catch-und-sofort-Abbrechen einer Ändern des Standorts in Javascript fertig. 
- Zweites eine basiert auf dieser [Sitzung WWDC 2013](https://developer.apple.com/videos/play/wwdc2013/615)ein Ansatz welche ist als der ersten übersichtlicher und die wir für dieses Handbuch folgen sollen. Beachten Sie, dass dieser Ansatz nur bei iOS7 und höher funktioniert. 

Führen Sie die folgenden Schritte aus, für den iOS Stichprobe zu schließen:

1. Zunächst müssen Sie sicherstellen, dass Sie anhand des unsere [Erste Schritte-Lernprogramm](mobile-engagement-ios-get-started.md) Engagement für Mobile iOS SDK in Ihrer app Hybrid integrieren überschritten haben. Optional können Sie auch testen die Protokollierung aktivieren wie folgt, damit Sie sehen, wie wir sie aus der Webview auslösen die SDK Methoden. 
    
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
            [EngagementAgent setTestLogEnabled:YES];
           ....
        }

2. Vergewissern Sie sich nun, dass Ihre app Hybrid einen Bildschirm mit einer Webview enthält. Sie können sie zum Hinzufügen der `Main.storyboard` der app. 

3. Ordnen Sie diese Webview durch Klicken und ziehen das Webview aus der Ansicht Controller Szene in Ihrer **ViewController** die `ViewController.h` Bildschirm Ähnlichkeit bearbeiten direkt unterhalb der `@interface` Linie. 

4. Sobald Sie dies tun, wird ein Dialogfeld mit Bitte um einen Namen angezeigt. Geben Sie den Namen als **WebView**aus. Ihre `ViewController.h` Datei sollte wie folgt aussehen:

        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
        
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
        
        @end

5. Wir aktualisieren wird die `ViewController.m` Datei später jedoch zuerst erstelle wir die Bridge-Datei, die einen Wrapper über einige häufig verwendete Engagement für Mobile iOS-SDK Methoden erstellt. Erstellen einer neuen Kopf-Datei aufgerufen **EngagementJsExports.h** die verwendet die `JSExport` Verfahren beschrieben, die in der oben genannten [Sitzung](https://developer.apple.com/videos/play/wwdc2013/615) , um die systemeigene iOS Methoden verfügbar zu machen. 

        #import <Foundation/Foundation.h>
        #import <JavaScriptCore/JavascriptCore.h>
        
        @protocol EngagementJsExports <JSExport>
        
        + (void) sendEngagementEvent:(NSString*) name :(NSString*)extras;
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras;
        + (void) endEngagementJob:(NSString*) name;
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras;
        + (void) sendEngagementAppInfo:(NSString*) appInfo;
        
        @end

        @interface EngagementJs : NSObject <EngagementJsExports>

        @end

6. Als Nächstes werden wir des zweiten Teils der Bridge-Datei erstellen. Erstellen Sie eine Datei namens **EngagementJsExports.m** , die die Implementierung erstellen die tatsächliche Wrapper, indem Sie die Mobile Engagement iOS SDK Methoden enthalten sind. Beachten Sie auch, die wir analysieren sind die `extras` übergebene aus der Webview Javascript-Methode, wenn Sie die in einer `NSMutableDictionary` Objekt zu übergebenden mit Engagement SDK Methode Anrufe.  

        #import <UIKit/UIKit.h>
        #import "EngagementAgent.h"
        #import "EngagementJsExports.h"
        
        @implementation EngagementJs
        
        +(void) sendEngagementEvent:(NSString*)name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendEvent:name extras:extrasInput];
        }
        
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] startJob:name extras:extrasInput];
        }
        
        + (void) endEngagementJob:(NSString*) name {
           [[EngagementAgent shared] endJob:name];
        }
        
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendError:name extras:extrasInput];
        }
        
        + (void) sendEngagementAppInfo:(NSString*) appInfo {
           NSMutableDictionary* appInfoInput = [self ParseExtras:appInfo];
           [[EngagementAgent shared] sendAppInfo:appInfoInput];
        }
        
        + (NSMutableDictionary*) ParseExtras:(NSString*) input {
           NSData *data = [input dataUsingEncoding:NSUTF8StringEncoding];
           NSError* error = nil;
           NSMutableDictionary* extras = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
           
           return extras;
        }
        
        @end

5. Jetzt wieder zum der **ViewController.m** und aktualisieren ihn mit den folgenden Code: 
        
        #import <JavaScriptCore/JavaScriptCore.h>
        #import "ViewController.h"
        #import "EngagementJsExports.h"
        
        @interface ViewController ()
        
        @end
        
        @implementation ViewController
        
        - (void)viewDidLoad
        {
           self.webView.delegate = self;
           [super viewDidLoad];
           [self loadWebView];
        }
        
        - (void)loadWebView {
           NSBundle* mainBundle = [NSBundle mainBundle];
           NSURL* htmlPage = [mainBundle URLForResource:@"LocalPage" withExtension:@"html"];
           
           NSURLRequest* urlReq = [NSURLRequest requestWithURL:htmlPage];
           [self.webView loadRequest:urlReq];
        }
        
        - (void)webViewDidFinishLoad:(UIWebView*)wv
        {
           JSContext* context = [wv valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
           
           context[@"EngagementJs"] = [EngagementJs class];
        }
        
        - (void)webView:(UIWebView*)wv didFailLoadWithError:(NSError*)error
        {
           NSLog(@"Error for WEBVIEW: %@", [error description]);
        }
        
        - (void)didReceiveMemoryWarning {
           [super didReceiveMemoryWarning];
           // Dispose of any resources that can be recreated.
        }
        
        @end

6. Beachten Sie folgende Punkte zur Datei **ViewController.m** ein:

    - In der `loadWebView` Methode, wir sind Laden einer lokale HTML-Datei **LocalPage.html** , dessen Code wir weiter überprüfen wird aufgerufen. 
    - In der `webViewDidFinishLoad` Methode, wir Titelleisten der `JsContext` und unsere Wrapperklasse zuordnen. Dadurch wird aufrufen unsere Wrapper SDK Methoden verwenden den Ziehpunkt **EngagementJs** aus der WebView. 

7. Erstellen Sie eine Datei namens **LocalPage.html** mit den folgenden Code ein:

        <!doctype html>
        <html>
           <head>
               <style type='text/css'>
                   html { font-family:Helvetica; color:#222; }
                   h1 { color:steelblue; font-size:22px; margin-top:16px; }
                   h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
               </style>
               
               <script type="text/javascript">
               
               window.onerror = function(err)
               {
                   alert('window.onerror: ' + err);
               }
               
               function send(inputId)
               {
                   var input = document.getElementById(inputId);
                   if(input)
                   {
                       var value = input.value;
                       // Example of how extras info can be passed with the Engagement logs
                       var extras = '{"CustomerId":"MS290011"}';
                   }
                   
                   if(value && value.length > 0)
                   {
                       switch(inputId)
                       {
                           case "event":
                           EngagementJs.sendEngagementEvent(value, extras);
                           break;
                           
                           case "job":
                           EngagementJs.startEngagementJob(value, extras);
                           window.setTimeout(
                           function(){
                               EngagementJs.endEngagementJob(value);
                           }, 10000 );
                           break;
        
                           case "error":
                           EngagementJs.sendEngagementError(value, extras);
                           break;
                           
                           case "appInfo":
                           var appInfo = '{"customer_name":"' + value + '"}';
                           EngagementJs.sendEngagementAppInfo(appInfo);
                           break;
                       }
                   }
               }
               </script>
               
           </head>
           <body>
               <h1>Bridge Tester</h1>
               
               <div id='engagement'>
                   
                   <br/>
                   <h2>Event</h2>
                   <input type="text" id="event" size="35">
                   <button onclick="send('event')">Send</button>
                   
                   <br/>
                   <h2>Job</h2>
                   <input type="text" id="job" size="35">
                   <button onclick="send('job')">Send</button>
                   
                   <br/>
                   <h2>Error</h2>
                   <input type="text" id="error" size="35">
                   <button onclick="send('error')">Send</button
                   
                   <br/>
                   <h2>AppInfo</h2>
                   <input type="text" id="appInfo" size="35">
                   <button onclick="send('appInfo')">Send</button>
               
               </div>
           </body>
        </html>

8. Beachten Sie folgende Punkte zur HTML-Datei ein:

    -   Es enthält eine Reihe von Eingabefeldern angezeigt, in dem Sie Daten als Namen für Ihre Veranstaltung Auftrag, Fehler, AppInfo zu verwendende eingeben können. Beim Klicken auf die Schaltfläche neben dem Eintrag, wird für das Javascript-aufgerufen, die schließlich die Methoden aus der Datei Bridge Anruf an die Engagement für Mobile iOS SDK übergeben aufgerufen. 
    -   Wir werden kategorisieren auf einige statische zusätzliche Informationen auf Ereignisse, Aufträge und sogar Fehler, um zu veranschaulichen, wie Sie dies tun kann. Diese zusätzlichen Informationen wird als eine JSON Zeichenfolge, die gesendet, wenn Sie in Suchen der `EngagementJsExports.m` ablegen, analysiert und übergeben sowie Ereignisse, Aufträge, Fehler zu senden. 
    -   Ein Mobile Engagement Auftrag wird mit dem Namen 10 Sekunden ausführen, und fahren Sie Ihnen im Eingabefeld angegebenen deaktivieren ausgeschlossen. 
    -   Eine Mobile Engagement Appinfo oder Kategorie wird mit 'Kundenname' als statische Schlüssel und der Wert, den in der Eingabe als Wert für die Kategorie eingegebene übergeben. 
 
9. Führen Sie die app, und Sie werden Folgendes angezeigt. Jetzt einige Namen für ein Testereignis wie folgt aus, und klicken Sie neben dem Eintrag auf **Senden** . 

    ![][1]

10. Jetzt, wenn Sie auf der Registerkarte **Überwachen** Ihrer app und Aussehen unter **Ereignisse-Details >**zugreifen, sehen Sie dieses Ereignis zusammen mit den statischen app-Informationen anzeigen, die wir senden möchten. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
