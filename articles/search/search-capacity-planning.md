<properties
    pageTitle="Skalieren Ressourcenebenen für die Abfrage und Indizierung Auslastung in Azure suchen | Microsoft Azure"
    description="Kombinationen aus Partition und Kopie Computerressourcen,, in dem jeder Ressource in einer anderen Einheit berechenbaren suchen Preis Kapazität, Planung in Azure Suche basiert."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/24/2016"
    ms.author="heidist"/>

# <a name="scale-resource-levels-for-query-and-indexing-workloads-in-azure-search"></a>Maßstab Ressourcenebenen für die Abfrage und Indizierung Auslastung in Azure-Suche

Nachdem Sie [eine Preisgestaltung Stufe auswählen](search-sku-tier.md) und [Bereitstellen eines Suchdiensts](search-create-service-portal.md)besteht darin gemeldet, optional die Anzahl der Replikate oder von Ihrem Dienst verwendeten Partitionen vergrößern. Pro Ebene bietet eine feste Anzahl von Abrechnung Einheiten. In diesem Artikel wird erläutert, wie diese Einheiten, um eine optimale Konfiguration erzielen, die Ihren Anforderungen für die Ausführung einer Abfrage, Indizierung und Speicherung Salden zugewiesen. 

Ressourcenkonfiguration ist verfügbar, wenn Sie einen Dienst an den [grundlegenden Ebene](http://aka.ms/azuresearchbasic) oder eine der [Standard Ebenen](search-limits-quotas-capacity.md)bereitstellen. Für berechenbare Dienste auf diese Ebenen ist Kapazität in Schritten *Suchen Einheiten* (SU) erworben haben, in dem jede Partition und die Kopie als eine SU ermittelt. Verwenden von weniger SUs Ergebnisse in ein proportional unteren zurück. Abrechnung wendet in Kraft, solange der Dienst bereitgestellt wird. Wenn Sie einen Dienst vorübergehend nicht verwenden, ist die einzige Möglichkeit zur Abrechnung zu vermeiden, indem Sie den Dienst löschen, und klicken Sie dann bei Bedarf später Neuerstellen.

## <a name="terminology-partitions-and-replicas"></a>Terminologie: Partitionen und Replikate

Partitionen und der befinden sich die primären Ressourcen, die einen Suchdienst zurück.

**Partitionen** bieten Index Speicher und EA für Lese-und Schreibzugriff Vorgänge (beispielsweise beim Neuerstellen oder Aktualisieren eines Indexes).

**Replikate** sind Instanzen von der Suchdienst, vor allem zum Laden Saldo Abfragevorgänge verwendet. Jedes Replikat hostet immer eine Kopie eines Indexes. Wenn Sie 12 Replikate haben, müssen Sie 12 Kopien von jeder Index-Dienst geladen. 

> [AZURE.NOTE] Es gibt keine Möglichkeit, direkt bearbeiten oder verwalten, welche Indizes für ein Replikat ausgeführt. Eine Kopie jeder Index auf jedes Replikat ist Bestandteil der Dienstarchitektur an.

## <a name="how-to-allocate-partitions-and-replicas"></a>Wie Partitionen und der zugewiesen werden

In Azure suchen wird ein Dienst Anfangs eine minimale Ebene von Ressourcen aus einer Partition und eine Kopie reserviert. Für Ebenen, die sie unterstützen, können Sie inkrementell Datenverarbeitungsressourcen anpassen, indem Sie Partitionen erhöhen, wenn Sie mehr Speicherplatz und EA oder Replikate für größere Datenmengen der Abfrage oder eine bessere Leistung benötigen. Ein einzelner Dienst müssen ausreichende Ressourcen, um alle Auslastung (Indizierung und Abfragen) zu behandeln. Sie können keine Auslastung zwischen mehreren Diensten unterteilen.

Vergrößern oder Ändern der Zuordnung von Replikaten und Partitionen, empfiehlt es sich im Portal verwenden. Im Portal erzwingt Grenzwerte zulässigen Kombinationen, die unter Höchstgrenzen bei bleiben:

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com/) , und wählen Sie aus der Suchdienst.
2. Öffnen Sie in den Einstellungen das Blade Skala zu, und verwenden Sie die Schieberegler zum Vergrößern oder verkleinern die Anzahl der Partitionen und der.

Eine Alternative zum Portal ist der [Management REST-API](https://msdn.microsoft.com/library/azure/dn832687.aspx) verwenden, wenn Sie ein Skript oder Code-basierten provisioning Ansatz erforderlich.

Regel benötigen Applikationen suchen Sie weiteren Kopien als Partitionen, besonders, wenn die Dienstvorgänge in Richtung Abfrage Auslastung verzerrt werden. Im Abschnitt auf [hohen Verfügbarkeit](#HA) erklärt, warum.

> [AZURE.NOTE] Nachdem ein Dienst bereitgestellt wird, kann es zu einer höheren SKU direkte aktualisiert werden. Sie müssen zum Erstellen eines Suchdiensts auf der neuen Stufe und Ihre Indizes neu zu laden. Hilfe bei der Bereitstellung finden Sie unter [erstellen einen Azure Suchdienst im Portal](search-create-service-portal.md) .

<a id="HA"></a>
## <a name="high-availability"></a>Hohe Verfügbarkeit

Da sie einfach und relativ schnell skalieren ist, sollten, Sie mit einer Partition und eine beginnen oder zwei Replikate, und klicken Sie dann als Abfrage Datenmengen erstellen, auf die maximale Replikate und Partitionen Zentrales Skalieren von SKU unterstützt. Für viele Dienste auf den Stufen Basic oder S1 bietet eine Partition genügend Speicherplatz und EA (bei 15 Millionen Dokumente pro Partition).

Führen Sie Abfrage Auslastung hauptsächlich für Replikate. Sie wahrscheinlich benötigen zusätzliche Replikate, wenn Sie mehr Durchsatz oder hohen Verfügbarkeit benötigen.

Allgemeine Empfehlungen für hohen Verfügbarkeit sind:

- 2 Replikate hohen Verfügbarkeit der schreibgeschützt Auslastung (Abfragen)
- 3 oder mehr Replikate hohen Verfügbarkeit der Lese-und Schreibzugriff Auslastung (Abfragen sowie Indizierung als einzelne Dokumente hinzugefügt, aktualisiert oder gelöscht werden)

Service Level-Agreements SERVICELEVEL für Suchmaschinen Azure sind das Ziel bei der Abfragevorgänge und bei der Aktualisierung von Indizes bestehen, hinzufügen, aktualisieren oder Löschen von Dokumenten.

**Index Verfügbarkeit während einer Wiederherstellung**

Hoher Verfügbarkeit für die Suche Azure bezieht sich auf Abfragen und die Aktualisierung von Indizes, die nicht betreffen erneutes Erstellen eines Indexes. Wenn Sie der Index erfordert neu zu erstellen, beispielsweise wenn Sie hinzufügen oder Löschen eines Felds, einen Datentyp ändern oder Umbenennen eines Felds, müssten Sie den Index neu zu erstellen, indem Sie wie folgt vorgehen: Index löschen, den Index neu zu erstellen und die Daten erneut laden. 

Wenn Index Verfügbarkeit während einer Wiederherstellung beibehalten möchten, müssen Sie eine zweite Kopie der der Index bereits Herstellung auf denselben Dienst (mit einem anderen Namen) oder einen Index gleichnamigen haben, klicken Sie auf einem anderen Dienst, und klicken Sie dann Umleitung bereitzustellen oder über Logik in Ihren Code fehl.

## <a name="disaster-recovery"></a>Wiederherstellung

Es gibt derzeit keine integrierten Verfahren für die Wiederherstellung. Hinzufügen von Partitionen oder Replikate wäre einer falschen Strategie für die Besprechung Disaster Wiederherstellung Ziele. Der am häufigsten verwendete Ansatz ist auf Redundanz Ebene der Dienst hinzufügen, indem Sie einen zweiten Suchdienst in einem anderen Region bereitgestellt. Wie bei Verfügbarkeit während einer Wiederherstellung Index, muss die Umleitung oder -Logik Failover Code zugeordnet werden.

## <a name="increase-query-performance-with-replicas"></a>Erhöhen der Leistung von Abfragen mit Replikaten

Abfragewartezeit ist ein Indikator, dass zusätzliche Replikate erforderlich sind. Im Allgemeinen ist der erster Schritt zum Verbessern der abfrageleistung zum Hinzufügen weiterer dieser Ressource ein. Beim Hinzufügen von Replikaten zusätzliche Kopien des Indexes sind online geschaltet zum Vergrößern Abfrage Auslastung unterstützen und zum Laden die Anfragen über mehrere Replikate abzuwägen. 

Wir können nicht auf Abfragen pro Sekunde (QPS) Festplatte schätzt erläutern die notwendigen: Abfrage Leistung hängt von der Komplexität der Abfrage und verschiedenen Auslastung. Im Durchschnitt kein Replikat an Basic oder S1 SKUs kann etwa 15 QPS service, steht aber Ihre Durchsatz höhere oder niedrigere je nach Komplexität der Abfrage (facettierte Abfragen sind komplexere) und Wartezeit Netzwerk aus. Darüber hinaus ist es wichtig zu erkennen, dass während Replikate hinzufügen auf jeden Fall Skalierung und Performance hinzugefügt werden, das Ergebnis nicht unbedingt linearen ist: Hinzufügen von 3 Replikaten garantiert keine dreifach Durchsatz. 

Weitere Informationen zu QPS, einschließlich Ansätze für die Schätzung QPS für Ihre Last, finden Sie unter [Verwalten der Suchdienst](search-manage.md).

## <a name="increase-indexing-performance-with-partitions"></a>Erhöhen der Leistung bei der Indizierung mit Partitionen

Suchen von Applications, die in der Nähe Echtzeitdaten aktualisieren erfordern, proportional weitere Partitionen als Replikate benötigen. Hinzufügen von Partitionen verteilt Lese-und Schreibzugriff Vorgänge, über eine größere Anzahl von Ressourcen berechnen. Darüber hinaus haben Sie mehr Speicherplatz zum Speichern von zusätzlichen Indizes und Dokumente.

Größere Indizes dauert länger Abfrage. Als solche, finden Sie möglicherweise, dass jede inkrementell Vergrößerung Partitionen eine kleinere aber proportionale Erhöhung Replikate erforderlich ist. Wie bereits zuvor erwähnt, wird die Komplexität der Ihre Abfragen und die Lautstärke der Abfrage in wie schnell die Ausführung einer Abfrage, um aktiviert ist Faktor.

## <a name="basic-tier-partition-and-replica-combinations"></a>Grundlegende Ebene: Partition und Kopie Kombinationen

Ein Standard-Service können Sie genau 1 Partition haben und der 3 Software Update Services bis zu 3 Replikations für bis zu beschränken. Nur anpassbare Ressource ist Replikate. Wie bereits zuvor erwähnt, benötigen Sie mindestens 2 Replikate für hohen Verfügbarkeit für Abfragen.

<a id="chart"></a>
## <a name="standard-tiers-partition-and-replica-combinations"></a>Standard-Ebenen: Partition und Kopie Kombinationen

Diese Tabelle zeigt die Suche Einheiten als erforderlich, um Kombinationen aus Replikate und Partitionen, unterliegen der Maßeinheit 36 Suchen unterstützen (SU) bestehen, für alle standardmäßigen Ebenen. 

 - |**1-partition** |**2 Partitionen** |**3 Partitionen** |**4 Partitionen**|**6 Partitionen**|**12 Partitionen**|
---|----|---|---|---|---|---|
**1 Replikat**|1 SU|2 SU|3 SU|4 SU|6 SU|12 SU|
**2 Replikate**|2 SU|4 SU|6 SU|8 SU|12 SU|24 SU|
**3 Replikate**|3 SU|6 SU|9 SU|12 SU|18 SU|36 SU|
**4 Replikate**|4 SU|8 SU <|12 SU|16 SU|24 SU|N /|
**5 Replikate**|5 SU|10 SU|15 SU|20 SU|30 SU|N/V|
**6 Replikate**|6 SU|12 SU|18 SU|24 SU|36 SU|N/V|
**12 Replikate**|12 SU|24 SU|36 SU|N/V|N/V|N/V|

Suche Einheiten, Preise und Kapazität werden auf der Azure-Website ausführlich behandelt. Weitere Informationen finden Sie unter [Preise Details](https://azure.microsoft.com/pricing/details/search/) .

> [AZURE.NOTE] Die Anzahl der Replikate und Partitionen unterteilt gleichmäßig in 12 (insbesondere 1, 2, 3, 4, 6, 12). Dies liegt daran Azure Suche jeden Index in 12 mehrere Shards hinweg vorab aufteilt, damit es in gleich Teilen auf alle Partitionen verteilt werden kann. Angenommen, wenn der Dienst drei Partitionen weist, und Sie einen neuen Index erstellen, enthält jede Partition 4 mehrere Shards hinweg des Indexes. Wie Azure Suche mehrere Shards hinweg einen Index ist ein Implementierungsdetail, Ankündigung geändert in Zukunft lassen Sie wieder los. Obwohl die Zahl 12 heute ist, sollte nicht die Zahl immer 12 in der Zukunft liegen erwartet.

## <a name="billing-formula-for-replica-and-partition-resources"></a>Formel für Replikat und Partition Ressourcen Abrechnung

Die Formel zum Berechnen, wie viele SUs für bestimmte Kombinationen verwendet werden, ist das *Produkt* der Replikate und Partitionen, oder (R X P = SU). Beispielsweise 3 multipliziert mit 3 Partitionen Replikate als 9 SUs belastet wird.

Kosten pro SU wird durch die Ebenen, bei einer unteren pro Einheit Abrechnung Abschlag (Disagio) Basic als für Standard bestimmt. Klicken Sie auf [Details Preise](https://azure.microsoft.com/pricing/details/search/)können Sätzen für jede Ebene gefunden werden.

