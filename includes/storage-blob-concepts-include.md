## <a name="what-is-blob-storage"></a>Was ist Blob-Speicher?

Azure Blob-Speicher ist ein Dienst zum Speichern von große Datenmengen unstrukturierten Objekt, wie z. B. Textdaten oder binäre Daten, die über eine beliebige Stelle in der Welt über HTTP oder HTTPS zugegriffen werden können. Sie können Daten in die Welt öffentlich verfügbar machen oder zum Speichern von Anwendungsdaten privat Blob-Speicher verwenden.

Häufige Verwendungen Blob-Speicher umfassen:

- Erstellen von Bildern oder Dokumente direkt in einem browser
- Speichern von Dateien für verteilten Zugriff
- Wiedergabe von Video- und audio
- Speichern von Daten für die Sicherung und Wiederherstellung, Wiederherstellung und Archivierung
- Speichern von Daten für die Analyse von einer lokalen oder einen bestimmten Dienst Azure gehostet

## <a name="blob-service-concepts"></a>BLOB-Dienst Konzepte

Der Blob-Dienst enthält die folgenden Komponenten:

![Blob1][Blob1]

- **Speicherkonto:** Alle Zugriff auf Azure-Speicher erfolgt über ein Speicherkonto. Dieses Speicherkonto möglich, eine **Allgemeine Speicher-Konto** oder einem **Blob-Speicher-Konto** die zum Speichern von Objekten/Blobs spezialisierte ist. Weitere Informationen zu Speicherkonten finden Sie unter [Azure-Speicher Account](../articles/storage/storage-create-storage-account.md).

- **Container:** Ein Containers bietet eine Gruppierung einer Reihe von Blobs. Alle Blobs muss sich in einem Container. Ein Konto kann eine unbegrenzte Anzahl von Container enthalten. Ein Containers kann eine unbegrenzte Anzahl von Blobs speichern. Beachten Sie, dass der Containername Kleinbuchstaben werden muss.

- **Blob:** Eine Datei von einem beliebigen Typ und Größe. Azure-Speicher bietet drei Typen von Blobs: Blobs blockieren, Seite Blobs und Blobs anfügen.

    *Blockieren Blobs* eignen sich zum Speichern von Text oder binäre Dateien, wie z. B. Dokumente und Mediendateien. *Anfügen Blobs* ähneln blockieren Blobs, da diese bestehen aus Bausteinen sind optimiert, für Anfügen Vorgänge, damit sie für die Protokollierungsszenarien hilfreich sind. Ein einzelnes Blob blockieren oder Anfügen Blob kann bis zu 50.000 Blöcke von 4 MB enthalten jeweils für eine Gesamtgröße von etwas mehr als 195 GB (4 MB X 50.000).

    *Seitenblobs* kann bis zu 1 TB groß sein und sind für häufige Lese Vorgänge effizienter. Azure-virtuellen Computern verwenden Seitenblobs als OS und Daten Datenträger.

    Details zur Benennung von Containern und Blobs finden Sie unter [Naming und verweisen auf Container, Blobs, und Metadaten](https://msdn.microsoft.com/library/azure/dd135715.aspx).


[Blob1]: ./media/storage-blob-concepts-include/blob1.jpg
