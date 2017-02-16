<properties
    pageTitle="Azure Benachrichtigung Hubs Benutzer benachrichtigen für Android mit .NET Back-End-"
    description="Informationen Sie zum Senden von Pushbenachrichtigungen für Benutzer in Azure. Beispiele in Java für Android"
    documentationCenter="android"
    services="notification-hubs"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-notify-users-for-android-with-net-backend"></a>Azure Benachrichtigung Hubs Benutzer benachrichtigen für Android mit .NET Back-End-


[AZURE.INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

##<a name="overview"></a>(Übersicht)

Pushbenachrichtigungen Benachrichtigung Unterstützung in Azure ermöglicht es Ihnen zu einer einfach zu verwendendes, mehrere Plattformen und skalierten Pushbenachrichtigungen Infrastruktur die Implementierung von Pushbenachrichtigungen für Consumer und Enterprise Applications für mobile Plattformen vereinfacht zugegriffen werden. In diesem Lernprogramm erfahren Sie, wie Azure Benachrichtigung Hubs um Pushbenachrichtigungen an einen bestimmten app-Benutzer auf ein bestimmtes Gerät zu senden. Eine ASP.NET WebAPI Back-End-wird Siehe im Thema Anleitungen [registrieren aus Ihrer app Back-End-](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend)Clients authentifizieren und zum Generieren von Benachrichtigungen, verwendet. In diesem Lernprogramm erstellt am Hub Benachrichtigung, den Sie in das [Erste Schritte mit Benachrichtigung Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) Lernprogramm erstellt haben.

> [AZURE.NOTE] In diesem Lernprogramm wird davon ausgegangen, dass Sie erstellt und Ihre Benachrichtigung Hub wie in [Erste Schritte mit Benachrichtigung Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md)beschrieben konfiguriert haben.

[AZURE.INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="create-the-android-project"></a>Erstellen Sie das Projekt Android

Im nächsten Schritt wird die Android-Anwendung zu erstellen.

1. Führen Sie das [Erste Schritte mit Benachrichtigung Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) Lernprogramm zum Erstellen und Konfigurieren der app, um Pushbenachrichtigungen von GCM erhalten.

2. Öffnen Sie die Datei **res/layout/activity_main.xml** , ersetzen die mit den folgenden Inhalten Definitionen.

    Dadurch wird die neue EditText Steuerelemente für die Anmeldung bei als Benutzer hinzugefügt. Außerdem wird ein Feld für eine Kategorie Benutzername hinzugefügt, die in Benachrichtigungen aufgenommen werden soll, die Sie senden:

        <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
            android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">

        <EditText
            android:id="@+id/usernameText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="@string/usernameHint"
            android:layout_above="@+id/passwordText"
            android:layout_alignParentEnd="true" />
        <EditText
            android:id="@+id/passwordText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="@string/passwordHint"
            android:inputType="textPassword"
            android:layout_above="@+id/buttonLogin"
            android:layout_alignParentEnd="true" />
        <Button
            android:id="@+id/buttonLogin"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/loginButton"
            android:onClick="login"
            android:layout_above="@+id/toggleButtonGCM"
            android:layout_centerHorizontal="true"
            android:layout_marginBottom="24dp" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="WNS on"
            android:textOff="WNS off"
            android:id="@+id/toggleButtonWNS"
            android:layout_toLeftOf="@id/toggleButtonGCM"
            android:layout_centerVertical="true" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="GCM on"
            android:textOff="GCM off"
            android:id="@+id/toggleButtonGCM"
            android:checked="true"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="APNS on"
            android:textOff="APNS off"
            android:id="@+id/toggleButtonAPNS"
            android:layout_toRightOf="@id/toggleButtonGCM"
            android:layout_centerVertical="true" />
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/editTextNotificationMessageTag"
            android:layout_below="@id/toggleButtonGCM"
            android:layout_centerHorizontal="true"
            android:hint="@string/notification_message_tag_hint" />
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/editTextNotificationMessage"
            android:layout_below="@+id/editTextNotificationMessageTag"
            android:layout_centerHorizontal="true"
            android:hint="@string/notification_message_hint" />
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/send_button"
            android:id="@+id/sendbutton"
            android:onClick="sendNotificationButtonOnClick"
            android:layout_below="@+id/editTextNotificationMessage"
            android:layout_centerHorizontal="true" />
        </RelativeLayout>



3. Öffnen Sie die Datei **res/values/strings.xml** und ersetzen die `send_button` Definition mit den folgenden Zeilen, die die Zeichenfolge für Zellwerte der `send_button` und Zeichenfolgen für die anderen Steuerelemente hinzufügen:

        <string name="usernameHint">Username</string>
        <string name="passwordHint">Password</string>
        <string name="loginButton">1. Log in</string>
        <string name="send_button">2. Send Notification</string>
        <string name="notification_message_tag_hint">
            Recipient username tag
        </string>

    Grafische main_activity.xml Layout sollte nun wie folgt aussehen:

    ![][A1]

4. Erstellen Sie eine neue Klasse namens **RegisterClient** in demselben Paket wie Ihre `MainActivity` Class. Verwenden Sie den folgenden Code für die neue Klassendatei ein.

        import java.io.IOException;
        import java.io.UnsupportedEncodingException;
        import java.util.Set;

        import org.apache.http.HttpResponse;
        import org.apache.http.HttpStatus;
        import org.apache.http.client.ClientProtocolException;
        import org.apache.http.client.HttpClient;
        import org.apache.http.client.methods.HttpPost;
        import org.apache.http.client.methods.HttpPut;
        import org.apache.http.client.methods.HttpUriRequest;
        import org.apache.http.entity.StringEntity;
        import org.apache.http.impl.client.DefaultHttpClient;
        import org.apache.http.util.EntityUtils;
        import org.json.JSONArray;
        import org.json.JSONException;
        import org.json.JSONObject;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.util.Log;

        public class RegisterClient {
            private static final String PREFS_NAME = "ANHSettings";
            private static final String REGID_SETTING_NAME = "ANHRegistrationId";
            private String Backend_Endpoint;
            SharedPreferences settings;
            protected HttpClient httpClient;
            private String authorizationHeader;

            public RegisterClient(Context context, String backendEnpoint) {
                super();
                this.settings = context.getSharedPreferences(PREFS_NAME, 0);
                httpClient =  new DefaultHttpClient();
                Backend_Endpoint = backendEnpoint + "/api/register";
            }

            public String getAuthorizationHeader() {
                return authorizationHeader;
            }

            public void setAuthorizationHeader(String authorizationHeader) {
                this.authorizationHeader = authorizationHeader;
            }

            public void register(String handle, Set<String> tags) throws ClientProtocolException, IOException, JSONException {
                String registrationId = retrieveRegistrationIdOrRequestNewOne(handle);

                JSONObject deviceInfo = new JSONObject();
                deviceInfo.put("Platform", "gcm");
                deviceInfo.put("Handle", handle);
                deviceInfo.put("Tags", new JSONArray(tags));

                int statusCode = upsertRegistration(registrationId, deviceInfo);

                if (statusCode == HttpStatus.SC_OK) {
                    return;
                } else if (statusCode == HttpStatus.SC_GONE){
                    settings.edit().remove(REGID_SETTING_NAME).commit();
                    registrationId = retrieveRegistrationIdOrRequestNewOne(handle);
                    statusCode = upsertRegistration(registrationId, deviceInfo);
                    if (statusCode != HttpStatus.SC_OK) {
                        Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                        throw new RuntimeException("Error upserting registration");
                    }
                } else {
                    Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                    throw new RuntimeException("Error upserting registration");
                }
            }

            private int upsertRegistration(String registrationId, JSONObject deviceInfo)
                    throws UnsupportedEncodingException, IOException,
                    ClientProtocolException {
                HttpPut request = new HttpPut(Backend_Endpoint+"/"+registrationId);
                request.setEntity(new StringEntity(deviceInfo.toString()));
                request.addHeader("Authorization", "Basic "+authorizationHeader);
                request.addHeader("Content-Type", "application/json");
                HttpResponse response = httpClient.execute(request);
                int statusCode = response.getStatusLine().getStatusCode();
                return statusCode;
            }

            private String retrieveRegistrationIdOrRequestNewOne(String handle) throws ClientProtocolException, IOException {
                if (settings.contains(REGID_SETTING_NAME))
                    return settings.getString(REGID_SETTING_NAME, null);

                HttpUriRequest request = new HttpPost(Backend_Endpoint+"?handle="+handle);
                request.addHeader("Authorization", "Basic "+authorizationHeader);
                HttpResponse response = httpClient.execute(request);
                if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                    Log.e("RegisterClient", "Error creating registrationId: " + response.getStatusLine().getStatusCode());
                    throw new RuntimeException("Error creating Notification Hubs registrationId");
                }
                String registrationId = EntityUtils.toString(response.getEntity());
                registrationId = registrationId.substring(1, registrationId.length()-1);

                settings.edit().putString(REGID_SETTING_NAME, registrationId).commit();

                return registrationId;
            }
        }

    Diese Komponente implementiert REST Anrufe erforderlich, um die app Back-End-Kontakt, um Pushbenachrichtigungen zu registrieren. Die Benachrichtigung wie ausführlich [registrieren aus Ihrer app Back-End-](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend)Hub erstellte *RegistrationIds* auch lokal gespeichert. Beachten Sie, dass er eine Autorisierungstoken im lokalen Speicher gespeichert verwendet, wenn Sie auf die Schaltfläche **Log in** .

5. In Ihrer `MainActivity` Klasse entfernen oder Ihr privates Feld für kommentieren `NotificationHub`, und Hinzufügen eines Felds für den `RegisterClient` Klasse und einer Zeichenfolge für Ihre ASP.NET Back-End-Endpunkt. Ersetzen Sie unbedingt `<Enter Your Backend Endpoint>` mit der der tatsächlichen Back-End-Endpunkt zuvor abgerufen. Beispielsweise `http://mybackend.azurewebsites.net`.


        //private NotificationHub hub;
        private RegisterClient registerClient;
        private static final String BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";


6. In Ihrer `MainActivity` Klasse, in der `onCreate` Methode, entfernen oder kommentieren Sie Sie aus der Fehler bei der Initialisierung der `hub` Feld und den Anruf an die `registerWithNotificationHubs` Methode. Klicken Sie dann Code hinzufügen, um eine Instanz der Initialisierung der `RegisterClient` Class. Die Methode sollte die folgenden Zeilen enthalten:

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            MyHandler.mainActivity = this;
            NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);
            gcm = GoogleCloudMessaging.getInstance(this);

            //hub = new NotificationHub(HubName, HubListenConnectionString, this);
            //registerWithNotificationHubs();

            registerClient = new RegisterClient(this, BACKEND_ENDPOINT);

            setContentView(R.layout.activity_main);
        }

7. In Ihrer `MainActivity` Klasse, löschen oder kommentieren Sie Sie aus der gesamten `registerWithNotificationHubs` Methode. Es wird nicht in diesem Lernprogramm verwendet werden.

8. Fügen Sie den folgenden `import` Anweisungen zur Datei **MainActivity.java** .

        import android.widget.Button;
        import java.io.UnsupportedEncodingException;
        import android.content.Context;
        import java.util.HashSet;
        import android.widget.Toast;
        import org.apache.http.client.ClientProtocolException;
        import java.io.IOException;
        import org.apache.http.HttpStatus;


9. Klicken Sie dann fügen Sie die folgenden Methoden, die **Anmelden** -Schaltfläche behandeln Ereignis und senden Pushbenachrichtigungen klicken Sie auf hinzu.

        @Override
        protected void onStart() {
            super.onStart();
            Button sendPush = (Button) findViewById(R.id.sendbutton);
            sendPush.setEnabled(false);
        }

        public void login(View view) throws UnsupportedEncodingException {
            this.registerClient.setAuthorizationHeader(getAuthorizationHeader());

            final Context context = this;
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        String regid = gcm.register(SENDER_ID);
                        registerClient.register(regid, new HashSet<String>());
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed to register", e.getMessage());
                        return e;
                    }
                    return null;
                }

                protected void onPostExecute(Object result) {
                    Button sendPush = (Button) findViewById(R.id.sendbutton);
                    sendPush.setEnabled(true);
                    Toast.makeText(context, "Logged in and registered.",
                            Toast.LENGTH_LONG).show();
                }
            }.execute(null, null, null);
        }

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
            return basicAuthHeader;
        }

        /**
         * This method calls the ASP.NET WebAPI backend to send the notification message
         * to the platform notification service based on the pns parameter.
         *
         * @param pns     The platform notification service to send the notification message to. Must
         *                be one of the following ("wns", "gcm", "apns").
         * @param userTag The tag for the user who will receive the notification message. This string
         *                must not contain spaces or special characters.
         * @param message The notification message string. This string must include the double quotes
         *                to be used as JSON content.
         */
        public void sendPush(final String pns, final String userTag, final String message)
                throws ClientProtocolException, IOException {
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {

                        String uri = BACKEND_ENDPOINT + "/api/notifications";
                        uri += "?pns=" + pns;
                        uri += "&to_tag=" + userTag;

                        HttpPost request = new HttpPost(uri);
                        request.addHeader("Authorization", "Basic "+ getAuthorizationHeader());
                        request.setEntity(new StringEntity(message));
                        request.addHeader("Content-Type", "application/json");

                        HttpResponse response = new DefaultHttpClient().execute(request);

                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            DialogNotify("MainActivity - Error sending " + pns + " notification",
                                response.getStatusLine().toString());
                            throw new RuntimeException("Error sending notification");
                        }
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed to send " + pns + " notification ", e.getMessage());
                        return e;
                    }

                    return null;
                }
            }.execute(null, null, null);
        }


    Die `login` generiert eine Standardauthentifizierung Ereignishandler für die Schaltfläche **Anmelden** token verwenden, klicken Sie auf die Eingabewerte Benutzernamen und Ihr Kennwort (Beachten Sie, dass dies alle Token darstellt, der Authentifizierung des Farbschemas verwendet), und klicken Sie dann verwendet `RegisterClient` aufrufen, die Back-End-Registrierung.

    Die `sendPush` Methode ruft die Back-End-, um eine sichere Benachrichtigung für den Benutzer, basierend auf die Kategorie Benutzer auslösen. Die Benachrichtigung Plattform service, die `sendPush` Ziele richtet sich nach der `pns` Zeichenfolge übergeben wird.

10. In Ihrer `MainActivity` Klasse, aktualisieren Sie die `sendNotificationButtonOnClick` Methode zum Aufrufen der `sendPush` Methode mit des Benutzers Plattform Benachrichtigungsdienste wie folgt aktiviert.

        /**
         * Send Notification button click handler. This method sends the push notification
         * message to each platform selected.
         *
         * @param v The view
         */
        public void sendNotificationButtonOnClick(View v)
                throws ClientProtocolException, IOException {

            String nhMessageTag = ((EditText) findViewById(R.id.editTextNotificationMessageTag))
                    .getText().toString();
            String nhMessage = ((EditText) findViewById(R.id.editTextNotificationMessage))
                    .getText().toString();

            // JSON String
            nhMessage = "\"" + nhMessage + "\"";

            if (((ToggleButton)findViewById(R.id.toggleButtonWNS)).isChecked())
            {
                sendPush("wns", nhMessageTag, nhMessage);
            }
            if (((ToggleButton)findViewById(R.id.toggleButtonGCM)).isChecked())
            {
                sendPush("gcm", nhMessageTag, nhMessage);
            }
            if (((ToggleButton)findViewById(R.id.toggleButtonAPNS)).isChecked())
            {
                sendPush("apns", nhMessageTag, nhMessage);
            }
        }



## <a name="run-the-application"></a>Führen Sie die Anwendung


1. Führen Sie die Anwendung auf einem Gerät oder einen Emulator Android Studio verwenden.

2. Geben Sie in der app Android einen Benutzernamen und ein Kennwort ein. Beide müssen den gleichen Zeichenfolgenwert aus, und sie müssen keine Leerzeichen oder Sonderzeichen enthalten.

3. Klicken Sie in der Android-app auf **Anmelden**. Warten Sie auf einer Spruch Nachricht, die besagt **protokollierte in und registriert ist**. Dadurch werden die **Benachrichtigung senden** -Schaltfläche.

    ![][A2]

4. Klicken Sie auf die Umschaltfläche Schaltflächen, um alle Plattformen aktivieren, Sie haben, die app ausgeführt und registriert ein Benutzers.
5. Geben Sie den Namen des Benutzers, der die Benachrichtigung erhalten soll. Der Benutzer muss für Benachrichtigungen auf die Zielgeräte registriert sein.

6. Geben Sie eine Meldung für den Benutzer als eine Benachrichtigung Pushbenachrichtigungen zu erhalten.
7. Klicken Sie auf die **Benachrichtigung senden**.  Jedes Gerät, das eine Registrierung mit dem übereinstimmenden Username-Tag, der Pushbenachrichtigung erhalten.


[A1]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users.png
[A2]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users-enter-password.png
