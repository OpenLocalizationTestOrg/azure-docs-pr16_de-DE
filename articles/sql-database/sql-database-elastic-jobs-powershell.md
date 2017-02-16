<properties 
    pageTitle="Flexible Datenbank stellen mithilfe der PowerShell erstellen und verwalten" 
    description="PowerShell zum Verwalten von Pools Azure SQL-Datenbank" 
    services="sql-database" documentationCenter=""  
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="create-and-manage-a-sql-database-elastic-database-jobs-using-powershell-preview"></a>Erstellen und Verwalten von einer SQL-Datenbank flexible Datenbankaufträge mithilfe der PowerShell (Preview)

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShell](sql-database-elastic-jobs-powershell.md)



Die PowerShell-APIs für **flexible Datenbank Aufträge** (in der Vorschau), können Sie definieren eine Gruppe von Datenbanken für die Skripts ausgeführt werden. In diesem Artikel wird zum Erstellen und Verwalten von **Aufträgen flexible Datenbank** mithilfe von PowerShell-Cmdlets. Finden Sie unter [Übersicht über die flexible Aufträge](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Erforderliche Komponenten
* Ein Azure-Abonnement. Eine kostenlose Testversion finden Sie [kostenlose Testversion für einen Monat](https://azure.microsoft.com/pricing/free-trial/).
* Eine Reihe von Datenbanken, die mit den Tools flexible Datenbank erstellt werden soll. Finden Sie unter [Erste Schritte mit flexible Datenbanktools](sql-database-elastic-scale-get-started.md).
* Azure PowerShell. Ausführliche Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).
* **Flexible Datenbank Aufträge** PowerShell-Paket: finden Sie unter [Installieren von flexible Datenbank Aufträge](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Wählen Sie Ihr Abonnement Azure

Benennen Sie zum Auswählen des Abonnements benötigen Sie Ihr Abonnement-Id (**-SubscriptionId**) oder Abonnements (**-SubscriptionName**). Wenn Sie über mehrere Abonnements können Sie das Cmdlet " **Get-AzureRmSubscription** " ausführen und kopieren die gewünschten Abonnementinformationen aus dem Resultset verfügen. Nachdem Sie die Informationen zu Ihrem Abonnement haben, führen Sie das folgende Cmdlet um Abonnement als Standard, nämlich das Ziel für das Erstellen und Verwalten von Aufträgen festzulegen:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

Der [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) wird empfohlen, für die Verwendung zu entwickeln und Ausführen von PowerShell-Skripts gegen die Einzelvorgänge flexible Datenbank.

## <a name="elastic-database-jobs-objects"></a>Flexible Datenbankobjekte Aufträge

Die folgende Tabelle enthält, die alle Objekttypen **Flexible Datenbank** Aufträge sowie die zugehörige Beschreibung und relevanten PowerShell-APIs.

<table style="width:100%">
  <tr>
    <th>Objekttyp</th>
    <th>Beschreibung</th>
    <th>Verwandte PowerShell-APIs</th>
  </tr>
  <tr>
    <td>Anmeldeinformationen</td>
    <td>Verwenden Sie bei einer Verbindung mit Datenbanken für die Ausführung von Skripts oder einer Anwendung von DACPACs Benutzername und Kennwort. <p>Vor dem Senden an, und speichern in der Datenbank flexible Datenbankaufträge, ist das Kennwort verschlüsselt.  Das Kennwort wurde vom Dienst flexible Datenbankaufträge über die Anmeldeinformationen erstellt und mit der Installationsskript hochgeladen entschlüsselt.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>Neue AzureSqlJobCredential</p><p>Set-AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Skript</td>
    <td>Transact-SQL-Skript für die Ausführung über Datenbanken verwendet werden.  Das Skript sollte erstellt werden, um Idempotent sein, da der Dienst Ausführung des Skripts bei Fehlern wiederholt wird.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>Neue AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Anwendung auf Datenebene </a> Paket über Datenbanken angewendet werden soll.

    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Neue AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Ziel der Datenbank</td>
    <td>Name der Datenbank und Server auf einer SQL Azure-Datenbank zeigen.

    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Neue AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Shard Karte Ziel</td>
    <td>Kombination aus einer Datenbank Ziel- und Anmeldeinformationen verwendet werden, um Informationen in einer Datenbank flexible Shard Zuordnung zu bestimmen.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Neue AzureSqlJobTarget</p>
    <p>Set-AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Benutzerdefinierte Auflistung Ziel</td>
    <td>Definierte Gruppe von Datenbanken für Ausführung gemeinsam verwendet.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Neue AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Benutzerdefinierte Auflistung untergeordnete Ziel</td>
    <td>Ziel Datenbank, die aus einer benutzerdefinierten Auflistung verwiesen wird.</td>
    <td>
    <p>Hinzufügen von AzureSqlJobChildTarget</p>
    <p>Entfernen-AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>Position</td>
    <td>
    <p>Definition der Parameter für ein Projekt, die Ausführung auslösen oder einen Zeitplan zu erfüllen verwendet werden können.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>Neue AzureSqlJob</p>
    <p>Set-AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>Ausführung des Auftrags</td>
    <td>
    <p>Container von Vorgängen zur Erfüllung entweder Skript ausführen oder eine DACPAC anwenden, um ein Ziel mithilfe von Anmeldeinformationen für die Datenbankverbindungen mit Fehlern erforderlichen verarbeitet in Übereinstimmung mit einer Ausführungsrichtlinie.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start-AzureSqlJobExecution</p>
    <p>Beenden-AzureSqlJobExecution</p>
    <p>Warten-AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Die Ausführung der Aufgabe Position</td>
    <td>
    <p>Einzelne Arbeitseinheit um eine Aufgabe durchführen zu können.</p>
    <p>Wenn eine Aufgabe nicht erfolgreich kann ausführen, die resultierende Ausnahme Nachricht werden protokolliert, und eine neue übereinstimmende Position Aufgabe erstellt wird und in Übereinstimmung mit der Richtlinie angegebenen Datenausführungsverhinderung ausgeführt.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start-AzureSqlJobExecution</p>
    <p>Beenden-AzureSqlJobExecution</p>
    <p>Warten-AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Position Ausführungsrichtlinie</td>
    <td>
    <p>Steuerelemente für Projekt Ausführung Zeitlimit, Höchstwerte und Intervalle zwischen Wiederholungsversuche.</p>
    <p>Flexible Datenbankaufträge enthält eine Position Ausführung Standardrichtlinie die wesentlichen unbegrenzte Wiederholungsversuche Auftrag Vorgang Fehlern mit exponentiellen Backoff der Intervalle zwischen jede Wiederholung verursachen.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>Neue AzureSqlJobExecutionPolicy</p>
    <p>Set-AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Zeitplan</td>
    <td>
    <p>Zeitbasierten Spezifikation für die Ausführung in einem Zugriffsprobleme Intervall oder zu einem bestimmten Zeitpunkt stattfindet.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>Neue AzureSqlJobSchedule</p>
    <p>Set-AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>Position Trigger</td>
    <td>
    <p>Eine Zuordnung zwischen einem Projekt und einen Zeitplan für die Ausführung der Trigger Auftrags gemäß dem Zeitplan.</p>
    </td>
    <td>
    <p>Neue AzureSqlJobTrigger</p>
    <p>Entfernen-AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Unterstützte flexible Datenbank Aufträge gruppieren Typen
Der Auftrag führt Skripts Transact-SQL (T-SQL) oder einer Anwendung von DACPACs über eine Gruppe von Datenbanken. Wenn ein Auftrag gesendet wird, über eine Gruppe von Datenbanken ausgeführt werden soll, der Auftrag "Erweitert" die in untergeordneten Einzelvorgänge, wobei jeweils die angeforderte Ausführung einer einzelnen Datenbank in der Gruppe ausführt. 
 
Es gibt zwei Arten von Gruppen aus, die Sie erstellen können: 

* [Shard Karte](sql-database-elastic-scale-shard-map-management.md) Gruppe: Wenn eine Position auf eine Karte Shard Ziel gesendet wird, der Auftrag fragt die Shard Karte, um deren aktuellen mehrere Shards hinweg bestimmen und erstellt dann untergeordnete Einzelvorgänge für jede Shard in der Shard Karte.
* Benutzerdefinierte Auflistung Gruppe: eine benutzerdefinierte definiert Satz von Datenbanken. Bei eine benutzerdefinierte Auflistung ein Auftrags ausgerichtet ist, erstellt untergeordneten Aufträge für jede Datenbank derzeit in der benutzerdefinierten Auflistung.

## <a name="to-set-the-elastic-database-jobs-connection"></a>Wenn Sie die Datenbank flexible festlegen Aufträge Verbindung

Eine Verbindung muss mit der Einzelvorgänge *Steuerelement Datenbank* vor der Verwendung der Einzelvorgänge APIs festgelegt werden. Dieses Cmdlet ausgeführt, löst ein Fensters Anmeldeinformationen eingeblendet werden anfordern, Benutzername und Kennwort beim Installieren von Aufträge flexible Datenbank erstellt haben. Alle Beispiele finden Sie in diesem Thema wird davon ausgegangen, dass dieser erste Schritt bereits durchgeführt wurde.

Öffnen Sie eine Verbindung mit der Datenbank flexible Aufträge:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a>Verschlüsselte Anmeldeinformationen in der Datenbank flexible Aufträge

Datenbankbenutzers können in der Einzelvorgänge *Steuerelement Datenbank* und das Kennwort verschlüsselt eingefügt werden. Es ist erforderlich, speichern Anmeldeinformationen ein, um Aufträge zu einem späteren Zeitpunkt ausgeführt werden soll (mit Projektpläne) aktivieren.
 
Verschlüsselung arbeitet über ein Zertifikat, das als Teil der Installationsskript erstellt. Das Installationsskript erstellt, und es wird das Zertifikat in der Azure-Cloud-Dienst für die gespeicherten verschlüsselten Kennwörter entschlüsselt hochgeladen. Azure-Cloud-Dienst speichert später im öffentlichen Schlüssel innerhalb der Einzelvorgänge *Steuerelement Datenbank* womit die PowerShell-API oder klassische Azure-Portal Schnittstelle bereitgestellten Kennwort verschlüsselt, ohne dass das Zertifikat lokal installiert werden können.
 
Die Kennwörter Anmeldeinformationen werden verschlüsselt und somit sicher von Benutzern mit schreibgeschützten Zugriff auf flexible Aufträge Datenbankobjekte. Es ist jedoch möglich für einen bösartiger Benutzer mit Lese-und Schreibzugriff auf flexible Datenbankaufträge Objekte zum Extrahieren eines Kennworts. Anmeldeinformationen sind über Auftrag Ausführungen wiederverwendet werden soll. Anmeldeinformationen werden beim Herstellen von Verbindungen Zieldatenbanken übergeben. Es gibt derzeit keine Einschränkungen auf die Zieldatenbanken, die für jeden Eintrag verwendet, bösartiger Benutzer konnte eine Datenbank Ziel für eine Datenbank die bösartiger Benutzer gesteuert hinzuzufügen. Der Benutzer konnte ein Auftrags verwendet, diese Datenbank aus, um die Anmeldeinformationen des Kennworts zu erhalten neu starten.

Sicherheit bewährte Methoden für flexible Datenbank Stellen umfassen:

* Verwendung der APIs auf vertrauenswürdige Personen zu beschränken.
* Anmeldeinformationen sollten die minimal zum Durchführen der Aufgabe Auftrag erforderlichen Berechtigungen verfügen.  Weitere Informationen können in diesem [Autorisierung und Berechtigungen](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN-Artikel angezeigt werden.

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a>So erstellen Sie eine verschlüsselte Anmeldeinformationen für die Ausführung Auftrags über Datenbanken

Um eine neue verschlüsselte Anmeldeinformationen zu erstellen, fordert das [**Cmdlet "Get-Credential"**](https://technet.microsoft.com/library/hh849815.aspx) für einen Benutzernamen und Ihr Kennwort ein, die an den [**neu AzureSqlJobCredential Cmdlet**](https://msdn.microsoft.com/library/mt346063.aspx)übergeben werden kann.

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a>Aktualisieren von Anmeldeinformationen

Wenn Kennwörter ändern, verwenden Sie das [**Cmdlet "Set-AzureSqlJobCredential"**](https://msdn.microsoft.com/library/mt346062.aspx) , und legen Sie den Parameter **CredentialName** .

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a>Definieren einer Datenbank flexible Shard Karte Ziel

Verwenden Sie zum Ausführen eines Auftrags für alle Datenbanken in einer Gruppe von Shard (erstellt mit [flexible Datenbank-Client-Bibliothek](sql-database-elastic-database-client-library.md)) einer Karte Shard als Ziel Datenbank ein. In diesem Beispiel ist eine sharded Anwendung erstellt die flexible Datenbank-Client-Bibliothek erforderlich. Finden Sie unter [Erste Schritte mit flexible Tools Beispieldatenbank](sql-database-elastic-scale-get-started.md).

Die Shard Karte Manager-Datenbank als Ziel der Datenbank festgelegt werden muss, und klicken Sie dann die Karte bestimmte Shard als Ziel angegeben werden muss.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Erstellen Sie ein T-SQL-Skript für die Ausführung über Datenbanken

T-SQL-Skripts für die Ausführung zu erstellen, ist es dringend empfohlen, um diese benutzerspezifisch [Idempotent](https://en.wikipedia.org/wiki/Idempotence) erstellen und robuste gegen Fehler an. Flexible Datenbankaufträge wiederholt Ausführung eines Skripts bei jeder Ausführung ein Fehlers, unabhängig von der Einstufung des Fehlers gefunden.

Verwenden Sie das [**Cmdlet AzureSqlJobContent neu**](https://msdn.microsoft.com/library/mt346085.aspx) erstellen und speichern Sie ein Skript für die Ausführung und die Parameter **Fehlt das ContentName -** und **- CommandText** festlegen.

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Erstellen Sie ein neues Skript aus einer Datei
Wenn das T-SQL-Skript in einer Datei definiert ist, verwenden Sie, um das Skript zu importieren:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a>Ein T-SQL-Skript für die Ausführung Datenbanken Netzwerkservern  

Diese PowerShell-Skript aktualisiert den T-SQL-Befehl für ein vorhandenes Skript.

Legen Sie die folgenden Variablen entsprechend die gewünschten Skriptdefinition festgelegt werden:

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="to-update-the-definition-to-an-existing-script"></a>Die Definition auf ein vorhandenes Skript aktualisieren

    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a>Beim Erstellen eines Auftrags zum Ausführen eines Skripts über eine Shard Karte

Diese PowerShell-Skript startet einen Auftrag für die Ausführung eines Skripts über jede Shard in einer flexible skalieren Shard Zuordnung an.

Legen Sie die folgenden Variablen das gewünschte Skript und Ziel ein:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a>Zur Ausführung eines Auftrags 

Diese PowerShell-Skript führt einen vorhandenen Auftrag aus:

Aktualisieren Sie die folgende Variable entsprechend den Namen der gewünschten Position ausgeführt haben:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="to-retrieve-the-state-of-a-single-job-execution"></a>Zum Abrufen des Zustands des die Ausführung eines einzelnen Auftrags

Verwenden Sie das [**Cmdlet "Get-AzureSqlJobExecution"**](https://msdn.microsoft.com/library/mt346058.aspx) , und legen Sie den Parameter **JobExecutionId** den Status der Ausführung des Auftrags anzeigen.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Verwenden Sie das gleiche **Get-AzureSqlJobExecution** -Cmdlet mit dem Parameter **IncludeChildren** , um den Status der untergeordneten Position Ausführungen, nämlich den genauen Zustand für jede Ausführung des Auftrags für jede Datenbank, indem Sie den Auftrag als Ziel anzuzeigen.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a>Den Status über mehrere Auftrag Ausführungen anzeigen

Das [**Cmdlet "Get-AzureSqlJobExecution"**](https://msdn.microsoft.com/library/mt346058.aspx) verfügt über mehrere optionale Parameter, die zum Anzeigen von mehreren Auftrag Ausführungen, bis der bereitgestellten Parameter gefiltert verwendet werden können. Das folgende Beispiel veranschaulicht einige der möglichen Methoden Get-AzureSqlJobExecution verwenden:

Abgerufen Sie alle aktiven obersten Ebene Auftrag Ausführungen werden:

    Get-AzureSqlJobExecution

Rufen Sie alle obersten Ebene Auftrag Ausführungen, einschließlich inaktive Aufträge Ausführungen ab:

    Get-AzureSqlJobExecution -IncludeInactive

Rufen Sie alle untergeordneten Position Ausführungen einer bereitgestellten Auftrag Ausführung-ID, einschließlich inaktive Aufträge Ausführungen ab:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId –IncludeInactive -IncludeChildren

Alle Auftrag Ausführungen erstellt einen Zeitplan abrufen / Kombination, einschließlich inaktive Aufträge der beruflichen Position:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Abrufen von alle Aufträge eine angegebenen Shard Karte, einschließlich inaktive Aufträge verwendet:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive

Abrufen von alle Aufträge eine angegebene benutzerdefinierte Auflistung, einschließlich inaktive Aufträge verwendet:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive
 
Rufen Sie die Liste der Position Vorgang Ausführungen innerhalb einer bestimmten Position Ausführung:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Abrufen von Auftrag Ausführung Aufgabendetails:

Das folgende PowerShell-Skript kann verwendet werden, können Sie die Details für die Ausführung eines Auftrags Vorgang ist besonders hilfreich, wenn bei der Ausführung Debuggen anzeigen.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="to-retrieve-failures-within-job-task-executions"></a>Zum Abrufen von Fehlern in Auftrag Vorgang Ausführungen

Das **JobTaskExecution-Objekt** enthält eine Eigenschaft für den Lebenszyklus der Aufgabe sowie eine Eigenschaft. Wenn die Ausführung eines Auftrags Vorgang fehlgeschlagen ist, wird die Eigenschaft Lebenszyklus zu *fehlgeschlagen* festgelegt werden und die Nachrichteneigenschaft wird der resultierende Ausnahme Nachricht und deren Stapel festgelegt werden. Wenn ein Auftrag nicht erfolgreich war, ist es wichtig, um die Details der Projektaufgaben anzuzeigen, die für einen bestimmten Auftrag nicht erfolgreich war.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a>Warten Sie für die Ausführung eines Auftrags ausführen

Das folgende PowerShell-Skript kann für die Durchführung einer Position Aufgabe warten verwendet werden:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Erstellen einer Richtlinie für benutzerdefinierte Ausführung

Flexible Datenbankaufträge unterstützt das Erstellen von benutzerdefinierten Ausführungsrichtlinien, die beim Starten von Aufträge angewendet werden können.
  
Ausführungsrichtlinien lässt derzeit für definieren:

* Name: Bezeichner für die Ausführungsrichtlinie.
* Zeitlimit:-Gesamtzeit, bevor Sie ein Projekt durch flexible Datenbankaufträge abgebrochen wird.
* Ursprüngliche Wiederholungsintervalls: Intervall warten, bevor der erste "Wiederholen".
* Maximale Wiederholungsintervalls: Linienende der Intervalle "Wiederholen" zu verwenden.
* Wiederholen Sie Intervall Backoff zurück: Koeffizienten verwendet, um das nächste Intervall zwischen Wiederholungsversuche zu berechnen.  Die folgende Formel verwendet: (Initial Wiederholungsintervalls) * Math.pow ((Intervall Backoff Koeffizienten), (Anzahl der Wiederholungsversuche) – 2). 
* Maximale Versuche: Die maximale Anzahl von "Wiederholen" versucht innerhalb eines Auftrags durchzuführen.

Die Ausführung Standardrichtlinie verwendet die folgenden Werte:

* Name: Ausführung Standardrichtlinie
* Zeitlimit: 1 Woche
* Ursprüngliche Wiederholungsintervalls: 100 Millisekunden
* Maximale Wiederholungsintervalls: 30 Minuten
* Wiederholen Sie Intervall zurück: 2
* Maximale Versuche: 2.147.483.647

Erstellen Sie die gewünschten Ausführungsrichtlinie ein:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Aktualisieren einer benutzerdefinierten Ausführungsrichtlinie

Aktualisieren Sie die gewünschten Ausführungsrichtlinie zu aktualisieren:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
 
## <a name="cancel-a-job"></a>Abbrechen eines Auftrags

Flexible Datenbankaufträge unterstützt Absage Anfragen der Einzelvorgänge.  Wenn flexible Datenbankaufträge eine Absage für ein Projekt, das aktuell ausgeführte erkennt, wird versucht, den Auftrag zu beenden.

Es gibt zwei verschiedene Arten, flexible Datenbankaufträge einen Abbruch ausführen können:

1. Derzeit ausgeführten Aufgaben Abbrechen: Wenn ein Abbruch erkannt wird, während ein Vorgangs derzeit ausgeführt wird, wird ein Abbruch versucht innerhalb der aktuell ausgeführte Aspekt des Vorgangs.  Beispiel: ist eine Abfrage mit langer aktuell durchgeführt werden, wenn ein Abbruch versucht wird, werden beim Versuch, die Abfrage abzubrechen.
2. Abbrechen des Vorgangs Wiederholungsversuche: Wenn ein Abbruch durch den Thread Steuerelement erkannt wird, bevor eine Aufgabe für die Ausführung gestartet wird, der Thread Steuerelement zu vermeiden, starten den Vorgang und die Anfrage deklarieren, als abgebrochen wird.

Wenn ein Auftrag Abbruch für ein Projekt übergeordneten angefordert wird, wird die Kündigung Anforderung für das übergeordnete Projekt sowie für alle untergeordneten Aufträgen beachtet werden.
 
Um eine Absage senden möchten, verwenden Sie das [**Cmdlet AzureSqlJobExecution beenden**](https://msdn.microsoft.com/library/mt346053.aspx) , und legen Sie den Parameter **JobExecutionId** .

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a>Zum Löschen eines Auftrags und Verlauf asynchrone der beruflichen Position

Flexible Datenbankaufträge unterstützt asynchrone Löschen der Aufträge. Ein Projekt zum Löschen markiert werden kann und das System wird der Auftrag und alle zugehörigen Historie löschen, nachdem alle Auftrag Ausführungen für das Projekt abgeschlossen haben. Das System wird nicht automatisch aktiven Auftrag Ausführungen abgebrochen.  

Rufen Sie [**Stopp-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) um aktiven Auftrag Ausführungen abzubrechen.

Damit Auftrag Löschvorgang ausgelöst wird, verwenden Sie das [**Cmdlet AzureSqlJob entfernen**](https://msdn.microsoft.com/library/mt346083.aspx) , und setzen Sie den Parameter **JobName** .

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="to-create-a-custom-database-target"></a>Zum Erstellen eines benutzerdefinierten Datenbank Ziels

Sie können benutzerdefinierte Datenbank Ziele für direkte Ausführung oder für die Aufnahme innerhalb einer benutzerdefinierte Datenbankgruppe definieren. Da **flexible Datenbank Pools** sind noch nicht direkt unterstützt PowerShell-APIs verwenden, können Sie beispielsweise Erstellen einer benutzerdefinierten Datenbank Ziel und eines benutzerdefinierten Datenbank Websitesammlung Ziels das umfasst alle Datenbanken im Pool.

Legen Sie die folgenden Variablen zu wirken sich auf die gewünschte Datenbankinformationen aus:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a>Zum Erstellen eines benutzerdefinierten Datenbank Websitesammlung Ziels

Verwenden Sie das Cmdlet " [**New-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) " zum Definieren eines benutzerdefinierten Datenbank Websitesammlung Ziel um Ausführung in mehreren definierten Datenbank Zielen aktivieren aus. Nach dem Erstellen einer Datenbankgruppe, können Datenbanken das benutzerdefinierte Auflistung Ziel zugeordnet werden.

Legen Sie die folgenden Variablen in der gewünschten benutzerdefinierten Auflistung Zielkonfiguration entsprechen:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a>Hinzufügen von Datenbanken in einer benutzerdefinierten Datenbank Websitesammlung als Ziel

Verwenden Sie zum Hinzufügen eine Datenbank zu einer bestimmten benutzerdefinierten Auflistung das Cmdlet [**AzureSqlJobChildTarget hinzufügen**](https://msdn.microsoft.comlibrary/mt346064.aspx) .

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Überprüfen Sie die Datenbanken in einem benutzerdefinierten Datenbank Websitesammlung Ziel

Verwenden Sie das Cmdlet " [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) ", um die Datenbanken untergeordnetes Element in einem benutzerdefinierten Datenbank Websitesammlung Ziel abzurufen. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Erstellen eines Auftrags zum Ausführen eines Skripts über eine benutzerdefinierte Datenbank Websitesammlung Ziel

Verwenden Sie das Cmdlet [**Neu-AzureSqlJob**](https://msdn.microsoft.com/library/mt346078.aspx) , zum Erstellen eines Auftrags für eine Gruppe von Datenbanken, die von einem benutzerdefinierten Datenbank Websitesammlung Ziel definiert. Flexible Datenbankaufträge werden erweitern Sie den Auftrag in mehreren untergeordneten Einzelvorgänge, die jeweils einer Datenbank entsprechen, das das Ziel des benutzerdefinierten Datenbank Websitesammlung zugeordnet und stellen Sie sicher, dass das Skript in jeder Datenbank ausgeführt wird. In diesem Fall ist es wichtig, dass Skripts Idempotent flexibel in Bezug auf Wiederholungsversuche werden müssen.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Datensammlung in Datenbanken

Sie können ein Projekt zum Ausführen einer Abfrage über eine Gruppe von Datenbanken und senden die Ergebnisse zu einer bestimmten Tabelle verwenden. Die Tabelle kann nach dem Vorfall zum Anzeigen der Abfrageergebnisse von der jeweiligen Datenbank abgefragt werden. Dadurch wird eine asynchrone Methode zum Ausführen einer Abfrage über mehrere Datenbanken. Fehlgeschlagene Versuche werden automatisch über Wiederholungsversuche behandelt.

Die angegebene Zieltabelle wird automatisch erstellt, wenn er noch nicht vorhanden ist. Die neue Tabelle entspricht das Schema der zurückgegebene Ergebnisgruppe. Wenn ein Skript mehrere Ergebnismengen zurückgibt, werden flexible Datenbank Aufträge nur das erste in die Zieltabelle senden.

Das folgende PowerShell-Skript führt eine Skript und sammelt die Ergebnisse in einer angegebenen Tabelle. Dieses Skript wird davon ausgegangen, dass die ein einzelnes Resultset gibt ein T-SQL-Skript erstellt wurde und einer benutzerdefinierten Datenbank Websitesammlung Ziel erstellt wurde.

Dieses Skript verwendet das Cmdlet " [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) ". Festlegen der Parameter für das Skript, Anmeldeinformationen und Ausführungsziel:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a>Erstellen und Starten eines Auftrags für die Websitesammlung Datenszenarien

Dieses Skript verwendet das Cmdlet [**Start-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346055.aspx) an.
 
    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="to-schedule-a-job-execution-trigger"></a>Planen eines Auftrags Ausführung auslösen

Das folgende PowerShell-Skript kann zum Erstellen eines periodischen Zeitplans verwendet werden. [**Neu-AzureSqlJobSchedule**](https://msdn.microsoft.com/library/mt346068.aspx) unterstützt auch - DayInterval, - HourInterval, - MonthInterval, und WeekInterval - Parameter, aber dieses Skript verwendet eine Minute Intervall. Terminpläne, die nur einmal ausgeführt erstellt werden können von Übergabe - einmalige.

Erstellen eines neuen Projektplans an:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a>Zum Auslösen eines Auftrags für einen Zeitplan ausgeführt

Ein Position Trigger kann definiert werden, um eine Position nach einem Zeitplan ausgeführt haben. Das folgende PowerShell-Skript kann zum Erstellen eines Triggers Auftrag verwendet werden.

Verwenden Sie [Neu-AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346069.aspx) , und legen Sie die folgenden Variablen an der gewünschten Position und Zeitplan entsprechen:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    –JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>So entfernen Sie einen geplanten Association So beenden Sie die Position von Zeitplan ausgeführt

Wenn Zugriffsprobleme Ausführung des Auftrags durch einen Trigger Auftrag abbrechen möchten, kann der Trigger Auftrag entfernt werden. Entfernen eines Triggers Auftrag zum Beenden eines Auftrags nach einem Zeitplan mithilfe des [**Entfernen-AzureSqlJobTrigger-Cmdlet**](https://msdn.microsoft.com/library/mt346070.aspx)ausgeführt wird.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a>Abrufen von Auftrag Trigger an einen Zeitplan gebunden

Das folgende PowerShell-Skript kann verwendet werden, abgerufen und die Position Trigger registriert zu einer bestimmten Uhrzeit Terminplan angezeigt.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a>Zum Abrufen von Auftrag Trigger gebunden mit einem Projekt 

Verwenden Sie [Get-AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346067.aspx) abgerufen und Terminpläne mit einer registrierten Position angezeigt.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a>So erstellen Sie eine Anwendung auf Datenebene (DACPAC) für die Ausführung über Datenbanken

Zum Erstellen einer DACPAC finden Sie unter [Datenebene Applications](https://msdn.microsoft.com/library/ee210546.aspx). Um eine DACPAC bereitstellen, verwenden Sie das [Cmdlet "New-AzureSqlJobContent"](https://msdn.microsoft.com/library/mt346085.aspx)ein. Die DACPAC muss für den Dienst zugänglich sein. Es wird empfohlen, eine erstellten DACPAC in Azure-Speicher hochladen und einer [Access-Signatur freigegeben](../storage/storage-dotnet-shared-access-signature-part-1.md) für die DACPAC erstellen.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a>Eine Anwendung auf Datenebene (DACPAC) für die Ausführung Datenbanken Netzwerkservern

Zeigen Sie auf neue URIs können vorhandenen DACPACs innerhalb von Aufträgen für flexible Datenbank registriert aktualisiert werden. Verwenden Sie das [**Cmdlet "Set-AzureSqlJobContentDefinition"**](https://msdn.microsoft.com/library/mt346074.aspx) der DACPAC URI auf eine vorhandene registrierte DACPAC aktualisieren:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a>Beim Erstellen eines Auftrags, um eine Anwendung Datenebene (DACPAC) für Datenbanken übernommen

Nachdem eine DACPAC innerhalb von Aufträgen für flexible Datenbank erstellt wurde, kann ein Auftrags erstellt werden, um die DACPAC für eine Gruppe von Datenbanken übernommen. Das folgende PowerShell-Skript kann zum Erstellen eines DACPAC-Auftrags für eine benutzerdefinierte Sammlung von Datenbanken verwendet werden:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
