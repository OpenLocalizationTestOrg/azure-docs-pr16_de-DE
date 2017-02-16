<properties
    pageTitle="Aktivieren Sie gedehnt Datenbank für eine Datenbank | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren einer Datenbank für gedehnt Datenbank."
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
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="enable-stretch-database-for-a-database"></a>Aktivieren Sie gedehnt Datenbank für eine Datenbank

Wählen Sie zum Konfigurieren einer vorhandenen Datenbank für gedehnt Datenbank **Vorgänge | Dehnen | Aktivieren von** für eine Datenbank in SQL Server Management Studio, um den Assistenten zum **Aktivieren der Datenbank für gedehnt** zu öffnen. Sie können auch die Transact\-SQL gedehnt Datenbank für eine Datenbank aktivieren.

Bei Auswahl von Aufgaben **| Dehnen | Aktivieren von** für eine einzelne Tabelle, und Sie noch nicht die Datenbank für gedehnt Datenbank aktiviert haben, wird der Assistent die Datenbank für gedehnt Datenbank konfiguriert und können Sie die Tabellen im Rahmen des Prozesses auswählen. Führen Sie die Schritte in diesem Thema anstelle der Schritte in [Aktivieren gedehnt Datenbank für eine Tabelle](sql-server-stretch-database-enable-database.md)aus.

Aktivieren der gedehnt Datenbank anhand einer Datenbank oder einer Tabelle Db erfordert\_die Berechtigungen eines Websitebesitzers. Aktivieren der gedehnt Datenbank auf einer Datenbank erfordert auch Steuerelement DATABASE-Berechtigungen aus.

 >   [AZURE.NOTE] Später, wenn Sie gedehnt Datenbank deaktivieren, beachten Sie, dass gedehnt Datenbank deaktivieren, einer Tabelle oder für eine Datenbank das remote-Objekt nicht gelöscht. Wenn Sie die entfernte Tabelle oder die Remotedatenbank löschen möchten, müssen Sie es mithilfe der Azure-Verwaltungsportal ablegen. Der remote-Objekte weiterhin Azure Kosten entstehen, bis Sie manuell löschen.

## <a name="before-you-get-started"></a>Bevor Sie beginnen

-   Bevor Sie eine Datenbank für gedehnt konfigurieren, empfehlen wir, dass die Ausführung der gedehnt Datenbank Advisor zum Identifizieren von Datenbanken und Tabellen, die für gedehnt berechtigt sind. Außerdem identifiziert den gedehnt Datenbank Advisor blockierenden Probleme. Weitere Informationen finden Sie unter [identifizieren Datenbanken und Tabellen für die Datenbank gedehnt](sql-server-stretch-database-identify-databases.md).

-   Überprüfen Sie die [Einschränkungen für Strecken Datenbank](sql-server-stretch-database-limitations.md).

-   Dehnen Datenbank migriert Daten in Azure. Daher müssen Sie ein Azure-Konto und ein Abonnement für Abrechnung haben. Um ein Azure-Konto, [Klicken Sie auf hier](http://azure.microsoft.com/pricing/free-trial/)zu erhalten.

-   Haben Sie Informationen Verbindung, und melden Sie sich, dass Sie zum Erstellen eines neuen Azure-Servers oder einen vorhandenen Azure-Server auswählen müssen.

## <a name="a-nameenabletsqlserveraprerequisite-enable-stretch-database-on-the-server"></a><a name="EnableTSQLServer"></a>Voraussetzung: Aktivieren Sie gedehnt Datenbank, auf dem server
Bevor Sie gedehnt Datenbank anhand einer Datenbank oder einer Tabelle aktivieren können, müssen Sie es auf dem lokalen Server zu aktivieren. Dieser Vorgang erfordert Systemadministrator oder Serveradmin Berechtigungen.

-   Wenn Sie die erforderlichen Administratorberechtigungen verfügen, konfiguriert der Assistenten zum **Aktivieren der Datenbank für gedehnt** den Server für gedehnt.

-   Wenn Sie nicht über die erforderlichen Berechtigungen verfügen, muss ein Administrator die Option manuell durch Ausführen aktivieren **sp\_konfigurieren** , bevor Sie den Assistenten ausführen, oder ein Administrator hat, um den Assistenten auszuführen.

Um gedehnt Datenbank manuell auf dem Server zu aktivieren, führen Sie **sp\_konfigurieren** und aktivieren Sie die Option **remote-Daten archivieren** . Im folgende Beispiel ermöglicht die Option **Archiv remote-Daten** durch Festlegen des Werts 1 an.

```
EXEC sp_configure 'remote data archive' , '1';
GO

RECONFIGURE;
GO
```
Weitere Informationen finden Sie unter [Konfigurieren der remote-Daten archivieren Server-Konfigurations-Option](https://msdn.microsoft.com/library/mt143175.aspx) und [Sp_configure (Transact-SQL)](https://msdn.microsoft.com/library/ms188787.aspx).

## <a name="a-namewizardause-the-wizard-to-enable-stretch-database-on-a-database"></a><a name="Wizard"></a>Aktivieren Sie gedehnt Datenbank in einer Datenbank mit dem Assistenten
Informationen zum gedehnt-Assistenten die Datenbank aktivieren einschließlich der Informationen, die Sie zum eingeben und Optionen aus, die Sie vornehmen, finden Sie unter [Erste Schritte, indem Sie die Datenbank aktivieren gedehnt-Assistenten ausführen](sql-server-stretch-database-wizard.md)müssen.

## <a name="a-nameenabletsqldatabaseause-transact-sql-to-enable-stretch-database-on-a-database"></a><a name="EnableTSQLDatabase"></a>Verwenden Sie Transact\-SQL gedehnt Datenbank auf einer Datenbank aktivieren
Bevor Sie gedehnt Datenbank für einzelne Tabellen aktivieren können, müssen Sie es in der Datenbank aktivieren.

Aktivieren der gedehnt Datenbank anhand einer Datenbank oder einer Tabelle Db erfordert\_die Berechtigungen eines Websitebesitzers. Aktivieren der gedehnt Datenbank auf einer Datenbank erfordert auch Steuerelement DATABASE-Berechtigungen aus.

1.  Bevor Sie beginnen, wählen Sie einen vorhandenen Azure-Server für die Daten, die gedehnt Datenbank migriert werden, oder erstellen Sie einen neuen Azure-Server.

2.  Erstellen Sie eine Firewall-Regel auf dem Azure-Server mit IP-Adressbereich von SQL Server, mit dem SQL Server Kommunikation mit dem Remoteserver.

    Sie können die Werte problemlos finden, die Sie benötigen, und die Firewall-Regel erstellen, indem Sie versuchen, mit dem Azure Server Object Explorer in SQL Server Management Studio (SSMS) herstellen. SSMS hilft Ihnen, die Regel zu erstellen, öffnen das Dialogfeld das bereits die erforderlichen IP-Adresswerte enthält.

    ![Erstellen Sie eine Firewall-Regel in SSMS][FirewallRule]

3.  Zum Konfigurieren einer SQL Server-Datenbank für gedehnt Datenbank verfügt über die Datenbank Datenbank Master Schlüssel zu haben. Die Datenbank Master-Taste sichert die Anmeldeinformationen, die Datenbank gedehnt wird verwendet, um auf die Remotedatenbank verbinden. Hier ist ein Beispiel, eine neue Datenbank Master-Taste erstellt.

    ```tsql
    USE <database>;
    GO

    CREATE MASTER KEY ENCRYPTION BY PASSWORD ='<password>';
    GO
    ```

    Weitere Informationen über die Datenbank Master-Taste finden Sie unter [CREATE MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) und [Schlüssel Master Datenbank erstellen](https://msdn.microsoft.com/library/aa337551.aspx).

4.  Wenn Sie eine Datenbank für gedehnt Datenbank konfigurieren, müssen Sie einen Eintrag für gedehnt Datenbank verwenden für die Kommunikation zwischen den auf lokale SQL Server und dem Azure Remoteserver bereitstellen. Ihnen werden zwei Optionen zur Verfügung.

    -   Sie können ein Administrator-Anmeldeinformationen bereitstellen.

        -   Wenn Sie gedehnt Datenbank aktivieren, indem Sie den Assistenten ausführen, können Sie die Anmeldeinformationen zu diesem Zeitpunkt erstellen.

        -   Wenn Sie beabsichtigen, gedehnt Datenbank aktivieren, indem Sie **ALTER DATABASE**ausführen, müssen Sie die Anmeldeinformationen manuell vor dem Ausführen von **ALTER DATABASE** zum Aktivieren gedehnt Datenbank zu erstellen.

        Hier ist ein Beispiel, einen neuen Eintrag erstellt.

        ```tsql
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>
            WITH IDENTITY = '<identity>' , SECRET = '<secret>';
        GO
        ```

        Weitere Informationen zu den Anmeldeinformationen finden Sie unter [CREATE Datenbank ausgelegte CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx). Erstellen der Anmeldeinformations erfordert ALTER ANY CREDENTIAL Berechtigungen.

    -   Sie können eine partnerverbundkontakte Dienstkontos für SQL Server zur Kommunikation mit dem Remoteserver Azure, wenn die folgenden Punkte alle zutreffen verwenden.

        -   Das Dienstkonto, unter dem die Instanz von SQL Server ausgeführt wird, ist eine Domänenkonto an.

        -   Das Domänenkonto gehört zu einer Domäne, deren Active Directory mit Azure Active Directory verbunden ist.

        -   Der Remoteserver Azure sieht Azure Active Directory-Authentifizierung unterstützt.

        -   Das Dienstkonto, unter dem die Instanz von SQL Server ausgeführt wird, muss als Dbmanager oder Sysadmin-Konto auf dem Remoteserver Azure konfiguriert sein.

5.  Führen Sie zum Konfigurieren einer Datenbank für gedehnt Datenbank den Befehl ALTER DATABASE aus.

    1.  Für das Argument SERVER, geben Sie den Namen eines vorhandenen Azure Servers, einschließlich der `.database.windows.net` Teil des Namens der \- z. B. `MyStretchDatabaseServer.database.windows.net`.

    2.  Das Argument Anmeldeinformationen bieten eine vorhandene Administrator-Anmeldeinformationen ein, oder geben Sie verbundene\_SERVICE\_Konto = ON. Im folgende Beispiel stellt eine vorhandene Anmeldeinformationen.

    ```tsql
    ALTER DATABASE <database name>
        SET REMOTE_DATA_ARCHIVE = ON
            (
                SERVER = '<server_name>',
                CREDENTIAL = <db_scoped_credential_name>
            ) ;
    GO
    ```

## <a name="next-steps"></a>Nächste Schritte
-   [Aktivieren gedehnt Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md) Weitere Tabellen aktivieren.

-   [Monitor gedehnt Datenbank](sql-server-stretch-database-monitor.md) , um den Status der Datenmigration anzuzeigen.

-   [Anhalten Sie und fortsetzen Sie gedehnt Datenbank](sql-server-stretch-database-pause.md)

-   [Verwalten von und Problembehandlung bei gedehnt Datenbank](sql-server-stretch-database-manage.md)

-   [Zusätzliche gedehnt aktiviert Datenbanken](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Siehe auch

[Identifizieren von Datenbanken und Tabellen für gedehnt Datenbank](sql-server-stretch-database-identify-databases.md)

[ALTER Datenbank Festlegen von Optionen (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[FirewallRule]: ./media/sql-server-stretch-database-enable-database/firewall.png
