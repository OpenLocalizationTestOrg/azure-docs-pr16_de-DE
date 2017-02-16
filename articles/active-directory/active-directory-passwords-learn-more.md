<properties
    pageTitle="Weitere Informationen: Azure AD-Kennwort Management | Microsoft Azure"
    description="Erweiterte Themen auf Kennwort abgeschlossenen writebackvorgängen Kennwort abgeschlossenen writebackvorgängen Sicherheit Funktionsweise, einschließlich Verwaltung Azure AD Kennwort zurücksetzen des Kennworts wie Portal funktioniert und welche Daten durch das Zurücksetzen von Kennwörtern verwendet werden."
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

# <a name="learn-more-about-password-management"></a>Erfahren Sie mehr über die Verwaltung der Kennwörter

> [AZURE.IMPORTANT] **Sie hier sind, weil Sie Probleme beim Anmelden haben?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).

Wenn Sie bereits Password Management bereitgestellt haben, oder erfahren Sie mehr über die Funktionsweise vor der Bereitstellung von an, in diesem Abschnitt Ihnen einen guten Überblick über die technischen Konzepte für den Dienst vermitteln wesentlichen technischen einfach gefunden werden. Wir werden folgende Punkte:

* [**Kennwort abgeschlossenen writebackvorgängen (Übersicht)**](#password-writeback-overview)
  - [Funktionsweise der Kennwrt abgeschlossenen writebackvorgängen](#how-password-writeback-works)
  - [Szenarien für das Kennwort abgeschlossenen writebackvorgängen unterstützt](#scenarios-supported-for-password-writeback)
  - [Kennwort abgeschlossenen writebackvorgängen Sicherheitsmodell](#password-writeback-security-model)
* [**Wie Zurücksetzen des Kennworts Portal arbeiten?**](#how-does-the-password-reset-portal-work)
  - [Welche Daten durch das Zurücksetzen von Kennwörtern verwendet werden?](#what-data-is-used-by-password-reset)
  - [Zugreifen auf Kennwort zurücksetzen Daten für Ihre Benutzer](#how-to-access-password-reset-data-for-your-users)

## <a name="password-writeback-overview"></a>Kennwort abgeschlossenen writebackvorgängen (Übersicht)
Kennwort abgeschlossenen writebackvorgängen ist eine [Azure Active Directory verbinden](active-directory-aadconnect.md) Komponente, die aktiviert und von der aktuellen Azure Active Directory Premium-Abonnenten verwendet werden kann. Weitere Informationen finden Sie unter [Azure-Active Directory-Editionen](active-directory-editions.md).

Kennwort abgeschlossenen writebackvorgängen können Sie Ihrem Mandanten Cloud zum Schreiben von Kennwörtern vorlesen lokalen Active Directory konfigurieren.  Es Sicherheitsbelange Sie davor, einrichten und Verwalten einer komplizierte lokalen Self-service-Kennwort zurücksetzen Lösung abnimmt, und es auf einfache Weise cloudbasierten für Ihre Benutzer ihre lokalen-Kennwörter zurücksetzen, an welcher Stelle sie sich befinden.  Lesen Sie weiter für einige der wichtigsten Features von Kennwort abgeschlossenen writebackvorgängen:

- **0 (null) Verzögerung Feedback.**  Kennwort abgeschlossenen writebackvorgängen ist ein synchroner Vorgang.  Ihre Benutzer werden sofort benachrichtigt, wenn ihr Kennwort nicht Richtlinie gerecht oder konnte nicht zurücksetzen oder aus irgendeinem Grund geändert werden.
- **Unterstützt das Zurücksetzen von Kennwörtern für Benutzer mit AD FS oder andere Technologien Föderation.**  Mit Kennwort abgeschlossenen writebackvorgängen, solange die Partnerbenutzer-Konten in Ihrem Mandanten Azure AD-synchronisiert werden sie sind Lage zum Verwalten ihrer lokalen AD Kennwörter aus der Cloud.
- **Unterstützt das Zurücksetzen von Kennwörtern für Benutzer mit Kennwort Hash synchronisieren.** Wenn Sie der Kennwort zurücksetzen Dienst erkennt, dass es sich bei einem synchronisierten Benutzerkonto für Kennwort Hash synchronisieren aktiviert ist, wir sowohl des Kontos lokalen und Zurücksetzen Cloud Kennwort gleichzeitig.
- **Unterstützt das Ändern von Kennwörtern aus dem Bereich Access und Office 365.**  Wenn partnerverbundkontakte hinzugefügt oder Kennwort synchronisieren würden Benutzer ihre abgelaufenen oder nicht abgelaufenen Kennwörter ändern stammen, Schreiben diese Kennwörter wieder bei Ihrer lokalen AD-Umgebung.
- **Unterstützt das Schreiben von wieder Kennwörter beim Administrator diese aus Zurücksetzen der** [**Azure-Verwaltungsportal**](https://manage.windowsazure.com).  Immer, wenn ein Administrator Zurücksetzen von Kennwörtern, das Kennwort eines Benutzers in der [Azure-Verwaltungsportal](https://manage.windowsazure.com), wenn der Benutzer verbunden ist oder Kennwort synchronisieren möchten, können wir das Kennwort festlegen, die, das der Administrator auf Ihrem lokalen AD ebenfalls markiert.  Dies ist der Verwaltungsportal von Office derzeit nicht unterstützt.
- **Erzwingt die lokalen AD Kennwortrichtlinien.**  Wenn ein Benutzer seine das Zurücksetzen von Kennwörtern, wir stellen Sie sicher, dass Ihre lokalen erfüllt AD-Richtlinie, bevor Sie ihn in dieses Verzeichnis weiterleiten.  Dies umfasst Verlauf, Komplexität, Alter, Kennwortfilter und andere Kennwort Einschränkungen, die Sie in Ihrem lokalen Active Directory definiert haben.
- **Nicht erforderlich alle eingehenden Firewall-Regeln.**  Kennwort abgeschlossenen writebackvorgängen verwendet ein Azure-Dienstbus Relay als zugrunde liegenden Communication Channel, was bedeutet, dass Sie nicht verfügen, um alle eingehenden Firewallports für dieses Feature für die Arbeit zu öffnen.
- **Ist nicht für Benutzerkonten unterstützt, die innerhalb der geschützten Gruppen in Ihrem lokalen Active Directory vorhanden sind.** Weitere Informationen zu geschützten Gruppen finden Sie unter [geschützten Konten und Gruppen in Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

### <a name="how-password-writeback-works"></a>Wie funktioniert das Kennwort abgeschlossenen writebackvorgängen
Kennwort abgeschlossenen writebackvorgängen besteht aus drei primäre Komponenten:

- Kennwort zurücksetzen Cloud-Dienst (Dies ist auch in Azure AD-Kennwort ändern Seiten integriert)
- Mandanten-spezifische Azure-Dienstbus relay
- Klicken Sie auf Prem Kennwort zurücksetzen Endpunkt

Sie passen zusammen in beschriebenen der unter Diagramm:

  ![][001]

Wenn ein Hash partnerverbundkontakte oder Kennwort synchronisieren möchten Benutzer gehört zum Zurücksetzen oder ändern das Kennwort in der Cloud, Folgendes:

1.  Wir überprüfen, welche Art von Kennwort des Benutzers verwendet.  Wenn wir angezeigt wird, dass das Kennwort lokal verwaltet wird, klicken Sie dann stellen wir sicher, dass der abgeschlossenen writebackvorgängen Dienst ausgeführt wird.  Wenn Ja, wir zulassen, dass den Benutzer den Vorgang fortsetzen, ist es nicht, wir informieren den Benutzer, die ihr Kennwort hier kann nicht zurückgesetzt werden.
2.  Als Nächstes der Benutzer der entsprechenden Authentifizierung Gates übergibt und erreicht Bildschirms Kennwort zurücksetzen.
3.  Der Benutzer ein neues Kennwort und es bestätigt.
4.  Beim Klicken auf Absenden, verschlüsseln wir das nur-Text-Kennwort mit einem symmetrischen Schlüssel, der während des Setupprozesses abgeschlossenen writebackvorgängen erstellt wurde.
5.  Nach dem Verschlüsseln des Kennworts, enthalten wir es in eine Nutzlast, die über einen HTTPS-Kanal an Ihr Mandanten bestimmte Service Bus Relay gesendet wird (, die wir auch für Sie während des Setupprozesses abgeschlossenen writebackvorgängen eingerichtet).  Diese Relay ist durch ein Kennwort erzeugten geschützt, die nur Ihre lokale Installation bekannt ist.
6.  Sobald die Meldung Dienstbus erreicht, wird der Kennwort zurücksetzen Endpunkt automatisch aktiviert wird, und sieht, dass es eine Zurücksetzen der Anforderung ausstehend hat.
7.  Der Dienst wird dann für den Benutzer in Frage mithilfe des Cloud Ankerattributs.  Für der Nachschlagevorgang erfolgreich ist das Benutzerobjekt muss vorhanden sein, in dem AD-Connector-Bereich, dem entsprechenden MV Objekt verknüpft sein und dem entsprechenden AAD Verbinder Objekt verknüpft sein. Schließlich in Reihenfolge für synchronisieren, um dieses Konto zu finden, die Verknüpfung zwischen AD Verbinder Objekt und MV müssen die Regel synchronisieren `Microsoft.InfromADUserAccountEnabled.xxx` auf den Link.  Dies ist erforderlich, da Wenn der Anruf aus der Cloud eingeht, synchronisieren-Engine wird das Attribut CloudAnchor verwendet wird, um das Objekt Verbinder AAD nachzuschlagen, und klicken Sie dann den Link wieder in das MV-Objekt folgt und klicken Sie dann den Link zurück zur AD-Objekt folgt. Da es sich möglicherweise mehrere AD-Objekte (mit mehreren Gesamtstrukturen) für den gleichen Benutzer, die Synchronisierung-Engine abhängig von der `Microsoft.InfromADUserAccountEnabled.xxx` Link zu den richtigen Arbeitsbereich auswählen.
8.  Nachdem das Benutzerkonto gefunden wird, versuchen Sie wir zum Zurücksetzen des Kennworts direkt in die entsprechenden Active Directory-Struktur.
9.  Wenn der Kennwort Set-Vorgang erfolgreich ist, informieren wir den Benutzer, dessen Kennwort geändert wurde und, die sie auf dem Weg Fröhliche wechseln können.
10. Festlegen des Kennworts fehl wir Fehler zurückgegeben, der für den Benutzer, und lassen, versuchen Sie es erneut.  Der Vorgang kann auftreten, da der Dienst, nach unten wurde, da das Kennwort ein, die sie ausgewählt Richtlinien des Unternehmens, nicht gerecht, da keine den Benutzer in der lokalen AD gefunden werden oder eine beliebige Anzahl von Gründen.  Wir haben eine bestimmte Nachricht für zahlreiche diesen Fällen und dem Benutzer mitteilen, was sie tun können, um das Problem zu beheben.

### <a name="scenarios-supported-for-password-writeback"></a>Szenarien für das Kennwort abgeschlossenen writebackvorgängen unterstützt
In der nachfolgenden Tabelle wird beschrieben, welche Szenarien für welche Versionen von unserem Synchronisierungsfunktionen unterstützt werden.  Im Allgemeinen wird dringend empfohlen, dass die neueste Version von [Azure AD verbinden](active-directory-aadconnect.md#install-azure-ad-connect) installieren, wenn Sie das Kennwort abgeschlossenen writebackvorgängen verwenden möchten.

  ![][002]

### <a name="password-writeback-security-model"></a>Kennwort abgeschlossenen writebackvorgängen Sicherheitsmodell
Kennwort abgeschlossenen writebackvorgängen ist ein äußerst sicheren und robuste-Dienst an.  Um sicherzustellen, dass Ihre Informationen geschützt ist, aktivieren wir ein 4 gestuft Sicherheitsmodell, das nachfolgend beschrieben ist.

- **Mandanten bestimmte Dienstbus Relay** – Wenn Sie den Dienst einrichten, richten wir Relay Bus Mandanten-spezifischen Dienst, die durch ein erzeugten sicheres Kennwort geschützt ist, die Microsoft nie auf zugreifen kann.
- **Locked down Verschlüsselungsschlüssels für verschlüsselte sicherer Kennwörter** – nachdem das Relay Service Bus erstellt wurde, erstellen wir einen signifikante wir verwenden, um das Kennwort verschlüsseln, wie über das Kabel vertrauen symmetrischen Schlüssel.  Dieser Schlüssel befindet sich nur im Ihres Unternehmens geheimen Store in der Cloud, die stark gesperrt und überwacht, genau wie jedes Kennwort im Verzeichnis.
- **Industry standard TLS** – Wenn Sie ein Kennwort zurücksetzen oder Ändern der Vorgang in der Cloud auftritt, wir nehmen das nur-Text-Kennwort ein, und es mit Ihren öffentlichen Schlüssel verschlüsseln.  Wir plop, die dann in eine HTTPS-Nachricht die über einen verschlüsselten Kanal mithilfe von Microsoft SSL-Zertifikate zu Ihrem Dienst Bus Relay gesendet wird.  Nachdem Sie diese Nachricht in Dienstbus erreicht, Ihre auf Prem Agent aktiviert wird, authentifiziert unter Verwendung des sicheren Kennworts, das zuvor erstellt wurde mit Service, nimmt die verschlüsselte Nachricht, entschlüsselt ihn mit den privaten Schlüssel, die, den wir erstellt, und versucht, das Kennwort über die AD DS-Kennwort setzen-API festgelegt.  Dieser Schritt ist uns Ihre AD auf Prem Kennwortrichtlinien (Komplexität, Alter, Verlauf, Filter usw.) erzwingen ermöglicht es, in der Cloud.
- **Richtlinien zum Kennwortablauf** – schließlich Wenn aus irgendeinem Grund die Meldung in Dienstbus befindet, da der Dienst auf Prem ausgefallen ist, wird er Timeout und nach mehreren Minuten entfernt, um noch feiner Sicherheit zu erhöhen.

## <a name="how-does-the-password-reset-portal-work"></a>Wie Zurücksetzen des Kennworts Portal arbeiten?
Wenn ein Benutzer zu navigiert Zurücksetzen des Kennworts Portal, ein Workflow wird gestartet, um festzustellen, ob das Benutzerkonto gültig ist welche Organisation, die Benutzer gehört, wobei das Kennwort des Benutzers verwaltet wird, und unabhängig davon, ob der Benutzer lizenziert ist, verwenden Sie das Feature.  Lesen Sie die folgenden Schritte zur wissen möchten, der Logik der Seiten zum Zurücksetzen des Kennworts.

1.  Benutzer klickt auf die nicht auf Ihr Konto Link oder wechselt direkt zu [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com)zugreifen.
2.  Benutzer gibt eine Benutzer-Id, und übergibt eine Captcha.
3.  Azure AD wird überprüft, ob der Benutzer dieses Feature verwenden können, indem Sie folgende Schritte ausführen kann:
    - Überprüft, ob der Benutzer dieses Feature aktiviert und eine zugeordnete Azure AD-Lizenz verfügt.
        - Wenn der Benutzer nicht dieses Feature aktiviert oder eine Lizenz ausgestattet ist, wird der Benutzer aufgefordert, Kontakt mit Ihnen aufnehmen vertretene Administrator, um das Kennwort zurückzusetzen.
    - Überprüft, dass der Benutzer über die Berechtigung verfügt Herausforderung Daten an die vertretene Konto gemäß Administratorrichtlinie definiert sind.
        - Wenn die Richtlinie nur eine Herausforderung erfordert, ist sichergestellt, dass Benutzer über die entsprechenden Daten für mindestens eine große Herausforderung aktiviert, indem Sie die Administratorrichtlinie definiert.
          - Wenn der Benutzer nicht so konfiguriert ist, wird dem Benutzer empfohlen, Kontakt mit Ihnen aufnehmen vertretene Administrator, um das Kennwort zurückzusetzen.
        - Wenn die Richtlinie zwei Probleme erforderlich ist, ist sichergestellt, dass Benutzer über die entsprechenden Daten für mindestens zwei die Probleme, die Administratorrichtlinie aktivierte definiert ist.
          - Wenn der Benutzer nicht konfiguriert ist, und klicken Sie dann wir der Benutzer ist geraten, wenden Sie sich an vertretene Administrator, um das Kennwort zurückzusetzen.
    - Überprüft, ob das Kennwort des Benutzers lokal (Verbund oder Kennwort Hash synchronisieren hatten) verwaltet wird oder nicht.
       - Abgeschlossenen writebackvorgängen bereitgestellt wird, und das Kennwort des Benutzers lokal verwaltet wird, kann der Benutzer fortgesetzt werden zum Authentifizieren und das Kennwort zurücksetzen.
       - Wenn nicht abgeschlossenen writebackvorgängen bereitgestellt wird, und das Kennwort des Benutzers lokal verwaltet wird, wird der Benutzer aufgefordert, Kontakt mit Ihnen aufnehmen vertretene Administrator, um das Kennwort zurückzusetzen.
4.  Wenn festgestellt wird, dass der Benutzer erfolgreich das Kennwort zurücksetzen kann, wird der Benutzer schrittweise durch das Zurücksetzen geleitet.

Weitere Informationen zum Bereitstellen von Kennwort abgeschlossenen writebackvorgängen am [Erste Schritte: Azure AD Password Management](active-directory-passwords-getting-started.md).

### <a name="what-data-is-used-by-password-reset"></a>Welche Daten durch das Zurücksetzen von Kennwörtern verwendet werden?
Die folgende Tabelle Gliederungen, wo und wie diese Daten, während das Zurücksetzen von Kennwörtern verwendet werden und Ihnen bei der Entscheidung helfen, welche Authentifizierungsoptionen für die für Ihre Organisation geeignet sind. Diese Tabelle enthält auch alle Formatierung Anforderungen für Fälle, in dem Sie Daten im Auftrag von Benutzern aus einer Pfaden bereitstellen, die diese Daten nicht überprüft werden.

> [AZURE.NOTE] Rufnummer wird im Portal Registration wird nicht angezeigt, da Benutzer derzeit nicht diese Eigenschaft im Verzeichnis bearbeiten können.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Name der Kontakt</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Datenelement Azure-Active Directory</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Verwendeten / festgelegt werden, in dem?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Format-Anforderungen</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Rufnummer</p>
            </td>
            <td>
              <p>Telefonnummer</p>
              <p>z. B. Set-MsolUser - UserPrincipalName JWarner@contoso.com - Telefonnummer "+ 1 1234567890 x 1234"</p>
            </td>
            <td>
              <p>In verwendet werden:</p>
              <p>Kennwort zurücksetzen-Portal</p>
              <p>Konfigurierbare aus:</p>
              <p>Telefonnummer ist PowerShell, DirSync, Azure-Verwaltungsportal und der Administratorportal Office festgelegt werden.</p>
            </td>
            <td>
              <p>+ ccc Xxxyyyzzzz (z. B. + 1 1234567890)</p>
              <ul>
                <li class="unordered">
Geben Sie einen Ländercode muss<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
(Falls zutreffend) bieten eine Vorwahl müssen<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Muss eine + im Vordergrund des Land Code unterstützen haben<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Ein Leerzeichen zwischen Landes- und der Rest der Nummer müssen<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Erweiterungen werden nicht unterstützt, wenn Sie alle Erweiterungen angegeben haben, wir linkparameter sie von der Zahl vor dem Verteilen der Anruf.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Mobiltelefon</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>ODER</p>
              <p>Kontaktdetails</p>
              <p>(Authentifizierung, Telefon verwendet wird, wenn es Daten vorhanden ist, greift andernfalls folgt zurück in das Feld Mobiltelefon).</p>
              <p>z. B. Set-MsolUser - UserPrincipalName JWarner@contoso.com - Kontaktdetails "+ 1 1234567890 x 1234"</p>
            </td>
            <td>
              <p>In verwendet werden:</p>
              <p>Kennwort zurücksetzen-Portal</p>
              <p>Registrierung-Portal</p>
              <p>Konfigurierbare aus: </p>
              <p>AuthenticationPhone kann aus dem Kennwort zurücksetzen Registrierung Portal oder MFA Registrierung Portal festgelegt werden.</p>
              <p>Kontaktdetails kann festgelegt werden, von PowerShell, DirSync, Azure-Verwaltungsportal und im Verwaltungsportal von Office</p>
            </td>
            <td>
              <p>+ ccc Xxxyyyzzzz (z. B. + 1 1234567890)</p>
              <ul>
                <li class="unordered">
Müssen einen Ländercode bereitstellen.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Muss eine Vorwahl (falls zutreffend) unterstützen.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Müssen haben ein + im Vordergrund des Land Code bereitstellen.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Sie müssen ein Leerzeichen zwischen Landes- und der Rest der Nummer.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Erweiterungen werden nicht unterstützt, wenn Sie alle Erweiterungen angegeben haben, wir ignorieren, wenn der Anruf weiterleitet.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Alternative E-Mail</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>ODER</p>
              <p>AlternateEmailAddresses [0] </p>
              <p>(Authentifizierung, die E-Mail verwendet wird, wenn die vorhandenen Daten vorhanden ist, greift andernfalls folgt zurück in das Feld alternative e-Mail-).</p>
              <p>Hinweis: das alternative e-Mail-Feld wird als ein Array von Zeichenfolgen im Verzeichnis angegeben.  Wir verwenden Sie den ersten Eintrag in folgendem Array.</p>
              <p>z. B. Set-MsolUser - UserPrincipalName JWarner@contoso.com - AlternateEmailAddresses"email@live.com"</p>
            </td>
            <td>
              <p>In verwendet werden:</p>
              <p>Kennwort zurücksetzen-Portal</p>
              <p>Registrierung-Portal</p>
              <p>Konfigurierbare aus: </p>
              <p>AuthenticationEmail kann aus dem Kennwort zurücksetzen Registrierung Portal oder MFA Registrierung Portal festgelegt werden.</p>
              <p>AlternateEmail kann festgelegt werden, von PowerShell, der Azure-Verwaltungsportal und im Verwaltungsportal von Office</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">user@domain.com</a>oder甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
E-Mails sollten Standardformate als pro folgen.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Unicode-e-Mails werden unterstützt.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Sicherheitsfragen und Antworten</p>
            </td>
            <td>
              <p>So ändern Sie direkt im Verzeichnis nicht verfügbar.</p>
            </td>
            <td>
              <p>In verwendet werden:</p>
              <p>Kennwort zurücksetzen-Portal</p>
              <p>Registrierung-Portal </p>
              <p>Konfigurierbare aus: </p>
              <p>Die einzige Möglichkeit zum Festlegen der Sicherheitsfragen erfolgt über die Azure-Verwaltungsportal.</p>
              <p>Die einzige Möglichkeit zum Antworten auf Fragen zur Sicherheit für einen bestimmten Benutzer festgelegt ist, über das Portal Registrierung.</p>
            </td>
            <td>
              <p>Sicherheit Fragen maximal 200 Zeichen und ein min von 3 Zeichen</p>
              <p>Antworten können, eine Max 40 Zeichen und ein min von 3 Zeichen</p>
            </td>
          </tr>
        </tbody></table>

###<a name="how-to-access-password-reset-data-for-your-users"></a>Zugreifen auf Kennwort zurücksetzen Daten für Ihre Benutzer
####<a name="data-settable-via-synchronization"></a>Daten über die Synchronisierung festgelegt werden.
Die folgenden Felder können aus lokalen synchronisiert werden:

* Mobiltelefon
* Rufnummer

####<a name="data-settable-with-azure-ad-powershell"></a>Daten mit Azure AD-PowerShell festgelegt werden.
Die folgenden Felder, die mit Azure AD-PowerShell und das Graph-API zugegriffen werden:

* Alternative E-Mail
* Mobiltelefon
* Rufnummer
* Authentifizierung Telefon
* Authentifizierung-e-Mail

####<a name="data-settable-with-registration-ui-only"></a>Daten mit der Registrierung nur Benutzeroberfläche festgelegt werden.
Die folgenden Felder sind nur verfügbar über die Registrierung SSPR Benutzeroberfläche (https://aka.ms/ssprsetup):

* Sicherheitsfragen und Antworten

####<a name="what-happens-when-a-user-registers"></a>Was geschieht, wenn ein Benutzer registriert?
Wenn ein Benutzer registriert, die Registrierungsseite wird **immer** festlegen die folgenden Felder:

* Authentifizierung Telefon
* Authentifizierung-e-Mail
* Sicherheitsfragen und Antworten

Wenn Sie einen Wert für **Mobiltelefon** oder die **Alternative e-Mail-**bereitgestellt haben, können Benutzer sofort die Sie ihre Kennwörter zurücksetzen, auch wenn sie für den Dienst registriert haben.  Darüber hinaus werden Benutzer finden Sie unter diese Werte ein, wenn Sie zum ersten Mal registrieren, und wenn gewünscht zu ändern.  Jedoch, nachdem er erfolgreich registriert haben, werden diese Werte in den Feldern **Telefon Authentifizierung** und **Authentifizierung e-Mail-** Hilfethemas beibehalten werden.

Dies kann eine geeignete Möglichkeit zum Aufheben der Blockierung großen Anzahl von Benutzern die Verwendung SSPR und dennoch Benutzer diese Informationen über die Registrierung zu überprüfen.

####<a name="setting-password-reset-data-with-powershell"></a>Einstellung Kennwort zurücksetzen Daten mit PowerShell
Sie können die Werte für die folgenden Felder mit Azure AD-PowerShell festlegen.

* Alternative E-Mail
* Mobiltelefon
* Rufnummer

Um anzufangen, müssen Sie zuerst [herunterladen](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)und installieren Sie das Azure AD-PowerShell-Modul.  Nachdem Sie es installiert haben, können Sie die folgenden so konfigurieren Sie jedes Feld Schritte.

#####<a name="alternate-email"></a>Alternative E-Mail
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

#####<a name="mobile-phone"></a>Mobiltelefon
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

#####<a name="office-phone"></a>Rufnummer
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

####<a name="reading-password-reset-data-with-powershell"></a>Lesebereich Kennwort zurücksetzen Daten mit PowerShell
Sie können die Werte für die folgenden Felder mit Azure AD-PowerShell lesen.

* Alternative E-Mail
* Mobiltelefon
* Rufnummer
* Authentifizierung Telefon
* Authentifizierung-e-Mail

Um anzufangen, müssen Sie zuerst [herunterladen](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)und installieren Sie das Azure AD-PowerShell-Modul.  Nachdem Sie es installiert haben, können Sie die folgenden so konfigurieren Sie jedes Feld Schritte.

#####<a name="alternate-email"></a>Alternative E-Mail
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

#####<a name="mobile-phone"></a>Mobiltelefon
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

#####<a name="office-phone"></a>Rufnummer
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

#####<a name="authentication-phone"></a>Authentifizierung Telefon
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

#####<a name="authentication-email"></a>Authentifizierung-e-Mail
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

## <a name="links-to-password-reset-documentation"></a>Links zu Kennwort zurücksetzen Dokumentation
Nachstehend sind Links zu allen Seiten Dokumentation Azure AD Kennwort zurücksetzen aus:

* **Sie hier sind, weil Sie Probleme beim Anmelden haben?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) – erfahren Sie mehr über die sechs verschiedenen Komponenten des Dienst und was bedeutet jede
* [**Erste Schritte**](active-directory-passwords-getting-started.md) - erfahren, wie Sie gestatten Sie Benutzern, zurücksetzen und ihre Cloud oder lokalen Kennwörter ändern
* [**Anpassen**](active-directory-passwords-customize.md) – erfahren Sie, wie Sie das gewünschte Aussehen und Verhalten und Verhalten des Diensts zu den Anforderungen Ihrer Organisation anpassen
* [**Bewährte Methoden**](active-directory-passwords-best-practices.md) - erfahren, wie Sie schnell bereitstellen und Verwalten von Kennwörtern in Ihrer Organisation effektiv
* [**Erste Einsichten**](active-directory-passwords-get-insights.md) – erfahren Sie mehr über unsere integrierten reporting-Funktionen
* [**Häufig gestellte Fragen**](active-directory-passwords-faq.md) - erhalten Sie Antworten auf häufig gestellte Fragen
* [**Problembehandlung**](active-directory-passwords-troubleshoot.md) – erfahren, wie Sie schnell Behandeln von Problemen mit dem Dienst



[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"
