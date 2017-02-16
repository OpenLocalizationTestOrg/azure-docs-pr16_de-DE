<properties
    pageTitle="Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe der Azure-Portal"
    description="Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe der Azure-Portal"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-using-the-azure-portal"></a>Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe der Azure-Portal

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-export.md)
- [PowerShell](sql-database-export-powershell.md)

Dieser Artikel enthält Anweisungen für Archivierung Ihrer Azure SQL-Datenbank in eine BACPAC-Datei (gespeichert in Azure Blob-Speicher) mit dem [Azure-Portal](https://portal.azure.com).

Wenn Sie ein Archiv einer SQL Azure-Datenbank erstellen müssen, können Sie das Datenbankschema und die Daten in eine Datei BACPAC exportieren. Eine BACPAC-Datei ist einfach eine ZIP-Datei mit der Erweiterung BACPAC. Eine Datei BACPAC später in Azure Blob-Speicher oder in einer lokalen Speicher in einem lokalen Speicherort gespeichert werden kann und höher importierten zurück in SQL Azure-Datenbank oder in einer SQL Server lokal Installation. 

***Aspekte***

- Ein Archiv konsistent sein, müssen Sie sicherstellen, dass keine schreiben Aktivität erfolgt während des Exports, oder Sie aus einer [konsistent Kopie](sql-database-copy.md) Ihrer SQL Azure-Datenbank exportieren.
- Die maximale Größe einer BACPAC-Datei in Azure Blob-Speicher archiviert ist 200 GB. Um eine größere BACPAC-Datei in den lokalen Speicher zu archivieren, verwenden Sie das Programm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) Eingabeaufforderungsfenster. Dieses Programm im Lieferumfang von Visual Studio und SQL Server. Sie können auch das [herunterladen](https://msdn.microsoft.com/library/mt204009.aspx) der neuesten Version von SQL Server Data Tools können Sie dieses Programm zu gelangen.
- Archivierung auf Azure Premium Speicher mithilfe einer Datei BACPAC wird nicht unterstützt.
- Wenn der Exportvorgang 20 Stunden überschreitet, kann sie abgebrochen werden. Zum Steigern der Leistung während des Exportvorgangs können Sie folgende Aktionen ausführen:
 - Vergrößern Sie vorübergehend Ihre Dienstalter ein.
 - Beenden Sie alle lesen und Schreiben von Aktivität während des Exports.
 - Verwenden eines [gruppierten Index](https://msdn.microsoft.com/library/ms190457.aspx) mit nicht-Null-Werte für alle großen Tabellen. Ohne gruppierte Indizes möglicherweise ein Export fehl, wenn es mehr als 6 bis 12 Stunden dauert. Dies ist, da der Export-Dienst einen Table-Scan erledigen ausprobieren, um die gesamte Tabelle exportieren. Eine gute Möglichkeit, um festzustellen, ob die Tabellen für den Export optimiert sind ist **DBCC SHOW_STATISTICS** ausführen, und stellen Sie sicher, dass die *RANGE_HI_KEY* nicht null ist und deren Wert gute Verteilung hat. Weitere Informationen finden Sie unter [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).


> [AZURE.NOTE] BACPACs dienen nicht verwendet werden, für die Sicherung und Wiederherstellung. Azure SQL-Datenbank erstellt automatisch Sicherungskopien für jeden Benutzerdatenbank. Details finden Sie unter [Übersicht über die Business Continuity](sql-database-business-continuity.md).

Zum Durchführen dieses Artikels benötigen Sie Folgendes:

- Ein Azure-Abonnement.
- Einer Azure-SQL­Datenbank. 
- Ein [Standardspeicher Azure-Konto](../storage/storage-create-storage-account.md) mit einem Blob-Container, die BACPAC in standard-Speicher zu speichern.

## <a name="export-your-database"></a>Exportieren Sie Ihrer Datenbank

Öffnen Sie das Blade SQL-Datenbank für die Datenbank, die Sie exportieren möchten.

> [AZURE.IMPORTANT] Zu eine konsistente BACPAC Datei sichergestellt ist, sollten zunächst [eine Kopie der Datenbank erstellen](sql-database-copy.md) und exportieren Sie die Datenbankkopie. 

1.  Wechseln Sie zum [Azure-Portal](https://portal.azure.com)an.
2.  Klicken Sie auf **SQL-Datenbanken**.
3.  Klicken Sie auf die Datenbank zu archivieren.
4.  Klicken Sie auf **Exportieren** , um das Blade **Datenbank exportieren** zu öffnen, in das Blade SQL-Datenbank:

    ![Schaltfläche ' exportieren '][1]

5.  Klicken Sie auf **Speicher** , und wählen Sie Ihre Speicher-Konto und Blob Container, in dem die BACPAC gespeichert werden:

    ![Exportieren der Datenbank][2]

6. Wählen Sie Ihre Authentifizierungstyp aus. 
7.  Geben Sie die entsprechenden Authentifizierungsinformationen für den SQL Azure-Server mit der Datenbank, die Sie exportieren möchten.
8.  Klicken Sie auf **OK** , um die Datenbank zu archivieren. Erstellt eine Datenbankabfrage exportieren, und übermittelt ihn an den Dienst auf **OK** . Die Zeitdauer der Export ausführen, werden hängt von der Größe und Komplexität der Datenbank und Ihrer Dienstalter ab. Sie erhalten eine Benachrichtigung.

    ![Benachrichtigung exportieren][3]

## <a name="monitor-the-progress-of-the-export-operation"></a>Überwachen Sie den Fortschritt des Exportvorgangs

1.  Klicken Sie auf **SQL Server**.
2.  Klicken Sie auf dem Server, mit der ursprünglichen (Quelle)-Datenbank, die Sie gerade archiviert.
3.  Führen Sie einen Bildlauf nach unten zu Vorgängen.
4.  Klicken Sie in der SQL Server-Blade auf **Import/Export-Verlauf**:

    ![Importieren von Verlauf exportieren][4]

## <a name="verify-the-bacpac-is-in-your-storage-container"></a>Stellen Sie sicher, dass die BACPAC in Ihrem Speichercontainer ist

1.  Klicken Sie auf **Speicher-Konten**.
2.  Klicken Sie auf die Stelle, an der Sie das BACPAC-Archiv gespeichert Speicher-Konto.
3.  Klicken Sie auf **Container** , und wählen Sie die Datenbank in Details (Sie können herunterladen und Speichern der BACPAC hier) exportiert Container.

    ![.bacpac Dateidetails][5]  

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Importieren einer BACPAC mit einer Azure SQL-Datenbank, finden Sie unter [Importieren einer BACPCAC mit einer SQL Azure-Datenbank](sql-database-import.md)
- Weitere Informationen zum Importieren einer BACPAC zu einer SQL Server-Datenbank, finden Sie unter [Importieren einer BACPCAC zu einer SQL Server-Datenbank](https://msdn.microsoft.com/library/hh710052.aspx)



<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-notification.png
[4]: ./media/sql-database-export/export-history.png
[5]: ./media/sql-database-export/bacpac-archive.png

