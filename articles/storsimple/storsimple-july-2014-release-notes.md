<properties 
   pageTitle="Lassen Sie StorSimple 8000 Versionsinformationen | Microsoft Azure"
   description="Beschreibt die neuen Features, Tagesordnungspunkte und problemumgehungen zur Verfügung für die Juli 2014 Microsoft Azure StorSimple Release."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>StorSimple 8000 Reihe aktualisierte Version Versionsinformationen - Juli 2014 

## <a name="overview"></a>(Übersicht)

Die Folgen Versionsinformationen Identifizieren der geöffneten Probleme für die StorSimple 8000 Reihe Juli 2014 allgemeine Verfügbarkeit (GA) Version von Microsoft Azure StorSimple. Diese Version bezieht sich auf Softwareversion 6.3.9600.17215.  

Sofern nicht anders angegeben, werden diese Versionsinformationen gelten für alle Modelle des Geräts StorSimple. Die Versionsinformationen werden kontinuierlich aktualisiert. Probleme mit Anforderung der problemumgehung entdeckt werden, werden sie hinzugefügt. Bevor Sie Ihre Microsoft Azure StorSimple Lösung bereitstellen, beachten Sie die folgenden Informationen ein.  

## <a name="known-issues-in-this-release"></a>Bekannte Probleme in dieser Version
Die folgende Tabelle enthält eine Übersicht über bekannte Probleme in dieser Version.  
 
| Nein. | Feature | Problem | Kommentare/dieses Problem zu umgehen | Gilt für physische Gerät | Gilt für virtuelles Gerät |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Factory zurücksetzen | In einigen Fällen beim Ausführen einer Factory zurücksetzen das StorSimple Gerät ist hängen geblieben und diese Meldung angezeigt: **Zurücksetzen Factory wird ausgeführt (Phase 8)**. Dies passiert, wenn Sie STRG + C drücken, während das Cmdlet ausgeführt wird. | Drücken Sie STRG + C nicht nach dem Initiieren einer Factory zurücksetzen. Wenn Sie bereits in diesem Zustand sind, wenden Sie sich an den Microsoft-Support, für den nächsten Schritten fort. | Ja | Nein |
| 2 | Quorum Datenträger | Gelegentlich Wenn die Mehrzahl der Datenträger in der Einheit EBOD einer 8600 Gerät getrennt werden keine Quorum Datenträger wodurch, werden Speicherpool offline. Es verbleibt offline, auch wenn die Datenträger erneut verbunden sind. | Sie müssen das Gerät neu starten. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support, für den nächsten Schritten fort. | Ja | Nein |
| 3 | Cloud Momentaufnahme Fehlern | Gelegentlich kann eine Momentaufnahme der Cloud tritt der Fehler **Maximum Sicherung Grenzwert erreicht**. Dies geschieht, wenn Sie 255 online Klonen auf dem gleichen Gerät, aus der gleichen ursprünglichen Lautstärke überschreiten, die gelöscht wurden. | | Ja | Ja |
| 4 | Falsche Controller-ID | Wenn kein Ersatz Controller ausgeführt wird, kann als Controller 1 Controller 0 angezeigt. Während Controller Ersatz nachdem das Bild aus dem Peerknoten geladen wird kann die Controller-ID Anfangs als dem Peer-Controller ID angezeigt Dieses Verhalten möglicherweise auch gelegentlich nach einem Neustart des Systems angezeigt. | Es ist keine Benutzeraktion erforderlich. Dies wird sich selbst beheben nach Abschluss der Controller ersetzt. | Ja | Nein |
| 5 | Überwachen von Diagrammen Gerät | Der Dienst StorSimple Manager die Überwachung Gerät Diagramme funktionieren nicht, wenn grundlegende oder NTLM-Authentifizierung aktiviert ist, in der Konfiguration des Proxyservers für das Gerät. | Ändern Sie die Web-Proxy-Konfiguration für das Gerät mit Ihrem Dienst StorSimple Manager registriert ist, damit die Authentifizierung auf keine festgelegt ist. Führen Sie hierzu die der Windows PowerShell für StorSimple Set-HcsWebProxy-Cmdlet. | Ja | Ja |
| 6 | Speicherkonten | Verwenden zum Löschen des Speicherkontos der Speicherdienst ist ein nicht unterstütztes Szenario. Dies wird zu einer Situation führen, in dem Benutzerdaten abgerufen werden können. | | Ja | Ja |
| 7 | Failback | Ein Failback innerhalb von 24 Stunden der Wiederherstellung (DR) wird nicht unterstützt. | | Ja | Nein |
| 8 | Geräte-failover | Mehrere Failovers eines Containers aus dem gleichen Quellgerät Lautstärke für verschiedene Geräte wird nicht unterstützt. Failover auf einem einzelnen inaktive Gerät auf mehreren Geräten machen Lautstärke Container auf der ersten konnte nicht über Gerät Besitz Daten gehen verloren. Nach einem Failover werden diese Lautstärke Container angezeigt oder Verhalten sich anders, wenn Sie in der klassischen Azure-Portal angezeigt werden. | | Ja | Nein |
| 9 | Installation | Während der StorSimple Netzwerkadapter für SharePoint-Installation müssen Sie bieten eine Gerät IP-Adresse für die Installation wurde erfolgreich abgeschlossen. | | Ja | Nein |
| 10 | Netzwerk-Schnittstellen | Netzwerk-Schnittstellen Daten 2 und 3 von Daten wurden in der Software ausgetauscht. | Microsoft-Support wenden Sie sich an, wenn Sie diese Schnittstellen konfigurieren müssen. | Ja | Nein |


 
