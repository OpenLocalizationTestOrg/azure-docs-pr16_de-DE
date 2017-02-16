<properties
   pageTitle="Hinzufügen, Rollover und Entfernen von Zertifikaten in einem Dienst Fabric Cluster in Azure verwendet | Microsoft Azure"
   description="Beschreibt, wie eine sekundäre Cluster Zertifikat und dann auf Rollover das alte primären Zertifikat hochladen."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="chackdan"/>

# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a>Hinzufügen oder Entfernen von Zertifikaten für einen Dienst Fabric Cluster in Azure

Es wird empfohlen, dass Sie Vertrautmachen mit wie Dienst Fabric x. 509-Zertifikate, lesen Sie [Cluster Sicherheitsszenarios](service-fabric-cluster-security.md)verwendet. Sie müssen wissen, welche ein Cluster Zertifikat ist und was, bevor Sie weiteren fortfahren verwendet wird.

Dienst Fabric ermöglicht die Angabe zwei Cluster Zertifikate, einem primären und einem sekundären, wenn Sie während der Clustererstellung Zertifikat Sicherheit konfigurieren. Finden Sie unter [Erstellen eines Azure Clusters über Azure Ressourcenmanager](service-fabric-cluster-creation-via-arm.md) , Details oder [einen Azure Cluster über Portal erstellen](service-fabric-cluster-creation-via-portal.md) . Wenn Sie nur ein Zertifikat für Cluster Bereitstellen über Ressourcenmanager, und Sie angeben möchten, wird, die als primäre Zertifikat verwendet. Nach der Clustererstellung können Sie ein neues Zertifikat als sekundärer hinzufügen.

>[AZURE.NOTE] Bei einem sicheren Cluster benötigen Sie immer mindestens ein gültiges (nicht gesperrtes und nicht abgelaufenes) Zertifikat (primäre oder sekundäre) bereitgestellt ist dies nicht der Cluster funktioniert nicht mehr. 90 Tage bevor alle gültigen Zertifikate Ablauf, erreichen generiert das System eine Warnung Spur und auch eine Warnung Gesundheit Ereignis auf den Knoten. Es gibt es zurzeit keine e-Mail oder einer beliebigen anderen Benachrichtigung, die versendet Dienst Fabric zu diesem Thema. 


## <a name="add-a-secondary-certificate-using-the-portal"></a>Hinzufügen einer sekundären Zertifikat verwenden des Portals
Wenn Sie ein anderes Zertifikat als sekundärer hinzufügen möchten, müssen Sie das Zertifikat auf einer Azure Key Tresor hochladen und klicken Sie dann mit den virtuellen Computern im Cluster bereitstellen. Weitere Informationen finden Sie unter [Bereitstellen von Zertifikaten virtuellen Computern aus einem Kunden verwaltete Key Tresor](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx).

1. Zum [Hinzufügen von Zertifikaten für Schlüssel Tresor](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault) verweisen Sie, zum.

2. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com/) an, und suchen Sie die Ressource, der Sie dieses Zertifikat zum hinzufügen möchten.
3. Klicken Sie unter **EINSTELLUNGEN**klicken Sie auf **Sicherheit** , um das Cluster Sicherheit Blade anzuzeigen.
4. Klicken Sie auf das **"+ Zertifikat"** die Schaltfläche auf dem Blade, um das Blade **"Zertifikat hinzufügen"** zu gelangen.
5. Wählen Sie "Fingerabdruck sekundäre Zertifikats" aus der Dropdownliste aus, und füllen Sie den Fingerabdruck des Zertifikats des sekundären Zertifikats, das Sie in die Keyvault hochgeladen.

>[AZURE.NOTE]
Im Gegensatz zu während der Erstellung des Workflows Cluster berücksichtigt wir nicht in die Keyvault Informationen, die Details, da es wird davon ausgegangen, dass die Zeit, die Sie auf diese Blade befinden, indem Sie Sie mit den virtuellen Computern bereits das Zertifikat bereitgestellt haben und das Zertifikat bereits in der lokalen Zertifikat Store in der VMSS Instanz zur Verfügung ist.

Klicken Sie auf **Zertifikat**. Eine Bereitstellung gestartet wird, und auf das Cluster Sicherheit Blade blaue Statusleiste angezeigt wird.

![Screenshot der Zertifikatfingerabdruck im Portal][SecurityConfigurations_02]

Und erfolgreichen Abschluss dieser Bereitstellung, werden Sie entweder zur primären oder sekundären Zertifikat Management Operationen auf dem Cluster verwenden.

![Screenshot des Zertifikats Bereitstellung wird ausgeführt][SecurityConfigurations_03]

Hier ist ein Screenshot auf das Aussehen des Sicherheit Blades nach Abschluss die Bereitstellung.

![Screenshot der Zertifikatfingerabdruck nach der Bereitstellung][SecurityConfigurations_08]


Sie können jetzt verwenden Sie das neue Zertifikat, die Sie gerade hinzugefügt haben, um eine Verbindung herstellen, und führen Sie Vorgänge auf dem Cluster.

>[AZURE.NOTE]
Derzeit gibt es keine Möglichkeit zum Austauschen des primären und sekundären Zertifikate auf dem Portal dieses Feature ist in Works. Solange eine gültige Cluster Zertifikate vorhanden sind, wird der Cluster fein ausgeführt werden.

## <a name="add-a-secondary-certificate-and-swap-it-to-be-the-primary-using-resource-manager-powershell"></a>Fügen Sie ein Zertifikat einer sekundären hinzu und tauschen Sie aus, um die primäre mithilfe der Powershell Ressourcenmanager werden

Diesen Schritten wird vorausgesetzt, dass Sie mit der Funktionsweise der Ressourcenmanager vertraut sind und mindestens einen Dienst Fabric Cluster mithilfe einer Vorlage Ressourcenmanager bereitgestellt haben, und die Vorlage, die Sie mit einer der Cluster praktischen eingerichtet haben. Außerdem wird vorausgesetzt, dass Sie mit JSON vertraut sind.

>[AZURE.NOTE]
Wenn Sie eine Beispielvorlage und Parameter, die Sie verwenden können, denen Sie folgen, entlang oder als Ausgangspunkt suchen, können Sie ihn dann aus dieser [Git-Repo]. (https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample). 

#### <a name="edit-your-resource-manager-template"></a>Bearbeiten einer Vorlage Ressourcenmanager 

Wenn Sie das Beispiel aus der [Git-Repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) , denen Sie folgen entlang verwenden können wurden, finden Sie diese Änderungen in der Stichprobe 5-virtueller Computer-1-NodeTypes-Secure_Step2.JSON. Mithilfe von 5-virtueller Computer-1-NodeTypes-Secure_Step1.JSON einen sicheren Cluster bereitstellen

1. Öffnen Sie die Vorlage Ressourcenmanager verwendet Sie Sie Cluster bereitstellen.
2. Fügen Sie einen neuen Parameter "SecCertificateThumbprint" vom Typ "String". Ressourcenmanager Vorlage, die Sie während der Erstellungszeit oder aus den Schnellstart Vorlagen aus dem Portal heruntergeladen einsetzen, und nur für diesen Parameter suchen, sollten Sie es bereits definiert suchen.  
3. Suchen Sie die Definition der Ressource "Microsoft.ServiceFabric/clusters" aus. Finden Sie unter Eigenschaften "Zertifikat" JSON-Tag, das wie den folgenden JSON-Codeausschnitt aussehen sollte.
```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
        }
``` 

4. Hinzufügen einer neuen Kategorie "ThumbprintSecondary", und geben sie einen Wert "[parameters('secCertificateThumbprint')]".  

In diesem Fall jetzt die Definition der Ressource wie folgt aussehen sollte (je nach Quelle der Vorlage, es möglicherweise nicht genau wie den folgenden Codeausschnitt). Wie Sie, unter sehen können ist so vorgehen ein neues Zertifikat als Primärschlüssel angeben und Verschieben des aktuellen primären als sekundären.  Das Ergebnis der Rollover des aktuellen Zertifikats in das neue Zertifikat in einem Bereitstellungsschritt.

```JSON

      "properties": {
        "certificate": {
            "thumbprint": "[parameters('certificateThumbprint')]",
            "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
            "x509StoreName": "[parameters('certificateStoreValue')]"
        },

```

#### <a name="edit-your-template-file-to-reflect-the-new-parameters-you-added-above"></a>Bearbeiten Sie Ihre Vorlagendatei entsprechend hinzugefügte über die neuen Parameter

Wenn Sie das Beispiel aus der [Git-Repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) entlang folgen verwendet haben, können Sie beginnen, nehmen Sie Änderungen in der Stichprobe 5-virtueller Computer-1-NodeTypes-Secure.paramters_Step2.JSON 


Bearbeiten Sie den Ressourcenmanager Vorlagenparameter Datei, fügen Sie die neue Parameter für das SecCertificate und tauschen Sie die vorhandenen primären Zertifikat Details mit der sekundären aus, und Ersetzen Sie die Details des primären Zertifikat mit den neuen Zertifikat Details. 

```JSON
    "secCertificateThumbprint": {
      "value": "OLD Primary Certificate Thumbprint"
    },
   "secSourceVaultValue": {
      "value": "OLD Primary Certificate Key Vault location"
    },
    "secCertificateUrlValue": {
      "value": "OLD Primary Certificate location in the key vault"
     },
    "certificateThumbprint": {
      "value": "New Certificate Thumbprint"
    },
    "sourceVaultValue": {
      "value": "New Certificate Key Vault location"
    },
    "certificateUrlValue": {
      "value": "New Certificate location in the key vault"
     },

```

### <a name="deploy-the-template-to-azure"></a>Stellen Sie die Vorlage in Azure bereit.

1. Sie können nun Ihre Vorlage in Azure bereitstellen. Öffnen einer Azure PS Version 1 + Eingabeaufforderungsfenster.
2. Melden Sie sich an Ihre Azure-Konto, und wählen Sie das Abonnement für bestimmte Azure. Dies ist ein wichtiger Schritt für Personen, die Zugriff auf mehrere Azure-Abonnement verfügen.


```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId <Subcription ID> 

```

Testen der Vorlage vor der Bereitstellung. Verwenden der gleichen Ressourcengruppe, die Ihre Cluster derzeit in bereitgestellt wird.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

Bereitstellen Sie die Vorlage für Ihre Ressourcengruppe. Verwenden der gleichen Ressourcengruppe, die Ihre Cluster derzeit in bereitgestellt wird. Ausführen des Befehls New-AzureRmResourceGroupDeployment Sie müssen nicht im Modus angegeben werden, da der Standardwert **inkrementell**ist.

>[AZURE.NOTE]
Wenn Sie den Modus zu abgeschlossen festlegen, können Sie versehentlich Ressourcen löschen, die nicht in der Vorlage enthalten sind. Damit Sie nicht in diesem Szenario verwenden.
   

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

Hier ist ein Beispiel für die gleichen Powershell ausgefüllt.

```powershell
$ResouceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzureRmResourceGroupDeployment -ResourceGroupName $ResouceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResouceGroup2

```

Nachdem die Bereitstellung abgeschlossen ist, Herstellen einer Verbindung mit dem neuen Zertifikat Cluster mit, und führen Sie einige Abfragen. Wenn Sie tun können. Dann können Sie das alte primäre Zertifikat löschen. 

Wenn Sie ein selbst signiertes Zertifikat verwenden, vergessen Sie nicht in Ihrer lokalen TrustedPeople Zertifikatspeicher importieren.

```powershell
######## Set up the certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
Eine Kurzübersicht hier ist der Befehl für die Verbindung zu einem sicheren cluster 
```powershell
$ClusterName= "chackosecure5.westus.cloudapp.azure.com:19000"
$CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7SD1D630F8F3" 

Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $CertThumbprint  `
    -FindType FindByThumbprint `
    -FindValue $CertThumbprint `
    -StoreLocation CurrentUser `
    -StoreName My
```
Eine Kurzübersicht hier ist der Befehl Cluster Systemzustand abgerufen.
```powershell
Get-ServiceFabricClusterHealth 
```
 
## <a name="remove-the-old-certificate-using-the-portal"></a>Entfernen Sie das alte Zertifikat verwenden des Portals
Hier umfasst eine alte Zertifikat entfernen, sodass der Cluster nicht verwendet wird:

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com/) an, und navigieren Sie zu Sicherheitseinstellungen im Cluster.
2. Rechtsklick auf das Zertifikat, das Sie entfernen möchten.
3. Wählen Sie löschen aus, und folgen Sie den Anweisungen. 

[SecurityConfigurations_05]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_05.png


## <a name="next-steps"></a>Nächste Schritte
Lesen Sie diesen Artikel für Weitere Informationen zu Cluster Management:

- [Dienst Fabric Cluster Upgradevorgang und von Ihnen erwartet](service-fabric-cluster-upgrade.md)
- [Rollenbasierte Zugriff für Clients für die Einrichtung](service-fabric-cluster-security-roles.md)


<!--Image references-->
[SecurityConfigurations_02]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_02.png
[SecurityConfigurations_03]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_03.png
[SecurityConfigurations_05]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_05.png
[SecurityConfigurations_08]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_08.png
