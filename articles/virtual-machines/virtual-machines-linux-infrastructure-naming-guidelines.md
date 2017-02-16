<properties
    pageTitle="Richtlinien für die Benennung Infrastruktur | Microsoft Azure"
    description="Lernen Sie die wichtigsten Entwurf und Implementierung von Richtlinien für die Benennung von Azure-Infrastrukturdiensten aus."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="infrastructure-naming-guidelines"></a>Richtlinien für Infrastruktur

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Dieser Artikel befasst sich Kenntnisse fast Benennungskonvention für alle Ihre verschiedenen Azure Ressourcen eine logische und leicht verständliche Reihe von Ressourcen in Ihrer Umgebung zu erstellen.

## <a name="implementation-guidelines-for-naming-conventions"></a>Von Implementierungsrichtlinien für Benennungskonventionen

Entscheidungen:

- Was sind Ihre Benennungskonvention für Azure Ressourcen?

Aufgaben:

- Definieren der Affixtyp um über Ihre Ressourcen zur Konsistenz verwalten.
- Definieren von Speicher Kontonamen ausgehend von der Anforderung bis global eindeutig sein.
- Dokument die Benennungskonvention für alle Beteiligten zu Bereitstellungen Konsistenz sicherzustellen verteilen damit verwendet werden.

## <a name="naming-conventions"></a>Benennungskonventionen

Sie sollten eine gute Benennungskonvention angeordnet haben, bevor Sie etwas in Azure zu erstellen. Eine Benennungskonvention wird sichergestellt, dass alle Ressourcen, die eine vorhersehbar Name, welche geringere den Verwaltungsaufwand Management dieser Ressourcen zugeordnet.

Sie könnten Sie entscheiden folgen eine bestimmte Folge von Benennungskonvention für die gesamte Organisation oder für eine bestimmte Azure-Abonnements oder Konto definiert. Obwohl es für Einzelpersonen in Organisationen herstellen implizite Regeln bei der Arbeit mit Azure Ressourcen einfach ist, müssen Sie für Teams zusammenarbeiten in Azure skalieren können.

Stimmen Sie auf eine Reihe von Benennungskonventionen voraus. Es gibt einige Überlegungen hinsichtlich Benennungskonventionen, die über Ausschneiden, der festlegt, der Regeln.

## <a name="affixes"></a>Affixtyp

Während Sie suchen, um eine Benennungskonvention definieren, ist eine Entscheidung, ob das Affix am:

- Den Anfang des Namens (Präfix)
- Das Ende des Namens (Suffix)

Hier sind beispielsweise zwei mögliche Namen für eine Ressourcengruppe mithilfe der `rg` bringt an:

- RG-WebApp (Präfix)
- WebApp-Rg (Suffix)

Affixtyp können auf verschiedene Aspekte verweisen, die die entsprechenden Ressourcen zu beschreiben. Die folgende Tabelle zeigt einige Beispiele für in der Regel verwendet.

| Seitenverhältnis                               | Beispiele                                                               | Notizen                                                                                                      |
|:-------------------------------------|:-----------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------|
| Umgebung                          | Entwickler, Stg, prod                                                         | Je nach den Zweck und die Namen der einzelnen Umgebung.                                                     |
| Speicherort                             | Usw (Westen USA) verwenden (ostasiatische US 2)                                         | Abhängig von der Region des Datencenters oder die Region des Unternehmens.                               |
| Azure Komponente, Dienst- oder Produkt | RG für Ressourcengruppe, VNet für virtuelle Netzwerk                        | Je nach dem Produkt für das Ressource Unterstützung bereitstellt.                                          |
| Rolle                                 | DB, app, web                                                           | Abhängig von der Rolle des virtuellen Computers.                                                              |
| Instanz                             | 01, 02, 03 usw..                                                       | Für Ressourcen, die mehr als eine Instanz ausgeführt werden. Beispielsweise mit Lastenausgleich Webservern in einen Cloud-Dienst. |


Beim Einrichten Ihrer Benennungskonventionen, stellen Sie sicher, dass klar Zustand welche Affixtyp für jeden Typ von Ressourcen, und welche Positionen (Präfix im Vergleich mit einer Suffix) verwendet werden soll.

## <a name="dates"></a>Datumsangaben

Es ist oft wichtig, Erstellungsdatum aus den Namen einer Ressource zu bestimmen. Es empfiehlt sich das Datumsformat jjjjMMtt. Dieses Format wird sichergestellt ist, die nicht nur die vollständige Datum aufgezeichnet wird, aber auch die zwei Ressourcen, deren Namen unterscheiden sich nur auf das Datum, werden alphabetisch und chronologisch sortiert.

## <a name="naming-resources"></a>Benennen von Ressourcen

Definieren Sie jede Art von Ressourcen in die Benennungskonvention, sollte der haben Regeln, die definieren wie jede Ressource Namen zuweisen, die erstellt wird. Diese Regeln sollten auf alle Arten von Ressourcen, beispielsweise anwenden:

- Abonnements
- Konten
- Speicherkonten
- Virtuelle Netzwerke
- Subnetze
- Verfügbarkeit von Gruppen
- Ressourcengruppen
- Virtuellen Computern
- Endpunkte
- Netzwerk-Sicherheitsgruppen
- Rollen

Um sicherzustellen, dass der Name enthält genügend Informationen, um zu bestimmen, welche Ressource es verweist, aussagekräftigen Namen verwendet werden sollen.

## <a name="computer-names"></a>Computernamen

Beim Erstellen eines virtuellen Computers (virtueller Computer) erfordert Azure einen virtuellen Computer Namen mit bis zu 64 Zeichen, der für den Namen der Ressource verwendet wird. Azure verwendet den gleichen Namen für das Betriebssystem auf dem virtuellen Computer installiert ist. Jedoch möglicherweise diese Namen nicht immer die gleiche sein.

Wenn ein virtueller Computer aus einer VHD-Bilddatei, die bereits ein Betriebssystem enthält erstellt wurde, kann der Name des virtuellen Computers in Azure Netzwerknamen des virtuellen Computers Betriebssystem variieren. Diese Situation kann ein Maß an zusätzlichem Verwaltung von virtuellen Computern, hinzufügen, daher nicht empfohlen, gehen Sie wie folgt. Weisen Sie der Ressource Azure-virtuellen Computer denselben Namen wie dem Namen des Computers, den Sie an das Betriebssystem von diesen virtuellen Computer zuweisen.

Es empfiehlt sich, dass der Name des Azure-virtuellen Computers identisch mit dem Namen des Computers zugrunde liegenden Betriebssystem ist.

## <a name="storage-account-names"></a>Namen von Benutzerkonten

Speicherkonten sind spezielle Regeln, die für deren Namen vorhanden. Sie können nur Kleinbuchstaben und Zahlen. Weitere Informationen finden Sie unter [Erstellen eines Speicher-Kontos](../storage/storage-create-storage-account.md#create-a-storage-account) . Darüber hinaus sollten der Kontonamen Speicher mit von Core.Windows.NET befinden., ein Global gültigen und eindeutigen DNS-Name. Wenn das Speicherkonto Mystorageaccount aufgerufen wird, sollten die folgenden resultierenden DNS-Namen beispielsweise eindeutig sein:

- mystorageaccount.BLOB.Core.Windows.NET
- mystorageaccount.Table.Core.Windows.NET
- mystorageaccount.Queue.Core.Windows.NET


## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 