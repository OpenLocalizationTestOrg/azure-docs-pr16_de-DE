<properties 
    pageTitle="AMQP 1.0 Azure Service Bus und Ereignis Hubs Protokoll Leitfaden | Microsoft Azure" 
    description="Protokoll-Leitfaden für Ausdrücke und eine Beschreibung der AMQP 1.0 in Azure-Dienstbus und Ereignis Hubs" 
    services="service-bus,event-hubs" 
    documentationCenter=".net" 
    authors="clemensv" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="07/01/2016"
    ms.author="clemensv;jotaub;hillaryc;sethm"/>

# <a name="amqp-10-in-azure-service-bus-and-event-hubs-protocol-guide"></a>AMQP 1.0 Azure-Dienstbus und Ereignis Hubs Protocol-Leitfaden

Erweiterte Nachricht einfügen in die Warteschlange Protokoll 1.0 ist ein standardisierter Rahmung und Transfer Protokoll für asynchrone und zuverlässig übertragen von Nachrichten zwischen zwei Seiten. Es ist das primäre Protokoll Azure Service Bus Messaging und Azure Ereignis Hubs. Beide Dienste unterstützen auch HTTPS. Das eigene SBMP Protokoll, das ebenfalls unterstützt wird ist zugunsten AMQP schrittweise mehr verwendet.

AMQP 1.0 ist das Ergebnis der Zusammenarbeit zahlreicher Anbieter, die zusammengeführt Middlewarehersteller wie Microsoft und Red Hat, mit vielen messaging Middleware Benutzern, wie z. B. JP Morgan Chase Finanzdienstleister darstellt. Das technische Standardisierung-Forum für die AMQP-Protokoll und eine Durchwahl Spezifikationen ist OASIS, und es formeller Genehmigung als internationale Standard als ISO/IEC 19494 erreicht.

## <a name="goals"></a>Ziele

Dieser Artikel enthält eine Übersicht über die grundlegenden Konzepte des AMQP 1.0 Specification sowie eine kleine Gruppe von Spezifikationen für eine Durchwahl, die derzeit in der Fachausschuß OASIS AMQP Beendigung erfolgt per kurz und wird erläutert, wie Azure-Dienstbus implementiert, und klicken Sie auf diese Spezifikationen erstellt.

Das Ziel ist für alle Entwickler, der alle vorhandenen AMQP 1.0 Clientstapel auf einer beliebigen Plattform mit Azure-Dienstbus über AMQP 1.0 interagieren können.

Allgemeine Stapel allgemeine AMQP 1.0, z. B. Apache Proton oder AMQP.NET Lite, implementieren bereits alle Core AMQP 1.0 Gesten. Diese grundlegenden Gesten werden manchmal mit einer höheren Ebene API umschlossen; Apache Proton bietet auch mit zwei, die unbedingt Messenger-API und die reaktivieren Reaktor-API.

In der folgenden Erläuterung wird davon ausgegangen, dass die Verwaltung von AMQP Verbindungen, Sitzungen, und Links zur Verwendung von Rahmen übertragen und strömungsregelung behandelt werden, indem Sie den entsprechenden Stapel (z. B. Apache Proton-C) und viel, wenn ein bestimmter Aufmerksamkeit vom Entwickler nicht erforderlich. Angenommen das Vorhandensein wenigen API einfaches Compilerfreaks wie die Möglichkeit, die Verbindung herzustellen, und eine Form von *Absender* und *Empfänger* Abstraktion Objekte zu erstellen, die dann einige Form haben `send()` und `receive()` Vorgänge, Hilfethemas.

Wenn Sie erweiterte Funktionen von Azure-Dienstbus, wie etwa Nachricht durchsuchen oder die Verwaltung von Sitzungen, besprochen werden die werden in AMQP Ausdrücke, sondern auch als überlagerte bezeichnet Implementierung auf diese angenommenen API Abstraktion erläutert.

## <a name="what-is-amqp"></a>Was ist AMQP?

AMQP ist eine Rahmung und Transfer Protocol. Rahmung bedeutet, dass es Struktur für Datenstreams binäre enthält, die in eine beliebige Richtung Netzwerk-Verbindung Datenfluss. Die Struktur bietet Grundkonzepte für distinct Datenblöcke – Frames – zwischen den verbundenen Parteien ausgetauscht werden. Die Übertragung Funktionen stellen Sie sicher, dass beide kommunizieren Teilnehmer einer freigegebenen Grundlegendes zu zu Wenn Frames weitergeleitet werden müssen, und wann Übertragung abgeschlossen gilt herstellen können.

Im Gegensatz zu einer früheren Version abgelaufenen Entwurfsversionen von der Arbeitsgruppe AMQP gefertigt, die weiterhin verwenden, indem Sie ein paar Nachrichtenmakler haben, wird der Arbeitsgruppe abgeschlossen und standardisierte AMQP 1.0-Protokoll nicht das Vorhandensein einer Nachricht Bank oder eine bestimmte Suchtopologie für Personen innerhalb einer Nachricht Bank vorschreiben.

Das Protokoll kann für die symmetrischen Peer-to-Peer-Kommunikation für die Interaktion mit Nachrichtenmakler verwendet werden, die Warteschlangen und Veröffentlichen/Abonnieren Elemente wie unterstützen Azure Service Bus. Sie können auch verwendet werden für die Interaktion mit messaging-Infrastruktur, unterscheiden sich die Interaktion Muster von regulären Warteschlangen, wie bei Azure Ereignis Hubs der Fall ist. Ein Ereignis Hub verhält sich wie eine Warteschlange, wenn Ereignisse gesendet werden, aber verhält eher wie eine serielle Speicherdienst, wenn Ereignisse gelesen werden. Es ähnelt etwas Laufwerk ein. Der Client wählt einen Offset in den verfügbaren Datenstream und dann alle Ereignisse aus dieser Versatz in die neuesten unterstützt wird.

Das Protokoll AMQP 1.0 ist grundsätzlich erweiterbar, gleicht weiteren Spezifikationen zur Verbesserung seiner Funktionen. Die drei Erweiterung Spezifikationen, die in diesem Dokument besprochen wird verdeutlichen dies. Für die Kommunikation über vorhandene HTTPS/WebSockets Infrastruktur, Konfigurieren der systemeigenen AMQP TCP Ports schwierig sein kann, definiert Importspezifikation Bindung, wie AMQP über WebSockets layer. Für die Interaktion mit der messaging-Infrastruktur für die Verwaltung oder eine erweiterte Funktionalität Weise Anforderung/Antwort, definiert die AMQP Management-Spezifikation die erforderlichen grundlegende Interaktion primitive. Für die Integration von partnerverbundkontakte Autorisierung Modell definiert die AMQP Ansprüche-basierte Sicherheit Spezifikation zum Zuordnen und erneuern Autorisierung Token Links zugeordnet.

## <a name="basic-amqp-scenarios"></a>Grundlegende AMQP Szenarien

In diesem Abschnitt erläutert die grundlegende Verwendung des AMQP 1.0 mit Azure-Dienstbus, wozu auch Verbindungen, Sitzungen und Links erstellen und Übertragen von Nachrichten an und von Dienstbus Elemente wie Warteschlangen, Themen und Abonnements.

Die wichtigsten autorisierende Quelle zu erfahren Sie mehr über die Funktionsweise der AMQP ist der AMQP 1.0-Spezifikation, aber die Spezifikation geschrieben wurde, exakt Implementierung Leitfaden und nicht um das Protokoll zu vermitteln. In diesem Abschnitt Schwerpunkt auf Einführung in so viele Terminologie Bedarf für die Beschreibung wie Dienstbus AMQP 1.0 verwendet. Für eine umfassendere Einführung AMQP sowie eine breitere Beschreibung von AMQP 1.0 können Sie [diesen Kurs video][]überprüfen.

### <a name="connections-and-sessions"></a>Verbindungen und Sitzungen

![][1]

AMQP Ruft die kommunizieren Programme *Container*; die enthalten *Knoten*, welche die kommunizieren Elemente innerhalb dieser Container sind. Eine Warteschlange kann ein solcher Knoten sein. AMQP ermöglicht multiplexing, damit eine einzelne Verbindung für viele Kommunikationspfade zwischen Knoten verwendet werden kann. ein Anwendungsclient kann beispielsweise gleichzeitig aus einer Warteschlange empfangen und an eine andere Warteschlange über dieselbe Netzwerkverbindung senden.

Die Netzwerkverbindung ist für den Container somit verankert. Es wird durch den Container, in der Sie die Empfänger Rolle, die überwacht und eingehende TCP-Verbindungen akzeptiert eine ausgehende TCP-Sockets-Verbindung zu einem Container vornehmen Clientrolle initiiert. Die Verbindungshandshake enthält das Protokoll, Version, deklarieren oder die Verwendung der Sicherheit auf Benutzerebene Transport (SSL/TLS) und eine Authentifizierung/Autorisierung Handshake bei der Verbindungsbereich, der auf SASL basiert aushandeln aushandeln.

Azure Dienstbus erfordert die Verwendung von TLS jederzeit an. Sie Verbindungen Ports 5671, unterstützt, bei dem die TCP-Verbindung zuerst mit TLS überlagert vor dem Eingeben des AMQP Protokoll Handshakes und unterstützt auch Verbindungen Ports 5672, bei denen der Server sofort eine obligatorische Aktualisierung der Verbindung TLS bietet mithilfe des Modells AMQP verschrieben. Die AMQP WebSockets Bindung erstellt einen Tunnel Ports 443, die dann AMQP 5671 Verbindungen entspricht.

Nach dem Einrichten der Verbindung und TLS aus, bietet Dienstbus zwei SASL Verfahren Optionen:

-   NUR SASL wird häufig verwendet, für die Übergabe von Anmeldeinformationen für Benutzername und Kennwort auf einem Server. Dienstbus hat keinen Konten, aber benannte [Zugriffsschutz freigegeben Regeln](service-bus-shared-access-signature-authentication.md), die Verwaltung von Informationsrechten eröffnen und einen Schlüssel zugeordnet sind. Der Name einer Regel als den Benutzernamen und die Taste verwendet wird (wie base64 Text codierte) als das Kennwort verwendet wird. Die Rechte der ausgewählten Regel zugeordneten Aufsicht, die für die Verbindung zulässigen Vorgänge.

-   ANONYME SASL wird zum umgehen SASL Autorisierung aus, wenn der Client möchte Modell Ansprüche-basierte Sicherheit (CBS) verwenden, das später beschrieben wird verwendet. Mit dieser Option kann eine Client-Verbindung anonym hergestellt werden für kurze Zeit während der Client kann nur mit den Endpunkt CBS interagieren und der Handshake CBS ausführen muss.

Nachdem die Transport-Verbindung hergestellt wurde, wird im Container deklarieren die maximale Rahmengröße sie sind bereit, verarbeitet und nach der im Leerlauf Timeout werden diese einseitig ist keine Aktivität vorhanden, klicken Sie auf die Verbindung ist trennen.

Diese manuell deklarieren, wie viele gleichzeitig Kanäle unterstützt werden. Bei einem Kanal handelt es sich um einen Übertragungspfad unidirektionale, ausgehende, virtuelle über die Verbindung. Eine Sitzung hat einen Kanal aus jeder der verbundener Container einen bidirektionale Kommunikationspfad bilden.

Sitzungen haben ein Fenster-basierten Fluss Steuerelementmodell; Beim Erstellen eine Sitzung für jede Partei deklariert wie viele Frames in annehmen ist seine Fenster erhalten. Wie die Parteien Exchange Frames übertragenen Frames Füllung, die Fenster und übertragen zu beenden, und wenn das Fenster voll ist, bis das Fenster zurücksetzen oder erweiterten mit den *Datenfluss performative* erhält (*performative* wird auf Protokollstufe Gesten zwischen den beiden Parteien ausgetauscht AMQP bezeichnet).

Dieses Modell basierenden Fenster entspricht in etwa das TCP-Konzept der Fenster-basierten strömungsregelung, aber innerhalb des Sockets Ebene der Sitzung. Das Protokoll des Konzepts der gleicht für mehrere gleichzeitige Sitzungen vorhanden ist, sodass die hohe Priorität Datenverkehr ältere reduzierten normalen Datenverkehr, wie auf einen Autobahnen express Verantwortlichkeitsbereich Beipackzettel werden konnte.

Azure Service Bus verwendet aktuell genau eine Sitzung für jede Verbindung. Die Dienstbus maximale-Rahmengröße ist 262.144 Byte (256K Byte) für den Dienst Bus Standard- und Hubs Ereignis. Es ist 1.048.576 (1 MB) für Service Bus Premium. Dienstbus keine bestimmten Sitzung Ebene Drosselung Windows vorgangseinschränkung, setzt aber im Fenster regelmäßig als Teil der Link Ebene strömungsregelung (finden Sie [im nächsten Abschnitt](#links)).

Verbindungen, Kanäle und Sitzungen sind temporärer. Wenn die zugrunde liegende Verbindung Verbindungen, reduziert müssen TLS-Tunnel, SASL Autorisierungskontext und Sitzungen wiederhergestellt werden.

### <a name="links"></a>Links

![][2]

AMQP übermittelt Nachrichten über die Links. Ein Link handelt es sich um eine Kommunikationspfad über eine Sitzung, die Übertragung von Nachrichten in eine Richtung ermöglicht erstellt. die Übertragung Status Aushandlung befindet sich über den Link und bidirektionale zwischen den verbundenen Parteien.

Links können durch entweder Container erstellt werden, zu einem beliebigen Zeitpunkt und für eine vorhandene Sitzung, wodurch AMQP viele andere Protokolle, einschließlich HTTP und MQTT, wo finde ich die Einleitung der Übertragung und durchstellen Pfad einer Rechte ausschließlich der Herstellen einer Socketverbindung Partei abweicht.

Der Link initiieren Container gefragt werden, den gegenüberliegenden Container, um einen Link zu akzeptieren und sie wählt eine Rolle von Absender oder Empfänger. Daher, entweder auf Container initiieren kann, erstellen eine Richtung oder bidirektionale Kommunikationspfade mit letztere als Paare von Links erstellt.

Links werden mit dem Namen und Knoten zugeordnet. Wie in den Anfang erwähnt, sind Knoten kommunizieren Elemente innerhalb eines Containers aus.

Azure-Dienstbus ist ein Knoten eine Warteschlange, ein Thema, ein Abonnement oder einer Warteschlange für unzustellbare Nachrichten untergeordnete, einer Warteschlange oder das Abonnement direkt entspricht. Der Knotenname in AMQP wird daher die relative Name der Entität innerhalb der Dienstbus Namespace. Wenn eine Warteschlange **Myqueue**benannt wird, ist, die auch den Namen der AMQP Knoten. Ein Thema Abonnement liegt der Messe HTTP-API durch, die in einer "Abonnements" Ressource Websitesammlung und auf diese Weise ein Abonnement **Sub** sortiert wird, oder ein Thema **Mytopic** hat die AMQP Knoten Namen **Mytopic/Abonnements/Sub**.

Der verbindende Client auch erforderlich, um einen lokalen Knotennamen zum Erstellen von Links zu verwenden. Dienstbus steht kein ausführliche Informationen zu diesen Knotennamen und nicht interpretieren. AMQP 1.0-Client Stapel verwenden in der Regel ein Schema um zu gewährleisten, die in den Bereich des Clients diese temporärer Knotennamen eindeutig sind.

### <a name="transfers"></a>Übertragen

![][3]

Nachdem Sie eine Verknüpfung hergestellt wurde, können Nachrichten über diese Verknüpfung übertragen. In AMQP eine Übertragung mit einer Bewegung explizite Protokoll (die *Übertragung* performative) ausgeführt, die über einen Link an die Empfänger eine Nachricht vom Absender verschiebt. Eine Übertragung ist abgeschlossen, wenn sie "ausgeglichen", d. h., beide Teilnehmer einen Überblick über das Ergebnis dieser durchstellen freigegebenen eingerichtet haben.

Im einfachsten Fall kann der Absender senden auswählen, um Nachrichten "vorab ausgeglichen", was bedeutet, dass der Client ist nicht das Ergebnis interessiert und des Empfängers werden keine Feedback über das Ergebnis des Vorgangs. In diesem Modus von Azure-Dienstbus Ebene der AMQP-Protokoll unterstützt, aber nicht in einem Client-API verfügbar gemacht wurde.

Normale zutrifft, Nachrichten nicht ausgeglichene gesendet werden werden, und der Empfänger wird dann Annahme oder Ablehnung mithilfe der *Anordnung* performative anzugeben. Ablehnung tritt auf, wenn der Empfänger die Nachricht aus irgendeinem Grund nicht annehmen kann und die Ablehnungsnachricht enthält Informationen zu den Grund, also ein Fehler Struktur von AMQP definiert. Wenn Nachrichten aufgrund interner Fehler innerhalb Azure-Dienstbus abgelehnt werden, gibt der Dienst zusätzlichen Informationen in die Struktur, die zum Bereitstellen von Hinweisen Diagnose von Supportmitarbeiter, wenn Sie Kundendienstanfragen Ablage sind verwendet werden kann. Weitere Details zu Fehlern lernen später Sie.

Eine spezielle Form von Ablehnung ist im Zustand *freigegeben* , gibt an, dass der Empfänger enthält, keine technische Einwände gegen die Übertragung, aber auch keine Zinsen in die Übertragung aus. Die Anfrage angenommen, wenn eine Nachricht vorhanden ist, ist bei einem Dienstbus Client übermittelt, und der Client wählt die Meldung "Abbruch", da es nicht verarbeiten der Nachricht selbst Nachrichtenübermittlung ist zwar nicht vorsätzlich infolge Arbeit ausführen kann. Eine Variation dieses Staates ist im Zustand *geändert* , wodurch Änderungen an der Nachricht nach der Veröffentlichung an. Dieser Status wird zurzeit nicht von Dienstbus verwendet.

Die AMQP 1.0-Spezifikation definiert eine weitere Anordnung Zustand *erhalten* , die speziell hindert Link Wiederherstellung behandeln. Link Wiederherstellung ermöglicht Rekonstruieren des Zustands des einen Link und alle ausstehenden Übermittlungen über eine neue Verbindung und Sitzung, wenn der vorherigen Verbindung und Sitzung verloren gegangen sind.

Link Wiederherstellung unterstützt Azure Service Bus nicht. Wenn der Client die Verbindung mit einer Nachricht nicht ausgeglichene zu Dienstbus verliert übertragen steht noch aus, die Nachrichtenübermittlung geht verloren und der Client schließen wieder, stellen die Verbindung wieder her, und führen Sie die Übertragung erneut muss.

Als solche unterstützen Azure-Dienstbus Ereignis Hubs "mindestens einmal" und übertragen, in dem der Absender der Nachricht gespeichert und akzeptiert wurden Probleme sicher sein, jedoch werden nicht unterstützt "einmalig" Übertragung AMQP auf oberster Ebene, in dem das System versucht, auf den Link wiederherstellen, und fahren Sie mit den Übermittlung Zustand zur Vermeidung von Duplikaten der Übertragung der Nachricht handeln.

Für zukommen lassen Mögliches Duplikat sendet, Azure-Dienstbus Erkennung von Duplikaten als ein optionales Feature Warteschlangen und Themen unterstützt. Doppelte Datensätze Erkennung die Nachricht IDs aller eingehenden Nachrichten während einer User defined Zeitfensters, und legen Sie alle Nachrichten mit der gleichen Nachricht-IDs gesendet werden, während das gleiche Fenster im Hintergrund ab.

### <a name="flow-control"></a>Strömungsregelung

![][4]

Über das Sitzung Ebene Fluss-Modell, die noch beschrieben, weist jede Verknüpfung ein eigenes Fluss Steuerelementmodell. Sitzung Ebene strömungsregelung verhindert, dass den Container nachdem strömungsregelung Link Ebene die Anwendung Computersysteme wie viele Nachrichten verschoben, die über einen Link verarbeitet werden sollen, und wenn zu viele Frames am behandelt werden müssen.

Klicken Sie auf einen Link können Übertragung nur passieren, wenn der Absender hat genügend "link Kreditkarte". Link Kreditkarte handelt es sich um einen Indikator vom Empfänger *Fluss* performative, mit denen ein Link ausgelegte ist festlegen. Wenn und während des Absenders Link Kreditkarte zugeordnet ist, wird versucht, die Kreditkarte durch die Bereitstellung von Nachrichten verwenden. Jede Nachricht Übermittlung verringert der verbleibende Link Gutschrift mit einem. Wenn der Link Kredit von verwendet wird, beenden Lieferungen aus.

Wenn Dienstbus in die Rolle des Empfängers ist wird diese Absender sofort mit Kreditkarte ausreichend Link, bereitstellen, damit Nachrichten sofort gesendet werden können. Als Link Kreditkarte verwendet wird, wird Dienstbus gelegentlich einen *Fluss* performative an den Absender auf den Link Kreditkarte Saldo aktualisieren senden.

In der Rolle des Absenders sendet Dienstbus sorgfältig Nachrichten, um alle ausstehenden Link Kreditkarte verwenden.

Ein Anruf "erhalten" auf der Ebene API in einen *Fluss* performative mit Dienst gesendet werden, indem der Client übersetzt und Dienstbus wird die Kreditkarte nutzen, indem Sie die erste verfügbare, nicht gesperrte Nachricht aus der Warteschlange aufzeichnen, Sperren und ihn übertragen. Es ist keine Meldung für die Übermittlung verfügbar, hergestellt mit einem ausstehenden Kredit, der durch eine OLE-Verknüpfung, dass bestimmte Entität in der Reihenfolge der verspäteten aufgezeichneten bleibt und Nachrichten werden gesperrt und übertragen, sobald sie mit einem ausstehenden Kredit verfügbar sind.

Wenn die Übertragung in eines der terminal Staaten *angenommen*, *abgelehnt*oder *freigegeben*ausgeglichen wurde, wird die Sperre für eine Nachricht freigegeben. Die Nachricht wird aus Dienstbus entfernt, wenn der terminal Status *akzeptiert*wird. Es verbleibt im Dienstbus und an den nächsten Empfänger übermittelt wird, wenn die Übertragung eine der anderen Zustände erreicht. Dienstbus wird automatisch die Nachricht in der Entität für unzustellbare Nachrichten Warteschlange verschieben, wenn die für die Entität aufgrund von wiederholten abgelehnte oder Versionen zulässige maximale Übermittlung zählen erreicht.

Obwohl die offizielle Service Bus APIs nicht direkt solche eine Option heute offen legen, können ein Low-Level AMQP Protokollclient Link-Kreditkarte Modell zum Aktivieren der Interaktion "Abruf-Schreibweise" eine Einheit des Kredits für jede Anforderung empfangen in einem Datenmodell "Pushbenachrichtigungen-Schreibweise" ausgeben, indem Sie eine große Anzahl von Link Gutschriften und dann Nachrichten erhalten, sobald sie ohne weiteren Eingriff verfügbar sind. Pushbenachrichtigungen wird durch die eigenschafteneinstellungen [MessagingFactory.PrefetchCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.prefetchcount.aspx) oder [MessageReceiver.PrefetchCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.prefetchcount.aspx) unterstützt. Wenn sie ungleich 0 sind, von der Client AMQP als Link Kredits verwendet.

In diesem Zusammenhang ist es wichtig, zu verstehen, dass die Uhr für den Ablauf der Sperre für die Nachricht in der Entität wird gestartet, wenn die Nachricht stammt, von der Entität und nicht, wenn die Nachricht bei der Übertragung setzen wird ist. Immer, wenn der Client Readiness Nachrichten erhalten, indem Sie Link Kreditkarte zeigt an, ist es daher erwartet werden aktiv Nachrichten im Netzwerk herausziehen und bereit sind, diese verarbeitet werden. Andernfalls möglicherweise die Nachricht Sperre abgelaufen sind, bevor Sie die Nachricht auch übermittelt wird. Die Verwendung von Link-Kreditkarte strömungsregelung sollten die sofortige Bereitschaft für den Umgang mit verfügbaren Nachrichten an den Empfänger versandt direkt entsprechen.

Im Überblick bieten in den folgenden Abschnitten Schema einen Überblick über die performative Fluss während andere API Interaktionen an. Jeder Abschnitt beschreibt eine andere logische Operation an. Einige dieser Interaktionen möglicherweise "aus-", was bedeutet, dass sie nur ausgeführt werden möglicherweise einmal erforderlich. Erstellen den Absender einer Nachricht kann eine Netzwerkinteraktion nicht verursachen, bis die erste Nachricht gesendet oder angefordert wird.

Die Pfeile eingeblendet werden performative seine Richtung.

#### <a name="create-message-receiver"></a>Erstellen der Nachrichtenempfänger

| Client                                                                                                                                                | Dienstbus                                                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------------------------------------    |--------------------------------------------------------------------------------------------------------------------------------------------   |
| --> Anfügen ()<br/>Name = {Link Name,}<br/>Behandeln = {numerische Ziehpunkt}<br/>Rolle =**Empfänger**,<br/>Quelle = {Entitätsnamen}<br/>Target = {Client Link-Id}<br/>)         | Client hängt an Entität als Empfänger                                                                                                         |
| Ende der Link anfügen Dienstbus-Antworten                                                                                                     | < – anfügen ()<br/>Name = {Link Name,}<br/>Behandeln = {numerische Ziehpunkt}<br/>Rolle =**Absender**<br/>Quelle = {Entitätsnamen}<br/>Target = {Client Link-Id}<br/>)       |

#### <a name="create-message-sender"></a>Erstellen der Nachrichtenabsender

| Client                                                                                                            | Dienstbus                                                                                                           |
|------------------------------------------------------------------------------------------------------------------ |--------------------------------------------------------------------------------------------------------------------   |
| --> Anfügen ()<br/>Name = {Link Name,}<br/>Behandeln = {numerische Ziehpunkt}<br/>Rolle =**Absender**<br/>Quelle = {Client Link-Id}<br/>Target = {Entitätsname}<br/>)   | Keine Aktion                                                                                                                     |
| Keine Aktion                                                                                                                 | < – anfügen ()<br/>Name = {Link Name,}<br/>Behandeln = {numerische Ziehpunkt}<br/>Rolle =**Empfänger**,<br/>Quelle = {Client Link-Id}<br/>Target = {Entitätsname}<br/>)     |

#### <a name="create-message-sender-error"></a>Erstellen der Absender der Nachricht (Fehler)

| Client                                                                                                            | Dienstbus                                                           |
|------------------------------------------------------------------------------------------------------------------ |---------------------------------------------------------------------  |
| --> Anfügen ()<br/>Name = {Link Name,}<br/>Behandeln = {numerische Ziehpunkt}<br/>Rolle =**Absender**<br/>Quelle = {Client Link-Id}<br/>Target = {Entitätsname}<br/>)   | Keine Aktion                                                                     |
| Keine Aktion                                                                                                                 | < – anfügen ()<br/>Name = {Link Name,}<br/>Behandeln = {numerische Ziehpunkt}<br/>Rolle =**Empfänger**,<br/>Quelle = Null,<br/>Target = Null<br/>)<br/><br/><--trennen ()<br/>Behandeln = {numerische Ziehpunkt}<br/>geschlossen =**Wahr**,<br/>Fehler = {Fehlerinformationen}<br/>)  |

#### <a name="close-message-receiversender"></a>Schließen Absender/Empfänger

| Client                                            | Dienstbus                                       |
|-------------------------------------------------  |-------------------------------------------------  |
| --> trennen ()<br/>Behandeln = {numerische Ziehpunkt}<br/>geschlossen =**Wahr**<br/>)    | Keine Aktion                                                 |
| Keine Aktion                                                 | <--trennen ()<br/>Behandeln = {numerische Ziehpunkt}<br/>geschlossen =**Wahr**<br/>)    |

#### <a name="send-success"></a>Senden (Erfolg)

| Client                                                                                                                        | Dienstbus                                                                                           |
|------------------------------------------------------------------------------------------------------------------------------ |------------------------------------------------------------------------------------------------------ |
| --> durchstellen (<br/>Übermittlung-Id = {numerische Ziehpunkt}<br/>Übermittlung-Tag = {binäre Ziehpunkt}<br/>ausgeglichen =**falsch**,, mehr =**falsch**<br/>Bundesstaat =**null**,<br/>Lebenslauf =**falsch**<br/>)   | Keine Aktion                                                                                                     |
| Keine Aktion                                                                                                                             | <--(Anordnung<br/>Rolle = Empfänger,<br/>zuerst = {Übermittlung-Id}<br/>Letzte = {Übermittlung-Id}<br/>ausgeglichen =**Wahr**,<br/>Bundesstaat =**akzeptiert**<br/>)   |

#### <a name="send-error"></a>Senden (Fehler)

| Client                                                                                                                        | Dienstbus                                                                                                                   |
|------------------------------------------------------------------------------------------------------------------------------ |-----------------------------------------------------------------------------------------------------------------------------  |
| --> durchstellen (<br/>Übermittlung-Id = {numerische Ziehpunkt}<br/>Übermittlung-Tag = {binäre Ziehpunkt}<br/>ausgeglichen =**falsch**,, mehr =**falsch**<br/>Bundesstaat =**null**,<br/>Lebenslauf =**falsch**<br/>)   | Keine Aktion                                                                                                                             |
| Keine Aktion                                                                                                                             | <--(Anordnung<br/>Rolle = Empfänger,<br/>zuerst = {Übermittlung-Id}<br/>Letzte = {Übermittlung-Id}<br/>ausgeglichen =**Wahr**,<br/>Bundesstaat = (**abgelehnt**<br/>Fehler = {Fehlerinformationen}<br/>)<br/>)     |

#### <a name="receive"></a>Empfangen

| Client                                                                                                | Dienstbus                                                                                                                   |
|------------------------------------------------------------------------------------------------------ |------------------------------------------------------------------------------------------------------------------------------ |
| --> Fluss (<br/>Link-Kreditkarte = 1<br/>)                                                                                 | Keine Aktion                                                                                                                             |
| Keine Aktion                                                                                                     | < übertragen ()<br/>Übermittlung-Id = {numerische Ziehpunkt}<br/>Übermittlung-Tag = {binäre Ziehpunkt}<br/>ausgeglichen =**falsch**<br/>Weitere =**falsch**<br/>Bundesstaat =**null**,<br/>Lebenslauf =**falsch**<br/>)     |
| --> Anordnung (<br/>Rolle =**Empfänger**,<br/>zuerst = {Übermittlung-Id}<br/>Letzte = {Übermittlung-Id}<br/>ausgeglichen =**Wahr**,<br/>Bundesstaat =**akzeptiert**<br/>)   | Keine Aktion                                                                                                                             |

#### <a name="multi-message-receive"></a>Mit mehreren Nachrichten erhalten

| Client                                                                                                    | Dienstbus                                                                                                                       |
|--------------------------------------------------------------------------------------------------------   |--------------------------------------------------------------------------------------------------------------------------------   |
| --> Fluss (<br/>Link-Kreditkarte = 3<br/>)                                                                                 | Keine Aktion                                                                                                                                 |
| Keine Aktion                                                                                                         | < übertragen ()<br/>Übermittlung-Id = {numerische Ziehpunkt}<br/>Übermittlung-Tag = {binäre Ziehpunkt}<br/>ausgeglichen =**falsch**<br/>Weitere =**falsch**<br/>Bundesstaat =**null**,<br/>Lebenslauf =**falsch**<br/>)     |
| Keine Aktion                                                                                                         | < übertragen ()<br/>Übermittlung-Id = {numerische Ziehpunkt + 1}<br/>Übermittlung-Tag = {binäre Ziehpunkt}<br/>ausgeglichen =**falsch**<br/>Weitere =**falsch**<br/>Bundesstaat =**null**,<br/>Lebenslauf =**falsch**<br/>)   |
| Keine Aktion                                                                                                         | < übertragen ()<br/>Übermittlung-Id = {numerische Ziehpunkt + 2}<br/>Übermittlung-Tag = {binäre Ziehpunkt}<br/>ausgeglichen =**falsch**<br/>Weitere =**falsch**<br/>Bundesstaat =**null**,<br/>Lebenslauf =**falsch**<br/>)   |
| --> Anordnung (<br/>Rolle = Empfänger,<br/>zuerst = {Übermittlung-Id}<br/>Letzte = {Übermittlung Id + 2}<br/>ausgeglichen =**Wahr**,<br/>Bundesstaat =**akzeptiert**<br/>)     | Keine Aktion                                                                                                                                 |

### <a name="messages"></a>Nachrichten

In den folgenden Abschnitten wird erläutert, welche Eigenschaften aus den standardmäßigen AMQP Nachricht Abschnitten von Dienstbus verwendet werden und wie diese die offizielle Service Bus APIs zugeordnet.

#### <a name="header"></a>Kopfzeile

| Feldname        | Verwendung                             | Name der API          |
|----------------   |-------------------------------    |---------------    |
| Dauerhaftes           | -                                 | -                 |
| Priorität          | -                                 | -                 |
| TTL               | Gültigkeitsdauer für diese Meldung     | [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)     |
| erste-Erwerber    | -                                 | -                 |
| Count-Übermittlung    | -                                 | [DeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.deliverycount.aspx)   |

#### <a name="properties"></a>Eigenschaften

| Feldname            | Verwendung                                                                                                                             | Name der API                                      |
|---------------------- |---------------------------------------------------------------------------------------------------------------------------------  |--------------------------------------------   |
| Nachrichten-id            | Anwendungsspezifische, Freiform-Bezeichner für diese Nachricht. Zur Erkennung von Duplikaten verwendet.                                         | [Nachrichten-ID](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx)                                   |
| Benutzer-id               | Anwendungsspezifische Benutzer-ID, nicht vom Dienstbus interpretiert.                                                              | Der Dienst Bus-API nicht verfügbar.   |
| An                    | Anwendungsspezifische Zielbezeichner, die nicht von Dienstbus interpretiert.                                                       | [An](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.to.aspx)                                             |
| Betreff               | Anwendungsspezifische Zweck Bezeichner für Nachrichten, nicht vom Dienstbus interpretiert.                                                   | [Beschriftung](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)                                       |
| Antwort an              | Anwendungsspezifische Antwort-Path-Indikator, nicht vom Dienstbus interpretiert.                                                         | [ReplyTo](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.replyto.aspx)                                       |
| Korrelations-id        | Anwendungsspezifische Korrelations-ID, nicht vom Dienstbus interpretiert.                                                       | [CorrelationId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.correlationid.aspx)                               |
| Inhaltstyp          | Anwendungsspezifische Inhaltstyp Indikator für den Textkörper, die nicht von Dienstbus interpretiert.                                          | [ContentType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx)                                   |
| Inhalt-Codierung      | Anwendungsspezifische Inhalt Codierung Indikator für den Textbereich nicht vom Dienstbus interpretiert.                                      | Der Dienst Bus-API nicht verfügbar.   |
| Absolute-Ablauf-time  | Bei der Absolute Chatnachrichten die Nachricht abläuft, deklariert. Bei der Eingabe ignoriert (Kopfzeile Ttl eingehalten wird), bei der Ausgabe autorisierende.   | [ExpiresAtUtc](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.expiresatutc.aspx)                                 |
| Zeitpunkt der Erstellung         | Deklariert zum Zeitpunkt der Erstellung der Nachricht. Nicht verwendeten Dienstbus                                                           | Der Dienst Bus-API nicht verfügbar.   |
| Gruppen-id              | Anwendungsspezifische Bezeichner für eine zusammengehörige Gruppe von Nachrichten. Für Dienstbus Sitzungen verwendet.                                      | [SessionId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx)                                   |
| Gruppe-Reihenfolge        | Identifizieren die relative Sequenz Anzahl der Nachricht innerhalb einer Sitzung zu begegnen. Durch Dienstbus ignoriert.                         | Der Dienst Bus-API nicht verfügbar.   |
| Antworten auf Gruppen-id     | -                                                                                                                                 | [ReplyToSessionId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.replytosessionid.aspx)                             |

## <a name="advanced-service-bus-capabilities"></a>Erweiterte Dienstbus-Funktionen

Dieser Abschnitt enthält Azure Service-erweiterte Funktionen, die auf Entwurf Erweiterungen zu aktuell für AMQP in dem Fachausschuß OASIS entwickelt AMQP basieren. Azure Dienstbus implementiert den aktuellen Status der folgenden Entwürfe und eingeführt werden, wie diese Entwürfe standard Status erreicht haben Änderungen übernehmen.

> [AZURE.NOTE] Vorstellen, ein Muster Anforderung/Antwort Service Bus Messaging erweiterte Vorgänge unterstützt werden. Die Details dieser Vorgänge werden im Dokument beschrieben [AMQP 1.0 in Dienstbus: Anforderung/Antwort-basierten Vorgänge](https://msdn.microsoft.com/library/azure/mt727956.aspx).

### <a name="amqp-management"></a>AMQP Verwaltung

Die AMQP Management-Spezifikation ist der erste der Entwurf Extensions hier erläutert. Diese Spezifikation definiert eine Reihe von überlagern das Protokoll AMQP Protokoll Gesten, die über AMQP Management Interaktionen mit der messaging-Infrastruktur zu ermöglichen. Die Spezifikation definiert generische JOIN-Operationen wie *Erstellen*, *Lesen*, *Aktualisieren*und *Löschen* für die Verwaltung von Personen innerhalb einer messaging-Infrastruktur und eine Reihe von Vorgängen Abfrage an.

Alle diese Gesten erfordern eine Anforderung/Antwort-Interaktion zwischen dem Client und der messaging-Infrastruktur und daher die Spezifikation definiert, wie das Interaktion Muster auf AMQP modellieren: der Client eine Verbindung mit der messaging-Infrastruktur, leitet eine Sitzung und erstellt dann ein Paar von Links. Klicken Sie auf einen Link der Client fungiert als Absender und andererseits fungiert als Empfänger verwendet werden, wodurch ein Paar von Links, die als bidirektionale Kanal dienen können.

| Logische Operation            | Client                      | Dienstbus                 |
|------------------------------|-----------------------------|-----------------------------|
| Pfad der Anforderung Antwort erstellen | --> Anfügen ()<br/>Name = {*Link Namen*}<br/>behandeln = {*numerische Ziehpunkt*}<br/>Rolle =**Absender**<br/>Quelle =**null**,<br/>Target = "Myentity / $Management"<br/>)                            |Keine Aktion                             |
|Pfad der Anforderung Antwort erstellen                              |Keine Aktion                             | \<– (Anfügen<br/>Name = {*Link Namen*}<br/>behandeln = {*numerische Ziehpunkt*}<br/>Rolle =**Empfänger**,<br/>Quelle = Null,<br/>Target = "Myentity"<br/>)                            |
|Pfad der Anforderung Antwort erstellen                              | --> Anfügen ()<br/>Name = {*Link Namen*}<br/>behandeln = {*numerische Ziehpunkt*}<br/>Rolle =**Empfänger**,<br/>Quelle = "Myentity / $Verwaltung"<br/>Target = "Myclient$-Id"<br/>)                            |                             |Keine Aktion
|Pfad der Anforderung Antwort erstellen                              |Keine Aktion                             | \<– (Anfügen<br/>Name = {*Link Namen*}<br/>behandeln = {*numerische Ziehpunkt*}<br/>Rolle =**Absender**<br/>Quelle = "Myentity",<br/>Target = "Myclient$-Id"<br/>)                            |

Probleme dieser Paar von Links in den Ort an, die Anforderung/Antwort-Implementierung ist einfach: eine Anforderung ist eine Nachricht, die mit einer Entität innerhalb der messaging-Infrastruktur, die diesem Muster versteht. Klicken Sie in die Anfrage-Nachricht wird das Feld *Antwort an* im Abschnitt *Eigenschaften* auf die *Ziel* -ID für den Link auf dem vorführen die Antwort festgelegt. Die Behandlung Entität wird die Anforderung nicht verarbeiten, und klicken Sie dann die Antwort vorführen, über den Link, dessen *Ziel* -ID die ID angegebene *Antwort-* übereinstimmt.

Das Muster erfordert offensichtlich, dass der Client Container und der Client generierte Bezeichner für das Ziel Antworten eindeutig über alle Clients und Sicherheitsgründen auch schwer Vorhersagen sind.

Der Nachrichtenaustausch verwendet, für das Protokoll Management und für alle anderen Protokolle, mit denen das gleiche Muster kommen Ebene der Anwendung. Definieren sie keine neue AMQP auf Protokollstufe Gesten. Die besteht beabsichtigt, damit Applikationen sofortige der folgenden Erweiterungen mit kompatiblen AMQP 1.0 Stapel nutzen können.

Azure Dienstbus aktuell implementiert keine der wichtigsten Features der Managementspezifikation, aber das Anforderung/Antwort Muster Managementspezifikation definiert ist für das Feature Ansprüche-basierte Sicherheit und für die erweiterten Funktionen, die in den folgenden Abschnitten besprochen wird nahezu alle grundlegenden.

### <a name="claims-based-authorization"></a>Anspruchsbasierte Autorisierung

Der AMQP Ansprüche Grundlage Autorisierung (CBS) Spezifikation Entwurf basiert auf der Managementspezifikation Anforderung/Antwort Muster und ein GRG Modell für die Verwendung von partnerverbundkontakte Sicherheitstokens mit AMQP beschreibt.

Standardmäßige Sicherheitsmodell der AMQP in die Einleitung erläuterten basiert auf SASL und die AMQP Verbindungshandshake integriert. Mit SASL hat den Vorteil, den es ein extensible Modell für die eine Reihe von Verfahren bietet aus, die alle Protokoll, das auf SASL formell leans profitieren kann definiert wurden. Sind Sie dem diese Verfahren "Nur" für die Übertragung von Benutzernamen und Kennwörter "Externe" eine Bindung zur Sicherheit TLS auf "Anonym" zu express ohne explizite Authentifizierung/Autorisierung und eine Vielzahl von zusätzlichen Verfahren, mit denen Authentifizierung und/oder Autorisierung Anmeldeinformationen oder Token übergeben.

AMQP des SASL Integration hat zwei Nachteile:

-   Alle Anmeldeinformationen und Token sind auf die Verbindung beschränkt. Eine Infrastruktur gegebenenfalls auf einer Basis pro Entität gestaffelter Access-Steuerelement bereitstellen. Beispielsweise wird der Inhaber von ein Token an Warteschlange A, jedoch nicht in der Warteschlange b senden Mit der Autorisierungskontext auf die Verbindung verankert ist es nicht möglich, verwenden Sie eine einzelne Verbindung und noch Zugriff auf unterschiedliche Token für eine Warteschlange und B.

-   Access Token sind in der Regel nur für eine begrenzte Zeit. Dadurch wird erzwungen den Benutzer regelmäßig Token erfassende und bietet die Möglichkeit zu der Tokenausgeber verweigern ein frisch Token ausgeben, wenn die Berechtigungen des Benutzers Zugriff geändert haben. AMQP Verbindungen möglicherweise sehr längere Zeit dauern. Das Modell SASL bietet nur eine Chance, ein Token zur Verbindungszeit, festlegen, d. die messaging-Infrastruktur muss entweder trennen Sie den Client h., wenn das Token läuft ab, oder es, das Risiko eine kontinuierliche Kommunikation mit einem Client akzeptieren muss, wer hat Zugriffsrechte möglicherweise in der Zwischenzeit gesperrt wurde.

Die AMQP CBS-Spezifikation von Azure-Dienstbus, implementiert ermöglicht eine elegante problemumgehung für beide diese Probleme: dies einen Client Access Token mit jedem Knoten zu verbinden, und diese Token zu aktualisieren, bevor sie ablaufen ohne Unterbrechung des Nachrichtenflusses, zulässt.

CBS definiert einen virtuelle Management-Knoten, mit dem Namen *$cbs*, von der messaging-Infrastruktur bereitgestellt werden. Der Knoten Management akzeptiert Token im Namen einer beliebigen anderen Knoten in der messaging-Infrastruktur.

Die Aktion Protokoll vorhanden ist eine Besprechungsanfrage Antworten/Exchange gemäß der Managementspezifikation. Die bedeutet, dass der Client stellt ein Paar von Verknüpfungen mit dem *$cbs* Knoten her und übergibt eine Anforderung auf den Hyperlink der ausgehenden und wartet dann auf die Antwort auf den Hyperlink der eingehenden.

Die Nachricht Request weist die folgenden Anwendungseigenschaften:

| Schlüssel        | Optional | Value-Typ | Wert Inhalt                             |
|------------|----------|------------|--------------------------------------------|
| Vorgang  | Nein       | Zeichenfolge     | **sich-token**                                |
| Typ       | Nein       | Zeichenfolge     | Die Art des das Token abgelegt.            |
| Namen       | Nein       | Zeichenfolge     | Das Token gilt "Zielgruppe". |
| Ablauf | Ja      | Zeitstempel  | Die Anzeigedauer Ablauf das Token.              |

Die *Name* -Eigenschaft identifiziert die Entität, mit der das Token verknüpft werden muss. In Dienstbus ist den Pfad für die Warteschlange oder das Thema/Abonnement. *Die Eigenschaft* identifiziert den token Typ:

| Token Typ                      | Token Beschreibung      | Texttyp           | Notizen                                                    |
|---------------------------------|------------------------|---------------------|----------------------------------------------------------|
| amqp:jwt                        | JSON Web Token (JWT)   | AMQP Wert (Zeichenfolge) | Noch nicht verfügbar.  |
| amqp:SWT                        | Einfache Web Token (SWT) | AMQP Wert (Zeichenfolge) | Unterstützt nur SWT Token ausgestellt von AAD/ACS          |
| Servicebus.Windows.NET:sastoken | Service Bus SAS Token  | AMQP Wert (Zeichenfolge) | -                                                        |

Token verbieten, Verwaltung von Informationsrechten. Dienstbus weiß drei grundlegende Rechte: "Senden" Senden "," ermöglicht das Empfangen "Abhören" und "Verwalten" können Elemente bearbeiten können. SWT Token ausgestellt von AAD/ACS explizit einschließen australischen als Ansprüche. Service Bus SAS Token auf Regeln, die so konfiguriert ist, klicken Sie auf den Namespace oder juristische verweisen, und diese Regeln mit der Verwaltung von Informationsrechten konfiguriert sind. Das Token mit der entsprechenden Regel zugeordnete Schlüssel signieren macht den Bericht somit token ausdrückliche der jeweiligen Rechte. Das Token einer Entität mithilfe von *sich-Token* zugeordnet wird den verbundenen Client für die Interaktion mit dieser Entität pro token Rechte erlauben. Eine Verknüpfung, die der Kunden auf die Rolle des *Absenders* findet die erfordert, dass das "Senden" rechts, klicken Sie auf die Rolle des *Empfängers* aufzeichnen "Abhören" rechts erfordert.

Die Antwortnachricht weist die folgenden Werte für die *Eigenschaften von Anwendung*

| Schlüssel                | Optional | Value-Typ | Wert Inhalt                    |
|--------------------|----------|------------|-----------------------------------|
| Status-code        | Nein       | Ganzzahl        | HTTP-Antwortcode **[RFC2616]**. |
| Status-Beschreibung | Ja      | Zeichenfolge     | Beschreibung des Status.        |

Der Client kann *sich Token* wiederholt und für jede Person in der messaging-Infrastruktur aufrufen. Die Token sind auf dem aktuellen Client ausgelegte und verankert, klicken Sie auf die aktuelle Verbindung, was bedeutet, dass der Server alle beibehalten Token eingefügt wird, wenn die Verbindung getrennt.

Die aktuelle Dienstbus Implementierung ermöglicht CBS nur in Verbindung mit der SASL-Methode "Anonym". Eine SSL/TLS-Verbindung muss immer vor der SASL Handshake vorhanden sein.

Das anonyme Verfahren muss daher von der ausgewählten AMQP 1.0-Client unterstützt werden. Anonymer Zugriff bedeutet, dass der anfänglichen Verbindungshandshake, einschließlich der Erstellung der anfänglichen Sitzung ohne Dienstbus geschieht wissen, wer die Verbindung erstellt wird.

Sobald die Sitzung und die Verbindung hergestellt wurde, werden die Links zu den *$cbs* anfügen Knoten und senden die Anfrage *sich-Token* nur Vorgänge zulässig. Ein gültiges Token mithilfe erfolgreich eine Anforderung *sich-Token* für einige Knoten Entität innerhalb von 20 Sekunden nach dem Herstellen der Verbindung festgelegt werden muss, andernfalls wird die Verbindung einseitig durch Dienstbus getrennt.

Der Client ist für das Nachverfolgen von token Ablauf später verantwortlich. Wenn ein Sicherheitstoken abläuft, verwirft Dienstbus umgehend alle Links auf die Verbindung zu der jeweiligen Entität. Um dies zu verhindern, kann der Client das Token für den Knoten mit einer neuen zu einem beliebigen Zeitpunkt über den virtuelle *$cbs* Management Knoten mit der gleichen *sich-Token* Bewegung und ohne erste beim den Datenverkehr Nutzlast, der auf verschiedene Links fließt ersetzen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu AMQP finden Sie unter den folgenden Links:

- [Service Bus AMQP (Übersicht)]
- [AMQP 1.0-Unterstützung für aufgeteilt Servicebuswarteschlangen und Themen]
- [AMQP in Dienstbus für WindowsServer]

[In diesem video Kurs]: https://www.youtube.com/playlist?list=PLmE4bZU0qx-wAP02i0I7PJWvDWoCytEjD
[1]: ./media/service-bus-amqp/amqp1.png
[2]: ./media/service-bus-amqp/amqp2.png
[3]: ./media/service-bus-amqp/amqp3.png
[4]: ./media/service-bus-amqp/amqp4.png

[Service Bus AMQP (Übersicht)]: service-bus-amqp-overview.md
[AMQP 1.0-Unterstützung für aufgeteilt Servicebuswarteschlangen und Themen]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP in Dienstbus für WindowsServer]: https://msdn.microsoft.com/library/dn574799.aspx