<properties
    pageTitle="Aktivieren des Cross-app SSO unter iOS ADAL mit | Microsoft Azure"
    description="Verwendung der Features des ADAL SDK Single Sign On über Ihre Programme aktivieren. "
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>Aktivieren des Cross-app SSO unter iOS ADAL verwenden


Bereitstellen für einmaliges Anmelden (SSO), sodass die Benutzer müssen nur einmal geben ihre Anmeldeinformationen und diese Anmeldeinformationen automatisch arbeiten über Applications ist jetzt erwartet von Kunden. Die Probleme in ihren Benutzernamen und Ihr Kennwort eingeben, auf einem kleinen Bildschirm, oft Zeiten mit einen weiteren Faktor (2FA) wie einen Anruf oder einer texted Code, kombiniert ergibt Unzufriedenheit Symbolleiste, wenn ein Benutzer verfügt über Aktion nur einmal für das Produkt.

Wenn Sie die eine Identitätsplattform, die möglicherweise anderen Programmen wie Microsoft Accounts oder ein Arbeit-Konto bei Office 365 verwenden nutzen, erwarten Kunden darüber hinaus, dass die Anmeldeinformationen für alle Webanwendungen unabhängig von den Hersteller verwenden verfügbar sein soll.

Die Microsoft Identity Plattform und unsere Microsoft-Identität SDK, werden alle diese harte Arbeit für Sie und bietet Ihnen die Möglichkeit, entweder über eigene Suite von Applications erfreuen Sie Ihre Kunden mit SSO Ihre oder, wie bei unseren Bank Videofunktionen und Authentifizierung Applications, über das gesamte Gerät.

Diese Anleitung erfahren Sie, wie unsere SDK innerhalb der Anwendung dieses Angebot an Ihre Kunden senden konfigurieren.

Diese Anleitung gilt für:

* Azure-Active Directory
* B2C Azure-Active Directory
* B2B Azure-Active Directory
* Bedingte Azure-Active Directory-Zugriff


Beachten Sie, dass das nachfolgende Dokument wird vorausgesetzt, Sie verfügen über Kenntnisse zum [Bereitstellen von Applications im legacy-Portal für Azure Active Directory](./develop/active-directory-how-to-integrate.md) sowie haben Ihrer Anwendung mit der [Microsoft-Identität iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc)integriert.

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>SSO Konzepte in die Identität des Microsoft-Plattform

### <a name="microsoft-identity-brokers"></a>Microsoft-Identität Makler

Microsoft bietet Applikationen für jede mobilen Plattform, mit denen zur Integration von Anmeldeinformationen für Webanwendungen von anderen Herstellern als auch für spezielle erweiterte Features, die einem einzigen sicheren Ort aus der Anmeldeinformationen überprüfen erfordern ermöglicht. Wir nennen diese **Makler**. IOS und Android werden diese bis herunterladbaren Applikationen bereitgestellt, Kunden entweder unabhängig voneinander installieren oder auf dem Gerät abgelegt werden können, um nach einem Unternehmen, die für ihre Mitarbeiter einiger oder aller das Gerät verwaltet. Diese Makler unterstützen Verwalten von Sicherheit nur für einige Applikationen oder das gesamte Gerät basierend auf was IT-Administratoren schätzen. Dieses Feature ist in Windows durch eine Konto-Auswahl in das Betriebssystem, technisch als Web-Authentifizierung Makler bekannte integriert bereitgestellt.

Zu verstehen, wie wir diese Makler verwenden und wie Ihre Kunden, in deren Fluss Login für die Weitere Informationen finden Sie auf Microsoft Identity-Plattform finden Sie unter möglicherweise.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Muster für die Anmeldung bei auf mobilen Geräten

Zugriff auf Anmeldeinformationen auf Geräten führen zwei grundlegende Muster für die Microsoft Identity Plattform:

* Microsoft-Benutzernamen nicht-Bank
* Microsoft-Benutzernamen Broker

#### <a name="non-broker-assisted-logins"></a>Microsoft-Benutzernamen nicht-Bank

Microsoft-Benutzernamen nicht-Bank sind Login Erfahrung, die auf einer Zeile mit der Anwendung erfolgen und lokalen Speicher auf dem Gerät für die Anwendung verwenden. Dieser Speicher möglicherweise über Applikationen freigegeben werden, aber die Anmeldeinformationen sind eng mit der app oder Suite von apps mithilfe dieser Anmeldeinformationen. Dies ist die Benutzeroberfläche, die Sie wahrscheinlich in vielen mobilen Anwendungen kennengelernt haben, in dem Sie einen Benutzernamen und Ihr Kennwort in selbst eingeben.

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

Benutzernamen Bank unterstützt werden Login Erfahrung, die innerhalb der Anwendung Bank auftreten, und verwenden die Speicherung und Sicherheit von der Makler Anmeldeinformationen über alle Programme auf dem Gerät freigeben, die Nutzung der Microsoft Identity-Plattform. Dies bedeutet, dass Ihre Programme auf der Makler verlassen werden, damit Benutzer anmelden. IOS und Android werden diese bis herunterladbaren Applikationen bereitgestellt, Kunden entweder unabhängig voneinander installieren oder auf das Gerät nach einem Unternehmen, die das Gerät für ihre Benutzer verwaltet abgelegt werden können. Ein Beispiel für diese Art von Anwendung ist die Authentifizierung Azure-Anwendung unter iOS. Dieses Feature ist in Windows durch eine Konto-Auswahl in das Betriebssystem, technisch als Web-Authentifizierung Makler bekannte integriert bereitgestellt.
Die Oberfläche je nach Plattform unterschiedlich und kann manchmal Unterbrechung für Benutzer sein, wenn Sie nicht richtig verwaltet. Sie sind wahrscheinlich am häufigsten mit diesem Muster vertraut, wenn Sie die Facebook-Anwendung installiert haben und Facebook Login-Funktionalität in einer anderen Anwendung verwenden. Die Microsoft Identity-Plattform nutzt das gleiche Muster.

Für iOS, die dies zu einem "Übergang" Leads, gehört zum Animation, wo eine Anwendung zum Hintergrund während der Authentifizierung Azure Applications gesendet wird, in den Vordergrund für den Benutzer, welches Konto auszuwählen, diese in melden möchten.  

Für Android und Windows wird die Konto-Auswahl auf Ihrer Anwendung angezeigt, also weniger Unterbrechung für den Benutzer.

#### <a name="how-the-broker-gets-invoked"></a>Wie der Makler aufgerufen wird

Wenn ein kompatibles Bank, auf dem Gerät, wie die Anwendung Azure-Authentifizierung installiert ist geschieht den Microsoft Identität SDKs automatisch die Arbeit der Aufrufen der Makler für Sie aus, wenn ein Benutzer gibt an, dass sie sich mit einem beliebigen Konto aus der Plattform Microsoft Identity anmelden möchten. Möglicherweise einer persönlichen Microsoft Account, einer Arbeit oder Schule-Konto oder ein Konto, die Sie bereitstellen und Host in Azure mit unseren Produkten B2C und B2B. Mithilfe von extrem sicheren Algorithmen und Verschlüsselung stellen wir sicher, dass die Anmeldeinformationen sind für gestellte und zurück zur vorherigen Anwendung auf sichere Weise übermittelt. Die genaue technische Details der folgenden Verfahren wird nicht veröffentlicht, aber wurde mit für die Zusammenarbeit von Apple und Google entwickelt.

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

Wir verwenden hier die ADAL iOS SDK zu:

- Aktivieren von nicht-Bank persönlicher SSO für Ihre Suite von apps
- Aktivieren der Unterstützung für SSO Bank-Assistenten

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>On, SSO für nicht Bank Umwandlung unterstützt SSO

Für nicht Bank persönlicher SSO für Webanwendungen verwalten den Microsoft Identität SDKs viele ebenfalls zur Komplexität der SSO für Sie. Suchen des richtigen Benutzers im Cache und Verwalten von einer Liste der angemeldeten Benutzer für Ihre Abfrage umfasst.

Zum Aktivieren von SSO für Webanwendungen Sie besitzen, dass Sie die folgenden Schritte ausführen müssen:

1. Sicherzustellen, dass alle Benutzer Ihre Applikationen dieselbe Client-ID oder ID Anwendung.
* Stellen Sie sicher, dass alle Anwendungen das gleiche Signaturzertifikat von Apple freigeben, damit Sie Schlüsselbunde freigeben können
* Anfordern des gleichen Schlüsselbund Anspruchs für jede Anwendung.
* Informieren Sie die Microsoft Identität SDKs über dem Schlüsselbund, dass Sie uns verwenden möchten.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Verwenden die gleichen Client-ID / Anwendung ID für alle Applikationen in Ihre Suite von apps

Jede Anwendung müssen in der Reihenfolge für die Microsoft Identity Plattform zu wissen, dass die zulässige Token über Ihre Programme freigeben die gleichen Client-ID oder Anwendung ID freigeben Dies ist der eindeutige ID, die Ihnen zur Verfügung gestellt wurde, wenn Sie die erste Anwendung im Portal registriert.

Sie vielleicht, wie andere apps auf den Dienst Microsoft Identity erkennt, wenn sie die gleichen Anwendung ID verwendet Die Antwort ist mit den **URIs umleiten**. Jede Anwendung kann mehrere umleiten URIs im Portal Onboarding registriert haben. Jede app in Ihrem Suite kann eine andere Umleitung URI sind. Ein Beispiel für die wie folgt aussieht, lautet wie folgt:

App1 URI-Umleitung:`x-msauth-mytestiosapp://com.myapp.mytestapp`

App2 URI-Umleitung:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 URI-Umleitung:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

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



#### <a name="create-keychain-sharing-between-applications"></a>Erstellen Sie zwischen Programmen Freigabe Schlüsselbund

Aktivieren der schlüsselbundfreigabe ist nicht Gegenstand dieses Dokuments und fallende Apple in deren Dokument [Funktionen hinzufügen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Es ist wichtig, dass Sie sich entscheiden, was Sie Ihren Schlüsselbund aufgerufen werden sollen und die Funktion für alle Ihre Webanwendungen hinzufügen.

Wenn Sie verfügen auftreten Ansprüche ordnungsgemäß Sie einrichten eine Datei in den Ordner Ihres Projekts berechtigt `entitlements.plist` , enthält ein Element, das sieht wie folgt aus:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

Nachdem Sie die Schlüsselbund Ansprüche in jede Anwendung aktiviert haben, und SSO verwendet werden, informieren Sie über Ihren Schlüsselbund Microsoft Identität SDK mithilfe der folgenden Einstellung in Ihrer `ADAuthenticationSettings` mit der folgenden Einstellung:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [AZURE.WARNING]
Beim Freigeben einer Schlüsselbund über Ihre Applikationen kann jeder Anwendung Benutzer gelöscht oder schlechter allen Token über eine Anwendung löschen. Dies ist besonders verheerende, wenn Sie Applikationen, die klicken Sie auf die Token verfügen, die Arbeit im Hintergrund basieren. Freigeben einer Schlüsselbund entfernen bedeutet, dass Sie in alle sehr vorsichtig sein müssen Operationen über die Microsoft Identität SDKs.

Das war's schon! Microsoft Identität SDK wird jetzt Anmeldeinformationen daran freigeben. Die Benutzerliste wird auch Anwendungsinstanzen gemeinsam verwendet werden.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>On, SSO für Bank Umwandlung unterstützt SSO

Die Möglichkeit für eine Anwendung, alle Bank zu verwenden, die auf dem Gerät installiert ist, ist **standardmäßig deaktiviert**. Verwenden Sie die Anwendung mit der Makler muss führen Sie einige zusätzliche Konfiguration und einen Code in Ihrer Anwendung einfügen.

Die folgenden Schritte sind:

1. Aktivieren Sie Bank Modus in Code der Anwendung eines Anrufs an die MS SDK.
2. Einrichten einer neuen URI-Umleitung und bereitstellen, die sowohl die app und Ihre app-Registrierung.
3. Registrieren eine URL-Schema.
4. iOS9 Support: hinzufügen eine Berechtigungsstufe zu einer Datei "Info.plist".


#### <a name="step-1-enable-broker-mode-in-your-application"></a>Schritt 1: Aktivieren der Bank Modus in Ihrer Anwendung
Die Möglichkeit für eine Anwendung verwenden der Makler wird aktiviert, wenn Sie das "Kontext" oder der ersten Einrichtung des Objekts Authentifizierung erstellen. Hierzu Anmeldeinformationentyp Ihrer im Code festlegen:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
Die `AD_CREDENTIALS_AUTO` Einstellung ermöglicht Microsoft Identität SDK versuchen, Aufrufen der Makler `AD_CREDENTIALS_EMBEDDED` wird verhindert, Microsoft Identität SDK wird in der Makler.

#### <a name="step-2-registering-a-url-scheme"></a>Schritt 2: Registrieren eines URL-Schema
Die Microsoft Identity-Plattform verwendet URLs zum Aufrufen der Makler und anschließend die Steuerung wieder an Ihrer Anwendung zurückzugeben. Um die Schleife abzuschließen benötigen Sie eine URL-Schema registriert für eine Anwendung, der die Microsoft Identity Plattform kennen. Dies kann über eine beliebige andere app Phishingversuchen sein, die Sie möglicherweise bereits mit der Anwendung registriert sind.

> [AZURE.WARNING]
Es empfiehlt sich, das URL-Schema relativ zu einer anderen app mit dem gleichen URL-Schema die Wahrscheinlichkeit, dass minimieren eindeutige vornehmen. Apple erzwingt nicht die Eindeutigkeit eines URL-Schemas, die im app Store registriert sind.

Es folgt ein Beispiel, wie dies in der Konfiguration des Projekts angezeigt wird. Sie können dies auch im XCode sowie vornehmen:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>Schritt 3: Einrichten einer neuen Umleitung URI mit der URL des Farbschemas

Um sicherzustellen, dass wir die Anmeldeinformationen Token immer an die richtige Anwendung zurückgeben, müssen wir stellen Sie sicher, dass wir an Ihrer Anwendung auf eine Weise zurückzurufen, die das Betriebssystem iOS überprüfen können. Das Betriebssystem iOS Berichte die ID-Paket der Anwendung, die sie auf der Microsoft-Bank Applications aus. Dies kann nicht durch eine Anwendung unerwünschten manipuliert werden. Daher nutzen wir dies zusammen mit den URI des unsere Bank-Anwendung, um sicherzustellen, dass die Token an die richtige Anwendung zurückgegeben werden. Wir ist es erforderlich, richten Sie diese eindeutige umleiten URI sowohl in Ihrer Anwendung, und legen Sie als URI umleiten in unseren Entwicklerportal.

Der URI-Umleitung muss in die richtige Form des:

`<app-scheme>://<your.bundle.id>`

ex: *X-Msauth-mytestiosapp://com.myapp.mytestapp*

Dieser URI umleiten muss in Ihre app-Registrierung mithilfe der [Azure klassischen Portal](https://manage.windowsazure.com/)angegeben werden. Weitere Informationen zum Azure AD-app Registration wird finden Sie unter [Integration mit Azure Active Directory](./develop/active-directory-how-to-integrate.md).


##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>Schritt 3a: eine Umleitung URI Ihrer app und Entwickler-Portal zur Unterstützung der Authentifizierung basierendes Zertifikat hinzufügen

Basiert Zertifikat unterstützt Authentifizierung, die eine zweite "Msauth" muss registriert werden in Ihrer Anwendung und das [Azure klassischen Portal](https://manage.windowsazure.com/) Zertifikatauthentifizierung verarbeitet werden, wenn Sie die Unterstützung in Ihrer Anwendung hinzufügen möchten.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*


#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a>Schritt 4: iOS9: Hinzufügen ein Konfigurationsparameters zu Ihrer Anwendung

ADAL verwendet – CanOpenURL: überprüft, ob der Makler auf dem Gerät installiert ist. Gesperrt, 9 Apple iOS ab, was eine Anwendung Phishingversuchen für Abfragen können. Sie müssen "Msauth" im Abschnitt LSApplicationQueriesSchemes Hinzufügen Ihrer `info.plist file`.

<key>LSApplicationQueriesSchemes</key>
<array>
     <string>Msauth</string>
</array>

### <a name="youve-configured-sso"></a>Sie haben SSO konfiguriert!

Jetzt Microsoft Identität SDK automatisch sowohl Anmeldeinformationen über Ihre Programme freigeben und der Makler aufrufen, wenn sie auf ihrem Gerät vorhanden ist.
