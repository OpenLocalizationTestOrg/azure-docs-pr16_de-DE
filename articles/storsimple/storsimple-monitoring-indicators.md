<properties 
    pageTitle="Indikatoren für die Überwachung StorSimple | Microsoft Azure" 
    description="Beschreibt die - Leuchtdioden (LED) und akustischen Alarme verwendet, um den Status des Geräts StorSimple zu überwachen."
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
    ms.date="08/18/2016"
    ms.author="alkohli" />

# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>Verwenden Sie zum Verwalten von Ihrem Geräts StorSimple Indikatoren für die Überwachung   

## <a name="overview"></a>(Übersicht)

Ihr Gerät StorSimple umfasst Leuchtdioden für – (LED) und Weckrufe, dass Sie zum Überwachen der Module und den allgemeinen Status des Geräts StorSimple verwenden können. Die überwachen Indikatoren finden Sie auf der Hardware-Komponenten des Geräts primäre Einheit und die Anlage EBOD. Die überwachen Indikatoren können LED oder akustischen Alarme sein.

Es gibt drei LED-Status verwendet, um den Status eines Moduls anzugeben: Grün, Grün, Rot-gelb oder rot-orange blinkt.  

- Grün LED darstellen einen fehlerfreien Betriebssystem (Status).  
- Anzeige blinkt grün dargestellten Werte rot-orange LED das Vorhandensein von nicht-kritischen Zuständen, die Eingriff erfordern möglicherweise.  
- Rot-orange LED darauf hinzuweisen, dass es sich bei kritischen Fehlern innerhalb des Moduls vorhanden.  

Der Rest der in diesem Artikel werden die verschiedenen Überwachung Indikator LED, deren Speicherort auf dem StorSimple Gerät, das Gerätestatus basierend auf der LED Staaten und alle zugehörigen akustischen Alarme.

## <a name="front-panel-indicator-leds"></a>Vorderseite-LED

Die Vorderseite, auch bekannt als die *Vorgänge Systemsteuerung* oder *Ops Systemsteuerung*zeigt den aggregieren Status der alle Module im System. Die Vorderseite ist die primäre StorSimple und die EBOD Einheit und unten dargestellt ist.  

   ![Vorderseite Gerät][1]
 
Die Vorderseite enthält die folgenden Indikatoren:  

1. Stummschalten
2. Power-Indikator LED (Rot/Grün-orange)
3. Modul Fehleranzeige geführt haben (in Rot-Gelb/aus)
4. Logische Fehleranzeige geführt haben (in Rot-Gelb/aus
5. Einheit-ID anzeigen  

Der größte Unterschied zwischen der Vorderseite LED-Anzeigen auf dem Gerät und die für die Anlage EBOD ist die **System Einheit Identifikationsnummer** auf der LED-Anzeige. Die Standard-Einheit-ID, die auf dem Gerät angezeigt ist **00**, während die Standard-Einheit-ID, die angezeigt wird, klicken Sie auf die Anlage EBOD **01**wird. So können Sie schnell zwischen dem Gerät und die Einheit EBOD unterscheiden, wenn das Gerät aktiviert ist. Wenn Ihr Gerät deaktiviert ist, verwenden Sie die Informationen in [einem neuen Gerät aktivieren](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) um das Gerät aus der Einheit EBOD zu unterscheiden.  

## <a name="front-panel-led-status"></a>Vorderseite LED-status  

Verwenden Sie in der folgenden Tabelle, um den Status der LED-Anzeigen auf der Vorderseite für das Gerät oder die Einheit EBOD erkennbar zu identifizieren.  

|System power | Modul-Fehler | Logische Fehlerstrukturanalyse | Erinnerung | Status|
|-------------|---------------|-----------------|-------|-------|
|Rot-Gelb | DEAKTIVIEREN     | DEAKTIVIEREN | N/V | IK Power verloren gegangen sind, auf zusätzliche Power oder IK Power auf Betrieb und den Controller, Module entfernt wurden.|
|Grün | AUF | AUF | N/V | OPS Systemsteuerung einschalten (5 s) testen Zustand|
|Grün | DEAKTIVIEREN | DEAKTIVIEREN | N/V | Klicken Sie auf alle Funktionen gute Power|
|Grün | AUF |N/V | PCM-Fehler-LED, Lüfter Fehler-LED | Alle PCM Fehlerstrukturanalyse, Lüfter Fehlerstrukturanalyse, über oder unter Temperatur|
| Grün | AUF | N/V | E/a-Modul LED-Anzeigen  | Alle Controller Modul-Fehler|
| Grün | AUF | N/V | N/V | Anlage Logik Fehlerstrukturanalyse|
| Grün | Flash | N/V | Modul Status-LED auf Controller-Modul. PCM-Fehler-LED, Lüfter Fehler-LED | Unbekannten Controller Modultyp installiert, I2C Bus Fehler, Controller Modul wichtige Produkt Daten (VPD) Konfiguration zurück |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Power-Modul (PCM) LED Kühlung   

Klicken Sie auf der Rückseite der primären Einheit oder EBOD-Einheit auf jedem Modul PCM kann Power Kühlung Modul (PCM) LED gefunden werden. In diesem Thema wird erläutert, wie Sie die folgenden LED zum Überwachen des Status von Ihrem Gerät StorSimple.  

- PCM LED-Anzeigen für die primäre Einheit
- PCM LED-Anzeigen für die Anlage EBOD

## <a name="pcm-leds-for-the-primary-enclosure"></a>PCM LED-Anzeigen für die primäre Einheit  

Das Gerät StorSimple verfügt über eine 764W PCM Modul mit einem weiteren Akku. Die folgende Abbildung zeigt das LED-Anzeige Panel für das Gerät.  

   ![PCM LED-Anzeigen auf die primäre Einheit][2]

LED Legende:

1. IK Power-Fehler
2. Lüfterausfall
3. Fehlerstrukturanalyse Akku
4. PCM OK
5. Ausfall DC
6. Gute Akku  

Der Status für die PCM wird auf der LED-Anzeige angezeigt. Klicken Sie im Bereich des Geräts PCM LED weist sechs LED-anzeigen. Vier von diesen LED-anzeigen Anzeigen des Status der Netzteil und der Die verbleibenden zwei LED Geben Sie den Status des Moduls in der PCM Stützbatterie. In den folgenden Tabellen können Sie um den Status der PCM zu bestimmen.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>PCM-LED für Netzteil und
| Status | PCM OK (Grün) | IK Fail (Orange) | Lüfter Fail (Orange) | DC Fail (Orange) |
|--------|----------------|-----------------------|------------------|----------------------|
| Keine IK Power (mit Einheit) | DEAKTIVIEREN | DEAKTIVIEREN | DEAKTIVIEREN | DEAKTIVIEREN|
| Keine IK Power (diese PCM) | DEAKTIVIEREN | AUF | DEAKTIVIEREN | AUF |
| IK präsentieren PCM auf - OK     | AUF | DEAKTIVIEREN | DEAKTIVIEREN | DEAKTIVIEREN |
| PCM Fail (Lüfter Fail) | DEAKTIVIEREN | DEAKTIVIEREN | AUF | N/V |
| PCM Fehlerstrukturanalyse (über Amp über Spannung, über aktuelle) | DEAKTIVIEREN | AUF | AUF | AUF |
| PCM (Lüfter außerhalb des Fehlertoleranz) | AUF | DEAKTIVIEREN | DEAKTIVIEREN | AUF |
| Standby-Modus | Blinken | DEAKTIVIEREN | DEAKTIVIEREN | DEAKTIVIEREN |
| PCM Firmware herunterladen | DEAKTIVIEREN | Blinken | Blinken | Blinken |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>PCM-LED für die Stützbatterie  

| Status | Gute Akku (Grün) | Akku Fehler (gelb) |
|--------|----------------------|-----------------------|
| Akku nicht vorhanden | DEAKTIVIEREN | DEAKTIVIEREN |
| Akku vorhanden und berechnete | AUF | DEAKTIVIEREN |
| Akku Belastung oder Wartung Erledigung | Blinken | DEAKTIVIEREN |
| "Weiche" Fehlerstrukturanalyse Akku (wiederhergestellt) | DEAKTIVIEREN | Blinken |
| "Harte" Fehlerstrukturanalyse Akku (nicht wiederhergestellt) | DEAKTIVIEREN | AUF |
| Akku disarmed | Blinken | DEAKTIVIEREN |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>PCM LED-Anzeigen für die Anlage EBOD  

Die Einheit EBOD verfügt über eine 580 w bei PCM- und kein zusätzlicher Akku. Klicken Sie im Bereich PCM für die Anlage EBOD weist LED-Indikator anzeigen nur für die Power benötigtes Material und der Lüfter. Die folgende Abbildung zeigt diese LED-anzeigen.

   ![PCM LED-Anzeigen auf die Anlage EBOD][3] 
 
In der folgenden Tabelle können um den Status der PCM zu bestimmen.  

| Status | PCM OK (Grün) | IK Fail (Orange) | Lüfter Fail (Orange) | DC Fail (Orange) |
|--------|---------------|------------------------|------------------|----------------------|
| Keine IK Power (mit Einheit) | DEAKTIVIEREN | DEAKTIVIEREN | DEAKTIVIEREN | DEAKTIVIEREN |
| Keine IK Power (diese PCM) | DEAKTIVIEREN | AUF | DEAKTIVIEREN | AUF |
| IK präsentieren PCM auf – OK | AUF | DEAKTIVIEREN | DEAKTIVIEREN | DEAKTIVIEREN |
| PCM Fail (Lüfter Fail) | DEAKTIVIEREN | DEAKTIVIEREN | AUF | X |
| PCM Fehlerstrukturanalyse (über Amp, über Spannung, über aktuelle | DEAKTIVIEREN | AUF | AUF | AUF |
| PCM (Lüfter außerhalb des Fehlertoleranz) | AUF | DEAKTIVIEREN | DEAKTIVIEREN | AUF |
| Standby-Modell | Blinken | DEAKTIVIEREN | DEAKTIVIEREN | DEAKTIVIEREN |
| PCM Firmware herunterladen | DEAKTIVIEREN | Blinken | Blinken | Blinken |

## <a name="controller-module-indicator-leds"></a>Controller-Modul LED  

Das Gerät StorSimple enthält LED für den primären Controller und die EBOD Controller Module.   

### <a name="monitoring-leds-for-the-primary-controller"></a>Überwachen der LED-Anzeigen für die primäre controller
Die folgende Abbildung hilft Ihnen die LED-Anzeigen auf dem primären Controller identifizieren. (Alle Komponenten aufgelistet sind in Ausrichtung zur Makrofunktion.)  

   ![Überwachen der LED - primären controller][4]
 
Anhand der folgenden Tabelle feststellen, ob das Modul Controller einwandfrei funktioniert.  

### <a name="controller-indicator-leds"></a>Controller-LED  

| LED-ANZEIGE | Beschreibung                                                                            
|---- | ----------- |
| ID-LED (Blau) | Gibt an, dass das Modul identifiziert wird. Wenn die blaue LED auf einer laufenden Controller blinkt, der Controller ist der aktiven Controller, und das andere das standby Controller ist. Weitere Informationen finden Sie unter [Identifizieren der aktiven Controller auf Ihrem Gerät](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Fehler-LED (gelb) | Gibt einen Fehler in der Steuerung an.        
| OK LED (Grün) | Grün zeigt an, dass der Controller OK ist. Anzeige blinkt grün zeigt einen Controller VPD Konfigurationsfehler an. |
| SAS Aktivität LED (Grün) | Grün zeigt an, eine Verbindung mit keine aktuellen Tätigkeit ein. Blinkt grün zeigt an, dass die Verbindung laufenden Aktivität hat. |
| Ethernet-Status LED-Anzeigen | Zeigt an, klicken Sie rechts auf Link/Netzwerk Aktivität: (Grün) Link aktive, (aufleuchtende Grün) Netzwerk Aktivität. Links gibt netzwerkgeschwindigkeit an: (gelb) 1000 Mb/s, 100 Mb/s (Grün) und (aus) 10 Mb/s. Je nach dem Komponentenmodell möglicherweise diese Light blinken, auch wenn die Schnittstelle nicht aktiviert ist. |
| Beitrag LED-Anzeigen | Zeigt den Fortschritt Boot an, wenn der Controller aktiviert ist. Wenn das Gerät StorSimple nicht starten, hilft diese LED Microsoft Support den Punkt im Boot-Vorgang zu identifizieren, an dem der Fehler aufgetreten ist. |

>[AZURE.IMPORTANT] 
Wenn die Fehler-LED leuchtet, gibt es ein Problem mit der Controller-Modul, das aufgelöst werden möglicherweise durch einen Neustart von des Controllers. Microsoft-Support wenden Sie sich an, wenn Sie einen Neustart von des Controllers dieses Problem nicht behoben wird.  


### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>Überwachen der LED-Anzeigen für die EBOD (EBOD Einheit)  

Jede der 6 Gb/s SAS EBOD Controller verfügt über LED-Anzeigen, die den Status angeben, wie in der folgenden Abbildung gezeigt.  

  ![LED - EBOD Einheit für die Überwachung][5]

Verwenden Sie in der folgenden Tabelle, um zu bestimmen, ob das EBOD Controller-Modul ordnungsgemäß funktioniert.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD Controller Modul-LED  

|Status | E/a-Modul OK (Grün) | E/a-Modul-Fehler (Orange) | Host Portaktivität (Grün) |
|-------|----------------------|-------------------------------|----------------------------|
| Controller-Modul OK | AUF | DEAKTIVIEREN | - |
| Controller-Modul-Fehler | DEAKTIVIEREN | AUF | - |
| Keine externen Host Port-Verbindung | - | - | DEAKTIVIEREN |
| Externe Host Port Verbindung – keine Aktivität | - | - | AUF |
| Externe Host Port Verbindung - Aktivität | - | - | Blinken |
| Controller Modul Metadaten zurück | Blinken | - | - |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>Laufwerk-LED für die primäre Einheit und EBOD Einheit

Das Gerät StorSimple enthält Laufwerke befindet sich in der primären Einheit und die Anlage EBOD. Jedes Datenträgerlaufwerk enthält Indikator LED, Überwachung, wie in diesem Abschnitt beschrieben. 

Für die Laufwerke der Laufwerkstatus ein grünes erkennbar LED und eine rot-orange LED an der Vorderseite jedes Laufwerk Carrier Moduls bereitgestellt. Die folgende Abbildung zeigt diese LED-anzeigen.

  ![Festplattenlaufwerk LED-Anzeigen][6]
 
Verwenden Sie in der folgenden Tabelle, um den Zustand jedes Datenträgerlaufwerk zu ermitteln, der wiederum die insgesamt Vorderseite LED-Status wirkt sich auf.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>Laufwerk-LED für die Anlage EBOD  

| Status | Aktivität OK LED (Grün) | Fehler-LED (Rot-gelb) | Verknüpfte Ops Systemsteuerung LED |
|-------|--------------------------|----------------------|-------------------------|
| Kein Laufwerk installiert | DEAKTIVIEREN | DEAKTIVIEREN | Keine |
| Laufwerk installiert sein und Betrieb | Ein-/ausschalten aufleuchtende mit Aktivität | X | Keine |
| SCSI-Einheit Services (SES) Gerät Identität festlegen | AUF | Aufleuchtende 1 Sekunde auf/1 zweites deaktivieren | Keine |
| SES Gerät Fehlerstrukturanalyse Bit festlegen | AUF | AUF | Logische Fehlerstrukturanalyse (Rot) |
| Stromsteuerungsausfall Verbindung | DEAKTIVIEREN | AUF | Modul-Fehler (Rot) |

## <a name="audible-alarms"></a>Akustischen Alarme  

Ein Gerät StorSimple enthält akustischen Alarme sowohl die primäre Einheit und die Einheit EBOD zugeordnet. Eine Erinnerung akustischen befindet sich die Vorderseite der beiden Anlagen (auch bekannt als der Bereich Ops). Die akustischen Erinnerung zeigt an, wenn eine Bedingung Fehlerstrukturanalyse vorhanden ist. Die folgenden Situationen werden die Erinnerung aktivieren:  

- Fehlerstrukturanalyse Lüfter oder Fehler
- Spannung außerhalb des zulässigen Bereichs
- Über oder unter Temperatur Bedingung
- Zur Überlauf
- Systemfehler
- Logische Fehlerstrukturanalyse
- Stromversorsorgungsfehler
- Zum Entfernen der eine Potenz Kühlmodul (PCM)  

Die folgende Tabelle beschreibt die verschiedenen Zuständen Erinnerung.  

### <a name="alarm-states"></a>Erinnerung Staaten  

| Erinnerung Zustand | Aktion | Aktion mit Stummschalten gedrückt |
|------------|---------|---------------------------------|
| S0 | Normalen Modus: im Hintergrund | Signalton zweimal |
| S1 | Fehlerstrukturanalyse-Modus: 1 Sekunde auf/1 zweites deaktivieren | Übergang S2 oder S3 (siehe Hinweise) |
| S2 | Erinnern Sie Modus: wiederkehrender Signalton | Keine |
| S3 | Stumm Modus: im Hintergrund | Keine |
| S4 | Kritische Fehlerstrukturanalyse-Modus: fortlaufender Erinnerung | Nicht verfügbar: Stummschalten nicht aktiv |

> [AZURE.NOTE] 

>  - Erinnerung Zustand S1 Wenn Sie keine Stummschalten innerhalb von zwei Minuten drücken wechselt der Zustand automatisch in S2 oder S3.  
>  - Erinnerung Staaten S1 nach S4 zurück zum S0, nachdem die Bedingung Fehlerstrukturanalyse deaktiviert ist.  
>  - Kritische Fehler Zustand S4 kann aus einem beliebigen anderen Zustand eingegeben werden.  

Sie können die akustischen Erinnerung Stummschalten, indem Sie auf dem Bedienfeld Ops Stummschalten drücken. Automatische Stummschalten tritt nach zwei Minuten auf, wenn Sie der Schalter Stummschalten nicht manuell betrieben wird. Wenn die Erinnerung stumm geschaltet sind, werden weiterhin Sound mit kurzen wiederkehrender Töne, um anzugeben, dass ein Problem weiterhin besteht. Die Erinnerung werden im Hintergrund, wenn alle Probleme deaktiviert sind.  

Die folgende Tabelle beschreibt die verschiedenen Erinnerung Bedingungen.  

### <a name="alarm-conditions"></a>Erinnerung Bedingungen  

| Status | Schwere | Erinnerung | LED-Anzeige panel-OPS |
|--------|---------|--------|----------------|
| PCM Benachrichtigung – Verlust der Gleichstrom aus einem einzelnen PCM | Fehlerstrukturanalyse – ohne Verlust der Redundanz | S1 | Modul-Fehler|
|PCM Benachrichtigung – Verlust der Gleichstrom aus einem einzelnen PCM | Fehlerstrukturanalyse – Verlust der Redundanz | S1 | Modul-Fehler |
| PCM Lüfter fail | Fehlerstrukturanalyse – Verlust der Redundanz | S1 | Modul-Fehler |
| SBB Modul gefunden PCM Fehlerstrukturanalyse | Fehlerstrukturanalyse | S1 | Modul-Fehler |
| PCM entfernt. | Fehler in der Konfiguration | Keine | Modul-Fehler |
| Fehler bei der Anlage | Fehlerstrukturanalyse – kritische | S1 | Modul-Fehler |
| Niedrig Warnung Temperatur | Warnung | S1 | Modul-Fehler |
| Warnung bei hoher Temperatur Warnung | Warnung | S1 | Modul-Fehler |
| Über Temperatur Erinnerung | Fehlerstrukturanalyse – kritische | S1 | Modul-Fehler |
| Fehler bei der I2C bus | Fehlerstrukturanalyse – Verlust der Redundanz | S1 | Modul-Fehler |
| OPS panel-Kommunikation zurück (I2C) | Fehlerstrukturanalyse – kritische     | S1 | Modul-Fehler |
| Controller-Fehler | Fehlerstrukturanalyse – kritische | S1 | Modul-Fehler |
| SBB Benutzeroberflächen-Modul-Fehler | Fehlerstrukturanalyse – kritische | S1 | Modul-Fehler |
| SBB Benutzeroberflächen-Modul-Fehler – keine funktionsfähige Module verbleibende | Fehlerstrukturanalyse – kritische | S4 | Modul-Fehler |
| Benutzeroberflächen-Modul SBB entfernt | Warnung | Keine | Modul-Fehler |
| Laufwerk Power Steuerelement Fehlerstrukturanalyse | Warnung – ohne Verlust von Laufwerk power | S1 | Modul-Fehler |
| Laufwerk Power Steuerelement Fehlerstrukturanalyse | Fehlerstrukturanalyse – kritische; Verlust der Laufwerk power | S1 | Modul-Fehler |
| Laufwerk entfernt | Warnung | Keine | Modul-Fehler |
| Nicht genügend Power verfügbar | Warnung | keine | Modul-Fehler |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [StorSimple Hardware-Komponenten und Status](storsimple-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png

 
