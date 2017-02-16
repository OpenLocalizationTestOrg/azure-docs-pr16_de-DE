<properties
    pageTitle="Verwalten von virtuellen Computern mit Azure Ressourcenmanager und c# | Microsoft Azure"
    description="Verwalten von virtuellen Computern mit Azure Ressourcenmanager und c#."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-azure-resource-manager-and-c"></a>Verwalten von Azure virtuellen Computern mit Azure Ressourcenmanager und C#  

Die Aufgaben in diesem Artikel wird gezeigt, wie virtuellen Computern, z. B. starten, beenden und aktualisieren verwalten. Ein virtueller Computer muss in einer Ressourcengruppe zum Abschließen der Vorgänge in diesem Artikel vorhanden sein.

Wenn Sie die Aufgaben in diesem Artikel ausführen zu können, müssen Sie folgende Aktionen ausführen:

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- [Ein Authentifizierungstoken](../resource-group-authenticate-service-principal.md)

## <a name="create-a-visual-studio-project-and-install-packages"></a>Erstellen eines Projekts Visual Studio und Pakete installieren

NuGet-Pakete sind am einfachsten, die Bibliotheken, die Sie zum Abschließen der Vorgänge in diesem Artikel müssen zu installieren. Die Bibliotheken, die Sie für diesen Artikel installieren sind Azure Active Directory-Authentifizierung Library und die berechnen Anbieter Ressourcenbibliothek. Führen Sie diese Schritte, um die Bibliotheken in Visual Studio zu erhalten:

1. Klicken Sie auf **Datei** > **neue** > **Projekt**.

2. In **Vorlagen** > **C#-**, wählen Sie **Console-Anwendung**, geben Sie den Namen und Speicherort des Projekts, und klicken Sie dann auf **OK**.

3. Mit der rechten Maustaste im Explorer-Lösung des Projektnamens ein, und klicken Sie dann auf **NuGet-Pakete verwalten**.

4. Typ *Active Directory* in das Suchfeld ein, klicken Sie auf **Installieren** , für das Paket Active Directory-Authentifizierungsbibliothek, und folgen Sie dann die Anweisungen zur Installation des Pakets.

5. Wählen Sie am oberen Rand der Seite **Vorabversion enthalten**. Typ *Microsoft.Azure.Management.Compute* in das Suchfeld ein, für die Bibliotheken .NET zu berechnen, klicken Sie auf **Installieren** , und folgen Sie dann die Anweisungen, um das Paket zu installieren.

Sie können nun die Bibliotheken zum Verwalten Ihrer virtuellen Computer verwenden.

## <a name="set-up-the-project"></a>Einrichten des Projekts

Nachdem Sie nun die Anwendung wird erstellt, und die Bibliotheken installiert sind, erstellen Sie ein Token mithilfe der Anwendungsinformationen. Dieses Token wird zur Anfragen zu Azure Ressourcenmanager Authentifizierung verwendet.

1. Öffnen Sie die Datei Program.cs für das Projekt, das Sie erstellt haben, und fügen Sie diese Anweisungen an den Anfang der Datei verwenden:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;
        
2. Hinzufügen von Variablen für die Main-Methode der Klasse Programm, um den Namen der Ressourcengruppe und den Namen des virtuellen Computers und Ihre Abonnement-ID angeben:

        var groupName = "resource group name";
        var vmName = "virtual machine name";  
        var subscriptionId = "subsciption id";

    Sie können die Abonnement-ID durch Ausführen der Get-AzureRmSubscription suchen.
    
3. Das Token abrufen, das die Anmeldeinformationen erstellen, fügen Sie diese Methode zur Klasse Programm erforderlich ist:

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
    
4. Um die Anmeldeinformationen zu erstellen, fügen Sie diesen Code zur Methode Main in Program.cs ein:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

5. Speichern Sie die Datei Program.cs.

## <a name="display-information-about-a-virtual-machine"></a>Anzeigen von Informationen zu einem virtuellen Computern

1. Fügen Sie diese Methode zur Klasse Programm in das Projekt, das Sie zuvor erstellt haben:

        public static async void GetVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Getting information about the virtual machine...");

          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(
            groupName, 
            vmName, 
            InstanceViewTypes.InstanceView);

          Console.WriteLine("hardwareProfile");
          Console.WriteLine("   vmSize: " + vmResult.HardwareProfile.VmSize);

          Console.WriteLine("\nstorageProfile");
          Console.WriteLine("  imageReference");
          Console.WriteLine("    publisher: " + vmResult.StorageProfile.ImageReference.Publisher);
          Console.WriteLine("    offer: " + vmResult.StorageProfile.ImageReference.Offer);
          Console.WriteLine("    sku: " + vmResult.StorageProfile.ImageReference.Sku);
          Console.WriteLine("    version: " + vmResult.StorageProfile.ImageReference.Version);
          Console.WriteLine("  osDisk");
          Console.WriteLine("    osType: " + vmResult.StorageProfile.OsDisk.OsType);
          Console.WriteLine("    name: " + vmResult.StorageProfile.OsDisk.Name);
          Console.WriteLine("    createOption: " + vmResult.StorageProfile.OsDisk.CreateOption);
          Console.WriteLine("    uri: " + vmResult.StorageProfile.OsDisk.Vhd.Uri);
          Console.WriteLine("    caching: " + vmResult.StorageProfile.OsDisk.Caching);

          Console.WriteLine("\nosProfile");
          Console.WriteLine("  computerName: " + vmResult.OsProfile.ComputerName);
          Console.WriteLine("  adminUsername: " + vmResult.OsProfile.AdminUsername);
          Console.WriteLine("  provisionVMAgent: " + vmResult.OsProfile.WindowsConfiguration.ProvisionVMAgent.Value);
          Console.WriteLine("  enableAutomaticUpdates: " + vmResult.OsProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);

          Console.WriteLine("\nnetworkProfile");
          foreach (NetworkInterfaceReference nic in vmResult.NetworkProfile.NetworkInterfaces)
          {
            Console.WriteLine("  networkInterface id: " + nic.Id);
          }

          Console.WriteLine("\nvmAgent");
          Console.WriteLine("  vmAgentVersion" + vmResult.InstanceView.VmAgent.VmAgentVersion);
          Console.WriteLine("    statuses");
          foreach (InstanceViewStatus stat in vmResult.InstanceView.VmAgent.Statuses)
          {
            Console.WriteLine("    code: " + stat.Code);
            Console.WriteLine("    level: " + stat.Level);
            Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
            Console.WriteLine("    message: " + stat.Message);
            Console.WriteLine("    time: " + stat.Time);
          }

          Console.WriteLine("\ndisks");
          foreach (DiskInstanceView idisk in vmResult.InstanceView.Disks)
          {
            Console.WriteLine("  name: " + idisk.Name);
            Console.WriteLine("  statuses");
            foreach (InstanceViewStatus istat in idisk.Statuses)
            {
              Console.WriteLine("    code: " + istat.Code);
              Console.WriteLine("    level: " + istat.Level);
              Console.WriteLine("    displayStatus: " + istat.DisplayStatus);
              Console.WriteLine("    time: " + istat.Time);
            }
          }

          Console.WriteLine("\nVM general status");
          Console.WriteLine("  provisioningStatus: " + vmResult.ProvisioningState);
          Console.WriteLine("  id: " + vmResult.Id);
          Console.WriteLine("  name: " + vmResult.Name);
          Console.WriteLine("  type: " + vmResult.Type);
          Console.WriteLine("  location: " + vmResult.Location);
          Console.WriteLine("\nVM instance status");
          foreach (InstanceViewStatus istat in vmResult.InstanceView.Statuses)
          {
            Console.WriteLine("\n  code: " + istat.Code);
            Console.WriteLine("  level: " + istat.Level);
            Console.WriteLine("  displayStatus: " + istat.DisplayStatus);
          }
          
        }

2. Zum Aufrufen der Methode, die Sie soeben hinzugefügt haben, fügen Sie diesen Code zur Methode Main:

        GetVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
    
3. Speichern Sie die Datei Program.cs.

4. Klicken Sie auf in Visual Studio **Starten** , und klicken Sie dann melden Sie sich bei Azure AD mit demselben Benutzernamen und Ihr Kennwort ein, die Sie mit Ihrem Abonnement verwenden.

    Wenn Sie diese Methode ausführen, sollte nun wie in diesem Beispiel angezeigt werden:
    
        Getting information about the virtual machine...
        hardwareProfile
          vmSize: Standard_A0

        storageProfile
          imageReference
            publisher: MicrosoftWindowsServer
            offer: WindowsServer
            sku: 2012-R2-Datacenter
            version: latest
          osDisk
            osType: Windows
            name: myosdisk
            createOption: FromImage
            uri: http://store1.blob.core.windows.net/vhds/myosdisk.vhd
            caching: ReadWrite

          osProfile
            computerName: vm1
            adminUsername: account1
            provisionVMAgent: True
            enableAutomaticUpdates: True

          networkProfile
            networkInterface 
              id: /subscriptions/{subscription-id}
                /resourceGroups/rg1/providers/Microsoft.Network/networkInterfaces/nc1

          vmAgent
            vmAgentVersion2.7.1198.766
            statuses
            code: ProvisioningState/succeeded
            level: Info
            displayStatus: Ready
            message: GuestAgent is running and accepting new configurations.
            time: 4/13/2016 8:35:32 PM

          disks
            name: myosdisk
            statuses
              code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
              time: 4/13/2016 8:04:36 PM

          VM general status
            provisioningStatus: Succeeded
            id: /subscriptions/{subscription-id}
              /resourceGroups/rg1/providers/Microsoft.Compute/virtualMachines/vm1
            name: vm1
            type: Microsoft.Compute/virtualMachines
            location: centralus

          VM instance status
            code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
            code: PowerState/running
              level: Info
              displayStatus: VM running

## <a name="stop-a-virtual-machine"></a>Beenden eines virtuellen Computers

Sie können einen virtuellen Computer auf zwei Arten beenden. Sie können ein virtuellen Computers zu beenden und alle zugehörigen Einstellungen beibehalten, aber weiterhin dafür in Rechnung gestellt oder beenden ein virtuellen Computers können und sie freigeben. Wenn Sie ein virtuellen Computer freigegeben ist, sind alle Ressourcen, die zugeordnet auch freigegeben und Abrechnung enden dafür aus.

1. Kommentieren Sie keinen Code aus, die Sie zuvor hinzugefügt haben, für die Main-Methode, mit Ausnahme des Codes zum Abrufen der Anmeldeinformationen.

2. Diese Methode der Klasse Programm hinzufügen:

        public static async void StopVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Stopping the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.PowerOffAsync(groupName, vmName);
        }

    Wenn Sie die virtuellen Computern freigeben möchten, ändern Sie den Anruf ausschalten in diesen Code:

        computeManagementClient.VirtualMachines.Deallocate(groupName, vmName);

3. Zum Aufrufen der Methode, die Sie soeben hinzugefügt haben, fügen Sie diesen Code zur Methode Main:

        StopVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei Program.cs.

5. Klicken Sie auf in Visual Studio **Starten** , und klicken Sie dann melden Sie sich bei Azure AD mit demselben Benutzernamen und Ihr Kennwort ein, die Sie mit Ihrem Abonnement verwenden.

    Den Status des virtuellen Computers geändert, so weiterspielen sollte angezeigt werden. Wenn die Methode, die einen Deallocate, den Status beendet wird (freigegeben) ausgeführt wurde.

## <a name="start-a-virtual-machine"></a>Starten eines virtuellen Computers

1. Kommentieren Sie keinen Code aus, die Sie zuvor hinzugefügt haben, für die Main-Methode, mit Ausnahme des Codes zum Abrufen der Anmeldeinformationen.

2. Diese Methode der Klasse Programm hinzufügen:

        public static async void StartVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Starting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.StartAsync(groupName, vmName);
        }

3. Zum Aufrufen der Methode, die Sie soeben hinzugefügt haben, fügen Sie diesen Code zur Methode Main:

        StartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei Program.cs.

5. Klicken Sie auf in Visual Studio **Starten** , und klicken Sie dann melden Sie sich bei Azure AD mit demselben Benutzernamen und Ihr Kennwort ein, die Sie mit Ihrem Abonnement verwenden.

    Den Status des virtuellen Computers zu Ausführung ändern sollte angezeigt werden.

## <a name="restart-a-running-virtual-machine"></a>Starten einer laufenden virtuellen Computern

1. Kommentieren Sie keinen Code aus, die Sie zuvor hinzugefügt haben, für die Main-Methode, mit Ausnahme des Codes zum Abrufen der Anmeldeinformationen.

2. Diese Methode der Klasse Programm hinzufügen:

        public static async void RestartVirtualMachineAsync(
          TokenCredentials credential,
          string groupName,
          string vmName,
          string subscriptionId)
        {
          Console.WriteLine("Restarting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.RestartAsync(groupName, vmName);
        }

3. Zum Aufrufen der Methode, die Sie soeben hinzugefügt haben, fügen Sie diesen Code zur Methode Main:

        RestartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei Program.cs.

5. Klicken Sie auf in Visual Studio **Starten** , und klicken Sie dann melden Sie sich bei Azure AD mit demselben Benutzernamen und Ihr Kennwort ein, die Sie mit Ihrem Abonnement verwenden.

## <a name="resize-a-virtual-machine"></a>Ändern der Größe eines virtuellen Computers

Dieses Beispiel zeigt, wie Sie die Größe eines ausgeführten virtuellen Computers zu ändern.

1. Kommentieren Sie keinen Code aus, die Sie zuvor hinzugefügt haben, für die Main-Methode, mit Ausnahme des Codes zum Abrufen der Anmeldeinformationen.

2. Diese Methode der Klasse Programm hinzufügen:

        public static async void UpdateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Updating the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.HardwareProfile.VmSize = "Standard_A1";
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Zum Aufrufen der Methode, die Sie soeben hinzugefügt haben, fügen Sie diesen Code zur Methode Main:

        UpdateVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei Program.cs.

5. Klicken Sie auf in Visual Studio **Starten** , und klicken Sie dann melden Sie sich bei Azure AD mit demselben Benutzernamen und Ihr Kennwort ein, die Sie mit Ihrem Abonnement verwenden.

    Die Größe des virtuellen Computers geändert, so Standard_A1 sollte angezeigt werden.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Fügen Sie einen Datenträger mit einem virtuellen Computer

Dieses Beispiel zeigt, wie ausgeführten virtuellen Computers einen Datenträger hinzugefügt.

1. Kommentieren Sie keinen Code aus, die Sie zuvor hinzugefügt haben, für die Main-Methode, mit Ausnahme des Codes zum Abrufen der Anmeldeinformationen.

2. Diese Methode der Klasse Programm hinzufügen:

        public static async void AddDataDiskAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Adding the disk to the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.StorageProfile.DataDisks.Add(
            new DataDisk
              {
                Lun = 0,
                Name = "mydatadisk1",
                Vhd = new VirtualHardDisk
                  {
                    Uri = "https://mystorage1.blob.core.windows.net/vhds/mydatadisk1.vhd"
                  },
                CreateOption = DiskCreateOptionTypes.Empty,
                DiskSizeGB = 2,
                Caching = CachingTypes.ReadWrite
              });
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Zum Aufrufen der Methode, die Sie soeben hinzugefügt haben, fügen Sie diesen Code zur Methode Main:

        AddDataDiskAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei Program.cs.

5. Klicken Sie auf in Visual Studio **Starten** , und klicken Sie dann melden Sie sich bei Azure AD mit demselben Benutzernamen und Ihr Kennwort ein, die Sie mit Ihrem Abonnement verwenden.

## <a name="delete-a-virtual-machine"></a>Löschen eines virtuellen Computers

1. Kommentieren Sie keinen Code aus, die Sie zuvor hinzugefügt haben, für die Main-Methode, mit Ausnahme des Codes zum Abrufen der Anmeldeinformationen.

2. Diese Methode der Klasse Programm hinzufügen:

        public static async void DeleteVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Deleting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.DeleteAsync(groupName, vmName);
        }

3. Zum Aufrufen der Methode, die Sie soeben hinzugefügt haben, fügen Sie diesen Code zur Methode Main:

        DeleteVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei Program.cs.

5. Klicken Sie auf in Visual Studio **Starten** , und klicken Sie dann melden Sie sich bei Azure AD mit demselben Benutzernamen und Ihr Kennwort ein, die Sie mit Ihrem Abonnement verwenden.

## <a name="next-steps"></a>Nächste Schritte

Wäre es Probleme bei einer-Bereitstellung, können Sie bei der [Problembehandlung Ressource Gruppe Bereitstellungen mit Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md) aussehen.
