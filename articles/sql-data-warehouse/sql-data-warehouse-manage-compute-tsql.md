<properties
   pageTitle="Verwalten von berechnen Power in Azure SQL Data Warehouse (REST) | Microsoft Azure"
   description="Transact-SQL (T-SQL) Aufgaben Skalierung Leistung durch DWUs anpassen. Sparen Sie Kosten, indem Sie dieselbe Skalierung wieder Zeiten nicht Höchstwert."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a>Verwalten von berechnen Power in Azure SQL-Data Warehouse (T-SQL)

> [AZURE.SELECTOR]
- [(Übersicht)](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Skalierung der Leistung durch Skalierung berechnen, Ressourcen und Arbeitsspeicher, um die Notwendigkeit, Ändern der Arbeitsbelastung entsprechen. Speichern von Kosten durch Skalierung zurück Ressourcen während nicht Höchstwert Zeiten oder Anhalten berechnen ganz. 

Diese Sammlung von Aufgaben verwendet T-SQL auf:

- Aktuelle DWU Ansichtsoptionen
- Ändern von berechnen Ressourcen durch DWUs anpassen

Wenn Sie anhalten oder Wiederaufnehmen einer Datenbank, wählen Sie eine der anderen Plattformoptionen oben in diesem Artikel aus.

Informationen hierzu finden Sie Informationen finden Sie unter [Verwalten Power Übersicht zu berechnen][].

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Aktuelle DWU Ansichtsoptionen

So zeigen Sie die aktuellen DWU Einstellungen für die Datenbanken an:

1. Öffnen Sie SQL Server-Objekt-Explorer in Visual Studio 2015.
2. Verbinden Sie mit der master-Datenbank mit der logischen SQL-Datenbankserver verbunden sind.
2. Wählen Sie aus der sys.database_service_objectives dynamische Management-Ansicht. Hier ist ein Beispiel: 

```
SELECT
 db.name [Database],
 ds.edition [Edition],
 ds.service_objective [Service Objective]
FROM
 sys.database_service_objectives ds
 JOIN sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Maßstab berechnen

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

So ändern Sie die DWUs


1. Verbinden Sie mit der master-Datenbank mit Ihrer logischen SQL-Datenbankserver verbunden sind.
2. Verwenden Sie die [ALTER DATABASE][] TSQL-Anweisung ein. Im folgende Beispiel: Legt das Ebene Ziel Dienst auf DW1000 für die Datenbank MySQLDW. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte

Andere Verwaltungsaufgaben finden Sie unter [Übersicht über die Verwaltung][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Verwaltung (Übersicht)]: ./sql-data-warehouse-overview-manage.md
[Verwalten von berechnen Power (Übersicht)]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ÄNDERN DER DATENBANK]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
