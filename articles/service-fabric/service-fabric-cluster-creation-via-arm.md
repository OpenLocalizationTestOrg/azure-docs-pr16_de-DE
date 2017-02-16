
<properties
   pageTitle="Erstellen Sie einen sicheren Dienst Fabric Cluster mit Azure Ressourcenmanager | Microsoft Azure"
   description="Dieser Artikel beschreibt, wie eine sichere Dienst Fabric Cluster in Azure mit Azure Ressourcenmanager, Azure-Taste Tresor und Azure Active Directory (AAD) für Client-Authentifizierung einrichten."
   services="service-fabric"
   documentationCenter=".net"
   authors="chackdan"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="create-a-service-fabric-cluster-in-azure-using-azure-resource-manager"></a>Erstellen Sie einen Dienst Fabric Cluster in Azure mit Azure Ressourcenmanager

> [AZURE.SELECTOR]
- [Azure Ressourcenmanager](service-fabric-cluster-creation-via-arm.md)
- [Azure-portal](service-fabric-cluster-creation-via-portal.md)

Dies ist eine schrittweise Anleitung, die Sie durch die Schritte zum Einrichten von einem sicheren Azure Service Fabric Cluster in Azure mit Azure Ressourcenmanager geführt. Dieses Handbuch führt Sie durch die folgenden Schritte aus:

 - Einrichten von Key Tresor Tasten zum Cluster und Anwendung Sicherheit zu verwalten.
 - Erstellen Sie einen gesicherten Cluster in Azure Azure Ressourcenmanager.
 - Benutzerauthentifizierung mit Azure Active Directory (AAD) für die Clusterverwaltung.

Ein sicherer Cluster handelt es sich um einen Cluster, der verhindert den nicht autorisierten Zugriff auf Management-Vorgänge, wozu auch bereitstellen, aktualisieren und Löschen von Applications, Services und die darin enthaltenen Daten. Ein unsicher Cluster ist ein Cluster, die jeder Herstellen einer Verbindung zu einem beliebigen Zeitpunkt mit und Management Vorgänge ausführen kann. Obwohl es so erstellen Sie einen unsicheren Cluster möglich ist, ist es **dringend empfohlen, um eine sichere Cluster zu erstellen**. Ein neuer Cluster muss eine unsicheren Cluster, **können nicht geschützt werden, höher** ist – erstellt werden.

Die Konzepte sind für das Erstellen der secure Cluster, identisch Zuordnungseinheiten Linux Cluster oder Windows-Cluster sind. Weitere Informationen und Helper Skripts für das Erstellen der secure Linux Cluster finden Sie unter [Erstellen von secure Cluster unter Linux](#secure-linux-clusters)

## <a name="log-in-to-azure"></a>Melden Sie sich bei Azure
Dieses Handbuch verwendet [Azure PowerShell][azure-powershell]. Wenn Sie eine neue PowerShell-Sitzung zu starten, melden Sie sich bei Ihrem Azure-Konto, und wählen Sie Ihr Abonnement vor dem Ausführen von Azure Befehlen aus.

Melden Sie sich bei Ihrem Konto Azure aus:

```powershell
Login-AzureRmAccount
```

Wählen Sie Ihr Abonnement aus:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a>Einrichten von Key Tresor

In diesem Abschnitt führt durch Erstellen einer Taste Tresor für einen Dienst Fabric Cluster in Azure und für Applikationen Dienst Fabric. Eine vollständige Anleitung auf-Taste Tresor, schlagen Sie in der [Schlüssel Tresor erste Schritte mit][key-vault-get-started].

Dienst Fabric verwendet x. 509-Zertifikate, mit einen Cluster secure Anwendung Sicherheitsfeatures zur Verfügung. Azure-Taste Tresor dient zum Verwalten von Zertifikaten für Dienst Fabric Cluster in Azure. Wenn ein Cluster in Azure bereitgestellt wird, der Azure Ressourcenanbieter Dienst Fabric Cluster Softwareentwickler Zertifikate von Key Tresor abruft; installiert sie auf dem Cluster virtuellen Computern

Das folgende Diagramm veranschaulicht die Beziehung zwischen Schlüssel Tresor, einem Dienst Fabric Cluster und der Anbieter Azure Ressourcen, der Zertifikate in Schlüssel Tresor gespeichert werden, wenn es sich um einen Cluster erstellt verwendet:

![Zertifikatinstallation][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Der erste Schritt besteht im Erstellen einer Ressourcengruppe speziell für Schlüssel Tresor. Einer eigenen Ressourcengruppe Schlüssel Tresor Inbetriebnahme wird empfohlen. So können Sie die Datenverarbeitung und Speicher Ressourcengruppen, einschließlich Entfernen der Ressourcengruppe, die Ihrem Dienst Fabric Cluster ohne Verlust der Schlüssel und Kennwörter enthält. Die Ressourcengruppe aus, die Ihrem Tresor Schlüssel hat muss in der gleichen Region als Cluster, die es verwendet wird.

```powershell

    New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    
    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Erstellen von Key Tresor 

Erstellen Sie einen Schlüssel Tresor in die neue Ressourcengruppe ein. Der Schlüssel Tresor **für Bereitstellung aktiviert werden muss** für den Dienst Fabric Ressourcenanbieter abzurufenden Zertifikate aus es, und installieren Sie auf Knoten im Cluster zulassen:

```powershell

    New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment
    
    
    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all
    
    
    Tags                             :
```

Wenn Sie eine vorhandene Schlüssel Tresor verfügen, können Sie es zur Bereitstellung mit Azure CLI aktivieren:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```

<a id="add-certificate-to-key-vault"></a>
## <a name="add-certificates-to-key-vault"></a>Hinzufügen von Zertifikaten zum Schlüssel Tresor

Zertifikate sind in Service Architektur zur Bereitstellung von Authentifizierung und Verschlüsselung verschiedene Aspekte von einem Cluster und deren Applikationen gesichert. Weitere Informationen darüber, wie Zertifikate Dienst Struktur verwendet werden, finden Sie unter [Service Fabric Cluster Sicherheitsszenarien][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Cluster und Server-Zertifikat (erforderlich) 

Dieses Zertifikat ist erforderlich, um einen Cluster secure und nicht autorisierten Zugriff darauf verhindern. Es bietet Clustersicherheit auf einige Arten:
 
 - **Cluster Authentifizierung:** Authentifiziert Knoten-zu-Knoten-Kommunikation für Cluster Föderation an. Nur Knoten, die ihre Identität mit dieser Urkunde nachweisen können können Cluster teilnehmen.
 - **Serverauthentifizierung:** Authentifiziert die Cluster Verwaltung von Endpunkten in einer Management-Client, damit der Management-Client weiß, dass es in den richtigen Cluster zu sprechen. Dieses Zertifikat enthält auch SSL für die Verwaltung HTTPS API und Dienst Fabric Explorer über HTTPS.

Um diesen Zwecken verwendet werden kann, muss das Zertifikat die folgenden Anforderungen entsprechen:

 - Das Zertifikat muss einen privaten Schlüssel enthalten.
 - Das Zertifikat muss für Key Exchange, exportiert in eine Datei Personal Information Exchange (PFX-Datei) erstellt werden.
 - Der Name des Zertifikats Betreff muss die Zugriff auf den Dienst Fabric Cluster verwendete Domäne übereinstimmen. Diese Matchng ist erforderlich, um SSL für der Clusters HTTPS Management Endpunkte und Dienst Fabric Explorer bereitzustellen. Sie können kein SSL-Zertifikat von einer Zertifizierungsstelle (CA) beziehen, für die `.cloudapp.azure.com` Domäne. Sie müssen einen benutzerdefinierten Domänennamen für Ihren Cluster erwerben. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Zertifikats Betreff des benutzerdefinierten Domänennamens für Ihren Cluster verwendet übereinstimmen.

### <a name="application-certificates-optional"></a>Anwendung Zertifikate (optional)

Eine beliebige Anzahl zusätzlicher Zertifikate kann in einem Cluster aus Sicherheitsgründen Anwendung installiert werden. Beachten Sie vor dem Erstellen des Clusters an, die Anwendung Sicherheitsszenarien, die erfordern ein Zertifikat auf den Knoten installiert werden, wie ein:

 - Verschlüsseln und Entschlüsseln des Anwendung Konfigurationswerte
 - Verschlüsselung von Daten über Knoten während der Replikation 

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Formatierung von Zertifikaten für die Verwendung der Ressource Azure-Anbieter

Private Schlüssel Dateien (PFX-Datei) können hinzugefügt und direkt über Schlüssel Tresor verwendet werden. Der Anbieter Azure Ressourcen erfordert jedoch Tasten in einem speziellen JSON-Format gespeichert werden, die die PFX-Datei als Base-64 enthält codierte Zeichenfolge und das Kennwort für den privaten Schlüssel. Um diese Anforderungen aufnehmen zu können, müssen Tasten in eine JSON-Zeichenfolge platziert und dann als *Kennwörter* in Schlüssel Tresor gespeichert werden.

Um dieses Verfahren zu erleichtern, steht ein PowerShell-Modul [auf GitHub][service-fabric-rp-helpers]. Wie folgt vor, um das Modul verwenden:

  1. Herunterladen des gesamten Inhalts der Repo in einem lokalen Verzeichnis. 
  2. Importieren des Moduls in der PowerShell-Fenster an:

  ```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
  ```
     
Die `Invoke-AddCertToKeyVault` Befehl in diesem PowerShell-Modul automatisch einen Zertifikat privater Schlüssel in eine JSON-Zeichenfolge formatiert, und lädt diese in Tresor-Taste. Verwenden sie das Zertifikat Cluster und alle weiteren Anwendung Zertifikate zum Schlüssel Tresor hinzuzufügen. Wiederholen Sie diesen Schritt für jede zusätzliche Zertifikate, die Sie in Ihren Cluster installieren möchten.

```powershell
 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"
    
    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault
    
    
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Die vorherigen Zeichenfolgen sind alle erforderlichen Schlüssel Tresor Komponenten für das Konfigurieren der Dienst Fabric Cluster Ressourcenmanager Vorlage, die Zertifikate für Knoten Authentifizierung, Management Endpunkt Sicherheit und Authentifizierung installiert, und alle weiteren Anwendung Sicherheitsfeatures, die x. 509-Zertifikate verwenden. An diesem Punkt sollten Sie jetzt die folgende Einrichtung in Azure haben:

 - Taste Tresor Ressourcengruppe
   - Key Tresor
     - Cluster Serverzertifikat-Authentifizierung
     - Anwendung Zertifikate

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Einrichten von Azure Active Directory für Client-Authentifizierung

AAD ermöglicht Organisationen (als Mandanten bezeichnet) zum Verwalten des Benutzerzugriffs auf bereit, mit denen in Clientanwendungen mit einer webbasierten Login Benutzeroberfläche unterteilt sind und Anwendungen mit einer native Client zur Verfügung steht. In diesem Dokument wird davon ausgegangen, dass Sie bereits einen Mandanten erstellt haben. Wenn nicht, lesen [So erhalten Sie eine Azure Active Directory-Mandanten]zunächst[active-directory-howto-tenant].

Ein Dienst Fabric Cluster bietet mehrere Felder und Optionen zu deren Management-Funktionen, einschließlich der webbasierten [Dienst Fabric Explorer] [ service-fabric-visualizing-your-cluster] und [Visual Studio][service-fabric-manage-application-in-visual-studio]. Daher erstellen Sie zwei AAD Applikationen zum Steuern des Zugriffs auf den Cluster, eine Web-Anwendung und einer systemeigenen Anwendung.

Um einige der Schritte, die beim Konfigurieren AAD mit einem Dienst Fabric Cluster zu vereinfachen, haben wir eine Reihe von Windows PowerShell-Skripts erstellt.

>[AZURE.NOTE] Sie müssen Ausführen dieser Schritte *vor dem* Cluster dies in Fällen, in denen die Skripts, Clusternamen und die Endpunkte erwarten, erstellen sollten diese die geplanten Werte, nicht diejenigen, die Sie bereits erstellt haben.

1. [Laden Sie die Skripts] [ sf-aad-ps-script-download] auf Ihren Computer.

2. Mit der rechten Maustaste in der Zip-Datei, wählen Sie **Eigenschaften**, und klicken Sie dann das Kontrollkästchen **Blockierung aufheben** und anwenden.

3. Extrahieren der Zip-Datei.

4. Führen Sie `SetupApplications.ps1`, die TenantId, ClusterName und WebApplicationReplyUrl als Parameter bereitstellen. Beispiel:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    Sie können Ihre **TenantId** durch Ausführen des PowerShell-Befehls "Get-AzureSubscription'' suchen '. Dadurch wird die **TenantId** für jedes Abonnement angezeigt.

    Der **ClusterName** wird verwendet, um die AAD Applikationen erstellt, das Skript Präfix wird. Es muss nicht den tatsächlichen Clusternamen übereinstimmen, genau wie erleichtern für Sie den Dienst Fabric Cluster AAD Elemente zuordnen, die sie mit verwendet werden, sind nur vorgesehen ist.

    Die **WebApplicationReplyUrl** ist der Standardendpunkt, den für Ihre Benutzer AAD zurückgibt, nach Abschluss des Entwurfsprozesses Anmeldung. Sie sollten dies auf den Dienst Fabric Explorer-Endpunkt für Ihren Cluster, festlegen die standardmäßig wird:

    https://&lt;Cluster_domain&gt;: 19080/Explorer

    Sie werden aufgefordert, melden Sie sich bei einem Konto, das über Administratorberechtigungen für den Mandanten AAD verfügt. Wenn Sie dies tun, wird das Skript zum Erstellen der Web und systemeigenen Applikationen zum Darstellen des Dienst Fabric Clusters fortgesetzt. Wenn Sie des Mandanten Applications im [Azure klassischen Portal betrachten][azure-classic-portal], sollten Sie zwei neue Einträge sehen:

    - *ClusterName*\_Cluster
    - *ClusterName*\_Client

    Die Skripts gedruckt, die durch die Vorlage für Ressourcenmanager Azure erforderliche, beim Erstellen des Clusters im nächsten Abschnitt Json also beibehalten die PowerShell-Fenster zu öffnen.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Erstellen einer Dienst Fabric Cluster Ressourcenmanager Vorlage

In diesem Abschnitt werden die Ausgabe der vorherigen PowerShell Befehle in einem Dienst Fabric Cluster Ressourcenmanager Vorlage verwendet.

Ressourcenmanager Stichprobe Vorlagen stehen im [Azure Schnellstart Vorlagenkatalog auf GitHub][azure-quickstart-templates]. Diese Vorlagen können als Ausgangspunkt für die Vorlage Cluster verwendet werden. 

### <a name="create-the-resource-manager-template"></a>Erstellen der Vorlage Ressourcenmanager

Dieses Handbuch verwendet [secure Cluster mit 5 Knoten] [ service-fabric-secure-cluster-5-node-1-nodetype-wad] Beispielvorlage und Vorlagenparameter. Herunterladen von `azuredeploy.json` und `azuredeploy.parameters.json` auf Ihren Computer, und öffnen Sie beide Dateien in Ihrem bevorzugten Editor.

### <a name="add-certificates"></a>Hinzufügen von Zertifikaten

Zertifikate werden zu einer Cluster Ressourcenmanager Vorlage durch einen Verweis auf den Schlüssel Tresor mit Zertifikatsschlüssel hinzugefügt. Es wird empfohlen, dass diese Taste Tresor Werte in einer Ressourcenmanager Parameter Vorlagendatei beibehalten der Ressourcenmanager Vorlage Datei wieder verwendbare und Werte, die speziell für eine Bereitstellung kostenlos platziert werden.

#### <a name="add-all-certificates-to-the-vmss-osprofile"></a>Fügen Sie alle Zertifikate hinzu, um die VMSS osProfile

Jedes Zertifikat, das im Cluster installiert sein muss, muss im Abschnitt OsProfile der Ressource VMSS (Microsoft.Compute/virtualMachineScaleSets) konfiguriert sein. Dies weist den Anbieter für Ressourcen, das Zertifikat auf den virtuellen Computern zu installieren. Dies umfasst das Zertifikat Cluster Zertifikate als auch alle Anwendung Sicherheit, den, die Sie für Ihre Applikationen verwenden möchten:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-service-fabric-cluster-certificate"></a>Konfigurieren der Dienst Fabric Cluster Zertifikat

Das Cluster Authentifizierungszertifikat muss auch in der Dienst Fabric Clusterressource (Microsoft.ServiceFabric/clusters) und in der Dienst Fabric-Erweiterung für VMSS in der Ressource VMSS konfiguriert sein. Dadurch wird den Dienst Fabric-Anbieter für die Ressourcen zur Verwendung für Cluster und Serverauthentifizierung für Management Endpunkte konfigurieren.

##### <a name="vmss-resource"></a>VMSS Ressourcen:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Dienst Fabric Ressourcen:

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-aad-config"></a>Einfügen von AAD config

Die zuvor erstellte AAD-Konfiguration direkt an Ihre Vorlage Ressourcenmanager eingefügt werden kann jedoch empfohlen wird die Werte in Parameter zuerst in eine Parameterdatei, die Vorlage Ressourcenmanager wiederverwendet und kostenlose Werte bestimmten in eine Bereitstellung beibehalten extrahiert werden sollen.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### <a name="a-configure-arm-aconfigure-resource-manager-template-parameters"></a>< ein "Konfigurieren-Cloud" ></a>Vorlagenparameter Ressourcenmanager konfigurieren

Verwenden Sie schließlich Ausgabewerte der Schlüssel Tresor und AAD PowerShell-Befehle die Parameterdatei gefüllt wird:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
An diesem Punkt sollten Sie jetzt die folgenden haben:

 - Taste Tresor Ressourcengruppe
    - Key Tresor
    - Cluster Serverzertifikat-Authentifizierung
    - Daten-Chiffrierung Zertifikat
 - Azure Active Directory-Mandanten 
    - AAD Anwendung für Web-basiertes Management und Service Fabric-Explorer
    - AAD Anwendung für die Clientverwaltung von native
    - Benutzer mit zugewiesenen Rollen 
 - Dienst Fabric Cluster Ressourcenmanager Vorlage
    - Über die Taste Tresor konfigurierte Zertifikate
    - Azure-Active Directory konfiguriert 

Das folgende Diagramm veranschaulicht, in dem Schlüssel Tresor und AAD Konfiguration in Ihre Ressourcenmanager Vorlage passt.

![Ressourcenmanager Abhängigkeit Karte][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>Erstellen des Clusters

Sie können nun mithilfe der [Cloud-Bereitstellung]Cluster erstellen[resource-group-template-deploy].

#### <a name="test-it"></a>Testen Sie

Verwenden Sie den folgenden PowerShell-Befehl So testen Sie Ihre Vorlage Ressourcenmanager mit einer Parameterdatei ein:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>Es bereitstellen

Wenn das Ressourcenmanager Vorlage erfolgreich überprüft wird, verwenden Sie den folgenden PowerShell-Befehl die Ressourcenmanager Vorlage mit einer Parameterdatei bereitgestellt:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>
## <a name="assign-users-to-roles"></a>Zuweisen von Benutzern zu Rollen

Nachdem Sie die Anwendungen zum Darstellen des Clusters erstellt haben, müssen Sie die Rollen von Fabric Service unterstützt die Benutzer zuweisen: schreibgeschützt und Administrator. Sie können dazu die [Azure klassischen Portal][azure-classic-portal].

1. Navigieren Sie zu Ihrem Mandanten, und wählen Sie Applikationen.
2. Wählen Sie die Webanwendung, die einen Namen hat wie `myTestCluster_Cluster`.
3. Klicken Sie auf die Registerkarte Benutzer.
4. Wählen Sie einen Benutzer zuweisen, und klicken Sie auf die Schaltfläche **zuweisen** am unteren Rand des Bildschirms ein.

    ![Zuweisen von Benutzern zu Rollen-Schaltfläche][assign-users-to-roles-button]

5. Wählen Sie die Rolle des Benutzers zuweisen.

    ![Zuweisen von Benutzern zu Rollen][assign-users-to-roles-dialog]

>[AZURE.NOTE] Weitere Informationen zu Rollen in Dienst Architektur finden Sie unter [rollenbasierte Access Control für Fabric Service Clients](service-fabric-cluster-security-roles.md).

 <a name="secure-linux-cluster"></a> 
##  <a name="create-secure-clusters-on-linux"></a>Erstellen sicherer Cluster unter Linux

Um den Vorgang zu erleichtern, ein Helper Skript bereitgestellt wurde [hier](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Für die Verwendung dieses Helper Skripts, wird davon ausgegangen, dass Sie verfügen bereits über Azure CLI installiert ist, ist in Ihrem Pfad. Stellen Sie sicher, dass das Skript Berechtigungen zum Ausführen von ausgeführt hat `chmod +x cert_helper.py` nach dem herunterladen. Dieser erste Schritt besteht zur Anmeldung bei Ihrem Azure-Konto mithilfe der CLI mit den `azure login` Befehl. Verwenden Sie nach der Azure-Konto anmelden die Helper mit Ihrer Zertifizierungsstelle signiertes Zertifikat, wie im folgenden Befehl ein:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]

The -ifile parameter can take a .pfx or a .pem file as input, with the certificate type (pfx or pem, or ss if it is a self-signed cert).
The parameter -h prints out the help text.
```

Dieser Befehl gibt die folgenden drei Zeichenfolgen wie die Ausgabe an: 

1. Eine SourceVaultID, also die ID für die neue KeyVault ResourceGroup, die es für Sie erstellt haben. 

2. Eine CertificateUrl für den Zugriff auf das Zertifikat.

3. Eine CertificateThumbprint, die für die Authentifizierung verwendet wird.


Im folgenden Beispiel wird gezeigt, wie den Befehl verwendet:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Ausführen des vorherigen Befehls bietet Ihnen die drei Zeichenfolgen wie folgt:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

 Der Name des Zertifikats Betreff muss die Zugriff auf den Dienst Fabric Cluster verwendete Domäne übereinstimmen. Dies ist erforderlich, um SSL für der Clusters HTTPS Management Endpunkte und Dienst Fabric Explorer bereitzustellen. Sie können kein SSL-Zertifikat von einer Zertifizierungsstelle (CA) beziehen, für die `.cloudapp.azure.com` Domäne. Sie müssen einen benutzerdefinierten Domänennamen für Ihren Cluster erwerben. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern muss der Name des Zertifikats Betreff des benutzerdefinierten Domänennamens für Ihren Cluster verwendet übereinstimmen.

Hierbei handelt es sich um die Einträge zum Erstellen von eines sicheren Dienst Fabric Clusters (ohne AAD) beschriebenen am [Ressourcenmanager konfigurieren Vorlagenparameter](#configure-arm)erforderlich. Sie können mit dem sicheren Cluster über Anweisungen bei [Authentifizierung des Clientzugriffs auf einem Cluster](service-fabric-connect-to-secure-cluster.md)herstellen. Linux Vorschau Cluster unterstützt AAD Authentifizierung nicht. Sie können den Administrator und Client Rollen [Zuweisen von Rollen für Benutzer](#assign-roles)im Abschnitt beschriebenen zuweisen. Wenn Sie Administrator und Client Rollen für einen Linux Vorschau Cluster angeben, müssen Sie bieten Zertifikatfingerabdruck für die Authentifizierung (im Gegensatz zu den Betreff Name, da keine Kette Überprüfung oder Sperrung in dieser Preview-Version ausgeführt wird).


Wenn Sie ein selbst signiertes Zertifikat zum Testen verwenden möchten, können Sie dasselbe Skript verwenden, um ein selbst signiertes Zertifikat generieren und Hochladen auf KeyVault, indem Sie die Kennzeichnung `ss` anstelle der Zertifikatspfad und Zertifikatsname bereitstellt. Beispielsweise finden Sie unter den folgenden Befehl aus, für das Erstellen und ein selbst signiertes Zertifikat hochladen:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net" 
```

Dieser Befehl gibt die gleichen drei Zeichenfolgen, SourceVault, CertificateUrl und CertificateThumbprint, die verwendet wird, um eine sichere Linux Cluster zusammen mit den Speicherort zu erstellen, bei dem das selbst signierte Zertifikat platziert wurde. Sie benötigen das Verbindung mit dem Cluster selbst signierte Zertifikat.  Sie können mit dem sicheren Cluster über Anweisungen bei [Authentifizierung des Clientzugriffs auf einem Cluster](service-fabric-connect-to-secure-cluster.md)herstellen. Der Name des Zertifikats Betreff muss die Zugriff auf den Dienst Fabric Cluster verwendete Domäne übereinstimmen. Dies ist erforderlich, um SSL für der Clusters HTTPS Management Endpunkte und Dienst Fabric Explorer bereitzustellen. Sie können kein SSL-Zertifikat von einer Zertifizierungsstelle (CA) beziehen, für die `.cloudapp.azure.com` Domäne. Sie müssen einen benutzerdefinierten Domänennamen für Ihren Cluster erwerben. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern muss der Name des Zertifikats Betreff des benutzerdefinierten Domänennamens für Ihren Cluster verwendet übereinstimmen.

Die Parameter, die vom Skript Helper bereitgestellten können im Portal ausgefüllt werden, wie im Abschnitt [Erstellen eines Cluster im Portal Azure](service-fabric-cluster-creation-via-portal.md#create-cluster-portal)beschrieben.

## <a name="next-steps"></a>Nächste Schritte

An diesem Punkt müssen Sie einen sicheren Cluster mit Azure Active Directory-Authentifizierung bereitstellen Management. [Verbinden mit Ihren Cluster](service-fabric-connect-to-secure-cluster.md) weiter, und informieren Sie sich zum [Verwalten der Anwendung Kennwörter](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Behandeln von Problemen mit der Einrichtung Azure Active Directory für Client-Authentifizierung

Wenn Sie ein Problem während der Einrichtung von Azure Active Directory für die Authentifizierung auftreten, überprüfen Sie die folgenden Suggestings für mögliche Lösungen.

### <a name="service-fabric-explorer-prompts-for-selecting-certificate"></a>Dienst Fabric-Explorer fordert Sie auf Zertifikat auswählen

#### <a name="problem"></a>Problem

Nach der Anmeldung erfolgreich auf AAD Anmeldeseite im Dienst Fabric-Explorer im Browser zurück zur Startseite fordert jedoch ein Dialogfeld zum Auswählen eines Zertifikats.

![Dialogfeld für umfassende Zertifikat auswählen][sfx-select-certificate-dialog]

#### <a name="reason"></a>Grund

Der Benutzer wurde keine Rolle in AAD Clusteranwendung zugewiesen. Daher schlägt AAD Authentifizierung auf Dienst Fabric Cluster ein. Dienst Fabric Explorer greift auf Zertifikatauthentifizierung zurück.

#### <a name="solution"></a>Lösung

Führen Sie die Schritte zum Einrichten von AAD und weisen Sie Benutzerrollen zu. "Benutzer ZUORDNUNG erforderlich in ACCESS-APP" wird auch als eingeschaltet werden empfohlen `SetupApplications.ps1` unterstützt.

### <a name="connect-with-powershell-fails-with-error-the-specified-credentials-are-invalid"></a>Verbinden mit PowerShell fehlschlägt Fehler: die angegebenen Anmeldeinformationen sind ungültig

#### <a name="problem"></a>Problem

Wenn PowerShell Verbindung zum Cluster mithilfe von "AzureActiveDirectory" Sicherheit-Modus verwenden, nach Anmeldung erfolgreich auf der Anmeldeseite AAD datenbankverbindung Fehler: die angegebenen Anmeldeinformationen sind ungültig angezeigt.

#### <a name="solution"></a>Lösung

Dasselbe wie oben.

### <a name="service-fabric-explorer-signing-in-return-failure-aadsts50011"></a>Melden Sie sich im Gegenzug Fehler Dienst Fabric-Explorer: AADSTS50011

#### <a name="problem"></a>Problem

Nach der Anmeldung auf AAD Anmeldeseite im Dienst Fabric-Explorer, gibt Seite Sign in Fehler – AADSTS50011: die Antwortadresse &lt;Url&gt; die Antwort-Adressen, die so konfiguriert, dass für die Anwendung stimmen nicht überein: &lt;Guid&gt;. 

![Umfassende Antwortadresse nicht überein][sfx-reply-address-not-match]

#### <a name="reason"></a>Grund

Der cluster(web)-Anwendung, Dienst Fabric Explorer Versuche AAD authentifizieren darstellt, wird als Teil der Anforderung die Umleitung Rückgabe-URL. Aber es ist nicht in der Liste der AAD Anwendung "Antwort-URL" aufgeführt.

#### <a name="solution"></a>Lösung

Fügen Sie Url der Dienst Fabric Explorer hinzu-URL' Antworten' auf der Registerkarte Konfigurieren cluster(web) Anwendung oder Ersetzen Sie eines der Elemente in der Liste. Klicken Sie dann speichern.

![Web-Anwendung Antwort-url][web-application-reply-url]

### <a name="can-i-reuse-the-same-aad-tenant-for-multiple-clusters"></a>Kann ich den gleichen AAD Mandanten für mehrere Cluster wieder verwenden?

#### <a name="answer"></a>Antwort

Ja. Aber denken Sie daran, die URL der Dienst Fabric Explorer an Ihrer Anwendung cluster(web) hinzufügen, die andernfalls Dienst Fabric Explorer funktioniert.

### <a name="why-do-i-still-need-server-certificate-while-aad-enabled"></a>Warum weiterhin benötige Serverzertifikat ich während AAD aktiviert?

#### <a name="answer"></a>Antwort

Führen Sie FabricClient und FabricGateway gemeinsamen Authentifizierung. Bei AAD Authentifizierung bietet AAD Integration Clientidentität Server und Serverzertifikat verwendet wird, um die Serveridentität zu überprüfen. Weitere Informationen zur Funktionsweise von Zertifikat auf Dienst Fabric Überprüfen von [x. 509-Zertifikate und Fabric Service][x509-certificates-and-service-fabric]

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype-wad]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype-wad/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png