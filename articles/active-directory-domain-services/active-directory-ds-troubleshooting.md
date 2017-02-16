<properties
    pageTitle="Azure-Active Directory-Domänendiensten: Problembehandlungsleitfadens | Microsoft Azure"
    description="Problembehandlung bei der Azure-Active Directory-Domänendiensten"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure Active Directory-Domänendiensten - Leitfadens zur Problembehandlung
Dieser Artikel bietet Hinweise zur Problembehandlung für Probleme, die beim Einrichten oder Verwalten von Domänendiensten Azure Active Directory (AD) auftreten können.


## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Sie können keine Azure Active Directory-Domänendienste für Ihr Azure AD-Verzeichnis aktivieren.
Dieser Abschnitt hilft Ihnen die Problembehandlung von Fehlern, wenn Sie versuchen, Azure Active Directory-Domänendiensten für Ihr Verzeichnis aktivieren und fehlschlägt oder wieder in 'Deaktiviert' umgeschaltet wird.

Wählen Sie die Schritte zur Problembehandlung, die entsprechen, die Fehlermeldung, die auftreten.

|**Fehlermeldung**|**Auflösung**|
|---|:---|
|*Der Name contoso100.com ist bereits in diesem Netzwerk. Geben Sie einen Namen, der nicht verwendet wird.*|[Namenskonflikt im Netzwerk virtuelle Domäne](active-directory-ds-troubleshooting.md#domain-name-conflict)
|*Domänendiensten konnte nicht in dieser Azure AD-Mandanten aktiviert werden. Der Dienst keinen entsprechenden Berechtigungen verfügen, die Anwendung namens 'Azure AD-Domäne Services synchronisieren'. Löschen Sie die Anwendung namens 'Azure AD-Domäne Services synchronisieren' und dann versuchen Sie, die für Ihren Mandanten Azure Active Directory-Domänendiensten aktivieren.*|[Domänendiensten hat keinen über die erforderlichen Berechtigungen zur Anwendung Azure AD-Domäne Services synchronisieren](active-directory-ds-troubleshooting.md#inadequate-permissions)|
|*Domänendiensten konnte nicht in dieser Azure AD-Mandanten aktiviert werden. Die Anwendung Domänendiensten in Ihrem Azure AD-Mandanten hat die erforderlichen Berechtigungen zum Aktivieren von Domänendiensten keinen. Löschen Sie die Anwendung mit der Anwendung Bezeichner d87dcbc6-a371-462e-88e3-28ad15ec4e64 und dann versuchen Sie, die für Ihren Mandanten Azure Active Directory-Domänendiensten aktivieren.*|[Die Anwendung Domänendiensten ist in Ihrem Mandanten nicht ordnungsgemäß konfiguriert.](active-directory-ds-troubleshooting.md#invalid-configuration)|
|*Domänendiensten konnte nicht in dieser Azure AD-Mandanten aktiviert werden. Microsoft Azure AD-Anwendung, die in Ihrem Azure AD-Mandanten deaktiviert ist. Aktivieren Sie die Anwendung mit der Anwendung Bezeichner 00000002-0000-0000-c000-000000000000 und dann versuchen Sie, die für Ihren Mandanten Azure Active Directory-Domänendiensten aktivieren.*|[Die Microsoft Graph-Anwendung ist in Ihrem Azure AD-Mandanten deaktiviert.](active-directory-ds-troubleshooting.md#microsoft-graph-disabled)|


### <a name="domain-name-conflict"></a>Namenskonflikt Domäne
**Fehlermeldung:**

*Der Name contoso100.com ist bereits in diesem Netzwerk. Geben Sie einen Namen, der nicht verwendet wird.*

**Behebung:**

Stellen Sie sicher, dass Sie nicht mit eine vorhandene Domäne mit den gleichen Domänennamen für diese virtuelle Netzwerk verfügen. Beispielsweise wird davon ausgegangen Sie, dass Sie eine Domäne "contoso.com" bereits verfügbar für das ausgewählte virtuelle Netzwerk aufgerufen haben. Versuchen Sie später eine Azure-Active Directory-Domänendiensten verwaltete Domäne mit den gleichen Domänennamen (d. h., "contoso.com") in diesem virtuellen Netzwerk aktivieren. Bei dem Versuch, Azure-Active Directory-Domänendiensten aktivieren auftreten einen Fehler.

Dieser Fehler wird aufgrund von Namenskonflikten für den Domänennamen in diesem virtuellen Netzwerk. In diesem Fall müssen Sie einen anderen Namen verwenden, Ihre Azure-Active Directory-Domänendiensten verwalteten Domäne einrichten. Alternativ können Sie heben die vorhandene Domäne bereitstellen und fahren Sie mit Azure Active Directory-Domänendiensten aktivieren.


### <a name="inadequate-permissions"></a>Unzureichender Berechtigungen
**Fehlermeldung:**

*Domänendiensten konnte nicht in dieser Azure AD-Mandanten aktiviert werden. Der Dienst keinen entsprechenden Berechtigungen verfügen, die Anwendung namens 'Azure AD-Domäne Services synchronisieren'. Löschen Sie die Anwendung namens 'Azure AD-Domäne Services synchronisieren' und dann versuchen Sie, die für Ihren Mandanten Azure Active Directory-Domänendiensten aktivieren.*

**Behebung:**

Überprüfen Sie, es ist eine Anwendung mit dem Namen 'Azure AD-Domäne Services synchronisieren' in Ihrem Verzeichnis Azure AD-. Wenn diese Anwendung vorhanden ist, löschen Sie ihn, und aktivieren Sie Azure-Active Directory-Domänendiensten erneut.

Führen Sie die folgenden Schritte aus, um die Anwendung zu überprüfen und zu löschen, wenn die Anwendung vorhanden ist:

  1. Navigieren Sie zu der **Azure klassischen-Portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
  2. Wählen Sie im linken Bereich **Active Directory** -Knotens.
  3. Wählen Sie den Azure AD-Mandanten (Verzeichnis) für den Sie Azure Active Directory-Domänendiensten aktivieren möchten.
  4. Navigieren Sie zur Registerkarte **Applications** .
  5. Wählen Sie die Option **Applikationen wurde von meiner Firma zugegriffen** , in der Dropdownliste aus.
  6. Aktivieren Sie für eine Anwendung namens **Azure AD-Domäne Services synchronisieren**. Wenn die Anwendung vorhanden ist, fahren Sie zu löschen.
  7. Nachdem Sie die Anwendung gelöscht haben, versuchen Sie, Azure-Active Directory-Domänendiensten erneut zu aktivieren.


### <a name="invalid-configuration"></a>Ungültige Konfiguration
**Fehlermeldung:**

*Domänendiensten konnte nicht in dieser Azure AD-Mandanten aktiviert werden. Die Anwendung Domänendiensten in Ihrem Azure AD-Mandanten hat die erforderlichen Berechtigungen zum Aktivieren von Domänendiensten keinen. Löschen Sie die Anwendung mit der Anwendung Bezeichner d87dcbc6-a371-462e-88e3-28ad15ec4e64 und dann versuchen Sie, die für Ihren Mandanten Azure Active Directory-Domänendiensten aktivieren.*

**Behebung:**

Überprüfen Sie, wenn Sie eine Anwendung mit dem Namen 'AzureActiveDirectoryDomainControllerServices' (mit der Anwendung Kennung d87dcbc6-a371-462e-88e3-28ad15ec4e64) in Ihrem Verzeichnis Azure AD-haben. Wenn diese Anwendung vorhanden ist, müssen Sie löschen und Azure-Active Directory-Domänendiensten erneut aktivieren.

Verwenden Sie das folgende PowerShell-Skript zum Suchen nach der Anwendung, und löschen Sie ihn aus.

> [AZURE.NOTE] Dieses Skript verwendet **Azure Version 2 AD-PowerShell** -Cmdlets. Lesen Sie für eine vollständige Liste aller verfügbaren Cmdlets und das Modul herunterladen die [Dokumentation zur AzureAD PowerShell](https://msdn.microsoft.com/library/azure/mt757189.aspx)aus.

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>Microsoft Graph deaktiviert
**Fehlermeldung:**

Domänendiensten konnte nicht in dieser Azure AD-Mandanten aktiviert werden. Microsoft Azure AD-Anwendung, die in Ihrem Azure AD-Mandanten deaktiviert ist. Aktivieren Sie die Anwendung mit der Anwendung Bezeichner 00000002-0000-0000-c000-000000000000 und dann versuchen Sie, die für Ihren Mandanten Azure Active Directory-Domänendiensten aktivieren.

**Behebung:**

Überprüfen Sie, wenn Sie eine Anwendung mit dem Bezeichner 00000002-0000-0000-c000-000000000000 deaktiviert haben. Diese Anwendung ist Microsoft Azure AD-Anwendung und bietet Graph-API Zugriff auf Ihre Azure AD-Mandanten. Azure-Active Directory-Domänendiensten benötigt diese Anwendung aktiviert sein, damit Ihre Azure AD-Mandanten an Ihre Domäne verwalteten synchronisieren.

Um diesen Fehler zu beheben, aktivieren Sie diese Anwendung und dann versuchen Sie, die für Ihren Mandanten Azure Active Directory-Domänendiensten aktivieren.


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Benutzer können keine Anmeldung bei der Azure-Active Directory-Domänendiensten verwalteten Domäne
Wenn Sie einen oder mehrere Benutzer in Ihrem Azure AD-Mandanten nicht bei der neu erstellten verwalteten Domäne anmelden können, führen Sie die folgenden Schritte zur Problembehandlung aus:

- **Anmeldung mit Benutzerprinzipalnamen Format:** Versuchen Sie, melden Sie sich mit dem UPN-Format (z. B. 'joeuser@contoso.com') anstelle der SAMAccountName-Format ('CONTOSO\joeuser'). Der SAMAccountName möglicherweise für Benutzer, dessen UPN Präfix zu langen oder identisch mit einem anderen Benutzer auf die verwaltete Domäne, automatisch generiert. UPN-Format ist immer innerhalb einer Azure AD-Mandanten eindeutig sein.

> [AZURE.NOTE] Es empfiehlt sich, mit dem UPN-Format Anmeldung bei der Azure-Active Directory-Domänendiensten verwalteten Domäne.

- Stellen Sie sicher, dass Sie nach die Schritte in der Leitfaden für erste Schritte [Kennwortsynchronisation aktiviert](active-directory-ds-getting-started-password-sync.md) haben.

- **Externe Konten:** Stellen Sie sicher, dass das Benutzerkonto für die betroffenen nicht mit einem externen Konto in den Azure AD-Mandanten ist. Beispiele für externe Konten sind Microsoft-Konten (z. B. 'joe@live.com') oder aus einer externen Benutzerkonten Azure AD-Verzeichnis. Da Azure Active Directory-Domänendiensten hat keinen Anmeldeinformationen für solche Benutzerkonten, können diese Benutzer zu der verwalteten Domäne anmelden.

- **Synchronisiert Konten:** Wenn die betroffenen Benutzerkonten aus einem lokalen Verzeichnis synchronisiert werden, überprüfen Sie Folgendes aus:
    - Bereitgestellt oder auf die [neueste empfohlene Version von Azure AD verbinden](active-directory-ds-getting-started-password-sync.md#install-or-update-azure-ad-connect)aktualisiert haben.

    - Sie haben, [Führen Sie eine vollständige Synchronisation](active-directory-ds-getting-started-password-sync.md)Azure AD-Verbinden konfiguriert.

    - Abhängig von der Größe Ihres Verzeichnisses möglicherweise es einige Minuten dauern für Benutzerkonten und Anmeldeinformationen Werten Azure Active Directory-Domänendiensten zur Verfügung. Stellen Sie sicher, dass Sie warten Sie so lange vor dem Authentifizierung (abhängig von der Größe von Ihrem Verzeichnis - ein paar Stunden in den Tag oder zwei für großen Verzeichnissen) wiederholen.

    - Wenn das Problem weiterhin besteht, nachdem Sie die vorherigen Schritte überprüft, starten Sie den Dienst Microsoft Azure AD synchronisieren. Starten Sie von Ihrem Computer synchronisieren ein Eingabeaufforderungsfenster, und führen Sie die folgenden Befehle:
      1. Netto beenden 'Microsoft Azure AD synchronisieren'
      2. Netto starten 'Microsoft Azure AD synchronisieren'

- **Nur Cloud - Konten**: des betroffenen Benutzerkontos einer nur-Cloud-Benutzerkonto ist, stellen Sie sicher, dass der Benutzer das Kennwort geändert hat, nachdem Sie Azure Active Directory-Domänendiensten aktiviert. Dieser Schritt führt die Anmeldeinformationen Hashes erforderlich für Azure Active Directory-Domänendiensten generiert werden soll.


## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Benutzer von Ihrer Azure AD-Mandanten entfernt werden aus der verwalteten Domäne nicht entfernt.
Azure AD verhindert, dass unbeabsichtigtes Löschen von Benutzerobjekten. Wenn Sie ein Benutzerkonto aus Ihrem Azure AD-Mandanten löschen, wird das entsprechenden Benutzerobjekt in den Papierkorb verschoben. Wenn dieser Löschvorgang an Ihre verwalteten Domäne synchronisiert wird, wird das entsprechende Benutzerkonto, deaktiviert markiert werden soll. Dieses Feature können Sie wiederherstellen oder das Benutzerkonto später wiederherstellen.

Wenn das Benutzerkonto über Ihre verwalteten Domäne vollständig entfernen möchten, löschen Sie den Benutzer dauerhaft von Ihrem Azure AD-Mandanten. Verwenden Sie das Entfernen-MsolUser PowerShell-Cmdlet mit dem '-RemoveFromRecycleBin' option, wie in diesem [MSDN-Artikel](https://msdn.microsoft.com/library/azure/dn194132.aspx)beschrieben.


## <a name="contact-us"></a>Wenden Sie sich an uns
Wenden Sie sich an den Azure Active Directory-Domänendiensten-Produktteam zu [Feedback freigeben oder technischen Support] (aktiv-Directory-ds-Kontakt-us.md).
