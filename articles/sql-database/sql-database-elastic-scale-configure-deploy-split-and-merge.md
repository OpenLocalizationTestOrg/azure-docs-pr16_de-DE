<properties
    pageTitle="Bereitstellen einen geteilten und Zusammenführen Dienst | Microsoft Azure"
    description="Trennen und Zusammenführen mit flexible Datenbanktools"
    services="sql-database"  
    documentationCenter=""
    authors="ddove"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/27/2016"
    ms.author="ddove" />

# <a name="deploy-a-split-merge-service"></a>Bereitstellen eines geteilten Seriendruck Diensts 

Das Tool Teilen und zusammenführen können Sie das Verschieben von Daten zwischen sharded Datenbanken. Finden Sie unter [Verschieben von Daten zwischen skalierten Clouddatenbanken](sql-database-elastic-scale-overview-split-and-merge.md)

## <a name="download-the-split-merge-packages"></a>Laden Sie die Pakete Teilen und Zusammenführen

1. Laden Sie die neueste Version von NuGet aus [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).
2. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zu dem Verzeichnis, in dem Sie nuget.exe heruntergeladen haben. Der Download enthält PowerShell Commmands.
3. Laden Sie das neueste Teilen und Zusammenführen-Paket in das aktuelle Verzeichnis mit der unter Befehl:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge`  

Die Dateien werden in ein Verzeichnis mit dem Namen **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** , wobei *x.x.xxx.x* die Versionsnummer widerspiegelt, platziert. Teilen und Zusammenführen-Service-Dateien **Content\splitmerge\service** untergeordnete Verzeichnis, und im Teilen und Zusammenführen PowerShell Skripts (und finden Sie erforderliche Client-DLL) im Verzeichnis untergeordnete **Content\splitmerge\powershell** .

## <a name="prerequisites"></a>Erforderliche Komponenten

1. Erstellen einer Azure SQL-DB-Datenbank, die als die Datenbank Teilen und Zusammenführen Status verwendet wird. Wechseln Sie zum [Azure-Portal](https://portal.azure.com)an. Erstellen Sie eine neue **SQL-Datenbank**. Benennen Sie der Datenbank, und erstellen Sie einen neuen Administrator und ein Kennwort. Achten Sie darauf, um den Namen und das Kennwort zur späteren Verwendung aufzuzeichnen.

2. Stellen Sie sicher, dass Ihre Azure SQL-Datenbankserver Azure-Diensten, damit das Herstellen einer Verbindung ermöglicht. Im Portal in der **Firewall-Einstellungen**stellen Sie sicher, dass die Einstellung für **den Zugriff auf Dienste Azure** **auf**festgelegt ist. Klicken Sie auf das Symbol "Speichern".

    ![Zulässige services][1]

3. Erstellen Sie ein Azure-Speicher-Konto, das für die Diagnose Ausgabe verwendet wird. Wechseln Sie zu der Azure-Portal an. Klicken Sie in der linken Leiste klicken Sie auf **neu**, klicken Sie auf **Daten + Speicher**, und klicken Sie dann **Speicher**.

4. Erstellen einer Azure-Cloud-Dienst, die den Dienst Teilen und Zusammenführen enthalten sollen.  Wechseln Sie zu der Azure-Portal an. Klicken Sie in der linken Leiste auf **neu**, und klicken Sie dann **zu berechnen**, **Cloud-Dienst**und **Erstellen**. 


## <a name="configure-your-split-merge-service"></a>Konfigurieren des Diensts Teilen und Zusammenführen

### <a name="split-merge-service-configuration"></a>Dienstkonfiguration Teilen und Zusammenführen

1. Erstellen Sie in den Ordner, in dem Sie die Teilen und Zusammenführen Assemblys heruntergeladen haben eine Kopie der Datei **ServiceConfiguration.Template.cscfg** , die entlang **SplitMergeService.cspkg** geliefert, und benennen Sie sie in **ServiceConfiguration.cscfg**.

2. Öffnen von **ServiceConfiguration.cscfg** in einem Text-Editor wie Visual Studio, das Eingaben, z. B. das Format der Zertifikatfingerabdruck überprüft.

3. Erstellen Sie eine neue Datenbank, oder wählen Sie eine vorhandene Datenbank dienen als die Datenbank Status für Vorgänge teilen und Zusammenführen und Abrufen der Verbindungszeichenfolge zurück, der die Datenbank aus. 

    **Wichtige** Zu diesem Zeitpunkt muss die Datenbank Status lateinische Sortierreihenfolge verwenden (SQL\_Latein-1\_allgemeine\_CP1\_CI\_AS). Weitere Informationen finden Sie unter [Name der Windows-Sortierung (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).

    Mit Azure SQL-DB ist die Verbindungszeichenfolge normalerweise des Formulars:

        "Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30" .
4.    Geben Sie in der Datei Cscfg in die **SplitMergeWeb** und die **SplitMergeWorker** Rolle Abschnitten der Einstellung ElasticScaleMetadata diese Verbindungszeichenfolge.

5.    Geben Sie eine gültige Verbindungszeichenfolge für die Rolle **SplitMergeWorker** zu Azure-Speicher für die Einstellung **WorkerRoleSynchronizationStorageAccountConnectionString** .
        
### <a name="configure-security"></a>Konfigurieren der Sicherheit

Weitere Informationen zum Konfigurieren der Sicherheit des Diensts finden Sie an der [Konfiguration der Sicherheit Teilen und Zusammenführen](sql-database-elastic-scale-split-merge-security-configuration.md).

Zum Zweck einer einfachen Test Bereitstellung in diesem Lernprogramm wird eine minimale Anzahl von Konfigurationsschritte sein ausgeführte Dienst Einstieg und ausgeführt werden. Die folgenden Schritte ermöglichen nur das eine/Computerkonto ausführen, damit die Kommunikation mit dem Dienst.

### <a name="create-a-self-signed-certificate"></a>Erstellen eines selbstsignierten Zertifikats

Erstellen Sie ein neues Verzeichnis, und führen Sie aus diesem Verzeichnis ein [Entwickler für Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx) Eingabeaufforderungsfenster mit den folgenden Befehl aus:

    makecert ^
    -n "CN=*.cloudapp.net" ^
    -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2" ^
    -a sha1 -len 2048 ^
    -sr currentuser -ss root ^
    -sv MyCert.pvk MyCert.cer

Sie sind ein Kennwort zum Schützen von privaten Schlüssels abgefragt. Geben Sie ein sicheres Kennwort ein, und bestätigen Sie es. Sie sind für das Kennwort erneut anschließend verwendet werden dann aufgefordert. Klicken Sie am Ende, um es im Speicher Vertrauenswürdige Stamm-Zertifizierung Rechtsgrundlagenverzeichnis importieren auf **Ja** .

### <a name="create-a-pfx-file"></a>Erstellen einer PFX-Datei

Führen Sie den folgenden Befehl aus dem gleichen Fenster, in dem Makecert ausgeführt wurde; Verwenden Sie dasselbe Kennwort, das Sie verwendet, um das Zertifikat zu erstellen:

    pvk2pfx -pvk MyCert.pvk -spc MyCert.cer -pfx MyCert.pfx -pi <password>

### <a name="import-the-client-certificate-into-the-personal-store"></a>Importieren von dem Client-Zertifikat in den persönlichen Speicher
1. Doppelklicken Sie im Windows-Explorer auf **MyCert.pfx**.
2. Des **Import-Assistenten Zertifikat** **Aktueller Benutzer** wählen Sie aus, und klicken Sie auf **Weiter**.
3. Bestätigen Sie den Dateipfad, und klicken Sie auf **Weiter**.
4. Geben Sie das Kennwort ein, lassen Sie **alle erweiterten Eigenschaften mit einbeziehen** aktiviert, und klicken Sie auf **Weiter**.
5. Verlassen, **Wählen Sie den Zertifikat Store [...] automatisch** aktiviert, und klicken Sie auf **Weiter**.
6. Klicken Sie auf **Fertig stellen** und auf **OK**.

### <a name="upload-the-pfx-file-to-the-cloud-service"></a>Hochladen der PFX-Datei in die Cloud-service

Wechseln Sie zum [Azure-Portal](https://portal.azure.com)an.

1. Wählen Sie die **Cloud-Dienste**.
2. Wählen Sie den Cloud-Dienst, die, den Sie soeben erstellt, für den Dienst geteilten/Zusammenführen haben.
3. Klicken Sie im oberen Menü auf **Zertifikate** .
4. Klicken Sie in der unteren Leiste auf **Hochladen** .
5. Wählen Sie die PFX-Datei aus, und geben Sie dasselbe Kennwort wie oben.
6. Nachdem abgeschlossen ist, kopieren Sie den Fingerabdruck des Zertifikats aus den neuen Eintrag in der Liste aus.

### <a name="update-the-service-configuration-file"></a>Aktualisieren der Konfiguration Dienstdatei

Fügen Sie den Fingerabdruck des Zertifikats oben in der Fingerabdruck/Wert-Attribut diese Einstellungen kopiert.
Für die Worker-Rolle:

    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

Für die Web-Rolle:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />
    <Setting name="AllowedClientCertificateThumbprints" value="" />
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />


Beachten Sie, dass für die Herstellung Bereitstellungen Zertifikate trennen sollte bitte für die Zertifizierungsstelle, für die Verschlüsselung, das Serverzertifikat und Client-Zertifikate verwendet werden. Ausführliche Anweisungen finden Sie unter [Sicherheit konfigurieren](sql-database-elastic-scale-split-merge-security-configuration.md).

## <a name="deploy-your-service"></a>Bereitstellen des Diensts

1. Wechseln Sie zum [Azure-Portal](https://manage.windowsazure.com)an.
2. Klicken Sie auf der Registerkarte **Cloud-Dienste** auf der linken Seite, und wählen Sie aus der Cloud-Dienst, den Sie zuvor erstellt haben.
3. Klicken Sie auf **Dashboard**.
4. Wählen Sie das staging-Umgebung aus und dann klicken Sie auf **eine neue staging Bereitstellung hochladen**.

    ![Staging][3]

5. Geben Sie im Dialogfeld eine Bereitstellung Bezeichnung ein. Klicken Sie für 'Package' und 'Konfiguration' auf 'Aus lokalen', und wählen Sie die Datei **SplitMergeService.cspkg** und die .cscfg-Datei, die Sie zuvor konfiguriert.
6. Stellen Sie sicher, dass das Kontrollkästchen Beschriftung **bereitstellen, auch wenn eine oder mehrere Rollen eine einzelne Instanz enthalten** aktiviert ist.
7. Drücken Sie die Schaltfläche mit dem Häkchen in der rechten unteren Ecke die Bereitstellung beginnen soll. Erwarten sie ein paar Minuten dauern.

![Hochladen][4]


## <a name="troubleshoot-the-deployment"></a>Behandeln von Problemen mit der Bereitstellung

Wenn Ihre Webrolle online ist, fehlschlägt, ist es wahrscheinlich ein Problem mit der Konfiguration der Sicherheit. Überprüfen Sie, dass das SSL konfiguriert ist, wie zuvor beschrieben.

Wenn Ihre Rolle Worker nicht online geschaltet, aber Ihre Webrolle erfolgreich ist, ist es wahrscheinlich ein Problem mit dem Herstellen einer Verbindung mit der Status-Datenbank, die Sie zuvor erstellt haben.

* Stellen Sie sicher, dass die Verbindungszeichenfolge in Ihre .cscfg korrekt ist.
* Überprüfen Sie, dass der Server und die Datenbank vorhanden sind und die Benutzer-Id und das Kennwort richtig sind.
* Azure SQL-DB sollte die Verbindungszeichenfolge des Formulars liegen:

        "Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30" .

* Stellen Sie sicher, dass der Servername nicht mit **https://**beginnt.
* Stellen Sie sicher, dass Ihre Azure SQL-Datenbankserver Azure-Diensten, damit das Herstellen einer Verbindung ermöglicht. Hierzu öffnen Sie https://manage.windowsazure.com, klicken Sie auf der linken Seite auf "SQL-Datenbanken", klicken Sie oben auf "Server", und wählen Sie Ihren Server. Klicken Sie oben auf **Konfigurieren** , und stellen Sie sicher, dass die **Azure Services** auf "Ja" eingestellt ist. (Siehe Abschnitt "erforderliche Komponenten" oben in diesem Artikel).

## <a name="test-the-service-deployment"></a>Testen der Service-Bereitstellung

### <a name="connect-with-a-web-browser"></a>Verbinden Sie mit einem Webbrowser

Ermitteln des Web-Endpunkts Ihres Dienstes Teilen und zusammenführen. Sie können dies im klassischen Azure-Portal suchen, indem Sie dem **Dashboard** von Ihrem Cloud-Dienst und suchen Sie auf der rechten Seite unter **Website-URL** . Ersetzen Sie **http://** mit **https://** , da die Standardeinstellungen für den HTTP-Endpunkt deaktivieren. Laden Sie die Seite für diese URL in Ihrem Browser ein.

### <a name="test-with-powershell-scripts"></a>Testen Sie mit PowerShell-Skripts

Die Bereitstellung und Ihrer Umgebung können durch Ausführen der Stichprobe enthalten PowerShell Skripts getestet werden.

Die Skriptdateien enthalten sind:

1. **SetupSampleSplitMergeEnvironment.ps1** - richtet eine Datenebene Test für geteilte/Zusammenführen (Siehe detaillierte Beschreibung der folgenden Tabelle)
2. **ExecuteSampleSplitMerge.ps1** - führt Test-Vorgänge auf dem Test aus Datenebene (Siehe detaillierte Beschreibung der folgenden Tabelle)
3. **GetMappings.ps1** – auf oberster Ebene Stichprobe Skript, das den aktuellen Status der Zuordnungen Shard gedruckt wird.
4. **ShardManagement.psm1** – Helper Skript, das die ShardManagement-API umbrochen wird.
5. **SqlDatabaseHelpers.psm1** – Helper Skript zum Erstellen und Verwalten von SQL-Datenbanken

<table style="width:100%">
  <tr>
    <th>PowerShell-Datei</th>
    <th>Schritte</th>
  </tr>
  <tr>
    <th rowspan="5">SetupSampleSplitMergeEnvironment.ps1</th>
    <td>1.    Erstellt eine Shard Karte Manager-Datenbank</td>
  </tr>
  <tr>
    <td>2.    2 Shard Datenbanken erstellt.
  </tr>
  <tr>
    <td>3.    Erstellt eine Zuordnung Shard für diese Datenbank (löscht, die alle vorhandenen Shard auf diese Datenbanken maps). </td>
  </tr>
  <tr>
    <td>4.    Erstellt eine kleine Beispieltabelle in beiden der mehrere Shards hinweg und füllt die Tabelle in einem der mehrere Shards hinweg.</td>
  </tr>
  <tr>
    <td>5.    Deklariert die SchemaInfo für die sharded-Tabelle.</td>
  </tr>

</table>

<table style="width:100%">
  <tr>
    <th>PowerShell-Datei</th>
    <th>Schritte</th>
  </tr>
<tr>
    <th rowspan="4">ExecuteSampleSplitMerge.ps1 </th>
    <td>1.    Sendet eine Teilen Anforderung an das Teilen und Zusammenführen Dienst Web Front-End, das Hälfte die Daten aus der ersten Shard in der zweiten Shard teilt.</td>
  </tr>
  <tr>
    <td>2.    Der Web-Front-End für den Teilen Anforderungsstatus abfragt und wartet, bis die Anforderung abgeschlossen ist.</td>
  </tr>
  <tr>
    <td>3.    Sendet eine Zusammenführen Anforderung an das Teilen und Zusammenführen Dienst Web Front-End, das die Daten aus der zweiten Shard wieder auf die erste Shard verschoben wird.</td>
  </tr>
  <tr>
    <td>4.    Die Web-Front-End für den Seriendruck Anforderungsstatus abfragt und wartet, bis die Anforderung abgeschlossen ist.</td>
  </tr>
</table>

## <a name="use-powershell-to-verify-your-deployment"></a>Verwenden von PowerShell zur Überprüfung Ihrer bereitstellungs

1.    Öffnen eines neuen Fensters der PowerShell und navigieren Sie zu dem Verzeichnis, in dem Sie das Paket Teilen und Zusammenführen heruntergeladen haben, und navigieren Sie in dem Verzeichnis "Powershell".
2.    Erstellen einen Azure SQL-Datenbankserver (oder wählen Sie einen vorhandenen Server), in dem die Shard Karte Manager und mehrere Shards hinweg erstellt werden.

    Hinweis: Das Skript SetupSampleSplitMergeEnvironment.ps1 erstellt alle diese Datenbanken auf demselben Server standardmäßig um das Skript einfach zu halten. Dies ist eine Einschränkung des Diensts Teilen und Zusammenführen selbst nicht.

    Eine SQL-Authentifizierung Anmeldung mit Lese-und Schreibzugriff auf die Datenbanken wird für den Dienst Teilen und Zusammenführen zum Verschieben von Daten und aktualisieren die Karte Shard erforderlich sein. Da der Teilen und Zusammenführen-Dienst in der Cloud ausgeführt wird, unterstützt es integrierte Authentifizierung nicht aktuell.

    Stellen Sie sicher, dass die IP-Adresse des Computers Ausführen dieser Skripts zugreifen dürfen der SQL Azure-Server konfiguriert ist. Sie finden diese Einstellung unter dem SQL Azure-Server / Konfiguration / zugelassene IP-Adressen.

3.    Führen Sie das Skript SetupSampleSplitMergeEnvironment.ps1 um die Stichprobe Umgebung zu erstellen.

    Dieses Skript ausgeführt wird, alle vorhandenen Shard Management Kartendaten Strukturen auf die Shard Karte Manager-Datenbank und der mehrere Shards hinweg Wischen. Möglicherweise empfiehlt es sich um das Skript erneut ausführen, wenn Sie die Karte Shard oder mehrere Shards hinweg erneut Initialisierung möchten.

    Beispiel-Befehlszeile:

        .\SetupSampleSplitMergeEnvironment.ps1 `
            -UserName 'mysqluser' ` 
       -Kennwort 'MySqlPassw0rd' ' - ShardMapManagerServerName 'abcdefghij.database.windows.net'

4.    Führen Sie zum Anzeigen der Zuordnungen, die aktuell vorhanden sind in der Stichprobe Umgebung Skript Getmappings.ps1.

        .\GetMappings.ps1 `
            -UserName 'mysqluser' ` 
       -Kennwort 'MySqlPassw0rd' ' - ShardMapManagerServerName 'abcdefghij.database.windows.net'

5.    Führen Sie ExecuteSampleSplitMerge.ps1 Skript zum Ausführen einer Teilung (verschieben Hälfte die Daten auf der ersten Shard in der zweiten Shard) und dann einen Zusammenführen-Vorgang (verschieben die Daten wieder auf die erste Shard). Wenn Sie so konfiguriert, SSL dass und Links den HTTP-Endpunkt deaktiviert haben, stellen Sie sicher, dass Sie stattdessen den Endpunkt https:// verwenden.

    Beispiel-Befehlszeile:

        .\ExecuteSampleSplitMerge.ps1 `
            -UserName 'mysqluser' ` 
       -Kennwort 'MySqlPassw0rd' `
            -ShardMapManagerServerName 'abcdefghij.database.windows.net' ` 
       - SplitMergeServiceEndpoint 'https://mysplitmergeservice.cloudapp.net' ' - CertificateThumbprint '0123456789abcdef0123456789abcdef01234567'

    Wenn Sie erhalten die unter zurück, ist es wahrscheinlich ein Problem mit der Web-Endpunkt Zertifikat. Versuchen Sie, Herstellen einer Verbindung mit der Web-Endpunkt mit Ihrem bevorzugten Webbrowser, und überprüfen Sie, ob das der Zertifikatfehler ist aufgetreten.

        Aufrufen WebRequest: Die zugrunde liegende Verbindung wurde geschlossen: Vertrauensstellung für den SSL/TLSsecure-Kanal konnte nicht eingerichtet werden.

    Wenn sie erfolgreich war, sollte die Ausgabe wie Aussehen der unter:

        > .\ExecuteSampleSplitMerge.ps1 - Benutzername 'Mysqluser'-http://mysplitmergeservice.cloudapp.net' Kennwort 'MySqlPassw0rd' - ShardMapManagerServerName 'abcdefghij.database.windows.net' - SplitMergeServiceEndpoint' CertificateThumbprint – 0123456789abcdef0123456789abcdef01234567 senden Teilen Anforderung Began Teilen Vorgang mit Id dc68dfa0-e22b-4823-886a-9bdc903c80f3 Umfragen Teilen und Zusammenführen Anforderungsstatus. Drücken Sie STRG + C an das Ende des Vorgangsfortschritts: 0 % | Status: In der Warteschlange | Details: [Information] in Warteschlange Anforderung Fortschritt: 5 % | Status: Starten | Details: [Information] starten Teilen und Zusammenführen Zustand Computer für die Anfrage.
Status: 5 % | Status: Starten | Details: [Information] durchführen Datenkonsistenz überprüft auf Ziel mehrere Shards hinweg.
Status: 20 % | Status: CopyingReferenceTables | Details: [Information] verschieben Bezug Tabellen aus der Quelle zum Ziel Shard.
Status: 20 % | Status: CopyingReferenceTables | Details: [Informativen] warten auf Bezugstabellen Kopieren Abschluss.
Status: 20 % | Status: CopyingReferenceTables | Details: [Information] verschieben Bezug Tabellen aus der Quelle zum Ziel Shard.
Status: 44 % | Status: CopyingShardedTables | Details: [Information] verschieben Key Bereich [100:110) der Sharded Tabellen Fortschritt: 44 % | Status: CopyingShardedTables | Details: [Information] erfolgreich kopiert Key Bereich [100:110) für die Tabelle [Dbo]. [MyShardedTable]...... Status: 90 % | Status: Durchführen | Details: [Information] gelöscht erfolgreich Shardlets in der Tabelle [Dbo]. [MyShardedTable].
Status: 90 % | Status: Durchführen | Details: [Informativen], löschen alle temporären Tabellen, die beim Verarbeiten der Anforderung erstellt wurden.
Status: 100 % | Status: Erfolgreich verlaufen ist | Details: [Information] erfolgreich Anforderung verarbeitet.
Zusammenführen Anforderung Began Zusammenführen-Vorgang mit Id 6ffc308f-d006-466b-b24e-857242ec5f66 Umfragen Anforderungsstatus wird gesendet. Drücken Sie STRG + C an das Ende des Vorgangsfortschritts: 0 % | Status: In der Warteschlange | Details: [Information] in Warteschlange Anforderung Fortschritt: 5 % | Status: Starten | Details: [Information] starten Teilen und Zusammenführen Zustand Computer für die Anfrage.
Status: 5 % | Status: Starten | Details: [Information] durchführen Datenkonsistenz überprüft auf Ziel mehrere Shards hinweg.
Status: 20 % | Status: CopyingReferenceTables | Details: [Information] verschieben Bezug Tabellen aus Quelle Ziel Shard.
Status: 44 % | Status: CopyingShardedTables | Details: [Information] verschieben Key Bereich [100:110) der Sharded Tabellen Fortschritt: 44 % | Status: CopyingShardedTables | Details: [Information] erfolgreich kopiert Key Bereich [100:110) für die Tabelle [Dbo]. [MyShardedTable]...... Status: 90 % | Status: Durchführen | Details: [Information] gelöscht erfolgreich Shardlets in der Tabelle [Dbo]. [MyShardedTable].
Status: 90 % | Status: Durchführen | Details: [Informativen], löschen alle temporären Tabellen, die beim Verarbeiten der Anforderung erstellt wurden.
Status: 100 % | Status: Erfolgreich verlaufen ist | Details: [Information] erfolgreich Anforderung verarbeitet.

6.    Experimentieren Sie mit anderen Datentypen! Diese Skripts tragen einen Optionaler ShardKeyType - Parameter, der Sie den Typ des Schlüssels angeben kann. Die Standardeinstellung ist Int32, aber Sie können auch angeben, Int64, Guid oder Binärzahl.

## <a name="create-requests"></a>Erstellen von Besprechungsanfragen

Mithilfe das Web-Benutzeroberfläche oder durch Importieren und verwenden das SplitMerge.psm1 PowerShell-Modul, das Ihre Anfragen durch die Webrolle gesendet wird, kann der Dienst verwendet werden.

Der Dienst kann Daten in Tabellen sharded und Bezugstabellen verschieben. Eine Tabelle sharded verfügt über eine Schlüsselspalte Sharding und anderen Zeilendaten auf jede Shard. Eine Bezugstabelle ist nicht sharded, damit sie Daten aus der gleichen Zeile auf jeder Shard enthält. Verweis-Tabellen sind nützlich für die Daten, die nicht häufig geändert und werden verwendet, um die Verknüpfung mit sharded Tabellen in Abfragen.

Um einen geteilten-Zusammenführen-Vorgang ausführen zu können, müssen Sie deklarieren sharded Tabellen und Verweis-Tabellen, die Sie aufgerufen haben möchten. Dies geschieht mit **SchemaInfo** API. Diese API befindet sich im Namespace **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** .

1.    Erstellen Sie eine Beschreibung der Tabelle übergeordneten Schemanamens **ShardedTableInfo** -Objekt für jede Tabelle sharded (optional, Standardwert ist "Dbo"), den Tabellennamen und den Spaltennamen in dieser Tabelle, die die Taste Sharding enthält.
2.    Erstellen Sie eine Beschreibung der Tabelle übergeordneten Schemanamens **ReferenceTableInfo** -Objekt für jede Tabelle Bezug (optional, Standardwert ist "Dbo") und den Namen der Tabelle.
3.    Hinzufügen der oben genannten TableInfo Objekte in ein neues **SchemaInfo** Objekt.
4.    Rufen Sie einen Verweis auf eine **ShardMapManager** -Objekt, und rufen Sie **GetSchemaInfoCollection**.
5.    Hinzufügen von der **SchemaInfo** zu den **SchemaInfoCollection**, den Namen der Shard Karte bereitstellen.

Ein Beispiel dafür kann in das Skript SetupSampleSplitMergeEnvironment.ps1 angezeigt werden.

Beachten Sie, dass die Zieldatenbank (oder ein Schema für alle Tabellen in der Datenbank) auf der Dienst Teilen und Zusammenführen nicht für Sie erstellt wird. Sie müssen vorab erstellt werden, bevor eine Anforderung an den Dienst gesendet.


## <a name="troubleshooting"></a>Behandlung von Problemen

Sehen Sie möglicherweise die unten angegebene Meldung beim Ausführen der Stichprobe Powershell-Skripts:

    Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel.

Dieser Fehler weist darauf hin, dass das SSL-Zertifikat nicht richtig konfiguriert ist. Bitte folgen Sie den Anweisungen im Abschnitt 'Herstellen einer Verbindung mit einem Webbrowser'.

Wenn Sie Besprechungsanfragen übermitteln können sehen Sie dies:

 [Ausnahme] System.Data.SqlClient.SqlException (0x80131904): Die gespeicherte Prozedur nicht gefunden ' Dbo. InsertRequest'. 

In diesem Fall überprüfen Sie Ihre Konfigurationsdatei, insbesondere die Einstellung für **WorkerRoleSynchronizationStorageAccountConnectionString**. Dieser Fehler gibt normalerweise an, dass die Worker-Rolle nicht erfolgreich Metadaten Initialisierung der Datenbank bei der ersten Verwendung konnte. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/allowed-services.png
[2]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/manage.png
[3]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/staging.png
[4]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/upload.png
[5]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/storage.png
 
