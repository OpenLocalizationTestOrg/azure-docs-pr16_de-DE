<properties 
    pageTitle="Erstellen einer Sicherungskopie Ihrer app in Azure" 
    description="Erfahren Sie, wie in der App-Verwaltungsdienst Azure Sicherungen Ihrer Apps zu erstellen." 
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

# <a name="back-up-your-app-in-azure"></a>Erstellen einer Sicherungskopie Ihrer app in Azure


Das Feature sichern und Wiederherstellen in [Azure-App-Dienst](../app-service/app-service-value-prop-what-is.md) können Sie die auf einfache Weise app Sicherungskopien manuell oder automatisch erstellen. Sie können Ihre app zu einem vorherigen Status wiederherstellen, oder erstellen Sie eine neue app basierend auf eine der Sicherungen Ihrer ursprünglichen app. 

Informationen zum Wiederherstellen einer app aus einer Sicherung finden Sie unter [Wiederherstellen eine app in Azure](web-sites-restore.md).

<a name="whatsbackedup"></a>
## <a name="what-gets-backed-up"></a>Was gesichert wird 
App-Dienst können Sie die folgenden Informationen zurück:

* App-Konfiguration
* Der Inhalt der Datei
* Alle Azure SQL-Datenbanken oder Azure MySQL (ClearDB)-Datenbanken, die Verbindung zu Ihrer Anwendung (Sie auswählen können, welche in die Sicherung einschließen)

Diese Informationen auf Sichern der Azure-Speicher-Konto und Container, den Sie angeben. 

> [AZURE.NOTE] Jede Sicherung ist eine vollständige offline Kopie der app, keine inkrementell aktualisieren.

<a name="requirements"></a>
## <a name="requirements-and-restrictions"></a>Anforderungen und Einschränkungen

* Das Feature zum Sichern und Wiederherstellen ist der App-Serviceplan werden in der **Standardansicht** Ebene oder höher erforderlich. Weitere Informationen zu Ihren App-Serviceplan mit einer höheren Ebene skalieren finden Sie unter [Einrichten einer app in Azure skalieren](web-sites-scale.md). Beachten Sie, dass **Premium** Ebene eine größere Anzahl von tägliche Sicherungen als **Standard** Ebene ermöglicht.
* Sie benötigen eine Azure-Speicher-Konto und Container im selben Abonnement als die app, die Sie sichern möchten. Weitere Informationen über Konten Azure-Speicher finden Sie unter den [Links](#moreaboutstorage) am Ende dieses Artikels.
* Sicherungskopien können bis zu 10 GB von Hilfeinhalten app und Datenbank sein. Sie erhalten eine Fehlermeldung, wenn die Sicherungsdatei Größe dieser Grenzwert überschritten wird. 

<a name="manualbackup"></a>
## <a name="create-a-manual-backup"></a>Erstellen Sie eine manuelle Sicherung

2. Im [Portal Azure](https://portal.azure.com)navigieren Sie zu Ihrer app Blade, wählen Sie **Einstellungen**und dann **Sicherungskopien**. Das Blade **Sicherungskopien** wird angezeigt.
    
    ![Sicherungskopien Seite][ChooseBackupsPage]

    >[AZURE.NOTE]Wenn Sie die folgende Meldung angezeigt wird, klicken Sie auf, um Ihre App-Serviceplan zu aktualisieren, bevor Sie Sicherungskopien fortsetzen können.
Weitere Informationen finden Sie unter [Einrichten einer app in Azure skalieren](web-sites-scale.md) .  
    >![Wählen Sie Speicherkonto](./media/web-sites-backup/01UpgradePlan.png)

3. Klicken Sie in das Blade **Sicherungskopien** auf **Speicher: nicht konfiguriert** Speicher-Konto konfigurieren.

    ![Wählen Sie Speicherkonto][ChooseStorageAccount]
    
4. Wählen Sie Ihre Sicherung Ziel, indem Sie eine **Speicher-Konto** und **Container**aus. Das Speicherkonto muss mit dem gleichen als der app-Abonnement gehören, die Sie sichern möchten. Wenn Sie möchten, können Sie ein neues Speicherkonto oder einen neuen Container in den entsprechenden Blades erstellen. Wenn Sie fertig sind, klicken Sie auf **auswählen**.
    
    ![Wählen Sie Speicherkonto](./media/web-sites-backup/02ChooseStorageAccount1.png)
    
5. Klicken Sie in das Blade **Sicherung Einstellungen konfigurieren** , das noch geöffnet bleibt auf **Einstellungen für die Datenbank**, dann wählen Sie die Datenbanken, die in der Sicherungskopien (SQL-Datenbank oder MySQL) enthalten sein sollen, dann auf **OK**.  

    ![Wählen Sie Speicherkonto](./media/web-sites-backup/03ConfigureDatabase.png)

    > [AZURE.NOTE]  Für eine Datenbank in dieser Liste angezeigt werden muss der Verbindungszeichenfolge im Abschnitt **Verbindungszeichenfolgen** des Blades **ApplicationSettings** für Ihre app vorhanden sein.

6. Klicken Sie auf **Speichern**, in dem Blade **Sicherung Einstellungen konfigurieren** .  

7. Klicken Sie in der Befehlsleiste des Blades **Sicherungskopien** auf **Jetzt sichern**.
    
    ![Schaltfläche ' erstellen '][BackUpNow]
    
    Während der Sicherung wird eine Nachricht des Vorgangsfortschritts angezeigt.

Nachdem Sie den Container für Sicherungskopien und Speicherkonto konfiguriert haben, können Sie eine manuelle Sicherung zu einem beliebigen Zeitpunkt durchgeführt werden.  

<a name="automatedbackups"></a>
## <a name="configure-automated-backups"></a>Konfigurieren Sie die automatische Sicherungskopien

1. Klicken Sie in das Blade **Sicherungskopien** auf **Terminplan: nicht konfiguriert**. 

    ![Wählen Sie Speicherkonto](./media/web-sites-backup/05ScheduleBackup.png)
    
1. Festlegen Sie in den **Einstellungen für Sicherungskopien Terminplan** Blade der **Sicherung geplant** **auf**und klicken Sie dann konfigurieren Sie den Sicherung Terminplan nach Bedarf, und klicken Sie auf **OK**.
    
    ![Aktivieren Sie automatische Sicherungskopien][SetAutomatedBackupOn]
    
4. Klicken Sie in das **Konfigurieren von Einstellungen für Sicherungskopien** Blade, das noch geöffnet bleibt auf **Speicher Einstellungen**, und wählen Sie dann Ihre Sicherung Ziel, indem Sie eine **Speicher-Konto** und **Container**. Das Speicherkonto muss mit dem gleichen als der app-Abonnement gehören, die Sie sichern möchten. Wenn Sie möchten, können Sie ein neues Speicherkonto oder einen neuen Container in den entsprechenden Blades erstellen. Wenn Sie fertig sind, klicken Sie auf **auswählen**.
    
    ![Wählen Sie Speicherkonto](./media/web-sites-backup/02ChooseStorageAccount1.png)
    
5. Klicken Sie in das Blade **Sicherung Einstellungen konfigurieren** auf **Datenbank Einstellungen**, dann wählen Sie die Datenbanken, die in der Sicherungskopien (SQL-Datenbank oder MySQL) enthalten sein sollen und dann auf **OK**.  

    ![Wählen Sie Speicherkonto](./media/web-sites-backup/03ConfigureDatabase.png)

    > [AZURE.NOTE]  Für eine Datenbank in dieser Liste angezeigt werden muss der Verbindungszeichenfolge im Abschnitt **Verbindungszeichenfolgen** des Blades **ApplicationSettings** für Ihre app vorhanden sein.

6. Klicken Sie auf **Speichern**, in dem Blade **Sicherung Einstellungen konfigurieren** .  

<a name="partialbackups"></a>
## <a name="backup-just-part-of-your-app"></a>Zusätzliche nur Teil der app

Manchmal möchten Sie nicht alle Elemente auf der app sichern. Es folgen einige Beispiele:

-   Sie [Richten Sie wöchentliche Sicherungskopien](web-sites-backup.md#configure-automated-backups) der app, die statischen Inhalt, der nie ändert enthält, z. B. alten Blogbeiträge und Bildern.
-   Ihre app verfügt über 10GB des Inhalts (der max Betrag, den Sie nacheinander sichern können).
-   Sie möchten nicht, um die Protokolldateien zu sichern.

Teilweise Sicherungskopien können Sie genau die Dateien wählen Sie sichern möchten.

### <a name="exclude-files-from-your-backup"></a>Ausschließen von Dateien aus der Sicherung

Zum Ausschließen von Dateien und Ordnern aus Ihrer Sicherungskopien Erstellen einer `_backup.filter` in den Ordner D:\home\site\wwwroot der app, und geben Sie die Liste der Dateien und Ordner, die Sie in den dort ausschließen möchten. Eine einfache Möglichkeit, Zugriff auf diese erfolgt über die [Kudu Console](https://github.com/projectkudu/kudu/wiki/Kudu-console). 

Angenommen Sie, Sie eine app, die Protokolldateien und statische Bilder aus vergangenen Jahre, die nie ändern abgelegt werden enthält. Sie verfügen bereits über eine vollständige Sicherung der app, die die alte Bilder enthält. Sie möchten nun die app täglich sichern, aber nicht bezahlen zum Speichern von Protokolldateien oder den statischen Grafikdateien, die nie ändern möchten.

![' Protokolle ' Ordner][LogsFolder]
![Ordner Bilder][ImagesFolder]
    
Die folgende Schritte aus anzeigen, wie Sie diese Dateien aus der Sicherung ausschließen möchten.

1. Wechseln Sie zu `http://{yourapp}.scm.azurewebsites.net/DebugConsole` und identifizieren Sie die Ordner, die Sie aus Ihrer Sicherungskopien ausschließen möchten. In diesem Beispiel würden Sie die folgenden Dateien und Ordner, die in die Benutzeroberfläche dargestellt ausschließen möchten:

        D:\home\site\wwwroot\Logs
        D:\home\LogFiles
        D:\home\site\wwwroot\Images\2013
        D:\home\site\wwwroot\Images\2014
        D:\home\site\wwwroot\Images\brand.png

    [AZURE.NOTE] Die letzte Zeile zeigt, dass Sie Einzelpersonen Dateien als auch Ordner ausschließen können.

2. Erstellen Sie eine Datei namens `_backup.filter` und setzen Sie die Liste oben in der Datei, aber entfernen `D:\home`. Liste ein Verzeichnis oder eine Datei pro Zeile an. Damit Sie der Inhalt der Datei sein soll:

    \site\wwwroot\Logs \LogFiles \site\wwwroot\Images\2013 \site\wwwroot\Images\2014 \site\wwwroot\Images\brand.PNG

3. Diese Datei zum Hochladen der `D:\home\site\wwwroot\` Verzeichnis Ihrer Website mithilfe von [ftp](web-sites-deploy.md#ftp) oder eine andere Methode. Wenn Sie möchten, können Sie die Datei erstellen direkt in `http://{yourapp}.scm.azurewebsites.net/DebugConsole` , und fügen Sie den Inhalt dort.

4. Führen Sie die gleiche Weise, wie es, [manuell](#create-a-manual-backup) oder [automatisch](#configure-automated-backups)gewohnt Sicherungskopien.

Jetzt, alle Dateien und Ordner, die in angegeben sind `_backup.filter` aus der Sicherung ausgeschlossen. In diesem Beispiel werden die Protokolldateien und den Grafikdateien 2013 und 2014 nicht mehr sowie brand.png gesichert werden.

>[AZURE.NOTE] Sie wiederherstellen teilweise Sicherungen Ihrer Website auf die gleiche Weise, wie Sie [eine normale Sicherung wiederherstellen](web-sites-restore.md)möchten. Die Wiederherstellung wird das richtige tun.
>
>Wenn eine vollständige Sicherung wiederhergestellt wird, wird alle Inhalte auf der Website durch die Sicherung wird ersetzt. Wenn eine Datei auf der Website, aber nicht in die Sicherung ist, wird er gelöscht. Wenn eine teilweise Sicherung wiederhergestellt wird, wird alle Inhalte, die in einem der Verzeichnisse auf schwarzer Liste oder eine beliebige Datei auf schwarzer Liste befindet ungeändert bleiben jedoch.

<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a>Wie Sicherungen gespeichert sind

Nachdem Sie eine oder mehrere Sicherungskopien für Ihre app vorgenommen haben, werden die Sicherungskopien auf **Container** Falz Ihr Speicherkonto als auch Ihre app angezeigt. In dem Speicherkonto besteht aus jeder Sicherung ZIP-Datei, die die gesicherten Daten enthält, und eine XML-Datei, die ein Manifest den Inhalt der ZIP-Datei enthält. Sie können Entzippen Sie ihn, und diese Dateien zu navigieren, wenn Sie Ihre Sicherungskopien zugreifen, ohne Ausführen einer app wiederherstellen möchten.

Die Sicherungskopie der Datenbank für die app wird im Stammverzeichnis der ZIP-Datei gespeichert. Für eine SQL-Datenbank Dies ist eine BACPAC-Datei (ohne Erweiterung) und importiert werden kann. Zum Erstellen einer neuen SQL-Datenbank basierend auf den Export BACPAC finden Sie unter [Importieren einer BACPAC-Datei zum Erstellen einer neuen Datenbank](http://technet.microsoft.com/library/hh710052.aspx).

> [AZURE.WARNING] Ändern aller Dateien in Ihrem **Websitebackups** Container kann dazu führen, dass die Sicherung vorgesehen ist ungültig und können daher nicht wiederherstellbar.

<a name="nextsteps"></a>
## <a name="next-steps"></a>Nächste Schritte
Informationen zum Wiederherstellen einer app aus einer Sicherung finden Sie unter [Wiederherstellen eine app in Azure](web-sites-restore.md). Sie können auch sichern und Wiederherstellen der App-Service-apps mit REST-API (finden Sie unter [Verwenden der restlichen zu sichern und Wiederherstellen von App-Dienst apps](websites-csm-backup.md)).

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.


<!-- IMAGES -->
[ChooseBackupsPage]: ./media/web-sites-backup/01ChooseBackupsPage.png
[ChooseStorageAccount]: ./media/web-sites-backup/02ChooseStorageAccount.png
[IncludedDatabases]: ./media/web-sites-backup/03IncludedDatabases.png
[BackUpNow]: ./media/web-sites-backup/04BackUpNow.png
[BackupProgress]: ./media/web-sites-backup/05BackupProgress.png
[SetAutomatedBackupOn]: ./media/web-sites-backup/06SetAutomatedBackupOn.png
[Frequency]: ./media/web-sites-backup/07Frequency.png
[StartDate]: ./media/web-sites-backup/08StartDate.png
[StartTime]: ./media/web-sites-backup/09StartTime.png
[SaveIcon]: ./media/web-sites-backup/10SaveIcon.png
[ImagesFolder]: ./media/web-sites-backup/11Images.png
[LogsFolder]: ./media/web-sites-backup/12Logs.png
[GhostUpgradeWarning]: ./media/web-sites-backup/13GhostUpgradeWarning.png
 
