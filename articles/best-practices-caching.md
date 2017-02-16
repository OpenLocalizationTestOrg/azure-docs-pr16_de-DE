<properties
   pageTitle="Zwischenspeichern Anleitungen | Microsoft Azure"
   description="Anleitungen zum Verbessern der Leistung und Skalierbarkeit Zwischenspeichern."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/14/2016"
   ms.author="masashin"/>


# <a name="caching-guidance"></a>Anleitungen Zwischenspeichern

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

Zwischenspeichern ist ein gebräuchliches Verfahren, das Ziel ist, um die Leistung und Skalierbarkeit eines Systems zu verbessern. Dies geschieht durch Kopieren vorübergehend häufig verwendete Daten auf schnelle Speicher, die befindet schließen ', um die Anwendung. Wenn diese schnelle Datenspeicher näher an die Anwendung als die ursprüngliche Quelle befindet, kann dann Zwischenspeichern erheblich Reaktionszeiten für Clientanwendungen verbessern, indem Daten schnelleres erstellen.

Zwischenspeichern ist am effektivsten, wenn eine Clientinstanz wiederholt dieselben Daten liest insbesondere dann, wenn alle folgenden Kriterien in den ursprünglichen Datenspeicher anwenden:
- Es bleibt relativ statisch.
- Es ist langsam im Vergleich zu der Geschwindigkeit des Caches.
- Es wird eine hohe Konflikte unterliegt.
- Es ist weit beim Netzwerkwartezeit Zugriff zu einem verlangsamten führen kann.

## <a name="caching-in-distributed-applications"></a>Zwischenspeichern in verteilten Clientanwendungen

Verteilte Applikationen implementieren in der Regel eine oder beide der folgenden Strategien beim Zwischenspeichern von Daten:

- Verwenden von einem privaten Cache speichern, wo lokal Daten auf dem Computer gespeichert werden, die eine Instanz einer Anwendung oder Dienst ausgeführt wird.
- Verwenden einer freigegebenen Cache für Dokumente, die als eine gemeinsame Quelle die, indem Sie mehrere Prozesse und/oder Maschinen zugegriffen werden kann.

In beiden Fällen kann Zwischenspeichern clientseitige und/oder serverseitigen durchgeführt werden. Clientseitige Zwischenspeichern erfolgt durch den Vorgang, die die Benutzeroberfläche für ein System, wie etwa einen Webbrowser oder desktop-Anwendung enthält.
Serverseitige Zwischenspeichern erfolgt durch den Prozess, der Business Services bietet, die per Remotezugriff ausgeführt werden.

### <a name="private-caching"></a>Private Zwischenspeichern.

Der grundlegendste Typ des Caches ist ein in-Memory-Speicher. Es hat frei, in dem Adressbereich eines einzelnen Prozesses und der in diesem Prozess ausgeführte Code direkt zugreifen. Diese Art von Cache geht sehr schnell für den Zugriff auf. Sie können auch ein sehr effektives Mittel bereitstellen, zum Speichern von bequeme Datenmengen statischen, da ein Cachegröße in der Regel durch das Volumen Arbeitsspeicher beschränkt ist, die auf dem Computer des Prozesses Hostinganbieter verfügbar ist.

Wenn Sie weitere Informationen als physisch im Speicher möglich ist im cache müssen, können Sie den zwischengespeicherte Daten im lokalen Dateisystem schreiben. Dies ist langsamer als Daten zugreifen, die im Speicher vorhanden ist, jedoch sollten immer noch schneller und die Zuverlässigkeit als Abrufen von Daten in einem Netzwerk sein.

Wenn Sie mehrere Instanzen einer Anwendung dieses Modell gehostet, die gleichzeitig ausgeführt wird, weist jede Anwendungsinstanz einen eigenen unabhängigen Cache mit einem eigenen Kopie der Daten aus.

Stellen Sie sich einen Cache als Momentaufnahme der ursprünglichen Daten zu einem bestimmten Zeitpunkt in der Vergangenheit. Wenn diese Daten nicht statisch ist, ist es wahrscheinlich, dass Anwendungsinstanzen der anderen unterschiedliche Versionen der Daten in die Caches halten. Daher kann die gleiche Abfrage ausgeführt wird, indem Sie diese Instanzen unterschiedliche Ergebnisse zurückgeben, wie in Abbildung 1 dargestellt.

![Verwenden eines in-Memory-Caches in verschiedenen Instanzen einer Anwendung](media/best-practices-caching/Figure1.png)

_Abbildung 1: Verwenden eines in-Memory-Caches in verschiedenen Instanzen einer Anwendung_

### <a name="shared-caching"></a>Freigegebene Zwischenspeichern

Verwenden eines freigegebenen Caches helfen Probleme zu beheben, dass die Daten in jeder Cache, unterscheiden sich möglicherweise die mit im Arbeitsspeicher Zwischenspeichern auftreten können. Freigegebene Zwischenspeichern wird sichergestellt, dass andere Anwendungsinstanzen der gleichen Ansicht der zwischengespeicherten Daten finden Sie unter. Dazu Auffinden des Caches in einem anderen Speicherort, in der Regel als Teil einer separaten Dienst gehostet werden, wie in Abbildung 2 dargestellt werden.

![Verwenden eines freigegebenen Caches](media/best-practices-caching/Figure2.png)

_Abbildung 2: Verwenden eines freigegebenen Caches_

Ein wichtiger Vorteil der freigegebenen Zwischenspeichern Ansatz ist der Skalierbarkeit, die ihn enthält. Viele gemeinsam genutzten Cache Services werden mit einem Cluster Servern implementiert und Nutzung der Software, die die Daten über den Cluster auf transparente Weise verteilt. Eine Instanz der Anwendung sendet einfach eine Anforderung an den Cachedienst an.
Die zugrunde liegende Infrastruktur ist zuständig für das Ermitteln des Orts für die zwischengespeicherten Daten im Cluster. Sie können den Cache einfach skalieren, durch Hinzufügen weiterer Server.

Es gibt zwei Nachteile der freigegebenen Zwischenspeichern Ansatz Hauptfenster:
- Der Cache ist langsamer auf zugreifen, da jede Instanz der Anwendung nicht mehr lokal gehalten wird.
- Die Anforderung einer separaten cachediensts implementiert wird möglicherweise Komplexität der Lösung hinzufügen.

## <a name="considerations-for-using-caching"></a>Aspekte der Verwendung zwischenspeichern

Den folgenden Abschnitten werden die Aspekte für den Entwurf, und verwenden einen Cache ausführlicher aus.

### <a name="decide-when-to-cache-data"></a>Entscheiden Sie, wann Daten zwischenspeichern

Zwischenspeichern kann Leistung und Skalierbarkeit Verfügbarkeit erheblich verbessern. Je mehr Daten, die Sie besitzen und größer die Anzahl der Benutzer, die auf diese Daten, die größeren der Vorteile der Konvertierung in Zwischenspeichern zugreifen müssen. Dies liegt daran Zwischenspeichern verringert die Wartezeit und Konflikte, die Behandlung großer Datenmengen gleichzeitige Anforderungen im ursprünglichen Datenspeicher zugeordnet wurde.

Eine Datenbank unterstützt möglicherweise eine eingeschränkte Anzahl von aktiven Verbindungen. Abrufen von Daten aus einem freigegebenen Cache, ermöglicht jedoch anstelle der zugrunde liegenden Datenbank für eine Clientanwendung auf diese Daten zuzugreifen, selbst wenn die Anzahl der verfügbaren Verbindungen zurzeit leer ist. Darüber hinaus, wenn die Datenbank nicht mehr verfügbar ist, Clientanwendungen möglicherweise weiterhin mithilfe der Daten, die gehalten werden im Cache.

Erwägen Sie das Zwischenspeichern von Daten, die häufig lesen aber selten (z. B. Daten, die über einen höheren Anteil der gelesen Vorgänge als Vorgänge schreiben) geändert werden. Jedoch empfohlen nicht, dass Sie den Cache als autoritative Speicher von wichtigen Informationen verwenden. In diesem Fall stellen Sie sicher, dass alle Änderungen, die Ihrer Anwendung verlieren ausnahmslos immer zu einem beständigen Datenspeicher gespeichert werden. Dies bedeutet, dass, wenn der Cache nicht verfügbar ist, Ihrer Anwendung angewendet werden weiterhin kann, indem Sie die Datenspeicher und Sie wichtige Informationen verlieren nicht.

### <a name="determine-how-to-cache-data-effectively"></a>Bestimmen Sie, wie Daten effektiv Zwischenspeichern.

Der Schlüssel zum Effektives Verwenden von einem Cache liegt in bestimmen geeigneten Daten Cache und zwischenzuspeichern, um ihn zum entsprechenden Zeitpunkt. Um den Cache für Dokumente bei Bedarf das erste Mal, dass sie von einer Anwendung abgerufen werden, können die Daten hinzugefügt werden. Dies bedeutet, dass die Anwendung nur einmal die Daten aus dem Datenspeicher abgerufen werden sollen muss, und die nachfolgenden Zugriff mithilfe des Caches erfüllt werden kann.

Alternativ kann ein Cache teilweise oder vollständig mit Daten im voraus, in der Regel aufgefüllt werden beim Start der Anwendung (eines Ansatzes sendet genannt). Jedoch nicht möglicherweise ratsam, implementieren für einen großen Cache sendet, da dieser Ansatz erzwingen zu kann eine plötzliche, hohe Auslastung im ursprünglichen Datenspeicher beim Start der Anwendung ausgeführt.

Häufig können Sie entscheiden, ob vollständig oder teilweise vorab einen Cache, und wählen Sie die Daten in den Cache eine Analyse der von Verwendungsmustern helfen. Beispielsweise kann das sein Startwert für Kunden, die die Anwendung regelmäßig (vielleicht jeden Tag) verwenden, aber nicht für Benutzer von der Anwendung nur einmal pro Woche für den Cache für die statischen Benutzerprofildaten hilfreich.

In der Regel Zwischenspeichern funktioniert auch mit Daten, die unveränderlich ist oder selten ändert. Zählen Referenzinformationen z. B. Produkt und Preisinformationen in einer e-Commerce-Anwendung oder freigegebene statische Ressourcen, die teure erstellt werden. In den Cache beim Start der Anwendung minimieren bei Bedarf auf Ressourcen sowie zur Verbesserung der Systemleistung können einige oder alle diese Daten geladen werden. Möglicherweise auch entsprechende Hintergrund vorgenommen haben, auf die Bezug regelmäßig aktualisiert Daten im Cache, um sicherzustellen, dass auf dem neuesten Stand sind oder die wird der Cache aktualisiert werden beim Verweisen auf Daten geändert wird.

Zwischenspeichern ist weniger nützlich für dynamic Data, obwohl es einige Ausnahmen für diese Aspekte gibt (Siehe den Abschnitt Cache hochgradig dynamische Daten weiter unten in diesem Artikel Weitere Informationen). Wenn Sie die Originaldaten regelmäßig ändern, die zwischengespeicherte Informationen sind sehr schnell veraltet, oder der Aufwand für das mit dem ursprünglichen Datenspeicher Cache synchronisieren reduziert die Effektivität der Zwischenspeichern.

Beachten Sie, dass ein Cache nicht die vollständigen Daten für eine Entität enthalten sind. Beispielsweise, wenn ein Datenelement ein mehrwertiges Objekts wie eine Bank mit einem Name, Adresse und Saldo darstellt, möglicherweise einige dieser Elemente statisch (beispielsweise den Namen und die Adresse), bleiben, während andere (beispielsweise der Saldo) dynamischeren möglicherweise. In diesen Fällen kann es hilfreich sein, die statische Teile der Daten im cache und nur die restliche Daten abzurufen (oder berechnen) Wenn dies erforderlich ist.

Es empfiehlt sich, dass Sie bei der Durchführung Leistungsanalyse testen und die Verwendung zu bestimmen, ob Pre-Population oder bei Bedarf Laden des Caches oder eine Kombination aus beiden, geeignet ist. Die Entscheidung sollte auf die Flüchtigkeit und die Verwendung eines Musters Daten basieren. Cache Nutzung und Leistung Analyse ist besonders wichtig in Clientanwendungen, die auftreten hoher Auslastung und hochgradig skalierbare werden müssen. Beispielsweise möglicherweise in hochgradig skalierbare Szenarien es Startwert für den Cache, um die Belastung Datenspeicher Höchstwert Vorkommen reduzieren sinnvoll.

Zwischenspeichern kann auch verwendet werden, um zu vermeiden, wiederholte Berechnungen, während die Anwendung ausgeführt wird. Wenn ein Vorgang Daten transformiert oder eine komplizierte Berechnung ausführt, können sie die Ergebnisse des Vorgangs im Cache speichern. Wenn dieselbe Berechnung danach erforderlich ist, kann die Anwendung einfach die Ergebnisse aus dem Cache abrufen.

Eine Anwendung kann Daten ändern, die in einem Cache vorhanden ist. Wir empfehlen jedoch als vorübergehende Datenspeicher, der zu einem beliebigen Zeitpunkt verschwinden konnte den Cache an. Speichern Sie wichtige Daten nicht nur im Cache. Stellen Sie sicher, dass Sie die Informationen in den ursprünglichen Datenspeicher ebenfalls verwalten. Dies bedeutet, dass, wenn der Cache nicht mehr verfügbar ist, Sie das Risiko von Datenverlusten minimieren.

### <a name="cache-highly-dynamic-data"></a>Cache hochgradig dynamische Daten

Wenn Sie sich schnell ändernden Informationen in einem beständigen Datenspeicher speichern, können sie zusätzlichen Aufwand für das System vorgangseinschränkung. Betrachten Sie beispielsweise ein Gerät, das Status oder einige andere Maße fortlaufend Berichte aus. Wenn eine Anwendung beschließt, dass diese Daten auf der Grundlage die zwischengespeicherte Informationen fast immer veraltet werden kann, die im cache, klicken Sie dann könnte desselben Betrags true, wenn speichern und diese Informationen aus dem Datenspeicher abrufen. In der Zeitaufwand zu speichern und diese Daten abrufen, es möglicherweise nicht geändert haben.

Sollten Sie in einer Situation wie folgt die Vorteile des Speicherns von dynamischen Informationen direkt im Cache statt in der beständigen Datenspeicher aus. Wenn die Daten nicht kritische und nicht erforderlich sind, Überwachung, ist Klicken Sie dann es unerheblich, ob die gelegentliche Änderung verloren.

### <a name="manage-data-expiration-in-a-cache"></a>Ablauf von Daten in einem Cache verwalten

In den meisten Fällen sind Daten, die in einem Cache gehalten werden eine Kopie der Daten, die in den ursprünglichen Datenspeicher vorhanden ist. Die Daten in den ursprünglichen Datenspeicher ändert sich möglicherweise, nachdem sie die bewirken, dass die zwischengespeicherten Daten veralten zwischengespeichert wurde. Viele Zwischenspeichern Systeme ermöglichen es Ihnen so konfigurieren Sie den Cache, um Daten ablaufen und den Zeitraum für den Daten nicht mehr aktuell eventuell zu verringern.

Wenn zwischengespeicherte Daten laufen ab, ist es aus dem Cache entfernt, und die Anwendung muss die Daten aus dem ursprünglichen Datenspeicher (sie können die Informationen neu abgerufen wieder Cache abzulegen) abrufen. Wenn Sie den Cache konfigurieren, können Sie eine Ablaufrichtlinie Standard festlegen. In vielen Cache Dienste können Sie auch vorschreiben, die Gültigkeitsdauer für einzelne Objekte beim programmgesteuert im Cache speichern.
Einige Caches aktivieren Sie die Gültigkeitsdauer angeben, als absoluten Wert oder einen gleitenden Wert, der bewirkt, dass das Element aus dem Cache entfernt werden, wenn es nicht innerhalb des angegebenen Zeitraums zugegriffen werden kann. Diese Einstellung überschreibt alle Cache organisationsweite Ablaufrichtlinie, allerdings nur für die angegebenen Objekte an.

> [AZURE.NOTE] Erwägen Sie die Gültigkeitsdauer für den Cache und die Objekte, die sie sorgfältig enthält. Wenn Sie es zu kurz vornehmen, Objekte zu schnell läuft, und werden Sie die Vorteile der Verwendung des Caches reduzieren. Wenn Sie den Zeitraum zu lang vornehmen, riskieren Sie die Daten veraltet sind.

Es ist es möglich, dass der Cache überfüllt möglicherweise, wenn Daten sehr lange resident bleiben darf. In diesem Fall möglicherweise keine Anfragen Hinzufügen neuer Einträge im Cache einige Elemente in einem Prozess bekannt als Entfernung zwangsweise entfernt werden. Cache Services entfernen in der Regel Daten auf Basis (LRU) kleinste zuletzt verwendete, aber Sie können in der Regel außer Kraft setzen dieser Richtlinie und verhindern, dass Elemente zu entfernende. Aber wenn Sie diesem Ansatz einführen, das Risiko von bis zu den Speicher, der im Cache zur Verfügung. Eine Ausnahme bei der Anwendung, die versucht, ein Element den Cache hinzuzufügen.

Einige Zwischenspeichern Implementierungen möglicherweise zusätzliche Entfernung Richtlinien bereit. Es gibt verschiedene Typen von Entfernung Richtlinien ein. Hierzu gehören:
- Eine zuletzt verwendete Richtlinie (in der Annahme, dass die Daten nicht erneut erforderlich sein werden).
- Eine First in First Out-Richtlinie (älteste Daten zuerst entfernt).
- Eine explizite freistellen-Richtlinie basierend auf einer ausgelöste Ereignis (beispielsweise die zu ändernden Daten).

### <a name="invalidate-data-in-a-client-side-cache"></a>Daten in einem Cache clientseitige ungültig

Daten, die in einem Cache clientseitige gehalten werden in der Regel gilt außerhalb der Zusammenarbeit mit der der Dienst, der die Daten an dem Client enthält. Ein Dienst kann nicht direkt einen Client hinzufügen oder Entfernen von Informationen aus einem Cache clientseitige erzwingen.

Dies bedeutet, dass für einen Client möglich, die einen schlecht konfigurierten Cache wird verwendet ist, um weiterhin veralteten Informationen verwenden. Angenommen, wenn die Ablaufrichtlinien des Caches ordnungsgemäß implementiert nicht zur Verfügung, möglicherweise ein Client veralteten Informationen verwenden, der lokal zwischengespeichert werden, wenn die Informationen in der ursprünglichen Datenquelle geändert hat.

Wenn Sie eine Webanwendung, die Daten über eine HTTP-Verbindung fungiert erstellen, können Sie implizit einen Webclient (wie ein Browser oder Webproxy) erzwingen, die neueste Informationen abgerufen werden sollen. Dies ist möglich, wenn eine Ressource durch eine Änderung in der URI der Ressource, die aktualisiert wird. Im Webclient in der Regel verwenden Sie den URI einer Ressource als Schlüssel im Cache clientseitige, damit ignoriert, wenn der URI verwandelt hat, der Webclient eine zuvor Cache Versionen einer Ressource und die neue Version stattdessen abgerufen.

## <a name="managing-concurrency-in-a-cache"></a>Verwalten von Parallelität in einem cache

Caches werden häufig von mehreren Instanzen einer Anwendung freigegeben werden soll. Jede Anwendungsinstanz zu lesen und Ändern von Daten im Cache. Der Fehler, die mit einem beliebigen freigegebenen Datenspeicher auftreten gelten daher auch für einen Cache. In einer Situation, in denen eine Anwendung muss zum Ändern von Daten, die im Cache vorhanden ist, müssen Sie sicherstellen, dass Aktualisierungen von eine Instanz der Anwendung nicht die von einer anderen Instanz vorgenommenen Änderungen zu überschreiben.

Abhängig von der Art der Daten und die Wahrscheinlichkeit, dass diese Konflikte können Sie eines der beiden Verfahren auf Concurrency übernehmen:

- __Optimistische.__ Unmittelbar vor dem Aktualisieren der Daten, überprüft die Anwendung, ob die Daten im Cache geändert haben, nachdem sie abgerufen wurden. Wenn die Daten nicht geändert hat, kann die Änderung vorgenommen werden. Andernfalls muss die Anwendung entscheiden, ob Sie ihn zu aktualisieren. (Die Geschäftslogik, die steuert dieser Entscheidung werden anwendungsspezifische.) Dieser Ansatz eignet sich für Situationen, in dem Updates seltene sind oder wo Konflikte wahrscheinlich nicht ausgeführt werden.
- __Pessimistische.__ Wenn sie die Daten abruft, sperrt die Anwendung es im Cache, um zu verhindern, dass eine andere Instanz ändern. Dieses Verfahren wird sichergestellt, dass Konflikte nicht durchgeführt werden können, doch können sie auch andere Instanzen, die zum Verarbeiten der gleichen Daten müssen blockieren. Eingeschränkte Parallelität kann Einfluss auf die der Skalierbarkeit einer Lösung, und es wird empfohlen, nur für kurzlebige Vorgänge. Dieser Ansatz ist für Situationen geeignet, in dem Konflikte sehr viel eher bereit, sind, insbesondere dann, wenn eine Anwendung aktualisiert wird, mehrere Elemente im Cache und muss sicherstellen, dass diese Änderungen konsistent angewendet werden.

### <a name="implement-high-availability-and-scalability-and-improve-performance"></a>Implementieren Sie hohe Verfügbarkeit und Skalierbarkeit und verbessern Sie der Leistung

Vermeiden Sie einen Cache als primärer Bestand Daten; Dies ist die Rolle des ursprünglichen Datenspeicher aus dem Cache gefüllt wird. Der ursprünglichen Datenspeicher ist für die Sicherstellung der Beibehaltung der Daten verantwortlich.

Achten Sie darauf, dass Sie nicht kritische Abhängigkeiten von die Verfügbarkeit von einer freigegebenen cachediensts in Ihrer Lösungen vorstellen. Eine Anwendung sollten funktionsfähiges fortsetzen, wenn der Dienst, der den freigegebenen Cache nicht verfügbar ist. Die Anwendung sollte nicht abteilungsdrucker oder ein Fehler auftreten, während Sie zum Fortsetzen des cachediensts warten.

Daher müssen Sie die Anwendung vorbereiten die Verfügbarkeit des cachediensts erkennen und liegen zurück zu den ursprünglichen Datenspeicher, wenn der Cache nicht zugegriffen werden kann. Die [Verbindung sprachabhängig Muster](http://msdn.microsoft.com/library/dn589784.aspx) eignet sich für dieses Szenario. Der Dienst, den Cache bereitstellt, wiederhergestellt werden kann, und, sobald sie verfügbar sind, der Cache aufgefüllt sein kann, wie Daten im ursprünglichen Datenspeicher, eine Strategie, wie etwa das [Cache-Aside Muster](http://msdn.microsoft.com/library/dn589799.aspx)folgen Formular gelesen werden.

Jedoch es möglicherweise Skalierbarkeit beeinträchtigt werden auf dem System, wenn die Anwendung wieder in den ursprünglichen Datenspeicher fällt, wenn der Cache vorübergehend nicht verfügbar ist.
Während der Datenspeicher wiederhergestellt wird, wird im ursprünglichen Datenspeicher mit Anforderung von Daten, sodass Zeitlimit swamped werden konnte und Fehler bei Verbindungen.

Sollten Sie einen lokalen Cache privaten in jede Instanz der Anwendung in Verbindung mit dem freigegebenen Cache, die alle Anwendungsinstanzen Zugriff auf implementieren. Wenn die Anwendung ein Element abruft, sie können überprüfen Sie zunächst im lokalen Cache aus, und klicken Sie dann in der freigegebenen zwischenspeichern, und klicken Sie abschließend in der ursprünglichen Daten gespeichert werden. Der lokale Cache kann mithilfe der Daten im freigegebenen Cache oder in der Datenbank, wenn der freigegebene Cache nicht verfügbar ist gefüllt werden.

Dieser Ansatz erfordert sorgfältige Konfiguration, um zu verhindern, dass den lokalen Cache zu veraltet sind in Bezug auf den freigegebenen Cache. Jedoch fungiert der lokale Cache als Puffer, wenn der freigegebene Cache nicht erreichbar ist. Abbildung 3 zeigt diese Struktur.

![Verwenden einen lokalen Cache privaten mit gemeinsam genutzten Cache](media/best-practices-caching/Caching3.png)
_Abbildung 3: verwenden einen lokalen Cache privaten mit gemeinsam genutzten Cache_

Um große Caches werden unterstützt, die relativ langer Lebensdauer Daten enthalten, bieten einige Dienste Cache eine hohe Verfügbarkeit-Option, die Automatisches Failover implementiert werden, wenn der Cache nicht mehr verfügbar ist. Dieser Ansatz in der Regel umfasst Replikation der Cache Daten, die auf einem Cacheserver primären zu einem sekundären Cacheserver gespeichert ist, und auf den sekundären Server wechseln, wenn der primäre Server fehlschlägt oder Connectivity geht verloren.

Klicken Sie zum Verringern der Wartezeit, die an mehrere Ziele schreiben zugeordnet hat möglicherweise die Replikation auf den sekundären Server asynchrone auftreten, wenn Daten auf dem primären Server in den Cache geschrieben werden. Dieser Ansatz führt zu die Möglichkeit, dass möglicherweise einige zwischengespeicherter Informationen verloren gehen, wenn ein Fehler aufgetreten, aber das Verhältnis dieser Daten kleine im Vergleich zu die Gesamtgröße des Caches sollten.

Wenn gemeinsam genutzten Cache groß ist, ist dies möglicherweise die zwischengespeicherten Daten über Knoten reduzieren die Wahrscheinlichkeit von Konflikten und gleichzeitig Skalierbarkeit unterteilen von Vorteil. Viele freigegebenen Caches unterstützen die Möglichkeit, Knoten dynamisch hinzufügen (und entfernen) und die Daten partitionsübergreifend neu zu verteilen. Dieser Ansatz möglicherweise in der wird die Auflistung von Knoten als einen einzelnen nahtlose Cache Clientanwendungen präsentiert umfassen Cluster. Intern ist jedoch die Daten Ihres zwischen Knoten eine vordefinierten Verteilung Strategie, die die Laden von gleichmäßig Salden folgen. Der [vorherigen Leitfaden Daten](http://msdn.microsoft.com/library/dn589795.aspx) auf der Microsoft-Website enthält weitere Informationen zu den möglichen Partitionierungsstrategien.

Cluster kann auch die Verfügbarkeit des Caches erhöhen. Wenn ein Knoten fehlschlägt, kann der Rest des Caches weiterhin zugegriffen werden.
Cluster wird häufig in Verbindung mit Replikation und Failover verwendet. Jeder Knoten repliziert werden kann, und das Replikat kann schnell online geschaltet werden, wenn der Knoten fehlschlägt.

Viele lesen und Schreibvorgänge sind wahrscheinlich einzelner Datenwerte oder Objekte (Variablen) haben. Jedoch möglicherweise gelegentlich sein können speichern oder große Datenmengen schnell abrufen.
Beispielsweise könnten sendet einen Cache gehören Hunderte oder Tausende von Elementen in den Cache schreiben. Eine Anwendung muss möglicherweise eine große Anzahl verknüpfter Artikel als Teil der gleichen Anforderung aus dem Cache abzurufen.

Stapel Operationen für folgende Zwecke bereitstellen, viele umfangreiche Caches. Dies ermöglicht eine Clientanwendung, um eine große Anzahl von Elementen in einer einzelnen Anforderung zu verpacken und reduziert den zusätzlichen Aufwand, der Durchführung einer großen Anzahl kleine Anfragen zugeordnet wurde.

## <a name="caching-and-eventual-consistency"></a>Zwischenspeichern und tatsächlichen Konsistenz

Für das Cache-Aside Muster entwickelt müssen die Instanz der Anwendung, die den Cache auffüllt Zugriff auf die zuletzt verwendete und konsistente Version der Daten aus. In einem System, die tatsächlichen Konsistenz (beispielsweise eine repliziert Datenspeicher) implementiert möglicherweise dies nicht der Fall sein.

Eine Instanz der Anwendung konnte ein Datenelement ändern und die zwischengespeicherte Version des betreffenden Elements ungültig. Eine andere Instanz der Anwendung möglicherweise versuchen, lesen diesen Artikel aus einem Cache, wodurch eine Cache-verpassen, damit die Daten aus dem Datenspeicher gelesen und es in den Cache fügt. Wenn der Datenspeicher vollständig nicht mit den anderen Replikaten synchronisiert wurde, konnte Instanz der Anwendung jedoch lesen und füllen den Cache mit dem alten Wert.

Weitere Informationen zum Behandeln von Konsistenz der Daten finden Sie unter der [Daten Konsistenz Einführung in](http://msdn.microsoft.com/library/dn589800.aspx) Seite auf der Microsoft-Website.

### <a name="protect-cached-data"></a>Schützen von zwischengespeicherten Daten

Unabhängig von den Cachedienst verwenden, sollten Sie zum Schutz der Daten, die im Cache vor unbefugtem Zugriff vorhanden ist. Es gibt zwei wesentlichen Bereiche an:

- Der Datenschutz der Daten im cache
- Der Schutz der Daten, wie er fließt zwischen dem Cache und die Anwendung, die den Cache verwendet wird

Zum Schützen von Daten im Cache könnte die cachediensts eine Authentifizierungsmethode implementiert werden, die erforderlich sind, dass Applikationen Folgendes angeben:
- Bestimmen, welche Identitäten Daten im Cache zugreifen können.
- Welche Vorgänge (Lesen und Schreiben), die diese Identitäten ausführen dürfen.

Klicken Sie zum Aufwand verringern, die weist zugeordnet lesen und Schreiben von Daten, nachdem eine Identität gewährt wurde, schreiben und/oder Lesezugriff auf den Cache, dass Identität alle Daten im Cache verwenden kann.

Wenn Sie zum Einschränken des Zugriffs auf eine Teilmenge der zwischengespeicherten Daten benötigen, können Sie eine der folgenden Aktionen ausführen:

- Teilen Sie den Cache in Partitionen (mithilfe von verschiedenen Cache Servers), und nur gewähren des Zugriffs auf Identitäten für die Partitionen, die sie Zugriff haben sollen, verwendet.
- Verschlüsseln Sie die Daten in jede Teilmenge mithilfe von anderen Tasten, und geben Sie den Schlüssel für die Verschlüsselung nur für Identitäten, die Zugriff auf jede Teilmenge sein soll. Eine Clientanwendung möglicherweise weiterhin alle Daten im Cache abrufen, aber es werden nur die Daten zu entschlüsseln, für die sie die Schlüssel hat.

Sie müssen die Daten auch schützen, wie er in und aus dem Cache fließt. Hierzu werden von der Netzwerkinfrastruktur, mit denen Clientanwendungen Verbindung mit dem Cache bereitgestellten Sicherheitsfeatures abhängig. Wenn der Cache mit einem Ort-Server innerhalb derselben Organisation, hostet die Clientanwendungen implementiert ist, möglicherweise dann die Isolation des Netzwerks selbst keine Sie zusätzliche Schritte erforderlich. Wenn der Cache Remote befindet und eine TCP- oder HTTP-Verbindung über ein öffentliches Netzwerk (z. B. im Internet erfordert), sollten Sie SSL implementieren.

## <a name="considerations-for-implementing-caching-with-microsoft-azure"></a>Aspekte bei der Implementierung mit Microsoft Azure Zwischenspeichern

Azure bietet Azure Redis Cache. Dies ist eine Implementierung der geöffneten Quelle Redis Cache, die als Dienst in einer Azure Datacenter ausgeführt wird. Es stellt einen Zwischenspeichern Dienst, der von einem beliebigen Azure-Anwendung zugegriffen werden kann, ob die Anwendung als einen Clouddienst, einer Website oder in einer Azure-virtuellen Computern implementiert wird. Caches können von Clientanwendungen verwendet werden, die die entsprechenden Zugriffstaste aufweisen.

Azure Redis Cache ist eine Lösung mit hoher Performance zwischenspeichern, die Verfügbarkeit, Skalierbarkeit und Sicherheit bereitstellt. Es wird in der Regel als Dienst auf einen oder mehrere dedizierte Computer verteilt ausgeführt. Er versucht, wie viele Informationen wie möglich im Speicher, um sicherzustellen, dass raschen Zugriff zu speichern. Diese Architektur soll niedriger Wartezeit und hohen Durchsatz bereitstellen, indem Sie die langsame e/a-Vorgänge ausführen müssen.

 Azure Redis Cache ist kompatibel mit vielen verschiedenen APIs, die von Clientanwendungen verwendet werden. Wenn Sie vorhandene Applikationen, die bereits Azure Redis Cache ausgeführt lokal verwenden verfügen, bietet der Azure Redis Cache einen schnelle Migrationspfad zum Zwischenspeichern von in der Cloud.

> [AZURE.NOTE] Azure bietet auch die Cachediensts verwaltet. Dieser Dienst basiert auf Azure Service Fabric-Cache-Engine. Sie können einen verteilten Cache erstellen, der von Applications lose gemeinsam verwendet werden können. Cache gehostet wird auf leistungsfähige Servern in einer Azure Datacenter ausgeführt.
Allerdings werden diese Option wird nicht mehr empfohlen und wird nur bereitgestellt werden, um vorhandene Anwendungen unterstützt, die erstellt wurden, um sie zu verwenden. Verwenden Sie für alle neuen Entwicklung stattdessen Azure Redis Cache.
>
> Darüber hinaus unterstützt Azure in Rolle Zwischenspeichern. Dieses Feature ermöglicht es Ihnen einen Cache zu erstellen, der auf einen Clouddienst spezifisch sind.
Der Cache von Instanzen von einer Rolle Web oder Arbeitskollegen gehostet wird, und nur von Rollen, die als Teil der gleichen Cloud-Dienst Bereitstellung Maßeinheit betrieben werden zugegriffen werden kann. (Eine Bereitstellungseinheit ist ein Festlegen der Rolleninstanzen, die als in einen bestimmten Bereich Cloud-Dienst bereitgestellt werden.) Ist der Cache gruppiert, und alle Instanzen der Rolle innerhalb derselben Bereitstellungseinheit, die den Cache für hostet als Teil des gleichen Cachecluster. Allerdings werden diese Option wird nicht mehr empfohlen und wird nur bereitgestellt werden, um vorhandene Anwendungen unterstützt, die erstellt wurden, um sie zu verwenden. Verwenden Sie für alle neuen Entwicklung stattdessen Azure Redis Cache.
>
> Sowohl Azure verwaltet Cachediensts und Azure In Rolle Cache sind derzeit für Rente auf 16. November 2016 geplant.
Es wird empfohlen, zu Azure Redis Cache in Vorbereitung für diese Einstellung von Websites zu migrieren. Weitere Informationen finden Sie auf der Seite   [Was ist Azure Redis Cache Angebot, und welche Größe sollte ich verwenden?](redis-cache/cache-faq.md#what-redis-cache-offering-and-size-should-i-use) auf der Microsoft-Website.


### <a name="features-of-redis"></a>Features von Redis

 Redis ist mit mehr als einem Cacheserver einfache. Es bietet eine verteilte in-Memory-Datenbank mit einer umfangreichen Befehlssatz, die viele häufige Szenarien unterstützt. Diese werden weiter unten in diesem Dokument im Abschnitt verwenden Redis Zwischenspeichern beschrieben. In diesem Abschnitt werden einige der wichtigsten Features, die Redis bereitstellt zusammengefasst.

### <a name="redis-as-an-in-memory-database"></a>Redis als in-Memory-Datenbank

Redis unterstützt beide lesen und Schreiben von Vorgängen. In Redis können schreibt Systemfehler entweder regelmäßig gespeichert werden, in einer lokalen Snapshot-Datei oder in einer Protokolldatei nur zum Anfügen geschützt werden. Dies ist nicht der Fall in vielen Caches (das vorübergehendes Datenspeicher berücksichtigt werden sollen).

 Alle schreibt sind asynchrone und Clients aus lesen und Schreiben von Daten werden nicht blockiert. Wenn Redis Starts ausgeführt, es liest die Daten aus der Momentaufnahme oder Log und zum Erstellen von in-Memory-Caches verwendet. Weitere Informationen finden Sie unter [Beibehaltung Redis](http://redis.io/topics/persistence) auf der Website Redis.

> [AZURE.NOTE] Redis garantiert nicht, dass alle schreibt bei einem Ausfall gespeichert, jedoch mit schlechtesten können Sie nur ein paar Sekunden im Wert von Daten verloren gehen. Denken Sie daran, dass ein Cache nicht als eine autoritative Datenquelle dienen vorgesehen ist, und es ist Aufgabe des Applications Cache verwenden, um sicherzustellen, dass wichtige Daten erfolgreich in einen entsprechenden Datenspeicher gespeichert ist. Weitere Informationen finden Sie unter den [Cache-Aside Muster](http://msdn.microsoft.com/library/dn589799.aspx).

#### <a name="redis-data-types"></a>Redis Datentypen

Redis ist ein Schlüssel-Wert-Speicher, in dem Werte einfache Typen oder komplexe Datenstrukturen, z. B. Hashes, Listen enthalten können und festgelegt. Eine Reihe von atomaren Operationen für diese Datentypen unterstützt. Tasten kann permanent oder markiertes mit eine begrenzte Time-to-live, an welcher Stelle die Taste und den entsprechenden Wert automatisch aus dem Cache entfernt werden. Weitere Informationen zu Redis Schlüssel und Werte finden Sie auf der Seite [eine Einführung in Datentypen und Abstraktionen Redis](http://redis.io/topics/data-types-intro) auf der Website Redis.

#### <a name="redis-replication-and-clustering"></a>Redis Replikation und Cluster

Redis unterstützt die Master-/untergeordnete Replikation Sicherstellung der Verfügbarkeit und Wartung Durchsatz. Schreiben Sie, dass Vorgänge zu einem Redis master-Knoten in einem oder mehreren untergeordneten Knoten repliziert werden. Lesen Vorgänge können das Master-Shape oder eine Untergebenen realisiert werden.

Im Falle einer Partition Netzwerk können Untergebenen weiterhin Daten dienen und anschließendes transparent erneutes synchronisieren mit das Master-Shape, wenn die Verbindung wiederhergestellt ist. Weitere Details finden Sie auf der Seite [Replikation](http://redis.io/topics/replication) auf der Website Redis.

Redis stellt auch Cluster, Sie transparent Daten in mehrere Shards hinweg auf Servern und die Last verteilt. Dieses Feature erhöht Skalierbarkeit, da neue Redis Server hinzugefügt werden können, und tragen die Daten als die Größe des Caches neu partitioniert.

Darüber hinaus kann jeder Server im Cluster mithilfe von Master-/untergeordnete Replikation repliziert werden. Dadurch wird die Verfügbarkeit sichergestellt, über die einzelnen Knoten im Cluster. Weitere Informationen zu Cluster und Sharding finden Sie auf der [Redis Cluster Lernprogramm-Seite](http://redis.io/topics/cluster-tutorial) auf der Website Redis.

### <a name="redis-memory-use"></a>Redis Arbeitsspeicher verwenden

Ein Redis Cache ist begrenzte Größe, von die die auf dem Host verfügbaren Ressourcen abhängig. Wenn Sie auf einen Server Redis konfigurieren, können Sie die maximale Speichermenge angeben, die sie verwenden kann. Sie können auch einen Schlüssel konfigurieren, in einem Redis Cache eine Uhrzeit Ablauf haben, nach der sie automatisch aus dem Cache entfernt wird. Dieses Feature kann in-Memory-Caches aus Auffüllen mit alten oder veralteten Daten verhindern.

Wie Arbeitsspeicher voll ist, kann Redis automatisch Schlüssel und deren Werte entfernen anhand einer Anzahl von Richtlinien. Die Standardeinstellung ist LRU (mindestens zuletzt verwendet), aber Sie können auch andere Richtlinien wie Schlüssel zufällig entfernen oder Deaktivieren der Entfernung ganz (welche, Groß-/Kleinschreibung Versuche, den Cache Fail Elemente hinzugefügt werden, wenn er voll ist) auswählen. Die Seite [Als LRU-Cache Redis verwenden](http://redis.io/topics/lru-cache) enthält weitere Informationen.

### <a name="redis-transactions-and-batches"></a>Redis Transaktionen und Blattnamen

Redis ermöglicht eine Clientanwendung und übermitteln Sie eine Reihe von Vorgängen, das Lesen und Schreiben von Daten im Cache als eine atomarische Transaktion. Alle Befehle in der Transaktion werden garantiert sequenziell ausgeführt, und werden keine Befehle ausgestellt von anderen gleichzeitigen Clients dazwischen interwoven werden.

Diese sind jedoch nicht wahr Transaktionen während eine relationale Datenbank, die sie ausführen möchten. Verarbeitung von Transaktionen besteht aus zwei Phasen – die erste ist, wenn die Befehle sind in der Warteschlange, und das zweite ist, wenn die Befehle ausgeführt werden. In welcher Phase Befehl Einfügen werden die Befehle, die die Transaktion umfassen vom Client übermittelt. Wenn einige Art von Fehler auftritt zu diesem Zeitpunkt (z. B. einen Syntaxfehler, oder die falsche Anzahl von Parametern) dann Redis einige Lösungsvorschläge: die gesamte Transaktion Verarbeitungszeit und verwirft diese.

Während der Phase ausführen Führt Redis jeder Befehl in der Warteschlange nacheinander. In dieser Phase ein Befehls schlägt fehl, Redis weiterhin mit den nächsten Befehl in der Warteschlange und werden nicht zurücksetzen die Auswirkungen alle Befehle, die bereits ausgeführt haben. Diese vereinfachte Form des Transaktion hindert Sicherstellung der Leistung und vermeiden Sie Leistungsprobleme zu vermeiden, die durch die Konflikte verursacht werden.

Redis implementiert ein Formular zur Unterstützung bei der Verwaltung von Konsistenz optimistischen Sperren. Ausführliche Informationen zu Transaktionen und Sperren mit Redis finden Sie auf der [Seite Transaktionen](http://redis.io/topics/transactions) auf der Website Redis.

Redis unterstützt auch ausgelegt Batchverarbeitung von Serviceanfragen. Das Redis-Protokoll, das zum Senden von Befehlen auf einem Server Redis verwenden Clients kann ein Client eine Reihe von Vorgängen als Teil der gleichen Anforderung senden. Dies kann dazu beitragen um Paketfragmentierung im Netzwerk zu verringern. Wenn der Stapel verarbeitet wird, wird jeder Befehl ausgeführt. Wenn Sie einen der folgenden Befehle fehlerhaft sind, diese abgelehnt (dies mit einer Transaktion nicht), aber die verbleibenden Befehle ausgeführt werden. Es gibt auch keine Garantie über die Reihenfolge, in der die Befehle in den Stapel verarbeitet werden sollen.

### <a name="redis-security"></a>Redis Sicherheit

Redis liegt der Schwerpunkt rein auf den schnellen Zugriff auf Daten, und kann nur innerhalb einer vertrauenswürdigen Umgebung ausgeführt werden, die nur von vertrauenswürdigen Clients zugegriffen werden kann. Redis unterstützt ein eingeschränkte Sicherheitsmodell basierend auf Kennwortauthentifizierung. (Es ist möglich, Authentifizierung vollständig zu entfernen, auch wenn dies empfohlen wird.)

Alle authentifizierten Clients dasselbe Kennwort globale freigeben und Zugang zu dem dieselben Ressourcen. Wenn Sie umfassendere Sicherheit benötigen, müssen Sie eigene Sicherheitsstufe vor dem Server Redis implementieren und Client-Anfragen sollte diese zusätzlichen Schicht passieren. Redis sollte nicht direkt mit nicht vertrauenswürdigen oder nicht authentifizierte Clients verfügbar gemacht werden.

Sie können Befehle Zugriff einschränken, indem sie zu deaktivieren oder diese umbenennen (und nur berechtigte Clients mit den neuen Namen).

Redis unterstützt nicht direkt irgendeiner Weise verschlüsselt Daten, damit alle Codieren von Clientanwendungen ausgeführt werden muss. Darüber hinaus bietet Redis keine Art von Transport Sicherheit. Wenn Sie Daten schützen, wie er im Netzwerk fließt müssen, wird empfohlen, einen Proxy SSL implementieren.

Weitere Informationen finden Sie auf der Seite [Redis Sicherheit](http://redis.io/topics/security) auf der Website Redis.

> [AZURE.NOTE] Azure Redis Cache bietet eine eigene Sicherheitsstufe, über den Clients eine Verbindung herstellen. Die zugrunde liegenden Redis Server sind mit dem öffentlichen Netzwerk nicht verfügbar.

### <a name="using-the-azure-redis-cache"></a>Verwenden des Caches Azure Redis

Redis Cache Azure ermöglicht den Zugriff auf Redis-Servern, die auf Servern gehostet, die an einer Azure Datencenter mit; Sie fungiert als Fassade, die Steuerung des Benutzerzugriffs und Sicherheit bereitstellt. Sie können einen Cache mithilfe der Azure-Verwaltungsportal bereitstellen. Im Portal bietet vordefinierte Konfigurationen, die im Bereich von einem 53GB Cache als dedizierten Dienst, den ohne Replikation (keine Garantie für Verfügbarkeit) ausgeführt wird, klicken Sie auf freigegebene Hardware unterstützt SSL-Kommunikation (für private) und Master-/untergeordnete Replikation mit einer Vereinbarung zum SERVICELEVEL von 99,9 % Verfügbarkeit, nach unten zu einem 250 MB Zwischenspeichern ausführen.

Mithilfe der Azure-Verwaltungsportal an, können Sie auch die Richtlinie Entfernung des Caches konfigurieren und Steuern des Zugriffs auf den Cache durch Hinzufügen von Benutzern, die die Rollen zur Verfügung gestellt; Besitzer, Mitwirkender Reader. Diese Rollen definieren die Vorgänge, die Mitglieder ausführen können. Beispielsweise Mitglieder der Rolle Besitzer verfügen über den Cache (einschließlich Sicherheit) und deren Inhalt Kontrolle, Mitglied der Rolle "Mitwirkender" Lesen und Schreiben von Informationen im Cache können und Mitglied der Rolle Leser nur Daten aus dem Cache abrufen können.

Die meisten Verwaltungsaufgaben über die Azure-Verwaltungsportal durchgeführt werden und daher, dass viele administrative Befehle zur Verfügung, in der Standardversion von Redis nicht verfügbar, einschließlich der Möglichkeit zum programmgesteuerten, ändern Sie die Konfiguration sind konfigurieren war(en) der Server Redis zusätzliche Slaves oder zwangsweise Daten auf einem Datenträger speichern.

Der Azure-Verwaltungsportal umfasst geeignete grafisch dargestellt, die Ihnen die Leistung des Caches überwachen ermöglicht. Beispielsweise können Sie die Anzahl der Verbindungen gemacht wird anzeigen, die Anzahl der Anfragen ausgeführt, die Lautstärke der Lese- und schreibt, und die Anzahl der Cache Treffer im Vergleich zu-Cache-Fehler. Verwenden diese Informationen, die Sie ermitteln können, die Effektivität des Caches und gegebenenfalls zu einer anderen Konfiguration wechseln oder die Richtlinie Entfernung ändern. Darüber hinaus können Sie Benachrichtigungen erstellen, die e-Mail-Nachrichten an den Administrator senden, wenn eine oder mehrere kritische Kriterien außerhalb eines bestimmten liegen. Beispielsweise überschreitet die Anzahl der Cachefehler einen angegebenen Wert in der letzten Stunde, könnte ein Administrator darüber benachrichtigt, Cache möglicherweise zu klein oder Daten möglicherweise werden zu entfernende zu schnell.

Sie können auch CPU, Arbeitsspeicher und Auslastung des Netzwerks für den Cache überwachen.

Besuchen Sie für Weitere Informationen sowie Beispiele zur Verwendung erstellen und Konfigurieren einer Azure Redis Cache die Seite [runde um Azure Redis Cache](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) Azure Blog ein.

## <a name="caching-session-state-and-html-output"></a>Zwischenspeichern Sitzungszustand und seiner ursprünglichen HTML-Ausgabe

Wenn Sie das Erstellen von ASP.NET Applications web, mithilfe von Azure Webrollen ausführen, Statusinformationen und HTML-Ausgabe in einem Azure Redis Cache gespeichert werden kann. Die Session State Provider für Azure Redis Cache ermöglicht es Ihnen, die Sitzungsinformationen zwischen verschiedenen Instanzen einer ASP.NET Web-Anwendung gemeinsam nutzen und äußerst in Webfarm Situationen, wo Client-/ Server-Zugehörigkeit steht nicht zur Verfügung und Sitzung Daten im Arbeitsspeicher Zwischenspeichern wäre nicht geeignet, ist.

Verwenden die Session State Provider mit Azure Redis Cache bietet mehrere Vorteile, einschließlich:

- Bundesstaat Sitzung, dem eine große Anzahl von Instanzen einer ASP.NET Web-Anwendung, verbesserte Skalierbarkeit bereitstellen gemeinsam verwendet werden kann,
- Es unterstützt gesteuert, gleichzeitigen Zugriff auf derselben Sitzung Zustandsdaten für mehrere Reader und einem einzelnen Autor, und
- Sie können Komprimierung Arbeitsspeicher speichern und verbessern die Leistung des Netzwerks.

Weitere Informationen finden Sie auf der Microsoft-Website auf der Seite [ASP.NET Session State Provider für Azure Redis Cache](redis-cache/cache-aspnet-session-state-provider.md) .

> [AZURE.NOTE] Verwenden Sie nicht die Session State Provider für Azure Redis Cache für ASP.NET Applications, die außerhalb der Azure-Umgebung ausgeführt werden. Die Wartezeit für den Zugriff auf den Cache anhand von Azure kann die Leistungsvorteile von Zwischenspeichern von Daten beseitigen.

Auf ähnliche Weise ermöglicht die Ausgabe-Cache-Anbieter für Azure Redis Cache, die von einer ASP.NET Web-Anwendung generierten HTTP-Antworten zu speichern. Mit dem Ausgabe-Cache-Anbieter mit Azure Redis Cache kann die Reaktionszeiten von Applications verbessern, die komplexe HTML-Ausgabe gerendert werden; Anwendungsinstanzen Generieren von ähnliche Antworten vornehmen können von der freigegebenen Ausgabe Fragmenten in dieser Ausgabe neuem HTML-Code generieren, statt der Cache verwenden.  Weitere Informationen finden Sie auf der Seite [ASP.NET Ausgabe-Cache-Anbieter für Azure Redis Cache](redis-cache/cache-aspnet-output-cache-provider.md) auf der Microsoft-Website.

### <a name="azure-redis-cache"></a>Azure Redis cache

Azure Redis Cache ermöglicht den Zugriff auf Redis-Servern, die an einer Azure Datencenter gehostet werden. Sie fungiert als Fassade, die Steuerung des Benutzerzugriffs und Sicherheit bereitstellt. Sie können einen Cache mithilfe des Azure-Portals bereitstellen.

Das Portal bietet eine Reihe von vordefinierten Konfigurationen vornehmen. Dieser Bereich aus einem 53 GB Cache als dedizierten Dienst ausführen unterstützt, die SSL-Kommunikation (für private) und Master-/untergeordnete Replikation mit einer Vereinbarung zum SERVICELEVEL von 99,9 % Verfügbarkeit, auf einen 25 0 MB Cache ohne Replikation (keine Garantie für Verfügbarkeit) auf freigegebenen Hardware ausgeführt werden.

Verwenden des Azure-Portals an, können Sie auch konfigurieren Sie die Richtlinie Entfernung des Caches und Steuern des Zugriffs auf den Cache durch Hinzufügen von Benutzern, die die Rollen bereitgestellt.  Diese Funktionen, die die Vorgänge zu, die Mitglieder ausführen können definieren, gehören Besitzer, Mitwirkender und Reader. Beispielsweise Mitglieder der Rolle Besitzer verfügen über den Cache (einschließlich Sicherheit) und deren Inhalt Kontrolle, Mitglied der Rolle "Mitwirkender" Lesen und Schreiben von Informationen im Cache können und Mitglied der Rolle Leser nur Daten aus dem Cache abrufen können.

Die meisten Verwaltungsaufgaben werden über das Portal Azure durchgeführt. Aus diesem Grund sind viele administrative Befehle, die in der Standardversion von Redis sind, nicht verfügbar, einschließlich der Möglichkeit, ändern Sie die Konfiguration programmgesteuert, fahren Sie den Server Redis, zusätzliche Untergebenen konfigurieren oder zwangsweise Daten auf einem Datenträger speichern.

Azure-Portal umfasst geeignete grafisch dargestellt, die Ihnen die Leistung des Caches überwachen ermöglicht. Beispielsweise können Sie die Anzahl der Verbindungen vorgenommen, die Anzahl der Anfragen durchgeführt werden, die Lautstärke der Lese- und schreibt, und die Anzahl der Cachetreffer im Vergleich zu Cachefehler anzeigen. Anhand dieser Informationen können Sie bestimmen die Effektivität des Caches und falls erforderlich, wechseln Sie zu einer anderen Konfiguration, oder ändern Sie die Richtlinie Entfernung.

Darüber hinaus können Sie Benachrichtigungen erstellen, die e-Mail-Nachrichten an den Administrator senden, wenn eine oder mehrere kritische Kriterien außerhalb eines bestimmten liegen. Angenommen, möchten Sie einen Administrator benachrichtigen, wenn die Anzahl der Cachefehler einen angegebenen Wert in der letzten Stunde, überschreitet, da dies bedeutet Cache möglicherweise zu klein oder Daten möglicherweise werden zu entfernende zu schnell.

Sie können auch die CPU, Arbeitsspeicher und Auslastung des Netzwerks für den Cache überwachen.

Besuchen Sie für Weitere Informationen sowie Beispiele zur Verwendung erstellen und Konfigurieren einer Azure Redis Cache die Seite [runde um Azure Redis Cache](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) Azure Blog ein.

## <a name="caching-session-state-and-html-output"></a>Zwischenspeichern Sitzungszustand und seiner ursprünglichen HTML-Ausgabe

Wenn Sie erstellen besagen ASP.NET Web Applications, mithilfe von Azure Webrollen ausführen, Sitzung gespeichert werden kann, Informationen und HTML-Ausgabe in einem Azure Redis Cache. Der Sitzung Bundesstaat-Anbieter für Azure Redis Cache ermöglicht es Ihnen, die Sitzungsinformationen zwischen verschiedenen Instanzen einer ASP.NET Web-Anwendung gemeinsam nutzen und äußerst in Webfarm Situationen, wo Client-/ Server-Zugehörigkeit steht nicht zur Verfügung und Sitzung Daten im Arbeitsspeicher Zwischenspeichern wäre nicht geeignet, ist.

Verwenden dem Sitzung Zustand Anbieter mit Azure Redis Cache Funktionsumfang mehrere Vorteile, einschließlich:

- Freigabe von Sitzungsstatus mit einer großen Anzahl der Instanzen des ASP.NET Web Applications.
- Bereitstellen von verbesserte Skalierbarkeit.
- Unterstützung von gesteuert, gleichzeitige Access auf der gleichen Sitzung Zustandsdaten für mehrere Reader und einem einzelnen Autor.
- Verwenden Komprimierung Arbeitsspeicher speichern und verbessern die Leistung des Netzwerks ein.

Weitere Informationen finden Sie auf der Microsoft-Website auf der Seite [ASP.NET Sitzung Bundesstaat-Anbieter für Azure Redis Cache](redis-cache/cache-aspnet-session-state-provider.md) .

> [AZURE.NOTE] Verwenden Sie nicht den Sitzung Bundesstaat-Anbieter für Azure Redis Cache mit ASP.NET Applications, die außerhalb der Azure-Umgebung ausgeführt werden. Die Wartezeit für den Zugriff auf den Cache anhand von Azure kann die Leistungsvorteile von Zwischenspeichern von Daten beseitigen.

Auf ähnliche Weise ermöglicht der Ausgabe-Cache-Anbieter für Azure Redis Cache, die von einer ASP.NET Web-Anwendung generierten HTTP-Antworten zu speichern. Mit dem Ausgabe-Cache-Anbieter mit Azure Redis Cache kann die Reaktionszeiten von Applications verbessern, die komplexe HTML-Ausgabe gerendert werden. Anwendungsinstanzen, die ähnliche Antworten generieren umso Verwendung von der freigegebenen Ausgabe Satzteile in den Cache generiert, sondern dieser HTML-Code ausgeben neuem an. Weitere Informationen finden Sie auf der Microsoft-Website auf der Seite [ASP.NET Ausgabe-Cache-Anbieter für Azure Redis Cache](redis-cache/cache-aspnet-output-cache-provider.md) .

## <a name="building-a-custom-redis-cache"></a>Erstellen einen benutzerdefinierten Redis cache

Azure Redis Cache fungiert als Fassade zu den zugrunde liegenden Redis-Servern. Aktuell eine feste Anzahl von Konfigurationen unterstützt, aber bietet keine Redis Cluster wird. Wenn Sie eine erweiterte Konfiguration erforderlich, die nicht über den Cache Azure Redis (beispielsweise ein Cache größer als 53 GB) sind können erstellen und Hosten Ihrer eigenen Redis-Servern mithilfe von Azure-virtuellen Computern.

Dies ist ein potenziell komplexer Prozess, denn Sie müssen möglicherweise mehrere virtuelle Computer als master und untergeordneten Knoten dienen soll Implementieren der Replikation erstellen. Darüber hinaus, wenn Sie einen Cluster erstellen möchten, benötigen Sie mehrere Master-Shapes und untergeordneten Servern. Eine minimale gruppierte Replikation Suchtopologie, die eine hohe Verfügbarkeit und Skalierbarkeit bietet umfasst mindestens sechs virtuellen Computern als drei Paare von Master-/untergeordnete Servers (ein Cluster muss mindestens drei master-Knoten enthalten) angeordnet.

Jedes Paar Master-/untergeordnete sollten eng nebeneinander befinden, um Wartezeit zu minimieren. Jeden Satz von Paare kann jedoch in anderen Azure Rechenzentren in unterschiedlichen Regionen ansässig ausgeführt werden, wenn zwischengespeicherte Daten in der Nähe der Anwendungen zu suchen, die Möglichkeit zur gemeinsamen Nutzung höchstwahrscheinlich werden soll. Die Seite [Ausführen einer CentOS Linux virtuellen Computers in Azure Redis](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) auf der Microsoft-Website führt durch ein Beispiel, das zeigt, wie das Erstellen und Konfigurieren eines Redis Knotens als eine Azure-virtuellen Computer ausgeführt.

[AZURE.NOTE] Bitte beachten Sie, wenn Sie Ihren eigenen Redis Cache auf diese Weise implementieren, Überwachung, verwalten und Schützen des Diensts verantwortlich werden.

## <a name="partitioning-a-redis-cache"></a>Einen Redis Cache Partitionierung

Cache Partitionierung umfasst die Aufteilung Cache auf mehreren Computern. Diese Struktur hat mehrere Vorteile über mit einem einzelnen Cacheserver, einschließlich:

- Erstellen einen Cache, ist sehr viel größer als auf einem Server gespeichert werden können.
- Verteilen von Daten auf Servern, Verbessern der Verfügbarkeit aus. Wenn ein Server fehlschlägt oder nicht zugegriffen werden mehr, können die Daten, die ihn enthält nicht verfügbar ist, aber die Daten auf den verbleibenden Servern weiterhin zugegriffen werden. Für einen Cache ist dies nicht entscheidend, da die zwischengespeicherten Daten nur eine vorübergehende Kopie der Daten, die in einer Datenbank gespeichert ist. Zwischengespeicherte Daten auf einem Server, der nicht mehr verfügbar ist, können stattdessen auf einem anderen Server zwischengespeichert werden.
- Verteilen die Last auf Servern, wodurch die Leistung und Skalierbarkeit verbessert.
- Geolocating Daten an die Benutzer, die darauf zugreifen, wodurch Wartezeit schließen.

Für einen Cache ist die am häufigsten verwendete Form Aufteilung Sharding. Bei dieser Strategie ist jedes Partition (oder Shard) einen Redis Cache in einem eigenen rechts aus. Daten ist auf einer bestimmten Partition geleitet, mithilfe von Sharding Logik, die unterschiedliche Methoden zum Verteilen von Daten verwenden können. Das [Sharding Muster](http://msdn.microsoft.com/library/dn589797.aspx) enthält weitere Informationen zum Sharding implementieren.

Zum Implementieren der Partitionierung in einem Cache Redis, können Sie eine der folgenden Vorgehensweisen ausführen:

- _Serverseitige Abfrage routing._ Bei diesem Verfahren sendet eine Clientanwendung eine Anforderung an einen beliebigen Redis Server, die den Cache (wahrscheinlich den nächsten Server) umfassen. Jeder Redis-Server speichert Metadaten, die die Partition, es enthält, und auch enthält Informationen darüber, welche Partitionen befinden, klicken Sie auf andere Server zu beschreiben. Der Server Redis untersucht die Clientanforderung. Wenn sie lokal aufgelöst werden kann, werden sie diesen Vorgang. Andernfalls wird er die Anforderung an den entsprechenden Server weiterleiten. Dieses Modell wird von Redis Cluster implementiert und wird auf der Seite [Redis Cluster Lernprogramm](http://redis.io/topics/cluster-tutorial) auf der Website Redis ausführlicher beschrieben. Redis Cluster ist für Clientanwendungen transparent und zusätzliche Redis Server Cluster (und die Daten erneut aufgeteilt) hinzugefügt werden können, ohne dass die Clients neu zu konfigurieren.

- _Clientseitige Partitionierung._ In diesem Modell enthält die Clientanwendung Logik (möglicherweise in Form einer Dokumentbibliothek), die Anfragen weiterleitet an den entsprechenden Redis Server. Dieser Ansatz kann mit Azure Redis Cache verwendet werden. Erstellen Sie mehrere Azure Redis Cache (einer für jede Datenpartition) und Implementieren der clientseitige Logik, die die Anforderungen an die richtige Cache weiterleitet. Wenn das Partitionierungsschema ändert (Wenn beispielsweise zusätzliche Azure Redis Caches erstellt wurden), müssen Clientanwendungen neu konfiguriert werden.

- _Proxy-eingesetzten unterstützt Partitionierung._ In diesem Schema Client Applications senden an einen temporären Proxydienst, der versteht, wie die Daten aufgeteilt ist anfordert, und klicken Sie dann leitet die Anforderung an die entsprechende Redis-Server. Dieser Ansatz kann auch mit Azure Redis Cache verwendet werden. der Proxy-Dienst kann als Azure-Cloud-Dienst implementiert werden. Dieser Ansatz erfordert eine zusätzliche Sicherheitsstufe Komplexität den Dienst implementiert wird, und Serviceanfragen mehr Zeit in Anspruch als mit clientseitig Partitionierung ausführen.

Die Seite [Partitioning: zum Teilen von Daten zwischen mehreren Instanzen von Redis](http://redis.io/topics/partitioning) der Redis Website bietet weitere Informationen zum Verteilen mit Redis implementieren.

### <a name="implement-redis-cache-client-applications"></a>Implementieren der Redis-Cache-Clientanwendungen

Redis unterstützt Clientanwendungen in mehreren programming Sprachen verfasst. Wenn Sie neue Applikationen mithilfe von .NET Framework erstellen, besteht darin empfiehlt es sich, die StackExchange.Redis-Client-Bibliothek verwenden. Diese Bibliothek bietet eine .NET Framework-Objektmodell, das die Details zum Herstellen einer Verbindung mit einem Server Redis, Befehle senden und Empfangen von Antworten fasst. Es ist als Paket NuGet in Visual Studio zur Verfügung. Diese gleichen Bibliothek können Sie die Verbindung mit einer Azure Redis Cache oder einen benutzerdefinierten Redis Cache eines virtuellen Computers gehostet.

Verbindung zu einem Server Redis, die Sie mithilfe der statischen `Connect` der Methode der `ConnectionMultiplexer` Class. Die Verbindung aus, die diese Methode erstellt während der gesamten Dauer der Clientanwendung verwendet werden soll, und die gleiche Verbindung von mehreren gleichzeitigen Threads verwendet werden kann. Verbinden Sie und trennen Sie die Funktion Redis ausführen, da dies die Leistung beeinträchtigen kann jeder nicht.

Sie können die Verbindungsparameter, beispielsweise die Adresse der Host Redis und das Kennwort angeben. Wenn Sie Azure Redis Cache verwenden, ist das Kennwort entweder der primären oder sekundären Schlüssel, der für Azure Redis Cache mithilfe der Azure-Verwaltungsportal generiert wird.

Nachdem Sie auf dem Server Redis verbunden haben, können Sie einen Ziehpunkt in der Datenbank Redis ermitteln, die als Cache fungiert. Stellt die Verbindung Redis der `GetDatabase` Methoden für diese Aufgabe. Dann können Sie Elemente aus dem Cache abrufen und Speichern von Daten im Cache mithilfe der `StringGet` und `StringSet` Methoden. Diese Methoden erwarten einen Schlüssel als Parameter und geben Sie dem Element entweder im Cache, der einen übereinstimmenden Wert (`StringGet`) oder das Element dem Cache mit dieser Taste hinzufügen (`StringSet`).

Je nach Standort des Servers Redis möglicherweise viele Vorgänge entstehen einige Wartezeit ist eine Anforderung an den Server übertragen und eine Antwort an den Client zurückgegeben wird. Asynchrone Versionen viele Methoden, die um Hilfe-Clientanwendungen reagiert bleiben verfügbar gemacht stellt die StackExchange-Bibliothek. Diese Methoden unterstützt das [Asynchrone Muster aufgabenbasierten](http://msdn.microsoft.com/library/hh873175.aspx) in .NET Framework.

Der folgende Codeausschnitt zeigt eine Methode namens `RetrieveItem`. Es wird eine Implementierung des Musters Cache-Aside basierend auf Redis und der Bibliothek StackExchange veranschaulicht. Die Methode akzeptiert einen Schlüsselwert Zeichenfolge und versucht, das entsprechende Element aus dem Cache Redis abzurufen, die durch Aufrufen der `StringGetAsync` Methode (die asynchrone Version der `StringGet`).

Wenn das Element nicht gefunden wird, wird dem Abruf aus der zugrunde liegenden Datenquelle mithilfe der `GetItemFromDataSourceAsync` Methode (also eine lokale Methode und nicht Teil der Bibliothek StackExchange). Es wird dann zum Cache hinzugefügt, mithilfe der `StringSetAsync` Methode, sodass es beim nächsten schneller abgerufen werden kann.

```csharp
// Connect to the Azure Redis cache
ConfigurationOptions config = new ConfigurationOptions();
config.EndPoints.Add("<your DNS name>.redis.cache.windows.net");
config.Password = "<Redis cache key from management portal>";
ConnectionMultiplexer redisHostConnection = ConnectionMultiplexer.Connect(config);
IDatabase cache = redisHostConnection.GetDatabase();
...
private async Task<string> RetrieveItem(string itemKey)
{
    // Attempt to retrieve the item from the Redis cache
    string itemValue = await cache.StringGetAsync(itemKey);

    // If the value returned is null, the item was not found in the cache
    // So retrieve the item from the data source and add it to the cache
    if (itemValue == null)
    {
        itemValue = await GetItemFromDataSourceAsync(itemKey);
        await cache.StringSetAsync(itemKey, itemValue);
    }

    // Return the item
    return itemValue;
}
```

Die `StringGet` und `StringSet` Methoden sind nicht auf Abrufen oder speichern Zeichenfolgenwerte beschränkt. Sie können alle Elemente übernehmen, die als ein Array von Bytes serialisiert wird. Wenn Sie ein Objekt .NET speichern müssen, können Sie es als Bytestream serialisieren und Verwenden der `StringSet` Methode, um es in den Cache zu schreiben.

Sie können ebenso ein Objekt aus dem Cache lesen, mithilfe der `StringGet` Methode und es als Objekt .NET deserialisieren. Im folgenden Code wird eine Reihe von Erweiterungsmethoden für die Benutzeroberfläche IDatabase (die `GetDatabase` Methode einer Verbindung Redis gibt eine `IDatabase` Objekt), und entsprechendem Code, der Stichprobe, die folgenden Methoden zum Lesen und Schreiben verwendet eine `BlogPost` Objekt im Cache:

```csharp
public static class RedisCacheExtensions
{
    public static async Task<T> GetAsync<T>(this IDatabase cache, string key)
    {
        return Deserialize<T>(await cache.StringGetAsync(key));
    }

    public static async Task<object> GetAsync(this IDatabase cache, string key)
    {
        return Deserialize<object>(await cache.StringGetAsync(key));
    }

    public static async Task SetAsync(this IDatabase cache, string key, object value)
    {
        await cache.StringSetAsync(key, Serialize(value));
    }

    static byte[] Serialize(object o)
    {
        byte[] objectDataAsStream = null;

        if (o != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream())
            {
                binaryFormatter.Serialize(memoryStream, o);
                objectDataAsStream = memoryStream.ToArray();
            }
        }

        return objectDataAsStream;
    }

    static T Deserialize<T>(byte[] stream)
    {
        T result = default(T);

        if (stream != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream(stream))
            {
                result = (T)binaryFormatter.Deserialize(memoryStream);
            }
        }

        return result;
    }
}
```

Der folgende Code zeigt eine Methode namens `RetrieveBlogPost` , die diese Erweiterungsmethoden zum Lesen und Schreiben einer serialisierbaren verwendet `BlogPost` Objekt im Cache, die dem Cache-Aside Muster folgen:

```csharp
// The BlogPost type
[Serializable]
private class BlogPost
{
    private HashSet<string> tags = new HashSet<string>();

    public BlogPost(int id, string title, int score, IEnumerable<string> tags)
    {
        this.Id = id;
        this.Title = title;
        this.Score = score;
        this.tags = new HashSet<string>(tags);
    }

    public int Id { get; set; }
    public string Title { get; set; }
    public int Score { get; set; }
    public ICollection<string> Tags { get { return this.tags; } }
}
...
private async Task<BlogPost> RetrieveBlogPost(string blogPostKey)
{
    BlogPost blogPost = await cache.GetAsync<BlogPost>(blogPostKey);
    if (blogPost == null)
    {
        blogPost = await GetBlogPostFromDataSourceAsync(blogPostKey);
        await cache.SetAsync(blogPostKey, blogPost);
    }

    return blogPost;
}
```

Redis unterstützt Befehl pipelining, wenn eine Clientanwendung mehrere asynchrone Anfragen sendet. Redis bündeln können Sie die Anfragen mithilfe der gleichen Verbindungs statt empfangen und Befehle in einer Sequenz strict beantworten.

Dieser Ansatz hilft beim Wartezeit reduzieren, indem Sie effizienteren Nutzung des Netzwerks. Der folgende Codeausschnitt zeigt ein Beispiel, die die Details der beiden Kunden gleichzeitig abruft. Der Code sendet zwei Anfragen und dann führt einige andere Verarbeitung (nicht dargestellt) vor dem Warten auf die Ergebnisse zu erhalten. Die `Wait` Methode des Cacheobjekts ähnelt dem .NET Framework `Task.Wait` Methode:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
var task1 = cache.StringGetAsync("customer:1");
var task2 = cache.StringGetAsync("customer:2");
...
var customer1 = cache.Wait(task1);
var customer2 = cache.Wait(task2);
```

Die Seite [Redis Azure-Cache-Dokumentation](https://azure.microsoft.com/documentation/services/cache/) auf der Microsoft-Website enthält weitere Informationen zum Schreiben von Clientanwendungen, die die Azure Redis Cache verwendet werden kann. Weitere Informationen finden Sie auf der [Verwendungsseite grundlegende](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) auf der Website StackExchange.Redis.

Die Seite [Pipelines und Multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) auf derselben Website enthält weitere Informationen über asynchrone Vorgänge und pipelining mit Redis und der StackExchange-Bibliothek.  Im nächste Abschnitt in diesem Artikel verwenden Redis zwischenspeichern, enthält Beispiele für einige der erweiterten Techniken, die Sie auf Daten anwenden können, die in einem Redis Cache vorhanden ist.

## <a name="using-redis-caching"></a>Verwenden von Redis Zwischenspeichern.

Die einfachste Verwendung von Redis zum Zwischenspeichern von Bedenken ist Schlüssel-Wert-Paare, ist der Wert eine nicht interpretierte Folge von beliebiger Länge, der binären Daten enthalten kann. (Es ist im Wesentlichen ein Array von Bytes, die als Zeichenfolge behandelt werden kann). Dieses Szenario wurde in den Abschnitt implementieren Redis Cache-Clientanwendungen weiter oben in diesem Artikel dargestellt.

Beachten Sie, dass die Tasten auch nicht interpretierte Daten enthalten, sodass Sie alle binären Informationen als Schlüssel verwenden können. Je länger der Schlüssel ist, allerdings mehr Platz es dauert um zu speichern und die länger dauert Suchvorgänge ausführen. Nutzbarkeit und einfache Wartung der Schlüssellänge zur Verfügung sorgfältig entwerfen und aussagekräftige (aber nicht ausführlich) Tasten verwenden.

Verwenden Sie beispielsweise strukturierten Taste wie "Kunden: 100", um den Schlüssel für den Kunden-ID 100 statt einfach "100" darstellen. Dieses Schema ermöglicht es Ihnen leicht Werte unterscheiden, die verschiedene Datentypen zu speichern. Beispielsweise auch können die Taste "Bestellungen: 100" Sie den Schlüssel für den Auftrag mit ID 100 darstellen.

Abgesehen von eindimensionalen binäre Zeichenfolgen kann ein Wert in einem Redis Schlüsselwert Paar auch enthalten weitere strukturierte Informationen, einschließlich Listen, legt (sortiert und nicht sortiert) und wandelt. Redis bietet einen umfassenden Befehlssatz, der diese Dateitypen bearbeiten können, und viele dieser Befehle stehen in .NET Framework-Clientanwendungen über eine Clientbibliothek, wie z. B. StackExchange. Die Seite [eine Einführung in Datentypen und Abstraktionen Redis](http://redis.io/topics/data-types-intro) auf der Website Redis bietet eine ausführlichere Übersicht über diese Dateitypen und die Befehle, die Sie verwenden können, um diese zu bearbeiten.

In diesem Abschnitt werden einige allgemeine verwenden Fällen für die folgenden Datentypen und Befehle zusammengefasst.

### <a name="perform-atomic-and-batch-operations"></a>Atomare ausführen und Vorgänge Stapelverarbeitung

Redis unterstützt eine Reihe von atomaren Vorgänge Zeichenfolgenwerte abrufen und festlegen. Diese Vorgänge entfernen die möglichen Race gefährlichen Eigenschaften, die auftreten können, wenn separate mit `GET` und `SET` Befehle. Die Vorgänge, die verfügbar sind, gehören:

- `INCR`, `INCRBY`, `DECR`, und `DECRBY`, welche Operationen atomaren erhöhen und verringern auf ganze numerische Werte. Die StackExchange-Bibliothek bietet überladene Versionen der `IDatabase.StringIncrementAsync` und `IDatabase.StringDecrementAsync` Methoden zum Ausführen dieser Vorgänge und die resultierende Rückgabewert, die im Cache gespeichert ist. Der folgende Codeausschnitt veranschaulicht, wie diese Methoden verwenden:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  await cache.StringSetAsync("data:counter", 99);
  ...
  long oldValue = await cache.StringIncrementAsync("data:counter");
  // Increment by 1 (the default)
  // oldValue should be 100

  long newValue = await cache.StringDecrementAsync("data:counter", 50);
  // Decrement by 50
  // newValue should be 50
  ```

- `GETSET`, welche Ruft den Wert, der einen Schlüssel zugeordnet wurde, und es auf einen neuen Wert geändert. Die Bibliothek StackExchange bereitgestellt, diesen Vorgang durch die `IDatabase.StringGetSetAsync` Methode. Der folgende Codeausschnitt zeigt ein Beispiel für diese Methode. Dieser Code gibt den aktuellen Wert, der die Taste "Daten: Zähler" aus dem vorherigen Beispiel zugeordnet ist. Den Wert für diesen Schlüssel werden dann auf 0 (null), alle als Teil der gleichen Operation zurückgesetzt:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  string oldValue = await cache.StringGetSetAsync("data:counter", 0);
  ```

- `MGET`und `MSET`, die zurückzugeben oder eine Reihe von Zeichenfolgenwerte in einem Vorgang ändern können. Die `IDatabase.StringGetAsync` und `IDatabase.StringSetAsync` Methoden zur Unterstützung dieser Funktionalität, überladen sind, wie im folgenden Beispiel gezeigt:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  // Create a list of key-value pairs
  var keysAndValues =
      new List<KeyValuePair<RedisKey, RedisValue>>()
      {
          new KeyValuePair<RedisKey, RedisValue>("data:key1", "value1"),
          new KeyValuePair<RedisKey, RedisValue>("data:key99", "value2"),
          new KeyValuePair<RedisKey, RedisValue>("data:key322", "value3")
      };

  // Store the list of key-value pairs in the cache
  cache.StringSet(keysAndValues.ToArray());
  ...
  // Find all values that match a list of keys
  RedisKey[] keys = { "data:key1", "data:key99", "data:key322"};
  RedisValue[] values = null;
  values = cache.StringGet(keys);
  // values should contain { "value1", "value2", "value3" }
  ```

Sie können auch mehrere Vorgänge in einer Transaktion Redis kombinieren, wie im Abschnitt der Redis Transaktionen und Blattnamen zuvor in diesem Artikel beschrieben. Die StackExchange-Bibliothek bietet Unterstützung für Transaktionen über die `ITransaction` Schnittstelle.

Erstellen Sie eine `ITransaction` Objekt mithilfe der `IDatabase.CreateTransaction` Methode. Befehle für die Transaktion aufgerufen, um mithilfe von bereitgestellten Methoden der `ITransaction` Objekt.

Die `ITransaction` Benutzeroberfläche bietet Zugriff auf eine Reihe von Methoden, die ähnliche zugegriffen wird die `IDatabase` Schnittstelle, mit der Ausnahme, dass alle Methoden asynchrone sind. Diese bedeutet, dass er nur ausgeführt, wenn die `ITransaction.Execute` Methode aufgerufen wird. Der Wert, der von zurückgegeben wird die `ITransaction.Execute` Methode gibt an, ob die Transaktion erfolgreich erstellt wurde (WAHR) oder wenn es (falsch) fehlgeschlagen ist.

Der folgende Codeausschnitt zeigt ein Beispiel dieser vergrößert und verkleinert zwei Indikatoren als Teil derselben Transaktion:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
ITransaction transaction = cache.CreateTransaction();
var tx1 = transaction.StringIncrementAsync("data:counter1");
var tx2 = transaction.StringDecrementAsync("data:counter2");
bool result = transaction.Execute();
Console.WriteLine("Transaction {0}", result ? "succeeded" : "failed");
Console.WriteLine("Result of increment: {0}", tx1.Result);
Console.WriteLine("Result of decrement: {0}", tx2.Result);
```

Denken Sie daran, dass Redis Transaktionen im Gegensatz zu Transaktionen in relationalen Datenbanken sind. Die `Execute` Methode reiht einfach alle Befehle, die die Transaktion auszuführenden umfassen, und wenn jede hiervon ist falsch formatiert dann die Transaktion wird abgebrochen. Wenn alle Befehle erfolgreich zurückgestellt wurden, wird jeder Befehl asynchrone ausgeführt.

Wenn alle Befehl fehlschlägt, fortsetzen die andere weiterhin Verarbeitung. Wenn Sie müssen überprüfen, ob ein Befehl erfolgreich abgeschlossen wurde, müssen Sie die Ergebnisse des Befehls abzurufen, mithilfe der **Result** -Eigenschaft des entsprechenden Vorgangs, wie im Beispiel oben gezeigt. Lesen die Eigenschaft **Ergebnis** blockiert den einen Thread, bis die Aufgabe abgeschlossen wurde.

Weitere Informationen finden Sie auf der Seite [Transaktionen in Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) auf der Website StackExchange.Redis.

Beim Ausführen von Stapelvorgängen, können Sie die `IBatch` Schnittstelle der Bibliothek StackExchange. Diese Schnittstelle ermöglicht den Zugriff auf eine Reihe von Methoden, die ähnliche zugegriffen werden, indem Sie die `IDatabase` Schnittstelle, mit der Ausnahme, dass alle Methoden asynchrone sind.

Erstellen Sie eine `IBatch` Objekt mithilfe der `IDatabase.CreateBatch` Methode, und führen Sie dann den Stapel mithilfe der `IBatch.Execute` Methode, wie im folgenden Beispiel dargestellt. Dieser Code einfach einen Zeichenfolgenwert, Schritten und verringert die gleichen Indikatoren im vorherigen Beispiel verwendete festgelegt, und zeigt die Ergebnisse an:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
IBatch batch = cache.CreateBatch();
batch.StringSetAsync("data:key1", 11);
var t1 = batch.StringIncrementAsync("data:counter1");
var t2 = batch.StringDecrementAsync("data:counter2");
batch.Execute();
Console.WriteLine("{0}", t1.Result);
Console.WriteLine("{0}", t2.Result);
```

Es ist wichtig zu verstehen, dass im Gegensatz zu einer Transaktion, wenn Sie ein Befehl in einem Stapel schlägt fehl, weil es fehlerhaft, die andere Befehle noch ausgeführt werden möglicherweise. Die `IBatch.Execute` Methode kehrt eine Angabe der erfolgreichen oder nicht.

### <a name="perform-fire-and-forget-cache-operations"></a>Führen Sie Fire aus und vergessen Sie Cache Vorgänge

Redis Fire unterstützt und Vorgänge mithilfe des Befehls Kennzeichen vergessen. In diesem Fall der Client einfach initiiert einen Vorgang aber weist keine Zinsen im Ergebnis und wartet nicht auf den Befehl abgeschlossen sein soll. Im folgenden Beispiel wird gezeigt, wie zum Ausführen des Befehls INCR als ein Brand und Vorgang vergessen:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
await cache.StringSetAsync("data:key1", 99);
...
cache.StringIncrement("data:key1", flags: CommandFlags.FireAndForget);
```

### <a name="specify-automatically-expiring-keys"></a>Geben Sie automatisch ablaufenden Schlüssel

Beim Speichern eines Elements in einem Redis Cache können Sie einen Timeout angeben, nach dem das Element automatisch aus dem Cache entfernt wird. Sie können auch Abfragen, wie viel mehr Zeit ein Schlüssel hat, bevor es, mithilfe abläuft der `TTL` Befehl. Dieser Befehl steht für Applikationen StackExchange mithilfe der `IDatabase.KeyTimeToLive` Methode.

Der folgende Codeausschnitt zeigt, wie Sie eine Ablaufzeit von 20 Sekunden auf einem Schlüssel festlegen und die Gültigkeitsdauer des Schlüssels Abfragen:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration time of 20 seconds
await cache.StringSetAsync("data:key1", 99, TimeSpan.FromSeconds(20));
...
// Query how much time a key has left to live
// If the key has already expired, the KeyTimeToLive function returns a null
TimeSpan? expiry = cache.KeyTimeToLive("data:key1");
```

Sie können auch die Uhrzeit Ablauf zu einem bestimmten Datum und Uhrzeit festlegen, mithilfe des Befehls ablaufen, die in der Bibliothek StackExchange als verfügbar ist die `KeyExpireAsync` Methode:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration date of midnight on 1st January 2015
await cache.StringSetAsync("data:key1", 99);
await cache.KeyExpireAsync("data:key1",
    new DateTime(2015, 1, 1, 0, 0, 0, DateTimeKind.Utc));
...
```

> _Tipp:_ Sie können ein Element aus dem Cache manuell entfernen, mithilfe des Befehls ENTF, die über die StackExchange Bibliothek als verfügbar ist die `IDatabase.KeyDeleteAsync` Methode.

### <a name="use-tags-to-cross-correlate-cached-items"></a>Verwenden von Kategorien zum Cross-Cache Elemente korrelieren

Eine Redis ist eine Zusammenstellung von mehrere Elemente, die eine einzelne Taste freigeben. Sie können eine Gruppe mithilfe des Befehls SADD erstellen. Sie können die Elemente in einer Menge mithilfe des Befehls SMEMBERS abrufen. Die Bibliothek StackExchange implementiert den SADD-Befehl mit der `IDatabase.SetAddAsync` Methode und den SMEMBERS-Befehl mit der `IDatabase.SetMembersAsync` Methode.

Sie können auch vorhandene Datensätze erstellen Sie neue Datensätze mithilfe der SDIFF (festgelegten Differenz), SINTER (Schnittmenge) und SUNION (festlegen Union) Befehle kombinieren. Die Bibliothek StackExchange vereint diese Vorgänge in der `IDatabase.SetCombineAsync` Methode. Erste Parameter an diese Methode gibt den auszuführenden Festlegungsvorgang.

Die folgenden Codeausschnitte zeigen an, wie Datensätze schnell zu speichern und zu Abrufen von verknüpften Elementsammlungen hilfreich sein können. In diesem Code wird die `BlogPost` Typ, die im Abschnitt implementieren Redis Cache-Clientanwendungen weiter oben in diesem Artikel beschrieben wurde.

A `BlogPost` Objekt enthält vier Felder – Personalnummer, einen Titel, eine Rangfolge Bewertung und eine Auflistung von Kategorien. Der erste Codeausschnitt unten zeigt die Beispieldaten, die zum Auffüllen von einer C#-Liste verwendet wird `BlogPost` Objekte:

```csharp
List<string[]> tags = new List<string[]>()
{
    new string[] { "iot","csharp" },
    new string[] { "iot","azure","csharp" },
    new string[] { "csharp","git","big data" },
    new string[] { "iot","git","database" },
    new string[] { "database","git" },
    new string[] { "csharp","database" },
    new string[] { "iot" },
    new string[] { "iot","database","git" },
    new string[] { "azure","database","big data","git","csharp" },
    new string[] { "azure" }
};

List<BlogPost> posts = new List<BlogPost>();
int blogKey = 0;
int blogPostId = 0;
int numberOfPosts = 20;
Random random = new Random();
for (int i = 0; i < numberOfPosts; i++)
{
    blogPostId = blogKey++;
    posts.Add(new BlogPost(
        blogPostId,               // Blog post ID
        string.Format(CultureInfo.InvariantCulture, "Blog Post #{0}",
            blogPostId),          // Blog post title
        random.Next(100, 10000),  // Ranking score
        tags[i % tags.Count]));   // Tags--assigned from a collection
                                  // in the tags list
}
```

Sie können die Kategorien für jede speichern `BlogPost` als einen Satz in einem Redis-Cache-Objekt, und ordnen Sie die ID eines jeden Satz der `BlogPost`. Dadurch wird eine Anwendung alle Tags schnell zu finden, die einen bestimmten Blogbeitrag angehören. Zum Aktivieren der Suche in der entgegengesetzten Richtung, und suchen alle Blogbeiträge, die eine bestimmte Kategorie gemeinsam nutzen, können Sie einen weiteren Satz erstellen, die Bezüge auf die Kategorie-ID in die Taste Blogbeiträge enthält:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Tags are easily represented as Redis Sets
foreach (BlogPost post in posts)
{
    string redisKey = string.Format(CultureInfo.InvariantCulture,
        "blog:posts:{0}:tags", post.Id);
    // Add tags to the blog post in Redis
    await cache.SetAddAsync(
        redisKey, post.Tags.Select(s => (RedisValue)s).ToArray());

    // Now do the inverse so we can figure how which blog posts have a given tag
    foreach (var tag in post.Tags)
    {
        await cache.SetAddAsync(string.Format(CultureInfo.InvariantCulture,
            "tag:{0}:blog:posts", tag), post.Id);
    }
}
```

Diese Strukturen können Sie viele allgemeine Abfragen sehr effizient ausführen. Sie können beispielsweise suchen und alle Tags für Blogbeitrag 1 wie folgt angezeigt:

```csharp
// Show the tags for blog post #1
foreach (var value in await cache.SetMembersAsync("blog:posts:1:tags"))
{
    Console.WriteLine(value);
}
```

Sie können finden, dass alle Kategorien, die zum Blog feldbezogene 1 und Blog einen Beitrag 2 Posten anhand einer Schnittmenge, wie folgt:

```csharp
// Show the tags in common for blog posts #1 and #2
foreach (var value in await cache.SetCombineAsync(SetOperation.Intersect, new RedisKey[]
    { "blog:posts:1:tags", "blog:posts:2:tags" }))
{
    Console.WriteLine(value);
}
```

Und Sie können alle Blogbeiträge, die eine bestimmte Kategorie enthalten finden:

```csharp
// Show the ids of the blog posts that have the tag "iot".
foreach (var value in await cache.SetMembersAsync("tag:iot:blog:posts"))
{
    Console.WriteLine(value);
}
```

### <a name="find-recently-accessed-items"></a>Suchen kürzlich verwendeten Elemente

Eine allgemeine Aufgabe mit vielen Programmen erforderlich ist zu suchen, die am häufigsten zuletzt Elemente zugreifen. Angenommen, möchten eine Blogwebsite Informationen zu den zuletzt gelesen Blogbeiträge anzuzeigen.

Sie können dieses Feature mithilfe einer Liste Redis implementieren. Eine Liste der Redis enthält mehrere Elemente, die den gleichen Schlüssel gemeinsam nutzen. Die Liste fungiert als zwei Spitzen Warteschlange. Sie können Elemente an einem Ende der Liste drücken Sie mithilfe des LPUSH (links Pushbenachrichtigungen) und RPUSH (rechts Pushbenachrichtigungen) Befehle. Sie können Elemente aus einem Ende der Liste mit den Befehlen LPOP und RPOP abrufen. Sie können auch einen Satz von Elementen zurückgeben, mithilfe der Befehle LRANGE und ANORDNEN.

Die folgenden Codeausschnitte anzeigen, wie Sie mithilfe der Bibliothek StackExchange dieser Vorgänge ausführen können. In diesem Code wird die `BlogPost` Typ aus den vorherigen Beispielen. Wie ein Blogbeitrag von einem Benutzer, lesen Sie die `IDatabase.ListLeftPushAsync` Methode legt den Titel des Blogbeitrags auf eine Liste, die die Taste "Blog:recent_posts" im Cache Redis zugeordnet ist.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:recent_posts";
BlogPost blogPost = ...; // Reference to the blog post that has just been read
await cache.ListLeftPushAsync(
    redisKey, blogPost.Title); // Push the blog post onto the list
```

Weitere Blogbeiträge gelesen werden, werden ihre Titel auf derselben Liste abgelegt. Die Liste ist durch die Reihenfolge der geordnet, in dem die Titel hinzugefügt wurden. Die zuletzt gelesen von Blogbeiträgen befinden sich auf der linken Ende der Liste. (Der gleichen Blogbeitrag mehrmals gelesen werden, kann es mehrere Einträge in der Liste sind.)

Sie können den Titel der zuletzt gelesen Beiträge anzeigen, indem Sie mit der `IDatabase.ListRange` Methode. Diese Methode berücksichtigt die Taste, die die Liste, diese als Ausgangsbasis und einen Endpunkt enthält. Der folgende Code Ruft die Titel der am weitesten links befindlichen Ende der Liste der 10 von Blogbeiträgen (Elemente von 0 bis 9):

```csharp
// Show latest ten posts
foreach (string postTitle in await cache.ListRangeAsync(redisKey, 0, 9))
{
    Console.WriteLine(postTitle);
}
```

Beachten Sie, dass die `ListRangeAsync` Methode entfernt nicht Elemente aus der Liste aus. Dazu verwenden Sie die `IDatabase.ListLeftPopAsync` und `IDatabase.ListRightPopAsync` Methoden.

Um zu verhindern, dass die Liste endlos wächst, können Sie Elemente regelmäßig ausgemerzte, durch die Liste zuschneiden. Der Codeausschnitt unten zeigt, wie Sie alle entfernen, aber die fünf am weitesten links Elemente aus der Liste aus:

```csharp
await cache.ListTrimAsync(redisKey, 0, 5);
```

### <a name="implement-a-leader-board"></a>Implementieren ein Füllzeichen Brett

Standardmäßig werden die Elemente in einer Menge nicht in keiner bestimmten Reihenfolge aufrechterhalten. Sie können eine geordnete Menge mithilfe des Befehls ZADD erstellen (die `IDatabase.SortedSetAdd` Methode in der Bibliothek StackExchange). Mithilfe der einen numerischen Wert, der einen Faktor, die als Parameter an den Befehl bereitgestellt wird aufgerufen, werden die Elemente sortiert.

Im folgenden Codeausschnitt hinzugefügt eine sortierte Liste den Titel eines Blogbeitrags. In diesem Beispiel hat jeder Blogbeitrag auch ein Punktzahl-Feld, die den Rang des Blogbeitrags enthält.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:post_rankings";
BlogPost blogPost = ...; // Reference to a blog post that has just been rated
await cache.SortedSetAddAsync(redisKey, blogPost.Title, blogpost.Score);
```

Sie können den Titel des Beitrags Blog und Ergebnisse in aufsteigender Reihenfolge Punktzahl mithilfe Abrufen der `IDatabase.SortedSetRangeByRankWithScores` Methode:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(redisKey))
{
    Console.WriteLine(post);
}
```

> [AZURE.NOTE] Außerdem stellt die Bibliothek StackExchange der `IDatabase.SortedSetRangeByRankAsync` -Methode, die die Daten in der Reihenfolge der Punktzahl zurückgegeben werden, aber nicht die Ergebnisse zurück.

Sie können auch Elemente in absteigender Reihenfolge der Ergebnisse abrufen und Einschränken der Anzahl von Elementen, die zurückgegeben werden, indem Sie zusätzliche Parameter der `IDatabase.SortedSetRangeByRankWithScoresAsync` Methode. Im nächste Beispiel zeigt die Titel und die Ergebnisse der obersten 10 bewertete Blogbeiträge an:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(
                               redisKey, 0, 9, Order.Descending))
{
    Console.WriteLine(post);
}
```

Im nächste Beispiel wird mit der `IDatabase.SortedSetRangeByScoreWithScoresAsync` -Methode, die Sie verwenden können, um die Artikel einzuschränken, die auf, die zurückgegeben werden, die in einer angegebenen Punktzahl fallen Bereich:

```csharp
// Blog posts with scores between 5000 and 100000
foreach (var post in await cache.SortedSetRangeByScoreWithScoresAsync(
                               redisKey, 5000, 100000))
{
    Console.WriteLine(post);
}
```

### <a name="message-by-using-channels"></a>Nachricht mithilfe der Kanäle

Abgesehen von fungiert als Datencache, bietet ein Server Redis messaging über ein Verfahren leistungsfähige Publisher/Abonnenten. Clientanwendungen können auf einen Kanal abonnieren, und andere Programme oder Dienste können Nachrichten an den Kanal veröffentlichen. Abonnieren von Applications, dann diese Nachrichten erhalten und können diese verarbeiten.

Redis stellt den Befehl abonnieren für Clientanwendungen verwenden, um Kanäle zu abonnieren. Dieser Befehl erwartet den Namen der einen oder mehrere Kanäle, an denen die Anwendung Nachrichten akzeptiert. Die Bibliothek StackExchange enthält die `ISubscription` Benutzeroberflächen, womit eine .NET Framework-Anwendung zum abonnieren, sowie auf Kanäle veröffentlichen kann.

Erstellen Sie eine `ISubscription` Objekt mithilfe der `GetSubscriber` Methode der Verbindung mit dem Server Redis. Und Sie für Nachrichten auf einem Kanal, mithilfe hören der `SubscribeAsync` Methode dieses Objekts. Im folgenden Code wird gezeigt, wie mit einem Kanal mit dem Namen "Nachrichten: BlogPosts" abonnieren:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
await subscriber.SubscribeAsync("messages:blogPosts", (channel, message) =>
{
    Console.WriteLine("Title is: {0}", message);
});
```

Der erste Parameter für die `Subscribe` Methode ist der Name der Kanal. Dieser Name folgt die gleichen Konventionen, die von Tasten im Cache verwendet werden. Der Namen kann binären Daten enthält, obwohl es relativ kurze, aussagekräftige Zeichenfolgen verwenden, um sicherzustellen gute Leistung und Verwaltung des ratsam ist.

Beachten Sie auch, dass der verwendeten Kanäle Namespace von Tasten verwendete getrennt ist. Dies bedeutet, dass Sie können Kanäle und Schlüssel, die den gleichen Namen haben, obwohl Ihrer Anwendungscode schwieriger zu verwalten anlegen kann.

Der zweite Parameter ist eine Aktion Stellvertretung. Diese Stellvertretung führt asynchrone immer, wenn eine neue Nachricht im Kanal angezeigt wird. Dieses Beispiel zeigt die Meldung einfach auf die Verwaltungskonsole (die Nachricht enthält den Titel eines Blogbeitrags).

Um einen Kanal zu veröffentlichen, kann eine Anwendung mit dem Befehl Veröffentlichen Redis verwenden. Die Bibliothek StackExchange stellt die `IServer.PublishAsync` Methode, um diesen Vorgang auszuführen. Der nächste Codeausschnitt veranschaulicht, wie eine Nachricht an den Kanal "Nachrichten: BlogPosts" zu veröffentlichen:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
BlogPost blogpost = ...;
subscriber.PublishAsync("messages:blogPosts", blogPost.Title);
```

Es gibt einige Punkte, die Sie über das Veröffentlichen/Abonnieren Verfahren vertraut sein sollten:

- Mehrere Abonnenten können denselben Kanal abonnieren, und alle erhalten die Nachrichten, die mit diesem Kanal veröffentlicht werden.
- Abonnenten nur empfangen, die veröffentlicht wurden, nachdem sie abonniert haben. Kanäle sind nicht gepuffert, und nachdem Sie eine Nachricht veröffentlicht wird, die Redis Infrastruktur verschiebt die Nachricht an jeden Abonnenten und anschließend wieder entfernt.
- Standardmäßig werden Nachrichten von Abonnenten in der Reihenfolge empfangen, in denen sie gesendet werden. Sequenzieller Zustellung von Nachrichten, in einem hochgradig aktiven System mit einer großen Anzahl von Nachrichten und viele Abonnenten und Herausgeber kann die Leistung des Systems verlangsamen. Wenn jede Nachricht unabhängig ist und die Reihenfolge nicht von Bedeutung ist, können Sie die gleichzeitige Verarbeitung durch das System Redis aktivieren, die zur Verbesserung der Reaktionszeiten helfen können. Sie können dies in einem StackExchange Client erreichen, durch Festlegen der PreserveAsyncOrder der Verbindung vom Abonnenten false verwendet:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
redisHostConnection.PreserveAsyncOrder = false;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
```

## <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitungen

Das folgende Muster möglicherweise auch für Ihr Szenario relevant sein, beim Implementieren in Clientanwendungen Zwischenspeichern:

- [Cache-Aside Muster](http://msdn.microsoft.com/library/dn589799.aspx): Dieses Muster beschrieben, wie Sie bei Bedarf Daten in einem Cache aus einem Datenspeicher zu laden. Dieses Muster ist auch hilfreich, um Konsistenz zwischen den Daten, die im Cache gehalten werden und die Daten in den ursprünglichen Datenspeicher zu gewährleisten.
- Das [Sharding Muster](http://msdn.microsoft.com/library/dn589797.aspx) enthält Informationen über das horizontale Partitionierung Verbesserung Skalierbarkeit beim Speichern von und den Zugriff auf große Datenmengen implementieren.

## <a name="more-information"></a>Weitere Informationen

- Die Seite [MemoryCache Verzicht](http://msdn.microsoft.com/library/system.runtime.caching.memorycache.aspx) auf der Microsoft-website
- Die Seite [Redis Azure-Cache-Dokumentation](https://azure.microsoft.com/documentation/services/cache/) auf der Microsoft-website
- Der [Azure Redis Cache FAQ](redis-cache/cache-faq.md) -Seite auf der Microsoft-website
- Die Seite [Konfigurationsmodell](http://msdn.microsoft.com/library/windowsazure/hh914149.aspx) auf der Microsoft-website
- Der [Asynchrone Muster aufgabenbasierten](http://msdn.microsoft.com/library/hh873175.aspx) -Seite auf der Microsoft-website
- Die Seite [Pipelines und Multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) der StackExchange.Redis GitHub repo
- Die Seite [Redis Beibehaltung](http://redis.io/topics/persistence) der Redis-website
- Die [Seite ' Replikation '](http://redis.io/topics/replication) auf der Website Redis
- Die Seite [Redis Cluster Lernprogramm](http://redis.io/topics/cluster-tutorial) auf der Website Redis
- Die [Partitioning: zum Teilen von Daten zwischen mehreren Instanzen von Redis](http://redis.io/topics/partitioning) Seite auf der Website Redis
- Der Seite [Als LRU-Cache Redis verwenden](http://redis.io/topics/lru-cache) , klicken Sie auf der Website Redis
- Die Seite [Transaktionen](http://redis.io/topics/transactions) auf der Website Redis
- Die Seite [Redis Sicherheit](http://redis.io/topics/security) auf der Website Redis
- Die Seite [runde um Azure Redis Cache](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) Azure Blog
- Die [Ausführung einer CentOS Linux virtuellen Computers in Azure Redis](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) Seite auf der Microsoft-website
- Der [ASP.NET Sitzung Bundesstaat-Anbieter für Redis Azure-Cache](redis-cache/cache-aspnet-session-state-provider.md) -Seite auf der Microsoft-website
- Die [ASP.NET Ausgabe Cache für Azure Redis Cache](redis-cache/cache-aspnet-output-cache-provider.md) Anbieterseite auf der Microsoft-website
- Die Seite [Ein Einführung in Datentypen und Abstraktionen Redis](http://redis.io/topics/data-types-intro) auf der Website Redis
- Die [grundlegende Verwendung](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) -Seite auf der Website StackExchange.Redis
- Die Seite [Transaktionen in Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) die StackExchange.Redis repo
- Der [vorherigen Leitfaden Daten](http://msdn.microsoft.com/library/dn589795.aspx) auf der Microsoft-website
