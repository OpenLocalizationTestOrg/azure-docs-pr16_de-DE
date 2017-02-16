<properties
    pageTitle="Erste Schritte mit Azure Stapel Key Tresor | Microsoft Azure"
    description="Erste Schritte mit Azure Stapel Schlüssel Tresor"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ricardom"/>


# <a name="getting-started-with-key-vault"></a>Erste Schritte mit Schlüssel Tresor

In diesem Abschnitt werden die Schritte zum Erstellen einer Tresor, Schlüssel und Kennwörter verwalten sowie Autorisieren von Benutzern oder Applikationen Vorgänge im Tresor in Azure Stapel aufrufen. Den folgenden Schritten wird vorausgesetzt, ein Abonnement Mandanten vorhanden und KeyVault Service innerhalb dieser Abonnement registriert ist. Alle Beispielbefehle basieren auf die Cmdlets KeyVault verfügbar als Teil des SDK PowerShell Azure.

## <a name="enabling-the-tenant-subscription-for-vault-operations"></a>Aktivieren das Abonnement Mandanten für Tresor Vorgänge 

Bevor Sie Operationen für alle Tresor ausgeben können, müssen Sie sicherstellen, dass Ihr Abonnement für Vorgänge Tresor aktiviert ist. Bestätigen Sie, dass durch Ausführen der folgenden PowerShell-Befehls:

    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -AutoSize

Die Ausgabe der obigen Befehl sollten "Registriert" für jede Zeile den Status "Registrierung" melden.

    ProviderNamespace RegistrationState ResourceTypes Locations
    Microsoft.KeyVault Registered {operations} {local}
    Microsoft.KeyVault Registered {vaults} {local}
    Microsoft.KeyVault Registered {vaults/secrets} {local}
    

 Wenn dies nicht der Fall ist, sollten Sie den folgenden Befehl zum Registrieren des KeyVault-Diensts in Ihrem Abonnement aufrufen:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault

Und die folgenden ist das Ergebnis des Befehls:
    
    ProviderNamespace : Microsoft.KeyVault
    RegistrationState : Registered
    ResourceTypes : {operations, vaults, vaults/secrets}
    Locations : {local}


>[AZURE.NOTE] Wenn Sie die Fehlermeldung: "*das Abonnement nicht mit Azure Schlüssel Tresor registriert ist*" KeyVault-Cmdlets zum Aufrufen von bestätigen den Anbieter KeyVault Ressourcen pro Anweisungen oben aktiviert haben.


## <a name="creating-a-hardened-container-a-vault-in-azure-stack-to-store-and-manage-cryptographic-keys-and-secrets"></a>Erstellen einen gesicherten Container (ein Tresor) in Azure Stapel zum Speichern und Verwalten von cryptographic Tasten und Schlüssel

Zum Erstellen einer Tresor, sollte ein Mandanten zuerst eine Ressourcengruppe erstellen. Die folgenden PowerShell Befehle erstellen eine Ressourcengruppe aus, und klicken Sie dann eine Tresor in dieser Gruppe für die Ressource ein. Das Beispiel umfasst auch die Ausgabe die typische von diesem Cmdlet.

### <a name="creating-a-resource-group"></a>Erstellen eine Ressourcengruppe aus:

    New-AzureRmResourceGroup -Name vaultrg010 -Location local -Verbose -Force

Ergebnis:

    VERBOSE: Performing the operation "Replacing resource group ..." on target "".
    VERBOSE: 12:52:51 PM - Created resource group 'vaultrg010' in location 'local'
    ResourceGroupName : vaultrg010
    Location : local
    ProvisioningState : Succeeded
    Tags :
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010
    

### <a name="creating-a-vault"></a>Erstellen einer Tresor an:


    New-AzureRmKeyVault -VaultName vault010 -ResourceGroupName vaultrg010 -Location local -Verbose

Ergebnis:

    VaultUri : https://vault010.vault.azurestack.local
    TenantId : 5454420b-2e38-4b9e-8b56-1712d321cf33
    TenantName : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Sku : Standard
    EnabledForDeployment : False
    EnabledForTemplateDeployment : False
    EnabledForDiskEncryption : False
    AccessPolicies : {5454420b-2e38-4b9e-8b56-1712d321cf33}
    AccessPoliciesText :
    Tenant ID : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Object ID : ca342e90-f6aa-435b-a11c-dfe5ef0bfeeb
    Application ID :
    Display Name : Tenant Admin (tenantadmin1@msazurestack.onmicrosoft.com)
    Permissions to Keys : get, create, delete, list, update, import, backup, restore
    Permissions to Secrets : all
    OriginalVault : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010/providers/Microsoft.KeyVault/vaults/vault010
    VaultName : vault010
    ResourceGroupName : vaultrg010
    Location : local
    Tags : {}
    TagsTable :
    
Dieses Cmdlet die Ausgabe zeigt die Eigenschaften des Webparts für die wichtigsten Tresor, den Sie soeben erstellt haben. Die beiden wichtigsten Eigenschaften sind:

-   **Tresor Namen**: im Beispiel **vault010**hier. Verwenden Sie diesen Namen für andere Schlüssel Tresor Cmdlets.

-   **Tresor URI**: im Beispiel https://vault010.vault.azurestack.local hier. Anwendungen, die Ihrem Tresor durch seine REST-API verwenden müssen diesen URI verwenden.

Ihr Konto Azure ist jetzt auszuführende Vorgänge für diesen Key Tresor autorisiert. Ist noch niemand aus.


## <a name="operating-on-keys-and-secrets"></a>Tasten und Kennwörter Betrieb

Führen Sie nach dem Erstellen einer Tresor an, die unter Schritte zum Erstellen von Verwalten von Tasten und Kennwörter:

### <a name="creating-a-key"></a>Erstellen einen Schlüssel

Verwenden Sie einen Schlüssel erstellen, der **Hinzufügen-AzureKeyVaultKey** pro im folgenden Beispiel wird ein. Nach der erfolgreichen Erstellung Key wird das Cmdlet die neu erstellten wichtigen Details ausgeben.

    Add-AzureKeyVaultKey -VaultName \$vaultName -Name\$keyVaultKeyName -Verbose -Destination Software

Im folgenden werden die Ausgabe der *Hinzufügen-AzureKeyVaultKey* Cmdlets:

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff
    
Sie können jetzt Schlüssel verweisen, die Sie erstellt oder geladen Azure-Taste Tresor, mithilfe des URI. Immer Abrufen der aktuelle Version nicht **Https://vault010.vault.azurestack.local:443/Schlüssel/keyVaultKeyName001** mit. und diese bestimmte Version Abrufen mit **Https://vault010.vault.azurestack.local:443/Schlüssel/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff** .

### <a name="retrieving-a-key"></a>Abrufen von einem Schlüssel

Verwenden Sie die **Get-AzureKeyVaultKey** zum Abrufen von einem Schlüssel und seine Details pro im folgenden Beispiel:

    Get-AzureKeyVaultKey -VaultName vault010 -Name keyVaultKeyName001

So sieht die Ausgabe der Get-AzureKeyVaultKey

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff

### <a name="setting-a-secret"></a>Festlegen einer geheim

    $secretvalue = ConvertTo-SecureString 'User@123' -AsPlainText -Force
    Set-AzureKeyVaultSecret -Name MySecret-VaultName vault010 -SecretValue $secretvalue
    
Die Ausgabe

    Vault Name : vault010
    Name : MySecret
    Version : 65a387f2ed4a416180e852b970846f5b
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret/65a387f2ed4a416180e852b970846f5b
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags : 

### <a name="retrieving-a-secret"></a>Abrufen einer geheim

    Get-AzureKeyVaultSecret -VaultName vault010

Die Ausgabe

    Vault Name : vault010
    Name : MySecret
    Version :
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags :

Nun kann Ihren Key Tresor und Key oder geheim für Applikationen zu verwenden.
Sie müssen Clientanwendungen über diese autorisieren.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Autorisieren Sie die Anwendung verwendet den Schlüssel oder geheim 

Wenn die Anwendung Zugriff auf die Taste oder geheim im Tresor zustimmen möchten, verwenden Sie das Cmdlet "Set -**AzureRmKeyVaultAccessPolicy** " ein.

Wenn der Name Ihres Tresor *ContosoKeyVault ist* und die Anwendung zu autorisieren eine *Client-ID* des *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed hat*und Sie die Anwendung entschlüsseln, und melden Sie sich mit Tasten in Ihrem Tresor autorisieren möchten, führen Sie beispielsweise die folgenden:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Wenn Sie die gleiche Anwendung zum Lesen von vertrauliche Informationen in Ihrem Tresor autorisieren möchten, führen Sie Folgendes aus:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get


## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen eines virtuellen Computers mit einem Kennwort Tresor-Taste](azure-stack-kv-deploy-vm-with-secret.md)

[Bereitstellen eines virtuellen Computers mit einem Zertifikat Tresor-Taste](azure-stack-kv-push-secret-into-vm.md)