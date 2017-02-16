<properties
    pageTitle="Azure Active Directory B2C: Rufen Sie eine Web-API Anwendung Android | Microsoft Azure"
    description="In diesem Artikel wird gezeigt, wie Sie einem Android-Gerät 'Vorgangsliste' erstellen app, die eine Web Node.js API ruft mithilfe von OAuth 2.0 Person Token. Sowohl die Android-app und im Web API verwenden Azure Active Directory B2C zum Verwalten von Benutzeridentitäten und Authentifizieren von Benutzern."
    services="active-directory-b2c"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-call-a-web-api-from-an-android-application"></a>Azure AD-B2C: Rufen Sie eine Web-API Android-Anwendung

> [AZURE.WARNING] Dieses Lernprogramm erfordert einige wichtige Updates speziell für die Verwendung von ADAL Android für B2C entfernen.  Wir werden frische Anweisungen für die Verwendung von Azure AD B2C Android Apps in der nächsten Woche veröffentlichen, und es wird empfohlen, halten bis zu diesem Zeitpunkt deaktivieren.  Aber wenn Sie nur die Dinge ausprobieren möchten, gerne im nachstehenden Artikel fortzusetzen.



Mithilfe von B2C Azure Active Directory (Azure AD), können Sie leistungsfähige Self-service-Identität Verwaltungsfunktionen auf Ihrem Android apps und Web-APIs in wenigen einfachen Schritten hinzufügen. In diesem Artikel wird gezeigt, wie Sie einem Android-Gerät "Aufgabenliste" erstellen app, die eine Web Node.js API ruft mithilfe von OAuth 2.0 Person Token. Sowohl die Android-app und im Web API verwenden Azure AD B2C zum Verwalten von Benutzeridentitäten und Authentifizieren von Benutzern.

Dieser Schnellstart erfordert, dass Sie eine Web-API verfügen, die durch Azure AD mit B2C entwickelt vollständig geschützt ist. Wir haben eine für .NET und Node.js für Ihre Verwendung aufgebaut. Diese exemplarische Vorgehensweise wird vorausgesetzt, dass die Stichprobe Node.js Web API konfiguriert ist. Weitere Informationen finden Sie unter dem [Azure AD B2C Web API Node.js Lernprogramm](active-directory-b2c-devquickstarts-api-node.md).

Für Android-Clients, die Zugriff auf geschützte Ressourcen benötigen, bietet Azure AD der Active Directory Authentifizierung Bibliothek (ADAL). Der einzige Zweck des ADAL ist, die für Ihre app in Access Token abrufen zu erleichtern. Um zu veranschaulichen, wie einfach es ist, in diesem Leitfaden wird wir Anwendung Liste Android Aufgabe erstellen, die:

- Ruft Access Token, die eine Vorgangsliste API aufrufen, indem Sie mit dem [OAuth 2.0-Authentifizierung-Protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx)ab.
- Ruft Vorgangslisten Benutzer ab.
- Vorzeichen sich Benutzer.

> [AZURE.NOTE] In diesem Artikel behandelt keine Anmeldung, Anmeldung Implementierung und Verwaltung von Profilen mithilfe von Azure AD B2C. Es liegt der Schwerpunkt zum Web-APIs aufrufen, nachdem der Benutzer authentifiziert wird. Wenn Sie noch nicht geschehen ist, sollten Sie die Grundlagen des Azure AD B2C Lernen mit der [.NET Web app-erste Schritte-Lernprogramm](active-directory-b2c-devquickstarts-web-dotnet.md) beginnen.

## <a name="get-an-azure-ad-b2c-directory"></a>Abrufen einer Azure AD B2C-Verzeichnis

Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis erstellen, oder den Mandanten. Ein Verzeichnis ist ein Container für alle Benutzer, apps, Gruppen und mehr. Wenn Sie eine bereits besitzen, [Erstellen Sie ein Verzeichnis B2C](active-directory-b2c-get-started.md) , bevor Sie Sie in diesem Handbuch fortsetzen.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Als Nächstes müssen Sie eine app in Ihrem Verzeichnis B2C erstellen. Dadurch Azure AD-Informationen, die sichere Kommunikation mit Ihrem app benötigt werden. Der app und die Web-API werden durch eine einzelne **Anwendung-ID** in diesem Fall dargestellt, weil eine logische app bestehen aus. [Wenn Sie eine app erstellen, gehen Sie wie folgt vor.](active-directory-b2c-app-registration.md) Achten Sie darauf, um:

- Einschließen einer **Web app**/**Web API** in der Anwendung.
- Geben Sie `urn:ietf:wg:oauth:2.0:oob` als **Antwort-URL**. Es ist die Standard-URL für dieses Codebeispiel.
- Erstellen Sie einer **Anwendung geheim** für eine Anwendung, und kopieren Sie es zu. Sie werden diese später benötigen. Beachten Sie, dass dieser Wert muss [XML mit Escapezeichen versehen](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) werden, bevor Sie es verwenden.
- Kopieren Sie die **ID der Anwendung** , die zu Ihrer Anwendung zugeordnet ist. Sie auch benötigen diese später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen Sie Ihre Richtlinien

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

In Azure AD B2C wird jede Erfahrungen von einer [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Diese app enthält drei Identität Erfahrung: registrieren, melden Sie sich an, und melden Sie sich mithilfe von Facebook.  Sie müssen zum Erstellen einer Richtlinie für jedes Typs wie in der [Richtlinie Bezug Artikel](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)beschrieben. Wenn Sie Ihre drei Richtlinien erstellen, müssen Sie unbedingt:

- Wählen Sie den **Anzeigenamen** und andere Attribute Anmeldung in Ihrer Anmeldung Richtlinie aus.
- Wählen Sie die Anwendung Ansprüche **Anzeigenamen** und **Objekt-ID** in jeder Richtlinie aus. Sie können auch anderen Ansprüche auswählen.
- Kopieren Sie den **Namen** der einzelnen Richtlinie, nachdem Sie es erstellt haben. Es sollte das Präfix haben `b2c_1_`.  Sie benötigen diese Richtliniennamen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die drei Richtlinien erstellt haben, sind Sie bereit sind, Ihre app zu erstellen.

Beachten Sie, dass in diesem Artikel behandelt nicht so verwenden Sie die Richtlinien, die Sie soeben erstellt haben. Weitere Informationen zur Funktionsweise von Richtlinien in Azure AD B2C, beginnen Sie mit dem [Lernprogramm .NET Web app-erste Schritte](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Laden Sie den code

Der Code für dieses Lernprogramm [auf GitHub verwaltet wird](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android). Wenn Sie um das Beispiel beim Durcharbeiten zu erstellen, können Sie [das Gerüst Projekt als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/skeleton.zip). Sie können auch das Gerüst Klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-Android.git
```

> [AZURE.NOTE] **Sie müssen das Gerüst zum Abschluss des Lernprogramms herunterladen.** Aufgrund der Komplexität der Implementierung einer voll funktionsfähigen Android-Anwendungs hat das Gerüst UX Code, die ausgeführt werden kann, nachdem Sie in diesem Lernprogramm beendet haben. Dies ist ein Measure Zeit sparen für Entwickler. Der UX Code ist nicht mit dem Thema zum B2C zur Anwendung Android Hinzufügen von Belang.

Die fertige app steht auch [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip) oder das `complete` Zweig des gleichen Repositorys.

Um Maven zu erstellen, können Sie `pom.xml` auf der obersten Ebene.

  1. Führen Sie die Schritte im [Abschnitt erforderliche Komponenten Maven für Android einrichten](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).
  2. Richten Sie einen Emulator mit SDK 21 aus.
  3. Wechseln Sie zu der Stelle, an der Sie die Repo geklont Stammordner.
  4. Führen Sie den Befehl `mvn clean install`.
  5. Wechseln Sie in der Stichprobe Schnellstart `cd samples\hello`.
  6. Führen Sie den Befehl `mvn android:deploy android:run`.

Die app starten sollte angezeigt werden. Geben Sie die Benutzeranmeldeinformationen testen, um ihn zu testen.

Java-Archiv (JAR)-Paketen werden auch neben das Paket Android archivieren (AAR) gesendet werden.

## <a name="download-the-android-adal-and-add-it-to-your-android-studio-workspace"></a>Android ADAL herunterladen und zu dem Arbeitsbereich Android Studio hinzufügen

Sie haben die Optionen für diese Bibliothek im Projekt Android verwenden:

* Des Quellcodes können um die Bibliothek in "Ellipse" und Link an Ihrer Anwendung zu importieren.
* Wenn Sie Android Studio verwenden, können Sie verwenden Sie das Format der AAR-Paket und die Binärdateien verweisen.

### <a name="option-1-binaries-via-gradle-recommended"></a>Option 1: Binärdateien über Gradle (empfohlen)

Sie können die Binärdateien aus der zentralen Maven Repo erhalten. Das Paket AAR in Ihrem Projekt in Android Studio enthalten sein kann (z. B. in `build.gradle`) auf diese Weise:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:2.0.1-alpha') {
        exclude group: 'com.android.support'
    } // Recent version is 2.0.1-alpha
}
```

### <a name="option-2-aar-via-maven"></a>Option 2: AAR über Maven

Wenn Sie verwenden die `m2e` in "Ellipse"-Plug-in, Sie können angeben, die Abhängigkeit in Ihrer `pom.xml` Datei:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>2.0.1-alpha</version>
    <type>aar</type>
</dependency>
```

### <a name="option-3-source-via-git-last-resort"></a>Option 3: Datenquelle über Git (letzter Mittel)

Um den Quellcode des SDK über Git zu gelangen, geben Sie Folgendes ein:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

Verwenden die Verzweigung **Zusammenführung.**

## <a name="set-up-your-configuration-file"></a>Richten Sie Ihre Konfigurationsdatei

Verwenden Sie die Konfiguration, die Sie von einrichten früheren im Portal B2C Android Projekt zu konfigurieren.

Open `helpes/Constants.java` und füllen Sie die Werte für Folgendes:

```

package com.microsoft.aad.taskapplication.helpers;

import com.microsoft.aad.adal.AuthenticationResult;

public class Constants {

    public static final String SDK_VERSION = "1.0";
    public static final String UTF8_ENCODING = "UTF-8";
    public static final String HEADER_AUTHORIZATION = "Authorization";
    public static final String HEADER_AUTHORIZATION_VALUE_PREFIX = "Bearer ";

    // -------------------------------AAD
    // PARAMETERS----------------------------------
    public static String AUTHORITY_URL = "https://login.microsoftonline.com/hypercubeb2c.onmicrosoft.com/";
    public static String CLIENT_ID = "<your application id>";
    public static String[] SCOPES = {"<your application id>"};
    public static String[] ADDITIONAL_SCOPES = {""};
    public static String REDIRECT_URL = "<redirect uri>";
    public static String CORRELATION_ID = "";
    public static String USER_HINT = "";
    public static String EXTRA_QP = "";
    public static String FB_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNIN_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNUP_POLICY = "B2C_1_<your policy>";
    public static boolean FULL_SCREEN = true;
    public static AuthenticationResult CURRENT_RESULT = null;
    // Endpoint we are targeting for the deployed WebAPI service
    public static String SERVICE_URL = "http://localhost:3000/tasks";

    // ------------------------------------------------------------------------------------------

    static final String TABLE_WORKITEM = "WorkItem";
    public static final String SHARED_PREFERENCE_NAME = "com.example.com.test.settings";


}


```
- `SCOPES`: Anmeldet Bereichen, die Sie auf dem Server zu übergeben, die Sie vom Server, wenn ein Benutzer anfordern möchten. Damit die B2C Vorschau Sie übergeben `client_id`. Dies ist jedoch erwartet zu ändern `read scopes` in der Zukunft. Wenn auftritt, wird dieses Dokument aktualisiert.
- `ADDITIONAL_SCOPES`: Weitere Bereiche, die Sie für eine Anwendung verwenden möchten. Sie sind in der Zukunft verwendet werden soll.
- `CLIENT_ID`: Die Anwendung-ID, die Sie aus dem Portal erhalten haben.
- `REDIRECT_URL`: Umleitung der Stelle, an der das Token zurück bereitgestellt erwartet.
- `EXTRA_QP`: Nichts zusätzliche Sie auf dem Server in einem Format URL-codierte übergeben möchten.
- `FB_POLICY`: Die Richtlinie, die Sie aufrufen. Dies ist der wichtigste Teil für diese exemplarische Vorgehensweise.
- `EMAIL_SIGNIN_POLICY`: Die Richtlinie, die Sie aufrufen. Dies ist der wichtigste Teil für diese exemplarische Vorgehensweise.
- `EMAIL_SIGNUP_POLICY`: Die Richtlinie, die Sie aufrufen. Dies ist der wichtigste Teil für diese exemplarische Vorgehensweise.

## <a name="add-references-to-android-adal-to-your-project"></a>Fügen Sie Verweise auf Android ADAL zu einem Projekt

> [AZURE.NOTE]  ADAL für Android mithilfe einer Absicht basierendes Modell Authentifizierung aufgerufen. Intents verschaffen"über" arbeiten, führen Sie die app. In diesem Beispiel der gesamte und alle ADAL für Android Center zum Verwalten von Intents und Informationen dazwischen übergeben.

Informieren Sie zunächst Android, über das Layout Ihrer Anwendung, einschließlich der Intents, die Sie verwenden möchten. Diese Intents werden später in diesem Lernprogramm im Detail erläutert.

Aktualisieren Ihres Projekts `AndroidManifest.xml` Datei alle Ihre Intents enthalten:

```
   <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.microsoft.aad.taskapplication"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="11"
        android:targetSdkVersion="19" />

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="com.microsoft.aad.adal.AuthenticationActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:exported="true"
            android:label="@string/title_login_hello_app" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.LoginActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.UsersListActivity"
            android:label="@string/title_activity_feed" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.SettingsActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.AddTaskActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.ToDoActivity"
            android:label="@string/app_name" >
        </activity>
    </application>

</manifest>    
```

Wie Sie sehen können, definieren Sie fünf Aktivitäten. Verwenden Sie diese.

- `AuthenticationActivity`: Dies von ADAL stammt, und die Anmeldung Webanzeige bietet.
- `LoginActivity`: Dies zeigt Ihre Richtlinien Anmeldung und den Schaltflächen für jede Richtlinie.
- `SettingsActivity`: Sie verwenden die Appeinstellungen zur Laufzeit ändern.
- `AddTaskActivity`: Hiermit Sie zum Hinzufügen von Vorgängen zu Ihrem REST-API, die von Azure Active Directory geschützt sind.
- `ToDoActivity`: Dies ist das Hauptfenster Aktivitäten, das Aufgaben angezeigt werden.

## <a name="create-the-sign-in-activity"></a>Erstellen der Aktivität Anmeldung

Erstellen eine Aktivität Hauptfenster, und nennen Sie diese `LoginActivity`.

Erstellen Sie eine Datei namens `LoginActivity.java`.

Sie müssen die Aktivität Initialisierung, und fügen Sie einige Schaltflächen, mit die die Benutzeroberfläche gesteuert werden. Dies ist Ihnen vertraut, wenn Sie Android Code geschrieben haben.

```
import android.app.Activity;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;

/**
 * Created by brwerner on 9/17/15.
 */
public class LoginActivity extends Activity {

    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_signin);
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        Button button = (Button) findViewById(R.id.signin_facebook);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.FB_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signin_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNIN_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signup_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNUP_POLICY);
                startActivity(intent);

            }
        });

    }

}


```
Haben Sie jetzt Schaltflächen, mit denen Anrufen erstellt Ihre `ToDoActivity` Absicht (das ADAL ruft Sie, wann ein Token). Diese Aktion mithilfe Ihrer Aktivitäten fest als ein Bezug und einen zusätzlichen Parameter. Diese zusätzlichen Parameter übergebener der `intent.putExtra()` Methode. Definieren Sie `"thePolicy"` mit Angabe in `Constants.java`. Dies weist der Absicht, welche Richtlinie während der Authentifizierung aufzurufen.

## <a name="create-the-settings-activity"></a>Erstellen Sie die Einstellungen Aktivität

Dies ist eine Aktivität, die Ihre Einstellungen Benutzeroberfläche auffüllt.

Erstellen Sie eine Datei namens `SettingsActivity.java` für einfache erstellen, lesen, aktualisieren und löschen Sie Vorgänge.

```
 package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.Switch;
import android.widget.TextView;

import com.microsoft.aad.taskapplication.helpers.Constants;

/**
 * Settings page to try broker by options
 */
public class SettingsActivity extends Activity {

    //private CheckBox checkboxAskBroker, checkboxCheckBroker;
    private Switch fullScreenSwitch;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_settings);

        loadSettings();
//      checkboxAskBroker = (CheckBox) findViewById(R.id.askInstall);
//      checkboxCheckBroker = (CheckBox) findViewById(R.id.useBroker);

        Button save = (Button) findViewById(R.id.settingsSave);

        save.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView textView = (TextView)findViewById(R.id.authority);
                Constants.AUTHORITY_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.resource);
            //    Constants.SCOPES = textView.getText().toString();

                textView = (TextView)findViewById(R.id.clientId);
                Constants.CLIENT_ID = textView.getText().toString();

                textView = (TextView)findViewById(R.id.extraQueryParameters);
                Constants.EXTRA_QP = textView.getText().toString();

                textView = (TextView)findViewById(R.id.redirectUri);
                Constants.REDIRECT_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                Constants.SERVICE_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                textView.setText(Constants.SERVICE_URL);

                textView = (TextView)findViewById(R.id.fb_signin);
                textView.setText(Constants.FB_POLICY);

                textView = (TextView)findViewById(R.id.email_signin);
                textView.setText(Constants.EMAIL_SIGNIN_POLICY);

                textView = (TextView)findViewById(R.id.email_signup);
                textView.setText(Constants.EMAIL_SIGNUP_POLICY);

                textView = (TextView)findViewById(R.id.correlationId);
                textView.setText(Constants.CORRELATION_ID);

                fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
                Constants.FULL_SCREEN = fullScreenSwitch.isChecked();

                finish();
            }
        });


    }

    private void loadSettings() {
        TextView textView = (TextView)findViewById(R.id.authority);
        textView.setText(Constants.AUTHORITY_URL);
        textView = (TextView)findViewById(R.id.resource);
        textView.setText(Constants.SCOPES[0]);
        textView = (TextView)findViewById(R.id.clientId);
        textView.setText(Constants.CLIENT_ID);
        textView = (TextView)findViewById(R.id.extraQueryParameters);
        textView.setText(Constants.EXTRA_QP);
        textView = (TextView)findViewById(R.id.redirectUri);
        textView.setText(Constants.REDIRECT_URL);
        textView = (TextView)findViewById(R.id.serviceUrl);
        textView.setText(Constants.SERVICE_URL);
        textView = (TextView)findViewById(R.id.fb_signin);
        textView.setText(Constants.FB_POLICY);
        textView = (TextView)findViewById(R.id.email_signin);
        textView.setText(Constants.EMAIL_SIGNIN_POLICY);
        textView = (TextView)findViewById(R.id.email_signup);
        textView.setText(Constants.EMAIL_SIGNUP_POLICY);
        textView = (TextView)findViewById(R.id.correlationId);
        textView.setText(Constants.CORRELATION_ID);

        fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
        fullScreenSwitch.setChecked(Constants.FULL_SCREEN);
    }

    private void saveSettings(String key, boolean value) {
        SharedPreferences prefs = SettingsActivity.this.getSharedPreferences(
                Constants.SHARED_PREFERENCE_NAME, Activity.MODE_PRIVATE);
        Editor prefsEditor = prefs.edit();
        prefsEditor.putBoolean(key, value);
        prefsEditor.commit();
    }
}
```

## <a name="create-the-add-task-activity"></a>Erstellen der Aufgabe hinzufügen Aktivität

Sie können diese Aktivität zum Hinzufügen einer Aufgabe an Ihrem Endpunkt REST-API verwenden.

Erstellen Sie eine Datei namens `AddTaskActivity.java` und schreiben vor.

```
package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;

public class AddTaskActivity extends Activity {

    EditText textField;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add_task);
        textField = (EditText) findViewById(R.id.taskToAdd);
        Button button = (Button) findViewById(R.id.postTaskbutton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (textField.getText().toString() != null
                        && !textField.getText().toString().trim().isEmpty()
                        && Constants.CURRENT_RESULT != null) {

                    TodoListHttpService service = new TodoListHttpService();
                    try {
                        service.addItem(textField.getText().toString(), Constants.CURRENT_RESULT.getAccessToken());
                    } catch (Exception e) {
                        SimpleAlertDialog.showAlertDialog(getApplicationContext(), "Exception caught", e.getMessage());
                    }
                    finish();
                }
            }
        });
    }

}

```

## <a name="create-the-to-do-list-activity"></a>Erstellen Sie die Aufgabe Liste an

Dies ist der wichtigste Aktivität. Sie können einsetzen, er ein Token von Azure AD für eine Richtlinie zu gelangen, und verwenden Sie dann die Token den Aufgabe REST-API Server aufrufen.

Erstellen Sie eine Datei namens `ToDoActivity.java` und schreiben vor. (Anrufe werden später erläutert.)

```

package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.app.AlertDialog;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.view.Window;
import android.webkit.CookieManager;
import android.webkit.CookieSyncManager;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationCallback;
import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.adal.AuthenticationResult;
import com.microsoft.aad.adal.AuthenticationSettings;
import com.microsoft.aad.adal.UserIdentifier;
import com.microsoft.aad.adal.PromptBehavior;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.InMemoryCacheStore;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;
import com.microsoft.aad.taskapplication.helpers.Utils;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;


import java.io.UnsupportedEncodingException;
import java.net.MalformedURLException;
import java.net.URL;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class ToDoActivity extends Activity {



    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;
    private static AuthenticationResult sResult;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;


    /**
     * Initializes the activity
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_todo_items);
        CookieSyncManager.createInstance(getApplicationContext());
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        // Clear previous sessions
        clearSessionCookie();
        try {
            // Provide key info for Encryption
            if (Build.VERSION.SDK_INT < 18) {
                Utils.setupKeyForSample();
            }

        Button button = (Button) findViewById(R.id.addTaskButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, AddTaskActivity.class);
                startActivity(intent);
            }
        });

        button = (Button) findViewById(R.id.appSettingsButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, SettingsActivity.class);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.switchUserButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, LoginActivity.class);
                startActivity(intent);

            }
        });


        final TextView name = (TextView)findViewById(R.id.userLoggedIn);


        mLoginProgressDialog = new ProgressDialog(this);
        mLoginProgressDialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
        mLoginProgressDialog.setMessage("Login in progress...");
        mLoginProgressDialog.show();
        // Ask for token and provide callback
        try {
            mAuthContext = new AuthenticationContext(ToDoActivity.this, Constants.AUTHORITY_URL,
                    false);
            String policy = getIntent().getStringExtra("thePolicy");

            if(Constants.CORRELATION_ID != null &&
                    Constants.CORRELATION_ID.trim().length() !=0){
                mAuthContext.setRequestCorrelationId(UUID.fromString(Constants.CORRELATION_ID));
            }

            AuthenticationSettings.INSTANCE.setSkipBroker(true);

            mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES, policy, Constants.CLIENT_ID,
                    Constants.REDIRECT_URL, getUserInfo(), PromptBehavior.Always,
                    "nux=1&" + Constants.EXTRA_QP,
                    new AuthenticationCallback<AuthenticationResult>() {

                        @Override
                        public void onError(Exception exc) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }
                            SimpleAlertDialog.showAlertDialog(ToDoActivity.this,
                                    "Failed to get token", exc.getMessage());
                        }

                        @Override
                        public void onSuccess(AuthenticationResult result) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }

                            if (result != null && !result.getToken().isEmpty()) {
                                setLocalToken(result);
                                updateLoggedInUser();
                                getTasks();
                                ToDoActivity.sResult = result;
                                Toast.makeText(getApplicationContext(), "Token is returned", Toast.LENGTH_SHORT)
                                        .show();

                                if (sResult.getUserInfo() != null) {
                                    name.setText(result.getUserInfo().getDisplayableId());
                                    Toast.makeText(getApplicationContext(),
                                            "User:" + sResult.getUserInfo().getDisplayableId(), Toast.LENGTH_SHORT)
                                            .show();
                                }
                            } else {
                                //TODO: popup error alert
                            }
                        }
                    });
        } catch (Exception e) {
            SimpleAlertDialog.showAlertDialog(ToDoActivity.this, "Exception caught", e.getMessage());
        }
        Toast.makeText(ToDoActivity.this, TAG + "done", Toast.LENGTH_SHORT).show();
    } catch (InvalidKeySpecException e) {
            e.printStackTrace();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }}

```


 Möglicherweise haben Sie bemerkt, dass diese Methoden abhängt, die noch nicht geschehen ist geschrieben wurde. Sie umfassen `updateLoggedInUser()`, `clearSessionCookie()`, und `getTasks()`. Schreiben Sie die weiter unten in diesem Handbuch. Jetzt können Sie den Fehler in Android Studio ignorieren.

Erläuterung der Parameter:

  - `SCOPES`: Erforderlich, die Bereiche anfordern des Zugriffs auf werden soll. Für die Vorschau B2C Dies ist identisch mit `client_id`, aber dies zu erwarten ist in der Zukunft ändern.
  - `POLICY`: Die Richtlinie für beim Authentifizieren des Benutzers werden soll.
  - `CLIENT_ID`: Erforderlich, stammen aus dem Azure AD-Portal.
  - `redirectUri`: Kann als Ihren Paketnamen eingerichtet werden. Es ist nicht erforderlich, um zur Verfügung gestellt werden, für die `acquireToken` anrufen.
  - `getUserInfo()`: Die Möglichkeit zum Nachschlagen, ob ein Benutzer bereits im Cache vorhanden ist. Der Parameter auch beschrieben, wie einen Benutzer auffordern, wenn der Benutzer nicht gefunden wird oder wenn Access-Token des Benutzers ungültig ist. Diese Methode wird weiter unten in diesem Handbuch geschrieben werden.
  - `PromptBehavior.always`: Hilft zur Eingabe von Anmeldeinformationen ein, den Cache und Cookie überspringen aufgefordert.
  - `Callback`: Aufgerufen, nachdem ein Autorisierungscode für ein Token ausgetauscht werden. Sie haben ein Objekt `AuthenticationResult`, der Zugriffstoken, Ablaufdatum und ID token Informationen enthält.

> [AZURE.NOTE]  Die Microsoft Intune Unternehmen Portal app bietet die Bank-Komponente, und es möglicherweise auf dem Gerät des Benutzers installiert. Die app bietet einen einzelnen einmaliges Anmelden (SSO) Zugriff über alle Programme auf dem Gerät. Entwickler sollten kann begonnen Intune zulässig ist. ADAL für Android wird das Konto Bank verwendet, es ist ein Benutzerkonto in der Authentifizierung erstellt. Wenn der Makler verwenden möchten, muss der Entwickler eine spezielle registrieren `redirectUri` für der Makler verwenden. `redirectUri`befindet sich das Format der msauth://packagename/Base64UrlencodedSignature. Sie können abrufen `redirectUri` für Ihre app mithilfe des Skripts `brokerRedirectPrint.ps1` oder mithilfe des Anrufs API `mContext.getBrokerRedirectUri()`. Die Signatur bezieht sich auf Ihre signierenden Zertifikate aus dem Google Play Store.

 Sie können mithilfe den Benutzer Bank überspringen:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```
> [AZURE.NOTE] Die Komplexität der dieser B2C Schnellstart zu reduzieren, entschieden wir haben der Makler in unserem Beispiel überspringen.

Erstellen Sie anschließend Helper Methoden, mit die das Token während der Authentifizierung Anrufe, die dem Vorgang API alleine abrufen.

In der gleichen `ToDoActivity.java` ablegen, Schreiben Sie vor.

```
    private void getToken(final AuthenticationCallback callback) {

        String policy = getIntent().getStringExtra("thePolicy");

        // one of the acquireToken overloads
        mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES,
                policy, Constants.CLIENT_ID, Constants.REDIRECT_URL, getUserInfo(),
                PromptBehavior.Always, "nux=1&" + Constants.EXTRA_QP, callback);
    }
```

Fügen Sie auch Methoden, die "festlegen" und "abrufen" werden `AuthenticationResult` (die hat Ihr Tokens) der globalen Vorlage `Constants`. Obwohl `ToDoActivity.java` verwendet `sResult` in deren Zahlungen, müssen Sie diese Methoden hinzufügen. Wenn Sie nicht möchten, nicht Ihrer anderen Aktivitäten müssen Zugriff auf das Token Arbeit zugewiesen wurde (wie etwa das Hinzufügen eines Vorgangs in `AddTaskActivity.java`).

```

    private AuthenticationResult getLocalToken() {
        return Constants.CURRENT_RESULT;
    }

    private void setLocalToken(AuthenticationResult newToken) {


        Constants.CURRENT_RESULT = newToken;
    }


```
## <a name="create-a-method-to-return-a-user-identifier"></a>Erstellen Sie eine Methode, um eine Benutzer-ID zurückzukehren.

ADAL für Android steht für den Benutzer in Form von einer `UserIdentifier` Objekt. Dieses Objekt verwaltet den Benutzer. Das Objekt können Sie feststellen, ob es sich um denselben Benutzer in Ihre Anrufe verwendet wird. Mithilfe dieser Informationen können Sie abhängig von dem Cache, anstatt neue Anrufen auf dem Server. Um dies zu erleichtern, wir erstellt die `getUserInfo()` Methode, zurückgibt `UserIdentifier`. Hiermit können Sie mit `acquireToken()`. Wir auch erstellt eine `getUniqueId()` Methode, die die ID des gibt `UserIdentifier` im Cache.

```
  private String getUniqueId() {
        if (sResult != null && sResult.getUserInfo() != null
                && sResult.getUserInfo().getUniqueId() != null) {
            return sResult.getUserInfo().getUniqueId();
        }

        return null;
    }

    private UserIdentifier getUserInfo() {

        final TextView names = (TextView)findViewById(R.id.userLoggedIn);
        String name = names.getText().toString();
        return new UserIdentifier(name, UserIdentifier.UserIdentifierType.OptionalDisplayableId);
    }

```

### <a name="write-helper-methods"></a>Schreiben Sie Helper Methoden

Schreiben Sie einige Helper Methoden, mit denen Sie Cookies deaktivieren, und geben Sie als Nächstes `AuthenticationCallback`. Diese Methoden werden für die Stichprobe rein verwendet, um sicherzustellen, dass Sie in einem übersichtlichen Zustand sind, wenn Sie anrufen, Ihre `ToDo` Aktivität.

In der gleichen Datei namens `ToDoActivity.java`, Schreiben Sie vor.

```

    private void clearSessionCookie() {

        CookieManager cookieManager = CookieManager.getInstance();
        cookieManager.removeSessionCookie();
        CookieSyncManager.getInstance().sync();
    }
```

```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        mAuthContext.onActivityResult(requestCode, resultCode, data);
    }

```   

## <a name="call-the-task-api"></a>Rufen Sie die Aufgabe API

Nachdem Sie Ihre Aktivität bereit zum Extrahieren von Token haben, können Sie Ihre API Zugriff auf den Server Aufgabe schreiben.

`getTasks`Stellt ein Array, die Vorgänge in Ihrem Server darstellt.

Beginnen Sie mit `getTasks`.

In der gleichen Datei namens `ToDoActivity.java`, Schreiben Sie vor.

```
    private void getTasks() {
        if (sResult == null || sResult.getToken().isEmpty())
            return;

        List<String> items = new ArrayList<>();
        try {
            items = new TodoListHttpService().getAllItems(sResult.getToken());
        } catch (Exception e) {
            items = new ArrayList<>();
        }

        ListView listview = (ListView) findViewById(R.id.listViewToDo);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_list_item_1, android.R.id.text1, items);
        listview.setAdapter(adapter);
    }

```

Schreiben Sie auch eine Methode, die die Tabellen bei der ersten Ausführung Initialisierung wird.

In der gleichen Datei namens `ToDoActivity.java`, Schreiben Sie vor.

```
    private void initAppTables() {
        try {
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

        } catch (Exception e) {
            createAndShowDialog(new Exception(
                    "There was an error creating the Mobile Service. Verify the URL"), "Error");
        }
    }

```

Sie können sehen, dass diese Code einige zusätzlichen Methoden seine Arbeit erledigen erfordert. Schreiben Sie diese weiter.

### <a name="create-an-endpoint-url-generator"></a>Erstellen Sie einen Endpunkt URL-generator

Sie müssen die Endpunkt-URL zu erzeugen, die Sie eine Verbindung herstellen können. Führen Sie die in die gleiche Klassendatei aus.

**In der gleichen Datei** namens `ToDoActivity.java`, Schreiben Sie vor.

 ```
    private URL getEndpointUrl() {
        URL endpoint = null;
        try {
            endpoint = new URL(Constants.SERVICE_URL);
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        return endpoint;
    }

 ```


Beachten Sie, dass Sie die Anfrage in den Code im nächsten Abschnitt erläutert das Access-Token hinzufügen.

## <a name="write-some-ux-methods"></a>Einige Methoden UX schreiben

Android müssen Sie einige Rückrufe, um die Steuerung der app zu behandeln. Hierbei handelt es sich `createAndShowDialog` und `onResume()`. Dies ist Ihnen vertraut, wenn Sie Android Code geschrieben haben.

In der gleichen Datei namens `ToDoActivity.java`, Schreiben Sie vor.

```
    @Override
    public void onResume() {
        super.onResume(); // Always call the superclass method first

        updateLoggedInUser();
        // User can click logout, it will come back here
        // It should refresh list again
        getTasks();
    }


```

Als Nächstes verwalten Sie Ihrer Rückrufe Dialogfeld ein.

In der gleichen Datei namens `ToDoActivity.java`, Schreiben Sie vor.

```
    /**
     * Creates a dialog and shows it
     *
     * @param exception The exception to show in the dialog
     * @param title     The dialog title
     */
    private void createAndShowDialog(Exception exception, String title) {
        createAndShowDialog(exception.toString(), title);
    }

    /**
     * Creates a dialog and shows it
     *
     * @param message The dialog message
     * @param title   The dialog title
     */
    private void createAndShowDialog(String message, String title) {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setMessage(message);
        builder.setTitle(title);
        builder.create().show();
    }

```

Sie sollten jetzt haben eine `ToDoActivity.java` Datei, die kompiliert wird. Das gesamte Projekt sollte an dieser Stelle auch kompilieren.

## <a name="run-the-sample-app"></a>Führen Sie die Beispielapp

Schließlich erstellen Sie, und führen Sie die app in Android Studio oder "Ellipse". Registrieren Sie, oder melden Sie sich die app. Erstellen von Aufgaben für den Benutzer angemeldet. Melden Sie sich ab und wieder anmelden, als ein anderer Benutzer aus, und klicken Sie dann erstellen Aufgaben für diesen Benutzer.

Beachten Sie, dass die Aufgaben gespeicherten pro Benutzer zur-API, da die API die Identität des Benutzers aus dem Access-Token extrahiert empfangenen.

Als Referenz der vollständigen Beispiel [als ZIP-Datei bereitgestellt wird](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip). Sie können auch aus GitHub Klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-Android```


## <a name="important-information"></a>Wichtige Informationen


### <a name="encryption"></a>Verschlüsselung

ADAL verschlüsselt die Token und Store in `SharedPreferences` standardmäßig. Sie können prüfen der `StorageHelper` Klasse, um die Details anzuzeigen. Android eingeführt **AndroidKeyStore für 4.3(API18)** sicheren Speicher privater Schlüssel. ADAL verwendet, die für API18 und höher. Wenn ADAL für unteren SDK-Versionen verwendet werden soll, müssen Sie einen geheimen Schlüssel am bieten `AuthenticationSettings.INSTANCE.setSecretKey`.

### <a name="session-cookies-in-web-view"></a>Sitzungscookies im Web anzeigen

Android Webanzeige wird nicht Sitzungscookies gelöscht, nachdem die app geschlossen ist. Sie können dies mit dem folgenden Code behandeln.
```
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
[Erfahren Sie mehr über Cookies](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).
