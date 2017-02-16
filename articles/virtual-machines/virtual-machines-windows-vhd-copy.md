<properties
    pageTitle="Erstellen einer Kopie eines bestimmten virtuellen Computers in Azure | Microsoft Azure"
    description="Informationen Sie zum Erstellen einer Kopie eines spezielle Windows virtuellen Computers in Azure, in dem Modell zur Bereitstellung von Ressourcenmanager ausgeführt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
# <a name="create-a-copy-of-a-specialized-windows-vm-running-in-azure"></a>Erstellen einer Kopie eines spezielle Windows virtuellen Computers in Azure ausgeführt 

In diesem Artikel wird veranschaulicht, wie Sie das Tool AZCopy, um eine Kopie der virtuellen Festplatte eines spezielle Windows virtuellen Computers zu erstellen, die in Azure ausgeführt wird. Sie können die Kopie der virtuellen Festplatte dann zum Erstellen eines neuen virtuellen Computers verwenden. 

- Wenn zum Kopieren eines GRG virtuellen Computers finden Sie unter [So erstellen Sie ein Bild virtueller Computer aus einer vorhandenen GRG Azure-virtuellen Computer](virtual-machines-windows-capture-image.md).

- Wenn Sie eine virtuelle Festplatte aus einer lokalen virtueller Computer, wie eine erstellt Hyper-V, die finden Sie unter [Hochladen einer virtuellen Windows aus einer lokalen virtueller Computer in Azure](virtual-machines-windows-upload-image.md)hochladen möchten.


## <a name="before-you-begin"></a>Vorbemerkung

Stellen Sie sicher, dass Sie:

- Informationen über die **Quell- und Zielfelder Speicherkonten**ist. Für die Quelle virtueller Computer müssen Sie Speicher-Konto und Container Namen. In der Regel werden der Containernamen **virtueller Festplatten**. Sie müssen ein Ziel-Speicher-Konto verfügen. Wenn Sie noch keine haben, können Sie mit dem Portal einen erstellen (**Weitere Dienste** > Speicher-Konten > hinzufügen) oder mithilfe des Cmdlets [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) . 

- Azure [PowerShell 1.0](../powershell-install-configure.md) haben (oder höher) installiert.

- Heruntergeladen und das [Tool AzCopy](../storage/storage-use-azcopy.md)installiert haben. 


## <a name="deallocate-the-vm"></a>Freigeben Sie den virtuellen Computer

Freigeben Sie den virtuellen Computer, die von der virtuellen Festplatte zu kopierenden freigibt. 

- **Portal**: Klicken Sie auf **virtuellen Computern** > **"MyVM"** > Beenden
- **PowerShell**: `Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM` den virtuellen Computer mit dem Namen **"MyVM"** in der Gruppe für die Ressource **MyResourceGroup**freigegeben.

Der **Status** für den virtuellen Computer im Azure-Portal von **weiterspielen** auf **beendet (freigegeben)**geändert wird.


## <a name="get-the-storage-account-urls"></a>Die URLs der Speicher-Konto abrufen

Sie benötigen die URLs der Quell- und Zielfelder Speicherkonten. Die URLs aussehen: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Wenn Sie bereits im Speicher-Konto und Container bekannt sind, können Sie die Informationen in geschweifte Klammern So erstellen Sie Ihre URL einfach ersetzen. 

Der Azure-Portal oder Azure Powershell können Sie um die URL zu erhalten:

- **Portal**: Klicken Sie auf **Weitere Dienste** > **Speicherkonten**  >  <storage account> **Blobs** und der Quelldatei für die virtuelle Festplatte befindet sich wahrscheinlich im Container **virtuelle Festplatten** . Klicken Sie auf **Eigenschaften** für den Container, und kopieren Sie den Text mit der Bezeichnung **URL**. Sie benötigen die URLs der Quell- und Ziel-Container aus. 

- **PowerShell**: `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` die Informationen für die virtuellen Computer mit der Bezeichnung **"MyVM"** in der Ressource Gruppe **MyResourceGroup**abgerufen. Suchen Sie in den Suchergebnissen im Abschnitt **Speicher Profile** für die **Virtuelle Festplatte Uri**. Im erste Teil der Uri ist die URL zum Container und des letzten Teils der OS virtuelle Festplatte Name für den virtuellen Computer.

## <a name="get-the-storage-access-keys"></a>Abrufen der Speicher-Tastenkombinationen

Suchen der Tastenkombinationen für Quell- und Speicherkonten. Weitere Informationen zu Tastenkombinationen finden Sie unter [Informationen zum Azure-Speicherkonten](../storage/storage-create-storage-account.md).

- **Portal**: Klicken Sie auf **Weitere Dienste** > **Speicherkonten**  >  <storage account> **Alle Einstellungen** > **Zugriffstasten**. Kopieren Sie die Taste mit **Schlüssel1**bezeichnet.
- **PowerShell**: `Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup` der Ressource Gruppe **MyResourceGroup**die Taste Speicherplatz für den Speicher Konto **Mystorageaccount** erhält. Kopieren Sie die Taste mit **Schlüssel1**bezeichnet.


## <a name="copy-the-vhd"></a>Kopieren Sie die virtuelle Festplatte 

Sie können Dateien zwischen Speicherkonten mit AzCopy kopieren. Für den Zielcontainer Wenn der angegebene Container nicht vorhanden ist, wird es für Sie erstellt. 

Wenn AzCopy verwenden möchten, öffnen Sie ein Eingabeaufforderungsfenster auf dem lokalen Computer, und navigieren Sie zu dem Ordner, in dem AzCopy installiert ist. Ähnlich wie *c:\Programme Dateien (x86) \Microsoft SDKs\Azure\AzCopy*werden. 

Wenn Sie alle Dateien innerhalb eines Containers kopieren möchten, verwenden Sie die **Befehlszeilenoption/s** . Hiermit kann die OS virtuelle Festplatte und alle Datenträger Daten kopieren, wenn Sie sich im gleichen Container befinden. Dieses Beispiel zeigt, wie aller Dateien in den Container **Mysourcecontainer** im Speicher Konto **Mysourcestorageaccount** in den Container **Mydestinationcontainer** im Speicher **Mydestinationstorageaccount** Konto zu kopieren. Ersetzen Sie die Namen der Speicherkonten und Container durch eigene. Ersetzen Sie `<sourceStorageAccountKey1>` und `<destinationStorageAccountKey1>` mit Ihrer eigenen Tasten.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Wenn Sie nur eine bestimmte virtuelle Festplatte in einem Container mit mehreren Dateien kopieren möchten, können Sie auch den Dateinamen, der mit dem Schalter /Pattern angeben. In diesem Beispiel werden nur die Datei, die mit dem Namen **myFileName.vhd** kopiert.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
        /Pattern:myFileName.vhd
```


Wenn der Vorgang abgeschlossen ist, erhalten Sie eine Nachricht, die sieht ungefähr wie folgt aus:

```
  Finished 2 of total 2 file(s).
  [2016/10/07 17:37:41] Transfer summary:
  -----------------
  Total files transferred: 2
  Transfer successfully:   2
  Transfer skipped:        0
  Transfer failed:         0
  Elapsed time:            00.00:13:07
```

## <a name="troubleshooting"></a>Behandlung von Problemen

- Wenn Sie AZCopy, verwenden, wenn Sie sehen, dass die Fehlermeldung "Fehler bei der Authentifizierung der Anfrage-Server. Stellen Sie sicher, dass der Wert von Autorisierung Kopfzeile ordnungsgemäß einschließlich der Signatur formatiert ist." und verwenden Sie die 2-Taste oder die Taste sekundären Speicher, versuchen Sie, die primäre oder 1st Speicher-Taste.


## <a name="next-steps"></a>Nächste Schritte

- Sie können einen neuen virtuellen Computer durch [Anfügen die Kopie der virtuellen Festplatte zu eines virtuellen Computers als einen Datenträger OS](virtual-machines-windows-create-vm-specialized.md)erstellen.












