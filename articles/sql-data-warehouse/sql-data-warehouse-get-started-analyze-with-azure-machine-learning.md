<properties
   pageTitle="Analysieren von Daten mit Azure maschinellen Learning | Microsoft Azure"
   description="Formular mit Azure maschinellen Learning einer Vorhersage maschinellen learning Modell basierend auf den in Azure SQL-Data Warehouse gespeicherten Daten erstellen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/14/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="analyze-data-with-azure-machine-learning"></a>Analysieren von Daten mit Azure Computer-Schulung

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Learning Azure-Computern](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

In diesem Lernprogramm verwendet Azure maschinellen Learning, um einen Vorhersage Computer learning Modell basierend auf den in Azure SQL-Data Warehouse gespeicherten Daten zu erstellen. Insbesondere erstellt dies eine zielgruppenorientierten Marketingkampagne für Adventure Works, die Kopiershop bewältigen kann, indem Vorhersage, wenn Sie ein Kunden wahrscheinlich eine bewältigen kann oder nicht kaufen wird.

> [AZURE.VIDEO integrating-azure-machine-learning-with-azure-sql-data-warehouse]


## <a name="prerequisites"></a>Erforderliche Komponenten
Um dieses Lernprogramm durchgehen, müssen Sie folgende Aktionen ausführen:

- Ein SQL Data Warehouse vorab mit Beispieldaten AdventureWorksDW geladen. Um dies bereitstellen, finden Sie unter [Erstellen einer SQL-Data Warehouse][] , und wählen Sie die Beispieldaten zu laden. Wenn Sie bereits ein Datawarehouse haben, aber keinen Beispieldaten, können Sie die [Beispieldaten manuell laden][].

## <a name="1-get-data"></a>1 Abrufen von Daten
Die Daten sind in der Ansicht dbo.vTargetMail in der Datenbank AdventureWorksDW. Lesen Sie diese Daten:

1. Melden Sie sich bei [Azure maschinellen Learning Studio][] , und klicken Sie auf Meine Versuche.
2. Klicken Sie auf **+ neu** , und wählen Sie **Leere experimentieren**.
3. Geben Sie einen Namen für Ihre experimentieren: Marketing ausgerichtet.
4. Ziehen Sie das **Reader** -Modul aus dem Bereich Module in den Zeichenbereich aus.
5. Geben Sie die Details der Datenbank SQL Data Warehouse im Bereich Eigenschaften an.
6. Geben Sie die Datenbank **Abfrage** , um die relevante Daten zu lesen.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Führen Sie die experimentieren, indem Sie auf **Ausführen** klicken Sie unter den Zeichenbereich experimentieren.
![Ausführen des Versuchs][1]


Nach Beendigung der Versuch erfolgreich ausgeführt klicken Sie auf die Ausgabeanschluss am unteren Rand des Moduls Reader, und wählen Sie **visualisieren** , um die importierten Daten anzuzeigen.
![Anzeigen von importierten Daten][3]


## <a name="2-clean-the-data"></a>2. Bereinigen der Daten
Um die Daten zu bereinigen, legen Sie einige Spalten, die für das Modell nicht relevant sind. Zweck

1. Ziehen Sie das Modul **Project-Spalten** in den Zeichenbereich aus.
2. Klicken Sie auf **Launch Spalte Ansichtsauswahl** im Bereich Eigenschaften, um anzugeben, welche Spalten Sie löschen möchten.
![Project-Spalten][4]

3. Zwei Spalten ausschließen: CustomerAlternateKey und den Begriff geographykey ein.
![Entfernen überflüssiger Spalten][5]


## <a name="3-build-the-model"></a>3 Erstellen Sie 3 das Modell
Wir werden die Daten 80-20 Teilen: 80 % zum Schulen von eines Computers learning Modell und 20 %, um das Modell zu testen. Wir stellen wird der "Zwei-Class" Algorithmen für dieses Problem binäre Klassifizierung verwenden.

1. Ziehen Sie das Modul **geteilten** in den Zeichenbereich aus.
2. Geben Sie 0.8 für Teiler von Zeilen in der ersten Ausgabe Dataset im Bereich Eigenschaften aus.
![Aufteilen von Daten in Schulung und Test festlegen][6]
3. Ziehen Sie das **Beiden-Klasse verstärkt Entscheidungsstruktur** Modul in den Zeichenbereich aus.
4. Ziehen Sie das **Modell Zug** -Modul in den Zeichenbereich, und geben Sie die Eingaben. Klicken Sie dann auf die **Spalte Ansichtsauswahl starten** im Bereich Eigenschaften.
      - Erste Eingabe: ML Algorithmus.
      - Zweiten Eingabeparameter: Daten, die den Algorithmus zu schulen.
![Schließen Sie das Modul Zug Modell][7]
5. Wählen Sie die Spalte **BikeBuyer** , wie die Spalte Vorhersagen.
![Wählen Sie die Spalte Vorhersagen][8]


## <a name="4-score-the-model"></a>4. Punktzahl Modell
Wir werden nun testen, wie das Modell Testdaten durchführt. Vergleichen wir den Algorithmus unsere Wahl mit einer anderen Algorithmus zu finden Sie unter der besser vornimmt.

1. Ziehen Sie **Punktzahl Modell** Modul in den Zeichenbereich aus.
    Erste Eingabe: Modell zweite Eingabe gelernt: Testen der Daten ![Punktzahl Modell][9]
2. Ziehen Sie die **Beiden-Klasse Bayes Punkt Computer** in den Zeichenbereich experimentieren. Wir werden vergleichen, wie dieser Algorithmus im Vergleich zu der beiden-Klasse verstärkt Entscheidungsstruktur ausführt.
3. Kopieren Sie und fügen Sie der Module Zug Model and Punktzahl Modell in den Zeichenbereich ein.
4. Ziehen Sie das **Modell auswerten** -Modul in den Zeichenbereich, die zwei Algorithmen verglichen werden soll.
5. **Führen Sie** die experimentieren.
![Führen Sie die experimentieren.][10]
6. Klicken Sie auf die Ausgabeanschluss am unteren Rand des Moduls Modell ausgewertet werden soll, und klicken Sie auf visualisieren.
![Visualisieren der Auswertung Ergebnisse][11]

Die Metrik zur Verfügung gestellt werden der Kurve ROC, Genauigkeit Rückruf Diagramm, und heben Sie Kurve. Betrachten diese Kennzahlen aus, können wir sehen, dass das erste Modell besser als der zweite durchgeführt. Um anzuzeigen, was das erste Modell regressionsgleichung, klicken Sie auf die Ausgabeanschluss des Modells Punktzahl und Visualisieren klicken Sie auf.
![Visualisieren Resultat][12]

Zwei weitere Spalten, die zum Testen Dataset hinzugefügt werden angezeigt.

- Bewertete Wahrscheinlichkeiten: die Wahrscheinlichkeit, dass ein Kunde ein bewältigen kann Käufer ist.
- Bewertete Etiketten: die Klassifizierung fertig vom Modell – bewältigen kann Käufer (1) oder nicht (0). Dieser Wahrscheinlichkeit Schwellenwert für die Beschriftungen auf 50 % festgelegt ist und angepasst werden kann.

Vergleich der Spalte BikeBuyer (tatsächlich) mit den Bezeichnungen bewertet (Vorhersage), können Sie sehen, wie gut das Modell ausgeführt hat. Nächste Schritte können Sie dieses Modell stellen Vorhersagen für neue Kunden und dieses Modell als Webdienst veröffentlichen oder Schreiben Sie in SQL Data Warehouse Ergebnisse zurück.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Erstellen von Vorhersagen maschinellen learning Modelle finden Sie unter [Einführung in Computer auf Azure Learning][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure maschinellen Learning studio]:https://studio.azureml.net/
[Einführung in Schulung auf Azure-Computer]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Laden Sie die Beispieldaten manuell]: sql-data-warehouse-load-sample-databases.md
[Erstellen einer SQL Datawarehouse]: sql-data-warehouse-get-started-provision.md
