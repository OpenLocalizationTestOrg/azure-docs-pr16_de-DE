<properties
   pageTitle="Verwalten von Azure Daten dem Analytics mithilfe von Azure SDK für Node.js | Azure"
   description="Erfahren Sie, wie Daten dem Analytics-Konten, Datenquellen, Aufträge und Benutzer per Azure SDK für Node.js verwalten"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="manage-azure-data-lake-analytics-using-azure-sdk-for-nodejs"></a>Verwalten von Azure Daten dem Analytics Azure SDK für Node.js verwenden


[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Azure SDK Node.js kann für die Verwaltung von Azure Daten dem Analytics-Konten, Aufträge und Kataloge verwendet werden. Um Management Thema mit anderen Tools angezeigt wird, klicken Sie auf der Registerkarte wählen Sie oben.

Bis zu diesem Zeitpunkt unterstützt:

  *  **Node.js Version: 0.10.0 oder höher**
  *  **REST-API Version für Konto: 2015-10-01-Vorschau**
  *  **REST-API-Version für den Katalog: 2015-10-01-Vorschau**
  *  **REST-API Version für Position: 2016-03-20-Vorschau**

## <a name="features"></a>Features

- Account-Management: erstellen, abrufen, Liste, aktualisieren und löschen.
- Verwaltung der beruflichen Position: übermitteln, erhalten, Liste, Abbrechen.
- Katalog Management: Abrufen, Liste, erstellen (geheimen Daten), aktualisieren (Kennwörter), löschen (Kennwörter).

## <a name="how-to-install"></a>So installieren Sie

```bash
npm install azure-arm-datalake-analytics
```

## <a name="authenticate-using-azure-active-directory"></a>Authentifizieren Sie mit Azure Active Directory

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-analytics-client"></a>Erstellen Sie den Daten dem Analytics-client

```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var acccountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## <a name="create-a-data-lake-analytics-account"></a>Erstellen eines Daten dem Analytics-Kontos

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created to create
// a Data Lake Analytics account. See the Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-jobs"></a>Abrufen einer Liste der Aufträge

```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-databases-in-the-data-lake-analytics-catalog"></a>Eine Liste der Datenbanken im Katalog Analytics dem Daten abrufen
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Siehe auch

- [Microsoft Azure SDK für Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK für Node.js - Sees Datenspeicher Management](https://github.com/Azure/azure-sdk-for-node/tree/autorest/lib/services/dataLake.Store)
