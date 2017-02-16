<properties 
   pageTitle="StorSimple virtuelle Array Updates Versionsinformationen | Microsoft Azure"
   description="Beschreibt öffnen Probleme und Lösungen für das Ausführen von Update 0,2 und 0,1 virtuelle StorSimple-Array."
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
   ms.date="06/16/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>StorSimple virtuellen Array Update 0,2 und 0,1 – Versionsinformationen

## <a name="overview"></a>(Übersicht)

Die folgenden Versionsinformationen stellen die kritische öffnen sowie die gelöst Probleme für Microsoft Azure StorSimple Virtual Array Updates. (Microsoft Azure StorSimple virtuelle Matrix ist auch bekannt als StorSimple lokale virtuelle Gerät oder virtuelle Gerät StorSimple.) 

Die Versionsinformationen werden kontinuierlich aktualisiert und kritische Probleme, die erfordern einer problemumgehung entdeckt werden, werden diese hinzugefügt. Bevor Sie Ihre StorSimple virtuelle Gerät bereitstellen, überprüfen Sie sorgfältig die Informationen in den Versionsinformationen enthalten sind.

Aktualisieren von 0,2 entspricht der Software Version **10.0.10280.0**. Aktualisieren von 0,1 ist Version **10.0.10279.0**. Im folgenden Abschnitt aufgelistet, die Änderungen für jedes Update. 

> [AZURE.NOTE] Updates werden Unterbrechung und startet das Gerät neu. Wenn e/a ausgeführt wird, wird das Gerät Ausfallzeiten anfallen.

## <a name="issues-fixed-in-the-update-02"></a>Probleme bei der Aktualisierung 0,2
Aktualisieren von 0,2 umfasst alle Änderungen von Update 0,1 sowie in der folgenden Tabelle beschriebenen Fix:

Feature                              | Problem                                                                                                                                                                                                                                                                                                                           |
--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
Updates                                 | In der letzten Version waren nicht, damit Sie mit der lokalen Web-Benutzeroberfläche zum Installieren von Updates hatten Aktualisierungen im Portal Azure klassischen automatisch erkannt. In dieser Version wird dieses Problem behoben. Nach der Installation von Update 0,2, können Sie den zukünftigen Updates, die über das klassische Azure-Portal installieren.                       

## <a name="whats-new-in-the-update-01"></a>Was ist neu in der Aktualisierung 0.1

Aktualisieren von 0,1 enthält die folgenden Updates und verbesserte. 

- **Verbesserte Stabilität für Cloud Ausfall**: dieser Version sind in mehreren alle Updates um Wiederherstellung, sichern, wiederherstellen und im Falle einer Cloud Connectivity Unterbrechung Stufen. 

- **Verbesserte Leistung wiederherzustellen**: dieser Version sind in alle Updates, die Sie sich die Abschlusszeit der Einzelvorgänge wiederherstellen erheblich Ausschneiden haben.

- **Automatische Optimierung der Reservierung**: Wenn Daten auf Speicherdefizite Datenmengen gelöscht werden, müssen die Blöcke nicht verwendeter Speicher freigegeben werden. Diese Version hat den Speicherplatz freigeben Prozess aus der Cloud in der nicht verwendete Speicherplatz verfügbar schneller im Vergleich zu älteren Versionen resultierender verbessert.

- **Neue virtuelle Laufwerk Bilder**: neue virtuelle Festplatte, VHDX und VMDK sind jetzt über das klassische Azure-Portal zur Verfügung. Sie können diese Bilder, um neue Update 0,1 Geräte Bereitstellung herunterladen.

- **Verbessern Sie die Genauigkeit der Einzelvorgänge Status im Portal**: In der früheren Version der Software, Position Statusberichte im Portal wurde nicht präzise. In dieser Version wird dieses Problem behoben.

- **Beitreten zu einer Domäne auftreten**: Updates im Zusammenhang mit Domäne beitritt und Umbenennen des Geräts.


## <a name="issues-fixed-in-the-update-01"></a>Probleme in das Update 0,1 auf.

Die folgende Tabelle enthält eine Zusammenfassung der Probleme in dieser Version.

| Nein.  | Feature                              | Problem                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | VMDK                                 | In manchen Versionen VMware wurde der Datenträger OS als bewirken, dass Benachrichtigungen verschlüsselten und andere normale Vorgänge unterbrechen angezeigt. In dieser Version wurde dieses Problem behoben.                                                                                                                                                                                    |
| 2    | iSCSI-server                         | In der letzten Version war der Benutzer zum Angeben eines Gateways für jede Netzwerkschnittstelle aktiviert Ihres StorSimple virtuellen Geräts erforderlich. Dieses Verhalten wird in dieser Version wurden geändert, sodass der Benutzer mindestens ein Gateway für alle aktivierten Netzwerkschnittstellen konfigurieren muss.                                                                              |
| 3    | Support-Paket                      | Unterstützen Sie in der früheren Version der Software Paket Websitesammlung fehlgeschlagen ist, wenn die Paketgrößen größer als 1 GB wurden. In dieser Version wird dieses Problem behoben.                                                                                                                                                                               |
| 4    | Cloudzugriff                         |  In der letzten Version Wenn die StorSimple virtuelle Matrix verfügt nicht über Netzwerkkonnektivität und neu gestartet wurde, müssten die lokale Benutzeroberfläche Verbindungsprobleme. In dieser Version wird dieses Problem behoben.                                                                                                                            |
| 5    | Überwachen von Diagrammen                    | In der vorherigen Version, und folgen einem Gerät Failover angezeigt die Cloud Kapazität Auslastung Diagramme falsche Werten in der klassischen Azure-Portal an. Dies ist in der aktuellen Version behoben.                                                                                                                          |



## <a name="known-issues-in-the-update-01"></a>Bekannte Probleme bei der Aktualisierung 0.1

In der folgenden Tabelle enthält eine Zusammenfassung der bekannten Probleme für das StorSimple virtuelle Array und enthält die Probleme aus früheren Versionen Release notiert haben. **Die in dieser Version notiert haben Probleme-Version mit ein Sternchen gekennzeichnet sind. Fast alle Probleme in dieser Liste haben von der GA-Version des StorSimple virtuelle Arrays übertragen.**


| Nein. | Feature | Problem | Problemumgehung/Kommentare |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Updates | Die virtuelle Geräte, die in der Preview-Version erstellt können nicht auf eine unterstützte Version der allgemeinen Verfügbarkeit aktualisiert werden. | Diese virtuellen Geräte müssen über für die allgemeine Verfügbarkeit-Version mit einem Disaster Wiederherstellung (DR) Workflow Fehler werden. |
| **2.** | Bereitgestellte Datenträger | Nachdem Sie einen Datenträger einer bestimmten angegebenen Größe bereitgestellt haben und erstellt das entsprechende StorSimple virtuelle Gerät, Sie müssen nicht vergrößern oder verkleinern den Datenträger Daten. Vergeblich versucht, führt zu einem Verlust der alle Daten in den lokalen Ebenen des Geräts. |   |
| **3.** | Gruppenrichtlinien | Wenn ein Gerät Domäne gehört, kann Anwenden einer Gruppenrichtlinie für die den Gerätevorgang beeinträchtigen. | Stellen Sie sicher, dass Ihr virtuelle Array in einem eigenen (Organisationseinheit) für Active Directory ist und keine Gruppenrichtlinienobjekte (Gruppenrichtlinienobjekt) angewendet werden.|
| **4.** | Lokale Web-Benutzeroberfläche | Wenn die Features für die erweiterte Sicherheit in Internet Explorer (IE ESC) aktiviert sind, möglicherweise einige lokale Webseiten des Benutzeroberfläche wie Problembehandlung oder Wartung nicht ordnungsgemäß funktioniert. Schaltflächen auf diesen Seiten funktionieren möglicherweise auch nicht. | Verbesserte Sicherheit-Funktionen in Internet Explorer deaktivieren.|
| **5.** | Lokale Web-Benutzeroberfläche | In einer Hyper-V virtuellen Computern Schnittstellen Netzwerk-Schnittstellen im Web Benutzeroberfläche Leuchten als 10 Gbps an. | Dieses Verhalten ist eine Spiegelung von Hyper-V. Hyper-V zeigt immer 10 Gbps für virtuelle Netzwerkadapter. |
| **6.** | Gestufte Datenmengen oder Freigaben | Byte-Bereich durch das Sperren für Applikationen, die Arbeiten mit der StorSimple gestufte Datenmengen wird nicht unterstützt. Wenn Byte Bereich Sperren aktiviert ist, wird StorSimple Stufen funktioniert nicht. | Empfohlene Maßnahmen umfassen: <br></br>Deaktivieren Sie in der Anwendungslogik Sperren Byte-Bereich ein.<br></br>Wählen Sie aus, die Daten für diese Anwendung lokal angeheftete Datenmengen im Gegensatz zum gestufte Datenmengen richtige.<br></br>*Einschränkung*: Wenn mit lokal Datenmengen angehefteten und Byte Bereich Sperren aktiviert ist, beachten Sie, dass die Lautstärke lokal angeheftete online sein kann, bevor die Wiederherstellung abgeschlossen ist. In diesen Fällen ist eine Wiederherstellung wird ausgeführt, müssen Sie Abschluss des Wiederherstellungsvorgangs zu warten. |
| **7.** | Gestufte Freigaben | Arbeiten mit großer Dateien kann sich langsam Ebene führen. | Bei der Arbeit mit großer Dateien, empfehlen wir, dass die größte Datei kleiner als 3 % der Größe freigeben. |
| **8.** | Kapazität für Freigaben verwendet | Sehen Sie möglicherweise Verbrauch in Abwesenheit von Daten für die Freigabe freigeben. Dies ist, da die verwendete Kapazität für Freigaben Metadaten umfasst. |   |
| **9.** | Wiederherstellung | Sie können nur von einem Dateiserver in derselben Domäne wie die des Quellgeräts die Wiederherstellung durchführen. Wiederherstellung an ein Gerät in eine andere Domäne wird in dieser Version nicht unterstützt. | Dies wird in einer späteren Version implementiert werden. |
| **10.** | Azure PowerShell | Über die Azure PowerShell in dieser Version werden nicht die StorSimple virtuelle Geräte verwaltet. | Die Verwaltung der virtuellen Geräte sollten über das Azure klassischen Portal und der lokale Web-Benutzeroberfläche erfolgen. |
| **11.** | Kennwort ändern | Die virtuelle Array Gerät Konsole akzeptiert nur Eingabe im En-US-Tastatur-Format. |   |
| **12.** | CHAP | CHAP-Anmeldeinformationen, die nach der Erstellung können nicht entfernt werden. Wenn Sie die CHAP-Anmeldeinformationen ändern möchten, müssen Sie darüber hinaus Offlineschalten der Datenmengen, und zeigen Sie diese online die vorgenommene Änderung wirksam wird. | Diese werden in einer späteren Version behoben sein. |
| **13.** | iSCSI-server  | Die 'verwendete Speicher' für einen Datenträger iSCSI angezeigt möglicherweise in der StorSimple Manager-Dienst und dem iSCSI Host anders. | Die iSCSI-Server verfügt über das Dateisystem-Ansicht.<br></br>Das Gerät sieht der zugeordnet wird, wenn die Lautstärke am die maximale Größe wurde blockiert.|
| **14.** | Datei-Server *  | Wenn eine Datei in einem Ordner eine alternative Daten Stream (anzeigen) zugeordnet ist, wird die anzeigen nicht gesichert oder über Wiederherstellung, Klonen und Element Ebene Wiederherstellung wiederhergestellt.| |


## <a name="next-step"></a>Als Nächstes

[Installieren von Updates](storsimple-ova-install-update-01.md) auf Ihre StorSimple Virtual Array.
