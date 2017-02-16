<properties
   pageTitle="Aktivieren Sie die Daten als Transparent Verschlüsselung (TDE) für Dehnen SQL Server-Datenbank auf Azure TSQL | Microsoft Azure"
   description="Aktivieren Sie Transparent Data Verschlüsselung (TDE) für Dehnen SQL Server-Datenbank auf Azure TSQL"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Aktivieren Sie Transparent Data Verschlüsselung (TDE) für Dehnen Datenbank auf Azure (Transact-SQL)
> [AZURE.SELECTOR]
- [Azure-portal](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Transparente Daten Verschlüsselung (TDE) können Sie das Risiko von bösartige Aktivitäten gescannt, indem Sie in Echtzeit und Verschlüsselung von der Datenbank, zugeordneten Sicherungskopien und Transaktionsprotokolldateien bei Rest ohne Änderungen an der Anwendung ausführen.

TDE verschlüsselt die Speicherung eine gesamte Datenbank mithilfe eines symmetrischen Schlüssels des Verschlüsselungsschlüssels Datenbank bezeichnet. Des Verschlüsselungsschlüssels für die Datenbank, die von einer integrierten Serverzertifikat geschützt ist. Das integrierten Serverzertifikat ist eindeutig für jeden Azure-Server. Microsoft dreht diese Zertifikate automatisch mindestens alle 90 Tage. Eine allgemeine Beschreibung TDE finden Sie unter [Transparent Daten Verschlüsselung (TDE)].

##<a name="enabling-encryption"></a>Aktivieren der Verschlüsselung

Zum Aktivieren der TDE für eine Azure-Datenbank, die die Daten gespeichert werden, die aus einer gedehnt aktiviert SQL Server-Datenbank migriert, gehen Sie folgendermaßen vor:

1. Verbinden Sie mit der *master* -Datenbank auf Azure Servers mit der Datenbank, die über einen Benutzernamen, der Administrator oder als Mitglied der Rolle **Dbmanager** in der master-Datenbank
2. Führen Sie die folgende Anweisung Verschlüsselung die Datenbank.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

##<a name="disabling-encryption"></a>Deaktivieren der Verschlüsselung

Zum Deaktivieren von TDE für eine Azure Datenbank, die die Daten gespeichert werden, die aus einer gedehnt aktiviert SQL Server-Datenbank migriert, gehen Sie folgendermaßen vor:

1. Verbinden Sie mit der *master* -Datenbank mithilfe eines Benutzernamens, das ein Administrator oder als Mitglied der Rolle **Dbmanager** in der master-Datenbank ist
2. Führen Sie die folgende Anweisung Verschlüsselung die Datenbank.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

##<a name="verifying-encryption"></a>Überprüfen der Verschlüsselung

Gehen Sie folgendermaßen vor um zu überprüfen, dass die der Verschlüsselungsstatus für eine Azure-Datenbank, die die Daten gespeichert werden aus einer gedehnt aktiviert SQL Server-Datenbank migriert:

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


<!--Anchors-->
[Transparent Data Verschlüsselung (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
