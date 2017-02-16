<properties 
    pageTitle="XEvent Ereignisdatei Code für SQL-Datenbank | Microsoft Azure" 
    description="Stellt PowerShell und Transact-SQL für eine Stichprobe von zwei Phasen Code, die das Ziel Ereignis-Datei in einem erweiterten Ereignis Azure SQL-Datenbank veranschaulicht. Azure-Speicher ist Bestandteil dieses Szenario erforderlich." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>Ereignis Datei Zielcode für erweiterte Ereignisse in SQL-Datenbank

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Eine vollständige Code-Beispiel möchten für eine robuste Methode zum Erfassen und Berichts-Informationen für ein Ereignis erweiterte.


In Microsoft SQL Server wird das [Ereignisdatei Ziel](http://msdn.microsoft.com/library/ff878115.aspx) verwendet, um das Ereignis Ausgaben in einer lokalen Festplatte-Datei speichern. Solche Dateien sind jedoch nicht verfügbar mit Azure SQL-Datenbank. Verwenden Sie stattdessen die Azure-Speicherdienst zur Unterstützung von dem Ereignisdatei überein.


Dieses Thema enthält ein Codebeispiel zwei Phasen:


- PowerShell, um eine Azure-Speicher Container in der Cloud zu erstellen.

- Transact-SQL:
 - Um den Container Azure-Speicher ein Ereignisdatei Ziel zuweisen.
 - Erstellen und Starten der Veranstaltung, und vieles mehr.


## <a name="prerequisites"></a>Erforderliche Komponenten


- Ein Azure-Konto und Ihr Abonnement. Sie können für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)registrieren.


- Jede Datenbank, die Sie eine Tabelle in erstellen können.
 - Optional können Sie [erstellen eine **AdventureWorksLT** Demo-Datenbank](sql-database-get-started.md) in Minuten.


- SQL Server Management Studio (ssms.exe), idealerweise seine aktuelle monatliche Updateversion. Laden Sie die neuesten ssms.exe aus:
 - Thema mit dem Titel [Herunterladen von SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx)aus.
 - [Einen direkten Link zur Downloadwebsite.](http://go.microsoft.com/fwlink/?linkid=616025)


- Sie müssen die [Azure PowerShell-Module](http://go.microsoft.com/?linkid=9811175) installiert haben.
 - Module stellen die Befehle wie - **Neu-AzureStorageAccount**bereit.


## <a name="phase-1-powershell-code-for-azure-storage-container"></a>Phase 1: PowerShell-Code für Container Azure-Speicher


Diese PowerShell ist Phase 1 der zwei Phasen Code Probe an.

Das Skript beginnt mit Befehle zum Aufräumen erst nach einer vorhergehenden möglich und rerunnable ist.



1. Fügen Sie das Skript PowerShell in einen einfachen Text-Editor, wie z. B. Notepad.exe, und speichern Sie das Skript als Datei mit der Erweiterung **. ps1**.

2. Starten Sie PowerShell ISE als Administrator.

3. Geben Sie dazu aufgefordert werden ein.<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>ein, und drücken Sie dann die EINGABETASTE.

4. Öffnen Sie in der PowerShell ISE Ihre **ps1** -Datei ein. Führen Sie das Skript.

5. Das Skript startet zunächst ein neues Fenster, in dem Sie bei Azure anmelden.
 - Wenn Sie das Skript ohne Unterbrechung Ihrer Sitzung erneut ausführen, müssen Sie die geeignete Option des Befehls **Hinzufügen-AzureAccount** das Kommentieren von.


![PowerShell ISE mit Azure-Modul ist installiert, für die Ausführung von Skripts bereit.][30_powershell_ise]


&nbsp;


```
## TODO: Before running, find all 'TODO' and make each edit!!

#--------------- 1 -----------------------


# You can comment out or skip this Add-AzureAccount
# command after the first run.
# Current PowerShell environment retains the successful outcome.

'Expect a pop-up window in which you log in to Azure.'


Add-AzureAccount

#-------------- 2 ------------------------


'
TODO: Edit the values assigned to these variables, especially the first few!
'

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME'
$policySasExpiryTime = '2016-01-28T23:44:56Z'
$policySasStartTime  = '2015-08-01'


$storageAccountName     = 'gmstorageaccountxevent'
$storageAccountLocation = 'West US'
$contextName            = 'gmcontext'
$containerName          = 'gmcontainerxevent'
$policySasToken         = 'gmpolicysastoken'


# Leave this value alone, as 'rwl'.
$policySasPermission = 'rwl'

#--------------- 3 -----------------------


# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for the current PowerShell environment.'


Select-AzureSubscription -SubscriptionName $subscriptionName


#-------------- 4 ------------------------


'
Clean up the old Azure Storage Account after any previous run, 
before continuing this new run.'


If ($storageAccountName)
{
    Remove-AzureStorageAccount -StorageAccountName $storageAccountName
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString()

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...'

New-AzureStorageAccount `
    -StorageAccountName $storageAccountName `
    -Location           $storageAccountLocation

[System.DateTime]::Now.ToString()

[System.Media.SystemSounds]::Beep.Play()


'
Get the primary access key for your storage account.
'


$primaryAccessKey_ForStorageAccount = `
    (Get-AzureStorageKey `
        -StorageAccountName $storageAccountName).Primary

"`$primaryAccessKey_ForStorageAccount = $primaryAccessKey_ForStorageAccount"

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
'

#--------------- 6 -----------------------


# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
'

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $primaryAccessKey_ForStorageAccount


'Create a container within the storage account.
'


$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context


'Create a security policy to be applied to the SAS token.
'

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime 

'
Generate a SAS token for the container.
'
Try
{
    $sasTokenWithPolicy = New-AzureStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken
}
Catch 
{
    $Error[0].Exception.ToString()
}

#-------------- 7 ------------------------


'Display the values that YOU must edit into the Transact-SQL script next!:
'

"storageAccountName: $storageAccountName"
"containerName:      $containerName"
"sasTokenWithPolicy: $sasTokenWithPolicy"

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
'

'
(Later, return here to delete your Azure Storage account. See the preceding - Remove-AzureStorageAccount -StorageAccountName $storageAccountName)'

'
Now shift to the Transact-SQL portion of the two-part code sample!'

# EOFile
```


&nbsp;


Notieren Sie sich die einige benannten Werten, die das PowerShell-Skript gedruckt wird, wenn er endet. Sie müssen diese Werte in Transact-SQL-Skript bearbeiten, die als Phase 2 folgt.


## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>Phase 2: Transact-SQL-Code, der Azure-Speicher Container verwendet wird


- In diesem Codebeispiel Phase 1 haben Sie ein PowerShell-Skript zum Erstellen eines Containers Azure-Speicher ausgeführt.
- Als Nächstes muss das folgende Transact-SQL-Skript in Phase 2, den Container verwenden.


Das Skript beginnt mit Befehle zum Aufräumen erst nach einer vorhergehenden möglich und rerunnable ist.


PowerShell-Skript gedruckt wenige benannte Werten aus, wenn der Workflow beendet. Sie müssen das Transact-SQL-Skript, um diesen Werten bearbeiten. Suchen Sie **erledigen** in Transact-SQL-Skript, um die Bearbeitungspunkte zu suchen.


1. Öffnen Sie SQL Server Management Studio (ssms.exe).

2. Verbinden Sie mit Ihrer Datenbank Azure SQL-Datenbank.

3. Klicken Sie auf diese Option, um eine neue Abfragebereich zu öffnen.

4. Fügen Sie im Bereich mit das folgende Transact-SQL-Skript.

5. Suchen nach jeder **erledigen** in das Skript, und nehmen Sie die entsprechenden Änderungen vor.

6. Speichern Sie, und führen Sie das Skript.


&nbsp;


> [AZURE.WARNING] Der SAS Schlüsselwert, der von der vorhergehenden PowerShell-Skript generiert möglicherweise beginnen mit einem '?' (Fragezeichen). Wenn Sie die SAS-Taste in das folgende T-SQL-Skript verwenden, müssen Sie *den Zeilenabstand '?' entfernen*. Andernfalls wird Ihre anstrengungen durch Sicherheit blockiert.


&nbsp;


```
---- TODO: First, run the PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.


SET NOCOUNT ON;
GO


----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO


CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO


------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).


IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO


IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO


ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO


-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO


-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount to delete your Azure Storage account!';
GO
```


&nbsp;


Wenn das Ziel nicht anfügen beim Ausführen, müssen Sie beenden und neu starten der Veranstaltung:


```
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


&nbsp;


## <a name="output"></a>Die Ausgabe


Wenn das Transact-SQL-Skript abgeschlossen ist, klicken Sie auf eine Zelle unter der Spaltenüberschrift **Event_data_XML** . Eine **<event>** Element wird angezeigt, die eine UPDATE-Anweisung anzeigt.

Hier ist ein **<event>** Element, die während des Testens generiert wurde:


&nbsp;


```
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```

&nbsp;


Das vorherige Transact-SQL-Skript verwendet die folgende Systemfunktion, um die Event_file zu lesen:

- [Sys.fn_xe_file_target_read_file (Transact-SQL)](http://msdn.microsoft.com/library/cc280743.aspx)


Eine Erläuterung der erweiterte Optionen für die Anzeige von Daten aus der erweiterten Ereignissen ist verfügbar:

- [Erweiterte Anzeigen der Zieldaten aus erweiterten Ereignissen](http://msdn.microsoft.com/library/mt752502.aspx)

&nbsp;


## <a name="converting-the-code-sample-to-run-on-sql-server"></a>Konvertieren der Stichprobe Code für SQL Server ausgeführt


Angenommen Sie, Sie möchten das vorherige Transact-SQL-Beispiel auf Microsoft SQL Server ausgeführt werden.


- Zur Vereinfachung würden Sie vollständig Verwenden des Containers Azure-Speicher mit einer einfachen Datei wie **C:\myeventdata.xel**ersetzen möchten. Auf die lokale Festplatte des Computers, auf dem SQL Server gehostet, würde die Datei geschrieben werden.


- Sie benötigen keine Arten von Transact-SQL-Anweisungen für **MASTER KEY erstellen** und **Anmeldeinformationen erstellen**.


- In der **CREATE EVENT SESSION** -Anweisung in der **ZIELLISTE hinzufügen** -Klausel, ersetzen Sie den Http-Wert zugewiesen vorgenommen **Filename =** mit vollständigen Pfad LIKE **C:\myfile.xel**.
 - Kein Konto Azure-Speicher muss beteiligt sein.


## <a name="more-information"></a>Weitere Informationen


Weitere Informationen zu Konten und Container in der Azure-Speicherdienst finden Sie unter:

- [So verwenden Sie BLOB-Speicher von .NET](../storage/storage-dotnet-how-to-use-blobs.md)
- [Benennen und verweisen auf Container, Blobs und Metadaten](http://msdn.microsoft.com/library/azure/dd135715.aspx)
- [Arbeiten mit den Container Root](http://msdn.microsoft.com/library/azure/ee395424.aspx)
- [Lektion 1: Erstellen einer gespeicherten Zugriffsrichtlinie und eine Signatur gemeinsamen Zugriff auf eine Azure container](http://msdn.microsoft.com/library/dn466430.aspx)
    - [Lektion 2: Erstellen einer SQL Server-Anmeldeinformationen mithilfe einer freigegebenen Access-Signatur](http://msdn.microsoft.com/library/dn466435.aspx)




<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

