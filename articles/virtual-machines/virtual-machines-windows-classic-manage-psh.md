<properties
   pageTitle="Verwalten Sie Ihre virtuellen Computer mithilfe von Azure PowerShell | Microsoft Azure"
   description="Erfahren Sie, Befehle, die Sie zum Automatisieren von Aufgaben, die bei der Verwaltung Ihrer virtuellen Computern verwenden können."
   services="virtual-machines-windows"
   documentationCenter="windows"
   authors="singhkays"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

   <tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/12/2016"
   ms.author="kasing"/>

# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Verwalten Sie Ihre virtuellen Computer mithilfe von Azure PowerShell

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Die automatisierte viele Aufgaben, die Sie jeden Tag zum Verwalten Ihrer virtuellen Computer ausführen können mithilfe von Azure PowerShell-Cmdlets. In diesem Artikel erhalten Sie Beispiele für Befehle für einfacher Aufgaben und Links zu Artikeln, die die Befehle für komplexere Aufgaben angezeigt werden.

>[AZURE.NOTE] Wenn Sie noch nicht installiert und Azure PowerShell noch nicht konfiguriert wurde, können Sie die Anweisungen im Artikel [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)abrufen.

## <a name="how-to-use-the-example-commands"></a>So verwenden Sie die Beispielbefehle
Sie müssen ein Teil des Texts in die Befehle mit Text zu ersetzen, die für Ihre Umgebung geeignet ist. Die < und > Symbole zeigen Text ersetzt werden muss. Wenn Sie den Text zu ersetzen, entfernen Sie die Symbole, aber belassen Sie die Anführungszeichen im Ort.

## <a name="get-a-vm"></a>Abrufen eines virtuellen Computers
Dies ist eine einfache Aufgabe, die Sie häufig verwenden möchten. Verwenden Sie diese erhalten von Informationen zu eines virtuellen Computers, führen Sie Aufgaben auf einen virtuellen oder erste ausgeben, um in einer Variablen zu speichern.

Um Informationen zu den virtuellen Computer zu erhalten, führen Sie diesen Befehl, ersetzen alles in Anführungszeichen, einschließlich der < und > Zeichen:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Wenn die Ausgabe in einer Variablen $vm speichern möchten, führen Sie Folgendes aus:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Melden Sie sich an einen Windows-basierten virtuellen

Führen Sie die folgenden Befehle:

>[AZURE.NOTE] Sie können den virtuellen Computern und Cloud-Dienstnamen aus der Anzeige des Befehls **Get-AzureVM** erhalten.
>
    $svcName = "<cloud service name>"
    $vmName = "<virtual machine name>"
    $localPath = "<drive and folder location to store the downloaded RDP file, example: c:\temp >"
    $localFile = $localPath + "\" + $vmname + ".rdp"
    Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch

## <a name="stop-a-vm"></a>Beenden eines virtuellen Computers

Führen Sie diesen Befehl ein:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

>[AZURE.IMPORTANT]Verwenden Sie diesen Parameter, die virtuelle IP-Adresse des Cloud-Dienst beibehalten, falls es sich um den letzten virtuellen Computer in die Cloud-Dienst handelt. <br><br> Wenn Sie den Parameter StayProvisioned verwenden, erhalten Sie weiterhin für den virtuellen Computer Abrechnung.

## <a name="start-a-vm"></a>Starten eines virtuellen Computers

Führen Sie diesen Befehl ein:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Fügen Sie einen Datenträger
Dieser Vorgang ist ein paar Schritte erforderlich. Verwenden Sie zuerst die *** hinzufügen-Cmdlet AzureDataDisk *** den Datenträger auf das Objekt $vm hinzufügen. Verwenden Sie dann **Update-AzureVM** -Cmdlet aktualisieren Sie die Konfiguration von den virtuellen Computer aus.

Sie müssen entscheiden, ob fügen Sie einen neuen Datenträger oder die Daten enthält. Für einen neuen Datenträger wird der Befehl VHD-Datei erstellt und fügt es.

Wenn Sie einen neuen Datenträger installieren zu können, führen Sie diesen Befehl aus:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Um einen vorhandenen Daten Datenträger anfügen möchten, führen Sie diesen Befehl aus:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Wenn Datenträger mit Daten aus einer vorhandenen .vhd-Datei im BLOB-Speicher anfügen möchten, führen Sie diesen Befehl aus:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Erstellen eines Windows-basierten virtuellen Computers

Führen Sie die Anweisungen zum Erstellen eines neuen Windows-basierten virtuellen Computers in Azure in [Azure-PowerShell zu erstellen und Windows-basierten virtuellen Maschinen vorkonfiguriert verwenden](virtual-machines-windows-classic-create-powershell.md). In diesem Thema schrittweise erstellt Sie durch die Erstellung einer Azure PowerShell-Befehl, die festlegen einen Windows-basierten virtuellen, die vorkonfiguriert werden kann:

- Mit Active Directory-Domänenmitgliedschaft.
- Mit zusätzlichen Festplatten.
- Als Mitglied einer vorhandenen Lastenausgleich festlegen.
- Mit einer statischen IP-Adresse.
