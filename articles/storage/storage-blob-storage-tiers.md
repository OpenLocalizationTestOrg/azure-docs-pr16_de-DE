<properties
    pageTitle="Aussagekräftige Azure-Speicher für Blobs | Microsoft Azure"
    description="Speicherebenen für Azure Blob-Speicher bieten Kosten effiziente Speicher Objekt-Daten nach Access Mustern. Die Speicherebene aussagekräftige ist für Daten optimiert, die weniger häufig zugegriffen werden kann."
    services="storage"
    documentationCenter=""
    authors="michaelhauss"
    manager="vamshik"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="mihauss"/>


# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Azure Blob-Speicher: Tastaturkürzel und kühlt Speicherebenen

## <a name="overview"></a>(Übersicht)

Azure-Speicher bietet nun zwei Ebenen von Speicherplatz für BLOB-Speicher (Objektspeicher), damit Sie Ihre Daten am häufigsten kostengünstig je nach dessen Verwendung speichern können. Die Azure **wichtiges Speicherebene** wurde optimiert, zum Speichern von Daten, die häufig zugegriffen werden kann. Die Azure **aussagekräftige Speicherebene** wurde optimiert, zum Speichern von Daten, die sich selten zugänglich und langer Lebensdauer befindet. Daten in der Speicherebene aussagekräftige Verfügbarkeit einer etwas tiefer tolerieren können, immer noch erfordert jedoch hohe Zuverlässigkeit und ähnliche Zeit für den Zugriff auf und Durchsatz Merkmale als wichtiges Daten. Für aussagekräftige Daten werden etwas tiefer Verfügbarkeit Vereinbarung zum SERVICELEVEL und höhere Kosten, weil Access zulässigen vor-und Nachteile für viel Speicher senken.

Heute wächst in der Cloud gespeicherte Daten in einer exponentiellen Tempo. Zum Verwalten von Kosten für Indexeigenschaften Speicher erweitern, empfiehlt es sich zum Organisieren von Daten auf Grundlage von Attributen wie Häufigkeit von Access und geplanten Aufbewahrungszeitraum. Daten in der Cloud gespeichert werden können etwas ganz anderes werden wie er generiert, verarbeitet und während ihrer Lebensdauer zugegriffen wird. Einige Daten ist aktiv zugegriffen und in der Audiodatei geändert. Einige Daten erfolgt sehr häufig früh in der Lebensdauer mit Access erheblich als from Daten ablegen. Einiger Daten im Leerlauf in der Cloud und selten ist, falls überhaupt, nur ein Mal zugegriffen gespeichert.

Jedes dieser Daten Access Szenarien beschrieben, die über die Vorteile von einer gestaffelter Ebene bilden, die nach einem bestimmten Access Muster optimiert ist. Mit der Einführung der aktiven und aussagekräftige Speicher leisten trennen Azure Blob-Speicher jetzt diese Notwendigkeit gestaffelter Speicherebenen mit Adressen Preise Modelle.

## <a name="blob-storage-accounts"></a>BLOB-Speicher-Konten

**BLOB-Speicher-Konten** sind spezielle Speicher-Konten für Ihre unstrukturierten Daten als Blobs (Objekte) in Azure-Speicher zu speichern. Mit Blob-Speicher-Konten können Sie jetzt zwischen aktiven und aussagekräftige Speicherebenen zum Speichern von auswählen, die weniger genutzten aussagekräftige Daten bei einer unteren Speicher Kosten, und speichern Weitere häufig verwendete wichtiges Daten bei einer unteren Access Kosten. BLOB-Speicherkonten ähneln Ihrer vorhandenen allgemeine Speicher-Konten und freigeben aller hervorragende Zuverlässigkeit, Verfügbarkeit, Skalierbarkeit und Performance-Funktionen, mit denen Sie noch heute, einschließlich 100 %-API Konsistenz für blockieren Blobs und Blobs anfügen.

> [AZURE.NOTE] BLOB-Speicherkonten unterstützt nur blockieren und Blobs und nicht Seitenblobs angefügt werden soll.

BLOB-Speicherkonten verfügbar machen das Attribut **In Access** die ermöglichen es Ihnen, die Speicherebene als **Hotspot** oder **aussagekräftige** je nach den gespeicherten Daten in das Konto angeben. Ist eine Änderung im Verwendungsmuster Ihrer Daten, können Sie auch zwischen diesen Speicherebenen zu einem beliebigen Zeitpunkt wechseln.

> [AZURE.NOTE] Ändern der Speicherebene möglicherweise zusätzliche Gebühren. Bitte finden Sie im Abschnitt [Preise und Abrechnung](storage-blob-storage-tiers.md#pricing-and-billing) Weitere Details ein.

Verwendung Beispielszenarien für die Leiste wichtiges Speicher umfassen:

- Daten, die in der aktiven Nutzung oder (aus gelesen und geschrieben) häufig zugegriffen werden soll.
- Daten, die für die Verarbeitung und tatsächlichen Migration an die Schicht aussagekräftige Speicher bereitgestellt werden.

Verwendung der Beispielszenarien für die Leiste aussagekräftige Speicher umfassen:

- Sichern, Archivierung und Disaster Wiederherstellung Datasets.
- Ältere Medieninhalten nicht häufig nicht mehr angezeigt, sondern zur Verfügung sofort beim Zugriff auf erwartet wird.
- Große Datenmengen, der gespeicherte Kosten effektiv sein, während Sie weitere Daten für die Verarbeitung von zukünftigen gesammelt werden müssen. (*z.*B. langfristiges Speichern von wissenschaftlichen Daten, unformatierten werden Daten aus einer Fertigung Einrichtung)
- Ursprüngliche (unformatierten) Daten, die auch nachdem sie in der endgültigen verwendbar Form verarbeitet wurde beibehalten müssen. (*z.*B. unformatierten Mediendateien nach Umcodierung in andere Formate)
- Kompatibilität und Archivierung Daten, die längere Zeit gespeichert werden und wie nie zugegriffen werden. (*z.*B. Sicherheit Kamera entfernt, alte X-Rays/Kernspintomographien für Organisationen im Gesundheitswesen, Aufzeichnungen Audio- und Transkripts der Kundenanrufe für Finanzdienstleister)

Weitere Informationen zu Speicherkonten finden Sie unter [zu Azure-Speicher-Konten](storage-create-storage-account.md) .

Für Applikationen mit nur Anforderung blockieren oder Blob-Speicher anfügen, empfehlen wir die Verwendung von BLOB-Speicherkonten gestaffelter Preisgestaltung Modell gestufte Speicher nutzen. Jedoch wir wissen, dass dies möglicherweise nicht möglich, unter bestimmten Umständen, wobei mithilfe von allgemeine Speicher-Konten die beste Wahl, z. B. werden:

- Sie müssen Tabellen, Warteschlangen oder Dateien verwenden und möchten Ihre Blobs in demselben Speicherkonto gespeichert. Beachten Sie, dass keine technische zum Speichern von diese Elemente in der gleichen Konto Vorteile Zahlen, die dieselbe gemeinsamer Schlüssel.
- Sie müssen immer noch die Bereitstellung Klassisch verwenden. BLOB-Speicher-Konten sind nur verfügbar über das Modell zur Bereitstellung von Azure Ressourcenmanager.
- Sie müssen Seitenblobs verwenden. BLOB-Speicherkonten unterstützt Seitenblobs nicht. Es empfiehlt sich im Allgemeinen, blockieren Blobs verwenden, es sei denn, Sie einer bestimmten Seitenblobs benötigen.
- Sie verwenden eine Version der [Speicher Services REST-API](https://msdn.microsoft.com/library/azure/dd894041.aspx) , die älter als 2014-02-14 ist oder einer Clientbibliothek mit eine ältere Version als 4.x und Ihrer Anwendung können nicht aktualisiert werden.

> [AZURE.NOTE] BLOB-Speicher-Konten werden in einem Großteil Azure Regionen mit mehr, denen Sie folgen derzeit unterstützt. Sie können die aktualisierte Liste der verfügbaren Bereiche auf der Seite [Azure-Dienste nach Region](https://azure.microsoft.com/regions/#services) suchen.

## <a name="comparison-between-the-storage-tiers"></a>Vergleich zwischen Speicherebenen

Die folgende Tabelle zeigt den Vergleich zwischen zwei Speicherebenen:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Wichtiges Speicherebene</center></strong></td>
    <td><strong><center>Aussagekräftige Speicherebene</center></strong></td
</tr>
<tr>
    <td><strong><center>Verfügbarkeit</center></strong></td>
    <td><center>99,9 %</center></td>
    <td><center>99 %</center></td>
</tr>
<tr>
    <td><strong><center>Verfügbarkeit<br>(RAS-GRS liest)</center></strong></td>
    <td><center>99,99 %</center></td>
    <td><center>99,9 %</center></td>
</tr>
<tr>
    <td><strong><center>Verwendung Gebühren</center></strong></td>
    <td><center>Höhere Kosten für die Speicherung<br>Zugreifen auf und Transaktion Senken</center></td>
    <td><center>Speicher senken<br>Höhere Kosten, weil Access und Transaktion</center></td>
</tr>
<tr>
    <td><strong><center>Minimale Objektgröße<center></strong></td>
    <td colspan="2"><center>N/V</center></td>
</tr>
<tr>
    <td><strong><center>Mindestens Speicherplatz Dauer<center></strong></td>
    <td colspan="2"><center>N/V</center></td>
</tr>
<tr>
    <td><strong><center>Wartezeit<br>(Zeit bis zum ersten Byte)<center></strong></td>
    <td colspan="2"><center>Millisekunden</center></td>
</tr>
<tr>
    <td><strong><center>Skalierbarkeit und Leistung Ziele<center></strong></td>
    <td colspan="2"><center>Identisch allgemeine Speicher-Konten</center></td>
</tr>
</tbody>
</table>

> [AZURE.NOTE] BLOB-Speicherkonten unterstützen die gleichen Leistung und Skalierbarkeit Ziele als allgemeine Speicher-Konten. Weitere Informationen finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](storage-scalability-targets.md) .

## <a name="pricing-and-billing"></a>Preise und Abrechnung

BLOB-Speicherkonten verwenden ein neues Preisgestaltung Modells für BLOB-Speicher basieren auf die Speicherkategorie. Wenn Sie ein Blob-Speicher-Konto verwenden, Abrechnung unter folgenden Aspekten Ursachen zurückzuführen:

- **Kosten für die Speicherung**: Zusätzlich gespeicherte Datenmenge, abhängig von die Kosten des Speicherns von Daten im Speicherebene. Die Kosten pro Gigabyte ist für die Leiste aussagekräftige Speicher als für die Leiste wichtiges Speicher geringer.
- **Zugriff auf Daten Kosten**: für Daten in der aussagekräftige Speicherebene, Sie werden in Rechnung gestellt Gebühr pro Gigabyte Daten Access zum Lesen und schreiben.
- **Transaktionskosten**: eine Gebühr pro Transaktion für beide Ebenen vorhanden ist. Es ist jedoch die Kosten pro Transaktion für die Leiste aussagekräftige Speicher größer als die für die Leiste wichtiges Speicher.
- **Kosten für Datenübertragung Geo-Replikation**: Dies gilt nur für Konten mit Geo-Replikation konfiguriert, einschließlich GRS und RAS-GRS. Geo-Replikation Datenübertragung budgetgerecht eine Gebühr pro Gigabyte.
- **Ausgehende Daten Kosten übertragen**: Rechnung für Bandbreite Verwendung pro Gigabyte Abständen mit allgemeine Speicherkonten konsistent ausgehende Datenübertragung (Daten, die aus einer Azure Ihrer Region übertragen wird) zu tätigen.
- **Ändern der Speicherebene**: Ändern der Speicherebene aus coole zu Tastaturkürzel eine Belastung gleich, lesen alle Daten in dem Speicherkonto für jeden Übergang vorhandenen entstehen. Andererseits, werden ändern die Speicherebene aus wichtiges in coole kostenlose Kosten.

> [AZURE.NOTE] Um Benutzerberechtigungen zum Testen der neuen Speicherebenen und überprüfen Sie die Funktionalität Beitrag Launch, die Gebühr für die Speicherung ändern wird Ebene aus coole zu Tastaturkürzel deaktivieren bis zu 30 % 2016 Juni abgesehen werden. Juli 1st 2016 wird gestartet, wird die Gebühr auf alle Übergänge von coole zu Tastaturkürzel angewendet werden. Detaillierte Informationen zur Preisgestaltung Modell für Blob-Speicher Konten finden Sie unter, [Azure Speicher Preise](https://azure.microsoft.com/pricing/details/storage/) Seite. Weitere Details für ausgehenden Daten durchstellen Gebühren finden Sie unter Seite [Daten Übertragung Preise Details](https://azure.microsoft.com/pricing/details/data-transfers/) .

## <a name="quick-start"></a>Schnellstart

In diesem Abschnitt werden wir die folgenden Szenarien, die über das Azure-Portal zeigen:

- So erstellen Sie ein Blob-Speicher-Konto.
- Informationen zum Verwalten eines Blob-Speicher-Konto.

### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

#### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Erstellen einer Blob-Speicher-Konto mithilfe von Azure-portal

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)aus.

2. Wählen Sie im Menü Hub **neu** > **Daten + Speicher** > **Speicher-Konto**.

3. Geben Sie einen Namen für Ihr Speicherkonto aus.

    Dieser Name muss global eindeutig sein. Es wird als Teil der URL verwendet, um die Objekte im Speicherkonto zugreifen verwendet.  

4. Wählen Sie als das Modell zur Bereitstellung von **Ressourcenmanager** aus.

    Mehrstufige Speicher kann nur mit Ressourcenmanager Speicherkonten verwendet werden. Dies ist das Modell empfohlene Bereitstellung für neue Ressourcen. Weitere Informationen schauen Sie sich die [Ressourcenmanager Azure Übersicht](../azure-resource-manager/resource-group-overview.md).  

5. Wählen Sie in der Dropdownliste Art von Konto **Blob-Speicher**ein.

    Dies ist die Stelle, an der Sie den Typ des Speicherkontos auswählen. Mehrstufige Speicher ist nicht verfügbar in allgemeine Speicher. Es ist jedoch nur in den BLOB-Speicher Typ Konto verfügbar.    

    Beachten Sie, dass, wenn Sie diese Option auswählen, die Leistung Ebene in Standard festgelegt ist. Mehrstufige Speicher ist nicht mit der Premium Leistung Ebene verfügbar.

6. Wählen Sie die Replikationsoption für das Speicherkonto: **LRS**, **GRS**oder **RAS-GRS**. Die Standardeinstellung ist **RAS-GRS**.

    LRS = lokal redundante Speicherung; GRS = Geo redundante Speicher (2 Regionen); RAS-GRS ist Lesezugriff Geo redundante Speicher (2 Regionen mit der zweiten Lesezugriff).

    Detaillierte Informationen zur Azure-Speicher Replikationsoptionen schauen Sie sich [Replikation Azure-Speicher](storage-redundancy.md).

7. Wählen Sie die richtige Speicherebene für Ihre Anforderungen: Legen Sie die **Ebene des Zugriffs** auf **aussagekräftige** oder **Hotspot**. Die Standardeinstellung ist **interessant**.

8. Wählen Sie das Abonnement, in dem das neue Speicherkonto erstellt werden soll.

9. Geben Sie eine neue Ressourcengruppe, oder wählen Sie eine vorhandene Ressourcengruppe aus. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md).

10. Wählen Sie den Bereich für Ihr Speicherkonto aus.

11. Klicken Sie auf **Erstellen** , um das Speicherkonto zu erstellen.

#### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>Ändern der Speicherebene eines Verwenden des Portals Azure BLOB-Speicher-Kontos

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)aus.

2. Um mit Ihrem Speicherkonto zu navigieren, wählen Sie alle Ressourcen aus, und wählen Sie dann Ihr Speicherkonto.

3. Klicken Sie in das Blade Einstellungen auf **Konfiguration** , damit die Kontokonfiguration ändern und/oder anzeigen.

4. Wählen Sie die richtige Speicherebene für Ihre Anforderungen: Legen Sie die **Ebene des Zugriffs** auf **aussagekräftige** oder **Hotspot**.

5. Klicken Sie auf Speichern im Kopfbereich des Blades.

> [AZURE.NOTE] Ändern der Speicherebene möglicherweise zusätzliche Gebühren. Bitte finden Sie im Abschnitt [Preise und Abrechnung](storage-blob-storage-tiers.md#pricing-and-billing) Weitere Details ein.

## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Auswertung und Migrieren zu Blob-Speicher-Konten

Der Zweck der in diesem Abschnitt ist für eine reibungslose Übergang zum Blob-Speicher-Konten mit Benutzern dabei helfen können. Es gibt zwei Szenarien mit mehreren Benutzern ein:

- Sie haben ein vorhandenes allgemeine Speicher-Konto und eine Änderung mit einem Blob-Speicher-Konto mit der richtigen Speicherebene auswerten möchten.
- Sie haben entschieden, ein Blob-Speicher-Konto verwenden oder bereits eine haben und möchten, überprüfen Sie, ob die Speicherebene wichtiges oder aussagekräftige verwendet werden sollen.

In beiden Fällen besteht die erste Tagesordnungspunkt zu schätzen Sie die Kosten für Speicherung und den Zugriff auf Ihre Daten in einem Blob-Speicher-Konto gespeichert, die mit den aktuellen Kosten vergleichen.

### <a name="evaluating-blob-storage-account-tiers"></a>Auswertung BLOB-Speicher Konto Ebenen

Zur Schätzung der Kosten für Speicherung und den Zugriff auf Daten in einem Blob-Speicher-Konto gespeichert, müssen Sie bewerten Ihrer vorhandenen Verwendung eines Musters oder das erwarteten Verwendungsmuster ungefähre. Im Allgemeinen werden Sie wissen möchten:

- Der Speicherbedarf – wie viel Daten gespeichert werden und wie ändert dies monatlich?
- Ihr Speicher Access Muster - verbleibenden Daten werden lesen und schreiben, mit dem Konto (einschließlich der neue Daten)? Wie viele Transaktionen für den Zugriff auf Daten verwendet werden und welche Arten von Transaktionen sind?

#### <a name="monitoring-existing-storage-accounts"></a>Überwachen von vorhandenen Speicherkonten

Zum Überwachen der Ihrer vorhandenen Speicherkonten, und diese Daten sammeln, können Sie nutzen Azure-Speicher Analytics die Protokollierung ausführt und den Kennzahlen Daten für ein Speicherkonto bereitstellt.
Speicher Analytics können Kennzahlen gespeichert werden, die aggregierte Transaktion Statistiken und Kapazität Daten zu Anfragen zum Dienst BLOB-Speicher für sowohl allgemeine Speicher-Konten als auch BLOB-Speicherkonten enthalten.
Diese Daten werden in bekannten Tabellen in demselben Speicherkonto gespeichert.

Weitere Informationen hierzu finden Sie [Informationen zu Speicher Analytics Metrik](https://msdn.microsoft.com/library/azure/hh343258.aspx) und [Speicher Analytics Kennzahlen Tabellenschema](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [AZURE.NOTE] BLOB-Speicherkonten verfügbar machen den Tabelle Endpunkt nur für das Speichern und den Zugriff auf die Daten Kennzahlen für dieses Konto.

Zum Überwachen der Speicherbedarf für den Blob-Speicher-Dienst, müssen Sie die Kapazität Metrik aktivieren.
Mit diesem aktiviert haben werden Kapazitätsdaten aufgezeichnet tägliche für Speicher-Konto Blob-Dienst und aufgezeichneten als ein Tabelleneintrag, der in der Tabelle *$MetricsCapacityBlob* innerhalb der gleichen Speicherkonto geschrieben werden.

Zum Überwachen des Daten Access Musters für den Blob-Speicher-Dienst, müssen Sie die stündlich Transaktion Metrik Ebene einer API zu aktivieren.
Mit diesem aktiviert pro API sind Transaktionen zusammengefasster stündlich und als ein Tabelleneintrag, der in der Tabelle *$MetricsHourPrimaryTransactionsBlob* innerhalb der gleichen Speicherkonto geschrieben wird aufgezeichnet. Die Tabelle *$MetricsHourSecondaryTransactionsBlob* Einträge die Transaktionen an den sekundären Endpunkt bei RAS-GRS Speicherkonten.

> [AZURE.NOTE] Für den Fall, dass Sie eine allgemeine Speicher-Konto verfügen, in dem Sie die Seitenblobs und virtuellen Computern Datenträger entlang blockieren gespeichert haben und Anfügen BLOB-Daten, ist dieses Verfahren Abschätzung nicht verfügbar. Dies ist, da Sie keine Möglichkeit, eindeutige Kapazität haben und Transaktion Kennzahlen basierend auf den Typ des Blob für nur blockieren und Anfügen Blobs, die mit einem BLOB-Speicherkonto migriert werden können.

Um eine gute Annäherung an Daten Verbrauch und Access Muster zu erhalten, empfehlen wir Sie wählen Sie einen Aufbewahrungszeitraum für die Metrik, die von Ihrer Verwendung von regulären Supportmitarbeiter ist und aufgeführt.
Eine Möglichkeit ist die Kennzahlen Daten für sieben Tage beibehalten und jede Woche, für die Analyse am Ende des Monats für die Daten sammeln.
Eine andere Möglichkeit ist dann, die Kennzahlen Daten für die letzten 30 Tage beibehalten und sammeln und Analysieren von Daten am Ende der Zeitraum von 30 Tagen.

Details zum Aktivieren sammeln und Anzeigen von Kennzahlen Daten finden Sie in, [Azure-Speicher aktivieren Metrik und Kennzahlen Daten anzeigen](storage-enable-and-view-metrics.md).

> [AZURE.NOTE] Speichern von, zugreifen auf und herunterladen Analytics-Daten werden auch genau wie normale Benutzerdaten in Rechnung gestellt.

#### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Die Nutzung Verwendung Kennzahlen Schätzung Kosten

##### <a name="storage-costs"></a>Kosten für Speicher

Der letzte Eintrag in der Kapazität Kennzahlen Tabelle *$MetricsCapacityBlob* mit Key aus der Zeile *'Daten'* zeigt die Speicherkapazität von Benutzerdaten verbraucht.
Der letzte Eintrag in der Kapazität Kennzahlen Tabelle *$MetricsCapacityBlob* mit der Zeile Key *"Analytics"* zeigt die Speicherkapazität durch die Protokolle Analytics verbraucht.

Diese total Kapazität von beiden Benutzerdaten verbraucht und Analytics Protokolle (falls aktiviert) können die Kosten des Speicherns von Daten im Speicherkonto verwendet werden.
Die gleiche Methode auch für-Schätzung Speicherkosten für Block verwendet werden kann und Anfügen Blobs in allgemeine Speicher-Konten.

##### <a name="transaction-costs"></a>Transaktionskosten

Die Summe der *'TotalBillableRequests'*, in allen Einträgen für eine API in der Tabelle Metrik zeigt die Gesamtzahl der Transaktionen für diese bestimmten API an. *z. B.*, die Gesamtzahl der *'GetBlob'* Transaktionen in einem bestimmten Zeitraum können berechnet durch die Summe der Summe berechenbaren Besprechungsanfragen für alle Einträge mit der Taste Zeile *' Benutzer; GetBlob'*.

Zur Transaktionskosten für BLOB-Speicherkonten Schätzung zu können, müssen Sie die Transaktionen in drei Gruppen unterteilen, da diese Preise anders angegeben sind.

- Schreiben Sie Transaktionen wie *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'*und *'CopyBlob'*ein.
- Löschen Sie Transaktionen wie *'DeleteBlob'* und *'DeleteContainer'*ein.
- Alle anderen Transaktionen.

Zur Transaktionskosten für allgemeine Speicherkonten Schätzung zu können, müssen Sie alle Transaktionen unabhängig von den Vorgang/API aggregieren.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Daten in Daten zuzugreifen und Geo-Replikation übertragen Kosten

Während Speicher Analytics nicht die Menge der Daten lesen und Schreiben mit einem Speicherkonto bereitstellt, kann es ungefähr geschätzt werden, indem Sie die Tabelle Kennzahlen.
Die Summe der *'TotalIngress'* in allen Einträgen für eine API in der Tabelle Metrik zeigt die Gesamtmenge eingehende Daten in Bytes für die bestimmte API an.
Auf ähnliche Weise gibt die Summe der *'TotalEgress'* die Gesamtmenge Ausgang Daten in Byte an.

Um die Daten-Access-Kosten für BLOB-Speicherkonten davon ausgehen, müssen Sie die Transaktionen in zwei Gruppen aufzuteilen.

- Die Menge der Daten aus dem Speicherkonto abgerufen werden kann geschätzt werden, indem Sie die Summe der *'TotalEgress'* für hauptsächlich die Vorgänge *'GetBlob'* und *'CopyBlob'* .
- Die Menge der Daten, die mit dem Speicherkonto geschrieben kann geschätzt werden, indem Sie die Summe der *'TotalIngress'* für hauptsächlich *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* und *'AppendBlock'* Vorgänge ansehen.

Die Kosten der Datenübertragung für BLOB-Speicherkonten Geo-Replikation kann auch mithilfe der Schätzung für den Umfang der Daten, die bei einem GRS oder RAS-GRS Speicherkonto geschrieben berechnet werden.

> [AZURE.NOTE] Eine ausführlichere beispielsweise zur Berechnung von Kosten für die Verwendung der Speicherebene wichtiges oder aussagekräftige sehen Sie sich bitte an den häufig gestellten Fragen mit dem Titel *"Was Hotspot und coole Access Ebenen sind und wie sollte ich feststellen, welche Version Use?"* der [Seite Preise Azure-Speicher](https://azure.microsoft.com/pricing/details/storage/).

### <a name="migrating-existing-data"></a>Migrieren von vorhandenen Daten

Ein Blob-Speicher-Konto ist ein spezielles zum Speichern von nur blockieren und Blobs anfügen. Vorhandene allgemeine Speicher-Konten, die Sie zum Speichern von Tabellen, Warteschlangen, Dateien und Festplatten sowie Blobs zulassen, können nicht auf BLOB-Speicherkonten konvertiert werden. Wenn die Speicherebenen verwenden möchten, müssen Sie neue BLOB-Speicherkonten erstellen und die vorhandenen Daten in den neu erstellten Konten migrieren.
Die folgenden Methoden können vorhandene Daten in BLOB-Speicherkonten von lokal Speichergeräten, von Drittanbieter-Cloud-Speicher-Anbieter oder von Ihrer vorhandenen allgemeine Speicher-Konten in Azure migrieren:

#### <a name="azcopy"></a>AzCopy

AzCopy ist ein Windows-Befehlszeilendienstprogramm entwickelt für leistungsfähige Kopieren von Daten an und von Azure-Speicher. Sie können AzCopy verwenden, können Sie Daten aus Ihrer vorhandenen allgemeine Speicher-Konten in Ihr Konto der Blob-Speicher kopieren oder Hochladen von Daten aus Ihrem lokalen Speichergeräte in Ihr Konto der Blob-Speicher.

Weitere Informationen hierzu finden Sie unter [Übertragen von Daten mit den AzCopy Befehlszeilenprogramm](storage-use-azcopy.md).

#### <a name="data-movement-library"></a>Daten Bewegung Bibliothek

Azure Speicher Daten Bewegung Bibliothek für .NET basiert auf Bewegung Framework Core Daten, die auf die Bedürfnisse von AzCopy. Leistungsfähige, zuverlässig und einfach durchstellen Datenoperationen ähnliche AzCopy dient die Bibliothek. Dadurch können Sie alle Vorteile von den Features von AzCopy in Ihrer Anwendung systembedingt ohne Umgang mit ausgeführt werden und externe Instanzen von AzCopy überwacht wird.

Weitere Informationen hierzu finden Sie unter [Azure Speicher Daten Bewegung Bibliothek für .net](https://github.com/Azure/azure-storage-net-data-movement)

#### <a name="rest-api-or-client-library"></a>REST-API oder Clientbibliothek

Sie können eine benutzerdefinierte Anwendung zum Migrieren der Daten in ein Blob-Speicher-Konto mithilfe einer der Azure Clientbibliotheken oder die Dienste Azure-Speicher REST-API erstellen. Azure-Speicher stellt rich Client-Bibliotheken für mehrere Sprachen und Plattformen wie .NET, Java, C++, Node.JS, PHP, Ruby und Python. Client-Bibliotheken bieten erweiterte Funktionen wie "Wiederholen" Logik, Protokollierung und parallele Uploads aus. Sie können auch direkt gegen die REST-API entwickeln die durch eine andere Sprache aufgerufen werden können, das HTTP-/HTTPS-Anfragen herstellt.

Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure Blob-Speicher](storage-dotnet-how-to-use-blobs.md).

> [AZURE.NOTE] Speichern von BLOBs, die mit clientseitig Verschlüsselung verschlüsselt Verschlüsselung-bezogene Metadaten, die mit dem Blob gespeichert. Es ist absolut entscheidend, dass alle Verfahren kopieren sicherstellen sollten, dass die Blob-Metadaten und besonders die Verschlüsselung-bezogene Metadaten, wird zwar beibehalten. Wenn Sie die Blobs ohne diese Metadaten kopieren, wird der Inhalt Blob nicht erneut einer abrufbaren sein. Weitere Details zum Metadaten Verschlüsselung relevant finden Sie unter [clientseitige Verschlüsselung Azure-Speicher](storage-client-side-encryption.md).

## <a name="faqs"></a>Häufig gestellte Fragen

1. **Bereits Speicherkonten jedoch verfügbar sind?**

    Ja, vorhandene Speicherkonten stehen immer noch zur Verfügung und Preise oder Funktionalität unverändert sind.  Sie verfügen nicht über die Möglichkeit, eine Speicherebene auszuwählen und tiering-Funktionen nicht in der Zukunft aufweisen.

2. **Warum und wann sollte ich verwenden von BLOB-Speicherkonten?**

    BLOB-Speicher-Konten sind speziell zum Speichern von Blobs und können wir die neuen Blob reduzierte Features vorstellen. Zukunft Blob-Speicher-Konten sind die empfohlene Methode zum Speichern von Blobs, als zukünftiger Funktionen wie hierarchische Speicher und Stufen erhalten eine Einführung Grundlage dieser Kontotyp. Es ist jedoch bis zu, dass Sie, wenn Sie migrieren möchten basierend auf Ihren geschäftlichen Anforderungen.

3. **Kann ich meine vorhandene Speicher-Konto mit einem BLOB-Speicherkonto konvertieren?**

    Nein. BLOB-Speicher-Konto ist eine andere Art von Speicher-Konto, und Sie müssen neu zu erstellen und zum Migrieren von Daten aus, wie bereits dargelegt.

4. **Kann ich Objekte in beiden Speicherebenen in demselben Konto speichern?**

    Das *' in Access'* -Attribut, das festlegt, mit die Speicherebene eine Ebene Konto festgelegt ist und gilt für alle Objekte in diesem Konto. Sie können keine das Attribut der Ebene Zugriff auf Objektebene festlegen.

5. **Kann ich die Speicherebene meines Blob-Speicher-Kontos ändern?**

    Ja. Sie werden können die Speicherebene zu ändern, indem Sie das Attribut *' in Access'* auf das Speicherkonto. Ändern der Speicherebene gelten für alle Objekte in das Konto. Ändern die Speicherebene aus wichtiges zu coole keine Gebühren, beim Ändern von coole fallen werden zu Tastaturkürzel entstehen einer pro GB Kosten für alle Daten in das Konto lesen.

6. **Wie oft kann ich die Speicherebene meines Blob-Speicher-Kontos ändern?**

    Während wir nicht erzwingen, führen Sie eine Einschränkung auf wie häufig die Speicherebene geändert werden kann, wenden Sie sich bitte beachten Sie, die das Ändern der Speicherebene von coole zu Tastaturkürzel erhebliche Gebühren anfallen. Es empfiehlt sich nicht, die Speicherebene häufig ändern.

7. **Werden die Blobs in der Speicherebene aussagekräftige als die in der Speicherebene wichtiges abweichendem?**

    BLOBs in der Speicherebene wichtiges enthält die gleiche Wartezeit als Blobs allgemeine Speicher-Konten. BLOBs in der Speicherebene aussagekräftige enthält eine ähnliche Wartezeit (in Millisekunden) als Blobs allgemeine Speicher-Konten.

    BLOBs in der Speicherebene aussagekräftige können eine etwas tiefer Verfügbarkeit Dienstalter (Vereinbarung zum SERVICELEVEL) als die Blobs in der Ebene wichtiges Storage gespeichert sind. Weitere Informationen hierzu finden Sie unter [Vereinbarung zum SERVICELEVEL für die Speicherung](https://azure.microsoft.com/support/legal/sla/storage).

8. **Kann ich Seitenblobs und virtuellen Computern Datenträger in BLOB-Speicherkonten speichern?**

    BLOB-Speicherkonten unterstützt nur blockieren und Blobs und nicht Seitenblobs angefügt werden soll. Azure-virtuellen Computern Datenträger als Unterstützung über Seitenblobs und daher BLOB-Speicherkonten können nicht zum Speichern von virtuellen Computern Datenträger verwendet werden. Es ist jedoch möglich, Sicherungskopien der Datenträger virtuellen Computern als blockieren Blobs in einem Blob-Speicher-Konto gespeichert.

9. **Muss ich meine vorhandenen Clientanwendungen BLOB-Speicherkonten über ändern?**

    BLOB-Speicher-Konten sind 100 %-API mit allgemeine Speicherkonten für Block einheitliche und Blobs anfügen. Wie eine Anwendung ist mit Blobs blockieren oder Anfügen Blobs, und Sie die 2014-02-14 Version der [Speicher Services REST-API](https://msdn.microsoft.com/library/azure/dd894041.aspx) oder höher verwenden und dann Ihrer Anwendung sollten nur zusammenarbeiten. Wenn Sie eine ältere Version von das Protokoll verwenden, müssen Sie die Anwendung für die neue Version Weitere nahtlos mit beide Speicher Kontotypen arbeiten zu aktualisieren. Im Allgemeinen verwenden wir immer wird empfohlen, die neueste Version unabhängig davon, welche Speicher-Konto verwenden Eingabe.

10. **Werden eine Änderung der Benutzeroberfläche es?**

    BLOB-Speicher-Konten sind sehr ähnlich eine allgemeine Speicher-Konten für das Speichern von blockieren und Anfügen Blobs und die wichtigsten Features von Azure-Speicher, einschließlich hoher Zuverlässigkeit und Verfügbarkeit, Skalierbarkeit, Leistung und Sicherheit unterstützen. Als die Features und Einschränkungen, die speziell für Blob-Speicher-Konten und deren Speicherebenen, die über alles andere Hervorhebung wurde bleibt.

## <a name="next-steps"></a>Nächste Schritte

### <a name="evaluate-blob-storage-accounts"></a>Auswerten Blob-Speicher-Konten

[Überprüfen der Verfügbarkeit von Blob-Speicher-Konten nach region](https://azure.microsoft.com/regions/#services)

[Auswerten Sie Verwendung Ihrer aktuellen Speicher Konten durch das Aktivieren der Azure-Speicher Kennzahlen](storage-enable-and-view-metrics.md)

[Aktivieren Sie BLOB-Speicher Preise nach region](https://azure.microsoft.com/pricing/details/storage/)

[Kontrollkästchen-Datenübertragung Preise](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Erste Schritte mit Blob-Speicher-Konten

[Erste Schritte mit Azure Blob-Speicher](storage-dotnet-how-to-use-blobs.md)

[Das Verschieben von Daten aus Azure-Speicher](storage-moving-data.md)

[Übertragen von Daten mit den AzCopy Befehlszeilenprogramms](storage-use-azcopy.md)

[Durchsuchen Sie und zu analysieren Sie Ihre Speicherkonten](http://storageexplorer.com/)
