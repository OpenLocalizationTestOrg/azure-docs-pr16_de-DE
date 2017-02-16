<properties
   pageTitle="Aktivieren Sie die Daten als Transparent Verschlüsselung (TDE) für Dehnen SQL Server-Datenbank auf Azure | Microsoft Azure"
   description="Aktivieren Sie die Daten als Transparent Verschlüsselung (TDE) für Dehnen SQL Server-Datenbank auf Azure"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Aktivieren Sie Transparent Data Verschlüsselung (TDE) für Dehnen auf Azure-Datenbank
> [AZURE.SELECTOR]
- [Azure-portal](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Transparent Daten Verschlüsselung (TDE) können Sie das Risiko von bösartige Aktivitäten gescannt, indem Sie in Echtzeit und Verschlüsselung von der Datenbank, zugeordneten Sicherungskopien und Transaktionsprotokolldateien bei Rest ohne Änderungen an der Anwendung ausführen.

TDE verschlüsselt die Speicherung eine gesamte Datenbank mithilfe eines symmetrischen Schlüssels des Verschlüsselungsschlüssels Datenbank bezeichnet. Des Verschlüsselungsschlüssels für die Datenbank, die von einer integrierten Serverzertifikat geschützt ist. Das integrierten Serverzertifikat ist eindeutig für jeden Azure-Server. Microsoft dreht diese Zertifikate automatisch mindestens alle 90 Tage. Eine allgemeine Beschreibung TDE finden Sie unter [Transparent Daten Verschlüsselung (TDE)].

##<a name="enabling-encryption"></a>Aktivieren der Verschlüsselung

Zum Aktivieren der TDE für eine Azure-Datenbank, die die Daten gespeichert werden, die aus einer gedehnt aktiviert SQL Server-Datenbank migriert, gehen Sie folgendermaßen vor:

1. Öffnen Sie die Datenbank im [Azure-portal](https://portal.azure.com)
2. Klicken Sie in der Datenbank Blade auf die Schaltfläche " **Einstellungen** "
3. Wählen Sie die Option **Transparent Data-Verschlüsselung**![][1]
4. Wählen Sie die Einstellung **auf** , und wählen Sie dann auf **Speichern**
![][2]


##<a name="disabling-encryption"></a>Deaktivieren der Verschlüsselung

Zum Deaktivieren von TDE für eine Azure Datenbank, die die Daten gespeichert werden, die aus einer gedehnt aktiviert SQL Server-Datenbank migriert, gehen Sie folgendermaßen vor:

1. Öffnen Sie die Datenbank im [Azure-portal](https://portal.azure.com)
2. Klicken Sie in der Datenbank Blade auf die Schaltfläche " **Einstellungen** "
3. Wählen Sie die Option **Transparent Data-Verschlüsselung**
4. Wählen Sie die Einstellung **Deaktivieren** , und wählen Sie dann auf **Speichern**




<!--Anchors-->
[Transparent Data Verschlüsselung (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
