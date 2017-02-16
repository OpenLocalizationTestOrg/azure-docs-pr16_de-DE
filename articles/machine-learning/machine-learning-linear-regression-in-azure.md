<properties 
    pageTitle="Verwenden lineare Regression Computer interessante | Microsoft Azure" 
    description="Einen Vergleich der lineare Regression Datenmodellen in Excel und Azure maschinellen Learning Studio" 
    metaKeywords="" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/09/2016" 
    ms.author="kbaroni;garye" />

# <a name="using-linear-regression-in-azure-machine-learning"></a>Verwenden von Azure Computer interessante lineare regression

> *Kate Baroni* und *Robert Boatman* sind Enterprise Lösungsarchitekten in Microsoft Daten Einsichten Center of Excellence. In diesem Artikel beschreiben sie Migrieren einer vorhandenen Regression Analysis-Suite in eine Cloud-basierte Lösung mit Azure maschinellen Learning preiszugeben.  
 
&nbsp; 
  
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  
 
## <a name="goal"></a>Ziel

Unser Projekt mit zwei Ziele vor Augen:  

1. Vorhersageanalytik verwenden, um die Genauigkeit der unserer Organisation monatliche Umsatz Projektionen zu verbessern.  
2. Verwenden von Azure ML zu bestätigen, optimieren, vergrößern die Geschwindigkeit, und die Skalierung der unsere Ergebnisse.  

Wie viele Unternehmen durchläuft eine monatliche Umsatz prognostizieren Prozess unserer Organisation. Unser small Business Analysten-Team wurde mit der Verwendung von maschinellen Schulung zur Unterstützung von des Prozess, und verbessern die Genauigkeit der Wettervorhersage angezeigt werden sollen.  Das Team aufgewendet mehrere Monate Sammeln von Daten aus mehreren Quellen und die Datenattribute durch statistischen Analysen Identifizieren von Key Attributen, die auf die Dienste sales Planung relevanten ausgeführt hat.  Nächste Schritte wurde Prototypen statistische Regression Modelle auf der Registerkarte Daten in Excel zu beginnen.  Innerhalb weniger Wochen hatten wir ein Excel-Regression-Modell, die das aktuelle Feld und Finanzen prognostizieren Prozesse outperforming wurde. Dies bot geplante Vorhersageergebnis.  


Wir haben klicken Sie dann im nächsten Schritt zu unseren Vorhersageanalytik über auf Azure ML ermitteln, wie Azure ML Vorhersage Leistung verbessern konnte bewegen.


## <a name="achieving-predictive-performance-parity"></a>Vorhersagen Leistung Unstimmigkeit erreichen

Unsere erste Priorität wurde Unstimmigkeit zwischen Azure ML und Excel Regression Modellen zu erzielen.  Angegebenen genauen derselben Daten, und die gleichen Teilung für Schulung und Testen von Daten, die wir Vorhersage Leistung Unstimmigkeit zwischen Excel und Azure ML erzielen möchten.   Fehler beim Anfangs wir. Das Excel-Datenmodell übertroffen Azure ML Modell.   Der Fehler wurde wegen unzureichender Verständnis der Basis Tool Einstellung in Azure ML. Nach eine Synchronisierung mit dem Produktteam Azure ML wir besser zu verstehen der Einstellung für unsere Datasets erforderlichen Basis gewonnen und Unstimmigkeit zwischen den beiden Modellen erzielt.  

### <a name="create-regression-model-in-excel"></a>Erstellen von Regressionsmodell in Excel
Unsere Regression Excel verwendet das Modell von standard lineare Regression in der Excel-Analyse-Funktionen. 

Wir berechnete *Mittelwert absoluten % zurück* , und es als die Leistungsmeasure für das Modell verwendet.  Zum Einblenden der zugehörigen eines Arbeitsmodells mithilfe von Excel 3 Monate gedauert hat.  Wir geschaltet viele der Schulung in der Azure ML Versuch der letzten Endes in Grundlegendes zu Anforderungen Vorteil wurde.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Erstellen von vergleichbaren experimentieren Azure Computer interessante  
Wir gefolgt folgendermaßen vor, um unser experimentieren in Azure ML zu erstellen:  

1.  Hochgeladen Dataset als eine CSV-Datei auf Azure ML (sehr klein)
2.  Erstellt eine neue experimentieren, und [Wählen Sie Spalten im Dataset] verwendet[ select-columns] Modul Sie dieselben in Excel verwendete Datenfeatures auswählen   
3.  Verwendet die [Aufgeteilten Daten] [ split] Modul (mit *Relativen Ausdruck* Modus), die Daten an die genaue gleichen Zug unterteilen, wie in Excel erledigt  
4.  Mit der [Lineare Regression] vertraut gemacht[ linear-regression] dokumentierten Modul (Standardoptionen nur), und im Vergleich der Ergebnisse in unseren Regression Excel-Datenmodell

### <a name="review-initial-results"></a>Überprüfen Sie die ersten Ergebnisse
Zuerst übertroffen das Excel-Datenmodell klar Azure ML Modell hat:  

|   |Excel|Azure ML|
|---|:---:|:---:|
|Leistung|   |  |
|<ul style="list-style-type: none;"><li>So angepasst R Quadrat</li></ul>| 0.96 |N/V|
|<ul style="list-style-type: none;"><li>Koeffizienten <br />Ermittlung</li></ul>|N/V|   0.78<br />(niedrig Genauigkeit)|
|Mittelwert der absoluten zurück |  $9. 5M|  19.4 $ M|
|Mittlerer absoluten Fehler (%)|   6.03 %|  12.2 %

Wenn wir Vorgang und die Ergebnisse vom Entwickler und Daten Wissenschaftler Azure ML Teammitglieder ausgeführt haben, sie einige nützliche Tipps schnell zur Verfügung.  

* Wenn Sie verwenden die [Lineare Regression] [ linear-regression] Modul in Azure ML, zwei Methoden werden bereitgestellt:
    *  Online Farbverlauf Abstieg: Möglicherweise besser geeignet für umfangreichere Probleme
    *  Einfache kleinsten Quadrate: Dies ist die Methode, die, der meisten Personen vorstellen, wenn sie lineare Regression hören. Für kleine Datasets kann einfache kleinsten Quadrate eine weitere optimale Wahl sein.
*  Erwägen Sie die Anpassung des Parameters L2 Regularization Stärke, um die Leistung zu verbessern. Festlegen auf 0.001 standardmäßig und für unsere kleine Datenmenge, die wir 0,005 für optimale Leistung festlegen.    

### <a name="mystery-solved"></a>Mysterium lösen des vorliegenden!
Wenn wir die Empfehlungen angewendet, erreicht wir die gleichen geplante Leistung in Azure ML als mit Excel:   

|| Excel|Azure ML (Initiale)|Mit der kleinsten Quadrate Azure ML|
|---|:---:|:---:|:---:|
|Beschriftete Wert  |Verfolgt (numerisch)|gleichen|gleichen|
|Teilnehmern  |Excel-Daten > Analysis -> Regression|Lineare Regression.|Lineare Regression|
|Optionen von Teilnehmern|N/V|Standardeinstellungen|einfache kleinsten Quadrate<br />L2 = 0,005|
|Datengruppe zurück|26 Zeilen, 3 Features, 1 Beschriftung.   Alle numerischen.|gleichen|gleichen|
|Teilen: Zug|Excel gelernt in den ersten 18 Zeilen, die in den letzten 8 Zeilen getestet.|gleichen|gleichen|
|Teilen: Testen|Excel Regression Formel auf die letzte 8 Zeilen angewendet|gleichen|gleichen|
|**Leistung**||||
|So angepasst R Quadrat|0.96|N/V||
|Koeffizienten von Ermittlung|N/V|0.78|0.952049|
|Mittelwert der absoluten zurück |$9. 5M|19.4 $ M|$9. 5M|
|Mittlerer absoluten Fehler (%)|<span style="background-color: 00FF00;">6.03 %</span>|12.2 %|<span style="background-color: 00FF00;">6.03 %</span>|

Darüber hinaus im Vergleich der Excel-Koeffizienten gut für das Feature-Stärken im Modell Azure ausgebildeten ein:

||Excel-Koeffizienten|Azure Feature-Stärken|
|---|:---:|:---:|
|Achsenabschnitt/Verschiebung|19470209.88|19328500|
|A-Feature|0.832653063|0.834156|
|Feature "B"|11071967.08|11007300|
|Funktion C|25383318.09|25140800|

## <a name="next-steps"></a>Nächste Schritte

Wir möchten Azure ML Webdienst in Excel verwenden.  Unsere Business Analysten abhängig von Excel, und wir eine Möglichkeit zum Aufrufen der Azure ML-Webdiensts für eine Reihe von Excel-Daten und geschätzten Werte nach Excel wechselt erforderlich.   

Es sollte auch unser Modell, optimieren mithilfe der Optionen und Algorithmen, die in Azure ML verfügbar.

### <a name="integration-with-excel"></a>Integration in Excel
Unsere Lösung wurde, um unser Azure ML Regressionsmodell Prozessen durch Erstellen von einem Webdienst aus dem Modell ausgebildeten umsetzen.  Innerhalb weniger Minuten der Webdienst erstellt wurde, und wir konnten nennen Sie diese direkt in Excel einen geschätzten Umsatzwert zurückgegeben.    

Im Abschnitt *Web Services-Dashboards* enthält eine herunterladbare Excel-Arbeitsmappe.  Die Arbeitsmappe im Lieferumfang von vorformatierten der eingebettete Web Service-API und Schema Informationen.   Beim Klicken auf die *Excel-Arbeitsmappe herunterladen*, es wird geöffnet, und Sie können mit Ihrem lokalen Computer speichern.    

![][1]
 
Mit der Arbeitsmappe öffnen werden kopieren Sie Ihre vordefinierten Parameter in den blauen Parameter Abschnitt, wie unten dargestellt.  Sobald die Parameter eingegeben werden, Excel Sie an den Webdienst AzureML ruft und die geschätzte bewertet werden Etiketten im Abschnitt geschätzte Werte grün angezeigt.  Die Arbeitsmappe weiterhin Vorhersagen für Parameter auf Grundlage von ausgebildeten Modell für alle eingegeben haben, klicken Sie unter Parameter Zeilenelementen erstellen.   Weitere Informationen zum Verwenden dieser Funktion finden Sie unter [Verarbeitung eines Webdiensts Azure maschinellen Learning aus Excel](machine-learning-consuming-from-excel.md). 

![][2]
 
### <a name="optimization-and-further-experiments"></a>Optimierung und weitere Versuche
Jetzt, da wir einen Basisplan mit unseren Excel-Datenmodell hatten, verschoben wir anstehen unser Azure ML lineare Regressionsmodell zu optimieren.  Wir verwendet das Modul [Featureauswahl Filter-basierten] [ filter-based-feature-selection] zur Verbesserung der Auswahl des Ausgangsdaten Elemente und es beigetragen uns eine Verbesserung der Leistung von 4,6 % erzielen Mittelwert absoluten zurück.   Für zukünftige Projekte verwenden wir dieses Feature die uns Wochen speichern konnte, bei der Iteration durch Datenattribute zum Suchen des richtigen Satzes von Features für die Modellierung verwendet werden soll.  

Weiter wir zusätzliche Algorithmen wie [Bayes'sche] einbeziehen möchten[ bayesian-linear-regression] oder [Verstärkt Entscheidungsbäume] [ boosted-decision-tree-regression] in unseren experimentieren, um die Leistung zu vergleichen.    

Wenn Sie mit Regression experimentieren möchten, ist ein guter Dataset zu versuchen Aufwand Effizienz Regression Stichprobendataset, das sind zahlreiche numerischen Attributen aus. Das Dataset wird als Teil der Stichprobe Datasets in ML Studio bereitgestellt.  Eine Vielzahl von learning Module können Sie entweder Heizung Auslastung oder Kühlung Auslastung Vorhersagen.  Anhand der folgenden Tabelle ist, dass ein Vergleich der verschiedenen Regression Leistung gegen Aufwand Effizienz Dataset Vorhersage für die Zielvariable Kühlung laden lernfähig: 

|Modell|Mittelwert der absoluten zurück|Quadratwurzel Mittel im Quadrat zurück|Relativen, absoluten zurück|Relativer im Quadrat zurück|Koeffizienten von Ermittlung
|---|---|---|---|---|---
|Stärkere Entscheidungsstruktur|0.930113|1.4239|0.106647|0.021662|0.978338
|Lineare Regression (Farbverlauf Unterlänge)|2.035693|2.98006|0.233414|0.094881|0.905119
|Trainingsfällen Regression|1.548195|2.114617|0.177517|0.047774|0.952226
|Lineare Regression (einfache kleinsten Quadrate)|1.428273|1.984461|0.163767|0.042074|0.957926  

## <a name="key-takeaways"></a>Hauptargumente 

Wir gelernt sehr durch aus laufenden Excel Regression und Azure maschinellen Learning Weinbauversuche parallel. Geplante Modell in Excel erstellen und Vergleich mit Modellen mit Azure ML [Lineare Regression] [ linear-regression] beigetragen uns Azure ML erfahren Sie, und wir Verkaufschancen zum Verbessern der Leistung von Daten Auswahl und Modell erkannt.         

Wir festgestellt, dass es ratsam [Featureauswahl Filter-basierten] verwenden ist[ filter-based-feature-selection] Projekte Vorhersage zukünftiger beschleunigen.  Featureauswahl auf Ihre Daten anwenden, können Sie ein verbessertes Modell in Azure ML mit bessere Leistung insgesamt erstellen. 

Die Möglichkeit zum Übertragen der Vorhersage analytisches Prognostizieren von Azure ML nach Excel Vorabgenehmigungsantrag ermöglicht eine wesentlich höhere in die Möglichkeit, um Ergebnisse auf eine umfassende Business Benutzergruppe als Zielgruppe erfolgreich zu ermöglichen.     


## <a name="resources"></a>Ressourcen
Einige Ressourcen werden für Hilfe aufgelistet, die Sie mit Regression arbeiten:  

* Regressionsanalyse in Excel.  Wenn Sie in Excel Regression nie versucht haben, in diesem Lernprogramm erleichtert: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Prognostizieren von Regression im Vergleich mit einer.  Tyler Chessman hat einen Blog-Artikel erläutert, wie die Uhrzeit in Excel, die eine gute Einführung Beschreibung der lineare Regression enthält prognostizieren Reihe geschrieben. [http://sqlmag.com/SQL-Server-Analysis-Services/Understanding-Time-Series-Forecasting-Concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts)  
*   Einfache kleinsten Quadrate lineare Regression: Fehler, Probleme und unvorhergesehene Probleme.  Für eine Einleitung und eine Erläuterung der Regression: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/ )

[1]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
