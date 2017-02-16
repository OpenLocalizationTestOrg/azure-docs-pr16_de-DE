<properties 
    pageTitle="Verwenden Sie allgemeine DocumentDB Fällen | Microsoft Azure" 
    description="Erfahren Sie mehr über die oberen fünf Fällen für DocumentDB verwenden: Benutzer generiert Inhalt, ereignisprotokollierung, Daten im Katalog, Voreinstellungen Benutzerdaten und Internet der Dinge (IoT)." 
    services="documentdb" 
    authors="h0n" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="hawong"/>

# <a name="common-documentdb-use-cases"></a>Allgemeine DocumentDB verwenden Fällen
Dieser Artikel bietet einen Überblick über mehrere häufige Verwendung Fällen für DocumentDB.  Die Empfehlungen in diesem Artikel dienen als Ausgangspunkt bei der Entwicklung Ihrer Anwendung mit DocumentDB.   

Nach dem Lesen dieses Artikels, können Sie die folgenden Fragen beantworten ausführen: 
 
- Was sind die allgemeinen Fällen wird DocumentDB?
- Was sind, dass die Vorteile der Verwendung von DocumentDB als Benutzer generiert Inhalte?
- Was sind, dass die Vorteile der Verwendung von DocumentDB als Katalogdaten gespeichert werden?
- Was sind, dass die Vorteile der Verwendung von DocumentDB als ein Ereignisprotokoll speichern?
- Was sind, dass die Vorteile der Verwendung von DocumentDB als eine Voreinstellungen Benutzerdaten gespeichert?
- Was sind die Vorteile der Verwendung von DocumentDB als Datenspeicher für Betriebssysteme Internet der Dinge (IoT)?

## <a name="common-use-cases-for-documentdb"></a>Gemeinsame Verwendung Fällen für DocumentDB
Azure DocumentDB ist eine allgemeine NoSQL-Datenbank, die in einer Vielzahl von Applications und Verwenden von Fällen verwendet wird. Es ist eine gute Wahl für eine Anwendung, die niedrig Reaktionszeiten der Reihenfolge der Millisekunden benötigt, und muss schnell skalieren. Es folgen einige Attribute des DocumentDB, die sich gut für Applikationen leistungsfähige erleichtern.

- DocumentDB teilt den systembedingt auf Ihre Daten für hohen Verfügbarkeit und Skalierbarkeit.
- Die DocumentDB weist SSD gesicherten Speicher mit Low-Wartezeit Reihenfolge der Millisekunden Reaktionszeiten.
- DocumentDB der Unterstützung für Konsistenz Ebenen wie tatsächlichen, Sitzung und begrenzt Veraltung ermöglicht niedrige Kosten zu Performance-Verhältnis. 
- DocumentDB verfügt über eine flexible Preisgestaltung Daten geeignete-Modell, das Speicher und Durchsatz unabhängig voneinander m.
- Des DocumentDB reservierte Durchsatz Modell ermöglicht Ihnen, die Anzahl der lesen/schreiben, statt der zugrunde liegenden Hardware CPU/Arbeitsspeicher/IOPs befassen.
- Der DocumentDB Entwurf ermöglicht, die Sie zu der Anfrage großer Datenmengen in der Reihenfolge der Milliarden von Abfragen pro Tag skalieren.

Diese Attribute sind besonders hilfreich, wenn es Web, mobil, geht Spiele und IoT-Programmen, die niedrig Reaktionszeiten benötigen und große Mengen von Lese- und schreibt behandelt werden müssen. 

## <a name="user-generated-content"></a>Benutzer erzeugte Inhalt
Eine allgemeine Anwendungsfall-für DocumentDB gespeichert ist, und Benutzer Abfrage generierten Inhalt (UGC) für das Web und mobile Applications, besonders soziale Medien Applications.  Einige Beispiele für UGC sind chatten, Tweets, von Blogbeiträgen, Bewertungen und Kommentare.  Häufig ist die UGC in sozialen Medien Clientanwendungen eine Mischung aus freie Formulartext, Eigenschaften, Kategorien und Beziehungen, die nicht durch starre Struktur begrenzt werden.   

Inhalte wie Chats können Kommentare und Beiträge in DocumentDB gespeichert werden ohne Transformationen oder komplexe Objekt relationale Zuordnung Ebenen.  Dateneigenschaften können hinzugefügt oder einfach um Anforderungen entsprechen, wie der Code der Anwendung Entwickler durchlaufen somit heraufstufen schnelle Entwicklung geändert werden.  

Programme, die mit verschiedenen sozialen Netzwerken integrieren müssen zum Ändern von Schemas aus diesen Netzwerken reagieren.  Wie Daten automatisch in DocumentDB standardmäßig indiziert ist, sind die Daten bereit sind, zu einem beliebigen Zeitpunkt abgefragt werden.  Daher müssen diese Applikationen die Flexibilität zum Abrufen von Projektionen gemäß ihren Bedürfnissen entsprechenden.       

Viele der sozialen Applikationen zur globaler Ebene ausgeführt und können ein Verwendungsmuster unvorhergesehene aufweisen.  Flexibilität bei der Skalierung des Datenspeicher ist wichtig, wie der Anwendung Layer skaliert Verwendung bei Bedarf entsprechend zu.  Sie können sich durch Hinzufügen von weiteren Datenpartitionen unter einem Konto DocumentDB skalieren.  Darüber hinaus können Sie zusätzliche DocumentDB Konten über mehrere Regionen erstellen. Dienstverfügbarkeit Region DocumentDB finden Sie unter [Azure Regionen](https://azure.microsoft.com/regions/#services).   

## <a name="catalog-data"></a>Daten im Katalog
Katalog Daten Verwendungsszenarien umfassen speichern und eine Reihe von Attributen für Elemente wie Personen, Orte und Produkte Abfragen.  Einige Beispiele für Daten im Katalog sind Benutzerkonten, Produktkataloge, Gerät Register für IoT und Rechnung Material Systeme.  Attribute für diese Daten können variieren und ändert sich im Laufe der Zeit für die Anwendung Anforderungen anpassen.  

Erwägen Sie ein Beispiel eines Produktkatalogs für einen Lieferanten automotive Webparts an. Möglicherweise müssen Sie jeden Teil ihrer eigenen Attribute zusätzlich zu den allgemeinen Attributen, die alle Teile gemeinsam nutzen.  Darüber hinaus können Attribute für einen bestimmten Teil des folgenden Jahres ändern, wenn ein neues Modell freigegeben wird.  Als einem JSON-Dokument-Speicher DocumentDB unterstützt flexible Schemas können Sie zur Darstellung von Daten mit geschachtelte Eigenschaften und somit ist es gut geeignet zum Speichern von Produktdaten-Katalog.       

## <a name="logging-and-time-series-data"></a>Protokollierung und Time-Serie Daten
Protokollierung Anwendung häufig im große Datenmengen ausgegeben und mit wechselnder Attribute möglicherweise basierend auf die Version bereitgestellte Anwendung oder die Komponente Protokollieren von Ereignissen.  Log-Daten werden nicht durch komplexe Beziehungen oder starre Strukturen begrenzt. Log-Daten werden zunehmend, im JSON-Format beibehalten, da JSON einfache und einfach für Menschen lesbar ist.
   
Es gibt in der Regel zwei Bereiche verwenden Fällen im Zusammenhang mit Ereignisprotokolldaten.  Die erste Anwendungsfall-ist zum Ausführen von Ad-hoc-Abfragen über eine Teilmenge der Daten zur Behandlung dieses Problems.  Während der Problembehandlung, wird eine Teilmenge der Daten zunächst aus den Protokollen, in der Regel durch Zeitreihe abgerufen.  Klicken Sie dann erfolgt ein Drilldown durch das Dataset mit Fehlerebenen oder Fehlermeldungen filtern. Dies ist, wobei Ereignisprotokollen in DocumentDB gespeichert wird ein Vorteil ist. Log-Daten, die in DocumentDB gespeichert werden standardmäßig automatisch indiziert, und daher bereit sind, zu einem beliebigen Zeitpunkt abgefragt werden. Darüber hinaus können Daten partitionsübergreifend Log Daten als eine Uhrzeit-Serie beibehalten werden. Ältere Protokolle können zum Haut Speicherplatz pro Ihrer Aufbewahrungsrichtlinie Variationswebsites.          

Die zweite Anwendungsfall-umfasst zeitintensive Daten Analytics Aufträge offline über eine große Datenmengen Log durchgeführt.  Beispiele für diese Anwendungsfall-gehören die Verfügbarkeit von Analysis Server, Analyse der Anwendung Fehler und Clickstream Datenanalyse.  Normalerweise ist Hadoop verwendet, um die folgenden Arten von Analysen durchführen.  DocumentDB Datenbanken ist mit der Hadoop Verbinder für DocumentDB als Datenquellen und senken für Schwein, Struktur und Karte/verringern Aufträge funktionsfähig. Details für DocumentDB Hadoop Netzwerke finden Sie unter [Ausführen ein Auftrags Hadoop mit DocumentDB und HDInsight](documentdb-run-hadoop-with-hdinsight.md).      

## <a name="gaming"></a>Spiele
Die Datenbankebene ist wichtige Komponente des Ausführen von Spielen. Modernes Spiele grafisch Verarbeitung auf Mobile/Console-Clients ausführen, jedoch in der Cloud angepasste und personalisierte Inhalte wie im Spiel Stats, Integration sozialer Medien und höchst-Punktzahl setzen vorführen aufsetzen. Spiele können nur mit extrem niedrige Wartezeiten für Lesen schreibt Bereitstellen einer ansprechenden im Spiel Erfahrung und die Datenbankebene muss positiven und machen in Sätzen Anforderung während des neuen Spiel wird gestartet und Updates Feature zu behandeln.

DocumentDB von umfangreichen Maßstab Spiele wie verwendet [das durchlaufen inaktive: des Menschen keine Flächen](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) von [Nächsten Spiele](http://www.nextgames.com/), und [Rand 5: Erziehungsberechtigten](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Verwenden in beiden Fällen, Hauptvorteile von DocumentDB wurden die folgenden:

- DocumentDB ermöglicht die Leistung zu skalierende nach oben oder nach unten elastisch. Dadurch wird die Spiele Aktualisierung Profil und Stats aus Dutzende Millionen von gleichzeitigem Spieler behandelt, indem Sie einzelne API aufrufen.
- DocumentDB unterstützt Millisekunden liest und schreibt in einer beliebigen fällt ab beim Spielen zu vermeiden.
- Filtern anhand mehrerer verschiedene Eigenschaften in Echtzeit ermöglicht DocumentDB die automatische Indizierung, z. B. Player durch ihre internen Player IDs, oder deren GameCenter, Facebook, Google-IDs oder basierend auf Player Mitgliedschaft in einer Guild Abfrage suchen. Dies ist möglich, ohne komplexe indizieren oder Sharding Infrastruktur erstellen.
- Features für soziale Netzwerke einschließlich im Spiel Chatnachrichten, Player Guild Mitgliedschaften, Probleme abgeschlossen, höchst-Punktzahl setzen und sozialen Diagramme sind einfacher zu mit einem Schema flexible implementieren.
- DocumentDB als eine verwaltete Plattform-as-a-Service (PaaS) erforderlich minimalen Setup und Verwaltung arbeiten, um eine schnelle Iteration zulässig ist, und schneller auf den Markt.


## <a name="user-preferences-data"></a>Voreinstellungen Benutzerdaten
Heute kommen die meisten modernen Web- und Windows-Dienste mit komplexen Sichten und Erfahrung ein. Diese Ansichten und Erfahrung sind in der Regel dynamische, Voreinstellungen des Benutzers oder Stimmung catering und branding Anforderungen.  Daher müssen Applikationen individuelle Einstellungen effektiv abrufen, um Elemente der Benutzeroberfläche und Erfahrung schnell gerendert werden können. 

JSON ist eine effektive Format zum Benutzeroberfläche Layoutdaten darstellen, wie nicht nur einfache ist, aber auch einfach werden von JavaScript interpretiert können.  DocumentDB bietet tunable Konsistenz Ebenen, die schnelle liest mit niedriger Wartezeit schreibt zulassen. Wenn Sie also Speicherns Benutzeroberfläche Layoutdaten einschließlich der Einstellungen für individuelle als JSON-Dokumente in DocumentDB ein effektives Mittel, diese Daten über die Verbindung zu erhalten.

## <a name="iot-and-device-sensor-data"></a>IoT und Gerät Sensordaten
IoT verwenden Fällen freigeben häufig einige Muster in wie diese Aufnahme, verarbeiten und Speichern von Daten aus.  Zunächst ermöglichen Aufnahme Daten, die Daten aus dem Gerät Sensoren von verschiedenen Gebietsschemas unerwartet Aufnahme kann diese Systeme.  Als Nächstes diese Systeme verarbeiten und Analysieren streaming Daten, um Echtzeit Einsichten abgeleitet werden. Und nicht zuletzt, besonders wenn Sie nicht alle Daten landen später in einem Datenspeicher für Ad-hoc-Abfragen und offline Analytics.    

Microsoft Azure bietet Fällen verwenden, Rich-Dienste, die für IoT genutzt werden können.  Azure IoT Services sind eine Reihe von Diensten, einschließlich Azure Ereignis Hubs, Azure DocumentDB, Azure Stream Analytics, Azure Benachrichtigung Hub, Azure maschinellen Schulung, Azure HDInsight und PowerBI. 

Unerwartet Daten können von Azure Ereignis Hubs aufgenommen werden als mit niedriger Wartezeit hohen Durchsatz Daten Aufnahme vergleichbar. Aufgenommen Daten, die für Echtzeit Einblick verarbeitet werden müssen, können in Azure Stream Analytics für Echtzeit Analytics geleitet werden. Daten können in DocumentDB für die Ad-hoc-Abfrage geladen werden. Nachdem die Daten in DocumentDB geladen haben, sind die Daten bereit sind, abgefragt werden.  Die Daten in DocumentDB können als Referenzdaten als Teil des Echtzeit Analytics verwendet werden. Darüber hinaus können Daten weiteren eingeschränkt und verarbeitet werden durch Herstellen einer Verbindung DocumentDB Daten mit HDInsight für Schwein, Struktur oder Karte/verringern Projekte.  Raffinierte Daten werden dann wieder zur DocumentDB für Berichte geladen.   

Eine Stichprobe IoT-Lösung mit DocumentDB, EventHubs und Storm finden Sie unter [Hdinsight-Storm-Beispiele Repository auf GitHub](https://github.com/hdinsight/hdinsight-storm-examples/).

Weitere Informationen zu Azure Angeboten für IoT finden Sie unter [Erstellen von Internet der Your Dinge](http://www.microsoft.com/en-us/server-cloud/internet-of-things.aspx).

## <a name="next-steps"></a>Nächste Schritte
 
Um mit DocumentDB anzufangen, können Sie erstellen Sie ein [Konto](https://azure.microsoft.com/pricing/free-trial/) , und führen Sie unsere [learning Pfad](https://azure.microsoft.com/documentation/learning-paths/documentdb/) um Informationen zu DocumentDB und die benötigten Informationen zu finden. 

Oder, wenn Sie, lesen Sie weitere möchten Informationen zu Kunden DocumentDB verwenden, die folgenden Kunden Geschichten stehen zur Verfügung:

- [Nächste Spiele](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). Die toten Walking: Mann's Land Spiel soars # 1 von Azure DocumentDB unterstützt.
- [Rand](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Art der Implementierung Rand 5 für soziale Netzwerke Spielverlauf mit Azure DocumentDB ein.
- [Cortana Analytics-Katalog](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana Analytics Gallery - skalierbare Community-Website auf Azure DocumentDB erstellt.
- [Zum Kinderspiel](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Führende Integrator bietet internationalen Unternehmen globale Einblick in wenigen Minuten mit Flexible Cloud-Technologien.
- [Republik News](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Hinzufügen von Intelligence die Nachrichten Informationen mit Zweck engagierten Bürgern bereitzustellen. 
- [SGS International](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Aktivieren für einheitliche Farbe in der ganzen Welt wichtige Marken zu SGS ein. Und SGS in Azure aktiviert.
- [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Globale Füllzeichen Telenor wird mit der Cloud mit der Geschwindigkeit eines Starts verschoben. 
- [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). Im Store der Zukunft läuft unter schnelle Suche und den einfachen Datenfluss.
