<properties 
    pageTitle="Wiederherstellen einer Azure-app" 
    description="Erfahren Sie, wie Sie Ihre app aus einer Sicherung wiederherstellen." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/06/2016" 
    ms.author="cephalin"/>

# <a name="restore-an-app-in-azure"></a>Wiederherstellen einer Azure-app

In diesem Artikel wird veranschaulicht, wie eine app im [App-Verwaltungsdienst Azure](../app-service/app-service-value-prop-what-is.md) wiederherstellen, die Sie zuvor (siehe [Sichern Ihrer app in Azure](web-sites-backup.md)) gesichert haben. Sie können Ihre app mit seiner verknüpften Datenbanken (SQL-Datenbank oder MySQL) bei Bedarf in einem früheren Zustand wiederherstellen, oder erstellen Sie eine neue app basierend auf einer Sicherungskopie Ihrer ursprünglichen app. Erstellen einer neuen app, die parallel auf die neueste Version ausgeführt wird für A nützlich sein kann / B zu testen.

Wiederherstellen von Sicherungskopien steht für apps in **Standard-** und **Premium** Ebene ausgeführt. Informationen zum Einrichten Ihrer app Skalieren finden Sie unter [Einrichten einer app in Azure skalieren](web-sites-scale.md). **Premium** Ebene ermöglicht eine größere Anzahl von tägliche Sicherungen als **Standard** Ebene ausgeführt werden.

<a name="PreviousBackup"></a>
## <a name="restore-an-app-from-an-existing-backup"></a>Wiederherstellen einer app aus einer vorhandenen Sicherung

1. Klicken Sie auf das Blade **Einstellungen** der app im Portal Azure auf **Sicherungskopien** , um das Blade **Sicherungskopien** anzuzeigen. Klicken Sie dann auf **Jetzt wiederherstellen** in der Befehlsleiste. 
    
    ![Wählen Sie jetzt wiederherstellen][ChooseRestoreNow]

3. Wählen Sie zuerst die zusätzliche Quelle aus, in das Blade **Wiederherstellen** . 

    ![](./media/web-sites-restore/021ChooseSource.png)
    
    Die Option **App Sicherung** zeigt alle vorhandenen Sicherungskopien der aktuellen app, und können Sie einfach eine auswählen. 
    Die Option **Speicher** können Sie jede ZIP-Sicherungsdatei aus allen vorhandenen Speicher Azure-Konto und Container in Ihrem Abonnement auswählen. 
    Wenn Sie versuchen, eine Sicherungskopie von einer anderen app wiederherstellen, verwenden Sie die Option **Speicher** .

4. Anschließend geben Sie das Ziel für die app wiederherstellen, im **Ziel wiederherstellen**.

    ![](./media/web-sites-restore/022ChooseDestination.png)
    
    >[AZURE.WARNING] Wenn Sie **Überschreiben**auswählen, werden alle vorhandene Daten in Ihrer aktuellen app gelöscht. Bevor Sie auf **OK**klicken, stellen Sie sicher, dass genau ist, was Sie tun möchten.
    
    Sie können **Vorhandene App** wiederherstellen die Sicherung app zu einer anderen app in derselben Gruppe Resoure auswählen. Bevor Sie diese Option verwenden, sollten Sie bereits einer anderen app in Ihrem Ressourcengruppe mit Spiegelung Datenbankkonfiguration auf einen definierten in der app-Sicherung erstellt haben. 
    
5. Klicken Sie auf **OK**.

<a name="StorageAccount"></a>
## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Herunterladen Sie oder löschen Sie eine Sicherungskopie von einem Speicherkonto
    
1. Im Hauptfenster von **Durchsuchen** Blade des Portals Azure, wählen Sie **Speicherkonten**aus.
    
    Eine Liste Ihrer vorhandenen Speicher-Konten wird angezeigt. 
    
2. Wählen Sie das Speicherkonto, das die Sicherung enthält, die Sie herunterladen oder löschen möchten.
    
    Das Blade für das Speicherkonto wird angezeigt.

3. Wählen Sie in dem Speicher Accountn Blade den gewünschten container
    
    ![Container anzeigen][ViewContainers]

4. Wählen Sie die Sicherungsdatei ein, die Sie herunterladen oder löschen möchten.

    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)

5. Klicken Sie auf **herunterladen** oder **Löschen** , je nachdem, was Sie tun möchten.  

<a name="OperationLogs"></a>
## <a name="monitor-a-restore-operation"></a>Überwachen von einer Wiederherstellung
    
1. Navigieren Sie zum Anzeigen von Details zu den Erfolg oder Fehler des Wiederherstellungsvorgangs app zu **Überwachungsprotokoll** vorher in der Azure-Portal. 
    
    Das **Überwachungsprotokolle** Blade zeigt alle Ihre Vorgänge, sowie die Ebene, Status, Ressourcen und Details Zeit.
    
2. Führen Sie einen Bildlauf nach unten um zu suchen, die gewünschten Vorgang wiederherstellen aus, und klicken Sie auf, um es auszuwählen.

Das Blade Details werden die verfügbare Informationen im Zusammenhang mit der Wiederherstellung angezeigt.
    
## <a name="next-steps"></a>Nächste Schritte

Sie können auch sichern und Wiederherstellen der App-Service-apps mit REST-API (finden Sie unter [Verwenden der restlichen zu sichern und Wiederherstellen von App-Dienst apps](websites-csm-backup.md)).

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
 
