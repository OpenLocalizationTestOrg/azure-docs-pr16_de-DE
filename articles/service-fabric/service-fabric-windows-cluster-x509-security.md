<properties
   pageTitle="Herstellen einer Verbindung eine sichere private Cluster mit | Microsoft Azure"
   description="Dieser Artikel beschreibt, wie Kommunikation innerhalb der eigenständigen oder private Cluster sowie zwischen der Clients zu sichern."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/08/2016"
   ms.author="dkshir"/>

# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Sichern von einem eigenständigen Cluster mit x. 509-Zertifikate unter Windows

In diesem Artikel beschrieben, wie die Kommunikation zwischen den verschiedenen Knoten von Ihrem Windows-Cluster eigenständigen gesichert ebenfalls wie zum Authentifizieren Clients Herstellen einer Verbindung mit diesem Cluster mit x. 509-Zertifikaten. Dadurch wird sichergestellt, dass nur autorisierte Benutzer greifen auf den Cluster, die bereitgestellten Anwendungen und Verwaltungsaufgaben ausführen können.  Zertifikat Sicherheit sollte auf dem Cluster aktiviert werden, wenn der Cluster erstellt wird.  

Weitere Informationen zur Clustersicherheit von wie z. B.-Knoten Security Client-zu-Knoten Sicherheit und rollenbasierte Access-Steuerelement finden Sie unter [Cluster Sicherheitsszenarien](service-fabric-cluster-security.md).

## <a name="which-certificates-will-you-need"></a>Welche Zertifikate benötigen Sie?

So starten Sie mit einem Knoten in Ihrem Cluster [eigenständigen Cluster Paket herunterladen](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) . In der heruntergeladenen Pakets finden Sie eine Datei **ClusterConfig.X509.MultiMachine.json** . Öffnen Sie die Datei, und überprüfen Sie im Abschnitt **Sicherheit** , klicken Sie im Abschnitt **Eigenschaften** :

    "security": {
        "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        "CertificateInformation": {
            "ClusterCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ServerCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ClientCertificateThumbprints": [{
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            }, {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }],
            "ClientCertificateCommonNames": [{
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint" : "[Thumbprint]",
                "IsAdmin": true
            }]
            "HttpApplicationGatewayCertificate":{
                "Thumbprint": "[Thumbprint]",
                "X509StoreName": "My"
            }
        }
    }

In diesem Abschnitt werden die Zertifikate, die Sie zum Sichern Ihrer eigenständigen Windows Clusters benötigen. Legen die Werte von **ClusterCredentialType** und **ServerCredentialType** auf *X509*, um Zertifikaten basierende Sicherheit zu aktivieren.

>[AZURE.NOTE] Einen [Fingerabdruck](https://en.wikipedia.org/wiki/Public_key_fingerprint) ist die primäre Identität des ein Zertifikat. Lesen Sie [zum Abrufen des Fingerabdrucks eines Zertifikats](https://msdn.microsoft.com/library/ms734695.aspx) zu den Fingerabdruck der Zertifikate finden, die Sie erstellen.

Die folgende Tabelle listet die Zertifikate auf, denen Sie auf Ihr Cluster Setup müssen:

|**CertificateInformation-Einstellung**|**Beschreibung**|
|-----------------------|--------------------------|
|ClusterCertificate|Dieses Zertifikat ist zum Sichern der Kommunikation zwischen den Knoten in einem Cluster erforderlich. Sie können zwei verschiedene Zertifikate, einem primären und einem sekundären für Upgrade verwenden. Festlegen des Fingerabdrucks des primären Zertifikats im Abschnitt **Fingerabdruck** und mit der sekundäre **ThumbprintSecondary** Variablen an.|
|ServerCertificate|Beim Versuch, eine Verbindung mit diesem Cluster, wird dieses Zertifikat an den Client dargestellt. Zur Vereinfachung können Sie auswählen, das gleiche Zertifikat für *ClusterCertificate* und *ServerCertificate*verwenden. Sie können zwei verschiedene Serverzertifikate, einem primären und einem sekundären für Upgrade verwenden. Festlegen des Fingerabdrucks des primären Zertifikats im Abschnitt **Fingerabdruck** und mit der sekundäre **ThumbprintSecondary** Variablen an. |
|ClientCertificateThumbprints|Dies ist eine Reihe von Zertifikate, mit denen Sie auf den authentifizierten Clients installieren möchten. Sie können eine Anzahl von anderen Client-Zertifikate auf den Computern, die Sie Zugriff auf den Cluster zulassen möchten installiert haben. Legen Sie den Fingerabdruck des jedes Zertifikat in der Variablen **CertificateThumbprint** . Wenn Sie die **IsAdmin** *Wahr*, festlegen, und klicken Sie dann der Client mit diesem Zertifikat installiert ist Administrator Management Aktivitäten im Cluster ausführen kann. Wenn die **IsAdmin** *false*ist, kann der Client mit diesem Zertifikat nur die Aktionen für den Benutzerzugriff, in der Regel schreibgeschützt zulässige ausführen. Weitere Informationen zu Rollen finden Sie unter [rollenbasierte Zugriffssteuerung (RBAC)](service-fabric-cluster-security.md/#role-based-access-control-rbac)  |
|ClientCertificateCommonNames|Legen Sie den Anzeigenamen des ersten Client-Zertifikat für die **CertificateCommonName**ein. Die **CertificateIssuerThumbprint** ist der Fingerabdruck für den Herausgeber des Zertifikats. Lesen Sie [Arbeiten mit Zertifikaten](https://msdn.microsoft.com/library/ms731899.aspx) Allgemeine Namen und den Herausgeber mehr kennen.|
|HttpApplicationGatewayCertificate|Dies ist eine optionale Zertifikat, das werden kann angegeben werden, wenn Sie Ihre HTTP-Anwendungsgateway schützen möchten. Stellen Sie sicher, dass ReverseProxyEndpointPort in NodeTypes festgelegt ist, wenn Sie dieses Zertifikat verwenden.|

Hier Cluster-Beispielkonfiguration, in dem die Zertifikate Cluster, Client und Server erteilt wurde.

 ```
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
      "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
      "iPAddress": "10.7.0.6",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ServerCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpoint": "19001",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="aquire-the-x509-certificates"></a>Abgerufen werden x. 509 Zertifikaten
Um die Kommunikation innerhalb der Cluster zu sichern, müssen Sie zunächst x. 509-Zertifikate für Ihre Clusterknoten erhalten. Darüber hinaus, um eine Verbindung mit diesem Cluster autorisierten Computern/Benutzern beschränken möchten, müssen Sie erwerben und Installieren von Zertifikaten für die Clientcomputer.

Für Cluster, die Produktionsarbeitslasten ausgeführt werden, sollten Sie eine [Zertifizierungsstelle (Certificate Authority, CA)](https://en.wikipedia.org/wiki/Certificate_authority) signiert x 509-Zertifikat Cluster gesichert. Informationen dazu, wie diese Zertifikate hinzufügen, wechseln Sie zu [wie: Beziehen eines Zertifikats](http://msdn.microsoft.com/library/aa702761.aspx).

Für Cluster, die Sie Testzwecken verwenden, können Sie auswählen, ein selbst signiertes Zertifikat verwendet.

## <a name="optional-create-a-self-signed-certificate"></a>Optional: Erstellen eines selbstsignierten Zertifikats
Eine Möglichkeit, ein selbst signiertes Zertifikat erstellen, die ordnungsgemäß gesichert werden können besteht darin, das Skript *CertSetup.ps1* im Ordner im Verzeichnis *C:\Programme c:\Programme\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*Service Fabric SDK verwenden. Bearbeiten Sie diese Datei, und verwenden Sie diese Option zum Erstellen eines Zertifikats mit einen geeigneten Namen.

Jetzt exportieren Sie das Zertifikat in eine PFX-Datei mit einem Kennwort geschützt. Zuerst müssen Sie den Fingerabdruck des Zertifikats erhalten. Führen Sie die Anwendung certmgr.exe. Navigieren Sie zu dem Ordner **Lokale Computer\Personal** , und suchen Sie das Zertifikat, das Sie gerade erstellt haben. Doppelklicken Sie auf das Zertifikat, das sie öffnen, und wählen Sie die Registerkarte *Details* , und führen Sie einen Bildlauf nach unten bis zum Feld *Fingerabdruck* . Kopieren Sie den Fingerabdruckwert in der PowerShell-Befehl anhand der die Leerzeichen entfernen.  Ändern Sie den Wert *$pswd* in ein Eignung sicheres Kennwort zum schützen ihn, und führen die PowerShell:

```   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

Um die Details eines Zertifikats, das auf dem Computer installiert anzuzeigen, können Sie den folgenden PowerShell-Befehl ausführen:

```
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Wenn Sie ein Azure-Abonnement haben, führen Sie alternativ im Abschnitt [Hinzufügen von Zertifikaten Schlüssel Tresor](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).

## <a name="install-the-certificates"></a>Installieren der Zertifikate
Nachdem Sie die Zertifikate haben, können Sie diese auf die Cluster-Knoten installieren. Die Knoten müssen die neuesten Windows PowerShell 3.x installiert wurden. Sie müssen diese Schritte auf den einzelnen Knoten, für die sowohl Cluster-Server und alle sekundären Zertifikate wiederholen.

1. Kopieren Sie die Datei(en) PFX-Datei auf den Knoten.
2. Öffnen Sie ein PowerShell als Administrator aus, und geben Sie die folgenden Befehle. Ersetzen Sie die *$pswd* durch das Kennwort, das Sie zum Erstellen dieses Zertifikats verwendet. Ersetzen Sie die *$PfxFilePath* mit den vollständigen Pfad der PFX-Datei, die an diesen Knoten kopiert.

    ```
    $pswd = "1234"
    $PfcFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```

3. Als Nächstes müssen Sie das Steuerelement Zugriff auf diesem Zertifikat festlegen, dass Sie der Dienst Fabric-Prozess, der unter dem Konto Netzwerkdienst ausgeführt wird, durch Ausführen der folgenden Skripts genutzt werden kann. Geben Sie den Fingerabdruck des Zertifikats und "Netzwerkdienst" für das Dienstkonto ein. Sie können überprüfen, dass die ACLs für das Zertifikat mithilfe des Tools für certmgr.exe aus, und sehen sich die Privatschlüssel verwalten auf das Zertifikat korrekt sind.

    ```
    param
    (
        [Parameter(Position=1, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$pfxThumbPrint,

        [Parameter(Position=2, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$serviceAccount
        )

        $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; };

        # Specify the user, the permissions and the permission type
        $permission = "$($serviceAccount)","FullControl","Allow"
        $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission;

        # Location of the machine related keys
        $keyPath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\";
        $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName;
        $keyFullPath = $keyPath + $keyName;

        # Get the current acl of the private key
        $acl = (Get-Item $keyFullPath).GetAccessControl('Access')

        # Add the new ace to the acl of the private key
        $acl.SetAccessRule($accessRule);

        # Write back the new acl
        Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop

        #Observe the access rights currently assigned to this certificate.
        get-acl $keyFullPath| fl
        ```

4. Repeat the steps above for each server certificate. You can also use these steps to install the client certificates on the machines that you want to allow access to the cluster.

## Create the secure cluster
After configuring the **security** section of the **ClusterConfig.X509.MultiMachine.json** file, you can proceed to [Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section to configure the nodes and create the standalone cluster. Remember to use the **ClusterConfig.X509.MultiMachine.json** file while creating the cluster. For example, your command might look like the following:

```
.\CreateServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab - AcceptEULA $true
```

Once you have the secure standalone Windows cluster successfully running, and have setup the authenticated clients to connect to it, follow the section [Connect to a secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) to connect to it. For example:

```
Verbinden von ServiceFabricCluster - ConnectionEndpoint 10.7.0.4:19000 - KeepAliveIntervalInSec 10 - X509Credential - ServerCertThumbprint 057b9544a6f2733e0c8d3a60013a58948213f551 - FindType FindByThumbprint - FindValue 057b9544a6f2733e0c8d3a60013a58948213f551 - StoreLocation CurrentUser - StoreName Meine
```

If you are logged on to one of the machines in the cluster, since this already has the certificate installed locally you can simply run the Powershell command to connect to the cluster and show a list of nodes:

```
Verbinden ServiceFabricCluster Get-ServiceFabricNode
```
To remove the cluster call the following command:

```
.\RemoveServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab
```
