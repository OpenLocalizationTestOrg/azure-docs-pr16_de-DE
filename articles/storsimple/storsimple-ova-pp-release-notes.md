<properties 
   pageTitle="StorSimple virtuelle Array Versionsinformationen | Microsoft Azure"
   description="Beschreibt öffnen Probleme und Lösungen für die virtuelle StorSimple-Matrix."
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
   ms.date="05/13/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-release-notes"></a>StorSimple virtuelle Array – Versionsinformationen

## <a name="overview"></a>(Übersicht)

Die folgenden Versionsinformationen stellen die kritische öffnen Probleme bei der März 2016 allgemeine Verfügbarkeit (GA) Version des Microsoft Azure StorSimple virtuelle Arrays (auch bekannt als StorSimple lokale virtuelle Gerät oder dem StorSimple virtuelle Gerät). Diese Version bezieht sich auf Softwareversion 10.0.10271.0.

Die Versionsinformationen werden kontinuierlich aktualisiert und kritische Probleme, die erfordern einer problemumgehung entdeckt werden, werden diese hinzugefügt. Bevor Sie Ihre StorSimple virtuelle Gerät bereitstellen, überprüfen Sie sorgfältig die Informationen in den Versionsinformationen enthalten sind. 

Die folgende Tabelle enthält eine Übersicht über bekannte Probleme in dieser Version.


| Nein. | Feature | Problem | Problemumgehung/Kommentare |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Updates | Die virtuelle Geräte, die in der Preview-Version erstellt können nicht auf eine unterstützte Version der allgemeinen Verfügbarkeit aktualisiert werden. | Diese virtuellen Geräte müssen über für die allgemeine Verfügbarkeit-Version mit einem Disaster Wiederherstellung (DR) Workflow Fehler werden. |
| **2.** | Bereitgestellte Datenträger | Nachdem Sie einen Datenträger einer bestimmten angegebenen Größe bereitgestellt haben und erstellt das entsprechende StorSimple virtuelle Gerät, Sie müssen nicht vergrößern oder verkleinern den Datenträger Daten. Vergeblich versucht, führt zu einem Verlust der alle Daten in den lokalen Ebenen des Geräts. |   |
| **3.** | Gruppenrichtlinien | Wenn ein Gerät Domäne gehört, kann Anwenden einer Gruppenrichtlinie für die den Gerätevorgang beeinträchtigen. | Stellen Sie sicher, dass Ihr virtuelle Array in einem eigenen (Organisationseinheit) für Active Directory ist und keine Gruppenrichtlinienobjekte (Gruppenrichtlinienobjekt) angewendet werden.|
| **4.** | Lokale Web-Benutzeroberfläche | Wenn die Features für die erweiterte Sicherheit in Internet Explorer (IE ESC) aktiviert sind, möglicherweise einige lokale Webseiten des Benutzeroberfläche wie Problembehandlung oder Wartung nicht ordnungsgemäß funktioniert. Schaltflächen auf diesen Seiten funktionieren möglicherweise auch nicht. | Verbesserte Sicherheit-Funktionen in Internet Explorer deaktivieren.|
| **5.** | Lokale Web-Benutzeroberfläche | In einer Hyper-V virtuellen Computern Schnittstellen Netzwerk-Schnittstellen im Web Benutzeroberfläche Leuchten als 10 Gbps. | Dieses Verhalten ist eine Spiegelung von Hyper-V. Hyper-V zeigt immer 10 Gbps für virtuelle Netzwerkadapter. |
| **6.** | Gestufte Datenmengen oder Freigaben | Byte-Bereich durch das Sperren für Applikationen, die Arbeiten mit der StorSimple gestufte Datenmengen wird nicht unterstützt. Wenn Byte Bereich Sperren aktiviert ist, wird StorSimple Stufen funktioniert nicht. | Empfohlene Maßnahmen umfassen: <br></br>Deaktivieren Sie in der Anwendungslogik Sperren Byte-Bereich ein.<br></br>Wählen Sie aus, die Daten für diese Anwendung lokal angeheftete Datenmengen im Gegensatz zum gestufte Datenmengen richtige.<br></br>*Einschränkung*: Wenn mit lokal Datenmengen angehefteten und Byte Bereich Sperren aktiviert ist, beachten Sie, dass die Lautstärke lokal angeheftete online sein kann, bevor die Wiederherstellung abgeschlossen ist. In diesen Fällen ist eine Wiederherstellung wird ausgeführt, müssen Sie Abschluss des Wiederherstellungsvorgangs zu warten. |
| **7.** | Gestufte Freigaben | Arbeiten mit großer Dateien kann sich langsam Ebene führen. | Bei der Arbeit mit großer Dateien, empfehlen wir, dass die größte Datei kleiner als 3 % der Größe freigeben. |
| **8.** | Kapazität für Freigaben verwendet | Sehen Sie möglicherweise Verbrauch in Abwesenheit von Daten für die Freigabe freigeben. Dies ist, da die verwendete Kapazität für Freigaben Metadaten umfasst. |   |
| **9.** | Wiederherstellung | Sie können nur von einem Dateiserver in derselben Domäne wie die des Quellgeräts die Wiederherstellung durchführen. Wiederherstellung an ein Gerät in eine andere Domäne wird in dieser Version nicht unterstützt. | Dies wird in einer späteren Version implementiert werden. |
| **10.** | Azure PowerShell | Über die Azure PowerShell in dieser Version werden nicht die StorSimple virtuelle Geräte verwaltet. | Die Verwaltung der virtuellen Geräte sollten über das Azure klassischen Portal und der lokale Web-Benutzeroberfläche erfolgen. |
| **11.** | Kennwort ändern | Die virtuelle Array Gerät Konsole akzeptiert nur Eingabe im En-US-Tastatur-Format. |   |
| **12.** | CHAP | CHAP-Anmeldeinformationen, die nach der Erstellung können nicht entfernt werden. Wenn Sie die CHAP-Anmeldeinformationen ändern möchten, müssen Sie darüber hinaus Offlineschalten der Datenmengen, und zeigen Sie diese online die vorgenommene Änderung wirksam wird. | Diese werden in einer späteren Version behoben sein. |
| **13.** | iSCSI-server  | Die 'verwendete Speicher' für einen Datenträger iSCSI angezeigt möglicherweise in der StorSimple Manager-Dienst und dem iSCSI Host anders. | Die iSCSI-Server verfügt über das Dateisystem-Ansicht.<br></br>Das Gerät sieht der zugeordnet wird, wenn die Lautstärke am die maximale Größe wurde blockiert.|
