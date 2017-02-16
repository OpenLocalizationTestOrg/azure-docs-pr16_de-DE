<properties
   pageTitle="Verwenden Redgates Daten Plattform Studio zum Laden von Daten in SQL Data Warehouse | Microsoft Azure"
   description="Informationen Sie zum Verwenden Redgates Daten Plattform Studio für Logistikszenarien Daten."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>


# <a name="load-data-with-redgate-data-platform-studio"></a>Laden Sie die Daten mit Redgate Daten Plattform Studio

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)
- [Daten Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
- [BCP](sql-data-warehouse-load-with-bcp.md)

In diesem Lernprogramm erfahren Sie, wie [Redgates Daten Plattform Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) zum Verschieben von Daten aus einer lokalen SQL Server in Azure SQL-Data Warehouse verwendet. Daten Plattform Studio gilt die am besten geeignete Compatibility Updates und Optimierungen, daher es am schnellsten können ist Sie den ersten Schritten mit SQL Data Warehouse.

> [AZURE.NOTE] [Redgate](http://www.red-gate.com) ist eine seit langem Microsoft-Partner, der verschiedene SQL Server-Tools bietet. Dieses Feature in Daten Plattform Studio wurde für kommerzielle und nicht kommerzielle Nutzung frei zur Verfügung gestellt.

## <a name="before-you-begin"></a>Vorbemerkung
### <a name="create-or-identify-resources"></a>Erstellen oder Identifizieren von Ressourcen

Bevor Sie beginnen in diesem Lernprogramm, müssen Sie haben:

- **Mit lokalen SQL Server-Datenbank**: aus einer lokalen SQL Server stammen, müssen die Daten, die Sie in SQL Data Warehouse importieren möchten (Version 2008 R2 oder höher). Daten Plattform Studio können keine Daten direkt aus einer SQL Azure-Datenbank oder auf Textdateien importieren.
- **Azure-Speicherkonto**: Daten Plattform Studio Stufen die Daten in Azure BLOB-Speicher, bevor Sie geladen werden in SQL Data Warehouse. Das Speicherkonto muss das Modell zur Bereitstellung von "Ressourcenmanager" (die Standardeinstellung) statt der "Klassischen" Modell zur Bereitstellung verwenden. Wenn Sie ein Speicherkonto besitzen, erfahren Sie, wie Sie ein Speicherkonto zu erstellen. 
- **SQL Data Warehouse**: in diesem Lernprogramm verschiebt die Daten aus mit lokalen SQL Server in SQL Data Warehouse, daher Sie ein Datawarehouse online sind müssen. Wenn Sie noch nicht über ein Datawarehouse verfügen, erfahren Sie, wie eine Azure SQL-Data Warehouse erstellen.

> [AZURE.NOTE] Wenn das Speicherkonto und das Datawarehouse in derselben Region erstellt werden, wird die Leistung verbessert.

## <a name="step-1-sign-in-to-data-platform-studio-with-your-azure-account"></a>Schritt 1: Melden Sie bei Daten Plattform Studio mit Ihrem Azure-Konto an
Öffnen Sie Ihren Browser, und navigieren Sie zu der Website [Daten Plattform Studio](https://www.dataplatformstudio.com/) . Melden Sie sich mit dem gleichen Azure-Konto, das Sie zum Erstellen des Speicher-Konto und Data Warehouse verwendet werden soll. Wenn Ihre e-Mail-Adresse mit einer Arbeit oder Schule Konto und einem Microsoft-Konto verknüpft ist, müssen Sie das Konto auszuwählen, das auf Ihre Ressourcen zugreifen können.

> [AZURE.NOTE] Ist dies zum ersten Mal Daten Plattform Studio verwenden, werden Sie aufgefordert, nach der Berechtigung zum Verwalten Ihrer Azure Ressourcen der Anwendung.

## <a name="step-2-start-the-import-wizard"></a>Schritt 2: Starten der Import-Assistent
Wählen Sie aus dem Hauptbildschirm DPS importieren Azure SQL-Data Warehouse Link zu den Importassistenten zu starten.

![][1]

## <a name="step-3-install-the-data-platform-studio-gateway"></a>Schritt 3: Installieren Sie das Daten Plattform Studio Gateway
Um eine Verbindung mit lokalen SQL Server-Datenbank mit müssen Sie das Gateway DPS installieren. Das Gateway ist ein Client-Agent, der bietet Zugriff auf Ihre Umgebung lokal, extrahiert die Daten, und lädt diese in Ihr Speicherkonto. Ihre Daten durchläuft nie Redgates Servers. Um das Gateway zu installieren:

1.  Klicken Sie auf den Link **Gateway erstellen**
2. Herunterladen Sie und installieren Sie das Gateway mit dem bereitgestellten installer

![][2]

> [AZURE.NOTE] Das Gateway kann auf jedem Computer mit Netzwerkzugriff auf die Quelldatenbank von SQL Server installiert werden. Es greift auf SQL Server-Datenbank mithilfe der Windows-Authentifizierung mit den Anmeldeinformationen des aktuellen Benutzers.

Nach der Installation ändert sich des Status des Gateways auf verbunden, und Sie können wählen Sie weiter.

## <a name="step-4-identify-the-source-database"></a>Schritt 4: Identifizieren der Quelldatenbank
Geben Sie im Textfeld *Servername Geben Sie* den Namen des Servers, der die Datenbank hostet, und wählen Sie **Weiter**aus. Klicken Sie dann im Dropdown-Menü, wählen Sie die Datenbank, der Sie Daten importieren möchten.

![][3]

DPS untersucht die ausgewählte Datenbank für die zu importierenden Tabellen. Standardmäßig importiert DPS alle Tabellen in der Datenbank. Sie können aktivieren oder deaktivieren Tabellen, indem Sie den Link alle Tabellen zu erweitern. Wählen Sie die Schaltfläche Weiter nach vorne verschieben.

## <a name="step-5-choose-a-storage-account-to-stage-the-data"></a>Schritt 5: Wählen Sie ein Speicherkonto, um die Daten Phaseneigenschaften
DPS fordert Sie nach einem Speicherort für die Daten Phaseneigenschaften. Wählen Sie ein vorhandenes Speicherkonto aus Ihrem Abonnement, und wählen Sie **Weiter**aus.

> [AZURE.NOTE] DPS einen neuen Blob-Container in der ausgewählten Speicherkonto erstellen und Verwenden eines distinct Ordners für jede importieren.

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Schritt 6: Wählen Sie eine Datawarehouse
Wählen Sie als Nächstes importieren die Daten in einer online [Azure SQL-Data Warehouse](http://aka.ms/sqldw) -Datenbank aus. Nachdem Sie Ihre Datenbank ausgewählt haben, müssen Sie geben Sie die Anmeldeinformationen zum Verbinden mit der Datenbank, und wählen Sie **Weiter**aus.

![][5]

> [AZURE.NOTE] DPS werden Quelldatentabellen in das Datawarehouse zusammengeführt. DPS Sie gewarnt werden, wenn der Tabellenname erfordert vorhandene Tabellen im Datawarehouse überschrieben werden sollen. Sie können optional vorhandenen Objekte im Datawarehouse löschen, indem an zu laufen, Löschen aller vorhandene Objekten vor dem Importieren.

## <a name="step-7-import-the-data"></a>Schritt 7: Importieren der Daten
DPS bestätigt, dass Sie die Daten importieren möchten. Klicken Sie einfach auf die Schaltfläche Start importieren, um die Daten importieren zu beginnen.

![][6]

DPS zeigt eine Visualisierung, die mit dem Status extrahieren und Hochladen der Daten aus SQL Server lokal und den Fortschritt des Imports in SQL Data Warehouse angezeigt.

![][7]

Nach Abschluss des Importvorgangs zeigt DPS eine Zusammenfassung des Datenimports und Kompatibilität Updates, die durchgeführt wurden, einen Bericht zu ändern.

![][8]

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie Ihre Daten in SQL Data Warehouse untersuchen, Sie zunächst anzeigen:

- [Abfrage SQL Azure Datawarehouse (Visual Studio)][]
- [Visualisieren von Daten mit Power BI][]

Erfahren Sie mehr über Redgates Daten Plattform Studio:

- [Besuchen Sie die DPS-homepage](http://www.dataplatformstudio.com/)
- [Schauen Sie sich eine Demo zu DPS auf Kanal 9](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Übersicht über weitere Methoden zum Migrieren, und Laden Ihre Daten im SQL Data Warehouse finden Sie unter:

- [Migrieren von Ihrer Lösung zu SQL Data Warehouse][]
- [Laden von Daten in Azure SQL-Data Warehouse](./sql-data-warehouse-overview-load.md)

Weitere Hinweise zur Entwicklung finden Sie unter der [Übersicht über die Entwicklung von SQL Data Warehouse](./sql-data-warehouse-overview-develop.md).

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Abfrage SQL Azure Datawarehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Visualisieren von Daten mit Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Migrieren von Ihrer Lösung zu SQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md