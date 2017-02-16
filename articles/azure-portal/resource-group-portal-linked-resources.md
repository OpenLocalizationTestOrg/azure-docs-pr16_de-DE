<properties 
    pageTitle="Verwandte und verknüpften Ressourcen in der Kachel-Katalog" 
    description="Informationen Sie zu verwandten und verknüpfte Ressourcen, die im Katalog der Kachel des Portals Azure Vorschau angezeigt werden." 
    services="azure-portal" 
    documentationCenter="" 
    authors="adamabdelhamed" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-portal" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/16/2015" 
    ms.author="adamab"/>

# <a name="related-and-linked-resources-in-the-tile-gallery"></a>Verwandte und verknüpften Ressourcen in der Kachel-Katalog

Klicken Sie im Katalog Kachel können Sie Kacheln für eine bestimmte Ressource suchen, und ziehen diese auf Ihrer aktuellen Blade. Klicken Sie im Katalog Kachel können Sie Management Ansichten erstellen, die Ressourcen verteilt. Für eine bestimmte Ressource gehören die zugehörigen Ressourcen an alle Ressourcen, die die gleichen Ressourcengruppe als Ressource freigeben und alle Ressourcen, die in den oder aus der Ressource verknüpfen.

## <a name="linked-resources-in-azure-resource-manager"></a>Verknüpfte Ressourcen in Azure Ressourcenmanager

Verknüpfen ist ein Feature über Azure-Manager.  Sie können Sie Beziehungen zwischen Ressourcen deklarieren, auch wenn diese nicht in derselben Ressourcengruppe befinden. Durch das Verknüpfen hat keine Auswirkung auf die Laufzeit von Ressourcen, keine Auswirkung auf Abrechnung und keine Auswirkung auf Access rollenbasierte.  Es ist einfach ein Verfahren, die Sie verwenden können, um Beziehungen darzustellen, sodass erzielen Sie Tools, wie der Kachel Katalog eine leistungsfähigen Management bereitgestellt werden.  Ihre Tools können prüfen die Links, über die Links API und bieten auch benutzerdefinierte Beziehung Management auftritt. 

## <a name="how-do-i-link-my-resources"></a>Wie link ich meine Ressourcen?

Wenn Sie Ressourcen über das Portal oder indem Sie eine Vorlage über Azure PowerShell oder Azure CLI erstellen, werden die Links für einige abhängigen Ressourcen automatisch erstellt. Sie können auch programmgesteuert Ressourcen mithilfe der [Verknüpfte Ressourcen REST-API](https://msdn.microsoft.com/library/azure/mt238499.aspx) oder indem Sie die Beziehungen in der Vorlage deklarieren verknüpfen. Eine ausführliche Übersicht über das Arbeiten mit verknüpften Ressourcen finden Sie unter [Erstellen einer Verknüpfung Ressourcen in Azure Ressourcenmanager](../resource-group-link-resources.md).

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie eine Einführung in das Schreiben von Azure Ressourcenmanager Vorlagen benötigen, finden Sie unter [Authoring-Vorlagen](../resource-group-authoring-templates.md).
- Um Informationen zum Erstellen von Verknüpfungen zwischen Ressourcen ausführlicher erläutert wird, finden Sie unter [Erstellen einer Verknüpfung Ressourcen in Azure Ressourcenmanager](../resource-group-link-resources.md).
- Weitere Informationen zum Arbeiten mit Ressourcengruppen über die Vorschau-Portal finden Sie unter [Using the Azure Preview-Portal zum Verwalten Ihrer Azure Ressourcen](resource-group-portal.md).
