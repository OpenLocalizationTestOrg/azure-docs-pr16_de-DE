<properties
    pageTitle="Senden von sicheren Pushbenachrichtigungen mit Azure Benachrichtigung Hubs"
    description="Informationen Sie zum sicheren Pushbenachrichtigungen zu einer Android-app aus Azure zu senden. Codebeispielen in Java und c# geschrieben wurde."
    documentationCenter="android"
    keywords="Drücken Sie die Benachrichtigung Pushbenachrichtigungen, Nachrichten, android Pushbenachrichtigungen Pushbenachrichtigungen"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

#<a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Senden von sicheren Pushbenachrichtigungen mit Azure Benachrichtigung Hubs

> [AZURE.SELECTOR]
- [Windows-Dienst](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

##<a name="overview"></a>(Übersicht)

> [AZURE.IMPORTANT] Um dieses Lernprogramms abgeschlossen haben, müssen Sie ein aktives Azure-Konto verfügen. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Pushbenachrichtigungen Benachrichtigung Unterstützung in Microsoft Azure ermöglicht Ihnen eine Nachricht einfach zu verwendendes, für mehrere Plattformen, skalierten Pushbenachrichtigungen Infrastruktur, Zugriff auf, die um die Implementierung von Pushbenachrichtigungen für Consumer und Enterprise Applications für mobile Plattformen erheblich zu vereinfachen.

Aufgrund von behördliche oder Einschränkungen Sicherheit, manchmal Anwendung möglicherweise etwas in der Benachrichtigung einschließen möchten, die über die Standardansicht Pushbenachrichtigungen Benachrichtigungsinfrastruktur übermittelt werden können. In diesem Lernprogramm beschrieben, wie die gleiche Oberfläche zu erzielen, indem Sie vertraulichen Informationen über eine sichere, authentifizierte Verbindung zwischen dem Client Android-Gerät und der app Back-End-senden.

Ist der Fluss auf hoher Ebene wie folgt:

1. Die app-Back-End:
    - Stores secure Nutzlast in die Back-End-Datenbank.
    - Sendet die ID dieser Benachrichtigung am Android-Gerät (keine sichere Informationen gesendet).
2. Die app auf dem Gerät, wenn Sie die Benachrichtigung empfangen:
    - Android-Gerät Kontakte die Back-End-secure Nutzlast anfordern.
    - Die app kann die Nutzlast als eine Benachrichtigung auf dem Gerät anzeigen.

Es ist wichtig, beachten Sie, dass im vorhergehenden Fluss (und in diesem Lernprogramm) davon ausgegangen, dass das Gerät eine Authentifizierungstoken im lokalen Speicher, speichert nach der Anmeldung des Benutzers. Dadurch wird eine vollständig nahtlose zur Verfügung steht, sichergestellt, da das Gerät die Benachrichtigung des secure Payload mithilfe dieses Token abrufen kann. Wenn eine Anwendung auf dem Gerät nicht Authentifizierungstoken speichert, oder diese Token abgelaufen sein können, sollte die app Gerät nach Erhalt der Pushbenachrichtigung eine generische Benachrichtigung Bestätigung durch den Benutzer die app gestartet angezeigt werden. Die app, klicken Sie dann authentifiziert den Benutzer und zeigt die Benachrichtigung Nutzlast.

In diesem Lernprogramm erfahren, wie secure Pushbenachrichtigungen zu senden. Es wird erstellt auf das Lernprogramm [Benutzer benachrichtigen](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) , damit Sie die Schritte in diesem Lernprogramm zuerst ausführen soll, wenn Sie noch nicht geschehen ist.

> [AZURE.NOTE] In diesem Lernprogramm wird davon ausgegangen, dass Sie erstellt und Ihre Benachrichtigung Hub wie in [Erste Schritte mit Benachrichtigung Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md)beschrieben konfiguriert haben.

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Ändern Sie das Projekt Android

Jetzt, da Sie Ihre app Back-End um nur die *Id* eines Pushbenachrichtigung senden geändert haben, müssen Sie ändern Ihre Android app, um die Benachrichtigung verarbeitet und Rückruf der Back-End zum Abrufen der sicheren Nachricht angezeigt werden.
Um dies zu erreichen, müssen Sie sicherstellen, dass Ihre app Android weiß, wie selbst mit der Back-End-Authentifizierung, wenn es sich um die Pushbenachrichtigungen erhält.

Wir werden jetzt *Login* illustrieren ändern, um den Wert von Authentifizierung Header in den freigegebenen Voreinstellungen der app zu speichern. Analoge Verfahren können beliebige Authentifizierungstoken (z. B. OAuth Token) gespeichert, die die app zu verwenden, ohne dass Benutzeranmeldeinformationen verwendet werden.

1. Fügen Sie im Projekt Android app am oberen Rand der Klasse **MainActivity** die folgenden Konstanten hinzu:

        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";

2. Immer noch in der Klasse **MainActivity** Aktualisieren der `getAuthorizationHeader()` Methode, um den folgenden Code enthalten:

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);

            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();

            return basicAuthHeader;
        }

3. Fügen Sie den folgenden `import` Anweisungen am oberen Rand der **MainActivity** -Datei:

        import android.content.SharedPreferences;

Jetzt ändern wir den Ereignishandler, der aufgerufen wird, wenn die Benachrichtigung empfangen wurde.

4. In der **MyHandler** Klasse ändern der `OnReceive()` Methode enthalten:

        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }

5. Fügen Sie anschließend die `retrieveNotification()` Methode, ersetzen den Platzhalter `{back-end endpoint}` mit dem Back-End-Endpunkt beim Bereitstellen von Ihrem Back-End abgerufen:

        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);

            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }


Diese Methode ruft Ihre app Back-End zum Abrufen von Inhalt der Benachrichtigung, die mit den Anmeldeinformationen, die in den freigegebenen Voreinstellungen gespeichert und als normaler Benachrichtigung angezeigt. Benachrichtigung sieht der app-Benutzer genau wie alle anderen Pushbenachrichtigung.

Beachten Sie, dass es empfehlenswert ist, die Fällen fehlende Authentifizierung Kopfzeile Eigenschaft oder abgelehnt werden müssen die Back-End zu behandeln. Bestimmte zur Verwendung von diesen Fällen wird hauptsächlich Ihrer Erfahrungen Ziel abhängig. Eine Möglichkeit ist eine Benachrichtigung für eine generische Aufforderung zum Abrufen der ist-Benachrichtigung für die Benutzer authentifiziert angezeigt werden.

## <a name="run-the-application"></a>Führen Sie die Anwendung

Wenn Sie die Anwendung ausführen zu können, führen Sie folgende Schritte aus:

1. Stellen Sie sicher, dass **AppBackend** in Azure bereitgestellt wird. Wenn Visual Studio verwenden zu können, führen Sie die **AppBackend** Web-API-Anwendung. Eine ASP.NET-Webseite wird angezeigt.

2. Führen Sie die app in "Ellipse" klicken Sie auf eine physische Android-Gerät oder den Emulator.

3. Geben Sie in der app Android UI einen Benutzernamen und ein Kennwort ein. Dabei kann es sich um eine beliebige Zeichenfolge, aber sie müssen den gleichen Wert sein.

4. Klicken Sie in der app Android UI auf **Anmelden**. Klicken Sie dann auf **Pushbenachrichtigungen zu senden**.
