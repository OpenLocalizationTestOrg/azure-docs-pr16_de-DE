<properties
    pageTitle="SQL­Datenbank: Was ist eine DTU? | Microsoft Azure"
    description="Grundlegendes zu welche einer Azure SQL-Datenbank ist die Transaktionseinheit."
    keywords="Datenbankoptionen, die Leistung der Datenbank"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="explaining-database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Erläuterung Transaktion Einheiten für die Datenbank (DTUs) und flexible Datenbank Transaktion Einheiten (eDTUs)

In diesem Artikel wird erläutert, Transaktion Einheiten für die Datenbank (DTUs) und flexible Datenbank Transaktion Einheiten (eDTUs) und was passiert, wenn Sie die maximale DTUs oder eDTUs treffen.  

## <a name="what-are-database-transaction-units-dtus"></a>Was sind die Datenbank Transaktion Einheiten (DTUs)

Eine DTU ist eine Maßeinheit der Ressourcen, die garantiert in einer eigenständigen SQL Azure-Datenbank auf einer bestimmten Leistungsstufe innerhalb einer [eigenständigen Datenbank Dienstebene](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels)zur Verfügung. Eine DTU ist ein angeglichene Maß für die CPU, Arbeitsspeicher und Daten ein-/Ausgabe und Transaktion Log e/a in einem Verhältnis bestimmt, indem eine OLTP Benchmark-Auslastung des realen OLTP Auslastung typische werden soll. Verdopplung der DTUs durch Erhöhen der Leistungsstufe einer Datenbank entspricht verdoppelt Festlegen der Ressource für diese Datenbank verfügbar. Beispielsweise bietet eine Datenbank Premium P11 mit 1750 DTUs 350 x DTU mehr als eine einfache Datenbank mit 5 DTUs Power zu berechnen. Um das Verständnis hinter der OLTP Benchmark Arbeitsbelastung verwendet, um den Übergang DTU bestimmen finden Sie in der [SQL-Datenbank Benchmark-Übersicht](sql-database-benchmark-overview.md).

![Einführung in die SQL-Datenbank: einzelne Datenbank DTUs durch die Ebenen- und Ebene](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Sie können [Ändern](sql-database-scale-up.md) , Dienst Ebenen zu einem beliebigen Zeitpunkt mit minimaler Ausfallzeit an Ihrer Anwendung (im Allgemeinen aus weniger als vier Sekunden). Für viele Unternehmen und apps reicht die Datenbanken erstellen, und wählen einzelne Datenbank Leistung bei Bedarf nach oben oder unten, insbesondere dann, wenn Verwendungsmuster relativ vorhersehbar sind. Wenn Sie nicht vorhersehbar Verwendungsmustern haben, kann aber es ist schwer zu Kosten und Modell Business verwalten. In diesem Szenario verwenden Sie eine flexible Ressourcenpool mit einer bestimmten Anzahl von eDTUs an.

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Was sind flexible Datenbank Transaktion Einheiten (eDTUs)

Eine eDTU ist eine Maßeinheit der Gruppe von Ressourcen (DTUs), die von einer Reihe von Datenbanken auf einem SQL Azure-Server - bezeichnet eine [flexible Pool](sql-database-elastic-pool.png)gemeinsam verwendet werden können. Flexible Pools bieten eine einfache kostengünstige Lösung zum Verwalten der Leistungsziele auf mehrere Datenbanken, die Streuung mit wechselnder aufweisen und nicht vorhersehbar Verwendungsmuster. Finden Sie unter [flexible Pools und Dienst Ebenen](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) für Weitere Informationen.

![Einführung in die SQL-Datenbank: eDTUs durch die Ebenen- und Ebene](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Ein Ressourcenpool ist eine festgelegte Anzahl von eDTUs, die für einen festgelegten Preis angegeben. Innerhalb der Ressourcenpool werden einzelne Datenbanken automatische Skalierung in Festlegen von Parametern die Flexibilität erteilt. Bei hoher Auslastung kann eine Datenbank zum erfüllen bei Bedarf weitere eDTUs nutzen. Datenbanken unter light Belastung nutzen kleiner und Datenbanken keine Auslastung nutzen keine eDTUs. Bereitstellen von Ressourcen für den gesamten Pool und nicht für einzelne Datenbanken vereinfacht Ihre Verwaltungsaufgaben. Außerdem verlangt Sie ein vorhersehbar Budget für den Ressourcenpool haben.

Zusätzliche eDTUs können zu einem vorhandenen Pool mit Störungsfreiheit Datenbank oder keine Auswirkung auf die Datenbanken im flexible Pool hinzugefügt werden. Auf ähnliche Weise, wenn zusätzliche eDTUs nicht mehr benötigt werden können sie von einem vorhandenen Pool an einer beliebigen Stelle Zeitpunkt entfernt werden. Sie können addieren oder Subtrahieren von Datenbanken mit dem Pool. Wenn Sie eine Datenbank vorhersehbar unter-genutzt wird es Ressourcen, zu verschieben.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Wie kann ich die Anzahl der DTUs von meinem Arbeitsbelastung benötigt feststellen?

Wenn Ihnen zum Migrieren gesuchte einer vorhandenen lokalen, oder SQL Server virtuellen Computern Arbeitsbelastung mit Azure SQL-Datenbank, können Sie die [DTU Taschenrechner](http://dtucalculator.azurewebsites.net/) ungefähre die Anzahl der DTUs erforderlich. Einer bestehenden Arbeitsbelastung Azure SQL-Datenbank können [SQL-Datenbank Abfrage Performance-Sicherung](sql-database-query-performance.md) Sie um zu Ihrer Datenbank Ressourcenverbrauch (DTUs) abzurufenden tieferer Einblick zum Optimieren Ihrer Arbeitsbelastung zu verstehen. [Sys.dm_db_ Resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV können Sie auch um Verbrauchsinformationen Ressource für den letzten Stunde zu erhalten. Alternativ können im Katalog Ansicht [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) auch abgefragt werden, um den gleichen Daten für die letzten 14 Tage erhalten zwar bei einer unteren Genauigkeit von 5 Minuten Mittelwerten.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Wie feststellen kann ich, ob ich von einem flexible Ressourcenpool profitieren?

Pools eignen sich für eine große Anzahl von Datenbanken mit bestimmten Auslastung Mustern. Für eine bestimmte Datenbank ist diese Muster durch niedrigen durchschnittlichen Auslastung mit relativ seltene Auslastung Spitzen charakterisiert. SQL-Datenbank automatisch wertet zurückliegenden Ressource: Einsatz von Datenbanken in einen vorhandenen SQL-Datenbankserver und die entsprechenden Poolkonfiguration der Azure-Portal empfiehlt. Weitere Informationen finden Sie unter [Wann sollte eine Datenbank flexible Ressourcenpool verwendet werden?](sql-database-elastic-pool-guidance.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Was passiert, wenn ich meine maximale DTUs drücken

Leistungsmerkmale sind kalibriert und geregelt, um die benötigten Ressourcen zum Ausführen Ihrer Datenbank Arbeitsbelastung auf der max Grenzwerte für Ihre ausgewählten Dienst Ebene/Leistungsstufe zulässige bereitstellen. Wenn Sie Ihre Arbeitsbelastung der Grenzwerte in einem der CPU-/ Daten EA/Log EA Grenzwerte Berührung ist, Sie weiterhin die Ressourcen auf die maximal zulässige Ebene erhalten, aber Sie sind wahrscheinlich höhere Wartezeiten für Ihre Abfragen finden Sie unter. Diese Grenzwerte führen nicht in Fehlern, aber lieber eine Verzögerung in die Arbeitsbelastung, es sei denn, die Verzögerung, dass Abfragen Anzeigedauer Sie mit dem beginnen also schwerwiegende wird. Wenn Sie Grenzwerte der maximal zulässigen gleichzeitigen Benutzer Sitzungen/Anfragen (Arbeitsthreads) erreichen, sehen Sie explizite Fehler an. Finden Sie unter [Azure SQL-Datenbank Ressource Grenzwerte](sql-database-resource-limits.md) Informationen Beschränkung auf Ressourcen mit Ausnahme des CPU, Speicher, Daten i/o, und Transaktion Log i/o.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den DTUs und eDTUs verfügbar für eigenständigen Datenbanken und flexible Pools finden Sie unter [Service Ebene](sql-database-service-tiers.md) .
- Finden Sie unter [Azure SQL-Datenbank Ressource Grenzwerte](sql-database-resource-limits.md) Informationen Beschränkung auf Ressourcen mit Ausnahme des CPU, Speicher, Daten i/o, und Transaktion Log i/o.
- Finden Sie unter [SQL Datenbank Abfrage Leistung Einblicke](sql-database-query-performance.md) zu verstehen, der Verbrauch (DTUs).
- Finden Sie unter [SQL-Datenbank Benchmark-Übersicht](sql-database-benchmark-overview.md) zu verstehen, die Methoden hinter der OLTP Benchmark Arbeitsbelastung verwendet, um den Übergang DTU zu bestimmen.