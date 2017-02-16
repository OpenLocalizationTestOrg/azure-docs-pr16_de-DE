<properties
   pageTitle="Azure virtueller Computer Sicherung schlägt fehl: Kommunikation mit dem virtuellen Computer Agent Antrag auf Snapshot - Momentaufnahme virtueller Computer Sub Vorgang Timeout | Microsoft Azure"
   description="Symptome Ursachen und Lösungen für Azure-virtuellen Computer Sicherung Fehlern im Zusammenhang mit konnte nicht mit dem virtuellen Computer Agent Antrag auf Snapshot kommunizieren. Snapshot virtueller Computer Sub Vorgang Zeitlimit zurück"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="cfreeman"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jimpark; markgal;genli"/>

# <a name="azure-vm-backup-fails-could-not-communicate-with-the-vm-agent-for-snapshot-status---snapshot-vm-sub-task-timed-out"></a>Azure virtueller Computer Sicherung schlägt fehl: Kommunikation mit dem virtuellen Computer Agent Antrag auf Snapshot – Timeout Momentaufnahme virtueller Computer Sub-Aufgabe

## <a name="summary"></a>Zusammenfassung

Nach dem registrieren, und Planen eines virtuellen Computern (virtuellen Computers) für die Sicherung Azure, initiiert die Sicherung Azure Service den Auftrag Sicherung zum geplanten Zeitpunkt aus, indem Sie zum Kommunizieren mit der Sicherungsdatei Erweiterung auf dem virtuellen Computer auf einen Point-in-Time Snapshot. Es gibt Bedingungen, die die Momentaufnahme verhindern möglicherweise ausgelöst wurde, die zu einer Sicherung Fehler führt. Dieser Artikel enthält Schritte zur Problembehandlung für Problemen im Zusammenhang mit Azure-virtuellen Computer Sicherung Fehlern im Zusammenhang mit Momentaufnahme Timeoutfehler.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Problem

Microsoft Azure Sicherung für Infrastruktur als Service (IaaS) virtueller Computer schlägt fehl, und die Position Fehlerdetails im [Portal Azure](https://portal.azure.com/)gibt die folgende Fehlermeldung angezeigt:

**Kommunikation mit dem virtuellen Computer Agent Antrag auf Snapshot - Zeitlimit Momentaufnahme virtueller Computer Sub-Vorgang.**

## <a name="cause"></a>Ursache
Es gibt vier häufige Ursachen für diesen Fehler an:

- Der virtuellen Computer keinen Zugriff auf das Internet.
- Der Microsoft Azure-virtuellen Computer-Agent auf dem virtuellen Computer installiert ist veraltet (für Linux virtuellen Computern).
- Die Sicherung Erweiterung schlägt zu aktualisieren, oder laden.
- Der Status Momentaufnahmen kann nicht abgerufen werden, oder die Momentaufnahmen können nicht geöffnet werden.

## <a name="cause-1-the-vm-does-not-have-internet-access"></a>Ursache 1: Der virtuellen Computer kann nicht mit dem Internet zugegriffen werden.
Pro der Anforderung zum Bereitstellung für der virtuellen Computer hat keinen Zugriff Internet oder kann nur direkte, die Zugriff auf die Azure Infrastruktur zu verhindern.

Die Sicherung Erweiterung erfordern die Verbindung zu den Azure öffentlichen IP-Adressen ordnungsgemäß funktioniert. Dies ist, da es Befehle an einen Endpunkt Azure-Speicher (HTTP-URL) zum Verwalten der Momentaufnahmen der den virtuellen Computer sendet. Wenn die Erweiterung keinen Zugriff auf das öffentliche Internet, schlägt die Sicherung später.

### <a name="solution"></a>Lösung
Verwenden Sie in diesem Szenario eine der folgenden Methoden um zu beheben:

- Weißen Azure Datencenter IP-Bereiche
- Erstellen eines Pfads für HTTP-Datenverkehr

### <a name="to-whitelist-the-azure-datacenter-ip-ranges"></a>Zur weißen Liste der Azure wird der Datacenter IP-Bereiche

1. Rufen Sie die [Liste der Azure Datacenter IP-Adressen](https://www.microsoft.com/download/details.aspx?id=41653) um weißen Liste hinzugefügt werden.
2. Aufheben der Blockierung der IP-Adressen mithilfe des Cmdlets New-NetRoute. Führen Sie dieses Cmdlet auf dem Azure-virtuellen Computer in einem erhöhten PowerShell-Fenster (als Administrator ausführen) aus.
3. Hinzufügen von Regeln zum Netzwerk Sicherheit Gruppe (NSG) Wenn Sie den Zugriff auf die IP-Adressen verfügen.

### <a name="to-create-a-path-for-http-traffic-to-flow"></a>Zum Erstellen eines Pfads für HTTP-Datenverkehr

1. Wenn Sie Einschränkungen Network Speicherort (z. B. NSG) haben, stellen Sie einen HTTP-Proxyserver, um den Datenverkehr weiterleiten bereit.
2. Wenn Sie eine Netzwerksicherheitsgruppe (NSG) haben, Hinzufügen von Regeln zum Zugriff auf das Internet aus dem HTTP-Proxy zulassen.

Erfahren Sie, wie [ein HTTP-Proxy nach Sicherungskopien virtueller Computer einrichten](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

## <a name="cause-2-the-microsoft-azure-vm-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Ursache 2: Des Microsoft Azure-virtuellen Computer-Agents auf dem virtuellen Computer installiert ist veraltet (für Linux virtuellen Computern)

### <a name="solution"></a>Lösung
Am häufigsten Agent-bezogene oder Erweiterung-bezogene Fehler für Linux virtuellen Computern sind durch Probleme verursacht, die einen alten virtueller Computer Agent beeinflussen. Als allgemeine Richtlinie sind die ersten Schritte zur Behebung dieses Problems Folgendes:

1. [Installieren den neuesten Azure-virtuellen Computer-Agent](https://github.com/Azure/WALinuxAgent).
2. Stellen Sie sicher, dass der Azure-Agent des virtuellen Computers ausgeführt wird. Führen Sie hierzu den folgenden Befehl aus:```ps -e```

    Wenn dieser Vorgang nicht ausgeführt wird, verwenden Sie die folgenden Befehle neu zu starten.

    Für Ubuntu:```service walinuxagent start```

    Für die anderen Verteilung: '''service Waagent starten
```

3. [Configure the auto restart agent](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).

4. Run a new test backup. If the failures persist, please collect logs from the following folders for further debugging.

    We require the following logs from the customer’s VM:

    - /var/lib/waagent/*.xml
    - /var/log/waagent.log
    - /var/log/azure/*

If we require verbose logging for waagent, follow these steps to enable this:

1. In the /etc/waagent.conf file, locate the following line:

    Enable verbose logging (y|n)

2. Change the **Logs.Verbose** value from n to y.
3. Save the change, and then restart waagent by following the previous steps in this section.

## Cause 3: The backup extension fails to update or load
If extensions cannot be loaded, then Backup fails because a snapshot cannot be taken.

### Solution
For Windows guests:

1. Verify that the iaasvmprovider service is enabled and has a startup type of automatic.
2. If this is not the configuration, enable the service to determine whether the next backup succeeds.

For Linux guests:

The latest version of VMSnapshot Linux (extension used by backup) is 1.0.91.0

If the backup extension still fails to update or load, you can force the VMSnapshot extension to be reloaded by uninstalling the extension. The next backup attempt will reload the extension.

### To uninstall the extension

1. Go to the [Azure portal](https://portal.azure.com/).
2. Locate the particular VM that has backup problems.
3. Click **Settings**.
4. Click **Extensions**.
5. Click **Vmsnapshot Extension**.
6. Click **uninstall**.

This will cause the extension to be reinstalled during the next backup.

## Cause 4: The snapshots status cannot be retrieved or the snapshots cannot be taken
VM backup relies on issuing snapshot command to underlying storage. The backup can fail because it has no access to storage or because of a delay in snapshot task execution.

### Solution
The following conditions can cause snapshot task failure:

| Cause | Solution |
| ----- | ----- |
| VMs that have Microsoft SQL Server Backup configured. By default, VM Backup runs a VSS Full backup on Windows VMs. On VMs that are running SQL Server-based servers and on which SQL Server Backup is configured, snapshot execution delays may occur. | Set following registry key if you are experiencing backup failures because of snapshot issues.<br><br>[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE" |
| VM status is reported incorrectly because the VM is shut down in RDP. If you shut down the virtual machine in RDP, check the portal to determine whether that VM status is reflected correctly. | If it’s not, shut down the VM in the portal by using the ”Shutdown” option in the VM dashboard. |
| Many VMs from the same cloud service are configured to back up at the same time. | It’s a best practice to spread out the VMs from the same cloud service to have different backup schedules. |
| The VM is running at high CPU or memory usage. | If the VM is running at high CPU usage (more than 90 percent) or high memory usage, the snapshot task is queued and delayed and eventually times out. In this situation, try on-demand backup. |
|The VM cannot get host/fabric address from DHCP.|DHCP must be enabled inside the guest for IaaS VM Backup to work.  If the VM cannot get host/fabric address from DHCP response 245, then it cannot download ir run any extensions. If you need a static private IP, you should configure it through the platform. The DHCP option inside the VM should be left enabled. View more information about [Setting a Static Internal Private IP](../virtual-network/virtual-networks-reserved-private-ip.md).|
