<properties
    pageTitle="Überwachen, diagnose und Problembehandlung bei Speicher | Microsoft Azure"
    description="Verwenden Sie Funktionen wie Speicher Analytics, clientseitige Protokollierung und anderen Drittanbieter-Tools, um zu identifizieren, diagnose und Problembehandlung bei Azure-Speicher-bezogene Probleme."
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="jahogg"/>

# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Überwachen, diagnose und Problembehandlung bei Microsoft Azure-Speicher

[AZURE.INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>(Übersicht)

Diagnose und Behebung von Problemen in einer verteilten Anwendung in einen Cloud-Umgebung gehostet wird, können komplexer als in herkömmlichen Umgebungen sein. Applikationen können in einer Infrastruktur PaaS oder IaaS lokal auf einem mobilen Gerät oder in einer Kombination bereitgestellt werden. In der Regel Ihrer Anwendung Netzwerkdatenverkehr möglicherweise zu Öffentliche und private Netzwerken durchlaufen und Ihrer Anwendung möglicherweise mehrere Speicher-Technologien wie Microsoft Azure-Speichertabellen, Blobs, Warteschlangen verwenden oder Dateien sowie weitere Daten speichert beispielsweise als relationale und Datenbanken Dokument.

Zum Verwalten von solche Applikationen erfolgreich sollten vorausschauende zu überwachen und verstehen, wie zum Identifizieren und beheben alle Aspekte des diese und deren abhängigen Technologien. Als Benutzer Azure-Speicher Dienste sollten Sie kontinuierlich überwachen der Speicher-Dienste, die Ihre Anwendung für Änderungen unerwarteten Verhalten (z. B. langsamer als Deutsch Reaktionszeiten) verwendet, und verwenden Protokollierung ausführlichere Daten zu sammeln und ein Problem – ein tieferer Einblick zu analysieren. Die Diagnoseinformationen, die Sie erhalten von sowohl für die Überwachung und Protokollierung hilft Ihnen, die Ursache des Problems anhand Ihrer Anwendung aufgetreten. Dann können Sie das Problem beheben und ermitteln die entsprechenden Schritte aus, die Sie ergreifen können, um ihn zu beheben. Azure-Speicher ein Core Azure Service, und Formulare einen wichtigen Bestandteil der meisten Lösungen, die die Azure Infrastruktur Kunden bereitstellen. Azure-Speicher enthält Funktionen, um die Überwachung, Diagnose und Problembehandlung Speicher in der Cloud-basierte Clientanwendungen zu vereinfachen.

> [AZURE.NOTE] Speicherkonten mit einem Replikationstyp Zone redundante Speicher (ZRS) haben keine der Kennzahlen oder Protokollierung Videofunktionen zu diesem Zeitpunkt aktiviert. 

Eine praktische Anleitung mit End-to-End-Problembehandlung in Clientanwendungen Azure-Speicher finden Sie unter [End-to-End-Behandlung von Problemen mit Azure Speicher Kennzahlen und Protokollierung, AzCopy, und Nachricht Analyzer](storage-e2e-troubleshooting.md).

+ [Einführung]
    + [Wie dieses Handbuch strukturiert ist]
+ [Überwachen Ihrer Speicherdienst]
    + [Überwachung Dienststatus]
    + [Überwachen der Kapazität]
    + [Überwachen der Verfügbarkeit]
    + [Überwachen der Leistung]
+ [Diagnostizieren von Speicher Probleme]
    + [Reaktionsgruppendienst Integrität Probleme]
    + [Leistungsprobleme]
    + [Diagnostizieren von Fehlern]
    + [Speicher Emulator Probleme]
    + [Tools für Speicher-Protokollierung]
    + [Mithilfe der Tools für Netzwerk-Protokollierung]
+ [End-to-End-tracing]
    + [Abgleichen der Log-Daten]
    + [Client-Anforderung-ID]
    + [Server-Anfrage-ID]
    + [Zeitstempel]
+ [Hinweise zur Problembehandlung]
    + [Kennzahlen anzeigen hohe AverageE2ELatency und niedrig AverageServerLatency]
    + [Kennzahlen anzeigen niedrig AverageE2ELatency und niedrig AverageServerLatency, aber der Client treten Wartezeit]
    + [Kennzahlen anzeigen hoher AverageServerLatency]
    + [Bei der Nachrichtenübermittlung auf eine Warteschlange unerwartete Verzögerungen auftreten]
    + [Kennzahlen anzeigen eine Steigerung in PercentThrottlingError]
    + [Kennzahlen anzeigen eine Steigerung in PercentTimeoutError]
    + [Kennzahlen anzeigen eine Steigerung in PercentNetworkError]
    + [Der Client ist HTTP 403 (Verboten) Nachrichten empfangen.]
    + [Der Client ist HTTP 404 (nicht gefunden) Nachrichten empfangen.]
    + [Der Client ist HTTP 409 (Konflikt) Nachrichten empfangen.]
    + [Kennzahlen anzeigen niedrig PercentSuccess oder Analytics Protokolleinträge haben Vorgänge mit Transaktionsstatus der ClientOtherErrors]
    + [Kennzahlen Kapazität Anzeigen einer unerwartete erhöhen in Kapazität speichernutzung]
    + [Unerwartete nach einem Neustart des virtuellen Computern auftreten, die eine große Anzahl von angefügten virtuellen Festplatten aufweisen]
    + [Das Problem tritt auf, verwenden Sie für die Entwicklung oder Test Speicheremulator]
    + [Sie werden bei der Installation der Azure SDK für .NET Probleme auftreten.]
    + [Sie haben ein anderes Problem mit einer Speicherdienst]
+ [Anlagen]
    + [Anlage 1: Verwenden von Fiddler, HTTP und HTTPS-Verkehr zu erfassen]
    + [Anlage 2: Verwenden von Wireshark zum Netzwerkverkehr erfassen]
    + [Anlage 3: Mithilfe von Microsoft Nachricht Analyzer Netzwerkverkehr erfassen]
    + [Anlage 4: Verwenden von Excel Kennzahlen anzeigen und dann wieder anmelden Daten]
    + [Anlage 5: Überwachung mit Anwendung Einsichten für Visual Studio Team Services]

## <a name="a-nameintroductionaintroduction"></a><a name="introduction"></a>Einführung

Dieses Handbuch Sie mithilfe von Features wie Azure-Speicher Analytics, wird gezeigt, wie zusammenhängende clientseitige Protokollierung in der Bibliothek Azure-Speicher Client und andere Tools von Drittanbietern zu identifizieren, diagnose und Problembehandlung bei Azure-Speicher Probleme.

![][1]

*Abbildung 1 Überwachung, Diagnose und Problembehandlung*

Dieses Handbuch richtet sich an hauptsächlich von Entwicklern Onlinedienste gelesen werden, die Azure Storage Services und IT-Experten verantwortlich für die Verwaltung von online-Dienste verwenden. Die von diesem Leitfaden Ziele sind:

- Sie Verwalten der Gesundheit und Leistung von Ihren Konten Azure-Speicher unterstützen.
- Zu bieten Ihnen die notwendigen Prozesse und Tools, mit deren Hilfe Sie entscheiden, ob eines Problems in einer Anwendung auf Azure-Speicher bezieht.
- Damit Sie einige Hinweise zur Behebung von Problemen im Zusammenhang mit Azure-Speicher können.

### <a name="a-namehow-this-guide-is-organizedahow-this-guide-is-organized"></a><a name="how-this-guide-is-organized"></a>Wie dieses Handbuch strukturiert ist

Abschnitt "[Überwachung der Speicherdienst]" beschrieben, wie zum Überwachen der Gesundheit und Leistung Ihrer Azure-Speicher Dienste Azure-Speicher Analytics Kennzahlen (Speicher Kennzahlen) verwenden.

Im Abschnitt "[diagnostizieren Speicher Probleme]" beschrieben, wie Probleme mit Azure-Speicher Analytics Logging (Protokollierung Speicher) diagnostizieren. Es wird beschrieben, wie clientseitige Protokollierung mit Hilfe der Funktionen in einem der Clientbibliotheken wie der Speicher-Client-Bibliothek für .NET oder Java SDK Azure aktivieren wird.

Im Abschnitt "[Ende zum Tracing]" wird beschrieben, wie Sie die Informationen in verschiedenen Protokolldateien und Kennzahlen Daten zuordnen können.

Im Abschnitt "[Problembehandlung Anleitungen]" finden Sie Hinweise zur Problembehandlung für einige häufige Speicher-bezogene Probleme, die auftreten können.

Die "[Anlagen]" enthalten Informationen zur Verwendung von anderen Tools wie Wireshark und Netmon zum Analysieren von Daten im Netzwerk Paket, Fiddler zum Analysieren von HTTP-/HTTPS-Nachrichten und Microsoft Nachricht Analyzer für das Protokolldaten abgleichen.


## <a name="a-namemonitoring-your-storage-serviceamonitoring-your-storage-service"></a><a name="monitoring-your-storage-service"></a>Überwachen Ihrer Speicherdienst

Wenn Sie mit Windows-Systemleistung vertraut sind, können Sie Speicherplatz Kennzahlen als eine Entsprechung Azure-Speicher von Windows Performance Monitor Indikatoren vorstellen. Im Speicher Kennzahlen finden Sie eine umfassende Reihe von Kennzahlen (Indikatoren in Windows Performance Monitor Terminologie) wie die Verfügbarkeit von Diensten, die Gesamtzahl der Anfragen Dienst oder Prozentsatz der erfolgreichen Anfragen in Service (eine vollständige Liste der der verfügbaren Kennzahlen, finden Sie unter <a href="http://msdn.microsoft.com/library/azure/hh343264.aspx" target="_blank">Speicher Analytics Kennzahlen Tabellenschema</a> auf MSDN). Sie können angeben, ob der Speicherdienst zu sammeln und aggregieren Kennzahlen stündlich oder jede Minute angezeigt werden soll. Weitere Informationen dazu, wie Sie Kennzahlen aktivieren und überwachen Sie Ihre Speicherkonten finden Sie unter <a href="http://go.microsoft.com/fwlink/?LinkId=510865" target="_blank">Aktivieren Speicher Kennzahlen</a> auf MSDN.

Sie können auswählen, auf denen stündlich metrische im klassischen Azure-Portal anzeigen und Konfigurieren von Regeln, die von Administratoren benachrichtigt werden möchten per e-Mail senden, wenn eine stündliche Metrik einen bestimmten Schwellenwert überschreitet (Weitere Informationen finden Sie auf die Seite <a href="http://msdn.microsoft.com/library/azure/dn306638.aspx" target="_blank">wie: benachrichtigen Benachrichtigungen erhalten und Verwalten von Warnungsregeln in Azure</a>). Der Speicherdienst sammelt Kennzahlen mit bemüht, aber möglicherweise nicht jeder Speichervorgang aufzeichnen.

Abbildung 2 unten zeigt die Seite Monitor im klassischen Azure-Portal, z. B. Verfügbarkeit, Anfragen insgesamt und durchschnittliche Wartezeit Zahlen für ein Speicherkonto Kennzahlen angezeigt werden. Eine Benachrichtigungsregel wurde auch für die einen Administrator benachrichtigen, wenn Verfügbarkeit unterhalb einer bestimmten Ebene fällt eingerichtet. Diese Daten nicht anzeigen, ist eine mögliche Bereich für die Untersuchung der Tabelle Service Erfolg Prozentsatz weniger als 100 % wird (Weitere Informationen finden Sie im Abschnitt "[Kennzahlen anzeigen niedrig PercentSuccess oder Analytics Protokolleinträge haben Vorgänge mit Transaktionsstatus ClientOtherErrors]").

![][2]

*Abbildung 2 Speicher Kennzahlen im klassischen Azure-Portal anzeigen*

Überwachen Sie ständig Ihre Azure Applications, um sicherzustellen, dass sie fehlerfrei und Ausführen von erwartungsgemäß sind:

- Definieren von einigen grundlegenden Faktoren für die Anwendung, die Ihnen ermöglichen, vergleichen Sie die aktuelle Daten und identifizieren Sie wesentlichen Änderungen in das Verhalten von Azure-Speicher und die Anwendung zu. Die Werte des Basisplans metrischen, in vielen Fällen werden bestimmte Anwendung, und Sie sollten sie einführen, wenn Sie die Leistung Testen der Anwendung sind.
- Aufzeichnung Minute Kennzahlen und verwenden diese zum Überwachen der aktiv unerwarteter Fehler und Bildschirmdarstellung auftreten, wie z. B. Spitzen Fehler zählt oder Sätzen anfordern.
- Aufzeichnung stündliche Kennzahlen und verwenden sie zum Überwachen der Durchschnittswerte wie in Mittelwert der Fehler zählt und Anfordern von Sätzen.
- Untersuchung läuft potenzieller Probleme mit Diagnosetools aus, wie weiter unten im Abschnitt "[diagnostizieren Speicher Probleme]" behandelt

Die Diagramme in der Abbildung 3 unten veranschaulichen, wie aus, für die stündlich Kennzahlen eintritt Spitzen in Aktivität ausgeblendet werden kann. Die stündlich Metrik werden konstanter Geschwindigkeit von Besprechungsanfragen, während die Minute anzeigen Kennzahlen Fluktuationen angezeigt werden, die wirklich stattfinden.

![][3]

Der Rest der in diesem Abschnitt beschrieben, welche Metrik sollten Sie überwachen und warum.

### <a name="a-namemonitoring-service-healthamonitoring-service-health"></a><a name="monitoring-service-health"></a>Überwachung Dienststatus

Sie können das [Klassische Azure-Portal](https://manage.windowsazure.com) verwenden, um die Integrität des der Speicherdienst (und andere Dienste Azure) in den Azure Regionen auf der ganzen Welt anzuzeigen. So können Sie sofort sehen, wenn ein Problem nicht Testen der Speicherdienst in der Region auswirkt, die Sie für eine Anwendung verwenden.

Das klassische Azure-Portal können auch mit der Anfragen Benachrichtigungen bereitstellen, die die verschiedenen Azure Dienste beeinflussen.
Hinweis: Diese Informationen wurde zuvor zusammen mit zurückliegende Daten, auf dem Azure Service Dashboard bei <a href="http://status.azure.com" target="_blank">http://status.azure.com</a>zur Verfügung.

Während der klassischen Azure-Portal Gesundheitsinformationen aus innerhalb der Azure Rechenzentren (innen nach außen Überwachung) sammelt, sollten Sie auch, eine Position außerhalb in Ansatz zum synthetische Transaktionen zu erzeugen, die in regelmäßigen Abständen Ihrer Azure gehostete Webanwendung von mehreren Speicherorten zugreifen. <a href="http://www.keynote.com/solutions/monitoring/web-monitoring" target="_blank">Leitgedanke</a>, <a href="https://www.gomeznetworks.com/?g=1" target="_blank">Gomez</a>und Anwendung Einsichten für Visual Studio Team Services angebotenen Dienste sind Beispiele für dieser Ansatz außen. Weitere Informationen zu Anwendung Einsichten für Visual Studio Team Services, finden Sie im Anhang "[Anlage 5: Überwachung mit Anwendung Einsichten für Visual Studio Team Services]."

### <a name="a-namemonitoring-capacityamonitoring-capacity"></a><a name="monitoring-capacity"></a>Überwachen der Kapazität

Speicher Kennzahlen speichert nur Kapazität Kennzahlen für den Blob-Dienst, da Blobs normalerweise den größten Anteil gespeicherten Daten zu berücksichtigen (zum Zeitpunkt der schreiben, es ist nicht möglich, Speicher Kennzahlen zu verwenden, um die Kapazität Ihrer Tabellen und Warteschlangen überwachen). Sie können diese Daten in der Tabelle **$MetricsCapacityBlob** suchen, wenn Sie für den Blob-Dienst für die Überwachung aktiviert haben. Speicher Kennzahlen Einträge diese Daten einmal pro Tag, und Sie können den Wert von der **RowKey** feststellen, ob die Zeile eine Entität enthält, auf den Benutzerdaten (Wert **Daten**) oder Analytics-Daten (Wert **Analytics**) bezieht. Jede gespeicherte Entität enthält Informationen zu den Speicherplatz verwendet (**Kapazität** gemessen in Byte) und die aktuelle Anzahl von Containern (**ContainerCount**) und Blobs (**ObjectCount**) im Speicherkonto verwendet. Weitere Informationen zu der Metrik Kapazität, die in der Tabelle **$MetricsCapacityBlob** gespeichert finden Sie unter <a href="http://msdn.microsoft.com/library/azure/hh343264.aspx" target="_blank">Speicher Analytics Kennzahlen Tabellenschema</a> auf MSDN.

> [AZURE.NOTE] Sie sollten diese Werte für eine frühe Warnung, dass Sie die Grenzwerte Kapazität Ihres Kontos Speicherplatz nähern überwachen. Im klassischen Azure-Portal können Sie auf der Seite **Monitor** für Ihr Speicherkonto Sie hinzufügen Warnungsregeln, um Sie zu benachrichtigen, wenn aggregieren Speicher verwenden überschreitet oder unter Grenzwerte an, denen von Ihnen angegebenen fällt.

Schätzen der Größe von Speicherobjekten, wie Blobs Hilfe finden Sie im Blogbeitrag <a href="http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx" target="_blank">Grundlegendes zu Azure Speicher Abrechnung – Bandbreite, Transaktionen, und Kapazität</a>.

### <a name="a-namemonitoring-availabilityamonitoring-availability"></a><a name="monitoring-availability"></a>Überwachen der Verfügbarkeit

Sie sollten die Verfügbarkeit der Speicherdienste in Ihr Speicherkonto überwachen, indem Sie den Wert in der Spalte **Verfügbarkeit** der Tabellen pro Stunde oder Minute Kennzahlen für die Überwachung – **$MetricsHourPrimaryTransactionsBlob**, **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$MetricsCapacityBlob**. Die **Verfügbarkeit von** Spalte enthält einen Prozentwert, der die Verfügbarkeit von dem Dienst oder der von der Zeile (im **RowKey** gezeigt, wenn die Zeile Kriterien für den Dienst als Ganzes oder für einen bestimmten API-Vorgang enthält) dargestellte API Vorgang angibt.

Jeder Wert kleiner als 100 % zeigt an, dass einige Speicheranfragen Fehlschlagen sind. Sie können sehen, warum diese fehlschlagen indem Sie die anderen Spalten in die Kennzahlen-Daten, die die Zahlen der Anfragen mit verschiedenen Fehlertypen wie **ServerTimeoutError**anzeigen. Sie sollten nicht vorhaben, finden Sie unter **Verfügbarkeit** vorübergehend Gründen wie vorübergehende Servertimeout weniger als 100 % liegen, während der Dienst Partitionen auf eine bessere Lastenausgleich Anforderung verschiebt; die Logik "Wiederholen" in der Clientanwendung sollten solche wiederkehrender Bedingungen behandelt werden. Die Seite <a href="http://msdn.microsoft.com/library/azure/hh343260.aspx" target="_blank"></a> Listet die Transaktion, die Speicher Kennzahlen in deren **Verfügbarkeit** Berechnung enthält.

Im klassischen Azure-Portal können Sie auf der Seite **Monitor** für Ihr Speicherkonto Sie hinzufügen Warnungsregeln, um Sie zu benachrichtigen, wenn die **Verfügbarkeit** für einen Dienst unter den Schwellenwert fällt, den Sie angeben.

Im Abschnitt "[Problembehandlung Anleitungen]" in diesem Handbuch werden einige allgemeine Speicher Dienstprobleme bei der Verfügbarkeit.

### <a name="a-namemonitoring-performanceamonitoring-performance"></a><a name="monitoring-performance"></a>Überwachen der Leistung

Um die Leistung der Speicherdienste zu überwachen, können Sie die folgende Metrik aus den Tabellen pro Stunde und Minute Kennzahlen verwenden.

- Die Werte in die **AverageE2ELatency** und **AverageServerLatency** zeigen der durchschnittlichen Dauer der Speicherdienst oder API Vorgangstyp sehr um Besprechungsanfragen. **AverageE2ELatency** ist ein Maß für End-to-End-Wartezeit, die die Verarbeitungszeit für die Anforderung lesen und senden die Antwort sowie die Verarbeitungszeit für die Anforderung umfasst (daher umfasst Netzwerkwartezeit, nachdem die Anforderung der Speicherdienst erreicht). **AverageServerLatency** ist ein Maß nur die Verarbeitungszeit und daher schließt alle Netzwerkwartezeit im Zusammenhang mit der Kommunikation mit dem Client. Finden Sie unter im Abschnitt "[Kennzahlen anzeigen hohe AverageE2ELatency und niedrig AverageServerLatency]" weiter unten in diesem Leitfaden für eine Erläuterung, warum es möglicherweise ein erheblichen Unterschied zwischen diesen zwei Werten.
- Die Werte in den Spalten **TotalIngress** und **TotalEgress** zeigen die Gesamtmenge der Daten in Byte, um eingehenden und ausgehenden aus Ihrer Speicherdienst oder über einen bestimmten Typ von API Vorgang an.
- Die Werte in der Spalte **TotalRequests** zeigen die Gesamtzahl der Besprechungsanfragen, die der Speicherdienst API Operation empfängt. **TotalRequests** ist die Gesamtzahl der Besprechungsanfragen, die der Speicherdienst empfängt.

In der Regel überwachen für unerwartete Änderungen in einem der folgenden Werte als Indikator Sie, dass Sie ein Problem haben, das untersucht werden muss.

Im klassischen Azure-Portal können Sie auf der Seite **Monitor** für Ihr Speicherkonto Sie hinzufügen Warnungsregeln, um Sie zu benachrichtigen, wenn eines der Performance-Werte für diesen Dienst unter liegen oder überschritten, die Sie angeben.

Im Abschnitt "[Problembehandlung Anleitungen]" in diesem Handbuch werden einige allgemeine Speicher Dienstprobleme bei der Leistung.


## <a name="a-namediagnosing-storage-issuesadiagnosing-storage-issues"></a><a name="diagnosing-storage-issues"></a>Diagnostizieren von Speicher Probleme

Es gibt eine Reihe von Methoden, dass Sie bei der Lösung eines Problems oder beachten Ihrer Anwendung sind möglicherweise, hierzu gehören:

- Eine Haupt-Fehler, der bewirkt, dass die Anwendung abstürzt oder nicht mehr funktionieren.
- Signifikante Änderungen in der Metrik, die Werte des Basisplans sind Sie für die Überwachung wie im vorherigen Abschnitt "[für die Überwachung der Speicherdienst]." beschrieben.
- Berichte aus der Benutzer der Anwendung, die ein bestimmter Vorgang abgeschlossen haben, wie erwartet, oder, dass einige Features nicht funktioniert.
- Innerhalb der Anwendung generierte Fehler angezeigt, die in die Protokolldateien oder über eine andere Benachrichtigungsmethode.

In der Regel liegen Probleme bei der Azure-Speicherdienste in eine der vier weitgefasste Kategorien:

- Die Anwendung verfügt über ein Leistungsproblem, entweder von Ihren Benutzern gemeldet oder offen gelegt, indem Sie sich die Performance-Werte.
- Es liegt ein Problem mit der Infrastruktur Azure-Speicher in einem oder mehreren Bereichen ein.
- Eine Anwendung ist einen Fehler, entweder von Ihren Benutzern gemeldet oder zu einer Steigerung in einem der Metrik zur Zählung zurück zu überwachenden erkannte auftritt.
- Bei der Entwicklung und Testen verwenden Sie möglicherweise lokale Speicheremulator; Sie können einige Probleme auftreten, die speziell für die Verwendung der Speicheremulator beziehen.

Den folgenden Abschnitten werden die Schritte Sie befolgen zum Identifizieren und Beheben von Problemen in jedem der folgenden vier Kategorien. Im Abschnitt "[Problembehandlung Anleitungen]" weiter unten in diesem Handbuch finden Sie weitere Details zu einige häufige Probleme auftreten können.

### <a name="a-nameservice-health-issuesaservice-health-issues"></a><a name="service-health-issues"></a>Reaktionsgruppendienst Integrität Probleme

Gesundheit Dienstprobleme sind in der Regel außerhalb des Steuerelements. Das klassische Azure-Portal enthält Informationen zur laufenden Probleme mit Azure-Dienste einschließlich Speicherdienste. Wenn Sie für Lesezugriff Geo redundante Speicher entschieden, wenn Sie Ihr Speicherkonto erstellt haben, konnte klicken Sie dann im Falle von Daten, die derzeit in der gewohnten Standort befinden nicht verfügbar ist, Ihrer Anwendung vorübergehend zu der schreibgeschützte Kopie in den zweiten Standort wechseln. Hierzu muss Ihrer Anwendung wechseln zwischen der Verwendung der primären und sekundären Speicherpositionen, und kann in einem Modus mit eingeschränkter Funktionalität mit schreibgeschützten Daten arbeiten. Die Azure-Speicher-Client-Bibliotheken können Sie eine Richtlinie "Wiederholen" definieren, die vom sekundären Speicher lesen können, für den Fall, dass eine gelesene vom primären Speicher schlägt fehl. Die Anwendung muss auch bewusst sein, dass die Daten in der zweiten Standort später konsistent sind. Weitere Informationen finden Sie im Blogbeitrag <a href="http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/04/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx" target="_blank">Azure Redundanz Speicheroptionen und redundante Lesezugriff Geo-Speicher</a>.

### <a name="a-nameperformance-issuesaperformance-issues"></a><a name="performance-issues"></a>Leistungsprobleme

Die Leistung der Anwendung kann subjektive, insbesondere aus der Sicht des Benutzers sein. Daher ist es wichtig, dass der grundlegenden Faktoren zur Verfügung, damit Sie erkennen können, wobei es möglicherweise ein Leistungsproblem. Viele Faktoren können die Leistung von einer Azure-Speicherdienst aus der Perspektive der Clientanwendung beeinflusst. Diese Faktoren möglicherweise in der Speicherdienst, im Client oder in der Netzwerkinfrastruktur verwendet werden. Daher ist es wichtig, dass eine Strategie für den Ursprung der Leistungsproblem identifizieren.

Nachdem Sie den Speicherort wahrscheinlich die Ursache des Leistungsproblems aus der Metrik festgestellt haben, können Sie die Protokolldateien dann finden Sie ausführliche Informationen zum Identifizieren und beheben Sie das Problem weiter verwenden.

Möglicherweise der Abschnitt "[Problembehandlung Anleitungen]" weiter unten in diesem Handbuch bietet weitere Informationen zur einige allgemeine Leistung beziehen Sie Probleme auftreten.

### <a name="a-namediagnosing-errorsadiagnosing-errors"></a><a name="diagnosing-errors"></a>Diagnostizieren von Fehlern

Benutzer der Anwendung können der von der Clientanwendung gemeldeten Fehler benachrichtigt. Speicher Kennzahlen Einträge auch die Anzahl der verschiedenen Fehlertypen aus Ihrer Speicher-Diensten wie **NetworkError**, **ClientTimeoutError**oder **AuthorizationError**. Während der Speicher Kennzahlen nur die Anzahl der verschiedenen Fehlertypen Einträge, erhalten Sie weitere Details zu einzelnen Besprechungsanfragen, indem Netzwerk Protokolle, clientseitige und serverseitige untersuchen. In der Regel, erhalten der HTTP-Statuscode der Speicherdienst zurückgegebene Angabe der warum Fehler bei der Anforderung.

> [AZURE.NOTE] Beachten Sie, dass einige wiederkehrender Fehler finden Sie unter rechnen: z. B. Fehlern durch vorübergehende Netzwerkprobleme oder Anwendungsfehler.

Die folgenden Ressourcen auf MSDN sind für Grundlegendes zu Speicher-bezogene Status und Fehlercodes hilfreich:

- <a href="http://msdn.microsoft.com/library/azure/dd179357.aspx" target="_blank">Allgemeine REST-API Fehlercodes</a>
- <a href="http://msdn.microsoft.com/library/azure/dd179439.aspx" target="_blank">Fehlercodes für BLOB-Dienst</a>
- <a href="http://msdn.microsoft.com/library/azure/dd179446.aspx" target="_blank">Fehlercodes für Warteschlange-Dienst</a>
- <a href="http://msdn.microsoft.com/library/azure/dd179438.aspx" target="_blank">Tabelle Dienst Fehlercodes</a>

### <a name="a-namestorage-emulator-issuesastorage-emulator-issues"></a><a name="storage-emulator-issues"></a>Speicher Emulator Probleme

Das Azure SDK enthält eine Speicheremulator, die Sie auf einer Entwicklungsarbeitsstation ausgeführt werden können. Dieser Emulator simuliert die meisten das Verhalten der Azure-Speicherdienste und eignet sich bei der Entwicklung und testen, sodass Sie Programme ausführen, die Dienste Azure-Speicher, ohne dass ein Azure-Abonnement und ein Konto Azure-Speicher verwenden.

Im Abschnitt "[Problembehandlung Anleitungen]" in diesem Handbuch werden einige häufig auftretende Probleme mit der Speicheremulator.

### <a name="a-namestorage-logging-toolsastorage-logging-tools"></a><a name="storage-logging-tools"></a>Tools für Speicher-Protokollierung

Speicher Protokollierung bietet serverseitige Protokollierung von Speicher-Anforderungen bei Ihrem Konto Azure-Speicher. Weitere Informationen zum Aktivieren der serverseitige Protokollierung und Access die Daten Log finden Sie unter <a href="http://go.microsoft.com/fwlink/?LinkId=510867" target="_blank">verwenden serverseitige Protokollierung</a> auf MSDN.

Der Speicher-Client-Bibliothek für .NET können Sie clientseitige Log Daten zu sammeln, zu die Storage Operationen Ihrer Anwendung gehört. Weitere Informationen zum Aktivieren clientseitige Protokollierung und Access die Daten Log finden Sie unter <a href="http://go.microsoft.com/fwlink/?LinkId=510868" target="_blank">clientseitige Protokollierung mithilfe der Speicher-Client-Bibliothek</a> auf MSDN.

> [AZURE.NOTE] In einigen Fällen (z. B. SAS Autorisierungsfehler) möglicherweise ein Benutzer einen Fehlerbericht für die Sie keine Anforderungsdaten in die Protokolle der serverseitigen Speicher finden können. Sie können die Protokollierungsfunktionen der Bibliothek Client Speicher verwenden, um festzustellen, ob die Ursache des Problems auf dem Client ist oder mithilfe von Netzwerk-Überwachungstools im Netzwerk untersuchen.

### <a name="a-nameusing-network-logging-toolsausing-network-logging-tools"></a><a name="using-network-logging-tools"></a>Mithilfe der Tools für Netzwerk-Protokollierung

Sie können den Datenverkehr zwischen Client und Server eine ausführliche Informationen zu den Daten, die Austausch von Client und Server sind und die zugrunde liegenden netzwerkbedingungen erfassen. Hilfreiche Netzwerk Protokollierungstools gehören:

- Fiddler (<a href="http://www.telerik.com/fiddler" target="_blank">http://www.telerik.com/fiddler</a>) ist ein kostenlos zur Verfügung für das Debuggen-Proxy, um die Kopf- und Nutzlastdaten HTTP- und HTTPS-Anforderung und Antwort Nachrichten untersuchen Sie aktiviert. Weitere Informationen finden Sie unter "[Anlage 1: Verwenden von Fiddler, HTTP und HTTPS-Verkehr zu erfassen]".
- Microsoft Network Monitor (Netmon) (<a href="http://www.microsoft.com/download/details.aspx?id=4865" target="_blank">http://www.microsoft.com/download/details.aspx?id=4865</a>) und Wireshark (<a href="http://www.wireshark.org/" target="_blank">http://www.wireshark.org/</a>) sind kostenlos Netzwerk Protokollanalyseprogrammen, mit die Sie detaillierte Paketinformationen für eine Vielzahl von Protokollen der anzeigen können. Weitere Informationen zu Wireshark, finden Sie unter "[Anlage 2: mithilfe von Wireshark zum Erfassen von Netzwerkverkehr]".
- Microsoft Nachricht Analyzer ist ein Tool von Microsoft, die hat Vorrang vor Netmon und die zusätzlich zum Erfassen von Daten im Netzwerk Paket, hilft Ihnen zum Anzeigen und Analysieren der Log-Daten aus anderen Tools erfasst. Weitere Informationen finden Sie unter "[Anlage 3: Verwenden von Microsoft Nachricht Analyse Netzwerkverkehr erfassen]".
- Wenn Sie eine einfache Verbindungstest um zu überprüfen, dass Ihre Clientcomputer mit der Azure-Speicherdienst, über das Netzwerk herstellen kann ausführen möchten, dies nicht mit dem standardmäßigen **Ping** -Tool auf dem Client. Das Tool **Tcping** können Sie jedoch um Konnektivität zu prüfen. **Tcping** ist unter <a href="http://www.elifulkerson.com/projects/tcping.php" target="_blank">http://www.elifulkerson.com/projects/tcping.php</a>zum Download verfügbar.

In vielen Fällen werden die Daten Log Protokollierung Speicher und die Speicher-Client-Bibliothek aus, um ein Problem diagnostizieren, aber in einigen Szenarien benötigen Sie möglicherweise detailliertere Informationen, die diese Netzwerk Protokollierungstools bereitstellen können. Beispielsweise ermöglicht das Fiddler zum Anzeigen von HTTP- und HTTPS-Nachrichten verwenden Sie Kopf- und Nutzlast und von den Speicher-Diensten, die können Sie prüfen, wie eine Clientanwendung Speichervorgänge wiederholt würde gesendete Daten anzeigen. Die Steuerung Protokollanalyseprogrammen wie Wireshark Ebene Paket aktivieren Sie TCP Daten sehen, die Sie verlorene Pakete und Verbindungsprobleme beheben aktivieren möchten. Nachricht Analyzer kann auf HTTP und TCP Ebenen ausgeführt werden.

## <a name="a-nameend-to-end-tracingaend-to-end-tracing"></a><a name="end-to-end-tracing"></a>End-to-End-tracing

End-to-End-Tracing mithilfe einer Vielzahl von Protokolldateien ist eine nützliche Technik zum Untersuchen potenzielle Probleme. Können die Informationen aus den Daten Kennzahlen als Hinweis, wo beginnen Sie Ihre Suche in den Protokolldateien für die detaillierten Informationen, die Sie zur Problembehandlung helfen.

### <a name="a-namecorrelating-log-dataacorrelating-log-data"></a><a name="correlating-log-data"></a>Abgleichen der Log-Daten

Bei der Anzeige von Protokollen in Clientanwendungen Netzwerk verfolgt und Speicher serverseitige Protokollierung ist es entscheidend, zu koordinieren können über die verschiedenen Protokolldateien anfordert. Die Protokolldateien enthalten eine Reihe von anderen Feldern, die als Korrelationskoeffizienten Bezeichner hilfreich sind. Die Client-Id wird das besonders hilfreich Feld zu verwenden, um die Einträge in den verschiedenen Protokollen zu koordinieren. Jedoch in manchen Fällen kann es nützlich sein, verwenden entweder die Id des Servers Anforderung oder Zeitstempel sein. Die folgenden Abschnitte enthalten weitere Details zu diesen Optionen.

### <a name="a-nameclient-request-idaclient-request-id"></a><a name="client-request-id"></a>Client-Anforderung-ID

Der Speicher-Client-Bibliothek generiert automatisch eine eindeutige Client-Anforderung-Id für jede Anforderung aus.

- In der clientseitige Log, die die Speicher-Client-Bibliothek erstellt wird, angezeigt wird die Client-Id in das **Client-Anforderung ID-** Feld jeder Log-Eintrag für die Anfrage ein.
- In einem Netzwerkspur wie eine von Fiddler erfasst steht die Client-Id in der Anforderungsnachrichten als den Wert von **X-ms-Client-Anfrage-Id** HTTP Header.
- In der serverseitige Protokollierung Speicher Log angezeigt wird die Client-Id in der Spalte Client Anforderung-ID ein.

> [AZURE.NOTE]Es ist möglich, dass mehrere Anforderungen an die gleiche Client-Id freigeben, da der Client diesen Wert zuweisen kann (auch wenn der Speicher-Client-Bibliothek automatisch einen neuen Wert weist). Wenn Wiederholungsversuche im Desktopclient freigeben alle Versuche die gleichen Client-Id an. Bei einem Stapel vom Client gesendet wird muss die Stapelverarbeitung Anforderung einzelnen Client-Id an.


### <a name="a-nameserver-request-idaserver-request-id"></a><a name="server-request-id"></a>Server-Anfrage-ID

Der Speicherdienst generiert automatisch Server Anforderung Ids.

- In der serverseitige Protokollierung Speicher Log wird die Server-Id der Spalte **ID anfordern Kopfzeile** angezeigt.
- In einem Netzwerkspur wie eine von Fiddler erfasst wird die Server-Id Antwortnachrichten als den Wert von **X-ms-Anforderung-Id** HTTP Header.
- Angezeigt wird in der clientseitige Log, die die Speicher-Client-Bibliothek erstellt wird, die Server-Id in der Spalte **Vorgang Text** für den Vertrieb mit Details der Antwort des Servers.

> [AZURE.NOTE] Der Speicherdienst weist immer einen eindeutigen Server Anforderung Id zu jeder Anforderung erhaltenen, damit jeder Versuch "Wiederholen" aus dem Client und jedem Vorgang in einem Stapel enthalten eine eindeutige Server-Id anfordern hat.

Wenn der Speicher-Client-Bibliothek im Client eine **StorageException** ausgelöst wird, enthält die Eigenschaft **RequestInformation** ein **RequestResult** -Objekt, das eine Eigenschaft **ServiceRequestID** enthält. Sie können auch ein Objekt **RequestResult** aus einer Instanz **OperationContext** zugreifen.

Im folgenden Beispiel wird veranschaulicht, wie Sie einen benutzerdefinierten **ClientRequestId** Wert festlegen, indem Sie ein Objekt **OperationContext** die Anfrage an der Speicherdienst anfügen. Es wird gezeigt, wie den **ServerRequestId** Wert aus der Antwortnachricht abzurufen.

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Create an Operation Context that includes custom ClientRequestId string based on constants defined within the application along with a Guid.
    OperationContext oc = new OperationContext();
    oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

    try
    {
        CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
        ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
        var downloadToPath = string.Format("./{0}", blob.Name);
        using (var fs = File.OpenWrite(downloadToPath))
        {
            blob.DownloadToStream(fs, null, null, oc);
            Console.WriteLine("\t Blob downloaded to file: {0}", downloadToPath);
        }
    }
    catch (StorageException storageException)
    {
        Console.WriteLine("Storage exception {0} occurred", storageException.Message);
        // Multiple results may exist due to client side retry logic - each retried operation will have a unique ServiceRequestId
        foreach (var result in oc.RequestResults)
        {
                Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
        }
    }


### <a name="a-nametimestampsatimestamps"></a><a name="timestamps"></a>Zeitstempel

Sie können Sie auch Zeitstempel verwandte Protokolleinträge suchen, aber Achten Sie darauf, dass Sie alle Uhr Schiefe zwischen Client und Server, die möglicherweise vorhanden ist. Sie sollten plus oder minus 15 Minuten, damit übereinstimmende serverseitigen Einträge basierend auf den Zeitstempel auf dem Client suchen. Beachten Sie, dass die Blob-Metadaten für die Blobs mit Metrik zeigt an, den Zeitraum für die Metrik in dem Blob gespeichert. Dies ist sinnvoll, wenn Sie viele Kennzahlen Blobs für den gleichen Minute oder Stunde haben.

## <a name="a-nametroubleshooting-guidanceatroubleshooting-guidance"></a><a name="troubleshooting-guidance"></a>Hinweise zur Problembehandlung

In diesem Abschnitt hilft Ihnen bei der Diagnose und Problembehandlung bei Ihrer Anwendung einiger häufige Probleme auftreten, wenn der Azure-Speicherdienste verwenden. Verwendung der nachstehenden Liste, die mit einem bestimmten Problem relevanten Informationen gesucht werden soll.

**Problembehandlung bei Entscheidungsstruktur**

----------

Bezieht sich das Problem auf die Leistung von einer der Speicherdienste?

- [Kennzahlen anzeigen hohe AverageE2ELatency und niedrig AverageServerLatency]
- [Kennzahlen anzeigen niedrig AverageE2ELatency und niedrig AverageServerLatency, aber der Client treten Wartezeit]
- [Kennzahlen anzeigen hoher AverageServerLatency]
- [Bei der Nachrichtenübermittlung auf eine Warteschlange unerwartete Verzögerungen auftreten]

----------

Bezieht sich das Problem auf die Verfügbarkeit von einer der Speicherdienste?

- [Kennzahlen anzeigen eine Steigerung in PercentThrottlingError]
- [Kennzahlen anzeigen eine Steigerung in PercentTimeoutError]
- [Kennzahlen anzeigen eine Steigerung in PercentNetworkError]

----------

Ist die Clientanwendung eine HTTP-4XX (z. B. 404) Antwort von einem Speicherdienst empfangen?

- [Der Client ist HTTP 403 (Verboten) Nachrichten empfangen.]
- [Der Client ist HTTP 404 (nicht gefunden) Nachrichten empfangen.]
- [Der Client ist HTTP 409 (Konflikt) Nachrichten empfangen.]

----------

[Kennzahlen anzeigen niedrig PercentSuccess oder Analytics Protokolleinträge haben Vorgänge mit Transaktionsstatus der ClientOtherErrors]

----------

[Kennzahlen Kapazität Anzeigen einer unerwartete erhöhen in Kapazität speichernutzung]

----------

[Unerwartete nach einem Neustart des virtuellen Computern auftreten, die eine große Anzahl von angefügten virtuellen Festplatten aufweisen]

----------

[Das Problem tritt auf, verwenden Sie für die Entwicklung oder Test Speicheremulator]

----------

[Sie werden bei der Installation der Azure SDK für .NET Probleme auftreten.]

----------

[Sie haben ein anderes Problem mit einer Speicherdienst]

----------

### <a name="a-namemetrics-show-high-averagee2elatency-and-low-averageserverlatencyametrics-show-high-averagee2elatency-and-low-averageserverlatency"></a><a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Kennzahlen anzeigen hohe AverageE2ELatency und niedrig AverageServerLatency

Die Abbildung Schlag aus den klassischen Azure-Portal Überwachungstools zeigt ein Beispiel, in dem die **AverageE2ELatency** erheblich größer ist als der **AverageServerLatency**.

![][4]

Beachten Sie, dass der Speicherdienst nur die Metrik **AverageE2ELatency** für erfolgreiche Anfragen berechnet und, im Gegensatz zu **AverageServerLatency**, die Zeit enthält, die der Client benötigt, um die Daten senden und Empfangen von Bestätigung aus der Speicherdienst. Daher könnte ein Unterschied zwischen **AverageE2ELatency** und **AverageServerLatency** aufgrund der Clientanwendung zu reagieren langsam oder aufgrund von Bedingungen im Netzwerk.

> [AZURE.NOTE] Sie können auch **E2ELatency** und **ServerLatency** für einzelne Speicher Operationen in die Protokollierung Speicher Log Daten anzeigen.

#### <a name="investigating-client-performance-issues"></a>Untersuchung von Leistungsproblemen client

Mögliche Gründe für den Client langsam reagiert sind Probleme eine eingeschränkte Anzahl von verfügbaren Verbindungen oder Threads. Sie können zur Lösung des Problems durch Ändern des Codes Client benutzerspezifisch effizienter (z. B. mit asynchronen Anrufe an der Speicherdienst) oder mithilfe eines größeren virtuellen Computers (mit mehr Kerne und mehr Speicher) wenden.

Für die Tabelle und Warteschlange-Dienste der Nagle-Algorithmus kann auch verursacht hohe **AverageE2ELatency** im Vergleich zu **AverageServerLatency**: Weitere Informationen finden Sie im Microsoft Azure-Speicher-Teamblog Nachricht <a href="http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx" target="_blank">Nagles-Algorithmus in Richtung kleine Anfragen nicht geeignet ist</a> . Sie können den Nagle-Algorithmus Code mithilfe der Klasse **ServicePointManager** im **System.NET-Namespace** deaktivieren. Sie sollten dies tun, bevor Sie alle Anrufe an die Tabelle oder Warteschlangendienste in Ihrer Anwendung, da dadurch Verbindungen, die bereits vorhanden sind, nicht beeinträchtigt wird geöffnet. Das folgende Beispiel stammt aus der **Application_Start** -Methode in eine Worker-Rolle.

    var storageAccount = CloudStorageAccount.Parse(connStr);
    ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
    tableServicePoint.UseNagleAlgorithm = false;
    ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
    queueServicePoint.UseNagleAlgorithm = false;

Sollten Sie sehen, wie viele anfordert, die Clientanwendung übermitteln ist die clientseitige Protokolle, und aktivieren Sie für allgemeine .NET Zusammenhang Leistungsengpässe in Ihren Kunden wie CPU, .NET Garbagecollection, Netzwerkverkehr oder Arbeitsspeicher (als Ausgangspunkt zur Behandlung dieses Problems .NET-Clientanwendungen, finden Sie unter <a href="http://msdn.microsoft.com/library/7fe0dd2y(v=vs.110).aspx" target="_blank">Debuggen, Spur, und ein Profil erstellen</a> auf MSDN).

#### <a name="investigating-network-latency-issues"></a>Untersuchung läuft Wartezeit Netzwerkproblemen

Normalerweise ist durch das Netzwerk verursacht End-to-End-Wartezeit aufgrund vorübergehende Zustände aus. Sie können beide vorübergehende und beständigen Netzwerkproblemen z. B. abgelegten Pakete mithilfe von Tools wie Wireshark oder Microsoft Nachricht Analyzer untersuchen.

Weitere Informationen zur Verwendung von Wireshark Netzwerkproblemen, finden Sie unter "[Anlage 2: mithilfe von Wireshark zum Erfassen von Netzwerkverkehr]."

Weitere Informationen zur Verwendung von Microsoft Nachricht Analyzer Netzwerkproblemen, finden Sie unter "[Anlage 3: Erfassen von Netzwerkverkehr mithilfe von Microsoft Message Analyzer]."

### <a name="a-namemetrics-show-low-averagee2elatency-and-low-averageserverlatencyametrics-show-low-averagee2elatency-and-low-averageserverlatency-but-the-client-is-experiencing-high-latency"></a><a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Kennzahlen anzeigen niedrig AverageE2ELatency und niedrig AverageServerLatency, aber der Client treten Wartezeit

In diesem Szenario ist höchstwahrscheinlich eine Verzögerung in den Eingang der Speicherdienst Speicher Anforderungen an. Sie sollten untersuchen, warum der Client-Anfragen nicht, aber durch für den Blob-Dienst Magazins sind.

Mögliche Gründe für den Client, senden Besprechungsanfragen verzögern sind Probleme eine eingeschränkte Anzahl von verfügbaren Verbindungen oder Threads. Sie sollten auch überprüfen, ob der Client mehrere Wiederholungsversuche ausführt, und überprüfen Sie den Grund aus, wenn dies der Fall ist. Sie können diesem programmgesteuert tun, indem in das Objekt **OperationContext** zugeordnet die Anfrage und Abrufen des Werts **ServerRequestId** gefunden. Weitere Informationen finden Sie im Beispiel im Abschnitt "[Server Anforderung-ID]".

Wenn es keine Probleme im Client liegen, sollten Sie mögliche Netzwerkproblemen wie z. B. Paketverlust untersuchen. Sie können mithilfe Tools wie Wireshark oder Microsoft Nachricht Analyzer Netzwerkproblemen untersuchen.

Weitere Informationen zur Verwendung von Wireshark Netzwerkproblemen, finden Sie unter "[Anlage 2: mithilfe von Wireshark zum Erfassen von Netzwerkverkehr]."

Weitere Informationen zur Verwendung von Microsoft Nachricht Analyzer Netzwerkproblemen, finden Sie unter "[Anlage 3: Erfassen von Netzwerkverkehr mithilfe von Microsoft Message Analyzer]."

### <a name="a-namemetrics-show-high-averageserverlatencyametrics-show-high-averageserverlatency"></a><a name="metrics-show-high-AverageServerLatency"></a>Kennzahlen anzeigen hoher AverageServerLatency

Bei hoher **AverageServerLatency** Blob Download Anfragen sollten Sie die Protokolle Speicher Protokollierung verwenden, um festzustellen, ob es wiederholte Anforderungen für die gleiche Blob (oder Satz von Blobs bereits). Hochladen von Besprechungsanfragen für Blob, sollten Sie ermitteln, welche blockieren Größe der Client verwendet wird (beispielsweise blockiert kleiner als 64 KB Aufwand führen kann, es sei denn, die liest auch in kleiner als 64 KB Segmente haben), und wenn mehrere Clients in das gleiche Blob parallele Blöcke hochladen. Sollten auch die pro Minute Metrik für die Anzahl der Anfragen, die mehr als Folge Spitzen der pro zweiten Skalierbarkeit Ziele: Siehe auch "[Kennzahlen anzeigen eine Erhöhung PercentTimeoutError]."

Wenn die hohe **AverageServerLatency** für Blob Besprechungsanfragen herunterladen Vorliegens wiederholte Abfragen derselben Blob oder Satz von Blobs angezeigt werden, klicken Sie dann sollten Sie diese Blobs mit Azure Cache oder Azure Content Delivery Network (CDN) zwischenspeichern. Bei Anfragen hochladen können Sie den Durchsatz verbessern, mithilfe einer größeren blockieren. Für Abfragen mit Tabellen ist es auch möglich, implementieren clientseitige Zwischenspeichern auf Clients, die die gleiche Abfragevorgänge ausführen und die Stelle, an der die Daten häufig geändert wird nicht.

Höchstwerte **AverageServerLatency** können auch befinden, der fehlerhaft entworfene Tabellen oder Abfragen, dass das Ergebnis in Scan-Vorgänge oder mit dem Anfügen/vorangestellt Anti-Muster folgen. Weitere Informationen finden Sie unter "[Kennzahlen anzeigen eine Erhöhung PercentThrottlingError]".

> [AZURE.NOTE] Finden Sie hier eine umfassende Prüfliste Leistung Checkliste: [Microsoft Azure-Speicher Leistung und Skalierbarkeit Checkliste](storage-performance-checklist.md).

### <a name="a-nameyou-are-experiencing-unexpected-delays-in-message-deliveryayou-are-experiencing-unexpected-delays-in-message-delivery-on-a-queue"></a><a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>Bei der Nachrichtenübermittlung auf eine Warteschlange unerwartete Verzögerungen auftreten

Wenn Sie, also eine Verzögerung zwischen der Zeit, die eine Anwendung fügt eine Nachricht an eine Warteschlange und die Zeit, die sie auftrat in der Warteschlange Lesen verfügbar sind, sollten dann Sie die folgenden Schritte aus, um das Problem diagnostizieren ausführen:

- Stellen Sie sicher, dass die Anwendung erfolgreich die Nachrichten in der Warteschlange hinzufügt. Überprüfen Sie, dass die Anwendung nicht die **AddMessage** -Methode mehrmals vor dem erfolgreiche Wiederholung ist. Die Protokolle der Speicher-Client-Bibliothek werden eine wiederholte Wiederholungsversuche Speicher Vorgänge angezeigt.
- Stellen Sie sicher, es gibt keine Uhr Schiefe zwischen Worker-Rolle, die die Nachricht an die Warteschlange hinzufügt und Worker-Rolle, die die Nachricht aus der Warteschlange liest, mit dem angezeigt wird, als wäre entsteht eine Verzögerung in Verarbeitung.
- Überprüfen Sie, ob die Worker-Rolle, die die Nachrichten aus der Warteschlange liest fehlschlägt. Wenn ein Warteschlange-Client **GetMessage** -Methode ruft, jedoch nicht mit einer Bestätigung reagiert, bleibt die Nachricht in der Warteschlange unsichtbare bis **InvisibilityTimeout** Periode läuft ab. Die Nachricht wird zu diesem Zeitpunkt für die Verarbeitung wieder zur Verfügung.
- Überprüfen Sie, ob die Länge der Warteschlange über einen Zeitraum wächst. Dies kann auftreten, wenn Sie keinen ausreichende Kollegen verfügbar, um alle Nachrichten verarbeiten, die andere Kollegen in der Warteschlange platziert werden. Sie sollten auch überprüfen, dass die Metrik, um festzustellen, ob löschen anfordert fehlerhaften und die Anzahl der zum Abrufen aus Nachrichten, die darauf hinweisen, möglicherweise wiederholt werden Versuche zum Löschen der Nachricht fehlgeschlagen.
- Untersuchen Sie die Protokollierung Speicher-Protokolle für Warteschlange Vorgänge, die größer als die erwarteten **E2ELatency** und **ServerLatency** Werte über längere Zeit als gewöhnlich angezeigt.


### <a name="a-namemetrics-show-an-increase-in-percentthrottlingerrorametrics-show-an-increase-in-percentthrottlingerror"></a><a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Kennzahlen anzeigen eine Steigerung in PercentThrottlingError

Drosselung Fehler auftreten, wenn Sie die Skalierbarkeit Ziele von einer Speicherdienst überschreiten. Der Speicherdienst bedeutet dies, um sicherzustellen, dass keine einzelnen Client oder den Mandanten den Dienst auf andere Kosten verwenden kann. Weitere Informationen finden Sie unter <a href="http://msdn.microsoft.com/library/azure/dn249410.aspx" target="_blank">Azure Speicher Skalierbarkeit und Leistungsziele</a> Details Skalierbarkeit Ziele für Speicherkonten und Leistungsziele für Partitionen innerhalb von Speicherkonten.

Wenn die **PercentThrottlingError** Metrik eine Erhöhung der Prozentwert der Anfragen, der mit einem Fehler Drosselung sind zeigen, müssen Sie eine der folgenden beiden Szenarien zu ermitteln:

- [Vorübergehende Erhöhung der PercentThrottlingError]
- [Permanent erhöhen PercentThrottlingError Fehler]

Eine Erhöhung **PercentThrottlingError** häufig zur gleichen Zeit als eine Erhöhung der Anzahl von Speicheranfragen vorkommt, oder wenn Sie zunächst sind laden Testen der Anwendung. Dies möglicherweise selbst im Client als "503 Server Busy" oder "500 Vorgang Timeout" HTTP-Statusnachrichten von Speichervorgänge manifest.

#### <a name="a-nametransient-increase-in-percentthrottlingerroratransient-increase-in-percentthrottlingerror"></a><a name="transient-increase-in-PercentThrottlingError"></a>Vorübergehende Erhöhung der PercentThrottlingError

Wenn die Spitzen in den Wert der **PercentThrottlingError** , die mit Zeiten hoher Aktivität für die Anwendung übereinstimmen angezeigt werden, sollten Sie einer exponentiellen (nicht linear) Back deaktivieren Strategie für Wiederholungsversuche in Ihren Kunden implementieren: dies der Partition unmittelbar entlasten und Ihrer Anwendung um Spitzen im Datenverkehr Glätten helfen. Weitere Informationen dazu, wie Sie mithilfe der Speicher-Client-Bibliothek "Wiederholen"-Richtlinien implementieren finden Sie unter <a href="http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx" target="_blank">Microsoft.WindowsAzure.Storage.RetryPolicies Namespace</a> auf MSDN.

> [AZURE.NOTE] Spitzen im Wert von **PercentThrottlingError** , die nicht mit Zeiten hoher Aktivität für die Anwendung übereinstimmen kann auch angezeigt werden: mögliche Ursache ist der Speicherdienst Partitionen zur Verbesserung der Lastenausgleich verschieben.

#### <a name="a-namepermanent-increase-in-percentthrottlingerrorapermanent-increase-in-percentthrottlingerror-error"></a><a name="permanent-increase-in-PercentThrottlingError"></a>Permanent erhöhen PercentThrottlingError Fehler

Wenn Sie einen konsistente hohen Wert für **PercentThrottlingError** Folgendes angezeigt werden eine permanent erhöhen in Ihrer Transaktion Datenmengen, oder wenn Sie Ihre ursprüngliche Laden Ausführen auf Ihrer Anwendung überprüft und dann müssen Sie ausgewertet werden soll, wie die Anwendung Speicher Partitionen verwendet wird, und gibt an, ob die Ziele Skalierbarkeit für ein Konto Speicherplatz bald erreicht wird. Angenommen, wenn begrenzungsebene Fehler auf eine Warteschlange (die als einzelne Partition ermittelt) angezeigt werden, sollten dann Sie zusätzliche Warteschlangen mithilfe die Transaktionen auf mehrere Partitionen verteilt. Wenn die begrenzungsebene Fehler in einer Tabelle angezeigt werden, müssen Sie ein anderes Partitionierungsschema, um Ihre Transaktionen mehrere partitionsübergreifend bekanntzumachen, mithilfe eines breiteren Bereichs der Schlüsselwerte Partition bietet. Eine häufige Ursache für dieses Problem ist das Prepend/anfügen Anti-Muster, wo Sie das Datum als das Partitionsschlüssel auswählen und dann alle Daten an einem bestimmten Tag einer Partition geschrieben: Auslastung, dies kann dazu führen einen Engpass schreiben. Sie sollten verwenden Sie einen anderen vorherigen Entwurf oder ausgewertet werden soll, ob Blob-Speicher verwenden eine bessere Lösung möglicherweise. Sie sollten auch prüfen, ob die begrenzungsebene als Ergebnis Spitzen in Ihren Verkehr stattfindet und Methoden für Ihre Muster der Anfragen Glätten ermitteln.

Wenn Sie Ihre Transaktionen mehrere partitionsübergreifend verteilen, müssen Sie weiterhin der Skalierbarkeit Grenzwerte für das Speicherkonto bekannt sein. Wenn Sie zehn Warteschlangen jeder Verarbeitung der bis zu 2.000 1KB Nachrichten pro Sekunde verwendet haben, werden Sie beispielsweise bei der Grenzwert für insgesamt 20.000 Nachrichten pro Sekunde für das Speicherkonto sein. Wenn Sie mehr als 20.000 Elemente pro Sekunde verarbeiten müssen, sollten Sie mehrere Speicherkonten in Betracht ziehen. Sie sollten auch Folgendes beachten, dass die Größe Ihrer Anfragen und Personen wirkt sich auf, wenn der Speicherdienst Kunden Steuerung: Wenn Sie größere Besprechungsanfragen und Personen haben, Sie möglicherweise werden gedrosselt früher.

Nicht effizient Abfrageentwurf kann auch auf der Skalierbarkeit Grenzwerte für Tabellenpartitionen Treffer zu verursachen. Beispielsweise müssen eine Abfrage mit einem Filter, die nur ein Prozent der Elemente in einer Partition markiert, aber, die alle Elemente in einer Partition scannt, jede Entität zugreifen. Jede Entität gelesen wird in Richtung der Gesamtzahl der Transaktionen in dieser Partition zählen; Daher können Sie leicht die Skalierbarkeit Ziele erreichen.

> [AZURE.NOTE] Keine Designs für nicht effizient Abfrage in der Anwendung sollten die Leistungstests angezeigt werden.

### <a name="a-namemetrics-show-an-increase-in-percenttimeouterrorametrics-show-an-increase-in-percenttimeouterror"></a><a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Kennzahlen anzeigen eine Steigerung in PercentTimeoutError

Metrischen anzeigen eine Steigerung **PercentTimeoutError** für eine Ihrer Dienste Speicher Zur gleichen Zeit empfängt der Client eine große Anzahl von "500 Vorgang Timeout" HTTP-Statusnachrichten von Speichervorgänge an.

> [AZURE.NOTE] Timeoutfehler vorübergehend als der Speicherdienst Salden Anfragen laden, indem Sie eine Partition auf einen neuen Server verschieben wird angezeigt.

Die **PercentTimeoutError** Metrik ist die Zusammenfassung der folgende Metrik: **ClientTimeoutError**, **AnonymousClientTimeoutError**, **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**und **SASServerTimeoutError**.

Der Servertimeout werden durch ein Fehler auf dem Server verursacht. Der Client Zeitlimit geschehen, da ein Vorgang auf dem Server mit das vom Client angegebene Timeout überschritten hat; Beispielsweise kann ein Client mithilfe der Speicher-Client-Bibliothek einen Timeout für einen Vorgang festlegen, mithilfe der Eigenschaft **ServerTimeout** der Klasse **QueueRequestOptions** .

Servertimeout Hinweis auf ein Problem mit der Speicherdienst, die weiteren Untersuchung erforderlich sind. Sie können die Kennzahlen verwenden, um festzustellen, ob Sie die Skalierbarkeit Grenzwerte für den Dienst erreichen und alle Spitzen im Datenverkehr zu identifizieren, die dieses Problem verursachen können. Wenn das Problem nur gelegentlich auftritt, möglicherweise aufgrund von Lastenausgleich Aktivität im Dienst. Wenn das Problem beständigen ist und nicht durch eine Anwendung, drücken die Grenzwerte Skalierbarkeit des Diensts verursacht wird, müssen Sie ein Problem Support auslösen. Für Zeitlimit Client müssen Sie entscheiden, ob das Timeout auf einen entsprechenden Wert in dem Client, und ändern Sie entweder der Timeoutwert im Client festlegen oder ermitteln, wie Sie die Leistung der Vorgänge in der Speicherdienst beispielsweise verbessern können durch Optimieren Ihrer Tabellenabfragen oder Verringern der Größe Ihrer Nachrichten.

### <a name="a-namemetrics-show-an-increase-in-percentnetworkerrorametrics-show-an-increase-in-percentnetworkerror"></a><a name="metrics-show-an-increase-in-PercentNetworkError"></a>Kennzahlen anzeigen eine Steigerung in PercentNetworkError

Metrischen anzeigen eine Steigerung **PercentNetworkError** für eine Ihrer Dienste Speicher Die **PercentNetworkError** Metrik ist die Zusammenfassung der folgende Metrik: **NetworkError**, **AnonymousNetworkError**und **SASNetworkError**. Diese treten bei der Speicherdienst einen Fehler erkennt, wenn der Client Speicher anfordert.

Die häufigste Ursache dieses Fehlers ist ein Client trennen, bevor ein Timeout in der Speicherdienst läuft ab. Überprüfen Sie den Code in Ihren Kunden zu verstehen, warum und wann der Client aus der Speicherdienst getrennt wird. Sie können auch Wireshark, Microsoft Nachricht Analyzer oder Tcping untersuchen Sie Probleme mit der Netzwerkkonnektivität im Desktopclient verwenden. Diese Tools werden in den [Anhängen]beschrieben.

### <a name="a-namethe-client-is-receiving-403-messagesathe-client-is-receiving-http-403-forbidden-messages"></a><a name="the-client-is-receiving-403-messages"></a>Der Client ist HTTP 403 (Verboten) Nachrichten empfangen.

Wenn Ihre Clientanwendung HTTP 403 (Verboten) Fehler auszulösen ist, ist eine mögliche Ursache, dass der Client einer abgelaufenen freigegeben Access Signatur (SAS) verwendet wird, wenn er eine Anforderung Speicher sendet (zwar anderen möglichen Ursachen Uhr Schiefe, ungültige Tasten und leeren Kopfzeilen gehören). Ist eine abgelaufene SAS-Taste die Ursache, sehen Sie keine Einträge in die serverseitige Protokollierung Speicher Log Daten. Die folgende Tabelle zeigt ein Beispiel aus dem clientseitige Protokoll generiert durch die Speicher-Client-Bibliothek, die veranschaulicht dieses Problem auftritt:

Datenquelle|Ausführlichkeit|Ausführlichkeit|Client-Anforderung-id|Vorgang text
---|---|---|---|---
Microsoft.WindowsAzure.Storage|Informationen|3|85d077ab-...|Speicherort primär pro Standort Modus PrimaryOnly Vorgang angefangen.
Microsoft.WindowsAzure.Storage|Informationen|3|85d077ab-...|Beginnend synchroner Anfrage https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;sr = c&amp;Si von eigene Richtlinie =&amp;Sig = OFnd4Rd7z01fIvh % 2BmcR6zbudIH2F5Ikm % 2FyhNYZEmJNQ % 3D&amp;api-Version = 2014-02-14.
Microsoft.WindowsAzure.Storage|Informationen|3|85d077ab-...|Warten auf Antwort.
Microsoft.WindowsAzure.Storage|Warnung|2|85d077ab-...|Ausnahme beim Warten auf Antwort: der Remoteserver hat einen Fehler zurückgegeben: (403) verboten..
Microsoft.WindowsAzure.Storage|Informationen|3|85d077ab-...|Antwort erhalten. Statuscode = 403, Serviceanfrage-ID = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, Content-MD5 = ETag =.
Microsoft.WindowsAzure.Storage|Warnung|2|85d077ab-...|Ausnahmefehler während des Importvorgangs: der Remoteserver hat einen Fehler zurückgegeben: (403) verboten...
Microsoft.WindowsAzure.Storage|Informationen|3 |85d077ab-...|Überprüft, ob der Vorgang wiederholt werden soll. Anzahl der Wiederholungsversuche = 0, HTTP-Statuscode = 403, Ausnahme = remote-Server einen Fehler zurückgegeben: (403) verboten...
Microsoft.WindowsAzure.Storage|Informationen|3|85d077ab-...|Die nächste Position wurde auf Primär, basierend auf den Speicherort Modus gesetzt.
Microsoft.WindowsAzure.Storage|Fehler|1|85d077ab-...|Wiederholen Sie den Vorgang Richtlinie lässt sich nicht für eine Wiederholung. Fehlerhafte mit dem Remoteserver hat einen Fehler zurückgegeben: (403) verboten.

In diesem Szenario sollten Sie ermitteln, warum das SAS Token abläuft, bevor der Client das Token an den Server sendet:

- Üblicherweise sollten keine Anfangszeit legen Sie beim Erstellen einer SAS für einen Client, sofort zu verwenden. Wenn es kleine Uhr Unterschiede zwischen der Host gibt, die mit der aktuellen Uhrzeit und der Speicherdienst SAS generieren, ist es möglich, dass der Speicherdienst einem SAS erhalten, die noch nicht gültig ist.
- Sie sollten eine kürzester Ablaufzeit auf einem SAS festlegen. Kleine Uhr Unterschiede zwischen der Host die SAS und der Speicherdienst generiert können erneut zu einer zuvor als erwartet offensichtlich Ablauf SAS führen.
- Unterstützt den Version Parameter in die SAS-Taste (beispielsweise **PA = 2012-02-12**) entsprechen die Version der Bibliothek Client Speicher verwenden. Sie sollten immer die neueste Version der Bibliothek Client Speicher verwenden. Weitere Informationen über die token SAS-Versionen finden Sie unter [Was ist neu für Microsoft Azure-Speicher](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/14/what-s-new-for-microsoft-azure-storage-at-teched-2014.aspx).
- 
- Wenn Sie Ihre Speicher Zugriffstasten neu generieren (klicken Sie auf einer beliebigen Seite in Ihrem Speicherkonto im klassischen Azure-Portal auf **Zugriffstasten verwalten** ) können alle vorhandenen SAS Token ungültig werden. Dies kann ein Problem sein, wenn Sie SAS Token mit einem langen Ablauf Zeitabstand für Clientanwendungen Cache generieren.

Wenn Sie die Speicher-Client-Bibliothek SAS Token generieren verwenden, ist einfach ein gültiges Token zu erstellen. Wenn Sie die REST-API und die Token SAS von hand bauen sollten Sie das Thema <a href="http://msdn.microsoft.com/library/azure/ee395415.aspx" target="_blank">Delegieren von Access mit einer freigegebenen Access-Signatur</a> auf MSDN aufmerksam durchlesen.

### <a name="a-namethe-client-is-receiving-404-messagesathe-client-is-receiving-http-404-not-found-messages"></a><a name="the-client-is-receiving-404-messages"></a>Der Client ist HTTP 404 (nicht gefunden) Nachrichten empfangen.
Wenn die Clientanwendung eine Nachricht HTTP 404 (nicht gefunden) vom Server eingeht, bedeutet dies, dass das Objekt, das der Client bei dem Versuch verwenden (beispielsweise eine Entität, Tabelle, Blob, Container oder Warteschlange) nicht in der Speicherdienst vorhanden ist. Es gibt eine Reihe von möglichen Ursachen haben, wie ein:

- [Der Client oder einem anderen Prozess gelöscht zuvor auf das Objekt]
- [Eine freigegebene Access Signatur (SAS) Autorisierung Problem]
- [Clientseitige JavaScript-Code verfügt nicht über die Berechtigung zum Zugreifen auf des Objekts]
- [Einen Fehler im Netzwerk]

#### <a name="a-nameclient-previously-deleted-the-objectathe-client-or-another-process-previously-deleted-the-object"></a><a name="client-previously-deleted-the-object"></a>Der Client oder einem anderen Prozess gelöscht zuvor auf das Objekt
In Szenarien, in dem der Client versucht, lesen, aktualisieren oder Löschen von Daten in einer Speicherdienst, ist es in der Regel einfach zu einen vorherigen Vorgang in die serverseitige Protokolle zu identifizieren, der das betreffende Objekt aus der Speicherdienst gelöscht haben. Sehr häufig zeigt protokollierten Daten an, dass das Objekt ein anderer Benutzer oder Prozess gelöscht haben. Zeigen in der serverseitige Protokollierung Speicher Log den Vorgangstyp und angefordert-Objekt-Key Spalten Wenn ein Client ein Objekt gelöscht.

In dem Szenario, in dem ein Client versucht, ein Objekt einfügen, kann es nicht sofort offensichtlich sein warum dies in einer Antwort HTTP 404 (nicht gefunden) führt, vorausgesetzt, dass der Client Erstellen eines neuen Objekts ist. Allerdings muss, wenn der Client erstellt einen Blob den Container Blob finden können muss, wenn der Client erstellt eine Nachricht, dass eine Warteschlange finden können muss, und wenn der Client eine Zeile hinzufügen, wird es die Tabelle zu suchen können.

Können die clientseitige Log aus der Speicher-Client-Bibliothek um zu ausführlichere zu verstehen, wenn der Client bestimmte Anfragen an der Speicherdienst sendet.

Das folgende clientseitige Protokoll generiert von Speicher-Client-Bibliothek veranschaulicht das Problem, wenn der Client den Container für das Blob nicht finden kann, die es erstellt. Dieses Protokoll umfasst die folgenden Speichervorgänge Details:

ID anfordern|Vorgang
---|---
07b26a5d-...|So löschen Sie den Container Blob **DeleteIfExists** -Methode. Beachten Sie, dass dieser Vorgang eine Anforderung **Kopf** zu prüfen, ob das Vorhandensein des Containers enthält.
e2d06d78...|**CreateIfNotExists** -Methode den Blob-Container erstellen. Beachten Sie, dass dieser Vorgang eine Anforderung **Kopf** enthält, die für das Vorhandensein des Containers überprüft. Der **Kopf** einer Nachricht 404 gibt jedoch weiterhin.
de8b1c3c-...|**UploadFromStream** -Methode das Blob zu erstellen. Die Anfrage **setzen** fehlschlägt mit einer Nachricht 404

Protokolleinträge:

ID anfordern |  Vorgang Text
---|---
07b26a5d-...|Synchroner Anfrage https://domemaildist.blob.core.windows.net/azuremmblobcontainer wird gestartet.
07b26a5d-...|StringToSign = HEAD...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 Jun 2014 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-...|Warten auf Antwort.
07b26a5d-... | Antwort erhalten. Statuscode = 200, Serviceanfrage-ID = eeead849... Content-MD5 = ETag = &quot;0x8D14D2DC63D059B&quot;.
07b26a5d-... | Antwort-Header verarbeitet wurden erfolgreich, fortzusetzen, des Vorgangs.
07b26a5d-... | Herunterladen von Textkörper der Antwort.
07b26a5d-... | Der Vorgang wurde erfolgreich abgeschlossen.
07b26a5d-... | Synchroner Anfrage https://domemaildist.blob.core.windows.net/azuremmblobcontainer wird gestartet.
07b26a5d-... | StringToSign = DELETE...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-... | Warten auf Antwort.
07b26a5d-... | Antwort erhalten. Statuscode = 202, Serviceanfrage-ID = 6ab2a4cf-..., Content-MD5 = ETag =.
07b26a5d-... | Antwort-Header verarbeitet wurden erfolgreich, fortzusetzen, des Vorgangs.
07b26a5d-... | Herunterladen von Textkörper der Antwort.
07b26a5d-... | Der Vorgang wurde erfolgreich abgeschlossen.
e2d06d78-... | Asynchrone Anforderung zum https://domemaildist.blob.core.windows.net/azuremmblobcontainer wird gestartet.</td>
e2d06d78-... | StringToSign = HEAD...x-ms-client-request-id:e2d06d78-...x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-...| Warten auf Antwort.
de8b1c3c-... | Synchroner Anfrage https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt wird gestartet.
de8b1c3c-... |  StringToSign = sich... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-ms-BLOB-Type:BlockBlob.x-ms-Client-Request-ID:de8b1c3c-...x-ms-Date:tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... | Vorbereiten der Anfrage Daten zu schreiben.
e2d06d78-... | Ausnahme beim Warten auf Antwort: der Remoteserver hat einen Fehler zurückgegeben: (404) nicht gefunden..
e2d06d78-... | Antwort erhalten. Statuscode = 404, Serviceanfrage-ID = 353ae3bc-..., Content-MD5 = ETag =.
e2d06d78-... | Antwort-Header verarbeitet wurden erfolgreich, fortzusetzen, des Vorgangs.
e2d06d78-... | Herunterladen von Textkörper der Antwort.
e2d06d78-... | Der Vorgang wurde erfolgreich abgeschlossen.
e2d06d78-... | Asynchrone Anforderung zum https://domemaildist.blob.core.windows.net/azuremmblobcontainer wird gestartet.
e2d06d78-...|StringToSign = sich... 0...x-ms-Client-Request-ID:e2d06d78-...x-ms-Date:tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-... | Warten auf Antwort.
de8b1c3c-... | Anfrage und Schreiben von Daten.
de8b1c3c-... | Warten auf Antwort.
e2d06d78-... | Ausnahme beim Warten auf Antwort: der Remoteserver hat einen Fehler zurückgegeben: (409) Konflikt...
e2d06d78-... | Antwort erhalten. Statuscode = 409 Serviceanfrage-ID = c27da20e-..., Content-MD5 = ETag =.
e2d06d78-... | Herunterladen von Fehler Antwort Textkörper.
de8b1c3c-... | Ausnahme beim Warten auf Antwort: der Remoteserver hat einen Fehler zurückgegeben: (404) nicht gefunden..
de8b1c3c-... | Antwort erhalten. Statuscode = 404, Serviceanfrage-ID = 0eaeab3e-..., Content-MD5 = ETag =.
de8b1c3c-...| Ausnahmefehler während des Importvorgangs: der Remoteserver hat einen Fehler zurückgegeben: (404) nicht gefunden..
de8b1c3c-... | Wiederholen Sie den Vorgang Richtlinie lässt sich nicht für eine Wiederholung. Fehlerhafte mit dem Remoteserver hat einen Fehler zurückgegeben: (404) nicht gefunden.
e2d06d78-... | Wiederholen Sie den Vorgang Richtlinie lässt sich nicht für eine Wiederholung. Fehlerhafte mit dem Remoteserver hat einen Fehler zurückgegeben: (409) Konflikt...

In diesem Beispiel zeigt das Protokoll, dass der Client (de8b1c3c-...); für Anfragen der Methode **CreateIfNotExists** (Anforderung-Id e2d06d78...) mit den Anfragen der Methode **UploadFromStream** verschachteln Dies geschieht, da die Clientanwendung diese Methoden asynchrone aufgerufen wird. Ändern Sie den asynchronen Code im Client, um sicherzustellen, dass es sich um den Container erstellt, bevor Sie versuchen, Daten in einen Blob im Container hochladen. Idealerweise sollten Sie alle Ihre Container im Voraus erstellen.

#### <a name="a-namesas-authorization-issueaa-shared-access-signature-sas-authorization-issue"></a><a name="SAS-authorization-issue"></a>Eine freigegebene Access Signatur (SAS) Autorisierung Problem

Wenn die Clientanwendung versucht, SAS Key zu verwenden, der nicht die erforderlichen Berechtigungen für den Vorgang enthalten ist, gibt der Speicherdienst eine Nachricht HTTP 404 (nicht gefunden) an den Client. Zur gleichen Zeit werden Sie auch einen Wert ungleich NULL **SASAuthorizationError** in die Metrik finden Sie unter.

Die folgende Tabelle enthält eine serverseitige Log beispielmitteilung aus der Protokolldatei Speicher Protokollierung:

<table>
  <tr>
    <td>Anfordern der Startzeit</td>
    <td>2014-05-30T06:17:48.4473697Z</td>
  </tr>
  <tr>
    <td>Vorgangstyp</td>
    <td>GetBlobProperties</td>
  </tr>
  <tr>
    <td>Status der Anfrage</td>
    <td>SASAuthorizationError</td>
  </tr>
  <tr>
    <td>HTTP-Statuscode</td>
    <td>404</td>
  </tr>
  <tr>
    <td>Authentifizierungstyp</td>
    <td>SAS</td>
  </tr>
  <tr>
    <td>Diensttyp</td>
    <td>BLOB</td>
  </tr>
  <tr>
    <td>Anfordern von URL</td>
    <td>
    https://domemaildist.BLOB.Core.Windows.NET/azureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;Amp; sr = c&amp;Amp; Si von eigene Richtlinie =&amp;Amp; Sig = XXXXX&amp;Amp; api-Version 2014-02-14 =&amp;Amp;</td>
  </tr>
  <tr>
    <td>Anfrage-Id-header</td>
    <td>a1f348d5-8032-4912-93ef-b393e5252a3b</td>
  </tr>
  <tr>
    <td>Client-Anforderung-ID</td>
    <td>2d064953-8436-4ee0-aa0c-65cb874f7929</td>
  </tr>
</table>

Sie sollten untersuchen, warum die Clientanwendung versucht, einen Vorgang auszuführen, die, dem Sie nicht für Berechtigungen gewährt wurde.

#### <a name="a-namejavascript-code-does-not-have-permissionaclient-side-javascript-code-does-not-have-permission-to-access-the-object"></a><a name="JavaScript-code-does-not-have-permission"></a>Clientseitige JavaScript-Code verfügt nicht über die Berechtigung zum Zugreifen auf des Objekts

Wenn Sie einen JavaScript-Client und der Speicherdienst ist HTTP 404 Nachrichten zurückgeben, prüfen Sie die folgenden JavaScript-Fehler im Browser.

    SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
    SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.

> [AZURE.NOTE] Können die Entwicklertools F12 in Internet Explorer die Nachrichten, die zwischen dem Browser und der Speicherdienst ausgetauscht werden, bei der Problembehandlung clientseitige JavaScript Probleme verfolgen.

Dieser Fehler auftreten, da im Webbrowser die Einschränkung <a href="http://www.w3.org/Security/wiki/Same_Origin_Policy" target="_blank">same Origin Policy</a> Sicherheit, die verhindert, dass eine Webseite Aufrufen einer API in eine andere Domäne aus der Domäne, die implementiert, der aus die Seite stammen.

Um JavaScript dieses Problem zu umgehen, können Sie Cross Origin Ressource freigeben (CORS) für den Speicherdienst konfigurieren, auf der Client zugreifen wird. Weitere Informationen finden Sie unter <a href="http://msdn.microsoft.com/library/azure/dn535601.aspx" target="_blank">Cross-Origin Ressource freigeben (CORS) Unterstützung für Azure Storage Services</a> auf MSDN.

Im folgenden Beispiel veranschaulicht, wie Ihre Blob-Dienst JavaScript unter in der Contoso-Domäne einen Blob in Ihrem Blob-Speicher-Dienst Zugriff auf zulassen konfigurieren:

    CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
    // Set the service properties.
    ServiceProperties sp = client.GetServiceProperties();
    sp.DefaultServiceVersion = "2013-08-15";
    CorsRule cr = new CorsRule();
    cr.AllowedHeaders.Add("*");
    cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
    cr.AllowedOrigins.Add("http://www.contoso.com");
    cr.ExposedHeaders.Add("x-ms-*");
    cr.MaxAgeInSeconds = 5;
    sp.Cors.CorsRules.Clear();
    sp.Cors.CorsRules.Add(cr);
    client.SetServiceProperties(sp);

#### <a name="a-namenetwork-failureanetwork-failure"></a><a name="network-failure"></a>Einen Fehler im Netzwerk

In einigen Fällen können verloren Netzwerkpakete zu der Speicherdienst HTTP 404 Nachrichten an den Client zurückgeben führen. Wenn Ihre Clientanwendung eine Entität aus der Tabelle Dienst löschen ist Sie finden Sie beispielsweise den Kunden ein reporting zu Speicher Ausnahme auslösen eines "HTTP 404 (nicht gefunden)" Statusnachricht vom Dienst Tabelle. Wenn Sie die Tabelle in der Tabelle Speicherdienst untersuchen, wird der Dienst die Entität nicht gelöscht, wie gewünscht.

Die Ausnahmedetails im Client gehören die Anfrage-Id (7e84f12d...) zugewiesen, indem Sie die Tabelle-Dienst für die Anforderung: mithilfe dieser Informationen können Sie die Details der Anforderung die Protokolle der serverseitigen Speicher durch Suchen in der Spalte **Anfrage-Id-Header** der Protokolldatei suchen. Sie können auch die Metrik um zu identifizieren, wenn Fehler wie z. B. dies auftreten, und suchen Sie die Protokolldateien basierend auf der Uhrzeit, die Metrik dieser Fehler aufgezeichnet, verwenden. Dieses Protokolleintrag zeigt an, dass die Fehlermeldung "HTTP (404) Client anderer Fehler" der Status der Löschvorgang fehlgeschlagen ist. Der gleichen Log-Eintrag enthält auch die Anfrage-Id, die auf dem Client in der Spalte **Client-Anforderung-Id** (813ea74f...) aus.

Der serverseitigen Log enthält darüber hinaus einen weiteren Eintrag mit dem gleichen Wert (813ea74f...) **Client-Anforderung-Id** bei einem erfolgreichen Löschvorgang für die betreffende Entität und vom selben Client. Dieser Löschvorgang erfolgreich wurden sehr kurz vor der fehlgeschlagene Anforderung gelöscht werden soll.

Mögliche Ursache für dieses Szenario ist, dass der Client eine Anforderung löschen, für die Entität in der Tabelle Service, die erfolgreich verlaufen ist gesendet, aber keine Bestätigung empfangen wurde, vom Server (möglicherweise, weil ein temporäres Netzwerkproblem). Der Client wiederholt dann automatisch den Vorgang (mit der gleichen **Client-Anforderung-Id**), und diese "Wiederholen" fehlgeschlagen ist, da die Entität bereits gelöscht wurde.

Wenn dieses Problem häufig auftritt, sollten Sie untersuchen, warum der Client fehlschlägt Empfangsbestätigungen aus der Tabelle Dienst erhalten. Ist das Problem wiederkehrender, sollten Sie überfüllt die Fehlermeldung "HTTP (404) wurde nicht gefunden" und der Client anmelden, sondern ermöglichen es dem Client, um den Vorgang fortzusetzen.

### <a name="a-namethe-client-is-receiving-409-messagesathe-client-is-receiving-http-409-conflict-messages"></a><a name="the-client-is-receiving-409-messages"></a>Der Client ist HTTP 409 (Konflikt) Nachrichten empfangen.

Die folgende Tabelle enthält eine Extrahieren aus dem serverseitigen Protokoll für zwei Clientvorgänge: **DeleteIfExists** und unmittelbar danach **CreateIfNotExists** mit demselben Namen Blob Container. Notiz, die jeder Client der Ergebnisse in zwei Anforderungen an den Server zuerst gesendet Anforderung einer **GetContainerProperties** prüfen, ob der Container vorhanden ist, gefolgt von der Anfrage **DeleteContainer** oder **CreateContainer** .

Zeitstempel|Vorgang|Ergebnis|Container mit dem Namen|Client-Anforderung-id
---|---|---|---|---
05:10:13.7167225|GetContainerProperties|200|mmcont|c9f52c89-...
05:10:13.8167325|DeleteContainer|202|mmcont|c9f52c89-...
05:10:13.8987407|GetContainerProperties|404|mmcont|bc881924-...
05:10:14.2147723|CreateContainer|409|mmcont|bc881924-...

Der Code in der Clientanwendung löscht und erstellt dann einen Blob-Container denselben Namen sofort neu: die **CreateIfNotExists** -Methode (Client anfordern ID bc881924 –...) später mit dem HTTP-409 (Konflikt) Fehler schlägt fehl. Wenn ein Client löscht, Blob Container, Tabellen oder Warteschlangen vorhanden kurzzeitig vor dem Namen ist wird wieder verfügbar.

Die Clientanwendung sollten eindeutigen Containernamen verwenden, wenn sie neue Container erstellt haben, ist das Löschen/erneutes Erstellen Muster allgemeine.

### <a name="a-namemetrics-show-low-percent-successametrics-show-low-percentsuccess-or-analytics-log-entries-have-operations-with-transaction-status-of-clientothererrors"></a><a name="metrics-show-low-percent-success"></a>Kennzahlen anzeigen niedrig PercentSuccess oder Analytics Protokolleinträge haben Vorgänge mit Transaktionsstatus der ClientOtherErrors

Die Metrik **PercentSuccess** erfasst des Prozentsatzes der Vorgänge, die basierend auf deren HTTP-Status Code erfolgreich waren. Vorgänge mit Statuscodes der 2XX zählen als erfolgreich, während die Vorgänge mit Statuscodes in 3XX, 4XX und 5XX Bereiche werden als nicht erfolgreich berücksichtigt und verringern den **PercentSucess** metrischen Wert. Diese Vorgänge werden in den Protokolldateien serverseitigen Speicher mit dem Transaktionsstatus **ClientOtherErrors**aufgezeichnet.

Es ist wichtig, beachten Sie, dass diese Vorgänge erfolgreich abgeschlossen haben und daher anderer Größen wie z. B. Verfügbarkeit nicht beeinträchtigen. Einige Beispiele für Vorgänge, die erfolgreich ausführen, aber nicht erfolgreich HTTP Statuscodes führen kann:
- **ResourceNotFound** (404 nicht gefunden,) beispielsweise aus eine GET-Anforderung in ein Blob, die nicht vorhanden ist.
- **ResouceAlreadyExists** (Konflikt 409), beispielsweise aus einem **CreateIfNotExist** Vorgang, in dem die Ressource bereits vorhanden ist.
- **ConditionNotMet** (Nicht geändert 304), beispielsweise aus einer bedingten Operation wie z. B., wenn ein Client sendet, einen **ETag** -Wert und einen HTTP- **If-keine-Match** -Header So fordern Sie ein Bild an, nur, wenn es seit dem letzten Vorgang aktualisiert wurde.

Sie können eine Liste der allgemeinen REST-API Fehlercodes suchen, die die Speicherdienste auf der Seite <a href="http://msdn.microsoft.com/library/azure/dd179357.aspx" target="_blank">Allgemeine REST API Fehlercodes</a>zurückgeben.

### <a name="a-namecapacity-metrics-show-an-unexpected-increaseacapacity-metrics-show-an-unexpected-increase-in-storage-capacity-usage"></a><a name="capacity-metrics-show-an-unexpected-increase"></a>Kennzahlen Kapazität Anzeigen einer unerwartete erhöhen in Kapazität speichernutzung


Wenn plötzlich angezeigt wird, können unerwartete Änderungen Kapazität Verwendung in Ihr Speicherkonto Sie Gründe untersuchen, indem Sie zuerst die untersucht metrischen Einheiten; Angenommen, eine erhöhen die Anzahl der Fehler beim Löschen Anfragen zu einer Erhöhung Blob-Speicher führen möglicherweise abgegeben haben wie die Anwendung bestimmte Aufräumen Vorgänge, die Sie möglicherweise erwartet haben, werden Sie können Speicherplatz nicht erwartungsgemäß werden können (z. B., weil die SAS Token für das Freigeben von Speicherplatz verwendet abgelaufen sind).

### <a name="a-nameyou-are-experiencing-unexpected-rebootsayou-are-experiencing-unexpected-reboots-of-azure-virtual-machines-that-have-a-large-number-of-attached-vhds"></a><a name="you-are-experiencing-unexpected-reboots"></a>Unerwartete nach einem Neustart von Azure virtuellen Computern auftreten, die eine große Anzahl von angefügten virtuellen Festplatten aufweisen

Wenn eine Azure-virtuellen Computern (VM) eine große Anzahl von angefügten virtuellen Festplatten aufweist, in demselben Speicherkonto sind, könnten Sie der Skalierbarkeit Ziele für eine einzelne Speicher Firma, die bewirken, dass den virtuellen Computer zum Fehlschlagen nicht überschreiten. Sie sollten die Minute Metrik für das Speicherkonto (**TotalRequests**/**TotalIngress**/**TotalEgress**) für Spitzen, die die Ziele Skalierbarkeit für ein Speicherkonto überschreiten. Finden Sie im Abschnitt "[Kennzahlen Erhöhung PercentThrottlingError anzeigen]", um festzustellen, ob begrenzungsebene Unterstützung für Ihr Speicherkonto aufgetreten ist.

Im Allgemeinen wird jede einzelne Eingabe- oder Ausgabe Vorgang für eine virtuelle Festplatte anhand eines virtuellen Computers in der zugrunde liegenden Seitenblob **Erste Seite** oder die **Seite setzen** Vorgänge übersetzt. Daher können Sie die geschätzte IOPS für Ihre Umgebung verwenden, wie viele virtuelle Festplatten optimieren in einem einzigen Speicher-Konto das spezielle Verhalten der Anwendung haben basierend auf können. Wir empfehlen nicht mehr als 40 Datenträger in einem einzigen Speicher-Konto Probleme. Einzelheiten finden Sie unter <a href="http://msdn.microsoft.com/library/azure/dn249410.aspx" target="_blank">Azure Speicher Skalierbarkeit und Leistung</a> der aktuellen Skalierbarkeit Ziele für Speicherkonten, insbesondere die Anfrage für insgesamt Zinssatz sowie die gesamte Bandbreite für die Art des verwendeten Programms Speicherkontos.
Wenn Sie die Ziele Skalierbarkeit für Ihr Speicherkonto überschreiten, sollten Sie Ihre virtuellen Festplatten in mehrere verschiedene Speicher-Konten, die Aktivität jede einzelne Firma zu verringern platzieren.

### <a name="a-nameyour-issue-arises-from-using-the-storage-emulatorayour-issue-arises-from-using-the-storage-emulator-for-development-or-test"></a><a name="your-issue-arises-from-using-the-storage-emulator"></a>Das Problem tritt auf, verwenden Sie für die Entwicklung oder Test Speicheremulator

Klicken Sie in der Regel Speicheremulator während der Entwicklung verwenden und testen, um die Anforderung für ein Konto Azure-Speicher zu vermeiden. Die häufig auftretende Probleme können bei Verwendung der Speicheremulator sind:

- [Feature "X" funktioniert nicht in der Speicheremulator]
- [Fehler "der Wert für eine der Überschriften HTTP ist nicht im richtigen Format" bei Verwendung von Speicheremulator]
- [Ausführen der Speicheremulator sind Administratorrechte erforderlich]

#### <a name="a-namefeature-x-is-not-workingafeature-x-is-not-working-in-the-storage-emulator"></a><a name="feature-X-is-not-working"></a>Feature "X" funktioniert nicht in der Speicheremulator

Speicheremulator unterstützt nicht alle Features der Azure-Speicherdienste wie dem Dateidienst. Weitere Informationen finden Sie unter <a href="http://msdn.microsoft.com/library/azure/gg433135.aspx" target="_blank">Unterschiede zwischen der Speicheremulator und Azure Storage Services</a> auf MSDN.

Verwenden Sie für diese Features, die Speicheremulator nicht unterstützt, der Azure-Speicherdienst in der Cloud.

#### <a name="a-nameerror-http-header-not-correct-formataerror-the-value-for-one-of-the-http-headers-is-not-in-the-correct-format-when-using-the-storage-emulator"></a><a name="error-HTTP-header-not-correct-format"></a>Fehler "der Wert für eine der Überschriften HTTP ist nicht im richtigen Format" bei Verwendung von Speicheremulator

Testen Sie die Anwendung, die Speicher-Client-Bibliothek gegen die lokale Speicheremulator und Methode ruft wie **CreateIfNotExists** möglicher Fehler mit der Fehlermeldung "der Wert für eine der Überschriften HTTP nicht im richtigen Format ist." Dies zeigt an, dass die Version des verwendeten Programms Speicheremulator derzeit die Version der Bibliothek Client Speicher nicht unterstützt. Die Speicher-Client-Bibliothek hinzugefügt die erfolgten Anfragen der Kopfzeile **X-ms-Version** . Wenn der Speicheremulator den Wert in der Kopfzeile **X-ms-Version** nicht erkannt wird, wird die Anforderung abgelehnt.

Die Protokolle Speicher Library-Client können Sie den Wert der **Kopfzeile X-ms-Version** finden Sie unter gesendeten. Sie können auch den Wert der **Kopfzeile X-ms-Version** angezeigt, wenn Sie Fiddler verwenden, um die Anfragen aus der Clientanwendung verfolgen.

Dieses Szenario tritt in der Regel ein, wenn Sie installieren und verwenden die neueste Version der Bibliothek Client Speicher ohne Speicheremulator aktualisieren. Sie sollten entweder Installieren der neueste Version von Speicheremulator, oder verwenden Cloud-Speicher anstelle der Emulator für Entwicklung und testen.

#### <a name="a-namestorage-emulator-requires-administrative-privilegesarunning-the-storage-emulator-requires-administrative-privileges"></a><a name="storage-emulator-requires-administrative-privileges"></a>Ausführen der Speicheremulator sind Administratorrechte erforderlich

Sie werden beim Ausführen der Speicheremulator Administrator-Anmeldeinformationen aufgefordert. Dies findet nur statt, wenn Sie zum ersten Mal Speicheremulator Initialisierung sind. Nach Sie Speicheremulator Initialisierung haben, brauchen Sie nicht über Administratorrechte verfügen, um erneut auszuführen.

Weitere Informationen finden Sie unter <a href="http://msdn.microsoft.com/library/azure/gg433132.aspx" target="_blank">Speicheremulator mithilfe des Tools Line Initialisierung</a> auf MSDN (Sie können auch in Visual Studio, die auch Administratorrechte erforderlich werden Speicheremulator Initialisierung).

### <a name="a-nameyou-are-encountering-problems-installing-the-windows-azure-sdkayou-are-encountering-problems-installing-the-azure-sdk-for-net"></a><a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>Sie werden bei der Installation der Azure SDK für .NET Probleme auftreten.

Wenn Sie versuchen, das SDK installieren, schlägt es versuchen, Speicheremulator auf Ihrem lokalen Computer installieren. Protokolldatei der Installation enthält eine der folgenden angezeigt:

- CAQuietExec: Fehler: nicht auf SQL Server-Instanz zugreifen.
- CAQuietExec: Fehler: keine Datenbank erstellen

Die Ursache ist ein Problem mit vorhandenen LocalDB Installation. Standardmäßig verwendet Speicheremulator LocalDB Daten beibehalten werden, wenn er die Dienste Azure-Speicher simuliert. Sie können Ihre LocalDB Instanz zurücksetzen, indem Sie die folgenden Befehle in ein Eingabeaufforderungsfenster ausführen, bevor Sie versuchen, das SDK installieren.

    sqllocaldb stop v11.0
    sqllocaldb delete v11.0
    delete %USERPROFILE%\WAStorageEmulatorDb3*.*
    sqllocaldb create v11.0

Der Befehl **Löschen** entfernt alle alten Datenbankdateien aus vorherigen Installationen von Speicheremulator.

### <a name="a-nameyou-have-a-different-issue-with-a-storage-serviceayou-have-a-different-issue-with-a-storage-service"></a><a name="you-have-a-different-issue-with-a-storage-service"></a>Sie haben ein anderes Problem mit einer Speicherdienst

Wenn die vorherigen Abschnitte zur Problembehandlung nicht das Problem, die, das Sie mit einer Speicherdienst haben einschließen, sollten Sie Diagnose und Problembehandlung Ihres Problems wie folgt vorgehen übernommen.

- Aktivieren Sie metrischen um festzustellen, ob Änderungen aus der Grundlinie erwartet vorhanden ist. Aus der Kennzahlen können Sie möglicherweise feststellen, ob das Problem vorübergehend oder permanent ist und welche Speichervorgänge das Problem ist auswirkt.
- Die Informationen für Kennzahlen können mit deren Hilfe Sie Ihre serverseitigen Log Daten Ausführlichere Informationen zu Fehlern suchen, die auftreten. Diese Informationen helfen Ihnen bei der Problembehandlung und das Problem zu beheben.
- Wenn die Informationen in die serverseitige Protokolle nicht ausreichend, um das Problem zu beheben, erfolgreich ist, können die Speicher Clientbibliothek clientseitige Protokolle Sie um das Verhalten von Clientanwendung und Tools wie Fiddler, Wireshark und Microsoft Nachricht Analyzer untersuchen von Ihrem Netzwerk zu ermitteln.

Weitere Informationen zur Verwendung von Fiddler, finden Sie unter "[Anlage 1: verwenden Fiddler, um HTTP und HTTPS-Verkehr zu erfassen]."

Weitere Informationen zur Verwendung von Wireshark, finden Sie unter "[Anlage 2: mithilfe von Wireshark zum Erfassen von Netzwerkverkehr]."

Weitere Informationen zur Verwendung von Microsoft Nachricht Analyzer, finden Sie unter "[Anlage 3: Erfassen von Netzwerkverkehr mithilfe von Microsoft Message Analyzer]."

## <a name="a-nameappendicesaappendices"></a><a name="appendices"></a>Anlagen

Die Anhänge beschreiben verschiedene Tools, die nützlich sein können bei der Diagnose und Behebung von Problemen mit Azure-Speicher (und andere Dienste). Diese Tools sind nicht Bestandteil der Azure-Speicher und einige Drittanbieter-Produkte sind. Als solche, die in diese Anhänge erläuterten Tools fallen nicht unter einer beliebigen Support-Vertrag mit Microsoft Azure oder Azure-Speicher mögliche, und Sie sollten daher als Teil des Prozesses Auswertung Lizenzierung und Support verfügbaren Optionen aus den Anbietern dieser Tools überprüfen.

### <a name="a-nameappendix-1aappendix-1-using-fiddler-to-capture-http-and-https-traffic"></a><a name="appendix-1"></a>Anlage 1: Verwenden von Fiddler, HTTP und HTTPS-Verkehr zu erfassen

Fiddler ist ein nützliches Tool für die Analyse des HTTP und HTTPS-Datenverkehrs zwischen der Clientanwendung und der Azure-Speicherdienst abgegeben haben. Sie können Fiddler von <a href="http://www.telerik.com/fiddler" target="_blank">http://www.telerik.com/fiddler</a>herunterladen.

> [AZURE.NOTE] Fiddler kann HTTPS-Datenverkehr entschlüsseln; Sie sollten die Fiddler Dokumentation sorgfältig, um zu verstehen, wie das funktioniert, und um die Auswirkungen Sicherheit zu lesen.

Dieser Abschnitt enthält eine kurze exemplarische Vorgehensweise zum Erfassen von Datenverkehr zwischen dem lokalen Computer, in dem Sie Fiddler installiert haben, und die Dienste Azure-Speicher Fiddler konfigurieren.

Nachdem Sie Fiddler gestartet haben, wird gestartet HTTP und HTTPS-Datenverkehr auf dem lokalen Computer erfassen. Es folgen einige hilfreiche Befehle zum Steuern der Fiddler:

- Beenden Sie und starten Sie den Datenverkehr erfassen. Im Hauptfenster Menü Wechseln Sie zu der **Datei** , und klicken Sie dann auf **Datenverkehr erfassen** ein-und ausblenden erfassen ein- und auszuschalten.
- Speichern Sie aufgezeichneter Datenverkehr Daten an. Wechseln Sie auf klicken Sie im Menü **Datei**, klicken Sie auf **Speichern**, und klicken Sie dann auf **Alle Sitzungen**: So können Sie den Datenverkehr in einer Sitzung Archivdatei zu speichern. Sie können eine Sitzung Archiv später für die Analyse erneut zu laden, oder senden Sie es an den Microsoft Support angefordert.

Um die Menge des Datenverkehrs beschränken, die Fiddler erfasst werden, können Sie Filter aus, die Sie auf der Registerkarte **Filter** konfigurieren. Das folgende Bildschirmabbild zeigt einen Filter, der nur den Datenverkehr an den Endpunkt des **contosoemaildist.table.core.windows.net** Speicher gesendet werden erfasst:

![][5]

### <a name="a-nameappendix-2aappendix-2-using-wireshark-to-capture-network-traffic"></a><a name="appendix-2"></a>Anlage 2: Verwenden von Wireshark zum Netzwerkverkehr erfassen

Wireshark ist, die Sie detaillierte Paketinformationen für eine Vielzahl von Protokollen der anzeigen ermöglicht Netzwerk Protokolle zu analysieren. Sie können Wireshark von <a href="http://www.wireshark.org/" target="_blank">http://www.wireshark.org/</a>herunterladen.

Im folgende Verfahren wird gezeigt, wie zu erfassen detaillierte Paketinformationen für den Datenverkehr aus dem lokalen Computer, haben Sie installiert Wireshark zum Dienst Tabelle in Ihr Konto Azure-Speicher.

1.  Starten Sie Wireshark auf Ihrem lokalen Computer an.
2.  Wählen Sie im Abschnitt **Starten** der lokalen Netzwerk-Benutzeroberfläche oder Schnittstellen, die mit dem Internet verbunden sind.
3.  Klicken Sie auf **Optionen erfassen**.
4.  Hinzufügen eines Filters auf das Textfeld **Filter erfassen** . **Host contosoemaildist.table.core.windows.net** wird beispielsweise Wireshark zum Erfassen von nur Pakete gesendet werden, in den oder aus der Tabelle-Endpunkt im Speicher **Contosoemaildist** Konto konfigurieren. Eine vollständige Liste der Filter erfassen finden Sie unter <a href="http://wiki.wireshark.org/CaptureFilters" target="_blank">http://wiki.wireshark.org/CaptureFilters</a>.

    ![][6]

5.  Klicken Sie auf **Start**. Wireshark wird jetzt alle erfassen, die die Pakete in den oder aus der Tabelle Endpunkt senden, wie Sie Ihre Client-Anwendung auf dem lokalen Computer verwenden.
6.  Wenn Sie fertig sind, klicken Sie im Hauptfenster auf **erfassen** , und klicken Sie dann auf **Beenden**.
7.  Um die aufgezeichneten Daten in einer Wireshark erfassen Datei speichern möchten, klicken Sie auf das Hauptmenü auf **Datei** und dann auf **Speichern**.

WireShark werden alle Fehler hervorheben, die Sie im Fenster **Packetlist** vorhanden sind. Sie können auch im Fenster **Experten Informationen** verwenden (klicken Sie auf **Analysieren**, und klicken Sie dann auf **Informationen von Experten**) eine Zusammenfassung von Fehlern und Warnungen anzeigen.

![][7]

Sie können auch die Option TCP-Daten anzeigen, wie die Anwendungsebene sieht, indem Sie mit der rechten Maustaste auf die TCP-Daten, und **Führen Sie die TCP-Stream**auswählen. Dies ist besonders hilfreich, wenn Sie das Abbild erstellen, ohne einen Filter erfassen erfasst. Finden Sie <a href="http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html" target="_blank">hier</a> Weitere Informationen.

![][8]

> [AZURE.NOTE] Weitere Informationen zur Verwendung von Wireshark finden Sie unter der <a href="http://www.wireshark.org/docs/wsug_html_chunked/" target="_blank">Wireshark-Benutzerhandbuch</a>.

### <a name="a-nameappendix-3aappendix-3-using-microsoft-message-analyzer-to-capture-network-traffic"></a><a name="appendix-3"></a>Anlage 3: Mithilfe von Microsoft Nachricht Analyzer Netzwerkverkehr erfassen

Sie können Sie verwenden von Microsoft Nachricht Analyzer, um HTTP und HTTPS-Verkehr auf ähnliche Weise zu Fiddler zu erfassen und in ähnlicher Weise auf Wireshark Netzwerkverkehr erfassen.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Konfigurieren einer Web Tracingsitzung mithilfe von Microsoft Nachricht Analyzer

Um eine Web Tracingsitzung für HTTP- und HTTPS-Datenverkehr mithilfe von Microsoft Nachricht Analyzer konfigurieren möchten, führen Sie die Anwendung Microsoft Nachricht Analyzer, und klicken Sie dann im Menü **Datei** auf **Spur /**. Wählen Sie in der Liste der verfügbaren Spur Szenarien **Web Proxy**ein. Klicken Sie dann im Bereich **Spur Szenariokonfiguration** in das Textfeld **HostnameFilter** , fügen Sie die Namen der Speicher Endpunkte (Sie können diese Namen in der klassischen Azure-Portal nachschlagen) an. Wenn der Name Ihres Kontos Azure-Speicher **Contosodata**ist, sollten Sie Folgendes in das Textfeld **HostnameFilter** hinzufügen:

    contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net

> [AZURE.NOTE] Ein Leerzeichen trennt die Hostnamen.

Wenn Sie zu dem Sammeln der Spur Daten bereit sind, klicken Sie auf die Schaltfläche **Start mit** .

Weitere Informationen zu den Microsoft Nachricht Analyzer **Web Proxy** Spur finden Sie unter <a href="http://technet.microsoft.com/library/jj674814.aspx" target="_blank">PEF-WebProxy Anbieter</a> auf TechNet.

Die integrierten **Web Proxy** Spur in Microsoft Nachricht Analyzer basiert auf Fiddler; Sie können clientseitige HTTPS-Datenverkehr erfassen und Anzeigen von unverschlüsselter HTTPS-Nachrichten. Der **Web Proxy** -Spur funktioniert, wenn Sie einen lokalen Proxy für alle HTTP und HTTPS-Datenverkehr, der sie Zugriff auf unverschlüsselter Nachrichten konfigurieren.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Diagnostizieren von Netzwerkproblemen mithilfe von Microsoft Nachricht Analyzer

Zusätzlich zu den Microsoft Nachricht Analyzer **Web Proxy** Spur zum Erfassen von Details zu den HTTP-/HTTPs-Datenverkehr zwischen der Clientanwendung und der Speicherdienst verwenden, können Sie auch die integrierten **Lokalen Link Layer** Spur zum Netzwerk Paket erfassen verwenden. Diese ermöglicht, die ähnliche Daten aufnehmen können Sie mit Wireshark erfassen und diagnostizieren Sie Probleme wie Netzwerk Pakete ausgelassen.

Das folgende Bildschirmabbild zeigt ein Beispiel **Lokalen Link Layer** Spur mit einigen **informativen** Nachrichten in der Spalte **DiagnosisTypes** . Klicken auf ein Symbol in der Spalte **DiagnosisTypes** zeigt die Details der Nachricht. In diesem Beispiel erneut der Server Nachricht #305 übertragen, da es keine Bestätigung vom Client erhalten haben:

![][9]

Wenn Sie die Sitzung Spur in Microsoft Nachricht Analyzer erstellen, können Sie den Filter, um die in der Spur Rauschen verringern angeben. Klicken Sie auf den Link **Konfigurieren** neben **Microsoft-Windows-NDIS-PacketCapture**, auf der **erfassen / verfolgen** Seite, wo Sie die Ablaufverfolgung definieren. Das folgende Bildschirmabbild zeigt eine Konfiguration, die TCP-Datenverkehr für die IP-Adressen der drei Speicherdienste Filter:

![][10]

Weitere Informationen zu den Microsoft Nachricht Analyzer lokalen Link Layer Spur finden Sie unter <a href="http://technet.microsoft.com/library/jj659264.aspx" target="_blank">PEF-NDIS-PacketCapture Anbieter</a> auf TechNet.

### <a name="a-nameappendix-4aappendix-4-using-excel-to-view-metrics-and-log-data"></a><a name="appendix-4"></a>Anlage 4: Verwenden von Excel Kennzahlen anzeigen und dann wieder anmelden Daten

Viele Tools ermöglichen es Ihnen Speicher Kennzahlen Daten aus Azure Table Storage durch Trennzeichen getrennte herunterladen, die die Daten für die Anzeige und Analyse in Excel geladen erleichtert. Speicher Protokollierungsdaten aus Azure Blob-Speicher ist bereits in einer durch Trennzeichen getrennte-Format, das Sie in Excel geladen werden können. Jedoch müssen Sie die entsprechenden Spaltenüberschriften in die Informationen bei <a href="http://msdn.microsoft.com/library/azure/hh343259.aspx" target="_blank">Analytics Log-Speicherformat</a> und <a href="http://msdn.microsoft.com/library/azure/hh343264.aspx" target="_blank">Speicher Analytics Kennzahlen Tabellenschema</a>basiert hinzufügen.

Um die Protokollierung Speicher Daten in Excel importieren, nachdem Sie es von Blob-Speicher heruntergeladen:

- Klicken Sie im Menü **Daten** auf **Aus Text**.
- Navigieren Sie zu der Protokolldatei, die Sie anzeigen, und klicken Sie auf **Importieren**möchten.
- Wählen Sie in Schritt 1 des **Textimport-Assistenten** **getrennt**ein.

Klicken Sie auf Schritt 1 des **Textimport-Assistenten**, **Semikolon** als Trennzeichen nur und wählen Sie als **Textbegrenzungszeichen**doppelte Anführungszeichen. Klicken Sie dann auf **Fertig stellen** , und wählen Sie aus, wo die Daten in der Arbeitsmappe gespeichert.

### <a name="a-nameappendix-5aappendix-5-monitoring-with-application-insights-for-visual-studio-team-services"></a><a name="appendix-5"></a>Anlage 5: Überwachung mit Anwendung Einsichten für Visual Studio Team Services

Sie können auch das Anwendung Einsichten Feature für Visual Studio Team Services als Teil der Leistung und Verfügbarkeit für die Überwachung verwenden. Dieses Tool kann:

- Stellen Sie sicher, dass Ihr Webdienst verfügbar ist und reagiert wird. Ob Ihre app ist eine Website oder ein Gerät-app, die ein Webdienst verwendet, können Sie alle paar Minuten testen Sie Ihre URL Orten auf der ganzen Welt, und können Sie feststellen, ob ein Problem vorliegt.
- Diagnostizieren Sie schnell alle Leistungsprobleme oder Ausnahmen in Ihrem Webdienst. Erfahren Sie, wenn CPU oder andere Ressourcen gestreckt wird werden, Stapel Spuren von Ausnahmen gelangen und Log auf einfache Weise nach. Wenn unter zulässigen Grenzwerten Leistung des app fällt, können wir Sie eine e-Mail-Nachricht senden. Sie können sowohl in .NET Java-Webdiensten überwachen.

AT ist der Zeitpunkt der Erstellung der Anwendung Einsichten in der Vorschau an. Weitere Informationen finden Sie bei <a href="http://msdn.microsoft.com/library/azure/dn481095.aspx" target="_blank">Anwendung Einsichten für Visual Studio Team Services auf MSDN</a>.


<!--Anchors-->
[Einführung]: #introduction
[Wie dieses Handbuch strukturiert ist]: #how-this-guide-is-organized

[Überwachen Ihrer Speicherdienst]: #monitoring-your-storage-service
[Überwachung Dienststatus]: #monitoring-service-health
[Überwachen der Kapazität]: #monitoring-capacity
[Überwachen der Verfügbarkeit]: #monitoring-availability
[Überwachen der Leistung]: #monitoring-performance

[Diagnostizieren von Speicher Probleme]: #diagnosing-storage-issues
[Reaktionsgruppendienst Integrität Probleme]: #service-health-issues
[Leistungsprobleme]: #performance-issues
[Diagnostizieren von Fehlern]: #diagnosing-errors
[Speicher Emulator Probleme]: #storage-emulator-issues
[Tools für Speicher-Protokollierung]: #storage-logging-tools
[Mithilfe der Tools für Netzwerk-Protokollierung]: #using-network-logging-tools

[End-to-End-tracing]: #end-to-end-tracing
[Abgleichen der Log-Daten]: #correlating-log-data
[Client-Anforderung-ID]: #client-request-id
[Server-Anfrage-ID]: #server-request-id
[Zeitstempel]: #timestamps

[Hinweise zur Problembehandlung]: #troubleshooting-guidance
[Kennzahlen anzeigen hohe AverageE2ELatency und niedrig AverageServerLatency]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Kennzahlen anzeigen niedrig AverageE2ELatency und niedrig AverageServerLatency, aber der Client treten Wartezeit]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Kennzahlen anzeigen hoher AverageServerLatency]: #metrics-show-high-AverageServerLatency
[Bei der Nachrichtenübermittlung auf eine Warteschlange unerwartete Verzögerungen auftreten]: #you-are-experiencing-unexpected-delays-in-message-delivery

[Kennzahlen anzeigen eine Steigerung in PercentThrottlingError]: #metrics-show-an-increase-in-PercentThrottlingError
[Vorübergehende Erhöhung der PercentThrottlingError]: #transient-increase-in-PercentThrottlingError
[Permanent erhöhen PercentThrottlingError Fehler]: #permanent-increase-in-PercentThrottlingError
[Kennzahlen anzeigen eine Steigerung in PercentTimeoutError]: #metrics-show-an-increase-in-PercentTimeoutError
[Kennzahlen anzeigen eine Steigerung in PercentNetworkError]: #metrics-show-an-increase-in-PercentNetworkError

[Der Client ist HTTP 403 (Verboten) Nachrichten empfangen.]: #the-client-is-receiving-403-messages
[Der Client ist HTTP 404 (nicht gefunden) Nachrichten empfangen.]: #the-client-is-receiving-404-messages
[Der Client oder einem anderen Prozess gelöscht zuvor auf das Objekt]: #client-previously-deleted-the-object
[Eine freigegebene Access Signatur (SAS) Autorisierung Problem]: #SAS-authorization-issue
[Clientseitige JavaScript-Code verfügt nicht über die Berechtigung zum Zugreifen auf des Objekts]: #JavaScript-code-does-not-have-permission
[Einen Fehler im Netzwerk]: #network-failure
[Der Client ist HTTP 409 (Konflikt) Nachrichten empfangen.]: #the-client-is-receiving-409-messages

[Kennzahlen anzeigen niedrig PercentSuccess oder Analytics Protokolleinträge haben Vorgänge mit Transaktionsstatus der ClientOtherErrors]: #metrics-show-low-percent-success
[Kennzahlen Kapazität Anzeigen einer unerwartete erhöhen in Kapazität speichernutzung]: #capacity-metrics-show-an-unexpected-increase
[Unerwartete nach einem Neustart des virtuellen Computern auftreten, die eine große Anzahl von angefügten virtuellen Festplatten aufweisen]: #you-are-experiencing-unexpected-reboots
[Das Problem tritt auf, verwenden Sie für die Entwicklung oder Test Speicheremulator]: #your-issue-arises-from-using-the-storage-emulator
[Feature "X" funktioniert nicht in der Speicheremulator]: #feature-X-is-not-working
[Fehler "der Wert für eine der Überschriften HTTP ist nicht im richtigen Format" bei Verwendung von Speicheremulator]: #error-HTTP-header-not-correct-format
[Ausführen der Speicheremulator sind Administratorrechte erforderlich]: #storage-emulator-requires-administrative-privileges
[Sie werden bei der Installation der Azure SDK für .NET Probleme auftreten.]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[Sie haben ein anderes Problem mit einer Speicherdienst]: #you-have-a-different-issue-with-a-storage-service

[Anlagen]: #appendices
[Anlage 1: Verwenden von Fiddler, HTTP und HTTPS-Verkehr zu erfassen]: #appendix-1
[Anlage 2: Verwenden von Wireshark zum Netzwerkverkehr erfassen]: #appendix-2
[Anlage 3: Mithilfe von Microsoft Nachricht Analyzer Netzwerkverkehr erfassen]: #appendix-3
[Anlage 4: Verwenden von Excel Kennzahlen anzeigen und dann wieder anmelden Daten]: #appendix-4
[Anlage 5: Überwachung mit Anwendung Einsichten für Visual Studio Team Services]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/overview.png
[2]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/portal-screenshot.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting-classic-portal/mma-screenshot-2.png
