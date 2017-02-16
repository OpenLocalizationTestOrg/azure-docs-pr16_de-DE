<properties
    pageTitle="Kopieren Sie Daten aus Blob-Speicher mit SQL-Datenbank | Microsoft Azure"
    description="In diesem Lernprogramm erfahren Sie, wie in einer Azure Data Factory Verkaufspipeline Aktivität kopieren verwendet, zum Kopieren von Daten aus dem Blob-Speicher mit SQL-Datenbank."
    Keywords="BLOB Sql, Blob-Speicher Daten kopieren"
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Kopieren Sie Daten aus Blob-Speicher mit SQL-Datenbank mit Daten Factory 
> [AZURE.SELECTOR]
- [Übersicht und erforderliche Komponenten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Assistent zum Kopieren von](data-factory-copy-data-wizard-tutorial.md)
- [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure Ressourcenmanager Vorlage](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


In diesem Lernprogramm erstellen Sie eine Factory Daten mit einer Verkaufspipeline zum Kopieren von Daten aus dem Blob-Speicher mit SQL-Datenbank.

Die Kopie Aktivität führt das Verschieben von Daten in Azure Data Factory. Es wird von einer global verfügbaren Service bereitgestellt, die Daten zwischen verschiedenen Datenspeicher sicheren, zuverlässigen und skalierbare So kopieren können. [Aktivitäten zum Verschieben von Daten](data-factory-data-movement-activities.md) finden Sie im Artikel für Details zur Aktivität kopieren.  

> [AZURE.NOTE] Ein detaillierter Überblick über die Daten Factory-Dienst finden Sie unter [Einführung in Azure Data Factory](data-factory-introduction.md) .

##<a name="prerequisites-for-the-tutorial"></a>Voraussetzungen für das Lernprogramm
Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Azure-Abonnement**.  Wenn Sie ein Abonnement besitzen, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Finden Sie im Artikel [Kostenlose Testversion](http://azure.microsoft.com/pricing/free-trial/) Details aus.
- **Azure-Speicher zu berücksichtigen**. In diesem Lernprogramm verwenden Sie den Blob-Speicher als **Quelle** Datenspeicher. Wenn Sie ein Azure-Speicher-Konto besitzen, finden Sie unter [Speicherkonto erstellen](../storage/storage-create-storage-account.md#create-a-storage-account) die Schritte zum Erstellen einer.
- **SQL Azure-Datenbank**. In diesem Lernprogramm verwenden Sie eine SQL Azure-Datenbank als **Ziel** Datenspeicher. Wenn Sie mit eine SQL Azure-Datenbank besitzen, die Sie in diesem Lernprogramm verwenden können, finden Sie unter [So erstellen und Konfigurieren einer SQL Azure-Datenbank](../sql-database/sql-database-get-started.md) zu erstellen.
- **SQL Server 2012/2014 oder Visual Studio 2013**. Verwenden Sie SQL Server Management Studio oder Visual Studio zum Erstellen einer Beispieldatenbank und die Ergebnisdaten in der Datenbank anzuzeigen.  

## <a name="collect-blob-storage-account-name-and-key"></a>Sammeln Sie Blob-Speicher Kontonamen und Schlüssel 
Sie benötigen den Kontonamen und kontoschlüssel Ihres Kontos Azure-Speicher zum Ausführen dieses Lernprogramms. Notieren Sie den **Kontonamen** und **kontoschlüssel** für Ihr Konto Azure-Speicher.

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Menü links auf **Weitere Dienste** , und wählen Sie **Speicher-Konten**.

    ![Durchsuchen - Speicher-Konten](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\browse-storage-accounts.png)
3. Wählen Sie das **Konto Azure-Speicher** , die Sie in diesem Lernprogramm verwenden möchten, in das Blade **Speicher-Konten** .
4. Wählen Sie **Zugriffstasten** -Link unter **EINSTELLUNGEN**aus.
5.  Klicken Sie auf **Kopieren** (Bild) Schaltfläche neben dem Textfeld **Speicher Kontonamen** und speichern/einfügen sie dort (zum Beispiel: in eine Textdatei).
6. Wiederholen Sie im vorhergehenden Schritt kopieren oder notieren Sie sich die **Schlüssel1**aus.
    
    ![Zugriffstaste Speicher](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\storage-access-key.png)
7. Schließen Sie alle Blades, indem Sie auf **X**.

## <a name="collect-sql-server-database-user-names"></a>Sammeln von SQLServer, die Datenbank, die Benutzernamen
Sie benötigen die Namen der Azure SQL Server-Datenbank und Benutzer Ausführen dieses Lernprogramms. Notieren Sie den Namen der **Server**, **Datenbank**und **Benutzer** für die SQL Azure-Datenbank.

1. Klicken Sie in der **Azure-Portal**klicken Sie auf der linken Seite auf **Weitere Dienste** , und wählen Sie **SQL-Datenbanken**.
2. Wählen Sie in der **SQL-Datenbanken Blade**die **Datenbank** , die Sie in diesem Lernprogramm verwenden möchten. Notieren Sie den **Datenbanknamen**.  
3. Klicken Sie in der **SQL-Datenbank** Blade unter **EINSTELLUNGEN**auf **Eigenschaften** .
4. Beachten Sie unten die Werte für den **SERVERNAMEN** und die **SERVER Administrator LOGIN**.
5. Schließen Sie alle Blades, indem Sie auf **X**.

## <a name="allow-azure-services-to-access-sql-server"></a>Zulassen von Azure Services Zugriff auf SQLServer 
Stellen Sie sicher, dass die **Einstellung für **den Zugriff auf Azure Services** für Ihre SQL Azure-Server aktiviert, damit der Daten Factory-Dienst Ihre SQL Azure-Server zugreifen kann** . Um zu überprüfen, und aktivieren Sie diese Einstellung, führen Sie die folgenden Schritte aus:

1. Klicken Sie auf **Weitere Dienste** -Hub auf der linken Seite, und klicken Sie auf **SQL Server**.
2. Wählen Sie Ihren Server, und klicken Sie unter **EINSTELLUNGEN**auf **Firewall** . 
4. Klicken Sie in das Blade **Firewall-Einstellungen** für den **Zugriff auf Dienste Azure zulassen**auf **auf** .
5. Schließen Sie alle Blades, indem Sie auf **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>Vorbereiten der Blob-Speicher und SQL-Datenbank 
Bereiten Sie nun Ihre Azure Blob-Speicher und SQL Azure-Datenbank für das Lernprogramm, indem Sie die folgenden Schritte ausführen:  

1. Starten Sie den Editor, fügen Sie den folgenden Text, und speichern Sie es als **emp.txt** **C:\ADFGetStarted** Ordner auf Ihrer Festplatte.

        John, Doe
        Jane, Doe

2. Verwenden Sie Tools wie [Azure-Speicher-Explorer](https://azurestorageexplorer.codeplex.com/) aus, um den Container **Adftutorial** erstellen und die **emp.txt** -Datei auf den Container hochzuladen.

    ![Azure-Speicher-Explorer. Kopieren Sie Daten aus Blob-Speicher mit SQL-Datenbank](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. Verwenden Sie das folgende SQL-Skript, um die Tabelle **emp** in Ihrem SQL Azure-Datenbank zu erstellen.  


        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

    **Wenn Sie SQL Server 2012/2014 auf Ihrem Computer installiert haben:** Anweisungen aus [Schritt2: Herstellen einer Verbindung mit der Verwaltung von Azure SQL-Datenbank mit SQL Server Management Studio SQL-Datenbank mit](../sql-database/sql-database-manage-azure-ssms.md#Step2) Artikel, um eine Verbindung mit dem SQL Azure-Server, und führen Sie das Skript SQL. In diesem Artikel wird im [klassischen Azure-Portal](http://manage.windowsazure.com)nicht im [neuen Azure-Portal](https://portal.azure.com)zum Konfigurieren der Firewall für einen SQL Azure-Server verwendet.

    Wenn Sie Ihren Kunden Zugriff auf den SQL Azure-Server nicht zulässig ist, müssen Sie zum Konfigurieren der Firewall für Ihre SQL Azure-Server für den Zugriff von Ihrem Computer (IP-Adresse). Finden Sie [in diesem Artikel](../sql-database/sql-database-configure-firewall-settings.md) Schritte zum Konfigurieren der Firewall für Ihre SQL Azure-Server aus.

Sie haben die erforderlichen Komponenten abgeschlossen. Sie können eine Daten Factory mithilfe einer der folgenden Methoden erstellen. Klicken Sie auf eine der Registerkarten am Anfang oder zum Ausführen des Lernprogramms die folgenden Links.     

- [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Assistent zum Kopieren von](data-factory-copy-data-wizard-tutorial.md)
