<properties
    pageTitle="Sammeln von Daten, um Ihr Modell Schulen: Computer Learning Empfehlungen API | Microsoft Azure"
    description="Learning Empfehlungen – sammeln Daten, um Ihr Modell Schulen Azure-Computern"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="luisca"/>

#  <a name="collecting-data-to-train-your-model"></a>Sammeln von Daten in Ihr Modell Schulen #

Die Empfehlungen-API lernfähig aus der vergangenen Transaktionen zu ermitteln, welche Elemente werden sollte auf einen bestimmten Benutzer empfohlen.

Nachdem Sie ein Modell erstellt haben, müssen Sie zwei bestimmte Informationen bereitstellen, bevor Sie eine Schulung ausführen können: einen Katalogdatei, und klicken Sie auf Verwendungsdaten.

>   Wenn Sie nicht bereits getan haben, empfehlen wir Ihnen die [Schnellstartübersicht](cognitive-services-recommendations-quick-start.md)ausführen.


## <a name="catalog-data"></a>Daten im Katalog ##

### <a name="catalog-file-format"></a>Katalog-Dateiformat ###

Die Katalogdatei enthält Informationen zu den Elementen, dass Sie an Ihren Kunden anbieten.
Die Daten im Katalog sollte das folgende Format folgen:

- Ohne Features-`<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- Mit Features-`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

#### <a name="sample-rows-in-a-catalog-file"></a>Beispiel für Zeilen in einem Katalogdatei

Ohne Features:

    AAA04294,Office Language Pack Online DwnLd,Office
    AAA04303,Minecraft Download Game,Games
    C9F00168,Kiruna Flip Cover,Accessories

Mit den Funktionen:

    AAA04294,Office Language Pack Online DwnLd,Office,, softwaretype=productivity, compatibility=Windows
    BAB04303,Minecraft DwnLd,Games, softwaretype=gaming,, compatibility=iOS, agegroup=all
    C9F00168,Kiruna Flip Cover,Accessories, compatibility=lumia,, hardwaretype=mobile

#### <a name="format-details"></a>Format-details

| Namen | Obligatorisch | Typ |  Beschreibung |
|:---|:---|:---|:---|
| Listenelement-Id |Ja | [A-Z], [a-Z], [0-9], [_] & #40; Unterstrich & #41; [-] & #40; Gedankenstrich & #41;<br> Die Maximallänge: 50 | Eindeutige ID eines Elements. |
| Elementnamen | Ja | Jedes beliebige alphanumerischen Zeichen<br> Die Maximallänge: 255 | Der Name des Elements. |
| Element-Kategorie | Ja | Jedes beliebige alphanumerischen Zeichen <br> Die Maximallänge: 255 | Kategorie, zu dem dieses Element (z. B. Cooking Bücher, Filmen...) gehört. kann leer sein. |
| Beschreibung | Nein, es sei denn, Features sind präsentieren (können aber leer sein) | Jedes beliebige alphanumerischen Zeichen <br> Die Maximallänge: 4000 | Beschreibung dieses Artikels. |
| Featureliste | Nein | Jedes beliebige alphanumerischen Zeichen <br> Die Maximallänge: 4000; Die maximale Anzahl von Features: 20 | Durch Trennzeichen getrennte Liste der Featurenamen = Wert Features, die zum Modell Empfehlungen verbessern verwendet werden kann.|

#### <a name="uploading-a-catalog-file"></a>Hochladen einer Katalogdatei

Prüfen Sie den [-API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e1) für das Hochladen einer Katalogdatei aus.  

Beachten Sie, dass der Inhalt der Katalogdatei als Hauptteil der Anforderung weitergegeben werden sollen.

Wenn Sie mehrere Katalogdateien in der gleichen Modell mit mehreren Anrufe hochladen, werden wir die neuen Katalogelemente einfügen. Vorhandene Elemente werden mit den ursprünglichen Werten bleiben. Daten im Katalog kann nicht mit dieser Methode aktualisiert werden.

>   Hinweis: Die maximale Dateigröße ist 200MB.
>   Die maximale Anzahl der Elemente im Katalog unterstützt ist 100.000 Elemente.


## <a name="why-add-features-to-the-catalog"></a>Gründe für das Hinzufügen von Features in den Katalog

Die Empfehlungen-Engine erstellt ein statistische Modell, das Sie entnehmen, welche Elemente werden als gut befunden oder von einem Kunden erworbenes Änderung möglich sind. Wenn Sie haben ist ein neues Produkt, mit der sie dem nie interagiert wurde weist, nicht möglich, ein Modell auf gemeinsame Vorkommen alleine zu erstellen. Angenommen, Sie beginnen, ein neues Angebot "Kinder Violine" in Ihrer Store, da Sie diese Violine nie verkauft haben, bevor Sie feststellen, welche anderen Elemente mit dieser Violine empfehlen können nicht.

Dies bedeutet, wenn das Modul für die Informationen zu diesen Violine bekannt ist (d. h. ist es eine musikalische Urkunde, es ist für untergeordnete fehlerbedingungen 7-10, es ist keine teure Violine usw.), und klicken Sie dann die-Engine aus anderen Produkten mit ähnlichen Features erfahren kann. Beispielsweise ist Sie noch Violine des in der Vergangenheit verkauft und in der Regel Personen, die Violins kaufen normalerweise CDs klassische Musik und Blatt Musik steht erwerben.  Das System kann diese Verbindungen zwischen den Funktionen finden und Vorschläge, wie basierend auf den Features während Ihrer neuen Violine etwas Verwendung hat.

Features werden als Teil der Daten im Katalog importiert, und klicken Sie dann ihre Rang (oder die Wichtigkeit des Features im Modell) zugeordnet wird, wenn ein Rang Build abgeschlossen ist. Feature Rang kann je nach der Muster Verwendungsdaten und Art der Elemente ändern. Allerdings für konsistente Verwendung/Elemente, der Rang sollten nur kleine Fluktuationen. Der Rang der Features ist eine nicht Negative Zahl an. Die Zahl 0 bedeutet, dass das Feature nicht eingestuft wurde (Wenn Sie diese API vor dem Abschluss der den ersten Rang Build Aufrufen geschieht). Das Datum, an dem der Rang zugeordnet wurde, heißt die Aktualität Punktzahl.


###<a name="features-are-categorical"></a>Features sind Categorical

Dies bedeutet, dass Sie Features erstellen sollten, die eine Kategorie ähneln. Beispielsweise Preis = 9.34 ist kein kategorisierten Feature. Andererseits, wie ein Feature PriceRange = Under5Dollars ist eine kategorisierte-Funktion. Ein weiterer gängiger Fehler besteht darin, den Namen des Elements als eine Funktion verwenden. In diesem Fall den Namen eines Elements eindeutig sein, damit sie eine Kategorie beschreiben möchten. Stellen Sie sicher, dass die Features Kategorien von Elementen darstellen.


###<a name="how-manywhich-features-should-i-use"></a>Verwenden Features wie m/welches sollte ich?


Schließlich erstellen Empfehlungen im Zusammenhang mit dem Erstellen eines Modells mit bis zu 20 Features unterstützt. Sie beispielsweise maximal 20 Features für die Elemente in Ihrem Katalog zuweisen, aber Sie führen Sie eine Rangfolge erstellen, und wählen Sie nur die Features, für die gilt hoher erwartet werden. (Ein Feature, mit dem Rang 2.0 oder mehrere ist eine gute Funktion verwenden!). 


###<a name="when-are-features-actually-used"></a>Wann sind Features tatsächlich werden verwendet?

Features werden durch das Modell verwendet, wenn es ist nicht genügend Transaktionsdaten Transaktionsinformationen allein Empfehlungen abgeben. Daher müssen Features die größte Auswirkung auf "kalt Elemente" – mit wenigen Transaktionen. Wenn alle Ihre Artikel ausreichende Transaktionsinformationen haben, müssen Sie nicht bereichern des Modells mit Features.


###<a name="using-product-features"></a>Verwenden die Produktfeatures

So verwenden Sie Features als Teil Ihrer erstellen, die Sie müssen:

1. Stellen Sie sicher, dass es sich bei Ihrem Katalog ausgestattet, wenn Sie hochladen.

2. Auslösen einer Rangfolge erstellen. Die Analyse auf der Wichtigkeit/Rang Features wird ausführen.

3. Auslösen einer Empfehlungen erstellen, Festlegen der Folgendes Parameter erstellen: Legen Sie UseFeaturesInModel auf WAHR, AllowColdItemPlacement auf "True" und der durch Trennzeichen getrennte Liste der Features, die Sie verwenden, um Ihr Modell verbessern möchten ModelingFeatureList festgelegt werden. Weitere Informationen finden Sie unter [Empfehlungen erstellen Typparameter](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d0) .





## <a name="usage-data"></a>Verwendungsdaten ##
Eine Verwendung-Datei enthält Informationen wie diese Elemente verwendet werden, oder die Transaktionen aus Ihrem Unternehmen.

#### <a name="usage-format-details"></a>Verwendungsdetails-Format
Eine Verwendung-Datei wird als CSV-Datei, (durch Kommas getrennten Werten), wobei jede Zeile in einer Datei Verwendung eine Interaktion zwischen einem Benutzer und ein Element darstellt. Jede Zeile wird wie folgt formatiert:<br>
`<User Id>,<Item Id>,<Time>,[<Event>]`



| Namen  | Obligatorisch | Typ | Beschreibung
|-------|------------|------|---------------
|Benutzer-Id|         Ja|[A-Z], [a-Z], [0-9], [_] & #40; Unterstrich & #41; [-] & #40; Gedankenstrich & #41;<br> Die Maximallänge: 255 |Eindeutiger Bezeichner eines Benutzers nachschlagen.
|Listenelement-Id|Ja|[A-Z], [a-Z], [0-9], [& #95;] & #40; Unterstrich & #41; [-] & #40; Gedankenstrich & #41;<br> Die Maximallänge: 50|Eindeutige ID eines Elements.
|Zeit|Ja|Datum im Format: JJJJ/MM/TTThh (z. B. 2013/06/20T10:00:00)|Zeitpunkt der Daten.
|Ereignis|Nein | Eine der folgenden Optionen:<br>• Klicken Sie auf<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Kauf| Die Art der Transaktion. |

#### <a name="sample-rows-in-a-usage-file"></a>Beispiel für Zeilen in einer Datei Verwendung

    00037FFEA61FCA16,288186200,2015/08/04T11:02:52,Purchase
    0003BFFDD4C2148C,297833400,2015/08/04T11:02:50,Purchase
    0003BFFDD4C2118D,297833300,2015/08/04T11:02:40,Purchase
    00030000D16C4237,297833300,2015/08/04T11:02:37,Purchase
    0003BFFDD4C20B63,297833400,2015/08/04T11:02:12,Purchase
    00037FFEC8567FB8,297833400,2015/08/04T11:02:04,Purchase

#### <a name="uploading-a-usage-file"></a>Hochladen einer Datei Verwendung

Schauen Sie sich die [-API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e2) zum Hochladen von Dateien Verwendung.
Beachten Sie, dass Sie den Inhalt der Datei Verwendung als Hauptteil der HTTP-Anruf übergeben müssen.

>  Hinweis:

>  Maximale Dateigröße: 200MB. Sie können mehrere Verwendung Dateien hochladen.

>  Sie müssen ein Katalogdatei hochladen, bevor Sie beginnen, Ihr Modell Verwendungsdaten hinzu. Nur die Elemente in der Katalogdatei werden der Phase Schulung verwendet werden. Alle anderen Elemente werden ignoriert.

## <a name="how-much-data-do-you-need"></a>Wie viele Daten benötigen Sie?

Die Qualität des Modells hängt stark von der Qualität und der Menge der Daten.
Das System lernt, wenn Benutzer verschiedene Elemente kaufen (Wir nennen diese gemeinsame vorkommen). Für FBT Build ist es auch wichtig zu wissen, welche Elemente in der gleichen Transaktionen erworben werden. 

Eine gute Faustregel ist, dass die meisten Elemente in mindestens 20 Transaktionen werden, damit Wenn Sie 10.000 Elemente in Ihrem Katalog hatten, wir empfehlen möchten aktuell mindestens 20 Mal diese Zahl oder Info 200.000 Transaktionen. Dies ist erneut eine Faustregel. Sie müssen so experimentieren Sie mit Ihren Daten.

Nachdem Sie ein Modell erstellt haben, können Sie zum Überprüfen, wie gut Ihr Modell wahrscheinlich ausführen ist [offline Auswertung](cognitive-services-recommendations-buildtypes.md) durchführen.
