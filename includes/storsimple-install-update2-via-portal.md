<!--author=alkohli last changed: 03/17/16-->

#### <a name="to-install-an-update-from-the-azure-portal"></a>So installieren Sie ein Update vom Azure-portal

1. Wählen Sie auf der Seite StorSimple Dienst Ihr Gerät aus. Navigieren Sie zu **Geräte** > **Wartung**.

2. Klicken Sie am unteren Rand der Seite auf **Updates überprüfen**. Zum Suchen nach verfügbaren Updates wird ein Projekt erstellt. Sie werden benachrichtigt, wenn Sie der Auftrag erfolgreich abgeschlossen wurde.

3. Im Abschnitt **Softwareupdates** auf derselben Seite sehen Sie sich, dass neue Softwareupdates verfügbar sind. Es empfiehlt sich, dass die Versionsinformationen überprüfen, bevor Sie ein Update auf Ihrem Gerät anwenden.

4. Klicken Sie am unteren Rand der Seite auf **Updates installieren**, und klicken Sie dann auf **OK**.

5. Das Dialogfeld **Updates installieren** stellen Sie sicher, dass die gegebenen Empfehlungen befolgt haben und dann wählen Sie **ich grundlegende Informationen zu den oben angegebenen Anforderung und bin auf meinem Gerät upgrade bereit,** und klicken Sie auf die Schaltfläche überprüfen.

    ![Bestätigen Sie die Meldung](./media/storsimple-install-update2-via-portal/InstallUpdate12_2M.png)

7. Eine Reihe von vorbereitende überprüft wird jetzt gestartet. Hierzu gehören:

    - **Controller Gesundheit überprüft** , um sicherzustellen, dass sowohl die Gerätecontroller fehlerfrei und online sind.

    - **Hardware-Komponente Gesundheit überprüft** , um sicherzustellen, dass alle Hardware-Komponenten auf Ihrem Gerät StorSimple fehlerfrei sind.

    - **Daten 0 überprüft** , um sicherzustellen, dass Daten 0 auf Ihrem Gerät aktiviert ist. Wenn diese Schnittstelle nicht aktiviert ist, müssen Sie zum Aktivieren der Arbeitsmappe, und führen Sie dann erneut.

    - **Daten 2 und 3 von Daten überprüft** , um sicherzustellen, dass Daten 2 und 3 von Daten Netzwerk-Schnittstellen nicht aktiviert sind. Wenn diese Schnittstellen aktiviert sind, müssen Sie diese zu deaktivieren, und versuchen Sie dann auf Ihrem Gerät zu aktualisieren. Mit dieser Prüfung wird ausgeführt, nur, wenn Sie auf einem Gerät mit GA-Software aktualisieren. Geräte, auf denen Versionen 0.1, 0,2 und 0,3 werden mit dieser Prüfung nicht benötigt werden.

    - **Gateway aktivieren** , auf jedem Gerät eine Version vor Update 1 ausgeführt. Mit dieser Prüfung auf dem Gerät, die für die Ausführung vor dem Update 1 Software ausgeführt wird, jedoch schlägt auf Geräten, die ein Gateway für einen Netzwerkadapter als Daten 0 konfiguriert haben.

    Das Update wird angewendet, wenn alle Prüfungen erfolgreich abgeschlossen werden. Sie werden benachrichtigt, dass überprüft wird ausgeführt werden.

    ![Vor dem Kontrollkästchen Benachrichtigung](./media/storsimple-install-update2-via-portal/InstallUpdate12_3M.png)

    Im folgenden finden ein Beispiel, in dem der Fehler bei Überprüfung. Sie benötigen, um sicherzustellen, dass sowohl die Gerätecontroller fehlerfrei und online sind. Sie müssen auch die Integrität der Hardware-Komponenten zu überprüfen. In diesem Beispiel benötigen Controller 0 und 1 Controller Komponenten Aufmerksamkeit. Möglicherweise müssen Sie den Microsoft-Support wenden Sie sich an, wenn Sie diese Probleme nicht, indem Sie sich selbst beheben können.

     ![Fehler beim überprüft](./media/storsimple-install-update2-via-portal/HCS_PreUpgradeChecksFailed-include.png)

8. Nach der erfolgreichen Prüfungen wird ein Aktualisierungsvorgang erstellt. Sie werden benachrichtigt, wenn die Aktualisierung erfolgreich erstellt wurde.

    ![Aktualisieren Sie die Erstellung der Position](./media/storsimple-install-update2-via-portal/InstallUpdate12_44M.png)

    Das Update wird dann auf Ihrem Gerät angewendet werden.

9. Um den Status des Auftrags Update zu überwachen, klicken Sie auf **Ansicht Position**. Klicken Sie auf der Seite **Projekte** sehen Sie den Aktualisierungsfortschritt.

10. Das Aktualisieren wird ein paar Stunden dauern. Wählen Sie das Update aus, und klicken Sie auf **Details** , um die Details des Projekts zu einem beliebigen Zeitpunkt anzuzeigen.

11. Nachdem das Projekt abgeschlossen ist, navigieren Sie zu der Seite **zum Warten** , und führen Sie einen Bildlauf nach unten bis zum **Softwareupdates**.
