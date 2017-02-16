<properties
   pageTitle="SQL Azure-Datenbank Allgemein Einschränkungen und Richtlinien"
   description="Diese Seite enthält einige allgemeinen Einschränkungen für Azure SQL-Datenbank als auch Bereiche Interoperabilität und Support."
   services="sql-database"
   documentationCenter="na"
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/06/2016"
   ms.author="carlrab" />

# <a name="azure-sql-database-general-limitations-and-guidelines"></a>SQL Azure-Datenbank Allgemein Einschränkungen und Richtlinien

Dieses Thema bietet allgemeine Einschränkungen und Richtlinien für Azure SQL-Datenbank. Ein umfassender Überblick Kontingente, ressourcenverwaltung und Unterstützung finden Sie unter den [zusätzlichen Ressourcen](#additional-guidelines) am Ende dieses Themas.

## <a name="connectivity-and-authentication"></a>Konnektivität und Authentifizierung

  - Windows-Authentifizierung wird nicht unterstützt. Finden Sie unter [Verwalten von Datenbanken und Anmeldedaten in SQL Azure-Datenbank](sql-database-manage-logins.md). Mit bestimmten Einschränkungen wird jedoch die Azure-Active Directory-Authentifizierung unterstützt. Finden Sie unter [Verbinden mit SQL-Datenbank mit Azure-Active Directory-Authentifizierung](sql-database-aad-authentication.md).

  - Microsoft Azure SQL-Datenbank unterstützt Stream (TDS) Protocol-Client, Version 7.3 oder höher Tabellendaten.

  - Nur TCP/IP Verbindungen zulässig sind.

  - SQL Server 2008, SQL Server-Browser wird nicht unterstützt, da Microsoft Azure SQL-Datenbank nicht dynamische Ports, nur Port 1433 verfügt.

## <a name="sql-server-agentjobs"></a>SQL Server-Agent/Aufträge

Microsoft Azure SQL-Datenbank unterstützt keine SQL Server-Agent, doch Sie flexible Aufträge zum Ausführen der Aufträge über zu viele Datenbanken verwenden können. Weitere Informationen zu flexible Aufträgen finden Sie unter [flexible Aufträge](sql-database-elastic-jobs-overview.md).

## <a name="sql-server-collation-support"></a>Unterstützung für die Sortierung von SQL Server

Die standardmäßige Sortierung verwendeten Microsoft Azure SQL-Datenbank ist **SQL_LATIN1_GENERAL_CP1_CI_AS**, wobei **LATIN1_GENERAL** Englisch (USA), **CP1** ist Codepage 1252, **CI** wird Groß-und Kleinschreibung und **Akzenten liegt** . Es ist nicht möglich, die Sortierung für V12 Datenbanken zu ändern. Weitere Informationen dazu, wie Sie die Sortierung festgelegt werden finden Sie unter [Sortieren (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="naming-requirements"></a>Benennungsanforderungen

Bestimmte Benutzernamen sind aus Sicherheitsgründen nicht zulässig. Sie können nicht die folgenden Namen verwenden:

 - **Administrator**
 - **Administrator**
 - **Gast**
 - **Quadratwurzel**
 - **SA**

Namen für alle neuen Objekte müssen den SQL Server-Regeln für Bezeichner entsprechen. Weitere Informationen finden Sie unter [Bezeichner](https://msdn.microsoft.com/library/ms175874.aspx).

Darüber hinaus können keine Anmeldung und Benutzernamen enthalten die \ Zeichen (Windows-Authentifizierung wird nicht unterstützt).

## <a name="additional-guidelines"></a>Weitere Richtlinien

- Zusätzlich zu den allgemeinen Einschränkungen in diesem Artikel beschriebenen unterhält SQL-Datenbank an bestimmte Serverressourcen und Einschränkungen basierend auf Ihrem **Dienstebene**. Übersicht der Dienst leisten finden Sie unter [SQL-Datenbank-Dienst Ebenen](sql-database-service-tiers.md).

- Weitere Beschränkungen SQL-Datenbank finden Sie unter [Azure SQL-Datenbank Ressource Grenzwerte](sql-database-resource-limits.md).

- Sicherheit verwandte Richtlinien finden Sie unter [Azure SQL-Datenbank Sicherheitsrichtlinien und Einschränkungen](sql-database-security-guidelines.md).

- Eine andere verwandter Bereich umgibt die Kompatibilität, die Azure SQL-Datenbank mit lokalen Versionen von SQL Server, wie etwa SQL Server 2014 und SQL Server 2016 hat. Die neueste Version von V12 Azure SQL-Datenbank weist viele Verbesserungen in diesem Bereich vorgenommen werden. Weitere Informationen hierzu finden Sie unter [Was ist neu in der SQL-Datenbank V12](sql-database-v12-whats-new.md).

- Informationen über die Verfügbarkeit von Treibern und Unterstützung für SQL-Datenbank finden Sie unter [Datenverbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md).
