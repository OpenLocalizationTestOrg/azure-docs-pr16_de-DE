<properties
    pageTitle="Verwenden Sie die Beispieldatensätzen in Computer Learning Studio | Microsoft Azure"
    description="Beschreibungen der Datengruppen in Beispiel-Modellen im Lieferumfang von ML Studio verwendet. Sie können diese Beispieldatensätzen für Ihre Versuche verwenden."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="use-the-sample-data-sets-in-azure-machine-learning-studio"></a>Verwenden Sie die Beispieldatensätzen in Azure maschinellen Learning Studio

[top]: #machine-learning-sample-datasets

Bei der Erstellung eines neuen Arbeitsbereichs Azure Computer interessante sind eine Reihe von Beispieldatensätzen und Versuche standardmäßig enthalten. Viele dieser Beispieldatensätzen werden von den Beispiel-Modellen im [Azure Cortana Intelligence Katalog](http://gallery.cortanaintelligence.com/)verwendet, und andere sind als Beispiele für verschiedene Arten von Daten in der Regel interessante Computer verwendet.

Einige dieser Daten Sätze stehen in Azure Blob-Speicher. Für diese Datensätze enthält, in der nachfolgenden Tabelle einen direkten Link. Sie können diese Datensätze in Ihre Versuche verwenden, mit den [Daten importieren] [ import-data] Modul.

Die restlichen diese Beispieldatensätzen werden unter **Datasets gespeichert** in der Palette Modul, das links neben den Zeichenbereich experimentieren aufgeführt, wenn Sie öffnen oder erstellen eine neue experimentieren in ML Studio.
Eine der folgenden Datensätze können in Ihrer eigenen experimentieren per Drag & Drop zu Ihrem Zeichnungsbereich experimentieren.


<!--
For a list of sample experiments available in ML Studio, see [Machine Learning Sample Experiments][sample-experiments].

[sample-experiments]: machine-learning-sample-experiments.md
-->

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<table>

<tr>
  <th align=left>DataSet-name</th>
  <th align=left>Datasetbeschreibung</th>
</tr>

<tr>
  <td valign=top>Erwachsenen Erhebung Einkommen binäre Klassifizierung dataset</td>
  <td valign=top>
Eine Teilmenge der Datenbank Erhebung 1994, funktioniert Erwachsene über das Alter von 16 mit einem angepassten Einkommen Index > 100 verwenden.<p> </p><b>Syntax:</b> klassifizieren Demographie Vorhersagen, ob eine Person über 50 K ein Jahr erzielt verwenden.<p> </p><b>Verwandte Recherchieren:</b> Kohavi, r, Becker, b, (1996). UCI maschinellen Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, Schule of Information und Computer Science </td>
</tr>

<tr ID=airport-codes-dataset>
  <td valign=top>AirPort Codes Dataset</td>
  <td valign=top>
US Airport Codes.<p> </p>Dieses Dataset enthält eine Zeile für jeden US Airport, der Airport-ID-Zahl und Namen zusammen mit den Speicherort Ort und Zustand bereitstellt.
  </td>
</tr>

<tr>
  <td valign=top>Kurs Autos Daten (Rohstoffe)</td>
  <td valign=top>
Informationen zu Autos von Stellen und modellieren, einschließlich der Preis Features wie die Anzahl der Zylinder und MPG sowie eines Formulars für Risikowert.<p> </p>Der Risikowert Anfangs automatisch Kurs zugeordnet ist, und klicken Sie dann tatsächlichen Risiko in einem Prozess, der bekanntermaßen stellen als symboling angepasst. Der Wert + 3 gibt an, dass das automatische riskant ist und ein Wert von-3 die It wahrscheinlich ziemlich sichere.<p> </p><b>Syntax:</b> Vorhersagen der Risikowert durch Features Regression oder Multivariate Klassifizierung verwenden. <p> </p><b>Verwandte Recherchieren:</b> Schlimmer, J.C. (1987). UCI maschinellen Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, Schule of Information und Computer Science </td>
</tr>

<tr ID=bike-rental-uci-dataset>
  <td valign=top>Bewältigen kann Miete UCI dataset</td>
  <td valign=top>
UCI bewältigen kann Miete Dataset, die auf real Daten aus Groß-Bikeshare Unternehmen, die einem bewältigen kann Miete Netzwerk in Washington DC unterhält basiert.<p> </p>Das Dataset enthält eine Zeile für jede Stunde des Tages in 2011 und 2012 für insgesamt 17,379 Zeilen ein. Der Zellbereich, der stündlich bewältigen kann Mieten ist von 1 auf 977.

  </td>
</tr>

<tr ID=bill-gates-rgb-image>
  <td valign=top>Bill Gates RGB-Bild</td>
  <td valign=top>
CSV-Daten in öffentlich zugängliche Bilddatei konvertiert.<p> </p>Der Code zum Konvertieren des Bilds wird in der Detailseite Modell <strong>Quantisierung K-bedeutet, dass Cluster mit Farbe</strong> bereitgestellt.
  </td>
</tr>

<tr>
  <td valign=top>Blutdruckmessung Spenden-Daten</td>
  <td valign=top>
Eine Teilmenge der Daten aus der Datenbank blutdruckmessung Spender Bluttransfusion Service Center von Hsin-Chu Ort, Taiwan.<p> </p>Spender Daten enthält die Monaten seit der letzten Spenden), und Häufigkeit oder die Gesamtzahl der Spenden, Zeit seit der letzten Spenden und Menge an blutdruckmessung gespendet.<p> </p><b>Syntax:</b> Ziel über Klassifizierung Vorhersagen, ob der Spender blutdruckmessung im März 2007 Gespendeter, 1 zeigt, wo eine Spender an, während der Zielperiode und 0 ein nicht-Geber ist. <p> </p><b>Verwandte Recherchieren:</b> Yeh, I.C., (2008). UCI maschinellen Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, Schule of Information und Computer Science <p> </p>Yeh, I-Cheng, Yang, König-Jang, und Ting, Ming-Tao, "Knowledge Discovery auf RFM Modell über Bernoulli-Folge" Experten Systeme mit Applications, 2008, <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr ID=book-reviews-from-amazon>
  <td valign=top>Adressbuch Bewertungen von Amazon</td>
  <td valign=top>
Kritiken Bücher in Amazon, von der Website amazon.com University of Pennsylvania Virenforscher (<a href="http://www.cs.jhu.edu/~mdredze/datasets/sentiment/">Grüße</a>) geöffnet. Finden Sie im Dokument Recherchieren "Biographien, Bollywood, Ausleger-Feldern und Blenders: Domäne Anpassung für Grüße Klassifikation" Johann Blitzer Dredze markieren und Fernando Pereira; Zuordnung von Computerlinguistik (ACL), 2007.<p> </p>Das ursprüngliche Dataset hat 975K Prüfungen mit Rangfolgen 1, 2, 3, 4 und 5. Die Prüfungen in Englisch geschrieben wurden und den Zeitraum 1997-2007 sind. Dieses Dataset wurde unten auf 10 K Prüfungen aufgenommen.
  </td>
</tr>

<tr>
  <td valign=top>Brust Krebs Daten</td>
  <td valign=top>
Eine der drei Krebs-bezogene Datasets bereitgestellten vom Onkologie Institute, die häufig in Computer Learning-Dokumentation angezeigt wird. Kombiniert mit Features von Laboranalyse Diagnoseinformationen von rund 300 Gewebeproben an.<p> </p><b>Syntax:</b> den Typ des Krebs klassifizieren, basierend auf 9 Attributen, von denen einige linearen und einige kategorisierten sind. <p> </p><b>Verwandte Recherchieren:</b> Wohlberg, W.H., Straße, W.N. und Mangasarian, O.L. (1995). UCI maschinellen Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, Schule of Information und Computer Science </td>
</tr>

<tr ID=breast-cancer-features>
  <td valign=top>Brust Krebs Features <td valign=top>
Das Dataset enthält Informationen für 102K verdächtigen Regionen (Kandidaten) von Röntgenbilder, jeweils durch 117 Features beschrieben. Die Features sind eigene und deren Bedeutung durch die Dataset Ersteller (Siemens Gesundheitswesen) nicht angezeigt wird. 
  </td>
</tr>

<tr ID=breast-cancer-info>
  <td valign=top>Brust Krebs Informationen</td>
  <td valign=top>
Das Dataset enthält weitere Informationen für die einzelnen Regionen verdächtigen Röntgenaufnahme Bilds. Jedes Beispiel enthält Informationen (z. B. Etikett Patienten Koordinaten relativ zum gesamten Bild Patch-ID) über die entsprechenden Zeilennummer im Dataset Brust Krebs Features. Jede Patienten verfügt über eine Reihe von Beispielen. Für Patienten eine Krebs Berechtigung, einige Beispiele für positive und einige sind negativ. Für Patienten, die eine Krebs besitzen, sind negativ Beispiele. Das Dataset enthält Beispiele 102K. Das Dataset verzerrt, 0,6 % der Punkte werden positive, der Rest ist negativ. Das Dataset wurde von Siemens Gesundheitswesen zur Verfügung gestellt.
  </td>
</tr>

<tr ID=crm-appetency-labels-shared>
  <td valign=top>CRM Appetency Etiketten freigegeben</td>
  <td valign=top>
Etikettenbogen KDD Cup 2009 Kunden Beziehung Vorhersage Herausforderung (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr ID=crm-churn-labels-shared>
  <td valign=top>CRM Änderung Etiketten freigegeben</td>
  <td valign=top>
Etikettenbogen KDD Cup 2009 Kunden Beziehung Vorhersage Herausforderung (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr ID=crm-dataset-shared>
  <td valign=top>CRM Dataset freigegeben</td>
  <td valign=top>
Diese Daten stammen aus der KDD Cup 2009 Beziehung Vorhersage Herausforderung für den Kunden (<a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>).<p> </p>Das Dataset enthält 50K Kunden des Unternehmens Französisch Telecom Orange. Hat 230 anonymes Features, die 190, mit denen numerische sind die einzelnen Kunden und 40 kategorisierten sind. Die Features sind sehr gering.
  </td>
</tr>

<tr ID=crm-upselling-labels-shared>
  <td valign=top>CRM Upselling Etiketten freigegeben</td>
  <td valign=top>
Etikettenbogen KDD Cup 2009 Kunden Beziehung Vorhersage Herausforderung (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td valign=top>Aufwand Effizienz Regression Daten</td>
  <td valign=top>
Eine Zusammenstellung von simulierten Aufwand Profile, basierend auf 12 unterschiedliche Baustein-Shapes. Die Gebäude unterscheiden sich in Bezug auf durch 8 Features wie Glas Bereich, Glas Bereich Verteilung und Ausrichtung unterschieden.<p> </p><b>Syntax:</b> verwenden Regression oder Klassifizierung, um die Bewertung basierend als eine von zwei tatsächlichen Werte Antworten Aufwand Effizienz Vorhersagen. Für mehrere Klasse Einstufung ist oder die Variable Antwort auf die nächste ganze Zahl abrunden. <p> </p><b>Verwandte Recherchieren:</b> Xifara, a & Tsanas, a (2012). UCI maschinellen Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, Schule of Information und Computer Science </td>
</tr>

<tr ID=flight-delays-data>
  <td valign=top>Flight verzögert Daten</td>
  <td valign=top>
Zugelassenen auf Zeit Leistung Flugdaten entnommen die TranStats Sammlung von Daten von der US-Verkehrsministerium (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">Zeit</a>).<p> </p>Das Dataset behandelt den Zeitraum April-Oktober 2013. Vor dem Hochladen auf Azure ML Studio, wurde das Dataset wie folgt verarbeitet:<ul><li>Das Dataset wurde so gefiltert, die nur 70 höchsten Auslastung Flughäfen in den USA Festland Deckblatt</li><li>Abgebrochen Flüge wurden als durch maximal 15 Minuten verzögert gekennzeichnet ist</li><li>Umgeleitete Flüge herausgefiltert wurden</li><li>Die folgenden Spalten ausgewählt wurden: Jahr, Monat, DayofMonth, Wochentag, Carrier, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, abgebrochen</li></ul>
</td>
</tr>

<tr>
  <td valign=top>Flight Uhrzeit-Leistung (Rohstoffe)</td>
  <td valign=top>
Datensätze Flugzeug Flight hinsichtlich der Ankunftszeiten und Abflugzeiten innerhalb der USA von Oktober 2011.<p> </p><b>Syntax:</b> Vorhersagen Flight Delays. <p> </p><b>Verwandte Recherchieren:</b> aus US-Abteilung von Shapes für Transport <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time</a>.
  </td>
</tr>

<tr>
  <td valign=top>Gesamtstruktur wird ausgelöst, Daten</td>
  <td valign=top>
Wetterdaten, wie Temperatur und Feuchtigkeit Indizes und windgeschwindigkeit, in einen Bereich des Nordost Portugal, kombiniert mit Waldbrandrisiko Datensätze enthält.<p> </p><b>Syntax:</b> Dies ist eine Aufgabe schwierig Regression das Ziel darin, den selbst Bereich des Waldbränden Vorhersagen. <p> </p><b>Verwandte Recherchieren:</b> Cortez, s., und Morais, a (2008). UCI maschinellen Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, Schule of Information und Computer Science <p> </p>[Cortez und Morais, 2007] S. Cortez und a Morais. Ein Daten Mining Ansatz zum Vorhersagen Waldbrandrisiko Meteorologische Daten verwenden. In J. Neves, m F. Santos und J. Machado Eds., neue Trends in KI, Verfahren der 13 EPIA 2007 - Portugiesisch Konferenz auf KI, Dezember, Guimarães, Portugal, Seiten 512-523, 2007. APPIA, ISBN-13 978-989-95618-0-9. Verfügbar: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr ID=german-credit-card-uci-dataset>
  <td valign=top>Deutsch Kreditkarte UCI dataset</td>
  <td valign=top>
UCI Statlog (Deutsch Kreditkarte) das Dataset (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + Deutsch + Kreditkarte + Daten</a>), verwenden die Datei german.data.<p> </p>Das Dataset klassifiziert Personen durch einen Satz von Attributen, die als niedrig oder hoch Kreditkarte Risiken beschrieben. Jedes Beispiel stellt eine Person an. Es gibt 20 Features, numerischen und Kategorieliste, und eine binäre Bezeichnung (Risikowert Kreditkarte). Kreditkarte hohem Risiko Einträge haben Bezeichnung = 2, Niedrig Kreditkarte Risiko Einträge haben Bezeichnung = 1. Die Kosten ein Beispiels niedrig Risiko als hoch sicherzustellen ist 1, während die Kosten ein Beispiels hohem Risiko als niedrig sicherzustellen 5 ist.
  </td>
</tr>

<tr ID=imdb-movie-titles>
  <td valign=top>IMDB Filmtitel</td>
  <td valign=top>
Das Dataset enthält Informationen zur Filme, die in Twitter Tweets bewertet wurden: IMDB Film-ID Film Namen und Genre, Fertigung Jahr. Es gibt 17 KB Filme im Dataset aus. Das Dataset wurde in das Papier "S. eingeführt werden. Dooms, T. De Pessemier und l Martens. MovieTweetings: ein Films im Dataset von Twitter gesammelt werden. Workshop Crowdsourcing und personenbezogenen Berechnung für Recommender Systeme, CrowdRec am RecSys 2013".
  </td>
</tr>

<tr>
  <td valign=top>Iris zwei Klassendaten</td>
  <td valign=top>
Vielleicht ist dies die beste bekannte Datenbank, in der Spracherkennung-Dokumentation Muster gefunden werden soll. Datenmenge ist relativ klein, 50 Beispiele des Blatts Maße aus drei Iris Varianten enthält.<p> </p><b>Syntax:</b> die Art des Iris aus der Maße Vorhersagen.  <p> </p><b>Verwandte Recherchieren:</b> Fisher, R.A. (1988). UCI maschinellen Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, Schule of Information und Computer Science </td>
</tr>

<tr ID=movie-tweets>
  <td valign=top>Film Tweets</td>
  <td valign=top>
Das Dataset ist eine erweiterte Version des Datasets Film Tweetings. Das Dataset hat 170K Bewertungen für Filme aus gut strukturierten Tweets auf Twitter extrahiert. Jede Instanz stellt einen Tweet und eines Tupels ist: Benutzer-ID, IMDB Film-ID, Bewertung, Zeitstempel, Anzahl von Favoriten für diesen Tweet und die Anzahl der Retweets von diesem Tweet. Das Dataset wurde durch a gesagt, S. Dooms, b Loni und d Tikk für Recommender Systeme Herausforderung 2014 zur Verfügung.
  </td>
</tr>

<tr>
  <td valign=top>MPG Daten für verschiedene Autos</td>
  <td valign=top>
Dieses Dataset ist eine leicht abgewandelte Version des Datasets von der Bibliothek StatLib des Carnegie Mellon University bereitgestellt. Das Dataset wurde in den 1983 American statistische Association Darstellung verwendet.<p> </p>Die Daten sind für verschiedene Autos in Meilen pro Gallone, zusammen mit Informationen wie etwa der Anzahl von Zylindern, Engine Verschiebung, Pferdestärken (PS), Gesamtgewicht und Beschleunigung Kraftstoffverbrauch aufgeführt.<p> </p><b>Syntax:</b> Vorhersagen Kraftstoffverbrauch basierend auf 3 mehrwertigen diskrete und 5 kontinuierlichen Attributen. <p> </p><b>Verwandte Recherchieren:</b> StatLib, Carnegie Mellon University, (1993). UCI maschinellen Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, Schule of Information und Computer Science </td>
</tr>

<tr>
  <td valign=top>Pima Indianern Diabetes binäre Klassifizierung dataset</td>
  <td valign=top>
Eine Teilmenge der Daten aus den nationalen Institute Diabetes und Magen- und Niere Krankheiten Datenbank. Das Dataset wurde Konzentration auf Frauen Patienten der indische Erbe Pima gefiltert. Die Daten enthält, medizinische Daten wie Glukose und Ebenen Inulinsirup sowie Leben Faktoren.<p> </p><b>Syntax:</b> Vorhersagen, ob der Betreff Diabetes (binäre Klassifizierung) verfügt. <p> </p><b>Verwandte Recherchieren:</b> Sigillito, Version (1990). UCI maschinellen Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml "</a>. Irvine, CA: University of California, Schule of Information und Computer Science </td>
</tr>

<tr>
  <td valign=top>Kundendaten Restaurant</td>
  <td valign=top>
Eine Reihe von Metadaten für die Kunden, einschließlich Demographie und Einstellungen.<p> </p><b>Syntax:</b> dieses Dataset, in Kombination mit der anderen beiden Restaurant Datasets, Schulen und Testen eines Systems Recommender verwenden. <p> </p><b>Verwandte Recherchieren:</b> Bache, K. und Lichman, m (2013). UCI maschinellen Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, Schule of Information und Computer Science.
  </td>
</tr>

<tr>
  <td valign=top>Restaurant Featuredaten</td>
  <td valign=top>
Eine Reihe von Metadaten über Restaurants und ihre Features, wie z. B. Essen Typ, Restaurant Formatvorlage und Position.<p> </p><b>Syntax:</b> dieses Dataset, in Kombination mit der anderen beiden Restaurant Datasets, Schulen und Testen eines Systems Recommender verwenden. <p> </p><b>Verwandte Recherchieren:</b> Bache, K. und Lichman, m (2013). UCI maschinellen Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, Schule of Information und Computer Science.
  </td>
</tr>

<tr>
  <td valign=top>Restaurant Bewertungen</td>
  <td valign=top>
Enthält Bewertungen von Benutzern zu Restaurants von 0 bis 2 auf einer Skala angegeben.<p> </p><b>Syntax:</b> dieses Dataset, in Kombination mit der anderen beiden Restaurant Datasets, Schulen und Testen eines Systems Recommender verwenden. <p> </p><b>Verwandte Recherchieren:</b> Bache, K. und Lichman, m (2013). UCI maschinellen Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, Schule of Information und Computer Science.
  </td>
</tr>

<tr>
  <td valign=top>Stählerne Annealing mit mehreren Klasse dataset</td>
  <td valign=top>
Dieses Dataset enthält eine Reihe von Datensätzen aus Stahl Ausglühen Versuche mit den physischen Attributen (Breite, Stärke, Typ (Schlange, Blatt usw.) des resultierenden stählerne Typen.<p> </p><b>Syntax:</b> Vorhersagen eines zwei numerische Class Attribute; Härte oder Stärke. Sie können auch Korrelationen zwischen Attributen analysieren.<p> </p>Stahlsorten führen Sie einen Standard Festlegen von SAE sowie von anderen Organisationen definiert. Sie für einen bestimmten 'Noten' (Class-Variable) suchen und die erforderlichen Werte verstehen möchten. <p> </p><b>Verwandte Recherchieren:</b> Sterling, d & Buntine, w (NV). UCI maschinellen Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, Schule of Information und Computer Science <p> </p>Ein nützliches Handbuch zu Stahlsorten finden Sie hier: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td valign=top>Teleskop Daten</td>
  <td valign=top>
Datensätze hoher Aufwand Gamma Particle Bursts sowie Hintergrundgeräusche, eines simulierten beide mit einem Monte Carlo Prozess.<p> </p>Der Zweck der Simulation wurde, um die Genauigkeit der Grund-basierten Verringerung Cherenkov Gamma Teleskope, statistische Methoden verwenden, um zwischen den gewünschten Signal (Duschen Cherenkov Strahlen) und Hintergrundgeräusche (hadronic Duschen initiiert von Kosmische in der oberen Luft) zu unterscheiden zu verbessern.<p> </p>Die Daten wurde vorher bearbeitete So erstellen Sie einen verlängerte Cluster mit Long Achse zur Mitte Kamera ausgerichtet ist. Die Merkmale dieser Ellipse, (oft Hillas Parameter bezeichnet) werden zwischen der Abbildung Parameter, die zur Diskriminierung verwendet werden können.<p> </p><b>Syntax:</b> Vorhersagen, ob ein Bild von einer Party Signal oder Hintergrund Rauschen darstellt.<p> </p><b>Hinweise:</b> einfache Klassifizierung Genauigkeit ist nicht für diesen Daten aussagekräftiger seit Klassifizierung von ein Ereignis Hintergrund ungeändert Signal schlechter als ein Signalereignis als Hintergrund klassifizieren. Vergleich von anderen Klassifizierern sollte das Diagramm ROC verwendet werden. Die Wahrscheinlichkeit, dass ein Ereignis Hintergrund akzeptiert, wie unter einer der folgenden Schwellenwerte Signal sein muss: 0,01, 0,02, 0,05, 0,1 oder 0,2.<p> </p>Beachten Sie, dass die Anzahl der Hintergrund Ereignisse (für hadronic Duschen h) unterschätzt wird, während in real-Maßeinheiten die Klasse h oder Rauschen die meisten Ereignisse darstellt. <p> </p><b>Verwandte Recherchieren:</b> Bock, R.K. (1995). UCI maschinellen Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California Schule von Informationen </td>
</tr>

<tr ID=weather-dataset>
  <td valign=top>Wetter Dataset</td>
  <td valign=top>
Stündlich Wetter Land-basierten Beobachtungen aus NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 to 201310">Seriendruckdaten aus 201304 zu 201310</a>).<p> </p>Die Wetterdaten behandelt Beobachtungen von Airport Wetterstationen, über den Zeitraum April-Oktober 2013 vorgenommen. Vor dem Hochladen auf Azure ML Studio, wurde das Dataset wie folgt verarbeitet:<ul><li>Wetter Station IDs wurden entsprechende Airport IDs zugeordnet.</li><li>Nicht zugeordnete 70 höchsten Auslastung Flughäfen Wetterstationen herausgefiltert wurden</li><li>Die Spalte "Datum" wurde in separaten Tag, Monat und Jahr Spalten aufgeteilt.</li><li>Die folgenden Spalten ausgewählt wurden: AirportID, Jahr, Monat, Tag, Uhrzeit, Zeitzone, SkyCondition, Sichtbarkeit, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, WindSpeed, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, Höhenmesser</li></ul>
  </td>
</tr>

<tr ID=wikipedia-sp-500-dataset>
  <td valign=top>Wikipedia SP 500 Dataset</td>
  <td valign=top>
Daten werden von Wikipedia (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) ausgehend von Artikeln jeder S & P 500 Firma, die als XML-Daten gespeichert wurden abgeleitet.<p> </p>Vor dem Hochladen auf Azure ML Studio, wurde das Dataset wie folgt verarbeitet:<ul><li>Extrahieren von Textinhalten für jeden Mandanten</li><li>Entfernen einer Wiki-Formatierung</li><li>Entfernen Sie nicht alphanumerische Zeichen</li><li>Konvertieren Sie aller Text in Kleinbuchstaben um</li><li>Bekannte Unternehmen Kategorien hinzugefügt wurden</li></ul><p> </p>Notiz, die für einige Unternehmen einen Artikel konnte nicht gefunden werden, damit die Anzahl der Datensätze kleiner als 500 ist.
  </td>
</tr>





<tr ID=direct-marketing>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td valign=top>
Das Dataset enthält Kundendaten und Hinweise auf ihre Antwort auf eine direkte Adressetiketten für eine Marketingkampagne. Jede Zeile steht für einen Kunden. Das Dataset enthält 9 Funktionen zu Benutzer Demographie und ältere Verhalten und 3 Etikett Spalten (besuchen, Konvertierung; und verbringen).  Besuchen wird eine binäre Spalte, die angibt, dass ein Kunde nach der Marketingkampagne besuchten, Konvertierung gibt an, ein Kunden etwas gekauft und aufwenden ist der Betrag, der aufgewendet wurde.  Das Dataset wurde von Kevin Hillstrom für MineThatData e-Mail-Analytics und Daten Mining Herausforderung zur Verfügung gestellt.
  </td>
</tr>

<tr ID=lyrl2004-tokens-test>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td valign=top>
Features von Test Beispiele im Dataset News RCV1-Version 2 Reuters. Das Dataset hat 781K Beiträgen sowie deren IDs (erste Spalte des Datasets). Jeder Artikel ist mit Token versehen, Stopworded, und ergaben. Das Dataset wurde von David zur Verfügung gestellt. D EINGETRAGEN. Lewis.
  </td>
</tr>

<tr ID=lyrl2004-tokens-train>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td valign=top>
Features von Schulung Beispiele im Dataset News RCV1-Version 2 Reuters. Das Dataset hat 23K Beiträgen sowie deren IDs (erste Spalte des Datasets). Jeder Artikel ist mit Token versehen, Stopworded, und ergaben. Das Dataset wurde von David zur Verfügung gestellt. D EINGETRAGEN. Lewis.
  </td>
</tr>

<tr ID=intrusion-detection>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td valign=top>
Ein Dataset aus dem KDD Cup 1999 Knowledge Discovery und Datamining-Tools induzierten Risikos (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>).<p> </p>Das Dataset heruntergeladen wurde in Azure Blob-Speicher (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) gespeichert und Schulung sowohl testen Datasets enthält. Schulung Dataset hat ungefähr 126K Zeilen und 43 Spalten, einschließlich der Etiketten. 3 Spalten Teil der Etiketteninformationen, und 40 Spalten aus der numerischen und Zeichenfolge/kategorisierten Features sind für das Modell Schulung zur Verfügung. Die Testdaten enthält ungefähr 22,5 K testen Beispiele mit den gleichen 43 Spalten wie die Schulungsdaten.

  </td>
</tr>

<tr ID=rcv1-v2-topics-qrels>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1-v2.topics.qrels.csv</a></td>
  <td valign=top>
Thema Zuordnungen nach News-Berichten im Dataset News RCV1-Version 2 Reuters. News-Artikel kann mehrere Themen zugeordnet werden. Das Format der einzelnen Zeilen ist "<topic name>  <document id> 1". Das Dataset enthält 2.6M Thema Zuordnungen. Das Dataset wurde von David zur Verfügung gestellt. D EINGETRAGEN. Lewis.
  </td>
</tr>

<tr ID=student-performance>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td valign=top>
Diese Daten stammen aus der KDD Cup 2010-Student Leistung Auswertung Herausforderung (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">Student Performance-Prüfung</a>). Die verwendeten Daten ist ein Satz Schulung Algebra_2008_2009 (Stamper, J., Niculescu-Mizil, a, Ritter, S., Gordon, G.J. und Koedinger, K.R. (2010). Algebra ich 2008-2009. Herausforderung Datenmenge aus KDD Cup 2010 im Zusammenhang mit Daten Mining Herausforderung. Besuchen sie <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a> oder <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>.<p> </p>Das Dataset heruntergeladen wurde in Azure Blob-Speicher (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) gespeichert und Protokolldateien aus einer Student Nachhilfe System enthält. Die angegebenen Features gehören Problem-ID und eine kurze Beschreibung, Studenten-ID, Zeitstempel und wie viele Versuche der Student vorgenommen werden, bevor Sie zur Lösung des Problems in der richtigen Umgang. Das ursprüngliche Dataset hat 8,9 M Einträge, dieses Dataset wurde unten auf die ersten 100 KB Zeilen aufgenommen. Das Dataset hat 23 Tabstopps getrennten Spalten verschiedener Typen: numerischen, kategorisierten, und Zeitstempel.

  </td>
</tr>




</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
