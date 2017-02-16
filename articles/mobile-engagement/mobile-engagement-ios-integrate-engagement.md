<properties
    pageTitle="Azure Engagement für Mobile iOS SDK Integration | Microsoft Azure"
    description="Neuesten Updates und Verfahren für iOS SDK für Azure Mobile Engagement"
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
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-on-ios"></a>So integrieren Engagement unter iOS

> [AZURE.SELECTOR]
- [Windows-Dienst](mobile-engagement-windows-store-integrate-engagement.md)
- [Silverlight für Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Dieses Verfahren beschreibt die einfachste Methode zum Rahmen des Projekts Analytics und Funktionen in Ihrer Anwendung iOS Überwachung aktivieren.

Das Engagement SDK erfordert iOS6 + und Xcode 8: das Bereitstellungsziel Ihrer Anwendung muss mindestens iOS 6.

> [AZURE.NOTE]
> Wenn Sie wirklich XCode 7 abhängig sind, können Sie die [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh)verwenden. Es ist ein bekanntes Problem auf das Modul Reichweite dieser vorherigen Version während der Ausführung auf 10-iOS-Geräte finden Sie unter [Integration Modul Reichweite](mobile-engagement-ios-integrate-engagement-reach.md) für weitere Details. Wenn Sie festlegen, dass die v3.2.4 SDK verwenden, und klicken Sie dann nur Überspringen der `UserNotifications.framework` im nächsten Schritt importieren.

Sind folgende Schritte aus, um den Bericht zum Berechnen von Statistiken in Bezug auf Benutzer, Sitzungen, Aktivitäten, stürzt ab und Technicals alle erforderlichen Protokolle aktivieren. Bericht der Protokolle benötigt, um andere Statistik wie Ereignisse, Fehler und Aufträge berechnet vorgenommen werden manuell mithilfe der Engagement-API (finden Sie unter [Erweiterte Mobile Projektdauer tagging-API in Ihrem iOS-app verwenden](mobile-engagement-ios-use-engagement-api.md) , da diese Statistiken abhängig von der Anwendung sind.

##<a name="embed-the-engagement-sdk-into-your-ios-project"></a>Das SDK Engagement in Ihrem Projekt iOS einbetten

- Herunterladen der iOS SDK aus [hier](http://aka.ms/qk2rnj).

- Hinzufügen der Engagement SDK zum Projekt iOS: in Xcode, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **"Dateien hinzufügen..."** , und wählen Sie die `EngagementSDK` Ordner.

- Engagement erfordert zusätzliche Framework entwickelt: Klicken Sie im Projekt-Explorer, öffnen Sie Ihr im Projektbereich, und wählen Sie das richtige Ziel. Klicken Sie dann öffnen Sie die Registerkarte **"Phasen erstellen"** , und klicken Sie im Menü **"Link binäre mit Bibliotheken"** fügen Sie diese Framework hinzu:

    -   `UserNotifications.framework`-Legen Sie die Verknüpfung als`Optional`
    -   `AdSupport.framework`-Legen Sie die Verknüpfung als`Optional`
    -   `SystemConfiguration.framework`
    -   `CoreTelephony.framework`
    -   `CFNetwork.framework`
    -   `CoreLocation.framework`
    -   `libxml2.dylib`

> [AZURE.NOTE] Das AdSupport Framework kann entfernt werden. Engagement benötigt dieses Framework, um die IDFA zu sammeln. IDFA Auflistung kann jedoch deaktiviert werden \<Ios-Sdk-Engagement-Idfa\> zur Einhaltung der neuen Apple-Richtlinien Bezug auf diese ID.

##<a name="initialize-the-engagement-sdk"></a>Initialisierung des Projekts SDK

Sie müssen Ihre Stellvertretung Anwendung zu ändern:

-   Importieren Sie am oberen Rand der Implementierungsdatei des Engagement-Agents aus:

        [...]
        #import "EngagementAgent.h"

-   Initialisierung Engagement innerhalb der Methode "**ApplicationDidFinishLaunching:**'oder'**Anwendung: DidFinishLaunchingWithOptions:**':

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
          [...]
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
          [...]
        }

##<a name="basic-reporting"></a>Grundlegende Berichterstattung

### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Empfohlene Methode: überladen Ihrer `UIViewController` Klassen

Um den Bericht, der alle erforderlichen Engagement zum Berechnen von Benutzern, Sitzungen, Aktivitäten, stürzt ab und technische Statistiken Protokolle zu aktivieren, Sie können einfach Anzeigen aller Ihrer `UIViewController` untergeordnete Klassen erben von der `EngagementViewController` Klassen (derselben Regel für `UITableViewController`  - \> `EngagementTableViewController`).

**Ohne Engagement:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**Mit Projekt:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Alternative Methode: anrufen `startActivity()` manuell

Wenn Sie nicht oder nicht überladen möchten Ihre `UIViewController` Klassen, Sie können Ihre Aktivitäten stattdessen starten, indem Sie das Anrufen von `EngagementAgent`des Methoden direkt.

> [AZURE.IMPORTANT] IOS SDK automatisch Ruft die `endActivity()` Methode, wenn die Anwendung geschlossen wird. Daher ist es *dringend* empfohlen, rufen Sie die `startActivity` Methode, wenn die Aktivität des Benutzers ändern, und rufen Sie die Option *nie* die `endActivity` Methode, da die aktuelle Sitzung beendet werden diese Methode aufzurufen erzwingt werden.

##<a name="location-reporting"></a>Speicherort reporting

Apple Vertragsbedingungen zulassen Clientanwendungen über Speicherort für Statistik Zweck nur nachverfolgen nicht. Auf diese Weise wird empfohlen Speicherort Berichte nur aktivieren, wenn die Anwendung auch den Speicherort für ein weiterer Grund Überwachung verwenden.

Beginnend mit iOS 8, müssen Sie eine Beschreibung, wie Ihre app Speicherort Services verwendet, indem Sie eine Zeichenfolge für den Schlüssel [NSLocationWhenInUseUsageDescription] oder [NSLocationAlwaysUsageDescription] in Ihrer app "Info.plist" Datei bereitstellen. Wenn Sie den Speicherort des Berichts im Hintergrund mit Engagement möchten, fügen Sie die Taste NSLocationAlwaysUsageDescription hinzu. Fügen Sie in allen anderen Fällen Schlüssel NSLocationWhenInUseUsageDescription ein.

### <a name="lazy-area-location-reporting"></a>Verzögerte Bereich Speicherort reporting

Verzögerte Bereich Speicherort reporting ermöglicht, melden das Land, Region und Ort, Geräte zugeordnet ist. Diese Art der Speicherort Berichterstattung verwendet nur Netzwerkspeicherorte (basierend auf Zelle ID oder WIFI). Der Bereich Gerät wird höchstens einmal pro Sitzung gemeldet. Die GPS nie verwendet wird, und daher diese Art von Bericht Speicherort hat nur wenige (nicht zu "Nein") Einfluss auf die.

Gemeldete Bereiche werden verwendet, um die geografischen Statistik zu Benutzern, Sitzungen, Ereignisse und Fehler berechnet. Sie können auch als Kriterium in Reichweite massensendungen zu ermitteln verwendet werden. Der letzte bekannte Bereich für ein Gerät gemeldet kann dank der [Device-API]abgerufen werden.

Zum Aktivieren der aus-Bereich Speicherort Weitergabe, fügen Sie die folgende Zeile nach der Initialisierung des Engagement Agents aus:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Echtzeit Speicherort reporting

Echtzeit Speicherort reporting ermöglicht, melden die Breite und Länge, Geräte zugeordnet ist. Standardmäßig diese Art der Speicherort Berichterstattung verwendet nur Netzwerkspeicherorte (basierend auf Zelle ID oder WIFI), und die Berichterstattung ist nur verfügbar, wenn die Anwendung (z. B. während einer Sitzung) im Vordergrund ausgeführt wird.

Echtzeit Speicherorte werden *nicht* zum Berechnen von Statistiken verwendet. Verwendungszweck nur für die Verwendung von Echtzeit Geo-Zäune zulassen ist \<Reichweite-Zielgruppe-Geofencing\> Kriterium in Reichweite massensendungen zu ermitteln.

Zum Aktivieren der Echtzeit Speicherort Weitergabe, fügen Sie die folgende Zeile nach der Initialisierung des Engagement Agents aus:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>GPS Grundlage reporting

Standardmäßig verwendet Echtzeit Speicherort reporting nur Netzwerkspeicherorte basiert. Zum Aktivieren der Verwendung von GPS-basierten Orte (die weit präzisere sind) hinzufügen:

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Hintergrund reporting

Standardmäßig ist Echtzeit Speicherort reporting nur aktiv, wenn die Anwendung (z. B. während einer Sitzung) im Vordergrund ausgeführt wird. Klicken Sie zum Aktivieren der Berichterstattung auch im Hintergrund hinzufügen:

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [AZURE.NOTE] Wenn die Anwendung im Hintergrund, ausgeführt wird nur Netzwerk Speicherorte basierend werden gemeldet, auch wenn Sie die GPS aktiviert.

Implementierung dieser Funktion ruft [StartMonitoringSignificantLocationChanges] auf, wenn die Anwendung in den Hintergrund wechselt. Achten Sie darauf, dass die Anwendung in den Hintergrund automatisch starten wird, wenn Sie ein neuen Speicherort Ereignis eintritt.

##<a name="advanced-reporting"></a>Erweiterte Berichte

Optional, wenn Sie bestimmte Anwendungsereignisse, Fehlern und Einzelvorgänge melden möchten, müssen Sie die Engagement-API verwenden, über die Methoden der der `EngagementAgent` Class. Ein Objekt dieser Klasse kann abgerufen werden durch Aufrufen der `[EngagementAgent shared]` statische Methode.

Die Engagement-API ermöglicht es den gesamten Rahmen des Projekts erweiterten Funktionen verwenden und finden Sie unter beschrieben, wie die Engagement-API unter iOS verwenden (aber auch in der technischen Dokumentation von der `EngagementAgent` Klasse).

##<a name="disable-idfa-collection"></a>IDFA Websitesammlung deaktivieren

Standardmäßig wird Engagement der [IDFA] verwendet, um einen Benutzer eindeutig identifizieren. Aber wenn Sie nicht an anderer Stelle in der app Werbung verwenden, möglicherweise Sie von der App Store Überprüfungsprozess zurückgewiesen. IDFA Auflistung kann deaktiviert werden, indem Sie das Präprozessormakro hinzufügen `ENGAGEMENT_DISABLE_IDFA` in der Pch-Datei (oder in der `Build Settings` Ihrer Anwendung). Dadurch wird sichergestellt, dass es ist keine Verweise auf `ASIdentifierManager`, `advertisingIdentifier` oder `isAdvertisingTrackingEnabled` in der Anwendung erstellen.

Integration in der Datei **prefix.pch** :

    #define ENGAGEMENT_DISABLE_IDFA
    ...

Sie können überprüfen, dass die Sammlung IDFA ordnungsgemäß in der Anwendung deaktiviert ist, indem Sie die Protokolle der Engagement testen. Testen der Integration finden Sie unter\<Ios-Sdk-Engagement-Test-Idfa\> Dokumentation weitere Informationen.

##<a name="disable-log-reporting"></a>Log melden deaktivieren

### <a name="using-a-method-call"></a>Verwenden eines Anrufs Methode

Wenn Sie Engagement zum Senden von Protokollen beenden möchten, können Sie anrufen:

    [[EngagementAgent shared] setEnabled:NO];

Dieser Anruf ist beständiger: Es verwendet `NSUserDefaults` zum Speichern der Informationen.

Sie können Log erneut, indem Sie die gleiche Funktion mit reporting aktivieren `YES`.

### <a name="integration-in-your-settings-bundle"></a>Integration in Ihrem Paket Einstellungen

Rufen Sie diese Funktion, sondern können Sie auch diese Einstellung direkt in Ihrem vorhandenen integrieren `Settings.bundle` Datei. Die Zeichenfolge `engagement_agent_enabled` muss verwendet werden, als eines der Einstellung Bezeichner und sie müssen ein Schalter ist zugeordnet werden (`PSToggleSwitchSpecifier`).

Im folgenden Beispiel wird der `Settings.bundle` zeigt, wie es implementiert wird:

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[Gerät API]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
