<properties
    pageTitle="Wiederherstellen eine einzelne Tabelle aus Azure SQL-Datenbank sichern | Microsoft Azure"
    description="Erfahren Sie, wie Sie eine einzelne Tabelle aus Azure SQL-Datenbank sichern wiederherstellen."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>


# <a name="how-to-restore-a-single-table-from-an-azure-sql-database-backup"></a>Zum Wiederherstellen einer einzelnen Tabelle aus einer Sicherung Azure SQL-Datenbank

Sie können eine Situation auftreten, in dem Sie versehentlich geändert einiger Daten in einer SQL-Datenbank, und möchten nun die einzelne betroffene Tabelle wiederherstellen. Dieser Artikel beschreibt, wie Sie eine einzelne Tabelle in einer Datenbank aus einer SQL-Datenbank [Automatische Sicherungskopien](sql-database-automated-backups.md)wiederherstellen.

## <a name="preparation-steps-rename-the-table-and-restore-a-copy-of-the-database"></a>Vorbereitende Schritte: Benennen Sie die Tabelle, und stellen Sie eine Kopie der Datenbank wieder her
1. Kennzeichnen Sie die Tabelle in der SQL Azure-Datenbank, die Sie durch die wiederhergestellte Kopie ersetzen möchten. Verwenden Sie Microsoft SQL Management Studio, um die Tabelle umzubenennen. Benennen Sie beispielsweise die Tabelle als &lt;Tabellenname&gt;_old.

    **Notiz** Zur Vermeidung von blockiert werden, stellen Sie sicher, dass keine Aktivität ausgeführt wird, klicken Sie auf die Tabelle, die Sie umbenennen möchten. Wenn Probleme auftreten, stellen Sie sicher, die Durchführung dieses Verfahrens während eines Wartungszeitfensters.

2. Wiederherstellen einer Sicherungskopie der Datenbank an eine Stelle in der Zeit, die Sie wiederherstellen, um mit den Schritten [Punkt-In_Time wiederherstellen](sql-database-recovery-using-backups.md#point-in-time-restore) möchten.

    **Notizen**:
    - Der Namen der Datenbank wiederhergestellt werden im Format DB-Name + TimeStamp; beispielsweise **Adventureworks2012_2016-01-01T22-12Z**. Dieser Schritt wird nicht auf dem Server den vorhandenen Datenbanknamen überschreiben. Dies ist ein Maß für die Sicherheit, und es hat sollen, können Sie die wiederhergestellte Datenbank zu überprüfen, bevor sie legen Sie ihre aktuelle Datenbank, und benennen Sie die wiederhergestellte Datenbank für die Verwendung der Herstellung.
    - Alle Ebenen in der Leistung von grundlegenden als Premium werden vom Dienst, mit unterschiedlichen Sicherung Aufbewahrung Kennzahlen, je nach der Ebene automatisch gesichert:

| DB wiederherstellen | Grundlegende Ebene | Standard-Ebenen | Premium Ebenen |
| :-- | :-- | :-- | :-- |
|  Zeitpunkt wiederherstellen |  Wiederherstellen eines Punkt, innerhalb von 7 Tagen|Einen Punkt in 35 Tagen wiederherstellen| Einen Punkt in 35 Tagen wiederherstellen|

## <a name="copying-the-table-from-the-restored-database-by-using-the-sql-database-migration-tool"></a>Kopieren die Tabelle aus der wiederhergestellten Datenbank mithilfe des Migrationstools für SQL-Datenbank
1. Herunterladen Sie und installieren Sie den [Assistenten für die Migration von SQL-Datenbank](https://sqlazuremw.codeplex.com).

2. Öffnen Sie den SQL-Datenbank Migrations-Assistenten, auf der Seite **Select Process** , wählen Sie **die Datenbank unter Analysieren/migrieren**, und klicken Sie dann auf **Weiter**.
![SQL-Datenbank Migrations-Assistent – Prozess auswählen](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)
3. Klicken Sie im Dialogfeld **mit Server verbinden** gelten Sie die folgenden Einstellungen:
 - **Servername**: Ihr SQL Azure-Instanz
 - **Authentifizierung**: **SQL Server-Authentifizierung**. Geben Sie Ihre Anmeldeinformationen ein.
 - **Datenbank**: **Master DB (Listen auf allen Datenbanken)**.
 - **Notiz** Standardmäßig speichert der Assistent Ihre Anmeldeinformationen an. Wenn Sie sie nicht möchten, wählen Sie **Anmeldeinformationen vergessen**.
![SQL-Datenbank Migrations-Assistent - Quelle auswählen - Schritt 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. Wählen Sie im Dialogfeld **Datenquelle auswählen** den Namen der wiederhergestellten Datenbank aus dem Abschnitt **vorbereitenden Schritte** als Quelle aus, und klicken Sie dann auf **Weiter**.

    ![SQL-Datenbank Migrations-Assistent - Quelle auswählen - Schritt 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)

5. Wählen Sie die Option **Wählen Sie bestimmte Datenbankobjekte aus** , und wählen Sie dann auf die Table(or tables), die Sie auf dem Zielserver migrieren möchten, klicken Sie im Dialogfeld **Wählen Sie Objekte** .
![SQL-Datenbank Migrations-Assistent – Objekte auswählen](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)

6. Klicken Sie auf der Seite **Zusammenfassung der Skript-Assistenten** auf **Ja** , wenn Sie aufgefordert werden, zu, ob Sie zum Generieren einer SQL-Skript bereit sind. Sie haben auch die Möglichkeit, das Skript TSQL zur späteren Verwendung zu speichern.
![SQL-Datenbank Migrations-Assistent – Skript-Assistent-Zusammenfassung](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)

7. Klicken Sie auf der Seite **Ergebnisse Zusammenfassung** auf **Weiter**.
![SQL-Datenbank Migrations-Assistent – Zusammenfassung der Ergebnisse](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)

8. Klicken Sie auf der Seite **Ziel-Server-Verbindung einrichten** klicken Sie auf **Verbindung mit Server herstellen**, und geben Sie die Details wie folgt:
    - **Servername**: Ziel-Server-Instanz
    - **Authentifizierung**: **SQL Server-Authentifizierung**. Geben Sie Ihre Anmeldeinformationen ein.
    - **Datenbank**: **Master DB (Listen auf allen Datenbanken)**. Diese Option Listet alle Datenbanken auf dem Server.

    ![SQL-Datenbank Migrations-Assistent – Setup Ziel-Server-Verbindung](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)

9. Klicken Sie auf **Verbinden**, wählen Sie die Zieldatenbank, der Sie die Tabelle, um verschieben möchten, und klicken Sie dann auf **Weiter**. Dies sollte Fertig stellen, das zuvor erstelltes Skript ausgeführt sollte, und es der neu verschobenen Tabelle in die Zieldatenbank kopiert.

## <a name="verification-step"></a>Überprüfungsschritt
1. Abfragen und Testen der neu kopierten Tabelle aus, um sicherzustellen, dass die Daten intakt sind. Nach der Bestätigung können Sie das Tabellenformular umbenannte **vorbereitenden Schritte** im Abschnitt löschen. (z. B. &lt;Tabellenname&gt;_old).

## <a name="next-steps"></a>Nächste Schritte

[Automatische Sicherungskopien SQL-Datenbank](sql-database-automated-backups.md)
