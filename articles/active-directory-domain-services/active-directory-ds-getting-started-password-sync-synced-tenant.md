<properties
    pageTitle="Azure-Active Directory-Domänendiensten: Aktivieren der Synchronisierung von Kennwörtern | Microsoft Azure"
    description="Erste Schritte mit Azure Active Directory-Domänendiensten"
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
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Aktivieren Sie die Synchronisierung von Kennwörtern nach Azure Active Directory-Domänendiensten
In den vorherigen Aufgaben ermöglichte Azure Active Directory-Domänendiensten für Ihren Azure AD-Mandanten. Der nächste Vorgang ist Synchronisierung von Kennwörtern in Azure Active Directory-Domänendiensten aktivieren. Nachdem Sie Anmeldeinformationen Synchronisation eingerichtet haben, können Benutzer mit der verwalteten Domäne mit ihren Anmeldeinformationen Ihres Unternehmens anmelden.

Die einzelnen Schritte unterscheiden sich, ob Ihre Organisation eine Cloud nur Azure AD hat Grundlage Mandanten oder für die Synchronisierung mit Ihrem lokalen Verzeichnis mit Azure AD verbinden festgelegt ist.

<br>

> [AZURE.SELECTOR]
- [Cloud nur Azure AD-Mandanten](active-directory-ds-getting-started-password-sync.md)
- [Synchronisiert Azure AD-Mandanten](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-synced-azure-ad-tenant"></a>Aufgabe 5: Aktivieren der Synchronisierung von Kennwörtern nach AAD Domänendiensten für eine synchronisierten Azure AD-Mandanten
A synchronisiert Azure AD-Mandanten mit Ihrer Organisation lokalen Verzeichnis mit Azure AD verbinden synchronisieren festgelegt ist. Azure AD verbinden synchronisiert nicht NTLM und Kerberos-Anmeldeinformationen Azure AD standardmäßig Werten. Um Azure Active Directory-Domänendiensten verwenden zu können, müssen Sie konfigurieren Azure AD-verbinden, um das Synchronisieren von Anmeldeinformationen Hashes für NTLM und Kerberos-Authentifizierung erforderlich ist. Die folgenden Schritte durch Aktivieren der Synchronisierung des der weiteren erforderlichen Anmeldeinformationen zu Ihrem Azure AD-Mandanten.


### <a name="install-or-update-azure-ad-connect"></a>Installieren oder Aktualisieren von Azure AD-verbinden
Installieren Sie die neueste empfohlene Version von Azure AD-verbinden auf eine Domäne verknüpften Computer an. Wenn Sie eine vorhandene Instanz des Installationsprogramms Azure AD verbinden haben, müssen Sie aktualisieren, um die neueste Version von Azure AD verbinden verwenden. Stellen Sie sicher, dass Sie die neueste Version von Azure AD Verbinden immer verwenden, um bekannte Probleme/Fehler zu vermeiden, die möglicherweise bereits behoben wurden.

**[Download Azure AD verbinden](http://www.microsoft.com/download/details.aspx?id=47594)**

Empfohlene Version: **1.1.281.0** - auf 7 September 2016 veröffentlicht.

  > [AZURE.WARNING] Sie müssen die neueste empfohlene Version von Azure AD Herstellen einer Verbindung mit die legacy Kennwort Anmeldeinformationen (für NTLM und Kerberos-Authentifizierung erforderlich) aktivieren die Synchronisierung mit Ihrem Azure AD-Mandanten installieren. Dieses Feature ist nicht verfügbar in früheren Versionen von Azure AD verbinden oder mit dem älteren Dirsync-Tool.

Installation Anweisungen für Azure AD verbinden stehen in den folgenden Artikel - [Erste Schritte mit Azure AD-verbinden](../active-directory/active-directory-aadconnect.md)


### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>Aktivieren der Synchronisierung NTLM und Kerberos Anmeldeinformationen Werten Azure AD-
Führen Sie Folgendes Powershellskript auf jede Active Directory-Struktur, vollständigen Synchronisierung erzwingen, und aktivieren Sie alle lokalen Benutzer Anmeldeinformationen Werten zu Ihrem Azure AD-Mandanten synchronisieren. Dieses Skript ermöglicht die Anmeldeinformationen Hashes erforderlich für NTLM/Kerberos-Authentifizierung zu Ihrem Azure AD-Mandanten synchronisiert werden.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

Abhängig von der Größe Ihres Verzeichnisses (Anzahl der Benutzer, gruppiert usw.), Synchronisierung von Anmeldeinformationen Werten Azure AD-dauert. Die Kennwörter werden verwendbar der Azure-Active Directory-Domänendiensten verwalteten Domäne in Kürze, nachdem die Anmeldeinformationen Hashes in Azure Active Directory synchronisiert haben.


<br>

## <a name="related-content"></a>Siehe auch

- [Aktivieren Sie die Synchronisierung von Kennwörtern nach AAD Domänendiensten für eine Cloud nur Azure AD-Verzeichnis](active-directory-ds-getting-started-password-sync.md)

- [Verwalten einer verwalteten Azure Active Directory-Domänendiensten-Domäne](active-directory-ds-admin-guide-administer-domain.md)

- [Teilnehmen an einem Windows-Computer mit einer verwalteten Azure Active Directory-Domänendiensten-Domäne](active-directory-ds-admin-guide-join-windows-vm.md)

- [Teilnehmen an einer Red Hat Enterprise Linux virtuellen Computern zu einer verwalteten Azure Active Directory-Domänendiensten-Domäne](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
