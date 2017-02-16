<properties
   pageTitle="Transparent Data Verschlüsselung in SQL Datawarehouse (T-SQL) | Microsoft Azure"
   description="Transparent Data Verschlüsselung (TDE) in SQL Datawarehouse (T-SQL)"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="get-started-with-transparent-data-encryption-tde"></a>Erste Schritte mit transparenten Daten Verschlüsselung (TDE)


> [AZURE.SELECTOR]
- [Übersicht über die Sicherheit](sql-data-warehouse-overview-manage-security.md)
- [Authentifizierung](sql-data-warehouse-authentication.md)
- [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
- [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Erforderliche Berechtigungen

Wenn als Transparent Daten Verschlüsselung (TDE) aktivieren möchten, müssen Sie ein Administrator oder als Mitglied der Rolle Dbmanager sein.

## <a name="enabling-encryption"></a>Aktivieren der Verschlüsselung

Wie folgt vor, um TDE für ein SQL Data Warehouse zu aktivieren:

1. Verbinden Sie mit der *master* -Datenbank auf dem Server mit der Datenbank, die über einen Benutzernamen, der Administrator oder als Mitglied der Rolle **Dbmanager** in der master-Datenbank
2. Führen Sie die folgende Anweisung Verschlüsselung die Datenbank.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Deaktivieren der Verschlüsselung

Wie folgt vor, um für eine SQL Data Warehouse TDE zu deaktivieren:

1. Verbinden Sie mit der *master* -Datenbank mithilfe eines Benutzernamens, das ein Administrator oder als Mitglied der Rolle **Dbmanager** in der master-Datenbank ist
2. Führen Sie die folgende Anweisung Verschlüsselung die Datenbank.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [AZURE.NOTE] Ein angehalten SQL Data Warehouse muss vor der Änderung an den Einstellungen TDE fortgesetzt werden.

## <a name="verifying-encryption"></a>Überprüfen der Verschlüsselung

Zum Überprüfen der Verschlüsselungsstatus für ein SQL Data Warehouse, führen Sie die folgenden Schritte aus:

1. Verbinden Sie mit der Datenbank *master* oder Instanz, die über einen Benutzernamen, der Administrator oder als Mitglied der Rolle **Dbmanager** in der master-Datenbank
2. Führen Sie die folgende Anweisung Verschlüsselung die Datenbank.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Ergebnis ```1``` zeigt an, eine verschlüsselte Datenbank, ```0``` zeigt eine nicht verschlüsselte Datenbank an.

## <a name="encryption-dmvs"></a>Verschlüsselung DMVs  

- [Sys.Databases][] 
- [Sys.dm_pdw_nodes_database_encryption_keys][]


<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[Sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[Sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
