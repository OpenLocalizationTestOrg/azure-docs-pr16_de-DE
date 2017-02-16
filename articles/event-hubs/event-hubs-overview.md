<properties 
    pageTitle="Übersicht über Azure Ereignis Hubs | Microsoft Azure"
    description="Einführung und Azure Ereignis Hubs im Überblick."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm" />

# <a name="azure-event-hubs-overview"></a>Azure Ereignis Hubs (Übersicht)

Viele moderne Lösungen beabsichtigen adaptive Kunden Erfahrung bereitstellen oder durch fortlaufender Feedback und automatisierte werden Produkte zu verbessern. Mit der Herausforderung, wie sicher und zuverlässig sehr große Mengen von Informationen aus vielen gleichzeitigen Herausgeber verarbeitet werden solche Lösungen konfrontiert. Microsoft Azure Ereignis Hubs ist ein verwalteten Plattform-Dienst, der die Grundlage für umfangreiche Daten Aufnahme in einer Vielzahl von Szenarien bildet. Beispiele für solche Szenarien sind in mobile-apps, Datenverkehr Informationen aus dem Web-Farmen im Spiel Ereignis erfassen in Console spielen, Überwachung Verhalten oder Daten werden von industrielle Maschinen gesammelt oder Fahrzeuge verbunden. Die allgemeine Rolle, die Ereignis Hubs in Lösungsarchitekturen wiedergegeben wird, dass es als "Vordertür" für ein Ereignis Verkaufspipeline oft bezeichnet ein *Ereignis Ingestor*fungiert. Ein Ereignis Ingestor ist eine Komponente oder den Dienst, der zwischen Ereignisherausgeber und Ereignisempfänger, um die Herstellung von eines Streams von Ereignissen aus der Verarbeitung diese Ereignisse entkoppeln befindet.

![Ereignis Hubs](./media/event-hubs-overview/IC759856.png)

Azure Ereignis Hubs ist ein Ereignis processing-Dienst, Ereignis und werden eingehende in der Cloud bei umfangreichen Skalieren mit niedriger Wartezeit und hohe Zuverlässigkeit bietet. Dieser Dienst, zusammen mit anderen Diensten untergeordneten ist besonders hilfreich, Anwendungsinstrumentation, Bearbeitung des Benutzers Erfahrung oder einen Workflow und Szenarien Internet der Dinge (IoT). Ereignis Hubs bietet einen Nachricht Stream behandeln Videofunktionen, und zwar ein Ereignis-Hub eine Entität ähnlich wie Warteschlangen und Themen handelt, hat Merkmale, die von herkömmlichen Enterprise messaging sehr unterscheiden. Enterprise messaging Szenarien erfordern häufig anspruchsvolle Funktionen wie Abfolge, Dead-Textformats, Unterstützung von Transaktionen und Garantien sicherer Übermittlung, während die dominante Sorge für Ereignis Aufnahme hohen Durchsatz und Verarbeitung Flexibilität für Ereignisstreams ist. Daher unterscheiden sich Ereignis Hubs Funktionen aus Dienstbus Themen, da diese Option hohen Durchsatz und Szenarien Verarbeitung von Ereignissen stark verzerrt werden. Als solches implementieren Ereignis Hubs einiger messaging-Funktionen nicht, die Themen verfügbar sind. Wenn Sie diese Funktionen benötigen, bleiben Themen die optimale Kombination aus.

Ein Ereignis Hub wird Ereignis Hubs Namespace auf oberster Ebene ähnlich wie Servicebuswarteschlangen und Themen erstellt. Ereignis Hubs werden als ihre primären API-Schnittstellen AMQP und HTTP verwendet. Das folgende Diagramm zeigt die Beziehung zwischen Ereignis Hubs und Dienstbus.

![Ereignis Hubs](./media/event-hubs-overview/IC741188.png)

## <a name="conceptual-overview"></a>Konzeptionelle Übersicht

Ereignis Hubs bietet streaming über ein Muster partitionierten Consumer Nachricht. Verwenden ein [Consumer Mitbewerber auftritt](https://msdn.microsoft.com/library/dn568101.aspx) -Modell, das in der einzelnen Consumer aus derselben Warteschlange oder Ressource lesen möchte, Warteschlangen und Themen. Diese induzierten Risikos für Ressourcen führt schließlich Komplexität und Maßstab Grenzwerte für die Verarbeitung von Applications Stream. Ereignis Hubs verwendet eine partitionierte Consumer Muster in der einzelnen Consumer nur eine bestimmte Teilmenge oder Teil der Nachricht Stream liest. Dieses Muster ermöglicht horizontale Skalierung für die Verarbeitung von Ereignissen sowie andere Stream ausgerichtete Features, die in Warteschlangen und Themen nicht verfügbar sind.

### <a name="partitions"></a>Partitionen

Eine Partition ist eine geordnete Abfolge von Ereignissen, die in einem Ereignis-Hub vorhanden ist. Beim Eintreffen von neuere Ereignisse, werden diese an das Ende dieser Sequenz hinzugefügt. Eine Partition kann als ein "Commit-Protokoll" vorstellen

![Ereignis Hubs](./media/event-hubs-overview/IC759857.png)

Partitionen beibehalten Daten für jeweils konfigurierten Aufbewahrungsrichtlinien, die auf der Ebene Ereignis Hub festgelegt werden. Diese Einstellung gilt alle partitionsübergreifend im Hub. Ereignisse ablaufen über die Zeit; Sie können nicht explizit löschen. Ein Ereignis Hub enthält mehrere Partitionen. Jede Partition ist unabhängig und einem eigenen Abfolge von Daten enthält. Daher wächst Partitionen häufig zu unterschiedlichen Sätzen.

![Ereignis Hubs](./media/event-hubs-overview/IC759858.png)

Die Anzahl der Partitionen bei der Erstellungszeit Ereignis Hub angegeben und muss zwischen 2 und 32 (die Standardeinstellung ist 4). Partitionen ein Daten Organisation Verfahren sind und weitere beziehen sich auf den Grad der untergeordneten Parallelität in verwendetes als auf Ereignis Hubs Durchsatz erforderlich. Dadurch wird die Option dar, der die Anzahl der Partitionen ein Ereignis Hub direkt im Zusammenhang mit der Anzahl der Leser gleichzeitig, die Sie erwartet haben. Nach der Erstellung des Ereignisses Hub ist die Partitionsanzahl nicht innere; Berücksichtigen Sie diese Anzahl im Hinblick auf langfristiges erwarteten skalieren ein. Sie können die Beschränkung 32 Partition erhöhen, indem Sie die Dienstbus Supportteam.

Während der Partitionen sind erkennbar und direkt an gesendet werden können, empfiehlt es sich, damit keine Daten auf bestimmte gesendet. Stattdessen können Sie höheren Ebene Konstrukte, die in den Abschnitten [Ereignis Publisher](#event-publisher) und [Publisher-Richtlinie](#capacity-and-security) eingeführt werden.

Nachrichten werden in dem Kontext des Ereignisses Hubs als *Ereignisdaten*bezeichnet. Ereignisdaten enthält, im Textkörper des Ereignisses, eine benutzerdefinierte Eigenschaftensammlung und verschiedene Metadaten über das Ereignis wie deren Offset an die Partition und deren Zahl in der Sequenz Stream. Partitionen werden durch eine Folge Ereignisdaten ausgefüllt.

## <a name="event-publisher"></a>Herausgeber Ereignisses

Jede Person, die Ereignisse oder Daten an einen Hub Ereignis sendet ist ein *Ereignis Publisher*. Ereignisherausgeber können Ereignisse mit HTTPS oder AMQP 1.0 veröffentlichen. Ereignisherausgeber Verwenden einer freigegebenen Access Signatur (SAS) Token identifizieren sich an einen Hub Ereignis und können eine eindeutige Identität haben, oder verwenden Sie eine allgemeine SAS Token, je nach Anforderung des Szenarios an.

Weitere Informationen zum Arbeiten mit SAS finden Sie unter [Freigegebene Access Signatur Authentifizierung mit Dienstbus](../service-bus-messaging/service-bus-shared-access-signature-authentication.md).

### <a name="common-publisher-tasks"></a>Allgemeine Publisher-Aufgaben

In diesem Abschnitt werden allgemeine Aufgaben für Ereignisherausgeber.

#### <a name="acquire-a-sas-token"></a>Erfassen Sie ein SAS token

Freigegebene Access Signatur (SAS) ist der Authentifizierungsmethode für Ereignis Hubs. Dienstbus bietet SAS Richtlinien am Namespace und Ereignis Hub Ebene an. Ein SAS Token aus einem SAS Schlüssel generiert wird und ein Hash SHA einer URL, die in einem bestimmten Format codiert ist. Verwenden den Namen der Schlüssel (Richtlinie) und der Token, Dienstbus neu generieren des Hashs und somit Authentifizierung des Absenders. In der Regel werden SAS Token für Ereignisherausgeber mit nur **Senden** von Berechtigungen für einen bestimmten Ereignis Verteiler erstellt. Dieses SAS token URL Verfahren ist die Grundlage für Publisher-Kennung in die Publisherrichtlinie eingeführt werden. Weitere Informationen zum Arbeiten mit SAS finden Sie unter [Freigegebene Access Signatur Authentifizierung mit Dienstbus](../service-bus-messaging/service-bus-shared-access-signature-authentication.md).

#### <a name="publishing-an-event"></a>Veröffentlichen eines Ereignisses

Sie können ein Ereignis über AMQP 1.0 oder HTTPS veröffentlichen. Dienstbus stellt eine [EventHubClient](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.eventhubclient.aspx) Klasse zum Veröffentlichen von Ereignissen zu einem Ereignis-Hub von .NET Clients. Für andere Laufzeiten und Plattformen können Sie alle AMQP 1.0-Client, wie z. B. [Apache Qpid](http://qpid.apache.org/)verwenden. Sie können Ereignisse einzeln veröffentlichen oder zusammengefasst. Eine einzelne Publikation (Ereignis Dateninstanz) sind maximal 256 KB ist, unabhängig davon, ob es sich um ein einzelnes Ereignis oder einen Stapel handelt. Veröffentlichen von Ereignissen größer als dies zu einem Fehler führt. Es ist eine bewährte Methode für Herausgeber Partitionen innerhalb der Ereignis-Hub nicht bewusst sein und nur eine *Taste partitionieren* (eingeführt werden im nächsten Abschnitt), oder seine Identität über deren SAS Token anzugeben.

Die Auswahl zur Verwendung von AMQP oder HTTPS gilt nur für die Verwendungsszenario. AMQP erfordert die Einrichtung eines beständigen bidirektionaler Sockets in Erweiterung Sicherheit auf Datensatzebene (TLS) oder SSL/TLS transportieren. Dies kann ein teurer Vorgang im Hinblick auf Netzwerkverkehr sein, sondern nur am Anfang einer Sitzung AMQP auftritt. HTTPS weist einen niedrigeren anfänglichen Aufwand, aber erfordert SSL zusätzlichen Aufwand für jede Anforderung. Für Herausgeber, die häufig Veröffentlichen von Ereignissen, bietet AMQP Leistung, Wartezeit und Durchsatz deutlich zu senken.

### <a name="partition-key"></a>Partitionsschlüssel

Partitionsschlüssel ist ein Wert, der eingehende Zuordnen von Ereignisdaten in bestimmten Partitionen zum Zweck der Datenorganisation verwendet wird. Der Partitionsschlüssel ist ein Absender gelieferten Wert in einem Ereignis-Hub übergeben. Es wird durch eine statische hashing Funktion verarbeitet das Ergebnis der Zuordnung Partition erstellt. Wenn Sie beim Veröffentlichen eines Ereignisses Partitionsschlüssel nicht angeben, wird eine Funktion RUNDEN Robert Zuordnung verwendet. Bei der Verwendung von Partition Schlüssel kennt Herausgeber des Ereignisses nur seine Partitionsschlüssel, nicht die Partition, in dem die Ereignisse veröffentlicht werden. Dieser Entkopplung Schlüssel und Partition isoliert den Absender aus benötigen zu viel der untergeordneten Verarbeitung und Speicherung Ereignisse kennen. Partition Tasten zum Organisieren von Daten für die Verarbeitung von untergeordneten wichtig sind, aber grundsätzlich nicht verknüpften auf sich selbst sind. Eine pro Gerät oder einen eindeutigen Identität ist eine gute Partitionsschlüssel, sondern andere Attribute wie Geography auch Benutzer werden zum Gruppieren von verwandten Ereignisse in einer einzelnen Partition verwendet. Die folgende Abbildung zeigt Ereignis Absender mithilfe von Partition Keys auf anheften.

![Ereignis Hubs](./media/event-hubs-overview/IC759859.png)

Ereignis Hubs wird sichergestellt, dass alle Ereignisse, die denselben Schlüsselwert Partition Freigabe in der Reihenfolge, und klicken Sie auf dieselbe Partition übermittelt werden. Wichtiger: Wenn Partition Tasten mit Publisher-Richtlinien, die im nächsten Abschnitt beschrieben verwendet werden müssen die Identität des Herausgebers und den Wert von der Partitionsschlüssel übereinstimmen. Andernfalls tritt ein Fehler auf.

### <a name="event-consumer"></a>Ereignisempfänger

Jede Person, die Ereignisdaten aus einem Ereignis-Hub liest ist ein Ereignisconsumer. Alle Ereignisempfänger lesen Stream von Ereignissen bis Partitionen in einer Gruppe Consumer. Jede Partition, sollte jeweils nur eine aktive Reader verfügen. Alle Ereignis Hubs Nutzer verbinden über die AMQP 1.0-Sitzung, in der Ereignisse übermittelt werden, sobald sie verfügbar sind. Der Client muss nicht für die Verfügbarkeit von Daten Umfrage.

#### <a name="consumer-groups"></a>Consumer Gruppen

Der Veröffentlichen/Abonnieren-Prozess des Ereignisses Hubs wird über Consumer Gruppen aktiviert. Eine Gruppe Consumer ist einer Ansicht (Bundesstaat, Position oder Offset) eine gesamte Ereignis Hub an. Consumer Gruppen aktivieren haben mehrere in Anspruch nehmen Applikationen auf die jeweils eine separate Ansicht des Streams Ereignis und Lesen des Streams unabhängig voneinander in ihrem eigenen Tempo und mit ihren eigenen Versatz. In einem Stream Verarbeitung Architektur entspricht jede untergeordneten Anwendung einer Gruppe Consumer. Wenn Sie das Ereignisdaten in langfristiges Speicher zu schreiben möchten, ist diese Speicher Autor Anwendung einer Gruppe Consumer. Komplexe Ereignis Verarbeitung wird von einer anderen Consumer Gruppe ausgeführt. Sie können nur über eine Gruppe Consumer Zugriff. Ein Ereignis Hub ist immer eine Consumer Standardgruppe, und Sie können bis zu 20 Consumer Gruppen für eine Standard Ebene Ereignis Hub erstellen.

Es folgen Beispiele für die Consumer Gruppe URI Messe:

    //<my namespace>.servicebus.windows.net/<event hub name>/<Consumer Group #1>
    //<my namespace>.servicebus.windows.net/<event hub name>/<Consumer Group #2>

Die folgende Abbildung zeigt den Ereignisempfänger innerhalb von Consumer Gruppen.

![Ereignis Hubs](./media/event-hubs-overview/IC759860.png)

#### <a name="stream-offsets"></a>Stream versetzt

Ein Offset ist die Position eines Ereignisses innerhalb einer Partition. Sie können einen Offset als clientseitige Cursor vorstellen. Der Offset ist ein Byte Nummerierung des Ereignisses. Dies ermöglicht es einen Ereignisconsumer (Reader), geben Sie ein Punkt im stream von Ereignissen aus dem sie Lesen von Ereignissen beginnen möchten. Sie können den Offset als Zeitstempel oder als einen Wert angeben. Nutzer sind zum Speichern ihrer eigenen Versatz Werte außerhalb des Ereignisses Hubs Dienstes verantwortlich.

![Ereignis Hubs](./media/event-hubs-overview/IC759861.png)

Innerhalb einer Partition umfasst jedes Ereignis einen Offset. Dieser Offset wird von Nutzer verwendet, um die Position in der Sequenz Ereignis für eine bestimmte Partition anzuzeigen. Versatz können übergeben werden an den Hub Ereignis als entweder eine Zahl oder einen Timestamp-Wert Wenn ein Leser eine Verbindung herstellt.

#### <a name="checkpointing"></a>Geänderte

*Geänderte* umfasst die Leser markieren oder ihre Position innerhalb einer Partition Ereignis Sequenz abzuschließen. Geänderte Consumer verantwortlich ist und auf Basis pro Partition innerhalb einer Gruppe Consumer auftritt. Dies bedeutet, dass für jede Gruppe Consumer jede Partition Reader seine aktuelle Position verfolgen muss im stream und den Dienst können darüber informieren, wenn sie den Datenstream abgeschlossen hält. Wenn Sie ein Reader aus einer-Abschnitt, getrennt wird, wenn sie eine Verbindung herstellen beginnt mit dem Lesebereich bei der Wissensstand, die zuvor durch den letzten Leser dieser Partition in dieser Gruppe Consumer übermittelt wurde. Wenn der Reader eine Verbindung herstellt, übergibt es diesen Offset an den Ereignis-Hub an die Position, an der lesen. Auf diese Weise können Sie auf beide Ereignisse kennzeichnen als "erledigt" geänderte Verwendungen und Stabilität im Fall eines Failovers zwischen Lesern, die auf anderen Computern bereitstellen verwenden. Da Ereignisdaten, für die Aufbewahrung Intervall zum Zeitpunkt der Hub Ereignis erstellt wurde angegeben beibehalten werden, ist es möglich, zu älteren Daten zurückkehren, indem Sie einem unteren Versatz von diesem Prozess geänderte angeben. Durch diese Verfahren ermöglicht geänderte sowohl Failover Stabilität und eine gesteuert Ereignis Stream Wiedergabe.

#### <a name="common-consumer-tasks"></a>Allgemeine Consumer Aufgaben

In diesem Abschnitt werden allgemeine Aufgaben für das Ereignis Hubs Ereignisempfänger oder Reader. Alle Ereignis Hubs Nutzer, die mit AMQP 1.0 hergestellt werden. AMQP 1.0 ist eine Sitzung und -Unterstützung bidirektionale Kommunikation Channel. Jede Partition verfügt über eine AMQP 1.0 Link Sitzung, die die Übertragung von Ereignissen getrennt von Partition erleichtert.

##### <a name="connect-to-a-partition"></a>Herstellen einer Verbindung eine Partition mit

Um Ereignisse aus einem Ereignis-Hub nutzen zu können, muss ein Consumer auf eine Partition verbunden sein. Wie zuvor schon erwähnt, können Sie immer Partitionen über eine Gruppe Consumer an. Als Teil des Modells partitionierten Consumer sollte nur ein einzigen Leser einem beliebigen Zeitpunkt innerhalb einer Gruppe Consumer auf einer Partition aktiv sein. Es ist üblich, bei der Verbindung mit einer Leasingmechanismus verwenden, um die Leser Verbindungen auf bestimmte koordinieren Partitionen direkt. Auf diese Weise ist es möglich, für jede Partition in einer Gruppe Consumer nur eine aktive Reader haben. Verwalten von der Position in der Sequenz für einen Reader ist eine wichtige Aufgabe, die durch geänderte erreicht ist. Dieses Feature wird mithilfe der Klasse [EventProcessorHost](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.eventprocessorhost.aspx) für .NET Clients vereinfacht. [EventProcessorHost](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.eventprocessorhost.aspx) ist eine intelligente Consumer-Agent und wird im nächsten Abschnitt beschrieben.

##### <a name="read-events"></a>Gelesene Ereignisse

Nachdem eine Sitzung AMQP 1.0 und Link ist für eine bestimmte Partition geöffnet, werden Ereignisse vom Dienst Ereignis Hubs an den AMQP 1.0-Client übermittelt. Dieses Verfahren Übermittlung ermöglicht höhere Durchsatz und unteren Wartezeit als Abruf-basierte Verfahren, wie z. B. HTTP-GET. Ereignisse an den Client gesendet werden, enthält eine Instanz jedes Ereignis wichtige Metadaten wie Bereich.verschieben und Sequenz, mit denen Prüfpunkt-Feature für das Ereignis Sequenz zu erleichtern.

![Ereignis Hubs](./media/event-hubs-overview/IC759862.png)

Es ist sicherstellen, dass Sie diesen Offset auf eine Weise verwalten, dass Sie am besten Verwalten von höchstwahrscheinlich in den Stream Verarbeitung können.

## <a name="capacity-and-security"></a>Kapazität und Sicherheit

Ereignis Hubs ist eine hochgradig skalierbare parallele Architektur für eingehende Stream. Als solche, gibt es verschiedene wichtige Aspekte zu beachten beim Ändern der Größe und dieselbe Skalierung eine Lösung basierend auf Ereignis Hubs. Die erste dieser Steuerelemente Kapazität heißt *Durchsatz Einheiten*, im folgenden Abschnitt beschrieben.

### <a name="throughput-units"></a>Durchsatz Einheiten

Die Durchsatzkapazität des Ereignisses Hubs wird durch Durchsatz Einheiten gesteuert. Durchsatz Einheiten werden Einheiten Kapazität vorab erworben werden. Eine einzelne Durchsatz Einheit umfassen Folgendes:

- Eingehende: Bis 1 MB pro zweiten oder 1000 Ereignisse pro Sekunde.

- Ausgang: Bis 2 MB pro Sekunde.

Um die benötigte Kapazität zur Verfügung gestellt, um die Anzahl der gekauften Durchsatz Einheiten eingehende beschränkt. Senden von Daten über diese Betrag ergibt Ausnahmefehler "Kontingent überschritten". Dieser Betrag ist entweder 1 MB pro zweiten oder 1000 Ereignisse pro Sekunde, stammt. Ausgang erzeugt kein Drosselung Ausnahmen, aber auf die Menge der erworbenen Durchsatz Zeiteinheiten vorgesehenen Datenübertragung beschränkt ist: 2 MB pro Sekunde pro Einheit Durchsatz. Wenn Sie für die Veröffentlichung Zins Ausnahmen erhalten oder würden finden Sie unter höheren Ausgang, achten Sie darauf, wie viele Durchsatz Einheiten prüfen Sie für den Namespace erworben haben, in denen das Ereignis Hub erstellt wurde. Um weitere Durchsatz Einheiten zu erhalten, können Sie die Einstellung auf der Seite **Namespaces** auf der Registerkarte **Skalieren** im [Azure klassischen Portal][]anpassen. Sie können auch diese Einstellung mithilfe der Azure-APIs ändern.

Während der Partitionen ein Daten Organisation Konzept sind, sind Durchsatz Einheiten rein ein Kapazität Konzept an. Durchsatz Einheiten pro Stunde in Rechnung gestellt werden und erworbenen sind. Nachdem Sie erworben haben, sind Durchsatz Einheiten für mindestens eine Stunde in Rechnung gestellt. Einheiten können bis zu 20 Durchsatz für einen Namespace Ereignis Hubs erworben werden, und es wird ein Azure-Konto maximal 20 Durchsatz Einheiten. Diese Durchsatz Einheiten werden von allen Ereignis Hubs in einem bestimmten Namespace gemeinsam genutzt.

Durchsatz Einheiten werden nach der Bereitstellung auf Basis optimale Leistung und immer möglicherweise nicht sofort kaufen. Wenn Sie eine bestimmte Kapazität benötigen, empfiehlt es sich, dass Sie diese Durchsatz Einheiten im Voraus kaufen. Wenn Sie mehr als 20 Durchsatz Einheiten benötigen, können Sie Azure Support, um weitere Durchsatz Einheiten auf Basis Zusicherung in Blöcken von 20, bis zu den ersten 100 Durchsatz Einheiten kaufen kontaktieren. Darüber hinaus können Sie auch Blöcke mit 100 Durchsatz Einheiten kaufen.

Es wird empfohlen, sorgfältig abzuwägen Durchsatz Einheiten und Partitionen, um optimale Skala mit Ereignis Hubs zu erzielen. Eine einzelne Partition hat die maximale Dezimalstellen eine Durchsatz Einheit. Die Anzahl der Einheiten Durchsatz sollten kleiner oder gleich der Anzahl von Partitionen in einem Hub Ereignis.

Preisinformationen finden Sie [Ereignis Hubs Preise](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="publisher-policy"></a>Publisherrichtlinie

Ereignis Hubs ermöglicht präzise steuern, Ereignisherausgeber durch *Richtlinien für Publisher*. Publisher-Richtlinien sind eine Reihe von Laufzeit-Funktionen, die großen Anzahl von unabhängigen Ereignisherausgeber zu erleichtern. Mit Publisher-Richtlinien verwendet jede Publisher den eigenen eindeutigen Bezeichner aus, wenn Ereignisse auf ein Ereignis-Hub mit den folgenden Verfahren zu veröffentlichen:

    //<my namespace>.servicebus.windows.net/<event hub name>/publishers/<my publisher name>

Sie müssen nicht Publisher Namen im Voraus erstellen, aber müssen sie das SAS Token verwendet, wenn ein Ereignis veröffentlicht unabhängige Publisher Identitäten Gewährleistung übereinstimmen. Weitere Informationen zu SAS finden Sie unter [Freigegebene Access Signatur Authentifizierung mit Dienstbus](../service-bus-messaging/service-bus-shared-access-signature-authentication.md). Bei der Verwendung von Publisher-Richtlinien wird der Wert **PartitionKey** auf dem Namen des Herausgebers festgelegt. Wenn Sie ordnungsgemäß funktioniert, müssen diese Werte übereinstimmen.

## <a name="summary"></a>Zusammenfassung

Azure Ereignis Hubs bietet eine hyper-Skala Ereignis und die telemetrieprotokoll Verarbeitung Dienst, die häufig bei einer beliebigen Skalierung Überwachung Anwendung und Benutzer Workflows verwendet werden kann. Die Möglichkeit zu bieten Veröffentlichen-Abonnieren von Funktionen, die mit geringer Wartezeit und bei umfangreichen, Ereignis Hub dient als den "auf"Verlauf für Big Data. Mit Publisher-basierten Identität und Sperrung Listen sind diese Funktionen in häufige Internet der Dinge Szenarien erweitert. Weitere Informationen zum Entwickeln von Applications Ereignis Hubs finden Sie unter des [Ereignisses Hubs programming Guide](event-hubs-programming-guide.md).

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie Informationen über Ereignis Hubs Konzepte erhalten haben, können Sie in den folgenden Szenarien verschieben.

- Erste Schritte mit einer [Veranstaltung Hubs Lernprogramm].
- Eine vollständige [Beispiel-Anwendung, die Ereignis Hubs verwendet].

[Azure klassischen-portal]: http://manage.windowsazure.com
[Ereignis Hubs Lernprogramm]: event-hubs-csharp-ephcs-getstarted.md
[Beispiel-Anwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Event-Hub-286fd097
