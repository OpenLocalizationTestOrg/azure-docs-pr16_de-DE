<properties
    pageTitle="Azure DevTest Labs häufig gestellte Fragen zu | Microsoft Azure"
    description="Hier finden Sie Antworten auf häufig gestellte Fragen für Azure DevTest Labs"
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
    ms.date="09/13/2016"
    ms.author="tarcher"/>

# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs häufig gestellte Fragen

Dieser Artikel beantwortet einige der am häufigsten Fragen zur Azure DevTest Labs.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="general"></a>Allgemeine
- [Was geschieht, wenn mein Frage hier beantwortet nicht zur Verfügung?](#what-if-my-question-isnt-answered-here)
- [Warum sollte ich Azure DevTest Labs werden verwendet?](#why-should-i-use-azure-devtest-labs) 
- [Was bedeutet "komfortablen, Self-service"?](#what-does-quotworry-free-self-servicequot-mean)
- [Wie kann ich Azure DevTest Labs verwenden?](#how-can-i-use-azure-devtest-labs) 
- [Wie verwende ich Azure DevTest Labs in Rechnung gestellt?](#how-am-i-billed-for-azure-devtest-labs) 
 
## <a name="security"></a>Sicherheit 
- [Was sind die verschiedenen Sicherheitsstufen in Azure DevTest Kursen?](#what-are-the-different-security-levels-in-azure-devtest-labs) 
- [Wie erstelle ich eine Rolle aus, damit Benutzer einen bestimmten Vorgang ausführen können?](#how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task) 
 
## <a name="cicd-integration--automation"></a>CI/CD-Integration und Automatisierung 
 
- [Lässt sich Azure DevTest Labs mit meinem CI/CD Toolchain integrieren?](#does-azure-devtest-labs-integrate-with-my-cicd-toolchain) 
 
## <a name="virtual-machines"></a>Virtuellen Computern 
 
- [Warum werden angezeigt nicht bestimmte virtuellen Computern in das Blade Azure-virtuellen Computern, die ich in Azure DevTest Labs angezeigt werden?](#why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs) 
- [Was ist der Unterschied zwischen benutzerdefinierten Bilder und Formeln?](#what-is-the-difference-between-custom-images-and-formulas) 
- [Wie kann ich mehrere virtuelle Computer gleichzeitig aus derselben Vorlage erstellen?](#how-do-i-create-multiple-vms-from-the-same-template-at-once) 
- [Wie verschiebe ich meine vorhandene Azure-virtuellen Computern in Meine Übung Azure DevTest Labs?](#how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab) 
- [Kann mehrere Datenträger zu meiner virtuellen Computern werden angefügt?](#can-i-attach-multiple-disks-to-my-vms) 
- [Wie automatisieren kann ich die Vorgehensweise zum Hochladen von Dateien von virtuellen Festplatte zum Erstellen von benutzerdefinierten Bilder?](#how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images) 
- [Wie kann ich die Vorgehensweise zum Löschen aller virtuellen Computern in meinem Kurs automatisieren?](#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab)
 
## <a name="artifacts"></a>Elemente 
 
- [Was sind die Elemente?](#what-are-artifacts) 
 
## <a name="lab-configuration"></a>Übung Konfiguration 
 
- [Wie erstelle ich eine Übung aus einer Vorlage Ressourcenmanager Azure?](#how-do-i-create-a-lab-from-an-azure-resource-manager-template) 
- [Warum werden meine virtuellen Computern in anderen Ressourcengruppen mit beliebigen Namen werden erstellt? Kann ich umbenennen oder ändern diese Ressourcengruppen?](#why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups) 
- [Wie viele Labs kann ich unter dem gleichen Abonnement erstellen?](#how-many-labs-can-i-create-under-the-same-subscription)
- [Wie viele virtuellen Computern kann ich pro Übung erstellen?](#how-many-vms-can-i-create-per-lab)
- [Wie freigeben kann ich mit meinem Kurs einen direkten Link?](#how-do-i-share-a-direct-link-to-my-lab)
- [Was ist ein Microsoft-Konto?](#what-is-a-microsoft-account)
 
## <a name="troubleshooting"></a>Behandlung von Problemen 
 
- [Fehler bei meinem Element während der Erstellung virtueller Computer Wie behebe ich sie?](#my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it) 
- [Warum Netzwerk ist nicht meine vorhandene virtuelle ordnungsgemäß speichern?](#why-isnt-my-existing-virtual-network-saving-properly)  

### <a name="what-if-my-question-isnt-answered-here"></a>Was geschieht, wenn mein Frage hier beantwortet nicht zur Verfügung?
Wenn Ihre Frage hier nicht aufgeführt ist, lassen Sie uns wissen, und wir helfen Ihnen eine Antwort zu finden.

- Stellen Sie eine Frage im [Disqus Thread](#comments) am Ende dieses häufig gestellte Fragen zu, und lassen Sie das Team Azure Cache und andere Mitglieder der Community Informationen zu den in diesem Artikel.
- Um eine größere Zielgruppe zu erreichen, stellen Sie eine Frage im [Forum Azure DevTest Labs im MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs), und nehmen Sie Kontakt mit der Azure DevTest Labs Teams und andere Mitglieder der Community.
- Um ein Feature anfordern, senden Sie Ihre Anfragen und Ideen, die [Azure DevTest Labs Benutzer VoIP](https://feedback.azure.com/forums/320373-azure-devtest-labs)zu gestalten.

### <a name="why-should-i-use-azure-devtest-labs"></a>Warum sollte ich Azure DevTest Labs werden verwendet? 
Azure DevTest Labs können Ihr Team Zeit und Geld speichern. Entwickler können Sie eigene Umgebungen mit mehreren verschiedene Grundlagen erstellen und verwenden Elemente schnell bereitstellen und Konfigurieren von Applications. Benutzerdefinierte Bilder und Formeln verwenden, können virtuellen Computern als Vorlagen gespeichert und problemlos reproduziert werden. Darüber hinaus bieten Labs mehrere zu konfigurierender Richtlinien, die es Administratoren Abfall zu reduzieren und Verwalten eines Teams Umgebungen Übung ermöglichen. Zu diesen Richtlinien zählen Auto-war(en), Kosten Schwellenwert für den maximalen virtuellen Computern pro Benutzer und die maximale Größe von virtuellen Computer an. Eine genauere Erklärung der Azure DevTest Labs lesen Sie die [Übersicht](devtest-lab-overview.md) oder checken Sie das [Einführungsvideo aus](/documentation/videos/videos/what-is-azure-devtest-labs). 

### <a name="what-does-worry-free-self-service-mean"></a>Was bedeutet "komfortablen, Self-service"?
"Komfortablen, Self-Service" bedeutet, dass Entwickler und Tester eigene Umgebungen nach Bedarf erstellen und Administratoren verfügen über die Sicherheit des wissen, dass Azure DevTest Labs wird minimiert verschwenden und Kosten steuern. Administratoren können festlegen, welche virtuellen Computer Größen zulässig sind, die maximale Anzahl von virtuellen Computern, und wann virtuellen Computern gestartet und beendet. Azure DevTest Labs erleichtert auch die Kosten überwachen und Festlegen von Benachrichtigungen darüber informieren, wie Ressourcen in der Kurs genutzt werden. 

### <a name="how-can-i-use-azure-devtest-labs"></a>Wie kann ich Azure DevTest Labs verwenden? 
Azure DevTest Labs eignet sich jedes Mal, wenn Sie benötigen Entwickler oder Umgebungen testen und diese schnell zu reproduzieren und/oder diese mit Kosten speichern Richtlinien verwalten soll. 

Hier sind einige Szenarien, denen unsere Kunden Azure DevTest Labs für verwenden: 

- Verwalten von Entwicklung und Test-Umgebungen in einer zentralen Stelle aus, erstellt über das Team Nutzung Richtlinien zum Verringern der Kosten und benutzerdefinierte Bilder freigeben.
- Entwickeln eine Anwendung verwenden benutzerdefinierter Bilder, um den Datenträgerstatus in den Phasen Entwicklung speichern.
- Verfolgen die Kosten in Bezug auf Status. 
- Erstellen von Masse Test-Umgebungen für Qualität Assurance zu testen.
- Verwenden die Elemente und Formeln auf einfache Weise konfigurieren und eine Anwendung auf verschiedenen Umgebungen reproduzieren. 
- Verteilen von virtuellen Computern für Hackathons (Entwickler oder Test Zusammenarbeit), und klicken Sie dann einfach Aufheben der Bereitstellung, wenn das Ereignis endet. 

### <a name="how-am-i-billed-for-azure-devtest-labs"></a>Wie verwende ich Azure DevTest Labs in Rechnung gestellt? 
Azure DevTest Labs ist ein kostenlos-Dienst, was bedeutet, dass Labs erstellen und Konfigurieren von Richtlinien, Vorlagen und Elemente frei ist. Sie Zahlen nur für den Azure Ressourcen innerhalb Ihrer Labs, wie virtuellen Computern, Speicherkonten und virtuelle Netzwerke verwendet. Erfahren Sie sich für Weitere Informationen über die Kosten von Ressourcen Übung [Azure DevTest Labs Preise](https://azure.microsoft.com/pricing/details/devtest-lab/)an. 

### <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>Was sind die verschiedenen Sicherheitsstufen in Azure DevTest Kursen?  
Sicherheitszugriff wird durch [Azure Role-Based Access Steuerelement (RBAC)](../active-directory/role-based-access-built-in-roles.md)festgelegt. Zum Verständnis der Funktionsweise von Access können Sie die Unterschiede zwischen einer Berechtigungsstufe, einer Rolle und einen Bereich durch RBAC definierten kennen müssen.

- **Berechtigung** - einer Berechtigungsstufe ist eine definierten Zugriff auf eine bestimmte Aktion. Beispielsweise könnte eine Berechtigung Lese-Zugriff auf alle virtuellen Computern. 
- **Rolle** – eine Rolle ist eine Reihe von Berechtigungen, die gruppiert und einem Benutzer zugeordnet werden kann. Angenommen, hat ein Abonnementbesitzer"" Zugriff auf alle Ressourcen innerhalb eines Abonnements. 
- **Bereich** - Bereich ist eine Ebene in der Hierarchie von Azure Ressource an. Ein Bereich könnte beispielsweise eine Ressourcengruppe oder eine einzelne Übung oder das gesamte Abonnement. 
 
Innerhalb des Gültigkeitsbereichs von Azure DevTest Labs, es gibt zwei Arten von Rollen zum Definieren von Benutzerberechtigungen: Übung Besitzer und Übung Benutzer.

- **Übung Besitzer** - Besitzer einer Übung hat den Zugriff auf alle Ressourcen innerhalb der Übung. Daher können diese Richtlinien, lesen und Schreiben alle virtuellen Computern, ändern das virtuelle Netzwerk, und so weiter. 
- **Übung Benutzer** – Übung Benutzer kann alle Übung Ressourcen, wie z. B. virtuelle Computer, virtuelle Netzwerke und Richtlinien anzeigen, jedoch nicht Richtlinien oder alle virtuellen Computern, die von anderen Benutzern erstellte ändern. Es ist auch möglich, benutzerdefinierte Rollen in Azure DevTest Kursen erstellen, und erfahren Sie, wie im Artikel, [erteilen Benutzerberechtigungen für bestimmte Übung Richtlinien](devtest-lab-grant-user-permissions-to-specific-lab-policies.md)ausführen. 
 
Da Bereiche hierarchische, sind, wenn Sie einem Benutzer mit einem bestimmten Bereich zugewiesen wurden, werden sie automatisch diese Berechtigungen bei jeder Low-Level-Bereich umfasst erteilt. Z. B., wenn ein Benutzer die Rolle des Abonnements sind zugewiesen ist, müssen dann sie Zugriff auf alle Ressourcen in einem Abonnement. Diese Ressourcen umfassen alle virtuellen Computern, alle virtuellen Netzwerke und alle Übungseinheiten. Auf diese Weise erbt die Abonnementbesitzer einer automatisch die Rolle des Übung Besitzer. Die entgegengesetzt ist jedoch nicht richtig. Die Besitzer einer Übung hat Zugriff auf eine Übung, also einen unteren Bereich als Abonnement-Level. Daher ist nicht der Besitzer einer Übung sehen virtuellen Computern oder virtuelle Netzwerke oder alle Ressourcen, die sich außerhalb der Übung befinden. 

### <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Wie erstelle ich eine Rolle aus, damit Benutzer einen bestimmten Vorgang ausführen können?
Informationen zum Erstellen von benutzerdefinierter Rollen und Zuweisen von Berechtigungen, die diese Rolle ein umfassender Artikel finden Sie hier. Hier ist ein Beispiel für ein Skript, die Rolle "DevTest Labs erweiterte Benutzer", die verfügt über die Berechtigung zum Starten und Beenden alle virtuellen Computern in der Übung erstellt:
 
    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User" 
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*') 
    $policyRoleDef.Id = $null 
    $policyRoleDef.Name = "DevTest Labs Advance User" 
    $policyRoleDef.IsCustom = $true 
    $policyRoleDef.AssignableScopes.Clear() 
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action") 
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  
 
### <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>Lässt sich Azure DevTest Labs mit meinem CI/CD Toolchain integrieren? 
Wenn Sie VSTS verwenden, ist es eine [Erweiterung Azure DevTest Labs Aufgaben](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) , die Sie Ihre Version Verkaufspipeline in Azure DevTest Kursen automatisieren können. Einige Verwendungsmöglichkeiten dieser Erweiterung umfassen:

- Erstellen und Bereitstellen eines virtuellen Computers automatisch konfigurieren, dass er mit den neuesten Stand Azure Datei kopieren oder PowerShell VSTS Aufgaben verwenden. 
- Erfassen den Status eines virtuellen Computers automatisch nach dem Testen, um einen Fehler des gleichen virtuellen Computers zur weiteren Untersuchung zu reproduzieren. 
- Löschen den virtuellen Computer am Ende der Verkaufspipeline Release aus, wenn es nicht mehr benötigt wird. 

Die folgenden Blogbeiträge bieten Anleitung und Informationen zur Verwendung der VSTS-Erweiterung:
 
- [Azure DevTest Labs – Erweiterung VSTS](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/) 
- [Bereitstellen eines neuen virtuellen Computers in einer vorhandenen AzureDevTestLab von VSTS](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS) 
- [Mithilfe von VSTS Release Management für kontinuierliche Bereitstellungen zu AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs) 
 
Für andere Toolchains CI/CD können die zuvor beschriebenen Szenarien, die über die Erweiterung VSTS Aufgaben erzielt werden können auf ähnliche Weise durch Bereitstellen von [Vorlagen Azure Ressourcenmanager](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) mit [Azure PowerShell-Cmdlets](../resource-group-template-deploy.md) und [.NET-SDKs](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/)erzielt werden. [REST-APIs für DevTest Labs](http://aka.ms/dtlrestapis) können Sie auch mit der Toolchain integriert werden soll.  

### <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Warum werden angezeigt nicht bestimmte virtuellen Computern in das Blade Azure-virtuellen Computern, die ich in Azure DevTest Labs angezeigt werden?
Beim Erstellen ein virtuellen Computers in Azure DevTest Kursen ist die Berechtigung erteilt, um diesen virtuellen Computer zuzugreifen. Sie können sowohl in der Labs Blade- und das Blade **virtuellen Computern** anzeigen können. Für Benutzer in der Rolle DevTest Labs können alle in den Kurs durch die Übung des **allen virtuellen Computern** Blade erstellte Maschinen angezeigt. Jedoch sind Benutzer in der Rolle DevTest Labs nicht automatisch Lesezugriff auf virtuellen Computer Ressourcen gewährt, die andere erstellt haben. Diese virtuellen Computern sind daher nicht in das Blade **virtuellen Computern** angezeigt. 

### <a name="what-is-the-difference-between-custom-images-and-formulas"></a>Was ist der Unterschied zwischen benutzerdefinierten Bilder und Formeln? 
Ein benutzerdefiniertes Bild ist eine virtuelle Festplatte (virtuelle Festplatte), während eine Formel ein Bild sind, die Sie zusätzliche Einstellungen konfigurieren können, die Sie speichern und zu reproduzieren können. Ein benutzerdefiniertes Bild möglicherweise wünschenswert, wenn Sie schnell mehrere Umgebungen mit grundlegenden, unveränderliche dasselbe Bild erstellen möchten. Eine Formel möglicherweise besser, wenn Sie die Konfiguration von Ihrem virtuellen Computer mit der neuesten Bits, eine virtuelle Netzwerk-Subnetz oder eine bestimmte Größe zu reproduzieren möchten. Eine genauere Erklärung finden Sie im Artikel, [Vergleich benutzerdefinierte Bilder und Formeln in DevTest Einheiten](devtest-lab-comparing-vm-base-image-types.md). 
 
### <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Wie kann ich mehrere virtuelle Computer gleichzeitig aus derselben Vorlage erstellen? 
Die [VSTS Aufgaben Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) oder [eine Vorlage Azure Ressourcenmanager generieren](devtest-lab-add-vm-with-artifacts.md#save-arm-template) können beim Erstellen einer virtuellen Computer und [die Ressourcenmanager Azure-Vorlage von Windows PowerShell bereitstellen](../resource-group-template-deploy.md). 
 
### <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Wie verschiebe ich meine vorhandene Azure-virtuellen Computern in Meine Übung Azure DevTest Labs? 
Wir Entwerfen einer Lösung für virtuellen Computern direkt in Azure DevTest Labs zu verschieben, aber derzeit können Sie Ihre vorhandenen virtuellen Computern in kopieren Azure DevTest Labs wie folgt: 

1. Kopieren Sie die Datei virtuelle Festplatte von Ihrer vorhandenen virtuellen Computer verwenden dieses [Windows PowerShell-Skript](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1) 
1. [Erstellen das benutzerdefinierte Bild](devtest-lab-create-template.md) innerhalb Ihrer Azure DevTest Labs Übung aus. 
1. Erstellen eines virtuellen Computers in der Kurs aus Ihrer benutzerdefiniertes Bild 
 
### <a name="can-i-attach-multiple-disks-to-my-vms"></a>Kann mehrere Datenträger zu meiner virtuellen Computern werden angefügt? 
Hinzufügen mehrerer Festplatten zu virtuellen Computern wird unterstützt.  
 
### <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Wie automatisieren kann ich die Vorgehensweise zum Hochladen von Dateien von virtuellen Festplatte zum Erstellen von benutzerdefinierten Bilder? 
Es gibt zwei Optionen:

- [Azure AzCopy](../storage/storage-use-azcopy.md#blob-upload) kann verwendet werden, zu kopieren oder virtuelle Festplatte Dateien in der Übung zugeordnete Speicherplatz Konto hochladen.
- [Microsoft Azure-Speicher-Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) wird eine eigenständige Anwendung, die auf Windows, OSX und Linux ausgeführt wird.   
 
Gehen Sie folgendermaßen vor, um das Ziel-Speicher-Konto Ihrer Übung zugeordnet finden Sie:

1. Melden Sie sich mit dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)aus. 
1. Klicken Sie im linken Bereich **Ressourcengruppen** auswählen. 
1. Suchen Sie, und wählen Sie die Ressourcengruppe aus Ihrer Übung zugeordnet. 
1. Wählen Sie in der **Übersicht** Blade eines der Speicherkonten aus. 
1. Wählen Sie **Blobs**aus.
1. Suchen Sie nach Uploads in der Liste. Wenn keine vorhanden ist, kehren Sie zu Schritt 4 zurück, und versuchen Sie es ein anderes Speicherkonto.
1. Verwenden Sie die **URL** als Ihr Ziel in Ihrem AzCopy Befehl ein.


### <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Wie kann ich die Vorgehensweise zum Löschen aller virtuellen Computern in meinem Kurs automatisieren?

Neben dem Löschen virtuellen Computern aus Ihrem Übung Azure-Portal an, können Sie alle im virtuellen Computern in Ihrem Kurs mithilfe eines PowerShell-Skripts löschen. Im folgenden Beispiel einfach ändern Sie die gewünschten Parameterwerte unter den Kommentar **Werte zu ändern** . Sie können Abrufen der `subscriptionId`, `labResourceGroup`, und `labName` Werte von Übung vorher in der Azure-Portal. 


    # Delete all the VMs in a lab
    
    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount
    
    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    
    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    
    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object { 
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}
    
    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }




### <a name="what-are-artifacts"></a>Was sind die Elemente? 
Elemente sind anpassbare Elemente, die zum Bereitstellen der neuesten Bits oder Ihre Entwicklertools auf einen virtuellen Computer verwendet werden können. Während der Erstellung mit wenigen Mausklicks in Ihre virtuellen Computer angehängt sind, und nachdem Sie der virtuellen Computer bereitgestellt wird, die Elemente bereitstellen und Konfigurieren der virtuellen Computer. Es gibt verschiedene bereits vorhandenen Elemente in unseren [öffentlichen Github Repository](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), aber Sie können auch auf einfache Weise [Verfassen Sie Ihre eigenen Elemente](devtest-lab-artifact-author.md). 

### <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Wie erstelle ich eine Übung aus einer Vorlage Ressourcenmanager Azure? 
Wir haben ein [Github Repository Übung Azure Ressourcenmanager Vorlagen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) , die Sie als bereitstellen können Unterlagen-ist oder zum Erstellen von benutzerdefinierter Vorlagen für Ihre Labs ändern. Jede dieser Vorlagen enthält einen Link, den Sie klicken können, um die Übung als bereitstellen-liegt unter eigene Azure-Abonnement, oder Sie können die Vorlage und [Bereitstellen mithilfe der PowerShell oder Azure CLI](../resource-group-template-deploy.md)anpassen.
 
### <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Warum werden meine virtuellen Computern in anderen Ressourcengruppen mit beliebigen Namen werden erstellt? Kann ich umbenennen oder ändern diese Ressourcengruppen? 
Damit Azure DevTest Labs zum Verwalten der Benutzerberechtigungen und den Zugriff auf virtuellen Computern sind Ressourcengruppen auf diese Weise erstellt. Während Sie den virtuellen Computer mit der gewünschten Namen in eine andere Ressourcengruppe verschieben können, wird dieser Vorgang daher nicht empfohlen. Wir arbeiten zur Verbesserung der diese Erfahrung, um größere Flexibilität zu ermöglichen.   
 
### <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Wie viele Labs kann ich unter dem gleichen Abonnement erstellen? 
Es gibt keine bestimmte Beschränkung für die Anzahl der Labs, die pro Abonnement erstellt werden können. Jedoch sind die verwendeten Ressourcen pro Abonnement beschränkt. Sie können über die [Grenzwerte und Kontingente auferlegt Azure-Abonnements](../azure-subscription-service-limits.md) und [wie diese Grenzwerte vergrößern](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests)lesen. 
 
### <a name="how-many-vms-can-i-create-per-lab"></a>Wie viele virtuellen Computern kann ich pro Übung erstellen? 
Es gibt keine bestimmte Beschränkung für die Anzahl der virtuellen Computern, die pro Übung erstellt werden können. Jedoch unterstützt die Übung aktuell nur etwa 40 virtuellen Computern gleichzeitig in standard-Speicher ausgeführt und 25 virtuellen Computern, die gleichzeitig in Premium Speicher ausgeführt werden. Wir arbeiten derzeit auf diese Grenzwerte zu erhöhen. 

### <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Wie freigeben kann ich mit meinem Kurs einen direkten Link?

Wenn Sie einen direkten Link zu Ihrer Übung Benutzer freigeben möchten, können Sie das folgende Verfahren ausführen.

1. Navigieren Sie zu der Übung Azure-Portal an.
2. Kopieren Sie die Übung URL im Browser, und teilen Sie sie für Ihre Benutzer Übung. 

>[AZURE.NOTE] Wenn Ihre Benutzer Übung externe Benutzer mit einem [Konto MSA sind](#what-is-a-microsoft-account) , und sie nicht in Active Directory Ihres Unternehmens gehören, können sie eine Fehlermeldung beim Navigieren zu den bereitgestellten Link. Wenn sie eine Fehlermeldung erhalten, weisen sie an, auf deren Namen in der oberen rechten Ecke des Portals Azure, und wählen das Verzeichnis, in dem die Übung aus dem **Verzeichnis** Abschnitt des Menüs vorhanden ist.

### <a name="what-is-a-microsoft-account"></a>Was ist ein Microsoft-Konto?

Ein Microsoft-Konto ist, was Sie für nahezu jedes Element verwenden, die Sie mit Microsoft-Geräte und Dienste ausführen. Es ist eine e-Mail-Adresse und das Kennwort für die Anmeldung bei Skype, Outlook.com, OneDrive, Windows Phone und Xbox LIVE – und dies bedeutet, dass Sie Dateien, Fotos, Kontakten und Einstellungen zu einem beliebigen Gerät folgen können. 

>[AZURE.NOTE] Microsoft-Konto verwendet, um "Windows Live ID" bezeichnet werden.
 
### <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Fehler bei meinem Element während der Erstellung virtueller Computer Wie behebe ich sie? 
Finden Sie im Blogbeitrag [zum Behandeln von Problemen mit weiß nicht Elemente in AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs) - mit einem der unsere MVPs geschrieben - Informationen zum Protokolle hinsichtlich der Fehler beim Element zu erhalten. 
 
### <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Warum Netzwerk ist nicht meine vorhandene virtuelle ordnungsgemäß speichern?  
Eine Möglichkeit ist, dass Ihre virtuellen Netzwerknamen Punkte enthält. Wenn dies der Fall ist, versuchen Sie die Perioden entfernen oder durch Bindestriche ersetzt werden, und versuchen Sie erneut, das virtuelle Netzwerk zu speichern.
