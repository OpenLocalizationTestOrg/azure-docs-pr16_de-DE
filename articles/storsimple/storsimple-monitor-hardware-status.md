<properties 
   pageTitle="StorSimple Hardware-Komponenten und Status | Microsoft Azure"
   description="Informationen Sie zum Überwachen der Hardwarekomponenten von Ihrem Gerät StorSimple über den Dienst StorSimple-Manager."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-monitor-hardware-components-and-status"></a>Verwenden Sie den Dienst StorSimple Manager Monitor Hardware-Komponenten und status

## <a name="overview"></a>(Übersicht)

In diesem Artikel werden die verschiedenen physischen und logischen Komponenten in Ihrem lokalen StorSimple Gerät. Es wird erläutert, wie den Gerät Komponentenstatus Überwachen mithilfe der Seite **zum Warten** in der StorSimple Manager-Dienst werden kann. 

Die Seite **zum Warten** von zeigt den Hardwarestatus aller StorSimple Gerät Komponenten. 

Klicken Sie unter der Liste der Komponenten für 8100 gibt es drei Abschnitte, in denen beschrieben:

- **Freigegebene Komponenten** – diese sind nicht Teil der Controller, wie z. B. Laufwerke, Einheit, PCM Komponenten PCM Temperatur, Spannung Linie und Linie aktuelle Sensoren.

- **Controller 0-Komponenten** – die Komponenten, die auf Controller 0, wie z. B. Controller, SAS Expander und Verbinder, Temperatursensoren Controller und die verschiedenen Netzwerkschnittstellen befinden.

- **Controller 1-Komponenten** – die Komponenten, die Controller 1, ähnlich wie die detaillierte für Controller 0 bilden.

Eine 8600-Gerät verfügt über weitere Komponenten, die die erweiterten Gruppe der Datenträger (EBOD) Einheit entsprechen. Klicken Sie unter der Liste der Komponenten gibt es fünf Abschnitte. Diese Elemente gibt es drei Abschnitte, die die Komponenten in der primären Einheit enthalten und mit den übereinstimmen für 8100 beschrieben werden. Es gibt zwei weitere Abschnitte für die Anlage EBOD beschreiben:

- **EBOD Einheit freigegebenen Komponenten** – die EBOD Einheit und PCM präsentieren Komponenten, die nicht Bestandteil der EBOD Controller sind.

- **EBOD Controller 0-Komponenten** – die Komponenten, die zu EBOD Einheit 0, beispielsweise dem Controller EBOD SAS Expander und Netzwerke und Controller Temperatursensoren befinden.

- **EBOD Controller 1-Komponenten** – die Komponenten, aus denen EBOD Einheit 1, ähnlich wie diese detaillierte EBOD Inhaltsbereich 0 besteht.

>[AZURE.NOTE] **Abschnitt Status Hardware ist nicht in der Seite zum Warten für ein StorSimple virtuelles Gerät (1100) vorhanden.**


## <a name="monitor-the-hardware-status"></a>Überwachen des Hardwarestatus

Führen Sie die folgenden Schritte aus, um den Hardwarestatus einer Komponente Gerät anzuzeigen:

1. Navigieren Sie zu **Geräten**, wählen Sie ein bestimmtes StorSimple Gerät aus. Klicken Sie auf diese Option, um in das Menü Gerät Ebene wechseln und dann auf **Wartung**. 
2. Suchen nach dem Abschnitt **Hardware Status** , und wählen Sie die verfügbaren Komponenten (wie zuvor beschrieben). Klicken Sie einfach auf einen Pfeil vor der Beschriftung Komponente erweitern die Liste und den Status der verschiedenen Gerätekomponenten anzeigen. Finden Sie die [Komponentenliste für die primäre Einheit detaillierte](#component-list-for-primary-enclosure-of-storsimple-device) und der [ausführliche Komponentenliste für die Anlage EBOD](#component-list-for-ebod-enclosure-of-storsimple-device).

2. Verwenden Sie das folgenden Codierung Farbschema, um bei der Interpretation des Komponentenstatus:
    -  **Aktivieren Sie Grün** – kennzeichnet eine Komponente **fehlerfrei** oder auf **OK** .
    -  **Gelbe** – kennzeichnet eine Komponente **Warnung** Zustand.
    -  **Rote Ausrufezeichen** – kennzeichnet eine Komponente, die den Status **Fehler** oder **Aufmerksamkeit** aufweist.
    -  **Weiß mit schwarzem Text** – kennzeichnet eine Komponente, die nicht vorhanden ist.

3. Wenn Sie eine Komponente, die nicht in einem Zustand **fehlerfrei** ist auftreten, wenden Sie sich an den Microsoft-Support. Wenn auf Ihrem Gerät Benachrichtigungen aktiviert sind, erhalten Sie eine e-Mail-Benachrichtigung. Wenn Sie eine fehlerhafte Hardware-Komponente ersetzen müssen, finden Sie unter [StorSimple Hardwareaustausch von Komponenten](storsimple-hardware-component-replacement.md).


## <a name="component-list-for-primary-enclosure-of-storsimple-device"></a>Liste der Komponenten für primäre Einheit StorSimple Gerät

In der folgenden Tabelle werden die physischen und logischen Komponenten, die in der primären Einheit von Ihrem lokalen StorSimple Gerät enthalten sind.

|Komponente|Modul|Typ|Speicherort|Feld austauschbare Einheit (FRU)?|Beschreibung|
|---|---|---|---|---|---|
|Versorgen Sie in Slot [0-11]|Laufwerke|Physische|Freigegeben|Ja|Eine Zeile wird für jede der SSD oder die Festplatte Laufwerke in der primären Einheit dargestellt.|
|Umgebende Temperatursensor|Anlage|Physische|Freigegeben|Nein|Misst die Temperatur in den Rahmen an.|
|Mid Ebene Temperatursensor|Anlage|Physische|Freigegeben|Nein|Misst die Temperatur der Mid Ebene an.|
|Akustische Erinnerung|Anlage|Physische|Freigegeben|Nein|Gibt an, ob das akustischen Erinnerung-Teilsystem in den Rahmen aktiviert ist.|
|Anlage|Anlage|Physischen|Freigegeben|Ja|Gibt das Vorhandensein des Chassis an.|
|Anlage-Einstellungen|Anlage|Physische|Freigegeben|Nein|Bezieht sich auf den Rahmen der Vorderseite.|
|Spannungssensoren Linie|PCM|Physischen|Freigegeben|Nein|Zahlreiche Zeile Spannungssensoren haben ihren Status angezeigt, der angibt, ob die Spannung gemessene in einem Rahmen an.|
|Aktuelle Sensoren Zeile|PCM|Physischen|Freigegeben|Nein|Aktuelle Sensoren mit zahlreichen Zeile haben ihren Status angezeigt, der angibt, ob das aktuelle gemessene in einem Rahmen.|
|Temperatursensoren in PCM|PCM|Physische|Freigegeben|Nein|Zahlreiche Temperatursensoren wie Eingang und Hotspot Sensoren sein Zustand angezeigt, an, ob die gemessene Temperatur in einem Rahmen ist.|
|Power-Lieferung [0; 1]|PCM|Physische|Freigegeben|Ja|Eine Zeile wird für jede der Power benötigtes Material in den zwei PCMs befindet sich an der Rückseite des Geräts dargestellt.|
|Kühlung [0; 1]|PCM|Physische|Freigegeben|Ja|Eine Zeile wird für jede der vier Kühllüfter, die in den beiden PCMs dargestellt.|
|Akku [0; 1]|PCM|Physische|Freigegeben|Ja|Eine Zeile wird für jedes der Sicherungsbatterie Module angezeigt, die in der PCM eingesetzt werden.|
|Metis|N/V|Logisch|Freigegeben|N/V|Zeigt den Status der Akkus: Gibt an, ob sie benötigen belasten und sind Annäherung Ende des Lebenszyklus an.|
|Cluster|N/V|Logisch|Freigegeben|N/V|Zeigt den Status der Cluster zwischen den beiden integrierter Controller Modulen erstellt wird.|
|Cluster-Knoten|N/V|Logisch|Freigegeben|N/V|Zeigt den Status der Controller als Teil der Cluster an.|
|Clusterquorum|N/V|Logisch||N/V|Gibt an, dass der meisten Datenträger Mitgliedschaft im Speicherpool Festplatte.|
|Festplatte Daten Speicherplatz|N/V|Logisch|Freigegeben|N/V|Der Speicherplatz, der für die Daten im Speicherpool Festplatte (Festplatte) verwendet wird.|
|Festplatte Management Speicherplatz|N/V|Logisch|Freigegeben|N/V|Der Speicherplatz im Speicherpool für Verwaltungsaufgaben Festplatte reserviert.|
|Festplatte Quorum Speicherplatz|N/V|Logisch|Freigegeben|N/V|Der Speicherplatz im Speicherpool für Clusterquorum Festplatte reserviert.|
|Festplatte Ersatz Speicherplatz|N/V|Logisch|Freigegeben|N/V|Der Speicherplatz im Speicherpool Festplatte für den Austausch Controller reserviert.|
|SSD der Datenbereich|N/V|Logisch|Freigegeben|N/V|Der Speicherplatz für Daten im Speicherpool einfarbige Zustand Laufwerk (SSD) verwendet wird.|
|SSD NVRAM Speicherplatz|N/V|Logisch|Freigegeben|N/V|Der Speicherplatz im Speicherpool SSD, der für NVRAM Logik vorgesehen ist.|
|Festplatte Speicherpool|N/V|Logisch|Freigegeben|N/V|Zeigt den Status der logischen Speicherpool, der vom Gerät Festplatten erstellt wird.|
|SSD Speicherpool|N/V|Logisch|Freigegeben|N/V|Zeigt den Status der logischen Speicherpool, der vom Gerät SSDs erstellt wird.|
|Controller [0; 1] [Status]|E/A|Physische|Controller|Ja|Zeigt den Status der Controller aktiven oder standby-Modus in den Rahmen ist.|
|Temperatursensoren in controller|E/A|Physische|Controller|Nein|Zahlreiche Temperatursensoren e/a-Modul, CPU-Temperatur, DIMM und PCIe Sensoren verfügen über ihren Status angezeigt, die angibt, ob die aufgetreten Temperatur in einem Rahmen ist oder nicht.|
|SAS expander|E/A|Physische|Controller|Nein|Gibt den Status der die serielle angefügten SCSI (SAS) Erweiterungsschaltfläche, die in Verbindung mit dem Controller integrierten Speicher verwendet wird.|
|SAS-Anschluss [0; 1]|E/A|Physische|Controller|Nein|Gibt den Zustand jedes Connectors SAS, der zur Verbindung mit dem Erweiterungssymbol SAS integrierten Speicher verwendet wird.|
|SBB Mid Ebene Verbindung|E/A|Physische|Controller|Nein|Gibt den Status des Verbinders Mid Ebene, die jeder Controller zur Mid-Ebene Verbindung verwendet wird.|
|Core Prozessor|E/A|Physische|Controller|Nein|Gibt den Status der der Prozessorkerne innerhalb jeder Controller.|
|Anlage Electronics power|E/A|Physische|Controller|Nein|Gibt den Status des Systems Power verwendet werden, indem Sie die Anlage.|
|Anlage Electronics Diagnose|E/A|Physische|Controller|Nein|Gibt den Status der vom Controller bereitgestellten Sub-Diagnose Systeme.|
|Baseboard Management Controller (BMC)|E/A|Physische|Controller|Nein|Gibt den Status des Baseboard Management Controller (BMC), also eine spezielle Service-Prozessor, der das Hardware-Gerät bis Sensoren überwacht und kommuniziert mit den Systemadministrator über eine unabhängige Verbindung.|
|Ethernet|E/A|Physische|Controller|Nein|Gibt den Status der einzelnen Netzwerk-Schnittstellen, d. h., die Verwaltung und die Daten-Ports auf dem Controller bereitgestellt.|
|NVRAM|E/A|Physische|Controller|Nein|Gibt den Status der NVRAM, einer permanenten Arbeitsspeicher gesichert Batterieenergie, die Anwendung wichtige Informationen in Betrieb beibehalten fungiert.|

## <a name="component-list-for-ebod-enclosure-of-storsimple-device"></a>Komponentenliste EBOD Inhaltsbereich StorSimple Gerät

In der folgenden Tabelle werden die in der Einheit EBOD von Ihrem lokalen StorSimple Gerät enthaltenen physischen und logischen Komponenten.

|Komponente|Modul|Typ|Speicherort|FRU?|Beschreibung|
|---|---|---|---|---|---|
|Versorgen Sie in Slot [0-11]|Laufwerke|Physische|Freigegeben|Ja|Eine Zeile wird für jedes der Festplatte Laufwerke an der Vorderseite der Einheit EBOD angezeigt.|
|Umgebende Temperatursensor|Anlage|Physische|Freigegeben|Nein|Misst die Temperatur in den Rahmen an.|
|Mid Ebene Temperatursensor|Anlage|Physische|Freigegeben|Nein|Misst die Temperatur der Mid Ebene an.|
|Akustische Erinnerung|Anlage|Physische|Freigegeben|Nein|Gibt an, ob das akustischen Erinnerung-Teilsystem in den Rahmen aktiviert ist.|
|Anlage|Anlage|Physische|Freigegeben|Ja|Gibt das Vorhandensein des Chassis an.|
|Anlage-Einstellungen|Anlage|Physische|Freigegeben|Nein|Verweist auf die OPS oder die Vorderseite der den Rahmen.|
|Spannungssensoren Linie|PCM|Physische|Freigegeben|Nein|Zahlreiche Zeile Spannungssensoren haben ihren Status angezeigt, der angibt, ob die Spannung gemessene in einem Rahmen an.|
|Aktuelle Sensoren Zeile|PCM|Physische|Freigegeben|Nein|Aktuelle Sensoren mit zahlreichen Zeile haben ihren Status angezeigt, der angibt, ob das aktuelle gemessene in einem Rahmen.|
|Temperatursensoren in PCM|PCM|Physische|Freigegeben|Nein|Zahlreiche Temperatursensoren wie Eingang und Hotspot Sensoren verfügen über deren Status angezeigt, der angibt, ob die gemessene Temperatur in einem Rahmen aus.|
|Power-Lieferung [0; 1]|PCM|Physische|Freigegeben|Ja|Eine Zeile wird für jede der Power benötigtes Material in den zwei PCMs befindet sich an der Rückseite des Geräts dargestellt.|
|Kühlung [0; 1]|PCM|Physische|Freigegeben|Ja|Eine Zeile wird für jede der vier Kühllüfter, die in den beiden PCMs dargestellt.|
|Lokaler Speicher [Festplatte]|N/V|Logisch|Freigegeben|N/V|Zeigt den Status der logischen Speicherpool, der vom Gerät Festplatten erstellt wird.|
|Controller [0; 1] [Status]|E/A|Physische|Controller|Ja|Zeigt den Status der Controller im Modul EBOD.|
|Temperatursensoren in EBOD|E/A|Physische|Controller|Nein|Zahlreiche Temperatursensoren aus jeder Controller haben ihren Status angezeigt, der angibt, ob die Temperatur aufgetreten in einem Rahmen an.|
|SAS expander|E/A|Physische|Controller|Nein|Gibt den Status der die Erweiterungsschaltfläche SAS, die Verbindung den integrierten Speicher an den Controller verwendet wird.|
|SAS-Anschluss [0-2]|E/A|Physische|Controller|Nein|Gibt den Zustand jedes Connectors SAS, der zur Verbindung mit dem Erweiterungssymbol SAS integrierten Speicher verwendet wird.|
|SBB Mid Ebene Verbindung|E/A|Physische|Controller|Nein|Gibt den Status des Verbinders Mid Ebene, die jeder Controller zur Mid-Ebene Verbindung verwendet wird.|
|Anlage Electronics power|E/A|Physische|Controller|Nein|Gibt den Status des Systems Power verwendet werden, indem Sie die Anlage.|
|Anlage Electronics Diagnose|E/A|Physische|Controller|Nein|Gibt den Status der vom Controller bereitgestellten Sub-Diagnose Systeme.|
|Verbindung mit Gerätecontroller|E/A|Physische|Controller|Nein|Gibt den Status der Verbindung zwischen dem EBOD e/a-Modul und dem Gerätecontroller.|

## <a name="next-steps"></a>Nächste Schritte
- Um den StorSimple-Manager-Dienst verwenden, um Ihr Gerät zu verwalten, fahren Sie mit [den Dienst StorSimple Manager zum Verwalten von Ihrem Geräts StorSimple verwenden](storsimple-manager-service-administration.md).
 
- Wenn Sie eine Gerätekomponente beheben mit dem beeinträchtigt oder Fehler beim Status, um [Indikatoren für die Überwachung StorSimple](storsimple-monitoring-indicators.md)verweisen müssen. 

- Um eine fehlerhafte Hardware-Komponente zu ersetzen, finden Sie unter [StorSimple Hardwareaustausch von Komponenten](storsimple-hardware-component-replacement.md).

- Wenn Sie Probleme mit Geräten weiterhin, [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md)haben.
