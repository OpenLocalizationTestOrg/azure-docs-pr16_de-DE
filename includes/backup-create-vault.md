## <a name="create-a-backup-vault"></a>Erstellen einer Sicherungskopie Tresor
Zum Sichern von Dateien und Daten von Windows Server oder Data Protection Manager (DPM) Azure oder beim IaaS virtuellen Computern in Azure sichern, müssen Sie eine Sicherungskopie Tresor in der geografische Region erstellen, in dem die Daten gespeichert werden sollen.

Die folgenden Schritte führt Sie durch die Erstellung der Tresor zum Speichern von Sicherungskopien verwendet.

1. Melden Sie sich bei dem [Verwaltungsportal](https://manage.windowsazure.com/)
2. Klicken Sie auf **neu** > **Data Services** > **Wiederherstellung Services** > **Sicherung Tresor** , und wählen Sie die **Symbolleiste zu erstellen**.

    ![Erstellen von Tresor](./media/backup-create-vault/createvault1.png)

3. Geben Sie einen Anzeigenamen ein, um die Sicherungsdatei Tresor zu identifizieren, für **den Parameter** . Dies muss für jedes Abonnement eindeutig sein.

4. Wählen Sie für den Parameter **Region** das geografische Region für die Sicherungsdatei Tresor aus. Die Auswahl bestimmt die geografische Region, die die Sicherung Daten gesendet werden. Eine geografische Region in der Nähe Ihres Standorts die Option auswählen, können Sie die Netzwerkwartezeit beim Sichern in Azure verringern.

5. Klicken Sie auf **Tresor erstellen** , um den Workflow abzuschließen. Es dauert eine Weile für die Sicherungsdatei Tresor erstellt werden. Um den Status zu überprüfen, können Sie die Benachrichtigungen am unteren Rand des Portals überwachen.

    ![Erstellen von Tresor](./media/backup-create-vault/creatingvault1.png)

6. Nachdem der Sicherungsdatei Tresor erstellt wurde, wird eine Meldung angezeigt, dass der Tresor erfolgreich erstellt wurde. Der Tresor wird auch als **aktiv**in den Ressourcen für Wiederherstellung Dienste aufgeführt.

    ![Erstellen von Tresor status](./media/backup-create-vault/backupvaultstatus1.png)


### <a name="azure-backup---storage-redundancy-options"></a>Azure Sicherung - Redundanz Speicheroptionen

>[AZURE.IMPORTANT] Der beste Zeitpunkt zum Identifizieren der Speicher Redundanz Option ist rechts nach der Erstellung der Tresor und bevor alle Computer zum Tresor registriert sind. Nachdem Sie ein Element zum Tresor registriert wurde, wird die Option Speicher Redundanz gesperrt ist und kann nicht geändert werden.

Ihr Unternehmen benötigt sollten die Speicherredundanz die Sicherung Azure Back-End-Speicher ermitteln. Wenn Sie als einen Endpunkt primären Sicherung Speicher Azure verwenden (z. B. Sichern in Azure aus einem Windows-Server), sollten Sie Geo redundante Speicheroption auswählen (die Standardeinstellung). Dies ist unter der Option zum **Konfigurieren** von Ihrem Tresor Sicherung angezeigt.

![GRS](./media/backup-create-vault/grs.png)

#### <a name="geo-redundant-storage-grs"></a>Geo redundante Speicher (GRS)
GRS unterhält sechs Kopien Ihrer Daten. Mit GRS Ihre Daten werden dreimal innerhalb der primären Region repliziert und dreimal werden auch in einer sekundären Region Hunderte von Meilen von der primären Region, Bereitstellen von der höchsten Ebene Zuverlässigkeit repliziert. Bei einem Fehler bei der primären Region, durch das Speichern von Daten in GRS Azure Sicherung werden sichergestellt, dass Ihre Daten dauerhaften in zwei separaten Regionen.

#### <a name="locally-redundant-storage-lrs"></a>Lokal redundante Speicher (LRS)
Lokal redundante Speicher (LRS) verwaltet drei Kopien Ihrer Daten. LRS wird dreimal innerhalb einer einzelnen Einrichtung in einem einzigen Bereich repliziert. LRS Datenschutz Ihrer normalen Hardware-Fehler, jedoch nicht von der Ausfall eine gesamte Azure Einrichtung.

Wenn Sie als einen Endpunkt dritten Sicherungsdatei Speicher Azure verwenden (z. B. sind Sie SCDPM verwenden, haben Sie eine lokale Sicherung lokalen & mithilfe von Azure für Ihre langfristig Aufbewahrung muss kopieren), sollten Sie lokal redundante Speicher über die Option **Konfigurieren** von Ihrem Tresor Sicherung auswählen. Dadurch wird unten die Kosten des Speicherns von Daten in Azure, während Sie eine niedrigere Ebene mit Zuverlässigkeit für Ihre Daten, die möglicherweise für Dritte Kopien zulässig.

![LRS](./media/backup-create-vault/lrs.png)
