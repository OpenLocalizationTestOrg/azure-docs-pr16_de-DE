<properties 
    pageTitle="Anfordern von Einheiten in DocumentDB | Microsoft Azure" 
    description="Informationen Sie zu verstehen, angeben und schätzen Anforderung Einheit Anforderungen in DocumentDB verwendet." 
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="mimig" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="syamk"/>

#<a name="request-units-in-documentdb"></a>Anfordern von Einheiten in DocumentDB
Jetzt erhältlich: DocumentDB [Anforderung Einheit Taschenrechner](https://www.documentdb.com/capacityplanner). Weitere Informationen finden Sie in [Ihrer Durchsatz muss Estimating](documentdb-request-units.md#estimating-throughput-needs).

![Durchsatz Taschenrechner][5]

##<a name="introduction"></a>Einführung
Dieser Artikel enthält eine Übersicht der Anfrage Einheiten in [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). 

Nach dem Lesen dieses Artikels, können Sie die folgenden Fragen beantworten ausführen:  

-   Was sind Einheiten fordert und Gebühren?
-   Wie gebe ich Anforderung Einheit Kapazität für eine Websitesammlung an?
-   Wie schätzen ich, dass der Anwendung Anforderung Gerät werden muss?
-   Was passiert, wenn ich die Anforderung Einheit Kapazität für eine Websitesammlung überschreiten?


##<a name="request-units-and-request-charges"></a>Einheiten fordert und Gebühren
DocumentDB bietet eine schnelle und vorhersehbar Leistung von Ressourcen *reserviert werden* , dass der Anwendung Durchsatz muss erfüllen.  Da Anwendung laden und Mustern Änderung im Zeitverlauf zugreifen, kann DocumentDB Sie einfach vergrößern oder verkleinern die Menge des reservierte Durchsatz an Ihrer Anwendung zur Verfügung.

Mit DocumentDB reservierte Durchsatz in Bezug auf die Anfrage Einheiten pro Sekunde Verarbeitung angegeben.  Sie können der Anforderung Einheiten als Währung Durchsatz, vorstellen vererbungseinstellungen Anforderung Einheiten verfügbar an Ihrer Anwendung auf Basis pro Sekunde Sie ein Menge *Reservieren garantiert* .  Jede Operation in DocumentDB - Ausgeben eines Dokuments, eine Abfrage ausführen, Aktualisieren eines Dokuments - verbraucht CPU, Arbeitsspeicher und IOPS.  D. h., budgetgerecht jedem Vorgang *Anforderung aufzulisten*, die in *einer anderen Einheit Anforderung*ausgedrückt wird.  Grundlegendes zu den Faktoren, die Anforderung Einheit Gebühren, zusammen mit Ihrer Anwendung Durchsatz Anforderungen, auswirken, können Sie Ihre Anwendung Kosten effektiv wie möglich ausgeführt. 

##<a name="specifying-request-unit-capacity"></a>Angabe Anforderung Einheit Kapazität
Beim Erstellen einer Websitesammlungs DocumentDB Geben Sie die Anzahl der Anfrage Einheiten pro Sekunde (RUs) für die Websitesammlung reserviert werden soll.  Nachdem die Sammlung erstellt wurde, ist die vollständige Verteilung der angegebenen RUs für die Verwendung der Auflistung reserviert.  Jede Sammlung ist immer dedizierte und Durchsatz Merkmale isoliert haben.  

Es ist wichtig zu beachten, dass DocumentDB für ein Modell Reservierung ausgeführt wird. Sie sind d. h., Abrechnung für den Durchsatz *reservierte* für die Websitesammlung, unabhängig davon, wie viele dieser Durchsatz aktiv *verwendet*wird.  Bedenken Sie, jedoch, die als Ihrer Anwendungs laden, Daten und Verwendung Muster ändern, können Sie problemlos nach oben und unten die Menge des skalieren, reserviert RUs bis DocumentDB SDKs oder mithilfe der [Azure-Portal](https://portal.azure.com).  Weitere Informationen an Skala Durchsatz nach oben oder unten finden Sie unter [DocumentDB Leistung Ebenen](documentdb-performance-levels.md).

##<a name="request-unit-considerations"></a>Anfordern der Maßeinheit Aspekte
Wenn Sie die Anzahl der Anfrage Einheiten für Ihre Websitesammlung DocumentDB reservieren schätzen, ist es wichtig, die folgenden Variablen berücksichtigen:

- **Dokumentgröße**. Wenn Dokumentgrößen die Einheiten verbraucht zum Lesen und schreiben, dass die Daten auch vergrößert werden erhöhen.
- **Anzahl der Dokument-Eigenschaft**. Unter Annahme Standard Indizierung aller Eigenschaften, die Einheiten, die zum Schreiben eines Dokuments verbraucht erhöht zunehmender Anzahl Eigenschaft.
- **Konsistenz der Daten**. Bei Verwendung von Daten Konsistenz Ebenen starken oder Veraltung begrenzt werden zusätzliche Einheiten verbraucht werden, um Dokumente zu lesen.
- **Indizierte Eigenschaften**. Eine Richtlinie Index für jede Auflistung bestimmt, welche Eigenschaften standardmäßig indiziert sind. Sie können Ihre Anforderung Einheitenverbrauch durch Einschränken der Anzahl der indizierten Eigenschaften oder durch Aktivieren der verzögerte Indizierung reduzieren.
- **Dokument mit einem Index**. Standardmäßig jedes Dokument automatisch indiziert ist, werden Sie weniger Anforderung Einheiten nutzen, wenn Sie nicht indizieren einige Ihrer Dokumente.
- **Abfrage Mustern**. Die Komplexität der Abfrage wirkt sich auf, wie viele anfordern Einheiten für einen Vorgang verbraucht werden. Die Anzahl der Prädikate, Art der Prädikate, Projektionen, Anzahl der UDFs und die Größe der Datenmenge Quelle alle beeinflussen die Kosten von Vorgängen Abfrage ein.
- **Skript-Verwendung**.  Nutzen wie bei Abfragen, gespeicherten Prozeduren und Triggern Anforderung Einheiten ausgehend von der Komplexität der Vorgänge durchgeführt werden. Bei der Entwicklung Ihrer Anwendung, prüfen Sie die Anfrage Gebühr Kopfzeile zum besseren Verständnis wie jede Operation Anforderung Einheit Kapazität in Anspruch nimmt.

##<a name="estimating-throughput-needs"></a>Schätzen Durchsatz Anforderungen
Eine Anforderung Einheit ist ein standardisierten Maß für die Verarbeitung von Kosten Anforderung an. Eine einzelne Anforderung Einheit darstellt, die Kapazität erforderlich, um ein einzelnes 1 KB JSON-Dokument 10 eindeutige Eigenschaftswerte (ausgenommen Systemeigenschaften) aus (über Self Link oder -Id) zu lesen. Eine Anforderung an (Einfügen) erstellen, ersetzen oder Löschen von im selben Dokument werden weitere Verarbeitung vom Dienst und dadurch auch weitere Anforderung Einheiten nutzen.   

> [AZURE.NOTE] Der Basisplan 1 Anforderung Einheit für ein Dokument 1KB entspricht einer einfachen Abrufen von Self Link oder Id des Dokuments.

###<a name="use-the-request-unit-calculator"></a>Verwenden der Anfrage Einheit Taschenrechner
Helfen Kunden fein optimieren, deren Durchsatz Abschätzung, gibt es eine Web-basierte [Anfrage Einheit Taschenrechner](https://www.documentdb.com/capacityplanner) helfen schätzen die Anfrage Einheit Anforderungen für Standardvorgänge, einschließlich:

- Dokument erstellt (Schreiben)
- Dokument liest
- Löscht Dokument
- Dokument-updates

Das Tool enthält auch Unterstützung für-Schätzung Daten Speicher Anforderungen basierend auf der Stichprobe Dokumente, die Sie bereitstellen.

Verwenden das Tool ist ganz einfach:

1. Hochladen eines oder mehrere Vertreter JSON-Dokumente.

    ![Hochladen von Dokumenten in der Anfrage Einheit Taschenrechner][2]

2. Geben Sie zum Schätzen der Speicherplatz für Daten, die Gesamtzahl der Dokumente, die Sie nicht vorhaben, speichern.

3. Geben Sie die Nummer des Dokuments erstellen, lesen, aktualisieren und Löschen von Vorgängen, die Sie (auf der Basis einer pro Sekunde benötigen). Hochladen von zum Schätzen der Anfrage Einheit Gebühren des Dokuments Update-Vorgänge, eine Kopie des Beispieldokuments aus Schritt 1 oben, die Updates typische Feld enthält.  Angenommen, wenn Updates Dokument normalerweise zwei Eigenschaften namens LastLogin und UserVisits ändern möchten, klicken Sie dann einfach kopieren Sie das Dokument, aktualisieren Sie die Werte für diese beiden Eigenschaften und Hochladen Sie das kopierte Dokument.

    ![Geben Sie im Einheit Besprechungsanfrage Rechner Durchsatz Anforderungen][3]

4. Klicken Sie auf berechnen und prüfen Sie die Ergebnisse.

    ![Anfordern der Maßeinheit Taschenrechner Ergebnisse][4]

>[AZURE.NOTE]Wenn Sie Dokumenttypen die erheblich im Hinblick auf Größe und die Anzahl der indizierten Eigenschaften variieren wird haben, laden Sie ein Beispiel für jeden *Typ* des typische Dokuments in das Tool, und klicken Sie dann berechnen Sie die Ergebnisse zu.

###<a name="use-the-documentdb-request-charge-response-header"></a>Verwenden der DocumentDB Anforderung Gebühr Antwort-header
Jeder Antwort vom Dienst DocumentDB enthält einen benutzerdefinierten Header (X-ms-Anforderung-Gebühr), der die Anforderung Einheiten für die Anforderung verbraucht enthält. Diese Kopfzeile ist auch über die DocumentDB SDKs zugegriffen werden kann. In .NET SDK ist RequestCharge eine Eigenschaft des Objekts ResourceResponse.  Für Abfragen stellt der DocumentDB Abfrage Explorer Azure-Portal Anforderung Gebühr für ausgeführten Abfragen.

![Untersuchen RU Gebühren im Abfrage-Explorer][1]

Mit diesem in Denken Sie daran, eine Methode zum schätzen, dass die Menge des reservierte Durchsatz von Ihrer Anwendung benötigten Aufzeichnen der Anfrage Einheit Gebühr Ausführung des Standardvorgänge für ein Vertreter Dokument, das von der Anwendung verwendeten zugeordnet ist, und klicken Sie dann die Anzahl der Vorgänge-Schätzung erwarten Sie pro Sekunde durchführen.  Achten Sie darauf, dass messen und typische Abfragen und DocumentDB Skript Verwendung ebenfalls enthalten.

>[AZURE.NOTE]Wenn Sie über die im Hinblick auf Größe und die Anzahl der indizierten Eigenschaften erheblich abweichen wird Dokumenttypen verfügen, nehmen Sie dann den jeweiligen Vorgang Anforderung Einheit Gebühr jeder *Typ* des normalen Dokument zugeordnet.

Beispiel:

1. Aufzeichnen der Anfrage Einheit Gebühr erstellen (Einfügen) eine typische Dokument. 
2. Die Anforderung Einheit Gebühr Lesen eines Dokuments Standard-Eintrag.
3. Aufzeichnen der Anfrage Einheit Gebühr Aktualisieren eines typischen Dokuments.
3. Aufzeichnen der Anfrage Einheit Gebühr der normalen, ein gängiges Dokument Abfragen an.
4. Aufzeichnen der Anfrage Einheit Belastung benutzerdefinierte Skripts (gespeicherten Prozeduren, Trigger, benutzerdefinierte Funktionen) durch die Anwendung genutzt
5. Berechnen der erforderlichen Anforderung Einheiten die geschätzte Anzahl von Vorgängen, die Sie zum Ausführen von pro Sekunde rechnen.

##<a name="a-request-unit-estimation-example"></a>Ein Beispiel für die Anfrage Einheit Schätzung
Beachten Sie das folgenden ~ 1KB-Dokument aus:

    {
     "id": "08259",
    "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
    "tags": [
        {
        "name": "cereals ready-to-eat"
        },
        {
        "name": "kellogg"
        },
        {
        "name": "kellogg's crispix"
        }
    ],
    "version": 1,
    "commonName": "Includes USDA Commodity B855",
    "manufacturerName": "Kellogg, Co.",
    "isFromSurvey": false,
    "foodGroup": "Breakfast Cereals",
    "nutrients": [
        {
        "id": "262",
        "description": "Caffeine",
        "nutritionValue": 0,
        "units": "mg"
        },
        {
        "id": "307",
        "description": "Sodium, Na",
        "nutritionValue": 611,
        "units": "mg"
        },
        {
        "id": "309",
        "description": "Zinc, Zn",
        "nutritionValue": 5.2,
        "units": "mg"
        }
    ],
    "servings": [
        {
        "amount": 1,
        "description": "cup (1 NLEA serving)",
        "weightInGrams": 29
        }
    ]
    }

>[AZURE.NOTE]Dokumente werden in DocumentDB, verkleinerte, damit das System berechnet, dass die Größe des Dokuments oben etwas weniger als 1KB ist.


Die folgende Tabelle zeigt die ungefähren Anforderung Einheit Gebühren für Standardvorgänge für dieses Dokument (die ungefähren Anforderung Einheit Gebühr wird davon ausgegangen, dass die Konto Konsistenz Ebene "Sitzung" festgelegt ist und dass alle Dokumente automatisch indiziert werden):

Vorgang|Einheit Gebühr anfordern 
---|---
Dokument erstellen|~ 15 RU 
Dokument lesen|~ 1 RU
Abfragedokument-ID|~2.5 RU

Darüber hinaus zeigt dieser Tabelle ungefähren Anforderung Einheit Gebühren für typische Abfragen, die in der Anwendung verwendet werden:

Abfrage|Einheit Gebühr anfordern|# Der zurückgegebene Dokumente
---|---|--- 
Wählen Sie Lebensmittel nach id|~2.5 RU|1 
Wählen Sie Nahrungsmittel vom Hersteller|~ 7 RU|7
Wählen Sie nach Lebensmittelgruppe und Reihenfolge nach Gewicht|~ 70 RU|100
Wählen Sie obersten 10 Nahrungsmittel in einer Lebensmittelgruppe aus.|~ 10 RU|10

>[AZURE.NOTE]RU Gebühren variieren basierend auf die Anzahl der Dokumente, die zurückgegeben werden.

Mithilfe dieser Informationen können wir die RU Anforderungen für diese Anwendung, die die Anzahl der Vorgänge und Abfragen, dass wir pro Sekunde erwarten angegebenen schätzen:

Abfrage-Vorgang|Geschätzte Anzahl pro Sekunde|Erforderliche RUs 
---|---|--- 
Dokument erstellen|10|150 
Dokument lesen|100|100 
Wählen Sie Nahrungsmittel vom Hersteller|25|175 
Wählen Sie nach Lebensmittel-Gruppen|10|700 
Wählen Sie Top 10 aus.|15|150 gesamt|155|1275

In diesem Fall erwarten wir eine Vorbedingung Durchschnittsdurchsatz 1,275 RU/s.  Runden auf die nächste 100 wir 1.300 RU/s für diese Anwendung Websitesammlung bereitstellen möchten.

##<a name="a-idrequestratetoolargea-exceeding-reserved-throughput-limits"></a><a id="RequestRateTooLarge"></a>Von mehr als reservierte Durchsatz Beschränkungen
Denken Sie daran, dass die Anforderung Einheitenverbrauch als ein Wert pro Sekunde ausgewertet wird. Für Applikationen, die die Anforderung bereitgestellte Einheit Abschlag (Disagio) eine Auflistung überschreiten, werden Anfragen dieser Sammlung gedrosselt werden, bis die Rate unterhalb der reservierte Ebene fällt. Tritt eine Einschränkung, der Server präventiv die Anfrage mit RequestRateTooLargeException (HTTP-Statuscode 429) zu beenden und die X-ms-"Wiederholen"-nach-ms Kopfzeile, der angibt, der Zeitdauer in Millisekunden an, die der Benutzer warten muss, vor dem erneuten der Anfrage zurückzukehren.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Wenn Sie die .NET Client SDK und LINQ Abfragen, und klicken Sie dann auf in den meisten Fällen haben Sie nie behandelt diese Ausnahme, während die aktuelle Version von .NET Client SDK implizit diese Antwort abgefangen verwenden, werden berücksichtigt die Kopfzeile Server festgelegten "Wiederholen" nach, und die Anfrage wiederholt. Es sei denn, Ihr Konto gleichzeitig von mehreren Clients zugegriffen wird, ist die nächste Wiederholung erfolgreich.

Wenn Sie mehr als einen Client hoch haben Betrieb über die Anforderung Zins; das Standardverhalten der "Wiederholen" möglicherweise nicht ausreichend und der Client löst eine DocumentClientException mit Statuscode 429 zur Anwendung. In diesen Fällen können Sie den Umgang mit "Wiederholen" Verhalten und Logik in Ihrer Anwendungsfehler Behandlung Routinen oder erhöhen des reservierten Durchsatzes für die Websitesammlung berücksichtigen.

##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum reservierte Durchsatz mit DocumentDB Azure-Datenbanken, untersuchen Sie diese Ressourcen:
 
- [DocumentDB Preise](https://azure.microsoft.com/pricing/details/documentdb/)
- [Verwalten von DocumentDB Kapazität](documentdb-manage.md) 
- [Modellieren von Daten in DocumentDB](documentdb-modeling-data.md)
- [DocumentDB Leistung Ebenen](documentdb-partition-data.md)

Weitere Informationen zum DocumentDB finden Sie unter der DocumentDB Azure- [Dokumentation](https://azure.microsoft.com/documentation/services/documentdb/). 

Um mit Skalierung und Performance-Tests mit DocumentDB anzufangen, finden Sie unter [Leistung und Skala mit Azure DocumentDB testen](documentdb-performance-testing.md).


[1]: ./media/documentdb-request-units/queryexplorer.png 
[2]: ./media/documentdb-request-units/RUEstimatorUpload.png
[3]: ./media/documentdb-request-units/RUEstimatorDocuments.png
[4]: ./media/documentdb-request-units/RUEstimatorResults.png
[5]: ./media/documentdb-request-units/RUCalculator2.png
