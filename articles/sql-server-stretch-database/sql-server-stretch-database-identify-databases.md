<properties
    pageTitle="Identifizieren von Datenbanken und Tabellen für gedehnt Datenbank durch Ausführen von gedehnt Datenbank Advisor | Microsoft Azure"
    description="Informationen Sie zum Identifizieren von Datenbanken und Tabellen, die für die Datenbank gedehnt geeignet sind."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="identify-databases-and-tables-for-stretch-database-by-running-stretch-database-advisor"></a>Identifizieren von Datenbanken und Tabellen für gedehnt Datenbank durch Ausführen von gedehnt Datenbank Advisor

Zum Identifizieren von Datenbanken und Tabellen, die für die Datenbank gedehnt geeignet sind, herunterladen Sie SQL Server 2016 Upgrade Advisor, und führen Sie die Advisor gedehnt Datenbank. Dehnen Datenbank Advisor identifiziert auch blockierenden Probleme.

## <a name="download-and-install-upgrade-advisor"></a>Herunterladen und Installieren von Upgrade Advisor
Herunterladen und Installieren von Upgrade Advisor aus [hier](http://go.microsoft.com/fwlink/?LinkID=613421). Dieses Tool ist nicht auf dem SQL Server-Installationsmedium enthalten.

## <a name="run-the-stretch-database-advisor"></a>Führen Sie den Datenbank Dehnen Advisor

1.  Ausführen von Upgrade Advisor.

2.  Wählen Sie **Szenarien**, und wählen Sie dann auf **Ausführen GEDEHNT Datenbank ADVISOR**.

3.  Klicken Sie auf das Blade **Ausführen gedehnt Datenbank Advisor** auf **ANALYSIEREN von Datenbanken zu markieren**.

4.  Geben Sie das Blade **Datenbanken auswählen** oder wählen Sie den Servernamen und die Authentifizierung Informationen. Klicken Sie auf **Verbinden**.

5.  Eine Liste der Datenbanken auf dem ausgewählten Server angezeigt wird. Wählen Sie die Datenbanken, die Sie analysieren möchten. Klicken Sie auf **auswählen**.

6.  Klicken Sie auf das Blade **Ausführen gedehnt Datenbank Advisor** klicken Sie auf **Ausführen**.  Die Analyse ausgeführt wird.

## <a name="review-the-results"></a>Überprüfen Sie die Ergebnisse

1.  Wenn die Analyse, klicken Sie auf das Blade **Datenbanken analysierte Daten abgeschlossen ist** wählen Sie eine der Datenbanken, die Sie zum Anzeigen des **Analyseergebnisse** Blades analysiert.

    Das Blade **Analyseergebnisse** Listen empfohlene Tabellen in der ausgewählten Datenbank, die den standardmäßigen Empfehlungen Kriterien entsprechen.

2.  Wählen Sie in der Liste der Tabellen in der **Analyseergebnisse** Blade eine empfohlene Tabellen das Blade **Tabellenergebnisse** angezeigt werden.

    Wenn es Probleme blockiert werden, listet das **Tabellenergebnisse** Blade blockierende Probleme für die ausgewählte Tabelle an. Informationen zu blockierenden Probleme, die durch die Datenbank Advisor gedehnt erkannt finden Sie unter [Einschränkungen für gedehnt Datenbank](sql-server-stretch-database-limitations.md).

3.  In der Liste der blockierenden Probleme in der **Tabellenergebnisse** Blade, wählen Sie eine der Probleme, um weitere Informationen zu den ausgewählten Problem anzuzeigen und Schritte Reduzierung vorgeschlagen. Implementieren Sie die vorgeschlagenen Reduzierung Schritte, wenn Sie die ausgewählte Tabelle für gedehnt Datenbank konfigurieren möchten.

## <a name="next-step"></a>Als Nächstes
Aktivieren Sie Dehnen Datenbank.

-   Um gedehnt-Datenbank auf einer **Datenbank**aktivieren, finden Sie unter [Aktivieren gedehnt Datenbank für eine Datenbank](sql-server-stretch-database-enable-database.md).

-   Zum Aktivieren von gedehnt Datenbank auf einer anderen **Tabelle**, wenn gedehnt der Datenbank bereits aktiviert ist, finden Sie unter [Aktivieren gedehnt Datenbank einer Tabelle](sql-server-stretch-database-enable-table.md).

## <a name="see-also"></a>Siehe auch

[Einschränkungen für Dehnen Datenbank](sql-server-stretch-database-limitations.md)

[Aktivieren Sie gedehnt Datenbank für eine Datenbank](sql-server-stretch-database-enable-database.md)

[Aktivieren Sie gedehnt Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md)

[Alle Themen Azure SQL Server gedehnt-Datenbank-Dienst](sql-server-stretch-database-index-all-articles.md)
