<properties 
    pageTitle="Starten und Beenden von virtuellen Computern mit Azure Automatisierung - PowerShell Workflow | Microsoft Azure"
    description="Grafische Version von Azure Automatisierung Szenario einschließlich Runbooks zum Starten und Beenden von klassischen virtuellen Computern."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="07/06/2016"
    ms.author="bwren" />

# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure Automatisierung Szenario - starten und Beenden von virtuellen Computern

Dieses Szenario Azure Automatisierung enthält Runbooks zum Starten und Beenden von klassischen virtuellen Computern an.  Sie können dieses Szenario für eine der folgenden Aktionen aus:  

- Verwenden Sie die Runbooks ohne Änderung in Ihrer eigenen Umgebung. 
- Ändern der Runbooks, um benutzerdefinierte Funktionen ausführen.  
- Rufen Sie die Runbooks aus einem anderen Runbooks als Teil eines gesamten Lösung. 
- Anhand der Runbooks als Lernprogramme um Runbooks authoring Konzepte zu erfahren. 

> [AZURE.SELECTOR]
- [Grafische](automation-solution-startstopvm-graphical.md)
- [PowerShell-Workflow](automation-solution-startstopvm-psworkflow.md)

Dies ist der PowerShell-Workflow Runbooks Version dieses Szenario. Es ist auch mithilfe von [grafisch Runbooks](automation-solution-startstopvm-graphical.md)zur Verfügung.

## <a name="getting-the-scenario"></a>Erste das Szenario

Dieses Szenario besteht aus zwei PowerShell Workflow Runbooks, die von den folgenden Links heruntergeladen werden kann.  Anzeigen der [grafischen Version](automation-solution-startstopvm-graphical.md) dieses Szenario für Links zu den grafisch Runbooks an.

| Runbooks | Link | Typ | Beschreibung |
|:---|:---|:---|:---|
| Start-AzureVMs | [Starten Sie Azure klassische virtuellen Computern](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) | PowerShell-Workflow | Startet alle klassischen virtuellen Computern in einer Azure Subscriptionor alle virtuellen Computer mit einem bestimmten Dienstnamen. |
| Beenden-AzureVMs | [Beenden der Azure klassische virtuellen Computern](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) | PowerShell-Workflow | Beendet alle virtuellen Computer in ein Konto Automatisierung oder alle virtuellen Computer mit einem bestimmten Dienstnamen.  |


## <a name="installing-and-configuring-the-scenario"></a>Installieren und Konfigurieren des Szenarios

### <a name="1-install-the-runbooks"></a>1: Installieren Sie die runbooks

Nach dem Herunterladen der Runbooks, können Sie diese mit dem Verfahren importieren [einer Runbooks](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook)importieren.

### <a name="2-review-the-description-and-requirements"></a>2. Überprüfen Sie die Beschreibung und Anforderungen
Die Runbooks einbeziehen von kommentierten Hilfetext, die eine Beschreibung und erforderlichen Ressourcen enthält.  Sie können auch die gleiche Informationen aus diesem Artikel abrufen. 

### <a name="3-configure-assets"></a>3 Konfigurieren von Anlagen
Die Runbooks erfordern die folgenden Ressourcen, die Sie erstellen und mit den entsprechenden Werten füllen müssen.

| Objekt-Datentyp | Name der Anlage | Beschreibung |
|:---|:---|:---|:---|
| Anmeldeinformationen | AzureCredential | Enthält die Anmeldeinformationen für ein Konto, das verfügt über die Berechtigung zum Starten und Beenden von virtuellen Computern im Azure-Abonnement.  Alternativ können Sie eine andere Anmeldeinformationen Anlage in den **Credential** -Parameter der **Hinzufügen-AzureAccount** Aktivität angeben. |
| Variable | AzureSubscriptionId | Enthält die Abonnement-ID Ihres Abonnements Azure. |

## <a name="using-the-scenario"></a>Verwenden des Szenarios

### <a name="parameters"></a>Parameter

Die Runbooks haben die folgenden Parameter.  Sie müssen Werte für alle erforderlichen Parameter angeben und Werte für andere Parameter je nach Ihren Anforderungen können optional bereitstellen.

| Parameter | Typ | Obligatorisch | Beschreibung |
|:---|:---|:---|:---|
| ServiceName | Zeichenfolge | Nein | Wenn ein Wert angegeben wird, werden alle virtuellen Computer mit dem Namen Service gestartet oder beendet.  Wenn kein Wert angegeben wird, werden alle klassischen virtuellen Computern im Azure-Abonnement gestartet oder beendet. |
| AzureSubscriptionIdAssetName | Zeichenfolge | Nein | Enthält den Namen der [Variable Anlage](#installing-and-configuring-the-scenario) , die die Abonnement-ID Ihres Abonnements Azure enthält.  Wenn Sie keinen Wert angeben, wird *AzureSubscriptionId* verwendet.  |
| AzureCredentialAssetName | Zeichenfolge | Nein | Enthält den Namen der [Anlage Anmeldeinformationen](#installing-and-configuring-the-scenario) , die die Anmeldeinformationen für die Runbooks mit enthält.  Wenn Sie keinen Wert angeben, wird *AzureCredential* verwendet.  |

### <a name="starting-the-runbooks"></a>Starten der runbooks

Eines der Verfahren können in [einer Runbooks in Azure Automatisierung beginnen](automation-starting-a-runbook.md) , um entweder von der Runbooks in diesem Szenario zu starten.

Im folgenden Beispielbefehle mithilfe Windows PowerShell ausgeführt **StartAzureVMs** , um alle virtuellen Computer mit dem Dienstnamen *MyVMService*zu starten.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Die Ausgabe

Die Runbooks wird [eine Nachricht ausgeben](automation-runbook-output-and-messages.md) für jedes virtuellen Computern, die angibt, und zwar unabhängig davon, ob die Start- oder beenden-Anweisung erfolgreich übermittelt wurde.  Sie können nach einer bestimmten Zeichenfolge in der Ausgabe das Ergebnis für jede Runbooks bestimmen aussehen.  In der folgenden Tabelle sind die möglichen Ausgabezeichenfolgen aufgeführt.

| Runbooks | Bedingung | Nachricht |
|:---|:---|:---|
| Start-AzureVMs | Virtuellen Computern wird bereits ausgeführt.  | "MyVM" wird bereits ausgeführt. |
| Start-AzureVMs | Starten Sie Anfrage für virtuellen Computern erfolgreich übermittelten | "MyVM" wurde gestartet |
| Start-AzureVMs | Fehler bei Start Anforderung virtuellen Computern  | "MyVM" konnte nicht gestartet werden |
| Beenden-AzureVMs | Virtuellen Computern wurde bereits beendet.  | "MyVM" wurde bereits beendet. |
| Beenden-AzureVMs | Beenden der Anfrage für virtuellen Computern erfolgreich abgesendet | "MyVM" wurde beendet |
| Beenden-AzureVMs | Fehler bei Beenden Anforderung virtuellen Computern  | Fehler beim Beenden "MyVM" |

Beispielsweise versucht der folgenden Codeausschnitt aus einer Runbooks alle virtuellen Computer mit dem Dienstnamen *MyServiceName*zu starten.  Wenn keines der Start-Anfragen Fail können Fehler Aktionen absolviert werden. 

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a>Ausführliche Projektstrukturplan-Codes

Es folgt eine ausführliche Analyse der der Runbooks in diesem Szenario.  Diese Informationen können Sie entweder die Runbooks anpassen oder um nur um für die Erstellung Ihrer eigenen Szenarios für die Automatisierung daraus zu lernen.

### <a name="parameters"></a>Parameter

    param (
        [Parameter(Mandatory=$false)] 
        [String]  $AzureCredentialAssetName = 'AzureCredential',
        
        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)] 
        [String] $ServiceName
    )

Der Workflow wird gestartet, indem Sie die Werte für die [Eingabeparameter](#using-the-scenario).  Wenn Sie die Elementnamen nicht zur Verfügung stehen werden Standardnamen verwendet.

### <a name="output"></a>Die Ausgabe

    # Returns strings with status messages
    [OutputType([String])]

Diese Zeile deklariert, dass die Ausgabe des Runbooks eine Zeichenfolge ist.  Dies ist nicht erforderlich, aber wenn des Runbooks als eine [untergeordnete Runbooks](automation-child-runbooks.md) verwendet wird, sodass ein übergeordneten Runbooks den Typ der zu erwarten weiß, ist am besten für.

### <a name="authentication"></a>Authentifizierung

    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

Legen Sie die nächsten Zeilen [Anmeldeinformationen](automation-configuring.md#configuring-authentication-to-azure-resources) und Azure-Abonnement, die für die restlichen des Runbooks verwendet wird.
Verwenden Sie zuerst **Get-AutomationPSCredential** können Sie um die Anlage zu gelangen, die die Anmeldeinformationen Zugriff auf Starten und Beenden von virtuellen Computern im Azure-Abonnement enthält. **Hinzufügen-AzureAccount** verwendet diese Anlage, um die Anmeldeinformationen festlegen.  Die Ausgabe wird eine-platzhalterprodukt Variable zugewiesen, damit es in der Ausgabe Runbooks enthalten ist.  

Die Variable Anlage mit dem Abonnement-ID abgerufen dann mit **Get-AutomationVariable** und das Abonnement mit **Select-AzureSubscription**festlegen.

### <a name="get-vms"></a>Abrufen von virtuellen Computern

    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName) 
    { 
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else 
    { 
        $VMs = Get-AzureVM
    }

**Get-AzureVM** dient zum Abrufen von den virtuellen Computern, die, denen mit der Runbooks eingesetzt werden kann.  Wenn der Wert in der **ServiceName** Eingabewerte Variablen bereitgestellt wird, werden nur die virtuellen Computer mit dem Namen Service dann abgerufen werden.  Wenn **ServiceName** leer ist, werden dann alle virtuellen Computern abgerufen.

### <a name="startstop-virtual-machines-and-send-output"></a>Start/Stopp virtuellen Computern und senden Ausgabe

    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

Im nächsten Schritt von Linien durch jeden virtuellen Computer.  Der **Parameter** des virtuellen Computers ist zunächst überprüft, ob es bereits ausgeführt wird oder angehalten, je nach des Runbooks.  Wenn sie bereits im Ziel Zustand ist, wird eine Nachricht Ausgabe und der Enden Runbooks gesendet.  Wenn dies nicht der Fall ist, klicken Sie dann **- AzureVM beginnen** oder **Beenden-AzureVM** wird verwendet, um versuchen, starten und Beenden des virtuellen Computers mit dem Ergebnis der Anforderung einer Variablen gespeichert.  Klicken Sie dann wird eine Nachricht gesendet, zur Ausgabe zurück, der angibt, ob die Anfrage beginnen oder beenden erfolgreich übermittelt wurde.


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Arbeiten mit untergeordneten Runbooks finden Sie unter [untergeordneten Runbooks in Azure Automatisierung](automation-child-runbooks.md) 
- Weitere Informationen zu ausgehenden Nachrichten während der Ausführung des Runbooks und Protokollierung zur Behandlung finden Sie unter [Runbooks Ausgabe und Nachrichten in Azure Automatisierung](automation-runbook-output-and-messages.md)
