<properties
    pageTitle="Zuweisen von Administratorrollen in Azure Active Directory | Microsoft Azure"
    description="Wird erläutert, welche Administratorrollen mit Azure Active Directory verfügbar sind und wie Sie diese zuweisen."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="curtand"/>

# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Zuweisen von Administratorrollen in Azure Active Directory

Azure Active Directory (Azure AD) können Sie verschiedene Funktionen dienen separate Administratoren festlegen. Diese Administratoren haben Zugriff auf verschiedenen Features in der Azure-Portal oder Azure klassischen Portal und, abhängig von ihrer Rolle, erstellen oder Bearbeiten von Benutzern, Zuweisen von Administratorrollen an andere Personen, Benutzerkennwörter zurücksetzen, Verwalten von Benutzerlizenzen und Domänen, die unter anderem verwalten können. Ein Benutzer mit der Rolle des Administrators zugewiesen ist haben dieselben Berechtigungen über alle Clouddienste, die Ihre Organisation abonniert hat, unabhängig davon, ob Sie die Rolle im Office 365-Portal oder im klassischen Azure-Portal oder mit dem Azure AD-Modul für Windows PowerShell zuweisen.

Die folgenden Administratorrollen stehen zur Verfügung:


- **Abrechnung Administrator**: tätigt Einkäufe, verwaltet Abonnements, verwaltet supporttickets und überwacht die Dienstgüte.

- **Globaler Administrator / Unternehmensadministratoren**: Zugriff auf alle Verwaltungsfunktionen hat. Die Person, die für das Konto Azure anmeldet, wird ein globaler Administrator. Nur globale Administratoren können anderen Administratorrollen zuweisen. In Ihrem Unternehmen kann mehr als ein globaler Administrator sein.

    > [AZURE.NOTE] In Microsoft Graph-API, Azure AD Graph-API und Azure AD-PowerShell wird diese Rolle als "Unternehmensadministrator" identifiziert. Es ist "Globaler Administrator" im [Azure-Portal](https://portal.azure.com).

- **Compliance-Administrator**:

- **CRM Dienstadministrator**: Benutzer mit dieser Rolle haben globale Berechtigungen in Microsoft CRM Online an, wenn der Dienst vorhanden ist. Weitere Informationen [zu Office 365-Administratorrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-US&ad=US)."

- **Kunden LockBox Access Genehmiger**: Wenn der LockBox Dienst aktiviert ist, können Benutzer mit dieser Rolle Anfragen genehmigen, für Microsoft Access-Firmeninformationen Technikern. Weitere Informationen [zu Office 365-Administratorrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-US&ad=US)."

- **Gerät-Administratoren**: Benutzer mit dieser Rolle werden Administratoren auf allen Windows-10-Geräten, die Azure Active Directory angehören. "

- **Verzeichnis Leser**: Dies ist eine ältere Rolle aus, die auf Anwendungen zugewiesen werden soll, die das [Framework Zustimmung](active-directory-integrating-applications.md)nicht unterstützen. Sie sollten nicht für alle Benutzer zugewiesen werden.

- **Directory-Synchronisierung Konten**: Verwenden Sie nicht. Diese Rolle ist wird automatisch an den Dienst Azure AD verbinden zugewiesen und nicht vorgesehen oder für andere Zwecke unterstützt.

- **Verzeichnis Autoren**: Dies ist eine ältere Rolle aus, die auf Anwendungen zugewiesen werden soll, die das [Framework Zustimmung](active-directory-integrating-applications.md)nicht unterstützen. Sie sollten nicht für alle Benutzer zugewiesen werden.

- **Exchange-Dienstadministrator**: Benutzer mit dieser Rolle haben globale Berechtigungen in Microsoft Exchange Online, wenn der Dienst vorhanden ist. Weitere Informationen [zu Office 365-Administratorrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-US&ad=US)."

- **Dienstadministrator Intune**: Benutzer mit dieser Rolle haben globale Berechtigungen in Microsoft Intune Online an, wenn der Dienst vorhanden ist. Weitere Informationen [zu Office 365-Administratorrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-US&ad=US).

- **Skype für Business Dienstadministrator**: Benutzer mit dieser Rolle besitzen globale Berechtigungen in Microsoft Skype für Unternehmen aus, wenn der Dienst vorhanden ist. Weitere Informationen [zu Office 365-Administratorrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-US&ad=US). Diese Rolle wurde zuvor wie die Rolle des **Lync-Dienstadministrator** bezeichnet.

- **Kennwort Administrator/Helpdesk-Administratoren**: Zurücksetzen von Kennwörtern, verwaltet dienstanforderungen und überwacht die Dienstgüte. Kennwort-Administratoren können nur Kennwörter für Benutzer und andere Administratoren Kennwort zurücksetzen.

    > [AZURE.NOTE] In Microsoft Graph-API, Azure AD Graph-API und Azure AD-PowerShell wird diese Rolle als "Helpdesk-Administrator" identifiziert.

- **SharePoint Dienstadministrator**: Benutzer mit dieser Rolle besitzen globale Berechtigungen in Microsoft SharePoint Online aus, wenn der Dienst vorhanden ist. Weitere Informationen [zu Office 365-Administratorrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-US&ad=US).

- **Dienstadministrator**: verwaltet dienstanforderungen und überwacht die Dienstgüte.

    > [AZURE.NOTE] Um einen Benutzer die Dienst Administratorrolle zuweisen möchten, muss globale Administrator zuerst Zuweisen von Administratorberechtigungen für den Benutzer in den Dienst, wie z. B. Exchange Online, und weisen Sie die Administratorrolle Dienst für den Benutzer in der klassischen Azure-Portal.

- **Benutzer Kontoadministrator**: Zurücksetzen von Kennwörtern und überwacht die Dienstgüte verwaltet Benutzerkonten, die Benutzergruppen und Serviceanfragen. Einige Einschränkungen beziehen sich auf der Berechtigungen von einer Benutzerverwaltungsadministrator. Er können beispielsweise löschen einen globalen Administrator oder erstellen anderen Administratoren. Sie können keine auch Kennwörter für rechnungs-, globale und Dienstadministratoren zurücksetzen.

- **Sicherheit Reader**: schreibgeschützten Zugriff auf eine Anzahl von Features für die Sicherheit der Identität Protection Center, berechtigten Identitätsmanagement, Monitor Office 365-Dienststatus und Office 365-Sicherheit und Compliance Center.

- **Security Administrator**: alle Berechtigungen der Rolle **Sicherheit** sowie eine Anzahl von zusätzlichen Administratorberechtigungen für die gleichen Dienste schreibgeschützt: Identität Protection Center, berechtigten Identitätsmanagement, Monitor Office 365-Dienststatus und Office 365-Sicherheit und Compliance Center.

## <a name="administrator-permissions"></a>Administratorberechtigungen

### <a name="billing-administrator"></a>Abrechnung-administrator

Führen Sie können | Nicht
------------- | -------------
<p>Anzeigen von Informationen Firma und Benutzer</p><p>Office-supporttickets verwalten</p><p>Abrechnung und Einkauf Vorgänge für Office-Produkte ausführen</p> | <p>Benutzerkennwörter zurücksetzen</p><p>Erstellen und Verwalten von Benutzeransichten</p><p>Erstellen, bearbeiten und Löschen von Benutzern und Gruppen und Verwalten von Benutzerlizenzen</p><p>Verwalten von Domänen</p><p>Verwalten von Firmeninformationen</p><p>Administrative Rollen an andere Personen delegieren</p><p>Verzeichnissynchronisierung verwenden</p><p>Anzeigen von Berichten</p>

### <a name="global-administrator"></a>Globaler administrator

Führen können | Nicht
------------- | -------------
<p>Anzeigen von Unternehmen und Benutzer Informationen</p><p>Office-supporttickets verwalten</p><p>Abrechnung und Einkauf Vorgänge für Office-Produkte ausführen</p> <p>Benutzerkennwörter zurücksetzen</p><p>Erstellen und Verwalten von Benutzeransichten</p><p>Erstellen, bearbeiten und Löschen von Benutzern und Gruppen und Verwalten von Benutzerlizenzen</p><p>Verwalten von Domänen</p><p>Verwalten von Firmeninformationen</p><p>Administrative Rollen an andere Personen delegieren</p><p>Verzeichnissynchronisierung verwenden</p><p>Aktivieren Sie oder deaktivieren Sie die kombinierte Authentifizierung</p><p>Anzeigen von Berichten</p> | N/V

### <a name="password-administrator"></a>Kennwortadministrator

Führen Sie können | Nicht
------------- | -------------
<p>Anzeigen von Informationen Firma und Benutzer</p><p>Office-supporttickets verwalten</p><p>Benutzerkennwörter zurücksetzen</p> | <p>Abrechnung und Einkauf Vorgänge für Office-Produkte ausführen</p><p>Erstellen und Verwalten von Benutzeransichten</p><p>Erstellen, bearbeiten und Löschen von Benutzer und Gruppen und Verwalten von Benutzerlizenzen</p><p>Verwalten von Domänen</p><p>Verwalten von Firmeninformationen</p><p>Administrative Rollen an andere Personen delegieren</p><p>Verzeichnissynchronisierung verwenden</p><p>Anzeigen von Berichten</p>

### <a name="service-administrator"></a>Dienstadministrator

Führen Sie können | Kann nicht
------------- | -------------
<p>Anzeigen von Unternehmen und Benutzer Informationen</p><p>Office-supporttickets verwalten</p> | <p>Benutzerkennwörter zurücksetzen</p><p>Abrechnung und Einkauf Vorgänge für Office-Produkte ausführen</p><p>Erstellen und Verwalten von Benutzeransichten</p><p>Erstellen, bearbeiten und Löschen von Benutzern und Gruppen und Verwalten von Benutzerlizenzen</p><p>Verwalten von Domänen</p><p>Verwalten von Firmeninformationen</p><p>Administrative Rollen an andere Personen delegieren</p><p>Verzeichnissynchronisierung verwenden</p><p>Anzeigen von Berichten</p>

### <a name="user-administrator"></a>Benutzeradministrator

Führen Sie können | Kann nicht
------------- | -------------
<p>Anzeigen von Informationen Firma und Benutzer</p><p>Office-supporttickets verwalten</p><p>Zurücksetzen von Benutzerkennwörtern, mit Einschränkungen. Er oder sie können keine Kennwörter für rechnungs-, globale und Dienstadministratoren zurücksetzen.</p><p>Erstellen und Verwalten von Benutzeransichten</p><p>Erstellen, bearbeiten und Löschen von Benutzern und Gruppen und Verwalten von Benutzerlizenzen, mit Einschränkungen. Er kann nicht löschen einen globalen Administrator oder andere Administratoren erstellen.</p> | <p>Abrechnung und Einkauf Vorgänge für Office-Produkte ausführen</p><p>Verwalten von Domänen</p><p>Verwalten von Firmeninformationen</p><p>Administrative Rollen an andere Personen delegieren</p><p>Verzeichnissynchronisierung verwenden</p><p>Aktivieren Sie oder deaktivieren Sie die kombinierte Authentifizierung</p><p>Anzeigen von Berichten</p>

### <a name="security-reader"></a>Sicherheit Reader

In | Führen Sie können
------------- | -------------
Identität Protection Center | Lesen Sie alle Berichte zur Sicherheit und Einstellungsinformationen für Sicherheitsfeatures<ul><li>Anti-spam<li>Verschlüsselung<li>Schutz vor Datenverlust<li>Anti-malware<li>Erweiterte Schutz<li>Anti-phishing<li>Regeln des e-Mail-Flusses
Verwaltung von Berechtigungen Identität | <p>Verfügt über schreibgeschützten Zugriff auf alle Informationen in Azure AD PIM angegeben: Richtlinien und Berichte für Azure AD-rollenzuweisungen Sicherheit prüft und zukünftig Zugriff auf Richtliniendaten und Berichte für Szenarien neben Azure AD-rollenzuweisung lesen.<p>**Nicht** anmelden für Azure AD PIM oder ändern Sie ihn. PIMs-Portal oder über PowerShell kann eine Person in dieser Rolle zusätzliche Rollen (z. B. globaler Administrator oder berechtigten Rolle Administrators) aktivieren, ist der Benutzer ein Kandidat für diese.
<p>Überwachen der Office 365-Dienststatus</p><p>Office 365-Sicherheit und Einhaltung von Vorschriften zentrieren</p> | <ul><li>Lesen und Benachrichtigungen verwalten<li>Lesen Sie die Sicherheitsrichtlinien<li>Lesen Sie Bedrohungsanalyse, Cloud App Discovery und Quarantäne in Suchen und untersuchen<li>Lesen Sie alle Berichte

### <a name="security-administrator"></a>Security Administrator

In | Führen Sie können
------------- | -------------
Identität Protection Center | <ul><li>Alle Berechtigungen der Rolle Sicherheit Reader.<li>Darüber hinaus die Möglichkeit, eine Ausnahme bilden jedoch Zurücksetzen von Kennwörtern alle IPC-Vorgänge ausführen.
Verwaltung von Berechtigungen Identität | <ul><li>Alle Berechtigungen der Rolle Sicherheit Reader.<li>Azure AD-Rollenmitgliedschaften oder Einstellungen **können nicht** verwaltet werden.
<p>Überwachen der Office 365-Dienststatus</p><p>Office 365-Sicherheit und Einhaltung von Vorschriften zentrieren | <ul><li>Alle Berechtigungen der Rolle Sicherheit Reader.<li>Das Feature "erweiterte Schutz" (Schadsoftware & Virus Schutz, bösartiger URL Config gezeichnet, die URL, usw.) können alle Einstellungen konfigurieren.

## <a name="details-about-the-global-administrator-role"></a>Details der globalen Administratorrolle

Globale Administrator hat Zugriff auf alle administrativen Funktionen. Standardmäßig ist die Person, die für ein Abonnement Azure anmeldet für das Verzeichnis die globalen Administratorrolle zugewiesen. Nur globale Administratoren können anderen Administratorrollen zuweisen.

## <a name="assign-or-remove-administrator-roles"></a>Zuweisen oder Entfernen von Administratorrollen

1. Im [Azure klassischen Portal](https://manage.windowsazure.com)klicken Sie auf **Active Directory**, und klicken Sie dann auf den Namen des Verzeichnisses Ihrer Organisation.

2. Klicken Sie auf der Seite **Benutzer** auf den Anzeigenamen des Benutzers, den Sie bearbeiten möchten.

3. Wählen Sie aus der Administratorrolle, die Sie diesem Benutzer zuweisen möchten, oder wählen Sie **Benutzer** aus, wenn Sie eine vorhandene Administratorrolle entfernen möchten, in der Liste **Organisationseinheit Rolle** .

4. Geben Sie im Feld **Alternative e-Mail-Adresse** einer e-Mail-Adresse ein. Diese e-Mail-Adresse ist für wichtige Benachrichtigungen, einschließlich der Kennwortrücksetzung selbst, so dass der Benutzer auf das e-Mail-Konto zugreifen, und zwar unabhängig davon, ob der Benutzer Azure zugreifen kann verwendet werden.

5. Wählen Sie **Zulassen** oder **Blockieren** , um anzugeben, ob den Benutzer anmelden und Dienste zugreifen dürfen.

6. Geben Sie einen Speicherort aus der **Verwendungsstandort** Dropdown-Liste aus.

7. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Ändern von Administratoren für eine Azure Abonnement Informationen Sie [zum Hinzufügen oder Ändern von Azure Administratorrollen](../billing-add-change-azure-subscription-administrator.md)

- Weitere Informationen, wie der Zugriff auf Ressourcen in Microsoft Azure gesteuert wird, finden Sie unter [Grundlegendes zu Ressourcen Access in Azure](active-directory-understanding-resource-access.md)

- Weitere Informationen zur Beziehung von Azure Active Directory zu Ihrem Abonnement Azure finden Sie unter [wie Azure-Abonnements Azure Active Directory zugeordnet sind](active-directory-how-subscriptions-associated-directory.md)

- [Verwalten von Benutzern](active-directory-create-users.md)

- [Verwalten von Kennwörtern](active-directory-manage-passwords.md)

- [Verwalten von Gruppen](active-directory-manage-groups.md)
