<properties
    pageTitle="Synchronisieren von Azure AD verbinden: Attribute mit Azure Active Directory synchronisiert | Microsoft Azure"
    description="Listet die Attribute, die mit Azure Active Directory synchronisiert werden."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Synchronisieren von Azure AD verbinden: Attribute mit Azure Active Directory synchronisiert.
In diesem Thema werden die Attribute aufgeführt, die durch Azure AD verbinden synchronisieren synchronisiert werden.  
Die Attribute werden gruppiert nach der zugehörigen Azure AD-app.

## <a name="attributes-to-synchronize"></a>Zu synchronisierenden Attribute
Eine häufige gestellte Frage ist *wie folgt eine Liste der minimalen Attribute zu synchronisieren*. Der Standard- und empfohlene Ansatz ist die Standardattribute beibehalten, damit eine vollständige globale Adressliste (Global Address List) in der Cloud erstellt werden kann sowie alle Features in Office 365-Auslastung. In einigen Fällen es gibt einige Attribute, der Ihre Organisation keine synchronisierten in der Cloud, da diese Attribute enthalten vertrauliche oder personenbezogene Informationen (personenbezogene Informationen) Daten wie in diesem Beispiel:  
![Fehlerhafte Attributen](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

In diesem Fall beginnen Sie mit der Liste der Attribute in diesem Thema und identifizieren Sie die Attribute, die sensible oder PII-Daten enthalten würde und können nicht synchronisiert werden. Deaktivieren Sie diese Attribute während der Installation mithilfe von [Azure AD-app und Attribut zu filtern](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering).

>[AZURE.WARNING] Beim Aufheben der Auswahl von Attributen, sollten Sie vorsichtig vor und nur deaktivieren Sie diese Attribute absolut nicht möglich, zu synchronisieren. Andere Attribute Auswahl möglicherweise eine negative Features auswirken.

## <a name="office-365-proplus"></a>Office 365 ProPlus

| Attributname| Benutzer| Kommentar |
| --- | :-: | --- |
| accountEnabled| X| Legt fest, ob ein Konto aktiviert ist.|
| CN| X|  |
| displayName| X|  |
| objectSID| X| mechanischen Eigenschaft. AD-Benutzer-ID verwendet, um die Synchronisierung zwischen Azure beizubehalten AD und AD.|
| pwdLastSet| X| mechanischen Eigenschaft. Wissen, wann neu zu berechnenden bereits ausgestellte Token verwendet. Kennwort synchronisieren und Föderation verwendet.|
| sourceAnchor| X| mechanischen Eigenschaft. Unveränderlich Bezeichner in Beziehung zwischen addiert und Azure AD-beizubehalten.|
| usageLocation| X| mechanischen Eigenschaft. Des Benutzers Land. Zum Lizenzen zugewiesen.|
| userPrincipalName| X| Benutzerprinzipalnamen ist der Benutzername für den Benutzer. In den meisten Fällen den Wert [Mail] identisch.|

## <a name="exchange-online"></a>Exchange Online

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Legt fest, ob ein Konto aktiviert ist.|
| Assistent| X| X|  |  |
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| Co| X| X|  |  |
| Unternehmen| X| X|  |  |
| Ländercode| X| X|  |  |
| Abteilung| X| X|  |  |
| Beschreibung| X| X| X|  |
| displayName| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| Vorname| X| X|  |  |
| homePhone| X| X|  |  |
| Info| X| X| X| Dieses Attribut ist aktuell nicht für Gruppen verbraucht.|
| Initialen| X| X|  |  |
| l| X| X|  |  |
| legacyExchangeDN| X| X| X|  |
| mailNickname| X| X| X|  |
| mangedBy|  |  | X|  |
| Manager| X| X|  |  |
| Mitglied|  |  | X|  |
| Mobile| X| X|  |  |
| MsDS-HABSeniorityIndex| X| X| X|  |
| MsDS-PhoneticDisplayName| X| X| X|  |
| msExchArchiveGUID| X|  |  |  |
| msExchArchiveName| X|  |  |  |
| msExchAssistantName| X| X|  |  |
| msExchAuditAdmin| X|  |  |  |
| msExchAuditDelegate| X|  |  |  |
| msExchAuditDelegateAdmin| X|  |  |  |
| msExchAuditOwner| X|  |  |  |
| msExchBlockedSendersHash| X| X|  |  |
| msExchBypassAudit| X|  |  |  |
| msExchCoManagedByLink|  |  | X|  |
| msExchDelegateListLink| X|  |  |  |
| msExchELCExpirySuspensionEnd| X|  |  |  |
| msExchELCExpirySuspensionStart| X|  |  |  |
| msExchELCMailboxFlags| X|  |  |  |
| msExchEnableModeration| X|  | X|  |
| msExchExtensionCustomAttribute1| X| X| X| Dieses Attribut wird derzeit nicht von Exchange Online genutzt. |
| msExchExtensionCustomAttribute2| X| X| X| Dieses Attribut wird derzeit nicht von Exchange Online genutzt. |
| msExchExtensionCustomAttribute3| X| X| X| Dieses Attribut wird derzeit nicht von Exchange Online genutzt. |
| msExchExtensionCustomAttribute4| X| X| X| Dieses Attribut wird derzeit nicht von Exchange Online genutzt. |
| msExchExtensionCustomAttribute5| X| X| X| Dieses Attribut wird derzeit nicht von Exchange Online genutzt. |
| msExchHideFromAddressLists| X| X| X|  |
| msExchImmutableID| X|  |  |  |
| msExchLitigationHoldDate| X| X| X|  |
| msExchLitigationHoldOwner| X| X| X|  |
| msExchMailboxAuditEnable| X|  |  |  |
| msExchMailboxAuditLogAgeLimit| X|  |  |  |
| msExchMailboxGuid| X|  |  |  |
| msExchModeratedByLink| X| X| X|  |
| msExchModerationFlags| X| X| X|  |
| msExchRecipientDisplayType| X| X| X|  |
| msExchRecipientTypeDetails| X| X| X|  |
| msExchRemoteRecipientType| X|  |  |  |
| msExchRequireAuthToSendTo| X| X| X|  |
| msExchResourceCapacity| X|  |  |  |
| msExchResourceDisplay| X|  |  |  |
| msExchResourceMetaData| X|  |  |  |
| msExchResourceSearchProperties| X|  |  |  |
| msExchRetentionComment| X| X| X|  |
| msExchRetentionURL| X| X| X|  |
| msExchSafeRecipientsHash| X| X|  |  |
| msExchSafeSendersHash| X| X|  |  |
| msExchSenderHintTranslations| X| X| X|  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| msExchUserHoldPolicies| X|  |  |  |
| MsOrg-IsOrganizational|  |  | X|  |
| objectSID| X|  | X| mechanischen Eigenschaft. AD-Benutzer-ID verwendet, um die Synchronisierung zwischen Azure beizubehalten AD und AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherTelephone| X| X|  |  |
| Pager| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Postleitzahl| X| X|  |  |
| proxyAddresses| X| X| X|  |
| publicDelegates| X| X| X|  |
| pwdLastSet| X|  |  | mechanischen Eigenschaft. Wissen, wann neu zu berechnenden bereits ausgestellte Token verwendet. Kennwort synchronisieren und Föderation verwendet.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Abgeleitet von groupType|
| Sn| X| X|  |  |
| sourceAnchor| X| X| X| mechanischen Eigenschaft. Unveränderlich Bezeichner in Beziehung zwischen addiert und Azure AD-beizubehalten.|
| Mo| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailPhoto| X| X|  |  |
| Titel| X| X|  |  |
| unauthOrig| X| X| X|  |
| usageLocation| X|  |  | mechanischen Eigenschaft. Des Benutzers Land. Zum Lizenzen zugewiesen.|
| userCertificate| X| X|  |  |
| userPrincipalName| X|  |  | Benutzerprinzipalnamen ist der Benutzername für den Benutzer. In den meisten Fällen den Wert [Mail] identisch.|
| userSMIMECertificates| X| X|  |  |
| wWWHomePage| X| X|  |  |

## <a name="sharepoint-online"></a>SharePoint Online

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Legt fest, ob ein Konto aktiviert ist.|
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| Co| X| X|  |  |
| Unternehmen| X| X|  |  |
| Ländercode| X| X|  |  |
| Abteilung| X| X|  |  |
| Beschreibung| X| X| X|  |
| displayName| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| Vorname| X| X|  |  |
| hideDLMembership|  |  | X|  |
| HomePhone| X| X|  |  |
| Info| X| X| X|  |
| Initialen| X| X|  |  |
| "ipPhone"| X| X|  |  |
| l| X| X|  |  |
| e-Mail-Nachrichten| X| X| X|  |
| mailNickname| X| X| X|  |
| managedBy|  |  | X|  |
| Manager| X| X|  |  |
| Mitglied|  |  | X|  |
| middleName| X| X|  |  |
| Mobile| X| X|  |  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointLinkedBy| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| objectSID| X|  | X| mechanischen Eigenschaft. AD-Benutzer-ID verwendet, um die Synchronisierung zwischen Azure beizubehalten AD und AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherIpPhone| X| X|  |  |
| otherMobile| X| X|  |  |
| otherPager| X| X|  |  |
| otherTelephone| X| X|  |  |
| Pager| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Postleitzahl| X| X|  |  |
| postOfficeBox| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mechanischen Eigenschaft. Wissen, wann neu zu berechnenden bereits ausgestellte Token verwendet. Kennwort synchronisieren und Föderation verwendet.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Abgeleitet von groupType|
| Sn| X| X|  |  |
| sourceAnchor| X| X| X| mechanischen Eigenschaft. Unveränderlich Bezeichner in Beziehung zwischen addiert und Azure AD-beizubehalten.|
| Mo| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailPhoto| X| X|  |  |
| Titel| X| X|  |  |
| unauthOrig| X| X| X|  |
| URL| X| X|  |  |
| usageLocation| X|  |  | mechanischen Eigenschaft. Des Benutzers Land. Zum Lizenzen zugewiesen.|
| userPrincipalName| X|  |  | Benutzerprinzipalnamen ist der Benutzername für den Benutzer. In den meisten Fällen den Wert [Mail] identisch.|
| wWWHomePage| X| X|  |  |

## <a name="lync-online"></a>Lync Online

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Legt fest, ob ein Konto aktiviert ist.|
| c| X| X|  |  |
| CN| X|  | X|  |
| Co| X| X|  |  |
| Unternehmen| X| X|  |  |
| Abteilung| X| X|  |  |
| Beschreibung| X| X| X|  |
| displayName| X| X| X|  |
| facsimiletelephonenumber| X| X| X|  |
| Vorname| X| X|  |  |
| HomePhone| X| X|  |  |
| "ipPhone"| X| X|  |  |
| l| X| X|  |  |
| e-Mail-Nachrichten| X| X| X|  |
| mailNickname| X| X| X|  |
| managedBy|  |  | X|  |
| Manager| X| X|  |  |
| Mitglied|  |  | X|  |
| Mobile| X| X|  |  |
| msExchHideFromAddressLists| X| X| X|  |
| MsRTCSIP-ApplicationOptions| X|  |  |  |
| MsRTCSIP-DeploymentLocator| X| X|  |  |
| MsRTCSIP-Linie| X| X|  |  |
| MsRTCSIP-OptionFlags| X| X|  |  |
| MsRTCSIP-OwnerUrn| X|  |  |  |
| MsRTCSIP-PrimaryUserAddress| X| X|  |  |
| MsRTCSIP-UserEnabled| X| X|  |  |
| objectSID| X|  | X| mechanischen Eigenschaft. AD-Benutzer-ID verwendet, um die Synchronisierung zwischen Azure beizubehalten AD und AD.|
| otherTelephone| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Postleitzahl| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mechanischen Eigenschaft. Wissen, wann neu zu berechnenden bereits ausgestellte Token verwendet. Kennwort synchronisieren und Föderation verwendet.|
| securityEnabled|  |  | X| Abgeleitet von groupType|
| Sn| X| X|  |  |
| sourceAnchor| X| X| X| mechanischen Eigenschaft. Unveränderlich Bezeichner in Beziehung zwischen addiert und Azure AD-beizubehalten.|
| Mo| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailPhoto| X| X|  |  |
| Titel| X| X|  |  |
| usageLocation| X|  |  | mechanischen Eigenschaft. Des Benutzers Land. Zum Lizenzen zugewiesen.|
| userPrincipalName| X|  |  | Benutzerprinzipalnamen ist der Benutzername für den Benutzer. In den meisten Fällen den Wert [Mail] identisch.|
| wWWHomePage| X| X|  |  |

## <a name="azure-rms"></a>Azure RMS

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Legt fest, ob ein Konto aktiviert ist.|
| CN| X|  | X| Allgemeine Name oder Alias. In den meisten Fällen das Präfix [Mail]-Wert.|
| displayName| X| X| X| Eine Zeichenfolge, die häufig Bankname der angezeigte Name (Vorname Nachname) darstellt.|
| e-Mail-Nachrichten| X| X| X| vollständige e-Mail-Adresse.|
| Mitglied|  |  | X|  |
| objectSID| X|  | X| mechanischen Eigenschaft. AD-Benutzer-ID verwendet, um die Synchronisierung zwischen Azure beizubehalten AD und AD.|
| proxyAddresses| X| X| X| mechanischen Eigenschaft. Von Azure AD verwendet. Enthält alle sekundäre e-Mail-Adressen für den Benutzer.|
| pwdLastSet| X|  |  | mechanischen Eigenschaft. Wissen, wann neu zu berechnenden bereits ausgestellte Token verwendet.|
| securityEnabled|  |  | X| Abgeleitet von GroupType.|
| sourceAnchor| X| X| X| mechanischen Eigenschaft. Unveränderlich Bezeichner in Beziehung zwischen addiert und Azure AD-beizubehalten.|
| usageLocation| X|  |  | mechanischen Eigenschaft. Des Benutzers Land. Zum Lizenzen zugewiesen.|
| userPrincipalName| X|  |  | Diese UPN ist der Benutzername für den Benutzer. In den meisten Fällen den Wert [Mail] identisch.|

## <a name="intune"></a>Intune

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Legt fest, ob ein Konto aktiviert ist.|
| c| X| X|  |  |
| CN| X|  | X|  |
| Beschreibung| X| X| X|  |
| displayName| X| X| X|  |
| e-Mail-Nachrichten| X| X| X|  |
| mailNickname| X| X| X|  |
| Mitglied|  |  | X|  |
| objectSID| X|  | X| mechanischen Eigenschaft. AD-Benutzer-ID verwendet, um die Synchronisierung zwischen Azure beizubehalten AD und AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mechanischen Eigenschaft. Wissen, wann neu zu berechnenden bereits ausgestellte Token verwendet. Kennwort synchronisieren und Föderation verwendet.|
| securityEnabled|  |  | X| Abgeleitet von groupType|
| sourceAnchor| X| X| X| mechanischen Eigenschaft. Unveränderlich Bezeichner in Beziehung zwischen addiert und Azure AD-beizubehalten.|
| usageLocation| X|  |  | mechanischen Eigenschaft. Des Benutzers Land. Zum Lizenzen zugewiesen.|
| userPrincipalName| X|  |  | Benutzerprinzipalnamen ist der Benutzername für den Benutzer. In den meisten Fällen den Wert [Mail] identisch.|

## <a name="dynamics-crm"></a>Dynamics CRM

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Legt fest, ob ein Konto aktiviert ist.|
| c| X| X|  |  |
| CN| X|  | X|  |
| Co| X| X|  |  |
| Unternehmen| X| X|  |  |
| Ländercode| X| X|  |  |
| Beschreibung| X| X| X|  |
| displayName| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| Vorname| X| X|  |  |
| l| X| X|  |  |
| managedBy|  |  | X|  |
| Manager| X| X|  |  |
| Mitglied|  |  | X|  |
| Mobile| X| X|  |  |
| objectSID| X|  | X| mechanischen Eigenschaft. AD-Benutzer-ID verwendet, um die Synchronisierung zwischen Azure beizubehalten AD und AD.|
| physicalDeliveryOfficeName| X| X|  |  |
| Postleitzahl| X| X|  |  |
| preferredLanguage| X|  |  |  |
| pwdLastSet| X|  |  | mechanischen Eigenschaft. Wissen, wann neu zu berechnenden bereits ausgestellte Token verwendet. Kennwort synchronisieren und Föderation verwendet.|
| securityEnabled|  |  | X| Abgeleitet von groupType|
| Sn| X| X|  |  |
| sourceAnchor| X| X| X| mechanischen Eigenschaft. Unveränderlich Bezeichner in Beziehung zwischen addiert und Azure AD-beizubehalten.|
| Mo| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| Titel| X| X|  |  |
| usageLocation| X|  |  | mechanischen Eigenschaft. Des Benutzers Land. Zum Lizenzen zugewiesen.|
| userPrincipalName| X|  |  | Benutzerprinzipalnamen ist der Benutzername für den Benutzer. In den meisten Fällen den Wert [Mail] identisch.|

## <a name="3rd-party-applications"></a>3rd Party applications
Diese Gruppe ist eine Gruppe von Attributen, die als die minimale Attribute erforderlich für eine generische Arbeitsbelastung oder einer Anwendung verwendet. Sie können für eine Arbeitsbelastung nicht aufgeführt wird, in einen anderen Abschnitt oder für ein nicht-Microsoft-app verwendet werden. Explizit für Folgendes verwendet:

- Yammer-(nur für Benutzer belegt ist)
- [Hybrid Business-to-Business (B2B) Cross Organisations Szenarien für die Zusammenarbeit von Ressourcen wie SharePoint angeboten](http://go.microsoft.com/fwlink/?LinkId=747036)

Diese Gruppe ist eine Gruppe von Attributen, die verwendet werden können, wenn Azure AD-Verzeichnis zur Unterstützung von Office 365, Dynamics oder Intune nicht verwendet wird. Es verfügt über eine kleine Gruppe von Core Attributen.

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Legt fest, ob ein Konto aktiviert ist.|
| CN| X|  | X|  |
| displayName| X| X| X|  |
| Vorname| X| X|  |  |
| e-Mail-Nachrichten| X|  | X|  |
| managedBy|  |  | X|  |
| mailNickName| X| X| X|  |
| Mitglied|  |  | X|  |
| objectSID| X|  |  | mechanischen Eigenschaft. AD-Benutzer-ID verwendet, um die Synchronisierung zwischen Azure beizubehalten AD und AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mechanischen Eigenschaft. Wissen, wann neu zu berechnenden bereits ausgestellte Token verwendet. Kennwort synchronisieren und Föderation verwendet.|
| Sn| X| X|  |  |
| sourceAnchor| X| X| X| mechanischen Eigenschaft. Unveränderlich Bezeichner in Beziehung zwischen addiert und Azure AD-beizubehalten.|
| usageLocation| X|  |  | mechanischen Eigenschaft. Des Benutzers Land. Zum Lizenzen zugewiesen.|
| userPrincipalName| X|  |  | Benutzerprinzipalnamen ist der Benutzername für den Benutzer. In den meisten Fällen den Wert [Mail] identisch.|

## <a name="windows-10"></a>Windows-10
Ein Windows 10 Domänenverbund computer(device) werden einige Attribute in Azure Active Directory synchronisiert. Weitere Informationen zu den Szenarien finden Sie unter [Verbinden Domänenverbund Geräten für 10 Windows Azure AD-auftritt](active-directory-azureadjoin-devices-group-policy.md). Diese Attribute immer synchronisieren und Windows 10 wird als eine app, Aufheben der Markierung eines können, nicht angezeigt. Ein Windows 10 Domänenverbund befindet, wird durch das Attribut UserCertificate ausgefüllt haben identifiziert.

| Attributname| Gerät| Kommentar |
| --- | :-: | --- |
| accountEnabled| X| |
| deviceTrustType| X| Hartcodierte Wert für Domäne Computer. |
| displayName | X| |
| ms-DS-CreatorSID | X| Abkürzung für RegisteredOwnerReference.|
| Objekt-GUID | X| Auch als Geräte-ID bezeichnet.|
| objectSID | X| Abkürzung für OnPremisesSecurityIdentifier.|
| Betriebssystem | X| Abkürzung für DeviceOSType.|
| operatingSystemVersion | X| Abkürzung für DeviceOSVersion.|
| userCertificate | X| |

Diese Attribute für **Benutzer** sind zusätzlich zu den anderen apps, die Sie ausgewählt haben.  

| Attributname| Benutzer| Kommentar |
| --- | :-: | --- |
| domainFQDN| X| Die Abkürzung LdapIpAddress. Beispielsweise "contoso.com".|
| domainNetBios| X| Die Abkürzung NetBiosName. Beispielsweise "Contoso".|

## <a name="exchange-hybrid-writeback"></a>Exchange-Hybrid abgeschlossenen writebackvorgängen
Diese Attribute werden wieder aus Azure AD zu lokalen Active Directory geschrieben, wenn Sie zum Aktivieren des **Exchange-Hybrid**auswählen. Abhängig von Ihrer Version Exchange möglicherweise weniger Attribute synchronisiert werden.

| Attributname| Benutzer| Kontakt| Gruppe| Kommentar |
| --- | :-: | :-: | :-: | --- |
| MsDS-ExternalDirectoryObjectID| X|  |  | Abgeleitet von CloudAnchor in Azure Active Directory. Dieses Attribut ist neu in Exchange 2016.|
| msExchArchiveStatus| X|  |  | Onlinearchiv: Ermöglicht Kunden e-Mail-Nachrichten archivieren.|
| msExchBlockedSendersHash| X|  |  | Filtern: Schreibt wieder lokalen filtern und online sichere und blockierte Absender Daten von Clients aus.|
| msExchSafeRecipientsHash| X|  |  | Filtern: Schreibt wieder lokalen filtern und online sichere und blockierte Absender Daten von Clients aus.|
| msExchSafeSendersHash| X|  |  | Filtern: Schreibt wieder lokalen filtern und online sichere und blockierte Absender Daten von Clients aus.|
| msExchUCVoiceMailSettings| X|  |  | Aktivieren von Unified Messaging (UM) – Online Voicemail: verwendet von Microsoft Lync Server Integration, um Lync Server anzuzeigen lokale, dass der Benutzer in Onlinedienste Voicemail hat.|
| msExchUserHoldPolicies| X|  |  | Rechtsstreitigkeiten halten: Ermöglicht Cloud-Dienste zu bestimmen, welche Benutzer sind unter Rechtsstreitigkeiten halten.|
| proxyAddresses| X| X| X| Nur die X500 Adresse von Exchange Online wird eingefügt.|

## <a name="device-writeback"></a>Gerät abgeschlossenen writebackvorgängen
Geräteobjekte werden im Active Directory erstellt. Dieser Objekte Geräte Azure AD hinzugefügt werden können oder Domänenverbund 10 Windows-Computer.

| Attributname| Gerät| Kommentar |
| --- | :-: | --- |
| altSecurityIdentities | X| |
| displayName | X| |
| DN | X| |
| MsDS-CloudAnchor | X| |
| MsDS-Geräte-ID | X| |
| MsDS-DeviceObjectVersion | X| |
| MsDS-DeviceOSType | X| |
| MsDS-DeviceOSVersion | X| |
| MsDS-DevicePhysicalIDs | X| |
| MsDS-KeyCredentialLink | X| Nur in Verbindung mit Windows Server 2016 Active Directory-schema |
| MsDS-IsCompliant | X| |
| MsDS-IsEnabled | X| |
| MsDS-IsManaged | X| |
| MsDS-RegisteredOwner | X| |


## <a name="notes"></a>Notizen

- Wenn Sie eine alternative Kennung verwenden zu können, wird der lokalen Attribut UserPrincipalName mit der Azure AD-Attribut OnPremisesUserPrincipalName synchronisiert. Das alternative ID-Attribut für Beispiel e-Mail wird mit der Azure AD-Attribut UserPrincipalName synchronisiert.
- In den Listen über gilt für der Objekttyp **Benutzer** auch das Objekt Typ **iNetOrgPerson**.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die Konfiguration [Azure AD verbinden synchronisieren](active-directory-aadconnectsync-whatis.md) .

Erfahren Sie mehr über die [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](active-directory-aadconnect.md).
