<properties 
    pageTitle="Freigeben von Notizen für Datenverwaltungsgateway | Factory Azure-Daten" 
    description="Daten Management Gateway Tory – Versionsinformationen" 
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
    ms.date="08/26/2016" 
    ms.author="spelluru"/>

# <a name="release-notes-for-data-management-gateway"></a>Versionsinformationen für das Datenverwaltungsgateway
Zu den Aufgaben für die Datenintegration mit modernen wird nahtlos Daten an und von lokal in die cloud verschoben. Daten Factory macht diese Integration nahtlos mit Datenverwaltungsgateway, einen Agent ist die Installation lokal zum Verschieben von Daten Hybrid aktivieren.

Finden Sie ausführliche Informationen zum Datenverwaltungsgateway und zur gemeinsamen Nutzung die folgenden Artikeln: 

- [Datenverwaltungsgateway](data-factory-data-management-gateway.md)
- [Verschieben von Daten zwischen lokalen und cloud Azure Data Factory verwenden](data-factory-move-data-between-onprem-and-cloud.md) 

## <a name="current-version-2260721"></a>Aktuelle Version (2.2.6072.1)

- Unterstützt das für das Gateway mithilfe der Konfigurations-Manager des HTTP-Proxy festlegen. Wenn konfiguriert, Azure Blob, Azure-Tabelle, Azure Daten Sees und Dokument DB über HTTP-Proxy zugegriffen werden.
- Unterstützt Verfahrensweise für Header TextFormat beim Kopieren von Daten aus dem und in Azure Blob, Datenspeicher Lake Azure lokalen Dateisystem und lokalen HDFS.
- Unterstützt das Kopieren von Daten aus Blob Anfügen und Seite Blob zusammen mit den bereits unterstützten Blob blockieren.
- Führt einen neuen gatewaystatus **Online (begrenzt)**, gibt an, dass die Hauptfunktionen des Gateways mit Ausnahme des interaktiven Vorgang Support für den Assistenten zum Kopieren von funktioniert.
- Verbessert die Stabilität des Gateways Registrierung Registrierungsschlüssel verwenden.

## <a name="earlier-versions"></a>Früheren Versionen

## <a name="2160401"></a>2.1.6040.1

- DB2-Treiber ist jetzt in der Gateway-Installationspaket enthalten. Sie müssen nicht separat installieren. 
- DB2-Treiber unterstützt jetzt Z/OS und DB2 für ich (AS / 400) zusammen mit den bereits unterstützten Plattformen (Linux, Unix und Windows). 
- Unterstützt die Verwendung von DocumentDB als Quelle oder Ziel für lokale Datenspeicher
- Kopieren von Daten von/bis Haut/Tastaturkürzel unterstützt BLOB-Speicher zusammen mit dem Konto bereits unterstützte allgemeine Speicher. 
- Können Sie die Verbindung zu einer lokalen SQL Server über Gateway mit remote-Anmeldeberechtigungen.  

## <a name="2060131"></a>2.0.6013.1

- Sie können die Sprache/Kultur durch ein Gateway verwendet werden, während der manuellen Installation auswählen.
- Wenn Gateway nicht wie erwartet funktioniert, können Sie auswählen, Gateway Protokolle der letzten sieben Tage an Microsoft zur Erleichterung Behandeln des Problems zu senden. Wenn Gateway nicht in der Cloud-Service angeschlossen ist, können Sie zum Speichern und archivieren Gateway Protokolle auswählen.  
- Verbesserte Benutzeroberfläche für Gateway-Konfigurations-Manager:
    - Machen Sie gatewaystatus auf der Registerkarte Start mehr sichtbar.
    - Neu angeordnete und vereinfachte Steuerelemente.
- Sie können Daten aus einem Speicher mit den [codefreien Vorschau Tools zum Kopieren](data-factory-copy-data-wizard-tutorial.md)kopieren. Finden Sie ausführliche Informationen zu diesem Feature im allgemeinen [Kopieren bereitgestellt](data-factory-copy-activity-performance.md#staged-copy) . 
- Sie können Datenverwaltungsgateway auf eingehende Daten direkt aus einer lokalen SQL Server-Datenbank in Azure maschinellen Learning verwenden.
- Verbesserte Leistung
    - Verbessern der Leistung bei Schema/Vorschau für SQL Server kopieren und codefreien Vorschau Tool anzeigen.



## <a name="11259531"></a>1.12.5953.1
- Updates

## <a name="11159181"></a>1.11.5918.1

- Maximale Größe des Ereignisprotokolls Gateway wurde von 1 MB auf 40 MB erhöht.
- Für den Fall, dass ein Neustart während Gateway automatische Aktualisierung erforderlich ist, wird ein Dialogfeld mit einer Warnung angezeigt. Sie können auswählen, rechts dann oder höher neu zu starten. 
- Für den Fall, dass Sie automatische Aktualisierung fehlschlägt, wiederholt Gateway Installer dreimal bei Maximum automatisch aktualisieren.
- Verbesserte Leistung
    - Verbessern der Leistung zum Laden von großer Tabellen aus lokalen Server in Szenario codefreien kopieren.
- Updates

## <a name="11058921"></a>1.10.5892.1

- Verbesserte Leistung
- Updates

## <a name="1958652"></a>1.9.5865.2

- 0 (null) Touch automatische Update-Funktion
- Neue Symbol in der Taskleiste mit Statusindikatoren gateway
- Möglichkeit zum "Jetzt aktualisieren" vom client
- Möglichkeit zum Aktualisieren Terminplan Uhrzeit eingestellt
- PowerShell-Skript für die automatische Aktualisierung ein-/ausschalten umschalten
- Unterstützung für JSON-format  
- Verbesserte Leistung
- Updates

## <a name="1858221"></a>1.8.5822.1

- Verbessern der Benutzeroberfläche zur Problembehandlung
- Verbesserte Leistung
- Updates

### <a name="1757951"></a>1.7.5795.1

- Verbesserte Leistung
- Updates

### <a name="1757641"></a>1.7.5764.1

- Verbesserte Leistung
- Updates

### <a name="1657351"></a>1.6.5735.1

- Unterstützung für lokale HDFS Quelle/Empfänger
- Verbesserte Leistung
- Updates

### <a name="1656961"></a>1.6.5696.1

- Verbesserte Leistung
- Updates

### <a name="1656761"></a>1.6.5676.1

- Support-Diagnosetools auf Konfigurations-Manager
- Unterstützung für Tabellenspalten für tabellarische Datenquellen für Azure Daten Factory
- Unterstützung DW SQL Azure-Daten Factory
- Unterstützung für Factory Azure-Daten in BlobSource und FileSource Reclusive
- Unterstützung für CopyBehavior – MergeFiles, PreserveHierarchy und FlattenHierarchy in BlobSink und FileSink mit binäre kopieren für Factory Azure-Daten
- Unterstützung von kopieren Aktivität Fortschritt für Azure Daten Factory reporting
- Support Connectivity Überprüfung der Datenquelle für Factory Azure-Daten
- Updates


### <a name="1656721"></a>1.6.5672.1

- Unterstützung für ODBC-Datenquelle für Azure Data Factory Tabellenname
- Verbesserte Leistung
- Updates

### <a name="1656581"></a>1.6.5658.1

- Support-Datei für Azure Daten Factory ignorieren
- Beibehalten der Hierarchie in binäre kopieren für Azure Daten Factory Support
- Unterstützung von kopieren Aktivität Idempotenz für Factory Azure-Daten
- Updates

### <a name="1656401"></a>1.6.5640.1

- Unterstützung für 3 Weitere Datenquellen für Azure Daten Factory (ODBC, OData, HDFS)
- Unterstützung in der CSV-Parser Angebot Zeichen Azure Data Factory
- Unterstützung für die Komprimierung (BZip2)
- Updates

### <a name="1556121"></a>1.5.5612.1

- Unterstützung von fünf relationale Datenbanken für Azure Data Factory (MySQL, PostgreSQL DB2 Teradata und Sybase)
- Unterstützung für die Komprimierung (Gzip und Deflate)
- Verbesserte Leistung
- Updates


### <a name="1455491"></a>1.4.5549.1

- Unterstützung des Oracle Datenquellen für Azure Daten Factory hinzufügen
- Verbesserte Leistung
- Updates

### <a name="1454921"></a>1.4.5492.1

- Unified Binärdatei, die sowohl Microsoft Azure-Daten Factory und Office 365 Power BI-Dienste unterstützt.
- Verbessern Sie den Prozess Konfigurationsbenutzeroberfläche und Registrierung
- Azure Data Factory – Azure eingehende und Ausgang support für SQL Server-Datenquelle

### <a name="1253031"></a>1.2.5303.1

-   Beheben Sie Timeout Problem, um weitere zeitraubende datenquellenverbindungen zu unterstützen. 
    
### <a name="1155268"></a>1.1.5526.8

- Erfordert .NET Framework 4.5.1 als Voraussetzung während der Installation.

### <a name="1051442"></a>1.0.5144.2

- Keine Änderungen, die Azure Data Factory Szenarien beeinflussen. 
