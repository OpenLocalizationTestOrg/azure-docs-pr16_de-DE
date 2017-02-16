<properties
   pageTitle="Verwenden Sie Factory Azure-Daten mit SQL Datawarehouse | Microsoft Azure"
   description="Tipps zur Verwendung von Azure Daten Factory (ADF) mit Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
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

# <a name="use-azure-data-factory-with-sql-data-warehouse"></a>Verwenden von Factory Azure-Daten mit SQL Datawarehouse

Azure Data Factory bietet eine vollständig verwaltete Methode für die Übertragung von Daten und die Ausführung von gespeicherten Prozeduren auf SQL Data Warehouse orchestriert.  Damit können Sie einfacher ansetzen und Zeitplan komplexe extrahieren transformieren und laden (ETL) Prozeduren mit SQL Data Warehouse. Eine genauere Übersicht Azure Data Factory finden Sie unter der [Azure-Daten Factory-Dokumentation][].

## <a name="data-movement"></a>Verschieben von Daten

Azure Data Factory ermöglicht das Verschieben von Daten zwischen verschiedenen Azure-Diensten und lokalen Quellen.  Generelle, aktuelle Integration von Azure Data Factory unterstützt das Verschieben von Daten in die und aus den folgenden Speicherorten:

+ Azure Blob-Speicher
+ SQL Azure-Datenbank
+ Lokalen SQLServer
+ SQLServer auf IaaS

Informationen zum Einrichten einer Kopie Aktivität finden Sie unter [Kopieren von Daten mit Azure Data Factory][]

## <a name="stored-procedures"></a>Gespeicherten Prozeduren
 Auf die gleiche Weise, die sie planen Datenübertragung verwendet werden kann, können auch Azure Data Factory zum Koordinieren der Ausführung von gespeicherten Prozeduren verwendet werden.  Komplexere Pipelines erstellt werden können, und die Berechnung Leistungsfähigkeit von SQL Data Warehouse nutzen Azure Data Factory besser eingeleitet.

## <a name="next-steps"></a>Nächste Schritte
Einen Überblick über die Integration finden Sie unter [Übersicht über die Data Warehouse SQL-Integration][].
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung von SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->

[Kopieren von Daten mit Azure Data Factory]: ../data-factory/data-factory-data-movement-activities.md
[Übersicht über die Entwicklung von SQL Data Warehouse]: ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse Integration (Übersicht)]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory-Dokumentation]:https://azure.microsoft.com/documentation/services/data-factory/

