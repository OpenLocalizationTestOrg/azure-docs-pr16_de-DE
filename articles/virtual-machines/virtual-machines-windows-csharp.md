<properties
    pageTitle="Bereitstellen von Azure Ressourcen mit c# | Microsoft Azure"
    description="Sie lernen, wie c# und Azure Ressourcenmanager verwenden, um Microsoft Azure Ressourcen zu erstellen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="deploy-azure-resources-using-c"></a>Bereitstellen von Azure Ressourcen mit C# 

In diesem Artikel wird gezeigt, wie Azure Ressourcen mit c# erstellt werden können.

Zuerst müssen Sie sicherstellen, dass Sie die folgenden Aufgaben abgeschlossen haben:

- Installieren Sie [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Überprüfen Sie die Installation von [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) oder [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Abrufen einer [Authentifizierungstoken](../resource-group-authenticate-service-principal.md)

Es dauert etwa 30 Minuten, führen Sie folgende Schritte aus.

## <a name="step-1-create-a-visual-studio-project-and-install-the-libraries"></a>Schritt 1: Erstellen eines Projekts Visual Studio und installieren Sie die Bibliotheken

NuGet-Pakete sind die einfachste Möglichkeit, die Bibliotheken zu installieren, die Sie benötigen, um dieses Lernprogramms fertig zu stellen. Um die Bibliotheken zu gelangen, die Sie in Visual Studio benötigen, führen Sie folgende Schritte aus:

1. Klicken Sie auf **Datei** > **neue** > **Projekt**.

2. In **Vorlagen** > **C#-**, wählen Sie **Console-Anwendung**, geben Sie den Namen und Speicherort des Projekts, und klicken Sie dann auf **OK**.

3. Mit der rechten Maustaste im Explorer-Lösung des Projektnamens ein, und klicken Sie dann auf **NuGet-Pakete verwalten**.

4. Typ *Active Directory* in das Suchfeld ein, klicken Sie auf **Installieren** , für das Paket Active Directory-Authentifizierungsbibliothek, und folgen Sie dann die Anweisungen zur Installation des Pakets.

5. Wählen Sie am oberen Rand der Seite **Vorabversion enthalten**. Typ *Microsoft.Azure.Management.Compute* in das Suchfeld ein, für die Bibliotheken .NET zu berechnen, klicken Sie auf **Installieren** , und folgen Sie dann die Anweisungen, um das Paket zu installieren.

6. Typ *Microsoft.Azure.Management.Network* in das Suchfeld ein, klicken Sie auf **Installieren** , für die Netzwerk .NET Bibliotheken, und folgen Sie dann die Anweisungen, um das Paket zu installieren.

7. Typ *Microsoft.Azure.Management.Storage* in das Suchfeld ein, klicken Sie auf **Installieren** , für die Speicher .NET Bibliotheken, und folgen Sie dann die Anweisungen, um das Paket zu installieren.

8. Geben Sie *Microsoft.Azure.Management.ResourceManager* in das Suchfeld ein, klicken Sie für die Ressource Management Bibliotheken auf **Installieren** .

Jetzt sind Sie bereit sind, verwenden die Bibliotheken zum Erstellen einer Anwendung.

## <a name="step-2-create-the-credentials-that-are-used-to-authenticate-requests"></a>Schritt 2: Erstellen der Anmeldeinformationen, mit denen Anfragen authentifizieren

Sie können jetzt Anwendungsinformationen, dass Sie zuvor in Anmeldeinformationen erstellt, mit denen Anfragen zu Azure Ressourcenmanager authentifizieren formatieren.

1. Öffnen Sie die Datei Program.cs für das Projekt, das Sie erstellt haben, und fügen Sie diese Anweisungen an den Anfang der Datei verwenden:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Azure.Management.Storage;
        using Microsoft.Azure.Management.Storage.Models;
        using Microsoft.Azure.Management.Network;
        using Microsoft.Azure.Management.Network.Models;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;

2. Um das Token erstellen, das erforderlich ist, fügen Sie diese Methode der Programmklasse hinzu:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token");
          }
          return token;
        }

    Ersetzen Sie {Client-Id} mit dem Bezeichner des Azure Active Directory-Anwendung {Client-geheim} zusammen mit der Tastenkombination für die AD-Anwendung, und klicken Sie auf {Mandanten-Id} mit den Mandanten Bezeichner für Ihr Abonnement. Sie können die Mandanten-Id durch Ausführen der Get-AzureRmSubscription suchen. Sie können die Zugriffstaste mithilfe des Azure-Portals suchen.

3. Zum Aufrufen der Methode, die Sie zuvor hinzugefügt haben, fügen Sie diesen Code zur Methode Main in der Datei Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Speichern Sie die Datei Program.cs.

## <a name="step-3-register-the-resource-providers-and-create-the-resources"></a>Schritt 3: Registrieren für die Ressourcenanbieter und Erstellen von Ressourcen

### <a name="register-the-providers-and-create-a-resource-group"></a>Registrieren Sie der Provider, und erstellen Sie eine Ressourcengruppe

Alle Ressourcen müssen in einer Ressourcengruppe enthalten sein. Bevor Sie Ressourcen zu einer Gruppe hinzufügen können, muss Ihr Abonnement mit den Ressourcen Anbietern registriert sein.

1. Fügen Sie Variablen für die Main-Methode der Klasse Programm, um die Namen zu geben, die für die Ressourcen verwendet werden soll:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var location = "location name";
        var storageName = "storage account name";
        var ipName = "public ip name";
        var subnetName = "subnet name";
        var vnetName = "virtual network name";
        var nicName = "network interface name";
        var avSetName = "availability set name";
        var vmName = "virtual machine name";  
        var adminName = "administrator account name";
        var adminPassword = "administrator account password";
        
    Ersetzen Sie die Variablenwerten mit dem Namen und Bezeichner, die Sie verwenden möchten. Sie können die Abonnement-ID durch Ausführen der Get-AzureRmSubscription suchen.

2. Zum Erstellen der Ressourcengruppe und Registrieren der Anbieter diese Methode der Klasse Programm hinzufügen:

        public static async Task<ResourceGroup> CreateResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location)
        {
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
            
          Console.WriteLine("Registering the providers...");
          var rpResult = resourceManagementClient.Providers.Register("Microsoft.Storage");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Network");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Compute");
          Console.WriteLine(rpResult.RegistrationState);
          
          Console.WriteLine("Creating the resource group...");
          var resourceGroup = new ResourceGroup { Location = location };
          return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
        }

3. Zum Aufrufen der Methode, die Sie zuvor hinzugefügt haben, fügen Sie diesen Code zur Methode Main:

        var rgResult = CreateResourceGroupAsync(
          credential,
          groupName,
          subscriptionId,
          location);
        Console.WriteLine(rgResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

### <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto

Ein [Speicher-Konto](../storage/storage-create-storage-account.md) ist erforderlich, die virtuelle Festplatte erstellte Datei ist des virtuellen Computers gespeichert.

1. Um das Speicherkonto zu erstellen, fügen Sie diese Methode zur Klasse Programm aus:

        public static async Task<StorageAccount> CreateStorageAccountAsync(
          TokenCredentials credential,       
          string groupName,
          string subscriptionId,
          string location,
          string storageName)
        {
          Console.WriteLine("Creating the storage account...");
          var storageManagementClient = new StorageManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await storageManagementClient.StorageAccounts.CreateAsync(
            groupName,
            storageName,
            new StorageAccountCreateParameters()
            {
              Sku = new Microsoft.Azure.Management.Storage.Models.Sku() 
                { Name = SkuName.StandardLRS},
              Kind = Kind.Storage,
              Location = location
            }
          );
        }

2. Zum Aufrufen der Methode, die Sie zuvor hinzugefügt haben, fügen Sie diesen Code für die Main-Methode der Klasse Programm:

        var stResult = CreateStorageAccountAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          storageName);
        Console.WriteLine(stResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-public-ip-address"></a>Erstellen der öffentlichen IP-Adresse

Eine öffentliche IP-Adresse ist erforderlich, um die Kommunikation mit den virtuellen Computern.

1. Um die öffentliche IP-Adresse des virtuellen Computers zu erstellen, fügen Sie diese Methode zur Klasse Programm aus:

        public static async Task<PublicIPAddress> CreatePublicIPAddressAsync(
          TokenCredentials credential,  
          string groupName,
          string subscriptionId,
          string location,
          string ipName)
        {
          Console.WriteLine("Creating the public ip...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await networkManagementClient.PublicIPAddresses.CreateOrUpdateAsync(
            groupName,
            ipName,
            new PublicIPAddress
            {
              Location = location,
              PublicIPAllocationMethod = "Dynamic"
            }
          );
        }

2. Zum Aufrufen der Methode, die Sie zuvor hinzugefügt haben, fügen Sie diesen Code für die Main-Methode der Klasse Programm:

        var ipResult = CreatePublicIPAddressAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          ipName);
        Console.WriteLine(ipResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-virtual-network"></a>Erstellen von virtuellen Netzwerk

Einer virtuellen Computern, die mit dem Ressourcenmanager Bereitstellungsmodell erstellt wird, muss in einem virtuellen Netzwerk.

1. Um ein Subnetz und ein virtuelles Netzwerk zu erstellen, fügen Sie diese Methode zur Klasse Programm aus:

        public static async Task<VirtualNetwork> CreateVirtualNetworkAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string vnetName,
          string subnetName)
        {
          Console.WriteLine("Creating the virtual network...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          
          var subnet = new Subnet
          {
            Name = subnetName,
            AddressPrefix = "10.0.0.0/24"
          };
          
          var address = new AddressSpace {
            AddressPrefixes = new List<string> { "10.0.0.0/16" }
          };
          
          return await networkManagementClient.VirtualNetworks.CreateOrUpdateAsync(
            groupName,
            vnetName,
            new VirtualNetwork
            {
              Location = location,
              AddressSpace = address,
              Subnets = new List<Subnet> { subnet }
            }
          );
        }
        
2. Zum Aufrufen der Methode, die Sie zuvor hinzugefügt haben, fügen Sie diesen Code für die Main-Methode der Klasse Programm:

        var vnResult = CreateVirtualNetworkAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          vnetName,
          subnetName);
        Console.WriteLine(vnResult.Result.ProvisioningState);  
        Console.ReadLine();
        
### <a name="create-the-network-interface"></a>Erstellen der Schnittstelle

Eine virtuellen Computern benötigt eine Schnittstelle der virtuelle Netzwerk kommunizieren.

1. Um eine Netzwerk-Benutzeroberfläche zu erstellen, fügen Sie diese Methode der Klasse Programm aus:

        public static async Task<NetworkInterface> CreateNetworkInterfaceAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string subnetName,
          string vnetName,
          string ipName,
          string nicName)
        {
          Console.WriteLine("Creating the network interface...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var subnetResponse = await networkManagementClient.Subnets.GetAsync(
            groupName,
            vnetName,
            subnetName
          );
          var pubipResponse = await networkManagementClient.PublicIPAddresses.GetAsync(groupName, ipName);

          return await networkManagementClient.NetworkInterfaces.CreateOrUpdateAsync(
            groupName,
            nicName,
            new NetworkInterface
            {
              Location = location,
              IpConfigurations = new List<NetworkInterfaceIPConfiguration>
              {
                new NetworkInterfaceIPConfiguration
                {
                  Name = nicName,
                  PublicIPAddress = pubipResponse,
                  Subnet = subnetResponse
                }
              }
            }
          );
        }

2. Zum Aufrufen der Methode, die Sie zuvor hinzugefügt haben, fügen Sie diesen Code für die Main-Methode der Klasse Programm:

        var ncResult = CreateNetworkInterfaceAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          subnetName,
          vnetName,
          ipName,
          nicName);
        Console.WriteLine(ncResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-an-availability-set"></a>Erstellen einer Menge Verfügbarkeit

Verfügbarkeit Sätze erleichtern die Verwaltung von den virtuellen Computern, die von der Anwendung verwendeten verwalten.

1. Um die Verfügbarkeit Menge zu erstellen, fügen Sie diese Methode der Klasse Programm aus:

        public static async Task<AvailabilitySet> CreateAvailabilitySetAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string avsetName)
        {
          Console.WriteLine("Creating the availability set...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await computeManagementClient.AvailabilitySets.CreateOrUpdateAsync(
            groupName,
            avsetName,
            new AvailabilitySet()
            {
              Location = location
            }
          );
        }

2. Zum Aufrufen der Methode, die Sie zuvor hinzugefügt haben, fügen Sie diesen Code für die Main-Methode der Klasse Programm:

        var avResult = CreateAvailabilitySetAsync(
          credential,  
          groupName,
          subscriptionId,
          location,
          avSetName);
        Console.ReadLine();

### <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Jetzt, da Sie alle unterstützenden Ressourcen erstellt haben, können Sie einen virtuellen Computer erstellen.

1. Um des virtuellen Computers zu erstellen, fügen Sie diese Methode zur Klasse Programm aus:

        public static async Task<VirtualMachine> CreateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName,
          string subscriptionId,
          string location,
          string nicName,
          string avsetName,
          string storageName,
          string adminName,
          string adminPassword,
          string vmName)
        {
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var nic = networkManagementClient.NetworkInterfaces.Get(groupName, nicName);

          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          var avSet = computeManagementClient.AvailabilitySets.Get(groupName, avsetName);

          Console.WriteLine("Creating the virtual machine...");
          return await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(
            groupName,
            vmName,
            new VirtualMachine
            {
              Location = location,
              AvailabilitySet = new Microsoft.Azure.Management.Compute.Models.SubResource
              {
                Id = avSet.Id
              },
              HardwareProfile = new HardwareProfile
              {
                VmSize = "Standard_A0"
              },
              OsProfile = new OSProfile
              {
                AdminUsername = adminName,
                AdminPassword = adminPassword,
                ComputerName = vmName,
                WindowsConfiguration = new WindowsConfiguration
                {
                  ProvisionVMAgent = true
                }
              },
              NetworkProfile = new NetworkProfile
              {
                NetworkInterfaces = new List<NetworkInterfaceReference>
                {
                  new NetworkInterfaceReference { Id = nic.Id }
                }
              },
              StorageProfile = new StorageProfile
              {
                ImageReference = new ImageReference
                {
                  Publisher = "MicrosoftWindowsServer",
                  Offer = "WindowsServer",
                  Sku = "2012-R2-Datacenter",
                  Version = "latest"
                },
                OsDisk = new OSDisk
                {
                  Name = "mytestod1",
                  CreateOption = DiskCreateOptionTypes.FromImage,
                  Vhd = new VirtualHardDisk
                  {
                    Uri = "http://" + storageName + ".blob.core.windows.net/vhds/mytestod1.vhd"
                  }
                }
              }
            }
          );
        }

    >[AZURE.NOTE] In diesem Lernprogramm erstellt einen virtuellen Computer mit einer Version des Betriebssystems Windows Server. Weitere Informationen zum Auswählen von anderen Bildern finden Sie unter [Navigieren und select Azure-virtuellen Computern Bilder mit Windows PowerShell und Azure CLI](virtual-machines-linux-cli-ps-findimage.md).

2. Zum Aufrufen der Methode, die Sie zuvor hinzugefügt haben, fügen Sie diesen Code zur Methode Main:

        var vmResult = CreateVirtualMachineAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          nicName,
          avSetName,
          storageName,
          adminName,
          adminPassword,
          vmName);
        Console.WriteLine(vmResult.Result.ProvisioningState);
        Console.ReadLine();

##<a name="step-4-delete-the-resources"></a>Schritt 4: Löschen der Ressourcen

Da Ihnen für Ressourcen, die in Azure berechnet werden, ist es immer empfiehlt sich das Löschen von Ressourcen, die nicht mehr benötigt werden. Wenn Sie den virtuellen Computern und alle unterstützenden Ressourcen löschen möchten, müssen Sie nur, lediglich die Ressourcengruppe löschen.

1.  Zum Löschen der Ressourcengruppe fügen Sie diese Methode zur Programmklasse aus:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.  Zum Aufrufen der Methode, die Sie zuvor hinzugefügt haben, fügen Sie diesen Code zur Methode Main:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

## <a name="step-5-run-the-console-application"></a>Schritt 5: Führen Sie die Anwendung Konsole

1. Um die Console-Anwendung ausführen zu können, klicken Sie auf in Visual Studio **Starten** , und klicken Sie dann melden Sie sich bei Azure AD mit demselben Benutzernamen und Ihr Kennwort ein, die Sie mit Ihrem Abonnement verwenden.

2. Drücken Sie die **EINGABETASTE** , nachdem jeder Statuscode zurückgegeben wird, um jede Ressource zu erstellen. Nach der Erstellung des virtuellen Computers führen Sie im nächsten Schritt, bevor Sie die EINGABETASTE drücken, um alle Ressourcen zu löschen.

    Es sollte ungefähr fünf Minuten für diese Console-Anwendung vollständig vom Anfang ausführen dauern, bis z. Bevor Sie beginnen, Löschen von Ressourcen EINGABETASTE, könnten Sie einige Minuten dauern, zur Überprüfung der Erstellung der Ressourcen Azure-Portal aus, bevor Sie sie löschen.

3. Um den Status der Ressourcen anzuzeigen, wechseln Sie zu der Überwachungsprotokolle Azure-Portal:

    ![Durchsuchen von Überwachungsprotokollen Azure-Portal](./media/virtual-machines-windows-csharp/crpportal.png)
    
## <a name="next-steps"></a>Nächste Schritte

- Verwenden einer Vorlage zum Erstellen eines virtuellen Computers unter Verwendung der Informationen in [Bereitstellen einer Azure-virtuellen Computern mithilfe von c# und Ressourcenmanager Vorlage](virtual-machines-windows-csharp-template.md)nutzen.
- Erfahren Sie, wie des virtuellen Computers zu verwalten, die Sie erstellt haben, indem Sie [Verwalten virtuellen Computern Azure Ressourcenmanager und PowerShell verwenden](virtual-machines-windows-csharp-manage.md).
