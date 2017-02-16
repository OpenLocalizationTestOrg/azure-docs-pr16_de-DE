<properties
   pageTitle="Migrieren von Daten in SQL Data Warehouse | Microsoft Azure"
   description="Tipps für die Migration von Daten in Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="migrate-your-data"></a>Migrieren von Daten
Daten können aus verschiedenen Quellen in Ihr SQL Data Warehouse mit einer Vielzahl Tools verschoben werden.  ADF kopieren, SSIS- und Bcp können alle verwendet werden, um dies zu erreichen. Sollte Denken Sie jedoch als die Menge der Daten erhöht Sie Migration von Daten in Schritte aufteilen. Dies ermöglicht die Möglichkeit, die einzelnen Schritte für die Leistung sowohl für eine robuste Lösung zur Sicherstellung einer Datenmigration interpolierten zu optimieren.

In diesem Artikel werden zuerst die einfachen Migrationsszenarien ADF kopieren, SSIS-und Bcp. Es dann nähere etwas genauer wie die Migration optimiert werden kann.

## <a name="azure-data-factory-adf-copy"></a>Kopieren von Azure Daten Factory (ADF)
[Kopieren der ADF][] [Azure Data Factory][]gehört. ADF kopieren können Sie Ihre Daten in einer flachen Dateien, die auf lokale speichern, um die remote flachen Dateien frei, die direkt in die Data Warehouse SQL Azure Blob-Speicher oder exportieren.

Wenn Ihre Daten in einer flachen Dateien beginnt, und klicken Sie dann Sie zuerst es in Speicher Azure Blob übertragen, bevor Sie mit einer laden müssen es in SQL Data Warehouse. Nachdem die Daten in Azure Blob-Speicher übertragen werden, können Sie auswählen und [ADF kopieren][] erneut zu verwenden, um die Daten in SQL Data Warehouse schieben.

PolyBase bietet auch eine leistungsfähige Option für die Daten geladen. Jedoch bedeutet statt einem zwei Tools verwenden. Wenn Sie die optimale Leistung benötigen PolyBase verwenden. Wenn eine einzelne Tool Erfahrung (und die Daten nicht großer sind) ist ADF Ihre Antwort an.

> [AZURE.NOTE] PolyBase erfordert Ihre Datendateien UTF-8 sein. Dies ist der Kopie ADF Standardeinstellung Codierung, sodass nichts zu ändern. Dies ist nur eine Erinnerung das Standardverhalten der ADF kopieren nicht ändern.

Leiten Sie über die folgenden Artikel für einige großartige [ADF Beispiele][].

## <a name="integration-services"></a>Integrationsservices ##
Integration Services (SSIS) ist ein leistungsfähige und flexible extrahieren transformieren und laden (ETL) Tool, die komplexe Workflows, Datentransformation und mehrere Optionen zum Laden von Daten unterstützt. Verwenden Sie SSIS, um einfach Daten Azure oder als Teil eines breiteren Migration übertragen.

> [AZURE.NOTE] SSIS können in UTF-8 ohne die Markierung für Byte-Zeichen in der Datei exportieren. Zum Konfigurieren Dies müssen Sie zuerst die abgeleitete Spalte Komponente verwenden Zeichendaten im Datenfluss mit der 65001 UTF-8-Codepage konvertieren. Nachdem die Spalten konvertiert wurden, Schreiben Sie die Daten in die Flatfile Ziel Netzwerkadapter, um sicherzustellen, dass 65001 auch als die Codepage für die Datei ausgewählt wurde.

SSIS verbindet mit SQL Data Warehouse, wie sie mit einer SQL Server-Bereitstellung verbinden möchten. Ihre Verbindungen müssen jedoch eine ADO.NET Verbindungs-Managers verwenden. Sie sollten auch so konfigurieren Sie die "verwenden Massen einfügen Wenn verfügbar" darauf achten Einstellung liegenden. Lizenzinformationen finden Sie im Artikel [ADO.NET Ziel Netzwerkadapter][] Weitere Informationen zu dieser Eigenschaft

> [AZURE.NOTE] Herstellen einer Verbindung mit Azure SQL-Data Warehouse über OLEDB wird nicht unterstützt.

Darüber hinaus ist immer die Möglichkeit, die ein Paket Fälligkeitsdatum zu begrenzungsebene oder Netzwerkprobleme fehlschlagen können. Entwurf verpackt, damit er ohne zum Zeitpunkt des Fehlers, fortgesetzt werden können wiederholen arbeiten, die durchgeführt werden, bevor der Fehler auftritt.

Weitere Informationen finden Sie in der [SSIS-Dokumentation][].

## <a name="bcp"></a>bcp
BCP ist ein Befehlszeilendienstprogramm, die für die Flatfile Daten importieren und exportieren vorgesehen ist. Einige Transformation kann während des Datenexports erfolgen. Zum Ausführen verwenden einfache Transformationen einer Abfrage auswählen und Transformieren der Daten aus. Nach dem exportieren, können die flachen Dateien dann direkt in das Ziel SQL Data Warehouse-Datenbank geladen werden.

> [AZURE.NOTE] Es ist oft eine gute Idee, die während des Datenexports in einer Ansicht auf Quellsystem verwendeten Transformationen kapseln. Dies stellt sicher, dass die Logik beibehalten wird und der Prozess wiederholt ist.

Bcp bietet folgende Vorteile:

- Einfache Handhabung. Bcp Befehle sind einfach zu erstellen und ausführen
- Erneutes wiederherzustellenden Ladevorgang gehört. Einmal exportierte die Laden werden kann beliebig häufig ausgeführt

Schwächen Bcp sind:

- Bcp funktioniert mit nur aus tabulierten flachen Dateien. Sie funktioniert nicht mit Dateien wie Xml oder JSON
- Bcp unterstützt exportieren in UTF-8 nicht. Möglicherweise verhindert PolyBase Bcp exportiert Daten mit
- Funktionen für die Transformation auf die exportieren Phase nur eingeschränkt und sind einfache Natur
- Bcp wurde robuste werden beim Laden von Daten über das Internet angepasst. Alle Netzwerk instabil werden möglicherweise ein Fehler beim Laden.
- Bcp basiert auf dem Schema, die in der Zieldatenbank vor dem Ladevorgang vorhanden

Weitere Informationen finden Sie unter [Bcp verwenden, um Daten in SQL Data Warehouse zu laden][].

## <a name="optimizing-data-migration"></a>Optimieren der Migration von Daten
Eine SQLDW Daten Migrationsvorgangs kann effektiv in drei verschiedene Schritte unterteilt werden:

1. Exportieren der Quelldaten
2. Datenübertragung und Azure
3. Laden Sie die Zieldatenbank SQLDW

Jeder Schritt kann einzeln optimiert werden, um eine robuste, erneut wiederherzustellenden und robuste Migrationsvorgangs zu erstellen, die Leistung bei jedem Schritt maximiert.

## <a name="optimizing-data-load"></a>Optimieren von Daten beim Laden
Betrachten diese in umgekehrter Reihenfolge für einen Moment; die schnellste Möglichkeit zum Laden von Daten ist über PolyBase. Optimieren für eine PolyBase Ladevorgang platziert erforderliche Komponenten auf die vorherigen Schritte, damit es am besten, dies zu verstehen ist im Vorfeld. Sie sind:

1. Codieren von Datendateien
2. Formatieren von Datendateien
3. Speicherort der Datendateien

### <a name="encoding"></a>Codierung
PolyBase erfordert Datendateien UTF-8 codiert werden. Dies bedeutet, dass sie beim Exportieren von Daten mit dieser Anforderung entsprechen muss. Wenn Ihre Daten nur einfache ASCII-Zeichen (nicht erweiterte ASCII-Zeichen) enthält dann diese Karte direkt den standardmäßigen UTF-8, und Sie müssen nicht zu viel über die Codierung Gedanken machen. Jedoch, wenn die Daten dieser Sonderzeichen enthält, wie Umlaute, Akzente oder Symbole oder Ihre Daten nicht Lateinisch Sprachen unterstützt müssen Sie sicherstellen, dass Ihre Dateien exportieren ordnungsgemäß UTF-8 codiert werden.

> [AZURE.NOTE] Exportieren von Daten in UTF-8 unterstützt Bcp nicht. Daher ist die beste Option für den Datenexport Integration Services oder ADF kopieren zu verwenden. Lohnt sich darauf hin, dass die UTF-8-Byte-Zeichen Reihenfolge markieren (Stücklisten) in der Datendatei nicht erforderlich ist.

Alle Dateien, die mit UTF-16 codiert müssen in ***früheren*** Datenübertragung neu geschrieben werden können.

### <a name="format-of-data-files"></a>Formatieren von Datendateien
PolyBase erfordert eine feste Zeile End \n oder Zeilenwechsel an. Ihre Datendateien müssen diese Standard entsprechen. Es sind keine Einschränkungen Zeichenfolge oder die Spalte-Abschlusszeichen aus.

Sie müssen für jede Spalte in der Datei als Teil der externen Tabelle in PolyBase definieren. Stellen Sie sicher, dass alle exportierte Spalten erforderlich sind und die Typen der erforderlichen Standards entsprechen.

Näheres wieder die [Migrieren Ihrer Schema] Artikel für Details auf unterstützte Datentypen.

### <a name="location-of-data-files"></a>Speicherort der Datendateien
SQL Data Warehouse verwendet PolyBase, um Daten aus Azure BLOB-Speicher ausschließlich zu laden. Daher müssen die Daten zuerst in Blob-Speicher übertragen wurden.

## <a name="optimizing-data-transfer"></a>Optimieren von Datenübertragung
Einer der langsamste Teile der Datenmigration ist die Übertragung der Daten in Azure. Nicht nur kann werden Bandbreite ein Problem, sondern auch die Zuverlässigkeit des Netzwerks behindern kann den Fortschritt. Migrieren von Daten zu Azure ist standardmäßig über das Internet, damit die Chancen durchstellen Fehler auftritt, einigermaßen wahrscheinlich sind. Dieser Fehler erfordern jedoch Daten ganz oder teilweise erneut gesendet werden.

Glücklicherweise haben Sie mehrere Optionen zur Verbesserung der Geschwindigkeit und Stabilität dieses Prozesses verwendet:

### <a name="expressroute"></a>[ExpressRoute][]
Sie möchten möglicherweise bietet [ExpressRoute][] , um die Übertragung zu beschleunigen. [ExpressRoute][] bietet eine bestehende private Verbindung in Azure, damit die Verbindung nicht über das öffentliche Internet geht. Dies ist jedoch kein obligatorischer Schritt. Es wird jedoch Durchsatz verbessern, bei der Daten in Azure aus einer lokalen oder gemeinsame Speicherort Einrichtung.

Die Vorteile der Verwendung von [ExpressRoute][] sind:

1. Verbesserte Zuverlässigkeit
2. Schnellere netzwerkgeschwindigkeit
3. Unteren Netzwerkwartezeit
4. höhere Netzwerk-Sicherheit

[ExpressRoute][] ist für eine Reihe von Szenarios Vorteil; nicht nur der Migration.

Interessiert? Weitere Informationen und Preise Bitte finden Sie auf der [ExpressRoute Dokumentation][].

### <a name="azure-import-and-export-service"></a>Azure Import / Export-Dienst
Azure Import / Export-Dienst umfasst Datenübertragung großer (TB ++) Übermittlungen Daten in Azure für große (GB ++) soll. Es umfasst das Schreiben von Daten auf dem Datenträger und die Lieferung an einem Azure Data Center. Klicken Sie dann werden in Ihrem Auftrag der Inhalt des Datenträgers in Azure-Speicher Blobs geladen.

Eine Ansicht auf hoher Ebene des Exportvorgangs importieren sieht wie folgt aus:

1. Konfigurieren eines Containers Azure BLOB-Speicher zum Empfangen von Daten
2. Exportieren von Daten in den lokalen Speicher
2. Kopieren Sie die Daten in 3,5 Zoll SATA II/III-Festplatten mit dem [Azure Import/Export-Tool]
3. Erstellen einer Auftrag Import mit dem Azure importieren und Exportieren Dienst bereitstellen [Azure Import/Export Tool] erstellten Journaldateien
4. Materialien für den Datenträger der benannten Azure Data center
5. Ihre Daten werden in Ihrem Container Azure BLOB-Speicher übertragen.
6. Laden Sie die Daten in SQLDW PolyBase verwenden

### <a name="azcopy-utility"></a>[AZCopy][] -Programm
Das Programm [AZCopy][] ist ein großartiges Tool zum Abrufen der Daten in Azure-Speicher Blobs übertragen. Es ist für kleine (MB ++) zu sehr großen (GB ++) Datenübertragung entwickelt. [AZCopy] wurde auch entwickelt zum Bereitstellen guten robuste Durchsatz beim Übertragen von Daten in Azure und können sich hervorragend für die Übertragung Schritt Daten besteht. Einmal übertragenen können Sie die Daten mithilfe von PolyBase in SQL Data Warehouse laden. Sie können auch AZCopy in der SSIS-Paketen mithilfe eines Vorgangs "Prozess ausführen" integrieren.

Wenn Sie AZCopy verwenden müssen Sie zunächst herunterzuladen und zu installieren. Es ist eine [Version der Herstellung][] und einer [Vorschauversion][] verfügbar.

Wenn Sie eine Datei aus Ihrem Dateisystem hochladen benötigen Sie einen Befehl wie den folgenden:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

Eine Zusammenfassung auf hoher Ebene Prozess könnte:

1. Konfigurieren eines Speicher Azure Blob Containers zum Empfangen von Daten
2. Exportieren von Daten in den lokalen Speicher
3. AZCopy von Daten in den Container Azure BLOB-Speicher
4. Laden Sie die Daten in SQL Data Warehouse PolyBase verwenden

Vollständige Dokumentation zur Verfügung: [AZCopy][].

## <a name="optimizing-data-export"></a>Optimieren von Daten exportieren
Zusätzlich zur Sicherstellung, dass die Garantien nach PolyBase der Export entspricht können Sie auch Zielwertsuche zum Optimieren des exportieren Daten den Prozess weiter zu verbessern.

> [AZURE.NOTE] Wie PolyBase erfordert, die Daten dass an, dass es wahrscheinlich nicht ist UTF-8-Format verwenden Sie Bcp den Datenexport ausführen. die Ausgabe Datendateien in UTF-8 unterstützt Bcp nicht. SSIS- oder ADF kopieren sind wesentlich besser geeignet, um die Durchführung dieser Art von Daten exportieren.

### <a name="data-compression"></a>Daten Komprimierung
PolyBase kann mit Gzip komprimierte Daten lesen. Wenn Sie Ihre Daten Gzip Dateien komprimieren können wird die Menge der Daten, die über das Netzwerk gedrückt wird minimiert werden.

### <a name="multiple-files"></a>Mehrere Dateien
Unterbrechen von großen Tabellen in mehrere Dateien nicht nur dem Schutz zur Verbesserung der Geschwindigkeit exportieren, sondern auch mit durchstellen Neustarts und der gesamten Verwaltbarkeit der Daten in der Azure Blob-Speicher. Eine übersichtliche viele Features von PolyBase ist, dass sie alle Dateien in einem Ordner liest und wie eine Tabelle zu behandeln. Daher ist es eine gute Idee, die die Dateien für jede Tabelle in einen eigenen Ordner isolieren.

PolyBase unterstützt auch ein Feature, das als "rekursive Ordner durchlaufen" bezeichnet. Dieses Feature können Sie um die Organisation der exportierten Daten zur Verbesserung der datenverwaltung Ihrer weiter zu verbessern.

Weitere Informationen zum Laden von Daten mit PolyBase finden Sie unter [PolyBase verwenden, um Daten in SQL Data Warehouse zu laden][].


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Migration finden Sie unter [Migrieren Ihrer Lösung zur SQL Data Warehouse][].
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung][].

<!--Image references-->

<!--Article references-->
[AZCopy]: ../storage/storage-use-azcopy.md
[ADF kopieren]: ../data-factory/data-factory-data-movement-activities.md 
[ADF Beispiele]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[Übersicht über die Entwicklung]: sql-data-warehouse-overview-develop.md
[Migrieren von Ihrer Lösung zu SQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Verwenden Sie zum Laden von Daten in SQL Data Warehouse bcp]: sql-data-warehouse-load-with-bcp.md
[Verwenden Sie zum Laden von Daten in SQL Data Warehouse PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Factory Azure-Daten]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute Dokumentation]: http://azure.microsoft.com/documentation/services/expressroute/

[Herstellung version]: http://aka.ms/downloadazcopy/
[Preview-version]: http://aka.ms/downloadazcopypr/
[ADO.NET Ziel Netzwerkadapter]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS-Dokumentation]: https://msdn.microsoft.com/library/ms141026.aspx
