<properties
   pageTitle="Clientzugriff auf einen Cluster authentifizieren | Microsoft Azure"
   description="Beschreibt, wie Clientzugriff auf einen Dienst Fabric Cluster mithilfe von Zertifikaten authentifiziert und zum Schutz der Kommunikation zwischen Clients und einem Cluster."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="connect-to-a-secure-cluster-without-aad"></a>Verbinden Sie mit einem sicheren Cluster ohne AAD
Ein Client zu einem Dienst Fabric Clusterknoten her, kann der Client authentifizierte und sichere Kommunikation mit Zertifikat Sicherheit hergestellt sein. Diese Authentifizierung wird sichergestellt, dass nur autorisierte Benutzer können auf den Cluster zugreifen und Anwendungen bereitgestellt und Verwaltungsaufgaben ausführen.  Zertifikat Sicherheit muss zuvor im Cluster aktiviert wurden, wenn der Cluster erstellt wurde.  Mindestens zwei Zertifikate sollte zum Sichern von Cluster, eine für das Zertifikat Cluster und Server und einen anderen für ClientAccess verwendet werden.  Es empfiehlt sich, dass Sie auch zusätzliche sekundäre und Client Access Zertifikate verwenden.  Weitere Informationen zu Cluster Sicherheitsszenarien finden Sie unter [Clustersicherheit](service-fabric-cluster-security.md).

Um die Kommunikation zwischen einem Client und einem Clusterknoten mit Zertifikat Sicherheit zu sichern, müssen Sie zuerst das Clientzertifikat erwerben und installieren. Das Zertifikat kann in den privaten (Mein) Speicher von dem lokalen Computer oder den aktuellen Benutzer installiert werden.  Sie benötigen außerdem den Fingerabdruck des Serverzertifikats, damit der Client Cluster authentifizieren kann.


Führen Sie das folgende PowerShell-Cmdlet das Client-Zertifikat auf dem Computer einrichten, in der Sie den Cluster zugreifen.

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
        -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

Wenn es sich um ein selbst signiertes Zertifikat ist, müssen Sie Ihres Computers "Vertrauenswürdige Personen" Store zu importieren, bevor Sie dieses Zertifikat in Verbindung mit einem sicheren Cluster verwenden können.

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
-FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
-Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```
<a id="connectsecureclustercli"></a> 
## <a name="connect-to-a-secure-cluster-using-azure-cli-without-aad"></a>Verbinden Sie mit einem sicheren Cluster Azure CLI ohne AAD verwenden

Herstellen einer Verbindung mit einem sicheren Cluster zu beschreiben die folgenden Azure CLI-Befehle. Die Zertifikatdetails müssen ein Zertifikat auf dem Clusterknoten übereinstimmen. 
 
Wenn Ihr Zertifikat Zertifizierungsstellen (Zertifizierungsstellen) vorhanden ist, müssen Sie den Parameter hinzufügen `--ca-cert-path` wie im folgenden Beispiel gezeigt: 

```
 azure servicefabric cluster connect --connection-endpoint https://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```
Wenn Sie mehrere Zertifizierungsstellen verfügen, verwenden Sie Kommas als Trennzeichen verwendet. 

 
Wenn der allgemeine Name im Zertifikat nicht den Verbindungsendpunkt übereinstimmt, könnten, verwenden Sie den Parameter `--strict-ssl-false` zum Umgehen der Überprüfung. 

```
azure servicefabric cluster connect --connection-endpoint https://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 --strict-ssl-false 
```
 
Wenn Sie die Überprüfung CA überspringen möchten, könnten Sie Hinzufügen der ``--reject-unauthorized-false`` Parameter, wie den folgenden Befehl aus:

```
azure servicefabric cluster connect --connection-endpoint https://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized-false 
```
 
Verwenden Sie zum Herstellen einer Verbindung zu einem Cluster mit ein selbst signiertes Zertifikat gesichert, entfernen die Überprüfung CA und die Überprüfung Allgemeine Namen folgenden Befehl ein:

```
azure servicefabric cluster connect --connection-endpoint https://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl-false --reject-unauthorized-false
```

Nachdem Sie eine Verbindung herstellen, sollten Sie andere CLI-Befehle für die Interaktion mit dem Cluster ausführen sein. 

<a id="connectsecurecluster"></a>
## <a name="connect-to-a-secure-cluster-using-powershell-without-aad"></a>Verbinden Sie mit einem sicheren Cluster mithilfe der PowerShell ohne AAD

Führen Sie den folgenden PowerShell-Befehl mit einem sicheren Cluster verbinden. Die Zertifikatdetails müssen ein Zertifikat auf dem Clusterknoten übereinstimmen.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

*ServerCertThumbprint* ist der Fingerabdruck des Serverzertifikats installiert ist, klicken Sie auf die Cluster-Knoten. *FindValue* ist der Fingerabdruck des Zertifikats Client Admin.
Wenn der Parameter ausgefüllt werden, sieht der Befehl wie im folgenden Beispiel: 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint A8136758F4AB8962AF2BF3F27921BE1DF67F4326 `
          -FindType FindByThumbprint -FindValue 71DE04467C9ED0544D021098BCD44C71E183414E `
          -StoreLocation CurrentUser -StoreName My
```




## <a name="connect-to-a-secure-cluster-using-the-fabricclient-apis"></a>Verbinden Sie mit einem sicheren Cluster mithilfe der FabricClient-APIs

Weitere Informationen zu FabricClient APIs finden Sie unter [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx). Die Knoten im Cluster müssen gültige Zertifikate, deren gemeinsamen Name oder DNS-Namen im SAN wird in der [Eigenschaft RemoteCommonNames](https://msdn.microsoft.com/library/azure/system.fabric.x509credentials.remotecommonnames.aspx) auf [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx)festlegen. Bei dieser Vorgehensweise ermöglicht gemeinsamen Authentifizierung zwischen dem Client und dem Clusterknoten.

```csharp
string clientCertThumb = "71DE04467C9ED0544D021098BCD44C71E183414E";
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string CommonName = "www.clustername.westus.azure.com";
string connection = "clustername.westus.cloudapp.azure.com:19000";

X509Credentials xc = GetCredentials(clientCertThumb, serverCertThumb, CommonName);
FabricClient fc = new FabricClient(xc, connection);
Task<bool> t = fc.PropertyManager.NameExistsAsync(new Uri("fabric:/any"));
try
{
    bool result = t.Result;
    Console.WriteLine("Cluster is connected");
}
catch (AggregateException ae)
{
    Console.WriteLine("Connect failed: {0}", ae.InnerException.Message);
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

...

static X509Credentials GetCredentials(string clientCertThumb, string serverCertThumb, string name)
{
    X509Credentials xc = new X509Credentials();

    // Client certificate
    xc.StoreLocation = StoreLocation.CurrentUser;
    xc.StoreName = "MY";
    xc.FindType = X509FindType.FindByThumbprint;
    xc.FindValue = thumb;

    // Server certificate
    xc.RemoteCertThumbprints.Add(thumb);
    xc.RemoteCommonNames.Add(name);

    xc.ProtectionLevel = ProtectionLevel.EncryptAndSign;
    return xc;
}
```


## <a name="next-steps"></a>Nächste Schritte

- [Dienst Fabric Cluster Upgradevorgang und von Ihnen erwartet](service-fabric-cluster-upgrade.md)
- [Verwalten Ihrer Fabric Service-Anwendung in Visual Studio](service-fabric-manage-application-in-visual-studio.md).
- [Fabric Dienststatus Modell Einleitung](service-fabric-health-introduction.md)
- [Sicherheit und RunAs](service-fabric-application-runas-security.md)
