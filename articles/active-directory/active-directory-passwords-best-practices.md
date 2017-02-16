<properties
    pageTitle="Bewährte Methoden: Azure AD-Kennwort Management | Microsoft Azure"
    description="Bewährte Methoden für Bereitstellung und Verwendung, Stichprobe Endbenutzer-Dokumentation und Schulung Handbücher für die Verwaltung der Kennwörter in Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="deploying-password-management-and-training-users-to-use-it"></a>Bereitstellen von Password Management und Schulung der Benutzer zur gemeinsamen Nutzung

> [AZURE.IMPORTANT] **Sie hier sind, weil Sie Probleme beim Anmelden haben?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).

Nachdem aktivieren Kennwort zurückgesetzt, besteht der nächste Schritt, die, den Sie ausführen müssen, mithilfe des Diensts in Ihrer Organisation Benutzer erhalten. Dazu müssen Sie sicherstellen, dass Ihre Benutzer sind so konfiguriert, dass den Dienst ordnungsgemäß und auch verwenden, dass die Benutzer die Schulung benötigten haben erfolgreich in ihre eigenen Kennwörter verwalten. In diesem Artikel werden Ihnen die folgenden Konzepte erläutert:

* [**So erhalten Sie Ihre Benutzer für die Verwaltung der Kennwörter konfiguriert**](#how-to-get-users-configured-for-password-reset)
  * [Wodurch zeichnet sich ein Konto so konfiguriert, dass für das Zurücksetzen von Kennwörtern](#what-makes-an-account-configured)
  * [Möglichkeiten zum Auffüllen von Authentifizierungsdaten selbst](#ways-to-populate-authentication-data)
* [**Die besten Ihrer Organisation Zurücksetzen des Kennworts eines bereit**](#what-is-the-best-way-to-roll-out-password-reset-for-users)
  * [Einführung in die e-Mail-basierte + Beispiel für e-Mail-Kommunikation](#email-based-rollout)
  * [Erstellen Sie Ihre eigenen benutzerdefinierten Kennwort Verwaltungsportal für Ihre Benutzer](#creating-your-own-password-portal)
  * [Wie Sie erzwungenen Registrierung, um zu erzwingen, dass Benutzer sich anmelden registrieren](#using-enforced-registration)
  * [Zum Hochladen von Authentifizierungsdaten für Benutzerkonten](#uploading-data-yourself)
* [**Beispiel für Benutzer und Support-Schulungsunterlagen (in Kürze verfügbar!)**](#sample-training-materials)

## <a name="how-to-get-users-configured-for-password-reset"></a>So erhalten Sie Benutzer so konfiguriert, dass für das Zurücksetzen von Kennwörtern
In diesem Abschnitt werden Sie verschiedene Methoden, an denen Sie sicherstellen können, dass jeder Benutzer in Ihrer Organisation effektiv zurücksetzen, falls sie ihr Kennwort vergessen Self-service-Kennwort verwenden kann.

### <a name="what-makes-an-account-configured"></a>Wodurch zeichnet sich ein Konto konfiguriert
Damit ein Benutzer das Zurücksetzen von Kennwörtern verwenden kann, müssen **Alle** der folgenden Bedingungen erfüllt sein:

1.  Das Zurücksetzen von Kennwörtern muss im Verzeichnis aktiviert sein.  Informationen Sie zum Aktivieren der durch Lesen [anderer Benutzer ihre Azure AD-Kennwörter zurücksetzen](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords) oder [anderer Benutzer zurücksetzen oder ändern ihre AD Kennwörter](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) Zurücksetzen des Kennworts
2.  Der Benutzer muss lizenziert sein.
 - Für Cloud-Benutzer muss der Benutzer **Alle bezahltes Office 365-Lizenz**, oder eine **Grundlegende AAD** oder **AAD Premium Lizenz** zugeordnet sind.
 - Für auf Prem Benutzer (Verbund oder Hash synchronisiert), die Benutzer **eine Lizenz aus AAD Premium müssen**.
3.  Der Benutzer muss den **minimalen Satz von definierten Authentifizierungsdaten** gemäß der aktuellen zurücksetzen Kennwortrichtlinien verfügen.
 - Authentifizierungsdaten gilt definiert werden, wenn das entsprechende Feld im Verzeichnis um regelkonformes Daten enthält.
 - Ein minimalen Satz von Authentifizierungsdaten ist wie bei **mindestens eine** der Authentifizierungsoptionen aktiviert, wenn eine Richtlinie ein Eingang konfiguriert ist, oder am **mindestens zwei** der aktiviert Authentifizierungsoptionen definiert, wenn Sie eine Richtlinie zwei Eingang konfiguriert ist.
4.  Wenn der Benutzer ein Konto lokal verwendet wird, muss klicken Sie dann [Kennwort abgeschlossenen Writebackvorgängen](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) aktiviert und aktiviert ist

### <a name="ways-to-populate-authentication-data"></a>Methoden zum Authentifizierungsdaten zu füllen.
Sie haben mehrere Optionen zum Angeben von Daten für Benutzer in Ihrer Organisation für das Zurücksetzen von Kennwörtern verwendet werden soll.

- Bearbeiten von Benutzern in der [Azure-Verwaltungsportal](https://manage.windowsazure.com) oder im [Verwaltungsportal von Office 365](https://portal.microsoftonline.com)
- Mit Benutzereigenschaften in Azure AD aus einer lokalen Active Directory-Domäne synchronisiert werden Azure-Active Directory-Synchronisierung
- Verwenden Sie Windows PowerShell so bearbeiten Sie Benutzereigenschaften anhand [der folgenden Schritte](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users)aus.
- Benutzer können ihre eigenen Daten zu registrieren, indem er sie in der Registrierung-Portal unter [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) begleitet
- Festlegen, dass Benutzer für die Kennwortrücksetzung, wenn sie bei ihrem Konto Azure AD-durch Festlegen anmelden registrieren der [**festlegen, dass Benutzer beim anmelden registrieren?**](active-directory-passwords-customize.md#require-users-to-register-when-signing-in) die Konfigurationsoption auf **Ja**.

Benutzer müssen für die Kennwortrücksetzung für das System entwickelt nicht registrieren.  Angenommen, wenn Sie in Ihrem lokalen Verzeichnis vorhandenen Mobil oder Office Telefonnummern zu erhalten haben, können Sie diese in Azure AD synchronisieren, und wir verwenden diese für die Kennwortrücksetzung automatisch.

Sie können auch weitere Informationen zum [Zurücksetzen wie die Daten von Kennwort verwendet werden](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) und [wie Sie einzelne Authentifizierung Felder mit PowerShell füllen können](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users)lesen.

## <a name="what-is-the-best-way-to-roll-out-password-reset-for-users"></a>Was ist die beste Methode für Benutzer Zurücksetzen des Kennworts eines bereit?
Nachfolgend werden die allgemeinen UC-Bereitstellung Schritte für das Zurücksetzen von Kennwörtern:

1.  Aktivieren Sie Kennwort zurücksetzen in Ihrem Verzeichnis, indem Sie die Registerkarte **Konfigurieren** der [Azure-Verwaltungsportal](https://manage.windowsazure.com) und bei Auswahl von **Ja** die Option **Benutzer aktiviert werden, für das Kennwort zurücksetzen** .
2.  Weisen Sie die entsprechenden Lizenzen für jeden Benutzer, denen Sie Kennwortrücksetzung möchte anbieten, die indem Sie auf die Registerkarte **Lizenzen** im [Verwaltungsportal Azure](https://manage.windowsazure.com).
3.  Optional schränken Sie Kennwort zurücksetzen zu einer Gruppe von Benutzern, verschoben, das Feature langsam Zeit, indem Sie die Umschaltfläche **Einschränken des Zugriffs zum Zurücksetzen des Kennworts** auf **Ja** gesetzt und Auswählen einer Sicherheitsgruppe für das Zurücksetzen von Kennwörtern (Beachten Sie, dass diese Benutzer müssen Lizenzen zugewiesen haben) aktivieren ein.
4.  Weisen Sie die Benutzer mit Kennwort zurücksetzen, indem Sie entweder senden sie eine e-Mail-Nachricht beim Registrieren, erzwungen Aktivieren der Registrierung im Bereich Access, oder indem Sie die entsprechenden Authentifizierungsdaten für diesen Benutzer mithilfe von DirSync, PowerShell oder der [Azure-Verwaltungsportal](https://manage.windowsazure.com)hochladen.  Weitere Informationen zu diesem unten wurde.
5.  Überprüfen Sie im Laufe der Zeit Benutzer registrieren, navigieren zur Registerkarte Berichte und den [**Kennwort zurücksetzen Registrierungsaktivität**](active-directory-passwords-get-insights.md#view-password-reset-registration-activity) Bericht anzeigen aus.
6.  Wenn eine gute Anzahl Benutzer registriert haben, schauen Sie sich diese Kennwort zurücksetzen, indem Sie navigieren zur Registerkarte Berichte und den [**Kennwort zurücksetzen Aktivität**](active-directory-passwords-get-insights.md#view-password-reset-activity) Bericht anzeigen.

Es gibt mehrere Methoden, um Ihre Benutzer darüber zu informieren, dass sie registrieren Sie sich für und Kennwort zurücksetzen in Ihrer Organisation verwenden können.  Sie werden nachfolgend detailliert beschrieben.

### <a name="email-based-rollout"></a>E-Mail-basierte UC-Bereitstellung
Vielleicht ist der einfachste Ansatz für Ihre Benutzer darüber informieren, registrieren Sie sich für oder verwenden Sie das Kennwort zurücksetzen durch das Senden einer e-Mails, anweisen, dies aber wünschen.  Es folgt eine Vorlage, die Sie dazu verwenden können.  Gerne die Farben ersetzen / Logos, mit denen der eigenen auswählen, um Ihre Anforderungen anpassen.

  ![][001]

Sie können [die e-Mail-Vorlage herunterladen](http://1drv.ms/1xWFtQM).

### <a name="creating-your-own-password-portal"></a>Erstellen eigener Kennwort-portal
Eine Strategie, die eignet sich für größere Kunden Bereitstellen von Kennwortverwaltungsfunktionen besteht darin, erstellen ein einzelnes "Kennwort Portals", mit denen Ihre Benutzer verwalten alle Elemente im Zusammenhang mit ihre Kennwörter in einem einzigen Ort.  

Viele unserer größten Kunden entscheiden, erstellen einen Stamm-DNS-Eintrag, wie https://passwords.contoso.com mit Links zu den Azure AD-Kennwort Portal zurücksetzen, Kennwort Registrierung Portal zurücksetzen und Kennwort Seiten zu ändern.  Auf diese Weise können in einer beliebigen e-Mail-Kommunikation und Handzettel, die Sie möchten, senden Sie ein einzelnes, ansprechender, URL enthalten, denen Benutzer zu wechseln können, wenn sie eine Sekunde erste Schritte mit dem Dienst haben.

Zum beginnen sollten, wir haben erstellt eine einfache Seite, die neueste reagieren Benutzeroberfläche Entwurf Paradigmen verwendet, und für alle Browser und mobile Geräte funktionieren.

  ![][007]

Sie können [die Websitevorlage herunterladen](https://github.com/kenhoff/password-reset-page).  Es empfiehlt sich, das Logo und die Farben auf den Bedarf Ihrer Organisation anpassen.

### <a name="using-enforced-registration"></a>Verwenden von erzwungenen Registrierung
Wenn Sie Ihre Benutzer für die Kennwortrücksetzung selbst registrieren möchten, können Sie auch erzwingen, damit bei der Anmeldung an der Access-Leiste am [http://myapps.microsoft.com](http://myapps.microsoft.com)registrieren.  Sie können diese Option des Verzeichnisses **Konfigurieren** Registerkarte aktivieren, durch Aktivieren der Option **Register beim Anmelden bei der Access-Systemsteuerung Benutzer erforderlich** .  

Sie können auch optional definieren, unabhängig davon, ob sie aufgefordert werden, erneut nach eine konfigurierbare Zeitspanne registrieren, indem Sie die **Anzahl der Tage, bevor die Benutzer ihre Kontaktdaten bestätigen müssen** Option ein Wert ungleich NULL ist. Weitere Informationen finden Sie unter [Anpassen des Benutzers Kennwort Management Verhalten](active-directory-passwords-customize.md#password-management-behavior) .

  ![][002]

Nachdem Sie diese Option, wenn Benutzer im Bereich Access anmelden aktivieren, wird ein Popup angezeigt, das sie darüber informiert, dass ihre Administrator, um zu überprüfen, deren Kontaktinformationen erforderlich ist. Sie können sie ihr Kennwort zurücksetzen, wenn sie schon einmal Zugriff auf ihr Konto verlieren.

  ![][003]

Klicken Sie auf **Jetzt überprüfen** bringt diese in der **Registrierung Portal Zurücksetzen des Kennworts** zu [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) und zwingt sie zu registrieren.  Registrierung über diese Methode kann durch Klicken auf die Schaltfläche **Abbrechen** oder das Fenster schließen geschlossen werden, aber das Erinnerung: jedes Mal, wenn er registrieren, wenn er nicht registrieren.

  ![][004]

### <a name="uploading-data-yourself"></a>Hochladen von Daten selbst
Wenn Sie Authentifizierungsdaten selbst hochladen möchten, müssen Benutzer für die Kennwortrücksetzung, damit Sie ihre Kennwörter zurücksetzen nicht registrieren.  Solange Benutzer haben die Authentifizierungsdaten auf ihrem Konto an, die das Kennwort entspricht definiert zurücksetzen Richtlinie, die Sie definiert haben, und dann diese Benutzer können ihre Kennwörter zurücksetzen.

Erfahren Sie, welche Eigenschaften über AAD verbinden oder Windows PowerShell festgelegt werden können, finden Sie unter [welche Daten werden durch das Zurücksetzen von Kennwörtern verwendet](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

Sie können die Authentifizierungsdaten über das [Azure-Verwaltungsportal](https://manage.windowsazure.com) hochladen, indem Sie die folgenden Schritte:

1.  Navigieren Sie zu Ihrem Verzeichnis im [Verwaltungsportal Azure](https://manage.windowsazure.com)- **Active Directory-Erweiterung** .
2.  Klicken Sie auf der Registerkarte **Benutzer** auf.
3.  Wählen Sie den Benutzer, die Sie interessiert, aus der Liste aus.
4.  Klicken Sie auf die Registerkarte erste finden Sie **Alternative E-Mail**, die als eine Eigenschaft verwendet werden können, aktivieren Sie das Zurücksetzen von Kennwörtern.

    ![][005]

5.  Klicken Sie auf der Registerkarte **Arbeitsinformationen** .
6.  Auf dieser Seite finden Sie die **Rufnummer**, **Mobiltelefon**, **Telefon Authentifizierung**und **Authentifizierung-e-Mail**.  Diese Eigenschaften können auch festgelegt werden, zu einem Benutzer ermöglichen, das Kennwort zurückzusetzen.

    ![][006]

Sehen Sie, [welche Daten werden durch das Zurücksetzen von Kennwörtern verwendet](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) , um zu sehen, wie jede dieser Eigenschaften verwendet werden kann.

Lesen Sie [zum Zugreifen auf Kennwort zurücksetzen Daten für Ihre Benutzer von PowerShell](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users) , um anzuzeigen, wie Sie lesen und diese Daten mit PowerShell festlegen können.

## <a name="sample-training-materials"></a>Beispiel für-Schulungsunterlagen
Wir arbeiten an-Schulungsunterlagen Beispiel, mit denen Sie schnellere Ihre IT-Abteilung und Ihre Benutzer im Griff zum Bereitstellen und das Zurücksetzen von Kennwörtern verwenden.  Demnächst!


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Links zu Kennwort zurücksetzen Dokumentation
Nachstehend sind Links zu allen Seiten Dokumentation Azure AD Kennwort zurücksetzen aus:

* **Sie hier sind, weil Sie Probleme beim Anmelden haben?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) – erfahren Sie mehr über die sechs verschiedenen Komponenten des Dienst und was bedeutet jede
* [**Erste Schritte**](active-directory-passwords-getting-started.md) - erfahren, wie Sie gestatten Sie Benutzern, zurücksetzen und ihre Cloud oder lokalen Kennwörter ändern
* [**Anpassen**](active-directory-passwords-customize.md) – erfahren Sie, wie Sie das gewünschte Aussehen und Verhalten und Verhalten des Diensts zu den Anforderungen Ihrer Organisation anpassen
* [**Erste Einsichten**](active-directory-passwords-get-insights.md) – erfahren Sie mehr über unsere integrierten reporting-Funktionen
* [**Häufig gestellte Fragen**](active-directory-passwords-faq.md) - erhalten Sie Antworten auf häufig gestellte Fragen
* [**Problembehandlung**](active-directory-passwords-troubleshoot.md) – erfahren, wie Sie schnell Behandeln von Problemen mit dem Dienst
* [**Erfahren Sie mehr**](active-directory-passwords-learn-more.md) - wechseln tief in die technische Details der Funktionsweise des service



[001]: ./media/active-directory-passwords-best-practices/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-best-practices/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-best-practices/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-best-practices/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-best-practices/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-best-practices/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-best-practices/007.jpg "Image_007.jpg"
