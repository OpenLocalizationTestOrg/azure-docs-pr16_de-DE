Es gibt zwei Arten von Speicherkonten aus:

### <a name="general-purpose-storage-accounts"></a>Allgemeine Speicher-Konten

Sie für Azure-Speicher-Dienste wie beispielsweise Tabellen, Warteschlangen, Dateien, Blobs und Azure-virtuellen Computern Datenträger unter einem einzigen Konto zugreifen bietet eine allgemeine Speicher-Konto. Diese Art von Speicherkonto besteht aus zwei Performance-Stufen:

- Eine standard-Speicher Leistung Stufe Sie zum Speichern von Tabellen, Warteschlangen und Dateien, wodurch Blobs und Azure-virtuellen Computers Festplatten.
- Eine Premium Speicher Leistung Ebene die aktuell Azure-virtuellen Computern Datenträger unterstützt. Finden Sie unter [Premium Speicher: leistungsstarke Storage für Azure-virtuellen Computern Auslastung](../articles/storage/storage-premium-storage.md) für ein detaillierter Überblick Premium-Speicher.

### <a name="blob-storage-accounts"></a>BLOB-Speicher-Konten

Einer BLOB-Speicherkonto ist eine spezielle Speicher zum Speichern Ihrer unstrukturierten Daten als Blobs (Objekte) in Azure-Speicher. BLOB-Speicherkonten ähneln Ihrer vorhandenen allgemeine Speicher-Konten, und teilen Sie die tollen Zuverlässigkeit, Verfügbarkeit, Leistung und Skalierbarkeit Funktionen, einschließlich 100 %-API Konsistenz für blockieren Blobs heute verwenden und Blobs anfügen. Für Applikationen mit nur Anforderung blockieren oder Blob-Speicher angefügt werden soll, empfehlen wir Blob-Speicher-Konten.

> [AZURE.NOTE] BLOB-Speicherkonten unterstützt nur blockieren und Blobs und nicht Seitenblobs angefügt werden soll.

BLOB-Speicherkonten verfügbar machen, das **In Access** -Attribut, während der Erstellung des Kontos angegeben und später nach Bedarf geändert werden kann. Es gibt zwei Arten von Access-Stufen, die auf der Grundlage Ihrer Daten Access Musters angegeben werden können:

- Eine **Hotspot** Access Ebene gibt an, dass die Objekte im Speicherkonto häufiger zugegriffen werden. So können Sie Daten einer Access-kostengünstiger zu speichern.
- Eine **aussagekräftige** Access-Ebene gibt an, dass die Objekte im Speicherkonto weniger häufig zugegriffen werden. So können Sie Daten am unteren Daten Speicherkosten zu speichern.

Ist eine Änderung im Verwendungsmuster Ihrer Daten, können Sie auch zwischen diesen Ebenen Access jederzeit wechseln. Ändern der Access-Ebene möglicherweise zusätzliche Gebühren. Bitte finden Sie unter [Preise und Rechnung für BLOB-Speicherkonten](../articles/storage/storage-blob-storage-tiers.md#pricing-and-billing) für weitere Details.

Weitere Informationen hierzu auf Blob-Speicher-Konten finden Sie unter [Azure BLOB-Speicher: kühlt und Tastaturkürzel Ebenen](../articles/storage/storage-blob-storage-tiers.md).

Bevor Sie ein Speicherkonto erstellen können, müssen Sie ein Azure-Abonnement verfügen, also einen Plan, der Sie auf einer Vielzahl von Azure Services zugreifen können. Sie können Azure ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)erste Schritte mit. Entscheiden Sie einmal einen Abonnementplan erwerben Sie können aus einer Vielzahl von [kaufen Optionen](https://azure.microsoft.com/pricing/purchase-options/)auswählen. Wenn Sie ein [MSDN-Abonnent](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)sind, erhalten Sie kostenlose monatliche Gutschriften, die mit Azure Dienste einschließlich Azure-Speicher verwendet werden können. Informationen für die Volumenlizenz finden Sie unter [Azure Speicher Preise](https://azure.microsoft.com/pricing/details/storage/) .

Um weitere Informationen zum Erstellen eines Kontos Speicher finden Sie unter Weitere Details [Speicherkonto erstellen](../articles/storage/storage-create-storage-account.md#create-a-storage-account) . Sie können bis zu 100 eindeutig benannten Speicherkonten mit jeweils einem Abonnement erstellen. Details zum Konto Speichergrenzwerte finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](../articles/storage/storage-scalability-targets.md) .
