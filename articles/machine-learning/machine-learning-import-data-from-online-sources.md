<properties
    pageTitle="Importieren von Daten in Computer Learning Studio aus Datenquellen online | Microsoft Azure"
    description="Wie Schulung Daten Azure maschinellen Learning Studio aus verschiedenen online Quellen importiert werden."
    keywords="Importieren von Daten, Datenformat, Datentypen, Datenquellen, Schulungsdaten"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;garye" />


# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a>Importieren von Daten in Azure maschinellen Learning Studio aus verschiedenen Quellen von online-Daten mit dem Modul importieren von Daten

Dieser Artikel beschreibt die Unterstützung für den Import von online-Daten aus verschiedenen Quellen und die Informationen zum Verschieben von Daten aus den folgenden Quellen in einem Versuch Azure maschinellen Learning erforderlich sind.

> [AZURE.NOTE] Dieser Artikel enthält allgemeine Informationen zu den [Daten importieren] [ import-data] Modul. Ausführlichere Informationen zu den Arten von Daten können Sie zugreifen, Formate, Parameter und Antworten auf häufig gestellte Fragen, finden Sie im Modul Bezug Thema für die [Daten importieren] [ import-data] Modul.

<!-- -->

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>Einführung

Sie können Daten in der Azure maschinellen Learning Studio aus einem von mehreren Datenquellen online zugreifen, während der Ausführung der Experimentieren mit den [Daten importieren] [ import-data] Modul:

- Eine Web-URL, die mithilfe von HTTP
- Hadoop HiveQL verwenden
- Azure Blob-Speicher
- Azure-Tabelle
- Azure SQL-Datenbank oder SQL Server Azure-virtuellen Computers
- Lokalen SQL Server-Datenbank
- Anbieter, derzeit OData-Datenfeed
 
Der Workflow für Ihre Versuche in Azure maschinellen Learning Studio besteht von Drag & Drop-Komponenten auf den Zeichenbereich aus. Zugriff auf online Datenquellen, fügen Sie die [Daten importieren] [ import-data] Modul, um Ihre besser, wählen Sie die **Datenquelle**, und geben Sie dann die Parameter für den Zugriff auf die Daten erforderlich. In der folgenden Tabelle sind die online unterstützte Datenquellen werden aufgeschlüsselt. Außerdem wird zusammengefasst, die Dateiformate, die unterstützt werden und Parameter, die Zugriff auf die Daten verwendet werden.

Beachten Sie, dass diese Schulungsdaten zugegriffen werden, während der Ausführung der Versuch, nur in diesem Versuch verfügbar. Daten, die in einem Dataset Modul gespeichert wurden stehen im Vergleich zu einem beliebigen experimentieren im Arbeitsbereich.

> [AZURE.IMPORTANT] Aktuell, die [Daten importieren] [ import-data] und [Exportieren von Daten] [ export-data] Module lesen und Schreiben von Daten aus Azure-Speicher erstellt die Bereitstellung Klassisch können. Kurzum, wird der neue Azure BLOB-Speicher Kontotyp, der ein wichtiges Speicher-Access-Leiste oder aussagekräftige Speicher-Access-Leiste bietet noch nicht unterstützt. 

> In der Regel auf alle Konten Azure-Speicher, dass die von Ihnen erstellte vor diesem Dienstoption verfügbar war sollten nicht betroffen. Wenn Sie ein neues Konto erstellen müssen, wählen Sie für das Modell Bereitstellung **klassischen** oder verwenden Sie Ressourcenmanager, und für die **Art des Kontos**, wählen Sie **Allgemeine** anstelle von **Blob-Speicher**aus. 

> Weitere Informationen finden Sie unter [Azure BLOB-Speicher: wichtiges und aussagekräftige Speicherebenen](../storage/storage-blob-storage-tiers.md).



## <a name="supported-online-data-sources"></a>Online von Datenquellen unterstützt
Azure maschinellen Learning **Daten importieren** -Modul unterstützt die folgenden Datenquellen:

Datenquelle | Beschreibung | Parameter |
---|---|---|
Über HTTP-Web-URL |Liest Daten in durch Trennzeichen getrennte Werte (CSV), Tabstopps getrennten Werten (TSV), Attribut-Relation-Dateiformat (ARFF) und Support Vektor Autos (SVM-hell) Formaten aus einem beliebigen Web-URL, die HTTP verwendet | <b>URL</b>: Gibt den vollständigen Namen der Datei, einschließlich URL der Website und den Dateinamen, und einer beliebigen Erweiterung. <br/><br/><b>Datenformat</b>: Gibt einen von unterstützten Formate: CSV, TSV, ARFF oder SVM-Light. Wenn die Daten eine Kopfzeile aufweist, wird es verwendet Spaltennamen zuweisen.|
Hadoop/HDFS|Liest Daten aus verteilten Speicher in Hadoop. Sie geben Sie die Daten mithilfe von HiveQL, einer Abfragesprache für SQL-ähnliche gewünschten. HiveQL kann auch verwendet werden, zum Aggregieren von Daten, und führen Sie die Daten filtern, bevor Sie die Daten zur maschinellen Learning Studio hinzuzufügen.|<b>Struktur Datenbankabfrage</b>: Gibt die Struktur Abfrage verwendet, um die Daten zu generieren.<br/><br/><b>HCatalog Server-URI</b> : den Namen eines Ihrer Cluster im Format *angegeben &lt;Ihren Clusternamen&gt;. azurehdinsight.net.*<br/><br/><b>Hadoop Konto Benutzername</b>: Gibt den Hadoop Benutzernamen Konto verwendet, um den Cluster bereitstellen.<br/><br/><b>Hadoop des Kennworts des Benutzerkontos</b> : Gibt die Anmeldeinformationen verwendet, wenn den Cluster provisioning an. Weitere Informationen finden Sie unter [Erstellen von Hadoop Cluster in HDInsight](../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Speicherort der Ausgabedaten</b>: Gibt an, ob die Daten in ein Hadoop distributed File System (HDFS) oder in Azure gespeichert ist. <br/><ul>Wenn Sie in HDFS Ausgabedaten speichern möchten, geben Sie den HDFS-Server-URI. (Achten Sie darauf, den Namen HDInsight Cluster, ohne das Präfix HTTPS:// verwenden). <br/><br/>Wenn Sie Ihre Ausgabedaten in Azure speichern möchten, müssen Sie den Kontonamen Azure-Speicher, Speicher Zugriffstaste und Speicher Container mit dem Namen angeben.</ul>|
SQL-Datenbank |Liest Daten, die in einer SQL Azure-Datenbank oder in einer SQL Server-Datenbank ausgeführt wird, klicken Sie auf eine Azure-virtuellen Computern gespeichert sind. | <b>Datenbankservername</b>: Gibt den Namen des Servers, auf dem die Datenbank ausgeführt wird.<br/><ul>Geben Sie den Servernamen, der generiert wird, bei Azure SQL-Datenbank. Es weist normalerweise das Format * &lt;Generated_identifier&gt;. database.windows.net.* <br/><br/>Geben Sie bei einer SQLServer auf einem Computer Azure-virtuellen gehostet *Tcp:&lt;DNS-Name des virtuellen Computers&gt;, 1433*</ul><br/><b>Name der Datenbank </b>: Gibt den Namen der Datenbank auf dem Server. <br/><br/><b>Server-Konto Benutzername</b>: Gibt einen Benutzernamen für ein Konto, das über Zugriffsberechtigungen für die Datenbank verfügt. <br/><br/><b>Server des Kennworts des Benutzerkontos</b>: Gibt das Kennwort für das Benutzerkonto.<br/><br/><b>Annehmen einer beliebigen Serverzertifikat</b>: Verwenden Sie diese Option (weniger sicher) aus, wenn Sie überspringen das Websitezertifikat überprüfen, bevor Sie Ihre Daten lesen möchten.<br/><br/><b>Datenbankabfrage</b>: Geben Sie eine SQL-Anweisung, die die Daten beschreibt Sie lesen möchten.|
Lokalen SQL-Datenbank |Liest Daten, die in einer lokalen SQL-Datenbank gespeichert ist. | <b>Datenverwaltungsgateway</b>: Gibt den Namen der das Datenverwaltungsgateway installiert auf einem Computer, wo sie die SQL Server-Datenbank zugreifen kann. Informationen zum Einrichten eines Gateways finden Sie unter [Ausführen erweiterte Analytics mit Azure maschinellen Learning mithilfe von Daten aus einer lokalen SQLServer](machine-learning-use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Datenbankservername</b>: Gibt den Namen des Servers, auf dem die Datenbank ausgeführt wird.<br/><br/><b>Name der Datenbank </b>: Gibt den Namen der Datenbank auf dem Server. <br/><br/><b>Server-Konto Benutzername</b>: Gibt einen Benutzernamen für ein Konto, das über Zugriffsberechtigungen für die Datenbank verfügt. <br/><br/><b>Benutzername und Kennwort</b>: Klicken Sie auf <b>Werte EINGABETASTE</b> , geben Sie Ihre Datenbankanmeldeinformationen. Sie können integrierte Windows-Authentifizierung oder SQL Server-Authentifizierung, je nachdem, wie Ihre lokalen SQL Server konfiguriert ist.<br/><br/><b>Datenbankabfrage</b>: Geben Sie eine SQL-Anweisung, die die Daten beschreibt Sie lesen möchten.|
Azure Table|Liest Daten aus der Tabelle Dienst in Azure-Speicher.<br/><br/>Wenn Sie große Datenmengen selten lesen möchten, verwenden Sie die Tabelle Azure Service. Es stellt eine flexible nicht relationalen (NoSQL), hochgradig skalierbar, preisgünstige und hoch verfügbare Storage-Lösung.| Die Optionen im Feld **Daten importieren** ändern, je nachdem, ob Sie zugreifen öffentliche Informationen oder einem privaten Speicherkonto, die eine Anmeldung erforderlich ist. Dies wird durch den <b>Authentifizierungstyp</b> denen Wert "PublicOrSAS" oder "Konto", bestimmt jeweils einen eigenen Satz von Parametern verfügt. <br/><br/><b>Öffentliche oder freigegebene Access Signatur (SAS) URI</b>: die Parameter sind:<br/><br/><ul><b>Tabelle URI</b>: Gibt den öffentlichen oder SAS-URL für die Tabelle an.<br/><br/><b>Gibt die Zeilen, die für die Eigenschaftennamen Scannen</b>: die Werte sind <i>TopN</i> so scannen Sie die angegebene Anzahl von Zeilen oder <i>ScanAll</i> , alle Zeilen in der Tabelle zu gelangen. <br/><br/>Wenn die Daten einheitlichen und vorhersehbar sind, wird empfohlen, dass Sie *TopN* wählen Sie aus, und geben Sie eine Zahl für N. Bei großen Tabellen kann dies schneller Lesebereich Zeiten führen.<br/><br/>Wenn die Daten mit Sätze von Eigenschaften, die variieren je nach den Tiefe und die Position der Tabelle strukturiert sind, wählen Sie die Option *ScanAll* so scannen Sie alle Zeilen an. Dadurch wird die Integrität Ihrer resultierende Eigenschaft und Metadaten Konvertierung sichergestellt.<br/><br/></ul><b>Private Speicher Konto</b>: die Parameter sind: <br/><br/><ul><b>Kontoname</b>: Gibt den Namen des Kontos, der die Tabelle zu lesen.<br/><br/><b>Kontoschlüssel</b>: Gibt den Speicherplatz Schlüssel mit dem Konto verbunden sind.<br/><br/><b>Tabellenname</b> : Gibt den Namen der Tabelle, die die zu lesenden Daten enthält.<br/><br/><b>Zeilen, die für die Eigenschaftennamen Scannen</b>: die Werte sind <i>TopN</i> so scannen Sie die angegebene Anzahl von Zeilen oder <i>ScanAll</i> , alle Zeilen in der Tabelle zu gelangen.<br/><br/>Wenn die Daten einheitlichen und vorhersehbar sind, empfehlen wir, dass Sie *TopN* wählen Sie aus, und geben Sie eine Zahl für N. Bei großen Tabellen kann dies schneller Lesebereich Zeiten führen.<br/><br/>Wenn die Daten mit Sätze von Eigenschaften, die variieren je nach den Tiefe und die Position der Tabelle strukturiert sind, wählen Sie die Option *ScanAll* so scannen Sie alle Zeilen an. Dadurch wird die Integrität Ihrer resultierende Eigenschaft und Metadaten Konvertierung sichergestellt.<br/><br/>|
Azure Blob-Speicher | Liest Daten in den Blob-Dienst in Azure-Speicher, einschließlich Bildern, unstrukturierten Text oder binäre Daten gespeichert.<br/><br/>Sie können den Blob-Dienst verwenden, Daten öffentlich verfügbar machen oder privat Anwendungsdaten gespeichert. Sie können von überall aus auf Ihre Daten zugreifen mithilfe von HTTP oder HTTPS-Verbindungen. | Ändern Sie die Optionen im Modul " **Daten importieren** " je nachdem, ob Sie zugreifen öffentliche Informationen oder einem privaten Speicherkonto, die eine Anmeldung erforderlich ist. Dies wird durch den <b>Authentifizierungstyp</b> bestimmt, denen einen Wert von "PublicOrSAS" oder "Konto" können.<br/><br/><b>Öffentliche oder freigegebene Access Signatur (SAS) URI</b>: die Parameter sind:<br/><br/><ul><b>URI</b>: Gibt den öffentlichen oder SAS-URL für den Blob Storage.<br/><br/><b>Dateiformat</b>: Gibt das Format der Daten im Blob-Dienst an. Die unterstützten Formate sind CSV-TSV und ARFF.<br/><br/></ul><b>Private Speicher Konto</b>: die Parameter sind: <br/><br/><ul><b>Kontoname</b>: Gibt den Namen des Kontos, der das Blob enthält Sie lesen möchten.<br/><br/><b>Kontoschlüssel</b>: Gibt den Speicherplatz Schlüssel mit dem Konto verbunden sind.<br/><br/><b>Pfad zum Container, Verzeichnis, oder Blob</b> : Gibt den Namen des Blob, die die zu lesenden Daten enthält.<br/><br/><b>BLOB-Dateiformat</b>: Gibt das Format der Daten im Blob-Dienst an. Die unterstützten Datenformaten sind CSV, TSV, ARFF und CSV mit einer angegebenen Codierung und Excel. <br/><br/><ul>Wenn das Format CSV- oder TSV ist, müssen Sie angeben, ob die Datei eine Kopfzeile enthält.<br/><br/>Sie können die Option Excel verwenden, Lesen von Daten aus Excel-Arbeitsmappen. Geben Sie im die Option <i>Excel-Datenformat</i> an, ob die Daten in einem Excel-Arbeitsblatt-Bereich oder in einer Excel-Tabelle ist. Geben Sie den Namen des Blatts oder der Tabelle, die Sie lesen möchten, in die Option <i>Excel-Tabelle oder eingebettete Tabelle </i>.</ul><br/>|
-Datenfeedanbieter | Liest Daten von einem unterstützten Stare Anbieter. Aktuell wird nur das Open Data Protocol (OData)-Format unterstützt. | <b>Daten Inhaltstyp</b>: Gibt das OData-Format.<br/><br/><b>Quelle URL</b>: Gibt die vollständige URL für den Datenfeed. <br/>Die folgende URL beispielsweise liest, aus der Northwind-Beispieldatenbank: http://services.odata.org/northwind/northwind.svc/|


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
