<properties
    pageTitle="Erstellen und Verwalten von skalierten, flexible Aufträge mit Azure SQL-Datenbanken | Microsoft Azure"
    description="Durchgehen Sie erstellen und Verwalten eines Auftrags flexible Datenbank aus."
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="ddove"/>

# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Erstellen und Verwalten von skalierten, Azure SQL-Datenbanken mit flexible Aufträge (Preview)

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShell](sql-database-elastic-jobs-powershell.md)


**Flexible Datenbank Aufträge** vereinfachen die Verwaltung von Gruppen von Datenbanken durch administrative Vorgänge wie Schemänderungen, Verwaltung von Anmeldeinformationen, Verweis Datenupdates, Sammlung von Daten oder Mandanten (Customer) werden Websitesammlung ausführen. Flexible Datenbankaufträge ist derzeit verfügbar über die Azure-Portal und den PowerShell-Cmdlets. Jedoch verringert die Azure Portals Flächen Funktionalität eingeschränkt zur Ausführung in allen Datenbanken in einer [Datenbank flexible Ressourcenpool (Preview)](sql-database-elastic-pool.md). Um zusätzliche Features und Ausführung von Skripts über eine Gruppe von Datenbanken, einschließlich einer benutzerdefinierten Websitesammlung oder eine Shard Reihe (erstellt mit [flexible Datenbank-Client-Bibliothek](sql-database-elastic-scale-introduction.md)) zugreifen zu können, finden Sie unter [Erstellen und Verwalten von Aufträgen mithilfe der PowerShell](sql-database-elastic-jobs-powershell.md). Weitere Informationen zu Aufträgen finden Sie unter [flexible Datenbank Aufträge (Übersicht)](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Erforderliche Komponenten

* Ein Azure-Abonnement. Kostenlose Testversion finden Sie unter [kostenlose Testversion für einen Monat](https://azure.microsoft.com/pricing/free-trial/).
* Ein Ressourcenpool flexible Datenbank. Finden Sie unter [Informationen zum flexible Datenbank pools](sql-database-elastic-pool.md)
* Installation von flexible Auftrag Dienst Datenbankkomponenten. Siehe [Auftragsdienst flexible Datenbank installieren](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Erstellen von Aufträgen

1. Klicken Sie mit der [Azure-Portal](https://portal.azure.com)aus einer vorhandenen Datenbank flexible Auftrag Pool **Auftrag erstellen**.
2. Geben Sie den Benutzernamen und das Kennwort für den Datenbankadministrator (bei der Installation von Aufträgen erstellt) für die Aufträge Steuerelement Datenbank (Metadatenspeicher für Aufträge).

    ![Benennen Sie den Auftrag, geben Sie oder fügen Sie den Code und klicken Sie auf Ausführen][1]
2. Geben Sie in das Blade **Auftrag erstellen** einen Namen für das Projekt aus.
3. Geben Sie den Benutzernamen und das Kennwort für die Verbindung auf die Zieldatenbanken mit den entsprechenden Berechtigungen für die Ausführung von Skript erfolgreich durchgeführt werden kann.
4. Fügen Sie ein, oder geben Sie im T-SQL-Skript.
5. Klicken Sie auf **Speichern** und dann auf **Ausführen**.

    ![Projekte erstellen und ausführen][5]

## <a name="run-idempotent-jobs"></a>Führen Sie Idempotent Aufträge

Wenn Sie ein Skript anhand einer Reihe von Datenbanken ausführen, müssen Sie sicher, dass das Skript Idempotent ist. D. h., muss das Skript mehrmals, ausführen können, selbst wenn Fehler beim in einem unvollständig Zustand vor. Beispielsweise, wenn ein Skript fehlschlägt, der Auftrag automatisch wiederholt wird, bis sie erfolgreich ist (innerhalb von Grenzen, als der Wiederholung Logik später nicht mehr den wiederholen). Die Möglichkeit hierfür besteht darin, verwenden den einer "IF EXISTS"-Klausel, und löschen Sie alle gefundene Instanz vor dem Erstellen eines neuen Objekts. Ein Beispiel ist hier dargestellt:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Alternativ Verwenden einer "IF NOT EXISTS"-Klausel vor dem Erstellen einer neuen Instanz aus:

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

Dieses Skript aktualisiert dann die zuvor erstellte Tabelle.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>Überprüfen Sie den Auftrag

Nachdem ein Projekt begonnen wurde, können Sie auf den Fortschritt überprüfen.

1. Klicken Sie auf **Aufträge verwalten**, von der Seite Ressourcenpool flexible-Datenbank.

    ![Klicken Sie auf "Projekte verwalten"][2]

2. Klicken Sie auf den Namen (a) eines Auftrags. Der **STATUS** kann "Erledigt" oder "Fehlgeschlagen". Details des Projekts werden (b) mit Datum und Uhrzeit der erstellen und ausführen. Unterhalb der, die die Liste (c) zeigt den Fortschritt des Skripts für jede Datenbank im Pool, dessen Datum und Uhrzeit Details zugewiesen.

    ![Aktivieren eines Auftrags abgeschlossen][3]


## <a name="checking-failed-jobs"></a>Auschecken fehlgeschlagen Aufträge

Wenn ein Auftrag fehlschlägt, kann ein Protokoll seiner Ausführung gefunden. Klicken Sie auf den Namen der fehlerhafte Auftrag, um die Details anzuzeigen.

![Aktivieren Sie einen fehlerhaften Auftrag][4]


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png

 
