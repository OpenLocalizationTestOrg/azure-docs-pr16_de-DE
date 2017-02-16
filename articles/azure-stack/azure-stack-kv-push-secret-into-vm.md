<properties
    pageTitle="Bereitstellen ein virtuellen Computers mit einem Zertifikat mit Azure Stapel Schlüssel Tresor | Microsoft Azure"
    description="Erfahren Sie, wie bereitstellen ein virtuellen Computers, und fügen Sie ein Zertifikat aus Azure Stapel Schlüssel Tresor"
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
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="create-vms-and-include-certificates-retrieved-from-key-vault"></a>Erstellen von virtuellen Computern und Zertifikate aus Tresor Schlüssel abgerufen einbeziehen

In Azure-Stapel virtuellen Computern Ressourcenmanager bis Azure bereitgestellt werden, und Sie können nun Zertifikate in Azure Stapel Schlüssel Tresor speichern. Klicken Sie dann platziert Azure Stapel (Anbieter werden bestimmte Microsoft.Compute Ressourcen) sie Ihre virtuellen Computer, wenn die virtuellen Computern bereitgestellt werden. Zertifikate können in vielen Szenarios, einschließlich SSL-Verschlüsselung und basierendes Zertifikatauthentifizierung verwendet werden.

Mithilfe dieser Methode können Sie das Zertifikat schützen. Es ist jetzt nicht in das Bild virtuellen Computer oder in der Anwendung Konfigurationsdateien oder einem anderen unsicheren Speicherort. Durch das Festlegen von entsprechenden Zugriffsrichtlinie für die wichtigsten Tresor, können Sie auch steuern, wer Zugriff auf Ihr Zertifikat erhält. Ein weiterer Vorteil ist, dass Sie alle Ihre Zertifikate an einem Ort in Azure Stapel Schlüssel Tresor verwalten können.

So sieht ein schnellen Überblick über den Prozess:

-   Benötigen Sie ein Zertifikat im PFX-Format.

-   Erstellen eines Key Tresor (mithilfe einer Vorlage oder das folgende Beispielskript) an.

-   Stellen Sie sicher, dass Sie den Schalter EnabledForDeployment aktiviert haben.

-   Laden Sie das Zertifikat als Geheimnis hoch.

## <a name="deploying-vms"></a>Bereitstellen von virtuellen Computern

Dieses Beispielskript erstellt eine Key Tresor, und klicken Sie dann speichert ein Zertifikat in die PFX-Datei in einem lokalen Verzeichnis, zum Key Tresor als Geheimnis gespeichert.

    $vaultName = "contosovault"
    $resourceGroup = "contosovaultrg"
    $location = "local"
    $secretName = "servicecert"
    $fileName = "keyvault.pfx"
    $certPassword = "abcd1234"

    # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

    $fileContentBytes = get-content \$fileName -Encoding Byte

    $fileContentEncoded =
    [System.Convert\]::ToBase64String(\$fileContentBytes)
    $jsonObject = @"
    {
    "data": "\$filecontentencoded",
    "dataType" :"pfx",
    "password": "\$certPassword"
    }
    @$jsonObjectBytes = [System.Text.Encoding\]::UTF8.GetBytes(\$jsonObject)
    $jsonEncoded = \[System.Convert\]::ToBase64String(\$jsonObjectBytes)
    Switch-AzureMode -Name AzureResourceManager
    New-AzureResourceGroup -Name \$resourceGroup -Location \$location
    New-AzureKeyVault -VaultName \$vaultName -ResourceGroupName
    $resourceGroup -Location \$location -sku standard -EnabledForDeployment
    $secret = ConvertTo-SecureString -String \$jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName \$vaultName -Name \$secretName -SecretValue \$secret

Im erste Teil das Skript liest die PFX-Datei, und es als ein JSON-Objekt mit der Datei Inhalt base64-codierte Stores. Dann wird das JSON-Objekt auch base64-codierte.

Als Nächstes erstellt eine neue Ressourcengruppe und erstellt dann eine Key Tresor. Beachten Sie den letzten Parameter für den Befehl New-AzureKeyVault '-EnabledForDeployment', der Zugriff gewährt Azure (speziell für die Ressource Microsoft.Compute-Anbieter) zum Lesen von geheimen Informationen aus dem Schlüssel für Bereitstellungen Vaulting.

Des letzten Befehls speichert das base64-codierte JSON-Objekt einfach im Key Tresor als Geheimnis.

So sieht die Ausgabe aus dem vorherigen Skript aus:

    VERBOSE:  – Created resource group 'contosovaultrg' in
    location
    'eastus'
    ResourceGroupName : contosovaultrg
    Location          : eastus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions NotActions
                        ======= ==========
                        \*
    ResourceId        :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56/re
    sourceGroups/contosovaultrg
    VaultUri             : https://contosovault.vault.azure.net
    TenantId             : xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
    TenantName           : xxxxxxxx
    Sku                  : standard
    EnabledForDeployment : True
    AccessPolicies       : {xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47}
    AccessPoliciesText   :
                           Tenant ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
                           Object ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-b092cebf0c80
                           Application ID         :
                           Display Name           : Derick Developer  (derick@contoso.com)
                           Permissions to Keys    : get, create, delete,
                           list, update, import, backup, restore
                           Permissions to Secrets : all
    OriginalVault        : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId           :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56                 
    /resourceGroups/contosovaultrg/providers/Microsoft.KeyV
                           ault/vaults/contosovault
    VaultName            : contosovault
    ResourceGroupName    : contosovaultrg
    Location             : eastus
    Tags                 : {}
    TagsTable            :
    SecretValue     : System.Security.SecureString
    SecretValueText :
    ew0KImRhdGEiOiAiTUlJSkN3SUJBekNDQ01jR0NTcUdTSWIzRFFFSEFh
    Q0NDTGdFZ2dpME1JSUlzRENDQmdnR0NTcUdTSWIzRFFFSEFhQ0NCZmtF           
    Z2dYMU1JSUY4VENDQmUwR0N5cUdTSWIzRFFFTUNnRUNvSUlFL2pDQ0JQ
    &lt;&lt;&lt; Output truncated… &gt;&gt;&gt;
    Attributes      :
    Microsoft.Azure.Commands.KeyVault.Models.SecretAttributes
    VaultName       : contosovault
    Name            : servicecert
    Version         : e3391a126b65414f93f6f9806743a1f7
    Id              :
    https://contosovault.vault.azure.net:443/secrets/servicecert
                      /e3391a126b65414f93f6f9806743a1f7

Nun können wir eine Vorlage virtueller Computer bereitstellen. Beachten Sie den URI für die geheimen aus der Ausgabe (wie in der vorherigen Ausgabe in grün hervorgehoben).

Sie benötigen eine Vorlage, die sich hier befindet. Die Parameter von besonderem Interesse (außer den üblichen virtueller Computer-Parametern) sind die Tresor Name, der Ressourcengruppe Tresor und den geheimen URI. Natürlich können Sie auch von GitHub herunterladen und ändern Sie nach Bedarf.

Bei diesem virtuellen Computer bereitgestellt wird, Barcode Azure das Zertifikat in den virtuellen Computer an.
Unter Windows werden die Zertifikate in die PFX-Datei mit dem privaten Schlüssel nicht exportiert hinzugefügt. Das Zertifikat wird an die Position des LocalMachine-Zertifikat, mit dem Zertifikat Store hinzugefügt, die der Benutzer bereitgestellt. Unter Linux wird die Zertifikatsdatei /var/lib/waagent im Verzeichnis, mit dem Dateinamen platziert &lt;UppercaseThumbprint&gt;für die X509 .crt Zertifikatsdatei, und &lt;UppercaseThumbprint&gt;.prv für den privaten Schlüssel.
Beide Dateien sind .pem formatiert.

Die Anwendung wird in der Regel findet das Zertifikat mithilfe des Fingerabdrucks und Änderung nicht benötigen.

## <a name="retiring-certificates"></a>Zurückziehen Zertifikate


Im vorherigen Abschnitt wurde die Vorgehensweise um ein neues Zertifikat an Ihrer vorhandenen virtuellen Computer zu übertragen. Aber Ihre alte Zertifikat ist weiterhin auf dem virtuellen Computer und kann nicht entfernt werden. Zur Erhöhung der Sicherheit können Sie das Attribut für die alte geheim auf 'Deaktiviert', ändern, damit auch, wenn eine alte Vorlage zum Erstellen eines virtuellen Computers mit dieser alte Version des Zertifikats versucht, wird. Hier ist, legen Sie eine bestimmte geheime Version deaktiviert werden:

    Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0

## <a name="conclusion"></a>Abschluss


Bei dieser Methode neue kann das Zertifikat, das Bild virtuellen Computer oder der Anwendungsnutzdaten getrennt bleiben. Daher einem Punkt der anzeigen entfernt haben.

Das Zertifikat kann auch erneuert und geladen Schlüssel Tresor ohne das Bild virtueller Computer oder das Bereitstellungspaket Anwendung erneut erstellen. Die Anwendung muss immer noch durch den neuen URI für diese neue Zertifikatsversion bereitgestellt werden sollen.

Indem Sie das Zertifikat von der virtuellen Computers oder der Anwendungsnutzdaten trennen, haben wir nun die Anzahl der Mitarbeiter verringert, die direkten Zugriff auf das Zertifikat.

Ein zusätzlicher Vorteil besteht haben Sie jetzt eine geeignete Stelle im Schlüssel Tresor verwalten Sie alle Ihre Zertifikate, einschließlich der alle Versionen, die über einen Zeitraum bereitgestellt wurden.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen eines virtuellen Computers mit einem Kennwort Tresor-Taste](azure-stack-kv-deploy-vm-with-secret.md)

[Kann eine Anwendung auf-Taste Tresor zugreifen](azure-stack-kv-sample-app.md)