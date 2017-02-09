<properties
 pageTitle="Ausgehende Scheduler-Authentifizierung"
 description="Ausgehende Authentifizierung Scheduler"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/15/2016"
 ms.author="deli"/>

# <a name="scheduler-outbound-authentication"></a>Ausgehende Scheduler-Authentifizierung

Möglicherweise müssen Scheduler Aufträge Dienste hervorheben, die Authentifizierung erforderlich ist. Auf diese Weise kann ein bezeichnet Dienst festlegen, wenn Sie der Auftrag Scheduler seine Ressourcen zugreifen kann. Einige dieser Dienste gehören anderen Azure Services, Salesforce.com, Facebook und sichere benutzerdefinierten Websites.

## <a name="adding-and-removing-authentication"></a>Hinzufügen und Entfernen von Authentifizierung

Authentifizierung mit einem Projekt Scheduler ist einfach: Fügen Sie ein JSON untergeordnetes Element hinzufügen `authentication` zu den `request` Element beim Erstellen oder Aktualisieren eines Auftrags. Kennwörter übergeben an den Dienst Scheduler in einer Anforderung bereitstellen, PATCH oder Beitrag – als Teil der `authentication` Objekt – nie in Antworten zurückgegeben werden. In Antworten, vertraulichen Informationen auf festgelegt ist null oder möglicherweise müssen Sie ein öffentlichen Token, die authentifizierte Entität darstellt.

Zum Entfernen der Authentifizierung setzen oder PATCH für den Auftrag explizit, Festlegen der `authentication` Objekt auf null. Alle Authentifizierungseigenschaften wieder in die Antwort wird nicht angezeigt.

Aktuell nur unterstützten Authentifizierungsarten sind die `ClientCertificate` Modell (für die Verwendung von SSL/TLS-Client-Zertifikate), die `Basic` Modell (für Standardauthentifizierung), und die `ActiveDirectoryOAuth` Modell (für Active Directory-OAuth Authentifizierung.)

## <a name="request-body-for-clientcertificate-authentication"></a>Textkörper für ClientCertificate Authentifizierung anfordern

Beim Hinzufügen von Authentifizierung mithilfe der `ClientCertificate` modellieren, geben Sie die folgenden zusätzlichen Elemente im Hauptteil Anforderung.  

|Element|Beschreibung|
|:---|:---|
|_Authentifizierung (übergeordnetes Element)_|Für die Verwendung eines SSL-Client-Zertifikats Authentifizierungsobjekt.|
|_Typ_|Erforderlich. Art der Authentifizierung. Der Wert muss für SSL-Client-Zertifikate `ClientCertificate`.|
|_PFX_|Erforderlich. Base64-codierte Inhalt der PFX-Datei.|
|_Kennwort_|Erforderlich. Kennwort für die PFX-Datei zugreifen.|


## <a name="response-body-for-clientcertificate-authentication"></a>Textkörper der Antwort für die ClientCertificate-Authentifizierung

Wenn eine Besprechungsanfrage mit Informationen Authentifizierung gesendet wird, enthält die Antwort die folgenden Authentifizierung-bezogene Elemente aus.

|Element |Beschreibung |
|:--|:--|
|_Authentifizierung (übergeordnetes Element)_ |Authentifizierungsobjekt für ein SSL-Client-Zertifikat verwenden.|
|_Typ_ |Art der Authentifizierung. Für SSL-Client-Zertifikate, ist der Wert `ClientCertificate`.|
|_certificateThumbprint_ |Der Fingerabdruck des Zertifikats.|
|_certificateSubjectName_ |Der Betreff distinguished Name des Zertifikats.|
|_certificateExpiration_ |Das Ablaufdatum des Zertifikats.|

## <a name="sample-rest-request-for-clientcertificate-authentication"></a>Beispiel für eine Anforderung für die Authentifizierung ClientCertificate REST

```
PUT https://management.azure.com/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-clientcertificate-authentication"></a>Beispiel für REST Antwort für die ClientCertificate-Authentifizierung

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="request-body-for-basic-authentication"></a>Anforderungstexts für Standardauthentifizierung

Beim Hinzufügen von Authentifizierung mithilfe der `Basic` modellieren, geben Sie die folgenden zusätzlichen Elemente im Hauptteil Anforderung.

|Element|Beschreibung|
|:--|:--|
|_Authentifizierung (übergeordnetes Element)_ |Für die Verwendung von Standardauthentifizierung Authentifizierungsobjekt.|
|_Typ_ |Erforderlich. Art der Authentifizierung. Bei der Standardauthentifizierung der Wert muss `Basic`.|
|_Benutzername_ |Erforderlich. Der Benutzername zum Authentifizieren.|
|_Kennwort_ |Erforderlich. Kennwort authentifiziert.|

## <a name="response-body-for-basic-authentication"></a>Textkörper der Antwort für die Standardauthentifizierung

Wenn eine Besprechungsanfrage mit Informationen Authentifizierung gesendet wird, enthält die Antwort die folgenden Authentifizierung-bezogene Elemente aus.

|Element|Beschreibung|
|:--|:--|
|_Authentifizierung (übergeordnetes Element)_ |Für die Verwendung von Standardauthentifizierung Authentifizierungsobjekt.|
|_Typ_ |Art der Authentifizierung. Für Standardauthentifizierung, ist der Wert `Basic`.|
|_Benutzername_ |Der authentifizierte Benutzername.|

## <a name="sample-rest-request-for-basic-authentication"></a>Beispiel für eine Anforderung für die Standardauthentifizierung REST

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-basic-authentication"></a>Beispiel für REST Antwort für die Standardauthentifizierung

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="request-body-for-activedirectoryoauth-authentication"></a>Textkörper für ActiveDirectoryOAuth Authentifizierung anfordern

Beim Hinzufügen von Authentifizierung mithilfe der `ActiveDirectoryOAuth` modellieren, geben Sie die folgenden zusätzlichen Elemente im Hauptteil Anforderung.

|Element |Beschreibung |
|:--|:--|
|_Authentifizierung (übergeordnetes Element)_ |Authentifizierungsobjekt für die Verwendung von ActiveDirectoryOAuth Authentifizierung.|
|_Typ_ |Erforderlich. Art der Authentifizierung. Der Wert muss für ActiveDirectoryOAuth Authentifizierung `ActiveDirectoryOAuth`.|
|_Mandanten_ |Erforderlich. Der Mandant Bezeichner für den Azure AD-Mandanten.|
|_Zielgruppe_ |Erforderlich. Dies ist auf https://management.core.windows.net/ festgelegt.|
|_clientId_ |Erforderlich. Geben Sie die Client-ID für Azure AD-Anwendung an.|
|_geheim_ |Erforderlich. Geheim des Clients, der das Token anfordert.|

### <a name="determining-your-tenant-identifier"></a>Bestimmen den Bezeichner Mandanten

Sie können den Mandanten Bezeichner für den Azure AD-Mandanten finden, indem Sie ausgeführt `Get-AzureAccount` in Azure PowerShell.

## <a name="response-body-for-activedirectoryoauth-authentication"></a>Antwort Textkörper für ActiveDirectoryOAuth-Authentifizierung

Wenn eine Besprechungsanfrage mit Informationen Authentifizierung gesendet wird, enthält die Antwort die folgenden Authentifizierung-bezogene Elemente aus.

|Element |Beschreibung |
|:--|:--|
|_Authentifizierung (übergeordnetes Element)_ |Authentifizierungsobjekt für die Verwendung von ActiveDirectoryOAuth Authentifizierung.|
|_Typ_ |Art der Authentifizierung. Für ActiveDirectoryOAuth Authentifizierung, ist der Wert `ActiveDirectoryOAuth`.|
|_Mandanten_ |Der Mandant Bezeichner für den Azure AD-Mandanten. |
|_Zielgruppe_ |Dies ist auf https://management.core.windows.net/ festgelegt.|
|_clientId_ |Die Client-ID für die Azure AD-Anwendung.|

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a>Beispiel für eine Anforderung REST für ActiveDirectoryOAuth-Authentifizierung

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a>Beispiel für REST Antwort für ActiveDirectoryOAuth-Authentifizierung

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "tenant":"microsoft.onmicrosoft.com",
               "audience":"https://management.core.windows.net/",
               "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type":"ActiveDirectoryOAuth"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "lastExecutionTime":"2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime":"2016-03-16T19:11:00Z",
         "executionCount":5,
         "failureCount":5,
         "faultedCount":1
      }
   }
}
```

## <a name="see-also"></a>Siehe auch


 [Was ist der Scheduler?](scheduler-intro.md)

 [Azure Scheduler Konzepte und Terminologie Entitätshierarchie](scheduler-concepts-terms.md)

 [Erste Schritte mit Scheduler Azure-Portal](scheduler-get-started-portal.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [Azure Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Bezug auf Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Azure Scheduler hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Grenzwerte für Azure Scheduler, Standardeinstellungen und Fehlercodes](scheduler-limits-defaults-errors.md)
