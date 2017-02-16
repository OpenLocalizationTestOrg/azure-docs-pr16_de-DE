<properties 
   pageTitle="Zusätzliche StorSimple Snapshot-Manager-Richtlinien | Microsoft Azure"
   description="Beschreibt, wie das StorSimple Snapshot-Manager MMC-Snap-in zum Erstellen und verwalten die Sicherung Richtlinien, die geplante Sicherungskopien steuern verwenden."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/12/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>Verwenden Sie zum Erstellen und Verwalten von Sicherung Richtlinien StorSimple Snapshot-Manager

## <a name="overview"></a>(Übersicht)

Eine Sicherung Richtlinie erstellt einen Zeitplan für die Lautstärke Daten sichern, lokal oder in der Cloud. Wenn Sie eine zusätzliche Richtlinie erstellen, können Sie auch eine Aufbewahrungsrichtlinie angeben. (Sie können bis zu 64 Momentaufnahmen beibehalten.) Weitere Informationen zu Sicherung Richtlinien, finden Sie unter [Sicherung Typen](storsimple-what-is-snapshot-manager.md#backup-type) in [StorSimple 8000-Serie: einer Hybrid-Cloud-Lösung](storsimple-overview.md).

In diesem Lernprogramm wird erläutert, wie Sie:

- Erstellen einer Sicherungskopie Richtlinie 
- Bearbeiten einer Sicherung Richtlinie 
- Löschen einer Sicherung Richtlinie 

## <a name="create-a-backup-policy"></a>Erstellen einer Sicherungskopie Richtlinie

Verwenden Sie das folgende Verfahren zum Erstellen einer neuen Sicherungskopie Richtlinie ein.

#### <a name="to-create-a-backup-policy"></a>So erstellen Sie eine zusätzliche Richtlinie

1. Klicken Sie auf das Desktopsymbol um StorSimple Snapshot-Manager zu starten.

2. Klicken Sie im **Bereich** mit der rechten Maustaste **Sicherung Richtlinien**, und klicken Sie auf **Sichern Richtlinie erstellen**.

    ![Erstellen einer Sicherungskopie Richtlinie](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    Das Dialogfeld **Erstellen einer Richtlinie** angezeigt wird. 

    ![Erstellen einer Richtlinie - Registerkarte Allgemein](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)

3. Führen Sie auf der Registerkarte **Allgemein** die folgende Informationen ein:

   1. Geben Sie in das Textfeld **Name** einen Namen für die Richtlinie ein.

   2. Geben Sie in das Textfeld **Volume-Gruppe** den Namen der Gruppe Lautstärke die Richtlinie zugeordnet.

   3. Wählen Sie **lokale Snapshot** oder **Snapshot Cloud**.

   4. Wählen Sie die Anzahl der Momentaufnahmen beibehalten. Wenn Sie **Alle**auswählen, werden 64 Momentaufnahmen beibehalten (das Maximum). 

4. Klicken Sie auf die Registerkarte **Zeitplan** .

    ![Erstellen einer Richtlinie - Registerkarte ' Terminplan '](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)

5. Führen Sie auf der Registerkarte **Zeitplan** die folgende Informationen ein: 

   1. Klicken Sie auf das Kontrollkästchen **Aktivieren** , um zur nächsten Sicherung zu planen.

   2. Wählen Sie unter **Einstellungen** **einmal**, **täglich**, **wöchentlich**oder **monatlich**. 

   3. Klicken Sie auf das Kalendersymbol, und wählen Sie ein Startdatum, in das Textfeld **Starten** .

   4. Klicken Sie unter **Erweiterte Einstellungen**können Sie optional wiederholen Zeitpläne und ein Enddatum festlegen.

   5. Klicken Sie auf **OK**.

Nachdem Sie eine zusätzliche Richtlinie erstellt haben, können Sie die folgenden Informationen im **Ergebnisbereich** angezeigt:

- **Name** – den Namen der Sicherungsdatei Richtlinie.

- **Typ** – lokale Snapshot oder eine Momentaufnahme der Cloud.

- **Volumen-Gruppe** – der Volume-Gruppe, der die Richtlinie zugeordnet.

- **Aufbewahrungsrichtlinien** – die Anzahl der Momentaufnahmen beibehalten; Das Maximum beträgt 64.

- **Erstellt** – das Datum, das dieser Richtlinie erstellt wurde.

- **Aktiviert** – gibt an, ob die Richtlinie aktuell in Kraft ist: **True** gibt an, dass es aktiviert; **False** gibt an, dass es nicht aktiviert ist. 

## <a name="edit-a-backup-policy"></a>Bearbeiten einer Sicherung Richtlinie

Verwenden Sie das folgende Verfahren zum Bearbeiten einer vorhandenen Sicherung Richtlinie ein.

#### <a name="to-edit-a-backup-policy"></a>So bearbeiten Sie eine zusätzliche Richtlinie

1. Klicken Sie auf das Desktopsymbol um StorSimple Snapshot-Manager zu starten. 

2. Klicken Sie auf den Knoten **Sicherung Richtlinien** , klicken Sie im **Bereich** . Alle Sicherung Richtlinien werden im Bereich **Ergebnisse** angezeigt. 

3. Mit der rechten Maustaste in der Richtlinie, die Sie bearbeiten möchten, und klicken Sie dann auf **Bearbeiten**. 

    ![Bearbeiten einer Sicherung Richtlinie](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png) 

4. Wenn das **Erstellen einer Richtlinie** -Fenster angezeigt wird, geben Sie die gewünschten Änderungen vor, und klicken Sie dann auf **OK**. 

## <a name="delete-a-backup-policy"></a>Löschen einer Sicherung Richtlinie

Gehen Sie folgendermaßen vor, um eine Sicherung Richtlinie löschen.

#### <a name="to-delete-a-backup-policy"></a>So löschen Sie eine zusätzliche Richtlinie

1. Klicken Sie auf das Desktopsymbol um StorSimple Snapshot-Manager zu starten. 

2. Klicken Sie auf den Knoten **Sicherung Richtlinien** , klicken Sie im **Bereich** . Alle Sicherung Richtlinien werden im Bereich **Ergebnisse** angezeigt. 

3. Mit der rechten Maustaste in der Sicherung Richtlinie, die Sie löschen möchten, und klicken Sie dann auf **Löschen**.

4. Wenn der bestätigungsmeldung angezeigt wird, klicken Sie auf **Ja**.

    ![Zusätzliche Richtlinie Bestätigung löschen](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie [StorSimple Snapshot-Manager verwalten Sie Ihre Lösung StorSimple verwendet](storsimple-snapshot-manager-admin.md).
- Erfahren Sie, wie [StorSimple Snapshot-Manager anzeigen und Verwalten von Sicherung Aufträge verwendet](storsimple-snapshot-manager-manage-backup-jobs.md).
