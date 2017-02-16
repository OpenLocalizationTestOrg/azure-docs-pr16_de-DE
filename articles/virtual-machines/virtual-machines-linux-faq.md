<properties
    pageTitle="Häufig gestellte Fragen zur Linux virtuellen Computern | Microsoft Azure"
    description="Bietet Antworten auf einige häufige Fragen zum virtuelle Linux-Computer mit dem Modell Ressourcenmanager erstellt."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Häufig gestellte Fragen zu Linux virtuellen Computern

Dieser Artikel behandelt einige häufig gestellte Fragen zu Linux erstellt in das Modell zur Bereitstellung von Ressourcenmanager mit Azure-virtuellen Computern an. Die Windows-Version dieses Themas finden Sie unter [häufig gestellte Fragen zu Windows virtuellen Computern](virtual-machines-windows-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Was kann ich eine Azure-virtuellen Computers werden ausgeführt?

Alle Abonnenten können Server-Software auf eine Azure-virtuellen Computern ausgeführt werden. Weitere Informationen finden Sie unter [Linux auf Azure-Endorsed Verteilung](virtual-machines-linux-endorsed-distros.md)


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Wie viel Speicher kann ich mit einem virtuellen Computer verwenden?

Jeder Datenträger kann bis zu 1 TB sein. Die Anzahl der Datenträger von Daten, die Sie verwenden können, hängt von der Größe des virtuellen Computers. Weitere Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).

Ein Konto Azure-Speicher bietet Speicherplatz für den Datenträger Betriebssystem und alle Datenlaufwerke an. Jeder Datenträger ist eine .vhd-Datei als Seitenblob gespeichert. Preise Details, finden Sie unter [Speicher Preise Details](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Wie kann ich meine virtuellen Computers zugreifen?

Richten Sie eine Verbindung zum Anmelden bei der virtuellen Computern, die mithilfe von Secure Shell (SSH). Lesen Sie die Anleitungen zum Verbinden [von Windows](virtual-machines-linux-ssh-from-windows.md) oder [Linux und Mac](virtual-machines-linux-mac-create-ssh-keys.md). Standardmäßig kann SSH bis zu 10 gleichzeitige Zugriffe. Sie können diese Nummer erhöhen, indem Sie die Konfigurationsdatei bearbeiten.


Wenn Probleme auftreten, lesen Sie [Behandeln von Problemen mit Secure Shell (SSH) Verbindungen](virtual-machines-linux-troubleshoot-ssh-connection.md).


## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Kann ich den temporären Datenträger (/ Entwicklung/sdb1) zum Speichern von Daten verwenden?

Verwenden Sie keine des temporären Datenträgers (/ Entwicklung/sdb1) zum Speichern von Daten. Es bestehen nur für die temporäre Speicherung. Sie riskieren, verlieren Daten, die nicht wiederhergestellt werden können.


## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kann ich kopieren oder Klonen einer vorhandenen Azure-virtuellen Computer?

Ja. Anweisungen finden Sie unter [So erstellen Sie eine Kopie eines Linux virtuellen Computers im Bereitstellungsmodell Ressourcenmanager](virtual-machines-linux-copy-vm.md).


## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Warum werden Kanada Mittel- und Kanada Osten Regionen bis Azure Ressourcenmanager nicht angezeigt?

Die beiden neuen Bereiche Kanada Mittel- und Kanada Osten sind nicht automatisch für die Erstellung von virtuellen Computern für vorhandene Azure-Abonnements registriert. Diese Registrierung wird automatisch ausgeführt, wenn über das Azure-Portal an eine andere Region Ressourcenmanager Azure mithilfe ein virtuellen Computers bereitgestellt wird. Nach der Bereitstellung eines virtuellen Computers zu einem beliebigen anderen Azure Region, sollte die neuen Regionen nachfolgenden virtuellen Computern zur Verfügung stehen.


## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Kann ich einen Netzwerkadapter zu hinzufügen meiner virtuellen Computer nach der Erstellung?

Nein. Hinzufügen von einen Netzwerkadapter kann nur zum Zeitpunkt der Erstellung vorgenommen werden.


## <a name="are-there-any-computer-name-requirements"></a>Gibt es eine beliebige Computer Namen Anforderungen?

Ja. Der Name des Computers kann maximal 64 Zeichen lang sein. Weitere Informationen, um die Benennung von Ressourcen finden Sie unter [Richtlinien für Infrastruktur](virtual-machines-linux-infrastructure-naming-guidelines.md) .


## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Was sind die Anforderungen Benutzername beim Erstellen eines virtuellen Computers?

Benutzernamen müssen 1 bis 64 Zeichen lang sein.

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

Kennwörter müssen 6-72 Zeichen lang sein und 3 aus den folgenden 4 Komplexität Anforderungen entsprechen:

- Untere Zeichen haben
- Haben Sie die obere Zeichen
- Haben Sie eine Ziffer
- Haben Sie ein Sonderzeichen (Regex übereinstimmen [\W_])

Die folgenden Kennwörter sind nicht zulässig:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">PA$ $word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Kennwort!</td>
        <td style="text-align:center">Kennwort1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
