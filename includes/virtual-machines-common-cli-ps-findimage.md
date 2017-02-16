

## <a name="azure-cli"></a>Azure CLI

> [AZURE.NOTE] Dieser Artikel beschreibt, wie zu navigieren, und wählen virtuellen Computern Bilder, eine aktuelle Installation des Azure CLI oder Azure PowerShell verwenden. Voraussetzung ist müssen Sie in den Modus Ressourcenmanager ändern. Geben Sie mit der CLI Azure dieser Modus durch Eingeben der `azure config mode arm`. 

Die einfachste und schnellste Möglichkeit, suchen Sie ein Bild, um entweder mit verwenden `azure vm quick-create` oder zum Erstellen einer Ressource Gruppenvorlage Datei aufrufen, ist die `azure vm image list` Befehl und den Speicherort, den Namen des Herausgebers (es ist nicht Groß-/Kleinschreibung beachtet!) und eines Angebots – übergeben, wenn Sie wissen, dass das Angebot. Die folgende Liste beträgt beispielsweise nur ein kurzes Beispiel – viele Listen sind sehr lange – aus, wenn Sie wissen, dass "Kanonisch" eines Herausgebers für das Angebot "UbuntuServer" ist.

    azure vm image list westus canonical ubuntuserver
    info:    Executing command vm image list
    warn:    The parameter --sku if specified will be ignored
    + Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
    data:    Publisher  Offer         Sku                OS     Version          Location  Urn
    data:    ---------  ------------  -----------------  -----  ---------------  --------  --------------------------------------------------------
    data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201604203  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201604203
    data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201605161  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201605161
    data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201606100  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606100
    data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201606270  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606270
    data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201607210  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201607210
    data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201608150  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201608150
    data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607220  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607220
    data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607230  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607230
    data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607240  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607240

Die Spalte **Urn** werden auf das Formular, das Sie zu übergeben `azure vm quick-create`.

Häufig, wissen nicht jedoch Sie noch, was verfügbar ist. In diesem Fall können Sie Bilder navigieren, indem Sie entdecken Herausgeber zuerst mithilfe von `azure vm image list-publishers` und reagieren auf die Aufforderung Speicherort mit einem Data Center Speicherort für Ihre Ressourcengruppe verwendet werden soll. Beispielsweise Listen folgenden alle Bild Herausgeber Westen US-Speicherort (das Argument Location durch Kleinbuchstaben und Entfernen von Leerzeichen standard Orten bestehen)

    azure vm image list-publishers
    info:    Executing command vm image list-publishers
    Location: westus
    + Getting virtual machine and/or extension image publishers (Location: "westus")
    data:    Publisher                                       Location
    data:    ----------------------------------------------  --------
    data:    a10networks                                     westus  
    data:    aiscaler-cache-control-ddos-and-url-rewriting-  westus  
    data:    alertlogic                                      westus  
    data:    AlertLogic.Extension                            westus  


Diese Listen können sehr lang sein, damit die obigen Beispiel Liste nur ein Ausschnitt ist. Einmal angenommen, dass ich schon festgestellt, dass kanonisch tatsächlich, ein Bild Publisher Westen US-Speicherort ist. Sie können ihre Angebote durch Einwahl finden `azure vm image list-offers` und den Speicherort und Herausgeber bei den Anweisungen, wie im folgenden Beispiel übergeben:

    azure vm image list-offers
    info:    Executing command vm image list-offers
    Location: westus
    Publisher: canonical
    + Getting virtual machine image offers (Publisher: "canonical" Location:"westus")
    data:    Publisher  Offer                      Location
    data:    ---------  -------------------------  --------
    data:    canonical  Ubuntu15.04Snappy          westus
    data:    canonical  Ubuntu15.04SnappyDocker    westus
    data:    canonical  UbunturollingSnappy        westus
    data:    canonical  UbuntuServer               westus
    data:    canonical  Ubuntu_Snappy_Core         westus
    data:    canonical  Ubuntu_Snappy_Core_Docker  westus
    info:    vm image list-offers command OK

Wir nun wissen, dass in der Region Westen US kanonisch **UbuntuServer** Angebot auf Azure veröffentlicht. Aber welche SKUs? Um die zu gelangen, die Sie anrufen `azure vm image list-skus` und Antworten auf die Aufforderung mit der Position, Publisher und anbieten, die Sie ermittelt haben.

    azure vm image list-skus
    info:    Executing command vm image list-skus
    Location: westus
    Publisher: canonical
    Offer: ubuntuserver
    + Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
    data:    Publisher  Offer         sku                Location
    data:    ---------  ------------  -----------------  --------
    data:    canonical  ubuntuserver  12.04.2-LTS        westus
    data:    canonical  ubuntuserver  12.04.3-LTS        westus
    data:    canonical  ubuntuserver  12.04.4-LTS        westus
    data:    canonical  ubuntuserver  12.04.5-DAILY-LTS  westus
    data:    canonical  ubuntuserver  12.04.5-LTS        westus
    data:    canonical  ubuntuserver  12.10              westus
    data:    canonical  ubuntuserver  14.04-beta         westus
    data:    canonical  ubuntuserver  14.04.0-LTS        westus
    data:    canonical  ubuntuserver  14.04.1-LTS        westus
    data:    canonical  ubuntuserver  14.04.2-LTS        westus
    data:    canonical  ubuntuserver  14.04.3-LTS        westus
    data:    canonical  ubuntuserver  14.04.4-DAILY-LTS  westus
    data:    canonical  ubuntuserver  14.04.4-LTS        westus
    data:    canonical  ubuntuserver  14.04.5-DAILY-LTS  westus
    data:    canonical  ubuntuserver  14.04.5-LTS        westus
    data:    canonical  ubuntuserver  14.10              westus
    data:    canonical  ubuntuserver  14.10-beta         westus
    data:    canonical  ubuntuserver  14.10-DAILY        westus
    data:    canonical  ubuntuserver  15.04              westus
    data:    canonical  ubuntuserver  15.04-beta         westus
    data:    canonical  ubuntuserver  15.04-DAILY        westus
    data:    canonical  ubuntuserver  15.10              westus
    data:    canonical  ubuntuserver  15.10-alpha        westus
    data:    canonical  ubuntuserver  15.10-beta         westus
    data:    canonical  ubuntuserver  15.10-DAILY        westus
    data:    canonical  ubuntuserver  16.04-alpha        westus
    data:    canonical  ubuntuserver  16.04-beta         westus
    data:    canonical  ubuntuserver  16.04.0-DAILY-LTS  westus
    data:    canonical  ubuntuserver  16.04.0-LTS        westus
    data:    canonical  ubuntuserver  16.10-DAILY        westus
    info:    vm image list-skus command OK

Mithilfe dieser Informationen finden Sie jetzt genau das Bild aus, indem Sie den ursprünglichen Anruf oben aufrufen.

    azure vm image list westus canonical ubuntuserver 16.04.0-LTS
    info:    Executing command vm image list
    + Getting virtual machine images (Publisher:"canonical" Offer:"ubuntuserver" Sku: "16.04.0-LTS" Location:"westus")
    data:    Publisher  Offer         Sku          OS     Version          Location  Urn
    data:    ---------  ------------  -----------  -----  ---------------  --------  --------------------------------------------------
    data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201604203  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201604203
    data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201605161  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201605161
    data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201606100  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606100
    data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201606270  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606270
    data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201607210  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201607210
    data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201608150  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201608150
    info:    vm image list command OK

Jetzt können Sie genau das Bild auswählen, die, das Sie verwenden möchten. Schnelles Erstellen einer virtuellen Computern unter Verwendung der Informationen URN, das Sie einfach zu finden, oder eine Vorlage mit diesen Informationen URN verwenden, finden Sie unter [Verwendung der CLI Azure für Mac, Linux, und Windows Azure-Ressourcenmanager](../articles/xplat-cli-azure-resource-manager.md).

## <a name="powershell"></a>PowerShell

> [AZURE.NOTE] Installieren Sie und konfigurieren Sie die [Neuesten Azure PowerShell](../articles/powershell-install-configure.md). Wenn Sie Azure PowerShell-Module unter 1.0 verwenden, Sie weiterhin die folgenden Befehle verwenden, doch müssen Sie zuerst `Switch-AzureMode AzureResourceManager`. 

Beim Erstellen eines neuen virtuellen Computers mit Azure Ressourcenmanager in einigen Fällen müssen Sie ein Bild mit der Kombination der folgenden Bildeigenschaften angeben:

- Publisher
- Angebot
- SKU

Angenommen, werden diese Werte benötigt, für die `Set-AzureRMVMSourceImage` PowerShell-Cmdlet oder eine Gruppe Vorlage Ressourcendatei in der Geben Sie den Typ des virtuellen Computers erstellt werden.

Wenn Sie diese Werte bestimmen müssen, können Sie die Bilder, um diese Werte bestimmen navigieren:

1. Liste der Herausgeber Bild.
2. Listen Sie für einen bestimmten Herausgeber ihre Angebote aus.
3. Liste einer angegebenen anbieten deren SKUs.


Zunächst Liste der Herausgeber mit der folgenden Befehle:

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Geben Sie Ihren Namen ausgewählten Publisher, und führen Sie die folgenden Befehle:

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Geben Sie Ihren Namen ausgewählten Angebot, und führen Sie die folgenden Befehle:

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Von der Anzeige von der `Get-AzureRMVMImageSku` Befehl, stehen Ihnen alle Informationen, die Sie benötigen, um das Bild für einen neuen virtuellen Computer anzugeben.

Die nachstehende Abbildung zeigt ein vollständiges Beispiel:

```powershell
PS C:\> $locName="West US"
PS C:\> Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Für den Herausgeber "MicrosoftWindowsServer":

```powershell
PS C:\> $pubName="MicrosoftWindowsServer"
PS C:\> Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer

Offer
-----
WindowsServer
```

Für das Angebot "WindowsServer":

```powershell
PS C:\> $offerName="WindowsServer"
PS C:\> Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus

Skus
----
2008-R2-SP1
2012-Datacenter
2012-R2-Datacenter
2016-Nano-Server-Technical-Previe
2016-Technical-Preview-with-Conta
Windows-Server-Technical-Preview
```

Kopieren Sie den Namen der ausgewählten SKU aus dieser Liste, und stehen Ihnen alle Informationen für die `Set-AzureRMVMSourceImage` PowerShell-Cmdlet oder für eine Ressource Gruppenvorlage.


<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/