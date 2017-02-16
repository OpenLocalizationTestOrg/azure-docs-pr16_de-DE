<properties
   pageTitle="Daten in DocumentDB mit Gültigkeitsdauer ablaufen | Microsoft Azure"
   description="Microsoft Azure DocumentDB bietet TTL die Möglichkeit, Dokumente, die automatisch nach einem bestimmten Zeitraum aus dem System gelöscht haben."
   services="documentdb"
   documentationCenter=""
   keywords="Gültigkeitsdauer"
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/28/2016"
   ms.author="kipandya"/>

# <a name="expire-data-in-documentdb-collections-automatically-with-time-to-live"></a>Daten in DocumentDB Websitesammlungen mit Gültigkeitsdauer automatisch abläuft

Applikationen können erstellen und speichern große Datenmengen. Einige dieser Daten, wie Computer generiert Daten, Protokolle, und Benutzer Veranstaltung, die Informationen für einen begrenzten Zeitraum nur sinnvoll ist. Sobald die Daten nicht mehr zu den Anforderungen der Anwendung überflüssigen lassen sich diese Daten bereinigen und die Anforderungen Speicher Anwendung zu verringern.

Mit "Zeit, live" oder TTL bietet Microsoft Azure DocumentDB die Möglichkeit, Dokumente aus der Datenbank nach einer bestimmten Zeitspanne automatisch bereinigt. Die Standard-Time to live kann auf Ebene der Websitesammlung festlegen und auf Basis pro Dokument überschrieben werden. Nachdem Sie TTL festgelegt haben, als eine Auflistung Standard oder eines Dokuments auf oberster Ebene entfernt DocumentDB automatisch Dokumente, die nach dieser Zeitspanne in Sekunden, vorhanden sind, seit der letzten Änderung.

Gültigkeitsdauer in DocumentDB verwendet einen Offset gegen aus, wenn das Dokument zuletzt geändert wurde. Dazu verwendet es jedem Dokument _ts Felds, das vorhanden ist. Das Feld "_ts" ist eine Unix-Format Epoche Timestamp das Datum und die Uhrzeit darstellt. Jedes Mal, wenn ein Dokument geändert wird, wird das Feld _ts aktualisiert. 

## <a name="ttl-behavior"></a>TTL-Verhalten

Das Feature TTL wird von TTL-Eigenschaften auf zwei Ebenen – der Ebene der Websitesammlung und der Ebene gesteuert. Die Werte werden in Sekunden festgelegt und werden als ein Delta aus der _ts, denen letzten des Dokuments am Änderung behandelt.

 1.  DefaultTTL für die Websitesammlung
  * Wenn (oder auf null festgelegt), Dokumente werden nicht automatisch gelöscht.
  
  * Wenn Sie präsentieren und den Wert ist "1" = unbegrenzte – Dokumente nicht standardmäßig abläuft
  
  * Wenn Sie präsentieren und den Wert ist eine Zahl ("n") – Dokumente ablaufen Sekunden nach der letzten Änderung "n"

 2.  TTL für Dokumente: 
  * Eigenschaft ist nur für die DefaultTTL für die übergeordnete Auflistung vorhanden ist.
  
  * Überschreibt den Wert DefaultTTL für die übergeordnete Sammlung.

Sobald das Dokument abgelaufen ist (Ttl + _ts > = Zeitmarke Server), das Dokument als "abgelaufen" markiert ist. Kein Vorgang kann auf diese Dokumente nach Ablauf dieser Zeit, und sie werden aus den Ergebnissen der Abfragen ausgeführt ausgeschlossen werden. Die Dokumente werden im System physisch gelöscht, und es werden gelöscht Ausführung zu einem späteren Zeitpunkt im Hintergrund. Dies keine [Anfordern Einheiten (RUs)](documentdb-request-units.md) aus dem Budget der Websitesammlung nutzen.

Die oben aufgeführten Logik kann in der folgenden Matrix angezeigt werden:

|       | Legen Sie für die Websitesammlung DefaultTTL fehlende/nicht | DefaultTTL =-1 auf die Websitesammlung | DefaultTTL = "n" auf die Websitesammlung|
| ------------- |:-------------|:-------------|:-------------|
| TTL Dokument fehlen| Nichts Dokument Ebene außer Kraft setzen, da das Dokument und für die Websitesammlung kein Konzept für TTL besitzen. | Keine Dokumente in dieser Websitesammlung läuft ab. | Die Dokumente in dieser Websitesammlung läuft ab, wenn n Intervall verstrichen ist. |
| TTL =-1 Dokument | Nichts zum Außerkraftsetzen von auf der Ebene der seit der Websitesammlung definiert die Eigenschaft DefaultTTL kein Dokuments überschrieben werden kann. TTL an einem Dokument ist von dem System dauerhaften interpretiert. | Keine Dokumente in dieser Websitesammlung läuft ab. | Das Dokument mit TTL =-1 in dieser Websitesammlung läuft nie ab. Alle anderen Dokumenten läuft ab nach "n" Intervall. |
|  TTL = n Dokument | Nichts auf der Ebene außer Kraft setzen. TTL an einem Dokument in dauerhaften von System interpretiert. | Das Dokument mit TTL = n läuft ab nach Intervall n, in Sekunden. Andere Dokumente werden erben Intervall von-1 und läuft nie ab. | Das Dokument mit TTL = n läuft ab nach Intervall n, in Sekunden. Andere Dokumente erbt die Sammlung "n" Intervall. |


## <a name="configuring-ttl"></a>Konfigurieren von TTL

Standardmäßig ist die Gültigkeitsdauer in allen DocumentDB Sammlungen, und klicken Sie auf alle Dokumente standardmäßig deaktiviert.

## <a name="enabling-ttl"></a>Aktivieren der TTL

Wenn TTL auf eine Auflistung oder die Dokumente in einer Websitesammlung aktivieren möchten, müssen Sie die Eigenschaft DefaultTTL eine Auflistung entweder-1 oder eine positive Zahl ungleich NULL festgelegt. Festlegen der DefaultTTL auf-1 wird, durch die Standardeinstellung, die alle Dokumente in der Auflistung für immer live werden jedoch die DocumentDB sollten Dienst dieser Websitesammlung für Dokumente überwachen, die diese Standardeinstellung überschrieben haben.

## <a name="configuring-default-ttl-on-a-collection"></a>Konfigurieren von Standard-TTL auf einer Websitesammlung

Sie können eine voreingestellte Zeit auf Ebene der Websitesammlung live konfigurieren. 

Um die Gültigkeitsdauer für eine Websitesammlung festgelegt, müssen Sie eine positive Zahl ungleich NULL angeben, die dem Punkt in Sekunden, alle Dokumente in der Auflistung nach den letzten geänderten Zeitstempel des Dokuments (_ts) abläuft angibt.

Alternativ können Sie legen die Standard-1, d. h., dass alle Dokumente in der Auflistung eingefügt endlos standardmäßig live werden.

## <a name="setting-ttl-on-a-document"></a>Einstellung TTL an einem Dokument

Zusätzlich zum Festlegen einer standardmäßigen TTL auf einer Websitesammlung können Sie bestimmte TTL Ebene eines Dokuments festlegen. Hiermit wird die Standardeinstellung der Websitesammlung außer Kraft setzen.

Wenn die Gültigkeitsdauer an einem Dokument festlegen möchten, müssen Sie eine positive Zahl ungleich NULL angeben, die festlegt, mit dem Punkt in Sekunden, das Dokument nach den letzten geänderten Zeitstempel des Dokuments (_ts) abläuft.

Um diesen Ablauf Offset festlegen möchten, legen Sie das TTL-Feld für das Dokument aus.

Wenn ein Dokument kein TTL-Feld enthält, gilt die Standardeinstellung der Websitesammlung.

Wenn TTL auf Ebene der Websitesammlung deaktiviert ist, wird das TTL-Feld im Dokument bis TTL erneut, klicken Sie auf die Auflistung aktiviert ist ignoriert.


## <a name="extending-ttl-on-an-existing-document"></a>Erweitern Sie TTL auf einem vorhandenen Dokument

Sie können die Gültigkeitsdauer in einem Dokument alle Schreibvorgang auf das Dokument wie folgt zurücksetzen. Hiermit wird der _ts auf die aktuelle Uhrzeit festgelegt, und der Countdown zur Dokument Ablauf, entsprechend der Festlegung durch die Gültigkeitsdauer (Ttl) wird erneut beginnen.

Wenn Sie den TTL-Wert eines Dokuments ändern möchten, können Sie das Feld aktualisieren, wie mit einem anderen festgelegt werden Feld möglich ist.


## <a name="removing-ttl-from-a-document"></a>TTL entfernen aus einem Dokument.

Wenn eine TTL an einem Dokument festgelegt wurde, und Sie nicht mehr, dass dieses Dokument abläuft möchten, können dann das Dokument abrufen, Entfernen des Felds TTL und das Dokument auf dem Server ersetzen.

Wenn TTL-Felds aus dem Dokument entfernt wird, wird die Standardeinstellung der Websitesammlung angewendet.

Zum Beenden eines Dokuments mit Ablauf und nicht aus der Sammlung erben müssen Sie den Wert für TTL auf-1 gesetzt.


## <a name="disabling-ttl"></a>Deaktivieren von TTL

TTL auf eine Auflistung vollständig deaktivieren, und beenden Sie den Hintergrundprozess aus wonach abgelaufene Dokumente, die die Eigenschaft DefaultTTL auf die Sammlung gelöscht werden soll.

Das Löschen dieser Eigenschaft unterscheidet sich von es auf-1 festlegen. Einstellung-1 bedeutet, dass neue Dokumente der Websitesammlung hinzugefügt wird für immer live, aber Sie können dies überschreiben, auf bestimmte Dokumente in der Auflistung.

Entfernen diese Eigenschaft vollständig aus der Auflistung bedeutet, dass keine Dokumente abläuft, auch wenn Dokumente, die explizit einen vorherigen Standard überschrieben haben vorhanden sind.


## <a name="faq"></a>Häufig gestellte Fragen

**Was wird TTL Kosten?**

Es ist keine zusätzliche Kosten zum Festlegen einer TTL an einem Dokument ein.

**Wie lange dauert es mein Dokument löschen, nachdem die Gültigkeitsdauer von ist?**

Die Dokumente werden als nicht verfügbar gekennzeichnet, sobald das Dokument abgelaufen ist (Ttl + _ts > = Zeitmarke Server). Kein Vorgang kann auf diese Dokumente nach Ablauf dieser Zeit, und sie werden aus den Ergebnissen der Abfragen ausgeführt ausgeschlossen werden. Die Dokumente werden durch das System im Hintergrund physisch gelöscht. Dies sind keine RUs aus der Auflistung Budget nutzen.

**Wenn einen Zeitraum dauert, der Dokumente zu löschen, werden diese in Richtung Meine Kontingent (und Rechnung) zählen, bis sie gelöscht erhalten?**

Nein, sobald das Dokument abgelaufen ist, Sie nicht für den Speicherort für diese Dokumente Abrechnung und die Größe der Dokumente werden nicht in die Richtung des Speicherkontingents für eine Websitesammlung zählen.

**Werden TTL an einem Dokument eine Auswirkung auf RU Gebühren haben?**

Nein, es werden keine Auswirkung auf RU Gebühren für Vorgänge innerhalb DocumentDB Dokument ausgeführt.

**Wird das Löschen von Dokumenten Einfluss auf den Durchsatz, die, den ich auf meinem Websitesammlung bereitgestellt haben?**

Nein, Erstellen von Besprechungsanfragen für Ihre Websitesammlung Priorität erhalten über den Hintergrundprozess zum Löschen von Dokumenten ausführt. TTL an ein beliebiges Dokument hinzufügen wird dies nicht auswirken.

**Wenn ein Dokument abläuft, wie lange verbleibt er in meiner Sammlung, bis sie gelöscht wird?**

Sobald das Dokument läuft ab kann es nicht mehr zugegriffen werden. Die genaue Uhrzeit bleibt ein Dokuments in der Websitesammlung, bevor tatsächlich gelöscht haben, nicht deterministisch ist und basiert auf, wenn der Hintergrund-Prozess das Dokument nicht gelöscht kann.

**Abgelaufene Dokumente werden gelöscht werden, auf allen Knoten, oder ist es "später Consistent?"**

Das Dokument wird zur gleichen Zeit auf allen Knoten und in allen Regionen nicht mehr verfügbar.

**Gibt es eine RU Kosten für Hintergrundaufgaben TTL Überwachung?**

Nein, es ist keine RU Kosten für diese.

**Wie oft werden Ablaufzeiten TTL aktiviert?**

TTL Ablaufzeiten Auschecken erfolgen nicht als Hintergrundprozess. Die Antwort auf eine Anforderung der Back-End-Dienst kann den Prüfungen Inline und ausschließen alle Dokumente, die abgelaufen sind. Das Löschen des physischen Dokuments ist der einzige Prozess, der asynchrone im Hintergrund ausgeführt wird. Die Häufigkeit dieses Prozesses wird durch die verfügbaren RUs auf die Auflistung bestimmt.

**Gelten das Feature TTL nur für ganze Dokumente oder können einzelne Dokument Immobilienwerte ablaufen?**

TTL gilt für das gesamte Dokument. Wenn Sie möchten nur einen Teil eines Dokuments abläuft, wird empfohlen, dass Sie den Teil aus dem Hauptdokument in einem separaten "verknüpfte" Dokument extrahieren und Sie dann die extrahierten Dokument TTL verwenden.

**Verfügt das Feature TTL Indizierung erfüllen müssen?**

Ja. Die Sammlung müssen entweder Lazy oder konsistent [Indizierung Richtlinie festgelegt](documentdb-indexing-policies.md) . Versuchen, eine Auflistung mit festlegen keine Indizierung DefaultTTL festlegen führt zu einem Fehler, bei dem Versuch, deaktivieren Sie indizieren auf eine Auflistung, die eine DefaultTTL bereits festgelegt wurde.


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Informationen zur Azure DocumentDB finden Sie in der [*Dokumentation*](https://azure.microsoft.com/documentation/services/documentdb/) Seite.




