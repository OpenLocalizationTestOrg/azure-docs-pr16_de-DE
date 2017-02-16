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

#<a name="how-to-integrate-engagement-reach-on-android"></a>Wie Engagement Reichweite auf Android-Gerät integriert werden soll.

> [AZURE.IMPORTANT] Führen Sie die Integration Verfahren wie Android Dokument, bevor Sie dieses Handbuch in der so Engagement integriert werden soll.

##<a name="standard-integration"></a>Standard-integration

Das erreichen SDK erfordert die **Android-Support-Bibliothek (v4)**.

Die schnellste Möglichkeit zum Hinzufügen der Bibliothek zu einem Projekt in **"Ellipse"** ist `Right click on your project -> Android Tools -> Add Support Library...`.

Wenn Sie "Ellipse" nicht verwenden, können Sie die Anweisungen lesen [können].

Kopieren Sie Reichweite Ressourcendateien aus dem SDK im Projekt:

-   Kopieren Sie die Dateien aus der `res/layout` Ordner übermittelt, mit dem SDK in die `res/layout` Ordner der Anwendung.
-   Kopieren Sie die Dateien aus der `res/drawable` Ordner übermittelt, mit dem SDK in die `res/drawable` Ordner der Anwendung.

Bearbeiten der `AndroidManifest.xml` Datei:

-   Im folgenden Abschnitt hinzufügen (zwischen der `<application>` und `</application>` Tags):

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
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
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

-   Sie benötigen diese Berechtigung System Benachrichtigungen wiedergeben, die beim Start nicht geklickt wurde (Andernfalls bleiben auf dem Datenträger, aber nicht mehr angezeigt werden, müssen Sie wirklich hierzu gehören).

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

-   Geben Sie ein Symbol für Benachrichtigungen (beide in-app und System diejenigen) verwendet wird, durch Kopieren und bearbeiten im folgenden Abschnitt (zwischen der `<application>` und `</application>` Tags):

            <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [AZURE.IMPORTANT] In diesem Abschnitt ist **obligatorisch** , wenn Sie, zur Verwendung von System Benachrichtigungen beim Erstellen der Reichweite massensendungen zu ermitteln beabsichtigen. Android verhindert, dass System Benachrichtigungen ohne Symbole angezeigt werden. Daher fehlt in diesem Abschnitt, werden Ihre Endbenutzer kann nicht empfangen.

-   Wenn Sie mit System-Benachrichtigungen, die einen Überblick über massensendungen zu ermitteln erstellen, müssen Sie die folgenden Berechtigungen hinzufügen (nach der `</application>` Kategorie) Wenn:

            <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
            <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

  -   Android M und wenn die Anwendung Android-API Stufe 23 oder höher, vorgesehen ``WRITE_EXTERNAL_STORAGE`` Berechtigung Benutzer Genehmigung erforderlich. Lesen Sie [diesen Abschnitt](mobile-engagement-android-integrate-engagement.md#android-m-permissions)aus.

-   Für Benachrichtigungen System können Sie auch in der Zeit für eine Marketingkampagne angeben, wenn das Gerät anrufen und/oder Vibrationsalarm sollte. Damit es funktioniert, müssen Sie sicherstellen, dass Sie die folgende Berechtigung deklariert (nach der `</application>` Kategorie):

            <uses-permission android:name="android.permission.VIBRATE" />

    Ohne diese Berechtigung verhindert Android System Benachrichtigungen, nicht mehr angezeigt wird, sofern Sie anrufen oder die Option Vibrate im erreicht haben für eine Marketingkampagne Manager aktiviert haben.

-   Wenn Sie die Anwendung unter Verwendung **ProGuard** und Fehler im Zusammenhang mit der Android-Support-Bibliothek oder die Engagement Jar, fügen Sie folgende Zeilen zu Ihrem `proguard.cfg` Datei:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

## <a name="native-push"></a>Systemeigene Pushbenachrichtigungen

Jetzt, da Sie Reichweite Modul konfiguriert haben, müssen Sie konfigurieren systemeigenen Pushbenachrichtigungen werden sollen, die massensendungen zu ermitteln, auf dem Gerät zu erhalten.

Wir Supportdienste zwei auf Android-Gerät:

  - Google Play Geräte: Verwenden Sie anhand des Leitfadens [zum Integrieren GCM mit Engagement Leitfaden](mobile-engagement-android-gcm-integrate.md) [Google Cloud Messaging] .
  - Amazon-Geräte: Verwenden Sie anhand des Leitfadens [zum Integrieren ADM mit Engagement Leitfaden](mobile-engagement-android-adm-integrate.md) [Amazon Gerät Messaging] .

Wenn Sie sowohl Amazon und Google Play Geräte, deren möglich, dass alles in 1 AndroidManifest.xml/APK für die Entwicklung adressieren möchten. Aber wenn Amazon gesendet werden, können sie Ihrer Anwendung ablehnen, findet GCM Code.

In diesem Fall sollten Sie mehrere APKs verwenden.

**Die Anwendung kann nun empfangen und Anzeigen von Reichweite massensendungen zu ermitteln!**

##<a name="how-to-handle-data-push"></a>Umgang mit Daten Pushbenachrichtigungen

### <a name="integration"></a>Integration

Wenn Sie die Anwendung Reichweite Daten schiebt empfangen werden soll, müssen Sie eine untergeordnete Klasse von erstellen `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` und ihn in Bezug auf die `AndroidManifest.xml` Datei (zwischen der `<application>` und/oder `</application>` Tags):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Dann können Sie außer Kraft setzen die `onDataPushStringReceived` und `onDataPushBase64Received` Rückrufe. Hier ist ein Beispiel:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Kategorie

Der Kategorieparameter ist optional, wenn Sie eine Daten Pushbenachrichtigungen für eine Marketingkampagne erstellen und ermöglicht es, dass Sie zum Filtern von Daten verschiebt. Dies ist sinnvoll, wenn mehrere übertragenen Empfänger Behandeln von verschiedenen Arten von Daten schiebt, oder wenn Sie verschiedene Arten übertragen möchten von `Base64` Daten und zum ihren Typ zu identifizieren, vor dem sie analysieren möchten.

### <a name="callbacks-return-parameter"></a>Rückrufe Absenderadresse parameter

Es folgen einige Richtlinien, um die Parameter der Rückgabetyp ordnungsgemäß zu behandeln `onDataPushStringReceived` und `onDataPushBase64Received`:

-   Ein übertragenen Empfänger sollte zurückgeben `null` in der Rückruf, wenn er nicht weiß, wie eine Pushbenachrichtigungen Daten behandelt. Sie sollten die Kategorie verwenden, um festzustellen, ob Ihr übertragenen Empfänger der Daten Pushbenachrichtigungen behandeln soll.
-   Sollte eine der übertragenen Empfänger zurückgeben `true` in der Rückruf, wenn sie die Daten Pushbenachrichtigungen akzeptiert.
-   Sollte eine der übertragenen Empfänger zurückgeben `false` in der Rückruf, wenn es die Pushbenachrichtigungen Daten erkennt, aber es aus irgendeinem Grund verwirft. Beispielsweise zurückgeben `false` Wenn die empfangenen Daten sind ungültig.
-   Wenn Sie einen Empfänger gibt übertragen `true` während er sich ein anderes eine gibt `false` für die gleichen Daten Pushbenachrichtigungen das Verhalten nicht definiert ist, sollten Sie nie ausführen, die.

Der Rückgabetyp wird nur für die Reichweite Statistiken verwendet:

-   `Replied`wird erhöht, wenn eine von Empfängern mit der übertragenen entweder zurückgegeben `true` oder `false`.
-   `Actioned`wird nur dann, wenn eine von Empfängern mit der übertragenen zurückgegeben erhöht `true`.

##<a name="how-to-customize-campaigns"></a>So passen Sie massensendungen zu ermitteln

Zum Anpassen von massensendungen zu ermitteln, können Sie in das erreichen SDK bereitgestellten Layouts ändern.

Bleiben alle Bezeichner, die in den Layouts verwendet, und behalten die Typen von Ansichten, die eine ID, vor allem für Text und Bild Ansichten verwenden. In einigen Ansichten dienen nur zum Ausblenden oder Anzeigen von Bereichen, damit deren Typ geändert werden kann. Überprüfen Sie den Quellcode, wenn Sie den Typ einer Ansicht in der bereitgestellten Layouts ändern möchten.

### <a name="notifications"></a>Benachrichtigungen

Es gibt zwei Arten von Benachrichtigungen: System und in der app-Benachrichtigungen die verschiedenen Layout-Dateien zu verwenden.

#### <a name="system-notifications"></a>System-Benachrichtigungen

Zum Anpassen von Benachrichtigungen System müssen Sie die **Kategorien**verwenden. Sie können die [Kategorien](#categories)springen.

#### <a name="in-app-notifications"></a>In der app-Benachrichtigungen

Standardmäßig wird eine Benachrichtigung in der app einer Ansicht, die Dank der Android-Methode der aktuellen Aktivität-Benutzeroberfläche dynamisch hinzugefügt wird `addContentView()`. Dies ist eine Benachrichtigung Überlagerung bezeichnet. Benachrichtigung überlagert eignen sich hervorragend zum eine schnelle Integration, da sie keine erfordern, dass Sie ein beliebiges Layout in der Anwendung zu ändern.

Zum Ändern des Aussehens Ihrer Benachrichtigung überlagert können Sie einfach die Datei ändern `engagement_notification_area.xml` an Ihre Bedürfnisse.

> [AZURE.NOTE] Die Datei `engagement_notification_overlay.xml` ist das Schema, mit dem eine Überlagerung Benachrichtigung erstellen, sie die Datei enthält `engagement_notification_area.xml`. Sie können es (beispielsweise im Benachrichtigungsbereich innerhalb der Überlagerung Positionierung) Ihren Anforderungen entsprechend anpassen.

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Benachrichtigung Layout als Teil eines Layouts für die Aktivität einbeziehen

Überlagert können eignen sich hervorragend für eine schnelle Integration aber schwierig sein, oder auf Effekte in Ausnahmefällen. Auf einer Aktivität Ebene, sodass sie mühelos auf Effekte für spezielle Aktivitäten zu verhindern, dass kann das System Überlagerung angepasst werden.

Sie können entscheiden, unsere Layout Benachrichtigung in Ihrer vorhandenen Layouts Dank der Anweisung Android **einschließen** aufnehmen möchten. Im folgenden ist ein Beispiel einer geänderten `ListActivity` Layout mit nur einem `ListView`.

**Bevor Sie Engagement Integration:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Nach dem Engagement Integration:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

In diesem Beispiel haben wir einen übergeordneten Container hinzugefügt, da das ursprüngliche Layout eine Listenansicht als Element der höchsten Ebene verwendet. Wir auch hinzugefügt `android:layout_weight="1"` werden sollen, hinzufügen eine Ansicht unter einer Listenansicht mit konfiguriert `android:layout_height="fill_parent"`.

Das Engagement erreichen SDK erkennt automatisch an, dass das Layout Benachrichtigung sich auf diese Aktivität befindet und eine Überlagerung für diese Aktivität nicht hinzugefügt werden.

> [AZURE.TIP] Wenn Sie eine ListActivity in Ihrer Anwendung verwenden, wird eine sichtbare Reichweite Überlagerung Sie verhindern, dass reagieren auf Elemente in der Listenansicht nicht mehr geklickt. Dies ist ein bekanntes Problem. Um dieses Problem zu umgehen sollten Sie das Layout der Benachrichtigung in Ihr eigenes Layout Aktivität Liste wie im vorherigen Beispiel einbetten.

##### <a name="disabling-application-notification-per-activity"></a>Deaktivieren der Anwendung Benachrichtigung pro Aktivität

Wenn Sie nicht die Überlagerung an Ihre Aktivität hinzugefügt werden soll, und wenn Sie das Layout der Benachrichtigung in Ihr eigenes Layout nicht enthalten, können Sie die Überlagerung für diese Aktivität in Deaktivieren der `AndroidManifest.xml` durch Hinzufügen einer `meta-data` Abschnitt wie im folgenden Beispiel:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="a-namecategoriesa-categories"></a><a name="categories"></a>Kategorien

Wenn Sie die bereitgestellten Layouts ändern möchten, ändern Sie das Aussehen Ihrer alle Benachrichtigungen aus. Mithilfe von Kategorien können Sie verschiedene zielgerichteten Looks (oftmals Verhaltensweisen) für Benachrichtigungen festlegen. Eine Kategorie kann angegeben werden, wenn Sie eine Zeit für eine Marketingkampagne erstellen. Lassen Sie beachten Sie, dass Kategorien ermöglichen auch Ankündigungen und Umfragen, anpassen, die später in diesem Artikel beschrieben ist.

Um eine Kategorie Ereignishandler für Ihre Benachrichtigungen zu registrieren, müssen Sie einen Anruf hinzufügen, bei der Initialisierung der Anwendungs ist.

> [AZURE.IMPORTANT] Lesen Sie bitte die Warnung über das Attribut Android: Prozess \<Android-Sdk-Engagement-Process\> im So integrieren Engagement Android Thema, bevor Sie fortfahren.

Im folgende Beispiel wird davon ausgegangen, Sie die vorherige Warnung bestätigt, und verwenden eine untergeordnete Klasse von `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

Die `MyNotifier` Objekt ist die Implementierung von der Benachrichtigung Kategorie Ereignishandler. Es ist entweder eine Implementierung von der `EngagementNotifier` Benutzeroberfläche oder eine Sub-Klasse der Standard-Implementierung: `EngagementDefaultNotifier`.

Beachten Sie, dass der gleiche Notifier kann mehrere Kategorien helfen, Sie können diese registrieren, wie folgt:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

Um die standardmäßige Kategorie Implementierung zu ersetzen, können Sie die Implementierung wie im folgenden Beispiel zu registrieren:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

Die aktuelle Kategorie in einem Ereignishandler verwendet wird als Parameter in den meisten Methoden außer Kraft, in setzen übergeben `EngagementDefaultNotifier`.

Es wird entweder als übergeben einer `String` Parameter oder indirekt in einer `EngagementReachContent` Objekt mit einer `getCategory()` Methode.

Sie können die meisten den Erstellungsprozess Benachrichtigung ändern, indem Methoden auf Neu `EngagementDefaultNotifier`, für die erweiterte Anpassung gerne bei technischen Unterlagen und den Quellcode ansehen.

##### <a name="in-app-notifications"></a>In der app-Benachrichtigungen

Wenn Sie alternative Layouts für eine bestimmte Kategorie verwenden möchten, können Sie diese wie im folgenden Beispiel implementieren:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Beispiel für `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Wie Sie sehen können, unterscheidet sich der Überlagerung Ansichtsbezeichner als Standardspeicherort zu. Es ist wichtig, dass jede Layout einen eindeutigen Bezeichner für überlagert verwenden.

**Beispiel für `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Wie Sie sehen können, unterscheidet sich der Benachrichtigung Bereich Ansichtsbezeichner als Standardspeicherort zu. Es ist wichtig, dass jede Layout einen eindeutigen Bezeichner für Benachrichtigung Bereiche verwendet.

Dieses einfache Beispiel der Kategorie macht Anwendung (oder in der app) Benachrichtigungen am oberen Rand des Bildschirms angezeigt. Wir nicht der standard-IDs verwendet, die im Infobereich selbst ändern.

Wenn Sie dies ändern möchten, müssen Sie Zellwerte der `EngagementDefaultNotifier.prepareInAppArea` Methode. Es wird empfohlen, suchen Sie in der technischen Dokumentation und den Quellcode des `EngagementNotifier` und `EngagementDefaultNotifier` Wenn dies Ebene der erweiterten Anpassung soll.

##### <a name="system-notifications"></a>System-Benachrichtigungen

Durch verlängern `EngagementDefaultNotifier`, Sie können außer Kraft setzen `onNotificationPrepared` so ändern Sie die Benachrichtigung, die durch die standardmäßige Implementierung vorbereitet wurde.

Beispiel:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

In diesem Beispiel wird eine Benachrichtigung des Systems für einen Inhaltstyp angezeigt wird, als ein laufendes Ereignis, wenn die Kategorie "laufende" verwendet wird.

Wenn Sie erstellen möchten die `Notification` Objekt neu erstellen, können Sie zurückkehren `false` die Methode und Anruf `notify` sich auf die `NotificationManager`. In diesem Fall ist es wichtig, dass Sie behalten eine `contentIntent`, eine `deleteIntent` und die Benachrichtigung Identifier, mit dem `EngagementReachReceiver`.

Hier ist ein Beispiel für eine solche Implementierung richtig aus:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice the call to get the right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Benachrichtigung nur Ankündigungen

Die Verwaltung von dem Klicken auf eine Benachrichtigung nur Ankündigung kann, indem Sie überschreiben angepasst werden `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` so ändern Sie die vorbereiteter `Intent`. Verwenden diese Methode, können Sie die Kennzeichen einfach zu optimieren.

Beispiel zum Hinzufügen der `SINGLE_TOP` kennzeichnen:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

Für Benutzer älterer Versionen Engagement Bitte beachten Sie, dass das System Benachrichtigungen ohne Aktion URL jetzt die Anwendung wird gestartet, wenn es im Hintergrund, wurde, damit diese Methode mit einer Ankündigung ohne Aktions-URL aufgerufen werden kann. Sie sollten, die beim Anpassen der Absicht.

Sie können auch implementieren `EngagementNotifier.executeNotifAnnouncementAction` von Grund auf neu.

##### <a name="notification-life-cycle"></a>Benachrichtigung Lebenszyklus

Wenn Sie die Standardkategorie verwenden, werden einige Methoden Lebenszyklus auf bezeichnet die `EngagementReachInteractiveContent` Objekt zum Erstellen von Statistiken und aktualisieren Sie den Zustand für eine Marketingkampagne:

-   Wenn die Benachrichtigung in der Anwendung oder setzen in der Statusleiste angezeigt wird, ist die `displayNotification` wird aufgerufen (die Statistiken Berichte) nach `EngagementReachAgent` Wenn `handleNotification` gibt `true`.
-   Wenn Sie die Benachrichtigung geschlossen wird, die `exitNotification` wird aufgerufen, Statistik gemeldet und nächste massensendungen zu ermitteln können nun bearbeitet werden.
-   Wenn Sie die Benachrichtigung klicken, `actionNotification` wird aufgerufen, Statistik gemeldet wird und die zugehörigen Intent wird gestartet.

Wenn die Implementierung von `EngagementNotifier` umgangen wird das Standardverhalten, müssen Sie diese Lebenszyklusmethoden aufrufen, indem Sie sich selbst. Die folgenden Beispiele verdeutlichen Fällen, wo das Standardverhalten umgangen wird:

-   Erweitern Sie nicht `EngagementDefaultNotifier`, z. B. implementiert Sie Kategorie Behandlung von Grund auf neu.
-   Für System-Benachrichtigungen überschrieben hat die `onNotificationPrepared` und Ihnen geänderte `contentIntent` oder `deleteIntent` in der `Notification` Objekt.
-   Für innerhalb der app-Benachrichtigungen überschrieben hat `prepareInAppArea`, müssen Sie mindestens zuordnen `actionNotification` einem Ihrer U.I gesteuert.

> [AZURE.NOTE] Wenn `handleNotification` löst eine Ausnahme aus, den Inhalt wird gelöscht und `dropContent` aufgerufen wird. Dies in Statistik angezeigt, und nächste massensendungen zu ermitteln können nun bearbeitet werden.

### <a name="announcements-and-polls"></a>' Ankündigungen ' und Umfragen

#### <a name="layouts"></a>Layouts

Sie können ändern, die `engagement_text_announcement.xml`, `engagement_web_announcement.xml` und `engagement_poll.xml` Dateien können Sie Text Ankündigungen, Web Ankündigungen und Umfragen anpassen.

Diese Dateien freigeben zwei allgemeine Layouts für den Titel und der Schaltflächenbereich. Das Layout für den Titel ist `engagement_content_title.xml` und die Diamantenindustrie zeichenbare Datei für den Hintergrund verwendet. Das Layout für die Aktion "und" Beenden "ist `engagement_button_bar.xml` und die Diamantenindustrie zeichenbare Datei für den Hintergrund verwendet.

In einer Umfrage, das Layout Frage und deren Auswahlmöglichkeiten sind dynamisch vergrößert mehrmals mithilfe der `engagement_question.xml` Layout-Datei für die Fragen und die `engagement_choice.xml` der Auswahlmöglichkeiten für Datei.

#### <a name="categories"></a>Kategorien

##### <a name="alternate-layouts"></a>Alternative layouts

Wie Benachrichtigungen kann die für eine Marketingkampagne Kategorie alternative Layouts für Ihre Ankündigungen und Umfragen können verwendet werden.

Angenommen, Sie zum Erstellen einer Kategorie für eine Ankündigung Text können Sie erweitern `EngagementTextAnnouncementActivity` und darauf verweisen die `AndroidManifest.xml` Datei:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Beachten Sie, dass die Kategorie im intent Filter verwendet wird, um die Differenz mit der standardmäßigen Ankündigung Aktivität zu gestalten.

Das erreichen SDK verwendet das intent System, um die richtige Aktivität für eine bestimmte Kategorie zu beheben, und es liegt wieder auf die Standardkategorie, wenn Fehler bei der die Auflösung.

Müssen Sie implementieren `MyCustomTextAnnouncementActivity`, wenn Sie einfach ändern des Layouts (unter Beibehaltung der gleichen Ansichtsbezeichner) möchten, einfach, müssen Sie die Klasse wie im folgenden Beispiel definieren:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class to use R.layout.my_text_announcement
              }
            }

Um die Standardkategorie der Ankündigungen von Text zu ersetzen, ersetzen Sie einfach `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` von der Implementierung.

Web Ankündigungen und Umfragen können auf ähnliche Weise angepasst werden.

Sie können für das Web Ankündigungen erweitern `EngagementWebAnnouncementActivity` und deklarieren Ihrer Aktivitäten in der `AndroidManifest.xml` wie im folgenden Beispiel:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in the intent is the data mime type -->
              </intent-filter>
            </activity>

Sie können für Umfragen erweitern `EngagementPollActivity` und Deklarieren der in der `AndroidManifest.xml` wie im folgenden Beispiel:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Implementierung von Grund auf

Sie können ohne Erweiterung eine der Kategorien für Ihre Aktivitäten Ankündigung (und Umfrage) implementieren der `Engagement*Activity` vom erreichen SDK bereitgestellten Klassen. Dies ist beispielsweise hilfreich ein Layout definieren, die nicht die gleichen Ansichten als standard Layouts verwendet werden soll.

Wie für erweiterte Benachrichtigung Anpassung empfiehlt es sich zu den Quellcode der standard-Implementierung betrachten.

Hier einige Punkte beachten müssen: Reichweite startet die Aktivität mit einer bestimmten (entsprechend des beabsichtigten Filters) plus einen zusätzlichen Parameter, also den Inhalt Bezeichner enthält.

Zum Abrufen des Content-Objekts die Felder enthalten, die Sie beim Erstellen der für eine Marketingkampagne auf der Website angegeben haben, können Sie dies ausführen:

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

Für die Statistik, sollten Sie melden, wird der Inhalt angezeigt, der `onResume` Ereignis:

            @Override
            protected void onResume()
            {
             /* Mark the content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Um eine aufzurufen, klicken Sie dann vergessen Sie nicht `actionContent(this)` oder `exitContent(this)` auf dem Inhalt Objekt, bevor die Aktivität in den Hintergrund wechselt.

Wenn Sie eine aufzurufen, die nicht `actionContent` oder `exitContent`, Statistiken wird nicht (d. h. keine Analytics auf die für eine Marketingkampagne) gesendet werden und wichtiger ist, die nächsten massensendungen zu ermitteln nicht benachrichtigt werden, bis der Anwendungsprozess neu gestartet wird.

Ausrichtung oder andere Änderungen Konfiguration umso des Codes knifflig feststellen, ob die Aktivität in den Hintergrund oder nicht, wechselt die standard-Implementierung wird sichergestellt, dass die Inhalte als beigetreten sind, wenn der Benutzer die Aktivität lässt gemeldet wird (entweder durch Drücken von `HOME` oder `BACK`), aber nicht, wenn die Orientierung geändert.

So sieht der interessante Teil der Implementierung aus:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have to check anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Wie Sie sehen können, wenn Sie angerufen, `actionContent(this)` und dann die Aktivität abgeschlossen `exitContent(this)` ohne jeweiligen problemlos aufgerufen werden können.

[Weitere Schritte]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud Messaging]:http://developer.android.com/guide/google/gcm/index.html
[Amazon Gerät Messaging]:https://developer.amazon.com/sdk/adm.html
