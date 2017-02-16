<properties
    pageTitle="Computergruppen in Log Analytics melden Suchbegriffe | Microsoft Azure"
    description="Computergruppen in Log Analytics ermöglichen es Ihnen, Log Suchbereiche auf eine bestimmte Gruppe von Computern.  In diesem Artikel werden die unterschiedlichen Methoden, die Sie verwenden können, zum Erstellen von Computergruppen und wie sie in einer Suche Log verwendet."
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
    ms.date="09/06/2016"
    ms.author="bwren"/>

# <a name="computer-groups-in-log-analytics-log-searches"></a>Computergruppen in Log Analytics melden Suchbegriffe
Computergruppen in Log Analytics ermöglichen Umfang [Suchbegriffe melden Sie sich](log-analytics-log-searches.md) auf eine bestimmte Gruppe von Computern.  Mit Computern, die entweder mithilfe einer Abfrage, die Sie definieren oder Gruppen aus unterschiedlichen Quellen importieren, wird jede Gruppe aufgefüllt.  Wenn die Gruppe in einer Suche Log enthalten ist, sind die Ergebnisse auf die Datensätze, die der Computer in der Gruppe entsprechen.

## <a name="creating-a-computer-group"></a>Erstellen einer Computergruppe
Sie können eine Computergruppe in Log Analytics mithilfe einer der Methoden in der folgenden Tabelle erstellen.  In den folgenden Abschnitten werden die Details für jede Methode bereitgestellt. 

| Methode | Beschreibung |
|:---|:---|
| Log-Suche       | Erstellen Sie ein Protokoll suchen, die eine Liste der Computer zurückgibt, und speichern Sie die Ergebnisse als Computergruppe. |
| Melden Sie sich suchen API   | Verwenden Sie die Log-Suche-API programmgesteuert eine Computergruppe basierend auf den Ergebnissen einer Suche Log erstellen. |
| Active Directory | Scannen Sie automatisch die Gruppenmitgliedschaft eines Agent-Computern, die Mitglieder einer Active Directory-Domäne sind, und erstellen Sie eine Gruppe für jede Sicherheitsgruppe in Log Analytics ein.
| WSUS              | Automatisch Scannen von WSUS-Servern oder Clients Hinblick auf Gruppen und erstellen Sie eine Gruppe für jede in Log Analytics. |


### <a name="log-search"></a>Log-Suche

Computergruppen erstellt von einer Suche Log enthält alle Computer zurückgegeben, indem Sie eine Suchabfrage, die Sie definieren.  Diese Abfrage wird ausgeführt, jedes Mal, wenn die Gruppe des Computers verwendet wird, damit alle Änderungen, die seit der Erstellung der Gruppe erkennbar werden.

Verwenden Sie das folgende Verfahren, um einer Computergruppe aus einer Suche Protokoll zu erstellen.

1. [Erstellen eines Suchbereichs Log](log-analytics-log-searches.md) , der eine Liste der Computer zurückgibt.  Die Suche muss distinct mehrere Computer mithilfe eines Beitrags, wie **Verschiedene Computer** oder **Measure count() vom Computer** in der Abfrage zurück.  
2. Klicken Sie auf die Schaltfläche **Speichern** am oberen Rand des Bildschirms.
3. Wählen Sie **Ja** zu **speichern diese Abfrage als Computergruppe:**.
4. Geben Sie einen **Namen** und eine **Kategorie** für die Gruppe ein.  Wenn eine Suche mit demselben Namen und Kategorie bereits vorhanden ist, werden Sie aufgefordert, es zu überschreiben.  Sie können mehrere Suchvorgänge mit demselben Namen in unterschiedlichen Kategorien haben. 

Nachstehend finden Sie Beispiel suchen, die Sie als Computergruppe speichern können.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

### <a name="log-search-api"></a>Melden Sie sich suchen-API

Computergruppen erstellt, mit der Log-Suche-API sind identisch mit einer Suche Log erweiterte Suche.

Ausführliche Informationen zum Erstellen einer Computergruppe mit der Log-Suche-API finden Sie unter [Computergruppen in Log Analytics melden Sie sich suchen REST-API](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Active Directory

Wenn Sie Log Analytics zum Importieren von Active Directory-Gruppenmitgliedschaft konfigurieren, wird es die Gruppenmitgliedschaft eine Domäne verbundene Computer mit dem Agent OMS analysieren.  Eine Computergruppe in Log Analytics für jede Sicherheitsgruppe in Active Directory erstellt, und jeder Computer den Computer Gruppen entspricht, die, denen Sie gehören, Sicherheitsgruppen hinzugefügt wird.  Alle 4 Stunden diese Mitgliedschaft kontinuierlich aktualisiert.  

Konfigurieren Sie Log Analytics zum Importieren von Active Directory-Sicherheitsgruppen aus dem Menü **Computergruppen** in Log Analytics- **Einstellungen**aus.  Wählen Sie **Automatisierung** , und klicken Sie dann **Importieren Active Directory-Gruppenmitgliedschaft von Computern**.  Es ist keine weitere Konfiguration erforderlich.

![Computergruppen aus Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Wenn Gruppen importiert wurden, werden im Menü aufgelistet, die Anzahl der Computer mit Gruppenmitgliedschaft erkannt und die Anzahl der Gruppen importiert.  Klicken Sie auf eine der folgenden Links, um die Einträge **ComputerGroup-Objekts** mit diesen Informationen zurückzukehren.

### <a name="windows-server-update-service"></a>Windows Server Update-Dienst

Wenn Sie Log Analytics Gruppenmitgliedschaft WSUS importieren konfigurieren, wird es die Zielgruppenadressierung Gruppenmitgliedschaft von einem beliebigen Computer mit OMS Agent analysieren.  Wenn Sie clientseitige verwenden haben verwendet, auf allen Computern, die mit OMS verbunden ist, und ist Teil einer beliebigen WSUS verwendet, Gruppen seine Gruppenmitgliedschaft zu Log Analytics importiert. Bei Verwendung von serverseitigen sollte das unterstellungsverhältnis verwendet, die OMS Agent auf der die zentrale in Reihenfolge für die Gruppenmitgliedschaftsinformationen zu OMS importiert werden sollen installiert werden.  Alle 4 Stunden diese Mitgliedschaft kontinuierlich aktualisiert. 

Konfigurieren Sie Log Analytics zum Importieren von Active Directory-Sicherheitsgruppen aus dem Menü **Computergruppen** in Log Analytics- **Einstellungen**aus.  Wählen Sie aus **Active Directory** , und klicken Sie dann **Importieren Active Directory-Gruppenmitgliedschaft von Computern**.  Es ist keine weitere Konfiguration erforderlich.

![Computergruppen aus Active Directory](media/log-analytics-computer-groups/configure-wsus.png)

Wenn Gruppen importiert wurden, werden im Menü aufgelistet, die Anzahl der Computer mit Gruppenmitgliedschaft erkannt und die Anzahl der Gruppen importiert.  Klicken Sie auf eine der folgenden Links, um die Einträge **ComputerGroup-Objekts** mit diesen Informationen zurückzukehren.

## <a name="managing-computer-groups"></a>Verwalten von Computergruppen

Sie können Computergruppen anzeigen, die aus einer Log suchen oder die Suche Log-API aus dem Menü **Computergruppen** in Log Analytics- **Einstellungen**erstellt wurden.  Klicken Sie auf das **X** in der Spalte **Entfernen** , um die Gruppe des Computers zu löschen.  Klicken Sie auf das Symbol **Ansicht Mitglieder** für eine Gruppe auf die Gruppe Log Suchen ausführen, der die Mitglieder zurückgibt. 

![Gespeicherten Computergruppen](media/log-analytics-computer-groups/configure-saved.png)

Um die Gruppe zu ändern, erstellen Sie eine neue Gruppe mit der gleichen **Kategorie** und **Namen** in der ursprünglichen Gruppe überschreiben aus.

## <a name="using-a-computer-group-in-a-log-search"></a>Verwenden einer Computergruppe in einer Log-Suche
Verwenden Sie die folgende Syntax, um zu einer Computergruppe in einer Suche Log verweisen.  Angeben, dass die **Kategorie** optional und nur ist erforderlich, wenn Sie verschiedene Kategorien Computergruppen mit demselben Namen haben. 

    $ComputerGroups[Category: Name]

Wenn eine Suche ausgeführt wird, werden zuerst die Mitglieder von Computergruppen in der Suche enthalten aufgelöst.  Wenn die Gruppe auf eine Log-Suche basiert, wird, dass die Suche ausgeführt, um die Mitglieder der Gruppe zurückgegeben werden, bevor der obersten Ebene Log Suche.

Computergruppen werden normalerweise mit der **IN** -Klausel in das Protokoll suchen, wie im folgenden Beispiel verwendet.

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

## <a name="computer-group-records"></a>Gruppieren von Datensätzen Computer

Ein Datensatz ist im OMS Repository für jeden Computergruppenmitgliedschaft aus Active Directory oder WSUS erstellt erstellt.  Diese Datensätze haben einen Typ von **ComputerGroup-Objekts** und die Eigenschaften in der folgenden Tabelle.  Datensätze werden für Computergruppen, die auf Grundlage der Log-Suche nicht erstellt werden.

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ                | *ComputerGroup-Objekts* |
| SourceSystem        | *SourceSystem*  |
| Computer            | Name des Computers ein Mitglied. |
| Gruppe               | Namen der Gruppe. |
| GroupFullName       | Vollständigen Pfad in der Gruppe, einschließlich der Quell- und Quellnamen ein.
| GroupSource         | Datenquelle dieser Gruppe von zusammengestellten wurde. <br><br>Active Directory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName     | Name der Datenquelle, die die Gruppen von gesammelt wurde.  Dies ist für Active Directory den Domänennamen. |
| "Verwaltungsgruppenname" | Name der Management Group für SCOM-Agents.  Für andere Agents, dies ist AOI -\<Arbeitsbereich-ID\> |
| TimeGenerated       | Datum und Uhrzeit die Computergruppe erstellt oder aktualisiert wurde. |



## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Log Suchbegriffe](log-analytics-log-searches.md) , zum Analysieren der Daten von Datenquellen und Lösungen erfasst.  