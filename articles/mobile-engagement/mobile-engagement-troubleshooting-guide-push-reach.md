<properties 
   pageTitle="Azure mobilen Engagement Problembehandlungsleitfadens - Pushbenachrichtigungen/Reichweite" 
   description="Problembehandlung bei der Interaktion und Benachrichtigung in Azure Mobile Engagement" 
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

# <a name="troubleshooting-guide-for-push-and-reach-issues"></a>Problembehandlung bei der Pushbenachrichtigungen Sie und erreicht haben Sie Probleme

Nachfolgend werden mögliche Probleme mit wie Azure Mobile Engagement Informationen für Ihre Benutzer sendet auftreten können.
 
## <a name="push-failures"></a>Pushbenachrichtigungen Fehlern

### <a name="issue"></a>Problem
- Schiebt funktionieren nicht (in der app aus dem app oder beide).

### <a name="causes"></a>Bewirkt, dass
- Oft ein Fehler Pushbenachrichtigungen weist eine Azure Mobile Engagement, Zeit oder eine andere erweiterte Funktion eines Auftrags für Mobile Azure nicht ordnungsgemäß integriert sind oder ein Upgrade im SDK erforderlich ist, um mit einer neuen OS oder Gerät Plattform ein bekanntes Problem zu beheben.
- Testen Sie nur eine In App Pushbenachrichtigungen und nur eine abwesend App Pushbenachrichtigungen um festzustellen, ob eine Bedingung ein Problem In App oder abwesend App ist.
- Testen der Benutzeroberfläche und den API im Rahmen der Problembehandlung zu sehen, welche zusätzliche Fehlerinformationen verfügbar ist beiden Orten.
- Abmelden bei der App funktionieren schiebt nicht, wenn sowohl Azure Mobile Engagement und Reichweite im SDK integriert sind.
- Schiebt wenn Zertifikate nicht gültig sind, oder PROD im Vergleich zu Entwickler ordnungsgemäß verwenden funktionieren nicht (nur iOS). (**Hinweis:** "von"-app Pushbenachrichtigungen möglicherweise nicht übermittelt werden zu iOS, wenn Sie sowohl die Entwicklung (Entwickler) und (PROD) Versionen der Anwendung auf dem gleichen Gerät installiert werden, da das Zertifikat des Sicherheitstokens zugeordnet von Apple ungültig gemacht werden können. Um dieses Problem zu beheben, deinstallieren Sie die Entwicklung und PROD Versionen der Anwendung, und installieren Sie nur die Version auf Ihrem Gerät neu zu.)
- Abmelden bei der App Pushbenachrichtigungen werden zählt in verschiedenen Plattformen anders gehandhabt (iOS zeigt weniger Informationen als Android aus, wenn auf einem Gerät systemeigenen schiebt deaktiviert sind, kann die API mehr Informationen als der Benutzeroberfläche Geben Sie auf Pushbenachrichtigungen Stats).
- Abmelden bei der App können schiebt durch Kunden OS Ebene (iOS und Android) blockiert werden.
- Abmelden bei der App werden schiebt angezeigt, als in der Azure Mobile Engagement Benutzeroberfläche deaktiviert, wenn sie werden nicht ordnungsgemäß integriert, aber möglicherweise automatisch aus der-API fehl.
- In der App funktionieren nicht schiebt, es sei denn, sowohl Azure Mobile Engagement und Reichweite im SDK integriert sind.
- GCM und ADM schiebt funktionieren nicht, es sei denn, Azure Mobile Engagement und auf dem Server bestimmte im SDK (nur Android) integriert sind.
- In der App und abwesend App sollten schiebt separat getestet werden, um festzustellen, ob es sich um ein Problem Pushbenachrichtigungen oder Reichweite ist.
- In der App schiebt erfordern, dass die app empfangen werden geöffnet sein.
- In der App ist schiebt häufig Setup nach einer Anmeldung oder Abmeldung app Info Kategorie gefiltert werden.
- Wenn Sie eine benutzerdefinierte Kategorie in Reichweite Anzeige von Benachrichtigungen in-app verwenden, müssen Sie den richtigen Lebenszyklus der Benachrichtigung folgen, da sonst die Benachrichtigung kann nicht deaktiviert werden bei der Benutzer schließen sie.
- Wenn Sie eine für eine Marketingkampagne, ohne Enddatum starten und ein Gerät empfangen werden, die im app Benachrichtigung jedoch erhalten bleibt nicht anzeigen, die es noch, immer noch der Benutzer wird darüber informiert das nächste Mal sie melden Sie sich bei der app, auch wenn Sie die für eine Marketingkampagne manuell beenden.
- Probleme mit der Pushbenachrichtigungen-API bestätigen Sie wirklich möchten verwenden die API Pushbenachrichtigungen statt der API erreicht haben (da die API erreichen öfter verwendet wird), und Sie nicht die Parameter "alle Aspekte" und "Notifier" verwirrt werden.
- Testen Sie Ihre Pushbenachrichtigungen für eine Marketingkampagne mit beide ein Gerät wider, die über Wi-Fi und 3G, um die Netzwerkverbindung als mögliche Ursache von Problemen zu unterdrücken.

## <a name="push-testing"></a>Testen von Pushbenachrichtigungen

### <a name="issue"></a>Problem
- Schiebt können ein bestimmtes Gerät basierend auf einem Gerät ID gesendet werden

### <a name="causes"></a>Bewirkt, dass

- Test-Geräte werden für jede Plattform anders einrichten, aber bewirken, dass ein Ereignis in Ihrer app auf einem Testgerät, und suchen Sie nach der Geräte-ID im Portal sollte arbeiten, um Ihre Geräte-ID für alle Plattformen finden.
- Test-Geräte funktionieren mit IDFA im Vergleich zu IDFV (nur für iOS).


## <a name="push-customization"></a>Anpassung von Pushbenachrichtigungen

### <a name="issue"></a>Problem
- Erweiterte Pushbenachrichtigungen Inhalt funktioniert nicht Element (badge, anrufen, Vibrationsalarm, Bild, usw.).
- Links vom schiebt funktionieren nicht (nicht in der app, um eine Website zu einer Stelle in der app app).
- Pushbenachrichtigungen Statistiken zeigen an, dass eine Pushbenachrichtigungen an so viele Menschen wie erwartet (zu viele oder zu wenig) nicht gesendet wurde.
- Pushbenachrichtigungen dupliziert und zweimal empfangen.
- Testgerät kann nicht für Azure Mobile Engagement legt (mit Ihrer eigenen Prod oder Entwickler app) registriert werden.

### <a name="causes"></a>Bewirkt, dass

- Link zu einer bestimmten Stelle in der app ist erforderlich "Kategorien" (nur Android) aus.
- Tiefe verknüpfende Phishingversuchen Benutzer an einem anderen Speicherort umleiten, nach dem Klicken auf eine der Pushbenachrichtigung in erstellt und direkt von der Anwendung und das Gerät OS nicht durch Mobile Engagement verwaltet werden müssen. (**Hinweis:** von app Benachrichtigungen können nicht verknüpft werden direkt zu an app für iOS wie möglich mit Android.)
- Externe Bild Servern müssen wäre HTTP "GET" verwenden und "" für einen Überblick legt entwickelt (nur Android).
- Im Code können Sie den Azure Mobile Engagement Agent deaktivieren, wenn die Tastatur geöffnet wird, und den Code den Azure Mobile Engagement Agent erneut aktivieren, sobald die Tastatur geschlossen ist, sodass die Tastatur Einfluss auf das Erscheinungsbild Ihrer Benachrichtigung (nur iOS) wird nicht.
- Einige Elemente nicht funktionieren in Test Simulationen, jedoch nur tatsächliche massensendungen zu ermitteln (badge, anrufen, Vibrationsalarm, Bild, usw.).
- Wenn Sie die Schaltfläche "Testen schiebt" verwenden, werden keine Server Seite Daten protokolliert. Daten werden nur für Pushbenachrichtigungen tatsächliche massensendungen zu ermitteln protokolliert.
- Problembehandlung bei mit, um das Problem zu isolieren: testen, zu reproduzieren, und eine real für eine Marketingkampagne, da sie jede etwas anders funktionieren.
- Die Zeitdauer, die Ihre "in der app" und "jederzeit" massensendungen zu ermitteln Ausführung geplant werden kann Übermittlung Zahlen Effekt, da ein für eine Marketingkampagne nur Benutzer, die "in"app werden während der Ausführung der für eine Marketingkampagne (und Benutzer, die ihre Einstellungen für Audiogeräte festlegen "von"-app-Benachrichtigungen erhalten haben) übermittelt werden.
- Die Unterschiede zwischen wie Android und iOS außerhalb des app-Benachrichtigungen verarbeitet macht es schwierig, Pushbenachrichtigungen Statistiken zwischen den Android und iOS Version Ihrer Anwendung direkt zu vergleichen. Android stellt weitere OS Ebene Benachrichtigung als iOS unterstützt. Android zeigt an, dass eine systemeigene Benachrichtigung erhalten, geklickt haben oder in der Mitte der Benachrichtigung gelöscht, aber iOS meldet diese Informationen nicht, es sei denn, die Benachrichtigung geklickt wird. 
- Der wichtigste Grund, den "abgelegte" Zahlen sind anders als die anderen als "übermittelt" Zahlen für massensendungen zu ermitteln erreichen besteht darin, dass Benachrichtigungen "in der app" und "von"-app anders berücksichtigt werden. "In"app-Benachrichtigungen werden durch Mobile Engagement behandelt, jedoch Benachrichtigungen "von"-app vom Benachrichtigung Center in das Betriebssystem Ihres Geräts behandelt werden.

## <a name="push-targeting"></a>Pushbenachrichtigungen verwendet

### <a name="issue"></a>Problem
- Integrierte verwendet, funktioniert nicht wie erwartet.
- App Info Kategorie verwendet, funktioniert nicht wie erwartet.
- Geo-Speicherort verwendet, funktioniert nicht wie erwartet.
- Sprachoptionen funktionieren nicht wie erwartet.

### <a name="causes"></a>Bewirkt, dass

- Stellen Sie sicher, dass Sie die app Info Kategorien über die Azure Mobile Engagement Benutzeroberfläche oder die API hochgeladen haben.
- Begrenzungsebene die Geschwindigkeit von Pushbenachrichtigungen oder Pushbenachrichtigungen Kontingent Ebene der Anwendung oder das Publikum Ebene der für eine Marketingkampagne einschränken kann verhindern, dass eine Person aus einer bestimmten Pushbenachrichtigungen empfangen, selbst wenn sie Ihre anderen Zielkriterien entsprechen. 
- Festlegen einer "Sprache" unterscheidet verwendet, basierend auf Land oder Gebietsschema, also auch anders als verwendet, basierend auf geografischen Position auf einem Telefon Speicherort oder GPS-Position.
- Die Nachricht in der "Standardsprache" wird an alle Kunden gesendet hat ihr Gerät auf eine alternative Sprache festgelegt, die Sie angeben.


## <a name="push-scheduling"></a>Planen von Pushbenachrichtigungen

### <a name="issue"></a>Problem
- Planen von Pushbenachrichtigungen funktioniert nicht wie erwartet (gesendet zu frühen oder verzögerte).

### <a name="causes"></a>Bewirkt, dass

- Zeitzonen können Probleme bei der Planung, besonders, wenn die Endbenutzer Zeitzone verwenden.
- Erweiterte Pushbenachrichtigungen Features können schiebt verzögern.
- Legt verwendet, basierend auf dem Telefon, die Einstellungen (anstelle von Tags der App-Informationen) verzögert werden können, da Azure Mobile Engagement Anfordern von Daten aus der Telefon-Echtzeit vor dem Senden einer Pushbenachrichtigungen auftritt.
- Massensendungen zu ermitteln, ohne ein Enddatum erstellt die Pushbenachrichtigungen lokal auf dem Gerät speichern und Anzeigen der Differenz das nächste Mal, wenn, das die app geöffnet wird, auch wenn die für eine Marketingkampagne manuell eingestellt ist.
- Mehrere für eine Marketingkampagne starten, zur gleichen Zeit dauern eine längere Zeit zum Scannen Ihrer Benutzerbasis (nur eine für eine Marketingkampagne nacheinander mit bis zu vier, auch Ziel nur für die aktive Benutzer beginnt, sodass die alte Benutzer keine zu überprüfenden testen).
- Wenn Sie die Option "Ignorieren Benutzergruppe werden Pushbenachrichtigungen für Benutzer über die-API gesendet" im Abschnitt "Campaign" eine Campaign Reichweite verwenden, die für eine Marketingkampagne wird nicht automatisch senden, müssen Sie sie manuell über die erreichen-API zu senden.
- Wenn Sie mithilfe einer benutzerdefinierten Kategorie in Reichweite innerhalb der app-Benachrichtigungen anzeigen, müssen Sie den richtigen Lebenszyklus einer Benachrichtigung folgen, da sonst die Benachrichtigung kann nicht deaktiviert werden Wenn der Benutzer schließen sie.

 
