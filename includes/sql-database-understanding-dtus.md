Die Datenbank Transaktion Einheit (DTU) ist der Maßeinheit in SQL-Datenbank, die die relative Leistungsfähigkeit von Datenbanken basierend auf ein praktisches Measure darstellt: die Datenbanktransaktion. Wir haben eine Reihe von Vorgängen, die für online-Transaktionen verarbeiten Anforderung (OLTP), und klicken Sie dann gemessen wie viele Transaktionen pro Sekunde unter vollständig abgeschlossen werden konnte Bedingungen geladen typische sind (Dies ist die Kurzfassung, Sie können die größter Details in der [Benchmark-Übersicht](../articles/sql-database/sql-database-benchmark-overview.md)lesen). 

Beispielsweise bietet eine Datenbank Premium P11 mit 1750 DTUs 350 x DTU mehr als eine einfache Datenbank mit 5 DTUs Power zu berechnen. 

![Einführung in die SQL-Datenbank: einzelne Datenbank DTUs durch die Ebenen- und Ebene.](./media/sql-database-understanding-dtus/single_db_dtus.png)

>[AZURE.NOTE] Wenn Sie eine vorhandene SQL Server-Datenbank migrieren, können Sie ein Drittanbieter-Tools, [Die Azure SQL-Datenbank DTU Taschenrechner](http://dtucalculator.azurewebsites.net/), erhalten eine Schätzung der die Leistungsstufe und service-Ebene, die die Datenbank in Azure SQL-Datenbank erfordern möglicherweise verwenden.

### <a name="dtu-vs-edtu"></a>DTU im Vergleich zu eDTU

Die DTU für einzelne Datenbanken übersetzt direkt an den eDTU für flexible Datenbanken. Angenommen, bietet eine Datenbank in einer Datenbank mit grundlegenden flexible Ressourcenpool bis zu 5 eDTUs. Dies ist dieselbe Performance wie eine einfache Datenbank. Der Unterschied besteht darin, dass die flexible Datenbank wird keine eDTUs aus dem Pool nutzen, bis Sie dies erforderlich ist. 

![Einführung in die SQL-Datenbank: Flexible Pools durch Ebene.](./media/sql-database-understanding-dtus/sqldb_elastic_pools.png)

Ein einfaches Beispiel kann. Optimieren Sie einer Datenbank mit grundlegenden flexible Ressourcenpool mit 1000 DTUs und legen Sie 800 Datenbanken darin ab. Solange nur 200 800 Datenbanken zu einem beliebigen Zeitpunkt genutzt werden in Zeit (5 DTU X 200 = 1000), wird nicht Sie Kapazität im Ressourcenpool erreichen, und Datenbank-Leistung wird nicht beeinträchtigt werden. In diesem Beispiel wird aus Gründen der Übersichtlichkeit vereinfacht. Die real Mathematik ist etwas komplizierter. Im Portal wird die mathematische für Sie und macht basierend auf zurückliegenden Datenbankverwendung empfohlen. Finden Sie unter [Preis und Leistung Aspekte für eine Datenbank flexible Ressourcenpool](../articles/sql-database/sql-database-elastic-pool-guidance.md) erfahren Sie, wie die Empfehlungen arbeiten oder die mathematische selbst durchführen. 
