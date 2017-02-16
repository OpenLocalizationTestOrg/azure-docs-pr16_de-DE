<properties
    pageTitle="Erste Schritte Azure AD-Android | Microsoft Azure"
    description="So erstellen Sie eine Android-Anwendung, die für die Anmeldung Azure AD integriert und Azure AD-Anrufe geschützt APIs OAuth verwenden."
    services="active-directory"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-android-app"></a>Integrieren von Azure AD in einer Android-App

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Wenn Sie eine desktop-Anwendung entwickeln, macht Azure AD einfach und unkompliziert für Sie Ihre Benutzer authentifizieren anhand ihrer Active Directory-Benutzerkonten.  Darüber hinaus können die Anwendung zu einem beliebigen Web-API von Azure AD, wie die Office 365-APIs oder der Azure-API geschützten sicher nutzen.

Für Android-Clients, die Zugriff auf geschützte Ressourcen benötigen, bietet Azure AD an den Active Directory-Authentifizierungsbibliothek oder ADAL.  ADAL des alleinige Zweck im Leben ist für Ihre app abzurufenden Zugriffstoken zu erleichtern.  Um zu veranschaulichen, wie einfach es ist, wird hier wir einer Aufgabenliste Android-Anwendung, die erstellen:

-   Ruft Token für das Anrufen von einer Aufgabe Liste-API mit dem [OAuth 2.0-Authentifizierung-Protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx)zugegriffen werden.
-   Ruft Aufgabenliste eines Benutzers
-   Vorzeichen Benutzer ab.

Um anzufangen, benötigen Sie eine Azure AD-Mandanten, in dem Sie Benutzer erstellen und Registrieren einer Anwendung.  Wenn Sie noch keinen Mandanten, [erfahren Sie, wie Sie eins zu erhalten](active-directory-howto-tenant.md)haben.

> [AZURE.TIP] Probieren Sie die Vorschau des unserer neuen [Entwicklerportal](https://identity.microsoft.com/Docs/Android) , die Ihnen den Einstieg und bei Azure Active Directory in wenigen Minuten erhalten helfen!  Die Entwicklerportal führt Sie durch das Verfahren zum Registrieren einer app und Azure AD in Ihren Code zu integrieren.  Wenn Sie fertig sind, haben Sie eine einfache Anwendung, die Benutzer in Ihrem Mandanten und einer Back-End-authentifizieren können, die Token akzeptieren und Validierung ausführen können. 

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Schritt 1: Herunterladen und Ausführen von Node.js REST-API erledigen Sample Server

In diesem Beispiel wird speziell entwickelt gegen unsere vorhandene Beispiel für das Erstellen eines einzelnen Mandanten Aufgabe REST-API für Microsoft Azure Active Directory geschrieben. Dies ist eine Vorbedingung für den Schnellstart.

Informationen zur Einrichtung dieser finden Sie auf unseren vorhandenen Beispielen hier:

* [Microsoft Azure Active Directory Stichprobe REST API-Dienst für Node.js](active-directory-devquickstarts-webapi-nodejs.md)

## <a name="step-2-register-your-web-api-with-your-microsoft-azure-ad-tenant"></a>Schritt 2: Registrieren des Webs-API mit Ihrem Microsoft Azure AD-Mandanten

**Was mache ich?**

*Microsoft Active Directory unterstützt das Hinzufügen von zwei Arten von Applications. Web-APIs, die Dienste anbieten von Benutzern und Applikationen (entweder im Web oder einer Anwendung auf einem Gerät), die diese Web-APIs zugreifen. In diesem Schritt werden Sie die Web-API registrieren, die lokal für dieses Beispiel testen ausgeführt werden. Normalerweise wäre diese Web-API REST-Dienst, der Funktionalität anbietet soll eine app für den Zugriff auf. Microsoft Azure Active Directory können alle Endpunkt schützen!*

*Hier wird davon ausgegangen, die erledigen REST-API weiter oben erwähnten registriert, aber dies funktioniert für alle Web-API gewünschte Azure Active Directory schützen möchten.*

Schritte zum Registrieren einer Web-API mit Microsoft Azure AD

1. Melden Sie sich mit dem [Azure-Verwaltungsportal](https://manage.windowsazure.com).
2. Klicken Sie in der linken NAV auf Active Directory
3. Klicken Sie auf den Directory Mandanten, in dem die Stichprobe Anwendung registriert werden soll.
4. Klicken Sie auf die Registerkarte Applications.
5. Klicken Sie in den Einzug auf Hinzufügen.
6. Klicken Sie auf "Eine Anwendung, die zur Entwicklung von meinem Unternehmen hinzufügen".
7. Geben Sie einen Anzeigenamen für die Anwendung, beispielsweise "TodoListService", wählen Sie "Web Anwendung und/oder Web API", und klicken Sie auf Weiter.
8. Für die URL anmelden, geben Sie die base-URL für das Beispiel standardmäßig ist `https://localhost:8080`.
9. Geben Sie für den App-ID-URI `https://<your_tenant_name>/TodoListService`, und Ersetzen `<your_tenant_name>` mit dem Namen der Azure AD-Mandanten.  Klicken Sie auf OK, um die Registrierung abzuschließen.
10. Klicken Sie auf die Registerkarte Konfigurieren der Anwendung, weiterhin im Azure-Portal.
11. **Suchen Sie den Wert für die Client-ID und reservieren kopieren**, werden Sie diese später benötigen, beim Konfigurieren Ihrer Anwendungs.

## <a name="step-3-register-the-sample-android-native-client-application"></a>Schritt 3: Registrieren der Stichprobe Android Native Client-Anwendungs

Registrieren der Web-Anwendungs ist der erste Schritt. Als Nächstes müssen Sie Azure Active Directory über auch Ihrer Anwendung zu informieren. Dadurch wird eine Anwendung zur Kommunikation mit der nur registrierten Web-API

**Was mache ich?**  

*Wie bereits erwähnt, unterstützt Microsoft Azure Active Directory Hinzufügen von zwei Arten von Applications. Web-APIs, die Dienste anbieten von Benutzern und Applikationen (entweder im Web oder einer Anwendung auf einem Gerät), die diese Web-APIs zugreifen. In diesem Schritt werden Sie die Anwendung in diesem Beispiel registrieren. Sie müssen, damit diese Anwendung kann Anforderung auf die Web-API zugreifen, ob Sie nur registriert vornehmen. Azure-Active Directory wird ablehnen, zu denen auch die Anwendung für Anmeldung bitten, es sei denn, es registriert ist! Dies ist Teil der Sicherheit des Modells aus.*

*Hier wird davon ausgegangen, diese Anwendung weiter oben erwähnten registriert, aber dies funktioniert für eine beliebige app, die Sie entwickeln.*

**Warum bin ich eine Anwendung und eine Web-API in einen Mandanten einfügen?**

*Wenn Sie vermuten, könnten Sie eine app erstellen, die eine externe API greift auf, die von einem anderen Mandanten in Azure Active Directory registriert ist. Wenn Sie dies tun, werden Ihre Kunden aufgefordert, auf die Nutzung der API in der Anwendung Zustimmung. Die übersichtliche gehört, Active Directory-Authentifizierungsbibliothek für iOS sorgt für diese Zustimmung für Sie! Wir erweiterte Features in zu gelangen, werden Sie sehen, dass dies ein wichtiger die erforderliche Arbeit zum Azure und Office sowie alle anderen Anbieter die Suite der Microsoft-APIs zugreifen ist. Jetzt da Sie Ihre Web-API und die Anwendung unter den gleichen Mandanten registriert wird eine Aufforderung zur Bestätigung auf nicht angezeigt. Dies ist normalerweise der Fall, wenn Sie eine Anwendung nur für Ihr eigenes Unternehmen verwenden entwickeln.*

1. Melden Sie sich mit dem [Azure-Verwaltungsportal](https://manage.windowsazure.com).
2. Klicken Sie in der linken NAV auf Active Directory
3. Klicken Sie auf den Directory Mandanten, in dem die Stichprobe Anwendung registriert werden soll.
4. Klicken Sie auf die Registerkarte Applications.
5. Klicken Sie in den Einzug auf Hinzufügen.
6. Klicken Sie auf "Eine Anwendung, die zur Entwicklung von meinem Unternehmen hinzufügen".
7. Geben Sie einen Anzeigenamen für die Anwendung, beispielsweise "TodoListClient kein Android", auswählen "Native Client-Anwendung", und klicken Sie auf Weiter.
8. Geben Sie für den URI umleiten `http://TodoListClient`.  Klicken Sie auf Fertig stellen.
9. Klicken Sie auf die Registerkarte Konfigurieren der Anwendung.
10. Suchen Sie den Wert für die Client-ID, und kopieren Sie ihn reservieren, Sie benötigen diese später beim Konfigurieren Ihrer Anwendungs.
11. Klicken Sie in "Berechtigungen zu anderen Applications" auf "Anwendung hinzufügen".  Wählen Sie "Andere" in der Dropdownliste "Anzeigen", und klicken Sie auf der oberen Häkchen.  Suchen Sie, und klicken Sie auf die TodoListService, und klicken Sie auf das Häkchen unten, um die Anwendung hinzuzufügen.  Wählen Sie "Access TodoListService" aus der Dropdownliste "Delegierte Berechtigungen" aus, und speichern Sie die Konfiguration.



Um Maven zu erstellen, können Sie die pom.xml auf obersten Ebene verwenden.

  * Klonen dieses Repo in in ein Verzeichnis Ihrer Wahl an:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  

  * Führen Sie die Schritte bei [Prerequests Abschnitt aus, um die Einrichtung Ihrer Maven für android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)
  * Emulator mit 19 SDK Setup
  * Wechseln Sie zu der Stelle, an der Sie die Repo geklont Stammordner
  * Führen Sie den Befehl: Säubern Mvn installieren
  * Wechseln Sie in der Stichprobe Schnellstart: cd Samples\hello
  * Führen Sie den Befehl: Mvn android: Bereitstellen von Android ausführen:
  * App starten auftreten Sie
  * Geben Sie Anmeldeinformationen des Testbenutzers versuchen Sie es aus!

JAR-Paketen werden auch neben das Paket Aar übermittelt.

### <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Schritt 4: Android ADAL herunterladen und Lizenz zu dem Arbeitsbereich "Ellipse"

Wir haben sie einfache haben mehrere Möglichkeiten zum Verwenden dieser Bibliothek im Projekt Android vorgenommen:

* Den Quellcode können diese Bibliothek in "Ellipse" und Link an Ihrer Anwendung zu importieren.
* Wenn Android Studio verwenden, können Sie *Aar* Paketformat verwenden und die Binärdateien verweisen.

####<a name="option-1-source-zip"></a>Option 1: Quelle Zip

Wenn Sie eine Kopie des Quellcodes herunterladen möchten, klicken Sie auf der rechten Seite der Seite auf "Herunterladen eines ZIP", oder klicken Sie auf [hier](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

####<a name="option-2-source-via-git"></a>Option 2: Quelle über Git

Zum Abrufen der Quellcode des SDK über Git einfach Folgendes ein:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

####<a name="option-3-binaries-via-gradle"></a>Option 3: Binärdateien über Gradle

Sie können die Binärdateien von zentralen Repo Maven erhalten. AAR Paket kann wie folgt in einem Projekt in AndroidStudio enthalten sein:

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
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

####<a name="option-4-aar-via-maven"></a>Option 4: Aar über Maven

Wenn Sie das m2e-Plug-in "Ellipse" verwenden, können Sie die Beziehung in der Datei pom.xml angeben:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


####<a name="option-5-jar-package-inside-libs-folder"></a>Option 5: JAR-Paket im Ordner Bibliotheken
Können Sie die JAR-Datei aus Maven die Repo abrufen und Ablegen in *Bibliotheken* Ordner in Ihrem Projekt. Sie müssen die erforderlichen Ressourcen zu Ihrem Projekt als auch kopieren, da die JAR-Paketen diese nicht enthalten.


### <a name="step-5-add-references-to-android-adal-to-your-project"></a>Schritt 5: Hinzufügen von Verweise auf Android ADAL zu einem Projekt


2. Fügen Sie einen Verweis zu einem Projekt, und geben Sie es als eine Android-Bibliothek. Wenn Sie unsicher sind, wie Sie dazu [Klicken Sie hier weitere Informationen] (http://developer.android.com/tools/projects/projects-eclipse.html)

3. Hinzufügen der Abhängigkeits Projekt für das Debuggen zu Ihren Project-Einstellungen

4. Aktualisieren Sie Ihres Projekts AndroidManifest.xml Datei aufnehmen möchten:

    ```Java
      <uses-permission android:name="android.permission.INTERNET" />
      <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
      <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
      ....
      <application/>
    ```

7. Erstellen Sie eine Instanz der AuthenticationContext bei Hauptfenster Ihrer Aktivitäten fest. Die Details dieses Anruf befinden sich außerhalb des Gültigkeitsbereichs von dieser Infodatei, aber Sie erhalten einen guten Start, indem Sie die [Android Native Client Stichprobe](https://github.com/AzureADSamples/NativeClient-Android)betrachtet. Es folgt ein Beispiel:

    ```Java
    // Authority is in the form of https://login.windows.net/yourtenant.onmicrosoft.com
    mContext = new AuthenticationContext(MainActivity.this, authority, true); // This will use SharedPreferences as            default cache
    ```
  * Hinweis: mContext ist ein Feld in Ihrer Aktivitäten fest

8. Kopieren Sie diesen Codeblock zum Ende des AuthenticationActivity nach der Benutzer gibt Anmeldeinformationen und erhält Autorisierungscode zu behandeln:

    ```Java
     @Override
     protected void onActivityResult(int requestCode, int resultCode, Intent data) {
         super.onActivityResult(requestCode, resultCode, data);
         if (mContext != null) {
             mContext.onActivityResult(requestCode, resultCode, data);
         }
     }
    ```

9. Um ein Token zu bitten definieren Sie einen Rückruf

    ```Java
    private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };
    ```
10. Überzeugen Sie sich für ein Token mithilfe dieser Rückruf:

    ```Java
     mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                    callback);
    ```

Erläuterung der Parameter:

  * Ressource ist erforderlich, und ist die Ressource, die Sie für den Zugriff auf.
  * ClientID erforderlich ist und im AzureAD Portal stammt.
  * Sie können RedirectUri als Ihre Packagename einrichten. Es ist nicht erforderlich, für den Anruf AcquireToken angegeben werden.
  * PromptBehavior können Sie zur Eingabe von Anmeldeinformationen Cache und Cookie überspringen aufgefordert.
  * Rückruf wird aufgerufen werden, nachdem die Autorisierungscode für ein Token ausgetauscht werden.

  Der Rückruf haben, wird ein Objekt des AuthenticationResult die Accesstoken hat, Datum abgelaufen, und Idtoken Informationen.

Optional: **acquireTokenSilent**

Sie können **AcquireTokenSilent** Zwischenspeichern verarbeitet und token aktualisieren aufrufen. Synchronisieren Sie ebenfalls diese Version darüber. Benutzer-ID zulässt als Parameter.

    ```java
     mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

11. **Bank**: Microsoft Intunes Unternehmens Portal app wird die Bank Komponente bereitstellen. Verwenden, wenn das Konto Bank, es ist ein Benutzerkonto bei dieser Authentifizierung und Entwicklertools erstellt wird wird ADAL nicht, um ihn zu überspringen auswählen. Entwickler kann den Benutzer Bank mit überspringen:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```

 Entwickler muss spezielle RedirectUri für Bank Verwendung zu registrieren. RedirectUri ist in das Format des msauth://packagename/Base64UrlencodedSignature. Sie können Ihre Redirecturi für Ihre app verwenden das Skript "brokerRedirectPrint.ps1" erhalten oder verwenden API Anruf mContext.getBrokerRedirectUri. Signatur bezieht sich auf Ihre signierenden Zertifikate.

 Aktuelle Bank-Modell ist für einen Benutzer. AuthenticationContext bietet-API-Methode, um den Benutzer Bank zu gelangen.

 ```java
 String brokerAccount =  mContext.getBrokerUser();
 ```
 Bank Benutzer wird zurückgegeben werden, wenn das Konto gültig ist.

 Ihre app-Manifest über die Berechtigungen verfügen, verwenden von Konten Onlinecodebeispiele: http://developer.android.com/reference/android/accounts/AccountManager.html

 * GET_ACCOUNTS
 * USE_CREDENTIALS
 * MANAGE_ACCOUNTS


Verwenden diese Anleitung, sollten Sie müssen erfolgreich mit Azure Active Directory integrieren verfügen. Weitere Beispiele für diese arbeiten, finden Sie auf der AzureADSamples / Repository auf GitHub.

## <a name="important-information"></a>Wichtige Informationen

### <a name="customization"></a>Anpassung

Bibliothek Projektressourcen können bei Anwendungsressourcen überschrieben werden. Dies geschieht, wenn Ihre app erstellt wird. Daher können Sie Authentifizierung Aktivität Layout an die Methode anpassen gewünschten. Sie müssen sicherstellen, dass die Id der Steuerelemente die ADAL uses(Webview) beibehalten.

### <a name="broker"></a>Bank

Bank Komponente wird mit Microsoft Intunes Unternehmens Portal app übermittelt werden. Konto wird im Account Manager erstellt. Kontotyp ist "com.microsoft.workaccount". Es kann nur einzelnes SSO-Konto. Nach Abschluss des Geräts Herausforderung für eine der apps wird SSO Cookie für diesen Benutzer erstellt.

### <a name="authority-url-and-adfs"></a>Zertifizierungsstelle Url und ADFS

ADFS wird nicht erkannt als Herstellung STS, daher müssen Sie der Instanz Discovery aktivieren, und geben Sie bei AuthenticationContext Konstruktor false.

Zertifizierungsstelle Url benötigt STS Instanz und Mandanten Namen: https://login.windows.net/yourtenant.onmicrosoft.com

### <a name="querying-cache-items"></a>Abfragen Cacheelemente

ADAL bietet Standard-Cache in SharedPreferences mit einige einfache Cache Funktionen der Abfrage an. Sie können den aktuellen Cache von AuthenticationContext mit erhalten:
```Java
 ITokenCacheStore cache = mContext.getCache();
```
Sie können auch die Implementierung Cache bereitstellen, wenn Sie sie anpassen möchten.
```Java
mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);
```

### <a name="promptbehavior"></a>PromptBehavior

ADAL die Möglichkeit, geben Sie einen auffordern Verhalten. Benutzeroberfläche wird PromptBehavior.Auto pop, wenn Aktualisierung Token ungültig ist und Benutzeranmeldeinformationen erforderlich sind. PromptBehavior.Always überspringt die Verwendung der Cache und Benutzeroberfläche immer anzeigen.

### <a name="silent-token-request-from-cache-and-refresh"></a>Automatische token Anforderung aus dem Cache und aktualisieren

Diese Methode verwendet keine Benutzeroberfläche Pop nach oben und keine Aktivität erforderlich. Es gibt token aus dem Cache gegebenenfalls zurück. Wenn Token abgelaufen ist, wird es versuchen Sie, diese aktualisieren. Wenn Aktualisierung Token abgelaufen oder fehlgeschlagen ist, wird AuthenticationException zurückgegeben.

    ```Java
    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

Sie können auch bei dieser Methode können anrufen synchronisieren vornehmen. Sie können festlegen auf Rückruf null oder AcquireTokenSilentSync verwenden.

### <a name="diagnostics"></a>Diagnose

Im folgenden sind die primären Quellen von Informationen für die Diagnose von Problemen:

+ Ausnahmen
+ Protokolle
+ Netzwerk-auf

Beachten Sie, dass Korrelations-IDs zentrale die Diagnose in der Bibliothek sind. Sie können festlegen, dass Ihre Korrelationskoeffizienten IDs für eines jeden Anforderung einer ADAL zu koordinieren soll mit anderen Vorgängen in Ihrem Code anfordern. Wenn Sie keine Id Korrelationen festlegen, und klicken Sie dann ADAL eine Zufallszahl, eine and alle generieren wird werden Nachrichten protokollieren und Netzwerk Anrufe mit der Korrelations-Id versehen. Die Self generiert Id Änderungen bei jeder Anforderung an.

#### <a name="exceptions"></a>Ausnahmen

Dies ist offensichtlich der ersten Diagnose. Wir versuchen, eine hilfreiche Fehlermeldungen bereitzustellen. Wenn Sie eine finden, die nicht hilfreich ist Bitte Ablegen eines Problems, und lassen Sie uns wissen. Geben Sie das Geräteinformationen wie Modell oder SDK Nr. auch aus.

#### <a name="logs"></a>Protokolle

Sie können die Bibliothek, um Nachrichten protokollieren zu generieren, die Sie verwenden können, Probleme beim Diagnostizieren konfigurieren. Konfigurieren der Protokollierung nach dem folgenden Aufrufen so konfigurieren Sie einen Rückruf, den ADAL zur hand deaktivieren jede Lognachricht verwendet werden, während sie erstellt werden.


 ```Java
 Logger.getInstance().setExternalLogger(new ILogger() {
     @Override
     public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
      ...
      // You can write this to logfile depending on level or errorcode.
      writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
     }
 }
 ```
Nachrichten können wie folgt in eine benutzerdefinierte Protokolldatei geschrieben werden. Leider gibt es Möglichkeit keine standard der erste Protokolle auf einem Gerät. Es gibt einige Dienste, die Ihnen dabei helfen können. Sie können auch eigene, z. B., senden die Datei auf einen Server festgelegter.

```Java
private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
}
```

##### <a name="logging-levels"></a>Protokollierungsebenen

+ Error(Exceptions)
+ Warn(Warning)
+ Info (Informationszwecke)
+ Ausführlich (Weitere Details)

Sie legen die Ebene wie folgt aus:
```Java
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);
 ```

 Alle Lognachrichten werden an Logcat zusätzlich keinerlei benutzerdefiniertes Protokoll liegen gesendet.
Sie können in einer Datei Formular Logcat protokollieren als angezeigte Belog erhalten:

 ```
  adb logcat > "C:\logmsg\logfile.txt"
 ```
 Wenn Sie weitere Beispiele zu Adb-Befehlen: https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat

#### <a name="network-traces"></a>Netzwerk-auf

Verschiedene Tools können Sie um den HTTP-Verkehr zu erfassen, den ADAL generiert.  Dies ist besonders hilfreich, wenn Sie das Protokoll OAuth vertraut sind oder wenn Sie Diagnoseinformationen an Microsoft oder anderen Supportkanäle bereitstellen müssen.

Fiddler ist die einfachste Tool zum Verfolgen von HTTP.  Verwenden Sie die folgenden Links, um ihn nach Zeitphasen bis zum richtig aufzeichnen ADAL Netzwerkverkehr setup.  Um nützlich sein, ist es erforderlich, vor Fiddler oder ein anderes Programm wie Charles, aufzeichnen unverschlüsselten SSL-Verkehr zu konfigurieren.  Hinweis: Spuren generiert auf diese Weise können umfangreiche Informationen, wie z. B. Access Token, Benutzernamen und Kennwörter enthalten.  Wenn Sie Herstellung Konten verwenden, geben Sie keinen dieser Spuren 3rd Parteien.  Wenn Sie eine Spur an eine andere Person angeben, um Unterstützung zu erhalten müssen, reproduzieren Sie das Problem, mit einem temporären Konto mit Benutzernamen und Kennwörter, die Sie freigeben nicht stört.

+ [Einrichten von Fiddler für Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
+ [Konfigurieren von Fiddler Regeln für ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)


### <a name="dialog-mode"></a>Dialogfeld Modus
AcquireToken Methode ohne Aktivität unterstützt das Dialogfeld auffordern.

### <a name="encryption"></a>Verschlüsselung

ADAL verschlüsselt die Token und Store in SharedPreferences standardmäßig an. Betrachten Sie die StorageHelper-Klasse, um die Details anzuzeigen. Android eingeführt AndroidKeyStore für 4.3(API18) sicheren Speicher privater Schlüssel. ADAL verwendet, die für API18 und höher. Wenn Sie ADAL für unteren SDK-Versionen verwenden möchten, müssen Sie geheimen Schlüssel am AuthenticationSettings.INSTANCE.setSecretKey bereitstellen

### <a name="oauth2-bearer-challenge"></a>Oauth2 Person zur Herausforderung

AuthenticationParameters Klasse enthält Funktionen, um die Authorization_uri von Oauth2 Person Herausforderung zu gelangen.

### <a name="session-cookies-in-webview"></a>Sitzungscookies in Webview

Android Webview deaktivieren Sitzungscookies nicht, nach dem Schließen der app. Sie können dies mit dem folgenden Beispielcode behandeln:
```java
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
Weitere Informationen zu Cookies: http://developer.android.com/reference/android/webkit/CookieSyncManager.html

### <a name="resource-overrides"></a>Ressource überschreibt

ADAL Bibliothek umfasst englische Zeichenfolgen für die beiden folgenden ProgressDialog Nachrichten.

Die Anwendung sollte gegebenenfalls von lokalisierte Zeichenfolgen überschrieben.

```Java
<string name="app_loading">Loading...</string>
<string name="broker_processing">Broker is processing</string>
<string name="http_auth_dialog_username">Username</string>
<string name="http_auth_dialog_password">Password</string>
<string name="http_auth_dialog_title">Sign In</string>
<string name="http_auth_dialog_login">Login</string>
<string name="http_auth_dialog_cancel">Cancel</string>
```

=======

### <a name="ntlm-dialog"></a>NTLM-Dialogfeld
Adal Version 1.1.0 unterstützt NTLM-Dialogfeld, in dem über OnReceivedHttpAuthRequest Ereignis aus WebViewClient verarbeitet wird. Layout des Dialogfelds und Zeichenfolgen angepasst werden können.

### <a name="cross-app-sso"></a>SSO Cross-app
Informationen Sie [zum Aktivieren von Cross-app SSO auf Android-Gerät mit ADAL](active-directory-sso-android.md)  


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
