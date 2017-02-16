<properties
   pageTitle="Verwenden von Azure Stream Analytics mit SQL Datawarehouse | Microsoft Azure"
   description="Tipps zur Verwendung von Azure Stream Analytics mit Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Verwenden von Azure Stream Analytics mit SQL Datawarehouse

Azure Stream Analytics ist eine vollständig verwaltete Dienst Tiefst-Wartezeit hoch verfügbare und skalierbare komplexe Ereignis Verarbeitung über streaming-Daten in der Cloud bereitstellen. Sie können die Grundlagen kennen, lesen Sie [Einführung in Azure Stream Analytics][]. So erstellen Sie eine End-to-End-Lösung mit Stream Analytics anhand des Lernprogramms [Erste Schritte mit Azure Stream Analytics][] dann weitere.

In diesem Artikel erfahren Sie, wie Ihre Datenbank Azure SQL-Data Warehouse als ein Empfänger Ausgabe für Ihre Aufträge Stream Analytics verwendet werden.

## <a name="prerequisites"></a>Erforderliche Komponenten

Führen Sie zunächst über die folgenden Schritte aus, in die [Erste Schritte mit Azure Stream Analytics][] -Lernprogramm.  

1. Erstellen Sie eine Ereignis Hub Eingabe
2. Konfigurieren Sie und starten Sie Ereignis-Generator-Anwendung
3. Bereitstellen eines Auftrags Stream Analytics
4. Angeben der Position Eingabe- und Abfrage

Erstellen Sie dann eine Azure SQL Data Warehouse-Datenbank

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Geben Sie die Position Ausgabe: Data Warehouse der Azure SQL-Datenbank

### <a name="step-1"></a>Schritt 1

Klicken Sie in Ihrem Auftrag Stream Analytics auf **die Ausgabe** vom oberen Rand der Seite, und klicken Sie dann auf **Ausgabe hinzufügen**.

### <a name="step-2"></a>Schritt 2

Wählen Sie SQL-Datenbank aus, und klicken Sie auf Weiter.

![][add-output]

### <a name="step-3"></a>Schritt 3
Geben Sie auf der nächsten Seite die folgenden Werte ein:

- *Die Ausgabealias*: Geben Sie einen Anzeigenamen für diese Ausgabe Position ein.
- *Abonnements*:
    - Ist die Data Warehouse SQL-Datenbank im selben als den Auftrag Stream Analytics-Abonnement, wählen Sie SQL-Datenbank verwenden aus aktuellen Abonnements.
    - Ist die Datenbank in ein anderes Abonnement, wählen Sie aus einer anderen Abonnement SQL-Datenbank verwenden.
- *Datenbank*: Geben Sie den Namen einer Ziel-Datenbank.
- *Server-Name*: Geben Sie den Servernamen für die Datenbank, die Sie gerade angegeben haben. Das klassische Azure-Portal können Sie um dies zu suchen.

![][server-name]

- *Benutzername*: Geben Sie den Benutzernamen eines Kontos, das über Schreibberechtigungen für die Datenbank verfügt.
- *Kennwort*: Geben Sie das Kennwort für das angegebene Benutzerkonto.
- *Tabelle*: Geben Sie den Namen der Zieltabelle in der Datenbank.

![][add-database]

### <a name="step-4"></a>Schritt 4

Klicken Sie auf die Schaltfläche Überprüfen, diese Position Ausgabe hinzuzufügen und stellen Sie sicher, dass Stream Analytics eine Verbindung mit der Datenbank herstellen können.

![][test-connection]

Wenn die Verbindung mit der Datenbank hergestellt wurde, wird eine Benachrichtigung am unteren Rand des Portals angezeigt. Sie können am unteren zum Testen der Verbindung mit der Datenbank Verbindung testen klicken.

## <a name="next-steps"></a>Nächste Schritte

Einen Überblick über die Integration finden Sie unter [Übersicht über die Data Warehouse SQL-Integration][].

Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung von SQL Data Warehouse][].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Einführung in Azure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Erste Schritte mit Azure Stream Analytics]: ../stream-analytics/stream-analytics-get-started.md
[Übersicht über die Entwicklung von SQL Data Warehouse]:  ./sql-data-warehouse-overview-develop.md
[Übersicht über die Data Warehouse SQL-integration]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
