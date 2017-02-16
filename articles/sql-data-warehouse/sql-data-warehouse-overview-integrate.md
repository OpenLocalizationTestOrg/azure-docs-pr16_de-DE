<properties
   pageTitle="Integrierte Lösungen mit SQL Data Warehouse erstellen | Microsoft Azure"
   description="Tools und Partner mit Lösungen, die mit SQL Data Warehouse integriert werden soll. "
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
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="leverage-other-services-with-sql-data-warehouse"></a>Nutzen Sie andere Services mit SQL Data Warehouse
Neben dem seine Kernfunktionalität ermöglicht SQL Data Warehouse Benutzer viele der anderen Diensten in Azure entlang es nutzen.  Insbesondere haben wir derzeit tief Integration mit den folgenden Schritten ergriffen:

+ Power BI
+ Factory Azure-Daten
+ Learning Azure-Computern
+ Azure Stream Analytics

Wir arbeiten, um eine Verbindung mit Weitere Dienste direkte Zusammenarbeit Azure im Netzwerk herzustellen.

##<a name="power-bi"></a>Power BI
Power BI-Integration können Sie die Potenz Berechnen des SQL-Data Warehouse mit dynamischen Berichten und Visualisierung von Power BI nutzen. Power BI-Integration umfasst aktuell:

+ **Direkte verbinden**: eine Verbindung mit logischen Pushdown für SQL Data Warehouse erweiterte.  Dadurch schneller Analyse auf einen größeren Wert.
+ **Öffnen Sie in Power BI**: die Schaltfläche 'In Power BI öffnen' übergibt Instanzinformationen zu Power BI, gleicht für eine weitere nahtlose Verbindung.

Finden Sie unter [integrieren mit Power BI](./sql-data-warehouse-integrate-power-bi.md) oder in der [Power BI-Dokumentation](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) Weitere Informationen.

##<a name="azure-data-factory"></a>Factory Azure-Daten
Azure Data Factory ermöglicht Benutzern zu eine verwaltete Plattform komplexe extrahieren-Auslastung Pipelines erstellen.  Data Warehouse SQL-Integration in Azure Data Factory umfassen Folgendes:

+ **Gespeicherte Prozeduren**: die Ausführung von gespeicherten Prozeduren auf SQL Data Warehouse koordinieren.
+ **Kopieren**: Verwenden Sie zum Verschieben von Daten in SQL Data Warehouse ADF.  Dieser Vorgang kann ADFs Standarddaten Bewegung Verfahren oder PolyBase im Hintergrund verwenden. 

Finden Sie unter [integrieren mit Azure Data Factory](./sql-data-warehouse-integrate-azure-data-factory.md) oder in der [Dokumentation Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) für Weitere Informationen.

##<a name="azure-machine-learning"></a>Learning Azure-Computern
Azure maschinellen Learning ist eine vollständig verwaltete Analytics-Dienst, der Benutzer komplizierte Modelle Nutzung von eine umfangreiche Sammlung von Vorhersagen Tools erstellen kann.  SQL Data Warehouse wird als Quelle und Ziel für diese Modelle mit den folgenden Funktionen unterstützt:

+ **Lesen von Daten:** Das Laufwerk bei SQL Data Warehouse mithilfe der T-SQL-Modelle.
+ **Schreiben von Daten:** Übernehmen Sie Änderungen aus einem beliebigen Modell wieder in SQL Data Warehouse.

Finden Sie unter [integrieren mit Azure maschinellen Learning](./sql-data-warehouse-integrate-azure-machine-learning.md) oder der [Azure maschinellen Learning Dokumentation](https://azure.microsoft.com/services/machine-learning/) Weitere Informationen.

##<a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics ist eine komplexe, vollständig verwaltete Infrastruktur für die Verarbeitung und Verarbeitung Ereignisdaten aus Azure Ereignis Hub generiert.  Integration in SQL Data Warehouse kann für das streaming von Daten, um effektiv verarbeitet und entlang relationaler Daten aktivieren tieferen, erweiterte Analyse gespeichert werden.  

+ **Beruflichen Position Ausgabe:** Senden Sie Ausgabe von Stream Analytics Aufträge direkt an SQL Data Warehouse.

Finden Sie unter [integrieren mit Azure Stream Analytics](./sql-data-warehouse-integrate-azure-stream-analytics.md) oder in der [Dokumentation Azure Stream Analytics](https://azure.microsoft.com/documentation/services/stream-analytics/) für Weitere Informationen.

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
