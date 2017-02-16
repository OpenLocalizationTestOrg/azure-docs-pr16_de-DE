<properties
    pageTitle="Azure Speicherreplikation | Microsoft Azure"
    description="Daten in Ihr Microsoft Azure-Speicher-Konto werden für Zuverlässigkeit und hohe Verfügbarkeit repliziert. Replikationsoptionen beziehen Sie lokal redundante Speicher (LRS), Zone redundante Speicher (ZRS), Geo redundante Speicher (GRS) und Lesezugriff Geo redundante Speicher (RAS-GRS)."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="azure-storage-replication"></a>Azure Speicherreplikation

Die Daten in Ihrem Microsoft Azure-Speicher-Konto ist immer repliziert, um Zuverlässigkeit und hohe Verfügbarkeit sicherzustellen. Replikation kopiert Ihre Daten in der Mitte der gleichen Daten oder zu einem zweiten Data Center, je nachdem, welche Replikationsoption wählen Sie Sie aus. Die Replikation sind Ihre Daten geschützt und behält Anwendung nach-oben-Zeit im Falle einer vorübergehenden Hardware-Fehlern. Wenn Ihre Daten auf einem zweiten Data Center repliziert werden, schützen, die auch über die Daten vor einem schwerwiegenden Fehler in der gewohnten Standort befinden.

Die Replikation stellt sicher, dass Ihr Speicherkonto [Servicelevel Vertrag SERVICELEVEL für Speicher](https://azure.microsoft.com/support/legal/sla/storage/) auch unter Fehlern entspricht. Lesen Sie der Vereinbarung zum SERVICELEVEL Informationen zum Azure-Speicher für Zuverlässigkeit und Verfügbarkeit garantiert. 

Wenn Sie ein Speicherkonto erstellen, können Sie eine der folgenden Optionen für die Replikation auswählen:  

- [Lokal redundante Speicher (LRS)](#locally-redundant-storage)
- [Zone redundante Speicher (ZRS)](#zone-redundant-storage)
- [Geo redundante Speicher (GRS)](#geo-redundant-storage)
- [Lesezugriff Geo redundante Speicher (RAS-GRS)](#read-access-geo-redundant-storage)

Lesezugriff Geo redundante Speicher (RAS-GRS) ist die Standardoption beim Erstellen eines neuen Kontos mit Speicher.

In der folgenden Tabelle bietet einen schnellen Überblick über die Unterschiede zwischen LRS, ZRS, GRS und RAS-GRS, während Sie die folgenden Abschnitte jede Art von Replikation ausführlicher Adresse.

| Strategie für die Replikation                                                               | LRS | ZRS | GRS | RAS-GRS |
|:----------------------------------------------------------------------------------|:---|:---|:---|:------|
| Daten werden für mehrere Rechenzentren repliziert.                                     | Nein  | Ja | Ja | Ja    |
| Daten können aus der zweiten Standort sowie aus der primäre Speicherort gelesen werden. | Nein  | Nein  | Nein  | Ja    |
| Die Anzahl der Kopien der Daten auf separate Knoten verwaltet.                             | 3   | 3   | 6   | 6      |

Finden Sie unter [Azure Speicher Preise](https://azure.microsoft.com/pricing/details/storage/) Preisinformationen für die verschiedenen Redundanz-Optionen.

>[AZURE.NOTE] Premium Speicher unterstützt nur lokal redundante Speicher (LRS). Informationen zum Premium Speicher finden Sie unter [Premium Speicher: leistungsstarke Storage für Azure-virtuellen Computern Auslastung](storage-premium-storage.md).

## <a name="locally-redundant-storage"></a>Lokal redundante Speicher

Lokal redundante Speicher (LRS) repliziert Ihre Daten dreimal innerhalb einer Speicher Mengen Einheit die in einem Datencenter in der Region gehostet wird, in dem Sie Ihr Speicherkonto erstellt haben. Eine Anforderung schreiben gibt erfolgreich nur einmal auf alle drei Replikate geschrieben wurde. Diese drei Replikationen jedes befinden sich in separaten Fehlerstrukturanalyse und Upgrade-Domänen in einem Speicher Mengen Einheit. 

Ein Maßstab Lagerplatz ist eine Zusammenstellung von Racks von Speicherknoten. Fehlerstrukturanalyse-Domäne (FD) ist eine Gruppe von Knoten, die eine physische Einheit des Fehlers darstellen und zugehörigen an den gleichen physischen Knoten berücksichtigt werden können. Eine Upgrade Domäne (UD) ist eine Gruppe von Knoten, die während eines Upgrades Service (Einführung) die Vorgehensweise zum zusammen aktualisiert werden. Die drei Replikate sind in eine Speicher Skalierungseinheit, um sicherzustellen, dass die Daten sind verfügbar, auch wenn Fehler bei der Hardware wirkt sich auf die eine den einzelnen Shapes für Gestelle oder bei der Aktualisierung von Knoten während einer Einführung über UDs und FDs verteilt. 

LRS ist die niedrigsten Kosten Option und bietet mindestens Zuverlässigkeit im Vergleich zu anderen Optionen. Bei einem Datacenter Ebene Ausfall (Fire, überfluten usw.) möglicherweise alle drei Replikationen verlorene oder nicht wiederhergestellt. Um dieses Risiko auszuschalten empfiehlt sich Geo redundante Speicher (GRS) für die meisten Applikationen.

Lokal redundante Speicherung möglicherweise immer noch in bestimmten Szenarien wünschenswert sein: 

- Stellt die höchsten maximalen Bandbreite aller Replikationsoptionen Azure-Speicher.

- Wenn eine Anwendung Daten gespeichert, die einfach sichergestellt werden kann sind, können Sie LRS auswählen.

- Einige Applikationen sind auf Replikation von Daten in einem Land aufgrund von Daten Governance Anforderungen nur eingeschränkt. In ein anderes Land könnte eine gepaarten Region sein sollen. Informationen zu Region Paare finden Sie unter [Azure Regionen](https://azure.microsoft.com/regions/) .

## <a name="zone-redundant-storage"></a>Zone redundante Speicher

Zone redundante Speicher (ZRS) repliziert Ihre Daten asynchrone für Rechenzentren in einem oder zwei Bereiche neben dem Speichern von drei Replikate ähnlich wie LRS, womit höhere Zuverlässigkeit als LRS. In ZRS gespeicherte Daten ist dauerhaften, auch wenn das primäre Rechenzentrum nicht verfügbar oder nicht behebbarer ist.
Planen der Verwendung von ZRS von Kunden sollten sich bewusst sein, die: 

- ZRS steht nur für blockieren Blobs in allgemeine Speicher-Konten, und ist nur in Speicher Dienstversionen 2014-02-14 oder höher unterstützt. 

- Da asynchroner Replikation eine Verzögerung umfasst, ist es bei einem lokalen Ausfall möglich, dass die Änderungen, die noch nicht in den sekundären repliziert wurde verloren geht, wenn die Daten von der primären wiederhergestellt werden können.

- Das Replikat möglicherweise nicht verfügbar, bis Microsoft Failovers zum Secondary eingestellt.

- ZRS Konten können später LRS oder GRS konvertiert werden. Auf ähnliche Weise kann ein vorhandenes LRS oder GRS Konto mit einem Konto ZRS konvertiert werden.

- ZRS Konten verfügen über keine Kennzahlen oder Protokollierung Videofunktionen. 

## <a name="geo-redundant-storage"></a>Geo redundante Speicher

Geo redundante Speicher (GRS) repliziert Ihre Daten sekundäre Region ein, die hundert Meilen von der primären Region ist. Wenn Ihr Speicherkonto GRS aktiviert wurde, ist Ihre Daten dauerhaften sogar im Fall einer abgeschlossen regionalen Ausfall oder eine in dem die primäre Region nicht wiederhergestellt werden kann.

Für ein Speicherkonto mit GRS aktiviert ist ein Update zuerst in die primäre Region übertragen wurde, in dem sie dreimal repliziert wird Klicken Sie dann ist die Aktualisierung der sekundäre Region asynchrone repliziert, wo es auch dreimal repliziert wird. 

GRS die primären und sekundären Regionen Replikaten in separaten Fehlerstrukturanalyse-Domänen verwalten und Aktualisieren von Domänen in einen Lagerplatz Maßstab wie mit LRS beschrieben.

Aspekte:

- Da asynchroner Replikation eine Verzögerung umfasst, ist es bei einem regionalen Ausfall möglich, dass die Änderungen, die noch nicht in den Bereich sekundäre repliziert wurde verloren geht, wenn die Daten aus der primären Region nicht wiederhergestellt werden können.

- Das Replikat ist nicht verfügbar, es sei denn, Microsoft Failover auf der Sekundärachse Region eingestellt.

- Wenn eine Anwendung aus der sekundäre Region lesen will sollte der Benutzer RAS-GRS aktivieren. 

Wenn Sie ein Speicherkonto erstellen, wählen Sie die primäre Region für das Konto aus. Die sekundäre Region wird auf der Grundlage der primären Region bestimmt, und kann nicht geändert werden. Die folgende Tabelle zeigt die primären und sekundären Region Kombinationen.

| Primärschlüssel             | Sekundären           |
|---------------------|---------------------|
| Nord-zentralen US    | Süd zentralen US    |
| Süd zentralen US    | Nord-zentralen US    |
| Ostasiatische US             | Westen US             |
| Westen US             | Ostasiatische US             |
| US-OST 2           | USA – zentral          |
| USA – zentral          | US-OST 2           |
| North Europa        | Westen Europa         |
| Westen Europa         | North Europa        |
| Süd Ostasien     | Ostasien           |
| Ostasien           | Süd Ostasien     |
| Ostasiatische China          | North China         |
| North China         | Ostasiatische China          |
| Japan OST          | Japan "Westen"          |
| Japan "Westen"          | Japan OST          |
| Brasilien Süd        | Süd zentralen US    |
| Australien OST      | Australien oder |
| Australien oder | Australien OST      |
| Indien Süd         | Indien Central       |
| Indien Central       | Indien Süd         |
| US Gov Iowa         | US Gov Virginia     |
| US Gov Virginia     | US Gov Iowa         |
| Kanada Central      | Kanada OST          |
| Kanada OST         | Kanada Central      |
| UK "Westen"             | UK-Süd            |
| UK-Süd            | UK "Westen"             |
| Deutschland Central     | Deutschland Nordost   |
| Deutschland Nordost   | Deutschland Central     |
| Westen USA 2           | Westen der USA – zentral     |
| Westen der USA – zentral     | Westen USA 2           |

Aktuelle Informationen zu Regionen von Azure unterstützt werden finden Sie unter [Azure Regionen](https://azure.microsoft.com/regions/).
 
## <a name="read-access-geo-redundant-storage"></a>Lesezugriff Geo redundante Speicher

Lesezugriff Geo redundante Speicher (RAS-GRS) maximiert Verfügbarkeit für Ihr Speicherkonto durch die Bereitstellung von schreibgeschützten Zugriff auf die Daten in der sekundäre Speicherort, sowie die Replikation über zwei Bereiche von GRS bereitgestellt. 

Wenn Sie schreibgeschützten Zugriff auf Ihre Daten in der Region sekundäre aktivieren, steht die Daten auf einem sekundären Endpunkt, zusätzlich zu den primären Endpunkt für Ihr Speicherkonto. Der sekundäre Endpunkt ähnelt der primären Endpunkt, aber fügt das Suffix `–secondary` auf den Namen des Kontos. Wenn Ihre primäre Endpunkt für den Blob-Dienst wird beispielsweise `myaccount.blob.core.windows.net`, und klicken Sie dann Ihre sekundäre Endpunkt ist `myaccount-secondary.blob.core.windows.net`. Zugriffstasten für Ihr Speicherkonto sind für die primären und sekundären Endpunkte identisch.

Aspekte:

- Eine Anwendung hat zum Verwalten von welcher Endpunkts mit interagieren wird bei Verwendung von RAS-GRS. 

- RAS-GRS ist hoher Verfügbarkeit zu gedacht. Überprüfen Sie die [Leistung Checkliste](storage-performance-checklist.md), Skalierbarkeit Anleitung.

## <a name="next-steps"></a>Nächste Schritte

- [Preise Azure-Speicher](https://azure.microsoft.com/pricing/details/storage/)
- [Informationen über Konten Azure-Speicher](storage-create-storage-account.md)
- [Azure-Speicher Skalierbarkeit und Leistungsziele](storage-scalability-targets.md)
- [Microsoft Azure Redundanz Speicheroptionen und Lesezugriff Geo redundante Speicher](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)  
- [SOSP Papier - Azure-Speicher: Einen hoch verfügbaren Speicherplatz Clouddienst mit signifikante Konsistenz](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)  
