## <a name="how-to-create-a-vnet-using-a-network-config-file-from-powershell"></a>So erstellen Sie eine VNet mit einer Network Config-Datei aus PowerShell

Azure verwendet eine XML-Datei, um alle verfügbaren VNets auf ein Abonnement definieren. Sie können diese Datei herunterladen und bearbeiten sie zum Ändern oder Löschen von vorhandenen VNets und neue erstellen. In diesem Dokument werden Sie erfahren, wie Sie diese Dateidownload genannt Dateien im Netzwerk Konfiguration (oder Netcgf), und bearbeiten, um eine neue VNet erstellen. Weitere Informationen zu den Netzwerk-Konfigurationsdatei der [Azure-virtuellen Netzwerk Konfigurationsschema](https://msdn.microsoft.com/library/azure/jj157100.aspx) zu überprüfen und.

Zum Erstellen einer VNet mithilfe einer Netcfg-Datei mithilfe der PowerShell führen Sie die folgenden Schritte aus.

1. Wenn Sie noch keine Erfahrung Azure PowerShell haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../articles/powershell-install-configure.md) , und folgen Sie den Anweisungen, ganz nach Ende melden Sie sich bei Azure, und wählen Sie Ihr Abonnement.
2. Verwenden Sie das Cmdlet " **Get-AzureVnetConfig** " der Azure-PowerShell-Konsole Konfigurationsdatei Netzwerk herunterladen, indem Sie den folgenden Befehl ausführen. 

        Get-AzureVNetConfig -ExportToFile c:\NetworkConfig.xml

    Erwartetes Ergebnis:

        XMLConfiguration                                                                                                     
        ----------------                                                                                                     
        <?xml version="1.0" encoding="utf-8"?>...  

3. Öffnen Sie die Datei, die Sie in Schritt 2 oben mit einem beliebigen XML- oder Text-Editor-Anwendung gespeichert haben, und suchen Sie nach der **<VirtualNetworkSites>** Element. Wenn Sie alle Netzwerke, die bereits erstellt haben, wird jedes Netzwerk angezeigt werden, als eigene **<VirtualNetworkSite>** Element.
4. Um das virtuelle Netzwerk beschrieben, die in diesem Szenario zu erstellen, fügen Sie die folgende XML-Daten direkt unter der **<VirtualNetworkSites>** Element:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

9.  Speichern Sie die Konfigurationsdatei Netzwerk.
10. Verwenden Sie das Cmdlet " **Set-AzureVnetConfig** " der Azure-PowerShell-Konsole Konfigurationsdatei Netzwerk hochladen, indem Sie den folgenden Befehl ausführen. Beachten Sie die Ausgabe unter dem Befehl verwenden, sollten Sie **erfolgreich** unter **OperationStatus**sehen. Wenn das heißt nicht der Fall, überprüfen Sie die XML-Datei auf Fehler.

        Set-AzureVNetConfig -ConfigurationPath c:\NetworkConfig.xml

    Hier ist die erwartete Ausgabe für den oben angegebenen Befehl aus:

        OperationDescription OperationId                          OperationStatus
        -------------------- -----------                          ---------------
        Set-AzureVNetConfig  49579cb9-3f49-07c3-ada2-7abd0e28c4e4 Succeeded 
    
11. Verwenden Sie das Cmdlet " **Get-AzureVnetSite** " der Azure-PowerShell-Konsole zu überprüfen, ob der neuen Website hinzugefügt wurde, indem Sie den folgenden Befehl ausführen. 

        Get-AzureVNetSite -VNetName TestVNet

    Hier ist die erwartete Ausgabe für den oben angegebenen Befehl aus:

        AddressSpacePrefixes : {192.168.0.0/16}
        Location             : Central US
        AffinityGroup        : 
        DnsServers           : {}
        GatewayProfile       : 
        GatewaySites         : 
        Id                   : b953f47b-fad9-4075-8cfe-73ff9c98278f
        InUse                : False
        Label                : 
        Name                 : TestVNet
        State                : Created
        Subnets              : {FrontEnd, BackEnd}
        OperationDescription : Get-AzureVNetSite
        OperationId          : 3f35d533-1f38-09c0-b286-3d07cd0904d8
        OperationStatus      : Succeeded