
<!--
includes/sql-data-warehouse-include-pause-description.md

Latest Freshness check:  2016-04-22 , barbkess.

As of circa 2016-04-22, the following topics might include this include:
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-powershell.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-rest-api.md

-->
Um Kosten zu speichern, können Sie anhalten und fortsetzen berechnen Ressourcen bei Bedarf. Beispielsweise wenn Sie die Datenbank während der Nacht oder am Wochenende verwenden, können Sie zeigen sie diese Zeiten, und es während des Tages fortsetzen. Für DWUs Sie wird nur belastet während die Datenbank angehalten ist.

Wenn Sie eine Datenbank zeigen:

- Berechnen und Speicherkapazität Ressourcen werden dem Pool der verfügbaren Ressourcen in der Mitte der Daten zurückgegeben.
- DWU Kosten sind 0 (null) für die Dauer der Pause.
- Datenspeicher ist nicht betroffen, und die Daten intakt bleibt. 
- SQL Data Warehouse bricht alle laufenden oder in der Warteschlange Operationen ab.