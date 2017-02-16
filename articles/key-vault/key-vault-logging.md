<properties
    pageTitle="Protokollierung Azure Key Tresor | Microsoft Azure"
    description="Mithilfe dieses Lernprogramms können Sie den ersten Schritten mit Azure-Taste Tresor Protokollierung."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/31/2016"
    ms.author="cabailey"/>

# <a name="azure-key-vault-logging"></a>Azure Key Tresor Protokollierung #
Azure-Taste Tresor ist in den meisten Regionen verfügbar. Weitere Informationen finden Sie unter die [Taste Tresor Preise Seite](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Einführung  
Nachdem Sie eine oder mehrere Key Depots erstellt haben, werden wahrscheinlich möchten Sie überwachen, wann und wie Ihre Key Depots zugegriffen, und von wem sind. Sie können dafür durch das Aktivieren der Protokollierung für Schlüssel Tresor, die Informationen in einem Konto Azure-Speicher speichert, die Sie bereitstellen. Ein neuer Container mit dem Namen **Einsichten-Protokolle-AuditEvent** wird automatisch für Ihr Speicherkonto angegebenen erstellt, und verwenden Sie dieses derselben Speicherkonto zum Sammeln von Protokolle für mehrere Key Depots.

Sie können Ihre Protokollierungsinformationen höchstens zugreifen, 10 Minuten nach der Schlüssel Vaulting Vorgang. In den meisten Fällen wird es als dadurch schneller sein.  Es liegt an Ihnen, Ihre Protokolle in Ihr Speicherkonto verwalten:

- Verwenden Sie Zugriff auf standard Azure Steuerelementmethoden So sichern Sie Ihre Protokolle einschränken, die darauf zugreifen können.
- Löschen Sie die Protokolle, die Sie nicht mehr in Ihrem Speicherkonto beibehalten möchten.

Verwenden Sie dieses Lernprogramm helfen Ihnen den Einstieg mit Azure-Taste Tresor Protokollierung, um Ihr Speicherkonto erstellen, aktivieren Sie die Protokollierung und interpretieren die protokollierten Informationen gesammelt.  


>[AZURE.NOTE]  In diesem Lernprogramm enthält nicht Anleitungen zum Erstellen von Key Depots, Schlüssel oder Kennwörter. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure Schlüssel Tresor](key-vault-get-started.md). Oder Plattformen Line Benutzeroberfläche Anweisungen finden Sie unter [Lernprogramm entspricht](key-vault-manage-with-cli.md).
>
>Sie können keine derzeit Azure-Taste Tresor Azure-Portal konfigurieren. Verwenden Sie stattdessen die folgenden Anweisungen Azure PowerShell.

Die Protokolle, die Sie sammeln können mithilfe von Log Analytics aus der Vorgänge Management Suite visualisiert werden soll. Weitere Informationen finden Sie unter [Azure Schlüssel Tresor (Preview)-Lösung in Log Analytics](../log-analytics/log-analytics-azure-key-vault.md).

Überblick über die Taste Tresor Azure erhalten finden Sie unter [Neuigkeiten Azure-Taste Tresor?](key-vault-whatis.md)

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramms abgeschlossen haben, müssen Sie Folgendes:

- Einer vorhandenen Key Tresor, die Sie verwendet haben.  
- Azure PowerShell **Mindestversion von 1.0.1**. Zum Azure PowerShell installieren und ordnen Sie sie Ihr Abonnement Azure finden Sie unter [Informationen zum Installieren und konfigurieren Sie Azure PowerShell](../powershell-install-configure.md). Wenn Sie Azure PowerShell bereits installiert haben und die Version der Azure-PowerShell-Konsole nicht kennen, geben Sie `(Get-Module azure -ListAvailable).Version`.  
- Über genügend Speicherplatz auf Azure für Ihre Schlüssel Tresor Protokolle.


## <a name="a-idconnectaconnect-to-your-subscriptions"></a><a id="connect"></a>Verbinden mit Ihrer Abonnements ##

Starten einer Sitzung Azure PowerShell, und melden Sie sich bei Ihrem Azure-Konto mit den folgenden Befehl aus:  

    Login-AzureRmAccount

Geben Sie im Popupmenü Browserfenster Ihren Azure-Konto-Benutzernamen und Ihr Kennwort ein. Azure PowerShell erhalten alle Abonnements, die mit diesem Konto und standardmäßig zugeordnet sind, verwendet die erste.

Wenn Sie mehrere Abonnements haben, müssen Sie möglicherweise eine bestimmte angeben, die mit Ihrem Azure-Taste Tresor erstellt wurde. Geben Sie Folgendes ein, um die Abonnements für Ihr Konto finden Sie unter:

    Get-AzureRmSubscription

Geben Sie dann, um anzugeben, das mit Ihrem Key Tresor verknüpft ist, die Sie anmelden werden das Abonnement:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Weitere Informationen zum Konfigurieren von Azure PowerShell finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).


## <a name="a-idstorageacreate-a-new-storage-account-for-your-logs"></a><a id="storage"></a>Erstellen einer neuen Firma von Speicherplatz für Ihre Protokolle ##

Obwohl Sie ein vorhandenes Speicherkonto für Ihre Protokolle verwenden können, erstellen wir ein neues Speicherkonto, das Taste Tresor Protokollen vorbehalten sein wird. Zur Vereinfachung für Wenn dies später angeben müssen können wir die Details in einer Variablen namens **sa**speichern.

Für weitere Verwaltung zu vereinfachen auch verwenden derselben Ressourcengruppe als wir, die unsere Key Tresor enthält. [Erste Schritte-Lernprogramm](key-vault-get-started.md)diese Ressourcengruppe heißt **ContosoResourceGroup** , und wir uns weiterhin den Ostasien Speicherort verwenden. Ersetzen Sie diese Werte für Ihre eigenen, sofern zutreffend:

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name ContosoKeyVaultLogs -Type Standard_LRS -Location 'East Asia'


>[AZURE.NOTE]  Wenn Sie ein vorhandenes Speicherkonto verwenden möchten, als den Key Tresor darf Sie einziges Abonnement verwenden und müssen sie das Modell zur Bereitstellung von Ressourcenmanager, statt die Bereitstellung Klassisch verwenden.

## <a name="a-ididentifyaidentify-the-key-vault-for-your-logs"></a><a id="identify"></a>Identifizieren Sie die wichtigsten Tresor für Ihre Protokolle ##

In unseren Schritte beim Abrufen Lernprogramm wurde unser Name Key Tresor **ContosoKeyVault**,, damit wir verwenden diese Namen, und speichern die Details in einer Variablen namens **kv**weiterhin wird:

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <a name="a-idenableaenable-logging"></a><a id="enable"></a>Aktivieren der Protokollierung ##

Zum Aktivieren der Protokollierung für Schlüssel Tresor verwenden wir des Cmdlets Set-AzureRmDiagnosticSetting zusammen mit der Variablen, die wir für unsere Speicher Neukunde und unsere Key Tresor erstellt haben. Wir werden auch Festlegen der **-aktiviert** auf **$true** kennzeichnen, und legen Sie die Kategorie auf AuditEvent. (die einzige Kategorie für die Taste Tresor Protokollierung):


    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

Die Ausgabe für diesen umfasst:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


Dies bestätigt, dass die Protokollierung jetzt für den Key Tresor, Informationen zu Ihrem Speicherkonto speichern aktiviert ist.

Optional können Sie auch Aufbewahrungsrichtlinie für Ihre Protokolle festlegen, sodass ältere Protokolle automatisch gelöscht werden. Beispielsweise Aufbewahrungsrichtlinie mit **RetentionEnabled -** Kennzeichnung auf **$true** und zum Festlegen Sie **Der RetentionInDays -** Parameter **90** , sodass die Protokolle, die älter als 90 Tage automatisch gelöscht werden.

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

Was wird protokolliert:

- Alle authentifizierte REST-API Anfragen werden der fehlgeschlagene Anfragen als Ergebnis Zugriffsberechtigungen, Systemfehler oder fehlerhafte Anfragen enthält, protokolliert.
- Vorgänge für die Taste Vaulting selbst, wozu auch erstellen, löschen, Einstellung Key Tresor-Richtlinien, und Aktualisieren der wichtigsten Tresor Attribute wie tags.
- Vorgänge auf-Taste und vertrauliche Informationen in die wichtigsten Tresor, wozu auch erstellen, ändern oder löschen diese Schlüssel oder Kennwörter; JOIN-Operationen wie anmelden, stellen Sie sicher, verschlüsseln Sie, entschlüsseln Sie, umbrechen Sie und Entpacken Sie Tasten, erhalten Sie Kennwörter, Liste Tasten und vertraulichen Daten und deren Versionen.
- Nicht authentifizierte Anfragen, die eine 401-Antwort führen. Beispielsweise Besprechungsanfragen, die verfügen nicht über ein Token Person oder falsch formatierte oder abgelaufen sind, oder ein ungültiges Token.  


## <a name="a-idaccessaaccess-your-logs"></a><a id="access"></a>Zugriff auf Ihre Protokolle ##

Key Tresor Protokolle werden im Container **Einsichten-Protokolle-AuditEvent** im Speicherkonto gespeichert, die Sie zur Verfügung gestellt. Um alle Blobs in diesem Container aufzulisten, geben Sie Folgendes ein:

    Get-AzureStorageBlob -Container 'insights-logs-auditevent' -Context $sa.Context

Das Ergebnis sieht in etwa wie folgt:

**Container Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**


**Namen**

**----**

**ResourceId = / ABONNEMENTS/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/Anbieter/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**

**ResourceId = / ABONNEMENTS/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/Anbieter/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**

** ResourceId = / ABONNEMENTS/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/Anbieter/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json***


Wie Sie aus dieser Ausgabe sehen können, führen Sie die Blobs eine Benennungskonvention: **ResourceId =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/h =<hour>/m =<minute>/filename.json**

Verwenden Sie die Werte für Datum und Uhrzeit UTC.

Da das gleiche Speicherkonto zum Sammeln von Protokolle für mehrere Ressourcen verwendet werden kann, ist die vollständige Ressourcen-ID in den Namen Blob sehr hilfreich, für den Zugriff oder Laden Sie einfach die Blobs, die Sie benötigen. Aber bevor wir dies tun, können wir zum Herunterladen der alle Blobs behandeln.

Erstellen Sie zuerst einen Ordner, um die Blobs herunterladen. Beispiel:

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

Klicken Sie dann erhalten Sie eine Liste der alle Blobs:  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

Leiten Sie diese Liste über 'Get-AzureStorageBlobContent' die Blobs in unseren Zielordner herunterladen:

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

Beim Ausführen dieser zweiten Befehl, die **/** Trennzeichen in die Namen Blob erstellen eine vollständige Ordnerstruktur unter Zielordner, und diese Struktur zum Herunterladen und speichern die Blobs als Dateien verwendet werden.

Zum Herunterladen Selektives Blobs verwenden Sie Platzhalterzeichen. Beispiel:

- Wenn Sie mehrere Key Depots haben und Protokolle für nur eine Key Tresor herunterladen möchten, mit dem Namen CONTOSOKEYVAULT3:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3

- Wenn Sie mehrere Ressourcengruppen und möchten, laden Sie die Protokolle für nur eine Ressourcengruppe verfügen, verwenden Sie `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'

- Wenn Sie alle Protokolle für den Monat Januar 2016 herunterladen möchten, verwenden Sie `-Blob '*/year=2016/m=01/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

Sie nun können betrachtet, was in den Protokollen wird gestartet. Aber vor dem Verschieben auf, zwei weitere Parameter für Get-AzureRmDiagnosticSetting, die Sie kennen müssen:

- Den Status des diagnoseeinstellungen für die Ressource ein Key Tresor Abfragen:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`

- So deaktivieren Sie die Protokollierung für die Ressource ein Key Tresor`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`


## <a name="a-idinterpretainterpret-your-key-vault-logs"></a><a id="interpret"></a>Bei der Interpretation der Schlüssel Tresor Protokolle ##

Einzelne Blobs werden als Text formatiert als JSON Blob gespeichert. Dies ist ein Beispiel für Log Eintrag Ausführung `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


Die folgende Tabelle listet die Feldnamen und eine Beschreibung an.


| Feldname        | Beschreibung |
| ------------- |-------------|
| Zeit      | Datum und Uhrzeit (UTC).|
| resourceId      | Azure Ressourcenmanager Ressourcen-ID. Für Schlüssel Tresor Protokolle ist dies immer die Taste Tresor Ressourcen-ID.|
| operationName      | Name des Vorgangs, wie in der folgenden Tabelle beschrieben.|
| operationVersion      | Dies ist die REST-API-Version, die vom Client angeforderten.|
| Kategorie      | Für Schlüssel Tresor Protokolle ist AuditEvent der einzelner Wert verfügbar.|
| ResultType-Wert      | Ergebnis der REST-API anfordern.|
| resultSignature      | HTTP-Status.|
| resultDescription     | Zusätzliche Beschreibung über das Ergebnis, wenn verfügbar.|
| durationMs      | Zeit die REST-API Anfrage, in Millisekunden. Dies schließt nicht die Netzwerkwartezeit, damit die Zeit, die Sie auf dem Client messen diesmal möglicherweise nicht übereinstimmt.|
| callerIpAddress      | IP-Adresse des Clients, die die Anforderung vorgenommen hat.|
| correlationId      | Eine optionale GUID, die der Client übergeben kann, um den clientseitigen Protokollen mit Service clientseitigen (Taste Tresor) Protokolle zu koordinieren.|
| Identität      | Identität aus dem Token, das angezeigt wurde, wenn Sie die REST-API Anforderung vornehmen. Dies ist in der Regel ein "Benutzer", "Service Tilgungsanteile" oder eine Kombination wie im Fall einer Anforderung infolge eines Azure PowerShell-Cmdlet "Benutzer + AppId".|
| Eigenschaften      | In diesem Feld wird andere Informationen basierend auf den Vorgang (OperationName) enthalten. In den meisten Fällen enthält Clientinformationen (die Useragent übergebene Zeichenfolge vom Client), die genauen REST-API Anfrage-URI, und klicken Sie auf HTTP-Statuscode. Darüber hinaus, wenn ein Objekt als Ergebnis einer Anforderung (z. B. KeyCreate oder VaultGet) zurückgegeben wird enthält auch der Schlüssel URI (wie "Id"), Tresor URI oder geheim URI er.|




Die Feldwerte **OperationName** sind im ObjectVerb-Format. Beispiel:

- Alle wichtigen Tresor Vorgänge weisen die ' Tresor`<action>`' formatieren möchten, wie `VaultGet` und `VaultCreate`.

- Alle wichtigen Vorgänge weisen die ' Schlüssel`<action>`' formatieren möchten, wie `KeySign` und `KeyList`.

- Alle geheimen Vorgänge weisen die ' geheim`<action>`' formatieren möchten, wie `SecretGet` und `SecretListVersions`.

Die folgende Tabelle listet die OperationName und die entsprechenden REST-API-Befehl.

| operationName        | REST API-Befehl |
| ------------- |-------------|
| Authentifizierung      | Über Azure Active Directory-Endpunkt|
| VaultGet      | [Erhalten von Informationen zu einem Key Tresor](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx)|
| VaultPut      | [Erstellen oder Aktualisieren eines Key Tresor](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx)|
| VaultDelete      | [Löschen eines Key Tresor](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx)|
| VaultPatch      | [Aktualisieren einer Key Tresor](https://msdn.microsoft.com/library/azure/mt620025.aspx)|
| VaultList      | [Liste aller wichtigen Depots in einer Ressourcengruppe](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx)|
| KeyCreate      | [Erstellen Sie einen Schlüssel](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx)|
| KeyGet      | [Erhalten von Informationen zu einem Schlüssel](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx)|
| KeyImport      | [Importieren eines Schlüssels in einem Tresor](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx)|
| KeyBackup      | [Sichern einer Taste](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).|
| KeyDelete      | [Löschen Sie einen Schlüssel](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx)|
| KeyRestore      | [Stellen Sie einen Schlüssel wieder her.](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx)|
| KeySign      | [Melden Sie sich mit einem Schlüssel](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx)|
| KeyVerify      | [Vergewissern Sie sich mit einem Schlüssel](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx)|
| KeyWrap      | [Umbrechen von einem Schlüssel](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx)|
| KeyUnwrap      | [Entpacken Sie einen Schlüssel](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx)|
| KeyEncrypt      | [Verschlüsseln mit einem Schlüssel](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx)|
| KeyDecrypt      | [Mit einem Schlüssel entschlüsseln](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx)|
| KeyUpdate      | [Aktualisieren Sie einen Schlüssel](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx)|
| KeyList      | [Liste der Tasten in einem Tresor](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx)|
| KeyListVersions      | [Liste der Versionen eines Schlüssels](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx)|
| SecretSet      | [Erstellen Sie einen geheimen Schlüssel](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx)|
| SecretGet      | [Abrufen von geheimen](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx)|
| SecretUpdate      | [Aktualisieren einer geheim](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx)|
| SecretDelete      | [Löschen Sie einen geheimen Schlüssel](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx)|
| SecretList      | [Liste vertrauliche Informationen in einem Tresor](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx)|
| SecretListVersions      | [Liste der Versionen für einen geheimen](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx)|




## <a name="a-idnextanext-steps"></a><a id="next"></a>Nächste Schritte ##

Ein, der in einer Webanwendung Azure-Taste Tresor verwendet wird, finden Sie unter [Verwenden Azure Schlüssel Tresor aus einer Web-Anwendung](key-vault-use-from-web-application.md).

Verweise für die Programmierung, finden Sie unter [Azure Schlüssel Tresor developer's Guide](key-vault-developers-guide.md).

Eine Liste der Azure PowerShell 1.0-Cmdlets für die Taste Tresor Azure finden Sie unter [Azure Schlüssel Tresor Cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx).

Ein Lernprogramm Key Drehung und Log Überwachung mit Azure-Taste Tresor finden Sie unter [Einrichten Schlüssel Tresor mit durchgehende Key Drehung und Überwachung](key-vault-key-rotation-log-monitoring.md).
