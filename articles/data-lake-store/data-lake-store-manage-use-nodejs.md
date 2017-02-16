<properties 
   pageTitle="Erste Schritte mit Azure Lake Datenspeicher mithilfe von Azure SDK für Node.js | Microsoft Azure"
   description="Erfahren Sie, wie Node.js für die Arbeit mit Lake Datenspeicher Konten und das Dateisystem verwenden." 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a>Erste Schritte mit Azure Lake Datenspeicher mithilfe von Azure SDK für Node.js

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)


Informationen Sie zum Verwenden der Azure SDK für Node.js erstellen Sie ein Konto Azure Lake Datenspeicher und einfache Operationen beispielsweise wie Erstellen von Ordnern, hoch- und Herunterladen von Datendateien oder Löschen von Ihrem Konto usw.. Weitere Informationen zu Lake Datenspeicher finden Sie unter [Übersicht der dem Datenspeicher](data-lake-store-overview.md). Unterstützt derzeit das SDK

  *  **Node.js Version: 0.10.0 oder höher**
  *  **REST-API Version für Konto: 2015-10-01-Vorschau**
  *  **REST-API Version für Dateisystem: 2015-10-01-Vorschau**

## <a name="prerequisites"></a>Erforderliche Komponenten

Vorbemerkung in diesem Artikel müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).

- **Erstellen einer Azure-Active Directory-Anwendung**. Die Anwendung Lake Datenspeicher mit Azure AD-authentifizieren mithilfe Sie die Azure AD-Anwendung. Es gibt verschiedene Ansätze mit Azure AD Authentifizierung die **Endbenutzer** oder **Dienst - Authentifizierung**sind. Anleitungen und Weitere Informationen zum Authentifizieren finden Sie unter [Authentifizieren mit dem Datenspeicher verwenden Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="how-to-install"></a>So installieren Sie

```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a>Authentifizieren Sie mit Azure Active Directory

Die folgenden Codeausschnitte zeigen zwei getrennten zum Authentifizieren mit Lake Datenspeicher mit Azure AD wird. Ausführliche Informationen zu auf verschiedene Methoden für die Authentifizierung mit Lake Datenspeicher verwenden finden Sie unter [Authentifizieren mit dem Datenspeicher verwenden Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

Der folgenden Codeausschnitt erfordert auch Eingaben wie Azure AD-Domänennamen, Client-ID für einen Azure AD-app usw. aus. Alle diese Details können aus einer Azure AD-, die Sie müssen erstellten Anwendung, abgerufen werden, die Details der obigen Link auch enthalten sind.

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-store-clients"></a>Erstellen der Lake Datenspeicher-Clients

```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a>Erstellen Sie ein Konto Lake Datenspeicher

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlsacct';
var location = 'eastus2';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location
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

## <a name="create-a-file-with-content"></a>Erstellen Sie eine Datei mit Inhalten
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var fileToCreate = '/myfolder/myfile.txt';
var options = {
  streamContents: new Buffer('some string content')
}

filesystemClient.fileSystem.listFileStatus(accountName, fileToCreate, options, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    // no result is returned, only a 201 response for success.
    console.log('response is: ' + util.inspect(response, {depth: null}));
  }
});
```

## <a name="get-a-list-of-files-and-folders"></a>Abrufen einer Liste von Dateien und Ordnern

```javascript
var util = require('util');
var accountName = 'testadlsacct';
var pathToEnumerate = '/myfolder';
filesystemClient.fileSystem.listFileStatus(accountName, pathToEnumerate, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Siehe auch

- [Microsoft Azure SDK für Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK für Node.js - Sees Analytics Datenverwaltung](https://www.npmjs.com/package/azure-arm-datalake-analytics)
