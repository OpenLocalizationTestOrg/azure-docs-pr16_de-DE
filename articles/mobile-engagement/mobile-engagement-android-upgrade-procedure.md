<properties 
    pageTitle="Azure mobilen Engagement Android SDK-Integration" 
    description="Neuesten Updates und Verfahren für Android SDK für Azure Mobile Engagement"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="upgrade-procedures"></a>Upgrade-Verfahren

Wenn Sie bereits eine ältere Version unseres SDK in die Anwendung integriert haben, müssen Sie die folgenden Punkte beachten beim Upgrade des SDKS.

Möglicherweise müssen Sie mehrere Verfahren ausführen, wenn Sie mehrere Versionen von SDK verpasst haben. Wenn Sie migrieren 1.4.0 auf 1.6.0 Sie zuerst das Verfahren "von 1.4.0 zu 1.5.0 müssen" klicken Sie dann das Verfahren "von 1.5.0 zu 1.6.0" beispielsweise.

Welche Version Sie aktualisieren aus, müssen Sie ersetzen die `mobile-engagement-VERSION.jar` durch den neuen.

##<a name="from-420-to-421"></a>Aus 4.2.0 müssen auf 4.2.1

Dieser Schritt kann auf einer beliebigen Version des SDK tatsächlich ausgeführt werden, es ist eine Verbesserung der Sicherheit, wenn Sie Reichweite Aktivitäten integrieren.

Sie sollten jetzt hinzufügen `exported="false"` in allen Reichweite Aktivitäten.

Reichweite Aktivitäten sollte nun wie folgt aussehen, klicken Sie auf Ihrer `AndroidManifest.xml`:

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

##<a name="from-400-to-410"></a>Von 4.0.0 zu 4.1.0

Das SDK jetzt Ziehpunkt neue Berechtigungsmodell von Android M.

Wenn Sie Features Speicherort oder große Bild Benachrichtigungen Bitte lesen Sie [diesen Abschnitt](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

Zusätzlich zu den neuen Berechtigungsmodell unterstützen wir nun Konfigurieren von Speicherort Features zur Laufzeit.
Wir weiterhin mit den Manifesten Parametern für Standort kompatibel sind, aber es ist jetzt veraltet. Wenn Runtime-Konfiguration verwenden möchten, entfernen Sie in den folgenden Abschnitten aus Ihrer ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

und Lesen Sie [aktualisierte Prozedur](mobile-engagement-android-integrate-engagement.md#location-reporting) , um stattdessen Runtime-Konfiguration verwenden.

##<a name="from-300-to-400"></a>Von 3.0.0 zu 4.0.0

### <a name="native-push"></a>Systemeigene Pushbenachrichtigungen

Systemeigener Pushbenachrichtigungen (GCM/ADM) wird jetzt auch für im app-Benachrichtigungen verwendet, damit Sie die Anmeldeinformationen systemeigenen Pushbenachrichtigungen für alle Arten von Pushbenachrichtigungen für eine Marketingkampagne konfigurieren müssen.

[Wenn dies nicht bereits Bitte gehen.](mobile-engagement-android-integrate-engagement-reach.md#native-push)

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Reichweite Integration in geändert wurde ``AndroidManifest.xml``.

Ersetzen Sie dies:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

Durch

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>
    <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
      <intent-filter>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
      </intent-filter>
    </receiver>

Es ist möglicherweise ein Bildschirm laden jetzt beim Klicken auf eine Ankündigung (mit Text/Web Content) oder einer Umfrage.
Sie müssen dies für diese massensendungen zu ermitteln, für die Arbeit in 4.0.0 hinzufügen:

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Ressourcen

Das neue einbetten `res/layout/engagement_loading.xml` Datei in Ihrem Projekt.

##<a name="from-240-to-300"></a>Aus 2.4.0 zu 3.0.0

Die folgenden beschrieben, wie Sie eine SDK Integration der Dienstleistung Capptain von Capptain SAS in eine app Azure Mobile Engagement betrieben migrieren. Wenn Sie von einer früheren Version migrieren, finden Sie in der Capptain-Website, um zuerst zur 2.4.0 zu migrieren, und wenden Sie dann das folgende Verfahren.

>[AZURE.IMPORTANT] Capptain und Mobile Engagement sind nicht dieselben Dienste und der nachfolgend beschriebenen nur hervorgehoben, wie Sie die app Client migrieren. Migrieren das SDK in der app wird nicht Ihre Daten migrieren aus den Capptain-Servern auf die Mobile Engagement Server.

### <a name="jar-file"></a>JAR-Datei

Ersetzen Sie `capptain.jar` von `mobile-engagement-VERSION.jar` in Ihrer `libs` Ordner.

### <a name="resource-files"></a>Ressourcendateien

Jeder Ressourcendatei, die wir bereitgestellt (mit dem Präfix `capptain_`) durch die neuen Dateien ersetzt werden muss (mit dem Präfix `engagement_`).

Wenn Sie diese Dateien angepasst haben, müssen Sie Ihre Anpassung auf der neuen Dateien, die **auch wurden alle Bezeichner in der Ressourcendateien umbenannt**erneut anzuwenden.

### <a name="application-id"></a>ID der Anwendung

Jetzt wird Engagement eine Verbindungszeichenfolge so konfigurieren Sie die SDK Bezeichner wie der Anwendungsbezeichner verwendet.

Sie verwenden müssen `EngagementAgent.init` Methode in Ihrer Aktivität Startprogramm für das wie folgt:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Die Verbindungszeichenfolge für eine Anwendung wird Azure-Portal angezeigt.

Entfernen Sie alle eines Anrufs an die `CapptainAgent.configure` als `EngagementAgent.init` ersetzt diese Methode.

Die `appId` kann nicht mehr mit konfiguriert werden `AndroidManifest.xml`.

Entfernen Sie in diesem Abschnitt aus Ihrer `AndroidManifest.xml` Wenn Sie vorhanden ist:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Java-API

Jeder Anruf an eine beliebige Java-Klasse unseres SDK muss umbenannt werden; beispielsweise `CapptainAgent.getInstance(this)` muss umbenannt werden `EngagementAgent.getInstance(this)`, `extends CapptainActivity` muss umbenannt werden `extends EngagementActivity` usw....

Wenn Sie mit der standardmäßigen Agent Preference Dateien integriert wurden, ist jetzt der Standarddateinamen `engagement.agent` und der Schlüssel ist `engagement:agent`.

Beim Erstellen von Web Ankündigungen ist jetzt der Javascript-Binder `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Viele Änderungen vorhanden ist, der Dienst ist nicht mehr freigegeben und viele der Empfänger nicht exportiert werden nicht mehr.

Die Dienstdeklaration ist jetzt einfacher. Hinzufügen und Entfernen der beabsichtigte filtern und alle Metadaten darin enthaltenen `exportable=false`.

Außerdem verlangt alles wird umbenannt, um Engagement zu verwenden.

Es sieht nun folgendermaßen aus:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Wenn Sie Testprotokolle aktivieren möchten, wird die Metadaten jetzt in der Anwendung Tag verschoben und umbenannt wurde:

            <application>
            
              <meta-data android:name="engagement:log:test" android:value="true" />
            
              <service/>
            
            </application>

Wurden alle anderen Metadaten nur umbenannt, hier finden Sie die vollständige Liste (der Kurs Umbenennen nur diejenigen, die Sie verwenden):

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>
            
            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Google Play und SmartAd nachverfolgen wurde SDK müssen Sie nur diese ohne Ersatz zu entfernen von entfernt:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

Die Reichweite Aktivitäten werden jetzt wie folgt deklariert:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            
Wenn Sie benutzerdefinierte Reichweite Aktivitäten haben, müssen Sie nur die beabsichtigten Aktionen entsprechend entweder ändern `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` oder `com.microsoft.azure.engagement.reach.intent.action.POLL`.

Die übertragenen Empfänger wurden umbenannt sowie wir fügen Sie nun `exported=false`. Hier ist die vollständige Liste der Empfänger mit der neuen Spezifikation (der Kurs Umbenennen nur diejenigen, die Sie verwenden):

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Nachverfolgen der Empfänger wurde entfernt, sodass Sie diesen Abschnitt entfernen müssen:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Notiz, die in die Deklaration von der Implementierung des Empfängers übertragenen **EngagementMessageReceiver** geändert hat die `AndroidManifest.xml`. Dies ist, da die API zum Senden und Entfernen von beliebigen XMPP Nachrichten von beliebigen XMPP Einheiten und die API zum Senden und Empfangen von Nachrichten zwischen Geräten entfernt wurden. Daher müssen Sie auch den folgenden Rückruf aus Ihrer **EngagementMessageReceiver** Implementierung zu löschen:

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

und

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

Löschen Sie dann alle Anruf auf **EngagementAgent** für ein:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

und

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard

Proguard Konfiguration kann durch mitgelieferten beeinträchtigt werden, wie die Regeln jetzt suchen:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }
            
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }
 
