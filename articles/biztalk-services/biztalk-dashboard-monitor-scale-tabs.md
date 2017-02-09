<properties 
    pageTitle="Dashboard, Monitor, Skalieren konfigurieren, und Hybrid Verbindungen BizTalk-Dienste | Microsoft Azure" 
    description="Informationen zu den Steuerelementen und Überwachen der Leistung auf der Portalseite Registerkarten für BizTalk-Dienste: Dashboard, Monitor, Dezimalstellen, konfigurieren und Hybrid Verbindungen. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="mandia"/>




# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>Überprüfen Sie die Registerkarten Dashboard, Monitor, Dezimalstellen, konfigurieren und Hybrid-Verbindung

Nachdem Sie Ihre BizTalk Service erstellen und der Anwendung bereitstellen, können Sie einige der BizTalk Service Einstellungen ändern und die Leistung der Anwendung überwachen. 

Wenn Sie das klassische Azure-Portal öffnen, werden Sie automatisch auf die Registerkarte **Alle Elemente** platziert. Klicken Sie zum Anzeigen Ihrer BizTalk Service wählen Sie auf der Registerkarte **Alle Elemente** aus Ihrem BizTalk Service, oder wählen Sie die Registerkarte **BIZTALK-Dienste** ; ein, und wählen Sie dann auf Ihren Namen BizTalk Service.

Daraufhin wird ein neues Fenster mit den folgenden Registerkarten. In diesem Thema werden die folgenden Registerkarten beschrieben.

## <a name="quick-start-quick-startquickstart"></a>Schnellstart (![Schnellstart][QuickStart])
Je nach der BizTalk Services Edition alle aufgeführte Optionen möglicherweise nicht zur Verfügung. 
<table border="1">
    <tr>
        <td><strong>Besorgen Sie sich die tools</strong></td>
        <td>Herunterladen der BizTalk-Dienste SDK, um die Visual Studio-Projektvorlagen auf Ihrem lokalen Computer installieren. Diese Vorlagen erstellen, die <strong>BizTalk-Dienste</strong> (Bridge) und <strong>BizTalk Dienst Elemente</strong> (Transformation) Visual Studio-Projekte, die in Ihrem BizTalk Service bereitgestellt werden.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Wie kann ich mithilfe von starten Azure BizTalk-Dienste SDK</a> , und <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">installieren die Azure BizTalk-Dienste SDK</a> Listet die Schritte zur Seite Erste Schritte auf.
        </td>
    </tr>
    <tr>
        <td><strong>Erstellen von Partner agreements</strong></td>
        <td>Öffnet das Azure BizTalk-Portal auf Azure, wo Sie Partner hinzufügen und Erstellen von X12, AS2, gehostet und EDIFACT EDI Agreements.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurieren von Komponenten für EDI-Messaging BizTalk Services-Portal</a> Listet die Schritte zur Seite Erste Schritte.
        </td>
    </tr>

<tr>
        <td><strong>Erfahren Sie mehr über BizTalk-Dienste</strong></td>
        <td>Wechseln Sie zu der <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">learning Center</a> erfahren Sie mehr über Azure BizTalk-Dienste.</td>
</tr>
</table>


In der Taskleiste am unteren Bildschirmrand können Sie folgende Aktionen ausführen:

<table border="1">

<tr>
<td><strong>Verwalten von</strong> die Bereitstellung Ihrer Anwendung</td>
<td>Öffnet das Azure BizTalk-Portal an. Das BizTalk-Portal ist der Eingang zu EDI-Konfiguration, einschließlich Partner hinzufügen und X12, AS2, erstellen und EDIFACT Agreements.
<br/><br/>
Dies ist <strong>Erstellen Partner Vereinbarungen</strong> entspricht, klicken Sie auf die Registerkarte <strong>Schnellstart</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurieren von Komponenten für EDI-Messaging BizTalk Services-Portal</a> enthält weitere Informationen über das BizTalk-Portal.</td>
</tr>

<tr>
<td><strong>Verbindungsinformationen</strong> des Namespace der Access-Steuerelement</td>
<td>Wenn Sie die Verbindungsinformationen auswählen, werden die Access-Steuerelement Namespace, Standard Herausgeber und Standard-Taste angezeigt. Sie können diese Werte kopieren.
<br/><br/>
Sie können auch das Steuerelement-Portal Access öffnen. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Erstellen eines Access-Steuerelements Namespace</a> enthält weitere Informationen zu der Access-Steuerelement-Portal an.</td>
</tr>

<tr>
<td><strong>Synchronisieren von Tasten</strong> im Speicherkonto</td>
<td>Wenn Sie ein Speicherkonto erstellen, werden einer primären und Sekundärschlüssel automatisch erstellt. Diese Schlüssel für die Verschlüsselung Steuern des Zugriffs auf Ihr Konto Speicher. Ihre BizTalk Service verwendet automatisch den Primärschlüssel. Aktivieren Sie <strong>Synchronisieren Schlüssel</strong> Benutzern den Wechsel zwischen den Primärschlüssel und den Sekundärschlüssel ohne Unterbrechung der BizTalk Service aus.
<br/><br/>
Sie möchten beispielsweise der BizTalk Service einen neuen Primärschlüssel für Speicher-Konto verwendet. Zweck
<br/><br/>
<ol>
<li>Wählen Sie Ihre BizTalk Service, und wählen Sie die <strong>Tasten synchronisieren</strong>. Wählen Sie die sekundäre Taste aus. Wenn Sie dies tun, startet der BizTalk Service mit der sekundäre-Taste.</li>
<li>Im Portal Azure klassischen wählen Sie Ihr Konto Speicher und neu generieren Sie des Primärschlüssels. Denken Sie daran, Ihre BizTalk Service Sekundärschlüssel verwendet wird.</li>
<li>Wählen Sie Ihre BizTalk Service, und wählen Sie die <strong>Tasten synchronisieren</strong>. Wählen Sie jetzt den Primärschlüssel. Dies ist der neuen Primärschlüssel, die Sie erneut generiert.</li>
<li>Im Portal Azure klassischen wählen Sie Ihr Konto Speicher und generieren Sie die sekundäre Taste neu zu.</li>
</ol>
<br/>
Dieses Verfahren wird als "Rollover Tasten" bezeichnet. Der Zweck ist Benutzern den Wechsel zwischen den Primärschlüssel und den Sekundärschlüssel ohne Unterbrechung der BizTalk Service aktivieren.</td>
</tr>

<tr>
<td><strong>Löschen von</strong> Ihrer Anwendung</td>
<td>Bei der Auswahl löschen, Ihre BizTalk Service und alle Elemente, die darauf bereitgestellt werden entfernt.</td>
</tr>
</table>


## <a name="dashboard"></a>Dashboard
Je nach der BizTalk Services Edition alle aufgeführte Optionen möglicherweise nicht zur Verfügung. 

Wenn Sie Ihren Namen BizTalk Service auswählen, wird die Registerkarte Dashboard angezeigt. Im Dashboard können Sie folgende Aktionen ausführen:

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>Übersicht über die Verwendung: Zeigt die Anzahl der verwendeten Hybrid-Verbindungen
Auch zeigt die Verwendung von Daten in GB an. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Metrische Diagramm: Zeigt eine feste Werteliste Leistungswerte
Diese Kennzahlen bieten in Echtzeit Werte in Bezug auf die Integrität des BizTalk Services. Sie können auch auswählen, die **Relative** oder **Absolute** Werte und des Zeitraums **Intervall** für die Metrik, die im Diagramm angezeigt werden. 

Eine Beschreibung dieser Performance-Werte finden Sie unter [Verfügbare statistische Werte](#Metrics) in diesem Thema.


##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Den ersten Blick: Listen Ihrer BizTalk Service-Eigenschaften

<table border="1">

<tr>
<td><strong>Überwachung Datenbankbenutzers aktualisieren</strong></td>
<td>Benutzername und Kennwort zum Anmelden bei der Datenbank zum Nachverfolgen von Änderungen.</td>
</tr>
<tr>
<td><strong>SSL-Zertifikat aktualisieren</strong></td>
<td>Aktualisieren Sie der BizTalk Service um ein anderes SSL-Zertifikat verwenden können. Ein selbst signiertes Zertifikat für SSL wird automatisch erstellt, wenn Sie <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">den BizTalk-Dienst erstellen</a>.</td>
</tr>
<tr>
<td><strong>Zertifikat herunterladen</strong></td>
<td>Sie können das SSL-Zertifikat durch Ihre BizTalk Service auf einem lokalen Computer herunterladen.</td>
</tr>
<tr>
<td><strong>Status</strong></td>
<td>Zeigt den aktuellen Status des BizTalk Service an. Finden Sie unter <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk-Dienste: Dienst Zustand Diagramm</a>. </td>
</tr>
<tr>
<td><strong>Dienst-URL</strong></td>
<td>Die URL für Ihre BizTalk-Dienst. Dies ist die gleiche wie die <strong>Domänen-URL</strong> eingegeben, wenn Ihre BizTalk Service erstellt wird.</td>
</tr>
<tr>
<td><strong>Öffentliche virtuelle IP-Adresse (VIP) Adresse</strong></td>
<td>Die IP-Adresse Ihre BizTalk Service zugeordnet ist. Es wird für alle Eingabewerte Endpunkte verwendet und die Quelladresse für ausgehenden Datenverkehr. Diese IP-Adresse gehört in Ihrem BizTalk Service, solange sie erstellt wurde. Wenn Sie die BizTalk Service löschen, wird die IP-Adresse in eine andere BizTalk Service zugewiesen.</td>
</tr>
<tr>
<td><strong>ACS-Namespace</strong></td>
<td>Mit dem Dienst BizTalk authentifiziert.</td>
</tr>
<tr>
<td><strong>Edition</strong></td>
<td>Listen eingegeben die Edition an, wenn der BizTalk Service erstellt wird.</td>
</tr>
<tr>
<td><strong>Speicherort</strong></td>
<td>Zeigt das geografische Region, das Ihre BizTalk Service hostet.</td>
</tr>
<tr>
<td><strong>Erstellt</strong></td>
<td>Zeigt das Datum und die Uhrzeit, zu der BizTalk Service erstellt wurde.</td>
</tr>
<tr>
<td><strong>Nachverfolgen der Datenbank</strong></td>
<td>Der Name der Azure SQL-Datenbank, in dem Ihre BizTalk Service untersuchten Verlauf Tabellen gespeichert. 
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Erläuterung Anforderungen</a> enthält Details der Datenbank nachverfolgen.</td>
</tr>
<tr>
<td><strong>Überwachung/Archivierung Speicher</strong></td>
<td>Die Namen der Azure-Speicher, in dem die Überwachung Ausgabe des BizTalk Service gespeichert.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Erläuterung der Anforderungen</a> enthält Details im Speicher-Konto.</td>
</tr>
<tr>
<td><strong>Namen des Abonnements.</strong></td>
<td>Listen das Abonnement, das Ihre BizTalk Service hostet. Das Abonnement regelt klassische Azure-Portal zugreifen.</td>
</tr>
<tr>
<td><strong>Abonnement-ID</strong></td>
<td>Wenn Sie ein Abonnement erstellt wurde, wird automatisch eine Abonnement-ID generiert. Abschluss REST-APIs verwenden zu können, müssen Sie möglicherweise die Abonnement-ID eingeben.</td>
</tr>
</table>

[BizTalk-Dienste: Provisioning klassischen mithilfe von Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) Listet die Schritte zum Erstellen einer BizTalk Service.


##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>Verwalten von Verbindungsinformationen, synchronisieren-Taste, und löschen Sie in der Taskleiste:

<table border="1">

<tr>
<td><strong>Verwalten von</strong> die Bereitstellung Ihrer Anwendung</td>
<td>Öffnet das Azure BizTalk-Portal an. Das BizTalk-Portal ist der Eingang zu EDI-Konfiguration, einschließlich Partner hinzufügen und X12, AS2, erstellen und EDIFACT Agreements.
<br/><br/>
Dies ist <strong>Erstellen Partner Vereinbarungen</strong> entspricht, klicken Sie auf die Registerkarte <strong>Schnellstart</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurieren von Komponenten für EDI-Messaging BizTalk Services-Portal</a> enthält weitere Informationen über das BizTalk-Portal.</td>
</tr>
<tr>
<td><strong>Verbindungsinformationen</strong> des Namespace der Access-Steuerelement</td>
<td>Zeigt die Access-Steuerelement Namespace, Herausgeber Standard und Standard Schlüsselwerte; die kopiert werden können.
<br/><br/>
Sie können auch das Steuerelement-Portal Access öffnen. Dieses Steuerelement-Portal zugreifen entspricht dem verwenden im linken Navigationsbereich der Option Active Directory.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Verwalten Ihrer ACS-Namespace</a> enthält weitere Informationen zu der Access-Steuerelement-Portal an.</td>
</tr>
<tr>
<td><strong>Synchronisieren von Tasten</strong> im Speicherkonto</td>
<td>Wenn Sie ein Speicherkonto erstellen, werden einer primären und Sekundärschlüssel automatisch erstellt. Diese Schlüssel für die Verschlüsselung Steuern des Zugriffs auf Ihr Konto Speicher. Ihre BizTalk Service verwendet automatisch den Primärschlüssel. Aktivieren Sie <strong>Synchronisieren Schlüssel</strong> Benutzern den Wechsel zwischen den Primärschlüssel und den Sekundärschlüssel ohne Unterbrechung der BizTalk Service aus.
<br/><br/>
Sie möchten beispielsweise der BizTalk Service einen neuen Primärschlüssel für Speicher-Konto verwendet. Zweck
<br/><br/>
<ol>
<li>Wählen Sie Ihre BizTalk Service, und wählen Sie die <strong>Tasten synchronisieren</strong>. Wählen Sie die sekundäre Taste aus. Wenn Sie dies tun, startet der BizTalk Service mit der sekundäre-Taste.</li>
<li>Im Portal Azure klassischen wählen Sie Ihr Konto Speicher und neu generieren Sie des Primärschlüssels. Denken Sie daran, Ihre BizTalk Service Sekundärschlüssel verwendet wird.</li>
<li>Wählen Sie Ihre BizTalk Service, und wählen Sie die <strong>Tasten synchronisieren</strong>. Wählen Sie jetzt den Primärschlüssel. Dies ist der neuen Primärschlüssel, die Sie erneut generiert.</li>
<li>Im Portal Azure klassischen wählen Sie Ihr Konto Speicher und generieren Sie die sekundäre Taste neu zu.</li>
</ol>
<br/>
Dieses Verfahren wird als "Rollover Tasten" bezeichnet. Der Zweck ist Benutzern den Wechsel zwischen den Primärschlüssel und den Sekundärschlüssel ohne Unterbrechung der BizTalk Service aktivieren.</td>
</tr>

<tr>
<td><strong>Löschen von</strong> Ihrer Anwendung</td>
<td>Ihre BizTalk Service und alle Elemente, die darauf bereitgestellt werden entfernt.</td>
</tr>
</table>


## <a name="monitor"></a>Monitor
Gilt nicht für die kostenlose Edition.

Wenn Sie Ihren Namen BizTalk Service auswählen, wird die Registerkarte Monitor steht zur Verfügung und zeigt die folgende Meldung an:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>Metrische Graph: Zeigt die ausgewählten Performance-Werte
Diese Kennzahlen bieten in Echtzeit Werte in Bezug auf die Integrität des BizTalk Services. Sie auswählen, welche Performance-Werte angezeigt werden. Maximal sechs Leistungswerte kann gleichzeitig angezeigt werden. 

Sie können auch auswählen, die **Relative** oder **Absolute** Werte und des Zeitraums **Intervall** für die Metrik, die angezeigt werden. 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>Entfernen oder Kennzahlen im Diagramm anzuzeigen:
1. Wählen Sie die Registerkarte **Monitor** aus.
2. Wählen Sie in der Taskleiste **Kennzahlen hinzufügen** :  
![Wählen Sie Kennzahlen hinzufügen][AddMetrics]
3. Überprüfen Sie die Performance-Werte, die angezeigt werden soll.
4. Wählen Sie das Kontrollkästchen, um zur Registerkarte **Monitor** zurückzukehren.
5. Wählen Sie den Kreis neben der Metrik die Metrik des Werts im Diagramm angezeigt werden.  

    Beispielsweise wird die **CPU-Auslastung** Metrik abgeblendet; die Ausgabe wird im Diagramm nicht angezeigt:  
![CPU-Auslastung Metrisch ist abgeblendet.][GrayedMetric]  

    Wählen Sie die abgeblendet, Kreis, aktivieren Sie die **CPU-Auslastung** Metrik deren Ausgabe im Diagramm angezeigt werden:  
![CPU-Auslastung Metrisch aktiviert ist][EnabledMetric]

6. Wählen Sie zum Entfernen einer Metrik aus der Liste und das Diagramm anzeigen in der Taskleiste **Metrisch löschen** aus. Um die metrischen zurück zur Liste hinzuzufügen, wählen Sie in der Taskleiste **Kennzahlen hinzufügen** , aktivieren Sie die Metrik, und wählen Sie das Kontrollkästchen, um zur Registerkarte **Monitor** zurückzukehren. Wählen Sie die abgeblendet, Kreis, um die Metrik aktivieren.

## <a name="a-namemetricsaavailable-metrics"></a><a name="Metrics"></a>Verfügbare statistische Werte
Performance-Indikatoren/Werte sind verfügbar:

<table border="1">

<tr>
<td><strong>RountdTrip Wartezeit</strong></td>
<td>Zeigt die durchschnittliche Verarbeitungszeit in Millisekunden (ms) für eine Nachricht ab dem Zeitpunkt an, dass die Nachricht eingeht, bis die Nachricht von der BizTalk Service vollständig verarbeitet wird über alle Brücken. Nur Nachrichten erfolgreich verarbeitet werden berücksichtigt.<br/><br/>
Wenn die folgenden Ereignisse auftreten, wird ein Zeitstempel erstellt:
<ul>
<li>Meldung Eintritt des Gateways</li>
<li>Nachricht wird an das Ziel weitergeleitet.</li>
<li>Ziel Antwort empfangen wird</li>
<li>Ziel Bestätigung Antwort an das Gateway gesendet werden</li>
</ul>
<br/>
Diese Metrik zeigt das Ergebnis der folgenden Berechnung:
<br/><br/>
[Ziel Bestätigung Antwort gesendet, um das Gateway] - [Meldung Eintritt des Gateways]</td>
</tr>
<tr>
<td><strong>Fehler bei der Quelle</strong></td>
<td>Zeigt die Gesamtzahl der Nachrichten, die nicht von der BizTalk Service, wenn Nachrichten aus der Quelle Endpunkte ziehen.</td>
</tr>
<tr>
<td><strong>CPU-Auslastung</strong></td>
<td>Listet die durchschnittliche % CPU-Zeit aller Instanzen der Rolle.</td>
</tr>
<tr>
<td><strong>Verarbeitung Wartezeit</strong></td>
<td>Zeigt die durchschnittliche Zeit In Millisekunden (ms) zum Verarbeiten einer Nachricht, die BizTalk Service über alle Brücken, den Zeitaufwand in Ziele ausschließen. Nur Nachrichten erfolgreich verarbeitet werden berücksichtigt.<br/><br/>
Wenn der folgenden Ereignisse ausgeführt werden, wird ein Zeitstempel erstellt:

<ul>
<li>Meldung Eintritt des Gateways</li>
<li>Nachricht wird an das Ziel weitergeleitet.</li>
<li>Ziel Antwort empfangen wird</li>
<li>Ziel Bestätigung Antwort an das Gateway gesendet werden</li>
</ul>
<br/>Diese Metrik zeigt das Ergebnis der folgenden Berechnung:<br/><br/>
[Ziel Bestätigung Antwort gesendet, um das Gateway] - [Meldung Eintritt des Gateways] - [Ziel Antwort erfolgt] + [Nachricht an das Ziel weitergeleitet wird]</td>
</tr>
<tr>
<td><strong>Fehler im Prozess</strong></td>
<td>Zeigt die Gesamtzahl der Nachrichten, die während der Verarbeitung durch den BizTalk Service über alle Brücken innerhalb eines Zeitintervalls konnten.</td>
</tr>
<tr>
<td><strong>Gesendete Nachrichten</strong></td>
<td>Zeigt die Gesamtzahl der Nachrichten in einem Zeitraum von der BizTalk Service über alle Brücken gesendet. Diese Metrik wird erhöht, wenn eine Nachricht von einer Verkaufspipeline gesendeten Routing Ziel erreicht. Diese Metrik wird nicht angegeben, dass eine Nachricht erfolgreich verarbeitet wird.<br/><br/>
In einem Szenario Anforderung-Antwort wird die Metrik erhöht, wenn das Ziel Routing eine Bestätigung Bestätigung wieder in der Verkaufspipeline sendet.</td>
</tr>
<tr>
<td><strong>Empfangene Nachrichten</strong></td>
<td>Zeigt die Gesamtzahl der Nachrichten, die durch die BizTalk Service über alle Brücken innerhalb eines Zeitintervalls empfangen. Wenn eine neue Nachricht bei der Verkaufspipeline eingeht, wird diese Metrik erhöht.</td>
</tr>
<tr>
<td><strong>Nachrichten In Bearbeitung</strong></td>
<td>Zeigt die Gesamtzahl der Nachrichten, die derzeit in einem Zeitraum von der BizTalk Service verarbeitet werden.</td>
</tr>
<tr>
<td><strong>Nachrichten verarbeitet</strong></td>
<td>Zeigt die Gesamtzahl der Nachrichten, die erfolgreich verarbeiteten der BizTalk Service über alle Brücken innerhalb eines Zeitintervalls. Wenn eine Nachricht von der Verkaufspipeline erfolgreich empfangen und erfolgreich an das Ziel weitergeleitet wird, wird diese Metrik erhöht.</td>
</tr>
</table>


## <a name="scale"></a>Skalieren
Auf der Registerkarte Skalierung können Sie addieren oder Subtrahieren von der Anzahl der Einheiten, die von Ihrer BizTalk Service verwendet. Standardmäßig ist es eine, Einheit konfiguriert wurde. Zusätzliche Einheiten können Ihre BizTalk Service skalieren hinzugefügt werden. Wenn Sie den Maßstab erhöhen, erhöhen Durchsatz. Wie viele Ressourcen erhöht auch, einschließlich bereitgestellten Brücken, Vereinbarungen, LOB Verbindungen und Verarbeitung Power. Angenommen, erhöhen Sie die Skala von 1 Einheit bis 2 Einheiten. In diesem Fall können Sie doppelt so viele Brücken bereitstellen, die Vereinbarungen doppelklicken, LOB Verbindungen doppelklicken und die Potenz Verarbeitung doppelklicken.

Für einige Editionen BizTalk bieten eine geeignete Skalierungsoption aus. In diesem Fall ist eine Einheit zulässig. Wenn Sie ermitteln möchten, wie viele Einheiten Ihre Edition skaliert werden kann, finden Sie in [BizTalk-Dienste: Editionen Diagramm](biztalk-editions-feature-chart.md).

Erhöhen der Anzahl der Einheiten kann sich Preise auswirken. Wenn Sie die Einheiten erhöhen, zeigt auswählen, **Speichern** eine Nachricht, die die Meldung angezeigt wird, wenn der Abrechnung beeinträchtigt wird. Wählen Sie dann auf Weiter. Wenn Sie die Anzahl der Einheiten, die BizTalk Service Status Änderungen von aktiv Aktualisierung erhöhen. In den Status aktualisieren wird Ihre BizTalk Service weiterhin ausgeführt.

[BizTalk-Dienste: Editionen Diagramm](biztalk-editions-feature-chart.md) "Einheit" definiert.


## <a name="configure"></a>Konfigurieren
Gilt nicht für Hybrid Verbindungen.

Legt den Sicherung Status keine oder automatische. Wenn keine festgelegt, die keine Sicherungskopien automatisch erstellt werden. Beim Festlegen auf automatisch, und konfigurieren Sie den Sicherungsspeicherort, die Häufigkeit der Sicherung, und wie lange die Sicherungsdateien beibehalten. 

[BizTalk Dienstleistungen: Sichern und Wiederherstellen von](biztalk-backup-restore.md) enthält die Details. 


## <a name="a-namehybridconnectionsahybrid-connections"></a><a name="HybridConnections"></a>Hybrid-Verbindungen
Hybrid Verbindungen verbinden eine Azure-Anwendung wie Web Apps oder Mobile-Apps in Azure-App-Dienst, mit einer lokalen Ressource, die eine statische TCP-, wie etwa SQL Server, MySQL, HTTP-Web-APIs und die meisten benutzerdefinierten Webdienste verwendet. Hybrid-Verbindungen werden in der klassischen Azure-Portal BizTalk Dienste verwaltet.

Hybrid Verbindungen in Azure-App-Verwaltungsdienst erstellen zu können, finden Sie unter [Zugriff auf lokale Ressourcen Hybrid Verbindungen in Azure-App-Dienst verwenden](../app-service-web/web-sites-hybrid-connection-get-started.md).

Zum Erstellen oder verwalten Hybrid Verbindungen Azure BizTalk-Dienste finden Sie unter [Hybrid Verbindungen](integration-hybrid-connection-overview.md).



## <a name="next"></a>Weiter
Jetzt, da Sie die anderen Registerkarten vertraut sind, können Sie weitere Informationen zur Azure BizTalk Services-Features:

- [BizTalk-Dienste: Begrenzungsebene](biztalk-throttling-thresholds.md)  
- [BizTalk-Dienste: Name des Herausgebers und Herausgeber Schlüssel](biztalk-issuer-name-issuer-key.md)  
- [BizTalk-Dienste: Sichern und Wiederherstellen](biztalk-backup-restore.md)

## <a name="see-also"></a>Siehe auch
- [Hybrid-Verbindungen](integration-hybrid-connection-overview.md)  
- [BizTalk-Dienste: Entwickler, Basic, Standard und Premium-Editionen Diagramm](biztalk-editions-feature-chart.md)  
- [BizTalk-Dienste: Provisioning klassischen mithilfe von Azure-portal](biztalk-provision-services.md)  
- [BizTalk-Dienste: BizTalk-Dienststatus-Diagramm](biztalk-service-state-chart.md)  
- [Wie kann ich mithilfe von starten Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[QuickStart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png
 
