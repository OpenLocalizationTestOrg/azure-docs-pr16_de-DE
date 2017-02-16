<properties 
   pageTitle="StorSimple virtuelle Array Updates Versionsinformationen | Microsoft Azure"
   description="Beschreibt öffnen Probleme und Lösungen für das Ausführen von Update 0,3 virtuelle StorSimple-Array."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/15/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-03-release-notes"></a>0,3 Versionsinformationen StorSimple virtuelle Array aktualisieren

## <a name="overview"></a>(Übersicht)

Die folgenden Versionsinformationen stellen die kritische öffnen sowie die gelöst Probleme für Microsoft Azure StorSimple Virtual Array Updates.

Die Versionsinformationen werden kontinuierlich aktualisiert und kritische Probleme, die erfordern einer problemumgehung entdeckt werden, werden diese hinzugefügt. Bevor Sie Ihre StorSimple Virtual Array bereitstellen, überprüfen Sie sorgfältig die Informationen in den Versionsinformationen enthalten sind.

Update 0,3 entspricht der Software Version **10.0.10288.0**.

> [AZURE.NOTE] Updates werden Unterbrechung und starten Sie das Gerät neu. Wenn e/a ausgeführt wird, geht das Gerät Ausfallzeiten aus.


## <a name="whats-new-in-the-update-03"></a>Was ist neu in der Aktualisierung 0,3

Update 0,3 ist hauptsächlich eine Fehlerkorrektur Generator an. In dieser Version wurden mehrere Fehler in Sicherung Fehlern in der Vorgängerversion resultierender behoben.

## <a name="issues-fixed-in-the-update-03"></a>Probleme bei der Aktualisierung 0,3

Die folgende Tabelle enthält eine Zusammenfassung der Probleme in dieser Version.

| Nein.  | Feature                              | Problem                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | Sicherungskopien                                |Ein Problem wurde in der früheren Version angezeigt, auf denen die Sicherungskopien für eine Dateifreigabe abschließen fehlschlägt würden. Wenn dieses Problem aufgetreten ist, der Auftrag Sicherung fehl, und eine Warnung vom Typ kritische StorSimple Manager-Dienst Benachrichtigung des Benutzers ausgelöst wurde. Dieses Problem keine Auswirkung auf die Daten auf die Anteile oder den Zugriff auf die Daten. Die Ursache wurde identifiziert und in dieser Version behoben. <br></br> Fix gilt rückwirkend nicht für Freigaben, die bereits dieses Problem auftritt. Kunden, die dieses Problem sehen, sollte zuerst Update 0,3 anwenden und dann wenden Sie sich an Microsoft Support, um eine vollständige System Sicherungskopie, um das Problem zu beheben. Anstatt eine Verbindung mit Microsoft-Support, können Kunden auch auf eine neue Freigabe aus einer fehlerfrei Sicherung für die betroffenen Freigaben wiederherstellen.                                                                                                                                                                                 |
| 2    | iSCSI                         | Ein Problem wurde in der früheren Version angezeigt, in dem die Datenmengen, beim Kopieren von Daten auf einen Datenträger, auf dem StorSimple virtuelle Array verschwinden würden. In dieser Version wurde dieses Problem behoben. <br></br> Die Updates werden wirksam nur auf neu erstellte Datenmengen. Die Updates werden nicht rückwirkend auf Datenmengen angewendet, die bereits dieses Problem auftritt. Kunden sollten die betroffenen Datenträger online über das klassische Azure-Portal anzuzeigen, führen Sie eine Sicherung für diese Datenträger und diese Datenträger klicken Sie dann auf neue Datenträger wiederherstellen möchten.                                                               |


## <a name="known-issues-in-the-update-03"></a>Bekannte Probleme bei der Aktualisierung 0,3

In der folgenden Tabelle enthält eine Zusammenfassung der bekannten Probleme für das StorSimple virtuelle Array und enthält die Probleme aus früheren Versionen Release notiert haben. 


| Nein. | Feature | Problem | Problemumgehung/Kommentare |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Updates | Die virtuelle Geräte, die in der Preview-Version erstellt können nicht auf eine unterstützte Version der allgemeinen Verfügbarkeit aktualisiert werden. | Diese virtuellen Geräte müssen über für die allgemeine Verfügbarkeit-Version mit einem Disaster Wiederherstellung (DR) Workflow Fehler werden. |
| **2.** | Bereitgestellte Datenträger | Nachdem Sie einen Datenträger einer bestimmten angegebenen Größe bereitgestellt haben und erstellt das entsprechende StorSimple virtuelle Gerät, Sie müssen nicht vergrößern oder verkleinern den Datenträger Daten. Beim Versuch, führen Sie die Ergebnisse in einem Verlust aller Daten in der lokalen Ebenen des Geräts. |   |
| **3.** | Gruppenrichtlinien | Wenn ein Gerät Domäne gehört, kann Anwenden einer Gruppenrichtlinie für die den Gerätevorgang beeinträchtigen. | Stellen Sie sicher, dass Ihr virtuelle Array in einem eigenen (Organisationseinheit) für Active Directory ist und keine Gruppenrichtlinienobjekte (Gruppenrichtlinienobjekt) angewendet werden.|
| **4.** | Lokale Web-Benutzeroberfläche | Wenn erweiterten Sicherheitsfeatures in Internet Explorer (IE ESC) aktiviert sind, möglicherweise einige lokale Webseiten des Benutzeroberfläche wie Problembehandlung oder Wartung nicht ordnungsgemäß funktioniert. Schaltflächen auf diesen Seiten funktionieren möglicherweise auch nicht. | Verbesserte Sicherheit-Funktionen in Internet Explorer deaktivieren.|
| **5.** | Lokale Web-Benutzeroberfläche | In einer Hyper-V virtuellen Computern Schnittstellen Netzwerk-Schnittstellen im Web Benutzeroberfläche Leuchten als 10 Gbps. | Dieses Verhalten ist eine Spiegelung von Hyper-V. Hyper-V zeigt immer 10 Gbps für virtuelle Netzwerkadapter. |
| **6.** | Gestufte Datenmengen oder Freigaben | Byte-Bereich durch das Sperren für Applikationen, die Arbeiten mit der StorSimple gestufte Datenmengen wird nicht unterstützt. Wenn Byte Bereich Sperren aktiviert ist, funktioniert StorSimple Stufen nicht. | Empfohlene Maßnahmen umfassen: <br></br>Deaktivieren Sie in der Anwendungslogik Sperren Byte-Bereich ein.<br></br>Wählen Sie aus, die Daten für diese Anwendung lokal angeheftete Datenmengen im Gegensatz zum gestufte Datenmengen richtige.<br></br>*Einschränkung*: Wenn mit lokal Datenmengen angehefteten und Byte Bereich Sperren aktiviert ist, die lokal angeheftete Lautstärke kann online sein, bevor die Wiederherstellung abgeschlossen ist. In diesen Fällen ist eine Wiederherstellung wird ausgeführt, müssen Sie Abschluss des Wiederherstellungsvorgangs zu warten. |
| **7.** | Gestufte Freigaben | Arbeiten mit großer Dateien kann sich langsam Ebene führen. | Bei der Arbeit mit großer Dateien, empfehlen wir, dass die größte Datei kleiner als 3 % der Größe freigeben. |
| **8.** | Kapazität für Freigaben verwendet | Sehen Sie möglicherweise Verbrauch freigeben, wenn es keine Daten für die Freigabe. Dies ist, da die verwendete Kapazität für Freigaben Metadaten umfasst. |   |
| **9.** | Wiederherstellung | Sie können nur von einem Dateiserver in derselben Domäne wie die des Quellgeräts die Wiederherstellung durchführen. Wiederherstellung an ein Gerät in eine andere Domäne wird in dieser Version nicht unterstützt. | Dies wird in einer späteren Version implementiert. |
| **10.** | Azure PowerShell | Über die Azure PowerShell in dieser Version werden nicht die StorSimple virtuelle Geräte verwaltet. | Die Verwaltung der virtuellen Geräte sollten über das Azure klassischen Portal und der lokale Web-Benutzeroberfläche erfolgen. |
| **11.** | Kennwort ändern | Die virtuelle Array Gerät Konsole akzeptiert nur Eingabe im En-US-Tastatur-Format. |   |
| **12.** | CHAP | CHAP-Anmeldeinformationen, die nach der Erstellung können nicht entfernt werden. Wenn Sie die CHAP-Anmeldeinformationen ändern möchten, müssen Sie darüber hinaus Offlineschalten der Datenmengen, und zeigen Sie diese online die vorgenommene Änderung wirksam wird. | Dieses Problem wird in einer späteren Version beschrieben. |
| **13.** | iSCSI-server  | Die 'verwendete Speicher' für einen Datenträger iSCSI angezeigt möglicherweise in der StorSimple Manager-Dienst und dem iSCSI Host anders. | Die iSCSI-Server verfügt über das Dateisystem-Ansicht.<br></br>Das Gerät sieht der zugeordnet wird, wenn die Lautstärke am die maximale Größe wurde blockiert.|
| **14.** | Dateiserver  | Wenn eine Datei in einem Ordner eine alternative Daten Stream (anzeigen) zugeordnet ist, wird die anzeigen nicht gesichert oder über Wiederherstellung, Klonen und Element Ebene Wiederherstellung wiederhergestellt.| |


## <a name="next-step"></a>Als Nächstes

[Installieren Sie Update 0,3](storsimple-ova-install-update-01.md) , auf Ihrem StorSimple virtuelle Array.

## <a name="references"></a>Verweise

Suchen Sie nach einer älteren Version Notiz? Gehe zu: 

- [StorSimple virtuelle Array Update 0,1 und 0,2 – Versionsinformationen](storsimple-ova-update-01-release-notes.md)

- [StorSimple virtuelle Array allgemeine Verfügbarkeit Versionsinformationen](storsimple-ova-pp-release-notes.md)
 

