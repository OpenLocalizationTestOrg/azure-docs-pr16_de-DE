<properties 
   pageTitle="Datenquellen Log Analytics | Microsoft Azure"
   description="Datenquellen definieren die Daten, dass Log Analytics sammelt von Agents und anderen Datenquellen verbunden.  In diesem Artikel werden des Konzepts der wie Log Analytics Datenquellen verwendet, wird erläutert, so konfigurieren sie die Details und enthält eine Übersicht über die unterschiedlichen Datenquellen verfügbar."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="data-sources-in-log-analytics"></a>Log Analytics Datenquellen

Log Analytics sammelt Daten aus Quellen im Arbeitsbereich OMS verbunden und in OMS Repository gespeichert.  Die Daten, die aus den einzelnen gesammelt werden durch den Datenquellen definiert, die Sie konfigurieren.  Daten im Repository OMS werden als eine Menge von Datensätzen gespeichert.  Jede Datenquelle erstellt Einträge eines bestimmten Typs mit jedes Typs auch eine eigene Gruppe von Eigenschaften an.

![Melden Sie sich Analytics-Datensammlung](./media/log-analytics-data-sources/overview.png)

Datenquellen unterscheiden sich OMS Lösungen, die ebenfalls Sammeln von Daten aus Quellen verbunden und Datensätze im Repository OMS erstellen.  Lösungen zu dem Arbeitsbereich hinzugefügt werden können, aus dem Lösungskatalog und weitere Analysetools im Portal OMS werden in der Regel bereitstellen.  

## <a name="summary-of-data-sources"></a>Zusammenfassung der Datenquellen

Die Datenquellen, die derzeit in Log Analytics verfügbar sind, werden in der folgenden Tabelle aufgelistet.  Jede verfügt über einen Link zu einer separaten Artikel ins Detail, für die Datenquelle.

| Datenquelle | Ereignistyp | Beschreibung |
|:--|:--|:--|
| [Benutzerdefinierte Protokolle](log-analytics-data-sources-custom-logs.md) | \<LogName\>_CL | Textdateien auf Windows oder Linux Agents Log Informationen enthalten sind. |
| [Windows-Ereignisprotokollen](log-analytics-data-sources-windows-events.md) | Ereignis | Auf Computern unter Windows gesammelten Ereignisse aus dem Ereignisprotokoll. |
| [Windows-Datenquellen](log-analytics-data-sources-performance-counters.md) | Perf | -Datenquellen, die von Windows-Computern gesammelt werden. |
| [Linux-Datenquellen](log-analytics-data-sources-performance-counters.md) | Perf | Datenquellen, die von Linux Computern gesammelt werden. |
| [IIS-Protokolle](log-analytics-data-sources-iis-logs.md) | W3CIISLog | Internetinformationsdienste protokolliert im W3C-Format. |
| [Syslog](log-analytics-data-sources-syslog.md) | Syslog | Syslog Ereignisse auf Computern mit Windows oder Linux. |

## <a name="configuring-data-sources"></a>Konfigurieren von Datenquellen

Sie Konfigurieren von Datenquellen aus dem Menü **Daten** in Log Analytics- **Einstellungen**aus.  Jede Konfiguration wird an alle verbundenen Datenquellen in dem OMS Arbeitsbereich übermittelt.  Sie können keine Agents aktuell aus dieser Konfiguration ausschließen.

![Konfigurieren von Windows-Ereignisse](./media/log-analytics-data-sources/configure-events.png)

2. Wählen Sie in der Verwaltungskonsole OMS die Kachel " **Einstellungen** " aus.
3. Wählen Sie **Daten**aus.
4. Klicken Sie auf die Datenquelle zu konfigurieren.
5. Folgen Sie den Link in der Dokumentation für jede Datenquelle in der obigen Tabelle Details auf deren Konfiguration.

## <a name="data-collection"></a>Datensammlung

Data Source Konfigurationen werden an Agents übermittelt, die direkt mit OMS innerhalb weniger Minuten verbunden sind.  Die angegebenen Daten aus der Agent erfasst und direkt an Analytics Log in Abständen, die speziell für jede Datenquelle übermittelt.  Finden Sie in der Dokumentation für jede Datenquelle für diese Angaben.

System Center Operations Manager (SCOM)-Agents in einer verbundenen Management Group unter sind Data Source Konfigurationen Management Packs übersetzt und an die Management Group unter 5 Minuten standardmäßig übermittelt.  Der Agent das Management Pack wie jedes andere downloads und sammelt die angegebenen Daten. Je nach Datenquelle werden die Daten entweder auf einem Server Management gesendet, der die Daten an der Log Analytics weiterleitet, oder der Agent sendet die Daten an Log Analytics ohne zu durchlaufen Management-Server. [Einzelheiten zur Datensammlung für OMS-Features und Lösungen](log-analytics-add-solutions.md#data-collection-details-for-oms-features-and-solutions) für Details finden Sie unter.  Sie erhalten Informationen über Details SCOM und OMS verbinden, und ändern die Häufigkeit dieser Konfiguration am [Konfigurieren die Integration mit System Center Operations Manager](log-analytics-om-agents.md)übermittelt wird.

## <a name="log-analytics-records"></a>Analytics Protokolldatensätze

Alle von Log Analytics gesammelte Daten wird als Datensätze im OMS Repository gespeichert.  Datensätze, die von unterschiedlichen Datenquellen gesammelt haben ihre eigenen Gruppe von Eigenschaften und durch **ihre Eigenschaft** identifiziert werden.  Finden Sie unter der Dokumentation für jede Datenquelle und Lösung Details auf jeden Datensatztyp.


## <a name="next-steps"></a>Nächste Schritte

- Informationen Sie zu [Lösungen](log-analytics-add-solutions.md) , die zusätzliche Funktionen für Protokoll Analytics und auch Daten in das OMS Repository sammeln.
- Erfahren Sie mehr über [Log Suchbegriffe](log-analytics-log-searches.md) , zum Analysieren der Daten von Datenquellen und Lösungen erfasst.  
- Konfigurieren Sie [Benachrichtigungen](log-analytics-alerts.md) , um die vorausschauende wichtiger auf Datenquellen und Lösungen erfassten Daten benachrichtigt.
