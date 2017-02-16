<properties
    pageTitle="Azure Active Directory-PowerShell Preview-Cmdlets für die Verwaltung der in Azure AD-| Microsoft Azure"
    description="Diese Seite enthält PowerShell Beispiele Sie Ihre Gruppen in Azure Active Directory verwalten können"
    keywords="PowerShell Azure AD, Azure Active Directory, Verwaltung von Gruppen, Gruppen"
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
    ms.date="09/29/2016"
    ms.author="curtand"/>

# <a name="azure-active-directory-preview-cmdlets-for-group-management"></a>Azure Active Directory Preview-Cmdlets für die Verwaltung

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-groups-create-azure-portal.md)
- [Azure klassischen-portal](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Das folgende Dokument wird mit Beispiele zum PowerShell zum Verwalten Ihrer Gruppen in Azure Active Directory (Azure AD) Verwenden von bereitgestellt werden.  Darüber hinaus Informationen zum mit dem Azure AD-PowerShell-Modul Vorschau einrichten abrufen. Zuerst müssen Sie [das Azure AD-PowerShell-Modul herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=828627).

## <a name="installing-the-azure-ad-powershell-module"></a>Azure AD-PowerShell-Modul installieren

Um die Vorschau AzureAD PowerShell-Modul installieren zu können, verwenden Sie die folgenden Befehle:

    PS C:\Windows\system32> install-module azureadpreview

Um zu überprüfen, dass das Modul Preview installiert wurde, verwenden Sie den folgenden Befehl aus:

    PS C:\Windows\system32> get-module azureadpreview

    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     1.1.146.0  azureadpreview                      {Add-AzureADAdministrati...}

Jetzt können Sie beginnen, verwenden die Cmdlets im Modul. Eine vollständige Beschreibung der Cmdlets im Modul AzureAD Vorschau finden Sie in der [Dokumentation der online](https://msdn.microsoft.com/library/azure/mt757216.aspx).

## <a name="connecting-to-the-directory"></a>Herstellen einer Verbindung mit dem Verzeichnis
Bevor Sie das Verwalten von Gruppen mit Azure AD-PowerShell-Vorschau-Cmdlets beginnen können, müssen Sie Ihre PowerShell-Sitzung zu dem Verzeichnis verbinden, die Sie verwalten möchten. Verwenden Sie hierzu den folgenden Befehl aus:

    PS C:\Windows\system32> Connect-AzureAD -Force

Das Cmdlet werden für die Anmeldeinformationen aufgefordert, die Sie zum Zugreifen auf Ihr Verzeichnis verwenden möchten. In diesem Beispiel verwenden wir karen@drumkit.onmicrosoft.com Zugriff auf das Verzeichnis Demo. Das Cmdlet zurückgegeben werden kann eine Bestätigung, um anzuzeigen, dass die Sitzung erfolgreich eine Verbindung zu Ihrem Verzeichnis hergestellt wurde:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Sie können nun mithilfe der AzureAD Preview-Cmdlets zum Verwalten von Gruppen in Ihrem Verzeichnis.

## <a name="retrieving-groups"></a>Abrufen von Gruppen
Zum Abrufen von vorhandener Gruppen aus Ihrem Verzeichnis können Sie das Cmdlet "Get-AzureADGroups" verwenden. Verwenden Sie das Cmdlet ohne Parameter aus, um alle Gruppen im Verzeichnis abzurufen:

    PS C:\Windows\system32> get-azureadgroup

Das Cmdlet gibt alle Gruppen in der verbundenen Verzeichnis zurück.

Den Parameter - ObjectID können zum Abrufen einer bestimmten Gruppe für die Sie die Gruppe ObjectID angeben:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

Das Cmdlet gibt nun die Gruppe zurück, deren ObjectID den Wert des Parameters entspricht, die Sie eingegeben haben:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Suche nach einer bestimmten Gruppe verwenden können Sie den Filterparameter. Für diesen Parameter übernimmt eine ODATA-Filter-Klausel, und gibt alle Gruppen, die den Filter, wie im folgenden Beispiel entsprechen:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Beachten Sie, dass die AzureAD PowerShell-Cmdlets Implementieren des OData-Abfrage Standards, Weitere Informationen finden Sie [hier](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Erstellen von Gruppen
Verwenden Sie zum Erstellen einer neuen Gruppe in Ihrem Verzeichnis das Cmdlet "New-AzureADGroup" ein. Dieses Cmdlet erstellt eine neue Sicherheitsgruppe mit der Bezeichnung "Marketing":

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Aktualisieren von Gruppen
Verwenden Sie das Cmdlet "Set-AzureADGroup" aus, um eine vorhandene Gruppe aktualisieren. In diesem Beispiel ändern wir die Eigenschaft DisplayName der Gruppe "Intune-Administratoren". Zunächst wird das Cmdlet "Get-AzureADGroup" mit die Gruppe suchen und mit dem Attribut DisplayName filtern:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Als Nächstes haben wir die Description-Eigenschaft auf den neuen Wert "Intune Gerät Administratoren" ändern:

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Nun, wenn wir die Gruppe erneut Sie finden, dass die Description-Eigenschaft aktualisiert wird sehen, um den neuen Wert wiederzugeben:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Löschen von Gruppen
Um Gruppen aus Ihrem Verzeichnis löschen möchten, verwenden Sie das Cmdlet entfernen-AzureADGroup wie folgt:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>Mitglieder von Gruppen verwalten
Wenn Sie Mitglieder zu einer Gruppe hinzufügen müssen, verwenden Sie das Cmdlet AzureADGroupMember hinzufügen. Dieser Befehl fügt ein Mitglied zur Gruppe Intune-Administratoren, die wir im vorherigen Beispiel verwendet:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Der Parameter - ObjectId ist die ObjectID der Gruppe, die wir ein Mitglied hinzufügen möchten, und die - RefObjectId ist die ObjectID des Benutzers, die wir als Mitglied der Gruppe hinzufügen möchten.

Verwenden Sie die vorhandene Mitglieder der Gruppe, um das Cmdlet Get-AzureADGroupMember, wie im folgenden Beispiel:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                        8120cc36-64b4-4080-a9e8-23aa98e8b34f User

Um das Element zu entfernen, die, das wir zuvor der Gruppe hinzugefügt haben, verwenden Sie das Cmdlet entfernen-AzureADGroupMember, wie hier dargestellt wird:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Um zu überprüfen, welche die Gruppenmitgliedschaften eines Benutzers nachschlagen, verwenden Sie das Cmdlet AzureADGroupIdsUserIsMemberOf auswählen. Dieses Cmdlet verwendet als Parameter ObjectId des Benutzers für die die Gruppenmitgliedschaft überprüfen, und eine Liste der Gruppen, deren Mitgliedschaften überprüft. Die Liste der Gruppen muss, sofern in Form einer komplexen Variablen vom Typ "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck", damit wir zuerst eine Variable mit diesem Typ erstellen müssen:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Als Nächstes stellen wir die Werte für die GroupIds So checken Sie das Attribut "GroupIds" dieser komplexen Variablen:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Jetzt, wenn wir die Gruppenmitgliedschaft eines Benutzers mit ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea gegen den Gruppen in $g überprüfen möchten, sollten wir verwenden:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                               Value
    -------------                                                                                               -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


Der zurückgegebene Wert ist eine Liste der Gruppen, die dieser Benutzer ein Mitglied ist. Sie können auch diese Methode zum Überprüfen der Kontakte, Gruppen oder Dienst Hauptbenutzer Mitgliedschaft in einer bestimmten Liste mit Gruppen mit Select-AzureADGroupIdsContactIsMemberOf, wählen Sie-AzureADGroupIdsGroupIsMemberOf oder Select-AzureADGroupIdsServicePrincipalIsMemberOf anwenden.

## <a name="managing-owners-of-groups"></a>Besitzer von Gruppen verwalten
Um Besitzer einer Gruppe hinzufügen möchten, verwenden Sie das Cmdlet AzureADGroupOwner hinzufügen:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Der Parameter - ObjectId ist die ObjectID der Gruppe, die wir einen Besitzer hinzufügen möchten, und der - RefObjectId ist die ObjectID des Benutzers, die wir als Besitzer der Gruppe hinzufügen möchten.

Zum Abrufen der Besitzer einer Gruppe verwenden Sie die Get-AzureADGroupOwner aus:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

Das Cmdlet gibt die Liste der Besitzer für die angegebene Gruppe zurück:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Wenn Sie einen Besitzer aus einer Gruppe entfernen möchten, verwenden Sie-AzureADGroupOwner entfernen:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="next-steps"></a>Nächste Schritte

Sie können weitere Azure-Active Directory-PowerShell-Dokumentation zur [Azure-Active Directory-Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260)suchen.

* [Verwalten des Zugriffs auf Ressourcen mit Azure-Active Directory-Gruppen](active-directory-manage-groups.md)

* [Integrieren von Ihrem lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
