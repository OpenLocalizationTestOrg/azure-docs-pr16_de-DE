<properties
    pageTitle="Verwenden Sie zum Sichern und Wiederherstellen von App-Dienst apps REST"
    description="Informationen Sie zum Verwenden von REST-API-Aufrufe zum Sichern und Wiederherstellen einer app im App-Verwaltungsdienst Azure"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a>Verwenden Sie zum Sichern und Wiederherstellen von App-Dienst apps REST

> [AZURE.SELECTOR]
- [PowerShell](../app-service/app-service-powershell-backup.md)
- [REST-API](websites-csm-backup.md)

[App-Dienst apps](https://azure.microsoft.com/services/app-service/web/) können als Blobs in Azure-Speicher gesichert werden. Die Sicherung kann auch des app Datenbanken enthalten. Wenn die app jemals versehentlich gelöscht wird oder wenn die app auf eine frühere Version wiederhergestellt werden muss, können sie über eine vorhandene Sicherungskopie wiederhergestellt werden. Sicherungskopien in geeigneten Abständen geplant werden können, oder Sicherungen zu einem beliebigen Zeitpunkt bei Bedarf erfolgen können.

In diesem Artikel wird erläutert, wie Sichern und Wiederherstellen einer app mit REST API Anforderungen. Wenn Sie zum Erstellen und Verwalten von app Sicherungskopien grafisch über das Azure Portal möchten, finden Sie unter [Sichern von einer Web-app in Azure-App-Verwaltungsdienst](web-sites-backup.md)

<a name="gettingstarted"></a>
## <a name="getting-started"></a>Erste Schritte
Um REST Anfragen zu senden, müssen Sie Ihre app **Name**, **Ressourcengruppe**und **Abonnement-Id**bekannt sind. Diese Informationen kann gefunden werden, indem Sie auf der app in **Der App Service** -Karte vorausgesetzt des [Azure-Portal](https://portal.azure.com). Für die Beispiele in diesem Artikel werden wir die Website **backuprestoreapiexamples.azurewebsites.net**konfigurieren. In der Ressourcengruppe Standard-Web-WestUS gespeichert ist, und klicken Sie auf ein Abonnement mit der ID 00001111-2222-3333-4444-555566667777 ausgeführt wird.

![Beispiel für Website Informationen][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>
## <a name="backup-and-restore-rest-api"></a>Sichern und Wiederherstellen REST-API
Jetzt werden mehrere Beispiele zum Verwenden Sie die REST-API zum Sichern und Wiederherstellen einer app behandelt. Jedes Beispiel enthält eine URL und HTTP-Anforderungstexts. Die URL für die Stichprobe enthält in geschweifte Klammern ein, beispielsweise {Abonnement-Id} eingeschlossen Platzhaltern. Ersetzen Sie die Platzhalter mit den entsprechenden Informationen für Ihre app ein. Als Referenz ist hier eine Erläuterung der einzelnen Platzhalter, die in den Beispiel-URLs angezeigt wird.

* Abonnement-Id – das Azure Abonnement, enthält die app-ID
* Ressourcen-Gruppenname – Name der Ressourcengruppe, enthält der app
* Name – Name der app
* Sicherung-Id – die Sicherung app-ID

Die vollständige Dokumentation der API, einschließlich verschiedene optionale Parameter, die in der HTTP-Anforderung enthalten sein können finden Sie unter der [Azure-Explorers](https://resources.azure.com/).

<a name="backup-on-demand"></a>
## <a name="backup-an-app-on-demand"></a>Sichern einer app bei Bedarf
Um eine app sofort zu sichern, senden Sie eine Besprechungsanfrage **Beitrag** zu **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.

Sieht die URL wie unsere Beispiel-Website verwenden. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**

Geben Sie ein JSON-Objekt in den Textkörper der Anforderung um anzugeben, welche Speicher-Konto zu verwenden, um die Sicherung speichern. Das JSON-Objekt müssen eine Eigenschaft namens **StorageAccountUrl**, die einer [URL SAS](../storage/storage-dotnet-shared-access-signature-part-1.md) Schreibzugriff auf den Azure-Speicher Container, das Sicherung Blob erteilen enthält. Wenn Sie Ihre Datenbanken sichern möchten, müssen Sie auch eine Liste mit den Namen, Datentypen und Verbindungszeichenfolgen der zu sichernden Datenbanken angeben.

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

Eine Sicherungskopie der app beginnt sofort gestartet, wenn die Anforderung empfangen wird. Die Sicherung möglicherweise einen langen Zeit in Anspruch nehmen. HTTP-Antwort enthält eine ID, die Sie verwenden, können in eine andere Anforderung, um den Status der Sicherung. Hier ist ein Beispiel für den Hauptteil der HTTP-Antwort zu unseren Sicherung Besprechungsanfrage ein.

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

>[AZURE.NOTE] Fehlermeldungen finden Sie in der Eigenschaft Log HTTP-Antwort.

<a name="schedule-automatic-backups"></a>
## <a name="schedule-automatic-backups"></a>Planen der automatischen Sicherung
Zusätzlich zu Sichern einer app aus, bei Bedarf, können Sie auch eine Sicherungskopie von automatischen Vorgängen planen.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Einrichten eines neuen Zeitplans für die automatischen Sicherung
Zum Einrichten einer Sicherung Terminplan **setzen** Anfrage zum Senden **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.

Hier sind, wie die URL für die Website unsere Beispiel aussieht. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**

Hauptteil der Anforderung müssen ein JSON-Objekt, der die Sicherungskopie der Konfiguration angibt. Hier ist ein Beispiel für alle erforderlichen Parameter ein.

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set to true to enable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

In diesem Beispiel wird die app alle sieben Tage automatisch gesichert werden konfiguriert. Der Parameter **FrequencyInterval** und **FrequencyUnit** nicht trennen bestimmen, wie oft die Sicherungen erfolgen. Gültige Werte für **FrequencyUnit** sind **Stunde** und **Tag**. Stellen Sie FrequencyInterval beispielsweise um eine app alle 12 Stunden sichern, bis 12 und FrequencyUnit Stunde bei.

Alte Sicherungskopien werden automatisch aus dem Speicherkonto entfernt. Sie können steuern, wie ALT die Sicherungskopien werden können, indem Sie das Festlegen des Parameters **RetentionPeriodInDays** . Müssen Sie mindestens eine Sicherung gespeichert, unabhängig davon, wie die alte **KeepAtLeastOneBackup** True festgelegt ist, wenn Sie immer möchten.

### <a name="get-the-automatic-backup-schedule"></a>Abrufen des Zeitplans für die automatischen Sicherung
Um eine app für die Sicherungskopie der Konfiguration zu erhalten, senden Sie eine **POST** -Anforderung zur URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.

Die URL für die Website unsere Beispiel ist **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>
## <a name="get-the-status-of-a-backup"></a>Abrufen des Status einer Sicherung
Je nachdem wie groß die app ist kann eine Sicherung eine Weile dauern. Sicherungskopien möglicherweise auch fehlschlägt, Timeout, oder teilweise erfolgreich ist. Um den Status aller einer app Sicherungskopien anzuzeigen, senden Sie eine **GET** -Anforderung zur URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.

Um den Status einer bestimmten Sicherung anzuzeigen, senden Sie eine GET-Anforderung an die URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**aus.

Hier sind, wie die URL für die Website unsere Beispiel aussieht. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1**

Der Nachrichtentext der Antwort enthält ähnlich wie in diesem Beispiel wird ein JSON-Objekt.

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

Der Status einer Sicherung ist eine aufgezählten ein. Hier sind alle möglichen Status aus.

* 0 – in Bearbeitung: die Sicherung gestartet wurde, aber noch nicht abgeschlossen.
* 1 – Fehler: Die Sicherung wurde nicht erfolgreich.
* 2 – erfolgreich: Die Sicherung wurde erfolgreich abgeschlossen.
* 3 – TimedOut: Die Sicherung wurde nicht abgeschlossen, Zeitpunkt und wurde abgebrochen.
* 4 – erstellt: Die Sicherung Anforderung ist in der Warteschlange jedoch noch nicht begonnen wurde.
* 5 – übersprungen: Die Sicherung ist nicht aufgrund eines Projektplans zu viele Sicherungskopien auslösen fortgeschritten.
* 6 – teilweise erfolgreich: Die Sicherung erfolgreich war, aber einige Dateien wurden nicht gesichert, da sie nicht gelesen werden konnten. In diesem Fall in der Regel daran, dass eine ausschließlich Sperren auf die Dateien platziert wurde.
* 7 – DeleteInProgress: Die Sicherung angefordert wurde gelöscht, aber noch nicht gelöscht.
* 8 – DeleteFailed: Die Sicherung konnte nicht gelöscht werden. Dies kann vorkommen, da die SAS-URL, die zum Erstellen der Sicherung verwendet wurde abgelaufen ist.
* 9 – gelöscht: Die Sicherung wurde erfolgreich gelöscht.

<a name="restore-app"></a>
## <a name="restore-an-app-from-a-backup"></a>Wiederherstellen einer app aus einer Sicherung
Wenn Ihre app gelöscht wurden, oder wenn Sie Ihre app zu einer früheren Version zurückkehren möchten, können Sie die app aus einer Sicherung wiederherstellen. Um eine Wiederherstellung aufzurufen, senden Sie eine **POST** -Anforderung an die URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.

Hier sind, wie die URL für die Website unsere Beispiel aussieht. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1/Restore**

Senden Sie in den Textbereich der Besprechungsanfrage ein JSON-Objekt, das die Eigenschaften für die Wiederherstellung enthält. Hier ein Beispiel, die alle erforderliche Eigenschaften enthält:

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL to storage container containing your website backup
    }
}
```

### <a name="restore-to-a-new-app"></a>Stellen Sie eine neue app wieder her
Manchmal möchten Sie möglicherweise eine neue app zu erstellen, wenn Sie eine Sicherungskopie, anstatt zu überschreiben einer bereits vorhandenen app wiederherstellen. Hierzu ändern Sie die Anfrage-URL auf neue app verweisen, die Sie erstellen möchten, und die Eigenschaft **Überschreiben** in das JSON auf **false**.

<a name="delete-app-backup"></a>
## <a name="delete-an-app-backup"></a>Löschen einer app-Sicherung
Wenn Sie eine Sicherungskopie löschen möchten, senden Sie eine Besprechungsanfrage **Löschen** , um die URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Hier sind, wie die URL für die Website unsere Beispiel aussieht. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1**

<a name="manage-sas-url"></a>
## <a name="manage-a-backups-sas-url"></a>Verwalten einer Sicherung SAS-URL
Azure-App-Dienst versucht, löschen die Sicherung von Azure-Speicher mithilfe der SAS-URL, der bereitgestellt wurde, wenn die Sicherung erstellt wurde. Wenn Sie diese URL SAS nicht mehr gültig ist, kann die Sicherung über die REST-API gelöscht werden. Sie können jedoch die SAS URL zugeordnet eine Sicherungskopie per **POST** -Anforderung zur URL aktualisieren **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Hier sind, wie die URL für die Website unsere Beispiel aussieht. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1/List**

Senden Sie in den Textbereich der Besprechungsanfrage ein JSON-Objekt, das die neuen SAS-URL enthält. Hier ist ein Beispiel für ein.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

>[AZURE.NOTE] Aus Gründen der Sicherheit wird eine Sicherungskopie zugeordnete SAS-URL nicht zurückgegeben, wenn eine GET-Anforderung für eine bestimmte Sicherung zu senden. Wenn Sie eine Sicherungskopie zugeordnete SAS URL anzeigen möchten, senden Sie eine POST-Anforderung an dieselbe URL oben. Fügen Sie ein leeres JSON-Objekt, in den Nachrichtentext anfordern. Die Antwort vom Server enthält alle, dass die Sicherung Informationen, einschließlich der zugehörigen SAS-URL.

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
