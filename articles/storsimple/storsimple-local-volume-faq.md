<properties 
   pageTitle="StorSimple lokal angehefteten Datenmengen häufig gestellte Fragen zu | Microsoft Azure"
   description="Bietet Antworten auf häufig gestellte Fragen zu StorSimple lokal angehefteten Datenmengen."
   services="storsimple"
   documentationCenter="NA"
   authors="manuaery"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/16/2016"
   ms.author="manuaery" />

# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>StorSimple lokal angehefteten Datenmengen: häufig gestellte Fragen (FAQ)

## <a name="overview"></a>(Übersicht)

Die folgenden befinden Fragen und Antworten, die Sie möglicherweise beim Erstellen Sie einen Datenträger StorSimple lokal angehefteten, Konvertieren eines gestufte Lautstärke auf einen Datenträger lokal angeheftete (und umgekehrt), oder Sichern und einer lokal angeheftete Volume wiederherstellen.

Fragen und Antworten sind in die folgenden Kategorien angeordnet.

- Erstellen eines lokal angehefteten Datenträgers
- Sichern einer lokal angeheftete
- Konvertieren eines gestuften Datenträgers auf einen Datenträger lokal angeheftete
- Wiederherstellen einer lokal angeheftete Lautstärke
- Fehlerhafte über einen Datenträger lokal angeheftete

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Fragen zum Erstellen eines lokal angehefteten Datenträgers

**F.** Was ist die maximale Größe eines lokal angeheftete Datenträgers, die ich auf Geräten 8000-Serie erstellt werden können?

**A** können Sie lokal angeheftete Datenmengen Bereitstellung bis zu 8.5 TB oder gestufte Datenmengen auf dem Gerät 8100 bis zu 200 TB. Klicken Sie auf das größere 8600 Gerät können Sie lokal angeheftete Datenmengen bereitstellen bis zu 22,5 TB oder gestufte Datenmengen bis zu 500 TB.

**F.** Ich habe mein Gerät 8100 zuletzt auf Update 2 aktualisiert und beim Versuch, mich bei einer lokal angeheftete Volume zu erstellen, ist die maximale verfügbare Größe nur 6 TB und nicht 8.5 TB. Warum werden können keiner 8.5 TB-Volume erstellt?

**A** können Sie lokal angeheftete Datenmengen nach Zeitphasen bis zum 8.5 bereitstellen oder TB gestuft Datenmengen auf dem Gerät 8100 bis zu 200 TB. Wenn Ihr Gerät bereits Datenmengen, und klicken Sie dann der verfügbaren Speicherplatz für das Erstellen eines lokal angehefteten Datenträgers proportional niedriger als dieser Höchstgrenze gehört gestuft wurde. Angenommen, wenn 100 TB gestufte Datenmengen bereits auf Ihrem Gerät 8100 bereitgestellt werden (die Hälfte der Gestufte Kapazität ist), wird dann die maximale Größe eines lokalen Datenträgers, die Sie auf dem Gerät 8100 erstellen können entsprechend zu 4 TB reduziert (etwa angehefteten Hälfte des maximalen lokal Volume-Kapazität).

Da einige lokale Speicherplatz auf dem Gerät verwendet wird, um das Spektrum arbeiten an gestufte Datenmengen zu hosten, wird der verfügbare Platz für das Erstellen einer lokal angeheftete Lautstärke verringert, wenn das Gerät Datenmengen gestuft wurde. Erstellen ein lokales angeheftete Volume proportional reduziert dagegen den verfügbaren Platz für gestufte Datenmengen auf. In der folgenden Tabelle werden die verfügbare Gestufte Kapazität auf Geräten 8100 und 8600 zusammengefasst, wenn lokal angeheftete Datenmengen erstellt werden.

|Lokal angeheftete Datenmengen bereitgestellte Kapazität|Verfügbare Kapazität für gestufte Datenträger - 8100 bereitgestellt werden|Verfügbare Kapazität für gestufte Datenträger - 8600 bereitgestellt werden|
|-----|------|------|
|0 | 200 TB | 500 TB |
|1 TB | 176.5 TB | 477.8 TB|
|4 TB | 105.9 TB | 411.1 TB |
|8.5 TB | 0 TB | 311.1 TB|
|10 TB | NV | 277.8 TB |
|15 TB | NV | 166.7 TB |
|22,5 TB | NV | 0 TB |


**F.** Warum ist lokal angeheftete Volume-Erstellung eine umfangreiche Operation? 

**A** Lokal angeheftete Datenmengen sind Linienstärke nach der Bereitstellung. Um Speicherplatz auf den lokalen Ebenen des Geräts zu erstellen, möglicherweise einige Daten aus vorhandenen gestufte Datenmengen während der Bereitstellung Prozess in der Cloud abgelegt werden. Und da dies abhängig von der Größe des Datenträgers bereitgestellt werden, die vorhandenen Daten auf dem Gerät und der verfügbaren Bandbreite in der Cloud, möglicherweise die Verarbeitungszeit zum Erstellen eines lokalen Datenträgers mehrere Stunden.

**F.** Wie lange dauert es zum Erstellen eines lokal angehefteten Datenträgers?

**A** Da lokal angeheftete Datenmengen Linienstärke bereitgestellt werden, möglicherweise einige vorhandenen Daten aus gestufte Datenmengen während der Bereitstellung Prozess in der Cloud abgelegt werden. Daher hängt die Verarbeitungszeit zum Erstellen eines lokal angehefteten Datenträgers Vielfache Faktoren, einschließlich der Größe des Datenträgers, die Daten auf Ihrem Gerät und der verfügbaren Bandbreite ein. Auf einem frisch installierten Gerät, das kein Datenträger enthält, ist die Zeit zum Erstellen eines lokal angehefteten Datenträgers etwa 10 Minuten pro TB Daten. Erstellung von lokalen Datenmengen möglicherweise jedoch einige Stunden basierend auf die Faktoren auf einem Gerät verwendet wird, der oben erläuterten dauern.

**F.** Ich möchte ein lokales angeheftete Volume zu erstellen. Gibt es bewährte Verfahren, die, denen ich benötige berücksichtigt werden?

**A** Lokal angeheftete Datenmengen eignen sich für Auslastung, die erfordern lokale Garantien von Daten zu jeder Zeit und in die cloud Wartezeiten/Kleinschreibung. Werden Sie während der erwägen die Verwendung der lokalen Datenmengen für eines Ihrer Auslastung, sollten Sie Folgendes beachten:

- Lokal angeheftete Datenmengen sind Linienstärke nach der Bereitstellung, und erstellen lokale Datenträger wirkt sich auf den verfügbaren Platz für gestufte Datenmengen. Daher sollten Sie mit kleineren Datenmengen beginnen und von Ihrem zunehmender Anforderung Speicher skalieren.

- Bereitstellung von lokale Datenträger ist eine umfangreiche Operation, die möglicherweise betreffen Senden der vorhandene Daten aus gestufte Datenmengen in der Cloud. Sie können daher reduzierten Leistung auf diesen Datenträger auftreten.

- Bereitstellung von lokale Datenträger ist ein Zeit in Anspruch nehmen Vorgang. Die tatsächliche Zeit, die zur Verwaltung der hängt von mehreren Faktoren: die Größe des die Lautstärke bereitgestellt werden, die Daten auf Ihrem Gerät und verfügbare Bandbreite. Wenn Sie in der Cloud nicht Ihrer vorhandenen Datenträger gesichert haben, ist Volume-Erstellung langsamer. Es wird empfohlen, dass Sie Momentaufnahmen Cloud der vorhandenen Datenträger, bevor Sie ein lokales Laufwerk bereitstellen.
 
- Sie können vorhandene gestufte Datenmengen in lokal angeheftete Datenmengen konvertieren, und diese Konvertierung umfasst die Bereitstellung von Speicherplatz auf dem Gerät für die resultierende lokal angeheftete Lautstärke (zusätzlich zur Deaktivierung des gestufte Daten [sofern vorhanden] aus der Cloud). Dies ist wiederum eine umfangreiche Operation, von die Faktoren, die abhängig über besprochen. Es wird empfohlen, dass Sie vor der Konvertierung Ihrer vorhandenen Datenträger sichern, wie der Prozess gehört auch langsamer, wenn keine vorhandenen Datenträger gesichert werden. Ihr Gerät möglicherweise auch reduzierten Leistung während dieses Vorgangs auftreten.
    
Weitere Informationen zum [Erstellen einer lokal angeheftete Lautstärke](storsimple-manage-volumes-u2.md#add-a-volume)

**F.** Kann ich gleichzeitig mehrere lokal angeheftete Datenmengen erstellen?

**A** Ja, aber lokal angeheftete Lautstärke Erstellung und Erweiterung Aufträge werden sequenziell ausgeführt.

Lokal angeheftete Datenmengen sind Linienstärke nach der Bereitstellung und setzt die Erstellung der lokalen Speicherplatz auf dem Gerät (welche bereits vorhandenen Daten aus gestufte Datenmengen während der Bereitstellung Prozess in der Cloud abgelegt werden könnte). Daher ist ein provisioning Auftrag wird ausgeführt, werden andere Aufträge zum Erstellen von lokalen Volume in der Warteschlange werden, bis die Löschung abgeschlossen ist.

Auf ähnliche Weise wird, wenn ein vorhandenes lokales Volume erweitert wird, oder ein gestufte Volume zu einem lokal angeheftete Volume konvertiert wird, klicken Sie dann die Erstellung eines neuen lokal angeheftete Datenträgers in der Warteschlange bis zum Abschluss der vorherigen Position. Erweitern die Größe eines lokal angeheftete Datenträgers umfasst die Erweiterung von den vorhandenen lokalen Speicherplatz für diesen Datenträger. Konvertierung von gestufte lokal fixiert Lautstärke umfasst auch die Erstellung einer lokalen Speicherplatz für die resultierende lokal Lautstärke angehefteten. In sowohl diese Vorgänge erstellen, oder erläuterten lokale Abstand eine lange Auftrag ausgeführt wird.

Sie können diese Aufträge auf der Seite **Aufträge** der Dienst Azure StorSimple-Manager anzeigen. Die Position, die aktiv verarbeitet wird wird kontinuierlich aktualisiert, um den Fortschritt der Bereitstellung von Speicherplatz wiederzugeben. Die verbleibende lokal angeheftete Lautstärke Einzelvorgänge Ausführung markiert ist, aber deren Status angehalten wird und werden entnommen, in der Reihenfolge, dass sie in der Warteschlange wurden.

**F.** Ich habe gelöscht ein lokales angeheftete Volume. Warum wird den freigegebenen Platz in den verfügbaren Platz wiedergegeben wird, wenn ich versuche, erstellen ein neues Volume angezeigt? 

**A** Wenn Sie ein lokales angeheftete Volume löschen, der verfügbaren Speicherplatz für neue Datenmengen möglicherweise nicht sofort aktualisiert. Der StorSimple Manager-Dienst wird im lokalen Speicherplatz ungefähr stündlich aktualisiert. Es wird empfohlen, dass Sie warten, eine Stunde, bevor Sie versuchen, das neue Volume erstellt.

**F.** Werden lokal angeheftete Datenmengen werden in der Cloud-Anwendung unterstützt?

**A** Lokal angeheftete Datenmengen werden in der Cloud-Anwendung (8010 und 8020 Geräte früher als das StorSimple virtuelle Gerät bezeichnet) nicht unterstützt.

**F.** Kann ich die Azure PowerShell-Cmdlets zum Erstellen und Verwalten von lokal angeheftete Datenmengen verwenden? 

**A** Nein, können keine lokal angeheftete Datenmengen über Azure PowerShell-Cmdlets erstellt werden (alle Lautstärke über Azure PowerShell erstellten gestuft ist). Empfehlen wir, dass Sie nicht die Azure PowerShell-Cmdlets verwenden, um alle Eigenschaften eines lokal angeheftete Datenträgers, ändern wie ihm den unerwünschten Effekt ändern Sie den Typ der Lautstärke ist zu gestufte.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Fragen zum Sichern einer lokal angeheftete Lautstärke

**F.** Sind die lokale Momentaufnahmen der lokal angeheftete Datenmengen unterstützt?

**A** Ja, können Sie lokale Momentaufnahmen der lokal angeheftete Datenträger nutzen. Jedoch wird empfohlen, dass Sie regelmäßig sichern Ihrer lokal angehefteten Datenmengen mit Cloud Momentaufnahmen, um sicherzustellen, dass Ihre Daten im Falle von einem Datenverlust geschützt ist.

**F.** Gibt es noch Richtlinien für die Verwaltung von lokalen Momentaufnahmen für lokales angeheftete Datenmengen?

**A** Häufige lokale Momentaufnahmen entlang der Änderung von Daten in die lokal angeheftete Lautstärke eine hohe Rate möglicherweise lokalen Speicherplatz auf dem Gerät schnell verbraucht werden und dazu führen, dass Daten aus gestufte Datenmengen in der Cloud gedrückt wird. Wir empfehlen daher, dass Sie die Anzahl der lokalen Momentaufnahmen minimieren.

**F.** Ich habe erhalten eine Benachrichtigung, die besagt, dass meine lokalen Momentaufnahmen der lokal angeheftete Datenmengen möglicherweise ungültig werden. Wann kann dies geschehen?

**A** Häufige lokale Momentaufnahmen entlang der Änderung von Daten in die lokal angeheftete Lautstärke eine hohe Rate möglicherweise dazu, dass die lokale Speicherplatz auf dem Gerät und schnell genutzt werden sollen. Wenn die lokalen Ebenen des Geräts stark verwendet werden, ein erweiterte Cloud Ausfall kann dazu führen, das Gerät voll und eingehende schreibt auf den Datenträger können dazu führen, die Momentaufnahmen durchführen (wie kein Leerzeichen vorhanden ist, um die Momentaufnahmen zu verweisen, der überschrieben wurde die ältere Blöcke mit Daten zu aktualisieren). In einem solchen weiterhin in die Lautstärke der schreibt bereitgestellt werden, aber die lokalen Momentaufnahmen möglicherweise ungültig. Es gibt keine Auswirkung auf die vorhandenen Cloud Momentaufnahmen aus.

Die Warnung werden Sie darüber benachrichtigt, dass in einem solchen auftreten kann, und stellen Sie sicher, dass Sie der Adresse zeitgerecht entweder durch Überprüfen von Ihrem lokalen Momentaufnahmen Zeitpläne zum Aufzeichnen von weniger häufig verwendeten lokaler Momentaufnahmen oder löschen älterer lokale Momentaufnahmen konvertiert, die nicht mehr benötigt werden.

Wenn die lokalen Momentaufnahmen ungültig sind, erhalten Sie eine Informationen Benachrichtigung, die Sie darüber informiert, dass die lokalen Momentaufnahmen für die Richtlinie für bestimmte Sicherung entlang der Liste der von der lokalen Momentaufnahmen konvertiert werden ungültig wurde, die ungültig wurden. Diese Momentaufnahmen werden automatisch gelöscht, und Sie werden nicht mehr diese Seite **Sicherung Kataloge** in der klassischen Azure-Portal anzeigen.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>Fragen zum Konvertieren eines gestuften Datenträgers auf einen Datenträger lokal angeheftete

**F.** Ich bin einige dann ganz auf dem Gerät beim Konvertieren einer gestufte Lautstärke auf einen Datenträger lokal angeheftete beobachten. Woran liegt dies? 

**A** Die Konvertierung zu umfasst zwei Schritte: 

  1. Bereitstellung von Speicherplatz auf dem Gerät für die bald-zu--konvertiert werden lokal angehefteten Lautstärke.
  2. Garantiert herunterladen gestuften Daten aus der Cloud zu lokalen sicherzustellen.

Beide Schritte sind lang ausgeführt Vorgänge, die die Größe des zu konvertierenden Daten auf dem Gerät, und klicken Sie auf verfügbare Bandbreite Datenträgers abhängig sind. Wie in der Cloud einiger Daten aus vorhandenen gestufte Datenmengen als Teil der Bereitstellung Prozess Effekts möglicherweise, kann Ihr Gerät reduzierten Leistung dieses Zeitraums kommen. Darüber hinaus kann die Konvertierung zu langsamer ist:

- Vorhandene Datenmengen wurden nicht in der Cloud gesichert; damit es wird, dass Sie Ihre Datenmengen empfohlen, bevor Sie eine Konvertierung einleiten sichern.

- Bandbreite Drosselung Richtlinien wurden angewendet die möglicherweise die verfügbare Bandbreite in der Cloud einschränken; Wir empfehlen daher, dass Sie eine dedizierte 40/s oder weitere Verbindung in der Cloud haben.

- Die Konvertierung kann aufgrund der oben erläuterten Vielfache Faktoren mehrere Stunden dauern; Wir empfehlen daher zum Ausführen dieses Vorgangs nicht Spitzen Zeiten oder auf ein Wochenende die Auswirkung auf die Ende Verbraucher zu vermeiden.

Weitere Informationen zum [Konvertieren einer gestufte Lautstärke auf einen Datenträger lokal angeheftete](storsimple-manage-volumes-u2.md#change-the-volume-type)

**F.** Kann ich die Lautstärke Konvertierung abzubrechen?

**A** Nein, nicht dem Abbrechen den Konvertierungsvorgang einmal initiiert. Wie im vorherigen Frage erläutert, wenden Sie sich bitte beachten Sie die potenziellen Leistungsproblemen, die Sie möglicherweise während der Prozedur auftreten, und führen Sie die optimalen Methoden bei der Planung der Konvertierung oben aufgeführten.

**F.** Was geschieht mit meinem Lautstärke, wenn die Konvertierung nicht durchgeführt?

**A** Volumen-Konvertierung kann aufgrund von Verbindungsproblemen Cloud fehl. Das Gerät möglicherweise die Konvertierung zu nach einer Reihe fehlgeschlagener Versuche, zeigen Sie unten gestufte Daten aus der Cloud nicht mehr. In diesem Fall der Typ der Lautstärke ist weiterhin die Lautstärke Quellentyp vor der Konvertierung und:

- Wichtige Warnung wird ausgelöst, wenn Sie um die Lautstärke Konvertierungsfehler aufmerksam zu machen. Weitere Informationen zu [Benachrichtigungen im Zusammenhang mit lokal angeheftete Datenmengen](storsimple-manage-alerts.md#locally-pinned-volume-alerts)

- Wenn Sie eine gestufte auf einen Datenträger lokal angeheftete konvertieren, weiterhin die Lautstärke Eigenschaften eines gestufte Datenträgers aufweisen, wie die Daten weiterhin in der Cloud befinden sich möglicherweise. Es wird empfohlen, dass Sie Probleme mit der Konnektivität und Sie dann den Konvertierungsvorgang wiederholen.
 
- Auch wenn die Konvertierung aus einer lokal angeheftete auf einen Datenträger gestufte fehlschlägt, obwohl die Lautstärke als lokal angeheftete Volume markiert wird, wird es als gestufte Volume ausgeführt werden (da Daten in der Cloud verschüttet haben könnten). Jedoch wird weiterhin Platz auf den lokalen Ebenen des Geräts. Dieser Speicherplatz verfügbar für andere lokal angehefteten Datenmengen nicht. Es wird empfohlen, dass Sie wiederholen Sie diesen Vorgang, um sicherzustellen, dass die Lautstärke Konvertierung abgeschlossen ist, und der lokale Speicherplatz auf dem Gerät freigegeben werden kann.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Fragen zum Wiederherstellen eines lokal angeheftete Lautstärke

**F.** Lokal angeheftete Datenmengen sofort wiederhergestellt werden?

**A** Ja, werden sofort lokal angeheftete Datenmengen wiederhergestellt. Sobald die Metadaten-Informationen für den Datenträger aus der Cloud als Teil des Wiederherstellungsvorgangs gezogen wird, wird die Lautstärke Onlineschalten und vom Host zugegriffen werden kann. Lokale Garantien für die Lautstärke, dass die Daten nicht vorhanden, bis alle Daten aus der Cloud heruntergeladen wurde, und treten möglicherweise wird jedoch die Leistung auf diesen Datenträger für die Dauer der Wiederherstellung verringert.

**F.** Wie lange dauert es ein lokales angeheftete Volume wiederherstellen?

**A** Lokal angeheftete Datenmengen werden sofort wiederhergestellt und online geschaltet, sobald die Lautstärke Metadateninformationen aus der Cloud abgerufen werden, während die Lautstärke Daten weiterhin im Hintergrund heruntergeladen werden. Dieser letzte Teil des Wiederherstellungsvorgangs – erste wieder die lokalen Garantien für die Lautstärke Daten – ist eine umfangreiche Operation und möglicherweise mehrere Stunden erneut lokale gemacht werden alle Daten in Anspruch nehmen. Die Verarbeitungszeit für die gleiche durchführen, hängt von Faktoren wie die Größe des Datenträgers wiederhergestellt wird und der verfügbaren Bandbreite ab. Wenn das ursprüngliche Volume, das wiederhergestellt werden gelöscht wurde, wird den lokalen Speicherplatz auf dem Gerät im Rahmen des Wiederherstellungsvorgangs erstellen zusätzliche Zeit übernommen.

**F.** Ich müssen meine vorhandene lokal angeheftete Lautstärke zu einer älteren Momentaufnahme (wenn die Lautstärke gestuft wurde) wiederherstellen. Wird die Lautstärke werden wiederhergestellt, wie in diesem Fall gestuft?

**A** Nein, wird die Lautstärke als lokal angeheftete Volume wiederhergestellt werden. Obwohl die Datumsangaben Momentaufnahme der Zeit, wenn die Lautstärke gestuft wurde, während der Wiederherstellung vorhandenen Datenträger StorSimple Typ des Datenträgers auf dem Datenträger immer verwendet wie sie aktuell vorhanden sind.

**F.** Ich mein lokal angeheftete Volume zuletzt erweitert, aber ich möchte nun die Daten in eine Zeitangabe wiederherstellen, wann die Lautstärke geringerer Größe haben. Wird wiederherstellen Ändern der Größe der aktuellen Lautstärke und müssen die Größe des Datenträgers erweitern, nachdem die Wiederherstellung abgeschlossen ist?

**A** Ja, die Größe der Wiederherstellung wird der Lautstärke, und Sie müssen die Größe des Datenträgers nach Abschluss der Wiederherstellung zu erweitern.

**F.** Kann ich den Typ der ein Volume während der Wiederherstellung ändern?

**A.** Nein, können Sie den Typ der Lautstärke während der Wiederherstellung nicht ändern.

- Datenmengen, die gelöscht wurden, werden als den Typ der Snapshot gehörende Kehrmatrix wiederhergestellt.

- Vorhandene Datenmengen werden wiederhergestellt, basierend auf ihren aktuellen Typ, unabhängig von den Typ in der Momentaufnahme gespeichert (siehe die vorherigen beiden Fragen).
 
**F.** Kann ich mein lokal angeheftete Volume wiederherstellen müssen, aber ich einen falschen Punkt in Time-Snapshot entnommen. Kann ich Abbrechen der aktuellen Wiederherstellung?

**A** Ja, können Sie eine laufende Wiederherstellung Abbrechen. Der Zustand des Datenträgers wird in den Zustand zu Beginn der Wiederherstellung rückgängig gemacht werden. Alle schreibt, die an die Lautstärke vorgenommen wurden, während der Ausführung die Wiederherstellung, jedoch verloren.

**F.** Sind die ersten Schritte eines Wiederherstellungsvorgangs auf eine der meine lokal angehefteten Datenmengen, und jetzt eine Momentaufnahme in meinem Rückstand Katalog aus, den ich erstellen recollect nicht angezeigt. Was ist dies verwendet?

**A** Dies ist die temporäre Momentaufnahme, die vor der Wiederherstellung erstellt wird und dient zum Zurücksetzen, falls die Wiederherstellung abgebrochen wird oder fehlschlägt. Führen Sie diese Momentaufnahme nicht gelöscht. Es wird automatisch nach Abschluss die Wiederherstellung gelöscht werden. Dieses Verhalten kann auftreten, wenn Ihre Arbeit Wiederherstellen nur lokal Datenmengen oder eine Mischung lokal angeheftete und gestufte Datenmengen angehefteten hat. Wenn Sie der Wiederherstellungsauftrag nur gestufte Datenmengen enthält, wird dieses Verhalten nicht auftreten.

**F.** Kann ich ein lokales angeheftete Volume Klonen?

**A** Ja, können Sie aus. Die lokal angeheftete Lautstärke wird jedoch als gestufte Volume standardmäßig klonen. Weitere Informationen zum [Klonen einer lokal angeheftete Lautstärke](storsimple-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Fragen zu fehlerhaften über einen Datenträger lokal angeheftete

**F.** Muss ich auf meinem Gerät an ein anderes physische Gerät fehl. Werden meine lokal angehefteten Datenmengen Fehler beim werden über lokal angehefteten oder gestuft?

**A** Abhängig von der Softwareversion des Zielgeräts tritt lokal angeheftete Datenmengen über als ein Fehler auf:

- Reihe aktualisieren 2 lokal fixiert, wenn das Gerät StorSimple 8000 ausgeführt wird
- 8000-Serie aktualisieren 1.x gestuft, wenn das Gerät StorSimple ausgeführt wird
- Das Gerät ist die Cloud Einheit gestuft (Softwareupdates Version 2 oder 1.x aktualisieren)

Weitere Informationen zum [Failover und DR der angehefteten lokal Datenmengen über Versionen](storsimple-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**F.** Sind lokal angeheftete Datenmengen sofort während der Wiederherstellung (DR) wiederhergestellt?

**A** Ja, sind lokal angeheftete Datenmengen sofort während des Failovers wiederhergestellt. Sobald die Metadaten-Informationen für den Datenträger aus der Cloud als Teil eines Failover gezogen wird, wird die Lautstärke auf dem Zielgerät Onlineschalten und vom Host zugegriffen werden kann. In der Zwischenzeit die Lautstärke Daten weiterhin im Hintergrund herunterladen, und treten möglicherweise reduzierte Leistung auf diesen Datenträger für die Dauer für das Failover.

**F.** Ich finden Sie unter der Auftrag Failover abgeschlossen ist, wie ich den Fortschritt lokal angeheftete Lautstärke, die wiederhergestellt werden auf dem Zielgerät verfolgen können?

**A** Während eines Vorgangs Failover wird als einmal alle führen Sie die Datenmengen Failover festlegen sofort wiederhergestellt und auf dem Zielgerät online geschaltet wurden der Auftrag Failover markiert. Dies umfasst alle lokal angehefteten Datenmengen, die möglicherweise über fehlgeschlagen wurde; jedoch sind lokale Garantien Daten nur verfügbar, wenn alle Daten für die Lautstärke heruntergeladen wurde. Sie können diese für jedes lokal angeheftete Volume überwachen, die nicht wurde, um weiter Überwachung der entsprechenden Einzelvorgänge wiederherstellen, die als Teil des Failovers erstellt werden. Diese einzelne Wiederherstellungsaufträge werden nur für lokales angeheftete Datenmengen erstellt werden.

**F.** Kann ich den Typ des ein Volume während des Failovers ändern?

**A** Nein, können Sie den Typ der Lautstärke während eines Failovers nicht ändern. Wenn Sie fehlschlagen über an ein anderes physische Gerät, das StorSimple 8000 ausgeführt wird 2 Reihe aktualisieren, die Datenmengen werden werden Fehler über basierend auf dem Datenträger in der Momentaufnahme gespeichert. Um eine andere Geräteversion über weiß nicht, beziehen sich auf die Frage oben auf dem Datenträger nach einem Failover.

**F.** Kann ich nicht über Volume Container mit lokal angeheftete Datenmengen mit der Cloud Einheit?

**A** Ja, können Sie aus. Die lokal angehefteten Datenmengen werden über als gestufte Datenmengen ein Fehler auf. Weitere Informationen zum [Failover und DR der angehefteten lokal Datenmengen über Versionen](storsimple-device-failover-disaster-recovery.md#considerations-for-device-failover)


