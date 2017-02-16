## <a name="overview"></a>(Übersicht)
Wenn Sie einen neuen virtuellen Computer (virtueller Computer) in eine Ressourcengruppe erstellen, indem Sie ein Bild aus [Azure Marketplace](https://azure.microsoft.com/marketplace/), ist das Standard-Laufwerk OS 127 GB. Obwohl es möglich ist, Hinzufügen von Datenträger mit Daten zu den virtuellen Computer (wie viele abhängig von der SKU Sie ausgewählt haben), und es darüber hinaus empfohlen hat, Anwendungen und stark CPU-Auslastung auf diesen Zusatz Festplatten häufig Suchfunktionen installieren Kunden benötigen, um das Laufwerk OS zur Unterstützung von bestimmter Szenarien wie den folgenden zu erweitern:

1.  Unterstützung für legacy-Anwendungen, die Komponenten auf OS Systemlaufwerk installiert.
2.  Migrieren einer physischen PC oder virtuellen Computern aus lokalen ein größeres OS Laufwerk an.

>[AZURE.IMPORTANT]Azure weist zwei verschiedenen Bereitstellungsmodelle für das Erstellen und Arbeiten mit Ressourcen: Ressourcenmanager und Classic. Dieser Artikel behandelt das Modell Ressourcenmanager verwenden. Microsoft empfiehlt, die meisten neue Bereitstellungen Ressourcenmanager Modell verwenden.

## <a name="resize-the-os-drive"></a>Ändern der Größe des OS-Laufwerks
In diesem Artikel wird das erreichen wir der Aufgabe, Ändern der Größe des OS Laufwerks Ressource-Manager Module von [Azure Powershell](../articles/powershell-install-configure.md)verwenden. Öffnen Sie Ihre Powershell ISE oder der Powershell-Fenster im administrativen Modus aus, und führen Sie die folgenden Schritte aus:

1.  Anmelden bei Ihrem Microsoft Azure-Konto im Modus für Ressourcen Management und wählen Sie Ihr Abonnement wie folgt aus:

    ```Powershell
    Login-AzureRmAccount
    Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
    ```

2.  Setzen Sie Ihre Gruppe Ressourcenname und der Name des virtuellen Computers wie folgt ein:

    ```Powershell
    $rgName = 'my-resource-group-name'
    $vmName = 'my-vm-name'
    ```

3.  Rufen Sie einen Verweis auf Ihre virtuellen Computer wie folgt ein:

    ```Powershell
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```

4. Beenden Sie den virtuellen Computer vor dem Ändern der Größe des Datenträgers ist wie folgt aus:

    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```

5.  Und hier kommt der Moment der wir gewartet haben! Legen Sie die Größe des Datenträgers OS auf den gewünschten Wert ein, und aktualisieren Sie den virtuellen Computer wie folgt:

    ```Powershell
    $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
    ```

    >[AZURE.WARNING]Die neue Größe sollten größer als die Größe des vorhandenen. Die maximal zulässige Anzahl ist 1023 GB.

6.  Aktualisieren den virtuellen Computer kann einige Sekunden dauern. Starten Sie den virtuellen Computer nach Beendigung der Befehl ausführen wie folgt:

    ```Powershell
    Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```

Und das war's schon! Jetzt RDP in den virtuellen Computer Computer Management (oder Datenträger Verwaltung) zu öffnen und des Laufwerks mithilfe den neu zugewiesenen Speicherplatz zu erweitern.

## <a name="summary"></a>Zusammenfassung
In diesem Artikel verwendet wir Azure Ressourcenmanager Module der Powershell, um das Laufwerk OS eines IaaS virtuellen Computers zu erweitern. Unter reproduziert wird das vollständige Skript für den Verweis:

```Powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="next-steps"></a>Nächste Schritte
Obwohl in diesem Artikel wir Schwerpunkt bei den OS Datenträger, der den virtuellen Computer erweitern, kann das Skript entwickelte auch zum Erweitern eine einzelne Zeile des Codes ändern, die den virtuellen Computer angefügten Festplatten mit den Daten verwendet werden. Beispielsweise, um den ersten Daten Datenträger, die den virtuellen Computer angefügt zu erweitern, ersetzen die ```OSDisk``` Objekt des ```StorageProfile``` mit ```DataDisks``` Arrays, und verwenden Sie einen numerischen Index einen Verweis auf den ersten Datenträger angefügten Daten abgerufen, wie unten dargestellt:

```Powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```
Auf ähnliche Weise anderen Datenträger, die den virtuellen Computer, entweder mithilfe eines Indexes obigen angefügt verwiesen werden kann oder das ```Name``` Eigenschaft des Datenträgers wie unten dargestellt:

```Powershell
($vm.StorageProfile.DataDisks | Where {$_.Name -eq 'my-second-data-disk'})[0].DiskSizeGB = 1023
```

Wenn Sie erfahren, wie eine Azure Ressourcenmanager VM Datenträger anfügen möchten, aktivieren Sie in diesem [Artikel](../articles/virtual-machines/virtual-machines-windows-attach-disk-portal.md).
