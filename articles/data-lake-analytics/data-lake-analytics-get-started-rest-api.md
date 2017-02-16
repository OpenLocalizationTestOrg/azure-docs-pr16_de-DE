<properties 
   pageTitle="Erste Schritte mit dem-Analytics Daten mithilfe von REST-API | Microsoft Azure" 
   description="Verwenden Sie zum Ausführen von Vorgängen auf Daten dem Analytics WebHDFS REST-APIs" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Erste Schritte mit Azure Daten dem Analytics REST-APIs verwenden

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Informationen Sie zum WebHDFS REST-APIs und Daten dem Analytics REST-APIs verwenden, um die Daten dem Analytics Konten, Aufträge und Katalog verwalten. 

## <a name="prerequisites"></a>Erforderliche Komponenten

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).

- **Erstellen einer Azure-Active Directory-Anwendung**. Sie verwenden Azure AD-Anwendung, um die Daten dem Analytics-Anwendung mit Azure AD-authentifizieren. Es gibt verschiedene Ansätze mit Azure AD Authentifizierung die **Endbenutzer** oder **Dienst - Authentifizierung**sind. Anweisungen und Weitere Informationen zum Authentifizieren finden Sie unter [Authentifizieren mit Daten dem Analytics Azure Active Directory verwenden](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).

- [Aufrollen](http://curl.haxx.se/). In diesem Artikel verwendet cURL, um zu veranschaulichen, wie REST-API Aufrufe für ein Konto Daten dem Analytics machen.

## <a name="authenticate-with-azure-active-directory"></a>Authentifizieren Sie mit Azure-Active Directory

Es gibt zwei Methoden zum Authentifizieren mit Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Endbenutzer-Authentifizierung (interaktiv)

Bei dieser Methode können Anwendung aufgefordert wird, den Benutzer sich anmelden und alle Vorgänge im Kontext des Benutzers ausgeführt werden. 

Führen Sie diese Schritte für die interaktive Authentifizierung aus:

1. Durchlaufen der Anwendung leiten Sie den Benutzer auf den folgenden URL ein:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<REDIRECT-URI > zur Verwendung in einer URL codiert werden muss. Verwenden Sie für https://localhost, also, `https%3A%2F%2Flocalhost`)

    Innerhalb dieses Lernprogramms können Platzhalterwerte in der URL oben ersetzen und fügen Sie ihn in der Adressleiste des Webbrowsers. Sie werden zum Authentifizieren mit Ihrer Azure Login weitergeleitet. Sobald Sie erfolgreich angemeldet haben, wird die Antwort in der Adressleiste des Browsers angezeigt. Die Antwort werden in folgendem Format:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Erfassen Sie den Autorisierungscode aus der Antwort ein. In diesem Lernprogramm können Sie den Autorisierungscode in der Adressleiste des Webbrowsers kopieren und übergeben Sie dieses an die POST-Anforderung der token Endpunkt, wie unten dargestellt:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] In diesem Fall die \<REDIRECT-URI > nicht codiert werden müssen.

3. Die Antwort ist ein JSON-Objekt, das eine Access-Token enthält (z. B. `"access_token": "<ACCESS_TOKEN>"`) und ein Token aktualisieren (z. B. `"refresh_token": "<REFRESH_TOKEN>"`). Ihrer Anwendung verwendet das Token Access beim Zugriff auf Azure Lake Datenspeicher und dem Aktualisieren Token auf ein anderes Access Token erhalten, wenn eine Access-Sicherheitstoken abläuft.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Wenn die Access-Sicherheitstoken abläuft, können Sie ein neues Access Token mit dem Token aktualisieren anfordern, wie unten dargestellt:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Weitere Informationen über interaktive Benutzerauthentifizierung finden Sie unter [Autorisierungscode Fluss erteilen](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Dienst-Authentifizierung (nicht interaktiv)

Verwenden diese Methode, stellt die Anwendung eine eigene Anmeldeinformationen ein, um die Vorgänge ausführen. Für dieses stellen Sie eine POST-Anforderung wie unten dargestellt aus: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Die Ausgabe dieser Anforderung wird ein Autorisierungstoken enthalten (gekennzeichnet durch `access-token` in der Ausgabe unten), die Sie später mit Ihre Anrufe REST-API übergeben werden. Speichern Sie dieses Authentifizierungstoken in eine Textdatei aus. Sie benötigen diese später in diesem Artikel.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

In diesem Artikel wird verwendet, die **nicht interaktiven** Ansatz. Weitere Informationen zu nicht interaktiven (Dienst-Aufrufe) finden Sie unter [Service Dienst Anrufe mit Anmeldeinformationen](https://msdn.microsoft.com/library/azure/dn645543.aspx).
## <a name="create-a-data-lake-analytics-account"></a>Erstellen eines Daten dem Analytics-Kontos

Bevor Sie ein Konto Daten dem Analytics erstellen können, müssen Sie eine Azure Ressourcengruppe und ein Konto Lake Datenspeicher erstellen.  Finden Sie unter [dem Datenspeicher Konto erstellen](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

Der folgende Curl-Befehl veranschaulicht, wie ein Konto zu erstellen:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Ersetzen Sie \< `REDACTED` \> mit dem Autorisierungstoken \< `AzureSubscriptionID` \> mit Ihrem Abonnement-ID, \< `AzureResourceGroupName` \> mit einem vorhandenen Ressourcengruppe Azure-Namen und \< `NewAzureDataLakeAnalyticsAccountName` \> unter einem neuen Namen für die Daten dem Analytics-Konto. Der Anforderungsnutzlast für diesen Befehl enthalten ist, in der **CreateDatalakeAnalyticsAccountRequest.json** -Datei, die für bereitgestellt wird die `-d` oben angegebenen Parameter. Der Inhalt der Datei input.json in etwa Folgendes aus:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Liste Daten Lake Analytics-Konten in einem Abonnement

Der folgende Curl-Befehl werden Konten in einem Abonnement aufgelistet:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Ersetzen Sie \< `REDACTED` \> mit dem Autorisierungstoken \< `AzureSubscriptionID` \> mit Ihrem Abonnement-ID an. Die Ausgabe ähnelt:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Erhalten von Informationen zu einem Daten dem Analytics-Konto

Der folgende Curl-Befehl veranschaulicht, wie eine Kontoinformationen zu erhalten:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Ersetzen Sie \< `REDACTED` \> mit dem Autorisierungstoken \< `AzureSubscriptionID` \> mit Ihrem Abonnement-ID, \< `AzureResourceGroupName` \> mit einem vorhandenen Ressourcengruppe Azure-Namen und \< `DataLakeAnalyticsAccountName` \> mit dem Namen eines vorhandenen Daten dem Analytics Kontos. Die Ausgabe ähnelt:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Liste Lake Datenspeicher eines Daten dem Analytics-Kontos

Der folgende Curl-Befehl werden Lake Datenspeicher eines Kontos aufgelistet:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Ersetzen Sie \< `REDACTED` \> mit dem Autorisierungstoken \< `AzureSubscriptionID` \> mit Ihrem Abonnement-ID, \< `AzureResourceGroupName` \> mit einem vorhandenen Ressourcengruppe Azure-Namen und \< `DataLakeAnalyticsAccountName` \> mit dem Namen eines vorhandenen Daten dem Analytics Kontos. Die Ausgabe ähnelt:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>U-SQL-Aufträge senden

Der folgende Curl-Befehl veranschaulicht, wie einen U-SQL-Auftrag zu senden:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Ersetzen Sie \< `REDACTED` \> mit dem Autorisierungstoken \< `DataLakeAnalyticsAccountName` \> mit dem Namen eines vorhandenen Daten dem Analytics Kontos. Der Anforderungsnutzlast für diesen Befehl enthalten ist, in der **SubmitADLAJob.json** -Datei, die für bereitgestellt wird die `-d` oben angegebenen Parameter. Der Inhalt der Datei input.json in etwa Folgendes aus:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

Die Ausgabe ähnelt:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>Liste U-SQL-Aufträge

Der folgende Curl-Befehl zeigt wie U-SQL-Aufträge aufgelistet:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Ersetzen Sie \< `REDACTED` \> mit dem Autorisierungstoken und \< `DataLakeAnalyticsAccountName` \> mit dem Namen eines vorhandenen Daten dem Analytics Kontos. 


Die Ausgabe ähnelt:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Abrufen von Katalogelementen

Der folgende Befehl aus Curl veranschaulicht, wie die Datenbanken aus dem Katalog zu erhalten:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

Die Ausgabe ähnelt:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Siehe auch

- Eine komplexe Abfrage finden Sie unter [Analysieren Website Protokolle Azure Daten dem Analytics verwenden](data-lake-analytics-analyze-weblogs.md).
- Um anzufangen U SQL Anwendungen entwickeln, finden Sie unter [entwickeln U-SQL-Skripts, die mit dem Datentools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL finden Sie unter [Erste Schritte mit Azure Daten dem Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md).
- Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Daten dem Analytics verwenden Azure-Portal](data-lake-analytics-manage-use-portal.md).
- Um einen Überblick über die Daten dem Analytics zu gelangen, finden Sie unter [Übersicht über die Azure Daten dem Analytics](data-lake-analytics-overview.md).
- Um die gleichen Lernprogramm mit anderen Tools angezeigt wird, klicken Sie auf die Registerkarte Selektoren am oberen Rand der Seite.
- Zum Melden von Diagnoseinformationen finden Sie unter [Zugreifen auf Diagnose Protokolle für Azure Daten dem Analytics](data-lake-analytics-diagnostic-logs.md)
