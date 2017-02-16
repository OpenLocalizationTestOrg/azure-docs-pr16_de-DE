<properties
    pageTitle="Aktivieren des Cross-app SSO auf Android-Gerät mit ADAL | Microsoft Azure"
    description="Verwendung der Features des ADAL SDK Single Sign On über Ihre Programme aktivieren. "
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Aktivieren des Cross-app SSO auf ADAL mit Android-Gerät


Bereitstellen für einmaliges Anmelden (SSO), sodass die Benutzer müssen nur einmal geben ihre Anmeldeinformationen und diese Anmeldeinformationen automatisch arbeiten über Applications ist jetzt erwartet von Kunden. Die Probleme in ihren Benutzernamen und Ihr Kennwort eingeben, auf einem kleinen Bildschirm, oft Zeiten mit einen weiteren Faktor (2FA) wie einen Anruf oder einer texted Code, kombiniert ergibt Unzufriedenheit Symbolleiste, wenn ein Benutzer verfügt über Aktion mehrere Zeit für das Produkt.

Wenn Sie die eine Identitätsplattform, die möglicherweise anderen Programmen wie Microsoft Accounts oder ein Arbeit-Konto bei Office 365 verwenden nutzen, erwarten Kunden darüber hinaus, dass die Anmeldeinformationen für alle Webanwendungen unabhängig von den Hersteller verwenden verfügbar sein soll.

Die Microsoft Identity Plattform und unsere Microsoft-Identität SDK, werden alle diese harte Arbeit für Sie und bietet Ihnen die Möglichkeit, entweder über eigene Suite von Applications erfreuen Sie Ihre Kunden mit SSO Ihre oder, wie bei unseren Bank Videofunktionen und Authentifizierung Applications, über das gesamte Gerät.

Diese Anleitung erfahren Sie, wie unsere SDK innerhalb der Anwendung dieses Angebot an Ihre Kunden senden konfigurieren.

Diese Anleitung gilt für:

* Azure-Active Directory
* B2C Azure-Active Directory
* B2B Azure-Active Directory
* Bedingte Azure-Active Directory-Zugriff

Beachten Sie, dass das nachfolgende Dokument wird davon ausgegangen, Sie verfügen über Kenntnisse zum [Bereitstellen von Applications im legacy-Portal für Azure Active Directory](./develop/active-directory-how-to-integrate.md) sowie haben Ihrer Anwendung mit dem [Microsoft Identität Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android)integriert.

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>SSO Konzepte in die Identität des Microsoft-Plattform

### <a name="microsoft-identity-brokers"></a>Microsoft-Identität Makler

Microsoft bietet Applikationen für jede mobilen Plattform, mit denen zur Integration von Anmeldeinformationen für Webanwendungen von anderen Herstellern als auch für spezielle erweiterte Features, die einem einzigen sicheren Ort aus der Anmeldeinformationen überprüfen erfordern ermöglicht. Wir nennen diese **Makler**. IOS und Android werden diese bis herunterladbaren Applikationen bereitgestellt, Kunden entweder unabhängig voneinander installieren oder auf dem Gerät abgelegt werden können, um nach einem Unternehmen, die für ihre Mitarbeiter einiger oder aller das Gerät verwaltet. Diese Makler unterstützen Verwalten von Sicherheit nur für einige Applikationen oder das gesamte Gerät basierend auf was IT-Administratoren schätzen. Dieses Feature ist in Windows durch eine Konto-Auswahl in das Betriebssystem, technisch als Web-Authentifizierung Makler bekannte integriert bereitgestellt.

Zu verstehen, wie wir diese Makler verwenden und wie Ihre Kunden, in deren Fluss Login für die Weitere Informationen finden Sie auf Microsoft Identity-Plattform finden Sie unter möglicherweise.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Muster für die Anmeldung bei auf mobilen Geräten

Zugriff auf Anmeldeinformationen auf Geräten führen zwei grundlegende Muster für die Microsoft Identity Plattform:

* Microsoft-Benutzernamen nicht-Bank
* Microsoft-Benutzernamen Broker

#### <a name="non-broker-assisted-logins"></a>Microsoft-Benutzernamen nicht-Bank

Microsoft-Benutzernamen nicht-Bank sind Login Erfahrung, die erfolgen Inline mit der Anwendung, und verwenden lokalen Speicher auf dem Gerät für die Anwendung. Dieser Speicher möglicherweise über Applikationen freigegeben werden, aber die Anmeldeinformationen sind eng mit der app oder Suite von apps mithilfe dieser Anmeldeinformationen. Dies ist die Benutzeroberfläche, die Sie wahrscheinlich in vielen mobilen Anwendungen kennengelernt haben, in dem Sie einen Benutzernamen und Ihr Kennwort in selbst eingeben.

Diese Benutzernamen haben die folgenden Vorteile:

-  Erfahrungen, die vollständig in der Anwendung vorhanden ist.
-  Anmeldeinformationen können Applikationen gemeinsam verwendet werden, die durch das gleiche Zertifikat, einzelnen anmelden Verbessern der Sammlung von Applications angemeldet sind.
-  Steuerelement, um die Protokollierung in Erfahrung werden mit der Anwendung vor und nach der Anmeldung bereitgestellt.

Diese Benutzernamen haben die folgenden Nachteile:

- Benutzer kann nicht einmaligen Anmeldung über alle apps auftreten, die eine Microsoft Identity, nur über die Microsoft Identities zu verwenden, die Ihrer Anwendung zugegriffen werden und konfiguriert haben.
- Ihrer Anwendung mit erweiterten Business Features wie bedingte Access nicht verwendet werden kann, oder verwenden Sie die InTune-Suite von Produkten.
- Die Anwendung kann nicht basierendes Zertifikatauthentifizierung für Geschäftskunden unterstützen.

Hier ist eine Darstellung über die Funktionsweise der Microsoft Identität SDKs mit dem freigegebenen Speicher Anwendungen zum Aktivieren von SSO:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Microsoft-Benutzernamen Broker

Benutzernamen Bank unterstützt werden Login Erfahrung, die innerhalb der Anwendung Bank auftreten, und verwenden die Speicherung und Sicherheit von der Makler Anmeldeinformationen über alle Programme auf dem Gerät freigeben, die Nutzung der Microsoft Identity-Plattform. Dies bedeutet, dass Ihre Programme auf der Makler verlassen werden, damit Benutzer anmelden. IOS und Android werden diese bis herunterladbaren Applications bereitgestellt, dass Kunden entweder unabhängig voneinander installieren oder auf das Gerät nach einem Unternehmen, die das Gerät für ihre Benutzer verwaltet abgelegt werden können. Ein Beispiel für diese Art von Anwendung ist die Authentifizierung Azure-Anwendung unter iOS. Dieses Feature ist in Windows durch eine Konto-Auswahl in das Betriebssystem, technisch als Web-Authentifizierung Makler bekannte integriert bereitgestellt.
Die Oberfläche je nach Plattform unterschiedlich und kann manchmal Unterbrechung für Benutzer sein, wenn Sie nicht richtig verwaltet. Sie sind wahrscheinlich am häufigsten mit diesem Muster vertraut, wenn Sie die Facebook-Anwendung installiert haben und Facebook Login-Funktionalität in einer anderen Anwendung verwenden. Die Microsoft Identity-Plattform nutzt das gleiche Muster.

Für iOS, die dies zu einem "Übergang" Leads, gehört zum Animation, wo eine Anwendung zum Hintergrund während der Authentifizierung Azure Applications gesendet wird, in den Vordergrund für den Benutzer, welches Konto auszuwählen, diese in melden möchten.  

Für Android und Windows wird die Konto-Auswahl auf Ihrer Anwendung angezeigt, also weniger Unterbrechung für den Benutzer.

#### <a name="how-the-broker-gets-invoked"></a>Wie der Makler aufgerufen wird

Wenn ein kompatibles Bank, auf dem Gerät, wie die Anwendung Azure-Authentifizierung installiert ist geschieht den Microsoft Identität SDKs automatisch die Arbeit der Aufrufen der Makler für Sie aus, wenn ein Benutzer gibt an, dass sie melden Sie sich mit einem beliebigen Konto aus der Microsoft Identity Plattform möchten. Dies könnte eine einer persönlichen Microsoft Account, eine Arbeit oder Schule-Konto oder ein Konto aus, die Sie bereitstellen und in Azure mit unseren Produkten B2C und B2B hosten. Mithilfe von extrem sicheren Algorithmen und Verschlüsselung stellen wir sicher, dass die Anmeldeinformationen sind für gestellte und zurück zur vorherigen Anwendung auf sichere Weise übermittelt. Die genaue technische Details der folgenden Verfahren wird nicht veröffentlicht, aber wurde mit für die Zusammenarbeit von Apple und Google entwickelt.

**Der Entwickler hat die Wahl der an, wenn Microsoft Identität SDK der Makler ruft oder den Microsoft-Fluss nicht Bank verwendet.** Wenn der Entwickler wählt nicht mithilfe des Ablaufs Bank unterstützt verlieren sie die Vorteile der Nutzung von SSO-Anmeldeinformationen, die der Benutzer möglicherweise bereits auf dem Gerät hinzugefügt als auch verhindert, dass die Anwendung zusammen mit Business-Features, die Microsoft seiner Kunden, wie bedingte Access Intune Verwaltungsfunktionen, stellt und Zertifikaten Authentifizierung basierende.

Diese Benutzernamen haben die folgenden Vorteile:

-  Benutzer auftritt SSO für alle Webanwendungen unabhängig von den Hersteller.
-  Ihrer Anwendung können Sie erweiterte Business Features wie bedingte Access nutzen oder dem InTune Produktpaket verwenden.
-  Eine Anwendung kann basierendes Zertifikatauthentifizierung für geschäftliche Benutzer unterstützen.
- Viel sicherer Anmeldeverhalten als Identität der Anwendung und dem Benutzer werden durch die Anwendung Bank mit Algorithmen zusätzliche Sicherheit und Verschlüsselung überprüft.

Diese Benutzernamen haben die folgenden Nachteile:

- Der Benutzer ist in iOS aus Ihrer Anwendung Erfahrung gewechselt, während Anmeldeinformationen ausgewählt sind.
- Verlust der die Möglichkeit, mit der Anmeldung für Ihre Kunden in Ihrer Anwendung zu verwalten.



Hier ist eine Darstellung über die Funktionsweise der Microsoft Identität SDKs die Bank Programme zum Aktivieren von SSO:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+

```

Auf der Grundlage dieser Hintergrundinformationen, das Sie besser zu verstehen und SSO innerhalb der Anwendungs, die mit dem Microsoft Identity Plattform und SDKs implementieren können.


## <a name="enabling-cross-app-sso-using-adal"></a>Aktivieren von Cross-app SSO ADAL verwenden

Verwenden Sie hier die ADAL Android SDK zu:

- Aktivieren von nicht-Bank persönlicher SSO für Ihre Suite von apps
- Aktivieren der Unterstützung für SSO Bank-Assistenten


### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>On, SSO für nicht Bank Umwandlung unterstützt SSO

Für nicht Bank persönlicher SSO für Webanwendungen verwalten den Microsoft Identität SDKs viele ebenfalls zur Komplexität der SSO für Sie. Suchen des richtigen Benutzers im Cache und Verwalten von einer Liste der angemeldeten Benutzer für Ihre Abfrage umfasst.

Zum Aktivieren von SSO für Webanwendungen Sie besitzen, dass Sie die folgenden Schritte ausführen müssen:

1. Sicherzustellen, dass alle Benutzer Ihre Applikationen dieselbe Client-ID oder ID Anwendung.
* Stellen Sie sicher, dass alle Ihre Applikationen die gleichen SharedUserID verfügen.
* Stellen Sie sicher, dass alle Anwendungen das gleiche Signaturzertifikat aus dem Google Play Store freigeben, damit Sie Speicherplatz freigeben können.

#### <a name="step-1-using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Schritt 1: Verwenden die gleichen Client-ID / Anwendung ID für alle Applikationen in Ihre Suite von apps

Jede Anwendung müssen in der Reihenfolge für die Microsoft Identity Plattform zu wissen, dass die zulässige Token über Ihre Programme freigeben die gleichen Client-ID oder Anwendung ID freigeben Dies ist der eindeutige ID, die Ihnen zur Verfügung gestellt wurde, wenn Sie die erste Anwendung im Portal registriert.

Sie vielleicht, wie andere apps auf den Dienst Microsoft Identity erkennt, wenn sie die gleichen Anwendung ID verwendet Die Antwort ist mit den **URIs umleiten**. Jede Anwendung kann mehrere umleiten URIs im Portal Onboarding registriert haben. Jede app in Ihrem Suite kann eine andere Umleitung URI sind. Ein Beispiel für die wie folgt aussieht, lautet wie folgt:

App1 URI-Umleitung:`msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`

App2 URI-Umleitung:`msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`

App3 URI-Umleitung:`msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`

....

Diese sind unter dieselbe Client-ID geschachtelt / Anwendung-ID und nachgeschlagen basierend auf den nächsten Schritt URI Sie uns in Ihrer SDK Konfiguration zurückzukehren.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Beachten Sie, dass das Format dieser URIs umgeleitet werden nachfolgend erläuterten. Sie können beliebiger URI umleiten verwenden, wenn Sie der Makler unterstützen möchten in diesem Fall müssen sie etwa wie oben angezeigt werden*


#### <a name="step-2-configuring-shared-storage-in-android"></a>Schritt 2: Konfigurieren von freigegebenen Speicher in Android

Festlegen der `SharedUserID` wird in diesem Dokument behandelt, jedoch durch Lesen der Google Android-Dokumentation auf das [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html)Kenntnisse werden können. Es ist wichtig, dass Sie sich entscheiden, was soll Ihre SharedUserID aufgerufen, und verwenden, die daran.

Nachdem Sie haben die `SharedUserID` in alle Ihre Clientanwendungen sind Sie bereit sind, verwenden Sie die SSO.

> [AZURE.WARNING]
Beim Freigeben einer Speicherplatz für Ihr Webanwendungen kann jeder Anwendung Benutzer gelöscht oder schlechter allen Token über eine Anwendung löschen. Dies ist besonders verheerende, wenn Sie Applikationen, die klicken Sie auf die Token verfügen, die Arbeit im Hintergrund basieren. Freigeben von Speicherplatz bedeutet, dass Sie alle entfernen Operationen über die Microsoft Identität SDKs sehr vorsichtig vor müssen.

Das war's schon! Microsoft Identität SDK wird jetzt Anmeldeinformationen daran freigeben. Die Benutzerliste wird auch Anwendungsinstanzen gemeinsam verwendet werden.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>On, SSO für Bank Umwandlung unterstützt SSO

Die Möglichkeit für eine Anwendung, alle Bank zu verwenden, die auf dem Gerät installiert ist, ist **standardmäßig deaktiviert**. Verwenden Sie die Anwendung mit der Makler muss führen Sie einige zusätzliche Konfiguration und einen Code in Ihrer Anwendung einfügen.

Die folgenden Schritte sind:

1. Aktivieren Sie in den Code Ihrer Anwendung eines Anrufs an die MS SDK Bank-Modus
2. Einrichten einer neuen URI-Umleitung und bereitstellen, die sowohl die app und Ihre app-Anmeldung
3. Die entsprechenden Berechtigungen in der Android Manifest einrichten


#### <a name="step-1-enable-broker-mode-in-your-application"></a>Schritt 1: Aktivieren der Bank Modus in Ihrer Anwendung
Die Möglichkeit für eine Anwendung verwenden der Makler wird beim Erstellen der "Einstellungen" oder der ersten Einrichtung Ihrer Instanz Authentifizierung aktiviert. Hierzu Ihre ApplicationSettings-Typ im Code festlegen:

```
AuthenticationSettings.Instance.setUseBroker(true);
```


#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>Schritt 2: Einrichten einer neuen Umleitung URI mit der URL des Farbschemas

Um sicherzustellen, dass wir die Anmeldeinformationen Token immer an die richtige Anwendung zurückgeben, müssen wir stellen Sie sicher, dass wir an Ihrer Anwendung auf eine Weise zurückzurufen, die das Betriebssystem Android überprüfen können. Das Betriebssystem Android verwendet eine der Raute des Zertifikats im die Google Play Store. Dies kann nicht durch eine Anwendung unerwünschten manipuliert werden. Daher nutzen wir dies zusammen mit den URI des unsere Bank-Anwendung, um sicherzustellen, dass die Token an die richtige Anwendung zurückgegeben werden. Wir ist es erforderlich, richten Sie diese eindeutige umleiten URI sowohl in Ihrer Anwendung, und legen Sie als URI umleiten in unseren Entwicklerportal.

Der URI-Umleitung muss in die richtige Form des:

`msauth://packagename/Base64UrlencodedSignature`

ex: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Dieser URI umleiten muss in Ihre app-Registrierung mithilfe der [Azure klassischen Portal](https://manage.windowsazure.com/)angegeben werden. Weitere Informationen zum Azure AD-app Registration wird finden Sie unter [Integration mit Azure Active Directory](./develop/active-directory-how-to-integrate.md).


#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>Schritt 3: Einrichten von die entsprechenden Berechtigungen in Ihrer Anwendung

Unsere Anwendung Bank Android verwendet das Feature Konten-Manager des Betriebssystems Android zum Verwalten von Anmeldeinformationen für Webanwendungen. Damit der Makler in Android verwenden können müssen Ihre app-Manifest Berechtigungen Onlinecodebeispiele Konten verwenden. Dies ist in der [Google-Dokumentation für Account Manager hier] (http://developer.android.com/reference/android/accounts/AccountManager.html) ausführlich

Insbesondere sind folgende Berechtigungen:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>Sie haben SSO konfiguriert!

Jetzt Microsoft Identität SDK automatisch sowohl Anmeldeinformationen über Ihre Programme freigeben und der Makler aufrufen, wenn sie auf ihrem Gerät vorhanden ist.
