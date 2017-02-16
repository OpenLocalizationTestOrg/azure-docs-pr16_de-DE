<properties
    pageTitle="DevTest Labs Konzepte | Microsoft Azure"
    description="Erfahren Sie, die grundlegenden Konzepte von DevTest Labs und wie es erleichtern kann erstellen, verwalten und Überwachen von Azure-virtuellen Computern"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="tarcher"/>

#<a name="devtest-labs-concepts"></a>DevTest Labs Konzepte

> [AZURE.NOTE]
> Dieser Artikel ist Teil 3 von 3 Teilen:
> 
> 1. [Was ist DevTest Labs?](devtest-lab-overview.md)
> 1. [Warum DevTest Labs?](devtest-lab-why.md)
> 1. **[DevTest Labs Konzepte](devtest-lab-concepts.md)**

##<a name="overview"></a>(Übersicht)

Die folgende Liste enthält DevTest Labs grundlegende Konzepte und Definitionen:

##<a name="artifacts"></a>Elemente
Elemente werden zum Bereitstellen und konfigurieren die Anwendung, nachdem ein virtueller Computer bereitgestellt wird. Elemente sind möglich:

- Tools, die Sie Installieren des virtuellen Computers – wie etwa Agents, Fiddler und Visual Studio möchten.
- Aktionen, die Sie Ausführen des virtuellen Computers – wie etwa einer Repo klonen möchten.
- Programme, die Sie testen möchten.

Elemente sind [Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md) JSON-Dateien, die enthalten Anweisungen zum Ausführen der Bereitstellung und Konfiguration anwenden. 

##<a name="artifact-repositories"></a>Element Repositorys
Element Repositorys sind Git Repositorys, in dem Elemente eingecheckt sind. Mehrere Labs in Ihrer Organisation Wiederverwendung und Freigabe können derselben Element Repositorys hinzugefügt werden.

## <a name="base-images"></a>Basis Bilder
Basis Bilder sind virtueller Computer Bilder mit den Tools und Einstellungen vorinstalliert und konfiguriert, um schnell ein virtuellen Computers zu erstellen. Sie können einen virtuellen Computer bereitstellen, indem Sie eine vorhandene Base auswählen und ein Element aus, um den Test-Agent zu installieren. Sie können den bereitgestellten virtuellen Computer dann als Basis speichern, damit die Basis verwendet werden kann, ohne den Test-Agent für jede Bereitstellung von den virtuellen Computer installieren zu müssen.

##<a name="formulas"></a>Formeln 
Formeln, die neben Basis Bildern stellen ein Verfahren schnelles virtueller Computer provisioning bereit. Eine Formel in DevTest Einheiten handelt es sich um eine Liste der Standardwerte für Eigenschaften zum Erstellen einer Übung virtueller Computer verwendet. Mit Formeln, virtueller Computer mit demselben Satz von Eigenschaften – wie Basis-Image, virtueller Computer Größe, virtuelles Netzwerk und Elemente – können erstellt werden, ohne dass diese Eigenschaften jedes Mal angeben müssen. Beim Erstellen eines virtuellen Computers aus einer Formel können die Standardwerte als verwendet werden – oder geändert wird.

##<a name="caps"></a>Feststelltaste
Feststelltaste Through-Abfälle in Ihrem Kurs Minimieren wird. Beispielsweise können Sie festlegen, eine Linienende, um die Anzahl der virtuellen Computern einzuschränken, die pro Benutzer oder in einem Kurs erstellt werden können.

##<a name="policies"></a>Richtlinien
Richtlinien zur Unterstützung bei Kosten in Ihr Kurs steuern. Beispielsweise können Sie eine Richtlinie virtuellen Computern basierend auf einem festgelegten Zeitplan automatisch beenden erstellen.

##<a name="security-levels"></a>Sicherheitsstufen
Sicherheitszugriff wird durch Azure Role-Based Access Steuerelement (RBAC) festgelegt. Zum Verständnis der Funktionsweise von Access können Sie die Unterschiede zwischen einer Berechtigungsstufe, einer Rolle und einen Bereich durch RBAC definierten kennen müssen. 

- Berechtigung - einer Berechtigungsstufe ist eine definierten Zugriff auf eine bestimmte Aktion (z. B. Lese-Zugriff auf alle virtuellen Computern). 
- Rolle - ist eine Rolle eines Satzes von Berechtigungen, die gruppiert und einem Benutzer zugeordnet werden kann. Angenommen, hat die Rolle des *Abonnements sind* Zugriff auf alle Ressourcen innerhalb eines Abonnements. 
- Bereich – ein Bereich ist eine Ebene in der Hierarchie von einer Ressource Azure - z. B. eine Ressourcengruppe, eine einzelne Übung oder das gesamte Abonnement).
 
Innerhalb des Gültigkeitsbereichs von DevTest Labs, es gibt zwei Arten von Rollen zum Definieren von Benutzerberechtigungen: Übung Besitzer und Übung Benutzer.

- Übung Besitzer - hat ein Besitzer Übung Zugriff auf alle Ressourcen innerhalb der Übung. Daher kann Besitzer einer Übung Richtlinien, lesen und Schreiben alle virtuellen Computern, ändern das virtuelle Netzwerk, und so weiter. 
- Übung Benutzer - ein Benutzers Übung alle Übung Ressourcen, wie z. B. virtuelle Computer, virtuelle Netzwerke und Richtlinien kann anzeigen, aber Richtlinien kann nicht geändert oder alle virtuellen Computern, die von anderen Benutzern erstellte. 


Wenn zum Erstellen von benutzerdefinierter Rollen in DevTest Kursen anzeigen möchten, finden Sie im Artikel, [erteilen Benutzerberechtigungen für bestimmte Übung Richtlinien](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Da Bereiche hierarchische, sind, wenn Sie einem Benutzer mit einem bestimmten Bereich zugewiesen wurden, werden sie automatisch diese Berechtigungen bei jeder Low-Level-Bereich umfasst erteilt. Z. B., wenn ein Benutzer die Rolle des Abonnements sind zugewiesen ist, müssen dann diese Zugriff auf alle Ressourcen in einem Abonnement, die allen virtuellen Computern, alle virtuellen Netzwerke und alle Übungseinheiten einschließen. Daher erbt die Abonnementbesitzer einer automatisch die Rolle des Übung Besitzer. Die entgegengesetzt ist jedoch nicht richtig. Die Besitzer einer Übung hat Zugriff auf eine Übung, also einen unteren Bereich als Abonnement-Level. Daher werden Besitzer einer Übung nicht sehen, virtuellen Computern oder virtuelle Netzwerke oder alle Ressourcen, die sich außerhalb der Übung befinden.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

##<a name="next-steps"></a>Nächste Schritte

[Erstellen einer Übung in DevTest Einheiten](devtest-lab-create-lab.md)