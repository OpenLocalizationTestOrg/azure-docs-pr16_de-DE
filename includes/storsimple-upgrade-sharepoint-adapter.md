<!--author=SharS last changed: 9/17/15-->

### <a name="upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-storsomple-adapter-for-sharepoint"></a>Upgrade von SharePoint 2010 nach SharePoint 2013, und klicken Sie dann Installieren der StorSomple für SharePoint

>[AZURE.IMPORTANT] Alle Dateien, die zuvor in externen Speicher über RSP verschoben wurden werden nicht verfügbar sein, bis die Aktualisierung abgeschlossen ist und die RSP-Funktion wieder aktiviert ist. Führen Sie um Benutzer Einfluss zu beschränken, irgendeine Aktualisierung oder Neuinstallation während einer geplanten Wartung-Fenster an.

#### <a name="to-upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-adapter"></a>Upgrade von SharePoint 2010 nach SharePoint 2013, und installieren Sie die Netzwerkadapter

1. Beachten Sie in der SharePoint 2010-Farm den Pfad der BLOB-Speicher für die gelegten BLOBs und die Inhaltsdatenbanken für die RSP-Code aktiviert ist. 

2. Installieren Sie und konfigurieren Sie die neue SharePoint 2013-Farm. 

3. Verschieben Sie Datenbanken,-Anwendungen und Websitesammlungen für die neue SharePoint 2013-Farm aus der SharePoint 2010-Farm. Anweisungen finden Sie unter [Übersicht über den Upgradeprozess auf SharePoint 2013](https://technet.microsoft.com/library/cc262483.aspx).

4. Installieren Sie die Netzwerkadapter StorSimple für SharePoint auf der neuen Farm ein. Verfahren finden Sie in [der StorSimple Netzwerkadapter für SharePoint installieren](#install-the-storsimple-adapter-for-sharepoint) .

5. Verwenden die Informationen, die Sie in Schritt 1 notiert haben, aktivieren Sie RSP-Code für den gleichen Satz von Inhaltsdatenbanken, und geben Sie den Pfad der BLOB-Speicher derselben, der in der SharePoint 2010-Installation verwendet wurde. Wechseln Sie zu [Konfigurieren RSP](#configure-rbs) Verfahren aus. Nachdem Sie diesen Schritt abgeschlossen haben, sollte zuvor gelegten Dateien aus der neuen Farm zugegriffen werden. 

### <a name="upgrade-the-storsimple-adapter-for-sharepoint"></a>Upgrade der StorSimple Netzwerkadapter für SharePoint

>[AZURE.IMPORTANT] Planen Sie diese Aktualisierung während einer geplanten Wartungsfenster aus den folgenden Gründen auftreten:
>
>- Zuvor gelegten Inhalt nicht zur Verfügung, bis der Netzwerkadapter neu installiert wird.
>
>- Alle Inhalte, die auf die Website hochgeladen wird, nach der Deinstallation der Vorgängerversion von der StorSimple Netzwerkadapter für SharePoint, aber vor der Installation auf der neuen Version wird in der Inhaltsdatenbank gespeichert werden. Sie möchten, dass der Inhalt mit dem Gerät StorSimple verschieben, nachdem Sie den neuen Netzwerkadapter installiert haben. Sie können das Microsoft` RBS Migrate()` PowerShell-Cmdlet Lieferumfang von SharePoint, um den Inhalt zu migrieren. Weitere Informationen finden Sie unter [Migrieren von Inhalten in oder aus RSP](https://technet.microsoft.com/library/ff628255.aspx). 


#### <a name="to-upgrade-the-storsimple-adapter-for-sharepoint"></a>So aktualisieren Sie die Netzwerkadapter StorSimple für SharePoint 

1. Deinstallieren Sie die vorherige Version des StorSimple Netzwerkadapter für SharePoint an.

    >[AZURE.NOTE] Dadurch wird automatisch RSP-Code auf die Inhaltsdatenbanken deaktiviert. Vorhandene BLOBs bleiben jedoch auf dem Gerät StorSimple. Da RSP deaktiviert ist, und die BLOBs nicht wieder auf die Inhaltsdatenbanken migriert wurde sind, tritt ein Fehler Anfragen für diese BLOBs. 
 
2. Installieren Sie die neue StorSimple Netzwerkadapter für SharePoint an. Die neue Netzwerkadapter die Inhaltsdatenbanken beginnen, die zuvor aktiviert oder deaktiviert für RSP-Code wird automatisch erkannt und die vorherigen Einstellungen verwenden.
