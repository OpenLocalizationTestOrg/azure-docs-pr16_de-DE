<properties
    pageTitle="Benachrichtigung Hubs Bruchfestigkeit News Lernprogramm - Android"
    description="Erfahren Sie, wie mit Azure Service Bus Benachrichtigung Hubs um abgeschnitten werden News Benachrichtigungen zu Android-Geräten zu senden."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>


# <a name="use-notification-hubs-to-send-breaking-news"></a>Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand zu senden

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

##<a name="overview"></a>(Übersicht)

In diesem Thema wird gezeigt, wie mit Azure Benachrichtigung Hubs abgeschnitten werden News Benachrichtigungen Android app übertragen werden können. Wenn Sie fertig sind, werden Sie Lage, melden Sie sich für die Bruchfestigkeit News-Kategorien, die, denen Sie interessiert sind, und erhalten nur Pushbenachrichtigungen für diese Kategorien. Dieses Szenario ist ein gemeinsames Muster für viele apps haben, in dem Benachrichtigungen des Planungsprozesses gesendet werden, die zuvor Interesse können, z. B. RSS-Reader, apps für Musik Lüfter usw. deklariert haben.

Übertragenen Szenarien aktiviert wird, indem Sie ein oder mehrere _Kategorien_ einschließlich, wenn eine Registrierung in der Benachrichtigung Hub zu erstellen. Beim Senden von Benachrichtigungen zu einer Kategorie, alle Geräte, die registriert haben, für die Kategorie die Benachrichtigung erhalten. Da Kategorien einfach Zeichenfolgen werden, müssen sie keinen im Voraus bereitgestellt werden. Weitere Informationen zu Kategorien finden Sie in der [Benachrichtigung Hubs Routing und Kategorie Ausdrücke](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Erforderliche Komponenten

In diesem Thema erstellt wird, klicken Sie auf die app, die Sie erstellt, in die [Erste Schritte mit Benachrichtigung Hubs haben][get-started]. Bevor Sie beginnen in diesem Lernprogramm, muss bereits abgeschlossen haben [Erste Schritte mit Benachrichtigung Hubs][get-started].

##<a name="add-category-selection-to-the-app"></a>Hinzufügen von Kategorieauswahl zur app

Dieser erste Schritt besteht hinzufügen die Elemente der Benutzeroberfläche an Ihre vorhandene Hauptfenster Aktivität, mit denen den Benutzer auswählen von Kategorien zu registrieren. Die Kategorien, die von einem Benutzer ausgewählt werden auf dem Gerät gespeichert. Wenn die app gestartet wird, wird eine Registrierung Gerät in der Benachrichtigung Hub mit den ausgewählten Kategorien als Kategorien erstellt.

1. Öffnen Sie die Datei res/layout/activity_main.xml, und Ersetzen Sie den Inhalt mit den folgenden:

        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">

                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>

2. Öffnen Sie die Datei res/values/strings.xml, und fügen Sie die folgenden Zeilen hinzu:

        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>

    Grafische main_activity.xml Layout sollte nun wie folgt aussehen:

    ![][A1]

3. Erstellen Sie nun eine Klasse **Benachrichtigungen** im gleichen Paket wie für Ihre **MainActivity** Klasse ein.

        import java.util.HashSet;
        import java.util.Set;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;

        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;

            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
        
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }

            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }

            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }

            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
        
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
        
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
        
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }

        }

    Diese Klasse verwendet den lokalen Speicher, um die Kategorien der Informationen zu speichern, dieses Gerät hat, erhalten. Darüber hinaus enthält Methoden zum Registrieren für diese Kategorien.


4. Entfernen Sie Ihre private Felder für **NotificationHub** und **GoogleCloudMessaging**und Hinzufügen eines Felds für **Benachrichtigungen**in der Klasse **MainActivity** :

        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;

5. Klicken Sie dann in der **OnCreate** -Methode, entfernen Sie die Initialisierung des Felds **Hub** und die Methode **RegisterWithNotificationHubs** an. Fügen Sie dann die folgenden Zeilen, die Initialisierung einer Instanz der Klasse **Benachrichtigungen** hinzu. 


        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;
    
            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);
    
            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);
    
            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`und `HubListenConnectionString` sollte bereits eingerichtet werden, mit der `<hub name>` und `<connection string with listen access>` Platzhalter mit Ihren Benachrichtigung Hubnamen und die Verbindungszeichenfolge für *DefaultListenSharedAccessSignature* , die Sie zuvor für Ihren Kunden.

    > [AZURE.NOTE] Da Anmeldeinformationen, die mit einer Clientanwendung verteilt werden nicht in der Regel sicher sind, sollten Sie nur die Taste für Listen Access Client-App verteilen. Abhören von Access ermöglicht, die Ihre app registrieren für Benachrichtigungen, aber vorhandene Registrierungen geändert werden kann, und nicht die Benachrichtigungen gesendet werden. Die vollständige Zugriffstaste wird in einem gesicherten Back-End-Dienst für Benachrichtigungen senden, und ändern die vorhandene Registrierungen verwendet.


6. Fügen Sie dann die folgenden Importe und `subscribe` Methode, um die Schaltfläche Abonnieren behandeln, klicken Sie auf Ereignis:
        
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;

        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();

            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");

            notifications.storeCategoriesAndSubscribe(categories);
        }

    Diese Methode erstellt eine Liste der Kategorien und verwendet die **Benachrichtigungen** -Klasse zum Speichern der Liste im lokalen Speicher und die entsprechenden Tags mit Ihrem Hub Benachrichtigung registrieren. Wenn Kategorien geändert werden, wird die Registrierung mit den neuen Kategorien neu erstellt werden.

Ihre app kann nun eine Reihe von Kategorien im lokalen Speicher auf dem Gerät gespeichert und mit dem Hub Benachrichtigung registrieren, wenn der Benutzer die Auswahl der Kategorien ändert.

##<a name="register-for-notifications"></a>Registrieren für Benachrichtigungen

Diese Schritte registrieren Sie sich mit dem Hub Benachrichtigung beim Start mit Hilfe der Kategorien, die im lokalen Speicher gespeichert wurden.

> [AZURE.NOTE] Da die Attribute zugewiesen von Google Cloud Messaging (GCM) zu einem beliebigen Zeitpunkt ändern kann, sollten Sie für Benachrichtigung Fehler vermeiden, häufig die Benachrichtigungen registrieren. In diesem Beispiel registriert für Benachrichtigung jedes Mal, die die app gestartet wird. Für apps, die häufig ausgeführt werden, können mehr als einmal pro Tag, Sie wahrscheinlich überspringen Registrierung um Bandbreite zu sparen, wenn Sie weniger als ein Tag nach der vorherigen Registrierung verstrichen ist.


1. Fügen Sie den folgenden Code am Ende der **OnCreate** Methode in der Klasse **MainActivity** hinzu:

        notifications.subscribeToCategories(notifications.retrieveCategories());

    Dadurch wird sichergestellt, dass jedes Mal, wenn die app gestartet Kategorien aus dem lokalen Speicher abgerufen und eine Registeration für diese Kategorien anfordert. 

2. Aktualisieren Sie dann die `onStart()` Methode zum der `MainActivity` Klasse wie folgt:

    @Overridegeschützte void onStart() {super.onStart();      IsVisible = WAHR;

        Set<String> categories = notifications.retrieveCategories();

        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }

    Dadurch wird die Hauptfenster Aktivität basierend auf den Status des zuvor gespeicherte Kategorien aktualisiert.

Die app ist jetzt vollständig, und speichern eine Reihe von Kategorien kann in den lokalen Speicher des Geräts verwendet, um mit dem Hub Benachrichtigung registrieren, wenn der Benutzer die Auswahl der Kategorien ändert. Als Nächstes werden wir eine Back-End-definieren, die diese app Kategorie Benachrichtigungen senden können.

##<a name="sending-tagged-notifications"></a>Markierte Benachrichtigungen senden

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Führen Sie die app und Generieren von Benachrichtigungen

1. Klicken Sie in Android Studio erstellen Sie die app, und starten sie auf einem Gerät oder Emulator.

    Beachten Sie, dass die app-Benutzeroberfläche eine Reihe von schaltet enthält, mit dem Sie wählen Sie die Kategorien zum Abonnieren aus.

2. Aktivieren eine oder mehrere Kategorien schaltet, klicken Sie dann auf **Abonnieren**.

    Die app wandelt die ausgewählten Kategorien in Kategorien und eine neue Gerät Registrierung für den ausgewählten Tags vom Hub Benachrichtigung anfordert. Die registrierten Kategorien werden zurückgegeben und in eine Benachrichtigung Spruch angezeigt.

4. Senden Sie eine neue Benachrichtigung durch Ausführen der .NET Console-app.  Alternativ können Sie mithilfe der Registerkarte Debuggen Ihrer Hub Benachrichtigung in der [Klassischen Azure-Portal]markiertes Vorlage-Benachrichtigungen senden.

    Benachrichtigungen für den ausgewählten Kategorien werden als Spruch Benachrichtigungen angezeigt.

##<a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben wir wie dem neusten Stand nach Kategorie zu übertragen. Erwägen Sie eine der folgenden Lernprogramme, die andere erweiterten Benachrichtigung Hubs Szenarien hervorheben durchführen:

+ [Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand lokalisierten übertragen]

    Informationen Sie zum Erweitern der abgeschnitten werden News app zum Senden lokalisierte Benachrichtigungen aktivieren.





<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand lokalisierten übertragen]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Azure klassischen-Portal]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
