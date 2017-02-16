<properties
    pageTitle="Häufig gestellte Fragen zu Azure Stapel | Microsoft Azure"
    description="Azure Stapel gestellte häufig Fragen."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="frequently-asked-questions-for-azure-stack"></a>Häufig gestellte Fragen zu Azure Stapel

## <a name="deployment"></a>Bereitstellung

### <a name="do-i-need-to-format-my-data-disks-before-starting-or-restarting-an-installation"></a>Muss ich meine Datenfestplatten vor dem Starten oder Neustarten einer Installations formatieren?

Datenträger unformatierten Format hinzugefügt werden. Wenn Sie das Betriebssystem neu installieren, müssen Sie prüfen, ob der alte Speicherpool noch vorhanden ist, und löschen die folgenden Schritte aus:

1. Server-Manager zu öffnen.
2. Wählen Sie Speicherpools aus.
3. Angezeigt, wenn ein Speicherpool aufgeführt ist.
4. Wenn aufgeführt, und aktivieren lesen / schreiben, mit der rechten Maustaste **Speicherpool** .
5. Mit der rechten Maustaste **virtuelle Festplatte** (unten links), und wählen Sie löschen.
6. Mit der rechten Maustaste **Speicherpool** , und klicken Sie auf Löschen.
7. Azure Stapel Skript erneut zu starten, und stellen Sie sicher, dass die Datenträger Überprüfung erfolgreich war.

Optional kann mit dem folgende Skript verwendet werden:

```PowerShell
$pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
if ($pools -ne $null) {
  $pools| Set-StoragePool -IsReadOnly $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$False
  $pools | Remove-StoragePool -Confirm:$False
}
```

### <a name="can-i-use-all-ssd-disks-for-the-storage-pool-in-the-poc-installation"></a>Kann ich alle SSD Datenträger für den Speicherpool in der Prüfung des Konzepts ist Installation verwenden?

Diese Konfiguration wird in dieser Version nicht unterstützt.  Weitere Informationen finden Sie unter den [Anforderungen Leitfaden](azure-stack-deploy.md) für Weitere Informationen.

### <a name="can-i-use-nvme-data-disks-for-the-microsoft-azure-stack-poc"></a>Kann ich für den Microsoft Azure Stapel Prüfung des Konzepts NVMe Daten Datenträger verwenden?

Während der Speicher Leerzeichen direkte NVMe Datenträger unterstützt, unterstützt Azure Stapel nur eine Teilmenge der möglichen Laufwerkstypen und möglichen Kombinationen für Speicher Leerzeichen direkte. 

### <a name="how-can-i-reinstall-azure-stack"></a>Wie kann ich Azure Stapel erneut installieren?
Führen Sie die Schritte im [Leitfaden für die erneute Bereitstellung](azure-stack-redeploy.md).  

## <a name="tenant"></a>Mandanten

### <a name="can-i-deploy-my-own-images-as-a-tenant"></a>Kann ich meinen eigenen Bilder als einen Mandanten bereitstellen?

Ja, kann genau wie in Azure, ein Mandanten Bilder in Azure-Stapel hochladen zusätzlich zu verwenden, die Bilder aus dem Dienstadministrator. Finden Sie eine Übersicht über das [Hinzufügen eines Bilds virtueller Computer](azure-stack-add-vm-image.md). 

## <a name="testing"></a>Testen

### <a name="can-i-use-nested-virtualization-to-test-the-microsoft-azure-stack-poc"></a>Kann ich mithilfe von verschachtelt Virtualisierung Testen der Microsoft Azure Stapel Prüfung des Konzepts ist?

Geschachtelte Virtualisierung wird nicht unterstützt und mit Azure Stapel Technical Preview 2 getestet.

## <a name="virtual-machines"></a>Virtuellen Computern

### <a name="does-azure-stack-support-dynamic-disks-for-virtual-machines"></a>Unterstützt Azure Stapel dynamische Datenträger für virtuellen Computern?

Dynamische Datenträger unterstützt Azure Stapel nicht.

## <a name="next-steps"></a>Nächste Schritte

[Behandlung von Problemen](azure-stack-troubleshooting.md)
