<properties 
   pageTitle="Windows-Ereignisprotokoll protokolliert in Log Analytics | Microsoft Azure"
   description="Windows-Ereignisprotokollen sind eine der am häufigsten verwendeten Datenquellen von Log Analytics verwendet.  In diesem Artikel beschreibt das Konfigurieren der Sammlung von Windows-Ereignisprotokollen und Details der Datensätze, die sie im Repository OMS erstellen."
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

# <a name="windows-event-log-data-sources-in-log-analytics"></a>Windows-Ereignisprotokolls Datenquellen Log Analytics

Windows-Ereignisprotokollen sind mit einer der am häufigsten verwendeten [Datenquellen](log-analytics-data-sources.md) für Windows-Agents verwendet werden, da dies die von den meisten Clientanwendungen Informationen und Fehler bei der Anmeldung beim verwendete Methode ist.  Sie können Ereignisse von standard-Protokolle, wie z. B. System und Anwendung erfassen, sowie alle von Applications, die Sie überwachen müssen erstellten benutzerdefinierten Protokolle angeben.

![Windows-Ereignisse](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Konfigurieren von Windows-Ereignisprotokoll protokolliert.

Konfigurieren Sie die Windows-Ereignisprotokollen aus dem [Menü ' Daten ' in Log Analytics-Einstellungen](log-analytics-data-sources.md#configuring-data-sources).

Log Analytics sammeln Ereignisse nur von den Windows-Ereignisprotokollen, die in den Einstellungen angegeben sind.  Sie können ein neues Protokoll hinzufügen, indem Sie den Namen der Log eingeben und auf **+**.  Für jedes Protokoll werden nur Ereignisse mit der ausgewählten Schweregrade erfasst.  Überprüfen Sie die Schweregrade für die bestimmten Protokoll, das Sie erfassen möchten.  Sie können keine weiteren Kriterien zum Filtern von Ereignissen bereitstellen.

![Konfigurieren von Windows-Ereignisse](media/log-analytics-data-sources-windows-events/configure.png)


## <a name="data-collection"></a>Datensammlung

Log Analytics wird jedes Ereignis sammeln, die einer ausgewählten schwere aus einer überwachten Ereignisprotokoll entspricht, wie das Ereignis erstellt wird.  Der Agent zeichnet seine Position in jedes Ereignisprotokoll, das sie von erfasst.  Wenn der Agent für einen Zeitraum Offlinemodus wechselt, klicken Sie dann erfasst Log Analytics Ereignisse aus, in dem letzten Unterbrechung, auch wenn diese Ereignisse erstellt wurden, während der Agent offline war.


## <a name="windows-event-records-properties"></a>Windows-Einträge Ereigniseigenschaften

Windows-Ereignisdatensätze verfügen über einen Typ des **Ereignisses** und verfügen über die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Computer            | Name des Computers, der das Ereignis von erfasst wurden. |
| EventCategory       | Kategorie des Ereignisses. |
| EventData           | Alle Ereignisdaten im unformatierten Format. |
| EventID             | Die Nummer des Ereignisses. |
| EventLevel          | Schwere des Ereignisses numerische Formular. |
| EventLevelName      | Schwere des Ereignisses in Form von Text. |
| Ereignisprotokoll            | Der Name des Ereignisprotokolls, die das Ereignis aus erfasst wurden. |
| ParameterXml        | Ereignis Parameterwerte im XML-Format. |
| "Verwaltungsgruppenname" | Name der Management Group für SCOM-Agents.  Für andere Agents handelt es sich um AOI-<workspace ID> |
| RenderedDescription | Beschreibung des Ereignisses mit Parameterwerte |
| Datenquelle              | Die Quelle des Ereignisses. |
| SourceSystem  | Typ des Agents das Ereignis wurde von erfasst. <br> OpsManager – Windows-Agent, entweder direkte verbinden oder SCOM <br> Linux – alle Linux-agents  <br> AzureStorage – Azure-Diagnose |
| TimeGenerated       | Datum und Uhrzeit des Ereignisses in Windows erstellt wurde. |
| Benutzername            | Der Benutzername des Kontos, der das Ereignis protokolliert. |



## <a name="log-searches-with-windows-events"></a>Log-Suchvorgänge mit Windows-Ereignisse

Die folgende Tabelle enthält verschiedene Beispiele für Protokoll suchen, die Windows-Ereignisprotokollierung Datensätze abgerufen werden.

| Abfrage | Beschreibung |
|:--|:--|
| Typ = Ereignis | Alle Ereignisse für Windows. |
| Typ = Ereignis EventLevelName = zurück | Alle Ereignisse im Windows mit Schwere der Fehler. |
| Typ = Ereignis & #124; Messen count() nach Quelle | Anzahl der Windows-Ereignisse nach Quelle. |
| Typ = Ereignis EventLevelName = Fehler & #124; Messen count() nach Quelle | Zählen von Windows Fehlerereignisse nach Quelle. |

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren von Log Analytics anderen [Datenquellen](log-analytics-data-sources.md) für die Analyse sammeln.
- Erfahren Sie mehr über [Log Suchbegriffe](log-analytics-log-searches.md) , zum Analysieren der Daten von Datenquellen und Lösungen erfasst.  
- Verwenden Sie [Benutzerdefinierte Felder](log-analytics-custom-fields.md) , um die Ereigniseinträge in einzelne Felder zu analysieren.
- Konfigurieren der [Sammlung von Datenquellen](log-analytics-data-sources-performance-counters.md) aus Ihrem Windows-Agents.