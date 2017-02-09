<properties
    pageTitle="Konsistenz Ebenen in DocumentDB | Microsoft Azure"
    description="DocumentDB hat vier Konsistenz Ebenen Saldo tatsächlichen Konsistenz, Verfügbarkeit und Wartezeit vor-und Nachteile helfen."
    keywords="tatsächlichen Konsistenz, Documentdb, Azure, Microsoft azure"
    services="documentdb"
    authors="syamkmsft"
    manager="jhubbard"
    editor="cgronlun"
    documentationCenter=""/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="syamk"/>

# <a name="consistency-levels-in-documentdb"></a>Konsistenz Ebenen in DocumentDB

Azure DocumentDB ist von Grund globaler Verteilung berücksichtigen entwickelt. Es soll vorhersehbar niedriger Wartezeit Garantien, eine 99,99 % Verfügbarkeit Vereinbarung zum SERVICELEVEL und mehrere klar definierte niedrige Konsistenz-Modelle bieten. Derzeit DocumentDB bietet vier Konsistenz Ebenen: sicherer, begrenzt Veraltung Sitzung und tatsächlichen. Neben **sicherer** und der **tatsächlichen Konsistenz** Modelle häufig angeboten von anderen Datenbanken NoSQL DocumentDB auch bietet zwei sorgfältig Code und operationalized Konsistenz-Modelle – **begrenzt Veraltung** und **Sitzung**, und ihre Nützlichkeit gegen realen verwenden Fällen bestätigt. Diese vier Konsistenz Ebenen aktivieren Sie wohl begründete vor-und Nachteile zwischen Konsistenz, Verfügbarkeit und Wartezeit zusammen. 

## <a name="scope-of-consistency"></a>Umfang der Konsistenz

Die Genauigkeit der Konsistenz ist auf die Anforderung einer einzelnen Benutzer beschränkt. Eine Anforderung schreiben möglicherweise ein einfügen, ersetzen, Upsert entsprechen, oder Löschen von Transaktion (mit oder ohne die Ausführung eines zugeordneten Pre oder Beitrag Triggers). Oder eine Anforderung schreiben möglicherweise die Ausführung von Transaktionen einer JavaScript gespeicherte Prozedur Betrieb über mehrere Dokumente innerhalb einer Partition entsprechen. Wie bei der schreibt, wird eine Transaktion gelesen/Abfrage auch auf die Anforderung einer einzelnen Benutzer beschränkt. Der Benutzer gegebenenfalls über eine große paginierende Resultset, die mehrere Partitionen dauern, aber jede Transaktion ist auf eine einzelne Seite ausgelegte und served aus innerhalb einer Einzelpartition lesen.

## <a name="consistency-levels"></a>Konsistenz Ebenen

Sie können eine Konsistenz Standardstufe für Ihr Datenbankkonto konfigurieren, die auf alle Websitesammlungen (über alle Datenbanken) unter Ihrem Datenbankkonto angewendet wird. Standardmäßig werden die Konsistenz-Standardstufe angegeben haben, klicken Sie auf das Datenbankkonto alle Lese- und Abfragen in den benutzerdefinierten Ressourcen verwendet werden. Allerdings können Sie die Ebene Konsistenz Anforderung einer bestimmten gelesen/Abfrage sicher –, durch Angabe des Anfrage-Headers [[X-ms-Konsistenz-Ebene]](https://msdn.microsoft.com/library/azure/mt632096.aspx) . Es gibt vier Arten von Konsistenz Ebenen vom DocumentDB Replikations-Protokoll unterstützt, die eine löschen Wechselwirkung zwischen bestimmten Konsistenzgarantien und Leistung, bereitstellen, wie unten beschrieben.

![Definiert die DocumentDB bietet mehrere, auch (niedrige) Konsistenz-Modelle zur Auswahl][1]

**Starken**: 

- Signifikante Konsistenz bietet eine [Linearizability](https://aphyr.com/posts/313-strong-consistency-models) Garantie mit der liest gewährleistet, dass die neueste Version eines Dokuments zurückzukehren. 
- Signifikante Konsistenz gewährleistet, dass es sich bei einer schreiben nur angezeigt wird, nachdem er als von den meisten Quorum Replikate belegt wird. Ein Schreiben wird entweder synchron übermittelt, als indem sowohl der primären und der Quorum der sekundäre oder dieser abgebrochen wird. Eine gelesene ist immer bestätigt, durch die meisten Quorum lesen, ein Client kann nie eine oder teilweise schreiben und ist immer gewährleistet, dass das neueste bestätigte schreiben lesen. 
- DocumentDB-Konten, die signifikante Konsistenz konfiguriert sind nicht mehr als eine Azure Region mit ihrem Konto DocumentDB zugeordnet. 
- Die Kosten für das Lesen (in Bezug auf die [Anfrage Einheiten](documentdb-request-units.md) verbraucht) mit signifikante Konsistenz ist größer als die Sitzung und tatsächlichen, aber begrenzte Veraltung entspricht.
 

**Bounded Veraltung**: 

- Begrenzt Veraltung Konsistenzgarantien, die die Lesen von höchstens *K* Versionen oder eines Dokuments oder *t* Präfixe hinter schreibt positiven möglicherweise Zeitintervall. 
- Wenn Veraltung auswählen begrenzt werden, kann das "Alter" daher auf zwei Arten konfiguriert werden: 
    - Anzahl der Versionen *K* des Dokuments, an dem die liest hinter der schreibt positiven
    - Zeitintervall *t* 
- Begrenzt Veraltung Angebote total globale Reihenfolge außer innerhalb des Fensters"Veraltung". Notiz, die garantiert monotone lesen, die in einem Bereich innen und außen "Veraltung Fenster" vorhanden ist. 
- Begrenzte Veraltung bietet eine Garantie für erhöhte Konsistenz als Sitzung oder tatsächlichen Konsistenz. Für Applikationen Global verteilt empfehlen wir, dass Sie begrenzte Veraltung für Szenarios verwenden, wir empfehlen Ihnen, sicherer Konsistenz haben, aber sinnvoll 99,99 % Verfügbarkeit und niedrige Wartezeit. 
- DocumentDB-Konten, die mit begrenzte Veraltung Konsistenz konfiguriert sind, können eine beliebige Anzahl von Azure Regionen mit ihrem Konto DocumentDB zuordnen. 
- Die Kosten für das Lesen (im Hinblick auf RUs verbraucht) mit begrenzte Veraltung höher als Sitzung und tatsächlichen Konsistenz, aber sicherer Konsistenz identisch ist.

**Sitzung**: 

- Im Gegensatz zu den globale Konsistenz-Modellen von sicherer und begrenzte Veraltung Konsistenz Ebenen angeboten wird die Sitzung Konsistenz auf Client-Sitzung beschränkt. 
- Session-Konsistenz ist ideal für alle Szenarien, wo eine Sitzung Gerät oder Benutzer beteiligt ist, da es garantiert monotonen liest, monotonen schreibt und Lesen Ihrer eigenen schreibt (RYW) garantiert. 
- Sitzung Konsistenz bietet vorhersehbar Konsistenz für eine Sitzung und Maximum gelesen Durchsatz und den niedrigsten Wartezeit schreibt und liest bietet. 
- DocumentDB-Konten, die mit Sitzung Konsistenz konfiguriert sind, können eine beliebige Anzahl von Azure Regionen mit ihrem Konto DocumentDB zuordnen. 
- Die Kosten für das Lesen (im Hinblick auf RUs verbraucht) mit Sitzung Konsistenz Ebene ist kleiner als sicherer und begrenzte Veraltung, aber mehr als tatsächlichen Konsistenz
 

**Tatsächlichen**: 

- Tatsächlichen Konsistenz garantiert, dass alle weiteren schreibt liegen keine, später von die Replikaten innerhalb der Gruppe innerhalb. 
- Tatsächlichen Konsistenz ist unsicherste Form von Konsistenz ein Client kann, in dem die Werte abrufen, die älter als die sind, die, denen Sie vor dem gesehen hatten.
- Tatsächlichen Konsistenz stellt die unsicherste gelesen Konsistenz, bietet jedoch die niedrigste Wartezeit zum sowohl lesen und schreiben.
- DocumentDB-Konten, die mit der tatsächlichen Konsistenz konfiguriert sind, können eine beliebige Anzahl von Azure Regionen mit ihrem Konto DocumentDB zuordnen. 
- Die Kosten für das Lesen (im Hinblick auf RUs verbraucht) mit den tatsächlichen Konsistenz Ebene alle DocumentDB Konsistenz Ebenen niedrigsten ist.


## <a name="consistency-guarantees"></a>Konsistenzgarantien

In der folgenden Tabelle werden die verschiedenen Konsistenzgarantien gemäß den vier Konsistenz Ebenen erfasst.

| Gewährleistung                                                         |    Starken                                       |    Begrenzte Veraltung                                                                           |    Sitzung                                       |    Tatsächlichen                                 |
|----------------------------------------------------------|-------------------------------------------------|------------------------------------------------------------------------------------------------|--------------------------------------------------|--------------------------------------------------|
|    **Total globale Reihenfolge**                                |    Ja                                          |    Ja, außerhalb des Fensters"Veraltung"                                                      |    Nein, Reihenfolge teilweise "Sitzung"                   |    Nein                                            |
|    **Konsistente Präfix Garantie**                       |    Ja                                          |    Ja                                                                                         |    Ja                                           |    Ja                                           |
|    **Monotonen liest**                                   |    Ja                                          |    Ja, über die Regionen außerhalb des Fensters Veraltung innerhalb eines Bereichs immer.     |    Ja, für die angegebene Sitzung                    |    Nein                                            |
|    **Monotonen schreibt**                                  |    Ja                                          |    Ja                                                                                         |    Ja                                           |    Ja                                           |
|    **Lesen Sie Ihrer schreibt**                                  |    Ja                                          |    Ja                                                                                         |    Ja (in der Region schreiben)                      |    Nein                                            |


## <a name="configuring-the-default-consistency-level"></a>Konfigurieren die Konsistenz Standardstufe

1.  Klicken Sie im [Portal Azure](https://portal.azure.com/)in der Jumpbar auf **DocumentDB (NoSQL)**.

2. Wählen Sie das Datenbankkonto zu ändern, in das Blade **DocumentDB (NoSQL)** .

3. Klicken Sie in das Konto Blade auf **Konsistenz Standard**.


4. Wählen Sie in das Blade **Konsistenz Standard** die neue Konsistenz Ebene aus, und klicken Sie auf **Speichern**.

    ![Screenshot hervorheben das Symbol Einstellungen und Konsistenz Standard-Eintrag](./media/documentdb-consistency-levels/database-consistency-level-1.png)

## <a name="consistency-levels-for-queries"></a>Konsistenz Berechtigungsstufen für Abfragen

Standardmäßig für Ressourcen benutzerdefinierte ist die Ebene Konsistenz für Abfragen die Konsistenz Ebene identisch für lesen. Standardmäßig wird der Index synchron auf jedem einfügen, ersetzen oder Löschen eines Dokuments in der Auflistung aktualisiert. Dadurch werden die Abfragen die gleichen Ebene der Konsistenz des Dokuments lautet wie berücksichtigt. Während der DocumentDB schreiben optimiert ist und unterstützt längere Datenmengen Dokument schreibt, synchrones Indexwartung und konsistente Abfragen erstellen, können Sie bestimmte Websitesammlungen, um deren Index verzögert aktualisieren konfigurieren. Verzögerte Indizierung weiteren steigert die Leistung schreiben, und ist ideal für Massen Aufnahme Szenarien, wenn eine Arbeitsbelastung überladene hauptsächlich gelesen wird.  

Indizierung Modus|  Liest|  Abfragen  
-------------|-------|---------
Konsistent (Standard)|   Wählen Sie sicherer, der abgegrenzt Veraltung Sitzung, oder tatsächlichen|    Wählen Sie sicherer, der abgegrenzt Veraltung Sitzung, oder tatsächlichen|
Verzögerte|   Wählen Sie sicherer, der abgegrenzt Veraltung Sitzung, oder tatsächlichen|    Tatsächlichen  

Als können gelesen Besprechungsanfragen, Sie die Ebene Konsistenz Anforderung einer bestimmten Abfrage senken durch Angabe des Anfrage-Headers [X-ms-Konsistenz-Ebene](https://msdn.microsoft.com/library/azure/mt632096.aspx) .

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Themen zu Konsistenz Ebenen und vor-und Nachteile tun möchten, empfiehlt es sich um den folgenden Ressourcen:

-   Doug Thomas. Repliziert Datenkonsistenz Erläuterung durch Baseball (Video).   
[https://www.YouTube.com/watch?v=gluIh8zd26I](https://www.youtube.com/watch?v=gluIh8zd26I)
-   Doug Thomas. Repliziert Datenkonsistenz Erläuterung durch Baseball.   
[http://Research.Microsoft.com/pubs/157411/ConsistencyAndBaseballReport.PDF](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf)
-   Doug Thomas. Sitzung Garantien für schwach konsistent repliziert Daten.   
[http://DL.ACM.org/Citation.cfm?id=383631](http://dl.acm.org/citation.cfm?id=383631)
-   Daniel Abadi. Konsistenz vor-und Nachteile in modernen verteilt Systeme Datenbankentwurf: Linienende ist nur einen Teil der Geschichte ".   
[http://Computer.org/CSDL/mags/Co/2012/02/mco2012020037-Abs.HTML](http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html)
-   Peter Bailis, Shivaram Venkataraman, Michael J. Franklin, Joseph M. Hellerstein, Ion Stoica. Probabilistisch begrenzt Veraltung (PBS) für praktische teilweise Quorumdatenträger.   
[http://VLDB.org/pvldb/vol5/p776_peterbailis_vldb2012.PDF](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
-   Werner Vogels. Tatsächlichen einheitlich – Fortsetzung.    
[http://allthingsdistributed.com/2008/12/eventually_consistent.HTML](http://allthingsdistributed.com/2008/12/eventually_consistent.html)


[1]: ./media/documentdb-consistency-levels/consistency-tradeoffs.png
