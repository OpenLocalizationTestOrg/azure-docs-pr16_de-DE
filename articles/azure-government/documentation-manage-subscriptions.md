<properties
    pageTitle="Azure Government Services | Microsoft Azure"
    description="Informationen zum Verwalten Ihrer Abonnements Azure Government"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/21/2016"
    ms.author="zakramer" />


#  <a name="managing-and-connecting-to-your-subscription-in-azure-government"></a>Verwalten und Herstellen einer Verbindung mit Ihrem Abonnement in Azure Government

Azure Regierung hat eindeutige URLs und Endpunkte für die Verwaltung Ihrer Umgebung. Es ist wichtig, die richtigen Verbindungen zum Verwalten Ihrer Umgebung über das Portal oder PowerShell verwenden. Nachdem Sie mit der Umgebung Azure Government verbunden sind, die den normalen Betrieb für die Verwaltung von einem Dienst funktioniert, wenn Sie die Komponente bereitgestellt wurde.

## <a name="connecting-via-the-portal"></a>Herstellen einer Verbindung über das-portal
Das Portal ist die primäre Methode, die meisten Personen mit Azure Government verbinden.  Um eine Verbindung herzustellen, navigieren Sie zu dem Portal unter [https://portal.azure.us](https://portal.azure.us).  Die ältere Version des Azure Portals kann über [https://manage.windowsazure.us](https://manage.windowsazure.us)zugegriffen werden.

Abonnements können durch Herstellen einer Verbindung mit [https://account.windowsazure.us](https://account.windowsazure.us)für Ihr Konto erstellt werden.

## <a name="connecting-via-powershell"></a>Herstellen einer Verbindung über PowerShell

Gibt an, ob Sie Azure PowerShell verwenden, um eine große Abonnement durch Skript oder eine Access-Features zu verwalten, die aktuell Azure-Portal keine stehen Verbindung zum Azure Government anstelle von öffentlichen Azure benötigte.  Wenn Sie in öffentlichen Azure PowerShell verwendet haben, ist es hauptsächlich identisch.  Die Unterschiede in Azure Government sind:

+ Herstellen einer Verbindung Ihres Kontos
+ Bereichsnamen

>[AZURE.NOTE] Wenn Sie PowerShell noch nicht verwendet haben, lesen Sie die [Einführung in Azure PowerShell](../powershell-install-configure.md).

Wenn Sie PowerShell beginnen, müssen Sie feststellen, Azure PowerShell zum Azure Government herstellen, indem Sie einen Umgebungsparameter-, die angeben.  Der Parameter stellt sicher, dass PowerShell mit den richtigen Endpunkten verbunden ist.  Wenn Sie melden Sie sich bei Ihrem Konto verbinden, wird die Auflistung von Endpunkten bestimmt.  Verschiedene APIs erfordern unterschiedliche Versionen von der Umgebung wechseln:

Verbindungstyp | Befehl
---|----
[Servicemanagement](https://msdn.microsoft.com/library/dn708504.aspx) -Befehle | `Add-AzureAccount -Environment AzureUSGovernment`
Befehle zur [Verwaltung von Ressourcen](https://msdn.microsoft.com/library/mt125356.aspx) | `Add-AzureRmAccount -EnvironmentName AzureUSGovernment`
[Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx) -Befehle | `Connect-MsolService -AzureEnvironment UsGovernment`
[Azure Active Directory-Befehl Version 2](https://msdn.microsoft.com/library/azure/mt757189.aspx) | `Connect-AzureAD -AzureEnvironmentName AzureUSGovernment`

Sie können auch verwenden den Schalter-Umgebung, die mit einem neu-AzureStorageContext mit Speicherkonto Herstellen einer Verbindung und AzureUSGovernment angeben.

### <a name="determining-region"></a>Bestimmen der region

Nachdem Sie verbunden sind, besteht eine weiterer Unterschied – die Regionen verwendet, um einen Dienst adressieren.  Jede Azure Cloud weist verschiedene Regionen.  Sie sehen sie auf der Seite Verfügbarkeit der Dienst aufgeführt.  Meist verwenden Sie den Bereich in den Speicherort Parameter für einen Befehl.

Es hat jedoch einen Haken.  Die Regionen Azure Government müssen anders als deren allgemeinen Namen formatiert werden:

Definierter name | Befehl
---|----
US Gov Virginia | USGov Virginia
US Gov Iowa | USGov Iowa

>[AZURE.NOTE] Es ist kein Leerzeichen zwischen der USA und gov – bei Verwendung des Parameters Speicherort.

Wenn Sie schon einmal die verfügbaren Regionen in Azure Government überprüfen möchten, können Sie führen Sie die folgenden Befehle und Drucken die aktuelle Liste:

    Get-AzureLocation

Wenn Sie die verfügbaren Umgebungen Näheres über Azure sind, können Sie Folgendes ausführen:

    Get-AzureEnvironment

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Informationen suchen, können Sie Auschecken:

+ [PowerShell-Dokumente auf GitHub](https://github.com/Azure/azure-powershell)
+ [Eine schrittweise Anleitung zum Herstellen einer Verbindung zu Ressourcenverwaltung](https://blogs.msdn.microsoft.com/azuregov/2015/10/08/configuring-arm-on-azure-gc/)
+ [Azure PowerShell-Dokumente auf MSDN](https://msdn.microsoft.com/library/mt619274.aspx)

Abonnieren Sie zusätzliche Informationen und Updates zum [Microsoft Azure Government Blog] (https://blogs.msdn.microsoft.com/azuregov/)
