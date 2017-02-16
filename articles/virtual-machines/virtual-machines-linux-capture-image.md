<properties
    pageTitle="Erfassen eines Linux VM als Vorlage verwenden | Microsoft Azure"
    description="Informationen Sie zum Erfassen und generalize ein Bild von einer Linux-basierten Azure-virtuellen Computern (virtueller Computer) mit dem Modell zur Bereitstellung von Azure Ressourcenmanager erstellt."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="iainfou"/>


# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Erfassen eines Linux virtuellen Computern, die auf Windows Azure ausgeführte

Führen Sie die Schritte in diesem Artikel generalize und Ihre Azure Linux virtuellen Computern (virtueller Computer) im Bereitstellungsmodell Ressourcenmanager erfassen. Wenn Sie den virtuellen Computer verallgemeinern, werden Sie persönlichen Kontoinformationen entfernen und Vorbereiten den virtuellen Computer als ein Bild verwendet werden soll. Klicken Sie dann erfassen Sie ein Bild GRG virtuelle Festplatte (virtuelle Festplatte) für das Betriebssystem, virtuellen Festplatten für angefügten Daten Datenträger und [Ressourcenmanager Vorlage](../azure-resource-manager/resource-group-overview.md) für die Bereitstellung neuer virtueller Computer. 

Zum Erstellen von virtuellen Computern, die das Bild mit Netzwerk-Ressourcen für jeden neuen virtuellen Computer richten Sie ein und mit der Vorlage (eine JavaScript Object Notation oder JSON-Datei) können Sie diese virtuelle Festplatte aufgenommene Bilder bereitstellen. Auf diese Weise kann einen virtueller Computer mit der aktuellen Softwarekonfiguration, ähnlich wie repliziert Verwendung von Bildern in der Azure Marketplace.

>[AZURE.TIP]Wenn Sie eine Kopie Ihrer vorhandenen Linux VM mit speziellen Zustand für die Sicherung oder für das Debuggen erstellen möchten, finden Sie unter [Erstellen einer Kopie eines auf Windows Azure ausgeführte Linux virtuellen Computers](virtual-machines-linux-copy-vm.md). Und wenn Sie eine Linux VHD aus einer lokalen virtuellen Computer hochladen möchten, finden Sie unter [Hochladen, und erstellen Sie eine Linux VM von benutzerdefinierten Image](virtual-machines-linux-upload-vhd.md).  

## <a name="before-you-begin"></a>Vorbemerkung

Stellen Sie sicher, dass Sie die folgenden Vorkenntnisse erfüllen:

* **Azure-virtuellen Computer erstellt haben, in dem Modell zur Bereitstellung von Ressourcenmanager** – Wenn Sie eine Linux VM erstellt haben, können Sie das [Portal](virtual-machines-linux-quick-create-portal.md), die [Azure CLI](virtual-machines-linux-quick-create-cli.md)oder [Ressourcenmanager Vorlagen](virtual-machines-linux-cli-deploy-templates.md)verwenden. 

    Konfigurieren Sie den virtuellen Computer nach Bedarf. Beispielsweise [Daten Datenträger hinzufügen](virtual-machines-linux-add-disk.md), Updates anwenden, und Installieren von Applications. 
* **Azure CLI** - Installation [Azure CLI](../xplat-cli-install.md) auf einem lokalen Computer.

## <a name="step-1-remove-the-azure-linux-agent"></a>Schritt 1: Entfernen des Azure Linux-Agents

Führen Sie zuerst den Befehl **Waagent** mit dem Parameter **entziehen** , auf die Linux VM. Dieser Befehl löscht Dateien und Daten zu den virtuellen Computer für verallgemeinern bereit zu machen. Details finden Sie unter der [Azure Linux Agent-Benutzerhandbuch](virtual-machines-linux-agent-user-guide.md).

1. Verbinden Sie mit Ihrer Linux VM SSH-Client verwenden.

2. Geben Sie im Fenster SSH mit dem folgenden Befehl aus:

    ```
    sudo waagent -deprovision+user
    ```
    >[AZURE.NOTE] Führen Sie diesen Befehl nur auf einem virtuellen Computer, die Sie als Bild aufzeichnen möchten. Sie gewährleistet jedoch nicht, dass das Bild aller vertrauliche Informationen deaktiviert ist oder für die Verteilung geeignet ist.

3. Geben Sie **y** , um den Vorgang fortzusetzen. Sie können Hinzufügen der **-erzwingen** Parameter diesen Bestätigungsschritt zu vermeiden.

4. Geben Sie nach Abschluss des Befehls **Beenden**. Dieser Schritt schließt den SSH-Client.

    
## <a name="step-2-capture-the-vm"></a>Schritt 2: Erfassen Sie den virtuellen Computer

Verwenden Sie die CLI Azure generalize und erfassen den virtuellen Computer an. Ersetzen Sie in den folgenden Beispielen Beispiel Parameternamen durch Ihre eigenen Werte ein. Beispiel für Parameternamen gehören **MyResourceGroup**, **MyVnet**und **"MyVM"**.

5. Öffnen Sie von Ihrem lokalen Computer Azure CLI und [Melden Sie sich Ihr Abonnement Azure](../xplat-cli-connect.md)aus. 

6. Stellen Sie sicher, dass Sie Ressourcenmanager Modus aufweisen.

    ```
    azure config mode arm
    ```

7. Fahren Sie den virtuellen Computer, die Sie bereits mit dem folgenden Befehl hat:

    ```
    azure vm deallocate -g MyResourceGroup -n myVM
    ```

8. Generalize den virtuellen Computer mit den folgenden Befehl aus:

    ```
    azure vm generalize -g MyResourceGroup -n myVM
    ```

9. Führen Sie nun den Befehl **Azure-virtuellen Computer zu erfassen** , der den virtuellen Computer erfasst werden. Im folgenden Beispiel die Bild virtuellen Festplatten mit Namen beginnend mit **MyVHDNamePrefix**erfasst werden, und die Option **t -** gibt den Pfad zu der Vorlage **MyTemplate.json**. 

    ```
    azure vm capture MyResourceGroup MyResourceGroup MyVHDNamePrefix -t MyTemplate.json
    ```

    >[AZURE.IMPORTANT]Die virtuelle Festplatte Bilddateien erhalten standardmäßig in demselben Speicherkonto erstellt, die ursprünglichen virtuellen verwendet. Verwenden Sie die *gleichen Speicherkonto* die virtuellen Festplatten für alle neuen virtuellen Computern gespeichert, die Sie aus dem Bild zu erstellen. 

6. Um den Speicherort einer erfasste Bild zu finden, öffnen Sie die JSON-Vorlage in einem Text-Editor ein. Finden Sie in der **StorageProfile**den **Uri** des **Bilds** befindet sich im **System** -Container aus. Beispielsweise ähnelt der URI des Bilds OS Datenträger`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Schritt 3: Erstellen eines virtuellen Computers aus das erfasste Bild
Jetzt können verwenden Sie das Bild mit einer Vorlage zum Erstellen einer Linux VM. Diese Schritte zeigen, wie mit der CLI Azure und die JSON-Vorlagendatei, die Sie erfasst, um den virtuellen Computer in ein neues virtuelles Netzwerk erstellen.

### <a name="create-network-resources"></a>Erstellen von Netzwerk-Ressourcen

Wenn Sie die Vorlage verwenden möchten, müssen Sie zuerst eine virtuelle Netzwerk und NIC für Ihre neue virtuellen Computer einrichten. Es empfiehlt sich, dass Sie an der Stelle eine Ressourcengruppe für diese Ressourcen erstellen Bild virtueller Computer gespeichert ist. Ausführen von Befehlen ähnlich wie die folgenden, durch Namen für Ihre Ressourcen und eine entsprechende Azure Position (diese Befehle "Centralus"):

    azure group create MyResourceGroup1 -l "centralus"

    azure network vnet create MyResourceGroup1 myVnet -l "centralus"

    azure network vnet subnet create MyResourceGroup1 myVnet mySubnet

    azure network public-ip create MyResourceGroup1 myIP -l "centralus"

    azure network nic create MyResourceGroup1 myNIC -k mySubnet -m myVnet -p myIP -l "centralus"

### <a name="get-the-id-of-the-nic"></a>Rufen Sie die Id der NIC

Zum Bereitstellen eines virtuellen Computers des Bilds mithilfe des JSON, die Sie während der Aufzeichnung gespeichert haben, benötigen Sie die Id des der NIC. Ermitteln sie den folgenden Befehl ausführen:

    azure network nic show MyResourceGroup1 myNIC

Die **Id** in der Ausgabe ähnelt dem`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`



### <a name="create-a-vm"></a>Erstellen eines virtuellen Computers
Führen Sie nun den folgenden Befehl zu Ihrer virtuellen Computer aus das erfasste Bild des virtuellen Computer zu erstellen. Verwenden Sie den Parameter **-f** , um den Pfad zu der Vorlage JSON-Datei anzugeben, die Sie gespeichert haben.

    azure group deployment create MyResourceGroup1 MyDeployment -f MyTemplate.json

Die Ausgabe des Befehls werden Sie aufgefordert, zum Angeben eines neuen Name des virtuellen Computers, den Administrator-Benutzernamen und Kennwort und die Id des die NIC, die Sie zuvor erstellt haben.

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    vmName: myNewVM
    adminUserName: myAdminuser
    adminPassword: ********
    networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic

Im folgende Beispiel wird gezeigt, was Sie für eine erfolgreiche Bereitstellung finden Sie unter:

    + Initialisierung Vorlage Konfigurationen und Parameter
    + Erstellen einer Bereitstellungsinformationen: die Bereitstellung Xxxxxxx Vorlage erstellt
    + Warten auf Abschluss der Bereitstellung: DeploymentName: MyDeployment Daten: ResourceGroupName: MyResourceGroup1 Daten: ProvisioningState: Daten erfolgreich: Zeitstempel: Xxxxxxx Daten: Modus: inkrementell Daten: Name Typwert


    data:    ------------------  ------------  -------------------------------------

    Daten: VmName Zeichenfolge MyNewVM


    Daten: VmSize Standard_D1 Zeichenfolge


    Daten: AdminUserName Zeichenfolge MyAdminuser


    Daten: AdminPassword SecureString undefiniert


    Daten: NetworkInterfaceId Zeichenfolge /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic Informationen: Bereitstellung Gruppe erstellen Befehl auf OK

### <a name="verify-the-deployment"></a>Überprüfen der bereitstellungs

SSH jetzt die virtuellen Computern, die Sie erstellt haben, um zu überprüfen, die Bereitstellung und Verwenden von den neuen virtuellen Computer. Informationen zum Verbinden über SSH finden Sie die IP-Adresse für den virtuellen Computer, die Sie erstellt haben, indem Sie den folgenden Befehl ausführen:

    azure network public-ip show MyResourceGroup1 myIP

Die öffentliche IP-Adresse wird in der Ausgabe des Befehls aufgeführt. Standardmäßig verbinden Sie mit der Linux VM von SSH auf Anschluss 22 aus.

## <a name="create-additional-vms"></a>Erstellen von zusätzlichen virtuellen Computern
Verwenden Sie zum Bereitstellen von zusätzlicher virtuellen Computern mit den Schritten im vorherigen Abschnitt erfasste Bild und Vorlage ein. Weitere Optionen zum Erstellen von virtuellen Computern aus dem Bild sind, mithilfe einer Vorlage Schnellstart oder Ausführen des Befehls **Azure-virtuellen Computer erstellen** .

### <a name="use-the-captured-template"></a>Verwenden Sie die aufgenommene Vorlage

Um das aufgenommene Bild und Vorlage verwenden möchten, führen Sie die Schritte (im vorherigen Abschnitt detaillierte):

* Sicherstellen Sie, dass das Bild virtueller Computer in demselben Speicherkonto ist, auf dem Ihre virtuellen Computers virtuelle Festplatte befindet.
* Kopieren Sie die Vorlage JSON-Datei, und geben Sie einen eindeutigen Namen für den Datenträger OS des neuen virtuellen Computers virtuelle Festplatte (oder virtuellen Festplatten). Angenommen, geben Sie die **StorageProfile**, klicken Sie unter **virtuelle Festplatte**im **Uri**, einen eindeutigen Namen für die **OsDisk** virtuelle Festplatte, ähnlich wie`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`
* Erstellen Sie einen Netzwerkadapter in derselben oder ein anderes virtuelle Netzwerk aus.
* Erstellen Sie die geänderte JSON-Vorlagendatei mithilfe einer bereitstellungs in der Ressourcengruppe, in der Sie das virtuelle Netzwerk einrichten.

### <a name="use-a-quickstart-template"></a>Verwenden einer Vorlage Schnellstart

Wenn Sie möchten das Netzwerk einrichten, die automatisch beim eines virtuellen Computers aus dem Bild erstellen, können Sie diese Ressourcen in einer Vorlage angeben. Beispielsweise finden Sie die [Vorlage 101 virtuellen Computer aus Benutzer Bild](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) aus GitHub aus. Diese Vorlage erstellt einen virtuellen von benutzerdefinierten Bild und die erforderlichen virtuellen Netzwerk, öffentliche IP-Adresse und NIC Ressourcen. Eine exemplarische Vorgehensweise bei Verwendung der Vorlage im Azure-Portal finden Sie unter [Erstellen eines virtuellen Computers aus ein benutzerdefiniertes Bild mithilfe einer Vorlage Ressourcenmanager](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).

### <a name="use-the-azure-vm-create-command"></a>Verwenden Sie den Azure-virtuellen Computer erstellen (Befehl)

In der Regel ist es am einfachsten, mithilfe einer Vorlage Ressourcenmanager um aus dem Bild ein virtuellen Computers zu erstellen. Allerdings können die virtuellen Computer _imperativ_ durch mit dem Befehl **Azure-virtuellen Computer erstellen** und der **F-** (**– Bild-Urn**) Parameter. Wenn Sie diese Methode verwenden, übergeben Sie auch den Parameter **-d** (**--os-Datenträger-virtuelle Festplatte**), um den Speicherort der OS VHD-Datei für den neuen virtuellen Computer anzugeben. Diese Datei muss im Container virtuellen Festplatten des Speicherkontos die Bilddatei virtuelle Festplatte gespeichert ist. Der Befehl kopiert die virtuelle Festplatte für den neuen virtuellen Computer automatisch in den Container **virtuelle Festplatten** .

Gehen Sie bevor Sie mit dem Bild ausführen **Azure-virtuellen Computer erstellen** folgendermaßen vor:

1.  Erstellen einer Ressourcengruppe oder eine vorhandene Ressourcengruppe für die Bereitstellung zu identifizieren.

2.  Erstellen einer öffentlichen IP-Adresse und eine NIC Ressource für den neuen virtuellen Computer an. Schritte zum Erstellen von einem virtuellen Netzwerk, öffentliche IP-Adresse und NIC mithilfe der CLI finden Sie weiter oben in diesem Artikel. (**Azure-virtuellen Computer erstellen** , können auch einen Netzwerkadapter erstellen, aber Sie müssen weitere Parameter für eine virtuelle Netzwerk und Subnetz übergeben.)


Führen Sie dann einen Befehl, der URIs an die neue OS virtuelle Festplatte-Datei, und das vorhandene Bild übergibt. In diesem Beispiel wird eine Größe Standard_A1 virtueller Computer in der Region ostasiatischen US erstellt.

    azure vm create MyResourceGroup1 myNewVM eastus Linux -d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" -Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd" -z Standard_A1 -u myAdminname -p myPassword -f myNIC

Befehl zusätzliche Optionen festlegen möchten, führen Sie `azure help vm create`.

## <a name="next-steps"></a>Nächste Schritte

Zum Verwalten Ihrer virtueller Computer mit der CLI finden Sie unter der Vorgänge im [Bereitstellen und Verwalten von virtuellen Computern mithilfe von Azure Ressourcenmanager Vorlagen und Azure CLI](virtual-machines-linux-cli-deploy-templates.md).
