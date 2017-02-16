<properties
   pageTitle="Learning Azure-Computern mit SQL Datawarehouse verwenden | Microsoft Azure"
   description="Lernprogramm für die Verwendung von Azure maschinellen Learning mit Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a>Verwenden von Azure-Computern Learning mit SQL Datawarehouse

Azure maschinellen Learning ist ein vollständig verwaltete Vorhersage Analytics-Dienst, den Vorhersage Modelle anhand von Daten in SQL Data Warehouse zu erstellen, und klicken Sie dann als sofort nutzen, Web Services veröffentlichen. Sie können die Grundlagen des Vorhersageanalytik und Computer durch Lesen [Einführung in Computer auf Azure Learning][]learning erfahren.  Sie können dann Informationen zum Erstellen, Schulen, bewerten und Testen eines Computer Learning-Modells mit [experimentieren Lernprogramm erstellen][].

In diesem Artikel erfahren Sie, wie Sie die folgenden Schritte ausführen mit [Azure maschinellen Learning Studio][]:

- Lesen von Daten aus Ihrer Datenbank zu erstellen, Schulen und bewerten ein Modells Vorhersage
- Schreiben von Daten in Ihrer Datenbank


## <a name="read-data-from-sql-data-warehouse"></a>Lesen von Daten aus SQL Data Warehouse

Wir werden Daten aus der Tabelle Product in der Datenbank AdventureWorksDW gelesen.

### <a name="step-1"></a>Schritt 1

Beginnen einer neuen experimentieren, indem Sie auf + neu am Fuß des Fensters maschinellen Learning Studio EXPERIMENTIEREN wählen Sie aus, und wählen Sie leere experimentieren. Wählen Sie den Standardnamen experimentieren am oberen Rand des Bereichs, und benennen Sie sie in einen aussagekräftigeren Namen, z. B. Zoll Preis Vorhersage.

### <a name="step-2"></a>Schritt 2

Suchen Sie das Modul Reader in der Palette des Datasets und Module auf der linken Seite des Zeichnungsbereichs experimentieren. Ziehen Sie das Modul aus, um den Zeichenbereich experimentieren.
![][drag_reader]

### <a name="step-3"></a>Schritt 3

Füllen Sie im Eigenschaftenbereich, und wählen Sie das Modul Reader.

1. Wählen Sie als Datenquelle SQL Azure-Datenbank aus.
2. Datenbankservername: Geben Sie den Servernamen ein. Sie können der [Azure-Portal][] zur Suche verwenden.

![][server_name]

3. Datenbankname: Geben Sie den Namen einer Datenbank auf dem Server, die Sie gerade angegeben haben.
4. Server-Konto Benutzername: Geben Sie den Benutzernamen eines Kontos, das über Zugriffsberechtigungen für die Datenbank verfügt.
5. Server des Kennworts des Benutzerkontos: Geben Sie das Kennwort für das angegebene Benutzerkonto.
6. Annehmen einer beliebigen Serverzertifikat: Verwenden Sie diese Option (weniger sicher) aus, wenn Sie überspringen das Websitezertifikat überprüfen, bevor Sie Ihre Daten lesen möchten.
7. Datenbankabfrage: Geben Sie eine SQL-Anweisung, die die Daten beschreibt Sie lesen möchten. In diesem Fall liest wir die Daten aus der Tabelle Product, die mithilfe der folgenden Abfrage.


```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a>Schritt 4

1. Führen Sie die experimentieren, indem Sie auf Ausführen klicken Sie unter den Zeichenbereich experimentieren.
2. Nach Abschluss der der Versuch wird das Modul Reader über einem grünen Häkchen, um anzugeben, dass sie erfolgreich durchgeführt wurde. Beachten Sie auch die fertig ausgeführt Status in der oberen rechten Ecke.

![][run]

3. Um die importierten Daten anzuzeigen, klicken Sie auf die Ausgabeanschluss am unteren Rand der Autos Dataset aus, und wählen Sie visualisieren.


## <a name="create-train-and-score-a-model"></a>Erstellen, Schulen und Bewerten eines Modells

Sie können nun Dataset zu verwenden:

- Erstellen ein Modells: Daten zu verarbeiten und Features definieren
- Schulen von Modell: auswählen und anwenden einen Schulung Algorithmus
- Bewerten und Testen des Modells: neue Zoll Preis Vorhersagen


![][model]

Weitere Informationen zum Erstellen von Schulen Sie, Punktzahl und Testen Sie einem Computer learning Modell verwenden die [experimentieren Sie Lernprogramm erstellen][].

## <a name="write-data-to-azure-sql-data-warehouse"></a>Schreiben von Daten in Azure SQL-Data Warehouse

Wir werden das Resultset ProductPriceForecast Tabelle in der Datenbank AdventureWorksDW schreiben.

### <a name="step-1"></a>Schritt 1

Suchen Sie nach Autor Moduls in der Palette des Datasets und Module auf der linken Seite des Zeichnungsbereichs experimentieren. Ziehen Sie das Modul aus, um den Zeichenbereich experimentieren.

![][drag_writer]

### <a name="step-2"></a>Schritt 2

Wählen Sie das Modul Autor, und füllen Sie im Eigenschaftenbereich.

1. Wählen Sie SQL Azure-Datenbank als Datenziel ein.
2. Datenbankservername: Geben Sie den Servernamen ein. Sie können der [Azure-Portal][] zur Suche verwenden.
3. Datenbankname: Geben Sie den Namen einer Datenbank auf dem Server, die Sie gerade angegeben haben.
4. Server-Konto Benutzername: Geben Sie den Benutzernamen eines Kontos, das über Schreibberechtigungen für die Datenbank verfügt.
5. Server des Kennworts des Benutzerkontos: Geben Sie das Kennwort für das angegebene Benutzerkonto.
6. Annehmen einer beliebigen Serverzertifikat (unsichere): Wählen Sie diese Option aus, wenn Sie nicht, um das Zertifikat anzuzeigen möchten.
7. Durch Trennzeichen getrennte Liste der Spalten gespeichert werden: Geben Sie eine Liste der Spalten Dataset oder Ergebnis, das Sie ausgeben möchten.
8. Datentabellenname: Geben Sie den Namen der Datentabelle.
9. Durch Trennzeichen getrennte Liste der Datentabelle Spalten: Geben Sie die Spaltennamen in der neuen Tabelle verwendet. Spaltennamen können sich von denen in der Quelle Dataset sein, aber die gleiche Anzahl von Spalten hier, die Sie für die Ausgabetabelle definieren muss angegeben werden.
10. Anzahl der Zeilen pro Vorgang SQL Azure geschrieben: Sie können die Anzahl der Zeilen, die in einer SQL-Datenbank in einem Vorgang geschrieben werden konfigurieren.

![][writer_properties]

### <a name="step-3"></a>Schritt 3

1. Führen Sie die experimentieren, indem Sie auf Ausführen klicken Sie unter den Zeichenbereich experimentieren.
2. Bei Beendigung der experimentieren müssen alle Module ein grünes Häkchen, um anzugeben, dass er erfolgreich abgeschlossen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung von SQL Data Warehouse][].

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[Übersicht über die Entwicklung von SQL Data Warehouse]: ./sql-data-warehouse-overview-develop.md
[Experimentieren Lernprogramm erstellen]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Einführung in Schulung auf Azure-Computer]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure-Computern Learning Studio]: https://studio.azureml.net/Home
[Azure-portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
