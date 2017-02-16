<properties 
   pageTitle="Azure mobilen Engagement Problembehandlungsleitfadens - Analytics" 
   description="Behandeln von Problemen mit Analytics, Überwachung, Segmentierung und Dashboard in Azure Mobile Engagement" 
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

# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a>Problembehandlung bei der Analytics, Überwachung, Segmentierung und Dashboard-Problemen

Im folgenden werden mögliche Probleme mit wie Azure Mobile Engagement Informationen über Ihre Anwendungen, Geräte und Benutzer sammelt treten möglicherweise.

## <a name="missingdelayed-information"></a>Fehlende/verzögerte Informationen

### <a name="issue"></a>Problem
- Informationen, wird in in Analytics, Segmentierung oder Dashboard angezeigte verzögert.
- Überwachung, fehlen Informationen.
- Fehlen Informationen Analytics, Segmentierung oder Dashboard.
- Grenzwerte für anzunehmen Segmentierung.

### <a name="causes"></a>Bewirkt, dass

- Können die Analytics-API Monitor-API und Segmente-API, um festzustellen, ob alle Daten Sie in der Benutzeroberfläche fehlen durch die APIs zu sehen ist.
- Wenn die Azure Mobile Engagement SDK nicht ordnungsgemäß in Ihre app integriert ist können dann Sie Informationen in den Analytics, Segmentierung, Überwachung oder Dashboards sehen nicht.
- Segmenten können nicht geändert, nachdem sie erstellt wurden, Segmente können nur "duplizierten" (kopiert) oder werden "gelöscht" (gelöscht). Segmente können nur 10 Kriterien enthalten.
- Die beste Methode zum Testen fehlenden Informationen von der Überwachung wird zu einem Testgerät für die Einrichtung, deinstallieren Sie und/oder installieren die app auf dem Testgerät.
- Informationen zur ist alle 24 Stunden für Analytics, Segmentierung oder Dashboards aktualisiert.
- Informationen in neue Segmente können nicht angezeigt werden, bis zu 24 Stunden, nachdem sie erstellt wurden, auch wenn das Segment zuvor genannten Informationen basiert.
- Filtern von Analytics-Daten in der Benutzeroberfläche werden alle dieses Typs unabhängig von der Version der app Beispielen (z. B. "stürzt ab" namentlich gefiltert werden von Version 1 und Version 2 der app angezeigt).
- Der Zeitraum für Analytics basiert auf dem Datum in der Benutzer Gerät Einstellungen, damit ein Benutzer, dessen Telefon des Datums nicht ordnungsgemäß festgelegt wurde, in der falschen Zeitraum angezeigt konnte.
- Legt keine serverseitigen, die Daten angemeldet ist, wenn Sie die Schaltfläche mithilfe "testen", Daten werden nur für Pushbenachrichtigungen tatsächliche massensendungen zu ermitteln protokolliert.

## <a name="cant-locate-items-in-ui"></a>Elemente in der Benutzeroberfläche nicht gefunden werden.

### <a name="issue"></a>Problem
- Keine Erstellen von Segmenten basierend auf bestimmte integrierte oder benutzerdefinierte app Informationen Kategorisieren von Kriterien.
- Nicht finden bestimmte integrierte oder benutzerdefinierte app Informationen Kategorisieren von Kriterien in Analytics, Überwachung oder Dashboards.
- Die Daten in Analytics, Überwachung, Segmentierung oder Dashboards nicht interpretiert werden.

### <a name="causes"></a>Bewirkt, dass

- Einige Elemente integriert und app Info Kategorien stehen nur als Pushbenachrichtigungen Kriterien aber möglicherweise nicht zu einem Segment hinzugefügt oder sichtbar aus Analytics, Überwachung oder Dashboard. 
- Für integrierte Elemente und app Info Kategorien, die ein Segment hinzugefügt werden können, müssen Setup-Liste der Kriterien in jeder für eine Marketingkampagne verwendet, Sie die gleiche Funktion wie verwendet, basierend auf einem Segment durchführen.
- Finden Sie in den Kontextmenüs in den Abschnitten Analytics, Überwachung, Segmentierung und Dashboards der Azure Mobile Engagement Benutzeroberfläche für weitere Hilfe und Informationen zu Vorgehensweisen.

## <a name="crash-troubleshooting"></a>Problembehandlung bei Abstürzen

### <a name="issue"></a>Problem
- Anwendung stürzt ab in Analytics, Überwachung oder Dashboard angezeigt werden.

### <a name="causes"></a>Bewirkt, dass

- Zur Problembehandlung Anwendung stürzt ab, angezeigt in Analytics, Überwachung oder Dashboard stellen Sie sicher, überprüfen Sie die Versionsinformationen bekannten Problemen mit früheren Versionen von SDK.
- Weiter zur Problembehandlung ausführen Anwendungsabstürzen ein Ereignisses aus einer Testgerät mit der Anwendung installiert und Aussehen von Ihrem Gerät-ID im Abschnitt "Monitor – Ereignisse" auf der Benutzeroberfläche Azure Mobile Engagement. Führen Sie dann das Ereignis, das die Anwendung abstürzen und zusätzliche Informationen im Abschnitt "Monitor – Absturz" der Benutzeroberfläche Azure Mobile Engagement Nachschlagen verursacht. 

 
