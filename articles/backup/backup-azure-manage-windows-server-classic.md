<properties
    pageTitle="Verwalten Azure Sicherung Depots und Servern mithilfe des Modells klassischen Bereitstellung Azure | Microsoft Azure"
    description="Verwenden Sie in diesem Lernprogramm erfahren Sie, wie Depots Azure Sicherung und Server verwalten."
    services="backup"
    documentationCenter=""
    authors="markgalioto"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jimpark;markgal"/>


# <a name="manage-azure-backup-vaults-and-servers-using-the-classic-deployment-model"></a>Verwalten von Azure Sicherung Depots und Servern mithilfe des Modells klassischen Bereitstellung

> [AZURE.SELECTOR]
- [Ressourcenmanager](backup-azure-manage-windows-server.md)
- [Klassische](backup-azure-manage-windows-server-classic.md)

In diesem Artikel finden Sie eine Übersicht über die Sicherung Verwaltungsaufgaben über die klassischen Azure-Portal und dem Microsoft Azure Sicherung Agent verfügbar.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressourcenmanager Modell zur Bereitstellung von.

## <a name="management-portal-tasks"></a>Portal Verwaltungsaufgaben
1. Melden Sie sich bei dem [Verwaltungsportal](https://manage.windowsazure.com).

2. Klicken Sie auf **Dienste Wiederherstellung**, und klicken Sie auf den Namen der Sicherungsdatei Tresor zum Anzeigen der Seite Schnellstart.

    ![Wiederherstellung services](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

Indem Sie die Optionen am oberen Rand der Seite Schnellstart auswählen, können Sie die verfügbaren Verwaltungsaufgaben sehen.

![Verwalten von Registerkarten](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a>Dashboard
Wählen Sie **Dashboard** Übersicht über die Verwendung des Servers angezeigt. **Übersicht über die Verwendung** der umfasst:

- Die Anzahl von Windows-Servern registriert in der cloud
- Die Anzahl der Azure-virtuellen Computern in der Cloud geschützt
- Der Gesamtspeicher in Azure verbraucht
- Der Status des zuletzt verwendete Aufträge

Am unteren Rand des Dashboards können Sie die folgenden Aufgaben ausführen:

- **Verwalten Zertifikat** – Wenn Sie ein Zertifikat zum Registrieren des Servers, und klicken Sie dann verwenden Sie diese Option zum Aktualisieren des Zertifikats verwendet wurde. Wenn Sie Anmeldeinformationen Tresor arbeiten, verwenden Sie nicht **Zertifikat verwalten**.
- **Löschen** : Löscht den aktuellen Sicherung Tresor. Wenn Sie eine Sicherungskopie Tresor nicht mehr verwendet wird, können Sie es zum Freigeben von Speicherplatz löschen. **Löschen** ist nur verfügbar, nachdem alle registrierte Servern aus dem Tresor gelöscht wurden.

![Zusätzliche Dashboard-Aufgaben](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a>Registrierte Elemente
Wählen Sie **Elemente registriert** die Namen der Server anzeigen möchten, die für diesen Tresor registriert sind.

![Registrierte Elemente](./media/backup-azure-manage-windows-server-classic/registered-items.png)

Der **Typ** Filter Standardwerte zu Azure virtuellen Computern an. Wenn Sie die Namen der Server anzeigen, um diesen Tresor registriert haben, wählen Sie **WindowsServer** aus dem Dropdownmenü aus.

Hier können Sie die folgenden Aufgaben ausführen:

- **Zulassen erneuten Registrierung** – Wenn diese Option ausgewählt ist für einen Server können **Registrier-Assistenten** in der lokalen Microsoft Azure Sicherung Agent den Server mit dem Sicherung Tresor ein zweites Mal zu registrieren. Möglicherweise müssen Sie sich wegen eines Fehlers in das Zertifikat erneut registrieren oder einen Server hätten neu erstellt werden.
- **Löschen** : Löscht einen Server aus der Sicherungsdatei Tresor. Alle gespeicherten Daten mit dem Server verbunden sind wird sofort gelöscht.

    ![Erfasste Artikel Aufgaben](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a>Geschützte Elemente
Wählen Sie **Geschützte Elemente** die Elemente anzeigen möchten, die von den Servern gesichert wurden.

![Geschützte Elemente](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a>Konfigurieren

Auf der Registerkarte **Konfigurieren** können Sie die entsprechenden Speicher Redundanz Option auswählen. Der beste Zeitpunkt die Speicher Redundanz Option ist rechts, nach dem Erstellen einer Tresor und bis zu einem beliebigen Computer registriert sind.

>[AZURE.WARNING] Nachdem Sie ein Element zum Tresor registriert wurde, wird die Option Speicher Redundanz gesperrt ist und kann nicht geändert werden.

![Konfigurieren](./media/backup-azure-manage-windows-server-classic/configure.png)

Finden Sie in diesem Artikel Weitere Informationen zu [Speicherredundanz](../storage/storage-redundancy.md).

## <a name="microsoft-azure-backup-agent-tasks"></a>Microsoft Azure Sicherung Agent Aufgaben

### <a name="console"></a>Konsole

Öffnen Sie die **Microsoft Azure Sicherung-Agent** (Sie können sie nach Dateien suchen den Computer *Microsoft Azure Sicherung*).

![Zusätzliche agent](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

Aus am rechten Rand der Sicherungsdatei Agentenkonsole verfügbaren **Aktionen** können Sie die folgenden Verwaltungsaufgaben ausführen:

- Register-Server
- Sicherung planen
- Jetzt sichern.
- Ändern von Eigenschaften

![Agent Console-Aktionen](./media/backup-azure-manage-windows-server-classic/console-actions.png)

>[AZURE.NOTE] Zum **Wiederherstellen von Daten**finden Sie unter [Dateien wiederherstellen in einem WindowsServer oder Windows-Clientcomputer](backup-azure-restore-windows-server.md).

### <a name="modify-an-existing-backup"></a>Ändern Sie eine vorhandene Sicherung

1. Klicken Sie in der Microsoft Azure Sicherung Agent auf **Sicherung planen**.

    ![Planen einer Windows Server-Sicherung](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)

2. Lassen Sie im **Assistenten Zeitplan** der ausgewählten Option **nehmen Änderungen vor, um zusätzliche Elemente oder Uhrzeitwerte** , und klicken Sie auf **Weiter**.

    ![Ändern einer geplanten Sicherung](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)

3. Wenn Sie Elemente hinzufügen oder ändern möchten, klicken Sie auf dem Bildschirm **Zu sichern Elemente auswählen** auf **Elemente hinzufügen**.

    Sie können auch auf dieser Seite des Assistenten **Ausschluss Einstellungen** festlegen. Wenn Sie Dateien ausschließen möchten, oder Dateitypen lesen Sie das Verfahren zum Hinzufügen von [Ausschlusswörterbüchern Einstellungen](#exclusion-settings).

4. Wählen Sie die Dateien und Ordner zu sichern, und klicken Sie auf **OK**.

    ![Hinzufügen von Elementen](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)

5. Geben Sie den **Zeitplan Sicherungsdatei** aus, und klicken Sie auf **Weiter**.

    Sie können (bei einem Maximum von 3 Mal pro Tag) täglich oder wöchentlich Sicherungen planen.

    ![Geben Sie Ihren Terminplan Sicherung](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

    >[AZURE.NOTE] Angeben eines Zeitplans Sicherung wird ausführlich in diesem [Artikel](backup-azure-backup-cloud-as-tape.md)erläutert.

6. Wählen Sie die **Aufbewahrungsrichtlinie** für die Sicherungskopie, und klicken Sie auf **Weiter**.

    ![Wählen Sie aus Ihrer Aufbewahrungsrichtlinie](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)

7. Klicken Sie auf dem Bildschirm **Bestätigung** überprüfen Sie die Informationen, und klicken Sie auf **Fertig stellen**.

8. Nach Beendigung des Assistenten die **Sicherung Zeitplan**erstellen, klicken Sie auf **Schließen**.

    Nach dem Ändern der Schutz, können Sie bestätigen, dass Sicherungskopien ordnungsgemäß auslösen sind, indem Sie auf der Registerkarte **Aufträge** und bestätigt, dass die Änderungen in der Sicherungsdatei Aufträge angezeigt werden.

### <a name="enable-network-throttling"></a>Netzwerk Begrenzungsebene aktivieren  
Der Sicherung von Azure-Agent bietet eine Beschränkung der Registerkarte, die können Sie steuern, wie die Bandbreite während der Datenübertragung verwendet wird. Dieses Steuerelement ist hilfreich, wenn Sie zum Sichern von Daten während der Arbeitszeiten jedoch möchten nicht, dass die Sicherung beeinträchtigen andere Datenverkehr im Internet. Beschränkung der Daten gilt durchstellen zum Sichern und Wiederherstellen von Aktivitäten.  

Um die Einschränkung zu aktivieren:

1. Klicken Sie in der **Sicherung Agent** **Ändern der Eigenschaften**auf.

2. Aktivieren Sie das Kontrollkästchen **Internet Bandbreite Verwendung für zusätzliche Vorgänge begrenzungsebene aktivieren** .

    ![Netzwerk begrenzungsebene](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)

3. Nachdem Sie die begrenzungsebene aktiviert haben, geben Sie die zulässige Bandbreite für die Sicherungsdatei Datenübertragung während der **Arbeitszeiten** und **nicht - Arbeitszeiten**.

    Die Bandbreite Werte beginnen bei 512 KB pro Sekunde (KB/s), und wechseln Sie können bis zu 1023 MB pro Sekunde (MB/s). Können Sie auch bestimmen am Anfang und Ende für **Arbeitszeiten**und welche Tage der Woche als Arbeit Tage. Die Zeit außerhalb der vorgesehenen Arbeitszeiten gilt nicht Arbeitszeiten werden.

4. Klicken Sie auf **OK**.

## <a name="exclusion-settings"></a>Ausschluss-Einstellungen

1. Öffnen Sie die **Microsoft Azure Sicherung-Agent** (Sie können sie nach Dateien suchen den Computer *Microsoft Azure Sicherung*).

    ![Öffnen der Sicherungskopie agent](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

2. Klicken Sie in der Microsoft Azure Sicherung Agent auf **Sicherung planen**.

    ![Planen einer Windows Server-Sicherung](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)

3. Lassen Sie im Terminplan-Assistenten Sicherung der ausgewählten Option **nehmen Änderungen vor, um zusätzliche Elemente oder Zeitangaben** , und klicken Sie auf **Weiter**.

    ![Ändern eines Projektplans](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)

4. Klicken Sie auf **Ausschlüsse**.

    ![Wählen Sie Elemente ausschließen](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)

5. Klicken Sie auf **Ausschluss hinzufügen**.

    ![Ausschlüsse hinzufügen](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)

6. Wählen Sie den Standort aus, und klicken Sie dann auf **OK**.

    ![Wählen Sie einen Speicherort für Ausschluss](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)

7. Fügen Sie im Feld **Dateityp** die Erweiterung hinzu.

    ![Nach Dateityp ausschließen](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    Hinzufügen einer MP3-Erweiterung

    ![Beispiel für Typ der Datei](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    Um eine andere Erweiterung hinzuzufügen, klicken Sie auf **Ausschluss hinzufügen** , und geben Sie einen anderen Typ Erweiterung (Hinzufügen einer JPEG-Erweiterung).

    ![Ein weiteres Beispiel der Datei geben](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)

8. Wenn Sie alle die Erweiterungen hinzugefügt haben, klicken Sie auf **OK**.

9. Fahren Sie mit der Terminplan Sicherung-Assistenten, indem Sie auf **Weiter** , bis der **Bestätigungsseite**, und klicken Sie auf **Fertig stellen**.

    ![Ausschluss Bestätigung](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a>Nächste Schritte
- [Wiederherstellen von WindowsServer oder Windows-Client aus Azure](backup-azure-restore-windows-server.md)
- Wenn Sie weitere Informationen zur Azure Sicherung finden Sie unter [Übersicht über die Sicherung Azure](backup-introduction-to-azure-backup.md)
- Besuchen Sie das [Forum für Azure Sicherung](http://go.microsoft.com/fwlink/p/?LinkId=290933)
