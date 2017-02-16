<properties
    pageTitle="Behandeln von Problemen mit Azure-virtuellen Computern Sicherung | Microsoft Azure"
    description="Behandeln von Problemen mit sichern und Wiederherstellen von Azure-virtuellen Computern"
    services="backup"
    documentationCenter=""
    authors="trinadhk"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="trinadhk;jimpark;"/>


# <a name="troubleshoot-azure-virtual-machine-backup"></a>Problembehebung bei der Sicherung von Azure-virtuellen Computern

> [AZURE.SELECTOR]
- [Wiederherstellung Services Tresor](backup-azure-vms-troubleshoot.md)
- [Zusätzliche Tresor](backup-azure-vms-troubleshoot-classic.md)

Sie können Fehler auftreten, während mit Azure Sicherung mit Informationen in der folgenden Tabelle aufgeführten beheben:

## <a name="discovery"></a>Suche

| Sicherung | Fehlerdetails | Dieses Problem zu umgehen |
| -------- | -------- | -------|
| Suche | Fehler beim neue Elemente – Microsoft Azure Sicherung festgestellt und interner Fehler zu ermitteln. Warten Sie einige Minuten, und versuchen Sie es dann erneut. | Wiederholen Sie den Erkennungsvorgang nach 15 Minuten.
| Suche | Fehler beim Entdecken Sie neue Elemente – wird eine andere Erkennungsvorgang bereits durchgeführt. Warten Sie, bis der aktuellen Erkennungsvorgang durchgeführt wurde. | Keine |

## <a name="register"></a>Registrieren Sie sich
| Sicherung | Fehlerdetails | Dieses Problem zu umgehen |
| -------- | -------- | -------|
| Registrieren Sie sich | Überschritten Sie Anzahl Daten Datenträger angefügter des virtuellen Computers die unterstützten – Bitte trennen Sie einige Datenträger Daten auf dieses virtuellen Computers zu, und wiederholen Sie den Vorgang. Azure Sicherung unterstützt bis zu 16 mit einer Azure-virtuellen Computern für die Sicherung verbundenen Daten-Datenträger | Keine |
| Registrieren Sie sich | Microsoft Azure Sicherung Fehler interner - warten Sie einige Minuten, und wiederholen Sie den Vorgang erneut aus. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support. | Sie können diesen Fehler wegen einer der folgenden nicht unterstützte Konfiguration des virtuellen Computer Premium LRS können. <br> Premium Speicher virtuellen Computern kann mithilfe der Wiederherstellung Services Tresor gesichert werden. [Weitere Informationen](backup-introduction-to-azure-backup.md/#back-up-and-restore-premium-storage-vms) |
| Registrieren Sie sich | Fehler bei der Registrierung mit Timeout Agent installieren des Vorgangs | Überprüfen Sie, ob die Version des Betriebssystems des virtuellen Computers unterstützt wird. |
| Registrieren Sie sich | Befehl Ausführen des fehlgeschlagen - ist ein anderer Vorgang in Bearbeitung für dieses Element ein. Warten Sie, bis der vorherige Vorgang abgeschlossen ist | Keine |
| Registrieren Sie sich | Virtuellen Computern Probleme virtuellen Festplatten auf Premium Storage gespeichert sind für die Sicherung nicht unterstützt. | Keine |
| Registrieren Sie sich | Virtuellen Computern-Agent ist nicht auf dem virtuellen Computer vorhanden – installieren Sie die obligatorische Voraussetzung virtueller Computer-Agent, und starten Sie den erneut. | [Weitere Informationen finden Sie](#vm-agent) Informationen zu virtuellen Computer Agent-Installation, und wie Sie die Installation des virtuellen Computer-Agents überprüfen. |

## <a name="backup"></a>Sicherung

| Sicherung | Fehlerdetails | Dieses Problem zu umgehen |
| -------- | -------- | -------|
| Sicherung | Konnte nicht mit dem virtuellen Computer Agent Antrag auf Snapshot kommunizieren. Timeout bei Momentaufnahme virtueller Computer Sub-Vorgang. – Bitte finden Sie den Leitfaden zur Problembehandlung zur Lösung des Problems. | Dieser Fehler wird ausgelöst, wenn es ein Problem mit der Agent virtueller Computer liegt oder Netzwerkzugriff auf die Azure Infrastruktur in irgendeiner Weise blockiert. Weitere Informationen über das [Debuggen von Momentaufnahme eines virtuellen Computers Probleme](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Wenn der Agent virtueller Computer nicht Probleme verursacht, starten Sie den virtuellen Computer neu. Manchmal kann ein falscher Zustand für virtuelle Computer Probleme verursachen und einen Neustart von den virtuellen Computer werden diese "schlechten Zustand" zurückgesetzt. |
| Sicherung | Fehler bei der Sicherung mit interner Fehler: Führen Sie den Vorgang in wenigen Minuten erneut. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support | Überprüfen Sie, ob in den Zugriff auf virtuellen Computer Speicher ein vorübergehendes Problem vorliegt. Überprüfen Sie [Azure Status](https://azure.microsoft.com/en-us/status/) , um festzustellen, ob alle laufenden Problem im Zusammenhang mit berechnen/Speicher/Netzwerk in der Region ein. Bitte wird "Wiederholen" die Sicherung Beitrag Problem verringert. |
| Sicherung | Der Vorgang konnte nicht ausgeführt werden, wie virtueller Computer nicht mehr vorhanden sind. | Sicherung kann nicht ausgeführt werden, wie der virtuellen Computer so konfiguriert, dass für die Sicherung gelöscht wurden. Beenden Sie weitere Sicherungskopien, indem Sie in der Ansicht der geschützten Elemente zu wechseln, wählen Sie geschütztes Element aus, und klicken Sie auf Schutz aufheben. Sie können Daten beibehalten, indem Sie die Option Daten beibehalten Sicherung auswählen. Sie können für diesen virtuellen Computer durch Klicken auf Aktivieren konfigurieren Schutz von Elementen registriert Ansicht später Schutz fortsetzen|
| Sicherung | Konnte nicht installiert werden die Azure Wiederherstellung Dienste ist Erweiterung für ausgewähltes Element - Agent virtueller Computer eine Vorbedingung für Azure Wiederherstellung Erweiterung aus. Installieren Sie den Agent Azure-virtuellen Computer, und starten Sie den erneut Registrierung | <ol> <li>Überprüfen Sie, ob der virtuellen Computer-Agent ordnungsgemäß installiert wurde. <li>Stellen Sie sicher, dass die Kennzeichnung auf virtuellen Computer Config richtig eingestellt ist.</ol> [Weitere Informationen finden Sie](#validating-vm-agent-installation) Informationen zu virtuellen Computer Agent-Installation, und wie Sie die Installation des virtuellen Computer-Agents überprüfen. |
| Sicherung | Befehl Ausführen des fehlgeschlagen - gibt es einen anderen Vorgang zurzeit in Bearbeitung für diesen Artikel. Warten Sie, bis der vorherige Vorgang abgeschlossen ist, und wiederholen Sie dann | Eine vorhandene Sicherung oder Wiederherstellungsauftrag für den virtuellen Computer ausgeführt wird und ein neues Projekt kann nicht gestartet werden, während der vorhandene Auftrag ausgeführt wird. |
| Sicherung | Erweiterung Fehler bei der Installation der Fehler "COM + konnte keine Verbindung mit dem Microsoft Distributed Transaktionskoordinator sprechen. | Dies bedeutet normalerweise, dass der Dienst COM + nicht ausgeführt wird. Hilfe zum Beheben dieses Problems wenden Sie sich an Microsoft Support. |
| Sicherung | Snapshot-Vorgang Fehler bei der Vorgang VSS "dieses Laufwerk durch BitLocker Drive Verschlüsselung gesperrt ist. Sie müssen dieses Laufwerk aus Control Panel entsperren. | BitLocker für alle Laufwerke des virtuellen Computers zu deaktivieren Sie, und prüfen Sie, ob das VSS Problem gelöst ist |
| Sicherung | Virtuellen Computern Probleme virtuellen Festplatten auf Premium Storage gespeichert sind für die Sicherung nicht unterstützt. | Keine |
| Sicherung | Azure-virtuellen Computern nicht gefunden. | Dies geschieht, wenn der primäre virtueller Computer wird gelöscht, aber die Sicherung Richtlinie eines virtuellen Computers weiterhin zu Sicherungskopie Suchen nach. Um diesen Fehler zu beheben: <ol><li>Erstellen Sie den virtuellen Computer mit demselben Namen und derselben Gruppe Ressourcenname [Name der Cloud-Dienst], neu. <br>(ODER) <li> Deaktivieren Sie den Schutz für diese virtuellen Computer, sodass die nachfolgende Sicherungen nicht ausgelöst werden. </ol> |
| Sicherung | Virtuellen Computern-Agent ist nicht auf dem virtuellen Computer vorhanden – installieren Sie die obligatorische Voraussetzung virtueller Computer-Agent, und starten Sie den erneut. | [Weitere Informationen finden Sie](#vm-agent) Informationen zu virtuellen Computer Agent-Installation, und wie Sie die Installation des virtuellen Computer-Agents überprüfen. |

## <a name="jobs"></a>Aufträge
| Vorgang | Fehlerdetails | Dieses Problem zu umgehen |
| -------- | -------- | -------|
| Auftrag abbrechen | Absage wird für diesen Auftrag nicht unterstützt: warten, bis der Auftrag abgeschlossen ist. | Keine |
| Auftrag abbrechen | Die Position ist nicht in einem Zustand abgebrochen werden: warten, bis der Auftrag abgeschlossen ist. <br>ODER<br> Der ausgewählte Auftrag ist nicht in einem Zustand abgebrochen werden: Warten Sie für das Projekt abgeschlossen.| Aller Wahrscheinlichkeit ist der Job fast abgeschlossen; Warten Sie, bis Auftragsabschluss |
| Auftrag abbrechen | Abbrechen des Auftrags kann nicht, da es nicht in den Fortschritt ist – Absage unterstützt nur für Projekte, die ausgeführt werden. Bitte Versuch Abbrechen auf in Bearbeitung befindlichen Position. | In diesem Fall aufgrund einer vorübergehendes Zustand. Warten Sie eine Minute, und wiederholen Sie den Vorgang abbrechen |
| Auftrag abbrechen | Nicht den Auftrag abbrechen - warten Sie, bis das Projekt abgeschlossen ist. | Keine |


## <a name="restore"></a>Stellen Sie wieder her
| Vorgang | Fehlerdetails | Dieses Problem zu umgehen |
| -------- | -------- | -------|
| Stellen Sie wieder her | Fehler bei der Wiederherstellung mit Cloud interner Fehler | <ol><li>Cloud-Dienst, dem Sie wiederherstellen möchten, ist mit DNS-Einstellungen konfiguriert. Sie können überprüfen <br>$deployment = "ServiceName" Get-AzureDeployment - ServiceName-Slot "Herstellung" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Ist Adresse, die so konfiguriert ist, bedeutet dies, dass die DNS-Einstellungen konfiguriert sind.<br> <li>Cloud-Dienst, in die Sie wiederherstellen möchten, die mit reservierte IP-Adressen konfiguriert ist und vorhandene virtuellen Computern in der Cloud-Dienst Zustand beendet werden.<br>Sie können ein Clouddienst IP reserviert hat, mithilfe der folgenden Powershell-Cmdlets prüfen:<br>$deployment = "Servicename" Get-AzureDeployment - ServiceName-Slot "Herstellung" $ Datenausführungsverhinderung. ReservedIPName <br><li>Sie versuchen, einen virtuellen Computer mit folgenden Inhalte Netzwerkkonfigurationen in in derselben Cloud-Service wiederherstellen. <br>-Virtuellen Computern unter Laden Lastenausgleich Konfiguration (interne und externe)<br>-Virtuellen Computern mit mehreren reservierte IP-Adressen<br>-Virtuellen Computern mit mehreren NICs<br>Wählen Sie einen neuen Clouddienst in der Benutzeroberfläche oder Näheres [Wiederherstellen Aspekte](./backup-azure-restore-vms.md/#restoring-vms-with-special-network-configurations) für virtuelle Computer mit speziellen Netzwerkkonfigurationen</ol> |
| Stellen Sie wieder her | Der ausgewählte DNS-Name wird bereits verwendet – Geben Sie an einen anderen DNS-Namen, und versuchen Sie es erneut. | Hier der DNS-Name bezieht sich auf den Namen der Cloud-Dienst (in der Regel, die mit. cloudapp.net). Dies muss eindeutig sein. Wenn dieser Fehler auftreten, müssen Sie einen anderen Namen des virtuellen Computers während der Wiederherstellung auswählen. <br><br> Dieser Fehler wird nur für Benutzer von der Azure-Portal angezeigt. Die Wiederherstellung über PowerShell erfolgreich ist, da es nur die Datenträger stellt und nicht den virtuellen Computer erstellen. Der Fehler wird konfrontiert werden, wenn Sie der virtuellen Computer explizit von Ihnen erstellt wurde, nachdem Sie den Datenträger wiederherstellen, Vorgang. |
| Stellen Sie wieder her | Die angegebene virtuelle Netzwerkkonfiguration ist nicht korrekt – Geben Sie an einer anderen virtuellen Netzwerkkonfiguration, und versuchen Sie es erneut. | Keine |
| Stellen Sie wieder her | Angegebenen Cloud-Dienst ist eine reservierte IP-Adresse verwenden, die nicht mit der Konfiguration des virtuellen Computers wiederherzustellende entsprechen – Geben Sie einen anderen Cloud-Service, der nicht reservierte IP verwendet wird, oder wählen Sie aus einer anderen Wiederherstellung, zeigen Sie auf Wiederherstellen aus. | Keine |
| Stellen Sie wieder her | Cloud-Dienst Grenzwert für die Anzahl der Eingabewerte Endpunkte erreicht hat: durch Angeben von einem anderen Cloud-Dienst oder mithilfe eines vorhandenen Endpunkts, wiederholen Sie den Vorgang. | Keine |
| Stellen Sie wieder her | Zusätzliche Tresor und das Ziel Speicher sind in zwei verschiedenen Bereichen: sicherstellen, dass das in der Wiederherstellung angegebene Speicherkonto in der gleichen Azure Region als die Sicherung Tresor ist. | Keine |
| Stellen Sie wieder her | Speicher-Konto für angegebene bei der Wiederherstellung nicht unterstützt - Basic/Standard Speicherkonten mit lokal redundante oder Geo redundante Replikation Einstellungen werden unterstützt. Wählen Sie einen unterstützten Speicher-Konto | Keine |
| Stellen Sie wieder her | Speicher Kontotyp für Wiederherstellungsvorgangs angegeben ist nicht online – stellen Sie sicher, dass das in der Wiederherstellung angegebene Speicherkonto online ist | Dies kann aufgrund einer vorübergehenden Fehler in Azure-Speicher oder aufgrund von einem Ausfall vorkommen. Wählen Sie ein anderes Speicherkonto an. |
| Stellen Sie wieder her | Ressourcengruppe Kontingents erreicht wurde – einige Ressourcengruppen aus Azure-Portal löschen, oder wenden Sie sich an Azure Support, um die Grenzwerte zu vergrößern. | Keine |
| Stellen Sie wieder her | Ausgewählten Subnetz nicht vorhanden - wählen Sie ein Subnetz das vorhanden ist | Keine |


## <a name="policy"></a>Richtlinie
| Vorgang | Fehlerdetails | Dieses Problem zu umgehen |
| -------- | -------- | -------|
| Erstellen der Richtlinie | Fehler beim Erstellen der Richtlinie – reduzieren Sie die Aufbewahrung möglichen Richtlinienkonfiguration fortzusetzen. | Keine |


## <a name="vm-agent"></a>Virtueller Computer-Agents

### <a name="setting-up-the-vm-agent"></a>Einrichten des virtuellen Computer-Agents
In der Regel, die virtuellen Computer-Agent ist bereits in virtuellen Computern, die aus dem Katalog Azure erstellt werden. Virtuellen Computern, die von lokalen Rechenzentren migriert werden müssten allerdings nicht des virtuellen Computer-Agents installiert. Für diese virtuelle Computer muss der Agent virtueller Computer explizit installiert werden. Weitere Informationen zum [Installieren des virtuellen Computer-Agents eines vorhandenen virtuellen Computers](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Für Windows-virtuellen Computern:

- Herunterladen Sie und installieren Sie die [MSI-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Sie benötigen Administratorrechte, um die Installation durchzuführen.
- [Die Eigenschaft virtueller Computer zu aktualisieren](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) , um anzugeben, dass der Agent installiert ist.

Für Linux virtuelle Computer:

- Installieren Sie neueste [Linux Agent](https://github.com/Azure/WALinuxAgent) von Github.
- [Die Eigenschaft virtueller Computer zu aktualisieren](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) , um anzugeben, dass der Agent installiert ist.


### <a name="updating-the-vm-agent"></a>Aktualisieren des virtuellen Computer-Agents
Für Windows-virtuellen Computern:

- Aktualisieren der virtuellen Computer-Agent ist so einfach wie die [virtuellen Computer Agent Binärdateien](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)erneut zu installieren. Jedoch müssen Sie sicherstellen, dass keine zusätzliche Operation ausgeführt wird, während des virtuellen Computer-Agents aktualisiert wird.

Für Linux virtuelle Computer:

- Folgen Sie den Anweisungen auf [Linux virtueller Computer-Agent aktualisieren](../virtual-machines/virtual-machines-linux-update-agent.md).


### <a name="validating-vm-agent-installation"></a>Virtueller Computer Agent Installation wird überprüft.
So aktivieren Sie die virtuellen Computer Agent-Version auf Windows-virtuellen Computern:

1. Melden Sie sich bei der Azure-virtuellen Computern, und navigieren Sie zu dem Ordner *C:\WindowsAzure\Packages*. Suchen Sie die Datei WaAppAgent.exe präsentieren.
2. Mit der rechten Maustaste in der Datei, wechseln Sie zu **Eigenschaften**, und wählen Sie dann auf die Registerkarte **Details** . Das Feld Produktversion sollten 2.6.1198.718 oder höher





