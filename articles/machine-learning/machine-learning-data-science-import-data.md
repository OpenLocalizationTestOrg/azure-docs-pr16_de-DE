<properties
    pageTitle="Importieren von Daten in Computer Learning Studio | Microsoft Azure"
    description="Wie Sie Ihre Daten in Azure Computer Learning Studio aus verschiedenen Datenquellen zu importieren. Erfahren Sie, welche Datentypen und Datenformaten unterstützt werden."
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
    ms.author="garye;bradsev" />


# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>Importieren Sie die Schulung Daten in Azure maschinellen Learning Studio aus unterschiedlichen Datenquellen

Wenn Ihre eigenen Daten zu entwickeln und eine Lösung Vorhersageanalytik Schulen maschinellen Learning Studio verwenden, können Sie folgende Aktionen ausführen: 

- Hochladen von Daten aus einer **lokalen Datei** im Voraus von Ihrer Festplatte ein Dataset Modul im Arbeitsbereich erstellt.  
- Zugriff auf Daten aus einer von mehreren **Datenquellen online** , während Ihr experimentieren ausgeführt wird, verwenden die [Daten importieren] [ import-data] Modul. 
- Verwenden Sie Daten aus einem anderen Azure Computer learning experimentieren, die als ein **Dataset**gespeichert. 

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

Jede dieser Optionen sind in einem der Themen im Menü oben beschrieben. Diese Thema wird gezeigt, wie Daten aus dieser verschiedener Datenquellen zur Verwendung in Computer Learning Studio importieren. 

> [AZURE.NOTE] Es gibt eine Reihe von Stichprobe Datasets in Computer Learning Studio, mit denen Sie für diesen Zweck, zur Verfügung. Informationen dazu finden Sie unter [Verwenden der Stichprobe Datasets in Azure maschinellen Learning Studio](machine-learning-use-sample-datasets.md)).

Diese Einführungsthema auch veranschaulicht, wie Daten zur Verwendung in Computer Learning Studio vorbereiten und beschreibt, welche Datenformaten und Datentypen unterstützt werden. 

> [AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a>Abrufen von Daten in Azure maschinellen Learning Studio zur Verwendung bereit.
Maschinelle Learning Studio dient rechteckige oder Tabellarisch Daten wie Textdaten konzipiert, die getrennt oder strukturiert Daten aus einer Datenbank, obwohl in einigen Fällen nicht rechteckige Daten verwendet werden können.

Es empfiehlt sich, wenn Ihre Daten relativ säubern sind. Das heißt, sollten Sie zu erledigen Problemen, z. B. nicht in Anführungszeichen eingeschlossene Zeichenfolgen, bevor Sie die Daten in Ihrer experimentieren hochladen.

Es stehen jedoch Module in Computer Learning Studio, mit denen einige Manipulation der Daten in Ihrer experimentieren. Je nach dem Computer learning Algorithmen, die Sie verwenden möchten, möglicherweise müssen Sie entscheiden, wie Sie Daten strukturelle Probleme wie fehlende Werte und geringe Datenmengen verarbeitet werden, und es gibt Module, die mit, die Ihnen helfen können. Suchen Sie im Abschnitt **Datentransformation** der Palette Modul für Module, die diese Funktionen durchführen.

Sie können an jeder Stelle in Ihrem Versuch anzeigen oder die Daten, die durch ein Modul erstellt werden, indem Sie mit der rechten Maustaste im Ausgang herunterladen. Je nach dem Modul können verschiedene Downloadoptionen verfügbar sein, oder Sie möglicherweise die Daten in Ihrem Webbrowser maschinellen Learning Studio angezeigt.

## <a name="data-formats-and-data-types-supported"></a>Formate und Daten unterstützten Datentypen

Sie können eine Reihe von Datentypen in Ihrem experimentieren importieren, je nachdem welche Verfahren Sie verwenden, um das Importieren von Daten und, woher diese stammen:

- Nur-Text (txt)
- Durch Trennzeichen getrennte Werte (CSV) mit einer Kopfzeile (CSV) oder ohne (. nh.csv)
- Tabstopps getrennten Werten (TSV) mit einer Kopfzeile (TSV) oder ohne (. nh.tsv)
- Excel-Datei
- Azure-Tabelle
- Strukturtabelle
- SQL-Datenbank-Tabelle
- OData-Werte
- SVMLight Daten (.svmlight) (siehe die [Definition von SVMLight](http://svmlight.joachims.org/) für Formatierungsinformationen)
- Attribut Beziehung Datei Format (ARFF) Daten (.arff) (siehe die [Definition von ARFF](http://weka.wikispaces.com/ARFF) für Formatierungsinformationen)
- ZIP-Datei (ZIP)
- R-Objekt oder Ihren Arbeitsbereich-Datei (. RData)

Wenn Sie Daten in einem Format wie ARFF, die Metadaten enthält importieren, verwendet maschinellen Learning Studio diese Metadaten, um die Überschrift und den Datentyp der einzelnen Spalten definieren.

Datenimports TSV oder CSV-Format, die diese Metadaten enthalten nicht, leitet maschinellen Learning Studio den Datentyp für jede Spalte durch die Daten aufnehmen. Wenn die Daten auch Spaltenüberschriften aufweist, bietet maschinellen Learning Studio Standardnamen aus.

Sie können explizit festlegen oder ändern Sie die Überschriften und Datentypen für Spalten, die mit dem [Bearbeiten der Metadaten][edit-metadata].

Die folgenden **Datentypen** werden vom Computer Learning Studio erkannt:

- Zeichenfolge
- Ganze Zahl
- Double
- Boolesch
- "DateTime"
- TimeSpan

Maschinelle Learning Studio verwendet einen internen Datentyp bezeichnet ***Datentabelle*** Übergabe von Daten zwischen Module. Sie können die Daten explizit in Datentabelle formatieren mithilfe der [Konvertieren in Dataset] konvertieren[ convert-to-dataset] Modul.

Jedem Modul, das als Datentabelle Formate akzeptiert wird die Daten konvertieren in einer Datentabelle im Hintergrund vor der Übergabe an den nächsten Modul.

Bei Bedarf können Sie Daten Tabellenformat wieder in der CSV-, TSV, ARFF oder mit anderen Modulen Konvertierung SVMLight-Format konvertieren.
Suchen Sie im Abschnitt **Daten Konvertierung des Formats** der Palette Modul für Module, die diese Funktionen durchführen.



<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
