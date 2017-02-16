<properties
    pageTitle="Leistungsindikatoren für Shard Landkarten-manager"
    description="ShardMapManager Klassen- und abhängige Weiterleitung Leistungsindikatoren"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="SilviaDoomra"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="SilviaDoomra"/>

# <a name="performance-counters-for-shard-map-manager"></a>Leistungsindikatoren für Shard Landkarten-manager

Sie können die Leistung des [Shard Karte Manager](sql-database-elastic-scale-shard-map-management.md), erfassen, besonders, wenn Sie mit [Daten abhängige routing](sql-database-elastic-scale-data-dependent-routing.md). Indikatoren werden mit Methoden der Klasse Microsoft.Azure.SqlDatabase.ElasticScale.Client erstellt.  

Indikatoren werden verwendet, um die Leistung von Vorgängen [Daten abhängige routing](sql-database-elastic-scale-data-dependent-routing.md) nachzuverfolgen. Diese Indikatoren sind in der Leistungsmonitor, unter der Kategorie "Flexible Datenbank: Shard Management" zugegriffen werden.

**Nach der neuesten Version:** Wechseln Sie zu [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Siehe auch [Upgraden einer app aus, um die neueste flexible Datenbank-Client-Bibliothek zu verwenden](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

* Um die Leistungskategorie und Datenquellen erstellen zu können, muss der Benutzer einen Teil der Gruppe der **Administratoren** auf dem Computer, auf dem die Anwendung sein.  

* Wenn Sie eine Instanz erstellen und aktualisieren die Indikatoren, muss der Benutzer ein Mitglied der Gruppe der **Administratoren** oder der **Leistung überwachen von Benutzern** sein. 

## <a name="create-performance-category-and-counters"></a>Erstellen Sie die Leistungskategorie und Indikatoren 

Um die Indikatoren zu erstellen, rufen Sie die CreatePeformanceCategoryAndCounters-Methode der [Klasse ShardMapManagmentFactory](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx). Nur der Administrator kann die Methode auszuführen: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

[Diese](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) PowerShell-Skript können Sie auch die Methode ausgeführt. Die Methode erstellt die folgenden Leistungsindikatoren:  

* **Zwischengespeicherte Zuordnungen**: Anzahl der Cache für die Karte Shard Zuordnungen.
*  **DDR pro Sekunde**: Zinsfuß abhängige Weiterleitung Datenoperationen für die Karte Shard. Der Zähler wird bei einem Anruf, um eine erfolgreiche Verbindung mit dem Ziel Shard [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) ergibt aktualisiert. 
*  **Zuordnung Nachschlage-Cache-Treffer/s**: Zinsfuß erfolgreich Cache-Suchvorgänge für Zuordnungen in der Karte Shard. 
*  **Nachschlagen Cache Fehler/sec Zuordnung**: Zinsfuß Fehler bei der Cache-Suchvorgänge für Zuordnungen in der Karte Shard.
*  **Zuordnungen hinzugefügt oder im Cache-sec aktualisiert**: Rate, mit welcher Zuordnungen hinzugefügt werden oder aktualisierten im Cache für die Karte Shard. 
*  **Zuordnungen von Cache-sec entfernt**: Zins, Zuordnungen werden aus dem Cache für die Karte Shard entfernt. 

Leistungsindikatoren werden für jede Zuordnung Cache Shard pro Prozess erstellt.  


## <a name="notes"></a>Notizen
Die folgenden Ereignisse auslösen die Erstellung der Leistungsindikatoren:  

* Initialisierung der [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) mit [möchten sofort richtig loslegen laden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), wenn die ShardMapManager alle Shard Karten enthält. Hierzu gehören die [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) und die [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) Methoden.
* Erfolgreiche Suche nach einer Shard Karte (mit [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) oder [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 

* Erfolgreiche Erstellung der Shard Karte mit CreateShardMap().

Die Leistungsindikatoren werden von allen Cache Operationen auf der Karte Shard und Zuordnungen aktualisiert werden. Erfolgreiche Entfernen der Shard Karte mit DeleteShardMap () Reults in der Leistungsindikatorinstanz löschen.  

## <a name="best-practices"></a>Bewährte Methoden

* Erstellung der Leistungskategorie und Indikatoren sollten vor dem Erstellen des Objekts ShardMapManager nur einmal durchgeführt werden. Jeder Ausführung des Befehls CreatePerformanceCategoryAndCounters() Löscht die vorherigen Indikatoren (von allen Instanzen gemeldete Daten verlieren) und neue erstellt.  

* Leistung Zähler Instanzen werden pro Prozess erstellt. Löschen der Leistungsindikatoren Instanzen werden alle zum Absturz der Anwendung oder Entfernen einer Zuordnung Shard aus dem Cache führen.  

### <a name="see-also"></a>Siehe auch

[Flexible Datenbank-Features (Übersicht)](sql-database-elastic-scale-introduction.md)  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

