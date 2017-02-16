<properties
    pageTitle="Einrichten von WinRM Zugriff für virtuellen Computern in Azure Ressourcenmanager | Microsoft Azure"
    description="So einrichten WinRM Access für die Verwendung mit einem Ressourcenmanager Azure-virtuellen Computern"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="singhkay"/>

# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>Einrichten von WinRM Zugriff für virtuellen Computern in Azure Ressourcenmanager

## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a>WinRM in Azure Servicemanagement im Vergleich mit einer Azure Ressourcenmanager

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell

* Übersicht über Azure-Manager finden Sie in diesem [Artikel](../azure-resource-manager/resource-group-overview.md)
* Unterschiede zwischen Azure Servicemanagement und Azure Ressourcenmanager finden Sie in diesem [Artikel](../resource-manager-deployment-model.md)

Der wesentliche Unterschied beim Einrichten des WinRM Konfiguration zwischen den beiden Stapeln ist wie das Zertifikat des virtuellen Computers installiert wird. In den Stapel Azure Ressourcenmanager werden die Zertifikate als Ressourcen, die vom Schlüssel Tresor Ressourcenanbieter verwaltet erstellt. Daher muss der Benutzer eigene Zertifikat bereitstellen und auf einer Taste Tresor vor der Verwendung einer virtuellen Computer hochladen.

Hier sind die Schritte, die Sie ergreifen können, um das Einrichten eines virtuellen Computers mit WinRM Connectivity müssen

1. Erstellen eines Key Tresor
2. Erstellen eines selbstsignierten Zertifikats
3. Hochladen des selbst signierten Zertifikats zum Schlüssel Tresor
4. Abrufen der URL für Ihre selbstsignierten Zertifikats im Tresor-Taste
5. Verwiesen Sie beim Erstellen eines virtuellen Computers werden Ihre URL selbstsignierten Zertifikaten

## <a name="step-1-create-a-key-vault"></a>Schritt 1: Erstellen eines Key Tresor

Sie können die unter Befehl aus, um die Taste Tresor erstellen

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>Schritt 2: Erstellen eines selbstsignierten Zertifikats
Sie können ein selbst signiertes Zertifikat verwenden dieses PowerShell Skripts erstellen.

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a>Schritt 3: Hochladen des selbst signierten Zertifikats zum Schlüssel Tresor

Vor dem Hochladen des Zertifikats zum Schlüssel Tresor in Schritt 1 erstellt haben, müssen sie in einem Format an, die der Anbieter für Ressourcen Microsoft.Compute verstanden werden konvertiert. Die unter PowerShell, Skript zulassen, Sie machen, was

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a>Schritt 4: Abrufen der URL für Ihre selbstsignierten Zertifikats im Tresor-Taste

Der Microsoft.Compute-Anbieter für Ressourcen benötigt eine URL zu der geheim innerhalb der Schlüssel Tresor während der Bereitstellung des virtuellen Computer an. Dadurch wird den Anbieter Microsoft.Compute Ressourcen zum Herunterladen der geheim und das entsprechende Zertifikat des virtuellen Computers zu erstellen.

>[AZURE.NOTE]Die URL für die geheimen muss als auch die Version enthalten. Unter Https://contosovault.vault.azure.net:443/Kennwörter/Contososecret/01h9db0df2cd4300a20ence585a6s7ve sieht eine Beispiel-URL


#### <a name="templates"></a>Vorlagen

Geklickt haben können Sie den Link, um die URL in der Vorlage mit den unter Code

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>PowerShell

Gelangen Sie diese URL mit den unter PowerShell-Befehl

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>Schritt 5: Verwiesen Sie werden Ihre URL selbstsignierten Zertifikaten beim Erstellen eines virtuellen Computers

#### <a name="azure-resource-manager-templates"></a>Azure Ressourcenmanager Vorlagen

Beim Erstellen eines virtuellen Computers Vorlagen durch, erhält das Zertifikat im Abschnitt Kennwörter und im Abschnitt WinRM wie folgt auf die verwiesen wird:

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

Beispielvorlage für die oben genannten hier [201-virtueller Computer-Winrm-Keyvault-Windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows) finden Sie unter

Quellcode für diese Vorlage finden Sie auf [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>PowerShell

    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a>Schritt 6: Herstellen einer Verbindung zu den virtuellen Computer
Bevor Sie mit dem virtuellen Computer verbinden können, die Sie sicherstellen müssen, ist der Computer für WinRM remote-Verwaltung konfiguriert. PowerShell als Administrator starten, und führen Sie den folgenden Befehl aus, um sicherzustellen, sind Sie Einrichten von.

    Enable-PSRemoting -Force

>[AZURE.NOTE] Möglicherweise müssen Sie sicherstellen, dass der WinRM-Dienst ausgeführt wird, wenn die oben genannten nicht funktioniert. Sie können diese mit ausführen.`Get-Service WinRM`

Nachdem das Setup abgeschlossen ist, können Sie die Verbindung zu den virtuellen Computer mithilfe der folgenden Befehl

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate