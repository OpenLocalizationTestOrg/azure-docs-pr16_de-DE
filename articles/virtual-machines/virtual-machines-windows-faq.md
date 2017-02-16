<properties
    pageTitle="Häufig gestellte Fragen zur Windows-virtuellen Computern | Microsoft Azure"
    description="Bietet Antworten auf einige häufige Fragen zum Windows mit dem Modell Ressourcenmanager erstellte Maschinen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Häufig gestellte Fragen zu Windows virtuellen Computern 


Dieser Artikel behandelt einige häufig gestellte Fragen zu Windows erstellt in das Modell zur Bereitstellung von Ressourcenmanager mit Azure-virtuellen Computern an. Die Linux-Version dieses Themas finden Sie unter [häufig gestellte Fragen zu Linux virtuellen Computern](virtual-machines-linux-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Was kann ich eine Azure-virtuellen Computers werden ausgeführt?

Alle Abonnenten können Server-Software auf eine Azure-virtuellen Computern ausgeführt werden. Informationen zu den Supportrichtlinien für die Ausführung von Microsoft-Server-Software in Azure finden Sie unter [Microsoft-Server-Software Unterstützung für Azure virtuellen Computern](https://support.microsoft.com/kb/2721672)

Bestimmte Versionen von Windows 7 und Windows 8.1 stehen MSDN Azure Vorteile und MSDN-Entwickler und Testen nutzungsbasierte Abonnenten, für die Entwicklung und Test Aufgaben. Anweisungen und Einschränkungen, einschließlich Details finden Sie unter [Windows-Client-Bilder für MSDN-Abonnenten](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/). 


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Wie viel Speicher kann ich mit einem virtuellen Computer verwenden?

Jeder Datenträger kann bis zu 1 TB sein. Die Anzahl der Datenträger von Daten, die Sie verwenden können, hängt von der Größe des virtuellen Computers. Weitere Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-windows-sizes.md).

Ein Konto Azure-Speicher bietet Speicherplatz für den Datenträger Betriebssystem und alle Datenlaufwerke an. Jeder Datenträger ist eine .vhd-Datei als Seitenblob gespeichert. Preise Details, finden Sie unter [Speicher Preise Details](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Wie kann ich meine virtuellen Computers zugreifen?

Richten Sie eine Verbindung mithilfe von Remote Desktop-Verbindung (RDP) für einen Windows-virtuellen. Anweisungen finden Sie unter [Verbinden, und melden Sie sich bei einer Azure-virtuellen Computern unter Windows](virtual-machines-windows-connect-logon.md). Bis zu zwei gleichzeitigen Verbindungen werden unterstützt, es sei denn, der Server als Host Sitzung Remote Desktop Services konfiguriert ist.  


Wenn Sie Probleme mit Remotedesktop Probleme auftreten, finden Sie unter [Behandeln von Problemen mit Remote Desktop-Verbindungen zu einem Windows-basierten Azure virtuellen Computern](virtual-machines-windows-troubleshoot-rdp-connection.md). 

Wenn Sie Hyper-V vertraut sind, suchen Sie möglicherweise für eine ähnliche VMConnect Tool. Azure bietet kein ähnliches Tool, da Console-Zugriff auf einen virtuellen Computer nicht unterstützt wird.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Kann ich den temporären Datenträger (Laufwerk D: standardmäßig) zum Speichern von Daten verwenden?

Verwenden Sie keine des temporären Datenträgers zum Speichern von Daten. Es ist jedoch nur temporäre Speicherung, damit Sie riskieren würde Verlust von Daten, die nicht wiederhergestellt werden können. Datenverlust kann auftreten, wenn des virtuellen Computers zu einem anderen Host bewegt wird. Ändern der Größe eines virtuellen Computers, sind Aktualisieren der Host oder ein Hardwarefehler auf dem Host einige Gründe, die einen virtuellen Computer verschieben können.

Wenn Sie eine Anwendung, die haben mit den Buchstaben des Laufwerks D: muss, können Sie Laufwerkbuchstaben zuweisen, damit der temporäre Datenträger einen anderen Wert als D: verwendet. Anweisungen finden Sie unter [Ändern Sie den Buchstaben des dem temporären Windows-Datenträger](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Wie kann ich den Laufwerkbuchstaben des temporären Datenträgers ändern?

Sie können den Buchstaben des Laufwerks ändern, indem Sie die Seite Datei verschieben und Neuzuweisen Laufwerkbuchstaben, aber Sie benötigen, um sicherzustellen, dass Sie die Schritte in einer bestimmten Reihenfolge ausführen. Anweisungen finden Sie unter [Ändern Sie den Buchstaben des dem temporären Windows-Datenträger](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Kann ich eine Sammlung Verfügbarkeit einer vorhandenen virtuellen Computer hinzufügen?

Nein. Wenn Sie Ihre virtuellen Computer Teil einer Verfügbarkeit festlegen möchten, müssen Sie den virtuellen Computer in der Gruppe erstellen. Gibt es zurzeit keine Möglichkeit zum Hinzufügen eines virtuellen Computers zu einer Verfügbarkeit festgelegt, nachdem er erstellt wurde.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Kann ich eine virtuellen Computern Azure hochladen?

Ja. Anweisungen finden Sie unter [Hochladen eines Bilds virtuellen Windows-Computer zu Azure](virtual-machines-windows-upload-image.md)

## <a name="can-i-resize-the-os-disk"></a>Kann ich den Datenträger OS Breite ändern?

Ja. Anweisungen finden Sie unter [wie das Laufwerk OS eines virtuellen Computers in einer Ressourcengruppe Azure zu erweitern](virtual-machines-windows-expand-os-disk.md).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kann ich kopieren oder Klonen einer vorhandenen Azure-virtuellen Computer?

Ja. Anweisungen finden Sie unter [So erstellen Sie eine Kopie in einem Windows-Computer im Bereitstellungsmodell Ressourcenmanager](virtual-machines-windows-vhd-copy.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Warum werden Kanada Mittel- und Kanada Osten Regionen bis Azure Ressourcenmanager nicht angezeigt?

Die beiden neuen Bereiche Kanada Mittel- und Kanada Osten sind nicht automatisch für die Erstellung von virtuellen Computern für vorhandene Azure-Abonnements registriert. Diese Registrierung wird automatisch ausgeführt, wenn über das Azure-Portal an eine andere Region Ressourcenmanager Azure mithilfe ein virtuellen Computers bereitgestellt wird. Nach der Bereitstellung eines virtuellen Computers zu einem beliebigen anderen Azure Region, sollte die neuen Regionen nachfolgenden virtuellen Computern zur Verfügung stehen.

## <a name="does-azure-support-linux-vms"></a>Unterstützt Azure Linux virtuellen Computern?

Ja. Um eine Linux VM zum Testen zu erstellen, finden Sie unter [Erstellen eines Linux virtuellen Computers auf das Portal mit Azure](virtual-machines-linux-quick-create-portal.md).

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Kann ich einen Netzwerkadapter zu hinzufügen meiner virtuellen Computer nach der Erstellung?

Nein. Hinzufügen von einen Netzwerkadapter kann nur zum Zeitpunkt der Erstellung vorgenommen werden.

## <a name="are-there-any-computer-name-requirements"></a>Gibt es eine beliebige Computer Namen Anforderungen?

Ja. Der Name des Computers kann maximal 15 Zeichen lang sein. Weitere Informationen, um die Benennung von Ressourcen finden Sie unter [Richtlinien für Infrastruktur](virtual-machines-windows-infrastructure-naming-guidelines.md) .

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Was sind die Anforderungen Benutzername beim Erstellen eines virtuellen Computers?

Benutzernamen kann maximal 20 Zeichen lang sein und kann nicht mit einem Punkt enden ("."). 

Die folgenden Benutzernamen sind nicht zulässig:

<table>
    <tr>
        <td style="text-align:center">Administrator </td><td style="text-align:center"> Administrator </td><td style="text-align:center"> Benutzer </td><td style="text-align:center"> Benutzer1</td>
    </tr>
    <tr>
        <td style="text-align:center">Testen </td><td style="text-align:center"> Benutzer2 </td><td style="text-align:center"> Test1 </td><td style="text-align:center"> Benutzer3</td>
    </tr>
    <tr>
        <td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> ein</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> ADM </td><td style="text-align:center"> Admin2 </td><td style="text-align:center"> ASPNET</td>
    </tr>
    <tr>
        <td style="text-align:center">Sicherung </td><td style="text-align:center"> Konsole </td><td style="text-align:center"> David </td><td style="text-align:center"> Gast</td>
    </tr>
    <tr>
        <td style="text-align:center">Johann </td><td style="text-align:center"> Besitzer </td><td style="text-align:center"> Quadratwurzel </td><td style="text-align:center"> Server</td>
    </tr>
    <tr>
        <td style="text-align:center">SQL </td><td style="text-align:center"> Support </td><td style="text-align:center"> Support_388945a0 </td><td style="text-align:center"> Sys</td>
    </tr>
    <tr>
        <td style="text-align:center">Test2 </td><td style="text-align:center"> . 3 </td><td style="text-align:center"> User4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>

## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Was sind die Anforderungen Kennwort beim Erstellen eines virtuellen Computers?

Kennwörter müssen 8-123 Zeichen lang sein und 3 aus den folgenden 4 Komplexität Anforderungen entsprechen:

- Untere Zeichen haben
- Haben Sie die obere Zeichen
- Haben Sie eine Ziffer
- Haben Sie ein Sonderzeichen (Regex übereinstimmen [\W_])

Die folgenden Kennwörter sind nicht zulässig:

<table>
    <tr>
        <td style="text-align:center">abc@123</td><td style="text-align:center">P@$$w0rd</td><td style="text-align:center">P@ssw0rd</td><td style="text-align:center">P@ssword123</td><td style="text-align:center">PA$ $word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td><td style="text-align:center">Kennwort!</td><td style="text-align:center">Kennwort1</td><td style="text-align:center">Password22</td><td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
