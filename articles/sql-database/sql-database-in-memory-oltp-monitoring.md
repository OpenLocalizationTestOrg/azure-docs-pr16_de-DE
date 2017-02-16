<properties
    pageTitle="Überwachen von in-Memory-Speicher XTP | Microsoft Azure"
    description="Schätzung und Monitor XTP in-Memory-Speicher verwenden, Kapazität; Beheben Kapazität Fehlers 41823"
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="monitor-in-memory-oltp-storage"></a>Monitor OLTP In-Memory-Speicher

Bei der Verwendung von [In-Memory OLTP](sql-database-in-memory.md)befindet sich die Daten in Tabellen Speicher optimiert und Tabellenvariablen in OLTP In-Memory-Speicher. Jede Ebene der Premium-Dienst verfügt über eine maximale OLTP In-Memory-Speichergröße, die [SQL-Datenbank Dienst Ebenen Artikel](sql-database-service-tiers.md#service-tiers-for-single-databases)beschrieben ist. Sobald diese Grenze überschritten wird, einfügen und Aktualisieren von Vorgängen möglicherweise weiß (mit Fehler 41823) nicht starten. An diesem Punkt werden müssen entweder löschen Daten, um Arbeitsspeicher freizugeben oder upgrade die Leistung Ebene der Datenbank.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Feststellen Sie, ob die Daten in der in-Memory-Speicher Linienende passen

Ermitteln der Speicher Linienende: Wenden Sie sich an den [SQL-Datenbank Dienst Ebenen Artikel](sql-database-service-tiers.md#service-tiers-for-single-databases) für die FESTSTELLTASTE Speicher von verschiedenen Ebenen Premium Service.

Schätzen Arbeitsspeicher Anforderungen für Works-Speicher optimiert Tabelle enthält die gleiche Weise für SQL Server, wie es in Azure SQL-Datenbank. Überprüfen Sie diesen Artikel auf [MSDN](https://msdn.microsoft.com/library/dn282389.aspx)ein paar Minuten dauern.

Beachten Sie, dass die Tabelle und Variable Tabellenzeilen, als auch Indizes, gegen die Größe des max Benutzer Daten zählen. Darüber hinaus, benötigt ALTER TABLE ausreichend Platz zum Erstellen einer neuen Version von die gesamte Tabelle und deren Indizes.

## <a name="monitoring-and-alerting"></a>Überwachen und warnen

Sie können in-Memory-Speicher für den Einsatz als Prozentwerte des im [Speicher zu begrenzen, für die der Leistung Ebene](sql-database-service-tiers.md#service-tiers-for-single-databases) der Azure- [Portal](https://portal.azure.com/)überwachen: 

- Klicken Sie in der Datenbank Blade suchen Sie das Auslastung Ressource, und klicken Sie auf Bearbeiten.
- Wählen Sie dann die Metrik `In-Memory OLTP Storage percentage`.
- Klicken Sie zum Hinzufügen einer Benachrichtigung klicken Sie auf das Feld Ressourcen Auslastung das metrische Blade öffnen und dann auf Warnung hinzufügen.

Oder verwenden Sie die folgende Abfrage, um die Nutzung von in-Memory-Speicher anzuzeigen:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Korrigieren Sie Situationen für Out-of-Memory - Fehler 41823

Out-of-Memory-Ergebnisse im einfügen, aktualisieren und erstellen Vorgänge Fehlermeldung 41823 weiß nicht ausgeführt.

Fehlermeldung 41823 weist darauf hin, dass die Tabellen Speicher optimiert und die Tabellenvariablen die maximale Größe überschritten haben.

Zum Beheben dieses Fehlers auf entweder:


- Löschen von Daten aus den Tabellen Speicher optimiert, potenziell Verschiebung Daten herkömmliche, auf dem Datenträger Tabellen. oder,
- Aktualisieren Sie die Dienstebene auf mit genügend in-Memory-Speicher für die Daten, die Sie in Tabellen Speicher optimiert beibehalten müssen.

## <a name="next-steps"></a>Nächste Schritte
Zusätzliche Ressourcen zu über [Azure SQL-Datenbank für die Überwachung dynamische Management-Ansichten](sql-database-monitoring-with-dmvs.md)
