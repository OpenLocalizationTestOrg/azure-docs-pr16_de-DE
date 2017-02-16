<properties
   pageTitle="Verwalten von Datenbanken in Azure SQL-Data Warehouse | Microsoft Azure"
   description="Übersicht über die Data Warehouse SQL-Datenbanken verwalten. Enthält Tools zum Projektmanagement, DWUs und die Skalierung der Systemleistung abfrageleistung, Einrichten von Richtlinien für gute Sicherheit und Wiederherstellen einer Datenbank von Beschädigung der Daten oder von einem regionalen Ausfall Problembehandlung."
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
   ms.date="08/16/2016"
   ms.author="barbkess;sonyama;"/>

# <a name="manage-databases-in-azure-sql-data-warehouse"></a>Verwalten von Inhaltsdatenbanken in Azure SQL-Data Warehouse

SQL Data Warehouse Automatisierung viele Aspekte der Datenbanken verwalten. Beispielsweise müssen zum Skalieren der Leistung Sie nur anpassen das richtige Maß berechnen Ressourcen bezahlen, und lassen Sie dann auf SQL Data Warehouse Skalierung und dieselbe Skalierung wieder alle Arbeit erledigen. 

Sie sollten zweifellos Überwachen Ihrer Arbeitsbelastung zum Identifizieren Indexeigenschaften Leistung sowie zum Behandeln von Problemen mit langer Abfragen. Sie müssen auch ein paar Sicherheitsaufgaben zum Verwalten von Berechtigungen für Benutzer und Benutzernamen ausführen.

Dieser Überblick befasst sich diese Aspekte beim Verwalten von SQL Data Warehouse.

- Tools zum Projektmanagement
- Maßstab berechnen
- Anhalten und fortsetzen
- Bewährte Methoden für die Leistung
- Abfrage für die Überwachung
- Sicherheit
- Sichern und Wiederherstellen

## <a name="management-tools"></a>Tools zum Projektmanagement

Sie können eine Vielzahl von Tools zum Verwalten von Datenbanken in SQL Data Warehouse verwenden. Wenn Sie Datenbanken verwalten, entwickeln Sie Tool Einstellungen für jeden Vorgang, die Sie ausführen müssen.

### <a name="azure-portal"></a>Azure-portal
Der [Azure-Portal][] ist eine webbasierte Portal, wo Sie können erstellen, aktualisieren und Löschen von Datenbanken und Datenbankressourcen überwachen. Dieses Tool eignet sich hervorragend ist, wenn Sie schnell einen Vorgang ausführen müssen oder nur erste mit Azure Schritte sind, eine kleine Anzahl von Datawarehouse-Datenbanken verwalten.

Um mit dem Portal Azure anzufangen, finden Sie unter [Erstellen einer SQL Data Warehouse (Azure-Portal)][].

### <a name="sql-server-data-tools-in-visual-studio"></a>SQL Server Data Tools in Visual Studio
[SQL Server Data Tools][] (SSDT) verwalten und Entwickeln Ihrer Datenbanken in Visual Studio können Sie eine Verbindung hergestellt werden. Wenn Sie eine Anwendung Entwicklertools Visual Studio oder anderen integrierten Entwicklung Umgebungen (IDEs) vertraut sind, versuchen Sie es mit SSDT in Visual Studio.

SSDT enthält die SQL Server-Objekt-Explorer, wodurch Sie zu visualisieren, zu verbinden und Ausführen von Skripts auf die Data Warehouse SQL-Datenbanken. Schnell Verbinden mit SQL Data Warehouse können Sie einfach die Schaltfläche **in Visual Studio geöffnet** , in der Befehlsleiste klicken, wenn die Datenbankdetails in der klassischen Azure-Portal anzeigen.  

Um mit SSDT in Visual Studio anzufangen, finden Sie unter [Abfrage Azure SQL-Data Warehouse mit Visual Studio][].

### <a name="command-line-tools"></a>Befehlszeile tools
Befehlszeilentools eignen sich optimal zum Automatisieren von Ihrer Auslastung.  PowerShell und Sqlcmd sind zwei großartige Arten, um Ihre Prozesse zu automatisieren.  Es wird empfohlen, diese Tools für die Verwaltung einer großen Anzahl von logischen Servern und Bereitstellen von Ressourcen Änderungen in einer Umgebung für die Herstellung, wie die notwendigen Aufgaben Skript erstellt und anschließend die automatische werden können.

### <a name="dynamic-management-views"></a>Dynamische Management-Ansichten 

DMVs sind die Grundbestandteile des SQL Data Warehouse verwalten. Fast alle Informationen, der im Portal Flächen beruht auf DMVs. Eine Liste der SQL Data Warehouse DMVs finden Sie unter [SQL Data Warehouse Systemansichten][].

Um anzufangen, finden Sie unter [Verbinden und Abfrage mit Sqlcmd][]und [Erstellen einer Datenbank (PowerShell)][].

## <a name="scale-compute"></a>Maßstab berechnen

In SQL Data Warehouse können Sie schnell die Leistung, oder indem Sie wieder steigenden oder abnehmenden berechnen Ressourcen CPU, Arbeitsspeicher und e/a-Bandbreite skalieren. Um die Leistung zu skalieren, Sie müssen, lediglich passen Sie die Anzahl der Datawarehouse Einheiten (DWUs), die die Datenbank SQL Data Warehouse zuweist. SQL Data Warehouse schnell nimmt die Änderung und die zugrunde liegenden Änderungen an der Hardware oder Software behandelt.

Weitere Informationen zum Skalieren DWUs finden Sie unter [Skalieren Leistung][].

##  <a name="pause-and-resume"></a>Anhalten und fortsetzen

Um Kosten zu speichern, können Sie anhalten und fortsetzen berechnen Ressourcen bei Bedarf. Beispielsweise wenn Sie die Datenbank während der Nacht und an Wochenenden verwenden, können Sie zeigen sie diese Zeiten, und es während des Tages fortsetzen. Für DWUs Sie wird nur belastet während die Datenbank angehalten ist.

Weitere Informationen finden Sie unter [berechnen anhalten][]und [fortsetzen zu berechnen][].

## <a name="performance-best-practices"></a>Bewährte Methoden für die Leistung

Wenn Sie mit einer neuen Technologie die ersten Schritte Unternehmen, sparen entdecken die Tipps und Tricks, die beste rechts vom Anfang arbeiten sehr viel Zeit Sie.  Finden Sie bewährte Methoden in vielen unserer Themen.

Viele eine Zusammenfassung der wichtigsten Aspekte bei der Entwicklung Ihrer Arbeitsbelastung finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].

## <a name="query-monitoring"></a>Abfrage für die Überwachung

Manchmal eine Abfrage zu lang ausgeführt wird, aber noch nicht sicher das, die Ursache ist. SQL Data Warehouse enthält dynamische Management Ansichten (DMVs), die Sie verwenden können, um herauszufinden, welche Abfrage zu lange dauert. 

Zum langer Abfragen finden Sie unter [Überwachen Ihrer Arbeitsbelastung DMVs verwenden][].

## <a name="security"></a>Sicherheit

Um ein sicheres System beibehalten möchten, müssen Sie sich gegen jede Art von unbefugtem Zugriff schützen, und in der Benachrichtigung werden. Ein Sicherheitssystem benötigt, um sicherzustellen, dass die Firewall-Regeln an Ort, sodass nur berechtigt sind IP-Adressen eine Verbindung herstellen können. Gemischte Authentifizierung von Benutzeranmeldeinformationen erforderlich. Nachdem ein Benutzer mit der Datenbank verbunden ist, sollte der Benutzer nur Berechtigungen, um eine minimale Anzahl von Aktionen auszuführen sind. Um Daten zu sichern, können Sie die Verschlüsselung verwenden. Außerdem ist es wichtig, dass Überwachung und nachverfolgen, damit Sie Ereignisse alle noch einmal durchgehen können, wenn eine verdächtige Aktivität vorhanden ist.

Informationen zum Verwalten der Sicherheit, Kopf über in der [Übersicht über die Sicherheit][].

## <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Probleme zuverlässigen Backps Ihrer Daten ist ein wesentlicher Bestandteil einer Herstellung-Datenbank. SQL Data Warehouse hält Ihre Daten durch automatisch Sichern Ihrer aktiven Datenbanken in regelmäßigen Abständen sicherer. Diese Sicherungskopien können Sie von Szenarien wiederherstellen, in dem Sie Ihre Daten beschädigt oder versehentlich gelöscht werden, die Daten oder die Datenbank haben.  Für die Planung des Sicherung Aufbewahrungsrichtlinie und zum Wiederherstellen einer Datenbank finden Sie unter [Stellen von Momentaufnahme wieder her][].

## <a name="next-steps"></a>Nächste Schritte
Verwenden von guten Datenbankentwurfs, dass Prinzipien zum Verwalten Ihrer Datenbanken in SQL Data Warehouse erleichtern werden. Um weitere, Kopf über in der [Entwicklung Übersicht][]zu erhalten.

<!--Image references-->

<!--Article references-->
[Erstellen einer SQL Datawarehouse (Azure Portal)]: sql-data-warehouse-get-started-provision.md
[Erstellen Sie eine Datenbank (PowerShell)]: sql-data-warehouse-get-started-provision-powershell
[connection]: sql-data-warehouse-develop-connections.md
[Abfrage SQL Azure Datawarehouse mit Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Verbinden und Abfragen mit sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Übersicht über die Entwicklung]: sql-data-warehouse-overview-develop.md
[Überwachen Sie Ihre Arbeitsbelastung DMVs verwenden]: sql-data-warehouse-manage-monitor.md
[Anhalten berechnen]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Wiederherstellen von snapshot]: sql-data-warehouse-restore-database-overview.md
[Berechnen des Lebenslaufs]: sql-data-warehouse-manage-compute-overview.md#resume-compute-performance-bk
[Maßstab Leistung]: sql-data-warehouse-manage-compute-overview.md#scale-performance-bk
[Übersicht über die Sicherheit]: sql-data-warehouse-overview-manage-security.md
[Bewährte Methoden für SQL Datawarehouse]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse Systemansichten]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure-portal]: http://portal.azure.com/
