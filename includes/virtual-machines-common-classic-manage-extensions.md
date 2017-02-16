


##<a name="using-vm-extensions"></a>Verwenden von virtuellen Computer Erweiterungen

Azure-virtuellen Computer-Erweiterungen implementieren Verhalten oder Features, mit denen eine andere Programme auf Azure-virtuellen Computern arbeiten (beispielsweise die Erweiterung **WebDeployForVSDevTest** ermöglicht Visual Studio zu Web Bereitstellen von Lösungen Ihrer Azure-virtuellen Computers), oder geben Sie die Möglichkeit für die Interaktion mit dem virtuellen Computer, einige andere Verhalten unterstützen (z. B. können die Zugriff virtueller Computer Erweiterungen aus Azure CLI PowerShell und REST-Clients zurücksetzen oder RAS-Werte der Azure-virtuellen Computers ändern).

>[AZURE.IMPORTANT] Eine vollständige Liste der von den Features Erweiterungen diese unterstützen, finden Sie unter [Azure-virtuellen Computer-Erweiterungen und Features](../articles/virtual-machines/virtual-machines-windows-extensions-features.md). Da jede Erweiterung virtueller Computer ein bestimmtes Feature unterstützt, hängt genau wie können und kann nicht mit der Erweiterung von der Erweiterung. Stellen Sie vor dem Ändern der virtuellen Computer, daher sicher, dass Sie die Dokumentation für die Erweiterung virtueller Computer gelesen haben, die Sie verwenden möchten. Entfernen einige Erweiterungen virtueller Computer wird nicht unterstützt. andere verfügen über gemeinsame Eigenschaften, die festgelegt werden können, die virtuellen Computer Verhalten grundlegend ändern.

Am häufigsten ausgeführten Aufgaben sind:

1.  Verfügbare Erweiterungen suchen

2.  Aktualisieren von Erweiterungen geladen

3.  Hinzufügen von Erweiterungen

4.  Entfernen von Erweiterungen

##<a name="find-available-extensions"></a>Suchen nach verfügbaren Erweiterungen

Suchen Sie nach Erweiterung und erweiterte Informationen zu verwenden:

-   PowerShell
-   Azure Plattformen Befehlszeile Interface (Azure CLI)
-   Servicemanagement REST-API

###<a name="azure-powershell"></a>Azure PowerShell

Einige Erweiterungen haben PowerShell-Cmdlets, die speziell darauf, die wodurch die Konfiguration von PowerShell einfacher sind möglicherweise; die folgenden Cmdlets für alle virtuellen Computer Erweiterungen funktioniert jedoch.

Die folgenden Cmdlets können Sie um Informationen über verfügbare Erweiterungen zu erhalten:

-   Für Instanzen von Webrollen oder Worker-Rollen, können Sie das Cmdlet " [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) ".
-   Für Instanzen von virtuellen Computern, können Sie das Cmdlet " [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) ".

     Im folgenden Code wird zeigt beispielsweise, wie die Informationen für die **IaaSDiagnostics** Erweiterung mithilfe der PowerShell aufgelistet.

        PS C:\> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics

        Publisher                   : Microsoft.Azure.Diagnostics
        ExtensionName               : IaaSDiagnostics
        Version                     : 1.2
        Label                       : Microsoft Monitoring Agent Diagnostics
        Description                 : Microsoft Monitoring Agent Extension
        PublicConfigurationSchema   :
        PrivateConfigurationSchema  :
        IsInternalExtension         : False
        SampleConfig                :
        ReplicationCompleted        : True
        Eula                        :
        PrivacyUri                  :
        HomepageUri                 :
        IsJsonExtension             : True
        DisallowMajorVersionUpgrade : False
        SupportedOS                 :
        PublishedDate               :
        CompanyName                 :


###<a name="azure-command-line-interface-azure-cli"></a>Azure Befehlszeile Interface (Azure CLI)

Einige Erweiterungen haben Azure CLI-Befehle, die zu spezifisch sind (die Erweiterung Docker virtueller Computer ist ein Beispiel für), die möglicherweise ihre Konfiguration erleichtern; die folgenden Befehle für alle virtuellen Computer Erweiterungen funktioniert jedoch.

Sie können den **Azure-virtuellen Computer Erweiterungsliste** Befehl verwenden, beziehen Informationen zu den verfügbaren Extensions, und Verwenden der **–-Json** Option, um alle verfügbaren Informationen über eine oder mehrere Erweiterungen anzuzeigen. Wenn Sie einen Erweiterungsnamen nicht verwenden, gibt der Befehl eine JSON-Beschreibung der alle verfügbaren Erweiterungen an.

Beispielsweise den folgenden Code wird gezeigt, wie die Informationen für die **IaaSDiagnostics** Erweiterung des Befehls Azure CLI **Azure-virtuellen Computer Erweiterungsliste** Liste und verwendet die **–-Json** Option vollständige Informationen aus.


    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



###<a name="service-management-rest-apis"></a>Servicemanagement REST-APIs

Die folgenden REST-APIs können Sie um Informationen über verfügbare Erweiterungen zu erhalten:

-   Für Instanzen von Webrollen oder Worker-Rollen, können Sie den [Liste Verfügbare Erweiterungen](https://msdn.microsoft.com/library/dn169559.aspx) Vorgang. Um die Liste der verfügbaren Erweiterungen-Versionen können Sie die [Liste Extension-Versionen](https://msdn.microsoft.com/library/dn495437.aspx)verwenden.

-   Für Instanzen von virtuellen Computern, können Sie den [Liste Ressource Erweiterungen](https://msdn.microsoft.com/library/dn495441.aspx) Vorgang. Um die Liste der verfügbaren Erweiterungen-Versionen können Sie die [Liste Ressource Extension-Versionen](https://msdn.microsoft.com/library/dn495440.aspx)verwenden.

##<a name="add-update-or-disable-extensions"></a>Hinzufügen, aktualisieren oder Deaktivieren von Erweiterungen

Extensions können hinzugefügt werden, wenn eine Instanz erstellt oder diese zu einer laufenden Instanz hinzugefügt werden können. Extensions können aktualisiert, deaktiviert oder entfernt werden. Sie können diese Aktionen mithilfe von Azure PowerShell-Cmdlets oder mithilfe der Service Management REST-API Vorgänge ausführen. Parameter sind für das Installieren und Einrichten von einigen Erweiterungen erforderlich. Öffentliche und private Parameter werden für Erweiterungen unterstützt.


###<a name="azure-powershell"></a>Azure PowerShell

Azure-PowerShell-Cmdlets verwenden, ist die einfachste Möglichkeit zum Hinzufügen und Aktualisieren von Erweiterungen. Wenn Sie die Erweiterung Cmdlets verwenden, wird der größte Teil der Konfiguration der Erweiterung für Sie erledigt. Manchmal müssen Sie eine Erweiterung programmgesteuert hinzuzufügen. Wenn Sie dies tun müssen, müssen Sie die Konfiguration der Erweiterung bereitstellen.

Die folgenden Cmdlets können Sie wissen, ob eine Erweiterung eine Konfiguration der öffentlichen und privaten Parameter erfordert:

-   Für Instanzen von Webrollen oder Worker-Rollen, können Sie das Cmdlet " **Get-AzureServiceAvailableExtension** ".

-   Für Instanzen von virtuellen Computern, können Sie das Cmdlet " **Get-AzureVMAvailableExtension** ".

###<a name="service-management-rest-apis"></a>Servicemanagement REST APIs

Wenn Sie eine Liste der verfügbaren Erweiterungen mithilfe der REST-APIs abrufen, erhalten Sie Informationen wie die Erweiterung ist so konfiguriert werden. Die Informationen, die zurückgegeben wird, möglicherweise Parameterinformationen dargestellt durch eine Schema öffentlichen und privaten Schema angezeigt. Öffentliche Parameterwerte werden in Abfragen über die Instanzen zurückgegeben. Private Parameterwerte werden nicht zurückgegeben.

Die folgenden REST-APIs können Sie wissen, ob eine Erweiterung eine Konfiguration der öffentlichen und privaten Parameter erfordert:

-   Instanzen von Webrollen oder Worker-Rollen, die **PublicConfigurationSchema** und **PrivateConfigurationSchema** Elemente enthalten Sie, die Informationen in der Antwort aus der [Liste Verfügbare Erweiterungen](https://msdn.microsoft.com/library/dn169559.aspx) Vorgang.

-   Für Instanzen von virtuellen Computern, die **PublicConfigurationSchema** und **PrivateConfigurationSchema** Elemente, die Informationen in der Antwort aus der [Liste Ressource Erweiterungen](https://msdn.microsoft.com/library/dn495441.aspx) Vorgang enthalten.

>[AZURE.NOTE]Extensions können auch Konfigurationen, die definiert sind mit JSON. Wenn diese Typen von Erweiterungen verwendet werden, wird nur für das Element **SampleConfig** verwendet.
