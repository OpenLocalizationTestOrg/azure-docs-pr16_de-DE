<properties 
    pageTitle="Verschieben von Daten zwischen Clouddatenbanken skalierten | Microsoft Azure" 
    description="Es wird erläutert, wie mehrere Shards hinweg Bearbeiten und Verschieben von Daten über einen lokal gehosteten flexible Datenbank-APIs verwenden." 
    services="sql-database" 
    documentationCenter="" 
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

# <a name="moving-data-between-scaled-out-cloud-databases"></a>Verschieben von Daten zwischen skalierten Clouddatenbanken

Wenn Sie eine Software als Dienstentwickler sind und Ihre app plötzlich viel Demand durchläuft, müssen Sie die Wachstum. So fügen Sie weitere Datenbanken (mehrere Shards hinweg) hinzu. Wie weitergeben Sie die Daten zu den neuen Datenbanken ohne Unterbrechung der Datenintegrität? Verwenden Sie das **Tool Teilen und Zusammenführen** zum Verschieben von Daten aus eingeschränkten Datenbanken auf die neuen Datenbanken ein.  

Das Tool Teilen und zusammenführen, die als Azure Webdienst ausgeführt wird. Ein Administrator oder Entwickler verwendet das Tool zum Verschieben von Shardlets (Daten aus einer Shard) zwischen verschiedenen Datenbanken (mehrere Shards hinweg). Das Tool verwendet Shard Karte Management zum Verwalten der Service-Metadaten-Datenbank und konsistente Zuordnung sicherzustellen.

![(Übersicht)][1]

## <a name="download"></a>Herunterladen
[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)


## <a name="documentation"></a>Dokumentation
1. [Flexible Datenbank Teilen und Zusammenführen Tool Lernprogramm](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
* [Sicherheitskonfiguration von Teilen und Zusammenführen](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Zur Sicherheit von Teilen und Zusammenführen](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Shard Karte management](sql-database-elastic-scale-shard-map-management.md)
* [Migrieren von vorhandenen Datenbanken zu skalieren möchten](sql-database-elastic-convert-to-use-elastic-tools.md)
* [Flexible Datenbanktools](sql-database-elastic-scale-introduction.md)
* [Flexible Datenbank Tools-Glossar](sql-database-elastic-scale-glossary.md)

## <a name="why-use-the-split-merge-tool"></a>Gründe für die Verwendung des Tools Teilen und Zusammenführen

**Flexibilität**

Applikationen müssen flexibel Strecken die Grenzen einer einzelnen DB von Azure SQL-Datenbank. Verwenden Sie das Tool zum Verschieben von Daten, je nach Bedarf hinzu neue Datenbanken Beibehaltung Integrität aus.

**Geteilte wachsen** 

Sie müssen insgesamt Kapazität explosionsartig verarbeitet zu erhöhen. Erstellen Sie hierzu zusätzlichen Kapazität durch Sharding die Daten und Verteilen von es auf inkrementell mehr Datenbanken bis Kapazität Anforderungen erfüllt sind. Dies ist ein Beispiel für das Feature 'Teilen' Prime. 

**Zum Verkleinern zusammenführen**

Kapazität muss ein Geschäft aufgrund der saisonale verkleinern. Das Tool können Sie weniger Maßeinheiten verkleinern, wenn Business verlangsamt. Das Seriendruckfeature '' im Dienst Teilen und Zusammenführen flexible Skalieren wird diese Anforderung behandelt. 

**Verwalten von Hotspots durch Shardlets verschieben**

Mit mehreren Mandanten pro Datenbank kann die Zuordnung von Shardlets auf mehrere Shards hinweg zu Kapazitätsengpässen auf einige mehrere Shards hinweg führen. Erneutes Zuweisen von Shardlets oder beschäftigt Shardlets verschieben, um neue oder weniger genutzte mehrere Shards hinweg setzt. 

## <a name="concepts--key-features"></a>Konzepte und wichtigsten features

**Kunden-gehosteten Diensten**

Das Teilen und Zusammenführen ist als Kunden gehosteten Dienst übermittelt. Sie müssen bereitstellen und hosten Sie den Dienst in Ihrem Microsoft Azure-Abonnement. Das Paket, das Sie von NuGet herunterladen enthält eine Konfigurationsvorlage mit den Informationen für eine bestimmte Bereitstellung ausführen. Finden Sie im [Lernprogramm Teilen und Zusammenführen](sql-database-elastic-scale-configure-deploy-split-and-merge.md) Details aus. Da der Dienst in Ihrem Abonnement Azure ausgeführt wird, können Sie steuern, und die meisten Sicherheitsaspekte des Diensts konfigurieren. Die Standardvorlage enthält die Optionen zum Konfigurieren von SSL, Zertifikat-basierten Client-Authentifizierung, Verschlüsselung für gespeicherte Anmeldeinformationen, DoS Schutz und IP-Einschränkungen. Weitere Informationen über die Sicherheitsaspekte finden Sie in der folgenden Dokument [Teilen und Zusammenführen Sicherheitskonfiguration](sql-database-elastic-scale-split-merge-security-configuration.md).

Die Standardeinstellung bereitgestellt mit einem Worker und eine Webrolle der Dienst ausgeführt wird. Jedes Element der A1-virtueller Speicher in Azure-Cloud-Diensten verwendet. Während Sie diese Einstellungen nicht ändern können, wenn Sie das Paket bereitstellen, können Sie sie nach einer erfolgreichen Bereitstellung im laufenden Cloud-Dienst (über das Azure-Portal) ändern. Beachten Sie, dass die Worker-Rolle für mehr als eine einzelne Instanz technischen Gründen nicht konfiguriert werden muss. 

**Shard Schema-integration**

Der Dienst Teilen und Zusammenführen interagiert mit der Karte Shard der Anwendung. Bei Verwendung den Dienst Teilen und Zusammenführen Teilen oder Zusammenführen von Bereichen oder Shardlets zwischen mehrere Shards hinweg wird der Dienst automatisch die Karte Shard auf dem neuesten Stand. Hierzu der Dienst eine Verbindung mit der Shard Karte Manager-Datenbank der Anwendung und behält die Bereiche und Zuordnungen als Zusammenführen/Teilen/verschieben Besprechungsanfragen Vorgangsfortschritt. Dadurch wird sichergestellt, dass die Karte Shard beim Teilen und Zusammenführen Vorgänge vertraut sind, klicken Sie auf immer eine Ansicht auf dem neuesten Stand dargestellt. Aufteilen, werden verbinden und Shardlet Bewegung Vorgänge implementiert, indem Sie eine Reihe von Shardlets aus der Quelle Shard in der Zielliste Shard verschieben. Während der Shardlet Verwaltungsvorgang der Shardlets unterliegen den aktuellen Stapel werden in der Karte Shard offline markiert und sind für Daten-abhängige Weiterleitung Verbindungen mit **OpenConnectionForKey** API nicht verfügbar. 

**Konsistente Shardlet Verbindungen**

Beim Verschieben von Daten für einen neuen Stapel von Shardlets gestartet wird, werden alle Shard-Karte bereitgestellten Daten-abhängige Weiterleitung Verbindungen auf das Speichern von der Shardlet Shard beendeten und nachfolgende Verbindungen aus der Zuordnung Shard APIs mit den folgenden Shardlets blockiert werden, während das Verschieben von Daten in Bearbeitung befindet, um Inkonsistenzen zu vermeiden. Verbindungen mit anderen Shardlets auf der gleichen Shard wird ebenfalls gelöscht erhalten, jedoch erneut erfolgreich sofort auf "Wiederholen". Nachdem der Stapel verschoben wird, die Shardlets online erneut für die Ziel-Shard markiert sind, und die Quelldaten werden aus der Quelle Shard entfernt. Der Dienst durchläuft diese Schritte für jedes Blatt aus, bis alle Shardlets verschoben wurden. Dies wird im Verlauf des Vorgangs abgeschlossen Zusammenführen/Teilen/verschieben zu mehreren Verbindung Kill Vorgängen führen.  

**Verwalten von Shardlet Verfügbarkeit**

Beschränken die Verbindung, indem Sie dem aktuellen Stapel von Shardlets wie oben beschrieben deaktivieren beschränkt des Gültigkeitsbereichs von Ausfall einer Gruppe von Shardlets nacheinander. Dies ist bevorzugten über ein Ansatz, würde die vollständige Shard offline für alle zugehörigen Shardlets im Verlauf eines Vorgangs Teilen oder Zusammenführen bleiben. Die Größe des Stapels, als die Anzahl der unterschiedlichen Shardlets nacheinander, verschieben definiert ist ein Konfigurationsparameter. Sie können für jeden Vorgang verbinden und Teilen je nach der Anwendung Verfügbarkeit und Leistung Anforderungen definiert werden. Beachten Sie, dass der Bereich, der in der Zuordnung Shard gesperrt ist möglicherweise größer als die angegebene Stapelgröße. Dies ist, da der Dienst die Bereichsgröße wählt so, dass die tatsächliche Anzahl der Schlüsselwerte für Sharding in den Daten ungefähr die Stapelgröße entspricht. Dies ist insbesondere für spärlich gefüllten Sharding Tasten Denken Sie daran. 

**Metadaten-Speicher**

Der Dienst Teilen und Zusammenführen verwendet eine Datenbank aus, deren Status verwalten und Protokolle während der Verarbeitung von Besprechungsanfragen zu belassen. Der Benutzer dieser Datenbank in ihrem Abonnement erstellt und enthält die Verbindungszeichenfolge für sie in der Konfigurationsdatei für die Bereitstellung von Service. Administratoren aus der Organisation des Benutzers können auch mit dieser Datenbank Anforderung Fortschritt überprüfen und detaillierte Informationen zur potenzielle Fehler ermitteln verbinden.

**Sharding-Präsenz**

Der Dienst Teilen und Zusammenführen unterscheidet zwischen Tabellen (1) sharded, Tabellen Bezug (2) und (3) normale Tabellen. Die Semantik eines Vorgangs Zusammenführen/Teilen/verschieben hängen vom Typ der Tabelle verwendet wird, und wird wie folgt definiert: 

* **Sharded Tabellen**: Verschieben von Teilen, Zusammenführen und Verschieben von Vorgängen Shardlets aus der Quelle zum Ziel Shard. Nach dem erfolgreichen Abschluss der Anfrage insgesamt sind diese Shardlets nicht mehr in der Quelle vorhanden. Beachten Sie, dass die Zieltabellen auf das Ziel Shard vorhanden sein müssen und darf keine Daten in den Zielbereich vor der Verarbeitung des Vorgangs ein. 

* **Verweis Tabellen**: für Verweis Tabellen teilen, Zusammenführen und Vorgänge und kopieren Sie die Daten aus der Quelle auf das Ziel Shard verschieben. Beachten Sie, dass keine Änderungen auf die Ziel-Shard für eine bestimmte Tabelle tritt auf, wenn eine Zeile bereits in dieser Tabelle am Ziel vorhanden ist. Die Tabelle enthält für jeden Vorgang Bezug Tabelle kopieren, Verarbeitung leer sein.

* **Weitere Tabellen**: anderen Tabellen können entweder die Quelle oder das Ziel eines Vorgangs Teilen und Zusammenführen vorhanden sein. Der Dienst Teilen und Zusammenführen ignoriert in diesen Tabellen Kopiervorgänge oder Verschieben von Daten. Beachten Sie jedoch, dass sie diese Vorgänge bei Einschränkungen beeinträchtigen können.

Die Informationen für den Verweis im Vergleich zu sharded Tabellen wird auf der Karte Shard von **SchemaInfo** APIs bereitgestellt. Das folgende Beispiel veranschaulicht die Verwendung dieser APIs auf einer bestimmten Shard Karte Manager Objekt Smm: 

    // Create the schema annotations 
    SchemaInfo schemaInfo = new SchemaInfo(); 

    // Reference tables 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region")); 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation")); 

    // Sharded tables 
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY")); 
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY")); 

    // Publish 
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo); 

Die Tabellen "Region" und 'gesetzlichen Feiertagen' als Referenztabellen definiert sind und mit geteilten/Zusammenführen/Verschieben der Daten kopiert werden. 'Kunden' und 'Orders' werden als sharded Tabellen wiederum definiert. C_CUSTKEY und O_CUSTKEY dienen als die Sharding-Taste. 

**Referenzielle Integrität**

Der Dienst Teilen und Zusammenführen analysiert Abhängigkeiten zwischen Tabellen und die Vorgänge zum Verschieben von Verweis Tabellen und Shardlets Phaseneigenschaften verwendet und Primärschlüssel fremdschlüsselbeziehungen. Verweis Tabellen werden im Allgemeinen zuerst in Abhängigkeitsreihenfolge, kopiert, und klicken Sie dann in der Reihenfolge von deren abhängigen Dateien innerhalb jeder Stapel Shardlets kopiert werden. Dies ist erforderlich, damit FS-PK Einschränkungen für die Ziel-Shard eintreffen die neuen Daten berücksichtigt werden. 

**Shard Karte Konsistenz und den tatsächlichen Abschluss**

Bei Fehlern Anwesenheit Dienst Teilen und Zusammenführen von Lebensläufen Vorgänge nach einem Ausfall und soll, führen Sie in Statusabfragen. Jedoch möglicherweise nicht behebbare Situationen, z. B., wenn das Ziel Shard verloren gegangen sind oder nicht repariert gefährdet wird. Unter diesen Umständen möglicherweise einige Shardlets, die verschoben wurden kenne weiterhin auf die Quelle Shard befinden. Der Dienst ist sichergestellt, dass Shardlet Zuordnungen nur aktualisiert werden, nachdem die notwendigen Daten erfolgreich an die Zielwebsite kopiert wurde. Shardlets werden nur die Quelle gelöscht, nachdem Sie alle ihre Daten an die Zielwebsite kopiert wurden und im entsprechenden Zuordnungen erfolgreich aktualisiert wurden. Der Löschvorgang erfolgt im Hintergrund, während des Bereichs auf das Ziel Shard bereits online ist. Der Dienst Teilen und Zusammenführen sichergestellt immer Richtigkeit der Zuordnungen in der Shard Zuordnung gespeichert.


## <a name="the-split-merge-user-interface"></a>Die Benutzeroberfläche von Teilen und Zusammenführen

Das Teilen und Zusammenführen Service-Paket enthält eine Worker-Rolle und eine Webrolle. Die Web-Rolle dient zum Teilen und Zusammenführen Besprechungsanfragen in eine interaktive Möglichkeit zu übermitteln. Die Hauptkomponenten der Benutzeroberfläche sind wie folgt aus:

-    Vorgangstyp: Den Vorgangstyp ist ein Optionsfeld, die steuert die Art des Vorgangs, die vom Dienst für diese Anforderung durchgeführt wird. Sie können entscheiden, ob die Teilung, Zusammenführen und Szenarien verschieben. Sie können auch einen zuvor zur Verfügung gestellt Vorgang abzubrechen. Verwenden von Teilen, zusammenführen, und verschieben Besprechungsanfragen für Bereich Shard Karten. Liste Shard ordnet nur verschieben Supportteam.

-    Shard Karte: Im nächste Abschnitt der Anforderungsparameter bedecken Sie Informationen über die Shard Karte und die Datenbank die Karte Shard hosten. Insbesondere müssen Sie zum Angeben des Namens des Azure SQL-Datenbankserver und der Datenbank Hostinganbieter die Shardmap Anmeldeinformationen zur Verbindung mit der Datenbank Shard Karte und schließlich auf den Namen der Karte Shard. Der Vorgang akzeptiert derzeit nur einen einzigen Satz von Anmeldeinformationen. Diese Anmeldeinformationen müssen, dass über die erforderlichen Berechtigungen zum Ausführen von Änderungen an der Shard Karte sowie unter ', um die Benutzerdaten in der mehrere Shards hinweg.

-    Quellbereich (Teilen und Zusammenführen): ein Teilen und Zusammenführen-Vorgang verarbeitet einen Bereich, der mit ihrem Höchst- und Schlüssel. Um einen Vorgang mit einem ungebundener hohe Schlüsselwert angeben möchten, aktivieren Sie das Kontrollkästchen "hohe Schlüssel ist max" und lassen Sie das Feld mit das hohe leer. Die Schlüsselwerte Bereich, die Sie angeben müssen nicht genau einer Zuordnung und deren Grenzen in der Karte Shard entsprechen. Wenn Sie keine Bereich Begrenzung gar angeben wird der Dienst den nächsten Bereich für Sie automatisch abgeleitet werden. Das Skript GetMappings.ps1 PowerShell können Sie die aktuellen Zuordnungen in einer Zuordnung angegebenen Shard abrufen.

-    Geteilte Quelle Verhalten (Teilen): für Vorgänge teilen, definieren Sie den Punkt, um den Quellbereich zu teilen. Sie ausführen können, indem Sie die Sharding-Taste unterbrochen werden soll. Verwenden Sie das Optionsfeld angeben, ob Sie den unteren Teil des Bereichs (mit Ausnahme des geteilten Schlüssels) verschieben möchten, oder soll im oberen Teil (einschließlich des geteilten Schlüssels) verschieben.

-    Datenquelle Shardlet (verschieben): Vorgänge unterscheiden sich von Teilen oder Zusammenführen Vorgänge, wie sie einen Bereich zur Beschreibung der Quelle keine erfordern verschieben. Eine Quelle für verschieben wird durch den Schlüsselwert Sharding, die Sie verschieben möchten, einfach identifiziert.

-    Adressieren Shard (Teilen): Nachdem Sie die Informationen auf der Quelle des unterbrochenen Vorgangs bereitgestellt haben, müssen Sie definieren, werden die Daten in durch die Bereitstellung der Azure SQL Server und die Datenbank Datenbankname für das Ziel kopiert werden soll.

-    Zielbereich (Zusammenführen): Seriendruck-Vorgänge Shardlets Zeilen zu einer vorhandenen Shard verschoben werden. Identifizieren Sie die vorhandenen Shard können, indem Sie die Begrenzung Bereich des bestehenden Bereichs, die Sie zusammenführen möchten.

-    Stapelgröße: Die Stapelgröße steuert die Anzahl der Shardlets, die jeweils während der Verlagerung von Daten offline gehen wird. Dies ist eine ganze Zahl, in dem kleinere Werte können Sie, wenn Sie vertrauliche in Perioden Ausfall Shardlets long sind. Größere Werte erhöht sich die Zeit, die einem angegebenen Shardlet ist offline aber möglicherweise verbessern der Leistung.

-    Vorgangs-Id (Abbrechen): Wenn Sie einer laufenden Betrieb, die nicht mehr benötigt wird verfügen, können Sie den Vorgang abbrechen können, indem Sie dessen Vorgangs-ID in diesem Feld. Sie können die Vorgangs-ID abrufen, aus der Tabelle der Anfrage Status (siehe Abschnitt 8.1) oder aus der Ausgabe in einem Webbrowser, wenn Sie die Anfrage gesendet.


## <a name="requirements-and-limitations"></a>Anforderungen und Einschränkungen 

Die aktuelle Implementierung des Diensts Teilen und Zusammenführen wird die folgenden Anforderungen und Einschränkungen: 

* Die mehrere Shards hinweg müssen vorhanden sein und in der Karte Shard registriert sein, bevor ein geteilten Zusammenführungsvorgangs auf diese mehrere Shards hinweg ausgeführt werden kann. 

* Der Dienst erstellt keine Tabellen oder eine beliebige andere Datenbankobjekte automatisch als Teil der Vorgänge. Dies bedeutet, dass das Schema für alle sharded Tabellen und Bezugstabellen vorhanden sein, klicken Sie auf das Ziel Shard bevor Zusammenführen/Teilen/verschieben Vorgänge ausgeführt werden müssen. Sharded Tabellen müssen insbesondere im Bereich leer sein, in dem neuen Shardlets werden, indem Sie einen Vorgang Zusammenführen/Teilen/verschieben hinzugefügt werden soll. Andernfalls wird der Vorgang die Überprüfung der anfänglichen Konsistenz für die Ziel-Shard fehl. Beachten Sie auch die Verweis, wenn nur Daten kopiert werden, wenn der Bezug Tabelle leer ist, und es gibt keine Garantie Konsistenz im Hinblick auf andere gleichzeitige Vorgänge für den Verweis Tabellen schreiben. Es empfiehlt sich dies: Teilen/zusammenführen-Vorgänge ausgeführt wird, stellen keine andere Vorgänge Änderungen mit den Bezugstabellen.

* Der Dienst beruht auf Zeilenidentität durch einen eindeutigen Index oder Schlüssel, die die Sharding-Taste zum Verbessern der Leistung und Zuverlässigkeit für große Shardlets enthält. Dadurch wird den Dienst zum Verschieben von Daten bei einer noch genauer Abstufung nur den Schlüsselwert Sharding. Dadurch verringern Sie die maximale Menge an Speicherplatz für Protokolldateien und sperren, die während des Vorgangs erforderlich sind. Erwägen Sie das Erstellen einen eindeutigen Index oder einen Primärschlüssel, einschließlich des Sharding Schlüssels in einer angegebenen Tabelle aus, wenn Sie die Tabelle mit Zusammenführen/Teilen/verschieben Anforderungen verwenden möchten. Aus Gründen der Leistung sollten die Taste Sharding der führende Spalte in der Schlüssel oder den Index.

* Im Verlauf der Verarbeitung von Besprechungsanfragen möglicherweise einige Shardlet Daten sowohl auf der Quell- und die Ziel-Shard präsentieren. Dies ist zum Schutz von Fehlern bei der Bewegung Shardlet erforderlich. Die Integration von Teilen und Zusammenführen mit der Shard Karte ist sichergestellt, dass Verbindungen durch die Daten mithilfe der Methode **OpenConnectionForKey** auf der Karte Shard APIs routing abhängige alle inkonsistenten mittlere Zuständen nicht angezeigt werden. Jedoch möglicherweise beim Herstellen einer Verbindung ohne mithilfe der Methode **OpenConnectionForKey** , um die Quelle oder das Ziel mehrere Shards hinweg, inkonsistente mittlere Zuständen angezeigt, wenn Zusammenführen/Teilen/verschieben Anfragen auf abgelegt werden. Diese Verbindungen möglicherweise teilweise oder doppelte Ergebnisse je nach der Anzeigedauer oder die zugrunde liegende die Verbindung Shard anzeigen. Diese Einschränkung betrifft aktuell von flexible Maßstab Multi-Shard-Abfragen vorgenommene Verbindungen.

* Die Datenbank Metadaten für den Dienst Teilen und Zusammenführen dürfen nicht zwischen den verschiedenen Rollen freigegeben werden. Beispielsweise muss eine Rolle des Diensts Teilen und Zusammenführen Staging ausgeführt zu einer anderen Metadaten-Datenbank als die Herstellung Rolle verweisen.
 

## <a name="billing"></a>Abrechnung 

Der Teilen und Zusammenführen-Dienst als Cloud-Dienst in Ihrem Abonnement Microsoft Azure ausgeführt wird. Wenden Sie daher Gebühren für Clouddienste auf Ihre Instanz des Diensts. Es sei denn, Sie häufig Zusammenführen/Teilen/verschieben Vorgänge ausführen, empfehlen wir, dass Sie Ihre Teilen und Zusammenführen Cloud-Dienst löschen. Dieses speichert die Kosten für die Ausführung oder Instanzen der Cloud-Dienst bereitgestellt. Sie können erneut bereitstellen und Ihre Flexibilität ausführbare Konfiguration zu starten, wenn Sie teilen oder Zusammenführen Vorgänge ausführen müssen. 
 
## <a name="monitoring"></a>Für die Überwachung 
### <a name="status-tables"></a>Statustabellen 

Der Teilen und Zusammenführen-Dienst bietet die **RequestStatus** -Tabelle in den Metadaten Datenbank zum Überwachen der fertigen und laufenden Anfragen zu speichern. Die Tabelle enthält eine Zeile für jede Anforderung von Teilen und zusammenführen, die in dieser Instanz des Diensts Teilen und Zusammenführen gesendet wurde. Es bietet die folgende Informationen für jede Anforderung:

* **Zeitstempel**: Datum und Uhrzeit, wann die Anforderung gestartet wurde.

* **OperationId**: eine GUID, die die Anforderung identifiziert. Diese Anforderung kann auch verwendet werden, um den Vorgang abzubrechen, während sie noch laufende ist.

* **Status**: den aktuellen Status der Anfrage. Für die laufenden Anfragen werden auch die aktuelle Phase, in denen die Anforderung ist.

* **CancelRequest**: eine Kennzeichnung, die angibt, ob die Anforderung wurde abgebrochen.

* **Status**: eine Schätzung der Prozentsatz der Fertigstellung für den Vorgang. Der Wert 50 gibt an, dass der Vorgang etwa 50 % abgeschlossen ist.

* **Details**: XML-Wert, der Fortschritt für genauere Berichte bietet. Der Bericht "Status" wird regelmäßig aktualisiert, während die Gruppen von Zeilen aus der Quelle zum Ziel kopiert werden. Bei Fehlern oder Ausnahmen enthält diese Spalte auch ausführliche Informationen zu dem Fehler an.


### <a name="azure-diagnostics"></a>Azure-Diagnose

Der Dienst Teilen und Zusammenführen verwendet basierend auf Azure SDK 2,5 für die Überwachung und Diagnose Azure-Diagnose. Die Diagnosekonfiguration zu steuern, wie hier beschrieben: [Aktivieren der Diagnose in Azure-Cloud-Diensten und virtuellen Computern](../cloud-services/cloud-services-dotnet-diagnostics.md). Download-Paket enthält zwei Diagnose Konfigurationen – eine für die Web-Rolle und eine für die Worker-Rolle. Diese Diagnose Konfigurationen für den Dienst folgen der Anleitung aus der [Cloud-Service-Grundlagen in Microsoft Azure](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649). Er enthält die Definitionen, um die Leistungsindikatoren, IIS-Protokolle, Windows-Ereignisprotokollen und Teilen und Zusammenführen Anwendungsereignisprotokollen melden. 

## <a name="deploy-diagnostics"></a>Bereitstellen von Diagnose 

Führen Sie zum Aktivieren von Überwachung und Diagnose mithilfe der Diagnose Konfigurations für das Web und Worker Rollen von NuGet-Paket bereitgestellten mithilfe der PowerShell Azure folgende Befehle aus: 

    $storage_name = "<YourAzureStorageAccount>" 
    
    $key = "<YourAzureStorageAccountKey" 
    
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  
    
    
    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 
    
    
    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

Weitere Informationen zum Konfigurieren und Bereitstellen von Diagnose Hier finden Sie: [Aktivieren der Diagnose in Azure-Cloud-Diensten und virtuellen Computern](../cloud-services/cloud-services-dotnet-diagnostics.md). 

## <a name="retrieve-diagnostics"></a>Abrufen von Diagnose 

Sie können Ihre Diagnose einfach aus dem Visual Studio Server-Explorer in den Azure Teil der Server-Explorer-Struktur zugreifen. Öffnen Sie eine Visual Studio-Instanz aus, und klicken Sie in der Menüleiste auf Ansicht, und Server-Explorer. Klicken Sie auf das Symbol Azure, für die Verbindung zu Ihrem Azure-Abonnement. Navigieren Sie zum Azure-Speicher > -> <your storage account> -> Tabellen -> WADLogsTable. Weitere Informationen finden Sie unter [Durchsuchen von Ressourcen mit Server-Explorer](http://msdn.microsoft.com/library/azure/ff683677.aspx). 

![WADLogsTable][2]

Der in der Abbildung oben hervorgehoben WADLogsTable enthält die detaillierten Ereignisse aus den Teilen und Zusammenführen-Dienst-Ereignisprotokoll. Beachten Sie, dass die standardmäßige Konfiguration des heruntergeladenen Pakets in Richtung einer Bereitstellung Herstellung ausgerichtet ist. Daher ist das Intervall, Protokolle und Indikatoren aus den Dienstinstanzen herausgezogen, Groß (5 Minuten). Reduzieren Sie das Intervall für Tests und Entwicklung, indem Sie die Diagnose Einstellungen im Web oder den Worker-Rolle an Ihre Bedürfnisse anpassen. Mit der rechten Maustaste auf die Rolle im Visual Studio Server-Explorer (siehe oben), und klicken Sie dann im Dialogfeld für die Konfiguration Diagnose Periode übertragen anpassen: 

![Konfiguration][3]


## <a name="performance"></a>Leistung

Im Allgemeinen wird eine bessere Leistung zu erwarten aus je höher, weitere leistungsfähige Dienst Ebenen in SQL Azure-Datenbank ein. Höhere EA, CPU- und Zuweisungen für die Ebenen der höheren Service profitieren die Massenkopieren und Löschvorgängen, die der Teilen und Zusammenführen-Dienst verwendet. Aus diesem Grund erhöhen Sie die Dienstebene nur für diese Datenbanken für einen Zeitraum definiert ist, begrenzt.

Der Dienst führt auch Validierung Abfragen als Teil der normalen Vorgänge an. Diese Überprüfung Abfragen unerwartete Vorhandensein von Daten im Zielbereich überprüfen, und stellen Sie sicher, dass alle Vorgang Zusammenführen/Teilen/Verschieben von einem konsistenten Zustand gestartet wird. Diese alle Abfragen über Sharding Key Bereiche definiert werden, indem Sie den Umfang des Vorgangs und der Stapelgröße liegen als Teil der Definition der Anfrage arbeiten. Diese Abfragen ausführen am besten, wenn ein Index präsentieren ist mit dem Sharding Schlüssel als führende Spalte enthält. 

Darüber hinaus ermöglichen eine Eigenschaft Eindeutigkeit mit dem Sharding Schlüssel als führende Spalte Dienst zu einer optimierten Ansatz, die Ressourcenverbrauch Log Platz und Arbeitsspeicher beschränkt. Diese Eigenschaft Eindeutigkeit ist erforderlich, um die große Datenmengen Größen (normalerweise über 1 GB) verschieben. 

## <a name="how-to-upgrade"></a>So aktualisieren

1. Folgen Sie den Schritten [Bereitstellen einer Dienst Teilen und Zusammenführen](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. Ändern der Konfigurationsdatei der Cloud-Dienst für die Bereitstellung Teilen und zusammenführen, um die neue Konfigurationsparameter wiederzugeben. Ein neuer erforderlichen Parameter ist die Informationen über das Zertifikat für die Verschlüsselung verwendet. Eine einfache Möglichkeit dazu ist die neue Konfigurationsdatei für die Vorlage aus dem Download anhand Ihrer bestehenden Konfiguration zu vergleichen. Stellen Sie sicher, dass Sie die Einstellungen für "DataEncryptionPrimaryCertificateThumbprint" und "DataEncryptionPrimary" für das Web und Worker-Rolle hinzufügen.
3. Bevor Sie das Update für Azure bereitstellen, stellen Sie sicher, dass alle derzeit ausgeführtes Teilen und Zusammenführen Vorgänge abgeschlossen haben. Sie können problemlos erreichen, indem RequestStatus und PendingWorkflows Tabellen in der Datenbank Teilen und Zusammenführen von Metadaten für laufenden Anfragen Abfragen.
4. Aktualisieren der vorhandenen Cloud-Service-bereitstellungs für Teilen und Zusammenführen in Ihrem Abonnement Azure mit dem neuen Paket und der aktualisierten Konfiguration Dienstdatei.

Sie müssen nicht bereitstellen eine neuen Datenbank von Metadaten für Teilen und Zusammenführen zu aktualisieren. Die neue Version wird die vorhandenen Metadaten-Datenbank auf die neue Version automatisch aktualisiert werden. 

## <a name="best-practices--troubleshooting"></a>Bewährte Methoden und Problembehandlung
 
-    Definieren von einem Mandanten testen, und probieren Sie Ihre wichtigsten Zusammenführen/Teilen/verschieben Vorgänge mit den Test-Mandanten über mehrere mehrere Shards hinweg. Stellen Sie sicher, dass alle Metadaten in der Karte Shard ordnungsgemäß definiert ist und die Vorgänge nicht Einschränkungen oder Fremdschlüssel verstoßen.
-    Behalten Sie den Mandanten Test Probleme im Zusammenhang mit Datengröße über die maximale Größe von der größten Mandanten, um sicherzustellen, dass Sie werden keine Datengröße auftritt. Auf diese Weise können Sie eine Obergrenze auf die Uhrzeit bewerten, die benötigt wird, um einen einzelnen Mandanten navigieren. 
-    Stellen Sie sicher, dass Ihr Schema löschen kann. Teilen und Zusammenführen-Dienst erfordert die Möglichkeit, Daten aus der Quelle Shard entfernen, nachdem die Daten erfolgreich an die Zielwebsite kopiert wurde. Beispielsweise **Trigger löschen** können verhindern, dass den Dienst Löschen der Daten auf der Quelle und möglicherweise nicht ordnungsgemäß funktionieren.
-    Der Schlüssel Sharding sollte die führenden Spalte in Ihrem Primärschlüssel oder einen eindeutigen Indexdefinition haben. Wird sichergestellt, dass die optimale Leistung für die Überprüfung Abfragen Teilen oder zusammenführen und für die tatsächlichen Daten Bewegung und Löschvorgang Vorgänge, die immer auf Sharding Key Bereiche ausgeführt werden.
-    Zusammengestellte den Teilen und Zusammenführen-Dienst in der Region und Daten Center, in denen Ihre Datenbanken befinden. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]



<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png
 
