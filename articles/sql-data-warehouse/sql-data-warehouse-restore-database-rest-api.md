<properties
   pageTitle="Wiederherstellen einer SQL Azure Datawarehouse (REST-API) | Microsoft Azure"
   description="REST-API Aufgaben zum Wiederherstellen einer Azure SQL-Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Wiederherstellen einer SQL Azure Datawarehouse (REST-API)

> [AZURE.SELECTOR]
- [(Übersicht)][]
- [Portal][]
- [PowerShell][]
- [REST][]

In diesem Artikel erfahren Sie, wie eine Azure SQL Data Warehouse mithilfe der REST-API wiederherstellen.

## <a name="before-you-begin"></a>Vorbemerkung

**Überprüfen Sie Ihre DTU Kapazität aus.** Jede SQL Data Warehouse gehostet wird von einer SQLServer (z. B. myserver.database.windows.net) die ein DTU Kontingent hat.  Bevor Sie eine SQL Data Warehouse wiederherstellen können, überprüfen Sie Folgendes der SQLServer verfügt über genügend verbleibende DTU Kontingent für die Datenbank, die wiederhergestellt wird. Erfahren Sie, wie erforderlich DTU berechnen oder weitere DTU anfordern finden Sie unter [DTU Kontingent Änderung anfordern][].

## <a name="restore-an-active-or-paused-database"></a>Wiederherstellen einer Datenbank aktiven oder angehalten

Zum Wiederherstellen einer Datenbank:

1. Abrufen der Liste der Datenbank Wiederherstellungspunkte mithilfe des Vorgangs Get-Datenbank wiederherstellen Punkte.
2. Beginnen Sie Ihre wiederherstellen mithilfe des Vorgangs [erstellen Datenbank wiederherstellen Anforderung][] aus.
3. Verfolgen Sie den Status Ihrer wiederherstellen mithilfe des [Datenbank Vorgangsstatus][] Vorgangs aus.

>[AZURE.NOTE] Nachdem die Wiederherstellung abgeschlossen ist, können Sie die wiederhergestellte Datenbank nach folgenden [Konfigurieren Ihrer Datenbank nach der Wiederherstellung][]konfigurieren.

## <a name="restore-a-deleted-database"></a>Wiederherstellen einer gelöschten Datenbank

Eine gelöschte Datenbank wiederherzustellen:

1.  Alle Ihre wiederherstellbaren gelöschten Datenbanken mit der [Liste wiederherstellbar abgelegten Datenbanken][] Operation aufgelistet.
2.  Rufen Sie die Details für die gelöschten Datenbank, die Sie wiederherstellen, indem Sie mithilfe des Vorgangs [wiederherstellbar gelöschte Datenbank abrufen][] möchten.
3.  Beginnen Sie Ihre wiederherstellen mithilfe des Vorgangs [erstellen Datenbank wiederherstellen Anforderung][] aus.
4.  Verfolgen Sie den Status Ihrer wiederherstellen mithilfe des [Datenbank Vorgangsstatus][] Vorgangs aus.

>[AZURE.NOTE] Zum Konfigurieren Ihrer Datenbank nach Abschluss die Wiederherstellung finden Sie unter [konfigurieren die Datenbank nach der Wiederherstellung][]. 


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen über die Business Continuity-Features von Azure SQL-Datenbank-Editionen, lesen Sie die [Azure SQL-Datenbank Business Continuity (Übersicht)][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Datenbank Business Continuity (Übersicht)]: ./sql-database-business-continuity.md
[Anfordern einer Änderung der DTU Kontingent]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Konfigurieren Sie die Datenbank nach der Wiederherstellung]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: ./powershell-install-configure.md
[(Übersicht)]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Erstellen Sie die Datenbank wiederherstellen Anforderung]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Datenbank Vorgangsstatus]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Abrufen von wiederherstellbar gelöschte Datenbank]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[Liste wiederherstellbar abgelegt Datenbanken]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
