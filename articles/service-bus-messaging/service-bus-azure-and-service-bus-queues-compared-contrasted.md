<properties 
    pageTitle="Azure Warteschlangen und Dienstbus reiht - verglichenen und bereitstehen | Microsoft Azure"
    description="Unterschiede und gemeinsamkeiten zwischen zwei Warteschlangentypen Azure angebotenen analysiert."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="09/23/2016"
    ms.author="sethm" />

# <a name="azure-queues-and-service-bus-queues---compared-and-contrasted"></a>Azure Warteschlangen und Dienstbus reiht - verglichenen und bereitstehen

In diesem Artikel analysiert die Unterschiede und gemeinsamkeiten zwischen den zwei Warteschlangentypen heute von Microsoft Azure angeboten: Azure Warteschlangen und Dienstbus Warteschlangen. Mithilfe dieser Informationen können Sie vergleichen und vergleichen Sie die jeweiligen Technologien und weitere Entscheidung vorzunehmen über die Lösung am besten Ihren Bedürfnissen entspricht.

## <a name="introduction"></a>Einführung

Microsoft Azure unterstützt zwei Arten von Warteschlange Verfahren: **Azure** und **Service Bus Warteschlangen**.

**Azure Warteschlangen**, die Teil der [Azure-Speicher](https://azure.microsoft.com/services/storage/) bereit sind, verfügen über eine einfache REST-basierten Get/sich/anheften Schnittstelle zuverlässigen, beständigen messaging innerhalb und zwischen Services bereitstellen.

**Servicebuswarteschlangen** sind Teil einer breiteren [Azure messaging](https://azure.microsoft.com/services/service-bus/) Infrastruktur, die die queuing sowie veröffentlichen/abonnieren, Web Service Remote und Integration Muster unterstützt. Weitere Informationen zu Servicebuswarteschlangen, Themen/Abonnements und Relays finden Sie unter den [Überblick Dienstbus messaging](service-bus-messaging-overview.md).

Während beide Warteschlangen Technologien gleichzeitig vorhanden sind, wurden Azure Warteschlangen zuerst dedizierte Warteschlange Methode Speicher auf die Dienste Azure-Speicher eingeführt werden. Servicebuswarteschlangen basieren auf der breiteren "vermittelte messaging" Infrastruktur für die Integration von Applications oder Komponenten der Anwendung, die mehrere Kommunikationsprotokolle, von Datenverträgen umfassen möglicherweise entworfen Domänen vertrauen, und/oder Netzwerk-Umgebungen.

In diesem Artikel werden die zwei Warteschlange Technologien angebotenen Azure mit einer Diskussion über die Unterschiede der Verhalten und Implementierung von den Features von jeder verglichen. Der Artikel enthält auch Leitfaden zum auswählen, welche Features gemäß Ihren Bedürfnissen der Anwendung-Entwicklung möglicherweise.

## <a name="technology-selection-considerations"></a>Technologie Auswahl Aspekte

Sowohl die Azure Warteschlangen Dienstbus Warteschlangen sind Implementierungen der Nachricht Queuing-Dienst auf Microsoft Azure aktuell angeboten. Jede weist eine etwas anders Funktionsumfang, d. h., wählen Sie eine oder das andere, oder verwenden beide, je nach den Erfordernissen von bestimmter Lösung oder Business/technischen Problem Sie lösen möchten.

Bestimmen, welche Warteschlange Technologie für eine bestimmte Lösung geeignetes, sollte Lösungsarchitekten und Entwickler die folgenden Vorschläge berücksichtigen. Weitere Informationen hierzu finden Sie im nächste Abschnitt.

Als Lösung Architect/Entwickler, **erwägen Sie Azure Warteschlangen** wenn:

- Ihrer Anwendung muss über 80 GB Nachrichten in einer Warteschlange gespeichert werden, in dem die Nachrichten eine Lebensdauer, die weniger als sieben Tage haben.

- Die Anwendung den Fortschritt für die Verarbeitung von einer Nachricht innerhalb der Warteschlange nachverfolgt werden sollen. Dies ist sinnvoll, wenn die Verarbeitung einer Nachricht Worker stürzt ab. Einer nachfolgenden Worker können Sie diese Informationen aus, wo die vorherige Worker aufgehört fortsetzen.

- Sie benötigen Server Seite Protokolle für alle Transaktionen für Ihre Warteschlangen ausgeführt.

Als Lösung Architect/Entwickler **Servicebuswarteschlangen verwenden,** wenn:

- Ihre Lösung muss Nachrichten empfangen, ohne die Warteschlange Umfrage. Mit Dienstbus, dies kann erreicht werden durch die Verwendung der Long-Umfragen receive-Methode mit TCP-basierte Protokolle, Dienstbus unterstützt.

- Ihre Lösung erfordert die Warteschlange Bereitstellen einer garantiert First-in-First-Out (FIFO) geordnete Übermittlung.

- Sie möchten eine symmetrische Erfahrung in Azure und unter Windows Server (private Cloud). Weitere Informationen finden Sie unter [Service Bus für Windows Server](https://msdn.microsoft.com/library/dn282144.aspx).

- Ihre Lösung muss automatische Erkennung von Duplikaten unterstützen können.

- Sie möchten die Anwendung zum Verarbeiten von Nachrichten als parallele langer Streams (Nachrichten sind verknüpft mit einem Stream mithilfe der [SessionId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) -Eigenschaft auf die Nachricht). In diesem Modell konkurriert jeden Knoten in der Anwendung in Anspruch nehmen für Streams, im Gegensatz zu Nachrichten aus. Wenn ein Stream zur ein in Anspruch nehmen Knoten angegeben wird, kann der Knoten den Status der Stream Anwendungszustand Transaktionen prüfen.

- Ihre Lösung erfordert Transaktionen Verhalten und Unteilbarkeit beim Senden oder Empfangen von mehreren Nachrichten aus einer Warteschlange an.

- Die Time to live (TTL) Eigenschaft die Arbeitsbelastung anwendungsspezifische kann die 7 Tage lang überschreiten.

- Eine Anwendung behandelt Nachrichten, die 64 KB überschreiten, können aber wird nicht wahrscheinlich Ansatz 256 KB beschränkt.

- Sie arbeiten mit einer Anforderung ein Modells rollenbasierte Access Warteschlangen und andere Berechtigungen für den Absender und Empfänger bereitstellen.

- Die Warteschlangengröße Ihrer werden nicht mehr als 80 GB vergrößert.

- Sie möchten das AMQP 1.0 Standard-basierte messaging-Protokoll verwenden. Weitere Informationen zu AMQP finden Sie unter [Übersicht über den Dienst Bus AMQP](./service-bus-amqp-overview.md).

- Sie können eine tatsächliche Migration von Punkt Warteschlange-basierte Kommunikation zu einer Nachricht Exchange Muster beabsichtigen, das die nahtlose Integration von Empfängern mit zusätzlichen (Abonnenten), ermöglicht jeweils unabhängige Kopien von einigen oder allen an die Warteschlange gesendeten Nachrichten empfangen. Letztere bezieht sich auf die Funktion Veröffentlichen/Abonnieren von Dienstbus systembedingt bereitgestellt.

- Ihre Lösung für messaging muss unterstützen die "Am meisten-einmalige" Übermittlung Garantie, ohne dass Sie die zusätzliche Infrastrukturkomponenten erstellen können.

- Sie möchten möglicherweise veröffentlichen und Stapeln von Nachrichten nutzen.

- Sie benötigen vollständige Integration in den Windows Communication Foundation (WCF) Kommunikationsstapel in .NET Framework.

## <a name="comparing-azure-queues-and-service-bus-queues"></a>Vergleich von Azure Warteschlangen und Dienstbus Warteschlangen

Die Tabellen in den folgenden Abschnitten bieten eine logische Gruppierung der Warteschlange-Features und ermöglichen Ihnen, auf einen Blick die Funktionen, die in sowohl in Azure Warteschlangen Servicebuswarteschlangen vergleichen.

## <a name="foundational-capabilities"></a>Grundlegende Funktionen

In diesem Abschnitt werden einige der grundlegenden Warteschlangen Funktionen von Azure Warteschlangen und Dienstbus Warteschlangen verglichen.

|Vergleichende Suchkriterien|Azure Warteschlangen|Reaktionsgruppendienst Bus Warteschlangen|
|---|---|---|
|Gewährleistung Sortierung|**Nein** <br/><br>Weitere Informationen finden Sie in der ersten Notiz im Abschnitt "Zusätzliche Informationen".</br>|**Ja - First In First Out (FIFO)**<br/><br>(durch die Verwendung von messaging Sitzungen)|
|Übermittlung Garantie|**Am wenigsten einmaligem**|**Am wenigsten einmaligem**<br/><br/>**In den meisten einmaligem**|
|Kernoperation-Unterstützung|**Nein**|**Ja**<br/><br/>|
|Erhalten von Verhalten|**Nicht blockierende**<br/><br/>(sofort abgeschlossen, wenn keine neue Nachricht nicht gefunden wird)|**Mit/ohne Timeout blockieren**<br/><br/>(Angebote long Umfragen oder die ["Comet-Methode"](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Nicht blockierende**<br/><br/>(durch die Verwendung von .NET managed API nur)|
|Pushbenachrichtigungen-Stil-API|**Nein**|**Ja**<br/><br/>[OnMessage](https://msdn.microsoft.com/library/azure/jj908682.aspx) und **OnMessage** Sitzungen .NET API.|
|Erhalten von Modus|**Anheften und verleasen**|**Anheften und Sperren**<br/><br/>**Erhalten und löschen**|
|Einen exklusiven Zugriffsmodus|**Verleasen-basierten**|**Sperren-basierten**|
|Dauer verleasen/Sperren|**30 Sekunden (Standard)**<br/><br/>**7 Tage (maximal)** (Sie können erneuern oder lassen Sie eine Nachricht verleasen [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API verwenden.)|**60 Sekunden (Standard)**<br/><br/>Sie können eine Nachricht Sperren mit [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API erneuern.|
|Genauigkeit verleasen/Sperren|**Nachrichtenebene**<br/><br/>(jede Nachricht kann einen anderen Timeoutwert, die Sie dann je nach Bedarf beim Verarbeiten der Nachricht aktualisieren können mithilfe der [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API haben)|**Warteschlange Ebene**<br/><br/>(jede Warteschlange weist eine Sperre Genauigkeit auf alle Nachrichten angewendet, aber Sie können die Sperre mithilfe der [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API erneuern.)|
|Zusammengefasst erhalten|**Ja**<br/><br/>(explizit angeben Nachricht zählen beim Abrufen von Nachrichten, bis zu einem Maximum von 32 Nachrichten)|**Ja**<br/><br/>(eine Eigenschaft für die Vorabversion Abruf implizit aktivieren oder explizit durch die Verwendung von Transaktionen)|
|Gespeicherten senden|**Nein**|**Ja**<br/><br/>(durch die Verwendung von Transaktionen oder clientseitige Batchverarbeitung)|

### <a name="additional-information"></a>Weitere Informationen

- Nachrichten in Warteschlange Azure sind in der Regel First-in-First-Out, aber können manchmal angeordnet sein; beispielsweise, wenn eine Nachricht Sichtbarkeit Timeoutdauer (z. B. als Ergebnis eine Clientanwendung abstürzt während der Verarbeitung) läuft ab. Wenn das Sichtbarkeit Timeout abläuft, wird die Nachricht erneut in der Warteschlange für eine andere Worker aus der Warteschlange entfernt sichtbar. An diesem Punkt möglicherweise neu sichtbare Nachricht in der Warteschlange (erneut aus der Warteschlange entfernt werden) nach einer Nachricht platziert werden, die ursprünglich in der Warteschlange befindliche dahinter war.

- Wenn Sie bereits mit dem Azure-Speicher Blobs oder Tabellen und Verwenden von Warteschlangen, die Verfügbarkeit von 99,9 % garantiert. Wenn Sie mit Servicebuswarteschlangen Blobs oder Tabellen verwenden, haben Sie niedrigere Verfügbarkeit.

- Das garantierte FIFO Muster in Servicebuswarteschlangen erfordert die Verwendung von messaging Sitzungen. Den Fall, dass die Anwendung stürzt ab, während der Verarbeitung einer Nachricht im Modus **Anheften und Sperren** , wird das nächste Mal, das ein Warteschlange-Empfänger eine messaging-Sitzung akzeptiert, mit fehlgeschlagene Nachricht gestartet nach Ablauf seiner Time to live (TTL) Zeit.

- Azure Warteschlangen dienen zur Unterstützung von standard Warteschlange Szenarien wie entkoppeln Anwendungskomponenten zum Erhöhen der Skalierbarkeit und Fehlertoleranz bei Fehlern laden Abgleich und Prozessworkflows erstellen.

- Servicebuswarteschlangen unterstützen die *Am wenigsten einmaligem* Übermittlung Garantie. Darüber hinaus können der *Am meisten einmaligem* semantische unterstützt werden, mithilfe von Sitzungszustand Zustand der Anwendung gespeichert und mithilfe von Transaktionen automatisch empfangen und aktualisieren Sie den Sitzungszustand.

- Azure Warteschlangen bieten eine einheitliche und konsistente Programmierung Modell über Warteschlangen, Tabellen und BLOBs – sowohl für Entwickler als auch für Vorgänge Teams.

- Servicebuswarteschlangen bieten Unterstützung für lokale Transaktionen im Kontext einer einzelnen Warteschlange.

- Der von Dienstbus unterstützte **erhalten, und löschen Sie** Modus bietet die Möglichkeit zum Verringern der Anzahl von messaging Vorgänge (und zugeordneten Kosten) gegen tiefgestellter Übermittlung Assurance.

- Azure Warteschlangen bieten Leases die Möglichkeit, um die Leases für Nachrichten zu erweitern. Können die Mitarbeiter der kurzen Leases Nachrichten zu verwalten. Auf diese Weise ist ein Worker stürzt ab, kann die Nachricht schnell erneut vom anderen Arbeitskräften verarbeitet werden. Darüber hinaus kann ein Arbeitskollegen der verleasen in einer Nachricht erweitern, wenn sie es verarbeiten länger als die aktuelle Uhrzeit verleasen.

- Azure Warteschlangen bieten einen Sichtbarkeit Timeout, mit dem Sie können nach der Enqueueing festlegen oder Entfernen von einer Nachricht an. Darüber hinaus können Sie eine Nachricht mit verschiedenen verleasen Werte zur Laufzeit aktualisieren und Aktualisieren von verschiedenen Werte über Nachrichten in der gleichen Warteschlange. Dienstbus Sperren Zeitlimit werden in den Metadaten Warteschlange definiert. jedoch können Sie die Sperre erneuern, indem Sie die Methode [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) .

- Die maximal zulässige beträgt für eine blockieren receive-Methode in Servicebuswarteschlangen 24 Tage. REST-basierten Zeitlimit haben jedoch einen Höchstwert von 55 Sekunden.

- Clientseitige Batchverarbeitung von Dienstbus bereitgestellt, kann ein Client Warteschlange mehrere Nachrichten in einer einzelnen Sendevorgang Stapel. Batchverarbeitung ist nur für asynchrone Sendevorgänge zur Verfügung.

- Features wie die 200 TB Obergrenze von Azure (wenn Virtualisierung Konten mehr) und unbegrenzte Warteschlangen machen Sie es eine ideale Plattform für SaaS-Anbieter.

- Azure Warteschlangen bieten eine flexible und leistungsfähige delegiert Access angibt.

## <a name="advanced-capabilities"></a>Erweiterte Funktionen

In diesem Abschnitt werden erweiterte Funktionen von Azure Warteschlangen und Dienstbus Warteschlangen verglichen.

|Vergleichende Suchkriterien|Azure Warteschlangen|Reaktionsgruppendienst Bus Warteschlangen|
|---|---|---|
|Geplante Übermittlung|**Ja**|**Ja**|
|Automatische inaktive Textformats|**Nein**|**Ja**|
|Zunehmender Warteschlange TTL Wert|**Ja**<br/><br/>(über der Sichtbarkeit Timeout in-situ-Update)|**Ja**<br/><br/>(über eine dedizierte API-Funktion bereitgestellt)|
|Unterstützung für Nachrichten für nicht verarbeitete Nachrichten|**Ja**|**Ja**|
|In-situ-update|**Ja**|**Ja**|
|Serverseitige Transaktionslog|**Ja**|**Nein**|
|Kennzahlen Speicher|**Ja**<br/><br/>**Minute Kennzahlen**: bietet in Echtzeit Kennzahlen zur Verfügbarkeit, TPS, API anrufen zählt, Fehler zählt und mehr alle in Echtzeit (zusammengefasster pro Minute und gemeldet innerhalb weniger Minuten aus, was gerade in Herstellung geschehen ist. Weitere Informationen finden Sie unter [Informationen zum Speicher Analytics Kennzahlen](https://msdn.microsoft.com/library/azure/hh343258.aspx).|**Ja**<br/><br/>(Massen Abfragen mithilfe des [GetQueues](https://msdn.microsoft.com/library/azure/hh293128.aspx))|
|Staatliche Verwaltung|**Nein**|**Ja**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx)|
|Automatische Weiterleitung von Nachrichten|**Nein**|**Ja**|
|Bereinigen Warteschlange (Funktion)|**Ja**|**Nein**|
|Nachrichtengruppen|**Nein**|**Ja**<br/><br/>(durch die Verwendung von messaging Sitzungen)|
|Status pro Nachrichtengruppe Anwendung|**Nein**|**Ja**|
|Erkennung von Duplikaten|**Nein**|**Ja**<br/><br/>(klicken Sie auf der Seite Absender konfigurierbare)|
|WCF-integration|**Nein**|**Ja**<br/><br/>(bietet Out-of-Box-WCF-Bindungen)|
|WF-integration|**Benutzerdefinierte**<br/><br/>(erfordert, erstellen eine benutzerdefinierte WF Aktivität)|**Systemeigene**<br/><br/>(bietet Out-of-Box-WF-Aktivitäten)|
|Durchsuchen die Nachrichtengruppen|**Nein**|**Ja**|
|Abrufen von Nachricht Sitzungen nach ID|**Nein**|**Ja**|

### <a name="additional-information"></a>Weitere Informationen

- Beide Warteschlangen Technologien aktivieren eine Meldung, die für die Übermittlung zu einem späteren Zeitpunkt geplant werden.

- Warteschlange automatische Weiterleitung ermöglicht Tausende von Warteschlangen und automatisch eine einzelne Warteschlange, deren e-Mails weiterleiten aus der die Anwendung empfangende die Nachricht benötigt. Sie können verwenden Sie dieses Verfahren, um die Sicherheit zu erzielen, Informationsfluss zu steuern und isolieren Speicher zwischen jede Nachricht Publisher.

- Azure Warteschlangen bieten Unterstützung für Nachrichteninhalt aktualisieren. Sie können dieses Feature zum Beibehalten von Statusinformationen und schrittweisen Fortschritt Updates in der Nachricht verwenden, sodass vom letzten bekannten Prüfpunkt, anstatt neu beginnen verarbeitet werden können. Mit Servicebuswarteschlangen können Sie das gleiche Szenario bestimmte Nachricht Sitzungen aktivieren. Sitzungen aktivieren Sie zum Speichern und Abrufen der Zustand der Anwendung Verarbeitung (mithilfe von [SetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx) und [GetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx)).

- [Inaktive Textformats](service-bus-dead-letter-queues.md), das nur von Servicebuswarteschlangen unterstützt wird, kann sein hilfreich, um Nachrichten, die von der Anwendung empfangen oder wenn Nachrichten ihr Ziel aufgrund einer abgelaufenen Time to live (TTL) Eigenschaft erreichen können nicht erfolgreich verarbeitet werden können. Der TTL-Wert gibt an, wie lange eine Nachricht in der Warteschlange verbleibt. Mit Dienstbus wird die Nachricht verschoben werden, um eine spezielle Warteschlange $DeadLetterQueue aufgerufen, wenn die Gültigkeitsdauer abläuft.

- Genügt "beschädigte" Nachrichten in Warteschlange Azure, wenn eine Nachricht entfernen aus der Warteschlange die Anwendung untersucht die Eigenschaft **[DequeueCount](https://msdn.microsoft.com/library/azure/dd179474.aspx)** der Nachricht ein. Ist **DequeueCount** über einen bestimmten Schwellenwert, verschiebt die Anwendung die Nachricht an eine Warteschlange anwendungsspezifische "unzustellbare" ein.

- Azure Warteschlangen aktivieren Sie erhalten eine detaillierte Protokollierung des alle Transaktionen in der Warteschlange befindet, als auch als aggregierte Kennzahlen ausgeführt. Beide Optionen sind nützlich für Debuggen und zu verstehen, wie eine Anwendung Azure Warteschlangen verwendet werden. Sie sind auch hilfreich für Optimieren Ihrer Anwendung, und reduzieren die Kosten für die Verwendung von Warteschlangen.

- Des Konzepts der "Nachricht Sitzungen" von Dienstbus unterstützt kann Nachrichten, die zu einer bestimmten logischen verknüpft Gruppe mit einer angegebenen Empfänger verwendet werden, wodurch wiederum eine Sitzung-ähnliche Zugehörigkeit zwischen Nachrichten und ihre jeweiligen Empfängern erstellt gehören. Sie können diese erweiterte Funktionen in Dienstbus mithilfe der [SessionID](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) -Eigenschaft auf eine Nachricht aktivieren. Empfänger können klicken Sie dann auf eine bestimmte Sitzung ID Abhören und Empfangen von Nachrichten, die den angegebenen Sitzungsbezeichner gemeinsam nutzen.

- Der Angebote und Erkennung automatisch von Servicebuswarteschlangen unterstützten Funktionen entfernt doppelte Nachrichten, die an eine Warteschlange oder ein Thema, basierend auf dem Wert der Eigenschaft [Nachrichten-ID](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) an.

## <a name="capacity-and-quotas"></a>Kapazität und Kontingente

In diesem Abschnitt vergleicht Azure Warteschlangen und Dienstbus Warteschlangen hinsichtlich der [Kapazität und Kontingente](service-bus-quotas.md) , die angewendet werden kann.

|Vergleichende Suchkriterien|Azure Warteschlangen|Reaktionsgruppendienst Bus Warteschlangen|
|---|---|---|
|Maximale Größe|**200 TB**<br/><br/>(auf einem einzelnen Konto Speicherkapazität begrenzt)|**1 GB zu 80 GB**<br/><br/>(finden Sie bei der Erstellung einer Warteschlange und- [Partitionierung aktivieren](service-bus-partitioning.md) – definiert im Abschnitt "Zusätzliche Informationen")|
|Maximale Größe für Nachrichten|**64 KB**<br/><br/>(48 KB bei Verwendung von **Base64** -Codierung)<br/><br/>Azure unterstützt große Nachrichten durch Kombinieren Warteschlangen und Blobs – an welcher Stelle Sie Warteschlange können bis zu 200GB für ein einzelnes Element.|**256 KB** oder **1 MB**<br/><br/>(einschließlich sowohl Kopf und der Text, maximale Headergröße: 64 KB).<br/><br/>Abhängig von der [Dienstebene](service-bus-premium-messaging.md).|
|Maximale Nachricht TTL|**7 Tage**|**`TimeSpan.Max`**|
|Maximale Anzahl der Warteschlangen|**Unbegrenzte**|**10.000**<br/><br/>(pro Dienstnamespace, kann erhöht werden)|
|Maximale Anzahl von gleichzeitigen clients|**Unbegrenzte**|**Unbegrenzte**<br/><br/>(100 gleichzeitige Verbindung Beschränkung gilt nur für TCP-Protokoll-basierte Kommunikation)|

### <a name="additional-information"></a>Weitere Informationen

- Dienstbus erzwingt Warteschlange Größengrenzwerte. Die maximale Größe, die bei der Erstellung der Warteschlange angegeben ist, und es kann einen Wert zwischen 1 und 80 GB haben. Wenn bei der Erstellung der Warteschlange festgelegten Wert für die Größe der Warteschlange erreicht wird, weitere eingehende Nachrichten abgelehnt und eine Ausnahme wird von der einen Code empfangen werden. Weitere Informationen zu Kontingenten in Dienstbus finden Sie unter [Service Bus Kontingente](service-bus-quotas.md).

- Sie können Servicebuswarteschlangen in 1, 2, 3, 4 und 5 GB Größen erstellen (die Standardeinstellung ist 1 GB). Mit Partitionierung aktiviert (die Standardeinstellung), Dienstbus erstellt 16 Partitionen für jedes GB, die Sie angeben. Wenn Sie eine Warteschlange, die Größe 5 GB ist erstellen, mit 16 Partitionen die maximale Größe wird (5 * 16) = 80 GB. Sie können die maximale Größe von partitionierten Warteschlange oder Thema anzeigen, indem Sie den Eintrag im [Portal Azure][]betrachtet.

- Mit Azure Warteschlangen Wenn der Inhalt der Nachricht nicht XML-sichere, wird muss, **Base64** -codierte sein. Wenn Sie **Base64**-Codierung die Nachricht, die Benutzer Nutzlast kann bis zu 48 KB anstelle von 64 KB sein.

- Mit Servicebuswarteschlangen, jede Nachricht, die in einer Warteschlange gespeicherten besteht aus zwei Teilen: einer Kopfzeile und Text. Die Gesamtgröße der Nachricht darf die maximale Größe von der Dienstebene unterstützt nicht überschreiten.

- Wenn die Kommunikation von Clients mit Servicebuswarteschlangen über das TCP-Protokoll, beträgt die maximale Anzahl der aktiven Verbindungen auf eine einzelne Dienstbus Warteschlange 100. Diese Anzahl Absender und Empfänger gemeinsam verwendet. Wenn dieses Kontingents erreicht wird, nachfolgende Anforderungen für zusätzliche Verbindungen abgelehnt, und es wird eine Ausnahme durch den einen Code empfangen werden. Diese Einschränkung ist nicht auf Clients eine Verbindung herstellen, die den REST-basierten API mit Warteschlangen.

- Wenn Sie mehr als 10.000 Warteschlangen in einem einzelnen Dienstbus Namespace benötigen, können Sie wenden Sie sich an das Supportteam Azure und Anfordern einer erhöhen. Um die Skalierung von bis 10.000 Warteschlangen mit Dienstbus, können Sie auch zusätzliche Namespaces mithilfe der [Azure-Portal][]erstellen.

## <a name="management-and-operations"></a>Verwaltung und Betrieb

In diesem Abschnitt vergleicht die Verwaltungsfunktionen von Azure Warteschlangen und Dienstbus Warteschlangen bereitgestellt.

|Vergleichende Suchkriterien|Azure Warteschlangen|Reaktionsgruppendienst Bus Warteschlangen|
|---|---|---|
|Projektmanagement-Protokoll|**Zeigen Sie auf HTTP-/HTTPS**|**Zeigen Sie auf HTTPS**|
|Laufzeit-Protokoll|**Zeigen Sie auf HTTP-/HTTPS**|**Zeigen Sie auf HTTPS**<br/><br/>**AMQP 1.0 Standard (TCP mit TLS)**|
|.NET verwalteten API|**Ja**<br/><br/>(.NET verwaltet Speicher-Client-API)|**Ja**<br/><br/>(Verwalteter vermittelte messaging-API)|
|Systemeigener C++|**Ja**|**Nein**|
|Java-API|**Ja**|**Ja**|
|PHP-API|**Ja**|**Ja**|
|Node.js API|**Ja**|**Ja**|
|Beliebige Metadaten-support|**Ja**|**Nein**|
|Warteschlange-Benennungskonventionen|**Bis zu 63 Zeichen lang sein**<br/><br/>(Buchstaben im Namen einer Warteschlange müssen Kleinbuchstaben sein.)|**Bis zu 260 Zeichen lang sein**<br/><br/>(Warteschlangenpfade und Namen sind Groß-und Kleinschreibung.)|
|Get-Warteschlange Länge-Funktion|**Ja**<br/><br/>(Ungefähren Wert Wenn Nachrichten über die Gültigkeitsdauer abläuft, ohne dass gelöscht.)|**Ja**<br/><br/>(Genauen, Point-in-Time-Wert.)|
|Anheften (Funktion)|**Ja**|**Ja**|

### <a name="additional-information"></a>Weitere Informationen

- Azure Warteschlangen bieten Unterstützung für beliebige Attribute, die auf die Beschreibung der Warteschlange in Form von Name/Wert-Paare angewendet werden können.

- Beide Warteschlange Technologien ermöglichen es, Einsehen einer Meldung, ohne ihn, sperren, die beim Implementieren eines Warteschlange-Explorer-Browser-Tools hilfreich sein können.

- Der Dienst Bus .NET vermittelte messaging APIs nutzen voll-Duplex TCP-Verbindungen für verbesserte Leistung gegenüber der restlichen über HTTP, und das standardmäßige AMQP 1.0-Protokoll unterstützen.

- Namen von Azure Warteschlangen 3-63 Zeichen lang sein können, kann Kleinbuchstaben, Zahlen und Bindestriche enthalten. Weitere Informationen finden Sie unter [benennen Warteschlangen und Metadaten](https://msdn.microsoft.com/library/azure/dd179349.aspx).

- Dienstbus Warteschlangennamen können bis zu 260 Zeichen lang sein und weniger einschränkende naming Regeln. Dienstbus Warteschlangennamen können Buchstaben, Zahlen, Punkte, Bindestriche und Unterstriche enthalten.

## <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung

In diesem Abschnitt wird erläutert, die Authentifizierung und Autorisierung Features von Azure Warteschlangen und Dienstbus Warteschlangen unterstützt.

|Vergleichende Suchkriterien|Azure Warteschlangen|Reaktionsgruppendienst Bus Warteschlangen|
|---|---|---|
|Authentifizierung|**Symmetrischen Schlüssel**|**Symmetrischen Schlüssel**|
|Sicherheitsmodell|Delegierter Zugriff über SAS Token.|SAS|
|Identität Anbieter Föderation|**Nein**|**Ja**|

### <a name="additional-information"></a>Weitere Informationen

- Jeder Anforderung entweder von der Warteschlangen Technologien muss authentifiziert werden. Öffentliche Warteschlangen mit anonymem Zugriff werden nicht unterstützt. Mit [SAS](service-bus-sas-overview.md), können Sie dieses Szenario verhindern, indem Sie eine schreibgeschützte SAS, schreibgeschützt SAS oder sogar auf einem vollständigen Zugriff SAS veröffentlichen.

- Die Authentifizierung des Farbschemas von Azure Warteschlangen bereitgestellten umfasst die Verwendung eines symmetrischen Schlüssels, also eine hashbasierten HMAC Message Authentication Code (), mit dem SHA-256-Algorithmus berechnet und als Zeichenfolge **Base64** codierte. Weitere Informationen zu den jeweiligen Protokoll finden Sie unter [Authentifizierung für die Azure-Speicher-Dienste](https://msdn.microsoft.com/library/azure/dd179428.aspx). Servicebuswarteschlangen unterstützt ein ähnliches Modell symmetrische Schlüssel verwenden. Weitere Informationen finden Sie unter [Freigegebene Access Signatur Authentifizierung mit Dienstbus](service-bus-shared-access-signature-authentication.md).

## <a name="cost"></a>Kosten

In diesem Abschnitt vergleicht Azure Warteschlangen und Dienstbus Warteschlangen im Hinblick auf Kosten.

|Vergleichende Suchkriterien|Azure Warteschlangen|Reaktionsgruppendienst Bus Warteschlangen|
|---|---|---|
|Warteschlange Transaktionskosten|**$0.0036**<br/><br/>(pro 100.000 Transaktionen)|**Grundlegende Ebene**: **$0,05**<br/><br/>(pro Millionen Vorgänge)|
|Berechenbare Vorgänge|**Alle**|**Empfangen Sie senden/nur**<br/><br/>(kostenlos für andere Vorgänge)|
|Im Leerlauf Transaktionen.|**Berechenbare**<br/><br/>(Die Abfragen einer leeren Warteschlange wird als berechenbare Transaktion gezählt.)|**Berechenbare**<br/><br/>(Eine empfangen anhand einer leeren Warteschlange wird eine Nachricht berechenbare angesehen.)|
|Speicherkosten|**$0,07**<br/><br/>(pro GB pro Monat)|**0,00**|
|Ausgehende Datenübertragung Kosten|**$0,12 - $0.19**<br/><br/>(Je nach Geography.)|**$0,12 - $0.19**<br/><br/>(Je nach Geography.)|

### <a name="additional-information"></a>Weitere Informationen

- Datenübertragung unterliegen basierend auf der Gesamtmenge der Daten, die die Azure Datencentern über das Internet in einem bestimmten Abrechnungszeitraum verlassen.

- Datenübertragung zwischen Azure Services innerhalb derselben Region ansässig werden nicht berechnet.

- Alle eingehenden Datenübertragung werden derzeit nicht berechnet.

- Ausgehend von der Unterstützung für lange Umfragen, kann mit Servicebuswarteschlangen Kosten effektiv Situationen sein, Low-Wartezeit Übermittlung erforderlich ist.

>[AZURE.NOTE] Alle Kosten werden können geändert. In dieser Tabelle widerspiegeln, aktuelle Preise und schließt keine Werbeaktion Angebote, die derzeit verfügbar sein können. Aktuelle Informationen zu Azure Preise finden Sie unter der Seite [Azure Preise](https://azure.microsoft.com/pricing/) . Weitere Informationen zur Preisgestaltung Dienstbus finden Sie unter [Dienstbus Preise](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="conclusion"></a>Abschluss

Durch Übernahme der umfassendere Kenntnisse der zwei Technologies, werden Sie welche Warteschlange-Technologie zu verwenden, klicken Sie auf Weitere Entscheidung vorzunehmen und wann. Klar hängt die Entscheidung darüber, wann Azure Warteschlangen oder Dienstbus Warteschlangen verwenden eine Reihe von Faktoren. Diese Faktoren möglicherweise stark auf den einzelnen Anforderungen Ihrer Anwendung und deren Architektur abhängig sind. Wenn die Anwendung bereits die wesentlichen Funktionen von Microsoft Azure verwendet, möchten Sie vielleicht aus Azure Warteschlangen, insbesondere dann, wenn Sie benötigen grundlegende Kommunikation und messaging zwischen Services oder Warteschlangen müssen, die mehr als 80 GB groß sein können.

Da Servicebuswarteschlangen eine Reihe von erweiterten Funktionen, wie z. B. Sitzungen, Transaktionen, bieten duplizieren, automatische Erkennung Dead-Textformats und permanente Veröffentlichen/Abonnieren-Funktionen, sie können eine bevorzugte Wahl sein, wenn Sie eine Anwendung Hybrid erstellen, oder wenn die Anwendung, die andernfalls diese Features erfordert.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel enthalten weitere Leitfäden und Informationen zur Verwendung von Azure Warteschlangen oder Dienstbus Warteschlangen.

- [So verwenden Sie Bus Servicewarteschlangen](service-bus-dotnet-get-started-with-queues.md)
- [So verwenden Sie die Warteschlange-Speicherdienst](../storage/storage-dotnet-how-to-use-queues.md)
- [Bewährte Methoden zur Steigerung der Leistung mit Dienstbus vermittelte messaging](service-bus-performance-improvements.md)
- [Einführung in die Warteschlangen und Themen in Azure Dienstbus](http://www.code-magazine.com/article.aspx?quickid=1112041)
- [Die Developer's Guide to Dienstbus](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
- [Verwenden des Warteschlangen-Diensts in Azure](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)
- [Grundlegendes zu Azure-Speicher Abrechnung – Bandbreite, Transaktionen und Kapazität](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)

[Azure-portal]: https://portal.azure.com
 
