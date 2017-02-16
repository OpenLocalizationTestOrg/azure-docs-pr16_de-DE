<properties
    pageTitle="DocumentDB Speicherplatz und Leistung | Microsoft Azure" 
    description="Informationen Sie zu Datenspeicher und Speicherung von Dokumenten in DocumentDB und wie DocumentDB Kapazität Anforderungen Ihrer Anwendung skaliert werden kann." 
    keywords="Speicherung von Dokumenten"
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="syamk"/>

# <a name="learn-about-storage-and-predictable-performance-provisioning-in-documentdb"></a>Erfahren Sie mehr über die Speicherung und vorhersehbar Leistung in DocumentDB bereitgestellt
Azure DocumentDB ist ein vollständig verwaltete, skalierbare dokumentorientierten NoSQL-Datenbank-Dienst für JSON-Dokumente. Mit DocumentDB verfügen Sie möglicherweise nicht auf virtuellen Computern vermieten, Software bereitstellen oder Datenbanken überwachen. DocumentDB wird betrieben und von Microsoft-Experten weltweit Verfügbarkeit, Leistung und Datenschutz vorführen fortlaufend überwacht.  

Sie können mit DocumentDB durch [Erstellen eines Kontos Datenbank](documentdb-create-account.md) und einer [DocumentDB Datenbank](documentdb-create-database.md) über das [Azure-Portal](https://portal.azure.com/)loslegen. DocumentDB Datenbanken sind in Maßeinheiten State Drive (SSD) gesicherten Speicher und Durchsatz zur Verfügung. Diese Speichereinheiten werden durch [erstellen Datenbank Websitesammlungen](documentdb-create-collection.md) in Ihrer Datenbankkonto jede Websitesammlung mit reservierte Durchsatz bereitgestellt, die nach oben oder nach unten zu einem beliebigen Zeitpunkt zu entsprechen den Bedarf Ihrer Anwendung skaliert werden kann. 

Wenn die Anwendung Ihre reservierten Durchsatz für eine oder mehrere Websitesammlungen überschreitet, werden Anfragen auf Basis pro Websitesammlung eingeschränkt. Dies bedeutet, dass einige Anwendung erfolgreich durchgeführt werden kann, während andere gedrosselt werden können.

Dieser Artikel enthält eine Übersicht über die Ressourcen und Kennzahlen Kapazität verwalten und planen die Speicherung von Daten zur Verfügung. 

## <a name="database-account"></a>Datenbankkonto
Als Azure-Abonnent können Sie eine oder mehrere DocumentDB Datenbankkonten zum Verwalten Ihrer Datenbankressourcen bereitstellen. Jedes Abonnement, die ein einzelnes Azure-Abonnement zugeordnet ist. 

DocumentDB Konten können über das [Azure-Portal](documentdb-create-account.md)oder mithilfe [einer Vorlage Cloud oder Azure CLI](documentdb-automation-resource-manager-cli.md)erstellt werden.

## <a name="databases"></a>Datenbanken
Eine einzelne DocumentDB Datenbank kann eine unbegrenzte Menge Speicherung von Dokumenten in Websitesammlungen gruppiert praktisch enthalten. Websitesammlungen bieten Isolation Performance: Jede Websitesammlung mit Durchsatz, die nicht für andere Websitesammlungen in der gleichen Datenbank oder Konto freigegeben werden bereitgestellt werden kann. Eine DocumentDB Datenbank ist in der Größe von GB bis hin zu TB SSD gesicherten Speicherung von Dokumenten und bereitgestellte Durchsatz flexible. Im Gegensatz zu einem herkömmlichen RDBMS-Datenbank eine Datenbank in DocumentDB ist nicht mit einem einzelnen Computer ausgelegte und mehreren Computern oder Cluster umfassen kann.  

Mit DocumentDB wie Sie benötigen, um Ihre Anwendungen skalieren können Sie mehrere Websitesammlungen und/oder Datenbanken erstellen. Datenbanken können über das [Azure-Portal](documentdb-create-database.md) oder über eine [DocumentDB SDKs](documentdb-dotnet-samples.md)erstellt werden.   

## <a name="database-collections"></a>Datenbanksammlungen
Jede DocumentDB Datenbank kann eine oder mehrere Websitesammlungen enthalten. Websitesammlungen dienen als hoch verfügbare Datenpartitionen Dokument gespeichert und verarbeitet. Jede Websitesammlung kann Dokumente mit heterogenen Schema zu speichern. DocumentDB des automatischen Indizierung und Abfragefunktionen können Sie auf einfache Weise filtern und Abrufen von Dokumenten. Eine Auflistung stellt den Bereich für die Ausführung von Dokument-Speicher und die Abfrage an. Eine Auflistung ist ebenfalls eine Domäne Transaktion für alle darin enthaltenen Dokumente. Websitesammlungen werden basierend auf dem Wert festlegen der Azure-Portal oder über den SDKs Durchsatz zugewiesen. 

Websitesammlungen werden automatisch durch DocumentDB in einem oder mehreren physischen Servern aufgeteilt. Wenn Sie eine Websitesammlung erstellen, können Sie den bereitgestellten Durchsatz in Bezug auf die Anfrage Einheiten pro Sekunde und eine zentrale Partition-Eigenschaft angeben. Der Wert dieser Eigenschaft wird von DocumentDB zum Verteilen von Dokumenten zwischen Partitionen und Weiterleiten von Besprechungsanfragen wie Abfragen. Der wichtigsten Partitionswert fungiert auch als die Begrenzungslinie Transaktion für gespeicherte Prozeduren und Trigger. Jede Websitesammlung hat eine reservierte Durchsatz speziell für die Websitesammlung, die nicht gemeinsam mit anderen Websitesammlungen in demselben Konto verwendet wird. Daher können Sie Ihrer Anwendung in Bezug auf die Speicherung und Durchsatz geeignet. 

Websitesammlungen können über das [Azure-Portal](documentdb-create-collection.md) oder über eine [DocumentDB SDKs](documentdb-sdk-dotnet.md)erstellt werden.   
 
## <a name="request-units-and-database-operations"></a>Anfordern von Einheiten und Datenbankvorgänge

Wenn Sie eine Websitesammlung erstellen, reservieren Sie Durchsatz in Bezug auf die [Anfrage Einheiten (RU)](documentdb-request-units.md) pro Sekunde an. Stattdessen nachdenkt und Verwalten von Hardware-Ressourcen, Sie können eine **Anfrage Einheit** als ein einzelnes Measure für die Ressourcen vorstellen verschiedene Datenbankvorgänge durchführen und eine Anwendung Anforderung service erforderlich. Lesen eines Dokuments 1 KB verbraucht derselben 1 RU unabhängig von der Anzahl von Elementen in der Auflistung oder die Anzahl der gleichzeitige Anforderungen gleichzeitig ausgeführt gespeichert. Alle Anfragen gegen DocumentDB, einschließlich komplexe Vorgänge aus, wie SQL-Abfragen vorhersehbaren RU Wert aufweisen, der zum Zeitpunkt der Entwicklung ermittelt werden können. Wenn Sie wissen, dass die Größe Ihrer Dokumente und die Häufigkeit der einzelnen Vorgänge (Lese-, schreibt und Abfragen), um für eine Anwendung zu unterstützen, können Sie den genauen Betrag des Durchsatzes an Ihrer Anwendung Anforderungen bereitstellen und Skalieren Ihrer Datenbank nach oben oder unten, Ihre Veranstaltung Anforderungen ändern. 

Mit Durchsatz in Blöcken mit 100 Anforderung Einheiten pro Sekunde, von bis zu mehreren Millionen Anforderung Einheiten pro Sekunde Hunderte kann jede Websitesammlung reserviert werden. Der bereitgestellte Durchsatz kann angepasst werden, während der gesamten Dauer einer Websitesammlung, die sich ändernden Verarbeitung Bedürfnissen anzupassen und zu Muster der Anwendung zugreifen. Weitere Informationen finden Sie unter [DocumentDB Leistung Ebenen](documentdb-performance-levels.md). 

>[AZURE.IMPORTANT] Websitesammlungen sind berechenbaren Einheiten. Die Kosten wird durch den bereitgestellte Durchsatz der Sammlung gemessen in einer anderen Einheit Anforderung pro Sekunde sowie die gesamte belegte Speicher in Gigabyte bestimmt. 

Wie viele Anforderung Einheiten wird eine bestimmte Operation wie einfügen, löschen, Abfrage- oder Ausführung der gespeicherten Prozedur nutzen? Eine Anforderung Einheit ist ein standardisierten Maß für die Verarbeitung von Kosten Anforderung an. Lesen eines Dokuments 1 KB ist 1 RU, sondern nur eine Anforderung einfügen, ersetzen oder Löschen von im selben Dokument werden weitere Verarbeitung vom Dienst und dadurch auch weitere Anforderung Einheiten nutzen. Jede Antwort des Diensts enthält einen benutzerdefinierten Header (`x-ms-request-charge`), die die Anforderung Einheiten für die Anforderung verbraucht Berichte. Diese Kopfzeile wird auch über die [SDKs](documentdb-sdk-dotnet.md)zugegriffen werden kann. In .NET SDK ist [RequestCharge](https://msdn.microsoft.com/library/azure/dn933057.aspx#P:Microsoft.Azure.Documents.Client.ResourceResponse`1.RequestCharge) eine Eigenschaft des Objekts [ResourceResponse](https://msdn.microsoft.com/library/azure/dn799209.aspx) . Wenn Sie Ihren Anforderungen Durchsatz, bevor Sie einzelne anrufen schätzen möchten, können Sie der [Kapazität können](documentdb-request-units.md#estimating-throughput-needs) , die mit diesem Abschätzung helfen verwenden. 

>[AZURE.NOTE] Eine einfache Abrufen des Dokuments mit [Sitzung Konsistenz](documentdb-consistency-levels.md)entspricht der Basisplan 1 Anforderung Einheit für ein Dokument 1 KB. 

Es gibt verschiedene Faktoren, die die Anforderung Einheiten für einen Vorgang mit einem DocumentDB Datenbank-Konto verbraucht auswirken. Diese Faktoren umfassen:

- Dokumentgröße. Wenn Dokumentgrößen die Einheiten verbraucht zum Lesen und schreiben, dass die Daten auch vergrößert werden erhöhen.
- Eigenschaftenanzahl. Unter Annahme Standard Indizierung aller Eigenschaften, die Einheiten, die zum Schreiben eines Dokuments verbraucht erhöht zunehmender Anzahl Eigenschaft.
- Konsistenz der Daten. Bei Verwendung von Daten Konsistenz Ebenen starken oder Veraltung begrenzt werden zusätzliche Einheiten verbraucht werden, um Dokumente zu lesen.
- Indizierte Eigenschaften. Eine Richtlinie Index für jede Auflistung bestimmt, welche Eigenschaften standardmäßig indiziert sind. Sie können Ihre Anforderung Einheitenverbrauch reduzieren, indem Sie begrenzen der Anzahl von indizierten Eigenschaften. 
- Dokument mit einem Index. Standardmäßig jedes Dokument automatisch indiziert ist, werden Sie weniger Anforderung Einheiten nutzen, wenn Sie nicht indizieren einige Ihrer Dokumente.

Weitere Informationen finden Sie unter [DocumentDB Anforderung Einheiten](documentdb-request-units.md). 

Hier beträgt beispielsweise eine Tabelle mit wie vielen Anforderung Einheiten Bereitstellen in drei anderen Dokumentgrößen (1 KB, 4 KB und 64 KB) und auf zwei verschiedenen Leistung Ebenen (500 "Lesezugriff/Sekunde" + 100 Schreibzugriff/Sekunde und 500 "Lesezugriff/Sekunde" + 500 "Schreibzugriff/Sekunde"). Die Datenkonsistenz in Sitzung konfiguriert wurde, und die Indizierung Richtlinie keine festgelegt wurde.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Dokumentgröße</strong></p></td>
            <td valign="top"><p><strong>Lesezugriff/Sekunde</strong></p></td>
            <td valign="top"><p><strong>Schreibzugriff/Sekunde</strong></p></td>
            <td valign="top"><p><strong>Anfordern von Einheiten</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1) + (100* 5) = 1.000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *5) + (100* 5) 3.000 RU/s =</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1.3) + (100* 7) 1,350 RU/s =</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *1.3) + (500* 7) 4,150 RU/s =</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *10) + (100* 48) 9,800 RU/s =</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *10) + (500* 48) 29,000 RU/s =</p></td>
        </tr>
    </tbody>
</table>

Nutzen Abfragen, gespeicherten Prozeduren und Triggern Anforderung Einheiten ausgehend von der Komplexität der Vorgänge durchgeführt werden. Bei der Entwicklung Ihrer Anwendung, prüfen Sie die Anfrage Gebühr Kopfzeile zum besseren Verständnis wie jede Operation Anforderung Einheit Kapazität in Anspruch nimmt.  


## <a name="choice-of-consistency-level-and-throughput"></a>Wahl der Konsistenz Ebene und Durchsatz
Die Wahl der Konsistenz Standardstufe wirkt sich auf den Durchsatz und Wartezeit. Sie können die Konsistenz Standardstufe programmgesteuert und über das Portal Azure festlegen. Sie können auch die Ebene Konsistenz auf jeden Anforderung außer Kraft setzen. Standardmäßig ist die Konsistenz Ebene an die **Sitzung**, die monotonen gelesen/schreibt bereitstellt festlegen und Lesen Ihrer Garantien schreiben. Sitzung Konsistenz eignet sich hervorragend für Benutzer reduzierte Applikationen und bietet eine perfekte Mischung aus Konsistenz und Leistung vor-und Nachteile.    

Anweisungen zum Ändern der Ebene Konsistenz Azure-Portal finden Sie unter [Verwalten von einem Konto DocumentDB](documentdb-manage-account.md#consistency). Oder Weitere Informationen Konsistenz Ebenen, finden Sie unter [verwenden Konsistenz Ebenen](documentdb-consistency-levels.md).

## <a name="provisioned-document-storage-and-index-overhead"></a>Bereitgestellte Dokument Speicher und Index Verwaltungsaufwand
DocumentDB unterstützt die Erstellung einer Partition und partitionierten Websitesammlungen. Jede Partition in DocumentDB unterstützt bis zu 10 GB SSD gesicherten Speicher. Die 10GB Speicher Dokument enthält die Dokumente plus Speicherplatz für den Index. Standardmäßig wird eine Auflistung DocumentDB konfiguriert automatisch alle Dokumente ohne explizit alle sekundären Indizes oder Schema indizieren. Ausgehend von Applications DocumentDB verwenden, ist der Aufwand typische Index zwischen 2 bis 20 %. Die Indizierung Technologie untersuchten DocumentDB ist sichergestellt, dass der Index Aufwand unabhängig von den Werten der Eigenschaften, nicht mehr als 80 % der Größe der Dokumente mit Standardeinstellungen überschreitet. 

Standardmäßig werden alle Dokumente automatisch anhand DocumentDB indiziert. Wenn Sie den Index Aufwand optimieren möchten, können Sie jedoch so entfernen Sie bestimmte Dokumente aus, die zum Zeitpunkt der einfügen oder Ersetzen eines Dokuments, indiziert werden, wie unter [DocumentDB Indizierung Richtlinien](documentdb-indexing-policies.md)auswählen. Sie können eine Auflistung DocumentDB, um alle Dokumente in der Sammlung von der Indizierung ausschließen konfigurieren. Sie können auch eine Auflistung DocumentDB, um nur bestimmte Eigenschaften oder Pfade mit Platzhaltern Ihrer Dokumente JSON Selektives indizieren, in [die Richtlinie Indizierung einer Websitesammlung konfigurieren](documentdb-indexing-policies.md#configuring-the-indexing-policy-of-a-collection)beschriebenen konfigurieren. Ausschließen von Eigenschaften oder Dokumente verbessert auch den Durchsatz schreiben – was bedeutet, dass Sie weniger Anforderung Einheiten genutzt werden.   

## <a name="next-steps"></a>Nächste Schritte

Um den Vorgang fortzusetzen Schulung zur Funktionsweise von DocumentDB finden Sie unter [Partitioning und dieselbe Skalierung in Azure DocumentDB](documentdb-partition-data.md).

Anweisungen zum Überwachen der Leistung Ebenen Azure-Portal finden Sie unter [Monitor ein Konto DocumentDB](documentdb-monitor-accounts.md). Weitere Informationen zur Auswahl der Leistung Ebenen für Websitesammlungen finden Sie unter [Leistung Ebenen in DocumentDB](documentdb-performance-levels.md).
 
