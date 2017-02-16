## <a name="next-steps"></a>Nächste Schritte
Nachdem Azure-Taste Tresor Integration aktiviert haben, können Sie auf der SQL-VM SQL Server-Verschlüsselung aktivieren. Zuerst müssen Sie eine asymmetrische innerhalb der wichtigsten Tresor und einem symmetrischen Schlüssel innerhalb von SQL Server Ihre virtuellen Computers zu erstellen. Klicken Sie dann werden Sie kann T-SQL-Anweisungen, um die Verschlüsselung der Datenbanken und Sicherungen aktivieren ausgeführt.

Es gibt verschiedene Arten von Verschlüsselung, die Sie nutzen können:

- [Transparent Data Verschlüsselung (TDE)](https://msdn.microsoft.com/library/bb934049.aspx)
- [Verschlüsselte Sicherungskopien](https://msdn.microsoft.com/library/dn449489.aspx)
- [Spalte Ebene Verschlüsselung (Gitternetz)](https://msdn.microsoft.com/library/ms173744.aspx)

Die folgenden Transact-SQL-Skripts stellen Beispiele für jeden der folgenden Bereiche.

>[AZURE.NOTE] Jede Beispiel basiert auf den beiden Angaben gemacht: als asymmetrische Schlüssel aus Ihrem Key Tresor **CONTOSO_KEY** aufgerufen und eine Funktion für die Integration von AKV erstellte Anmeldeinformationen bezeichnet **Azure_EKM_TDE_cred**.

### <a name="transparent-data-encryption-tde"></a>Transparent Data Verschlüsselung (TDE)
1. Erstellen Sie eine SQL Server-Anmeldung für TDE durch die Datenbank-Engine verwendet werden, und fügen Sie die Anmeldeinformationen hinzu.
    
        USE master;
        -- Create a SQL Server login associated with the asymmetric key 
        -- for the Database engine to use when it loads a database 
        -- encrypted by TDE.
        CREATE LOGIN TDE_Login 
        FROM ASYMMETRIC KEY CONTOSO_KEY;
        GO
        
        -- Alter the TDE Login to add the credential for use by the 
        -- Database Engine to access the key vault
        ALTER LOGIN TDE_Login 
        ADD CREDENTIAL Azure_EKM_TDE_cred;
        GO
    
2. Erstellen des Datenbank Schlüssels, der für TDE verwendet werden soll.
    
        USE ContosoDatabase;
        GO
        
        CREATE DATABASE ENCRYPTION KEY 
        WITH ALGORITHM = AES_128 
        ENCRYPTION BY SERVER ASYMMETRIC KEY CONTOSO_KEY;
        GO
        
        -- Alter the database to enable transparent data encryption.
        ALTER DATABASE ContosoDatabase 
        SET ENCRYPTION ON;
        GO

### <a name="encrypted-backups"></a>Verschlüsselte Sicherungskopien
1. Erstellen Sie eine SQL Server-Anmeldung von der Datenbank-Engine für die Verschlüsselung von Sicherungskopien verwendet werden soll, und fügen Sie die Anmeldeinformationen hinzu.
    
        USE master;
        -- Create a SQL Server login associated with the asymmetric key 
        -- for the Database engine to use when it is encrypting the backup.
        CREATE LOGIN Backup_Login 
        FROM ASYMMETRIC KEY CONTOSO_KEY;
        GO 
        
        -- Alter the Encrypted Backup Login to add the credential for use by 
        -- the Database Engine to access the key vault
        ALTER LOGIN Backup_Login 
        ADD CREDENTIAL Azure_EKM_Backup_cred ;
        GO
    
2. Sichern der Datenbank angeben-Verschlüsselung mit der asymmetrische Schlüssel im Key Tresor gespeichert.
    
        USE master;
        BACKUP DATABASE [DATABASE_TO_BACKUP]
        TO DISK = N'[PATH TO BACKUP FILE]' 
        WITH FORMAT, INIT, SKIP, NOREWIND, NOUNLOAD, 
        ENCRYPTION(ALGORITHM = AES_256, SERVER ASYMMETRIC KEY = [CONTOSO_KEY]);
        GO

### <a name="column-level-encryption-cle"></a>Spalte Ebene Verschlüsselung (Gitternetz)
Dieses Skript erstellt einen symmetrischen Schlüssel durch den asymmetrische Schlüssel im Key Tresor geschützt, und klicken Sie dann verwendet diesen zum Verschlüsseln der Daten in der Datenbank.

    CREATE SYMMETRIC KEY DATA_ENCRYPTION_KEY
    WITH ALGORITHM=AES_256
    ENCRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;
    
    DECLARE @DATA VARBINARY(MAX);
    
    --Open the symmetric key for use in this session
    OPEN SYMMETRIC KEY DATA_ENCRYPTION_KEY 
    DECRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;
    
    --Encrypt syntax
    SELECT @DATA = ENCRYPTBYKEY(KEY_GUID('DATA_ENCRYPTION_KEY'), CONVERT(VARBINARY,'Plain text data to encrypt'));
    
    -- Decrypt syntax
    SELECT CONVERT(VARCHAR, DECRYPTBYKEY(@DATA));
    
    --Close the symmetric key
    CLOSE SYMMETRIC KEY DATA_ENCRYPTION_KEY;

## <a name="additional-resources"></a>Zusätzliche Ressourcen
Weitere Informationen zum Verwenden dieser Verschlüsselungsfeatures finden Sie unter [EKM mit SQL Server-Verschlüsselungsfunktionen verwenden](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).

Beachten Sie, dass die Schritte in diesem Artikel wird davon ausgegangen, dass Sie bereits auf SQL Server ein Azure-virtuellen Computern haben. Wenn dies nicht der Fall ist, finden Sie unter [Bereitstellen einer SQL Server-virtuellen Computern in Azure](../articles/virtual-machines/virtual-machines-windows-portal-sql-server-provision.md). Andere Anleitung zum Ausführen von SQL Server auf Azure-virtuellen Computern finden Sie unter [SQL Server auf Azure-virtuellen Computern Übersicht](../articles/virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md).
