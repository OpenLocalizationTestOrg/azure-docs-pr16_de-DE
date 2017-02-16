<properties
    pageTitle="Analysieren von Kunden profitieren mit maschinellen Learning | Microsoft Azure"
    description="Fallstudie der Entwicklung einer integrierten Modell für die Analyse und Bewertung deutlich verlängern"
    services="machine-learning"
    documentationCenter=""
    authors="jeannt"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="jeannt"/>

# <a name="analyzing-customer-churn-by-using-azure-machine-learning"></a>Analysieren von Kunden profitieren, mithilfe von Azure Computer-Schulung

##<a name="overview"></a>(Übersicht)
Dieses Thema bietet eine Verweis Implementierung eines Kunden Änderung Analysis-Projekts, die mithilfe von Azure maschinellen Learning Studio erstellt wurde. Es wird erläutert, zugeordneten generische Modelle für ganzheitlich zur Lösung des Problems der industrielle deutlich verlängern. Wir messen auch die Genauigkeit der Modelle, die mit maschinellen Learning erstellt wurden, und wir erfahren Sie, wie Weiterentwicklung bewerten.  

### <a name="acknowledgements"></a>Empfangsbestätigungen

Diese experimentieren wurde entwickelt und von Serge Berger, Tilgungsanteile Daten Scientist bei Microsoft und Roger Barga, früher Produktmanager für Microsoft Azure maschinellen Learning getestet. Das Dokumentationsteam Azure Glücklicherweise erkennt ihr Fachwissen an und danke ihnen dieses Whitepaper.

>[AZURE.NOTE] Die Daten für diese experimentieren ist nicht öffentlich verfügbar. Ein Beispiel für ein Modell für maschinelle lernen für die Analyse der Änderung zu erstellen, finden Sie unter: [Cortana Intelligence](http://gallery.cortanaintelligence.com/) Katalog [Telekommunikation profitieren Modell-Vorlage](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5)


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="the-problem-of-customer-churn"></a>Das Problem der deutlich verlängern
Unternehmen, die in den Consumer-Markt und in allen Enterprise-Bereichen müssen Änderung behandelt. Manchmal Änderung ist übermäßige und Richtlinie Entscheidungen beeinflusst. Die herkömmliche Lösung ist höchst-von vornherein Churners Vorhersagen und Adresse ihren Bedürfnissen über einen Concierge, Marketingkampagnen, oder indem spezielle Jagd-anwenden. Diese Ansätze können aus zu Industry und auch nicht aus einer bestimmten Consumer Cluster zu einem anderen innerhalb einer Branche (beispielsweise Telekommunikation) abweichen.

Im Allgemeinen besteht darin, dass Unternehmen in diesem Zusammenhang spezielle Kunden Aufbewahrung minimieren müssen. Auf diese Weise wäre natürlich Methoden jeder Kunden mit der Wahrscheinlichkeit von Änderung Punktzahl und die oberste N diejenigen Adresse. Top-Kunden möglicherweise wissen, welche diejenigen; Beispielsweise ist eine Funktion Profit in komplexere Szenarien, bei der Auswahl des Kandidaten für spezielle Befreiung beschäftigt. Diese Aspekte sind jedoch nur einen Teil der umfassende Strategie für den Umgang mit Änderung. Unternehmen müssen auch Konto Risiko (und zugehörigen Risiko Fehlertoleranz), die Ebene und die Kosten des Eingriff und plausibel Kunden Segmentierung tragen.  

##<a name="industry-outlook-and-approaches"></a>Industry Outlook und Vorgehensweisen
Ausgefeilte Handhabung von Änderung wird bei der Anmeldung von einem entwickelte Branche. Das klassische Beispiel ist die Telekommunikationsbranche, in dem Abonnenten bekannt ist, dass Sie häufig zwischen einem Anbieter zu wechseln. Diese Änderung freiwilligen ist Prime Bedeutung. Darüber hinaus haben Anbieter Gesamtzeit signifikante Kenntnisse über *Treiber profitieren*, die die Faktoren sind die Kunden Laufwerk zu wechseln.

Hörer oder Gerät Wahlmöglichkeiten ist beispielsweise einen bekannten Treiber der Änderung im Unternehmen auf dem Mobiltelefon. Daher ist eine beliebte Richtlinie zum Preis von einen Telefonhörer für neue Teilnehmer sowie die Erhebung von eines vollen Preis für vorhandene Kunden für ein Upgrade subventionieren ein. In der Vergangenheit hat dieser Richtlinie zu Kunden hopping aus einem Anbieter in ein anderes um einen neuen Rabatt, abzurufen, der wiederum Anbieter zu verfeinern Sie ihre Strategien aufgefordert hat geführt.

Erhöhte Volatilität in Telefonhörer Angebote ist ein Faktor, der sehr schnell Modelle der Änderung erklärt, die aktuellen Telefonhörer Modellen basieren. Darüber hinaus sind Mobiltelefone nicht nur zur Erstellung Geräte. Diese stehen auch Weise Anweisungen (iPhone berücksichtigen), und diese für soziale Netzwerke vorausgesagt außerhalb des Gültigkeitsbereichs von regulären Telekommunikation Datasets.

Das Ergebnis für die Modellierung ist, dass Sie eine sound Richtlinie entwickeln können nicht einfach durch beseitigen bekannte Gründe für die Änderung. Tatsächlich ist ein fortlaufender Modellierung Strategy, einschließlich klassischen Modelle, die Quantifizierung kategorisierte Variablen (z. B. Entscheidungsbäume) **erforderlich**.

Verwenden große Datenmengen auf ihre Kunden, sind Organisationen big Data Analytics (insbesondere Änderung Erkennung basierend auf große Daten) als das Problem auf effektive Weise durchführen. Sie können mehr über die big Data Lösung des Problems der Änderung in der Empfehlungen für den Abschnitt ETL suchen.  

##<a name="methodology-to-model-customer-churn"></a>Methoden zum Modell deutlich verlängern
Ein allgemeiner Problembehandlung Prozess deutlich verlängern gelöst wird in Zahlen 1 bis 3 dargestellt:  

1.  Ein Risikomodell können Sie berücksichtigen, wie Aktionen Wahrscheinlichkeit und Risiken auswirken.
2.  Ein Eingriff Modell können Sie berücksichtigen, wie die Ebene Eingriff die Wahrscheinlichkeit von Änderung und der Menge an Kunden auswirken kann die Gültigkeitsdauer (CLV).
3.  Diese Analyse eignet sich für eine Qualitative Analyse, die auf eine proaktive Marketingkampagne weitergeleitet wird, die Kunden in den Segmenten optimale Angebot Vorführung ausgerichtet.  

![][1]

Dieser Ansatz weiterleiten aussehenden ist die beste Methode zum Behandeln von Änderung, aber es im Lieferumfang von Komplexität: Wir haben ein Multi-Modell Urtyp und Spur Abhängigkeiten zwischen den Modellen entwickeln möchte. Die Interaktion zwischen Modelle kann gekapselt werden, wie in der folgenden Abbildung dargestellt:  

![][2]

*Abbildung 4: Unified Urtyp Multi-Modell*  

Interaktion zwischen den Modellen ist Schlüssel, wenn wir sind einen umfassende Ansatz Aufbewahrung Kunden vorführen. Jedes Modell beeinträchtigt periodisch über einen Zeitraum; Daher ist die Architektur einer implizit Schleife (ähnlich wie der Urtyp festlegen, indem Sie einem DM Daten Mining Standard, [***3***]).  

Der gesamte Risiko-Entscheidung-Marketing Segmentierung/Gliederung-Zyklus ist immer noch eine GRG-Struktur, die auf viele Business Probleme verfügbar ist. Änderung Analyse ist einfach ein sicheres Vertreter dieser Gruppe Probleme, da sie alle Merkmale eines Problems komplexe Business aufweist, die nicht über eine vereinfachte Vorhersage Lösung zulässt. Die sozialen Aspekten der moderne Ansatz Service werden nicht in der Ansatz besonders hervorgehoben, aber die sozialen Aspekten sind in der Urtyp Modellierung eingeschlossen, wie sie in keinem Modell.  

Hier eine interessante Erweiterung ist big Data Analytics. Heutigen Telekommunikation und Retail Unternehmen sammeln umfassende Daten zu ihren Kunden, und wir können einfach vorgesehen ist, dass die Notwendigkeit der Multi-Modell Connectivity einen allgemeinen Trend, angegebenen entstehenden Trends wie Internet der Dinge und universell Geräte, die für Unternehmen smart Lösungen auf mehreren Ebenen wendet gewähren verwendet werden soll.  

 
##<a name="implementing-the-modeling-archetype-in-machine-learning-studio"></a>Implementieren der Modellierung Urtyp in Computer Learning Studio
Angegebenen das Problem nur beschrieben, was ist die beste Methode zum Implementieren eines integrierten Modellierung und bewerten Ansatz? In diesem Abschnitt werden wir veranschaulichen, wie wir dies mithilfe von Azure maschinellen Learning Studio erreicht.  

Der Multi-Modell Ansatz ist, muss beim Entwerfen einer globalen Urtyp für Änderung. Auch der Punktzahl (Vorhersage) Teil der Ansatz sollten Multi-Modell aus.  

Das folgende Diagramm veranschaulicht den Prototypen, dass wir erstellt haben, die vier Punktzahl Algorithmen in Computer Learning Studio Änderung Vorhersagen beschäftigt. Der Grund für die Verwendung eines Multi-Modell Ansatzes ist nicht nur für das Erstellen einer Klassifizierung Ensemble Genauigkeit, erhöhen aber auch zum Schutz vor Blinde Formstück und Nachschlagewerke Featureauswahl zu verbessern.  

![][3]

*Abbildung 5: Prototypen von einer Änderung modeling Ansatz*  

Die folgenden Abschnitte enthalten weitere Details zu den Prototypen, die wir implementiert mithilfe von maschinellen Learning Studio das Modell wird bewertet.  

###<a name="data-selection-and-preparation"></a>Datenauswahl und Vorbereitung
Die Daten verwendet, um die Modelle erstellen und Punktzahl Kunden aus einer vertikalen CRM-Lösung, mit den Daten, die zum Schützen von Kundendaten verborgen abgerufen wurde. Die Daten enthält Informationen in den USA 8.000 Abonnements, und es drei Quellen kombiniert: Daten (Abonnement Metadaten), Daten zu den Aktivitäten (Verwendung des Systems) und Support Kundendaten bereitgestellt. Die Daten nicht enthält alle Business verbundene Informationen zu den Kunden; Angenommen, enthält sie Loyalität Metadaten oder Kreditkarte Bewertungen der Lesbarkeit nicht.  

Zur Vereinfachung ETL und-Bereinigung Prozesse Daten werden außerhalb des Bereichs, da davon ausgegangen, dass die Daten zur Vorbereitung bereits weist an anderer Stelle durchgeführt wurde.   

Featureauswahl für Modellierung basiert auf vorläufigen Genauigkeit bewerten über die Sammlung vorausgesagt, die im Lieferumfang des Prozess, der das Modul zufällige Gesamtstruktur verwendet. Für die Durchführung in Computer Learning Studio werden die Mittel, Median und Ausgabebereich für Vertreter Features berechnete. Beispielsweise haben wir Aggregate für die qualitativen Daten, wie z. B. Mindest- und Höchstwerte für Benutzeraktivitäten hinzugefügt.    

Wir erfasst auch zeitliche Informationen für den letzten sechs Monaten. Wir analysiert Daten für ein Jahr, und wir hergestellt, auch wenn es statistisch signifikante Trends, die Auswirkung auf die Änderung erheblich nach sechs Monaten beeinträchtigt wird.  

Der wichtigste Punkt ist, dass der gesamte Prozess, einschließlich ETL, Featureauswahl, und modeling in Computer Learning Studio mithilfe von Datenquellen in Microsoft Azure implementiert wurde.   

Die folgenden Diagramme veranschaulichen die Daten, die verwendet wurde.  

![][4]

*Abbildung 6: Ausschnitt Datenquelle (verborgen)*  

![][5]


*Abbildung 7: Features extrahiert aus einer Datenquelle*
 
> Beachten Sie, dass diese Daten privat ist und können daher das Modell und die Daten nicht freigegeben werden.
> Jedoch eine ähnliche Modell öffentlich zugängliche Daten verwenden, finden Sie in diesem Beispiel die [Cortana Intelligence Katalog](http://gallery.cortanaintelligence.com/)experimentieren: [Telekommunikation Kunden profitieren](http://gallery.cortanaintelligence.com/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Wenn Sie weitere Informationen dazu, wie Sie eine Änderung Analysis-Modell mit Cortana Intelligence Suite implementieren können, empfiehlt es sich [dieses Video an](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) , indem Sie Programm Gruppenleiter Wee Hyong Tok. 
> 

###<a name="algorithms-used-in-the-prototype"></a>Algorithmen, die in der Prototypen verwendet werden.

Wir verwendet die folgenden vier Computer Learning Algorithmen, um die Prototypen (keine Anpassung) zu erstellen:  

1.  Logistische Regression (LR)
2.  Stärkere Entscheidungsstruktur (BT)
3.  Die durchschnittliche Perceptron (AP)
4.  Support Vektor Computer (SVM)  


Das folgende Diagramm veranschaulicht, einen Teil der Entwurfsoberfläche experimentieren, die die Reihenfolge angeben soll, in der die Modelle erstellt wurden:  

![][6]  


*Abbildung 8: Erstellen von Datenmodellen in Computer Learning Studio*  

###<a name="scoring-methods"></a>Punktzahl Methoden
Wir haben vier Modelle mithilfe eines Datasets beschriftete Schulung bewertet.  

Wir übermittelten auch Punktzahl Dataset zu einem vergleichbaren Modell erstellt, indem Sie die Desktopversion von SAS Enterprise Miner-Funktion 12. Wir gemessen die Genauigkeit des Modells SAS und alle vier Computer Learning Studio-Modelle.  

##<a name="results"></a>Ergebnisse
In diesem Abschnitt stellen wir unsere Ergebnisse über die Genauigkeit der Modelle, basierend auf der Punktzahl Dataset aus.  

###<a name="accuracy-and-precision-of-scoring"></a>Genauigkeit und Präzision der bewerten
Im Allgemeinen ist die Implementierung Azure Computer interessante hinter SAS in Genauigkeit um etwa 10 bis 15 % (Bereich unter Kurve oder AUC) ein.  

Die wichtigste Metrik in Änderung ist jedoch die Einreihungsfehler Rate: d. h., der die obersten N Churners als von der Klassifizierung geschätzten, welche hiervon tatsächlich hat **keine** Änderung, und besondere Behandlung noch erhalten? Das folgende Diagramm vergleicht diese Einreihungsfehler Abschlag (Disagio) alle Modelle:  

![][7]


*Abbildung 9: Passau Prototypen Bereich unter der Kurve*

###<a name="using-auc-to-compare-results"></a>Verwenden von AUC Ergebnisse vergleichen
Bereich unter Kurve (AUC) ist eine Metrik, die ein globaler Maß *Trennbarkeit* zwischen der Verteilung der Ergebnisse für positive und negative Population darstellt. Ähnelt dem herkömmlichen Empfänger Operator Merkmale (ROC) Graph an, aber ein wichtiger Unterschied besteht darin, dass Sie ein gegebener auswählen die AUC Metrik nicht erforderlich ist. In diesem Fall sind die Ergebnisse über **Alle** möglichen Ausdrücke aufgeführt. Im Gegensatz dazu das herkömmliche ROC Diagramm zeigt die positive Rate auf der vertikalen Achse und die falsch-positive Quote auf der horizontalen Achse, und der oberen Schwellenwert der Klassifizierung variiert.   

AUC im Allgemeinen als Maßeinheit für im Wert für unterschiedliche Algorithmen (oder andere Betriebssysteme) verwendet, da dies zulässt, dass Modelle über deren Werte AUC verglichen werden soll. Dies ist eine beliebte Ansatz in Branchen wie METEOROLOGIE und Biosciences. Auf diese Weise darstellt AUC ein beliebten Tools für die Bewertung der Leistung Klassifizierung.  

###<a name="comparing-misclassification-rates"></a>Vergleich der Unterschiede bei Einreihungsfehler Sätzen
Wir im Vergleich der Einreihungsfehler Sätzen für das betreffende Dataset mithilfe der CRM-Daten von ungefähr 8.000 Abonnements zu.  

-   Die SAS Einreihungsfehler Rate wurde 10 bis 15 %.
-   Die maschinelle Learning Studio Einreihungsfehler Rate wurde 15-20 % für die oben 200-300 Churners.  

In der Telekommunikation Industrie ist es wichtig, behoben werden nur die Kunden, die das höchste Risiko zu profitieren, indem sie eine Concierge-Dienst oder andere spezielle Behandlung anbieten haben. In diesem Zusammenhang wird die Computer Learning Studio Implementierung Ergebnisse zurückliegen SAS-Modell erreicht.  

Aus den gleichen Gründen, ist die Genauigkeit wichtiger als Genauigkeit, da wir hauptsächlich ordnungsgemäß Klassifizierung von potenziellen Churners interessiert sind.  

Das folgende Diagramm aus Wikipedia wird die Beziehung in einer Grafik lebendiger, leicht verständliche dargestellt:  

![][8]

*Abbildung 10: Abfrageeffizienz Genauigkeit und Präzision*

###<a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Genauigkeit und Präzision Ergebnisse für stärkere Entscheidungsbaummodell  

Das folgende Diagramm zeigt die Ergebnisse unformatierten bewerten mithilfe der maschinellen Learning Prototypen für das stärkere Entscheidung-Struktur-Modell, das passiert am genauesten zwischen vier Modelle werden:  

![][9]

*Abbildung 11: Stärkere Entscheidung Struktur Modell Merkmale*

##<a name="performance-comparison"></a>Leistungsvergleich
Wir im Vergleich der Geschwindigkeit, dessen Daten mithilfe der maschinellen Learning Studio-Modelle und eines vergleichbaren Modells bewertet wurde mithilfe der Desktopversion von SAS Enterprise Miner-Funktion 12.1 erstellt.  

Die folgende Tabelle enthält eine Übersicht über die Leistung der Algorithmen:  

*Tabelle 1. Allgemeine Leistung (Genauigkeit) der Algorithmen*

| LR|BT|AP|SVM|
|---|---|---|---|
|Durchschnittliche Modell|Die beste Modell|Volle Leistung|Durchschnittliche Modell|

Die Modelle in einem Computer Learning Studio übertroffen SAS mit 15-25 % für Geschwindigkeit Execution, aber Genauigkeit weitgehend auf Nennwert wurde gehostet wird.  

##<a name="discussion-and-recommendations"></a>Diskussion und Empfehlungen
In der Telekommunikationsbranche mehrere Methoden entwickelt haben, zum Analysieren von Änderung, einschließlich:  

-   Leiten Sie Kennzahlen für vier grundlegende Kategorien:
    -   **Entität (beispielsweise ein Abonnement)**. Bereitstellung grundlegenden Informationen zu den Abonnements und/oder Kunden, dass der Betreff der Änderung an.
    -   **Aktivität**. Beziehen Sie alle möglichen Verwendungsinformationen, die mit der Entität, beispielsweise die Anzahl der Benutzernamen verknüpft ist.
    -   **Unterstützen von Kunden**. Sammeln Sie Informationen aus Kunden Support-Protokolle, um anzugeben, ob das Abonnement Probleme oder Interaktionen mit Kundensupport hatte.
    -   **Mitbewerber und geschäftliche Daten**. Erhalten Sie Informationen zu den Kunden möglich (beispielsweise kann nicht verfügbar oder schwer zu verfolgen).
-   Verwenden Sie die Wichtigkeit zu Laufwerk Featureauswahl. Dies bedeutet, dass die stärkere Entscheidungsbaummodell immer eine Versprechen Ansatz ist.  

Die Verwendung der folgenden vier Kategorien erstellt den Eindruck, den ein einfache *deterministisch* Modell, basierend auf Indizes auf angemessenen Faktoren pro Kategorie gebildeten ausreichend sollten, um Kunden Risiko einer Änderung zu identifizieren. Obwohl dieses Konzept plausibel scheint, ist es leider zu verstehen und einen falsch. Der Grund ist, dass die Änderung ist ein zeitlicher Effekt und die Faktoren, um profitieren werden in der Regel in vorübergehende Zustände. Was einen Kunden zu berücksichtigen ist heute verlassen leads möglicherweise anders morgen, und es natürlich, unterscheiden sich sechs Monate ab jetzt. Daher eine *probabilistisch* Modell ist eine Notwendigkeit.  

Diese wichtige Beobachtung wird im Unternehmen, die in der Regel einen Business Intelligence-orientierte Ansatz zum Analytics lieber häufig übersehen, hauptsächlich, weil es ist ein einfacher verkaufen und recht einfach Automatisierung zulässt.  

Das Versprechen von Self-service Analytics mithilfe von maschinellen Learning Studio ist jedoch, dass die vier Kategorien von Informationen, sortiert nach Gruppe oder Abteilung, eine wertvolle Quelle für maschinelle Kennenlernen der Änderung werden.  

Ist eine weitere interessante Möglichkeit in Kürze Azure Computer interessante ist die Möglichkeit, ein benutzerdefiniertes Modul mit einem Repository eines vordefinierten Module hinzuzufügen, die bereits verfügbar sind. Diese Funktion, wird im Wesentlichen eine Verkaufschance zum Auswählen von Bibliotheken, und erstellen Sie Vorlagen für vertikalen Märkten erstellt. Es ist ein wichtiges Abgrenzungsmerkmal Azure Computer lernen im Markt.  

Wir hoffen, dass dieses Thema in der Zukunft fortsetzen besonders im Zusammenhang mit big Data Analytics.
  
##<a name="conclusion"></a>Abschluss
Dieses Dokument beschreibt einen sinnvollen Ansatz zur Bekämpfung des häufig auftretendes Problems der deutlich verlängern mithilfe eines generischen Frameworks. Wir Prototypen zur Bewertung von Datenmodellen angesehen, und es mithilfe von Azure maschinellen Learning implementiert werden. Schließlich bewertet wir die Genauigkeit und Leistung der Lösung im Hinblick auf vergleichbaren Algorithmen in SAS Prototypen.  

**Weitere Informationen:**  

In diesem Artikel werden? Geben Sie uns bitte Ihr Feedback. Teilen uns auf einer Skala von 1 (schlecht) bis 5 (hervorragend), wie bewerten Sie dieses Dokument und warum haben Sie ihm diese Bewertung? Beispiel:  

-   Sind Sie es nicht durch gute Beispiele, hervorragende Screenshots deaktivieren schreiben, oder ein weiterer Grund hohe Bewertung?
-   Sind Sie es aufgrund von Beispielen beeinträchtigt, fuzzy Screenshots oder missverständliche Writing niedrig Bewertung?  

Dieses Feedback hilft uns, die Qualität des Whitepapers zu verbessern, die wir lassen Sie wieder los.   

[Feedback senden](mailto:sqlfback@microsoft.com).
 
##<a name="references"></a>Verweise
Vorhersageanalytik [1] : jenseits der Vorhersagen, West McKnight, Informationsmanagement Juli/August 2011, p.18-20.  

[2] Wikipedia-Artikel: [Genauigkeit und Präzision](http://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [gewährleisten-DM 1.0: schrittweise Datamining Guide](http://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [big Data Marketing: lassen Sie Ihre Kunden effizienter und versorgen Wert](http://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telekommunikation profitieren Modell Vorlage](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) [Cortana Intelligence](http://gallery.cortanaintelligence.com/) -Katalog 
 
##<a name="appendix"></a>Anlage

![][10]

*Abbildung 12: Momentaufnahme einer Präsentation auf Änderung Prototypen*


[1]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-1.png
[2]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-2.png
[3]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-3.png
[4]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-4.png
[5]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-5.png
[6]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-6.png
[7]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-7.png
[8]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-8.png
[9]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-9.png
[10]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-10.png
