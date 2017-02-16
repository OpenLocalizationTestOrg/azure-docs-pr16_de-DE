<properties
    pageTitle="Verbinden Sie Ihre app mit Ihrem Netzwerk virtuelle mithilfe der PowerShell"
    description="Informationen zum Herstellen einer Verbindung mit von und Arbeiten mit virtuelle Netzwerke mithilfe der PowerShell konfigurieren"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="wpickett"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="ccompy"/>

# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a>Verbinden Sie Ihre app mit Ihrem Netzwerk virtuelle mithilfe der PowerShell #

## <a name="overview"></a>(Übersicht) ##

In der App-Verwaltungsdienst Azure können Sie Ihre app (Web, Mobil oder API) eine Azure-virtuellen Netzwerk (VNet) in Ihrem Abonnement verbinden. Dieses Feature heißt VNet Integration. Das Feature für die Integration von VNet sollte nicht mit dem Feature App-Service-Umgebung verwechselt werden, die Sie eine Instanz der App-Verwaltungsdienst Azure in Ihrem Netzwerk virtuelle ausführen können.

Das Feature für die Integration von VNet verfügt über eine Benutzeroberfläche (UI) in das neue Portal, das Sie verwenden können, mit virtuelle Netzwerke integriert werden soll, die mit klassischen Bereitstellungsmodell oder das Modell zur Bereitstellung von Azure Ressourcenmanager bereitgestellt werden. Wenn Sie weitere Informationen zu den Features erfahren möchten, finden Sie unter [Integrieren der app mit einem Azure virtuelle Netzwerk](web-sites-integrate-with-vnet.md).

In diesem Artikel wird nicht über die Verwendung der Benutzeroberfläche aber lieber dazu, wie Sie mithilfe der PowerShell Integration zu ermöglichen. Da die Befehle für jedes Bereitstellungsmodell unterscheiden, hat in diesem Artikel einen Abschnitt für jedes Modell zur Bereitstellung von.  

Bevor Sie in diesem Artikel fortzusetzen, stellen Sie sicher, dass Sie haben:

- Das aktuelle Azure PowerShell SDK installiert. Sie können dies mit der Web Platform Installer installieren.
- Eine app in Azure-App-Dienst in einem Standard oder Premium SKU ausgeführt.

## <a name="classic-virtual-networks"></a>Klassische virtuelle Netzwerke ##

In diesem Abschnitt wird erläutert, drei Aufgaben für virtuelle Netzwerke, die das Bereitstellungsmodell klassischen verwenden:

1. Verbinden Sie Ihre app zu einem bereits vorhandenen virtuellen Netzwerk, das einen Gateway sowie für Punkt-zu-Standort-Konnektivität konfiguriert ist.
1. Aktualisieren Sie Ihre virtuelle Netzwerk Integrationsinformationen für Ihre app ein.
1. Trennen Sie Ihre app von virtuellen Netzwerks ein.

### <a name="connect-an-app-to-a-classic-vnet"></a>Herstellen einer Verbindung eine klassische VNet mit einer app ###

Um einer app zu einem virtuellen Netzwerk herzustellen, führen Sie diese drei Schritte aus:

1. Deklarieren Sie zur Web-app, dass es ein bestimmtes virtuelles Netzwerk beitreten. Die app wird ein Zertifikat generieren, die mit dem virtuellen Netzwerk für Punkt-zu-Standort-Konnektivität gewährt wird.
1. Das Zertifikat des Web app an das virtuelle Netzwerk hochladen und dann das Punkt-zu-Standort VPN-Paket URI abrufen.
1. Aktualisieren der Web-app virtuelles Netzwerkverbindung mit dem Punkt-zu-Standort-Paket URI an.

Die erste und die dritte Schritte vollständig Skripts geeignet sind, aber im zweite Schritt erfordert eine einmalige, manuelle Aktion über das Portal oder Access **setzen** oder **PATCH** Aktionen am Endpunkt Azure Ressourcenmanager virtuelles Netzwerk ausführen. Wenden Sie sich an Azure Support Dies aktiviert haben. Bevor Sie beginnen, stellen Sie sicher, dass Sie ein klassisches virtuelles Netzwerk, das mit Punkt-zu-Standort-Konnektivität bereits aktiviert und einem bereitgestellten Gateway enthält. Um das Gateway zu erstellen und Punkt-zu-Standort-Konnektivität zu aktivieren, müssen Sie das Portal verwenden, wie bei der [Erstellung eines Gateways VPN]beschrieben[createvpngateway].

Das klassische virtuelle Netzwerk muss sich im selben Abonnement als Ihre App-Service-Plan, der die app befinden, die Sie in integrieren.

##### <a name="set-up-azure-powershell-sdk"></a>Einrichten von Azure PowerShell SDK #####

Öffnen Sie ein PowerShell-Fenster, und richten Sie Ihre Azure-Konto und das Abonnement mithilfe:

    Login-AzureRmAccount

Dieser Befehl wird eine Aufforderung zum Abrufen Ihrer Azure Anmeldeinformationen geöffnet. Nachdem Sie sich angemeldet haben, wenden Sie eine der folgenden Befehle das Abonnement auswählen, das Sie verwenden möchten. Stellen Sie sicher, dass Sie das Abonnement verwenden, die das virtuelle Netzwerk und die App-Serviceplan haben.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

oder

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>In diesem Artikel verwendeten Variablen #####

Um Befehle zu vereinfachen, werden wir eine **$Configuration** PowerShell-Variable mit bestimmten Konfiguration festlegen.

Setzen Sie eine Variable wie folgt in PowerShell mit den folgenden Parametern:

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

Die Position der app sollten die Position ohne Leerzeichen. Westen US beträgt beispielsweise Westus.

    $Configuration.WebAppLocation = "[Your web app Location]"

Das nächste Element ist, in dem das Zertifikat geschrieben werden sollen. Es sollte eine beschreibbare Pfad auf dem lokalen Computer sein. Vergewissern Sie sich am Ende CER aufnehmen möchten.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

Um anzuzeigen, was Sie festlegen, geben Sie **$Configuration**.

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

Im weiteren Verlauf dieses Abschnitts wird davon ausgegangen, dass Sie eine Variable, wie Sie soeben beschrieben erstellt haben.

##### <a name="declare-the-virtual-network-to-the-app"></a>Das virtuelle Netzwerk bei der app deklarieren #####

Verwenden Sie den folgenden Befehl aus, um der app feststellen, dass es dieses virtuelle Netzwerk verwendet wird. Dadurch wird die app erforderlichen Zertifikate generieren:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Wenn dieser Befehl erfolgreich ist, sollte **$vnet** eine Variable **Eigenschaften** enthalten. Die Variable **Eigenschaften** sollten sowohl den Fingerabdruck eines Zertifikats und das Zertifikatsdaten enthalten.

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a>Hochladen Sie das Zertifikat des Web app an das virtuelle Netzwerk #####

Ein Handbuch, ist die einmalige Schritt für jedes Abonnement und virtuelles Netzwerk Kombination erforderlich. D. h., wenn Sie apps im Abonnement A bis virtuelles Netzwerk A verbinden, müssen Sie diesen Schritt kann nur einmal unabhängig davon, wie viele apps, die Sie konfigurieren. Wenn Sie eine neue app zu einem anderen virtuellen Netzwerk hinzufügen möchten, müssen Sie die Aktion erneut. Der Grund dafür ist, dass eine Reihe von Zertifikaten auf ein Abonnementebene in der App-Verwaltungsdienst Azure generiert und festlegen wird einmal für jede virtuelle Netzwerk, dem die apps Verbindung generiert.

Die Zertifikate werden bereits festgelegt wurde oder wenn Sie diese Schritte, wenn Sie mit der gleichen virtuellen Netzwerk mithilfe des Portals integriert.

Dieser erste Schritt besteht die CER-Datei zu generieren. Im zweite Schritt ist die CER-Datei mit Ihrem Netzwerk virtuelle hochladen. Um die CER-Datei aus dem Anruf API im früheren Schritt zu generieren, führen Sie folgende Befehle aus.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

Das Zertifikat befindet sich in den Speicherort dieser **$Configuration.GeneratedCertificatePath** gibt.

Wenn Sie das Zertifikat manuell hochladen, verwenden Sie das [Azure-Portal] [ azureportal] und **(Klassische) durchsuchen virtuelles Netzwerk** > **VPN-Verbindungen** > **Punkt-zu-Standort** > **Zertifikate verwalten**. Laden Sie hier Ihr Zertifikat hoch.

##### <a name="get-the-point-to-site-package"></a>Abrufen des Punkt-zu-Standort-Pakets #####

Im nächsten Schritt beim Einrichten einer-Verbindungs auf eine Web app wird das Punkt-zu-Standort-Paket abrufen und Web app zur Verfügung stellen.

Speichern Sie die folgende Vorlage in einer Datei namens GetNetworkPackageUri.json an einer beliebigen Stelle auf dem Computer, z. B. C:\Azure\Templates\GetNetworkPackageUri.json ein.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


Festlegen der Eingabeparameter:

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

Rufen Sie das Skript an:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


Die Variable **$output. Outputs.packageUri** enthält jetzt das Paket URI Web app zugeordnet werden soll.

##### <a name="upload-the-point-to-site-package-to-your-app"></a>Hochladen des Punkt-zu-Standort-Pakets zu Ihrer Anwendung #####

Der letzte Schritt stellen die app mit diesem Paket bereit. Führen Sie einfach den nächsten Befehl ein:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Wenn eine Nachricht gefragt, zu bestätigen werden, dass Sie eine vorhandene Ressource überschrieben werden, stellen Sie sicher, um es zu ermöglichen.

Wenn dieser Befehl erfolgreich hergestellt wurde, sollte die app jetzt mit dem virtuellen Netzwerk verbunden. Zum Erfolg zu bestätigen, wechseln Sie zu der app-Verwaltungskonsole, und geben Sie Folgendes ein:

    SET WEBSITE_

Ist eine Umgebungsvariable mit dem Namen WEBSITE_VNETNAME, die einen Wert enthält, der den Namen des virtuellen Zielnetzwerks entspricht, wurden alle Konfigurationen erfolgreich abgeschlossen.

### <a name="update-classic-vnet-integration-information"></a>Klassische VNet Integrationsinformationen aktualisieren ###

Aktualisieren oder synchronisieren Sie Ihre Informationen neu, einfach, wiederholen Sie die Schritte aus, dass, wenn Sie die Integration im ersten Schritt erstellt haben. Diese Schritte sind:

1. Definieren Sie Ihre Konfigurationsinformationen ein.
1. Deklarieren Sie das virtuelle Netzwerk bei der app.
1. Abrufen des Punkt-zu-Standort-Pakets.
1. Hochladen des Punkt-zu-Standort-Pakets zu Ihrer Anwendung.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Trennen Sie die app aus einer klassischen VNet ###

Um die app zu trennen, benötigen Sie die Informationen, die während virtuelle Netzwerkintegration festgelegt wurde. Anhand dieser Informationen wird dann einen Befehl Ihre app von virtuellen Netzwerks trennen.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Ressourcenmanager virtuelle Netzwerke ##

Ressourcenmanager virtuelle Netzwerke haben Azure Ressourcenmanager APIs, die einige Prozesse klassischen virtuelle Netzwerke im Vergleich zu vereinfachen. Wir haben ein Skript, mit denen Sie die folgenden Aufgaben ausführen:

- Erstellen Sie ein Ressourcenmanager virtuelle Netzwerk und integrieren Sie Ihre app.
- Erstellen eines Gateways, Konfigurieren von Punkt-zu-Standort-Konnektivität in einem bereits vorhandenen Ressourcenmanager virtuelle Netzwerk, und klicken Sie dann Ihre app integrieren.
- Integrieren der app mit einem bereits vorhandenen Ressourcenmanager virtuelle Netzwerk, bei dem ein Gateway und Punkt-zu-Standort-Konnektivität aktiviert.
- Trennen Sie Ihre app von virtuellen Netzwerks ein.

### <a name="resource-manager-vnet-app-service-integration-script"></a>Ressourcenmanager VNet App Service Integration-Skript ###

Kopieren Sie das folgende Skript, und speichern Sie es in eine Datei. Wenn Sie nicht das Skript verwenden möchten, können Sie die Informationen aus, um herauszufinden, wie Elemente mit einem Ressourcenmanager virtuelle Netzwerk einrichten.


    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate to this VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with the following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create the virtual network and add it here. The way this works is:
        # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
        # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
        # 3) Get the VPN package from the gateway and pass it back to the App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association to VNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames

        # We need to check if this VNET is able to be joined to a App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have the right certificate, we will need to add it.
                # If it doesn't have a point-to-site range, we will need to add it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need to create one.
            Write-Host "This Virtual Network has no gateway. I will need to create one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with the following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need to create one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need to check if the certificate here exists in the gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting the VPN package and giving it to the App
        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
          else
        {
          Write-Host "Not connected to a VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound to this account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter the Resource Group of your App"

    $appName = Read-Host "Please enter the Name of your App"

    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

Speichern Sie eine Kopie des Skripts ein. In diesem Artikel heißt es V2VnetAllinOne.ps1, aber Sie können einen anderen Namen verwenden. Es sind keine Argumente für dieses Skript ein. Sie führen einfach Sie es. Erstes CD-das Skript ist aufgefordert werden, melden Sie sich an. Nachdem Sie sich angemeldet haben, wird das Skript ruft Details zu Ihrem Konto ab und gibt eine Liste der Abonnements. Ohne Berücksichtigung der Anfrage für Ihre Anmeldeinformationen ein, sieht die Ausführung des ersten Skripts wie folgt aus:

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) Demo-Abonnements (af5358e1-acac-2c90-a9eb-722190abf47a)
    2) MS-Test (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
    3) Lila Demo-Abonnements (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)

    Wählen Sie eine Option aus: 3

    Konto: ccompy@microsoft.com Umgebung: AzureCloud Abonnement: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d Mandanten: 722278f-fef1-499f-91ab-2323d011db47

    Geben Sie die Ressourcengruppe aus der App: Hcdemo-Rg Geben Sie den Namen der App: v2vnetpowershell Was möchten Sie tun?

    1) Hinzufügen eines neuen virtuellen Netzwerks zu einer App
    2) Hinzufügen eines vorhandenen virtuellen Netzwerks zu einer App
    3) Entfernen eines virtuellen Netzwerks aus einer App

Die restlichen in diesem Abschnitt werden alle diese drei Optionen beschrieben.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Erstellen Sie einer VNet Ressourcenmanager und mit integrieren ###

Wählen Sie zum Erstellen eines neuen virtuellen Netzwerks, das Ressourcenmanager Bereitstellung verwendet, und sie in der app integrieren, **1) Hinzufügen eines neuen virtuellen Netzwerks zu einer App**. Dies fordert Sie für den Namen des virtuellen Netzwerks. Wie Sie in der folgenden Einstellungen sehen können, verwendet ich in meinem Fall den Namen des v2pshell.

Das Skript bietet die Details zu dem virtuellen Netzwerk, das erstellt wird. Wenn ich möchte, kann ich einen beliebigen Wert ändern. In diesem Beispiel Ausführung erstellt habe ich ein virtuelles Netzwerk aus, das weist die folgenden Einstellungen:

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

Wenn Sie einen beliebigen Wert ändern möchten, geben Sie **Y** , und nehmen Sie die Änderungen vor. Wenn Sie mit den Einstellungen virtuelles Netzwerk zufrieden sind, geben Sie **N** ein, oder drücken Sie einfach die EINGABETASTE, wenn Sie dazu aufgefordert werden, zum Ändern der Einstellungen. Dort bis zum Abschluss des Skripts informiert Sie einige der welche It' zukünftig ausführen, bis er Erstellen des Gateways virtuelles Netzwerk beginnt. Dieser Schritt kann bis zu einer Stunde dauern. Es gibt keine Statusanzeige in dieser Phase, aber das Skript lassen Sie wissen, wann das Gateway erstellt wurde.

Wenn das Skript abgeschlossen ist, wird er **Fertig**angenommen. An diesem Punkt müssen Sie ein Ressourcenmanager virtuelles Netzwerk an, bei dem die Namen und die Einstellungen, die Sie ausgewählt haben. Dieses neue virtuelle Netzwerk wird auch in der app integriert werden.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Integrieren Sie Ihre app mit einem bereits vorhandenen Ressourcenmanager VNet ###

Wenn Sie, wenn Sie ein Ressourcenmanager virtuelles Netzwerk bereitstellen, das nicht über ein Gateway oder Punkt-zu-Standort-Konnektivität verfügen in einem bereits vorhandenen virtuellen Netzwerk integrieren sind, wird das Skript, eingerichtet. Wenn die VNET bereits Dinge eingerichtet hat, wechselt das Skript direkt auf die app-Integration. Um diese zu beginnen, klicken Sie einfach auf **2) hinzufügen ein VORHANDENES virtuelles Netzwerk zu einer App**.

Diese Option funktioniert nur, wenn Sie ein bereits vorhandener Ressourcenmanager virtuelles Netzwerk verfügen, das in der gleichen Abonnement als Ihre app ist. Nachdem Sie die Option auswählen, wird eine Liste Ihrer Ressourcenmanager virtuelle Netzwerke angezeigt werden.   

    Select a VNET to integrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Wählen Sie eine Option aus: 5

Wählen Sie einfach das virtuelle Netzwerk aus, die aus mit integriert werden soll. Wenn Sie bereits über ein Gateway, der Punkt-zu-Standort Connectivity aktiviert wurde verfügen, wird das Skript mit Ihrem Netzwerk virtuelle einfach Ihre app integrieren. Wenn Sie kein Gateway haben, müssen Sie das Gateway Subnetz angeben. Ihr Gateway Subnetz muss in Ihrem Netzwerk virtuelle Adresse Space, und es kann nicht in ein anderes Subnetz. Wenn Sie ein virtuelles Netzwerk ohne ein Gateway haben und diesen Schritt ausführen, können Sie Elemente wie folgt aussehen:

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

In diesem Beispiel erstellt habe ich eine virtuelle Netzwerk-Gateways, das weist die folgenden Einstellungen:

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association to VNET

Wenn Sie diese Einstellungen ändern möchten, können Sie dies tun. Andernfalls drücken Sie die EINGABETASTE, und das Skript Schlüsselaufgaben erstellen und Ihre app zu Ihrer virtuellen Netzwerk anfügen. Die Erstellungszeit Gateway ist jedoch weiterhin pro Stunde, daher sollten Sie sicherstellen, dass Sie berücksichtigen beibehalten. Wenn alles abgeschlossen ist, wird das Skript **erledigt**angenommen.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Trennen Sie die app aus einer VNet Ressourcenmanager ###

Trennen Ihre app aus dem virtuellen Netzwerk nicht Abschalten des Gateways oder Punkt-zu-Standort-Konnektivität zu deaktivieren. Sie möglicherweise, denn es nach etwas anderem verwendet. Es auch nicht es von anderen apps, die als dem getrennt, die Sie zur Verfügung gestellt. Wählen Sie zum Ausführen dieser Aktion **3) ein virtuelles Netzwerk aus einer App entfernen**. Wenn Sie dies tun, sehen Sie ungefähr wie folgt aus:

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Obwohl das Skript löschen angezeigt wird, wird es das virtuelle Netzwerk nicht gelöscht. Es werden nur die Integration entfernt. Bestätigen Sie, dass dies ist, was Sie tun möchten, der Befehl ganz schnell verarbeitet wird und Sie erfahren, **True** , wenn nicht mehr benötigt wird.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
