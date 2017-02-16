<properties 
    pageTitle="Verwenden Sie für Test- und Azure Speicheremulator | Microsoft Azure" 
    description="Azure Speicheremulator bietet eine kostenlose lokale Entwicklung-Umgebung für die Entwicklung und Tests mit Azure-Speicher. Informationen Sie zu Speicheremulator, einschließlich wie Anfragen authentifiziert werden, Herstellen einer Verbindung mit dem Emulator aus Ihrer Anwendung und so das Befehlszeile Tool verwenden." 
    services="storage" 
    documentationCenter="" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>
<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="tamram"/>

# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>Verwenden Sie für Test- und Azure Speicheremulator

## <a name="overview"></a>(Übersicht)

Microsoft Azure Speicheremulator bietet eine lokale größerem Funktionsumfang, die die Tabelle, Warteschlange und Azure Blob Dienste hinsichtlich der Entwicklung emuliert. Mit Speicheremulator können Sie Ihre Anwendung mit lokal, die Speicherdienste testen, ohne Erstellen eines Azure-Abonnements oder alle Kosten anfallen. Wenn Sie einverstanden, wie eine Anwendung im Emulator vertraut ist, können Sie bei der Verwendung von einem Konto Azure-Speicher in der Cloud wechseln.

> [AZURE.NOTE] Speicheremulator steht im Rahmen des [Microsoft Azure SDK](https://azure.microsoft.com/downloads/). Sie können auch mit dem [eigenständigen Installer](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409)Speicheremulator installieren. Um Speicheremulator konfigurieren zu können, müssen Sie auf dem Computer über Administratorrechte verfügen.
> 
> Speicheremulator wird derzeit nur unter Windows ausgeführt werden.
>  
> Beachten Sie, dass die Daten in eine Version von Speicheremulator erstellten nicht zugegriffen werden, wenn eine andere Version gewährleistet ist. Wenn Sie Ihre Daten für langfristig beibehalten werden müssen, empfiehlt es sich, dass Sie die Daten in einem Konto Azure-Speicher, sondern in Speicheremulator speichern.

## <a name="how-the-storage-emulator-works"></a>Funktionsweise der Speicheremulator
 
Speicheremulator verwendet eine lokale Microsoft SQL Server-Instanz und im lokalen Dateisystem Azure-Speicherdienste emuliert werden sollen. Standardmäßig wird der Speicheremulator eine Datenbank in Microsoft SQL Server 2012 Express LocalDB verwendet.  Sie können auch Speicheremulator Zugriff auf eine lokale Instanz von SQL Server statt auf die Instanz LocalDB konfigurieren. Weitere Informationen finden Sie unter [Start- und Initialisierung Speicheremulator](#start-and-initialize-the-storage-emulator) unter.

Sie können SQL Server Management Studio Express zum Verwalten Ihrer Installations LocalDB installieren. Speicheremulator eine Verbindung mit SQL Server oder LocalDB mithilfe der Windows-Authentifizierung. 

Einige in Funktionalität Unterschiede zwischen der Speicheremulator und Azure-Speicherservices. Weitere Informationen zu diesen Unterschieden finden Sie unter [Unterschiede zwischen der Speicheremulator und Azure-Speicher](#differences-between-the-storage-emulator-and-azure-storage).

## <a name="authenticating-requests-against-the-storage-emulator"></a>Abfragen Speicheremulator Authentifizierung

Wie bei Azure-Speicher in der Cloud, muss jeder Anforderung, die Sie anhand der Speicheremulator vornehmen authentifiziert werden, wenn sie eine anonyme Anforderung ist. Sie können Anfragen gegen Speicheremulator mithilfe der freigegebenen Schlüssel-Authentifizierung oder mit einer freigegebenen Access-Signatur (SAS) authentifizieren.

### <a name="authentication-with-shared-key-credentials"></a>Authentifizierung mit freigegebenen Schlüssel Anmeldeinformationen

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Weitere Informationen hierzu auf Verbindungszeichenfolgen finden Sie unter [Konfigurieren von Azure Speicher Verbindungszeichenfolgen](storage-configure-connection-string.md). 

### <a name="authentication-with-a-shared-access-signature"></a>Authentifizierung mit einer freigegebenen Access-Signatur 

Einige Azure-Speicher Client-Bibliotheken, beispielsweise die Bibliothek Xamarin unterstützt nur Authentifizierung mit einem gemeinsamen Zugriff Signatur (SAS) Token. Sie müssen zum Erstellen dieses SAS Tokens mit einem Tool oder einer Anwendung, die freigegebene Schlüssel-Authentifizierung unterstützt. Eine einfache Möglichkeit zum Generieren des SAS Tokens ist über Azure PowerShell:

1. Installieren Sie Azure PowerShell, wenn Sie noch nicht geschehen ist. Verwenden die neueste Version von der Azure-PowerShell-Cmdlets wird empfohlen. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md#Install) installationsanweisungen.

2. Öffnen Sie Azure PowerShell zu, und führen Sie die folgenden Befehle. Denken Sie daran, *Kontoname* ersetzen und *ACCOUNT_KEY ==* mit Ihren eigenen Anmeldeinformationen. Ersetzen Sie *CONTAINER_NAME* durch einen Namen Ihrer Wahl.

        $context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="
        
        New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context
        
        $now = Get-Date 
        
        New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri

Die resultierende gemeinsamen Zugriff Signatur URI für den neuen Container sollten ähnlich wie der folgende aus:

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss

Freigegebene Access Signatur erstellt, die mit diesem Beispiel gilt für einen Tag. Die Signatur gewährt Vollzugriff (d. h. lesen, schreiben, löschen, Liste) auf Blobs innerhalb des Containers.

Weitere Informationen zum gemeinsamen Zugriff Signaturen finden Sie unter [Verwenden von freigegebenen Access Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md).


## <a name="start-and-initialize-the-storage-emulator"></a>Starten oder zur Initialisierung Speicheremulator

Um die Azure Speicheremulator zu beginnen, wählen Sie die Schaltfläche Start, oder drücken Sie die Windows-Taste. Beginnen mit der Eingabe **Speicheremulator Azure**, und wählen Sie den Emulator aus der Liste der Programme. 

Wenn der Emulator ausgeführt wird, wird ein Symbol im Infobereich Windows-Taskleiste angezeigt.

Wenn Speicheremulator gestartet wird, wird ein Fenster Befehlszeile angezeigt. Sie können verwenden Sie diese Befehlszeile Fenster starten und beenden Speicheremulator sowie die Daten löschen, Abrufen des aktuellen Status oder zur Initialisierung des Emulators. Weitere Informationen finden Sie unter [Speicher Emulator Line Tool verweisen](#storage-emulator-command-line-tool-reference).

Wenn das Fenster Befehlszeile geschlossen ist, wird der Speicheremulator weiter ausgeführt. Um die Befehlszeile wieder zu verschieben, führen Sie die obigen Schritte, als wäre Speicheremulator ab.

Beim ersten Ausführen von Speicheremulator, ist die lokale Speicher-Umgebung für Sie Initialisierung. Der Initialisierungsprozess erstellt eine Datenbank in LocalDB und HTTP-Ports für jeden Dienst lokaler Speicher reserviert. 

Speicheremulator ist standardmäßig an das c:\Programme Dateien (x86) \Microsoft SDKs\Azure\Storage Emulatorverzeichnis installiert. 

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>Initialisierung Speicheremulator um eine andere SQL-Datenbank zu verwenden.

Das Befehlszeile Speicher Emulator-Tool können Initialisierung Speicheremulator auf einer SQL-Datenbank-Instanz als die Standardinstanz LocalDB verweisen. Beachten Sie, dass Sie das Tool Befehlszeilenoptionen mit Administratorrechten Back-End-Datenbank für Speicheremulator Initialisierung ausgeführt werden müssen:

1. Klicken Sie auf die Schaltfläche **Start** , oder drücken Sie die **Windows** -Taste. Beginnen mit der Eingabe `Azure Storage Emulator` , und wählen Sie es aus, wenn es angezeigt wird, um die Befehlszeile Speicher Emulator-Tool zu öffnen.
2. Geben Sie im Eingabeaufforderungsfenster den folgenden Befehl aus, wo `<SQLServerInstance>` der Namen der SQL Server-Instanz ist. Geben Sie zum Verwenden von LocalDb `(localdb)\v11.0` als SQL Server-Instanz.

        AzureStorageEmulator init /server <SQLServerInstance> 
    
    Sie können auch den folgenden Befehl aus, verwenden, der den Emulator mit den SQL Server-Standardinstanz übergibt:

        AzureStorageEmulator init /server .\\ 

    Alternativ können Sie den folgenden Befehl aus, der die Datenbank mit der LocalDB Standardinstanz erneut initialisiert:

        AzureStorageEmulator init /forceCreate 

Weitere Informationen zu diesen Befehlen finden Sie unter [Speicher Emulator Line Tool verweisen](#storage-emulator-command-line-tool-reference).

## <a name="addressing-resources-in-the-storage-emulator"></a>Adressieren Ressourcen Speicheremulator

Die Endpunkte für Speicheremulator unterscheiden sich von denen eines Kontos Azure-Speicher. Die Differenz ist, der lokale Computer Auflösung des Domänennamens, nicht durchgeführt wird, damit die Speicher Emulator Endpunkte eine lokale Adresse statt einen Domänennamen erforderlich.

Wenn Sie eine Ressource in einem Konto Azure-Speicher adressieren, verwenden Sie die folgenden Farbschema, wobei der Kontonamen Teil der URI-Hostname und die Ressource wird adressiert ist Bestandteil von URI-Pfad:

    <http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>

Der folgende URI beträgt beispielsweise eine gültige Adresse für ein Blob in einem Konto Azure-Speicher:

    https://myaccount.blob.core.windows.net/mycontainer/myblob.txt

Da die Auflösung des Domänennamens, von der lokale Computer nicht durchgeführt wird ist den Namen des Kontos in der Speicheremulator Teil des URL-Pfads anstelle der Hostname. Sie verwenden die folgenden des Farbschemas für eine Ressource in Speicheremulator ausgeführt:

    http://<local-machine-address>:<port>/<account-name>/<resource-path>

Beispielsweise die folgende Adresse möglicherweise verwendet werden, für den Zugriff auf einen Blob im Speicheremulator:

    http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt

Die Endpunkte für Speicheremulator sind:

    Blob Service: http://127.0.0.1:10000/<account-name>/<resource-path>
    Queue Service: http://127.0.0.1:10001/<account-name>/<resource-path>
    Table Service: http://127.0.0.1:10002/<account-name>/<resource-path>

### <a name="addressing-the-account-secondary-with-ra-grs"></a>Adressieren das sekundäre mit RAS GRS Konto

Ab Version 3.1 unterstützt das Speicher Emulator Konto Lesezugriff Geo redundante Replikation (RAS-GRS). Für den Speicherressourcen sowohl in der Cloud und im lokalen Emulator, können Sie den zweiten Standort zugreifen durch das Anhängen - sekundäre auf den Namen des Kontos. Beispielsweise die folgende Adresse möglicherweise verwendet werden, für den Zugriff auf ein Blob mithilfe des sekundären schreibgeschützt in Speicheremulator:

    http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt 

> [AZURE.NOTE] Verwenden Sie für den programmgesteuerten Zugriff auf die sekundären Speicheremulator die Speicher-Client-Bibliothek für .NET Version 3,2 oder höher ein. Finden Sie in der [Microsoft Azure-Speicher-Client-Bibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) Details.

## <a name="storage-emulator-command-line-tool-reference"></a>Speicher Emulator Befehlszeile Tool Bezug

Ab Version 3.0, wenn Sie Speicheremulator starten, wird ein Fenster Befehlszeile popupwarnung angezeigt. Verwenden Sie das Befehlszeile Fenster starten und beenden den Emulator sowie für Status Abfragen, und führen Sie andere Vorgänge aus.

> [AZURE.NOTE] Wenn Sie die Microsoft Azure berechnen Emulator installiert haben, wird ein Symbol in der Taskleiste angezeigt, wenn Sie Speicheremulator starten. Mit der rechten Maustaste auf das Symbol, um ein Menü, angezeigt, die eine grafische Möglichkeit zum Starten und Beenden der Speicheremulator bietet.

### <a name="command-line-syntax"></a>Die Syntax der Befehlszeile

    AzureStorageEmulator [start] [stop] [status] [clear] [init] [help]

### <a name="options"></a>Optionen

Geben Sie zum Anzeigen der Liste der Optionen `/help` in die Befehlszeile eingeben.

| Option | Beschreibung                                                    | Befehl                                                                                                 | Argumente auf:                                                                                                         |
|--------|----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Starten**  | Startet Speicheremulator.                                | `AzureStorageEmulator start [-inprocess]`                                                                    | *-inprocess*: Starten des Emulators im aktuellen Prozess statt einen neuen Prozess zu erstellen.                          |
| **Beenden**   | Tabstopps Speicheremulator an.                                    | `AzureStorageEmulator stop`                                                                                  |                                                                                                                   |
| **Status** | Gibt den Status der Speicheremulator aus.                     | `AzureStorageEmulator status`                                                                                |                                                                                                                   |
| **Löschen**  | Löscht die Daten in alle Dienste, die in der Befehlszeile angegeben. | `AzureStorageEmulator clear [blob] [table] [queue] [all]                                                    `| *BLOB*: Löscht BLOB-Daten. <br/>*Warteschlange*: Löscht Warteschlangendaten. <br/>*Tabelle*: Löscht Tabellendaten. <br/>*Alle*: Löscht alle Daten in alle Dienste. |
| **Initialisierung**   | Führt einmalige Initialisierung der Emulator einrichten.       | `AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate] [-inprocess]` | *-Server ServerName\instanceName*: Gibt den Hostinganbieter der Instanz von SQL Server. <br/>*-Sqlinstance InstanceName*: Gibt den Namen der SQL-Instanz, in der Standard-Server-Instanz verwendet werden soll. <br/>*-Forcecreate*: Erzwingt das Erstellen der SQL-Datenbank, auch wenn sie bereits vorhanden ist. <br/>*-inprocess*: führt Initialisierung im aktuellen Prozess anstelle einen neuen Prozess wird erstellt. Sie müssen den aktuellen Vorgang mit erweiterten Berechtigungen für die Durchführung Initialisierung starten.          |
                                                                                                                  
## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>Unterschiede zwischen der Speicheremulator und Azure-Speicher

Da Speicheremulator einer emulierten Umgebung in einer lokalen SQL-Instanz ausgeführt wird, gibt es jedoch einige Unterschiede zwischen dem Emulator und ein Konto Azure-Speicher in der Cloud in Funktionen:

- Speicheremulator unterstützt nur ein einzelnes feste Konto und einen Authentifizierungsschlüssel bekannten.

- Speicheremulator ist kein skalierbare Speicherdienst und eine große Anzahl von Clients gleichzeitig wird nicht unterstützt.

- Wie im [Adressierung Ressourcen Speicheremulator](#addressing-resources-in-the-storage-emulator)beschrieben, werden Ressourcen anders Speicheremulator im Vergleich zu einem Konto Azure-Speicher behandelt. Dieser Unterschied führt die Auflösung des Domänennamens verfügbar ist in der Cloud, aber nicht auf dem lokalen Computer.

- Ab Version 3.1 unterstützt das Speicher Emulator Konto Lesezugriff Geo redundante Replikation (RAS-GRS). Im Emulator alle Konten haben RAS-GRS aktiviert und nie eine Verzögerung zwischen der primären und sekundären Replikate vorhanden ist. Die erste Blob-Dienst Stats, erhalten Warteschlange Dienst Stats und erste Tabelle Dienst Stats Vorgänge werden unterstützt, klicken Sie auf das Konto sekundäre und gibt immer den Wert zurück, der `LastSyncTime` Antwortelement als die aktuelle Uhrzeit entsprechend der zugrunde liegenden SQL-Datenbank.

    Verwenden Sie für den programmgesteuerten Zugriff auf die sekundären Speicheremulator die Speicher-Client-Bibliothek für .NET Version 3,2 oder höher ein. Finden Sie in der [Microsoft Azure-Speicher-Client-Bibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) Details.

- Die Datei-Dienst und Protokoll SMB-Endpunkte werden in der Speicheremulator derzeit nicht unterstützt.

- Speicheremulator gibt eine VersionNotSupportedByEmulator (Fehlercode HTTP-Status 400 - Ungültige Anforderung) aus, wenn Sie eine Version der Speicherdienste verwenden, die noch nicht von der Version des Emulators unterstützt wird, die Sie verwenden.

### <a name="differences-for-blob-storage"></a>Unterschiede für Blob-Speicher 

BLOB-Speicher im Emulator gelten die folgenden Unterschiede aufweisen:

- Speicher Emulator nur unterstützt Blob Größen bis zu 2 GB.

- Eine Blob setzen Operation ist möglicherweise erfolgreich gegen ein Blob, der in der Speicheremulator vorhanden ist, und hat einen aktiven verleasen, selbst wenn die verleasen ID, die nicht als Teil der Anforderung angegeben wurde. 

- Anfügen von Blob Vorgänge vom Emulator nicht unterstützt werden. Versucht, einen Vorgang für eine anfügen Blob gibt eine FeatureNotSupportedByEmulator (Fehlercode HTTP-Status 400 - Ungültige Anforderung).

### <a name="differences-for-table-storage"></a>Unterschiede für Tabellenspeicher 

Gelten die folgenden Unterschiede aufweisen Tabellenspeicher im Emulator:

- In der Tabelle Dienst Speicheremulator Datumseigenschaften unterstützt nur von SQL Server 2005 unterstützten Bereich (*d. h.*, sie sind erforderlich, um nach 1 Januar 1753 liegen). Dieser Wert werden alle Datumsangaben vor 1 Januar 1753 geändert. Die Genauigkeit von Datumsangaben ist der Genauigkeit von SQL Server 2005, d. h. Datumsangaben präzise 1 werden eingeschränkte/300th einer Sekunde.

- Speicheremulator unterstützt Partition Schlüssel und Zeile Werte von Haupteigenschaften mit weniger als 512 Bytes. Darüber hinaus kann nicht für die Gesamtgröße der Kontonamen, Tabellenname und Key Eigenschaftennamen nicht trennen 900 Byte nicht überschreiten.

- Die Gesamtgröße einer Zeile in einer Tabelle in der Speicheremulator beträgt weniger als 1 MB.

- Speicheremulator, geben Sie die Eigenschaften von Daten `Edm.Guid` oder `Edm.Binary` unterstützt nur die `Equal (eq)` und `NotEqual (ne)` Vergleichsoperatoren in Abfrage filtern Zeichenfolgen.

### <a name="differences-for-queue-storage"></a>Unterschiede für Warteschlange-Speicher

Es gibt keine Unterschiede speziell für Warteschlange-Speicher im Emulator.

## <a name="storage-emulator-release-notes"></a>Speicher Emulator – Versionsinformationen

### <a name="version-45"></a>Version 4.5

- Feste Einheiten einen Fehler, der verursacht Initialisierung und Installation von Speicheremulator fehlschlägt, wenn die Datenbank sichern umbenannt wurde.

### <a name="version-44"></a>Version 4,4

- Speicheremulator unterstützt jetzt Version 2015-12-11 der Speicherdienste auf Tabelle, Warteschlange und Blob-Endpunkte.

- Der Speicheremulator Garbagecollection Blob-Daten ist jetzt effizienter mit einer großen Anzahl von Blobs gestellten.

- Feste Einheiten einen Fehler, der verursacht Container ACL XML etwas anders aus wie der Speicherdienst bedeutet überprüft werden soll.

- Feste Einheiten einen Fehler, der manchmal Max und min DateTime-Werte für den Bericht in der falschen Zeitzone verursacht.

### <a name="version-43"></a>Version 4.3

- Speicheremulator unterstützt jetzt Version 2015-07-08 der Speicherdienste auf Tabelle, Warteschlange und Blob-Endpunkte.

### <a name="version-42"></a>Version 4.2

- Speicheremulator unterstützt jetzt Version 2015-04-05 der Speicherdienste auf Tabelle, Warteschlange und Blob-Endpunkte.

### <a name="version-41"></a>Version 4.1

- Speicheremulator unterstützt jetzt Version 2015-02-21 Speicher Dienste auf Blob, Warteschlange und Tabelle Endpunkte, mit Ausnahme der neuen Features Blob angefügt werden soll. 

- Speicheremulator gibt jetzt eine aussagekräftige Fehlermeldung, wenn Sie eine Version der Speicherdienste verwenden, die von dieser Version des Emulators noch nicht unterstützt wird. Es empfiehlt sich, die neueste Version des Emulators verwenden. Wenn Sie eine VersionNotSupportedByEmulator (Fehlercode HTTP-Status 400 - Ungültige Anforderung) auftreten, laden Sie bitte die neueste Version von Speicheremulator.

- Einem Fehler behoben in dem ein Race Bedingung verursacht Entität Tabellendaten um bei gleichzeitiger Zusammenführen Vorgängen möglicherweise falsch.

### <a name="version-40"></a>Version 4.0

- Ausführbare Speicheremulator wurde in *AzureStorageEmulator.exe*umbenannt.

### <a name="version-32"></a>Version 3,2

- Speicheremulator unterstützt jetzt Version 2014-02-14 der Speicherdienste auf Tabelle, Warteschlange und Blob-Endpunkte. Beachten Sie, dass die Datei Endpunkte Speicheremulator derzeit nicht unterstützt werden. Details zu Version 2014-02-14 finden Sie unter [Versioning für die Azure-Speicher-Dienste](https://msdn.microsoft.com/library/azure/dd894041.aspx) .

### <a name="version-31"></a>Version 3.1

- Lesezugriff Geo redundante Speicher (RAS-GRS) wird jetzt in der Speicheremulator unterstützt. Der erste Blob-Dienst Stats, erhalten Warteschlange Dienst Stats und erste Tabelle Dienst Stats APIs für das Konto sekundäre unterstützt werden, und gibt den Wert des Elements Antwort LastSyncTime immer als die aktuelle Uhrzeit entsprechend der zugrunde liegenden SQL-Datenbank zurück. Verwenden Sie für den programmgesteuerten Zugriff auf die sekundären Speicheremulator die Speicher-Client-Bibliothek für .NET Version 3,2 oder höher ein. Details finden Sie unter der Microsoft Azure-Speicher-Client-Bibliothek für .NET verweisen.

### <a name="version-30"></a>Version 3.0

- Azure Speicheremulator wird nicht mehr in demselben Paket wie die Serveremulator geliefert.

- Speicher Emulator Benutzeroberfläche wird zugunsten einer Skripts geeignet Command Line Interface nicht mehr unterstützt. Details auf die Linie-Schnittstelle finden Sie unter Speicher Emulator Line Tool verweisen. Die Benutzeroberfläche weiterhin in Version 3.0 vorhanden sein, aber nur zugegriffen werden kann, wenn der Serveremulator installiert ist, indem Sie mit der rechten Maustaste auf das Taskleisten-Symbol und Anzeigen von Speicher Emulator Benutzeroberfläche auswählen.

- Version 2013-08-15 der Dienste Azure-Speicher ist jetzt vollständig unterstützt. (Dieser Version wurde zuvor nur unterstützt, indem Sie Speicheremulator Version 2.2.1 Vorschau.)
