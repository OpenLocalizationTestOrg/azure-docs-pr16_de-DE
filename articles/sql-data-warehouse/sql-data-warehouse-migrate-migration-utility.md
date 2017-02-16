<properties
   pageTitle="Migrieren von: Data Warehouse-Migrationsprogramm | Microsoft Azure"
   description="Migrieren Sie zu SQL Datawarehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="data-warehouse-migration-utility-preview"></a>Datawarehouse Migrationsprogramm (Preview)

> [AZURE.SELECTOR]
- [Herunterladen der Migrationsprogramm][]

Data Warehouse Migration Utility ist ein Tool zum Migrieren von Schema und die Daten aus SQL Server und Azure SQL-Datenbank zu Azure SQL-Data Warehouse. Während der Migration Schema ordnet das Tool automatisch das entsprechende Schema aus der Quelle zum Ziel. Nachdem das Schema migriert wurde, bietet die Tools die Option zum Verschieben von Daten mit automatisch generierten Skripts.

Zusätzlich zur Migration Schema und die Daten bietet dieses Tool die Option zum Generieren von Berichten Kompatibilität die Inkompatibilität zwischen den Quell- und Instanzen zusammenfassen die optimierten Migration verhindern möchten.

## <a name="get-started"></a>Erste Schritte
Als Voraussetzung für die Installation benötigen Sie BCP-Befehlszeilenprogramm Migrationsskripts und zum Anzeigen des Kompatibilitätsbericht Office ausführen. Nach dem Start des Programms an, das heruntergeladen wurde werden Sie aufgefordert, ein standard EULA akzeptieren, bevor Sie das Tool installiert wird.

Darüber hinaus zum Ausführen der Migration Utiliy benötigen Sie die Berechtigungen auf die Datenbank, die von Ihnen zum Migrieren gesuchte folgen: Datenbank erstellen, ALTER ANY DATABASE oder VIEW ANY DEFINITION.

### <a name="launching-the-tool-and-connecting"></a>Starten des Tools und verbinden
Starten Sie das Tool, indem Sie auf das Desktopsymbol nach der Installation wird ein. Beim Öffnen des Tools, werden Sie mit einer Seite ursprüngliche Verbindung aufgefordert, in dem Sie Ihre Quell-als auch für das Migrationstool auswählen können. Zu diesem Zeitpunkt werden SQL Server und Azure SQL-Datenbank als Quellen und SQL Data Warehouse als Ziel unterstützen. Nachdem Sie diese ausgewählt werden Sie aufgefordert, zu Ihrem Quellserver herstellen, indem Sie im Feld Servername ausfüllen und Authentifizierung, und klicken Sie dann auf 'Verbinden'.

Nach der Authentifizierung, zeigt das Tool eine Liste der Datenbanken, die auf dem Server vorhanden sind, die Sie verbunden sind. Sie können die Migration zu beginnen, indem Sie eine Datenbank, die Sie migrieren möchten, und klicken Sie dann auf 'Migrieren ausgewählt' auf.

## <a name="migration-report"></a>Die Migration – Bericht
Das Tool 'Datenbankkompatibilität prüfen' auswählen generieren ein Berichts zusammenfassen alle Objekt Inkompatibilität in der Datenbank von Ihnen die angeforderten migrieren. Eine umfassendere Liste mit einigen der SQL Server-Funktionen, die in SQL Data Warehouse nicht vorhanden ist kann in unserer [Migrationsdokumentation][]gefunden werden. Nachdem der Bericht generiert wird werden Sie möglicherweise zu speichern, und öffnen Sie den Bericht in Excel.

Beachten Sie, dass beim Generieren des Schemas Migration, die meisten Probleme zu wie 'Objekt' akzeptieren, um unmittelbare Migration der Daten angepasst wird. Überprüfen Sie die Änderungen vor, um sicherzustellen, dass Sie nicht weitere Anpassungen vornehmen, bevor Sie das Schema anwenden möchten.

## <a name="migrate-schema"></a>Migrieren von Schemas

Nach dem Herstellen der Verbindung auswählen 'Migrieren Schema' ein Schema Migrationsskript für die ausgewählten Tabellen generiert. Dieses Skript Ports die Struktur der Tabelle, Karten inkompatiblen Daten Dateitypen auf mehrere kompatible Formulare und Sicherheitsanmeldeinformationen und Schemadateien erstellt, wenn diese vom Benutzer in den Einstellungen für die Migration gekennzeichnet ist. Dieser Code für die gezielte SQL Data Warehouse Instanz ausgeführt werden kann, in einer Datei gespeichert, in die Zwischenablage kopiert oder sogar bevor eine weitere Aktion in Zeile bearbeitet.  

Wie oben erwähnt, beim Migrieren von Schema Überprüfen der Migration von, die Änderungen das Tool weist gestaltet und akzeptieren, um sicherzustellen, dass Sie vollständig kennen.  

## <a name="migrate-data"></a>Migrieren von Daten

Sie können durch Klicken auf die Option 'Daten migrieren' BCP-Skripts, die die Daten zunächst in flachen Dateien auf dem Server, verschieben, werden generieren und dann direkt in Ihrem SQL Data Warehouse. Es empfiehlt sich, dieses Verfahren zum Verschieben von kleiner Schritten mit Daten und als Wiederholungsversuche sind nicht integrierten und Fehler möglicherweise auftreten, wenn ein Verlust der Verbindung vorhanden ist. Um dies ausführen zu können, müssen Sie BCP-Befehlszeilenprogramm installiert haben und das Schema für die Daten bereits erstellt wurde.

Nachdem Sie die oben angegebenen Parameter ausgefüllt haben, müssen Sie einfach auf Ausführen Migration klicken und eine Reihe von zwei Pakete an der angegebenen Position generiert werden. Führen Sie die Exportdatei akzeptieren, um Daten aus der Migrationsquelle in flachen Dateien zu exportieren, und führen Sie die Importdatei akzeptieren, um Daten in SQL Data Warehouse zu importieren.

## <a name="next-steps"></a>Nächste Schritte
Jetzt, da Sie einige Daten migriert haben, schauen Sie sich zum [entwickeln][].

<!--Image references-->

<!--Article references-->
[die Migration – Dokumentation]: sql-data-warehouse-overview-migrate.md
[Entwickeln]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Herunterladen der Migrationsprogramm]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
