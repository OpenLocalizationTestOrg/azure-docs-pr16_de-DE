<properties 
   pageTitle="Azure mobilen Engagement Problembehandlungsleitfadens - Dienst" 
   description="Problembehandlung von Führungslinien für Azure mobilen Engagement" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-service-issues"></a>Problembehandlung bei der Dienstprobleme

Nachfolgend werden mögliche Probleme auftreten können mit Azure Mobile Engagement wie ausgeführt wird.

## <a name="service-outages"></a>Ausfall eines Dienstes

### <a name="issue"></a>Problem
- Probleme, die durch Ausfall eines Azure Mobile Engagement Dienstes verursacht werden angezeigt.

### <a name="causes"></a>Bewirkt, dass
- Probleme, die angezeigt werden, über den Ausfall eines Azure Mobile Engagement Dienstes verursacht werden können mehrere verschiedene Ursachen werden:
    - Isoliert Probleme, die ursprünglich für alle eines Auftrags für Mobile Azure systematische angezeigt werden
    - Bekannte Probleme, die aufgrund von Server Ausfall (nicht immer scheint Status Servers):
    - Planen von verzögert, Zielgruppenadressierung Fehler, Badge Update Probleme Statistiken beenden sammeln, Pushbenachrichtigungen Stopps arbeiten,-APIs beenden arbeiten, neue apps oder Benutzer kann nicht erstellt werden, DNS-Fehler und Timeoutfehler UI, API oder Apps auf einem Gerät.
    - Cloud Abhängigkeit Ausfall [Azure-Dienststatus](http://status.azure.com/)
    - Benachrichtigung Services (PNS) Abhängigkeit Ausfall Pushbenachrichtigungen
    - Ausfall der App Store

1) Klicken Sie zum Testen, um festzustellen, ob das Problem systematische ist, können Sie die gleiche Funktion aus einem anderen testen
   
   - Azure Mobile Engagement integriert-Anwendung
   - Testgerät
   - Gerät OS-Testversion
   - Für eine Marketingkampagne
   - Administratorkonto an
   - Browser (IE, Firefox, Chrome, usw.)
   - Computer

2) Zu testen, ob das Problem nur die Benutzeroberfläche oder die-APIs betrifft:

   - Testen Sie die gleiche Funktion aus der Azure Mobile Engagement Benutzeroberfläche und Azure Mobile Engagement API.

3) Zu testen, ob das Problem mit Ihrem Mobiltelefon Netzwerk ist:

   - Testen Sie während der Verbindung mit dem Internet über WIFI und über Ihr Mobiltelefon-Netzwerk 3 G verbunden.
   - Bestätigen Sie, dass Ihre Firewall einen der Azure Mobile Engagement IP-Adressen oder Ports nicht blockiert.

4) Zu testen, ob das Problem mit Ihrem Gerät ist:

   - Testen Sie, ob Ihr Gerät Verbindung zu Azure Mobile Engagement mit einer anderen integrierten Azure Mobile Engagement-app hergestellt wird.
   - Testen, die Sie Ereignisse, Aufträge und Absturz von Ihrem Telefon generieren können, die in der Azure Mobile Engagement Benutzeroberfläche angezeigt werden können. 
   - Testen Sie, ob Sie Pushbenachrichtigungen auf Ihr Gerät basierend auf deren Geräte-ID aus der Azure Mobile Engagement Benutzeroberfläche senden können 

5) Zu testen, ob das Problem mit der App ist:

   - Installieren Sie und Testen Sie die Anwendung aus einem Emulator statt auf einem physischen Gerät:
   
6) Zu testen, ob das Problem mit OS Upgrades auf Endbenutzer Geräte, das erfordern ein Upgrade SDK wird zu beheben:

   - Testen Sie die Anwendung auf verschiedenen Geräten mit unterschiedlichen Versionen des Betriebssystems.
   - Bestätigen Sie, dass Sie die neueste Version des SDK verwenden.
 
## <a name="connectivity-and-incorrect-information-issues"></a>Konnektivität und falsche Informationen zu Problemen

### <a name="issue"></a>Problem
- Probleme beim Anmelden bei der Benutzeroberfläche von Azure Mobile Engagement.
- Fehler bei der Verbindung mit Azure Mobile Engagement API.
- Probleme, die App Info Kategorien über die Gerät-API hochladen.
- Probleme beim Herunterladen von Protokollen oder die exportierten Daten aus Azure Mobile Engagement.
- Falsche Informationen werden auf der Benutzeroberfläche Azure Mobile Engagement angezeigt.
- Falsche Informationen in Azure Mobile Engagement Protokolle angezeigt.

### <a name="causes"></a>Bewirkt, dass
* Bestätigen Sie, dass Ihr Benutzerkonto über die erforderlichen Berechtigungen zum Durchführen der Aufgabe verfügt.
* Bestätigen Sie, dass das Problem nicht auf einem Computer oder Ihr lokales Netzwerk beschränkt ist.
* Bestätigen Sie, dass der Azure Mobile Engagement-Dienst keine besitzt Ausfall gemeldet.
* Stellen Sie sicher, dass Ihre App Info Kategorie Dateien alle diese Regeln folgen:
    - Verwenden Sie nur den UTF8-Zeichensatz (ANSI-Zeichensatzes wird nicht unterstützt).
    - Verwenden Sie ein Komma "," als Trennzeichen (Sie können einen Dienst Anforderung zu Anforderung so ändern Sie das CSV-Trennzeichen aus einem Komma öffnen "," in ein anderes Zeichen wie ein Semikolon ";").
    - Verwenden Sie "true" und "false" alle Kleinbuchstaben für boolesche Werte.
    - Verwenden Sie eine Datei, die kleiner als die maximale Dateigröße von 35 MB ist.
 
