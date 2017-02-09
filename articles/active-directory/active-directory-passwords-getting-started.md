<properties
    pageTitle="Erste Schritte: Azure AD-Kennwort Management | Microsoft Azure"
    description="Aktivieren Sie die Benutzer ihre eigenen Kennwörter zurücksetzen, ermitteln die erforderlichen Komponenten für das Zurücksetzen von Kennwörtern und aktivieren das Kennwort abgeschlossenen writebackvorgängen zum Verwalten von lokalen Kennwörtern in Active Directory."
    services="active-directory"
    keywords="Verwaltung von Active Directory Kennwort, Kennwort Verwaltung zurücksetzen Azure AD-Kennwort"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/05/2016"
    ms.author="asteen"/>

# <a name="getting-started-with-password-management"></a>Erste Schritte mit der Verwaltung der Kennwörter

> [AZURE.IMPORTANT] **Sie hier sind, weil Sie Probleme bei der Anmeldung Probleme auftreten?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).

Aktivieren der Benutzer zum Verwalten ihrer eigenen Cloud Azure Active Directory oder lokalen Active Directory Kennwörter Schritten nur wenigen einfachen. Stellen Sie sicher, dass haben Sie einige einfache Vorkenntnisse erfüllt, müssen Sie ein Kennwort aktiviert ändern und Zurücksetzen für die gesamte Organisation, bevor Sie diesen kennen. In diesem Artikel werden Sie die folgenden Konzepte durchzuführen:

* [**So aktivieren Sie die Benutzer ihre Cloud Azure-Active Directory-Kennwörter zurücksetzen**](#enable-users-to-reset-their-azure-ad-passwords)
 - [Self-service-Kennwort zurücksetzen erforderliche Komponenten](#prerequisites)
 - [Schritt 1: Konfigurieren von Richtlinien für Kennwörter zurücksetzen](#step-1-configure-password-reset-policy)
 - [Schritt 2: Hinzufügen von Kontakt Daten für Ihre Testbenutzer](#step-2-add-contact-data-for-your-test-user)
 - [Schritt 3: Zurücksetzen des Kennworts als Benutzer](#step-3-reset-your-azure-ad-password-as-a-user)
* [**So aktivieren Sie Benutzer zurücksetzen oder ändern die Kennwörter ihrer lokalen Active Directory**](#enable-users-to-reset-or-change-their-ad-passwords)
 - [Kennwort abgeschlossenen Writebackvorgängen erforderliche Komponenten](#writeback-prerequisites)
 - [Schritt 1: Herunterladen der neuesten Version von Azure AD-verbinden](#step-1-download-the-latest-version-of-azure-ad-connect)
 - [Schritt 2: Aktivieren Sie das Kennwort abgeschlossenen Writebackvorgängen in Azure AD verbinden über die Benutzeroberfläche oder PowerShell und überprüfen](#step-2-enable-password-writeback-in-azure-ad-connect)
 - [Schritt 3: Konfigurieren der Firewalls](#step-3-configure-your-firewall)
 - [Schritt 4: Einrichten von die geeigneten Berechtigungen](#step-4-set-up-the-appropriate-active-directory-permissions)
 - [Schritt 5: Zurücksetzen Sie des Kennworts AD als Benutzer und überprüfen](#step-5-reset-your-ad-password-as-a-user)

## <a name="enable-users-to-reset-their-azure-ad-passwords"></a>Aktivieren Sie die Benutzer ihre Azure AD-Kennwörter zurücksetzen
In diesem Abschnitt führt Sie durch Aktivieren von Self-service-Kennwort für Ihr AAD Cloud Verzeichnis, Registrieren von Benutzern für das Zurücksetzen von Kennwörtern Self-service und anschließend ausführen, schließlich ein Test Self-service Kennwort zurücksetzen als Benutzer zurücksetzen.

- [Self-service-Kennwort zurücksetzen erforderliche Komponenten](#prerequisites)
- [Schritt 1: Konfigurieren von Richtlinien für Kennwörter zurücksetzen](#step-1-configure-password-reset-policy)
- [Schritt 2: Hinzufügen von Kontakt Daten für die Testbenutzer Ihre](#step-2-add-contact-data-for-your-test-user)
- [Schritt 3: Zurücksetzen des Kennworts als Benutzer](#step-3-reset-your-azure-ad-password-as-a-user)


###  <a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie aktivieren und das Zurücksetzen von Kennwörtern Self-service verwenden können, müssen Sie die folgenden Vorkenntnisse ausführen:

- Erstellen einer AAD Mandanten. Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/)
- Ein Azure-Abonnement zu erhalten. Weitere Informationen finden Sie unter [Neuigkeiten von einem Azure AD-Mandanten?](active-directory-administer.md#what-is-an-azure-ad-tenant).
- Ordnen Sie Ihr Abonnement Azure mit Ihrem Mandanten AAD. Weitere Informationen finden Sie unter [wie Azure-Abonnements Azure AD zugeordnet sind](https://msdn.microsoft.com/library/azure/dn629581.aspx).
- Upgrade auf Azure AD Premium, Basic oder über eine Lizenz für Office 365 bezahlt. Weitere Informationen finden Sie unter [Azure-Active Directory-Editionen](https://azure.microsoft.com/pricing/details/active-directory/).

  >[AZURE.NOTE] Klicken Sie zum Aktivieren der Self-service Kennwortrücksetzung für Cloud-Benutzer müssen Sie Azure AD Premium, Azure AD grundlegende oder eine bezahltes Office 365-Lizenz aktualisieren.  Zum Aktivieren-Self-Service Kennwortrücksetzung für lokale Benutzer müssen Sie auf Azure AD Premium aktualisieren. Weitere Informationen finden Sie unter [Azure-Active Directory-Editionen](https://azure.microsoft.com/pricing/details/active-directory/). Diese Informationen enthält ausführliche Anweisungen zum Registrieren für Azure AD Premium oder grundlegende, wie Sie Ihr Plan für die Lizenz aktivieren, und aktivieren Ihren Azure AD-Zugriff und zum Zuweisen des Zugriffs auf Administrator und Benutzerkonten.

- Erstellen Sie mindestens ein Administrator und ein Benutzerkonto in Ihrem Verzeichnis AAD an.
- Zuweisen einer AAD Premium, Basic oder Office 365 bezahlt Lizenz zu der Administrator- und Benutzerkonten, die Sie erstellt haben.

### <a name="step-1-configure-password-reset-policy"></a>Schritt 1: Konfigurieren von Richtlinien für Kennwörter zurücksetzen
Führen Sie die folgenden Schritte aus, um Benutzer zurücksetzen Kennwortrichtlinien zu konfigurieren:

1.  Öffnen Sie einen Browser Ihrer Wahl, und wechseln Sie zum [Azure klassischen Portal](https://manage.windowsazure.com).
2.  Im [Azure klassischen Portal](https://manage.windowsazure.com)finden Sie die **Active Directory-Erweiterung** auf der Navigationsleiste auf der linken Seite aus.

    ![Kennwort-Verwaltung in Azure AD-][001]

3. Klicken Sie unter der Registerkarte **Verzeichnis** auf das Verzeichnis, in dem Sie das Kennwort zurücksetzen Ablaufrichtlinie für, z. B. konfigurieren möchten, Absatzzahlen.

    ![][002]

4.  Klicken Sie auf die Registerkarte **Konfigurieren** .

    ![][003]

5.  Klicken Sie auf der Registerkarte **Konfigurieren** führen Sie einen Bildlauf nach unten bis zum Abschnitt **Benutzerkennwort Richtlinie zurücksetzen** .  Dies ist die Stelle, an der Sie jeden Aspekt des Benutzers zurücksetzen Kennwortrichtlinien für ein angegebenes Verzeichnis konfigurieren.  

    >[AZURE.NOTE] Diese **Richtlinie gilt nur für Endbenutzer in Ihrer Organisation nicht Administratoren**. Aus Gründen der Sicherheit steuert Microsoft die Richtlinie für Kennwörter zurücksetzen für Administratoren. Wenn Sie in diesem Abschnitt nicht angezeigt werden, stellen Sie sicher, dass Sie sich angemeldet haben für die Azure Active Directory Premium oder Basic und dem Administratorkonto, den dieses Feature konfigurieren ist **eine Lizenz zugewiesen** .

    ![][004]

6.  Zum Konfigurieren des Benutzers Zurücksetzen des Kennworts Richtlinie, schieben Sie die Umschaltfläche **Benutzer aktiviert werden, für das Zurücksetzen von Kennwörtern** zur Einstellung **Ja** .  Dadurch werden weitere Steuerelemente die ermöglichen es Ihnen, wie dieses Feature in Ihrem Verzeichnis funktioniert konfigurieren.  Können Sie das Kennwort zurücksetzen, je nach Bedarf anpassen.  Wenn Sie mehr erfahren möchten zu welche für das Kennwort zurücksetzen Richtlinie Steuerelemente unterstützt, informieren Sie sich [anpassen: Azure AD Password Management](active-directory-passwords-customize.md).

    ![][005]

7.  Nachdem Konfigurieren von Benutzerkennwort Richtlinie nach Bedarf des Mandanten zurücksetzen, klicken Sie auf die Schaltfläche **Speichern** am unteren Rand des Bildschirms.

  >[AZURE.NOTE] Eine Richtlinie für zwei Herausforderung Benutzer Kennwörter zurücksetzen wird empfohlen, damit Sie sehen können, wie die Funktionalität der am häufigsten komplexe Groß-/Kleinschreibung arbeitet.

  ![][006]

### <a name="step-2-add-contact-data-for-your-test-user"></a>Schritt 2: Hinzufügen von Kontakt Daten für Ihre Testbenutzer
Sie haben mehrere Optionen zum Angeben von Daten für Benutzer in Ihrer Organisation für das Zurücksetzen von Kennwörtern verwendet werden soll.

-   Bearbeiten von Benutzern in der [Azure klassischen Portal](https://manage.windowsazure.com) oder im [Verwaltungsportal von Office 365](https://portal.microsoftonline.com)
-   Mit AAD verbinden Benutzereigenschaften in Azure AD aus einer lokalen Active Directory-Domäne synchronisiert werden
-   Verwenden von Windows PowerShell Benutzereigenschaften bearbeiten
-   Benutzer können ihre eigenen Daten zu registrieren, indem er sie in der Registrierung-Portal unter [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) begleitet
-   Festlegen, dass Benutzer registrieren für die Kennwortrücksetzung, wenn sie in den Bereich Access am [http://myapps.microsoft.com](http://myapps.microsoft.com) melden Sie sich an die Option **festlegen, dass Benutzer beim Anmelden bei der Access-Systemsteuerung registrieren** SSPR Konfiguration auf **Ja**festlegen.

Wenn Sie möchten weitere Informationen darüber, welche Daten durch das Zurücksetzen von Kennwörtern sowie alle Formatierung Anforderungen für diese Daten verwendet wird, melden Sie finden Sie unter [welche Daten durch das Zurücksetzen von Kennwörtern verwendet werden?](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

#### <a name="to-add-user-contact-data-via-the-user-registration-portal"></a>Hinzufügen von Benutzerdaten Kontakt über das Benutzer Registrierung-Portal
1.  Das Kennwort zurücksetzen Registrierung Portal verwenden möchten, müssen Sie die Benutzer in Ihrer Organisation mit einem Link zu dieser Seite ([http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)) bereitstellen oder aktivieren Sie die Option Benutzer automatisch registrieren müssen.  Nachdem sie auf diesen Link klicken, werden sie aufgefordert werden, sich mit Organisations-Konto anmelden.  Nach auf diese Weise finden sie die folgende Seite:

    ![][007]

2.  Hier können Benutzer bereitstellen, und überprüfen Sie ihre Mobiltelefonnummer ein, alternative e-Mail-Adresse oder Fragen zur Sicherheit.  Dies ist, wie die Überprüfung ein Mobiltelefon aussieht.

    ![][008]

3.  Nachdem ein Benutzer diese Information gibt an, wird die Seite aktualisieren, um anzugeben, dass die Informationen gültig ist (er enthält unter verborgen wurde).  Durch Klicken auf die Schaltfläche **Fertig stellen** oder **Abbrechen** , wird der Benutzer im Bereich Access angezeigt.

    ![][009]

4.  Nachdem ein Benutzer beide der folgenden Textstellen Informationen überprüft wurde, wird vertretene Profil mit den Daten aktualisiert werden, die hingegen bereitgestellt.  In diesem Beispiel wurde die **Office** -Telefonnummer manuell angegeben, sodass der Benutzer, die auch als Kontakt Methode für das Kennwort zurücksetzen verwenden kann.

    ![][010]

### <a name="step-3-reset-your-azure-ad-password-as-a-user"></a>Schritt 3: Zurücksetzen Sie Ihres Kennworts Azure AD-als Benutzer
Jetzt, da Sie konfiguriert haben ein Benutzer Richtlinie zurücksetzen und Kontaktdetails für Ihre Benutzer angegeben, diesen Benutzer Zurücksetzen eines Kennworts Self-service ausführen kann.

#### <a name="to-perform-a-self-service-password-reset"></a>Zurücksetzen eines Kennworts Self-service ausführen
1.  Wenn Sie auf einer Website wie [**Portal.microsoftonline.com an**](http://portal.microsoftonline.com)zugreifen, sehen Sie einen Anmeldebildschirm wie die unter.  Klicken Sie auf die **kann nicht auf Ihr Konto zugreifen?** Benutzeroberfläche für Link testen, das Kennwort zurücksetzen.

    ![][011]

2.  Nach dem Klicken auf **kann nicht auf Ihr Konto zugreifen?**, Sie zu einer neuen Seite die abgefragte eine **Benutzer-ID** wird, für die Sie ein Kennwort zurücksetzen möchten, geschaltet sind.  Geben Sie den Test- **Benutzer-ID** , die Captcha übergeben, und klicken Sie auf **Weiter**.

    ![][012]

3.  Da der Benutzer eine **Rufnummer**, **auf dem Mobiltelefon**und **Alternative e-Mail** in diesem Fall angegeben hat, sehen Sie, dass er oder sie alle als Optionen aus, um die erste Herausforderung übergeben erteilt wurden.

    ![][013]

4.  In diesem Fall wählen Sie **aufrufen** **Rufnummer** zuerst.  Beachten Sie, dass bei der Auswahl einer Methode telefonische Benutzer zur **Überprüfung ihrer Telefonnummer** aufgefordert werden, bevor sie ihre Kennwörter zurücksetzen können.  Dies ist, um zu verhindern, dass bösartige Einzelpersonen Spam Telefonnummern der Benutzer in Ihrer Organisation.

    ![][014]

5.  Nachdem der Benutzer ihre Telefonnummer auf Anruf Wand Ursache ein Drehfeld angezeigt und vertretene Telefon, um anrufen bestätigt.  Eine Nachricht wird wiedergegeben, nachdem er oder sie Ihr Telefon zeigt an, dass die **Benutzer sollten drücken "#"** zur Überprüfung vertretene Konto wählt.  Drücken dieser Taste automatisch Vergewissern Sie sich, dass der Benutzer über die erste Herausforderung verfügt und die Benutzeroberfläche zur zweiten Überprüfungsschritt wechseln.

    ![][015]

6.  Nachdem Sie die erste Herausforderung bestanden haben, wird die Benutzeroberfläche automatisch aktualisiert, um ihn aus der Liste der Auswahlmöglichkeiten zu entfernen, die der Benutzer verfügt.  In diesem Fall, da Sie zunächst Ihr **Smartphone Office**verwendet haben, bleiben nur **auf dem Mobiltelefon** und **Alternative E-Mail** als gültige Optionen als die Herausforderung für den zweiten Überprüfungsschritt verwendet.  Klicken Sie auf die Option für **Meine alternative E-Mail** -e-Mail.  Wenn Sie damit fertig sind, erhalten eine e-Mail drücken die alternative e-Mail auf Datei e-Mail.

    ![][016]

7.  Hier ist ein Beispiel für eine e-Mail-Nachricht, die Benutzer angezeigt werden – Beachten Sie der Mandanten branding:

    ![][017]

8.  Sobald Sie die e-Mail erhalten haben, wird die Seite zu aktualisieren, und die Überprüfung in der e-Mail gefunden werden, in das Eingabefeld abgebildet eingeben können zwar.  Nach der Eingabe eines Codes gemischte leuchtet die Schaltfläche Weiter, und Sie können im zweiten Überprüfungsschritt passieren.

    ![][018]

9.  Nachdem Sie die Anforderungen der Organisation Richtlinie erfüllt haben, dürfen Sie ein neues Kennwort auswählen.  Das Kennwort überprüft basierten es erfüllt AAD "sicheren" Kennwort Anforderungen (finden Sie unter [Kennwortrichtlinien in Azure AD-](https://msdn.microsoft.com/library/azure/jj943764.aspx)), und eine Bestätigung Stärke angezeigt wird, um den Benutzer darauf hinzuweisen, ob das eingegebene Kennwort dieser Richtlinie entspricht.

    ![][019]

10. Nachdem Sie übereinstimmende Kennwörter, die die Unternehmensrichtlinien erfüllen bieten, Zurücksetzen Ihres Kennworts, und Sie können melden Sie sich mit Ihrem neuen Kennwort sofort.

    ![][020]


## <a name="enable-users-to-reset-or-change-their-ad-passwords"></a>Aktivieren Sie Benutzer zurücksetzen oder Ändern des Kennworts AD

In diesem Abschnitt führt Sie durch das Kennwort zurücksetzen, um Kennwörter zurück zu einem lokalen Active Directory schreiben konfigurieren.

- [Kennwort abgeschlossenen Writebackvorgängen erforderliche Komponenten](#writeback-prerequisites)
- [Schritt 1: Herunterladen der neuesten Version von Azure AD-verbinden](#step-1-download-the-latest-version-of-azure-ad-connect)
- [Schritt 2: Aktivieren Sie das Kennwort abgeschlossenen Writebackvorgängen in Azure AD verbinden über die Benutzeroberfläche oder PowerShell und überprüfen](#step-2-enable-password-writeback-in-azure-ad-connect)
- [Schritt 3: Konfigurieren der Firewalls](#step-3-configure-your-firewall)
- [Schritt 4: Einrichten von die geeigneten Berechtigungen](#step-4-set-up-the-appropriate-active-directory-permissions)
- [Schritt 5: Zurücksetzen Sie des Kennworts AD als Benutzer und überprüfen](#step-5-reset-your-ad-password-as-a-user)


### <a name="writeback-prerequisites"></a>Voraussetzungen für die abgeschlossenen Writebackvorgängen
Bevor Sie aktivieren und das Kennwort abgeschlossenen Writebackvorgängen verwenden können, müssen Sie sicherstellen, dass Sie die folgenden Vorkenntnisse abgeschlossen haben:

- Sie können einem Azure AD-Mandanten mit Azure AD Premium aktiviert.  Weitere Informationen finden Sie unter [Azure-Active Directory-Editionen](active-directory-editions.md).
- Das Zurücksetzen von Kennwörtern wurde konfiguriert und in Ihrem Mandanten aktiviert.  Weitere Informationen finden Sie unter [anderer Benutzer ihre Azure AD-Kennwörter zurücksetzen](#enable-users-to-reset-their-azure-ad-passwords)
- Sie müssen mindestens ein Administrator und eine Test-Benutzerkonto mit einer Azure AD Premium-Lizenz, die Sie zum Testen Sie dieses Feature verwenden können.  Weitere Informationen finden Sie unter [Azure-Active Directory-Editionen](active-directory-editions.md).

  > [AZURE.NOTE] Stellen Sie sicher, dass das Administratorkonto, mit dem Kennwort abgeschlossenen Writebackvorgängen aktivieren, einer Cloud Administratorkonto (erstellt in Azure AD), nicht mit einem verbundenen Konto ist (erstellt lokalen AD und in Azure Active Directory synchronisiert).

- Sie können eine einzelne oder mehrere Gesamtstruktur AD lokalen Bereitstellung unter Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 oder Windows Server 2012 R2 mit den neuesten Servicepacks installiert.

  > [AZURE.NOTE] Wenn Sie eine ältere Version von Windows Server 2008 oder 2008 R2 ausgeführt werden, Sie können dieses Feature weiterhin verwenden, aber [herunterladen](https://support.microsoft.com/kb/2386717) und Installieren von KB 2386717, damit Sie in der Cloud Ihrem lokalen AD-Kennwortrichtlinien erzwingen müssen.

- Sie haben das Verbinden von Azure AD-Tool installiert und Ihre AD-Umgebung für die Synchronisierung in der Cloud erstellt haben.  Weitere Informationen finden Sie unter [Verwenden der lokalen Identitätsinfrastruktur in der Cloud](active-directory-aadconnect.md).

  > [AZURE.NOTE] Bevor Sie das Kennwort abgeschlossenen writebackvorgängen testen, sicherzustellen Sie, dass Sie zuerst einen vollständigen Import und eine vollständige Synchronisierung aus Active Directory und Azure AD-in Azure AD verbinden ausfüllen.

- Müssen Sie Wenn Sie Azure AD synchronisieren oder Azure AD verbinden **TCP 443** ausgehende verwenden (und in einigen Fällen **TCP 9350-9354**) geöffnet sein.  Finden Sie unter [Schritt 3: konfigurieren die Firewall](#step-3-configure-your-firewall) für Weitere Informationen. Mithilfe von DirSync für dieses Szenario wird nicht mehr unterstützt.  Wenn Sie weiterhin DirSync verwenden möchten, aktualisieren Sie auf die neueste Version von Azure AD verbinden vor der Bereitstellung von Kennwort abgeschlossenen writebackvorgängen.

  > [AZURE.NOTE] Wir empfehlen unbedingt für jede Person, die Azure AD synchronisieren oder DirSync-Upgrades auf die neueste Version von Azure AD verbinden Tools zur Sicherstellung bestmögliche Umgebung und neue Features wie diese freigegeben werden.


### <a name="step-1-download-the-latest-version-of-azure-ad-connect"></a>Schritt 1: Herunterladen der neuesten Version von Azure AD-verbinden
Kennwort abgeschlossenen Writebackvorgängen steht in Versionen von Azure AD verbinden oder die Azure AD Synchronisierungstool mit Version Zahl **1.0.0419.0911** oder höher.  Aufheben der Sperre Kennwort abgeschlossenen Writebackvorgängen mit automatischen Konto steht in Versionen von Azure AD verbinden oder die Azure AD Synchronisierungstool mit Version Zahl **1.0.0485.0222** oder höher. Wenn Sie eine ältere Version ausgeführt werden, aktualisieren Sie auf mindestens diese Version, bevor Sie fortfahren. [Klicken Sie hier, um die neueste Version von Azure AD verbinden herunterladen](active-directory-aadconnect.md#install-azure-ad-connect).

#### <a name="to-check-the-version-of-azure-ad-sync"></a>So überprüfen Sie die Version von Azure-Active Directory-Synchronisierung
1.  Navigieren Sie zu **%ProgramFiles%\Azure Active Directory-Synchronisierung\**.
2.  Suchen Sie die **ConfigWizard.exe** ausführbare Datei ein.
3.  Maustaste auf die ausführbare Datei aus, und wählen Sie aus dem Kontextmenü die Option **Eigenschaften** aus.
4.  Klicken Sie auf die Registerkarte **Details** .
5.  Suchen Sie nach der **Version der Datei** Feld.

    ![][021]

Wenn die Versionsnummer größer als oder gleich **1.0.0419.0911 ist**oder Installation Azure AD-Verbindung herstellen, können Sie zu überspringen [Schritt2: Kennwort abgeschlossenen Writebackvorgängen in Azure AD verbinden über die Benutzeroberfläche oder PowerShell aktivieren, und vergewissern Sie sich](#step-2-enable-password-writeback-in-azure-ad-connect).

 > [AZURE.NOTE] Ist dies zum ersten Mal installieren des Tools Azure AD verbinden, empfiehlt es sich, dass Sie einige bewährte Methoden zum Vorbereiten Ihrer Umgebung für die Verzeichnissynchronisation folgen.  Bevor Sie das Tool Azure AD verbinden installieren, müssen Sie Verzeichnissynchronisation entweder im [Verwaltungsportal von Office 365](https://portal.microsoftonline.com) oder das [Azure klassischen Portal](https://manage.windowsazure.com)aktivieren.  Weitere Informationen finden Sie unter [Verwalten von Azure AD verbinden](active-directory-aadconnect-whats-next.md).


### <a name="step-2-enable-password-writeback-in-azure-ad-connect"></a>Schritt 2: Aktivieren Sie das Kennwort abgeschlossenen writebackvorgängen in Azure AD-Verbindung herstellen
Jetzt, da Sie das Verbinden von Azure AD-Tool heruntergeladen haben, sind Sie bereit sind, aktivieren das Kennwort abgeschlossenen Writebackvorgängen.  Sie können auf eine der beiden folgenden Verfahren ausführen.  Im Bildschirm optionale Features des Azure AD verbinden Setup-Assistenten können Sie entweder Kennwort abgeschlossenen Writebackvorgängen aktivieren, oder Sie können über Windows PowerShell aktivieren.

#### <a name="to-enable-password-writeback-in-the-configuration-wizard"></a>So aktivieren Sie das Kennwort abgeschlossenen Writebackvorgängen im Konfigurations-Assistenten
1.  Öffnen Sie auf Ihrem **Computer synchronisieren Verzeichnis**den **Azure AD verbinden** Kontokonfigurations-Assistenten aus.
2.  Klicken Sie auf die Schritte, bis Sie zum Konfigurationsbildschirm **optionale Features** gelangen.
3.  Aktivieren Sie die Option **Kennwort schreiben-zurück** .

    ![][022]

4.  Schließen Sie den Assistenten, die letzte Seite die Änderungen werden zusammenfassen und umfasst das Kennwort abgeschlossenen Writebackvorgängen Konfiguration ändern.

> [AZURE.NOTE] Sie können durch entweder mit diesem Assistenten erneut ausführen, und deaktivieren das Feature oder durch Festlegen der Einstellung **Kennwörter wieder zu lokalen Verzeichnis schreiben** auf **Nein** im Abschnitt **Benutzer Kennwortrichtlinien zurücksetzen** des Verzeichnisses **Konfigurieren** Registerkarte im [Azure klassischen Portal](https://manage.windowsazure.com)Kennwort abgeschlossenen Writebackvorgängen zu einem beliebigen Zeitpunkt deaktivieren.  Weitere Informationen zum Anpassen Ihres Kennworts Erfahrung zurücksetzen, schauen Sie sich [anpassen: Azure AD Password Management](active-directory-passwords-customize.md).

#### <a name="to-enable-password-writeback-using-windows-powershell"></a>So aktivieren Sie das Kennwort abgeschlossenen Writebackvorgängen mithilfe von Windows PowerShell
1.  Öffnen Sie auf Ihrem **Computer synchronisieren Verzeichnis**ein neues **erhöhten Windows PowerShell-Fenster**an.
2.  Wenn das Modul noch nicht geladen ist, geben Sie die `import-module ADSync` Befehl aus, um die Cmdlets Azure AD verbinden in Ihre aktuelle Sitzung zu laden.
3.  Abrufen die Liste der Azure AD-Verbinder in Ihrem System durch Ausführen der `Get-ADSyncConnector` Cmdlet und speichern die Ergebnisse in `$aadConnectorName`, wie`$connectors = ADSyncConnector|where-object {$\_.name -like "\*AAD"}`
4.  So erhalten Sie den aktuellen Status der abgeschlossenen writebackvorgängen für den aktuellen Connector durch das folgende Cmdlet ausführen:`Get-ADSyncAADPasswordResetConfiguration –Connector $aadConnectorName.name`
5.  Aktivieren Sie das Kennwort abgeschlossenen Writebackvorgängen, indem Sie das Cmdlet ausgeführt:`Set-ADSyncAADPasswordResetConfiguration –Connector $aadConnectorName.name –Enable $true`

> [AZURE.NOTE]Wenn für einen Eintrag dazu aufgefordert werden, stellen Sie sicher, dass das Administratorkonto, die Sie für AzureADCredential angeben eines **cloud-Administratorkonto (erstellt in Azure AD)**, nicht mit einem verbundenen Konto ist (erstellt lokalen AD und in Azure Active Directory synchronisiert.
> [AZURE.NOTE]Sie können Kennwort abgeschlossenen Writebackvorgängen über PowerShell deaktivieren, indem Sie die gleichen Schritte wiederholen, oben, aber das übergeben `$false` in Schritt oder indem Sie die Einstellung **Kennwörter wieder zu lokalen Verzeichnis schreiben** auf **Nein** im **Abschnitt Benutzer Kennwortrichtlinien zurücksetzen** des Verzeichnisses **Konfigurieren** Registerkarte im [Azure klassischen Portal](https://manage.windowsazure.com).

#### <a name="verify-that-the-configuration-was-successful"></a>Stellen Sie sicher, dass die Konfiguration erfolgreich war.
Sobald die Konfiguration erfolgreich ist, sehen Sie die Meldung Kennwort zurücksetzen schreiben-Back in der Windows PowerShell-Fenster oder eine Erfolgsmeldung in der Konfiguration Benutzeroberfläche aktiviert ist.

Sie können auch feststellen, dass der Dienst ordnungsgemäß durch Öffnen Ereignisanzeige, navigieren zu Ereignisprotokoll der Anwendung, und suchen Sie nach Ereignis **31005 - OnboardingEventSuccess** aus der Quelle **PasswordResetService**installiert wurde.

  ![][023]

### <a name="step-3-configure-your-firewall"></a>Schritt 3: Konfigurieren der Firewalls
Nachdem Sie das Tool Azure AD verbinden Kennwort abgeschlossenen Writebackvorgängen aktiviert haben, müssen Sie sicherstellen, dass der Dienst in der Cloud verbinden kann.

1.  Sobald die Installation abgeschlossen ist, wenn Sie in Ihrer Umgebung unbekannte ausgehende Verbindungen blockiert werden, müssen Sie auch die folgenden Regeln der Firewall hinzufügen. Stellen Sie sicher, dass Sie Ihre AAD verbinden Computer neu starten, nachdem Sie diese Änderungen vorgenommen:
   - Ausgehende Verbindungen zulassen, über den Port 443 TCP
   - Verbindungen Sie ausgehende zu https://ssprsbprodncu-sb.accesscontrol.windows.net/
   - Wenn Sie einen Proxy verwenden, oder gibt es allgemein Connectivity Probleme, ausgehende Verbindungen zulassen, über den Port 9350-9354 und 5671 TCP port

### <a name="step-4-set-up-the-appropriate-active-directory-permissions"></a>Schritt 4: Einrichten der entsprechenden Active Directory-Berechtigungen
Für jede Gesamtstruktur, die Benutzer enthält, deren Kennwörter zurücksetzen, wenn X das Konto ist, die für diese Gesamtstruktur im Konfigurations-Assistenten (während der anfänglichen Konfiguration) angegeben wurde, und klicken Sie dann X muss das **Kennwort zurücksetzen**, **Kennwort ändern**, **Schreiben von Berechtigungen** angegeben werden, klicken Sie auf `lockoutTime`, und **Schreiben Sie Berechtigungen** für `pwdLastSet`, erweiterte Rechte, die sich im Stamm-Objekt für jede Domäne in dieser Gesamtstruktur. Rechts sollte markiert werden, während alle Benutzerobjekte vererbt.  

Wenn Sie nicht sicher, welches Konto sind bezieht sich um, die Konfiguration Azure Active Directory verbinden Benutzeroberfläche zu öffnen, und klicken Sie auf die Option **Überprüfen Ihrer Lösung** oben.  Das Konto, die, dem Sie über die Berechtigung zum hinzufügen müssen, ist in den folgenden Screenshot in Rot unterstrichenen.

**<font color="red">Stellen Sie sicher, dass Sie diese Berechtigung für jede Domäne in den einzelnen Gesamtstrukturen in Ihrem System einrichten, andernfalls Kennwort abgeschlossenen writebackvorgängen nicht ordnungsgemäß funktioniert.</font>**

  ![][032]

  Folgende Berechtigungen festlegen, die MA Dienstkontos für jede Gesamtstruktur zum Verwalten von Kennwörtern für Benutzerkonten innerhalb dieser Gesamtstruktur zulassen. Wenn Sie keine folgende Berechtigungen zuweisen, klicken Sie dann treten, obwohl abgeschlossenen writebackvorgängen ordnungsgemäß konfiguriert sein, erscheint Benutzer Fehler bei dem Versuch, die ihre Kennwörter lokalen aus der Cloud zu verwalten. Hier sind die detaillierten Schritte auf, wie Sie diese mit dem Management-Snap-in **Active Directory-Benutzer und Computer** ausführen können:

>[AZURE.NOTE] Es kann bis zu einer Stunde für folgende Berechtigungen auf alle Objekte in Ihrem Verzeichnis repliziert dauern.

#### <a name="to-set-up-the-right-permissions-for-writeback-to-occur"></a>So richten Sie die entsprechenden Berechtigungen für abgeschlossenen writebackvorgängen ausgeführt ein

1.  Öffnen Sie **Active Directory-Benutzer und Computer** mit einem Konto, das über die entsprechenden Domäne Administration Berechtigungen verfügt.
2.  Die Option **Im Menü Ansicht** stellen Sie sicher, dass **Erweiterte Funktionen** aktiviert ist.
3.  Klicken Sie im linken Bereich mit der rechten Maustaste auf das Objekt, das den Stamm der Domäne darstellt.
4.  Klicken Sie auf der Registerkarte **Sicherheit** .
5.  Klicken Sie dann auf **Erweitert**.

    ![][024]

6.  Klicken Sie auf der Registerkarte **Berechtigungen** auf **Hinzufügen**.

    ![][025]

7.  Wählen Sie das Konto, das Sie die Berechtigung zum erteilen möchten (Dies ist das Konto, das beim Einrichten der Synchronisierung für diese Gesamtstruktur angegeben wurde).
8.  Wählen Sie in der Dropdownliste oben **untergeordnete Objekte**ein.
9.  Im Dialogfeld **Berechtigungseintrag** , der angezeigt wird, aktivieren Sie das Kontrollkästchen **Kennwort zurücksetzen**, **Ändern des Kennworts**, **Schreiben von Berechtigungen** auf `lockoutTime`, und **Schreiben von Berechtigungen** auf `pwdLastSet`.

    ![][026]
    ![][027]
    ![][028]

10. Klicken Sie dann auf **Übernehmen/Ok** , bis alle geöffneten Dialogfelder.

### <a name="step-5-reset-your-ad-password-as-a-user"></a>Schritt 5: Zurücksetzen Sie Ihres Kennworts AD als Benutzer
Jetzt, da Kennwort abgeschlossenen Writebackvorgängen aktiviert wurde, können Sie testen, die zu diesem Zweck wird das Zurücksetzen des Kennworts eines Benutzers, dessen Konto in Ihrem Mandanten Cloud synchronisiert wurde.

#### <a name="to-verify-password-writeback-is-working-properly"></a>Um zu überprüfen, dass Kennwort abgeschlossenen Writebackvorgängen ordnungsgemäß funktioniert.
1.  Navigieren Sie zu [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com) , oder wechseln Sie zu einem beliebigen Organisations-ID-Anmeldebildschirm, und klicken Sie auf die **kann nicht auf Ihr Konto zugreifen?** Link.

    ![][029]

2.  Sie sollten jetzt eine neue Seite finden Sie unter der aufgefordert wird für Benutzer-ID, für die Sie ein Kennwort zurücksetzen möchten. Geben Sie Ihre Benutzer-ID testen, und führen Sie die Schritte illustrieren Kennwort zurücksetzen.
3.  Nachdem Sie Ihr Kennwort zurücksetzen, wird einen Bildschirm angezeigt, die sieht ungefähr wie folgt aus. Dies bedeutet Sie Ihr Kennwort in Ihrem lokalen wurden erfolgreich zurückgesetzt und/oder cloud Verzeichnisse durchsuchen.

    ![][030]

4.  Wechseln Sie zum bestätigen, dass der Vorgang wurde erfolgreich oder Diagnostizieren von Fehlern bei, auf Ihren **Computer synchronisieren Directory**, öffnen Sie die **Ereignisanzeige**, navigieren Sie zu der **Anwendungsereignisprotokoll**und Ereignis **31002 - PasswordResetSuccess** aus der Quelle **PasswordResetService** für Ihr Testbenutzer suchen aus.

    ![][031]


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Links zu Kennwort zurücksetzen Dokumentation
Nachstehend sind Links zu allen Seiten Dokumentation Azure AD Kennwort zurücksetzen aus:

* **Sie hier sind, weil Sie Probleme bei der Anmeldung Probleme auftreten?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) – erfahren Sie mehr über die sechs verschiedenen Komponenten des Dienst und was bedeutet jede
* [**Anpassen**](active-directory-passwords-customize.md) – erfahren Sie, wie Sie das gewünschte Aussehen und Verhalten und Verhalten des Diensts zu den Anforderungen Ihrer Organisation anpassen
* [**Bewährte Methoden**](active-directory-passwords-best-practices.md) - erfahren, wie Sie schnell bereitstellen und Verwalten von Kennwörtern in Ihrer Organisation effektiv
* [**Erste Einsichten**](active-directory-passwords-get-insights.md) – erfahren Sie mehr über unsere integrierten reporting-Funktionen
* [**Häufig gestellte Fragen**](active-directory-passwords-faq.md) - erhalten Sie Antworten auf häufig gestellte Fragen
* [**Problembehandlung**](active-directory-passwords-troubleshoot.md) – erfahren, wie Sie schnell Behandeln von Problemen mit dem Dienst
* [**Erfahren Sie mehr**](active-directory-passwords-learn-more.md) - wechseln tief in die technische Details der Funktionsweise des service



[001]: ./media/active-directory-passwords-getting-started/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-getting-started/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-getting-started/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-getting-started/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-getting-started/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-getting-started/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-getting-started/007.jpg "Image_007.jpg"
[008]: ./media/active-directory-passwords-getting-started/008.jpg "Image_008.jpg"
[009]: ./media/active-directory-passwords-getting-started/009.jpg "Image_009.jpg"
[010]: ./media/active-directory-passwords-getting-started/010.jpg "Image_010.jpg"
[011]: ./media/active-directory-passwords-getting-started/011.jpg "Image_011.jpg"
[012]: ./media/active-directory-passwords-getting-started/012.jpg "Image_012.jpg"
[013]: ./media/active-directory-passwords-getting-started/013.jpg "Image_013.jpg"
[014]: ./media/active-directory-passwords-getting-started/014.jpg "Image_014.jpg"
[015]: ./media/active-directory-passwords-getting-started/015.jpg "Image_015.jpg"
[016]: ./media/active-directory-passwords-getting-started/016.jpg "Image_016.jpg"
[017]: ./media/active-directory-passwords-getting-started/017.jpg "Image_017.jpg"
[018]: ./media/active-directory-passwords-getting-started/018.jpg "Image_018.jpg"
[019]: ./media/active-directory-passwords-getting-started/019.jpg "Image_019.jpg"
[020]: ./media/active-directory-passwords-getting-started/020.jpg "Image_020.jpg"
[021]: ./media/active-directory-passwords-getting-started/021.jpg "Image_021.jpg"
[022]: ./media/active-directory-passwords-getting-started/022.jpg "Image_022.jpg"
[023]: ./media/active-directory-passwords-getting-started/023.jpg "Image_023.jpg"
[024]: ./media/active-directory-passwords-getting-started/024.jpg "Image_024.jpg"
[025]: ./media/active-directory-passwords-getting-started/025.jpg "Image_025.jpg"
[026]: ./media/active-directory-passwords-getting-started/026.jpg "Image_026.jpg"
[027]: ./media/active-directory-passwords-getting-started/027.jpg "Image_027.jpg"
[028]: ./media/active-directory-passwords-getting-started/028.jpg "Image_028.jpg"
[029]: ./media/active-directory-passwords-getting-started/029.jpg "Image_029.jpg"
[030]: ./media/active-directory-passwords-getting-started/030.jpg "Image_030.jpg"
[031]: ./media/active-directory-passwords-getting-started/031.jpg "Image_031.jpg"
[032]: ./media/active-directory-passwords-getting-started/032.jpg "Image_032.jpg"
