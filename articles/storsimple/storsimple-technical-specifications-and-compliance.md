<properties 
   pageTitle="Technische Daten StorSimple | Microsoft Azure"
   description="Beschreibt die technische Daten und behördliche Normen Compliance-Informationen für die StorSimple Hardware-Komponenten."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="technical-specifications-and-compliance-for-the-storsimple-device"></a>Technische Daten und Kompatibilität für das Gerät StorSimple

## <a name="overview"></a>(Übersicht)

Hardware-Komponenten von Ihrem Gerät Microsoft Azure StorSimple erfüllen die technische Daten und behördliche Normen, die in diesem Artikel beschriebenen. Technischen Daten beschreiben Power und Kühlmodule (PCMs), Laufwerke, Speicherkapazität und Anlagen. Die Einhaltung von Vorschriften Informationen behandelt wie internationale Standards, Sicherheit und Emissionen und Kabel an.


## <a name="power-and-cooling-module-specifications"></a>Spezifikationen Power und Modul  

Das Gerät StorSimple weist zwei 100-240V Zeitzonen Fächer, SBB-kompatible Power Kühlmodule (PCMs). Dadurch wird eine redundanten Konfiguration. Wenn eine PCM fehlschlägt, wird das Gerät weiterhin normal auf die anderen PCM ausgeführt werden, bis das fehlerhafte Modul ersetzt wird.  

Die Einheit EBOD verwendet eine 580 W PCM und primäre Einheit einer 764 W PCM. Den folgenden Tabellen sind die technischen Spezifikationen der PCMs zugeordnet.

| Spezifikation           | 580 W PCM (EBOD)                                    | 764 W PCM (primär)                                |
|------------------------ | --------------------------------------------------- | -------------------------------------------------- |
| Maximale Ausgabe power    | 580 W                                               | 764                                                |
| Häufigkeit               | 50/60Hz                                            | 50/60Hz                                           |
| Spannung Bereichsauswahl | Automatische im Bereich: 90 – 264 V IK, 47-63 Hz               | Automatische im Bereich: 90-264 V IK, 47-63 Hz               |
| Maximaler Eingangsstrom  | 20 A                                                | 20 A                                               |
| Power Faktor Korrektur | > nominal Eingangsspannungsbereich 95 %                          | > nominal Eingangsspannungsbereich 95 %                         |
| Harmonische               | Entspricht EN61000-3-2                                   | Entspricht EN61000-3-2                                  |
| Die Ausgabe                  | 5 v Standby Spannung @ 2.0 A                          | 5 v Standby Spannung @ 2.7 A                         |
|                         | + 5 v @ 42 A                                          | + 5 v @ 40 A                                         |
|                         | + 12-v @ 38 A                                         | + 12-v @ 38 A                                        |
| Austauschbare Tastaturkürzel           |  Ja                                                | Ja                                                |
| Schalter und LED-Anzeigen       | IK ein/aus wechseln und vier Status-LED     | IK ein/aus wechseln und sechs Status-LED     |
| Anlage Kühlung       | Axialer Kühllüfter mit Variable Lüftergeschwindigkeitskontrolle  | Axialer Kühllüfter mit Variable Lüftergeschwindigkeitskontrolle |

 
## <a name="power-consumption-statistics"></a>Power-Verbrauchsstatistiken  

Der folgenden Tabelle sind die normalen Power Verbrauchsdaten (ist-Werte können aus der veröffentlichten abweichen) für die verschiedenen Modelle StorSimple Gerät an. 
 
 Bedingungen | 240 V IK | 240 V IK | 240 V IK | 110 V IK | 110 V IK | 110 V IK 
 ---------- | -------- | -------- | -------- | -------- | -------- | -------- 
 Lüfter langsam Laufwerke im Leerlauf | 1,45 A  |0.31 kW | 1057.76 BTU/h | 3.19 A | 0.34 kW | 1160.13 BTU/h 
 Zugreifen auf Laufwerke verlangsamen Lüfter | 1.54 A | 0,33 kW | 1126.01 BTU/h | 3.27 A | 0,36 kW | 1228.37 BTU/h 
 Lüfter machen, Laufwerke im Leerlauf, außerdem zwei PSUs | 2.14 A | 0.49 kW  | 1671.95 BTU/h | 4,99 A | 0.54 kW | 1842.56 BTU/h 
 Schnelle, Lüfter Laufwerke im Leerlauf, eine PSU außerdem eine im Leerlauf | 2.05 A | 0.48 kW | 1637.83 BTU/h | 4.58 A | 0,50 kW | 1706.07 BTU/h 
 Lüfter machen, Laufwerke zugreifen auf zwei PSUs eingeschaltet | 2,26 A | 0.51 kW | 1740.19 BTU/h | 4,95 A | 0.54 kW | 1842.56 BTU/h 
 Lüfter schnelle, außerdem Laufwerke zugreifen, eine PSU eine im Leerlauf | 2.14 A |0.49 kW | 1671.95 BTU/h | 4.81 A  | 0.53 kW | 1808.44 BTU/h 

## <a name="disk-drive-specifications"></a>Spezifikationen Festplattenlaufwerk  

Ihr Gerät StorSimple unterstützt bis zu 12 3,5 Formular Faktor serielle angefügt SCSI (SAS) Laufwerke. Die tatsächliche Laufwerke können eine Mischung aus Solid Laufwerken (SSDs) oder Festplatten (Festplatten), je nach Produktkonfiguration sein. Die 12 Laufwerkschächten für Festplatten befinden sich in eine 3 x 4-Konfiguration vor der Anlage. Zusätzlicher Speicher für eine andere 12 Laufwerke ermöglicht EBOD Einheit. Dies sind immer Festplatten.  

## <a name="storage-specifications"></a>Spezifikationen Speicher
Die StorSimple Geräte über eine Mischung Festplatten und Solid Laufwerke für sowohl die 8100 8600 verfügen. Die Summe verwendbare Kapazität für die 8100 und 8600 sind ungefähr 15 TB und 38 TB Hilfethemas. Die folgende Tabelle beschreibt die Details des SSD, Festplatte und Cloud Kapazität im Kontext der Lösung StorSimple Kapazität.

| Gerätemodell / Kapazität                         | 8100                                                    | 8600                                                    |
|------------------------------------------------|---------------------------------------------------------|---------------------------------------------------------|
| Anzahl der Festplatten (Festplatten)              |   8                                                     |  19                                                     |
| Anzahl der Solid Laufwerken (SSDs)            |   4                                                     |  5                                                      |
| Einzelne Festplattenkapazität                            |   4 TB                                                  |  4 TB                                                   |
| Einzelne SSD Kapazität                            |   400 GB                                                |  800 GB                                                 |
| Freie Kapazität                                 |   4 TB                                                  | 4 TB                                                    |
| Festplattenkapazität                            | 14 TB                                                   | 36 TB                                                   |
| SSD Kapazität                            | 800 GB                                                  | 2 TB                                                    |
| Total verwendbar Kapazität *                         | ~ 15 TB                                                 | ~ 38 TB                                                 |
| Maximale Lösung Kapazität (einschließlich Cloud)    | 200 TB                                                  | 500 TB                                                  |


<sup> * </sup> -  *Die gesamte verwendbare Kapazität enthält die verfügbaren Kapazität für Daten und Metadaten buffers.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Anlage Dimensionen und Stärke Spezifikationen  

Den folgenden Tabellen sind die verschiedenen Einheit Spezifikationen für Dimensionen und Stärke.  

### <a name="enclosure-dimensions"></a>Anlage Dimensionen
Die folgende Tabelle enthält die Dimensionen der Einheit in Millimeter und Zoll.

|Anlage |Millimeter |Insgesamt |
|----------|------------|-------| 
| Höhe |87.9 | 3,46 |
| Breite über Rackbefestigungsflansch | 483 | 19.02 |
| Breite über den Textkörper der Einheit | 443 | 17.44 |
| Tiefe von der front Rackbefestigungsflansch zu Ende Einheit Textkörper | 577 | 22.72 |
| Tiefe von Vorgängen Systemsteuerung zu entferntesten Ende Einheit | 630.5 | 24.82 |
| Tiefe von Rackbefestigungsflansch zu entferntesten Ende Einheit |   603 | 23.74 |

### <a name="enclosure-weight"></a>Anlage Stärke  

Je nach Konfiguration eine vollständig geladene primäre Einheit kann von 21 zu 33 kg abzuschätzen und zwei Personen zur Behandlung erfordert. 
 
| Anlage | Gewicht |
|-----------|--------| 
| Maximale Stärke (je nach der Konfiguration) |30 kg – 33 kg |
| Leer (keine Laufwerke gemäß) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Spezifikationen für Einheit-Umgebung  

Dieser Abschnitt listet die Angaben im Zusammenhang mit der Anlage-Umgebung. Temperatur, Luftfeuchtigkeit, Höhe, Shock, Vibration, Ausrichtung, Sicherheit und elektromagnetische Kompatibilität (EMC) sind in dieser Kategorie enthalten.  

### <a name="temperature-and-humidity"></a>Temperatur und Feuchtigkeit

| Anlage        | Temperatur-Bereich  | Einer relativer Umgebungsfeuchte | Maximale Feuchttemperatur   |
|------------------|----------------------------|---------------------------|--------------------|
| Betrieb      | 5° C - 35° C (41° F - 95° F)    | 20 % - 80 % nicht-kondensierend- | 28° C (82° F)        |
| Außer Betrieb  | – 40° C - 70° C (40° F - 158° F) | 5-100 %, nicht kondensierend  | 29° C (84° F)        |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Luftstrom, Höhe, Shock, Vibration, Ausrichtung, Sicherheit und EMC
 
| Anlage          | Betrieb Spezifikationen                                                |
|--------------------|---------------------------------------------------------------------------| 
| Luftstrom            | Luftfluss ist von vorne nach hinten. System muss mit einem low-pressure, Rückseite-Anlage Installation betrieben. Wieder Druck, die Shapes für Gestelle Türen und Hindernisse erstellte sollte 5 Pascal (0,5 mm Wasser Monitor) nicht überschreiten. |
| Höhe, Betrieb  | -30 Meter 3045 Meter (-100 Fuß auf 10.000 Fuß) mit maximale Geschäftsjahre Temperatur heben bewertet von 5 ° C über 7000 Fuß. |
| Höhe, außer Betrieb  | -305 Meter zu 12.192 m (-1,000 Fuß auf 40.000 Fuß) |
| Shock, Betrieb  | 5g 10 ms ½ Sinus |
| Shock, außer Betrieb  | Sinus von 30g 10 ms ½ |
| Vibration bei Betrieb  | 0.21g RMS 5 – 500 Hz Zufallszahl |
| Vibration, außer Betrieb  | 1.04g RMS 2-200 Hz Zufallszahl |
| Vibration, Verlagerung  | 3g 2-200 Hz Sinus |
| Ausrichtung und bereitstellen  | 19" den Shapes für Gestelle bereitstellen (2 EIA-Einheiten) |
| Rackschienen  | Racks mit IEC 297 angepasst Tiefe mindestens 700 mm (31.50 Zoll) |
| Sicherheit und Genehmigung  |   CE und UL EN 61000-3, IEC 61000-3, UL 61000-3 |
| EMC  | EN55022 (CISPR - A), FCC A |

## <a name="international-standards-compliance"></a>Internationale Standards compliance
Ihr Gerät Microsoft Azure StorSimple – Konformität mit den folgenden internationalen Normen:  

- CE - EN 60950-1  
- CB Bericht IEC 60950-1  
- UL- und cUL nach UL 60950-1  

## <a name="safety-compliance"></a>Sicherheit compliance  

Ihr Gerät Microsoft Azure StorSimple entspricht die folgenden Sicherheit Bewertungen:  

- System Produkt Typ Genehmigung: UL, cUL, CE  
- Sicherheit Compliance: UL 60950, IEC 60950, EN 60950  

## <a name="emc-compliance"></a>EMC compliance 

Ihr Gerät Microsoft Azure StorSimple entspricht die folgenden EMC Bewertungen.  

### <a name="emissions"></a>Emissionen

Das Gerät ist für durchgeführten und Strahlungsfelder Emissionen Ebenen EMC-kompatibel.  

- Durchgeführten Emissionen beschränken Ebenen: CFR 47 Teil 15 b Klasse A EN55022 Klasse A CISPR Klasse A  
- Emissionen Strahlungsfelder beschränken Ebenen: CFR 47 Teil 15 b Klasse A EN55022 Klasse A CISPR Klasse A   

### <a name="harmonics-and-flicker"></a>Harmonische und Flickr  

Das Gerät entspricht EN61000-3-2/3.  

### <a name="immunity-limit-levels"></a>Immunität Grenzwert Ebenen  
Das Gerät entspricht EN55024.  

## <a name="ac-power-cord-compliance"></a>IK Power Kabel compliance
  
Plug-Ins und die vollständige Power Kabel Assembly müssen entsprechen den Standards entsprechenden für das Land, in dem das Gerät verwendet wird, und sie müssen die Sicherheit Genehmigungen erfolgt, die in diesem Land zulässig sind. In den folgenden Tabellen Liste Standards für den USA und in Europa.  

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>Netzkabel - USA (muss NRTL aufgeführt sein)

| Komponente       | Spezifikation                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Kabel Typ       | PA oder SVT, 18 AWG Minimum, 3 Leiter, 2.0 Meter Maximallänge |
| Plug-Ins            | NEMA 5-15P Dreiadrige Anlage Plug-Ins bewertet 120 V, 10 A; oder IEC 320 C14 250 Volt, 10 A |
| Sockets          | IEC 320 C-13, 250 V, 10 A                                         |

### <a name="ac-power-cords---europe"></a>Netzkabel - Europa

| Komponente       | Spezifikation                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Kabel Typ       | Gleichmäßige, H05-VVF-3G1.0                                         |
| Sockets          | IEC 320 C-13, 250 V, 10 A                                         |

## <a name="supported-network-cables"></a>Unterstützte Netzwerkkabel  

10 Switch-Netzwerk-Schnittstellen Daten 2 und 3 von Daten, finden Sie in der [Liste der unterstützten Netzwerkkabel und Module](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Nächste Schritte

Sie können nun ein StorSimple Gerät im Datencenter bereitgestellt. Weitere Informationen finden Sie unter [Ihrem lokalen Gerät bereitstellen](storsimple-deployment-walkthrough-u2.md).  
