<properties
 pageTitle="Größen für Cloud Services | Microsoft Azure"
 description="Listet die verschiedenen virtuellen Computern Größen (IDs) für Azure-Cloud-Dienst Web- und Worker Rollen."
 services="cloud-services"
 documentationCenter=""
 authors="Thraka"
 manager="timlt"
 editor=""/>
<tags
 ms.service="cloud-services"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="tbd"
 ms.date="10/27/2016"
 ms.author="adegeo"/>

# <a name="sizes-for-cloud-services"></a>Größen für Cloud-Dienste

In diesem Thema werden die verfügbaren Größen und Optionen für Cloud-Dienst Rolleninstanzen (Webrollen und Worker-Rollen). Darüber hinaus Aspekte beim Bereitstellen, um bei der Planung verwenden Sie diese Ressourcen berücksichtigt werden. Jeder Größe hat eine ID, die Sie in der [Definition Dienstdatei](cloud-services-model-and-package.md#csdef)eingefügt werden.

Cloud Services ist der verschiedene Ressourcentypen berechnen von Azure angeboten. Klicken Sie auf [hier](cloud-services-choose-me.md) Weitere Informationen zum Cloud-Dienste.

> [AZURE.NOTE]Verwandte Azure Grenzwerte finden Sie unter [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](../azure-subscription-service-limits.md)

## <a name="sizes-for-web-and-worker-role-instances"></a>Größen für Web- und Worker Rolleninstanzen

Es gibt mehrere standard Größen aus auf Azure auswählen. Für einige dieser Formate Aspekte umfassen:

* D-Serie virtuellen Computern sollen Programme ausführen, die höhere berechnen Power und Leistung temporärer Speicherplatz anfordern. D-Serie virtuellen Computern bieten schnellere Prozessoren, ein höheres Arbeitsspeicher-to-Core Verhältnis sowie eine State Drive (SSD) für den temporären Datenträger. Details finden Sie in der Ankündigung Azure Blog, [Neuen D-Serie virtuellen Computern Größen](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

* Dv2-Serie, einem Nachfolger der ursprünglichen D-Serie, sind mit eine leistungsfähigere CPU. Die CPU Dv2-Serie ist etwa 35 % schneller als die CPU D-Serie. Er basiert auf der neuesten Generation 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell)-Prozessor und Intel Turbo steigern Technologie 2.0, bis zu 3.1 GHz wechseln können. Der Dv2-Serie weist dieselben Arbeitsspeicher und Konfigurationen als der D-Serie.

*   G-Serie virtuellen Computern bieten den meisten Arbeitsspeicher und Hosts, auf denen Intel Xeon E5 V3 familiäre Prozessoren ausgeführt.

*   Die A-Serie virtuellen Computern können auf einer Vielzahl von Hardware-Dateitypen und -Prozessoren bereitgestellt werden. Die Größe beschränkt auf Grundlage der Hardware, die Leistung konsistent Prozessoren für die laufenden Instanz, unabhängig von der Hardware anbieten, die an bereitgestellt werden. Fragen Sie ab, um die physische Hardware ermitteln möchten, auf der dieser Größe bereitgestellt wird, die virtuelle Hardware von innerhalb des virtuellen Computers.

*   Die Größe A0 ist zu viel abonnierten auf der physischen Hardware. Für diese bestimmte Größe möglicherweise anderen Bereitstellungen von Kunden die Leistung Ihrer laufenden Arbeitsbelastung auswirken. Die relative Leistung ist die erwarteten Basisplan, unterliegen einer ungefähren Streuung von 15 % unten beschrieben.


Die Größe des virtuellen Computers wirkt sich die Preise. Die Größe wirkt sich auch die Verarbeitung, Speicher, und Speicherkapazität des virtuellen Computers. Kosten für die Speicherung werden separat basierend auf verwendete Seiten im Speicherkonto berechnet. Details finden Sie unter [Virtuellen Computern Preise Details](https://azure.microsoft.com/pricing/details/virtual-machines/) und [Azure Speicher Preise](https://azure.microsoft.com/pricing/details/storage/). 


Unter folgenden Aspekten können Ihnen bei der Entscheidung für eine Größe hilfreich:


* Die Größe der A8-A11 und H-Serie ist auch bekannt als *berechnen ankommt Instanzen*. Die Hardware, die diesen Größen ausgeführt wird entworfen und optimiert für berechnen ankommt oder Netzwerk ankommt Applications, einschließlich High Performance computing (HPC) cluster-Anwendungen, modellieren und Simulationen. Die Reihe A8-A11 verwendet Intel Xeon E5-2670 @ 2,6 GHZ und der Serie H verwendet Intel Xeon E5-2667 v3 @ 3,2 GHz. Ausführliche Informationen und Hinweise zur Verwendung dieser Auswahl an Papiergrößen, finden Sie unter [der virtuellen Computern H-Serie und berechnen ankommt A-Serie](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md). 

* Dv2-Serie, D-Serie, G-Serie, eignen sich optimal für Applikationen, die schnellere CPUs verlangen, bessere lokale Festplatte Leistung oder höhere Arbeitsspeicher erfordert haben.  Sie bieten eine leistungsfähige Kombination für viele unternehmensweite Applikationen.

*   Einige der physischen Hosts in Azure Data Center möglicherweise nicht virtuellen Computern größere wie A5 – A11 unterstützt. Daher sehen Sie möglicherweise die Fehlermeldung **fehlgeschlagen virtuellen Computern {Computernamen} konfigurieren** oder **zum Erstellen von virtuellen Computern {Computernamen} fehlgeschlagen** beim Ändern der Größe von vorhandenen virtuellen Computers eine neue Größe; Erstellen eines neuen virtuellen Computers in einem virtuellen Netzwerk vor dem 16 April 2013 erstellt; oder Hinzufügen eines neuen virtuellen Computers zu einem vorhandenen Clouddienst. Finden Sie unter [Fehler: "Fehler beim Konfigurieren von virtuellen Computern"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) im Support-Forum zur Umgehung für jedes Bereitstellungsszenario des Problems.  

* Ihr Abonnement möglicherweise auch die Anzahl der Kerne einschränken, die Sie in bestimmte Größe Familien bereitstellen können. Um eine Kontingent zu erhöhen, wenden Sie sich an Azure-Support.


## <a name="performance-considerations"></a>Leistungsaspekte

Wir haben des Konzepts der der Azure berechnen Einheit (ACU) der Vergleich der Leistung berechnen (CPU) für Azure SKUs zu ermöglichen. Dies hilft Ihnen der SKU wahrscheinlich Indexeigenschaften Leistung erfüllen ist einfach zu identifizieren.  ACU wird derzeit auf ein kleines (Standard_A1) standardisiert virtueller Computer 100 und alle anderen SKUs dann wird darstellen ungefähr wie viel schneller, dass SKU eine Standardansicht Richtlinie ausgeführt werden kann. 

>[AZURE.IMPORTANT] Das ACU ist nur eine Richtlinie.  Die Ergebnisse für Ihre Arbeitsbelastung können variieren. 

<br>

|SKU-Familie |ACU/Core |
|---|---|
|[Standard_A0](#a-series)   |50 |
|[Standard_A1-4](#a-series) |100 |
|[Standard_A5-7](#a-series) |100 |
|[A8-A11](#a-series)    |225 *|
|[D1-14](#d-series) |160 |
|[D1-15v2](#dv2-series) |210 – 250 *|
|[G1-5](#g-series)  |180 - 240 *|
|[H](#h-series) |290 - 300 *|

ACUs markiert mit einem * Intel® Turbo Technologie CPU Häufigkeit erhöhen und bieten eine Steigerung der Leistung verwenden.  Die Menge der der steigern kann basierend auf der virtueller Speicher, Arbeitsbelastung und aufgrund der Ergebnisse auf dem gleichen Host unterschiedlich.

## <a name="size-tables"></a>Von Größentabellen

Die folgende Tabelle enthält die Größen und den von Ihnen bereitgestellten belasten.

* Speicherkapazität wird angezeigt, in einer anderen Einheit GiB oder 1024 ^ 3 Bytes. Wenn in GB gemessen Vergleich der Datenträger (1000 ^ 3 Bytes) in Datenträger gemessen in GiB (1024 ^ 3) Denken Sie daran, dass Kapazitätsnummern, die in GiB möglicherweise kleiner angezeigt. Beispielsweise 1023 GiB = 1098.4 GB

* Datenträgerdurchsatz wird in/a-Vorgänge pro Sekunde (IOPS) gemessen und/s, / s = 10 ^ 6 Bytes/s.

* Daten Datenträger können im Cache oder nicht-Cache-Modus ausgeführt werden. Der Host-Cache-Modus wird für zwischengespeicherte Datenvorgangs auf dem Datenträger **schreibgeschützt** oder **Lesen/Schreiben**festgelegt.  Für den Vorgang nicht gespeicherte Daten Datenträger wird der Host-Cache-Modus **keine**festgelegt.

* Maximale Bandbreite ist die maximale aggregierte Bandbreite zugewiesen und pro virtueller Computer Typ zugewiesen. Die maximale Bandbreite bietet Hinweise zur Auswahl des richtigen virtuellen Computer um sicherzustellen, dass ausreichend Netzwerkkapazität verfügbar ist. Beim Navigieren zwischen den niedrig, Mittel, hoch und sehr hoch, wird entsprechend den Durchsatz zu erhöhen. Ist-Netzwerk-Performance hängt von viele Faktoren, einschließlich Netzwerk- und Anwendung geladen und Netzwerkeinstellungen Anwendung.


## <a name="a-series"></a>A-Serie

| Größe        | CPUs | Arbeitsspeicher: GiB | Lokale Festplatte: GiB | Max Daten Datenträger | Max Daten Datenträgerdurchsatz: IOPS | Max NICs / Netzwerk Bandbreite |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A0 | 1         | 0.768        | 20                    | 1              | 1 x 500              | 1 / niedrig                   |
| Standard_A1 | 1         | 1,75         | 70                    | 2              | 2 x 500              | 1 / moderieren              |
| Standard_A2 | 2         | 3,5 GB       | 135                   | 4              | 4 x 500              | 1 / moderieren              |
| Standard_A3 | 4         | 7            | 285                   | 8              | 8 x 500              | 2 / hoch                  |
| Standard_A4 | 8         | 14           | 605                   | 16             | 16 x 500             | 4 / hoch                  |
| Standard_A5 | 2         | 14           | 135                   | 4              | 4 X 500              | 1 / moderieren              |
| Standard_A6 | 4         | 28           | 285                   | 8              | 8 x 500              | 2 / hoch                  |
| Standard_A7 | 8         | 56           | 605                   | 16             | 16 x 500             | 4 / hoch                  |

## <a name="a-series---compute-intensive-instances"></a>A-Serie-rechenintensiven Instanzen

Informationen und Hinweise zur Verwendung dieser Auswahl an Papiergrößen, finden Sie unter [der virtuellen Computern H-Serie und berechnen ankommt A-Serie](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).


| Größe         | CPUs | Arbeitsspeicher: GiB | Lokale Festplatte: GiB | Max Daten Datenträger | Max Daten Datenträgerdurchsatz: IOPS | Max NICs / Netzwerk Bandbreite |
|--------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A8 * | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / hoch                  |
| Standard_A9 * | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / sehr hoch             |
| Standard_A10 | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / hoch                  |
| Standard_A11 | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / sehr hoch             |

* RDMA in

## <a name="d-series"></a>D-Serie


| Größe         | CPUs | Arbeitsspeicher: GiB | Lokale SSD: GiB | Max Daten Datenträger | Max Daten Datenträgerdurchsatz: IOPS | Max NICs / Netzwerk Bandbreite |
|--------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1  | 1         | 3.5          | 50                   | 2              | 2 x 500              | 1 / moderieren              |
| Standard_D2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / hoch                  |
| Standard_D3  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / hoch                  |
| Standard_D4  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / hoch                  |
| Standard_D11 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / hoch                  |
| Standard_D12 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / hoch                  |
| Standard_D13 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / hoch                  |
| Standard_D14 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / sehr hoch             |

## <a name="dv2-series"></a>Dv2-Serie

| Größe            | CPUs | Arbeitsspeicher: GiB | Lokale SSD: GiB | Max Daten Datenträger | Max Daten Datenträgerdurchsatz: IOPS | Max NICs / Netzwerk Bandbreite |
|-----------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1_v2  | 1         | 3.5          | 50                   | 2              | 2 x 500              | 1 / moderieren              |
| Standard_D2_v2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / hoch                  |
| Standard_D3_v2  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / hoch                  |
| Standard_D4_v2  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / hoch                  |
| Standard_D5_v2  | 16        | 56           | 800                  | 32             | 32 x 500             | 8 / extrem hoch        |
| Standard_D11_v2 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / hoch                  |
| Standard_D12_v2 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / hoch                  |
| Standard_D13_v2 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / hoch                  |
| Standard_D14_v2 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / extrem hoch        |
| Standard_D15_v2 | 20        | 140          | 1.000                | 40             | 40 x 500             | 8 / extrem hoch        |

## <a name="g-series"></a>G-Serie

| Größe        | CPUs | Arbeitsspeicher: GiB  | Lokale SSD: GiB  | Max Daten Datenträger | Max Datenträgerdurchsatz: IOPS | Max NICs / Netzwerk Bandbreite |
|-------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_G1 | 2         | 28           | 384                  | 4              | 4 x 500            | 1 / hoch                  |
| Standard_G2 | 4         | 56           | 768                  | 8              | 8 x 500            | 2 / hoch                  |
| Standard_G3 | 8         | 112          | 1.536                | 16             | 16 x 500           | 4 / sehr hoch             |
| Standard_G4 | 16        | 224          | 3,072                | 32             | 32 x 500           | 8 / extrem hoch        |
| Standard_G5 | 32        | 448          | 6,144                | 64             | 64 x 500           | 8 / extrem hoch        |


## <a name="h-series"></a>H-Serie

Azure-H-Serie virtuellen Computern sind die nächsten Generation hohe Performance computing, dass virtuellen Computern auf berechnete high-End-Anforderungen, wie molekulare Modellierung und berechnete Pneumatik / Dynamics gerichtet. Diese 8 und 16 Core virtuellen Computern sind auf der Intel Haswell E5-2667 V3 Prozessor Technologie mit DDR4 Speicher und lokale SSD basierend integriert. 

Neben wesentlichen CPU-Leistung bietet die H-Serie unterschiedliche Optionen für niedrige Wartezeit RDMA Netzwerke FDR InfiniBand und mehrere Arbeitsspeicherkonfigurationen zur Unterstützung von Arbeitsspeicher stark berechnete Anforderungen verwenden.


| Größe           | CPUs | Arbeitsspeicher: GiB | Lokale SSD: GiB | Max Daten Datenträger | Max Datenträgerdurchsatz: IOPS | Max NICs / Netzwerk Bandbreite |
|----------------|-----------|-------------|--------------------------|----------------|---------------------------|------------------------------|
| Standard_H8    | 8         | 56          | 1000                     | 16             | 16 x 500                    | 8 / hoch                      |
| Standard_H16   | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / sehr hoch                  |
| Standard_H8m   | 8         | 112         | 1000                     | 16             | 16 x 500                    | 8 / hoch                      |
| Standard_H16m  | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / sehr hoch                 |
| Standard_H16r * | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / sehr hoch                  |
| Standard_H16mr * | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / sehr hoch                  |


* RDMA in

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Hinweise: Standard A0 - A4 mit CLI und PowerShell 

In der klassischen Bereitstellungsmodell sind einige virtueller Computer Größe Namen weicht in CLI und PowerShell:

* Standard_A0 ist ExtraSmall 
* Standard_A1 ist klein
* Standard_A2 ist "Mittel"
* Standard_A3 ist groß
* Standard_A4 ist ExtraLarge

## <a name="configure-sizes-for-cloud-services"></a>Konfigurieren von Größen für Cloud-Dienste

Sie können die Größe des virtuellen Computers für eine Instanz der Rolle als Teil des Service-Modells der [Definition Dienstdatei](cloud-services-model-and-package.md#csdef)beschriebenen angeben. Die Größe der Rolle bestimmt die Anzahl der CPU-Kerne, die Speicherkapazität und die lokale System Dateigröße, die auf einer laufenden Instanz zugeordnet wird. Wählen Sie die Rolle Größe auf Grundlage Ihrer Anwendung Ressource mehr aus.

Hier ist ein Beispiel für das Festlegen der Größe der Rolle [Standard_D2](#general-purpose-d) für eine Instanz der Web-Rolle sein:

```xml
<WorkerRole name="Worker1" vmsize="<mark>Standard_D2</mark>">
...
</WorkerRole>
```

## <a name="next-steps"></a>Nächste Schritte

- Informationen Sie zu [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](../azure-subscription-service-limits.md).
- Erfahren Sie mehr [über die virtuellen Computern H-Serie und berechnen ankommt A-Serie](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) für Auslastung wie High Performance Computing (HPC).

