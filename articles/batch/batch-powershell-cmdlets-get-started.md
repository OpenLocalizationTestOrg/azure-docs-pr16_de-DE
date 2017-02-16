<properties
   pageTitle="Erste Schritte mit Azure Stapel PowerShell | Microsoft Azure"
   description="Abrufen Sie eine kurze Einführung in die Azure PowerShell-Cmdlets zum Verwalten der Stapel Azure Service verwendbare"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="10/20/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-powershell-cmdlets"></a>Erste Schritte mit Azure Stapel PowerShell-cmdlets
Mit den Azure Stapel PowerShell-Cmdlets können Sie viele der Aufgaben, die Sie durch den Stapel APIs, Azure-Portal und der Azure Line Interface (CLI) ausführen, Skripts und ausführen. Dies ist eine kurze Einführung in die Cmdlets, die Sie zum Verwalten Ihrer Stapel Benutzerkonten und Arbeiten mit Ressourcen Stapel wie Pools, Projekten und Aufgaben verwenden können.

Eine vollständige Liste der Stapel Cmdlets und detaillierte Cmdlet Syntax finden Sie unter den [Stapel Azure-Cmdlet Bezug](https://msdn.microsoft.com/library/azure/mt125957.aspx).

In diesem Artikel basiert auf Cmdlets in Azure PowerShell Version 3.0.0. Es empfiehlt sich, dass Sie Ihre Azure PowerShell häufig um Dienstupdates und Erweiterungen nutzen aktualisieren.

## <a name="prerequisites"></a>Erforderliche Komponenten

Führen Sie die folgenden Vorgänge um Azure PowerShell zum Verwalten Ihrer Stapel Ressourcen zu verwenden.

* [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)

* Führen Sie das **Login-AzureRmAccount** -Cmdlet für die Verbindung zu Ihrem Abonnement (die Azure Stapel Cmdlets liefern im Modul Azure Ressourcenmanager):

    `Login-AzureRmAccount`

* **Registrieren Sie sich mit dem Blattnamen Anbieternamespace**. Dieser Vorgang muss nur ausgeführte **einmal pro Abonnement**sein.

    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Verwalten von Konten Stapel und Schlüssel

### <a name="create-a-batch-account"></a>Erstellen Sie ein Konto Stapel

**Neu-AzureRmBatchAccount** erstellt ein Stapel-Konto in einer bestimmten Ressourcengruppe an. Wenn Sie bereits über eine Ressourcengruppe besitzen, erstellen Sie eine Ausführung des Cmdlets [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx) . Geben Sie eine der Azure Regionen in den **Speicherort** Parameter, wie etwa "Zentralen US" aus. Beispiel:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Erstellen Sie ein Stapel-Konto klicken Sie dann in der Ressourcengruppe zurück, der einen Namen für das Konto in <*Kontoname*> und den Speicherort und Namen der Ressourcengruppe angibt. Erstellen des Kontos Stapel, kann einige Zeit in Anspruch nehmen. Beispiel:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [AZURE.NOTE] Der Blattname Konto muss das Azure Region für die Ressourcengruppe eindeutig sein, zwischen 3 und 24 Zeichen enthalten und Kleinbuchstaben und nur Zahlen verwenden.

### <a name="get-account-access-keys"></a>Abrufen von Konto-Tastenkombinationen
**Get-AzureRmBatchAccountKeys** zeigt die Tastenkombinationen, die mit einem Stapel Azure-Konto verknüpft ist. Führen Sie zum Beispiel vor, um die primären und sekundären Schlüssel des Kontos zu erhalten, die Sie erstellt haben.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Generieren einer neuen Zugriffstaste
**Neu-AzureRmBatchAccountKey** generiert die primäre oder sekundäre Konto einen neuen Product Key für ein Stapel Azure-Konto an. Angenommen, um einen neuen Primärschlüssel für Ihr Konto Stapel generieren, geben Sie Folgendes ein:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [AZURE.NOTE] Geben Sie "Sekundäre" für den Parameter **KeyType** an, um einen neuen sekundären Schlüssel zu generieren. Sie müssen die primären und sekundären Schlüssel separat neu zu generieren.

### <a name="delete-a-batch-account"></a>Löschen eines Kontos Stapel
**Entfernen-AzureRmBatchAccount** Löscht ein Stapel-Konto an. Beispiel:

    Remove-AzureRmBatchAccount -AccountName <account_name>

Wenn Sie dazu aufgefordert werden, zu bestätigen, dass Sie das Konto entfernen möchten. Entfernen des Kontos dauert einige Zeit in Anspruch.

## <a name="create-a-batchaccountcontext-object"></a>Erstellen Sie ein Objekt BatchAccountContext

Die Stapel PowerShell-Cmdlets verwenden, wenn Sie erstellen und Verwalten von Stapel Pools, Projekte, Aufgaben und anderen Ressourcen Authentifizierung zuerst erstellen Sie ein Objekt BatchAccountContext zum Speichern Ihrer Kontonamen und Schlüssel:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Sie übergeben das Objekt BatchAccountContext in Cmdlets, die den **BatchContext** -Parameter verwenden.

> [AZURE.NOTE] Standardmäßig wird der Firma Primärschlüssel für die Authentifizierung verwendet, jedoch können Sie die EINGABETASTE, um die verwenden, indem Sie Ihre BatchAccountContext-Objekt **KeyInUse** Eigenschaft explizit auswählen: `$context.KeyInUse = "Secondary"`.

## <a name="create-and-modify-batch-resources"></a>Erstellen und Ändern von Stapel Ressourcen
Verwenden Sie Cmdlets wie **Neu-AzureBatchPool**, **Neu-AzureBatchJob**und **Neu-AzureBatchTask** Ressourcen unter einem Stapel Konto erstellen. Sie finden entsprechende **Get -** und **Set -** Cmdlets zum Aktualisieren der Eigenschaften von vorhandenen Ressourcen und **Entfernen -** Cmdlets um Ressourcen unter einem Stapel Konto zu entfernen.

Wenn Sie viele dieser Cmdlets, sowie ein Objekt BatchContext übergeben verwenden müssen Sie erstellen oder Objekte, die ausführliche ressourceneinstellungen, enthalten übergeben, wie im folgenden Beispiel dargestellt. Finden Sie detaillierten Informationen für jedes Cmdlet Weitere Beispiele aus.

### <a name="create-a-batch-pool"></a>Erstellen von einem Stapel Ressourcenpool

Beim Erstellen oder einem Stapel Ressourcenpool aktualisieren, wählen Sie eine Cloud-Service-Konfiguration oder eine Konfiguration des virtuellen Computers für das Betriebssystem auf die Datenverarbeitungsknoten (finden Sie unter [Übersicht über die Features Stapel](batch-api-basics.md#pool)) aus. Ihre Auswahl bestimmt, ob es sich bei Ihrer Datenverarbeitungsknoten mit eine der [Azure Gast OS frei](../cloud-services/cloud-services-guestos-update-matrix.md#releases) oder mit einem der unterstützten Linux oder Windows virtueller Computer Bilder in der Azure Marketplace abgebildet wurden.

Wenn Sie **Neu-AzureBatchPool**ausführen, übergeben Sie die Standardeinstellungen des Betriebssystems in einem PSCloudServiceConfiguration oder PSVirtualMachineConfiguration Objekt. Das folgende Cmdlet erstellt beispielsweise ein neues Blatt Pool mit Größe klein berechnen Knoten in der Cloud Service-Konfiguration, mit der neuesten Version von Betriebssystem der Familie 3 (Windows Server 2012) abgebildet an. Hier gibt der Parameter **CloudServiceConfiguration** die *$configuration* -Variable wie das Objekt PSCloudServiceConfiguration. Der **BatchContext** Parameter gibt eine zuvor definierte Variable *$context* an, wie das Objekt BatchAccountContext.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Die Ziel-Anzahl der berechnen-Knoten in der neuen Pool wird durch eine automatische Skalierung Formel bestimmt. In diesem Fall ist die Formel einfach **$TargetDedicated = 4**, 4, die die Anzahl von Computeknoten im Pool höchstens ist.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Abfrage für Pools, Projekte, Aufgaben und andere details

Verwenden von Cmdlets wie **Get-AzureBatchPool**, **Get-AzureBatchJob**und **Get-AzureBatchTask** Abfrage für Personen, die unter einem Stapel Konto erstellt.

### <a name="query-for-data"></a>Abfrage für Daten

Verwenden Sie beispielsweise **Get-AzureBatchPools** , um Pools zu finden. Standardmäßig gespeichert dieser Abfragen für alle Pools unter Ihrem Konto, wenn Sie bereits das Objekt BatchAccountContext in *$context*aus:

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>Verwenden Sie einen OData-filter

Sie können angeben, einen OData-Filter den **Filter** -Parameter verwenden, um nur die Objekte zu finden, die, denen Sie interessiert. Beispielsweise können Sie alle Pools mit Ids beginnen mit "MyPool" finden:

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

Diese Methode ist nicht so flexibel wie "Where-Object" in einer lokalen Verkaufspipeline verwenden. Jedoch wird die Abfrage an den Stapel Dienst direkt gesendet, damit alle Filter auf dem Server, speichern die Internetbandbreite geschieht.

### <a name="use-the-id-parameter"></a>Verwenden Sie den Id-parameter

Eine Alternative zum OData Filter ist den **ID-** Parameter verwenden. Bei einem bestimmten Pool mit Id "MyPool" Abfragen:

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

Der **ID-** Parameter unterstützt nur vollständigen-ID-Suche, keine Platzhalterzeichen oder OData-Schreibweise Filter.

### <a name="use-the-maxcount-parameter"></a>Verwenden Sie den Parameter MaxCount

Standardmäßig gibt jedes Cmdlet maximal 1000 Objekte an. Wenn Sie diesen Grenzwert erreicht haben, verfeinern Sie Ihre Filter, um wieder weniger Objekte anzuzeigen, oder unter Verwendung des Parameters **MaxCount** maximal explizit festgelegt. Beispiel:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Legen Sie zum Entfernen der Obergrenze **MaxCount** 0 oder weniger.

### <a name="use-the-powershell-pipeline"></a>Verwenden der Verkaufspipeline PowerShell

Stapel Cmdlets kann der PowerShell Verkaufspipeline zum Senden von Daten zwischen Cmdlets genutzt werden. Dies hat die gleiche Auswirkung wie die Angabe eines Parameters, sondern arbeiten mit mehreren Personen erleichtert.

Angenommen, suchen und alle Vorgänge unter Ihrem Konto anzeigen:

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Restart (Neustart) jeder Datenverarbeitungsknoten in einem Ressourcenpool:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Verwalten von Paketen

Anwendungspakete bieten eine vereinfachte Möglichkeit zum Bereitstellen von Applications zu berechnen Knoten in der Pools. Mit den Stapel PowerShell-Cmdlets können Sie hochladen und Anwendungspakete in Ihr Konto Stapel verwalten und Bereitstellen Paketversionen, um den Knoten zu berechnen.

**Erstellen** einer Anwendung:

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

**Hinzufügen** eines Anwendungspakets:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

Legen Sie die **Standardversion** der Anwendung:

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

**Liste** der Anwendung Pakete

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

**Löschen** eines Anwendungspakets

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

**Löschen** einer Anwendung

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

>[AZURE.NOTE] Sie müssen alle Versionen einer Anwendung Anwendung löschen, bevor Sie die Anwendung löschen. Sie erhalten einen 'Konflikt' Fehler, wenn Sie versuchen, eine Anwendung zu löschen, die aktuell Anwendungspakete hat.

### <a name="deploy-an-application-package"></a>Ein Anwendungspaket bereitgestellt

Sie können eine oder mehrere Anwendungspakete für die Bereitstellung beim Erstellen einer Ressourcenpool angeben. Wenn Sie ein Paket zum Zeitpunkt der Erstellung Pool angeben, wird als Knoten Verknüpfungen Pool einzelnen Knoten bereitgestellt. Pakete sind auch bereitgestellt werden, wenn ein Knoten neu gestartet oder einem neuen Image versehen ist.

Geben Sie die `-ApplicationPackageReference` option beim Erstellen eines Pools so, dass ein Anwendungspaket zu dem Pool des Knoten bereitstellen, wie er im Ressourcenpool teilnehmen an. Zunächst erstellen Sie ein **PSApplicationPackageReference** Objekt, und konfigurieren Sie ihn mit der Anwendung-Id und Paket-Version, die Sie dem Pool des Datenverarbeitungsknoten bereitstellen möchten:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Jetzt im Ressourcenpool zu erstellen, und geben Sie das Paket Bezug Objekt als Argument für die `ApplicationPackageReferences` Option:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

Weitere Informationen zum Anwendungspaketen finden Sie in der [Bereitstellung der Anwendung mit Azure Stapel Anwendungspakete](batch-application-packages.md).

>[AZURE.IMPORTANT] Sie müssen [Link ein Konto Azure-Speicher](#linked-storage-account-autostorage) bei Ihrem Konto Stapel Anwendungspakete verwenden.

### <a name="update-a-pools-application-packages"></a>Ein Ressourcenpool Anwendungspakete aktualisieren

Um die zu einem vorhandenen Pool zugewiesen Anwendungen zu aktualisieren, müssen Sie zuerst erstellen Sie ein Objekt PSApplicationPackageReference mit den gewünschten Eigenschaften (Anwendung-Id und das Paket Version):

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

Als Nächstes im Ressourcenpool aus Stapel erhalten, deaktivieren Sie ein bestehendes Paket, unserer neuen Paket Verweis hinzufügen und Aktualisierungsdienst Stapel mit den neuen Ressourcenpool Einstellungen:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

Jetzt haben Sie dem Pool der Eigenschaften in den Stapel Dienst aktualisiert. Tatsächlich zur Bereitstellung der neuen Anwendungspakets zum Berechnen von Knoten im Pool müssen Sie jedoch neu starten oder ein neues Abbild diese Knoten. Sie können jeder Knoten in einem Ressourcenpool mit dem folgenden Befehl neu starten:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

>[AZURE.TIP] Sie können mehrere Anwendungspakete berechnen Knoten in einem Ressourcenpool bereitstellen. Wenn Sie zum *Hinzufügen* ein Anwendungspakets anstelle der aktuell bereitgestellten Pakete ersetzen möchten, lassen Sie die `$pool.ApplicationPackageReferences.Clear()` darüber liegenden Zeile.

## <a name="next-steps"></a>Nächste Schritte

* Ausführliche Cmdlet Syntax und Beispiele finden Sie unter [Azure Stapel Cmdlet verweisen](https://msdn.microsoft.com/library/azure/mt125957.aspx).

* Weitere Informationen zu Anwendungen und Anwendungspakete in Stapel finden Sie unter [Anwendung Bereitstellung mit Azure Stapel Anwendungspakete](batch-application-packages.md).
