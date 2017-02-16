


Es gibt mehrere standard Größen aus auf Azure auswählen. Für einige dieser Formate Aspekte umfassen:

*   D-Serie virtuellen Computern sollen Programme ausführen, die höhere berechnen Power und Leistung temporärer Speicherplatz anfordern. D-Serie virtuellen Computern bieten schnellere Prozessoren, ein höheres Arbeitsspeicher-to-Core Verhältnis sowie eine State Drive (SSD) für den temporären Datenträger. Details finden Sie in der Ankündigung Azure Blog, [Neuen D-Serie virtuellen Computern Größen](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

*   Dv2-Serie, einem Nachfolger der ursprünglichen D-Serie, sind mit eine leistungsfähigere CPU. Die CPU Dv2-Serie ist etwa 35 % schneller als die CPU D-Serie. Er basiert auf der neuesten Generation 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell)-Prozessor und Intel Turbo steigern Technologie 2.0, bis zu 3.1 GHz wechseln können. Der Dv2-Serie weist dieselben Arbeits- und Festplattenspeicher Konfigurationen als der D-Serie.

* F-Serie basiert auf der 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) Prozessor, die so weit wie 3.1 GHz Intel Turbo steigern Technologie 2.0 mit einer Geschwindigkeit erreichen kann. Dies ist die gleichen CPU-Leistung als die Dv2-Reihe von virtuellen Computern.  Zu einem unteren Listenpreis pro Stunde ist der F-Serie den optimalen Preis-Leistungsabfall in Azure Portfolio basierend auf den Azure berechnen Einheit (ACU) pro Core. 

    Der F-Serie führt außerdem einen neuen Standard in virtueller Speicher für Azure benennen. Für diese Serie und virtueller Computer veröffentlicht Größen in der Zukunft den numerischen Wert nach der Familienname Buchstaben, der die Anzahl der CPUs entsprechen wird. Zusätzliche Funktionen werden wie Storage Premium optimiert von Buchstaben folgen die Anzahl der numerische CPU Core bestimmt werden. Dieses Namensformat für zukünftige virtueller Computer Größen freigegeben verwendet werden, aber rückwirkend ändert sich nicht die Namen der alle vorhandenen virtuellen Computer Größen der freigegeben wurden.


*   G-Serie virtuellen Computern bieten den meisten Arbeitsspeicher und Hosts, auf denen Intel Xeon E5 V3 familiäre Prozessoren ausgeführt.


*   Verwenden DS-Serie, DSv2-Serie, Fs-Serie und GS-Serie virtuellen Computern kann Premium-Speicher, die leistungsfähige und niedrig Wartezeiten Speicher für e/a-stark Auslastung bereitstellt. Diese virtuellen Computern Formular Hosten des virtuellen Computers Datenträger und auch Bereitstellen eines lokalen Festplatten-Caches SSD Solid Laufwerken (SSDs) mit. Premium-Speicher steht in bestimmte Regionen zur Verfügung. Details finden Sie unter [Premium Speicher: leistungsstarke Storage für Azure-virtuellen Computern Auslastung](../articles/storage/storage-premium-storage.md).


*   Die A-Serie virtuellen Computern können auf einer Vielzahl von Hardware-Dateitypen und -Prozessoren bereitgestellt werden. Die Größe beschränkt auf Grundlage der Hardware, die Leistung konsistent Prozessoren für die laufenden Instanz, unabhängig von der Hardware anbieten, die an bereitgestellt werden. Fragen Sie ab, um die physische Hardware ermitteln möchten, auf der dieser Größe bereitgestellt wird, die virtuelle Hardware von innerhalb des virtuellen Computers.

*   Die Größe A0 ist zu viel abonnierten auf der physischen Hardware. Für diese bestimmte Größe können anderen Bereitstellungen von Kunden die Leistung von Ihrer laufenden Arbeitsbelastung auswirken. Die relative Leistung ist die erwarteten Basisplan, unterliegen einer ungefähren Streuung von 15 % unten beschrieben.


Die Größe des virtuellen Computers wirkt sich die Preise. Die Größe wirkt sich auch die Verarbeitung, Speicher, und Speicherkapazität des virtuellen Computers. Kosten für die Speicherung werden separat basierend auf verwendete Seiten im Speicherkonto berechnet. Details finden Sie unter [Virtuellen Computern Preise Details](https://azure.microsoft.com/pricing/details/virtual-machines/) und [Azure Speicher Preise](https://azure.microsoft.com/pricing/details/storage/). 


Unter folgenden Aspekten können Ihnen bei der Entscheidung für eine Größe hilfreich:


* Die Größe der A8-A11 und H-Serie ist auch bekannt als *rechenintensiven Instanzen*. Die Hardware, die diesen Größen ausgeführt wird entworfen und optimiert für berechnen ankommt oder Netzwerk ankommt Applications, einschließlich High Performance computing (HPC) cluster-Anwendungen, modellieren und Simulationen. Die Reihe A8-A11 verwendet Intel Xeon E5-2670 @ 2,6 GHZ und der Serie H verwendet Intel Xeon E5-2667 v3 @ 3,2 GHz. Ausführliche Informationen und Hinweise zur Verwendung von diese Auswahl an Papiergrößen, finden Sie unter [der virtuellen Computern H-Serie und berechnen ankommt A-Serie](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md). 



* Dv2-Serie, D-Serie, G-Serie, und die DS/GS Gegenstücken eignen sich optimal für Applikationen, die schnellere CPUs verlangen, besser lokale Festplatte Leistung oder höhere Arbeitsspeicher erfordert haben.  Sie bieten eine leistungsfähige Kombination für viele unternehmensweite Applikationen.

* Die F-Serie virtuellen Computern sind eine geeignete Auswahl für Auslastung, die fordern schnellere CPUs, aber ist nicht erforderlich, wie viel Speicher oder lokale SSD pro CPU Core.  Beispielsweise für Analytics, Spiele-Server, Webservern und Stapelverarbeitung profitieren von den Wert der F-Serie.

*   Einige der physischen Hosts in Azure Data Center möglicherweise nicht virtuellen Computern größere wie A5 – A11 unterstützt. Daher sehen Sie möglicherweise die Fehlermeldung **Fehler beim Konfigurieren von virtuellen Computern <machine name> ** oder **Fehler beim Erstellen von virtuellen Computern <machine name> ** beim Ändern der Größe von vorhandenen virtuellen Computers eine neue Größe; Erstellen eines neuen virtuellen Computers in ein virtuelles Netzwerk erstellt werden, bevor Sie 16 April 2013; oder Hinzufügen eines neuen virtuellen Computers zu einem vorhandenen Clouddienst. Finden Sie unter [Fehler: "Fehler beim Konfigurieren von virtuellen Computern"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) im Support-Forum zur Umgehung des Problems für jedes Bereitstellungsszenario.  

* Ihr Abonnement möglicherweise auch die Anzahl der Kerne einschränken, die Sie in bestimmte Größe Familien bereitstellen können. Um eine Kontingent zu erhöhen, wenden Sie sich an Azure-Support.


## <a name="performance-considerations"></a>Leistungsaspekte

Wir haben das Konzept von der Azure berechnen Einheit (ACU) der Vergleich der Leistung berechnen (CPU) für Azure SKUs zu ermöglichen. Dies hilft Ihnen der SKU höchstwahrscheinlich Indexeigenschaften Leistung erfüllen ist einfach zu identifizieren.  ACU wird derzeit auf ein kleines (Standard_A1) standardisiert virtueller Computer 100 und alle anderen SKUs dann wird darstellen ungefähr wie viel schneller, dass SKU eine Standardansicht Richtlinie ausgeführt werden kann. 

>[AZURE.IMPORTANT] Das ACU ist nur eine Richtlinie.  Die Ergebnisse für Ihre Arbeitsbelastung können variieren. 

<br>

|SKU-Familie |ACU/Core |
|---|---|
|[Standard_A0](#a-series)   |50 |
|[Standard_A1-4](#a-series) |100 |
|[Standard_A5-7](#a-series) |100 |
|[A8-A11](#a-series)    |225 *|
|[D1-14](#d-series) |160 |
|[D1-15v2](#dv2-series) |210 - 250 *|
|[DS1-14](#ds-series)   |160 |
|[DS1-15v2](#dsv2-series)   |210 – 250 * |
|[F1-F16](#f-series) | 210 – 250 *|
|[F1s-F16s](#fs-series) | 210 – 250 *|
|[G1-5](#g-series)  |180 - 240 *|
|[GS1-5](#gs-series)    |180 - 240 *|
|[H](#h-series) |290 - 300 *|


ACUs markiert mit einem * Intel® Turbo Technologie CPU Häufigkeit erhöhen und bieten eine Steigerung der Leistung verwenden.  Die Menge des der steigern abhängig der virtueller Speicher, Arbeitsbelastung und aufgrund der Ergebnisse auf dem gleichen Host ausgeführt.

## <a name="size-tables"></a>Von Größentabellen

Die folgende Tabelle enthält die Größen und den von Ihnen bereitgestellten belasten.

* Speicherkapazität wird angezeigt, in einer anderen Einheit GiB oder 1024 ^ 3 Bytes. Wenn in GB gemessen Vergleich der Datenträger (1000 ^ 3 Bytes) in Datenträger gemessen in GiB (1024 ^ 3) Beachten Sie, dass Kapazitätsnummern, die in GiB kleiner angezeigt werden können. Beispielsweise 1023 GiB = 1098.4 GB

* Datenträgerdurchsatz wird in/a-Vorgänge pro Sekunde (IOPS) gemessen und/s, / s = 10 ^ 6 Bytes/s.

* Daten Datenträger können im Cache oder zwischengespeicherter Modus ausgeführt werden.  Der Host-Cache-Modus wird für zwischengespeicherte Datenvorgangs auf dem Datenträger **schreibgeschützt** oder **Lesen/Schreiben**festgelegt.  Für den Vorgang nicht gespeicherte Daten Datenträger wird der Host-Cache-Modus **keine**festgelegt.


* Maximale Bandbreite ist die maximale aggregierte Bandbreite zugewiesen und pro virtueller Computer Typ zugewiesen. Die maximale Bandbreite bietet Hinweise zur Auswahl des richtigen virtuellen Computer um sicherzustellen, dass ausreichend Netzwerkkapazität verfügbar ist. Beim Navigieren zwischen den niedrig, Mittel, hoch und sehr hoch, wird entsprechend den Durchsatz zu erhöhen. Ist-Netzwerk-Performance hängt von viele Faktoren, einschließlich Netzwerk- und Anwendung geladen und Netzwerkeinstellungen Anwendung.


## <a name="a-series"></a>A-Serie

| Größe        | CPUs | Arbeitsspeicher: GiB | Lokale Festplatte: GiB | Max Daten Datenträger | Max-Daten Datenträgerdurchsatz: IOPS | Max NICs / Netzwerk Bandbreite |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A0 | 1         | 0.768        | 20                    | 1              | 1 x 500              | 1 / niedrig                   |
| Standard_A1 | 1         | 1,75         | 70                    | 2              | 2 x 500              | 1 / moderieren              |
| Standard_A2 | 2         | 3,5 GB       | 135                   | 4              | 4 x 500              | 1 / moderieren              |
| Standard_A3 | 4         | 7            | 285                   | 8              | 8 x 500              | 2 / hoch                  |
| Standard_A4 | 8         | 14           | 605                   | 16             | 16 x 500             | 4 / hoch                  |
| Standard_A5 | 2         | 14           | 135                   | 4              | 4 X 500              | 1 / moderieren              |
| Standard_A6 | 4         | 28           | 285                   | 8              | 8 x 500              | 2 / hoch                  |
| Standard_A7 | 8         | 56           | 605                   | 16             | 16 x 500             | 4 / hoch                  |

<br>
## <a name="a-series---compute-intensive-instances"></a>A-Serie-rechenintensiven Instanzen

Informationen und Hinweise zur Verwendung dieser Auswahl an Papiergrößen, finden Sie unter [den virtuellen Computern H-Serie und berechnen ankommt A-Serie](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).



| Größe         | CPUs | Arbeitsspeicher: GiB | Lokale Festplatte: GiB | Max Daten Datenträger | Max-Daten Datenträgerdurchsatz: IOPS | Max NICs / Netzwerk Bandbreite |
|--------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A8 *  | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / hoch                  |
| Standard_A9 * | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / sehr hoch             |
| Standard_A10 | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / hoch                  |
| Standard_A11 | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / sehr hoch             |

* RDMA in

<br>
## <a name="d-series"></a>D-Serie


| Größe         | CPUs | Arbeitsspeicher: GiB | Lokale SSD: GiB | Max Daten Datenträger | Max-Daten Datenträgerdurchsatz: IOPS | Max NICs / Netzwerk Bandbreite |
|--------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1  | 1         | 3.5          | 50                   | 2              | 2 x 500              | 1 / moderieren              |
| Standard_D2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / hoch                  |
| Standard_D3  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / hoch                  |
| Standard_D4  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / hoch                  |
| Standard_D11 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / hoch                  |
| Standard_D12 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / hoch                  |
| Standard_D13 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / hoch                  |
| Standard_D14 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / sehr hoch             |

<br>
## <a name="dv2-series"></a>Dv2-Serie

| Größe            | CPUs | Arbeitsspeicher: GiB | Lokale SSD: GiB | Max Daten Datenträger | Max-Daten Datenträgerdurchsatz: IOPS | Max NICs / Netzwerk Bandbreite |
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

<br>
## <a name="ds-series"></a>DS-Serie *


| Größe          | CPUs | Arbeitsspeicher: GiB | Lokale SSD: GiB | Max Daten Datenträger | Zwischengespeicherter Datenträgerdurchsatz: IOPS / / s (Cachegröße in GiB) | Max entsprechende Datenträgerdurchsatz: IOPS / / s | Max NICs / Netzwerk Bandbreite |
|---------------|-----------|--------------|--------------------------------|----------------|--------------------------------------------|----------------------------------------------|-----------------------|
| Standard_DS1  | 1   | 3.5          | 7       | 2     | 4.000 / 32 (43)               | 3.200 / 32    | 1 / moderieren              |
| Standard_DS2  | 2   | 7            | 14      | 4     | 8.000 / 64 (86)               | 6.400 / 64    | 2 / hoch                  |
| Standard_DS3  | 4   | 14           | 28      | 8     | 16.000 / 128 (172)            | 12,800 / 128  | 4 / hoch                  |
| Standard_DS4  | 8   | 28           | 56      | 16    | 32.000 / 256 (344)            | 25,600 / 256  | 8 / hoch                  |
| Standard_DS11 | 2   | 14           | 28      | 4     | 8.000 / 64 (72)               | 6.400 / 64    | 2 / hoch                  |
| Standard_DS12 | 4   | 28           | 56      | 8     | 16.000 / 128 (144)            | 12,800 / 128  | 4 / hoch                  |
| Standard_DS13 | 8   | 56           | 112     | 16    | 32.000 / 256 (288)            | 25,600 / 256  | 8 / hoch                  |
| Standard_DS14 | 16  | 112          | 224     | 32    | 64.000 / 512 (576)            | 51,200 / 512  | 8 / sehr hoch             |

/ S = 10 ^ 6 Bytes pro Sekunde und GiB 1024 = ^ 3 Bytes.

* Die maximale Datenträgerdurchsatz (IOPS oder/s) möglich mit einer Datenreihe DS virtueller Computer möglicherweise, durch die Anzahl beschränkt werden, Größe und Striping, der die angefügten Datenträger.  Details finden Sie unter [Premium Speicher: leistungsstarke Storage für Azure-virtuellen Computern Auslastung](../articles/storage/storage-premium-storage.md).



<br>
## <a name="dsv2-series"></a>DSv2-Serie *


| Größe             | CPUs | Arbeitsspeicher: GiB | Lokale SSD: GiB | Max Daten Datenträger | Zwischengespeicherter Datenträgerdurchsatz: IOPS / / s (Cachegröße in GiB) | Max entsprechende Datenträgerdurchsatz: IOPS / / s | Max NICs / Netzwerk Bandbreite |
|------------------|-----------|--------------|---------------------------|----------------|-------------------------------------------------|-------------------------------------------------|------------------------------|
| Standard_DS1_v2  | 1         | 3.5          | 7                         | 2              | 4.000 / 32 (43)                        | 3.200 / 48                                 | 1 moderieren                   |
| Standard_DS2_v2  | 2         | 7            | 14                        | 4              | 8.000 / 64 (86)                        | 6.400 / 96                                 | hohe 2                       |
| Standard_DS3_v2  | 4         | 14           | 28                        | 8              | 16.000 / 128 (172)                     | 12,800 / 192                               | 4 hoch                       |
| Standard_DS4_v2  | 8         | 28           | 56                        | 16             | 32.000 / 256 (344)                     | 25,600 / 384                               | hohe 8                       |
| Standard_DS5_v2  | 16        | 56           | 112                       | 32             | 64.000 / 512 (688)                     | 51,200 / 768                               | extrem hohe 8             |
| Standard_DS11_v2 | 2         | 14           | 28                        | 4              | 8.000 / 64 (72)                        | 6.400 / 96                                 | hohe 2                       |
| Standard_DS12_v2 | 4         | 28           | 56                        | 8              | 16.000 / 128 (144)                     | 12,800 / 192                               | 4 hoch                       |
| Standard_DS13_v2 | 8         | 56           | 112                       | 16             | 32.000 / 256 (288)                     | 25,600 / 384                               | hohe 8                       |
| Standard_DS14_v2 | 16        | 112          | 224                       | 32             | 64.000 / 512 (576)                     | 51,200 / 768                               | extrem hohe 8             |
| Standard_DS15_v2 | 20        | 140 GB       | 280                       | 40             | 80.000 / 640 (720)                     | 64.000 / 960                               | extrem hohe 8             |

/ S = 10 ^ 6 Bytes pro Sekunde und GiB 1024 = ^ 3 Bytes.

* Die maximale Datenträgerdurchsatz (IOPS oder/s) möglich mit einer Datenreihe DSv2 virtueller Computer möglicherweise, durch die Anzahl beschränkt werden, Größe und Striping, der die angefügten Datenträger.  Details finden Sie unter [Premium Speicher: leistungsstarke Storage für Azure-virtuellen Computern Auslastung](../articles/storage/storage-premium-storage.md).


<br>
## <a name="f-series"></a>F-Serie


| Größe         | CPUs | Arbeitsspeicher: GiB | Lokale SSD: GiB | Max Daten Datenträger | Max Datenträgerdurchsatz: IOPS | Max NICs / Netzwerk Bandbreite |
|--------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_F1  | 1         | 2            | 16                   | 2              | 2 x 500              | 1 / moderieren              |
| Standard_F2  | 2         | 4            | 32                   | 4              | 4 x 500              | 2 / hoch                  |
| Standard_F4  | 4         | 8            | 64                   | 8              | 8 x 500              | 4 / hoch                  |
| Standard_F8  | 8         | 16           | 128                  | 16             | 16 x 500             | 8 / hoch                  |
| Standard_F16 | 16        | 32           | 256                  | 32             | 32 x 500             | 8 / extrem hoch        |

<br>
## <a name="fs-series"></a>FS-Serie *

| Größe             | CPUs | Arbeitsspeicher: GiB | Lokale SSD: GiB | Max Daten Datenträger | Zwischengespeicherter Datenträgerdurchsatz: IOPS / / s (Cachegröße in GiB) | Max entsprechende Datenträgerdurchsatz: IOPS / / s | Max NICs / Netzwerk Bandbreite |
|---------------|-------|-----|----------|--------|------------------------------|---------------------------------|---------------|
| Standard_F1s  | 1     | 2   | 4        | 2      | 4.000 / 32 (12)         | 3.200 / 48        | 1 / moderieren       |
| Standard_F2s  | 2     | 4   | 8        | 4      | 8.000 / 64 (24)         | 6.400 / 96        | 2 / hoch           |
| Standard_F4s  | 4     | 8   | 16       | 8      | 16.000 / 128 (48)       | 12,800 / 192      | 4 / hoch           |
| Standard_F8s  | 8     | 16  | 32       | 16     | 32.000 / 256 (96)       | 25,600 / 384      | 8 / hoch           |
| Standard_F16s | 16    | 32  | 64       | 32     | 64.000 / 512 (192)      | 51,200 / 768      | 8 / extrem hoch |

/ S = 10 ^ 6 Bytes pro Sekunde und GiB 1024 = ^ 3 Bytes.

* Die maximale Datenträgerdurchsatz (IOPS oder/s) möglich mit einer Datenreihe Fs virtueller Computer möglicherweise, durch die Anzahl beschränkt werden, Größe und Striping, der die angefügten Datenträger.  Details finden Sie unter [Premium Speicher: leistungsstarke Storage für Azure-virtuellen Computern Auslastung](../articles/storage/storage-premium-storage.md).


<br>
## <a name="g-series"></a>G-Serie

| Größe        | CPUs | Arbeitsspeicher: GiB  | Lokale SSD: GiB  | Max Daten Datenträger | Max Datenträgerdurchsatz: IOPS | Max NICs / Netzwerk Bandbreite |
|-------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_G1 | 2         | 28           | 384                  | 4              | 4 x 500            | 1 / hoch                  |
| Standard_G2 | 4         | 56           | 768                  | 8              | 8 x 500            | 2 / hoch                  |
| Standard_G3 | 8         | 112          | 1.536                | 16             | 16 x 500           | 4 / sehr hoch             |
| Standard_G4 | 16        | 224          | 3,072                | 32             | 32 x 500           | 8 / extrem hoch        |
| Standard_G5 | 32        | 448          | 6,144                | 64             | 64 x 500           | 8 / extrem hoch        |



<br>
## <a name="gs-series"></a>GS-Serie *


| Größe         | CPUs | Arbeitsspeicher: GiB | Lokale SSD: GiB | Max Daten Datenträger | Zwischengespeicherter Datenträgerdurchsatz: IOPS / / s (Cachegröße in GiB) | Max entsprechende Datenträgerdurchsatz: IOPS / / s | Max NICs / Netzwerk Bandbreite |
|--------------|-----------|--------------|---------------------------|--------------------------------|----------------|--------------------------------------------|----------------------------------------------|-----------------------|
| Standard_GS1 | 2         | 28      | 56       | 4   | 10.000 / 100 (264)       | 5.000 / 125     | 1 / hoch                  |
| Standard_GS2 | 4         | 56      | 528      | 8   | 20.000 / 200 (528)       | 10.000 / 250    | 2 / hoch                  |
| Standard_GS3 | 8         | 112     | 1056    | 16  | 40.000 / 400 (1056)     | 20.000 / 500    | 4 / sehr hoch             |
| Standard_GS4 | 16        | 224     | 2,112    | 32  | 80.000 / 800 (2,112)     | 40.000 / 1.000  | 8 / extrem hoch        |
| Standard_GS5 | 32        | 448     | 4,224    | 64  | 160.000 / 1.600 (4,224)  | 80.000 / 2.000  | 8 / extrem hoch        |

/ S = 10 ^ 6 Bytes pro Sekunde und GiB 1024 = ^ 3 Bytes.

* Die maximale Datenträgerdurchsatz (IOPS oder/s) möglich mit einer Datenreihe GS virtueller Computer möglicherweise, durch die Anzahl beschränkt werden, Größe und Striping, der die angefügten Datenträger. 

<br>
## <a name="h-series"></a>H-Serie

Azure H-Serie-virtuellen Computern sind die nächsten Generation hohe Performance computing, dass virtuellen Computern auf berechnete high-End-Anforderungen, wie molekulare Modellierung und berechnete Pneumatik / Dynamics gerichtet. Diese 8 und 16 Core virtuellen Computern sind auf der Intel Haswell E5-2667 V3 Prozessor Technologie mit DDR4 Speicher und lokale SSD basierend integriert. 

Zusätzlich zu den wesentlichen CPU Power bietet die H-Serie unterschiedliche Optionen für niedrige Wartezeit RDMA Netzwerke FDR InfiniBand und mehrere Arbeitsspeicherkonfigurationen zur Unterstützung von Arbeitsspeicher stark berechnete Anforderungen verwenden.


| Größe           | CPUs | Arbeitsspeicher: GiB | Lokale SSD: GiB | Max Daten Datenträger | Max Datenträgerdurchsatz: IOPS | Max NICs / Netzwerk Bandbreite |
|----------------|-----------|-------------|--------------------------|----------------|---------------------------|------------------------------|
| Standard_H8    | 8         | 56          | 1000                     | 16             | 16 x 500                    | 8 / hoch                      |
| Standard_H16   | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / sehr hoch                  |
| Standard_H8m   | 8         | 112         | 1000                     | 16             | 16 x 500                    | 8 / hoch                      |
| Standard_H16m  | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / sehr hoch                 |
| Standard_H16r * | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / sehr hoch                  |
| Standard_H16mr * | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / sehr hoch                  |


* RDMA in

<br>
## <a name="n-series-preview"></a>N-Serie (Preview)

Die Größe der NC und NV ist auch bekannt als Instanzen GPU aktiviert. Hierbei handelt es sich um spezielle virtuellen Computern, die für die verschiedenen Szenarios und Verwenden von Fällen optimiert NVIDIA GPU Karten beinhalten. Die NV Größen sind optimiert und für remote-Visualisierung, streaming, Spiele, Codierung und VDI-Szenarien wie OpenGL und DirectX Framework Nutzung entwickelt. Die Größen NC sind weitere optimiert für rechenintensiv und Netzwerk stark Applications, Algorithmen, einschließlich CUDA und OpenCL basierten Anwendungen und Simulationen. 


### <a name="nv-instances"></a>NV Instanzen
Die NV Instanzen sind NVIDIA Tesla M60 GPUs betrieben und NVIDIA-Raster für Desktop schnellere Applikationen und virtuelle Desktops, werden Kunden ihre Daten oder Simulationen visualisieren können. Die Benutzer können ihre Grafiken stark Workflows für die Instanzen NV übergeordnete Grafikfunktionen erhalten und außerdem Ausführen einfacher Genauigkeit Auslastung wie Codierung und Rendern visualisiert werden sollen. Die M60 Tesla stellt 4096 CUDA Kerne in zwei GPU Design mit bis zu 36 Streams von 1080p h. 264.


| Größe          | CPUs | Arbeitsspeicher: GiB  | Lokale SSD: GiB | GPU            |
|---------------|-----------|--------------|---------------------------|----------------|
| Standard_NV6  | 6         | 56           | 380                       | 1 x NVIDIA M60 |
| Standard_NV12 | 12        | 112          | 680                       | 2 x NVIDIA M60 |
| Standard_NV24 | 24        | 224          | 1440                      | 4 x NVIDIA M60 |



### <a name="nc-instances"></a>NC Instanzen

Die NC Instanzen werden durch NVIDIA Tesla K80 betrieben. Benutzer können nun durch die Daten viel schneller durch Nutzung der CUDA für Energy Durchsuchen von Applications, Simulationen Absturz analysieren, Raytraced rendern, Tiefe Lern- und vieles mehr. Die Tesla K80 bietet 4992 CUDA Kerne mit einem Design zwei-GPU bis zu 2.91 Teraflops von mit doppelter Genauigkeit und bis zu 8.93 Teraflops der Leistung mit einfacher Genauigkeit. 


| Größe          | CPUs | Arbeitsspeicher: GiB  | Lokale SSD: GiB  | GPU            |
|---------------|-----------|--------------|---------------------------|----------------|
| Standard_NC6  | 6         | 56           | 380                       | 1 x NVIDIA K80 |
| Standard_NC12 | 12        | 112          | 680                       | 2 x NVIDIA K80 |
| Standard_NC24 | 24        | 224          | 1440                      | 4 x NVIDIA K80 |



<br>
## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Hinweise: Standard A0 - A4 mit CLI und PowerShell 


Im Bereitstellungsmodell klassischen sind einige virtueller Computer Größe Namen weicht in CLI und PowerShell:

* Standard_A0 ist ExtraSmall 
* Standard_A1 ist klein
* Standard_A2 ist "Mittel"
* Standard_A3 ist groß
* Standard_A4 ist ExtraLarge


## <a name="next-steps"></a>Nächste Schritte

- Informationen Sie zu [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](../articles/azure-subscription-service-limits.md).
- Erfahren Sie mehr [über die virtuellen Computern H-Serie und berechnen ankommt A-Serie](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) für Auslastung wie High Performance Computing (HPC).



