<properties 
   pageTitle="Grenzwerte für StorSimple System | Microsoft Azure"
   description="Beschreibt System Grenzwerte und empfohlene Größen für Komponenten StorSimple 8000-Serie und Verbindungen."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/06/2016"
   ms.author="alkohli" />

# <a name="storsimple-system-limits"></a>Grenzwerte für StorSimple-system

## <a name="overview"></a>(Übersicht)

StorSimple stellt skalierbar und flexibel Speicherplatz für Ihr Datencenter. Es gibt jedoch einige Einschränkungen, die Sie bedenken sollten beim Planen, bereitstellen und Betrieb die Lösung StorSimple. In der folgenden Tabelle werden diese Grenzwerte beschrieben und einige Leitfaden, damit Sie Ihre Lösung StorSimple optimal nutzen können.

| Grenzwert für Bezeichner | Grenzwert | Kommentare |
|----------------- | ------|--------- |
| Maximale Anzahl von Anmeldeinformationen für Speicher-Konto | 64 | |
| Maximale Anzahl der Lautstärke Container | 64 | |
| Maximale Anzahl der Datenträger | 255 | |
| Maximale Anzahl von lokal angeheftete Datenmengen | 32 | |
| Maximale Anzahl von Zeitplänen pro Bandbreite Vorlage | 168 | Einen Zeitplan für jede Stunde, die jeden Tag der Woche (24 * 7). |
| Maximale Größe eines gestufte Datenträgers auf physischen Geräten | 64 TB für 8100 und 8600 | 8100 und 8600 sind physische Geräte. |
| Maximale Größe eines gestufte Datenträgers auf virtuellen Geräten in Azure | 30 TB für 8010 <br></br> 64 TB für 8020 | 8010 und 8020 sind in Azure virtuelle Geräte, die Verwenden von Standard und Premium-Speicher. |
| Maximale Größe eines lokal angeheftete Datenträgers auf physischen Geräten | 8.5 TB für 8100 <br></br> 22,5 TB für 8600 | 8100 und 8600 sind physische Geräte. |
| Maximale Anzahl von iSCSI-Verbindungen | 512 | |
| Maximale Anzahl von iSCSI-Verbindungen aus Initiatoren | 512 | |
| Maximale Anzahl von Access Steuerelement Datensätzen pro Gerät | 64 | |
| Maximale Anzahl von Datenmengen pro Sicherung Richtlinie | 24 | |
| Maximale Anzahl von Sicherungskopien beibehalten pro Terminplan (in einer Sicherungskopie Richtlinie) | 64 | |
| Maximale Anzahl von Zeitplänen pro Sicherung Richtlinie | 10 | |
| Maximale Anzahl von Momentaufnahmen eines beliebigen Typs, die pro Volume aufbewahrt werden können | 256 | Diese Zahl schließt lokale Momentaufnahmen und Momentaufnahmen cloud. |
| Maximale Anzahl von Momentaufnahmen konvertiert, die in einem beliebigen Gerät vorhanden sein können | 10.000 | |
| Maximale Anzahl der Datenträger, die für die Sicherung, Wiederherstellung parallel verarbeitet werden können, oder zu kopieren | 16 |<ul><li>Wenn es mehr als 16 Datenmengen gibt, werden sie sequenziell verarbeitet, sobald Verarbeitung Steckplätze verfügbar sind.</li><li>Neue Sicherungen einer duplizierten oder eines wiederhergestellten gestufte Volumen kann erst Abschluss des Vorgangs erfolgen. Jedoch für ein lokales Laufwerk, sind Sicherungskopien zulässig, nachdem die Lautstärke online ist.</li></ul>|
| Wiederherstellen und Wiederherstellungszeit für gestufte Datenmengen Klonen | < 2 Minuten | <ul><li>Die Lautstärke ist innerhalb von zwei Minuten wiederherstellen oder Klonen Operation unabhängig von der Größe der Lautstärke zur Verfügung gestellt.</li><li>Die Lautstärke Leistung möglicherweise Anfangs langsamer als normal, da die meisten Daten und Metadaten befindet sich noch in der Cloud. Als Datenfluss aus der Cloud am Gerät StorSimple möglicherweise die Leistung erhöhen.</li><li>Die Gesamtzeit für Metadaten herunterladen hängt die zugewiesenen Volume-Größe aus. Metadaten wird automatisch in das Gerät im Hintergrund mit einer Geschwindigkeit von 5 Minuten pro TB Daten zugewiesenen Lautstärke eingebracht. Diese Rate kann durch Internetbandbreite in der Cloud beeinträchtigt werden.</li><li>Das Wiederherstellen oder den datenbeschriftungsreihe Vorgang ist abgeschlossen, wenn alle Metadaten auf dem Gerät ist.</li><li>Zusätzliche Vorgänge können nicht ausgeführt werden, bis die wiederherstellen oder Klonen vollständig abgeschlossen ist.|
| Wiederherstellen Zeit für lokales angeheftete Datenmengen | < 2 Minuten | <ul><li>Die Lautstärke ist innerhalb von zwei Minuten des Wiederherstellungsvorgangs, unabhängig von der Größe der Lautstärke zur Verfügung gestellt.</li><li>Die Lautstärke Leistung möglicherweise Anfangs langsamer als normal, da die meisten Daten und Metadaten befindet sich noch in der Cloud. Als Datenfluss aus der Cloud am Gerät StorSimple möglicherweise die Leistung erhöhen.</li><li>Die Gesamtzeit für Metadaten herunterladen hängt die zugewiesenen Volume-Größe aus. Metadaten wird automatisch in das Gerät im Hintergrund mit einer Geschwindigkeit von 5 Minuten pro TB Daten zugewiesenen Lautstärke eingebracht. Diese Rate kann durch Internetbandbreite in der Cloud beeinträchtigt werden.</li><li>Im Gegensatz zu gestufte Datenmengen, für lokales angeheftete Datenmengen werden die Lautstärke Daten auch lokal auf dem Gerät heruntergeladen. Bei der Wiederherstellung ist abgeschlossen, wenn das Gerät Daten des Datenträgers gebracht worden.</li><li>Die Wiederherstellungsvorgängen möglicherweise lang sein. Die Gesamtzeit, die zum Abschließen der Wiederherstellung hängt von der Größe des bereitgestellte lokalen Datenträgers, die Internetbandbreite und die vorhandenen Daten auf dem Gerät. Zusätzliche Vorgänge auf dem Datenträger lokal angeheftete zulässig sind, während des Wiederherstellungsvorgangs ausgeführt wird.|
|Verarbeitung Abschlag (Disagio) Cloud Momentaufnahmen| 15 Minuten/TB | <ul><li>Minimale Zeit für die Cloud stellen snapshot sofort bei einem Upload können pro TB zugewiesenen Lautstärke Daten in Sicherung an. </li><li> Total Cloud Momentaufnahme Zeit wird berechnet, indem Sie diesmal die Internetbandbreite in die cloud Auswirkung Snapshot-Uploadzeit hinzufügen.
| Maximale Client/Lese Durchsatz (wenn es sich um eine von der Ebene SSD bereitgestellt wird) * | 920/720 MB/s mit einer einzigen 10 GbE-Netzwerk-Schnittstelle | Bis zu 2 X mit MPIO und zwei Netzwerkschnittstellen. |
| Maximale Client/Lese Durchsatz (wenn es sich um eine von der Festplatte Ebene bereitgestellt wird) * | 120/250 MB/s |
| Maximale Client/Lese Durchsatz (wenn aus der Cloud Ebene bereitgestellt wird)* für Update 3 oder höher.** | 40/60 MB/s für gestuft Datenmengen<br><br>60/80 MB/s für gestuft Datenmengen mit Archivierung Option während der Erstellung der Lautstärke | Lese-Durchsatz hängt Clients generieren und Verwalten von ausreichende e/a-Warteschlangentiefe aus. <br><br>Geschwindigkeit erreicht hängt von der Geschwindigkeit des zugrunde liegenden Speicherkontos verwendet. | 

& #42; Maximale Durchsatz pro e/a-Typ wurde mit 100 Prozent lesen und Schreiben von 100 Prozent Szenarien gemessen. Tatsächliche Durchsatz möglicherweise niedriger und e/a hängt mischen und Bedingungen Netzwerk aus.

& #42; & #42; Möglicherweise niedrigere Leistung Zahlen bis zu 3 aktualisieren.

## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie die [Systemanforderungen StorSimple](storsimple-system-requirements.md)ein. 
