Ressource|Standardmäßige Grenzwert
---|---
Anzahl der Speicherkonten pro Abonnement|200<sup>1</sup>
TB pro Speicher-Konto|500 TB
Maximale Anzahl von Blob-Container, Blobs, Dateifreigaben, Tabellen, Warteschlangen, Personen oder Nachrichten pro Storage-Konto|Nur die Beschränkung beträgt die 500 TB Speicherkapazität-Konto
Max Größe eines einzelnen Blob Container, Tabelle oder einer Warteschlange|500 TB
Maximale Anzahl von Blöcken in einen Blockblob oder Blob Anfügen|50.000
Maximale Größe eines Blocks in ein Blob blockieren oder Blob Anfügen|4 MB
Maximale Größe eines Blob blockieren oder Blob Anfügen|50.000 x 4 MB (etwa 195 GB) 
Max Größe eines Seitenblob |1 TB
Max Größe einer Tabelle Entität|1 MB
Maximale Anzahl von Eigenschaften in einer Tabellenentität|252
Maximale Größe einer Nachricht in einer Warteschlange|64 KB
Max Größe einer Dateifreigabe|5 TB
Max Größe einer Datei in einem freigegebenen Ordner|1 TB
Maximale Anzahl von Dateien in einem freigegebenen Ordner|Nur die Beschränkung beträgt die 5 TB für die gesamte Kapazität der Dateifreigabe
Max 8 KB IOPS pro freigeben|1000
Maximale Anzahl von Dateien in einem freigegebenen Ordner|Nur die Beschränkung beträgt die 5 TB für die gesamte Kapazität der Dateifreigabe
Maximale Anzahl von Blob-Container, Blobs, Dateifreigaben, Tabellen, Warteschlangen, Personen oder Nachrichten pro Storage-Konto|Nur die Beschränkung beträgt die 500 TB Speicherkapazität-Konto
Maximale Anzahl von gespeicherten Access-Richtlinien pro Container, Dateifreigabe, Tabelle oder Warteschlange|5
Anfordern wie oft insgesamt (vorausgesetzt Objektgröße 1KB) pro Storage-Konto|Bis zu 20.000 IOPS, Personen pro Sekunde oder Nachrichten pro Sekunde
Ziel Durchsatz für einzelne blob|Bis zu 60 MB pro zweiten oder bis zu 500 Abfragen pro Sekunde
Ziel Durchsatz für einzelne Warteschlange (1 KB Nachrichten)|Bis zu 2000 Nachrichten pro Sekunde
Ziel Durchsatz für einzelne Tabellenpartition (1 KB-Einheiten)|Bis zu 2000 Elemente pro Sekunde
Ziel Durchsatz für einzelne Dateifreigabe|Bis zu 60 MB pro Sekunde
Max eingehende<sup>2</sup> pro Storage-Konto (uns Regionen)|10 Gbps Wenn GRS/ZRS<sup>3</sup> aktiviert, 20 Gbps für LRS
Max Ausgang<sup>2</sup> pro Storage-Konto (uns Regionen)|20 Gbps Wenn RAS-GRS/GRS/ZRS<sup>3</sup> aktiviert, 30 Gbps für LRS
Max eingehende<sup>2</sup> pro Storage-Konto (Europäische und asiatische Regionen)|5 Gbps Wenn GRS/ZRS<sup>3</sup> aktiviert, 10 Gbps für LRS
Max Ausgang<sup>2</sup> pro Storage-Konto (Europäische und asiatische Regionen)|10 Gbps Wenn RAS-GRS/GRS/ZRS<sup>3</sup> aktiviert, 15 Gbps für LRS

<sup>1</sup> Dies umfasst sowohl Standard- und Premium Speicher-Konten. Wenn Sie mehr als 200 Speicherkonten benötigen, stellen Sie eine Anforderung durch [Azure-Support](https://azure.microsoft.com/support/faq/). Das Team Azure-Speicher wird Ihr Unternehmen Fall überprüfen und kann bis zu 250 Speicherkonten genehmigen. 

<sup>2</sup> *Eingehende* bezieht sich auf alle Daten (Anfragen) mit einem Speicherkonto gesendet werden. *Ausgang* bezieht sich auf alle Daten (Antworten) von einem Speicherkonto empfangen werden.  

<sup>3</sup> Azure Speicher Replikationsoptionen umfassen:

- **RAS-GRS**: Geo redundante Speicher Lese-Zugriff. Wenn RAS-GRS aktiviert ist, werden Ausgang Ziele für den sekundären Speicherort für die primäre Position identisch.
- **GRS**: Geo redundante Speicherung. 
- **ZRS**: Zone redundante Speicherung. Nur für den Zugriffsschutz Blobs verfügbar. 
- **LRS**: lokal redundante Speicherung. 

