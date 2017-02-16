<properties
   pageTitle="Verwalten von berechnen Power in Azure SQL Data Warehouse (REST) | Microsoft Azure"
   description="PowerShell-Aufgaben zum Verwalten von Power zu berechnen. Maßstab berechnen Ressourcen durch DWUs anpassen. Oder anhalten und fortsetzen Ressourcen zum Speichern von Kosten zu berechnen."
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

# <a name="manage-compute-power-in-azure-sql-data-warehouse-rest"></a>Verwalten von berechnen Power in Azure SQL Data Warehouse (REST)

> [AZURE.SELECTOR]
- [(Übersicht)](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Skalierung der Leistung durch Skalierung berechnen, Ressourcen und Speicher zum Ändern der auf die Bedürfnisse Ihrer Arbeitsbelastung entsprechen. Speichern von Kosten durch Skalierung zurück Ressourcen während nicht Höchstwert Zeiten oder Anhalten berechnen ganz. 

Diese Sammlung von Aufgaben verwendet im Azure-Portal an:

- Maßstab berechnen
- Anhalten berechnen
- Berechnen des Lebenslaufs

Informationen hierzu finden Sie Informationen finden Sie unter [Übersicht zu berechnen verwalten][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Maßstab berechnen power

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Um die DWUs zu ändern, verwenden Sie die REST-API [Erstellen oder die Datenbank aktualisieren][] . Im folgende Beispiel: Legt das Ebene Ziel Dienst auf DW1000 für die Datenbank MySQLDW, die auf dem Server MeinServer gehostet wird. Der Server ist in einer Azure Ressourcengruppe mit dem Namen ResourceGroup1.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/MyServer/databases/MySQLDW?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Anhalten berechnen

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Um eine Datenbank zu anzuhalten, verwenden Sie die [Datenbank anhalten][] REST-API aus. Im folgende Beispiel wird eine Datenbank auf einem Server mit dem Namen Server01 gehostet Database02 namens angehalten. Der Server ist in einer Azure Ressourcengruppe mit dem Namen ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Berechnen des Lebenslaufs

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Um eine Datenbank zu starten, verwenden Sie die [Datenbank Lebenslauf][] REST-API aus. Im folgende Beispiel wird eine Datenbank auf einem Server mit dem Namen Server01 gehostet Database02 namens gestartet. Der Server ist in einer Azure Ressourcengruppe mit dem Namen ResourceGroup1. 

```
POST https://management.azure.com/subscriptions{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/resume?api-version=2014-04-01-preview HTTP/1.1
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte

Andere Verwaltungsaufgaben finden Sie unter [Übersicht über die Verwaltung][].

<!--Image references-->

<!--Article references-->
[Verwaltung (Übersicht)]: ./sql-data-warehouse-overview-manage.md
[Verwalten von berechnen (Übersicht)]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Anhalten-Datenbank]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Lebenslauf-Datenbank]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Erstellen oder aktualisieren Sie die Datenbank]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
