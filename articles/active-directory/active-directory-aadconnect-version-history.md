<properties
   pageTitle="Azure AD verbinden: Versionsverlauf Release | Microsoft Azure"
   description="In diesem Thema werden alle Versionen von Azure AD verbinden und Azure-Active Directory-Synchronisierung"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-version-release-history"></a>Azure AD verbinden: Release-Versionsverlauf

Das Team Azure Active Directory aktualisiert regelmäßig Azure AD-Verbinden mit neuen Features und Funktionen. Nicht alle Ergänzungen gelten für alle Zielgruppen.

In diesem Artikel dient, mit deren Hilfe Sie die Versionen verfolgen, die freigegeben wurden, und Sie wissen, ob Sie auf die neueste Version oder nicht aktualisieren müssen.

Dies ist eine Liste der verwandten Themen:

Thema |  
--------- | --------- |
Schritte zum Aktualisieren von Azure AD-verbinden | Andere Methoden zum [Upgrade von einer früheren Version auf die neueste](active-directory-aadconnect-upgrade-previous-version.md) Azure AD verbinden Release.
Erforderliche Berechtigungen | Welche Berechtigungen zum Anwenden von Updates erforderlich sind finden Sie unter [Konten und Berechtigungen](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade)
Herunterladen| [Download Azure AD verbinden](http://go.microsoft.com/fwlink/?LinkId=615771)

## <a name="112810"></a>1.1.281.0
Zur Verfügung: 2016 August

**Feste Probleme:**

- Intervall synchronisieren Änderungen nicht durchgeführt erst nach Abschluss der nächsten Synchronisierung Zyklus.
- Azure AD verbinden-Assistent akzeptiert keine Azure AD-Konto, dessen Benutzername mit einem Unterstrich beginnt (\_).
- Azure AD verbinden-Assistenten nicht authentifiziert Azure AD-Konto bereitgestellt werden, wenn das Kontokennwort zu viele Sonderzeichen enthält. Fehlermeldung "kann nicht Anmeldeinformationen überprüfen. Ein unerwarteter Fehler aufgetreten." wird zurückgegeben.
- Deinstallieren von Bereitstellungsserver deaktiviert die Synchronisierung von Kennwörtern in Azure AD-Mandanten und bewirkt, dass die Synchronisierung von Kennwörtern mit active Server fehlschlägt.
- Synchronisierung von Kennwörtern fehlschlägt in ungewöhnliche Fälle, wenn keine Kennworthash gespeichert, auf die Benutzer vorhanden ist.
- Wenn für das staging Modus Azure AD verbinden Server aktiviert ist, ist Kennwort abgeschlossenen writebackvorgängen nicht vorübergehend deaktiviert.
- Azure AD verbinden-Assistent wird die Synchronisierung der tatsächlichen Kennwort und Kennwort abgeschlossenen writebackvorgängen Konfiguration bei Server befindet sich im staging-Modus nicht angezeigt. Es wird immer diese als deaktiviert angezeigt.
- Konfiguration Änderungen Synchronisierung von Kennwörtern und Kennwort abgeschlossenen writebackvorgängen werden beim Server befindet sich im staging-Modus nicht vom Assistenten Azure AD verbinden beibehalten.

**Verbesserte:**

- Dieses Cmdlet der aktualisierten Start-ADSyncSyncCycle um anzugeben, ob es erfolgreich einen neuer synchronisieren Zyklus oder nicht gestartet werden kann.
- Dieses Cmdlet der hinzugefügten beenden-ADSyncSyncCycle zum Beenden der Synchronisierung Kreis und ein Vorgang derzeit ausgeführt werden.
- Dieses Cmdlet der aktualisierten beenden-ADSyncScheduler zum Beenden der Synchronisierung Kreis und ein Vorgang derzeit ausgeführt werden.
- Beim [Verzeichnis Erweiterungen](active-directory-aadconnectsync-feature-directory-extensions.md) Azure AD verbinden-Assistenten konfigurieren, kann Active Directory-Attribut vom Typ "Teletex-Zeichenfolge" jetzt ausgewählt sein.

## <a name="111890"></a>1.1.189.0
Veröffentlicht: 2016 Juni

**Feste beseitigt und produktverbesserungen:**

- Verbinden von Azure AD kann nun auf einen kompatiblen FIPS-Server installiert werden.
    - Synchronisierung von Kennwörtern finden Sie unter [Kennwort synchronisieren und FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips)
- Ein Problem behoben, wo konnte ein NetBIOS-Name nicht auf den vollqualifizierten Domänennamen in der Active Directory-Connector aufgelöst werden.

## <a name="111800"></a>1.1.180.0
Veröffentlicht: 2016 Mai

**Neue Features:**

- Gibt eine Warnung aus, und hilft Ihnen der Überprüfung von Domänen, wenn Sie es nicht durchgearbeitet haben, vor dem Ausführen von Azure AD verbinden.
- Unterstützung für [Microsoft Cloud-Deutschland](active-directory-aadconnect-instances.md#microsoft-cloud-germany)hinzugefügt.
- Unterstützung für die neuesten [Microsoft Azure Government-Cloud](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) -Infrastruktur mit neuen URL-Anforderungen hinzugefügt.

**Feste beseitigt und produktverbesserungen:**

- Hinzugefügt Filtern zum Synchronisieren Regel-Editor zu synchronisieren Regeln suchen zu erleichtern.
- Verbesserte Leistung, wenn ein Leerzeichen Verbinder zu löschen.
- Feste Einheiten einer Probleme beim dasselbe Objekt sowohl gelöscht und in der gleichen ausführen (sogenannte löschen/hinzufügen) hinzugefügt wurde.
- Deaktivierte Regeln synchronisieren können nicht mehr reaktivieren im Lieferumfang von Objekten und Attributen auf Upgrade oder Verzeichnis Schema aktualisieren.

## <a name="111300"></a>1.1.130.0
Veröffentlicht: 2016 April

**Neue Features:**

- Unterstützung für mehrwertige Attribute auf [Verzeichnis Extensions](active-directory-aadconnectsync-feature-directory-extensions.md)hinzugefügt.
- Hinzugefügte Unterstützung für weitere Konfiguration Variationen für die [Automatische Aktualisierung](active-directory-aadconnect-feature-automatic-upgrade.md) berechtigt für das Upgrade vor berücksichtigt werden.
- Einige Cmdlets für [benutzerdefinierte Scheduler](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler)wird hinzugefügt.

## <a name="111190"></a>1.1.119.0
Veröffentlicht: 2016 März

**Feste Probleme:**

- Vorgenommen wird, dass seit Kennwort synchronisieren Express-Installation unter Windows Server 2008 (R2 Pre-ab) verwendet werden kann nicht auf diesem Betriebssystem unterstützt.
- Upgrade von DirSync mit einer benutzerdefinierten Filterkonfiguration nicht erwartungsgemäß.
- Beim Upgrade auf eine neuere Version, und es werden keine Änderungen an der Konfiguration, eine vollständige importieren/Synchronisierung nicht geplant werden soll.

## <a name="111100"></a>1.1.110.0
Zur Verfügung: 2016 Februar

**Feste Probleme:**

- Upgrades einer früheren Version funktioniert nicht, wenn die Installation nicht im Standardordner **C:\Programme Dateien** ist.
- Wenn Sie installieren und Aufheben der Markierung **Starten der Synchronisierung Prozess...** am Ende des Assistenten wird die Installationsassistenten erneut ausführen den Scheduler nicht aktivieren.
- Der Scheduler funktioniert nicht erwartungsgemäß auf Servern, wo das Datum/Uhrzeit-Format nicht US-En ist. Es blockiert auch `Get-ADSyncScheduler` korrekten Zeitpunkt zurück.
- Wenn Sie als die Anmeldung Option und das Upgrade eine frühere Version von Azure AD-Verbinden mit ADFS installiert haben, können nicht Sie den Installationsassistenten erneut ausführen.

## <a name="111050"></a>1.1.105.0
Zur Verfügung: 2016 Februar

**Neue Features:**

- Die Funktion für [Automatische Aktualisierung](active-directory-aadconnect-feature-automatic-upgrade.md) für Express-Einstellungen-Kunden.
- Unterstützung für die Verwendung von MFA und PIM im Assistenten zum Installieren globaler Administrator.
    - Sie müssen Ihre Proxy auch den Datenverkehr in https://secure.aadcdn.microsoftonline-p.com dürfen, wenn Sie MFA verwenden dürfen.
    - Sie müssen https://secure.aadcdn.microsoftonline-p.com für MFA ordnungsgemäß funktioniert Ihrer Liste der vertrauenswürdigen Websites hinzufügen.
- Ändern des Benutzers Anmeldung Methode nach Erstinstallation zulässig.
- Im Assistenten zum Installieren [Domäne und Organisationseinheit Filtern](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) zulassen. Dies ermöglicht außerdem das Herstellen einer Verbindung mit Gesamtstrukturen, wenn nicht alle Domänen verfügbar sind.
- [Planer](active-directory-aadconnectsync-feature-scheduler.md) ist in der Synchronisierungs-Engine integriert.

**Features für die Kundendaten von Seitenansicht in GA:**

- [Gerät abgeschlossenen writebackvorgängen](active-directory-aadconnect-feature-device-writeback.md).
- [Directory-Erweiterungen](active-directory-aadconnectsync-feature-directory-extensions.md).

**Neue Features von Preview:**

- Das neue Standardformat synchronisieren Zyklus das Intervall 30 Minuten ist. 3 Stunden für alle früheren Versionen werden verwendet. Fügt Support, um das Verhalten der [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) zu ändern.

**Feste Probleme:**

- Die Seite überprüfen DNS-Domänen haben immer die Domänen erkannt.
- Anweisungen für die Domäne Administrator-Anmeldeberechtigungen beim ADFS konfigurieren.
- Der lokalen AD-Konten werden vom Assistenten nicht erkannt, wenn befindet sich in einer Domäne mit einer anderen DNS-Struktur als die Domäne aus.

## <a name="1091310"></a>1.0.9131.0
Zur Verfügung: 2015 Dezember

**Feste Probleme:**

- Kennwort synchronisieren funktioniert möglicherweise nicht, wenn Sie die Kennwörter in AD DS, jedoch funktioniert ändern, wenn Sie ein Kennwort festlegen.
- Wenn Sie einen Proxyserver verfügen, Azure AD-Authentifizierung möglicherweise ein Fehler auftreten, während der Installation oder heben Sie die Markierung auf der Konfigurationsseite, Upgrade.
- SQL Server schlägt fehl, wenn Sie nicht SA in SQL sind aus einer früheren Version von Azure AD-Verbinden mit einem vollständigen aktualisieren.
- Aktualisieren von einer früheren Version von Azure AD-Verbinden mit einem Remote wird SQL Server die Fehlermeldung "Kein ADSync SQL-Datenbank Zugriff auf" angezeigt.

## <a name="1091250"></a>1.0.9125.0
Zur Verfügung: 2015 November

**Neue Features:**

- Die ADFS zum Azure AD-vertrauen können neu zu konfigurieren.
- Aktualisieren von Active Directory-Schema können und erneut synchronisieren Regeln generieren.
- Deaktivieren Sie eine Regel synchronisieren können.
- Definieren von "AuthoritativeNull" als neue Literal in einer Regel synchronisieren.

**Neue Features von Preview:**

- [Azure AD verbinden Gesundheit für synchronisieren](active-directory-aadconnect-health-sync.md).
- Unterstützung für die Synchronisierung von Kennwörtern [Azure Active Directory-Domänendiensten](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) .

**Neue unterstützte Szenario:**

- Mehrere lokalen Exchange-Organisationen unterstützt. Weitere Informationen finden Sie unter [hybridbereitstellungen mit mehreren Active Directory-Gesamtstrukturen](https://technet.microsoft.com/library/jj873754.aspx) .

**Feste Probleme:**

- Kennwort Synchronisierungsprobleme:
    - Objekt von außerhalb des Bereichs auf in Bereich verschoben wird nicht das Datenbankkennwort synchronisiert haben. Diese schichtigen Organisationseinheit sowohl Attribut zu filtern.
    - Markieren eine neue Organisationseinheit synchron aufnehmen möchten ist eine vollständige Kennwort Synchronisierung nicht erforderlich.
    - Wenn ein deaktivierter Benutzer aktiviert ist, ist das Kennwort nicht synchron.
    - Die Kennwort "Wiederholen" Warteschlange unbegrenzte und die vorherige Grenze von 5.000 Objekte zu entfernenden wurde entfernt.
    - [Verbesserte Problembehandlung](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization).
- Es können keine Verbindung mit Active Directory mit Windows Server 2016 Gesamtstruktur funktionsübergreifendes Ebene.
- Kann nicht zum Ändern der Gruppe für die Gruppe filtern nach Erstinstallation verwendet.
- Ein neues Benutzerprofil wird nicht mehr auf dem Server Azure AD verbinden für jeden Benutzer daran, wie Sie ein Kennwort ändern Kennwort abgeschlossenen writebackvorgängen aktiviert erstellt werden.
- Long Integer Werte synchron Regeln Bereiche kann nicht verwendet werden.
- Das Kontrollkästchen "Gerät abgeschlossenen writebackvorgängen" bleibt deaktiviert, wenn es nicht erreichbar Domänencontroller sind.

## <a name="1086670"></a>1.0.8667.0
Zur Verfügung: 2015 August

**Neue Features:**

- Der Assistent zum Installieren von Azure AD verbinden ist nun für alle Sprachen für Windows Server lokalisiert.
- Hinzugefügte Unterstützung für Konto entsperren bei Verwendung von Azure AD-Kennwort Management.

**Feste Probleme:**

- Assistent zum Installieren von Azure AD verbinden stürzt ab, wenn ein anderer Benutzer weiterhin Installation statt der Person, die zuerst die Installation gestartet.
- Wenn eine frühere Deinstallation Azure AD verbinden fehlschlägt Azure AD verbinden synchronisieren ordnungsgemäß deinstallieren können, ist es nicht möglich, neu zu installieren.
- Nicht installieren Azure AD Verbinden mit Express installieren, wenn der Benutzer nicht in der Domäne aus, der die Struktur ist oder eine nicht englischen Version von Active Directory verwendet wird.
- Wenn Sie der vollqualifizierten Domänennamen des Benutzerkontos Active Directory aufgelöst werden kann, wird eine irreführende Fehlermeldung "Fehler beim commit des Schemas" angezeigt.
- Wenn das Konto in den Active Directory-Connector verwendet außerhalb des Assistenten geändert wird, tritt der Assistent auf nachfolgende ausgeführt wird.
- Verbinden von Azure AD kann manchmal auf einem Domain Controller installieren.
- Kann nicht aktivieren und deaktivieren "Staging Modus" aus, wenn die Erweiterungsattribute hinzugefügt wurden.
- Kennwort abgeschlossenen writebackvorgängen, schlägt aufgrund eines falschen Kennworts auf Active Directory-Connector einige Konfiguration.
- DirSync kann nicht aktualisiert werden, wenn dn in Attribut Filtern verwendet wird.
- Übermäßige CPU-Auslastung, wenn das Zurücksetzen von Kennwörtern verwenden.

**Entfernte Preview-Features:**

- Der Preview-Features, die [Benutzer abgeschlossenen writebackvorgängen](active-directory-aadconnect-feature-preview.md#user-writeback) vorübergehend entfernt wurde, basierend auf Feedback von Kunden Vorschau. Es wird später erneut hinzugefügt werden, wenn wir die bereitgestellte Feedback berücksichtigt haben.

## <a name="1086410"></a>1.0.8641.0
Veröffentlicht: 2015 Juni

**Erste Version von Azure AD verbinden.**

Geänderte Name aus Azure AD synchronisieren, Azure AD verbinden.

**Neue Features:**

- [Express-Einstellungen](./connect/active-directory-aadconnect-get-started-express.md) -installation
- [Konfigurieren von ADFS](./connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) können
- [Aktualisieren von DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) können
- [Unbeabsichtigte Löschen sperren](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
- [Das staging von Modus](active-directory-aadconnectsync-operations.md#staging-mode) eingeführt

**Neue Features von Preview:**

- [Benutzer abgeschlossenen writebackvorgängen](active-directory-aadconnect-feature-preview.md#user-writeback)
- [Gruppe abgeschlossenen writebackvorgängen](active-directory-aadconnect-feature-preview.md#group-writeback)
- [Gerät abgeschlossenen writebackvorgängen](active-directory-aadconnect-feature-device-writeback.md)
- [Directory-Erweiterungen](active-directory-aadconnect-feature-preview.md#directory-extensions)


## <a name="104940501"></a>1.0.494.0501
Veröffentlicht: 2015 Mai

**Neue Anforderung:**

- Azure AD-Synchronisierung erforderlich ist jetzt der .net Framework-Version 4.5.1 installiert sein.

**Feste Probleme:**

- Kennwort abgeschlossenen writebackvorgängen von Azure AD-wird mit einer Servicebus-Konnektivitätsfehlern fehl.

## <a name="104910413"></a>1.0.491.0413
Veröffentlicht: 2015 April

**Feste beseitigt und produktverbesserungen:**

- Active Directory-Connector verarbeitet keine Löschvorgänge ordnungsgemäß, wenn der Papierkorb aktiviert ist, und es mehrere Domänen in der Gesamtstruktur gibt.
- Die Leistung von Importvorgänge wurde für Azure Active Directory Connector verbessert.
- Wenn verfügt über eine Gruppe hat die Mitgliedschaft überschritten (standardmäßig der Grenzwert ist festgelegt auf 50k Objekte), die Gruppe in Azure Active Directory gelöscht wurde. Das neue Verhalten ist, dass die Gruppe bleibt, ein Fehler ausgelöst wird und keine neuen Änderungen für die Mitgliedschaft exportiert werden.
- Wenn ein eingerichtetes löschen mit den gleichen DN bereits in dem Bereich Verbinder vorhanden ist, kann ein neues Objekt bereitgestellt werden.
- Einige Objekte werden für während einer Synchronisierungs Delta synchronisiert werden zwar keine bereitgestellt werden, klicken Sie auf das Objekt Änderung markiert.
- Erzwingen eine Synchronisierung Kennwort wird auch die Liste der bevorzugte DC entfernt.
- Probleme mit einigen Objekten Staaten verfügt CSExportAnalyzer

**Neue Features:**

- Eine Verknüpfung kann jetzt mit "ANY" Objekttyp im Metaverse verbinden.

## <a name="104850222"></a>1.0.485.0222
Zur Verfügung: 2015 Februar

**Verbesserte:**

- Verbesserte importieren optimale Leistung.

**Feste Probleme:**

- Kennwort synchronisieren berücksichtigt das CloudFiltered Attribut untersuchten Attribut Filtern nach. Gefilterte Objekte werden im Bereich für die Synchronisierung von Kennwörtern nicht mehr.
- In wenigen Situationen, in dem der Suchtopologie sehr viele Domänencontroller hatten, funktioniert nicht Kennwort synchronisieren.
- "Angehalten-Server" beim Importieren aus der Azure AD-Verbinder nach Device Management wurde in Azure AD/Intune aktiviert.
- Teilnehmen an einer Fremdschlüssel Sicherheit Hauptbenutzer (FSPs) aus mehreren Domänen in der gleichen Struktur führt zu einem Fehler mehrdeutige Join.

## <a name="104751202"></a>1.0.475.1202
Zur Verfügung: 2014 Dezember

**Neue Features:**

- Führen Sie die Synchronisierung von Kennwörtern basierendes Attribut Filtern wird jetzt unterstützt. Weitere Informationen hierzu finden Sie unter [Synchronisierung von Kennwörtern mit Filtern](active-directory-aadconnectsync-configure-filtering.md).
- Das Attribut MsDS-ExternalDirectoryObjectID wird wieder in AD geschrieben werden. Dies fügt Unterstützung für Office 365-Anwendungen mit OAuth2 auf beide, Online zugreifen und lokaler Postfächer in einer Exchange-Hybridbereitstellung.

**Feste Upgrade Probleme:**

- Eine neuere Version von der Anmelde-Assistenten wird auf dem Server verfügbar.
- Ein Pfad für die benutzerdefinierte Installation wurde verwendet, um die Azure AD synchronisieren zu installieren.
- Ein ungültiges benutzerdefinierte Verknüpfung Kriterium blockiert das Upgrade.

**Weitere Updates:**

- Feste Vorlagen für Office Pro Plus an.
- Installationsprobleme mit festen aufgrund von Benutzernamen, die mit einem Bindestrich beginnen.
- Dadurch die Einstellung SourceAnchor verlieren, wenn Sie den Assistenten ein zweites Mal ausführen.
- Feste ETW für die Synchronisierung von Kennwörtern

## <a name="104701023"></a>1.0.470.1023
Zur Verfügung: 2014 Oktober

**Neue Features:**

- Synchronisierung von Kennwörtern aus mehreren lokalen AD in Azure Active Directory.
- Installation lokalisierten Benutzeroberfläche für alle Sprachen für Windows Server.

**Aktualisieren von AADSync 1.0 GA**

Wenn Sie bereits Azure AD synchronisieren installiert haben, gibt es einen weiterer Schritt, die, den Sie haben, wird für den Fall, dass Sie eine Out-of-Box-Synchronisierung geändert haben. Nachdem Sie auf die 1.0.470.1023 aktualisiert haben lassen Sie, Sie die Synchronisierung, die Regeln, die Sie geändert haben doppelt vorhanden sind. Für jede geänderte synchronisieren Regel folgendermaßen vor:

- Suchen Sie die Regel synchronisieren Sie geändert haben, und notieren Sie sich die Änderungen vor.
- Löschen Sie die Regel synchronisieren.
- Suchen Sie die neue synchronisieren Regel erstellte Azure AD synchronisieren, und erneut anzuwenden Sie die Änderungen.

**Berechtigungen für die AD-Konto**

Active Directory-Konto muss die Kennworthashes aus dem Active Directory lesen können zusätzliche Berechtigungen erteilt werden. Die Berechtigungen zum erteilen sind "Repliziert Directory Änderungen" und "Repliziert Directory Änderungen alle" benannte. Beide Berechtigungen sind erforderlich, um die Kennworthashes lesen können

## <a name="104190911"></a>1.0.419.0911
Zur Verfügung: 2014 September

**Erste Version von Azure AD synchronisieren.**

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](active-directory-aadconnect.md).
