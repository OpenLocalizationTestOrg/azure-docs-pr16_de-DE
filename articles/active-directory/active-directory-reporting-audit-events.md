<properties
   pageTitle="Azure Active Directory Audit Bericht Ereignisse | Microsoft Azure"
   description="Überwacht Ereignisse, die zum Anzeigen und Herunterladen von Ihrer Azure Active Directory verfügbar sind"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/19/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-audit-report-events"></a>Azure Active Directory-Bericht von Ereignissen

*Diese Dokumentation ist Bestandteil der [Azure Active Directory Reporting Guide] (aktiv-Directory-reporting-guide.md).*

Azure Active Directory-Überwachungsbericht hilft Kunden berechtigte Aktionen zu identifizieren, die in ihren Azure Active Directory aufgetreten sind. Berechtigte Aktionen einschließen EoP Änderungen (z. B. Erstellen von Rollen oder das Zurücksetzen von Kennwörtern), geändert Richtlinienkonfigurationen (beispielsweise Kennwortrichtlinien) oder Änderungen Directory-Konfiguration (z. B. Änderungen Domäne Föderation Einstellungen). Die Berichte enthalten den Eintrag Audit Ereignis den Namen der Akteur, die Aktion, die Zielressource auswirken, die Änderung, und das Datum und die Uhrzeit (in UTC) ausgeführt hat. Kunden können die Liste der von Ereignissen für ihre Azure Active Directory über das [Portal Azure](https://portal.azure.com/)-abrufen wie in der [Ansicht Ihrer überwachenden Protokolle](active-directory-reporting-azure-portal.md)beschrieben.


## <a name="list-of-audit-report-events"></a>Liste der Bericht von Ereignissen
<!--- audit event descriptions should be in the past tense --->

Ereignisse                               | Beschreibung des Ereignisses
------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Benutzerereignisse**                      |
Fügen Sie Benutzer hinzu                             | Einen Benutzer hinzugefügt im Verzeichnis.
Benutzer löschen                          | Einen Benutzer aus dem Verzeichnis gelöscht.
Festlegen von Lizenzeigenschaften               | Festlegen der Lizenzeigenschaften für einen Benutzer im Verzeichnis.
Benutzer zum Zurücksetzen von Kennwörtern                  | Zurücksetzen des Kennworts für einen Benutzer im Verzeichnis.
Benutzerkennwort ändern                 | Das Kennwort für einen Benutzer im Verzeichnis geändert.
Benutzerlizenz ändern                  | Geändert von einem Benutzer im Verzeichnis zugewiesene Lizenz. Um anzuzeigen, welche Lizenzen aktualisiert wurden, schauen Sie sich die [Aktualisierung](#update-user-attributes) Benutzereigenschaften unten
Benutzer aktualisieren                          | Einen Benutzer im Verzeichnis wird aktualisiert. Sie [finden Sie unter](#update-user-attributes) Attributen, die aktualisiert werden kann.
Festlegen Force-Benutzerkennwort ändern       | Legen Sie die Eigenschaft, die erzwingt, dass einen Benutzer bei der Anmeldung das Kennwort ändern.
Aktualisieren von Benutzeranmeldeinformationen        | Benutzer hat das Kennwort geändert.  
**Gruppe Ereignisse**                     |
Gruppe hinzufügen                            | Erstellt eine Gruppe im Verzeichnis an.
Gruppe aktualisieren                         | Eine Gruppe im Verzeichnis wird aktualisiert. Um anzuzeigen, welche Gruppeneigenschaften aktualisiert wurden, finden Sie unten im Abschnitt [Gruppe Eigenschaften erfasst:](#update-group-attributes)
Gruppe löschen                         | Eine Gruppe aus dem Verzeichnis gelöscht.
Mitglied zur Gruppe hinzufügen            | Ein Mitglied hinzugefügt einer Gruppe im Verzeichnis.
Entfernen Sie Mitglied aus Gruppe.         | Mitglied entfernt aus einer Gruppe im Verzeichnis.
CreateGroupSettings              | Einstellungen für erstellten Gruppen
UpdateGroupSettings          | Aktualisierte gruppeneinstellungen. Um anzuzeigen, welche gruppeneinstellungen aktualisiert wurden, finden Sie unten im Abschnitt [Gruppe Eigenschaften erfasst:](#update-group-attributes)
DeleteGroupSettings            | Gelöschte gruppeneinstellungen
SetGroupLicense                  | Legen Sie die Gruppenlizenz.
SetGroupManagedBy              | Legen Sie eine Gruppe von Benutzer verwaltet werden
AddGroupMember               | Element in der Gruppe hinzugefügt
RemoveGroupMember            | Entfernen Sie Mitglied aus Gruppe.
AddGroupOwner                | Gruppe hinzugefügten Besitzer
RemoveGroupOwner                 | Entfernte Besitzer von Gruppe
**Anwendungsereignisse**               |
Dienst Tilgungsanteile hinzufügen                | Dem Verzeichnis hinzugefügt ein Dienst Tilgungsanteile.
Entfernen der Tilgungsanteile service             | Ein Dienst Tilgungsanteile entfernt aus dem Verzeichnis.
Hinzufügen von Hauptbenutzer Service-Anmeldeinformationen    | Anmeldeinformationen ein, der Tilgungsanteile einen Dienst hinzugefügt.
Entfernen der wichtigsten Service-Anmeldeinformationen | Entfernte Anmeldeinformationen aus einem Dienst Hauptbenutzer.
Fügen Sie Delegation-Eintrag hinzu                 | Erstellt eine [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) im Verzeichnis an.
Die Delegation Eintrag festlegen                 | Eine [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) im Verzeichnis wird aktualisiert.
Entfernen Sie Delegation Eintrag              | Löschen einer [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) im Verzeichnis an.
**Rolle Ereignisse**                      |
Hinzufügen von Mitglied der Rolle zu Rolle              | Einen Benutzer hinzugefügt einer Rolle Verzeichnis.
Entfernen Sie Mitglied der Rolle aus Rolle         | Entfernt einen Benutzer aus einer Rolle Verzeichnis an.
Legen Sie Kontaktinformationen von Unternehmen      | Festlegen von Unternehmen Ebene kontakteinstellungen. Dies umfasst die e-Mail-Adressen für marketing sowie technische Benachrichtigungen zu Microsoft Online Services.
Fügen Sie Delegation-Eintrag hinzu                 | Erstellt eine [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) im Verzeichnis an.
Die Delegation Eintrag festlegen                 | Eine [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) im Verzeichnis wird aktualisiert.
Entfernen Sie Delegation Eintrag              | Löschen einer [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) im Verzeichnis an.
AddSevicePrincipalOwner          | Hinzugefügten Besitzer zum Dienst Tilgungsanteile.
RemoveSevicePrincipalOwner   | Besitzer von Dienst Tilgungsanteile entfernt.
AddApplication  | Fügen Sie die Anwendung hinzu.
UpdateApplication | Aktualisieren Sie die Anwendung. Um anzuzeigen, welche Einstellungen für die app aktualisiert wurden, finden Sie unten im Abschnitt [Anwendung Eigenschaften erfasst:](#update-application-attributes)
DeleteApplication | Löschen Sie die Anwendung.
RestoreApplication|Stellen Sie die Anwendung wieder her.
AddApplicationOwner   |     Besitzer Anwendung hinzufügen.
RemoveApplicationOwner    |     Besitzer von Anwendung zu entfernen.
**Rolle Ereignisse**                         |
Hinzufügen von Mitglied der Rolle zu Rolle                 | Einen Benutzer hinzugefügt einer Rolle Verzeichnis.
Entfernen Sie Mitglied der Rolle aus Rolle            | Entfernt einen Benutzer aus einer Rolle Verzeichnis an.
AddRoleDefinition               | Rollendefinition hinzugefügt.
UpdateRoleDefinition                | Aktualisierte Rollendefinition. Um anzuzeigen, welche Einstellungen Rolle aktualisiert wurden, finden Sie unten im Abschnitt [Rolle Definition Eigenschaften erfasst:](#update-role-definition-attributes)
DeleteRoleDefinition                | Gelöschte Rollendefinition.
AddRoleAssignmentToRoleDefinition | Rollenzuweisung Rolle Definition hinzugefügt.
RemoveRoleAssignmentFromRoleDefinition | Entfernte rollenzuweisung aus Rollendefinition.
AddRoleFromTemplate     | Hinzugefügte Rolle aus Vorlage.
UpdateRole  | Aktualisierte Rolle.
AddRoleScopeMemberToRole    | Suchbegriffs Element Rolle hinzugefügt.
RemoveRoleScopedMemberFromRole  | Entfernte Suchbegriffs Mitglied aus der Rolle.
**Geräteereignisse (alle neuen Ereignisse)**                    |
Hinzugefügt werden               | Hinzugefügte Gerät.
UpdateDevice            | Aktualisierte Gerät. Um anzuzeigen, welche Eigenschaften des Geräts aktualisiert wurden, finden Sie unter [Geräteeigenschaften Löschvorgänge](#update-device-attributes) unten im Abschnitt
DeleteDevice            | Gelöschte Gerät.
AddDeviceConfiguration      | Konfiguration der hinzugefügten Device.
UpdateDeviceConfiguration   | Konfiguration der aktualisierten Device. Um anzuzeigen, welche Eigenschaften von Gerät Konfiguration aktualisiert wurden, finden Sie unter [Gerät Konfigurationseigenschaften Löschvorgänge](#update-device-configuration-attributes) unten im Abschnitt
DeleteDeviceConfiguration   | Gerätekonfiguration von gelöschten.
AddRegisteredOwner     | Registrierte Besitzer an Gerät hinzugefügt.
AddRegisteredUsers     | Registrierte Benutzer hinzugefügt Gerät.
RemoveRegisteredOwner    | Registrierte Besitzer vom Gerät zu entfernen.
RemoveRegisteredUsers    | Registrierte Benutzer vom Gerät zu entfernen.
RemoveDeviceCredentials  | Anmeldeinformationen Gerät zu entfernen.
**B2B Ereignisse**                       |
Stapel Einladungen hochgeladen wird.              | Ein Administrator hat eine Datei mit Einladungen Partnerbenutzer gesendet werden hochgeladen.
Stapel Einladungen verarbeitet.             | Eine Datei mit von Einladungen für Partnerbenutzer wurde verarbeitet.
Externen Benutzer einladen.                | Verzeichnis ist ein externer Benutzer eingeladen wurden.
Lösen Sie externe Benutzer einladen ein.         | Ein externer Benutzer hat die Einladung zu dem Verzeichnis eingelöst.
Externen Benutzer zur Gruppe hinzufügen.          | Ein externer Benutzer wurde Zugehörigkeit zu einer Gruppe im Verzeichnis zugewiesen.
Externen Benutzer Anwendung zuweisen. | Ein externer Benutzer wurde direkten Zugriff auf eine Anwendung zugewiesen.
Erstellung von Viren Mandanten.               | Ein neuer Mandanten wurde durch die Einladung Rückzahlung in Azure Active Directory erstellt.
Erstellen des Viren Benutzers.                 | Ein Benutzer wurde in einem vorhandenen Mandanten in Azure AD durch die Einladung Rückzahlung erstellt.
**Administrative Einheiten (alle neuen Ereignisse)**             |
AddAdministrativeUnit   |   Fügen Sie administrative Einheit hinzu.
UpdateAdministrativeUnit|   Administrative Einheit zu aktualisieren. Um anzuzeigen, welche Eigenschaften Administrative Einheit aktualisiert wurden, finden Sie unten im Abschnitt [Administrative Einheiteneigenschaften erfasst:](#update-administrative-unit-attributes)
DeleteAdministrativeUnit|   Löschen Sie administrative Einheit.
AddMemberToAdministrativeUnit|  Hinzufügen von Mitglied zu administrative Einheit.
RemoveMemberFromAdministrativeUnit|     Entfernen Sie Mitglied aus administrative Einheit.
**Verzeichnisereignisse**                 |
Hinzufügen des Partners für Unternehmen               | Dem Verzeichnis hinzugefügt ein Partners.
Entfernen Sie Partner von Unternehmen          | Ein Partners entfernt aus dem Verzeichnis.
DemotePartner   |   Tieferstufen Sie Partner.
Hinzufügen einer Domäne zu Unternehmen                | Dem Verzeichnis hinzugefügt eine Domäne.
Entfernen der Domäne Unternehmen           | Eine Domäne entfernt aus dem Verzeichnis.
Domäne aktualisieren                        | Eine Domäne im Verzeichnis wird aktualisiert. Um anzuzeigen, welche Domäneneigenschaften aktualisiert wurden, finden Sie unten im Abschnitt [Domäneneigenschaften erfasst:](#update-domain-attributes)
Festlegen der Domänenauthentifizierung            | Die Standardeinstellung für die Domäne für das Unternehmen wird geändert.
Legen Sie Kontaktinformationen von Unternehmen      | Festlegen von Unternehmen Ebene kontakteinstellungen. Dies umfasst die e-Mail-Adressen für marketing sowie technische Benachrichtigungen zu Microsoft Online Services.
Festlegen von Föderation Einstellungen für Domäne    | Aktualisiert die Föderation Einstellungen für eine Domäne.
Überprüfen der Domäne                        | Überprüft eine Domäne auf Verzeichnis.
Überprüfen überprüft-e-Mail-Domäne         | Überprüft eine Domäne für das Verzeichnis über e-Mail-Überprüfung an.
Festlegen von DirSyncEnabled Kennzeichnung für Unternehmen   | Legen Sie die Eigenschaft mit dem Verzeichnis für Azure AD synchronisieren kann.
Festlegen von Kennwortrichtlinien                  | Legen Sie die Länge und Zeichen Einschränkungen für die Benutzerkennwörter.
Festlegen von Firmeninformationen              | Die Informationen des Unternehmens Ebene aktualisiert. Der PowerShell-Cmdlet [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) Weitere Informationen hierzu finden Sie unter.
SetCompanyAllowedDataLocation  |    Festlegen von Unternehmen zulässig Datenspeicherort.
SetCompanyDirSyncEnabled    |   Festlegen von DirSyncEnabled Kennzeichnung.
SetCompanyDirSyncFeature |  Festlegen von Dirsync-Features.
SetCompanyInformation |   Festlegen von Firmeninformationen.
SetCompanyMultiNationalEnabled    |     Festlegen von internationalen Unternehmen-Feature aktiviert.
SetDirectoryFeatureOnTenant   |     Legen Sie auf Mandanten-Directory-Funktion.
SetTenantLicenseProperties  |   Festlegen von Eigenschaften für Mandanten-Lizenz.
CreateCompanySettings   |   Erstellen von Einstellungen für Unternehmen
UpdateCompanySettings   |   Aktualisieren der Einstellungen für Unternehmen. Um anzuzeigen, welche Eigenschaften Unternehmen aktualisiert wurden, finden Sie unten im Abschnitt [Eigenschaften Unternehmen erfasst:](#update-company-attributes)
DeleteCompanySettings   |   Löschen von Einstellungen für Unternehmen
SetAccidentalDeletionThreshold   |  Legen Sie versehentlich gelöschter Schwellenwert an.
SetRightsManagementProperties   |   Festlegen von Rights Management Eigenschaften.
PurgeRightsManagementProperties     |   Bereinigen Sie Rights Management Eigenschaften an.
UpdateExternalSecrets   |   Aktualisieren von externen Kennwörter
**Richtlinienereignisse (alle neuen Ereignisse)**           |
AddPolicy   |   Fügen Sie die Richtlinie hinzu.
UpdatePolicy    |   Richtlinie aktualisiert werden.
DeletePolicy    |   Löschen Sie die Richtlinie.
AddDefaultPolicyApplication     |   Richtlinie Anwendung hinzufügen.
AddDefaultPolicyServicePrincipal    |   Hinzufügen von Richtlinien zu Dienst Tilgungsanteile.
RemoveDefaultPolicyApplication  |   Entfernen Sie Richtlinie aus Anwendung.
RemoveDefaultPolicyServicePrincipal     |   Entfernen Sie Richtlinie vom Dienst Hauptbenutzer an.
RemovePolicyCredentials     |   Entfernen Sie die Richtlinie Anmeldeinformationen.

## <a name="audit-report-retention"></a>Audit Bericht Aufbewahrungsrichtlinien
Ereignisse in der Azure AD-Überwachungsbericht werden 180 Tage lang aufbewahrt. Weitere Informationen zu Aufbewahrungsrichtlinien auf Berichte finden Sie unter [Azure Active Directory Bericht Aufbewahrungsrichtlinien aus](active-directory-reporting-retention.md).

Für Kunden, deren Audit Ereignisse für längere Aufbewahrung speichern möchten können die Reporting-API, regelmäßig Audit Ereignisse in einem separaten Datenspeicher herauszuziehen verwendet werden. Weitere Informationen finden Sie unter [Erste Schritte mit der Reporting-API](active-directory-reporting-api-getting-started.md) .

## <a name="properties-included-with-each-audit-event"></a>Eigenschaften der einzelnen Audit Ereignisse

Eigenschaft      | Beschreibung
------------- | --------------------------------------------------------------
Datum und Uhrzeit | Das Datum und die Uhrzeit, zu denen das Ereignis audit
Akteur         | Der Benutzer oder Dienst Tilgungsanteile, die die Aktion ausgeführt
Aktion        | Die Aktion, die durchgeführt wurde
Ziel        | Der Benutzer oder Dienst Tilgungsanteile, die auf die Aktion ausgeführt wurde


## <a name="update-user-attributes"></a>Attribute "Benutzer aktualisieren"
Das Ereignis "Benutzer aktualisieren" Audit enthält weitere Informationen zur Verwendung von Benutzerattributen aktualisiert wurden. Für jedes Attribut den vorherigen Wert und den neuen Wert in enthalten ist.

Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled                      | Der Benutzer kann sich anmelden.
AssignedLicense                     | Alle Lizenzen, die dem Benutzer zugewiesen wurden.
AssignedPlan                        | Reaktionsgruppendienst Pläne resultierender aus den dem Benutzer zugewiesenen Lizenzen.
LicenseAssignmentDetail             | Details zu dem Benutzer zugewiesene Lizenzen. Sofern Arbeitsgruppen-Lizenzierung stattgefunden hat, würde dies z. B. die Gruppe enthalten, die die Lizenz erteilt.
Mobile                              | Des Benutzers auf dem Mobiltelefon.
OtherMail                           | Alternative e-Mail-Adresse des Benutzers.
OtherMobile                         | Des Benutzers alternative auf dem Mobiltelefon.
StrongAuthenticationMethod          | Eine Liste der Überprüfung Methoden, die vom Benutzer für die kombinierte Authentifizierung, wie z. B. Voicemail anrufen, SMS oder Überprüfung Code aus einer mobilen app konfiguriert.
StrongAuthenticationRequirement     | Wenn die kombinierte Authentifizierung aktiviert ist, aktiviert oder deaktiviert für diesen Benutzer.
StrongAuthenticationUserDetails     | Die Telefonnummer des Benutzers, alternativen Telefonnummer und e-Mail-Adresse für die kombinierte Authentifizierung und das Kennwort zurücksetzen Überprüfung verwendet.
StrongAuthenticationPhoneAppDetail  | Details zum Forof Phone apps registriert 2FA Login ausführen
TelephoneNumber                     | Die Telefonnummer des Benutzers.
AlternativeSecurityId               | Eine alternative Sicherheits-ID für das Objekt.
CreationType                        |Methode der Erstellung des Benutzers (entweder durch Einladung oder Viren).
InviteTicket                        |Liste der Einladung Tickets für den Benutzer.
InviteReplyUrl                      |Liste der Urls nach Einladungsannahme Antworten.
InviteResources                     |Liste der Ressourcen, die der Benutzer eingeladen wurde.
LastDirSyncTime                     |Das Objekt aktualisiert wurde, da die Synchronisierung von der autorisierende zuletzt Directory (Kunde, lokal).
MSExchRemoteRecipientType           |Karten in MSO Empfänger Dateitypen. Verweisen Sie auf [Empfänger Typen MSO] https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx für Empfänger Typen
PreferredDataLocation               |Die gewünschte Position für die des Benutzers, der Gruppe, des Kontakts, Öffentliche Ordner oder des Geräts Daten.
ProxyAddresses                      |Die Adresse, an der ein Exchange Server-Empfänger-Objekt in einem fremden e-Mail-System erkannt wird.
StsRefreshTokensValidFrom           |Ausführt aktualisieren token Widerrufsinformationen: alle STS aktualisieren Token ausgestellt, bevor Sie dieses Mal gelten abgelaufen sind.
UserPrincipalName                   |Der UPN, die für einen Benutzer eine Internet-Stil-Anmeldename ist.
UserState                           |Status des Benutzers Ausstehende Genehmigung/PendingAcceptance/akzeptiert/PendingVerification.
UserStateChangedOn                  |Zeitstempel der letzten Änderung UserState. Zum Auslösen des Lebenszyklus Workflows verwendet.
Benutzertyp                            |Typ des Benutzers. Mitglied (0), Gast (1), Viral(2).

## <a name="update-group-attributes"></a>Attribute "Gruppe aktualisieren"
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Klassifizierung            | Die Klassifizierung für eine einheitliche Gruppe (HBI, MBI usw.).
Beschreibung               | Lesbare beschreibenden Ausdrücke über das Objekt.  
DisplayName               |Den Anzeigenamen für ein Objekt
DirSyncEnabled            |Gibt an, ob die Synchronisierung aus einer autorisierenden Directory (Kunde, lokal).
GroupLicenseAssignment    |Lizenzen zugewiesen einer Gruppe
GroupType                 |Typ des Gruppe Unified (0)
IsMembershipRuleLocked    |Gibt an, dass die Eigenschaft MembershipRule, indem der Gruppe Self-service-Verwaltungsdienst festgelegt wird und von den Benutzern nicht geändert werden können. Gilt nur für die Gruppen, wobei GroupType GroupType.DynamicMembership enthält, die
IsPublic                  |Kennzeichnen Sie, um anzugeben, ob die Gruppe öffentlichen und privaten ist.
LastDirSyncTime           |Letzte Uhrzeit des Objekts durch die Synchronisierung von der autorisierende aktualisiert wurde (Kunde, lokal) Verzeichnis.
E-Mail-Nachrichten                      |Primäre e-Mail-Adresse
MailEnabled               |Gibt an, ob die e-Mail-Funktion für diese Gruppe ist.
MailNickname              |Moniker für eine Adresse Adressbuch Objekt, normalerweise den Teil des e-Mail-Namens vorherige der "@" Symbol.   
MembershipRule            |Eine Zeichenfolge, die Kriterien, die von der Gruppe Self-service-Verwaltungsdienst verwendet, um zu bestimmen, welche Mitglieder dieser Gruppe angehören sollen. Siehe auch IsMembershipRuleLocked. Gilt nur für Gruppen, wobei GroupType GroupType.DynamicMembership enthält.
MembershipRuleProcessingState| Eine Enumerationswert nach der Gruppe Self-service-Verwaltungsdienst definieren den Status Mitgliedschaft für diese Gruppe Verarbeitung definiert. Gilt nur für Gruppen, wobei GroupType GroupType.DynamicMembership enthält.
ProxyAddresses            |Die Adresse, an der ein Exchange Server-Empfänger-Objekt in einem fremden e-Mail-System erkannt wird.
RenewedDateTime           |Wenn eine Gruppe zuletzt erneuert wurde Timestamp-Eintrag.   
SecurityEnabled           |Gibt an, ob die Mitgliedschaft in der Gruppe Autorisierung Entscheidungen auswirken kann.
WellKnownObject           |Beschriftungen ein Verzeichnisobjekt, definieren ihn als zu einem vordefinierten Satz.

## <a name="update-device-attributes"></a>Attribute "Gerät aktualisieren"
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled | Gibt an, ob eine Sicherheit Tilgungsanteile authentifiziert werden kann.
CloudAccountEnabled | Gibt an, ob eine Sicherheit Tilgungsanteile authentifiziert werden kann. Wenn das Gerät lokal beherrschen ist InTune geschrieben.
CloudDeviceOSType | Typ des Geräts auf der Grundlage der OS z. B. Windows RT, iOS. Beim Festlegen von einem Clouddienst (z. B. Intune), wird dieses Attribut für DeviceOSType autorisierende im Verzeichnis.
CloudDeviceOSVersion | Version des Betriebssystems. Beim Festlegen von einem Clouddienst (z. B. Intune), wird dieses Attribut für DeviceOSVersion autorisierende im Verzeichnis.
CloudDisplayName | Wert der DisplayName LDAP-Attribut. Beim Festlegen von einem Clouddienst (z. B. Intune), wird dieses Attribut autorisierende im Verzeichnis für DisplayName.
CloudCreated |Gibt an, ob das Objekt von Cloud-Dienste erstellt wurde.
CompliantUntil | Bis zu welcher Uhrzeit gilt Gerät kompatibel.
DeviceMetadata |Benutzerdefinierte Metadaten für das Gerät
DeviceObjectVersion | Dieses Attribut wird verwendet, um das Schemaversion des Geräts zu identifizieren.
DeviceOSType |Typ des Geräts auf der Grundlage der OS z. B. Windows RT, iOS. Vom Dienst Registrierung geschrieben und durch die MDM aktualisiert werden soll Hellblau Verwaltungsdienst oder STS-Verwaltungsdienst aus.
DeviceOSVersion |Version des Betriebssystems. Vom Dienst Registrierung geschrieben und durch die MDM aktualisiert werden soll Hellblau Verwaltungsdienst oder STS-Verwaltungsdienst aus.
DevicePhysicalIds |Mehrwertiges Attribut Bezeichner des physischen Geräts gespeichert werden soll. Hierzu gehören möglicherweise BIOS-IDs, TPM Fingerabdrücke, Hardware usw. bestimmte IDs.
DirSyncEnabled | Gibt an, ob die Synchronisierung eines autorisierenden (Kunden, lokal) Verzeichnis.
DisplayName |Den Anzeigenamen für ein Objekt
IsCompliant | Dieses Attribut dient zum Verwalten des mobilen Gerät Management Status des Geräts.
IsManaged |Dieses Attribut wird verwendet, um anzugeben, dass das Gerät von einem Cloud MDM verwaltet wird
LastDirSyncTime |Das Objekt aktualisiert wurde, da die Synchronisierung von der autorisierende zuletzt Directory (Kunde, lokal).

## <a name="update-device-configuration-attributes"></a>Attribute "Aktualisieren Gerätekonfiguration"
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
MaximumRegistrationInactivityPeriod |Die maximale Anzahl der Tage, die ein Gerät inaktiv davor sein kann, wird für Freistellen berücksichtigt.
RegistrationQuota   |Die Richtlinie verwendet, um die Anzahl von Gerät Registrierungen für einen einzelnen Benutzer zugelassen werden.

## <a name="update-service-principal-configuration-attributes"></a>Attribute "Hauptbenutzer Dienstkonfiguration aktualisieren"
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled |Gibt an, ob eine Sicherheit Tilgungsanteile authentifiziert werden kann.
AppPrincipalId | Externe, anwendungsspezifische Identität für eine Tilgungsanteile Sicherheit.
DisplayName |Den Anzeigenamen für ein Objekt
ServicePrincipalName    | Eine Benutzerprinzipalnamen Dienst mit "Name/Zertifizierungsstelle", wobei Name gibt Wert der Anwendung Class und Zertifizierungsstelle enthält mindestens, Hostname [: Port] oder "Name", einen Bezeichner für den Dienst Hauptbenutzer angibt.

## <a name="update-app-attributes"></a>Attribute "App aktualisieren"
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |Die Menge der Adressen (Umleiten von URLs), die einen Dienst Tilgungsanteile zugewiesen sind.
AppId                               |ID der Anwendung der App   
AppIdentifierUri | URI Anwendung, die die Anwendung identifiziert.  Es ist normalerweise der Anwendung Access-URL.
AppLogoUrl |Die Url für die Anwendung Logobild in einem CDN gespeichert.
AvailableToOtherTenants | True, die Anwendung wird mit mehreren Mandanten (d. h. können verwendet werden durch andere Mandanten).
DisplayName | Den Anzeigenamen für einen Namen für die Anwendung
Ansprüche |Liste der Anwendung Ansprüche.
ExternalUserAccountDelegationsAllowed |Kennzeichnen Sie angibt, ob die Ressource Anwendung ist eine vertrauenswürdigen und kann Delegation-Einträgen für externe Benutzerkonten erstellen.
GroupMembershipClaims |Die Mitgliedschaft Ansprüche Gruppenrichtlinien.
PublicClient | True, wenn der Client geheimen beibehalten, kann nicht (d. h. nicht vertraulichen Client in OAuth2.0)
RecordConsentConditions | Typen von Zustimmung Bedingungen, durch die Vertragsbedingungen definierten: keine (0), SilentConsentForPartnerManagedApp(1). Dieser Wert wird im Schema Graph-API verfügbar gemacht werden und kann nur von Administratoren Mandanten festlegen/geändert werden.
RequiredResourceAccess |XML-Inhalt eines Werts in der Eigenschaft RequiredResourceAccess.
WebApp |True, gibt an, dass diese Anwendung einer Web-app ist.
WwwHomepage |Der primären Webseite.

## <a name="update-role-attributes"></a>Attribute "Rolle aktualisieren"
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |Die Menge der Adressen (Umleiten von URLs), die einen Dienst Tilgungsanteile zugewiesen sind.
BelongsToFirstLoginObjectSet |True, gibt an, dass die Gruppe von Objekten erforderliche Anmeldung von der ersten Administrator für einen neuen Mandanten Aktivieren dieses Objekt gehört.
Vordefinierte |Gibt an, ob die Gültigkeitsdauer eines Objekts das System gehört.
Beschreibung | Lesbare beschreibenden Ausdrücke über das Objekt.  
DisplayName |Den Anzeigenamen für ein Objekt
MailNickname | Moniker für eine Adresse Adressbuch Objekt, normalerweise den Teil des e-Mail-Namens vorherige der "@" Symbol.
RoleDisabled |Gibt an, ob die Rolle des Zwecks von Access Prüfungen ignoriert werden soll.
RoleTemplateId | Identität der Rollenvorlage.
ServiceInfo |Service-spezifische provisioning Informationen, die von MOAC und/oder anderen Dienstinstanzen (der gleichen oder in verschiedenen Typen) belegt werden kann.
TaskSetScopeReference |Gibt eine TaskSet und eine Reihe von Bereichen eine Rolle oder RoleTemplate zugeordnet.
ValidationError |Informationen, die von einem partnerverbundkontakte Dienst zur Beschreibung eines dauerhaftes, Service-spezifische Fehlers im Hinblick auf die Eigenschaften oder einen Link von einem Administrator Objektaktion Problembehebung veröffentlicht werden.
WellKnownObject |Beschriftungen ein Verzeichnisobjekt, definieren ihn als zu einem vordefinierten Satz.

## <a name="update-role-definition-attributes"></a>Attribute "Rollendefinition aktualisieren"
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AssignableScopes    |Eine Zusammenstellung von Autorisierung Bereichen, die beim Zuweisen von diesem RoleDefinition zu einer Sicherheit Tilgungsanteile verwiesen werden kann.
DisplayName     |Den Anzeigenamen für ein Objekt
GrantedPermissions  |Durch diese RoleDefinition Berechtigungen.


## <a name="update-administrative-unit-attributes"></a>Attribute "Administrative Einheit aktualisieren"
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Beschreibung |   Diese Eigenschaft wird aktualisiert, wenn Sie die Beschreibung eine administrative Einheit ändern.
DisplayName |   Diese Eigenschaft wird aktualisiert, wenn Sie den Namen des eine administrative Einheit ändern.

## <a name="update-company-attributes"></a>Attribute "Update Unternehmen"
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AllowedDataLocation     | Ein Ort, in denen Benutzer des Unternehmens bereitgestellt werden zulässig ist.
AuthorizedServiceInstance   | Namen der Dienstinstanzen, die ein Plan bereitgestellt werden kann.
DirSyncEnabled |Gibt an, ob die Synchronisierung eines autorisierenden (Kunden, lokal) Verzeichnis.
DirSyncStatus |Gibt an, ob die Synchronisierung von Adresse Adressbuch Objekte in diesem Zusammenhang Mandanten eines autorisierenden (Kunden, lokal) erfolgt Directory; eine Erweiterung der DirSyncEnabled-Eigenschaft für Unternehmen Objekte.
DirSyncFeatures |Bit Kennzeichnung zum Festlegen der aktiviert und deaktiviert Dirsync-Features für den Mandanten verfolgen.
DirectoryFeatures |Aktiviert/deaktiviert Directory-Features.
DirSyncConfiguration |Enthält alle Dirsync-Konfiguration, die speziell für den aktuellen Mandanten.
DisplayName |Den Anzeigenamen für ein Objekt
IsMnc |Legen Sie booleschen Kennzeichen auf "true" iff, die das Unternehmen für das internationale Unternehmen-Feature aktiviert ist.
ObjectSettings | Eine Zusammenstellung von Einstellungen auf den Bereich des Objekts anwendbar.
PartnerCommerceUrl |Die URL des Partners Commerce-Website.
PartnerHelpUrl |URL der Partnerwebsite Hilfe.
PartnerSupportEmail | URL des Partners Support-e-Mail.
PartnerSupportTelephone |URL des Partners Support Telefon.
PartnerSupportUrl |Die URL des Partners Support-Website.
StrongAuthenticationDetails |Details im Zusammenhang mit StrongAuthentication.
StrongAuthenticationPolicy |Strenge Authentifizierung die Richtlinie für das Unternehmen.
TechnicalNotificationMail |E-Mail-Adresse zu technische Problemen in Verbindung mit einem Unternehmen Gruppenrichtlinien benachrichtigen.
TelephoneNumber |Telefonnummern, die die ITU Empfehlungen E.123 entsprechen.
TenantType |Die Art des einen Mandanten. Wenn dieser Wert nicht angegeben ist, ist der Mandanten ein Unternehmen ein. Andernfalls mögliche Werte sind: Microsoft-Support (0), SyndicatePartner (1), BreadthPartner (2) BreadthPartnerDelegatedAdmin (3) ResellerPartnerDelegatedAdmin (4) ValueAddedResellerPartnerDelegatedAdmin (5).
VerifiedDomain |Eine Reihe von DNS-Domänennamen gebunden auf einen Mandanten.

## <a name="update-domain-attributes"></a>Attribute "Domäne aktualisieren"
Attribut                           | Beschreibung
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Funktionen    |Bit-Optionen, beschreibt die Funktionen der Domäne, sofern vorhanden.
Standard |Zeigt an, ob die Domäne den Standardwert ist; beispielsweise das standardmäßige UserPrincipalName Suffix erstellt ein Administrator einen neuen Benutzer in MOAC.
Ursprüngliche |Gibt an, ob die Domäne für das Unternehmen, die ursprüngliche Domäne ist, von OCP zugewiesen wurden. Die ursprüngliche Domäne ist eine eindeutige Unterdomäne einer Microsoft Online-Domäne. e.g.contoso3.microsoftonline.com.
LiveType    |Typ des entsprechenden Windows Live Namespace, sofern vorhanden.
Namen    | Bezeichner für den Endpunkt.
PasswordNotificationWindowDays  |Die Anzahl der Tage vor Ablauf des ein Kennwort des Benutzers wird benachrichtigt.
PasswordValidityPeriodDays  | Die Anzahl der Tage, denen ein Kennwort für gute ist, bevor es geändert werden muss.

Audit-Einträge sind ein erforderliches Steuerelement für viele Compliance-Vorschriften. Für Kunden mit Azure Active Directory überwachenden Bericht zu deren Einhaltung von Vorschriften empfiehlt es sich, dass der Kunde eine Kopie der in diesem Hilfethema mit der Kopie der vom Kunden exportierte Überwachungsbericht zu den Berichtdetails erklären übermitteln. Wenn der Prüfer erfahren möchten, die Compliance-Vorschriften, die aktuell Azure entspricht, direkte des Prüfers zur [Compliance-Seite](https://azure.microsoft.com/support/trust-center/compliance/) des Microsoft Azure Trust Center.
