<properties
   pageTitle="Grundlegendes zu Microservices | Microsoft Azure"
   description="Warum erstellen Cloud-Clientanwendungen mit einem Microservices Ansatz für die Entwicklung moderne ist und wie Azure Service Fabric eine Plattform zu diesem Zweck bietet eine Übersicht über"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="mfussell"/>

# <a name="why-a-microservices-approach-to-building-applications"></a>Warum eine Microservices Ansatz zum Erstellen von Clientanwendungen?
Als Softwareentwickler gibt es keine neuen in wie wir denken Sie an eine Anwendung in Komponenten der Faktor. Es ist das zentrale Paradigma des Objekts Ausrichtung, Software Abstraktionen und Aufteilung in Komponenten. Heute ist diese Factorization normalerweise in Form von Klassen und Schnittstellen zwischen freigegebenen Bibliotheken und Technologieebenen erfolgen. In der Regel wird ein gestufte Ansatz mit einem Back-End-Speicher, Geschäftslogik mittlerer Ebene und eine Front-End-Benutzeroberfläche übernommen. Was *hat* in den letzten Jahren geändert wird, dass wir, als Entwickler, verteilte Applikationen für cloudbasierte vom Unternehmen erstellen.

Die ändern geschäftliche Anforderungen sind:

- Erstellen Sie und betreiben Sie einen Dienst bei, damit Kunden in der neuen Ländern / Regionen (zum Beispiel) erreicht haben.
- Schnellere Bereitstellung von Features und Funktionen, die eine Möglichkeit agiles auf Kunden reagieren können.
- Verbesserte Ressource Auslastung zu senken.

Diese geschäftliche Anforderungen Auswirkungen *wie* wir zum Erstellen von Applications.

Weitere Informationen zu den Azure Ansatz zum Microservices, finden Sie unter [Microservices: eine Anwendung Revolution Betrieben der Cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="monolithic-vs-microservice-design-approach"></a>Monolithische und Microservice Entwurfsansatz
Alle Programme, die über einen Zeitraum weiterentwickelt werden. Erfolgreiche Applikationen weiterentwickelt, dass Personen hilfreich. Nicht erfolgreiche Anwendungen nicht weiterentwickelt und schließlich werden nicht mehr unterstützt. Kommt die Frage: wie viel wissen Sie über Ihren Anforderungen heute, und was werden sie in der Zukunft? Beispielsweise angenommen, dass Sie eine reporting Anwendung für eine Abteilung erstellen. Sie sind sicher, dass die Anwendung innerhalb des Gültigkeitsbereichs von Ihrem Unternehmen bleibt und die Berichte kurzlebige werden können, die. Verwendeter Ansatz unterscheidet sich, sagen, erstellen einen Dienst Zielcomputern Videoinhalt zu mehreren Millionen Kunden. In manchen Fällen ist der erste etwas Tisch als Prüfung des Konzepts der Faktor, mit dem wissen, dass die Anwendung später neu gestaltet werden kann. Es ist überflüssig Technik zu viel etwas, die nie verwendet wird. Es ist den üblichen engineering Wechselwirkung. Wenn Unternehmen zu erstellen, die für die Cloud sprechen, ist der Annahme andererseits, Wachstum und die Verwendung. Das Problem ist, dass Wachstum und die Dezimalstellen nicht vorhersehbar sind. Wir möchte in der Lage sein, Prototypen schnell auch wissen, dass wir auf einem Pfad zukünftigen Erfolg behandelt werden. Dies ist der Ansatz schlanken Start: erstellen, messen, erfahren Sie, durchlaufen.

Während des Zeitraums Client-/ Server-gepflegt wir konzentrieren Sie sich darüber, wie Sie mithilfe von bestimmter Technologien in jeder Kategorie Gestufte Applications erstellen. Die Begriff "monolithische" Anwendung wurde für die folgenden Verfahren entwickelt. Die Schnittstellen gepflegt werden zwischen Ebenen, und eine weitere eng verknüpften Entwurf zwischen Komponenten innerhalb jeder Kategorie verwendet. Entwickler vorgesehen und miteinander verknüpfte sind ein paar ausführbare Dateien und DLL-Dateien in Bibliotheken, kompiliert Klassen angepasst. Es gibt Vorteile solche ein monolithische Ansatz aus. Es ist oft einfacher, Entwerfen und verfügt über schnellere Anrufe zwischen Komponenten, da diese Aufrufe häufig über IPC sind. Darüber hinaus überprüft jeder ein einzelnes Produkt, wodurch weitere Personen Ressourcen effizient sein. Ist, eine enge Bindung zwischen gestufte Ebenen Ergebnisse und Sie einzelne Komponenten skalieren kann nicht. Wenn Sie Updates oder Upgrades ausführen müssen, müssen Sie für andere Personen die verspäteten Ende warten. Es ist schwieriger agiles sein.

Microservices Adresse diese Nachteile und weitere eng mit den vorstehenden Business Anforderungen ausrichten, jedoch auch über sowohl Vorzüge und Nachteile. Die Vorteile von Microservices sind, die jeweils in der Regel einfacher Geschäftsfunktionen, die bereitstellen kapselt, skalieren nach oben oder unten, testen und unabhängig voneinander verwalten. Ein wichtiger Vorteil einen Microservice Ansatz ist, dass Teams, um mehr als Business-Szenarien durch Technologie, gesteuert werden, die das der gestufte Ansatz empfiehlt. In der Praxis entwickeln kleinere Teams eine Microservice basierend auf einem Kundenszenario, verwenden eine beliebige Technologien, die sie auswählen. Kurzum, muss nicht die Organisation Standardisierung Tech um monolithische Applikationen beizubehalten. Einzelne Teams, die eigenen Services ausführen können, was für sie basierend auf der Teamwebsite Fachwissen sinnvoll ist oder was für das Problem gelöst werden am besten geeignet ist. In der Praxis gibt es eine Reihe von empfohlene Technologien wie ein bestimmtes NoSQL-Speichers oder empfiehlt Anwendungsframework.

Verwalten der höheren Anzahl von separaten Einheiten und Weitere Informationen zu komplexen Bereitstellungen und Versioning schreibgeschützte Datenbankobjekt die Kehrseite der Microservices. Erhöht sich der Netzwerkdatenverkehr zwischen den Microservices sowie die entsprechenden Wartezeiten. Probleme zahlreiche ausführliche, detaillierte Services ist eine Anleitung für ein Albtraum Leistung. Ohne Tools zur Ansicht ist diese Abhängigkeiten, es schwierig "im gesamten System finden Sie unter". Standards sind, was den Microservice Ansatz stellen Sie Ihre Bereitschaft zum kommunizieren und Vergleich der aus einem Dienst benötigten Dinge arbeiten, anstatt starre Verträge. Es ist wichtig, diese Kontakte vorab in das Design, definieren, da Dienste unabhängig voneinander aktualisieren. Eine andere Beschreibung für das Erstellen eines Konzepts mit einem Microservices Ansatz geprägt ist "abgestimmte SOA."


***Am einfachsten wird im Microservices Entwurf zu einem Entkoppelter Verbund Dienste, mit unabhängigen Änderungen an einzelnen und: bei Standards für die Kommunikation.***


Als weitere Cloud-apps gefertigt wurden, entdecken Personen an, dass diese Gliederung der gesamten app in unabhängige, Szenario konzentrieren Services eine bessere längerfristige Ansatz ist.

## <a name="comparison-between-application-development-approaches"></a>Vergleich zwischen Anwendung Entwicklung Ansätze

![Dienst Fabric Plattform Anwendungsentwicklung][Image1]

1. Eine monolithische app enthält spezifische Funktionalität und ist normalerweise geteilt durch funktionsübergreifendes Ebenen, z. B. Web, Business und Daten.

2. Sie skalieren eine monolithische app, indem Sie es auf mehreren Servern/virtuellen Computern/Container klonen.

3. Eine Anwendung Microservice getrennt Funktionalität in separaten kleinere Services.

4. Dieser Ansatz skaliert durch die Bereitstellung von jedem Dienst unabhängig voneinander, Instanzen dieser Dienste über Servern/virtuellen Computern/Container erstellen.


Entwerfen mit einer Microservice Ansatz ist kein Allheilmittel für alle Projekte, aber genauer mit den oben beschriebenen Zielen ausgerichtet. Beginnen mit einem monolithischen Ansatz ist möglicherweise annehmbar, wenn Sie wissen, dass Sie später nicht die Möglichkeit, den Code in einer Microservice Entwurf überarbeiten Sie bei Bedarf müssen. In der Regel beginnen mit einer app monolithische und langsam Teilen sie Sie in Phasen, beginnend mit der Funktionsbereichen, die mehr skalierbare oder agiles sein müssen.

Zusammengefasst ist der Ansatz Microservice an Ihrer Anwendung aus einer Reihe kleiner Webdiensten verfassen.  Die Dienste werden im Container, die über einen Cluster Maschinen bereitgestellt. Kleinere Teams Entwickeln von einem Webdienst, der Schwerpunkt auf einem Szenario unabhängig voneinander zu testen, Version, bereitstellen und jeden Dienst skaliert werden, damit die Anwendung als Ganzes weiterentwickelt werden kann.

## <a name="what-is-a-microservice"></a>Was ist eine Microservice?

Es gibt verschiedene Definitionen der Microservices, und suchen im Internet bietet viele gute Ressourcen, die eigene Gesichtspunkte und Definitionen bereitstellen. Die meisten der folgenden Merkmale von Microservices sind jedoch weit nach Vereinbarung geschlossen:

- Schließen eines Kunden oder Business-Szenario. Was ist das Problem, das Sie lösen möchten?
- Von einem kleinen engineering Team entwickelt.
- In einem geschrieben Programmiersprache und alle Framework verwenden.
- Bestehen aus einem Code und (optional) staatliche beide über bilden unabhängig voneinander, bereitgestellt und skaliert.
- Interagieren Sie mit anderen Microservices über präzise definierten Schnittstellen und Protokolle.
- Eindeutige Namen (URLs) verwendet, um die Standortinformationen aufzulösen haben.
- Bleiben Sie konsistente und in Anwesenheit Fehlern zur Verfügung.

Sie können diese Merkmale in zusammenfassen:

***Microservice Applikationen bestehen klein, unabhängig voneinander Versionsnummern und skalierbare Kundenkontakt Dienste, die über standard Protokolle mit klar definierte Schnittstellen miteinander kommunizieren.***


Wir haben die ersten beiden Punkte im vorherigen Abschnitt behandelt und jetzt wir auf Erweitern und verdeutlichen möchten, die andere Personen.

### <a name="written-in-any-programming-language-and-use-any-framework"></a>In einem geschrieben Programmiersprache und alle Framework verwenden
Als Entwickler sollten wir frei wählen alle gewünschten Sprache oder Framework, je nachdem unsere Qualifikationen oder den Anforderungen der Dienst sollen. Sie möglicherweise einige Dienste die Leistungsvorteile von C++ vor allem sonst Wert. In anderen Diensten möglicherweise die einfache verwaltete Entwicklung in c# oder Java besonders wichtig. In einigen Fällen müssen Sie möglicherweise verwenden eines bestimmten Drittanbieter-Bibliothek, Daten Speicher Technologie oder bedeutet, dass die Dienste für die Clients verfügbar zu machen.

Wenn Sie eine Technologie, im Zusammenhang mit der Verwaltung Betrieb oder Lebenszyklus und dieselbe Skalierung des Diensts ausgewählt haben.

### <a name="allows-code-and-state-to-be-independently-versioned-deployed-and-scaled"></a>Ermöglicht das bereitgestellt und skaliert Code und Zustand unabhängig voneinander Versionen gespeichert werden.  

Jedoch die Auswahl der Microservices, die Code und optional den Status schreiben sollte unabhängig bereitstellen, aktualisieren und skalieren. Dies ist tatsächlich schwieriger Probleme zu lösen, da dies in Ihrer Wahl der Technologien hängt davon ab. Anpassungsbereich für, darüber vermittelt, wie Partition (oder Shard) die Code und den Status ist eine Herausforderung. Wenn der Code und der Status separate Technologien verwenden (die in der heute diese Regel zu tun ist), müssen die Bereitstellungsskripts für Ihre Microservice skalieren sie beide bewältigen kann. Dies ist auch über die Flexibilität und Flexibilität, damit Sie einige der Microservices aktualisieren können, ohne dass Sie alle gleichzeitig aktualisieren.
An den monolithische im Vergleich zu Microservice Ansatz für einen Moment zurückgegeben wird, zeigt das folgende Diagramm der Unterschiede bei der Ansatz zum Speichern von Bundesstaat aus.

#### <a name="state-storage-between-application-styles"></a>Speichern von Statusinformationen zwischen Anwendung von Formatvorlagen
![Dienst Fabric Plattform Zustandsspeicher][Image2]

***Ist Sie auf der linken Seite der monolithische Ansatz, mit einer einzelnen Datenbank und Ebenen von bestimmter Technologien aus.***

***Ist Sie auf der rechten Seite der Microservices Ansatz, ein Diagramm der verbundener Microservices, wo Zustand ist normalerweise auf die Microservice ausgelegte und verschiedene Technologien verwendet werden.***

Monolithische und systematisch in der Regel besteht eine einzelne Datenbank, die von der Anwendung verwendet. Der Vorteil besteht darin, ihn einem einzigen Speicherort, sodass sie mühelos bereitstellen. Jede Komponente kann eine einzelne Tabelle Zustand gespeichert haben. Teams müssen strict in extrahieren Zustand, also eine Herausforderung sein. Es gibt zwangsläufig Temptations zum Hinzufügen einer neuen Spalte zu einer vorhandenen Kundentabelle, führen Sie eine Verknüpfung zwischen Tabellen und Erstellen von Abhängigkeiten bei den Layer Speicher. Nachdem Sie in diesem Fall können keine einzelne Komponenten skaliert werden. In der Ansatz Microservices jeden Dienst verwaltet und eigenen Zustand gespeichert. Jeden Dienst ist verantwortlich für die Skalierung der Code und den Bundesstaat zusammen, um den Bedarf des Diensts entsprechen. Ein Nachteil besteht darin, wenn eine Notwendigkeit zum Erstellen von Ansichten oder Abfragen besteht, der die Daten der Anwendung wird Abfragen in unterschiedlichen Zustand speichern. Normalerweise ist dies gelöst, indem Sie eine separate Microservice, bei dem eine Ansicht für eine Sammlung von Microservices erstellt haben. Wenn Sie mehrere Ad-hoc-Abfragen für die Daten ausgeführt werden müssen, sollten jeder Microservice seine Daten in einer Datawarehousing-Dienst offline Analytics schreiben.

Versionsverwaltung gilt nur für die bereitgestellte Version von einer Microservice also mehrere, unterschiedliche Versionen bereitstellen und Ausführen von nebeneinander. Versionsverwaltung Adressen die Szenarien, wo eine neuere Version von einer Microservice während des Upgrades schlägt fehl und muss auf eine frühere Version zurückzusetzen. Das andere Szenario für Versioning ist Durchführung A/B-Schreibweise testen, wo andere Benutzer unterschiedliche Versionen des Diensts auftreten. Sie beträgt beispielsweise allgemeine Upgrade einer Microservice für eine bestimmte Gruppe von Kunden neue Funktionalität testen, bevor Sie weit Weitere parallelen. Nach der Microservices Lifecycle Management bringt uns dies jetzt zu Kommunikation zwischen diesen.


### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>Interagiert mit anderen Microservices über präzise definierten Schnittstellen und Protokolle

In diesem Thema benötigt wenig Aufmerksamkeit, da es umfassende Dokumentation auf Service-orientierte Architektur in den letzten 10 Jahren, die Kommunikationsmuster beschreiben veröffentlicht ist. Dienst Kommunikation verwendet in der Regel einen REST Ansatz mit Protokolle HTTP und TCP und XML oder JSON als das Format. Aus Sicht der Benutzeroberfläche empfiehlt es sich über die optimale Nutzung des Web Entwurf Ansatzes. Es gibt jedoch nichts hindert Sie binäre Protokolle oder eigene Datenformate verwenden. Bereiten Sie sich für Personen, dass nacheinander schwieriger Ihrer Microservices verwenden, wenn diese offen verfügbar sind.

### <a name="has-a-unique-name-url-used-to-resolve-its-location"></a>Hat einen eindeutigen Namen (URL) verwendet, um die Position zu beheben.

Denken Sie daran, wie wir sagen beibehalten, dass der Microservice Ansatz wie im Internet ist? Wie im Web muss Ihr Microservice adressiert werden, wo er ausgeführt wird. Wenn Sie sind Autos nachdenkt, und die einer bestimmten Microservice ausgeführt wird, werden Punkte schnell fehlerhafte geleitet. In die gleiche Weise wie DNS einen bestimmten URL zu einem bestimmten Computer aufgelöst wird muss der Microservice einen eindeutigen Namen haben, damit die aktuelle Position sichtbar ist. Microservices benötigen adressierbare Namen, die sie unabhängig von der Infrastruktur machen, die sie ausgeführt werden. Dies bedeutet, dass eine Interaktion zwischen wie der Dienst bereitgestellt wird und wie sich herausstellt, da es Service-Registrierung werden muss, vorhanden ist. Gleichmäßig, wenn ein Computer fehlschlägt die Registrierung Dienst muss Ihnen mitteilen, wo der Dienst jetzt ausgeführt wird. Gelangen Sie zum nächsten Thema: Stabilität und Konsistenz.

### <a name="remains-consistent-and-available-in-the-presence-of-failures"></a>Bleibt konsistent und in Anwesenheit Fehlern verfügbar

Umgang mit unerwarteter Fehler ist eines der schwierigsten Probleme zu lösen, insbesondere in einem verteilten System. Behandeln von Ausnahmen ist ähnlich der Code, den wir als Entwickler schreiben, und dies ist auch der Stelle, an der die meiste Zeit testen aufgewendet wurde. Aber das Problem ist etwas komplexer als das Schreiben von Code, um Fehler zu behandeln. Was geschieht, wenn der Computer, auf dem die Microservice läuft, schlägt fehl? Nicht nur müssen Sie diesen Fehler Microservice (harte Problem in einem eigenen) erkennen, aber Sie können auch noch eine Ihrer Microservice neu starten. Eine Microservice muss flexibel in Bezug auf Fehler und starten Sie häufig auf einem anderen Computer aus Gründen der Verfügbarkeit erneut. Diese auch gehört zum nach unten bis zum welchen Status für die Microservice gespeichert wurde, wo kann Wiederherstellung behilflich dieser Status aus, und ob es sich um erfolgreich starten können. Kurzum, muss es Stabilität in der Computer (dem Neustart des Prozesses) sowie Stabilität in den Zustand oder Daten (keine Daten verloren und die Daten gleich).

Die Probleme der Stabilität werden verstärkt, während andere Szenarien, z. B. wann Fehlern bei der Aktualisierung Anwendung erfolgen. Wiederherstellen muss nicht die Microservice, mit dem Bereitstellungssystem arbeiten. Es muss auch dann entscheiden, ob auf eine neuere Version voranzutreiben oder stattdessen Zurücksetzen einer früheren Version zu einen konsistenten Status verwalten fortgesetzt werden kann. Wie die folgenden Fragen an, ob es gibt genügend Computer verfügbar nach vorne verschieben beibehalten und zum Wiederherstellen der vorherigen Versionen der Microservice berücksichtigt werden müssen. Setzt die Microservice ausgeben Gesundheitsinformationen, um diese Entscheidungen treffen können.

### <a name="reports-health-and-diagnostics"></a>Berichte Gesundheit und Diagnose

Es sind möglicherweise offensichtlich, und es wird häufig übersehen, aber es ist wichtig, dass eine Microservice seine Gesundheit und Diagnose Berichte. Es gibt andernfalls einen kleinen Einblick aus Sicht der Vorgänge. Ereignisse, die über eine Reihe von unabhängigen Diensten abgleichen und Umgang mit maschinellen Uhr schief eingezogenen Blättern in der Ereignisreihenfolge am sinnvollsten ist eine Herausforderung. Auf die gleiche Weise, die Sie mit einem Microservice über: bei Protokollen und Datenformaten interagieren, entsteht es eine Standardisierung in So melden Sie sich Gesundheit und Ereignisse, die schließlich ein Ereignis Store für Abfragen und Anzeigen von notwendig. Bei einem Microservices Ansatz ist es Schlüssel, der die verschiedenen Teams auf einem einzelnen Protokollierungsformat stimmen.  Es muss ein einheitliches Verfahren zum Anzeigen von Diagnoseprotokollen Ereignisse in der Anwendung als Ganzes.

Gesundheit unterscheidet sich von der Diagnose. Gesundheit geht die Microservice reporting aktuellen Zustand entsprechende Aktionen ausführen. Ein guter Beispiel arbeitet mit Upgrades und Bereitstellung Verfahren zum Verwalten der Verfügbarkeit beschrieben. Ein Dienst möglicherweise werden momentan fehlerhaft, da ein Prozessabsturz oder Computer Neustart, aber weiterhin funktionsfähig. Die letzte benötigte besteht darin, schlechter anlegen mithilfe einer Aktualisierung. Die beste Vorgehensweise besteht darin Untersuchungen zuerst erledigen, oder lassen Sie Zeit für die Microservice wiederherstellen. Systemereignisse aus einer Microservice können wir treffen von Entscheidungen informiert und, Hilfe Self Reparatur Dienste zu erstellen.

## <a name="service-fabric-as-a-microservices-platform"></a>Dienst Fabric als Microservices Plattform

Azure Service-Struktur entwickelt von Microsoft Übergang am Übermitteln von Feld-Produkten, die in der Regel in der Formatvorlage monolithische wurden, zur Bereitstellung von Services. Die Oberfläche erstellen und große Dienste, wie etwa SQL Azure-Datenbanken und DocumentDB, betrieben geformt Dienst Fabric. Die Plattform entwickeltes über einen Zeitraum weitere und weitere Dienste es angenommen. Wichtiger Dienst Fabric hatten überall ausführen: nicht nur in Azure, sondern auch in Windows Server-Bereitstellungen eigenständigen.

***Zweck der Dienst Fabric besteht, erstellen und Ausführen von einem Dienst die Festplatte Probleme lösen und effizient, Nutzung von Infrastrukturressourcen, damit Teams Business-Probleme, die mit einem Microservices Ansatz beheben können.***

Fabric-Dienst bietet zwei weit gefasste Bereiche Sie Applikationen mit einem Microservices Ansatz erstellen können:

- Eine Plattform, die System Services zum Bereitstellen, aktualisieren, erkennen und fehlgeschlagene Dienste neu starten, ermitteln den Speicherort, Status verwalten und Überwachen des Systemzustands bereitstellt. Diese Systemdienste aktivieren facto verschiedene Aspekte des Microservices zuvor beschrieben.

-  Programmierung APIs oder Framework, damit Sie Applikationen als Microservices erstellen können: [zuverlässigen Akteuren und zuverlässigen Services](service-fabric-choose-framework.md). Code Ihrer Wahl können Sie natürlich um Ihre Microservice zu erstellen. Aber diese APIs stellen Sie den Auftrag einfacher und diese in die Plattform auf tieferer Ebene zu integrieren. Auf diese Weise können Sie Gesundheit und Diagnose Informationen abrufen beispielsweise oder können Nutzen der integrierten hohen Verfügbarkeit.

***Dienst ist, ist unabhängig auf, wie Sie Ihrem Dienst erstellen, und jeder Technologie können. Es wird jedoch integrierte programming APIs bereitgestellt, die zum Erstellen von Microservices erleichtern.***

### <a name="are-microservices-right-for-my-application"></a>Sind Sie Microservices rechts für meine Anwendung?

Vielleicht sind. Was wir erfahrener wurde als mehr Teams in Microsoft gestartet haben, für die Cloud geschäftlichen Gründen erstellen, von denen viele die Vorteile einen ähnlichen Microservice Ansatz Aufzeichnen von realisiert. Bing, hat beispielsweise Microservices in Suchen nach Jahren entwickeln. Für andere Teams war der Microservices Ansatz neu. Sie vorhanden, die es harte Probleme außerhalb ihrer Bereiche Core Stärke gelöst wurden. Dies ist, warum Dienst Fabric Zug als Technologie Wahl zum Erstellen von Diensten gewonnen.

Das Ziel der Dienst Fabric besteht darin, die Komplexität der Erstellung von Applications mit einem Microservice Ansatz zu verringern, so, dass Sie keinen aufzurufen, wie viele teure redesigns. Klein anfangen, Skalieren bei Bedarf Dienste deaktivieren, neue hinzufügen und weiterentwickelt mit Kunden Verwendung –, die der Ansatz ist. Wir wissen auch, dass in der Praxis vorhanden viele andere Probleme noch gelöst werden sind, um Microservices für die meisten Entwickler mehr zugänglich zu machen. Container und Modell der Akteur-Programmierung sind Beispiele für kleinen Schritten in die Richtung, und wir sind sicher, dass weitere Innovationen auftreten können, um dies zu erleichtern.
 
<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen:
    * [Übersicht über die Terminologie Fabric Service](service-fabric-technical-overview.md)
    * [Microservices: Eine Anwendung-Revolution Betrieben der cloud](https://azure.microsoft.com/en-us/blog/microservices-an-application-revolution-powered-by-the-cloud/)


[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
