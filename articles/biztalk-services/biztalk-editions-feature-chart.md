<properties
    pageTitle="Erfahren Sie mehr über die Features in BizTalk-Dienste Editionen | Microsoft Azure"
    description="Vergleich die Funktionen der BizTalk-Dienste Editionen: frei, Entwickler, Basic, Standard und Premium. MABS, WABS."
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
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="biztalk-services-editions-chart"></a>BizTalk-Dienste: Editionen Diagramm

Azure BizTalk Services bietet verschiedene Editionen. Lesen Sie diesen Artikel, um zu bestimmen, welche Edition für Ihren Anforderungen Szenario und Business richtig ist.


## <a name="compare-the-editions"></a>Vergleich der Editionen

**Freigeben (Preview)**

Erstellen und Verwalten von Hybrid-Verbindungen können. Eine Verbindung Hybrid ist eine einfache Möglichkeit, eine Verbindung mit einem lokalen System, wie SQL Server eine Azure-Website.

**Entwicklertools**

Enthält Hybrid Verbindungen, EAI und EDI Verarbeitung von Nachrichten mit einer einfach zu verwendendes Handel Partner-Verwaltungsportal und Unterstützung für allgemeine EDI-Schemas und umfassende EDI Verarbeitung über X12 und AS2. Erstellen Sie können allgemeine EAI Szenarien Herstellen einer Verbindung in der Cloud Services mit einem beliebigen HTTP/S, REST, FTP, WCF und SFTP Protokollen zum Lesen und Schreiben von Nachrichten.  Verbindung zu lokalen Branchensystemen mit sofort einsatzbereite SAP, Oracle Ihre Kundentransaktionen, Oracle DB, Siebel und SQL Server-Netzwerkadapter zu nutzen. Verwenden Sie eine orientierte Developer-Umgebung mit Visual Studio-Tools für einfache Entwicklung und Bereitstellung. Klicken Sie auf Entwicklung und Test Zwecke nur mit keine Dienst Ebene Vertrag SERVICELEVEL beschränkt.

**Grundlegende**

Die meisten Funktionen Entwicklertools mit erhöht umfasst Hybrid Verbindungen, EAI Brücken, EDI Agreements und BizTalk Netzwerkadapter Pack Verbindungen. Auch bietet hohe Verfügbarkeit und die Möglichkeit, die mit einem Dienst Ebene Vertrag SERVICELEVEL skalieren.

**Standard**

Hybrid Verbindungen, EAI Brücken, EDI Agreements und BizTalk Netzwerkadapter Pack Verbindungen umfasst alle Grundfunktionen mit erhöht. Auch bietet hohe Verfügbarkeit und die Möglichkeit, die mit einem Dienst Ebene Vertrag SERVICELEVEL skalieren.

**Premium**

Hybrid Verbindungen, EAI Brücken, EDI Agreements und BizTalk Netzwerkadapter Pack Verbindungen umfasst alle Standard-Funktionen mit erhöht. Enthält auch archivieren, hohen Verfügbarkeit und die Möglichkeit, die mit einem Dienst Ebene Vertrag SERVICELEVEL skalieren ein.


## <a name="editions-chart"></a>Editionen-Diagramm
In der folgenden Tabelle werden die Unterschiede aufgeführt.

<table border="1">
<tr bgcolor="FAF9F9">
        <th></th>
        <th>Freigeben (Preview)</th>
        <th>Entwicklertools</th>
        <th>Grundlegende</th>
        <th>Standard</th>
        <th>Premium</th>
</tr>

<tr>
<td><strong>Preis</strong></td>
<td colspan="5"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Preise Azure BizTalk-Dienste</a> <br/><br/> <a HREF="http://azure.microsoft.com/pricing/calculator/?scenario=full">Azure Preise Taschenrechner</a></td>
</tr>
<tr>
<td><strong>Minimale Standard-Konfiguration</strong></td>
<td>1 kostenlosen Einheit</td>
<td>1 Entwicklertools Einheit</td>
<td>1 Grundeinheit</td>
<td>1 Standardeinheit</td>
<td>1 Premium Einheit</td>
</tr>
<tr>
<td><strong>Skalieren</strong></td>
<td>Keine Skalierung</td>
<td>Keine Skalierung</td>
<td>Ja, in Schritten von 1 Grundeinheit</td>
<td>Ja, in Schritten von 1 Standardeinheit</td>
<td>Ja, in Schritten von 1 Premium Einheit</td>
</tr>
<tr>
<td><strong>Maximal zulässige Skalierung</strong></td>
<td>Keine Skalierung</td>
<td>Keine Skalierung</td>
<td>Bis zu 8 Einheiten</td>
<td>Bis zu 8 Einheiten</td>
<td>Bis zu 8 Einheiten</td>
</tr>
<tr>
<td><strong>EAI Brücken pro Einheit</strong></td>
<td>Nicht enthalten</td>
<td>25</td>
<td>25</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>EDI AS2</strong>
<br/><br/>
TPM Vereinbarungen enthält</td>
<td>Nicht enthalten</td>
<td>Enthalten. 10 Vereinbarungen pro Stück.</td>
<td>Enthalten. 50 Vereinbarungen pro Stück.</td>
<td>Enthalten. 250 Vereinbarungen pro Stück.</td>
<td>Enthalten. 1000 Vereinbarungen pro Stück.</td>
</tr>
<tr>
<td><strong>Hybrid Verbindungen pro Einheit</strong></td>
<td>5</td>
<td>5</td>
<td>10</td>
<td>50</td>
<td>100</td>
</tr>
<tr>
<td><strong>Hybrid Verbindung Datenübertragung (GB) pro Einheit</strong></td>
<td>5</td>
<td>5</td>
<td>50</td>
<td>250</td>
<td>500</td>
</tr>
<tr>
<td><strong>Dienst BizTalk Verbindungen mit Branchensystemen lokalen</strong></td>
<td>Nicht enthalten</td>
<td>1 Verbindung</td>
<td>2-Verbindungen</td>
<td>5 Verbindungen</td>
<td>25 Verbindungen</td>
</tr>
<tr>
<td align="left"><strong>Unterstützte Protokolle/Systeme:</strong>
<ul>
<li>HTTP</li>
<li>HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF</li>
<li>Dienstbus (SB)</li>
<li>Azure Blob</li>
<li>REST-APIs</li>
</ul>
</td>
<td>Nicht enthalten</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
</tr>
<tr>
<td><strong>Hohe Verfügbarkeit</strong>
<br/><br/>
Für Dienst Ebene Vertrag SERVICELEVEL, finden Sie unter <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011">Preise zu Azure BizTalk-Dienste</a>.
</td>
<td>Nicht enthalten</td>
<td>Nicht enthalten</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
</tr>
<tr>
<td><strong>Sichern und Wiederherstellen</strong></td>
<td>Nicht enthalten</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
</tr>
<tr>
<td><strong>Verlauf</strong></td>
<td>Nicht enthalten</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
</tr>
<tr>
<td><strong>Archivierung</strong><br/><br/>
Enthält die Nachweisbarkeit Bestätigung (NRR) und nachverfolgte Nachrichten herunterladen</td>
<td>Nicht enthalten</td>
<td>Im Lieferumfang von</td>
<td>Nicht enthalten</td>
<td>Nicht enthalten</td>
<td>Im Lieferumfang von</td>
</tr>
<tr>
<td><strong>Verwenden von benutzerdefiniertem code</strong></td>
<td>Nicht enthalten</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
</tr>
<tr>
<td><strong>Verwendung der können, einschließlich benutzerdefinierter XSLT</strong></td>
<td>Nicht enthalten</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
<td>Im Lieferumfang von</td>
</tr>
</table>

> [AZURE.NOTE] Für Stabilität vor Hardwarefehlern impliziert hohen Verfügbarkeit gibt es mehrere virtuellen Computern in einer einzelnen BizTalk Einheit.


## <a name="faqs"></a>Häufig gestellte Fragen

#### <a name="what-is-a-biztalk-unit"></a>Was ist eine Einheit BizTalk?
"Einheit" ist der atomare Ebene einer Bereitstellung Azure BizTalk-Dienste. Jede Edition verfügt über eine Einheit, die andere berechnen Kapazität und Arbeitsspeicher verfügt. Beispielsweise eine Grundeinheit verfügt über weitere berechnen als Developer, Standard verfügt über mehr als einfache berechnen usw.. Wenn Sie einen BizTalk-Service skalieren, skalieren Sie hinsichtlich Einheiten.

#### <a name="what-is-the-difference-between-biztalk-services-and-azure-biztalk-vm"></a>Was ist der Unterschied zwischen BizTalk-Dienste und Azure BizTalk virtueller Computer?
BizTalk-Dienste bietet eine true-Plattform-as-a-Service (PaaS)-Architektur für die Integration von Lösungen in der Cloud erstellen. Mit dem Modell PaaS Sie vollständig auf die Logik der konzentrieren, und lassen Sie alle Infrastruktur-Management an Microsoft, einschließlich:

- Keine müssen patch virtuellen Computern oder verwalten können.
- Microsoft gewährleistet Verfügbarkeit.
- Sie steuern die Skalierung bei Bedarf per einfach mehr oder weniger Kapazität über das Azure-Portal.

BizTalk Server auf Azure virtuellen Computern stellt eine Infrastruktur-as-a-Service (IaaS) Architektur bereit. Erstellen von virtuellen Computern und konfigurieren sie genau wie Ihre lokalen Umgebung, denen es einfacher zu vorhandenen Programme in der Cloud mit keine Änderungen Code ausführen. Mit IaaS sind Sie weiterhin Konfigurieren der virtuellen Computer, verwalten den virtuellen Computern (z. B. Installieren von Software und Betriebssystem-Patches) und die Anwendung für hohe Verfügbarkeit Architektur verantwortlich.

Wenn Sie erstellen neue Integration Lösungen, die Ihre Infrastruktur Verwaltungsaufwand minimieren betrachtet, verwenden Sie BizTalk-Dienste. Wenn Sie möchten Ihre vorhandenen BizTalk Lösungen schnell migrieren oder suchen Sie nach einer Umgebung bei Bedarf zum Entwickeln und Testen von Applications BizTalk Server, verwenden Sie BizTalk Server auf eine Azure-virtuellen Computern.

#### <a name="what-is-the-difference-between-biztalk-adapter-service-and-hybrid-connections"></a>Was ist der Unterschied zwischen BizTalk Dienst und Hybrid-Verbindungen?
Der Dienst BizTalk wird von einer Azure BizTalk-Dienst verwendet. Der Dienst BizTalk verwendet BizTalk Netzwerkadapter Pack Verbindung zu einem lokalen Zeile des Business BRANCHENSPEZIFISCHE System. Eine Verbindung Hybrid bietet eine einfache und bequeme Möglichkeit, Azure-Anwendungen, wie das Feature Web Apps in Azure-App-Verwaltungsdienst und Azure Mobile-Dienste, um eine lokale Ressource zu verbinden.

#### <a name="what-does-hybrid-connection-data-transfer-gb-per-unit-mean-is-this-per-minutehourdayweekmonth-what-happens-when-the-limit-is-reached"></a>Was bedeutet "Hybrid Verbindung Datenübertragung (GB) pro Einheit"? Handelt es sich um pro Minute/Stunde/Tages-/Wochen-/Monatsansicht? Was geschieht, wenn der Grenzwert erreicht ist?

Der Stückpreis Hybrid Verbindung hängt von der BizTalk Services Edition. Kurz gesagt, Kosten abhängen auf wie viele Daten Sie übertragen. Beispielsweise die Kosten 10 GB täglich Datenübertragung kleiner als 100 GB täglich übertragen. Verwenden Sie die [Preise Rechner](https://azure.microsoft.com/pricing/calculator/?scenario=full) für BizTalk-Dienste, um bestimmte Kosten festzulegen. In der Regel sind die Grenzwerte täglich erzwungen. Wenn Sie den Grenzwert überschreiten, wird jeder Wirkstoffzuschlag mit einer Geschwindigkeit von $1 pro GB berechnet.

#### <a name="when-i-create-an-agreement-in-biztalk-services-why-does-the-number-of-bridges-go-up-by-two-instead-of-just-one"></a>Beim Erstellen einer Vereinbarung BizTalk-Dienste steigen die Anzahl der Brücken warum durch zwei statt nur einem?

Jeder Vertrag besteht aus zwei verschiedenen Brücken: eine senden-Seite Kommunikation Verbindung und eine empfangen Seite Kommunikation Verbindung.

####  <a name="what-happens-when-i-hit-the-quota-limit-on-the-number-of-bridges-or-agreements"></a>Was geschieht, wenn ich das Speicherkontingent für die Anzahl der Brücken oder Vereinbarungen erreicht?

Sie können keine neuen Brücken bereitstellen, oder erstellen eine neue Kaufverträge. Um weitere bereitstellen zu können, müssen Sie Skalieren von mehr Einheiten des Diensts BizTalk, oder aktualisieren Sie auf eine höhere Edition.

#### <a name="how-do-i-migrate-from-one-tier-of-biztalk-services-to-another"></a>Wie migriere ich auf verschiedenen Ebenen der BizTalk-Dienste zu einem anderen?

Die kostenlose Edition kann nicht migriert oder'skaliert nach oben' an eine andere Ebene und kann nicht gesichert und wiederhergestellt werden an eine andere Ebene. Wenn Sie eine weitere Stufe benötigen, erstellen Sie eine neue BizTalk Service mithilfe der neuen Ebene. Alle Elemente, die mit der kostenlosen Edition, einschließlich Hybrid Verbindungen, erstellt in der neuen BizTalk Service neu erstellt werden müssen. 

Für die verbleibenden Editionen sollten die Sicherung und Wiederherstellung für die Migration der Elemente aus einer Ebene in eine andere. Beispielsweise sichern Sie Ihrer Elemente in der Standardansicht Ebene, und klicken Sie dann auf der Ebene Premium wiederherzustellen. [BizTalk Dienstleistungen: Sichern und Wiederherstellen von](biztalk-backup-restore.md) beschreibt die unterstützten Migrationspfade und Listen, welche Elemente gesichert werden sollen. Beachten Sie, dass Hybrid Verbindungen nicht gesichert werden. Nach dem Sichern und wiederherstellen, um eine neue Kategorie, erstellen Sie dann die Verbindungen Hybrid neu.  


#### <a name="is-the-biztalk-adapter-service-included-in-the-service-how-do-i-receive-the-software"></a>Ist der BizTalk Netzwerkadapter-Dienst in den Dienst enthalten? Wie erhalte ich die Software?

Ja, mit dem BizTalk Netzwerkadapter Pack Dienst Netzwerkadapter BizTalk sind in der Azure BizTalk-Dienste SDK [herunterladen](http://www.microsoft.com/download/details.aspx?id=39087).

## <a name="next-steps"></a>Nächste Schritte

Um im Portal Azure Azure BizTalk-Dienste zu erstellen, wechseln Sie zu [BizTalk-Dienste: Bereitstellung über das Azure-Portal](biztalk-provision-services.md). Zum Erstellen von Applications beginnen möchten, wechseln Sie zu [Azure BizTalk-Dienste](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="additional-resources"></a>Zusätzliche Ressourcen
- [BizTalk-Dienste: Bereitstellung über das Azure-portal](biztalk-provision-services.md)<br/>
- [BizTalk-Dienste: Provisioning Statusdiagramm](biztalk-service-state-chart.md)<br/>
- [BizTalk-Dienste: Dashboard, überwachen und Dezimalstellen Registerkarten](biztalk-dashboard-monitor-scale-tabs.md)<br/>
- [BizTalk-Dienste: Sichern und Wiederherstellen](biztalk-backup-restore.md)<br/>
- [BizTalk-Dienste: Begrenzungsebene](biztalk-throttling-thresholds.md)<br/>
- [BizTalk-Dienste: Name des Herausgebers und Herausgeber Schlüssel](biztalk-issuer-name-issuer-key.md)<br/>
- [Wie kann ich mithilfe von starten Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
