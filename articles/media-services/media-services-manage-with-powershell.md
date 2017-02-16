<properties 
    pageTitle="Verwalten von Azure Media Services-Konten mit PowerShell" 
    description="Informationen Sie zum Verwalten von Azure Media Services-Konten mit PowerShell-Cmdlets." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"
    ms.author="juliako"/>


#<a name="manage-azure-media-services-accounts-with-powershell"></a>Verwalten von Azure Media Services-Konten mit PowerShell

> [AZURE.SELECTOR]
- [Portal](media-services-portal-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [REST](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Wenn Sie ein Konto Azure Media Services erstellen können, müssen Sie ein Azure-Konto verfügen. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure kostenlose Testversion</a>.

##<a name="overview"></a>(Übersicht) 

Dieser Artikel listet die Azure PowerShell-Cmdlets für Azure Media Services (AMS) in Azure Ressourcenmanager Framework. Die Cmdlets, die in den Namespace **Microsoft.Azure.Commands.Media** vorhanden sind.

## <a name="versions"></a>Versionen

**ApiVersion**: "2015-10-01"
               

## <a name="new-azurermmediaservice"></a>Neue AzureRmMediaService

Erstellt einen Media-Dienst.

### <a name="syntax"></a>Syntax

Parameter festlegen: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

Parameter festlegen: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a>Parameter

**ResourceGroupName - &lt;Zeichenfolge&gt;**

Gibt den Namen der Ressourcengruppe diesen Dienst Medien gehört.

Aliase | keine
---|---
Erforderlich?   |  WAHR
Positionieren Sie?   |  0
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen? |True(ByPropertyName)
Annehmen Platzhalterzeichen?  |falsch

**Kontoname - &lt;Zeichenfolge&gt;**

Gibt den Namen des Diensts Medien.

Aliase |Namen
---|---
Erforderlich? |WAHR
Positionieren Sie? |1
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen? |falsch
Annehmen Platzhalterzeichen? |falsch

**-Speicherort &lt;Zeichenfolge&gt;**

Gibt den Ressourcenspeicherort des Diensts Medien.

Aliase |keine
---|---
Erforderlich? |WAHR
Positionieren Sie? |2
Standardwert  |keine
Eingaben Verkaufspipeline vorgenommen? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**StorageAccountId - &lt;Zeichenfolge&gt;**

Gibt ein primären Speicher-Konto, das mit dem Dienst Medien zugeordnet ist.

- Speicher Neukunde (erstellt mit den Ressourcen-Manager-API) unterstützt nur.

- Das Speicherkonto muss vorhanden und derselben Stelle mit dem Dienst Medien hat.

Aliase |keine
---|---
Erforderlich? |WAHR
Positionieren Sie? |3
Standardwert  |keine
Eingaben Verkaufspipeline vorgenommen? |True(ByPropertyName)
Festlegen von Parameternamen |StorageAccountIdParamSet
Annehmen Platzhalterzeichen?|falsch

**StorageAccounts - &lt;PSStorageAccount\[\]&gt;**

Gibt an, Speicherkonten, die mit dem Dienst Medien zugeordnet ist.

- Speicher Neukunde (erstellt mit den Ressourcen-Manager-API) unterstützt nur.

- Das Speicherkonto muss vorhanden und derselben Stelle mit dem Dienst Medien hat.

- Nur eine Speicher-Konto kann als primärem angegeben werden muss.

Aliase |keine
---|---
Erforderlich?  |WAHR
Positionieren Sie?  |3
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen? |True(ByPropertyName)
Festlegen von Parameternamen |StorageAccountsParamSet
Annehmen Platzhalterzeichen? |falsch

**-Tags &lt;Hashtable&gt;**

Gibt eine Hashtabelle der Tags, die den Media-Dienst zugeordnet sind.

- Beispiel:@{"tag1"="value1";"tag2"=:value2"}

Aliase |keine
---|---
Erforderlich?  |falsch
Positionieren Sie?  |mit dem Namen
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen? |falsch
Annehmen Platzhalterzeichen? |falsch

**&lt;Befehlsparameter&gt;**

Dieses Cmdlet unterstützt die allgemeinen Parameter:-Debuggen, - ErrorAction, ErrorVariable-, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - ausführliche, - WarningAction, und -WarningVariable.

### <a name="inputs"></a>Eingaben

Der Eingabe Typ ist den Typ der Objekte, die Sie an das Cmdlet leiten können.

### <a name="outputs"></a>Ausgaben

Der Typ ist den Typ der Objekte, die das Cmdlet gibt aus.

## <a name="set-azurermmediaservice"></a>Set-AzureRmMediaService

Aktualisiert einen Media-Dienst an.

### <a name="syntax"></a>Syntax

    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Parameter

**ResourceGroupName - &lt;Zeichenfolge&gt;**

Gibt den Namen der Ressourcengruppe diesen Dienst Medien gehört.

Aliase |keine
---|---
Erforderlich?  |WAHR
Positionieren Sie?  |0
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**Kontoname - &lt;Zeichenfolge&gt;**

Gibt den Namen des Diensts Medien.

Aliase |Namen
---|---
Erforderlich? |WAHR
Positionieren Sie? |1
Standardwert |Keine
Eingaben Verkaufspipeline vorgenommen? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |Falsch

**StorageAccounts - &lt;PSStorageAccount\[\]&gt;**

Gibt an, Speicherkonten, die mit dem Dienst Medien zugeordnet ist.

- Speicher Neukunde (erstellt mit den Ressourcen-Manager-API) unterstützt nur.

- Das Speicherkonto muss vorhanden und derselben Stelle mit dem Dienst Medien hat.

- Nur eine Speicher-Konto kann als primärem angegeben werden muss.

Aliase |keine
---|---
Erforderlich? |falsch
Positionieren Sie? |Mit dem Namen
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen? |True(ByPropertyName)
Festlegen von Parameternamen |StorageAccountsParamSet
Annehmen Platzhalterzeichen? |falsch

**-Tags &lt;Hashtable&gt;**

Gibt eine Hashtabelle der Tags, die diesen Dienst Medien zugeordnet sind.

- Die Kategorien, die den Media-Dienst zugeordnet sind, werden mit vom Kunden angegebenen Wert ersetzt.

Aliase |keine
---|---
Erforderlich? |Falsch
Positionieren Sie?  |Mit dem Namen
Standardwert |Keine
Eingaben Verkaufspipeline vorgenommen? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**&lt;Befehlsparameter&gt;**

Dieses Cmdlet unterstützt die allgemeinen Parameter:-Debuggen, - ErrorAction, ErrorVariable-, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - ausführliche, - WarningAction, und -WarningVariable.

### <a name="inputs"></a>Eingaben

Der Eingabe Typ ist den Typ der Objekte, die Sie an das Cmdlet leiten können.

### <a name="outputs"></a>Ausgaben

Der Typ ist den Typ der Objekte, die das Cmdlet gibt aus.

## <a name="remove-azurermmediaservice"></a>Entfernen-AzureRmMediaService

Entfernt einen Media-Dienst an.

### <a name="syntax"></a>Syntax

    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameter

**ResourceGroupName - &lt;Zeichenfolge&gt;**

Gibt den Namen der Ressourcengruppe diesen Dienst Medien gehört.

Aliase |keine
---|---
Erforderlich? |WAHR
Positionieren Sie? |0
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**Kontoname - &lt;Zeichenfolge&gt;**

Gibt den Namen des Diensts Medien.

Aliase |keine
---|---
Erforderlich? |WAHR
Positionieren Sie? |2
Standardwert |Keine
Eingaben Verkaufspipeline vorgenommen?  |True(ByPropertyName)
Annehmen Platzhalterzeichen? |Falsch

**&lt;Befehlsparameter&gt;**

Dieses Cmdlet unterstützt die allgemeinen Parameter:-Debuggen, - ErrorAction, ErrorVariable-, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - ausführliche, - WarningAction, und -WarningVariable.

### <a name="inputs"></a>Eingaben

Der Eingabe Typ ist den Typ der Objekte, die Sie an das Cmdlet leiten können.

### <a name="outputs"></a>Ausgaben

Der Typ ist den Typ der Objekte, die das Cmdlet gibt aus.

## <a name="get-azurermmediaservice"></a>Get-AzureRmMediaService

Ruft alle Mediendienste in einer Ressourcengruppe oder einem Dienst Medien mit einem bestimmten Namen ab.

### <a name="syntax"></a>Syntax

ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>] 

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameter

**ResourceGroupName - &lt;Zeichenfolge&gt;**

Gibt den Namen der Ressourcengruppe diesen Dienst Medien gehört.

Aliase |keine
---|---
Erforderlich? |WAHR
Positionieren Sie?  |0
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen? |True(ByPropertyName)
Festlegen von Parameternamen |ResourceGroupParameterSet, AccountNameParameterSet
Annehmen Platzhalterzeichen?   falsch

**Kontoname - &lt;Zeichenfolge&gt;**

Gibt den Namen des Diensts Medien.

Aliase |keine
---|---
Erforderlich? |WAHR
Positionieren Sie?  |1
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen? |True(ByPropertyName)
Festlegen von Parameternamen  |AccountNameParameterSet
Annehmen Platzhalterzeichen? |falsch

**&lt;Befehlsparameter&gt;**

Dieses Cmdlet unterstützt die allgemeinen Parameter:-Debuggen, - ErrorAction, ErrorVariable-, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - ausführliche, - WarningAction, und -WarningVariable.

### <a name="inputs"></a>Eingaben

Der Eingabe Typ ist den Typ der Objekte, die Sie an das Cmdlet leiten können.

### <a name="outputs"></a>Ausgaben

Der Typ ist den Typ der Objekte, die das Cmdlet gibt aus.

## <a name="get-azurermmediaservicekeys"></a>Get-AzureRmMediaServiceKeys

Ruft Schlüssel eines Diensts Medien ab.

### <a name="syntax"></a>Syntax

    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameter

**ResourceGroupName - &lt;Zeichenfolge&gt;**

Gibt den Namen der Ressourcengruppe diesen Dienst Medien gehört.

Aliase |keine
---|---
Erforderlich? |WAHR
Positionieren Sie?  |0
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**Kontoname - &lt;Zeichenfolge&gt;**

Gibt den Namen des Diensts Medien.

Aliase |keine
---|---
Erforderlich? |WAHR
Positionieren Sie? |1
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**&lt;Befehlsparameter&gt;**

Dieses Cmdlet unterstützt die allgemeinen Parameter:-Debuggen, - ErrorAction, ErrorVariable-, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - ausführliche, - WarningAction, und -WarningVariable.

### <a name="inputs"></a>Eingaben

Der Eingabe Typ ist den Typ der Objekte, die Sie an das Cmdlet leiten können.

### <a name="outputs"></a>Ausgaben

Der Typ ist den Typ der Objekte, die das Cmdlet gibt aus.

## <a name="set-azurermmediaservicekey"></a>Set-AzureRmMediaServiceKey

Erneut generiert einen primären oder sekundären Schlüssel Media-Dienst aus.

### <a name="syntax"></a>Syntax

    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Parameter

**ResourceGroupName - &lt;Zeichenfolge&gt;**

Gibt den Namen der Ressourcengruppe diesen Dienst Medien gehört.

Aliase |keine
---|---
Erforderlich?  |WAHR
Positionieren Sie?  |0
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen?  |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**Kontoname - &lt;Zeichenfolge&gt;**

Gibt den Namen des Diensts Medien.

Aliase |keine
---|---
Erforderlich? |WAHR
Positionieren Sie?  |1
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen?   |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**KeyType - &lt;KeyType&gt;**

Gibt den Key Typ des Diensts Medien.

- Primäre oder sekundäre

Aliase |keine
---|---
Erforderlich?  |WAHR
Positionieren Sie?  |2
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen? |falsch
Annehmen Platzhalterzeichen? |falsch

**&lt;Befehlsparameter&gt;**

Dieses Cmdlet unterstützt die allgemeinen Parameter:-Debuggen, - ErrorAction, ErrorVariable-, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - ausführliche, - WarningAction, und -WarningVariable.

### <a name="inputs"></a>Eingaben

Der Eingabe Typ ist den Typ der Objekte, die Sie an das Cmdlet leiten können.

### <a name="outputs"></a>Ausgaben

Der Typ ist den Typ der Objekte, die das Cmdlet gibt aus.

## <a name="sync-azurermmediaservicestoragekeys"></a>Synchronisieren-AzureRmMediaServiceStorageKeys

Synchronisiert Speicher Konto Schlüssel für ein Speicherkonto, dem Dienst Medien zugeordnet.

### <a name="syntax"></a>Syntax

    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameter

**ResourceGroupName - &lt;Zeichenfolge&gt;**

Gibt den Namen der Ressourcengruppe diesen Dienst Medien gehört.

Aliase |keine
---|---
Erforderlich? |WAHR
Positionieren Sie? |0
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**Kontoname - &lt;Zeichenfolge&gt;**

Gibt den Namen des Diensts Medien.

Aliase |keine
---|---
Erforderlich? |WAHR
Positionieren Sie? |1
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen? |True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**StorageAccountId - &lt;Zeichenfolge&gt;**

Gibt den Media-Dienst zugeordnete Speicherplatz Konto an.

Aliase |ID
---|---
Erforderlich? |WAHR
Positionieren Sie?  |2
Standardwert |keine
Eingaben Verkaufspipeline vorgenommen? |      True(ByPropertyName)
Annehmen Platzhalterzeichen? |falsch

**&lt;Befehlsparameter&gt;**

Dieses Cmdlet unterstützt die allgemeinen Parameter:-Debuggen, - ErrorAction, ErrorVariable-, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - ausführliche, - WarningAction, und -WarningVariable.

### <a name="inputs"></a>Eingaben

Der Eingabe Typ ist den Typ der Objekte, die Sie an das Cmdlet leiten können.

### <a name="outputs"></a>Ausgaben

Der Typ ist den Typ der Objekte, die das Cmdlet gibt aus.

## <a name="next-step"></a>Als Nächstes 

Schauen Sie sich learning Pfade Media-Dienste.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
