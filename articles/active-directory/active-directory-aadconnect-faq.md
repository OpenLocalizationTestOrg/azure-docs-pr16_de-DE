<properties
    pageTitle="Azure AD verbinden: Häufig gestellte Fragen zu | Microsoft Azure"
    description="Diese Seite enthält häufig gestellte Fragen zu Azure AD verbinden."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-faq"></a>Azure AD verbinden häufig gestellte Fragen

## <a name="general-installation"></a>Allgemeine installation
**F: funktioniert Installation, wenn die Azure AD globaler Administrator 2FA aktiviert wurde?**  
Mit der Build vom Februar 2016 wird dies unterstützt.

**F: Gibt es eine Möglichkeit zum Azure AD verbinden unbeaufsichtigten installieren?**  
Es wird nur unterstützt, um Azure AD verbinden mithilfe des Assistenten zu installieren. Eine unbeaufsichtigte und automatische Installation wird nicht unterstützt.

**F: Ich habe eine Gesamtstruktur, wo eine Domäne nicht erreichbar ist. Wie installiere ich Azure AD-Verbindung herstellen?**  
Mit der Build vom Februar 2016 wird dies unterstützt.

**F: funktioniert der AD DS-Dienststatus-Agent auf Server Core?**  
Ja. Nach der Installation des Agents können Sie die Registrierung mithilfe der folgenden PowerShell-Cmdlet abzuschließen: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Netzwerk
**F: Ich habe eine Firewall, Netzwerkgeräte, oder etwas, das die Verbindungen Höchstdauer beschränkt kann in meinem Netzwerk geöffnet bleiben. Wie lange sollten Meine Client Seite Timeoutschwellenwert bei Verwendung von Azure AD-Verbindung herstellen?**  
Alle Netzwerke Software, physische Geräte oder Sonstiges, die die Höchstdauer beschränkt, die Verbindungen geöffnet bleiben können sollten einen Schwellenwert von mindestens 5 Minuten (300 Sekunden) für die Verbindung zwischen dem Server, in dem der Azure AD verbinden Client installiert ist, und Azure Active Directory. Dies gilt auch für alle zuvor veröffentlichten Microsoft Identity Synchronisierung Tools.

**F: sind SLDs (einzelnes Etikett Domänen) unterstützt?**  
Nein, Azure AD verbinden lokaler Gesamtstrukturen/Domänen unter Verwendung von SLDs nicht unterstützt.

**F: sind NetBios named "gepunktete" unterstützt?**  
Nein, Azure AD verbinden unterstützt keine lokalen Gesamtstrukturen/Domänen, in dem der NetBIOS-Name einen Punkt enthält "." in den Namen.

## <a name="federation"></a>Föderation
**F: Was mache ich, wenn ich eine e-Mail-Nachricht erhalten, die fragt mich mein Office 365-Zertifikat erneuern**  
Verwenden Sie die Anleitung, die im Thema [Zertifikate erneuern](active-directory-aadconnect-o365-certs.md) zum Zertifikat erneuern beschrieben ist.

**F: Ich habe "Automatisch aktualisieren Identitätsempfänger" für Office 365 Identitätsempfänger festlegen. Habe ich Maßnahmen ergreifen, wenn mein Token Signaturzertifikat automatisch übernommen?**  
Verwenden Sie die Anleitung, die im [Zertifikate erneuern](active-directory-aadconnect-o365-certs.md)Artikel beschrieben ist.

## <a name="environment"></a>Umgebung
**F: unterstützt ist es, um den Server umbenennen, nach der Installation von Azure AD-Verbindung herstellen?**  
Nein. Ändern den Namen des Servers bewirkt, dass die Synchronisierungs-Engine nicht die Verbindung zu der SQL-Datenbank herstellen können, und der Dienst nicht gestartet werden.

## <a name="identity-data"></a>Identitätsdaten
**F: das Attribut UPN (UserPrincipalName) in Azure AD-entspricht auf Prem Benutzerprinzipalnamen - nicht warum?**  
Finden Sie in folgenden Artikeln:

- [Benutzernamen in Office 365, Azure oder Intune überein nicht die lokale Benutzerprinzipalnamen oder alternative Anmelde-ID.](https://support.microsoft.com/en-us/kb/2523192)
- [Änderungen werden nicht mit dem Tool Azure Active Directory-Synchronisierung synchronisiert, nach der Änderung eines Benutzerkontos Benutzerprinzipalnamen zum Verwenden einer anderen verbunddomäne](https://support.microsoft.com/en-us/kb/2669550)

Sie können auch Azure AD zum Zulassen der Synchronisierungs-Engine UserPrincipalName aktualisieren, wie in [Azure AD verbinden synchronisieren Service-Features](active-directory-aadconnectsyncservice-features.md)beschrieben konfigurieren.

## <a name="custom-configuration"></a>Benutzerdefinierte Konfiguration
**F: Wo sind die PowerShell-Cmdlets für Azure AD verbinden bekannt?**  
Mit Ausnahme der Cmdlets dokumentierten auf dieser Website werden die anderen PowerShell-Cmdlets finden Sie im Azure AD Verbinden zur Verwendung für Kunden nicht unterstützt.

* *F: kann ich mithilfe von "Server exportieren/Server importieren" finden Sie im *Synchronisierung Dienst-Manager* zum Verschieben von Konfiguration zwischen Servers? **  
Nein. Diese Option wird nicht alle Einstellungen der Konfiguration abrufen und sollte nicht verwendet werden. Verwenden Sie stattdessen den Assistenten zum Erstellen von der grundlegenden Konfigurations auf dem zweiten Server und verwenden den Synchronisierung Regel-Editor zum Generieren der PowerShell-Skripts, um alle benutzerdefinierten Regel zwischen Servern zu verschieben. Finden Sie unter [benutzerdefinierte Konfiguration von aktiv in Bereitstellungsserver verschieben](active-directory-aadconnect-upgrade-previous-version.md#move-custom-configuration-from-active-to-staging-server).

## <a name="troubleshooting"></a>Behandlung von Problemen
**F: Wie erhalte ich Hilfe bei der Azure AD-Verbindung herstellen?**

[Suchen Sie im Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

- Suchen Sie im Microsoft Knowledge Base (KB) für technische Lösungen für häufige Probleme zur Unterstützung für Azure AD verbinden.

[Microsoft Azure-Active Directory-Foren](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

- Können für technischen Fragen und aus der Community Antworten oder eigene Frage stellen, indem Sie auf Suchen, und suchen Sie [hier](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Azure AD verbinden Kundensupport](https://manage.windowsazure.com/?getsupport=true)

- Verwenden Sie diesen Link, um Support über das Azure-Portal zu erhalten.
