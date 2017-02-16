<properties 
    pageTitle="Bridge Android WebView mit einer systemeigenen Mobile Engagement Android SDK" 
    description="So erstellen Sie eine Verbindung zwischen WebView Javascript und systemeigenen Mobile Engagement Android SDK ausgeführt werden"      
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>Bridge Android WebView mit einer systemeigenen Mobile Engagement Android SDK

> [AZURE.SELECTOR]
- [Android-Brücke](mobile-engagement-bridge-webview-native-android.md)
- [iOS Bridge](mobile-engagement-bridge-webview-native-ios.md)

Einige mobile apps dienen als app Hybrid, in dem die app ähneln mithilfe der systemeigenen Android Entwicklung entwickelt, aber einige oder sogar alle Bildschirme innerhalb einer Android WebView gerendert werden. Sie können weiterhin Mobile Engagement Android SDK in solche apps nutzen und dieses Lernprogramms beschrieben, wie Sie wechseln Sie zu diesem Thema. Der folgende Beispielcode basiert auf dem Android-Dokumentation [hier](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). Es wird beschrieben, wie dieser Ansatz dokumentierten für Mobile Engagement Android SDKs häufig verwendeten Methoden so implementieren, dass eine Webview aus einem Hybriden app Anfragen Ereignisse überwachen, Aufträge, Fehlern bei der app-Info auch initiieren kann, während sie über unsere Android SDK Rohrleitungsplan verwendet werden kann. 

1. Zunächst müssen Sie sicherstellen, dass Sie anhand des unsere [Erste Schritte-Lernprogramm](mobile-engagement-android-get-started.md) Mobile Engagement Android SDK in Ihrer app Hybrid integrieren überschritten haben. Nachdem Sie in diesem Fall wird der `OnCreate` Methode wird wie folgt aussehen.  
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }

2. Vergewissern Sie sich nun, dass Ihre app Hybrid einen Bildschirm mit einer WebView enthält. Kann ich der Code dafür ähnelt, wo wir eine lokale HTML geladen sind, folgende Datei **Sample.html** wird der Webview in der `onCreate` Methode des Bildschirms. 

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }

        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }

3. Jetzt erstellen eine Brücke Datei mit dem Namen **WebAppInterface** der erstellt einen Wrapper über einige häufig verwendeten Mobile Engagement Android SDK Methoden, mit der `@JavascriptInterface` Ansatz in der [Dokumentation Android](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript)beschrieben:

        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
        
        import com.microsoft.azure.engagement.EngagementAgent;
        
        import org.json.JSONArray;
        import org.json.JSONObject;
        
        public class WebAppInterface {
            Context mContext;
        
            /** Instantiate the interface and set the context */
            WebAppInterface(Context c) {
                mContext = c;
            }
        
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
        
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
        
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
        
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  

4. Wenn wir die oben angegebenen Bridge Datei erstellt haben, müssen wir stellen Sie sicher, dass es unsere Webview zugeordnet ist. Dazu müssen zum Bearbeiten Ihrer `SetWebview` Methode, sodass die It wie folgt aussieht:

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }

5. In der obigen Codeausschnitt wir genannt `addJavascriptInterface` unsere Klasse Bridge unsere Webview zuordnen und auch erstellt einen Ziehpunkt **EngagementJs** zum Aufrufen der Methoden aus der Datei Bridge bezeichnet. 

6. Jetzt erstellen Sie die folgende Datei mit dem Namen **Sample.html** in Ihrem Projekt in einem Ordner namens **Posten** wird der in der Webview geladen und wo wir rufen die Methoden aus der Bridge-Datei ein.

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
                      log('window.onerror: ' + err);
                    }
        
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with the Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
        
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
        
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
        
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
        
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
        
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
        
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
        
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>

8. Beachten Sie folgende Punkte zur HTML-Datei ein:

    -   Es enthält eine Reihe von Eingabefeldern angezeigt, in dem Sie Daten als Namen für Ihre Veranstaltung Auftrag, Fehler, AppInfo zu verwendende eingeben können. Beim Klicken auf die Schaltfläche neben dem Eintrag, wird für das Javascript-aufgerufen, die schließlich die Methoden aus der Datei Bridge Anruf an die Mobile Engagement Android SDK übergeben aufgerufen. 
    -   Wir werden kategorisieren auf einige statische zusätzliche Informationen auf Ereignisse, Aufträge und sogar Fehler, um zu veranschaulichen, wie Sie dies tun kann. Diese zusätzlichen Informationen wird als eine JSON Zeichenfolge, die gesendet, wenn Sie in Suchen der `WebAppInterface` ablegen, analysiert und setzen Sie in einem Android-Gerät `Bundle` und wird zusammen mit senden Ereignisse, Aufträge, Fehlern übergeben. 
    -   Ein Mobile Engagement Auftrag wird mit dem Namen 10 Sekunden ausführen, und fahren Sie Ihnen im Eingabefeld angegebenen deaktivieren ausgeschlossen. 
    -   Eine Mobile Engagement Appinfo oder Kategorie wird mit 'Kundenname' als statische Schlüssel und der Wert, den in der Eingabe als Wert für die Kategorie eingegebene übergeben. 
 
9. Führen Sie die app, und Sie werden Folgendes angezeigt. Nun einige Namen für ein Testereignis wie folgt aus, und klicken Sie auf **Senden** , darunter. 

    ![][1]

10. Jetzt, wenn Sie auf der Registerkarte **Überwachen** Ihrer app und Aussehen unter **Ereignisse-Details >**zugreifen, sehen Sie dieses Ereignis zusammen mit den statischen app-Informationen anzeigen, die wir senden möchten. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
