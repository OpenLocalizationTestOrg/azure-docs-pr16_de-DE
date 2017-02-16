### <a name="tag-cmdlet-changes-in-latest-powershell-version"></a>Kategorie Cmdlet Änderungen in der neuesten Version von PowerShell

Der August 2016 Version von [Azure PowerShell 2.0] [ powershell] wesentlichen Änderungen in die Arbeit mit Kategorien enthält. Überprüfen Sie bevor Sie fortfahren die Version des Moduls AzureRm.Resources aus.

    Get-Module -ListAvailable -Name AzureRm.Resources | Select Version

Wenn Sie Ihre Azure PowerShell vor August 2016 zuletzt aktualisiert, sollte die Ergebnisse einer Version kleiner als 3.0 anzeigen.

    Version
    -------
    2.0.2

Wenn Sie seit August 2016 Azure PowerShell aktualisiert haben, sollte die Ergebnisse eine Version 3.0 anzeigen.

    Version
    -------
    3.0.1
    
Wenn Ihre Version des Moduls 3.0.1 oder höher Sie die letzte Cmdlets haben für die Arbeit mit Kategorien. Diese Version des Moduls Azure Ressourcen wird automatisch installiert, beim Installieren oder Aktualisieren von Azure PowerShell mithilfe der PowerShell-Katalog, PowerShellGet oder Web Platform Installer.  Wenn Ihre Version früher als 3.0.1 ist, können Sie mit dieser Version weiterhin, aber möglicherweise möchten Sie auf die neueste Version aktualisieren. Änderungen, die für die Arbeit mit Kategorien erleichtern umfasst die neueste Version. Beide Methoden werden in diesem Thema dargestellt.

### <a name="updating-your-script-for-changes-in-latest-version"></a>Aktualisieren das Skript auf Änderungen in der neuesten version 

In der neuesten Version der Parametername **Kategorien** in **Kategorie**, und der Typ geändert von **Hashtable []** zu **Hashtable**. Sie müssen nicht mehr für jeden Eintrag **Namen** und einen **Wert** angeben. Stattdessen bieten Sie Schlüssel / Wert-Paare im Format **Schlüssel = "Wert"**.

Aktualisieren Sie vorhandenes Skript, ändern Sie den Parameter **Kategorien** **Kategorie**, und ändern das Tag-Format, wie im folgenden Beispiel gezeigt.

    # Old
    New-AzureRmResourceGroup -Tags @{ Name = "testtag"; Value = "testval" } -Name $resourceGroupName -Location $location

    # New
    New-AzureRmResourceGroup -Tag @{ testtag = "testval" } -Name $resourceGroupName -Location $location 

Jedoch, beachten Sie, dass die Ressourcengruppen und Ressourcen weiterhin eine Eigenschaft **Kategorien** in ihren Metadaten zurückzukehren. Diese Eigenschaft wird nicht geändert.

### <a name="version-301-or-later"></a>Version 3.0.1 oder höher

Kategorien, die direkt auf Ressourcen und Ressourcengruppe vorhanden sein. Zeigen Sie die vorhandenen Tags finden Sie eine Ressource mit **Get-AzureRmResource** oder eine Ressourcengruppe mit **Get-AzureRmResourceGroup**aus. 

Beginnen wir mit einer Ressourcengruppe.

    Get-AzureRmResourceGroup -Name testrg1

Dieses Cmdlet gibt mehrere Teile der Metadaten für die Ressourcengruppe einschließlich welche Kategorien angewendet wurden, falls vorhanden.

    ResourceGroupName : testrg1
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
                    Name         Value
                    ===========  ==========
                    Dept         Finance
                    Environment  Production

Verwenden Sie zum Abrufen der Ressourcenmetadaten einschließlich Kategorien im folgenden Beispiel wird ein.

    Get-AzureRmResource -ResourceName tfsqlserver -ResourceGroupName testrg1

Sie sehen die Tagnamen in den Suchergebnissen aus.

    Name              : tfsqlserver
    ResourceId        : /subscriptions/{guid}/resourceGroups/tag-demo-group/providers/Microsoft.Sql/servers/tfsqlserver
    ResourceName      : tfsqlserver
    ResourceType      : Microsoft.Sql/servers
    Kind              : v12.0
    ResourceGroupName : testrg1
    Location          : westus
    SubscriptionId    : {guid}
    Tags              : {Dept, Environment}

Verwenden Sie **die Eigenschaft** zum Abrufen Tagnamen und Werte ein.

    (Get-AzureRmResource -ResourceName tfsqlserver -ResourceGroupName testrg1).Tags

Die folgende Ergebnisse zurückgegeben:

    Name                   Value
    ----                   -----
    Dept                   Finance
    Environment            Production

Statt Anzeigen der Kategorien für eine Ressource oder eine bestimmte Ressourcengruppe, möchten Sie oft alle Ressourcen oder Ressourcengruppen mit einem bestimmten Tag und Wert abrufen. Ressourcengruppen mit einer bestimmten Kategorie verwenden, um **Suchen-AzureRmResourceGroup** -Cmdlet mit dem **-Tag** Parameter.

Um Ressourcengruppen mit einem Tagwert abrufen möchten, verwenden Sie das folgende Format ein.

    (Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 

Verwenden Sie alle Ressourcen, die mit einem bestimmten Tag und Wert, um das Cmdlet **AzureRmResource suchen** .

    (Find-AzureRmResource -TagName Dept -TagValue Finance).Name
    
Um eine Kategorie zu einer Ressourcengruppe hinzufügen möchten, die keine vorhandenen Kategorien enthält, verwenden Sie den Befehl **Set-AzureRmResourceGroup** , und geben Sie ein Tag-Objekt.

    Set-AzureRmResourceGroup -Name test-group -Tag @{ Dept="IT"; Environment="Test" }

Die Ressourcengruppe zusammen mit den neuen Kategorie Werten zurückgibt.

    ResourceGroupName : test-group
    Location          : southcentralus
    ProvisioningState : Succeeded
    Tags              :
                    Name          Value
                    =======       =====
                    Dept          IT
                    Environment   Test
                    
Sie können Kategorien an eine Ressource hinzufügen, die keine vorhandene Tags mithilfe des Befehls **Set-AzureRmResource** enthält 

    Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceId /subscriptions/{guid}/resourceGroups/test-group/providers/Microsoft.Web/sites/examplemobileapp

Kategorien werden als Ganzes aktualisiert. Um eine Kategorie an eine Ressource hinzufügen, die anderen Kategorien enthält, verwenden Sie ein Array mit den Tags, die Sie behalten möchten. Zunächst wählen Sie die vorhandenen Kategorien aus, fügen Sie zu dieser Gruppe hinzu und erneutes Anwenden Sie alle Tags.

    $tags = (Get-AzureRmResourceGroup -Name tag-demo).Tags
    $tags += @{Status="approved"}
    Set-AzureRmResourceGroup -Name test-group -Tag $tags

Wenn Sie eine oder mehrere Kategorien entfernen möchten, speichern Sie einfach die Matrix ohne diejenigen, die Sie entfernen möchten.

Der Vorgang ist mit der für Ressourcen außer Sie **Get-AzureRmResource** und **Set-AzureRmResource** -Cmdlets verwenden. 

Um eine Liste aller Kategorien in einem Abonnement mithilfe der PowerShell erhalten möchten, verwenden Sie das Cmdlet " **Get-AzureRmTag** " ein.

    Get-AzureRmTag
    
Die Namen der Kategorie und der Anzahl von Ressourcen und Ressourcengruppe mit dem Tag zurückgibt.

    Name                      Count
    ----                      ------
    Dept                       8
    Environment                8

Sehen Sie möglicherweise Kategorien, die mit "Ausgeblendete-" beginnen und "Link:". Diese Tags sind internen Kategorien, die Sie ignorieren und vermeiden sollten ändern.

Verwenden Sie das Cmdlet " **New-AzureRmTag** " neue Kategorien zur Taxonomie hinzufügen. Diese Tags beinhaltet das AutoVervollständigen, obwohl er noch auf alle Ressourcen oder Ressourcengruppen angewendet wurde nicht geschehen ist. Zum Entfernen eines Kategorie Name/Werts entfernen Sie zuerst die Kategorie von Ressourcen es möglicherweise mit verwendet werden, und verwenden Sie dann das Cmdlet **AzureRmTag entfernen** aus der Taxonomie entfernen.

### <a name="versions-earlier-than-301"></a>-Versionen vor 3.0.1

Kategorien, die direkt auf Ressourcen und Ressourcengruppe vorhanden sein. Zeigen Sie die vorhandenen Tags finden Sie eine Ressource mit **Get-AzureRmResource** oder eine Ressourcengruppe mit **Get-AzureRmResourceGroup**aus. 

Beginnen wir mit einer Ressourcengruppe.

    Get-AzureRmResourceGroup -Name testrg1

Dieses Cmdlet gibt mehrere Teile der Metadaten für die Ressourcengruppe, welche Tags angewendet wurden, einschließlich, falls vorhanden.

    ResourceGroupName : testrg1
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
                    Name         Value
                    ===========  ==========
                    Dept         Finance
                    Environment  Production
                    
Verwenden Sie zum Abrufen der Ressourcenmetadaten im folgenden Beispiel wird ein. Die Ressourcenmetadaten zeigt die Kategorien nicht direkt an. 

    Get-AzureRmResource -ResourceName tfsqlserver -ResourceGroupName testrg1

Sie sehen die Ergebnisse, dass die Tags nur als Hashtable-Objekt angezeigt werden.

    Name              : tfsqlserver
    ResourceId        : /subscriptions/{guid}/resourceGroups/tag-demo-group/providers/Microsoft.Sql/servers/tfsqlserver
    ResourceName      : tfsqlserver
    ResourceType      : Microsoft.Sql/servers
    Kind              : v12.0
    ResourceGroupName : tag-demo-group
    Location          : westus
    SubscriptionId    : {guid}
    Tags              : {System.Collections.Hashtable}

Sie können die ist-Tags anzeigen, indem Sie **die Eigenschaft** .

    (Get-AzureRmResource -ResourceName tfsqlserver -ResourceGroupName tag-demo-group).Tags | %{ $_.Name + ": " + $_.Value }
   
Die gibt formatierten Ergebnisse aus:
    
    Dept: Finance
    Environment: Production
    
Statt Anzeigen der Kategorien für eine Ressource oder eine bestimmte Ressourcengruppe, möchten Sie oft alle Ressourcen oder Ressourcengruppen mit einem bestimmten Tag und Wert abrufen. Ressourcengruppen mit einer bestimmten Kategorie verwenden, um **Suchen-AzureRmResourceGroup** -Cmdlet mit dem **-Tag** Parameter.

Um Ressourcengruppen mit einem Tagwert abrufen möchten, verwenden Sie das folgende Format ein.

    Find-AzureRmResourceGroup -Tag @{ Name="Dept"; Value="Finance" } | %{ $_.Name }
    
Verwenden Sie alle Ressourcen, die mit einem bestimmten Tag und Wert, um das Cmdlet AzureRmResource suchen.

    Find-AzureRmResource -TagName Dept -TagValue Finance | %{ $_.ResourceName }

Um eine Kategorie zu einer Ressourcengruppe hinzufügen möchten, die keine vorhandenen Kategorien enthält, verwenden Sie den Befehl Set-AzureRmResourceGroup einfach, und geben Sie ein Tag-Objekt.

    Set-AzureRmResourceGroup -Name test-group -Tag @( @{ Name="Dept"; Value="IT" }, @{ Name="Environment"; Value="Test"} )
    
Die Ressourcengruppe zusammen mit den neuen Kategorie Werten zurückgibt.

    ResourceGroupName : test-group
    Location          : southcentralus
    ProvisioningState : Succeeded
    Tags              :
                Name          Value
                =======       =====
                Dept          IT
                Environment   Test

Sie können Kategorien an eine Ressource hinzufügen, die keine vorhandene Tags mithilfe des Befehls Set-AzureRmResource enthält.

    Set-AzureRmResource -Tag @( @{ Name="Dept"; Value="IT" }, @{ Name="Environment"; Value="Test"} ) -ResourceId /subscriptions/{guid}/resourceGroups/test-group/providers/Microsoft.Web/sites/examplemobileapp

Kategorien werden als Ganzes aktualisiert. Um eine Kategorie zu einer Ressource hinzufügen, die andere Tags enthält, verwenden Sie ein Array mit den Tags, die Sie behalten möchten. Zunächst wählen Sie die vorhandenen Kategorien, fügen Sie zu dieser Gruppe hinzu und erneutes Anwenden Sie alle Tags.

    $tags = (Get-AzureRmResourceGroup -Name tag-demo).Tags
    $tags += @{Name="status";Value="approved"}
    Set-AzureRmResourceGroup -Name test-group -Tag $tags

Wenn Sie eine oder mehrere Kategorien entfernen möchten, speichern Sie einfach die Matrix ohne diejenigen, die Sie entfernen möchten.

Der Vorgang ist mit der für Ressourcen außer Sie Get-AzureRmResource und Set-AzureRmResource-Cmdlets verwenden. 

Um eine Liste aller Kategorien innerhalb eines Abonnements mithilfe der PowerShell erhalten möchten, verwenden Sie das Cmdlet " **Get-AzureRmTag** " ein.

    Get-AzureRmTag
    
Die Namen der Kategorie und der Anzahl von Ressourcen und Ressourcengruppe mit dem Tag zurückgibt.

    Name                      Count
    ----                      ------
    Dept                       8
    Environment                8

Sehen Sie möglicherweise Kategorien, die mit "Ausgeblendete-" beginnen und "Link:". Diese Tags sind internen Kategorien, die Sie ignorieren und vermeiden sollten ändern.

Verwenden Sie das Cmdlet " **New-AzureRmTag** " zum Hinzufügen von neuer Kategorien zur Taxonomie aus. Diese Tags beinhaltet das AutoVervollständigen, obwohl er noch auf alle Ressourcen oder Ressourcengruppen angewendet wurde nicht geschehen ist. Zum Entfernen eines Kategorie Name/Werts entfernen Sie zuerst die Kategorie von Ressourcen es herangezogen werden, und verwenden Sie das Cmdlet **Entfernen-AzureRmTag** zum Entfernen aus der Taxonomie ein.


[powershell]: https://msdn.microsoft.com/library/mt619274(v=azure.200).aspx
