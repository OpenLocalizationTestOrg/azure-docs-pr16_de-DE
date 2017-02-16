<properties 
   pageTitle="Hardware für StorSimple 10 Switch-Schnittstellen | Microsoft Azure"
   description="Beschreibt die unterstützten kleine Formular zweifaktorielle Varianzanalyse austauschbare (SFP) Transceiver, Kabel und Schalter für die 10 Switch-Netzwerk-Schnittstellen auf Ihrem Gerät StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="supported-hardware-for-the-10-gbe-network-interfaces-on-your-storsimple-device"></a>Unterstützte Hardware für die 10 Switch-Netzwerk-Schnittstellen auf Ihrem Gerät StorSimple

## <a name="overview"></a>(Übersicht)

Dieser Artikel enthält Informationen zu ergänzenden Hardware, die mit Ihrem Gerät Microsoft Azure StorSimple passt.

## <a name="list-of-devices-tested-by-microsoft"></a>Liste der von Microsoft getestet Geräte

Microsoft hat getestet, die folgenden kleine Formular zweifaktorielle Varianzanalyse austauschbare (SFP) Transceiver, Kabel und Schalter, um sicherzustellen, dass sie optimal mit Geräte funktionieren. (In den folgenden Tabellen werden aktualisiert werden, sobald neuer Hardware getestet wird.)

### <a name="sfp-transceivers"></a>SFP + Transceiver

|Stellen Sie|Modell|
|---|---|
|Cisco|SFP, 10G--SR|

### <a name="cables"></a>Kabel

|S. Nein. |Stellen Sie|Modell|
|---|---|---|
| 1.|Cisco|SFP-H10GB-CU1M|
| 2.|Cisco|SFP-H10GB-CU2M|
| 3.|Cisco|SFP-H10GB-CU3M|
| 4.|Tripp Lite|N820 - 05M (OM3)|

### <a name="switches"></a>Schalter

|S. Nein.|Stellen Sie|Modell|
|---|---|---|
| 1. |Cisco|N3K-C3172PQ-10-GIGE|
| 2. |Cisco|N3K-C3048-ZM-F|
| 3. |Cisco|N5K-C5596UP-ANLAGEN|

## <a name="list-of-devices-tested-in-the-field"></a>Liste der Geräte in das Feld getestet

Dieser Abschnitt enthält die Liste der Geräte, die in das Feld erfolgreich durch StorSimple Kunden bereitgestellt wurden. Diese nicht von Microsoft getestet wurden aber wahrscheinlich für die Arbeit mit Ihrem Gerät StorSimple.
 
| Parameter                         | Wert                                    |
|-----------------------------------|------------------------------------------|
| Stellen Sie wechseln                     | Wacholder                                  |
| Switch-Modell                    | ex4550-32F                               |
| Betriebssystemversion wechseln | JunOS 12.3R9.4                           |
| Blade-Modell                     | Integrierte Ports (PIC 0)                    |
| Transceiver erstellen                  | Wacholder                                  |
| Transceiver Modell               | Teilenummer 740-021308 <br></br> Teilenummer 740-030658                   |
| Transceiver Firmwareversion    | Rev 01 Version 0.0 (gemeldet)            |
| Kabel-Modell                     | Duplex Jumper LC LC/50/125µ, OM3, LSZH |
| StorSimple Modell                | 8600                                     |
| StorSimple Softwareversion     | 6.3.9600.17491                           |


## <a name="list-of-devices-tested-by-oem-provider-mellanox"></a>Geräteliste getestet von OEM-Anbieter (Mellanox)  

Die folgenden kleine Formular zweifaktorielle Varianzanalyse austauschbare (SFP) Transceiver, Kabel und Schalter, um sicherzustellen, dass sie optimal mit Mellanox Netzwerk-Schnittstellen, wie z. B. 10 GbE-Netzwerk-Schnittstellen auf Ihrem Gerät StorSimple funktionieren Mellanox getestet.

### <a name="cables-and-modules-supported-by-mellanox"></a>Kabel und Module von Mellanox unterstützt 

Der folgenden Tabelle sind die Kabel und Module von Mellanox unterstützt. Diese nicht von Microsoft getestet wurden aber wahrscheinlich für die Arbeit mit Ihrem Gerät StorSimple.

| S. Nein. | Geschwindigkeit | Modell                 | Beschreibung                                            | Stellen Sie                |
|--------|-------|-----------------------|--------------------------------------------------------|-----------------------|
| 1.     | 10 GbE| CAB-SFP-SFP - 1M        | passive Kupferkabel SFP + 10 Gb/s 1m                   | Arista                |
| 2.     | 10 GbE| CAB-SFP-SFP - 2M        | passive Kupferkabel SFP + 10 Gb/s 2m                   | Arista                |
| 3.     | 10 GbE| CAB-SFP-SFP - 3M        | passive Kupferkabel SFP + 10 Gb/s 3m                   | Arista                |
| 4.     | 10 GbE| CAB-SFP-SFP - 5M        | passive Kupferkabel SFP + 10 Gb/s 5m                   | Arista                |
| 5.     | 10 GbE| Cisco SFP-H10GBCU1M   | Cisco SFP + Kabel                                       | Cisco                 |
| 6.     | 10 GbE| Cisco SFP-H10GBCU3M   | Cisco SFP + Kabel                                       | Cisco                 |
| 7.     |10 GbE | Cisco SFP-H10GBCU5M   | Cisco SFP + Kabel                                       | Cisco                 |
| 8.     | 10 GbE| J9281B HP X242 10 G    | SFP + zu SFP + 1m direkter Anschluss Kupferkabel             | HP                    |
| 9.     | 10 GbE| 455883-B21 HP BLc     | 10Gb SR SFP + Suchbegriffen                                       | HP                    |
| 10.    | 10 GbE| 455886-B21 HP BLc     | 10Gb LR SFP + Suchbegriffen                                       | HP                    |
| 11.    |10 GbE | 487649-B21 HP BLc     | SFP + 0,5 m 10GbE Kupferkabel                           | HP                    |
| 12.    |10 GbE | 487652-B21 HP BLc     | 1m 10GbE SFP + Kupferkabel                             | HP                    |
| 13.    |10 GbE | 487655-B21 HP BLc     | SFP + 3m 10GbE Kupferkabel                             | HP                    |
| 14.    |10 GbE | 487658-B21 HP BLc     | SFP + 7m 10GbE Kupferkabel                             | HP                    |
| 15.    |10 GbE | 537963-B21 HP BLc     | SFP + 5m 10GbE Kupferkabel                             | HP                    |
| 16.    |10 GbE | HP AP784A             | 3m Reihe C passiven Kupfer SFP + Kabel                  | HP                    |
| 17.    |10 GbE | HP AP785A             | 5m Reihe C passiven Kupfer SFP + Kabel                  | HP                    |
| 18.    |10 GbE | HP AP818A             | 1m Serie B aktiven Kupfer SFP + Kabel                   | HP                    |
| 19.    |10 GbE | HP AP819A             | 3m Serie B aktiven Kupfer SFP + Kabel                   | HP                    |
| 20.    |10 GbE | HP J9150A             | X132 10 G SFP + LC SR Transceiver                        | HP                    |
| 21.    |10 GbE | HP J9151A             | X132 10 G SFP + LC LR Transceiver                        | HP                    |
| 22.    |10 GbE | HP J9283B             | X242 10 G SFP + SFP + 3 m DAC Kabel                        | HP                    |
| 23.    |10 GbE | HP J9285B             | X242 10 G SFP + SFP + 7 m DAC Kabel                        | HP                    |
| 24.    | 10 GbE| HP JD095B             | X240 10 G SFP + SFP + 0,65 m DAC Kabel                     | HP                    |
| 25.    | 10 GbE| HP JD096B             | X240 10 G SFP + SFP + 1,2 m DAC Kabel                      | HP                    |
| 26.    | 10 GbE| HP JD097B             | X240 10 G SFP + SFP + 3 m DAD Kabel                        | HP                    |
| 27.    | 10 GbE| MAM1Q00A-QSA Mellanox | QSFP zu SFP + Karte                                   | Mellanox Technologien |
| 28.    | 10 GbE| MC2309124-006 Mt      | Passive Kupferkabel 1 X SFP+ zu QSFP 10 Gb/s 24awg 7 m   | Mellanox Technologien |
| 29.    | 10 GbE| MC2309124-007 Mt      | Passive Kupferkabel 1 X SFP+ zu QSFP 10 Gb/s 24awg 7 m   | Mellanox Technologien |
| 30.    | 10 GbE| MC2309130-003 Mt      | Passive Kupferkabel 1 X SFP+ zu QSFP 10 Gb/s 30awg 3 m   | Mellanox Technologien |
| 31.    | 10 GbE| MC2309130-00A Mt      | Passive Kupferkabel 1 X SFP+ zu QSFP 10 Gb/s 30awg 0,5 m | Mellanox Technologien |
| 32.    | 10 GbE| MC3309124-005 Mt      | Passive Kupfer Kabel 1 X SFP+ 10 Gb/s 24awg 5 m           | Mellanox Technologien |
| 33.    | 10 GbE| MC3309124-007 Mt      | Passive Kupfer Kabel 1 X SFP+ 10 Gb/s 24awg 7 m           | Mellanox Technologien |
| 34.    | 10 GbE| MC3309130-003 Mt      | Passive Kupfer Kabel 1 X SFP+ 10 Gb/s 30awg 3 m           | Mellanox Technologien |
| 35.    | 10 GbE| MC3309130-00A Mt      | Passive Kupfer Kabel 1 X SFP+ 10 Gb/s 30awg 0,5 m         | Mellanox Technologien |

### <a name="switches-supported-by-mellanox"></a>Schalter von Mellanox unterstützt 

Die folgende Tabelle enthält die Schalter von Mellanox unterstützt. Diese wurden nicht von Microsoft getestet aber wahrscheinlich für die Arbeit mit Ihrem Gerät StorSimple sind.

| S. Nein. | Geschwindigkeit | Modell | Beschreibung                                                         | Stellen Sie              |
|--------|-------|-------------|---------------------------------------------------------------------|-------------|
| 1.     | 10GbE | 516733-B21  | HP ProCurve 6120XG 10GbE Ethernet Blade wechseln                      | HP    |
| 2.     | 10GbE | 538113-B21  | HP 10GbE Pass-Through-Modul (PTM)                                  | HP    |
| 3.     | 10GbE | EN4093      | IBM PureFlex System Fabric EN4093 10 Gigabit skalierbare Switch-Modul | IBM   |
| 4.     | 1GbE  | 3020        | Cisco Catalyst 3020 1GbE wechseln blade                               | Cisco |
| 5.     | 1GbE  | X-3020       | Cisco Catalyst 3020 X 1GbE wechseln blade                              | Cisco |
| 6.     | 1GbE  | 438030-B21  | HP 1GbE Switch-Modul - GbE2c Schicht 2/3 Ethernet Blade wechseln       | HP    |
| 7.     | 1GbE  | 6120G       | HP ProCurve 6120G/XG 1GbE wechseln blade                              | HP    |

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über StorSimple Hardware-Komponenten und Status](storsimple-monitor-hardware-status.md).
