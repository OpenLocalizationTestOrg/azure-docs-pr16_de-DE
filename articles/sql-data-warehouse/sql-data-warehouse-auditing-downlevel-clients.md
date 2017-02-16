<properties
   pageTitle="SQL Data Warehouse kompatible Clients Unterstützung für die Überwachung von Daten | Microsoft Azure"
   description="Informationen Sie zu SQL Data Warehouse zur Unterstützung für kompatible Clients für die Überwachung von Daten"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="sql-data-warehouse----downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>Unterstützung für SQL Datawarehouse - Clients älterer Versionen für ü und dynamische Daten Masking

[Überwachung](sql-data-warehouse-auditing-overview.md) funktioniert mit SQL-Clients, die TDS Umleitung unterstützen.

Jeder Client die TDS 7.4 implementiert, sollten auch Umleitung unterstützen. Ausnahmen hierzu gehören JDBC 4.0, in dem das Feature für die Umleitung nicht vollständig unterstützt und lästige für Node.JS in die Umleitung wurde nicht implementiert.

Für "Kompatible Clients" sollte h. welche Support TDS Version 7.3 und unter - Server FQDN in der Verbindung Zeichenfolge geändert werden:

Ursprüngliche Server FQDN in der Verbindungszeichenfolge: <*Servername*>. database.windows.net

Geänderte Server FQDN in der Verbindungszeichenfolge: <*Servername*> .database. **sichere**. Windows

Eine Teilliste der "Clients älterer Versionen" umfasst:

- .NET 4.0 und früher,
- ODBC-10.0 und unten.
- JDBC (zwar JDBC TDS 7.4 unterstützt, das TDS Umleitung Feature wird nicht vollständig unterstützt)
- Langwierig (für Node.JS)

**Hinweis:** Der oben aufgeführten Server FDQN Änderung sein auch für die Anwendung einer Richtlinie Überwachung von SQL Server auf ohne erneute für einen Konfigurationsschritt in jeder Datenbank (temporäre Reduzierung) hilfreich.     
