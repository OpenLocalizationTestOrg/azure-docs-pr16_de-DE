<properties
   pageTitle="Ressourcenmanager und Servicemanagement (klassische) Bereitstellung Modi | Microsoft Azure"
   description="Erfahren Sie die Unterschiede zwischen Ressourcenmanager und klassischen Bereitstellungsmodelle aus."
   services="virtual-network"
   documentationCenter=""
   authors="telmosampaio"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/11/2016"
   ms.author="telmos"/>

# <a name="azure-deployment-models"></a>Modelle Azure-Bereitstellung

Die Azure-Plattform befindet sich im Übergang.  Gleich, ob Sie neu bei Azure oder haben mit diesem Gerät beigetragen, ist es wichtig, einige der wichtigsten Änderungen zu verstehen, die wir zur Plattform während dieses Übergangs vornehmen möchten.

Alle Azure Ressourcen unterstützen eine oder beide der folgenden Bereitstellungsmodelle:

- **Ressourcenmanager:** Dies ist die neueste Modell zur Bereitstellung für Azure Ressourcen. Die meisten neuere Ressourcen bereits unterstützen dieses Modell zur Bereitstellung und schließlich für alle Ressourcen erfolgt.   
 
- **Klassischen:** Dieses Modell wird von den meisten vorhandenen Azure Ressourcen heute unterstützt. Neue Ressourcen Azure hinzugefügt werden dieses Modell nicht unterstützt.

Die Dokumentation für jede Azure Ressourcendetails der Dienst es Modelle kann mit erstellt werden.

## <a name="why-does-this-matter"></a>Warum ist dies wichtig? 

Es ist wichtig, aus den folgenden Gründen:

- Die Azure-Plattformfeatures, mit denen Sie unterscheiden sich über diese beiden Modelle.  Beispielsweise erstellt die Ressourcenmanager Ressourcen Bereitstellungsmodell (oder nur Ressourcenmanager) kann erstellt werden mit [Azure Ressourcenmanager Vorlagen](azure-resource-manager/resource-group-overview.md#template-deployment), während die Ressourcen erstellt, mit dem klassischen Bereitstellungsmodell können nicht.
- Der einzelne Azure Ressource Features Verhalten können über die beiden Modelle unterschiedlich sein oder nur in einem Modell oder anderen vorhanden sind.  Lastenausgleich Datenfluss über bei Verwendung der Bereitstellung Klassisch erstellte Maschinen beträgt beispielsweise *implizit* , da virtuellen Computern Mitglieder der Azure-Cloud-Dienst sind und Lastenausgleich wird automatisch auf virtuellen Maschinen in einen Clouddienst. Virtuellen Computern erstellt Ressourcenmanager sind keine Mitglieder einer Cloud-Dienst, und eine separate Azure Lastenausgleich Ressource muss *explizit* erstellt, um Saldo Verkehr über mehrere virtuelle Computer zu laden.  
- Wie erstellen, konfigurieren und verwalten Sie Ihre Azure Ressourcen ist der Unterschied zwischen diesen beiden Modellen.
- Erstellt ein Modell zur Bereitstellung von Ressourcen können nicht unbedingt mit Ressourcen, die mit einem anderen Bereitstellungsmodell erstellt zusammenarbeiten. Erstellt ein Modell zur Bereitstellung von Azure virtuellen Computern kann beispielsweise nur mit Azure-virtuellen Netzwerken erstellt, mit der gleichen Modell zur Bereitstellung verbunden werden.    

Der zugrunde liegende jeder der Bereitstellungsmodelle ist eine Application programming Interface (API) für die einzelnen Ressourcen.  Es gibt eine [Ressource-Manager-API](https://msdn.microsoft.com/library/azure/dn948464.aspx) für das Modell zur Bereitstellung von Ressourcenmanager und eine [Service Management-API](https://msdn.microsoft.com/library/azure/ee460799.aspx) für die Bereitstellung klassisch. Entwickler können Code interagieren mit diesen APIs *direkt*schreiben.  

IT-Experten interagieren jedoch in der Regel folgenden APIs *indirekt* mithilfe eines grafischen Portal in einem Webbrowser, mithilfe von Azure-PowerShell-Cmdlets auf einem Windows-Computer oder mithilfe der Azure Command Line Interface (CLI) auf einem Computer mit einer Windows, OS X oder Linux. Alle drei Methoden indirekten von IT-Profis verwendeten interagieren direkt mit den APIs. Dies bedeutet, die bei neuer Funktionen zur Azure-Plattform oder Ressourcen eingeführt werden ist, es immer direkt über die API verfügbar zunächst mit den indirekt Methoden zur Unterstützung für die neue Ressourcen und Features erhalten, nachdem die-API zur Verfügung gestellt wird.  

In den folgenden Abschnitten erläutern, wie Azure Ressourcen mit dem die der verschiedenen Bereitstellungsmodelle über die drei indirekten Methoden konfiguriert werden.

## <a name="portals"></a>Communityportalen
Azure besteht aus zwei communityportalen:

- ** [Azure-Portal](https://manage.windowsazure.com):** Wenn Sie Azure für eine Weile verwendet haben, haben Sie dieses Portal verwendet. Es wird zum Erstellen und Konfigurieren von älteren Azure Ressourcen, die das Bereitstellungsmodell klassischen unterstützen. Sie können ihn erstellen oder Konfigurieren von Ressourcen, die nur Ressourcenmanager unterstützen. 
- ** [Azure Vorschau Portal](https://azure.microsoft.com/overview/preview-portal/):** Wenn Sie eine neuere Azure Ressource verwenden, haben Sie wahrscheinlich dieses Portal verwendet haben. Es kann zu erstellen und konfigurieren einige Azure Ressourcen verwendet werden. Schließlich zwar können erstellen und konfigurieren alle Azure Ressourcen mit. Für einige Ressourcen, die beide Bereitstellungsmodelle unterstützen, kann dieses Portal zu erstellen und Konfigurieren einer Ressource verwenden entweder Modell zur Bereitstellung verwendet werden. 

Einige Ressourcen und Features, können nur erstellt und in einem Portal oder anderen konfiguriert werden. Einige Features oder Ressourcen (noch) werden können nicht erstellt oder entweder Portal konfiguriert und können nur mit PowerShell, die CLI oder beide konfiguriert werden. Die Dokumentation für jede Azure Ressource erläutert, welches Verfahren mit erstellt werden können. 

## <a name="powershell"></a>PowerShell
Mit [PowerShell](powershell-install-configure.md) können Sie eine Befehlszeile oder Autor Skripts zu erstellen und Konfigurieren von Azure Ressourcen aus einem Windows-Computer.  Einzelne Azure Ressourcen haben [Ressourcenmanager Cmdlets](https://msdn.microsoft.com/library/azure/mt125356.aspx), [Servicemanagement Cmdlets](https://msdn.microsoft.com/library/azure/dn708504.aspx)oder beides.  Einige Ressourcen und Features können nur erstellt werden, und/oder mithilfe der PowerShell oder der CLI konfiguriert. Je nach der Ressource, wenn Ressourcenmanager PowerShell-Cmdlets verwenden müssen Sie zwei Optionen für das Erstellen und Konfigurieren von Azure Ressourcen:

- **Nur PowerShell-Cmdlets:** Sie können erstellen und konfigurieren jede Azure Ressource einzeln über die Cmdlets für die einzelnen Ressourcen. Sie können dafür über die Befehlszeile, oder indem Sie mehrere Befehle in einem PowerShell-Skript, das Sie speichern können und die Version.

- **PowerShell-Cmdlets mit einer Vorlage Azure Ressourcenmanager:** PowerShell können Azure Ressourcen mithilfe einer Vorlage Azure Ressourcenmanager erstellen. Vorlagen können gespeicherten und welche Version vorliegen. Weitere Informationen finden Sie im Artikel [Bereitstellen einer Anwendung mit Azure Ressourcenmanager Vorlage](resource-group-template-deploy.md) lesen. Vorhanden sein, mehrere [Azure Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates/) für allgemeine Lösungen, die heruntergeladen und auch geändert werden können.

## <a name="cli"></a>CLI
Sie können erstellen und Konfigurieren von Azure Ressourcen aus mithilfe der CLI auf Computern mit Windows, OS X oder Linux.  Lesen Sie den Artikel [Azure CLI installieren](xplat-cli-install.md) , um die CLI in Ihr Betriebssystem Wahl zu installieren. Wie PowerShell gibt es verschiedene Befehle, die verwendet werden müssen, je nachdem, ob Sie Ressourcen mithilfe der [Ressourcenmanager](xplat-cli-azure-resource-manager.md) oder die [klassischen (Servicemanagement)](./virtual-machines/virtual-machines-linux-classic-manage-visual-studio.md) Bereitstellungsmodelle erstellen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Ressourcenmanager](azure-resource-manager/resource-group-overview.md).
- Grundlegendes zum [Entwurfsvorlagen](best-practices-resource-manager-design-templates.md).
