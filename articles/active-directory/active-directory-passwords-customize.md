<properties
    pageTitle="Anpassen: Azure AD-Kennwort Management | Microsoft Azure"
    description="Vorgehensweise zum Anpassen von Aussehen und Verhalten, Verhalten und Benachrichtigungen Password Management in Azure Active Directory, die Ihren Anforderungen entsprechen."
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
    ms.date="08/03/2016"
    ms.author="asteen"/>

# <a name="customizing-password-management-to-fit-your-organizations-needs"></a>Anpassen der Verwaltung von Kennwort an den Anforderungen Ihrer Organisation anpassen.

> [AZURE.IMPORTANT] **Sie hier sind, weil Sie Probleme bei der Anmeldung Probleme auftreten?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).

Damit Benutzer die bestmögliche Leistung zu bieten, empfehlen wir, dass Sie untersuchen und experimentieren Sie mit allen Password Management Konfigurationsoptionen zur Verfügung. Tatsächlich können Sie nicht gestartet werden diese sofort untersuchen, indem Sie auf der Konfigurationsregisterkarte der **Active Directory-Erweiterung** im [Azure klassischen Portal](https://manage.windowsazure.com). Dieses Thema führt Sie durch alle anderen Password Management Anpassungen als Administrator aus innerhalb der Registerkarte **Konfigurieren** Ihres Verzeichnisses innerhalb des [Azure klassischen Portal](https://manage.windowsazure.com), umso einschließlich:

| Thema |  |
| --------- | --------- |
| Wie ich aktivieren oder deaktivieren das Zurücksetzen von Kennwörtern? | [Einstellung: Benutzer aktiviert für das Zurücksetzen von Kennwörtern](#users-enabled-for-password-reset) |
| Wie erstellen ich Kennwortrücksetzung auf eine bestimmte Gruppe von Benutzern? | [Einschränken von bestimmten Benutzern Zurücksetzen des Kennworts](#restrict-access-to-password-reset) |
| Wie ändere ich, welche Authentifizierung Methoden unterstützt werden? | [Festlegen: Authentifizierungsmethoden Benutzer bereitgestellt](#authentication-methods-available-to-users) |
| Wie ändere ich die Anzahl der Authentifizierungsmethoden erforderlich? | [Festlegen: der Anzahl der Authentifizierungsmethoden erforderlich](#number-of-authentication-methods-required) |
| Wie richte ich benutzerdefinierte Sicherheitsfragen? | [Einstellung: benutzerdefinierte Sicherheitsfragen](#custom-security-questions) |
| Wie richte ich vorerstellte lokalisierten Sicherheitsfragen? | [Einstellung: Knowledge basierende Sicherheitsfragen](#knowledge-based-security-questions) |
| Wie ändere ich, wie viele Fragen zur Sicherheit erforderlich sind? | [Festlegen: der Anzahl der Sicherheitsfragen für die Registrierung oder Zurücksetzen](#number-of-questions-required-to-register) |
| Wie kann ich meine Benutzer beim anmelden registrieren erzwingen? | [Erzwungenen Registrierung-basierten Einführung für das Zurücksetzen von Kennwörtern](#require-users-to-register-when-signing-in) |
| Wie kann ich meine Benutzer, deren registrierten regelmäßig erneut bestätigen erzwingen? | [Festlegen: der Anzahl der Tage, bevor die Benutzer ihre Authentifizierungsdaten erneut bestätigen müssen](#number-of-days-before-users-must-confirm-their-contact-data) |
| Wie kann ich anpassen, wie ein Benutzer an den Administrator wird? | [Einstellung: Anpassen des Links "Wenden Sie sich an den Administrator"](#customize-the-contact-your-administrator-link) |
| Wie kann ich Benutzer AD-Konten entsperren, ohne dass ein Kennwort zurückzusetzen zulassen? | [Einstellung: Aktivieren Benutzer ihre AD-Konten entsperren ohne Zurücksetzen eines Kennworts](#allow-users-to-unlock-accounts-without-resetting-their-password) |
| Wie kann ich ein Kennwort zurücksetzen Benachrichtigungen für Benutzer aktivieren? | [Einstellung: benachrichtigen Sie Benutzer, wenn ihr Kennwort zurückgesetzt werden](#notify-users-and-admins-when-their-own-password-has-been-reset) |
| Wie kann ich ein Kennwort zurücksetzen Benachrichtigungen für Administratoren aktivieren? | [Einstellung: andere Administratoren benachrichtigt werden, wenn ein Administrator ihr eigenes Kennwort zurücksetzen](#notify-admins-when-other-admins-reset-their-own-passwords) |
| Wie kann ich ein Kennwort zurücksetzen Aussehen und Verhalten anpassen? | [Einstellung: Firmenname, branding und logo](#password-management-look-and-feel) |


## <a name="password-management-look-and-feel"></a>Kennwort Management Aussehen und Verhalten
Die folgende Tabelle beschreibt, wie jedes Steuerelement die Oberfläche für die Benutzer registrieren für ein Kennwort zurücksetzen und ihre Kennwörter zurücksetzen auswirkt.  Sie können diese Optionen unter den Abschnitt **Directory Eigenschaften** des Verzeichnisses **Konfigurieren** Registerkarte innerhalb des [Azure-Verwaltungsportal](https://manage.windowsazure.com)konfigurieren.

<table>
            <tbody><tr>
              <td>
                <p>
                  <strong>Policy-Steuerung</strong>
                </p>
              </td>
              <td>
                <p>
                  <strong>Beschreibung</strong>
                </p>
              </td>
              <td>
                <p>
                  <strong>Wirkt sich auf?</strong>
                </p>
              </td>
            </tr>
            <tr>
              <td>
                <div id="directory-name">
                  <p>Name des Verzeichnisses</p>
                </div>
              </td>
              <td>
                <p>Bestimmt, welche Organisationsnamen Benutzer oder Administratoren finden Sie unter auf Kennwort e-Mail-Kommunikation zurücksetzen</p>
              </td>
              <td>
                <p>
                  <strong>E-Mails "Wenden Sie sich an den Administrator":</strong>
                </p>
                <ul>
                  <li class="unordered">
Bestimmt die Absenderadresse Anzeigename, z. B. "Microsoft für <strong>Absatzzahlen"</strong><br><br></li>
                  <li class="unordered">
Der Name bestimmt, Betreff der e-Mail, z. B.<strong>"Absatzzahlen</strong> Konto e-Mail-Überprüfungscode"<br><br></li>
                </ul>
                <p>
                  <strong>Kennwort zurücksetzen e-Mails an:</strong>
                </p>
                <ul>
                  <li class="unordered">
Bestimmt die Absenderadresse Anzeigename, z. B. "Microsoft für <strong>Absatzzahlen"</strong><br><br></li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>
                <div id="sign-in-and-access-panel-page-appearance">
                  <p>Anmelden und das Aussehen der Systemsteuerung Seiten zugreifen</p>
                </div>
              </td>
              <td>
                <p>Bestimmt, ob der Benutzer, besuchen den Seiten zum Zurücksetzen des Kennworts das Microsoft-Logo oder Ihr eigenes benutzerdefiniertes Logo angezeigt.  Dieses Konfigurationselement auch fügt Ihr branding mit dem Zugriff auf Bereich, und melden Sie sich die Seite.</p>
                <p>

                </p>
                <p>Sie können mehr über die Mandanten branding und Anpassung von Features in <a href="https://technet.microsoft.com/library/dn532270.aspx">Hinzufügen Unternehmen branding bis hin zu den anmelden und die Access-Systemsteuerung Seiten</a>erfahren.</p>
              </td>
              <td>
                <p>
                  <strong>Kennwort zurücksetzen Portal an:</strong>
                </p>
                <ul>
                  <li class="unordered">
Legt fest, ob Ihr Logo am oberen Rand des Portals Kennwort zurücksetzen, statt der Standardlogo Microsoft angezeigt wird.<br><br></li>
                  <li class="unordered">
                    <strong>Hinweis:</strong> Sie möglicherweise nicht angezeigt, des Logos auf der ersten Seite des Kennworts Portal zurücksetzen, wenn Sie direkt zu der Seite Kennwort zurücksetzen gelangen.  Nachdem ein Benutzer vertretene Benutzer-ID gibt und Weiter klickt, wird Ihr Logo angezeigt.  Sie können erzwingen, dass Ihr Logo beim Laden der Seite durch übergeben angezeigt werden, der Parameter Wh gewährleistet, dass das Kennwort zurücksetzen Seite wie folgt: <a href="https://passwordreset.microsoftonline.com?whr=wingtiptoysonline.com">https://passwordreset.microsoftonline.com?whr=wingtiptoysonline.com</a><br><br></li>
                </ul>
                <p>
                  <strong>"Wenden Sie sich an den Administrator" e-Mails:</strong>
                </p>
                <ul>
                  <li class="unordered">
Legt fest, ob Ihr Logo am unteren Rand der e-Mails an Administratoren gesendet, wenn der Benutzer wählen Sie zu erreichen, indem Sie auf den Link "Wenden Sie sich an den Administrator" auf der Benutzeroberfläche das Zurücksetzen von Kennwörtern angezeigt wird.<br><br></li>
                </ul>
                <p>
                  <strong>Kennwort zurücksetzen e-Mails an:</strong>
                </p>
                <ul>
                  <li class="unordered">
Legt fest, ob Ihr Logo am unteren Rand der e-Mails an Benutzer gesendet werden, wenn sie ihre Kennwörter zurücksetzen angezeigt wird.<br><br></li>
                </ul>
              </td>
            </tr>
          </tbody></table>

## <a name="password-management-behavior"></a>Kennwort Management Verhalten
Die folgende Tabelle beschreibt die Auswirkungen jedes Steuerelement auf der Benutzeroberfläche für Benutzer registrieren für Kennwort zurücksetzen und ihre Kennwörter zurücksetzen.  Sie können diese Optionen im Abschnitt **Benutzer Kennwortrichtlinien zurücksetzen** des Verzeichnisses **Konfigurieren** Registerkarte im [Verwaltungsportal Azure](https://manage.windowsazure.com)konfigurieren.

> [AZURE.NOTE]Das Administratorkonto derzeit muss es sich um eine zugewiesen, damit Sie diese Steuerelemente Richtlinie finden Sie unter AAD Premium-Lizenz verfügen.<br><br>Diese Richtlinie Steuerelemente beziehen sich nur auf Zurücksetzen, deren Kennwörter, die nicht Administratoren Endbenutzer.  **Administratoren verfügen über eine Standardrichtlinie alternative e-Mail-und/oder Mobiltelefon, das für sie von Microsoft angegeben ist, der nicht geändert werden kann.**

<table>
            <tbody><tr>
              <td>
                <p>
                  <strong>Policy-Steuerung</strong>
                </p>
              </td>
              <td>
                <p>
                  <strong>Beschreibung</strong>
                </p>
              </td>
              <td>
                <p>
                  <strong>Wirkt sich auf?</strong>
                </p>
              </td>
            </tr>
            <tr>
              <td>
                <div id="users-enabled-for-password-reset">
                  <p>Benutzer, die für das Zurücksetzen von Kennwörtern aktiviert</p>
                </div>
              </td>
              <td>
                <p>Bestimmt, ob das Zurücksetzen von Kennwörtern für Benutzer in diesem Verzeichnis aktiviert ist. </p>
              </td>
              <td>
                <p>
                  <strong>Registrierungsinformationen Portal:</strong>
                </p>
                <ul>
                  <li class="unordered">
Wenn Sie festlegen, um keine, keine Benutzer ihre eigenen Daten zur Herausforderung erfassen kann.<br><br></li>
                  <li class="unordered">
Wenn Ja festgelegt, jedem Benutzer im Verzeichnis Herausforderung Daten registrieren kann der Registrierung-Portal unter <a href="http://aka.ms/ssprsetup">http://aka.ms/ssprsetup</a>Anzeigebereich durch.<br><br></li>
                  <li class="unordered">
                    <strong>Hinweis:</strong> müssen Benutzer eine Azure AD Premium oder grundlegende Lizenz zugewiesen werden, bevor er für das Zurücksetzen von Kennwörtern erfassen können.<br><br></li>
                </ul>
                <p>
                  <strong>Kennwort zurücksetzen Portal:</strong>
                </p>
                <ul>
                  <li class="unordered">
Wenn die Einstellung no Benutzer muss der folgende Meldung angezeigt wenden Sie sich an den Administrator, um ihr Kennwort zurückzusetzen.<br><br></li>
                  <li class="unordered">
Wenn Sie auf Ja festgelegt, Benutzer ihre Kennwörter zurücksetzen automatisch nach <a href="http://passwordreset.microsoftonline.com">http://passwordreset.microsoftonline.com</a>soll, oder klicken auf den Link <strong>Zugreifen auf Ihr Konto kann nicht</strong> auf eine beliebige Organisations-ID-Anmeldeseite können.<br><br></li>
                  <li class="unordered">
                    <strong>Hinweis:</strong> müssen Benutzer eine Azure AD Premium oder grundlegende Lizenz zugewiesen, bevor sie ihre Kennwörter zurücksetzen können.<br><br></li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>
                <div id="restrict-access-to-password-reset">
                  <p>Einschränken des Zugriffs auf das Zurücksetzen von Kennwörtern</p>
                </div>
              </td>
              <td>
                <p>Bestimmt, ob nur eine bestimmte Gruppe von Benutzern mit Zurücksetzen von Kennwörtern zulässig ist. (Nur angezeigt, wenn <strong>Benutzer aktiviert Kennwort zurücksetzen</strong> auf <strong>Ja</strong>festgelegt ist).</p>
              </td>
              <td>
                <p>
                  <strong>Registrierungsinformationen Portal:</strong>
                </p>
                <ul>
                  <li class="unordered">
Diese Einstellung wirkt sich nicht auf Benutzerzugriff auf das Kennwort zurücksetzen Registrierung Portal aus. Wenn <strong>Benutzer aktiviert Kennwort zurücksetzen</strong> auf <strong>Ja</strong>festgelegt ist, können alle Endbenutzer in Ihrem Verzeichnis für die Kennwortrücksetzung am <a href="http://aka.ms/ssprsetup">http://aka.ms/ssprsetup</a>registrieren.
                  </li>
                </ul>
                <p>
                  <strong>Kennwort zurücksetzen Portal an:</strong>
                </p>
                <ul>
                  <li class="unordered">
Wenn Sie festlegen, um keine und dann alle Endbenutzern in Ihrem Verzeichnis ihre Kennwörter zurücksetzen kann.<br><br></li>
                  <li class="unordered">
Wenn Sie auf Ja festgelegt, dann nur in der <strong>Gruppe, die das Zurücksetzen von Kennwörtern ausführen können,</strong> Steuerelement angegebenen Endbenutzer ihre Kennwörter zurücksetzen können.<br><br></li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>
                <div id="group-that-can-perform-password-reset">
                  <p>Gruppe, die das Zurücksetzen von Kennwörtern ausführen können</p>
                </div>
              </td>
              <td>
                <p>Legt fest, welche Gruppe von Endbenutzern verwendet das Zurücksetzen von Kennwörtern zulässig ist. </p>
                <p>

                </p>
                <p>(Nur sichtbar, wenn <strong>Einschränken des Zugriffs auf das Zurücksetzen von Kennwörtern</strong> auf <strong>Ja</strong>festgelegt ist).</p>
              </td>
              <td>
                <p>
                  <strong>Hinweis:</strong>
                </p>
                <ul>
                  <li class="unordered">
Wenn keine Gruppe angegeben ist, und Sie klicken Sie auf <strong>Speichern</strong>, wird eine leere Gruppe namens <strong>SSPRSecurityGroupUsers</strong> für Sie erstellt.<br><br></li>
                  <li class="unordered">
Wenn Sie Ihre eigene Gruppe angeben möchten, können Sie eigene Anzeigenamen angeben.<br><br></li>
                </ul>
                <p>
                  <strong>Registrierungsinformationen Portal:</strong>
                </p>
                <ul>
                  <li class="unordered">
Diese Einstellung wirkt sich nicht auf Benutzerzugriff auf das Kennwort zurücksetzen Registrierung Portal aus. Wenn <strong>Benutzer aktiviert Kennwort zurücksetzen</strong> auf <strong>Ja</strong>festgelegt ist, können alle Endbenutzer in Ihrem Verzeichnis für die Kennwortrücksetzung am <a href="http://aka.ms/ssprsetup">http://aka.ms/ssprsetup</a>registrieren.
                  </li>
                </ul>
                <p>
                  <strong>Kennwort zurücksetzen Portal:</strong>
                </p>
                <ul>
                  <li class="unordered">
Wenn der <strong>Zugriff auf das Zurücksetzen von Kennwörtern beschränken</strong> auf <strong>Ja</strong>festgelegt ist, werden nur Endbenutzern in dieser Gruppe ihre Kennwörter zurücksetzen wäre.<br><br></li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>
                <div id="authentication-methods-available-to-users">
                  <p>Authentifizierungsmethoden Benutzer bereitgestellt</p>
                </div>
              </td>
              <td>
                <p>Legt fest, welche Aufgaben ein Benutzers zu verwenden, um das Kennwort zurückzusetzen zulässig ist.</p>
                <p>

                </p>
                <p>(Nur sichtbar, wenn <strong>Benutzer aktiviert Kennwort zurücksetzen</strong> auf <strong>Ja</strong>festgelegt ist).</p>
              </td>
              <td>
                <p>
                  <strong>Hinweis:</strong>
                </p>
                <ul>
                  <li class="unordered">
Mindestens eine Option muss ausgewählt sein.<br><br></li>
                  <li class="unordered">
Es wird dringend empfohlen, Aktivieren von Optionen für die Benutzer ihre Kennwörter zurücksetzen die größte Flexibilität verliehen mindestens 2.<br><br></li>
                  <li class="unordered">
Wenn Sie Fragen zur Sicherheit verwenden, empfehlen wir dringend, dass Sie diese in Verbindung mit einer anderen Authentifizierungsmethode verwenden, wie Sicherheitsfragen können weniger sicher als Telefon oder die e-Mail-basierte Kennwort zurückgesetzt Methoden werden an.<br><br></li>
                </ul>
                <p>
                  <strong>Welche Directory-Felder verwendet werden?</strong>
                </p>
                <ul>
                  <li class="unordered">
Telefon (Büro) entspricht dem Attribut <strong>Rufnummer</strong> für ein Benutzerobjekt im Verzeichnis.<br><br></li>
                  <li class="unordered">
Mobiltelefon entspricht entweder das <strong>Authentifizierung Mobile</strong> -Attribut (das nicht öffentlich sichtbar ist) oder das <strong>Mobiltelefon</strong> Attribut (das öffentlich sichtbar ist), und klicken Sie auf ein Benutzerobjekt im Verzeichnis.  Der Dienst zuerst überprüft, ob <strong>Authentifizierung Telefon</strong> Daten, und ist es keine präsentieren, greift auf das <strong>Mobiltelefon</strong> Attribut zurück.<br><br></li>
                  <li class="unordered">
Alternative e-Mail-Adresse entspricht entweder das <strong>Authentifizierung-e-Mail</strong> -Attribut (das nicht öffentlich sichtbar ist) oder das <strong>Alternative E-Mail</strong> -Attribut für ein Benutzerobjekt im Verzeichnis.  Der Dienst zuerst überprüft, ob Daten <strong>Authentifizierung-e-Mail</strong> , und ist es keine präsentieren, greift auf das <strong>Alternative E-Mail</strong> -Attribut zurück.<br><br></li>
                  <li class="unordered">
Fragen zur Sicherheit persönlich auf ein Benutzerobjekt im Verzeichnis gespeichert und können nur von Benutzern während der Registrierung beantwortet werden.  Aus Gründen der Sicherheit gibt es Möglichkeit zurzeit keine für einen Administrator voraus, bearbeiten oder Antworten auf diese Fragen finden Sie unter.<br><br></li>
                  <li class="unordered">
                    <strong>Hinweis: </strong>standardmäßig nur in die Cloud Rufnummer Attribute und Mobiltelefon in der Cloud-Verzeichnis aus Ihrem lokalen Verzeichnis synchronisiert werden.  Weitere Informationen über die lokale Attribute in der Cloud synchronisiert wurden, finden Sie unter <a href="https://msdn.microsoft.com/library/azure/dn764938.aspx">Attribute mit Azure Active Directory synchronisiert.</a><br><br></li>
                </ul>
                <p>
                  <strong>Registrierungsinformationen Portal:</strong>
                </p>
                <ul>
                  <li class="unordered">
Wirkt sich auf welche Authentifizierungsmethoden angezeigt werden, wenn Benutzer erfassen möchten.  Wenn Sie eine angegebenen Authentifizierungsmethode nicht aktivieren, werden Benutzer nicht automatisch für die Authentifizierungsmethode registriert sein.<br><br></li>
                  <li class="unordered">
                    <strong>Hinweis: </strong>Benutzer können zurzeit keine Verbindung zum Registrieren eigene Office-Telefonnummern; die von ihrem Administrator Authentifizierungsmethode definiert werden muss.<br><br></li>
                </ul>
                <p>
                  <strong>Kennwort zurücksetzen Portal an:</strong>
                </p>
                <ul>
                  <li class="unordered">
Legt fest, welche Authentifizierungsmethoden, die ein Benutzer als Herausforderung für einen angegebenen Überprüfungsschritt verwenden kann.  Angenommen, wenn ein Benutzer Daten in die <strong>Rufnummer</strong> und die <strong>Authentifizierung Telefon</strong> Felder in Azure Active Directory verfügt, können klicken Sie dann hingegen einer der folgenden Authentifizierungsmethoden Sie das Kennwort wiederherstellen.<br><br></li>
                  <li class="unordered">
                    <strong>Hinweis: </strong>Benutzer können ihr Kennwort zurücksetzen, wenn sie Daten in die Authentifizierungsmethoden präsentieren haben Sie als Administrator aktiviert haben.<br><br></li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>
                <div id="number-of-authentication-methods-required">
                  <p>Anzahl der Authentifizierungsmethoden erforderlich</p>
                </div>
              </td>
              <td>
                <p>Bestimmt die minimale Anzahl der verfügbaren Authentifizierungsmethoden, die ein Benutzer aufzurufen muss, um das Kennwort zurückzusetzen.</p>
                <p>

                </p>
                <p>(Nur angezeigt, wenn <strong>Benutzer aktiviert Kennwort zurücksetzen</strong> auf <strong>Ja</strong>festgelegt ist).</p>
              </td>
              <td>
                <p>
                  <strong>Hinweis:</strong>
                </p>
                <ul>
                  <li class="unordered">
1 oder 2 Authentifizierungsmethoden erforderlich festgelegt werden können.<br><br></li>
                </ul>
                <p>
                  <strong>Registrierungsinformationen Portal:</strong>
                </p>
                <ul>
                  <li class="unordered">
Legt fest, dass die minimale Anzahl von einem Benutzer Authentifizierungsmethoden registrieren muss, damit Sie mit der Registrierung Fertig stellen.<br><br></li>
                </ul>
                <p>
                  <strong>Kennwort zurücksetzen Portal an:</strong>
                </p>
                <ul>
                  <li class="unordered">
Wirkt sich auf Anzahl der Überprüfungsschritte ein Benutzer muss, damit Sie ein Kennwort zurücksetzen aufzurufen.  Ein Benutzer mit einer Information Authentifizierung wie (einen Anruf bei ihrem Telefon Office) oder eine e-Mail an ihre alternative e-Mail zur Überprüfung ihres Kontos ist ein Überprüfungsschritt definiert.<br><br></li>
                  <li class="unordered">
                    <strong>Hinweis:</strong> Wenn ein Benutzer keinen die erforderliche Menge von Authentifizierungsinformationen an vertretene Konto, um werden gemäß der Richtlinie Kennwort zurücksetzen erfolgreich definiert sind, dass Sie festgelegt haben, wird hingegen eine Fehlerseite angezeigt die sie Administrator und das Kennwort zurücksetzen anfordern anweisen wird.  <br><br></li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>
                <div id="number-of-questions-required-to-register">
                  <p>Anzahl der Fragen für die Registrierung erforderlich</p>
                </div>
              </td>
              <td>
                <p>Bestimmt die minimale Anzahl von Fragen, die ein Benutzer beim Registrieren für die Sicherheit Option Fragen beantworten muss.</p>
                <p>(Nur angezeigt, wenn das Kontrollkästchen <strong>Fragen zur Sicherheit</strong> aktiviert ist).</p>
              </td>
              <td>
                <p>
                  <strong>Hinweis:</strong>
                </p>
                <ul>
                  <li class="unordered">
3 bis 5 Fragen für die Registrierung erforderlich festgelegt werden können.<br><br></li>
                  <li class="unordered">
Anzahl der Fragen für die Registrierung erforderlich muss größer oder gleich der Anzahl von Fragen zum Zurücksetzen erforderlich.<br><br></li>
                  <li class="unordered">
Es empfiehlt sich, dass Sie festlegen, dass die Anzahl der Fragen erforderlich registrieren, um größer als die Anzahl zurückgesetzt, sodass die Benutzer eine größere Flexibilität haben, wenn ihre Kennwörter zurücksetzen erforderlich sein.  Dies ist auch eine sicherer Konfiguration, da wir zufällig auswählen, werden die Fragen für den Benutzer aus dem Pool für alle Fragen beantworten, die sie registriert haben.<br><br></li>
                </ul>
                <p>
                  <strong>Registrierungsinformationen Portal:</strong>
                </p>
                <ul>
                  <li class="unordered">
Bestimmt die minimale Anzahl von Fragen, die ein Benutzer beantworten muss, bevor der Benutzer für das Zurücksetzen von Kennwörtern vollständig registrierten gilt.<br><br></li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>
                <div id="number-of-questions-required-to-reset">
                  <p>Anzahl von Fragen zum Zurücksetzen erforderlich </p>
                </div>
              </td>
              <td>
                <p>Bestimmt die minimale Anzahl von Fragen, die ein Benutzer beantworten muss, wenn ein Kennwort zurücksetzen.</p>
                <p>

                </p>
                <p>(Nur angezeigt, wenn das Kontrollkästchen <strong>Fragen zur Sicherheit</strong> aktiviert ist).</p>
              </td>
              <td>
                <p>
                  <strong>Hinweis:</strong>
                </p>
                <ul>
                  <li class="unordered">
3 bis 5 Fragen zum Zurücksetzen erforderlich festgelegt werden können.<br><br></li>
                  <li class="unordered">
Anzahl von Fragen zum Zurücksetzen erforderlich muss kleiner oder gleich der Anzahl von Fragen, die für die Registrierung erforderlich.<br><br></li>
                </ul>
                <p>
                  <strong>Kennwort zurücksetzen Portal an:</strong>
                </p>
                <ul>
                  <li class="unordered">
Bestimmt die minimale Anzahl von Fragen, die ein Benutzer beantworten muss, bevor das Kennwort des Benutzers zurückgesetzt werden kann.<br><br></li>
                  <li class="unordered">
Zum Zeitpunkt der das Zurücksetzen von Kennwörtern wird diese Anzahl Fragen zufällig aus insgesamt Benutzerliste registrierten Fragen ausgewählt werden.  Angenommen, wenn der Benutzer 5 Fragen registriert hat, wird 3 von 5 diese Fragen gefunden zufällig ausgewählt werden, wenn der Benutzer gehört zum Zurücksetzen des Kennworts.  Der Benutzer muss dann diese Fragen richtig beantworten, bevor das Kennwort zurückgesetzt werden kann.<br><br></li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>
                <div id="knowledge-based-security-questions">
                  <p>Knowledge Grundlage Sicherheitsfragen</p>
                </div>
              </td>
              <td>
                <p>Definiert die vorerstellte Sicherheitsfragen, die, denen die Benutzer aus, wenn der Registrierung für das Zurücksetzen von Kennwörtern und ihre Kennwörter zurücksetzen auswählen können.</p>
                <p>

                </p>
                <p>(Nur angezeigt, wenn das Kontrollkästchen <strong>Fragen zur Sicherheit</strong> aktiviert ist).</p>
              </td>
              <td>
                <p>
                  <strong>Hinweis:</strong>
                </p>
                <ul>
                  <li class="unordered">
Alle Knowledge-basierten Fragen werden in den vollständigen Satz von Office 365-Sprachen, die vom Browser-Gebietsschema des Benutzers basierend lokalisiert.<br><br></li>
                  <li class="unordered">
Bis zu 20 total Fragen werden kann (die Summe der Antworten auf Ihre Fragen benutzerdefinierten und Knowledge-basierten) definiert.<br><br></li>
                 <li class="unordered">
Min Antwort Zeichengrenze beträgt 3 Zeichen.<br><br></li>
                  <li class="unordered">
Max Antwort Zeichengrenze beträgt 40 Zeichen.<br><br></li>
                  <li class="unordered">
Benutzer können nicht zweimal die gleiche Frage beantworten.<br><br></li>
                  <li class="unordered">
Benutzer können nicht zweimal vorsehen dieselbe Antwort auf zwei verschiedenen Fragen.<br><br></li>
                  <li class="unordered">
Alle Zeichensatz möglicherweise zum Definieren von Antworten (einschließlich Unicode-Zeichen) verwendet werden.<br><br></li>
                  <li class="unordered">
Die Anzahl der Fragen definiert muss größer oder gleich der Anzahl von Fragen, die für die Registrierung erforderlich.<br><br></li>
                </ul>
                <p>
                  <strong>Registrierungsinformationen Portal:</strong>
                </p>
                <ul>
                  <li class="unordered">
Legt fest, welche Fragen ein Benutzer beim Registrieren für das Zurücksetzen von Kennwörtern für beantworten kann.<br><br></li>
                </ul>
                <p>
                  <strong>Kennwort zurücksetzen Portal an:</strong>
                </p>
                <ul>
                  <li class="unordered">
Legt fest, welche Fragen ein Benutzers verwenden Sie ein Kennwort zurücksetzen.<br><br></li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>
                <div id="custom-security-questions">
                  <p>Benutzerdefinierte Fragen zur Sicherheit</p>
                </div>
              </td>
              <td>
                <p>Definiert die Sicherheitsfragen, die, denen die Benutzer aus, wenn der Registrierung für das Zurücksetzen von Kennwörtern und ihre Kennwörter zurücksetzen auswählen können.</p>
                <p>

                </p>
                <p>(Nur angezeigt, wenn das Kontrollkästchen <strong>Fragen zur Sicherheit</strong> aktiviert ist).</p>
              </td>
              <td>
                <p>
                  <strong>Hinweis:</strong>
                </p>
                <ul>
                  <li class="unordered">
Bis zu 20 total Fragen werden kann (die Summe der Antworten auf Ihre Fragen benutzerdefinierten und Knowledge-basierten) definiert.<br><br></li>
                  <li class="unordered">
Max Frage Zeichengrenze ist 200 Zeichen.<br><br></li>
                  <li class="unordered">
Min Antwort Zeichengrenze beträgt 3 Zeichen.<br><br></li>
                  <li class="unordered">
Max Antwort Zeichengrenze beträgt 40 Zeichen.<br><br></li>
                  <li class="unordered">
Benutzer können nicht zweimal die gleiche Frage beantworten.<br><br></li>
                  <li class="unordered">
Benutzer können nicht zweimal vorsehen dieselbe Antwort auf zwei verschiedenen Fragen.<br><br></li>
                  <li class="unordered">
Alle Zeichensatz möglicherweise zum Definieren von Fragen und Antworten (einschließlich Unicode-Zeichen) verwendet werden.<br><br></li>
                  <li class="unordered">
Die Anzahl der Fragen definiert muss größer oder gleich der Anzahl von Fragen, die für die Registrierung erforderlich.<br><br></li>
                  <li class="unordered">
Definieren von verschiedenen Fragen für unterschiedliche Gebietsschemas ist für benutzerdefinierte Fragen nicht unterstützt.  Alle benutzerdefinierte Fragen erscheint in die Sprache aus, in der Sie sie in der Benutzeroberfläche des administrativen eingeben, selbst wenn das Gebietsschema des Benutzers Browser unterscheidet.  Verwenden Sie stattdessen die "Kenntnisse basierend" Fragen, wenn Sie diese Fragen, die lokalisiert benötigen.<br><br></li>
                </ul>
                <p>
                  <strong>Registrierungsinformationen Portal:</strong>
                </p>
                <ul>
                  <li class="unordered">
Legt fest, welche Fragen ein Benutzer beim Registrieren für das Zurücksetzen von Kennwörtern für beantworten kann.<br><br></li>
                </ul>
                <p>
                  <strong>Kennwort zurücksetzen Portal an:</strong>
                </p>
                <ul>
                  <li class="unordered">
Legt fest, welche Fragen ein Benutzers verwenden Sie ein Kennwort zurücksetzen.<br><br></li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>
                <div id="require-users-to-register-when-signing-in">
                  <p>Festlegen, dass Benutzer registrieren Sie sich beim Anmelden?</p>
                </div>
                <p>

                </p>
              </td>
              <td>
                <p>Bestimmt ist ein Benutzer erfassen müssen Kontaktdaten für Kennwort zurücksetzen das nächste Mal er oder Anna anmeldet.  
                </p>
                <p>Diese Funktion kann auf eine Anmeldeseite, die einem geschäftlichen oder schulnotizbücher-Konto verwendet werden.  Solche Seiten umfassen alle Office 365, die Azure-Verwaltungsportal, im Bereich Access und partnerverbundkontakte oder Benutzerdefiniert entwickelt Programme, die mit Azure AD anmelden.
                </p>
                <p>

                </p>
                <p>Erzwungenen Registrierung gilt nur für Benutzer, die für das Zurücksetzen von Kennwörtern aktiviert sind, also wenn Sie das Feature "Beschränken des Zugriffs auf das Zurücksetzen von Kennwörtern" und Suchbegriffs Kennwortrücksetzung eine bestimmte Gruppe von Benutzern, und klicken Sie dann auf nur Benutzer in dieser Gruppe verwendet haben werden erfassen für die Kennwortrücksetzung beim Anmelden müssen.</p>
                <p>

                </p>
                <p>(Nur angezeigt, wenn <strong>Benutzer aktiviert Kennwort zurücksetzen</strong> auf <strong>Ja</strong>festgelegt ist).</p>
              </td>
              <td>
                <p>

                </p>
                <p>

                </p>
                <p>
                  <strong>Hinweis:</strong>
                </p>
                <ul>
                  <li class="unordered">
Wenn Sie dieses Feature deaktivieren, können Sie Benutzer auch manuell zu <a href="http://aka.ms/ssprsetup">http://aka.ms/ssprsetup</a> registrieren ihre Kontaktdaten senden.  <br><br></li>
                  <li class="unordered">
Benutzer können das Registrierung Portal durch Klicken auf den Link <strong>Melden Sie sich für das Zurücksetzen von Kennwörtern</strong> unter der Registerkarte Profil im Bereich Access auch erreichen.<br><br></li>
                  <li class="unordered">
Nicht Registrierung über diese Methode kann durch Klicken auf die Schaltfläche Abbrechen oder Schließen des Fensters angezeigt werden, und Benutzer werden jedes Mal, wenn sie registrieren, wenn er nicht registrieren gebettelt werden.<br><br></li>
                </ul>
                <p>
                  <strong>Registrierungsinformationen Portal:</strong>
                </p>
                <ul>
                  <li class="unordered">
Diese Einstellung wirkt sich nicht auf das Verhalten des Portals Registrierung selbst, lieber, es legt fest, ob das Registrierung Portal für Benutzer angezeigt wird, wenn im Bereich Access anzumelden.<br><br></li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>
                <div id="number-of-days-before-users-must-confirm-their-contact-data">
                  <p>Anzahl der Tage, bevor die Benutzer ihre Kontaktdaten bestätigen müssen</p>
                </div>
              </td>
              <td>
                <p>Wenn Sie <strong>festlegen, dass Benutzer registrieren</strong> aktiviert ist, bestimmt diese Einstellung den Zeitraum der verstreichen müssen, bevor ein Benutzer ihre Daten erneut bestätigen muss. </p>
                <p>

                </p>
                <p>(Nur angezeigt, wenn <strong>Benutzer beim Anmelden bei der Access-Systemsteuerung registrieren erforderlich</strong> auf <strong>Ja</strong>festgelegt ist).</p>
              </td>
              <td>
                <p>

                </p>
                <p>

                </p>
                <p>
                  <strong>Hinweis:</strong>
                </p>
                <ul>
                  <li class="unordered">
Werte zwischen 0-730 Tage akzeptiert werden, mit 0 Tage, was bedeutet, dass Benutzer nie aufgefordert werden, ihre Kontaktdaten erneut zu bestätigen.<br><br></li>
                </ul>
                <p>
                  <strong>Registrierungsinformationen Portal:</strong>
                </p>
                <ul>
                  <li class="unordered">
Diese Einstellung wirkt sich nicht auf das Verhalten des Portals Registrierung selbst, lieber, es legt fest, ob das Registrierung Portal für Benutzer angezeigt wird, wenn ihre Kontaktdaten reconfirmed werden müssen. <br><br></li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>
                <div id="customize-the-contact-your-administrator-link">
                  <p>Anpassen des Kontakts Ihr Administrator Link?</p>
                </div>
              </td>
              <td>
                <p>Steuert, unabhängig davon, ob den Kontakt der Administrator (Siehe links) angezeigten Link auf das Kennwort zurücksetzen Portal Wenn ein Fehler auftritt oder ein Benutzer auf ein Vorgang verweist auf eine benutzerdefinierte URL oder die e-Mail-Adresse zu lange warten muss.</p>
                <p>

                </p>
                <p>(Nur angezeigt, wenn <strong>Benutzer aktiviert Kennwort zurücksetzen</strong> auf <strong>Ja</strong>festgelegt ist).</p>
              </td>
              <td>
                <p>
                  <strong>Hinweis:</strong>
                </p>
                <ul>
                  <li class="unordered">
Wenn Sie diese Einstellung aktivieren, müssen Sie einer benutzerdefinierten URL oder die e-Mail-Adresse auswählen, indem Sie das Feld <strong>benutzerdefinierte e-Mail-Adresse oder Url</strong> direkt unterhalb dieser Einstellung ausfüllen.<br><br></li>
                </ul>
                <p>
                  <strong>Kennwort zurücksetzen Portal an:</strong>
                </p>
                <ul>
                  <li class="unordered">
Wenn einrichten keine Benutzer auf den Link hervorgehoben eine e-Mail-Nachricht an die primäre e-Mail-Adresse aller Mandanten Administratoren anfordern, dass das Kennwort zurückgesetzt werden weiterleitet werden.  Diese e-Mail-Nachricht wird anhand der folgenden Logik verteilt:<br><br></li>
                  <li class="unordered">
                    <ul>
                      <li class="unordered">
Falls vorhanden sind Administratoren Kennwort ein, senden Sie die e-Mail an alle Kennwort Administratoren, bis zu einem Maximum von 100 Anzahl Empfänger ein.<br><br></li>
                      <li class="unordered">
Wenn es keine Kennwort-Administratoren gibt, senden Sie die e-Mail an alle Benutzeradministratoren, bis zu einem Maximum von 100 total Empfänger.<br><br></li>
                      <li class="unordered">
Wenn es keine Benutzeradministratoren sind, senden Sie die e-Mail an alle globalen Administratoren, bis zu einem Maximum von 100 Anzahl Empfänger ein.<br><br></li>
                    </ul>
                  </li>
                  <li class="unordered">
Wenn Sie auf Ja festgelegt, diese Einstellung angepasst wird das Verhalten des markierten links oben auf einer benutzerdefinierten URL oder die e-Mail-Adresse verweisen, zu denen Ihre Benutzer navigieren können, um Hilfe beim Zurücksetzen des Kennworts zu erhalten.<br><br></li>
                  <li class="unordered">
Wenn Sie eine URL angeben, wird es in einer neuen Registerkarte geöffnet.<br><br></li>
                  <li class="unordered">
Wenn Sie eine e-Mail-Adresse angeben, erstellen wir einen e-Mail-Link an die e-Mail-Adresse.<br><br></li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>
                <div id="custom-email-address-or-URL">
                  <p>Benutzerdefinierte e-Mail-Adresse oder URL</p>
                </div>
              </td>
              <td>
                <p>Steuert die e-Mail-Adresse oder URL, <strong>wenden Sie sich an den Administrator</strong> , dem Punkt verknüpfen. </p>
                <p>

                </p>
                <p>(Nur sichtbare If <strong>anpassen, wenden Sie sich an Ihr Administrator Link</strong> auf <strong>Ja</strong>festgelegt ist).</p>
              </td>
              <td>
                <p>
                  <strong>Hinweis:</strong>
                </p>
                <ul>
                  <li class="unordered">
Eine gültige Intranet oder extranet-URL oder die e-Mail-Adresse muss sein.<br><br></li>
                </ul>
                <p>
                  <strong>Kennwort zurücksetzen Portal an:</strong>
                </p>
                <ul>
                  <li class="unordered">
Ändert sich der Link <strong>wenden Sie sich an den Administrator</strong> , wo verweist.<br><br></li>
                  <li class="unordered">
Wenn Sie eine e-Mail-Adresse angeben, wird der Link "Mailto" Link an die e-Mail-Adresse werden.<br><br></li>
                  <li class="unordered">
Wenn Sie die URL ein, werden die Verknüpfung eines standard-Referenz auf die URL, die in einer neuen Registerkarte geöffnet wird.  <br><br></li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>
                <div id="write-back-passwords-to-on-premises-directory">
                  <p>Schreiben der lokalen Verzeichnis Kennwörter</p>
                </div>
              </td>
              <td>
                <p>Steuert, ob Kennwort abgeschlossenen Writebackvorgängen für dieses Verzeichnis aktiviert ist, und wenn abgeschlossenen writebackvorgängen aktiviert ist, zeigt den Status des lokalen abgeschlossenen writebackvorgängen Dienst.</p>
                <p>

                </p>
                <p>Diese Einstellung ist ist sinnvoll, wenn Sie den Dienst vorübergehend zu deaktivieren, ohne erneut konfigurieren Azure AD-Verbindung herstellen möchten.</p>
              </td>
              <td>
                <p>

                </p>
                <p>
                  <strong>Hinweis:</strong>
                </p>
                <ul>
                  <li class="unordered">
Dieses Steuerelement wird nur angezeigt, wenn Sie das Kennwort abgeschlossenen Writebackvorgängen installiert haben, indem Sie Herunterladen der neuesten Version von Azure AD verbinden, und aktivieren die Option <strong>Kennwort abgeschlossenen Writebackvorgängen</strong> unter dem <strong>optionale Features</strong> Auswahlbildschirm.<br><br></li>
                  <li class="unordered">
Wenn Sie den Eindruck, dass es ist ein Problem mit dem Dienst Kennwort abgeschlossenen Writebackvorgängen aktiviert haben, können Sie auf dieser Registerkarte stammen und sehen Sie sich die Bezeichnung <strong>Kennwort wieder zu Dienststatus schreiben</strong> , um festzustellen, ob eine Bedingung falsch ist.<br><br></li>
                  <li class="unordered">
Status, die angezeigt werden können, sind:<br><br><ul><li class="unordered"><strong>Konfiguriert </strong>– alles wie erwartet funktioniert<br><br></li><li class="unordered"><strong>Nicht konfiguriert</strong> – Sie abgeschlossenen writebackvorgängen installiert haben, aber wir können nicht erreicht haben, ab dem Dienst, überprüfen, um sicherzustellen, dass Sie sind nicht ausgehende Verbindungen zu 443 blockieren und versuchen Sie den Dienst erneut zu installieren, wenn das Problem weiterhin besteht.<br><br></li></ul></li>
                </ul>
                <p>
                  <strong>Registrierungsinformationen Portal:</strong>
                </p>
                <ul>
                  <li class="unordered">
Wenn abgeschlossenen writebackvorgängen bereitgestellt und konfiguriert ist und dieser Schalter ist auf <strong>keine</strong>, und klicken Sie dann abgeschlossenen writebackvorgängen werden deaktiviert und Verbund und Kennwort Hash synchronisieren möchten werden Benutzer werden nicht zum Registrieren für Kennwort ihre Kennwörter zurücksetzen.<br><br></li>
                  <li class="unordered">
Wenn die Option auf <strong>Ja</strong>festgelegt ist, klicken Sie dann abgeschlossenen writebackvorgängen wird aktiviert, und Verbund und Kennwort Hash synchronisieren möchten werden Benutzer können ihre Kennwörter zurücksetzen.<br><br></li>
                </ul>
                <p>
                  <strong>Kennwort zurücksetzen Portal an:</strong>
                </p>
                <ul>
                  <li class="unordered">
Wenn der abgeschlossenen writebackvorgängen bereitgestellt und konfiguriert ist und dieser Schalter auf <strong>Nein</strong>festgelegt ist, klicken Sie dann abgeschlossenen writebackvorgängen werden deaktiviert und Verbund und Kennwort Hash synchronisieren möchten werden Benutzer nicht ihre Kennwörter zurücksetzen können.<br><br></li>
                  <li class="unordered">
Wenn die Option auf <strong>Ja</strong>festgelegt ist, klicken Sie dann abgeschlossenen writebackvorgängen wird aktiviert, und Verbund und Kennwort Hash synchronisieren möchten werden Benutzer können ihre Kennwörter zurücksetzen.<br><br></li>
                </ul>
              </td>
            </tr>
             <tr>
              <td>
                <div id="allow-users-to-unlock-accounts-without-resetting-their-password">
                  <p>Benutzerberechtigungen Sie zum Entsperren von Konten ohne ihr Kennwort zurückzusetzen</p>
                </div>
              </td>
              <td>

              <p>Angibt, ob Benutzer, die das Kennwort zurücksetzen Portal finden Sie auf die Option zum Entsperren ihren lokalen Active Directory-Konten ohne das Zurücksetzen ihres Kennworts gewährt werden soll. Standardmäßig Azure AD werden immer Entsperren von Konten beim Zurücksetzen eines Kennworts durchführen, mit dieser Einstellung können Sie diese beiden Operationen trennen.</p>

              <p>Wenn Sie auf "Ja" eingestellt, dann Benutzer gewährt werden die Option zum Zurücksetzen ihres Kennworts, und heben Sie die Sperre oder Aufheben der Sperrung ohne das Kennwort zurückzusetzen. </p>

              <p>Wenn zum Ausführen einer kombinierten Kennwort zurücksetzen und Konto entsperren Vorgang festlegen auf "Nein", und klicken Sie dann Benutzern nur werden können.</p>

              </td>
              <td>
                <p>
                  <strong>Hinweis:</strong>
                </p>
                <ul>
                  <li class="unordered">
Dieses Feature verwenden möchten, müssen Sie die August 2015 installieren oder zur späteren Freigabe von Azure AD verbinden (V. 1.0.8667.0 oder höher).<br><br><a href="http://www.microsoft.com/download/details.aspx?id=47594">Klicken Sie hier, um die neueste Version von Azure AD verbinden herunterladen.</a></li>

                  <li class="unordered">
                    <strong>Hinweis:</strong> Um dieses Feature zu testen, müssen Sie Kennwort abgeschlossenen writebackvorgängen aktivieren, und verwenden Sie ein Konto aus, die aus lokalen (wie etwa ein partnerverbundkontakte oder Kennwort synchronisierte Benutzer) zugegriffen werden konnte und verfügt über ein Konto gesperrt.  Benutzer, die nicht von lokal stammen und einem gesperrten Konto verfügen nicht über werden die Option zum Entsperren von ihren Konten nicht angezeigt werden.</li>
                </ul>
                <p>
                  <strong>Kennwort zurücksetzen Portal an:</strong>
                </p>
                <ul>
                  <li class="unordered">
Nachdem Portal aktivieren diese Option, wenn das Kennwort ein Benutzers mit einem lokalen Konto an, die gesperrt ist ankommt zurückgesetzt werden soll, wird er oder sie die Option aus, um ihr Konto entsperren, ohne das Kennwort zurückzusetzen zugewiesen werden.<br><br>Beachten Sie, dass Sie bei der Verwendung von Kennwort abgeschlossenen writebackvorgängen Konten bereits automatisch nicht gesperrte Abschnitte, wenn das Kennwort zurückgesetzt wird und diese Option einfach Vorgängen trennt.<br><br>Dies ist eine besonders hilfreich, Option aktivieren, wenn Sie feststellen, dass Ihr Helpdesk, Anrufe von Konto generiert werden, viele Anfragen entsperren.</li>
                </ul>
              </td>
            </tr>
          </tbody></table>

## <a name="password-management-notifications"></a>Kennwort Management Benachrichtigungen
Die folgende Tabelle beschreibt, wie jedes Steuerelement wirkt sich auf die Benutzeroberfläche für Benutzer und Administratoren, die Kennwort zurücksetzen Benachrichtigungen erhalten.  Sie können diese Optionen unter dem Abschnitt **Benachrichtigungen** des Verzeichnisses **Konfigurieren** Registerkarte im [Verwaltungsportal Azure](https://manage.windowsazure.com)konfigurieren.

<table>
            <tbody><tr>
              <td>
                <p>
                  <strong>Policy-Steuerung</strong>
                </p>
              </td>
              <td>
                <p>
                  <strong>Beschreibung</strong>
                </p>
              </td>
              <td>
                <p>
                  <strong>Wirkt sich auf?</strong>
                </p>
              </td>
            </tr>
            <tr>
              <td>
                <div id="notify-admins-when-other-admins-reset-their-own-passwords">
                  <p>Administratoren benachrichtigt werden, wenn andere Administratoren eigene Kennwörter zurücksetzen.</p>
                </div>
              </td>
              <td>
                <p>Legt fest, ob alle globale Administratoren per e-Mail an ihre primäre e-Mail-Adresse benachrichtigt werden soll, wenn Sie einen anderen Administrator eines beliebigen Typs ein eigenes Kennwort zurückgesetzt.</p>
              </td>
              <td>
                <p>
                  <strong>Kennwort zurücksetzen Portal an:</strong>
                </p>
                <ul>
                  <li class="unordered">
Wenn Sie festlegen, um keine, und klicken Sie dann keine Benachrichtigungen gesendet werden.<br><br></li>
                  <li class="unordered">
Wenn auf Ja festgelegt ist, klicken Sie dann alle anderen globalen Administratoren benachrichtigt werden soll, wenn eine beliebige einzelne Administrator ein eigenes Kennwort zurückgesetzt.<br><br></li>
                  <li class="unordered">
Diese Benachrichtigung wird an die primäre e-Mail-Adressen andere globalen Administratoren in der Organisation über eine e-Mail-Nachricht gesendet.<br><br></li>
                </ul>
                <p>
                  <strong>Beispiel:</strong>
                </p>
                <ul>
                  <li class="unordered">
Wenn diese Option aktiviert wurde beim Administrator A setzt sein Kennwort, es gibt 3 andere Administratoren in den Mandanten, B, C und D, und Administratoren, B, C und D würde erhalten eine e-Mail, die angibt, Zurücksetzen der Admin-A sein Kennwort.<br><br></li>
                </ul>
              </td>
            </tr>
            <tr>
              <td>
                <div id="notify-users-and-admins-when-their-own-password-has-been-reset">
                  <p>Benutzer und Administratoren benachrichtigt werden, wenn ihr eigenes Kennwort zurückgesetzt wurde</p>
                </div>
              </td>
              <td>
                <p>Legt fest, ob Endbenutzer oder Administratoren, die eigene Kennwörter zurücksetzen eine e-Mail-Benachrichtigung erhalten, dass ihr Kennwort zurückgesetzt wurde.</p>
              </td>
              <td>
                <p>
                  <strong>Kennwort zurücksetzen Portal an:</strong>
                </p>
                <ul>
                  <li class="unordered">
Wenn Sie festlegen, um keine, und klicken Sie dann keine Benachrichtigungen gesendet werden.<br><br></li>
                  <li class="unordered">
Wenn Ja festgelegt, dann immer, wenn ein Benutzer oder Administrator sein eigenes, Zurücksetzen von Kennwörtern hingegen eine Benachrichtigung erhalten, die angibt, seinen oder sein Kennwort zurückgesetzt wurde.<br><br></li>
                  <li class="unordered">
Diese Benachrichtigung wird über eine e-Mail-Nachricht gesendet, Benutzerprinzipalnamen und alternative (oder Authentifizierung) e-Mail-Adresse des Benutzers, der das Kennwort zurückzusetzen.<br><br></li>
                </ul>
              </td>
            </tr>
          </tbody></table>


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Links zu Kennwort zurücksetzen Dokumentation
Nachstehend sind Links zu allen Seiten Dokumentation Azure AD Kennwort zurücksetzen aus:

* **Sie hier sind, weil Sie Probleme bei der Anmeldung Probleme auftreten?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) – erfahren Sie mehr über die sechs verschiedenen Komponenten des Dienst und was bedeutet jede
* [**Erste Schritte**](active-directory-passwords-getting-started.md) - erfahren Sie, wie Sie Benutzern gestatten, zurücksetzen und ihre Cloud oder lokalen Kennwörter ändern
* [**Bewährte Methoden**](active-directory-passwords-best-practices.md) - erfahren, wie Sie schnell bereitstellen und Verwalten von Kennwörtern in Ihrer Organisation effektiv
* [**Erste Einsichten**](active-directory-passwords-get-insights.md) – erfahren Sie mehr über unsere integrierten reporting-Funktionen
* [**Häufig gestellte Fragen**](active-directory-passwords-faq.md) - erhalten Sie Antworten auf häufig gestellte Fragen
* [**Problembehandlung**](active-directory-passwords-troubleshoot.md) – erfahren, wie Sie schnell Behandeln von Problemen mit dem Dienst
* [**Erfahren Sie mehr**](active-directory-passwords-learn-more.md) - wechseln tief in die technische Details der Funktionsweise des service


[001]: ./media/active-directory-passwords-customize/001.jpg "Image_001.jpg"
