<properties
    pageTitle="Melden Sie sich Analytics Ansicht-Designer | Microsoft Azure"
    description="Ansicht-Designer im Log Analytics können Sie benutzerdefinierte Ansichten erstellen, in der OMS-Verwaltungskonsole, die Daten im Repository OMS andere Visualisierungen enthalten. Dieser Artikel enthält eine Übersicht über Ansicht-Designer und Verfahren zum Erstellen und benutzerdefinierte Ansichten bearbeiten."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer"></a>Log Analytics Ansicht-Designer
Die Ansicht-Designer im Log Analytics können Sie benutzerdefinierte Ansichten erstellen, in der OMS-Verwaltungskonsole, die Daten im Repository OMS andere Visualisierungen enthalten. Dieser Artikel enthält eine Übersicht über Ansicht-Designer und Verfahren zum Erstellen und benutzerdefinierte Ansichten bearbeiten.

Weitere Artikel, die für die Ansicht-Designer verfügbar sind:

- [Kachel Verweis](log-analytics-view-designer-tiles.md) - Bezug der Einstellungen für jede der Kacheln zur Verwendung in Ihre benutzerdefinierten Ansichten zur Verfügung. 
- [Visualisierung Webpart Verweis](log-analytics-view-designer-parts.md) - Bezug der Einstellungen für jede der Kacheln zur Verwendung in Ihre benutzerdefinierten Ansichten zur Verfügung. 


## <a name="concepts"></a>Konzepte
Mit dem Ansicht-Designer erstellte Ansichten enthalten die Elemente in der folgenden Tabelle.

| Webpart | Beschreibung |
|:--|:--|
| Kachel | Klicken Sie auf das Hauptfenster Log Analytics Übersicht Dashboard angezeigt.  Enthält eine visuelle Zusammenfassen von Informationen in die benutzerdefinierte Ansicht.  Verschiedene Typen von Kachel bieten andere Visualisierungen mit Datensätzen in OMS Repository.  Klicken Sie auf die Kachel, um die benutzerdefinierte Ansicht zu öffnen. |
| Benutzerdefinierte Ansicht | Angezeigt, wenn der Benutzer auf die Kachel klickt.  Enthält ein oder mehrere Visualisierung Teile an. |
| Visualisierung von Webparts | Visualisierung von Daten im Repository OMS basierend auf einen oder mehrere [Suchbegriffe Protokoll](log-analytics-log-searches.md).  Die meisten Teile mit werden eine Kopfzeile, die eine Visualisierung auf hoher Ebene bereitstellt und eine Liste der Top-Ergebnisse enthalten.  Typen von anderen Webpart Bereitstellen von Datensätzen im Repository OMS andere Visualisierungen.  Klicken Sie auf Elemente im Abschnitt Protokoll bietet detaillierte Datensätzen gesucht werden soll. |

![Ansicht-Designer (Übersicht)](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>Hinzufügen von Ansicht-Designer zu dem Arbeitsbereich
Zwar Ansicht-Designer in der Vorschau zur Verfügung, müssen Sie es in den Arbeitsbereich hinzufügen, indem Sie im Abschnitt **Einstellungen** des Portals OMS **Features Preview** auswählen.

![Aktivieren der Vorschau](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Erstellen und Bearbeiten von Ansichten

### <a name="create-a-new-view"></a>Erstellen einer neuen Ansicht
Öffnen Sie eine neue Ansicht in der **Ansicht-Designer** , indem Sie auf die Kachel Ansicht-Designer im Hauptfenster OMS Dashboard.

![Ansicht-Designer-Kachel](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Bearbeiten einer vorhandenen Ansicht
Öffnen Sie die Ansicht zum Bearbeiten einer vorhandenen Ansicht in der Ansicht-Designer, indem Sie auf die Kachel im Hauptfenster OMS Dashboard.  Klicken Sie dann auf die Schaltfläche **Bearbeiten** , um die Ansicht in der Ansicht-Designer zu öffnen.

![Bearbeiten einer Ansicht](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Klonen einer vorhandenen Ansicht
Wenn Sie eine Ansicht klonen, erstellt eine neue Ansicht, und klicken Sie in der Ansicht-Designer wird geöffnet.  Die neue Sicht haben denselben Namen wie das Original mit "angefügten bis zum Ende des es kopieren".  Um eine Ansicht klonen, öffnen Sie die vorhandene Ansicht, indem Sie auf die Kachel im Hauptfenster OMS Dashboard aus.  Klicken Sie dann auf die Schaltfläche **Klonen** , um die Ansicht in der Ansicht-Designer zu öffnen.

![Klonen einer Ansicht](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Löschen einer vorhandenen Ansicht
Wenn Sie eine vorhandene Ansicht löschen möchten, öffnen Sie die Ansicht, indem Sie auf die Kachel im Hauptfenster OMS Dashboard aus.  Klicken Sie dann klicken Sie auf die Schaltfläche **Bearbeiten** , um die Ansicht in der Ansicht-Designer zu öffnen, und klicken Sie auf **Ansicht löschen**.

![Löschen einer Ansicht](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Exportieren einer vorhandenen Ansicht
Sie können eine Ansicht in eine JSON-Datei exportieren, die Sie importieren in einen anderen Arbeitsbereich oder in einer [Vorlage Ressourcenmanager Azure](../resource-group-authoring-templates.md)verwenden können.  Öffnen Sie die Ansicht, indem Sie auf die Kachel im Hauptfenster OMS Dashboard, um eine vorhandene Ansicht zu exportieren.  Klicken Sie dann auf die Schaltfläche **Exportieren** , um eine Datei im Downloadordner des Browsers zu erstellen.  Der Name der Datei werden die Namen der Ansicht mit der Erweiterung *Omsview*.

![Exportieren einer Ansicht](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Importieren einer vorhandenen Ansicht
Sie können eine *Omsview* -Datei importieren, die Sie aus einer anderen Management Group unter exportiert.  Um eine vorhandene Ansicht zu importieren, müssen Sie zuerst erstellen Sie eine neue Ansicht.  Klicken Sie dann klicken Sie auf die Schaltfläche **Importieren** , und wählen Sie die Datei *Omsview* .  Die Konfiguration in der Datei wird in die vorhandene Sicht kopiert werden.

![Exportieren einer Ansicht](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Arbeiten mit Ansicht-Designer
Die Ansicht-Designer besteht aus drei Bereichen.  Im **Entwurfsbereich** stellt die benutzerdefinierte Ansicht.  Wenn Sie Kacheln und Teile, aus dem Bereich **Steuerelement** im **Entwurfsbereich** , die sie zur Ansicht hinzugefügt werden hinzufügen.  Im Bereich **Eigenschaften** werden die Eigenschaften für die Kachel oder ausgewählten Teils angezeigt.

![Ansicht-Designer](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Konfigurieren der Kachel "Ansicht"
Eine benutzerdefinierte Ansicht kann nur eine einzelne Druckseite verfügen.  Wählen Sie die **Kachel** Registerkarte im **Steuerelement** zum Anzeigen der aktuellen Kachel, oder wählen einen alternativen aus.  Im Bereich **Eigenschaften** werden die Eigenschaften für das aktuelle Fenster angezeigt.  Konfigurieren Sie die Kachel-Eigenschaften entsprechend den detaillierte Informationen in der [Kachel verweisen](log-analytics-view-designer-tiles.md) , und klicken Sie auf **Übernehmen** , um die Änderungen zu speichern.

### <a name="configure-visualization-parts"></a>Konfigurieren des Webparts Visualisierung
Eine Ansicht kann beliebig viele Visualisierung Teile enthalten.  Wählen Sie die Registerkarte **Ansicht** , und klicken Sie dann eine Visualisierung-Webpart, um die Ansicht hinzuzufügen.  Im Bereich **Eigenschaften** werden die Eigenschaften für das ausgewählte Teil angezeigt.  Konfigurieren der Eigenschaften der Datenansicht entsprechend den detaillierte Informationen in der [Visualisierung Webpart Bezug](log-analytics-view-designer-parts.md) aus, und klicken Sie auf **Übernehmen** , um die Änderungen zu speichern.

### <a name="delete-a-visualization-part"></a>Löschen eines Webparts Visualisierung
Sie können durch Klicken auf die Schaltfläche **X** in der oberen rechten Ecke des Webparts ein Webpart Visualisierung aus der Ansicht entfernen.

### <a name="rearrange-visualization-parts"></a>Visualisierung Teile neu anordnen
Ansichten haben nur eine Zeile mit Visualisierung WebParts.  Anordnen von vorhandenen Teilen einer Ansicht durch Klicken und ziehen diese an eine neue Position.


## <a name="next-steps"></a>Nächste Schritte

- Fügen Sie zu Ihrer benutzerdefinierten Ansicht [Kacheln](log-analytics-view-designer-tiles.md) hinzu.
- Hinzufügen von [Visualisierung Webparts](log-analytics-view-designer-parts.md) können Sie Ihre benutzerdefinierten Ansicht.
