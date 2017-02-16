<properties
    pageTitle="Log Analytics melden Sie sich suchen REST-API | Microsoft Azure"
    description="Dieses Handbuch bietet ein einfaches Lernprogramm, beschreibt, wie können Sie die Suche Log Analytics REST-API in Vorgänge Management Suite (OMS), und es enthält Beispiele, bei die gezeigt, wie die Befehle verwendet werden."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="log-analytics-log-search-rest-api"></a>Log Analytics Log Suche REST-API

Dieses Handbuch bietet ein einfaches Lernprogramm, beschreibt, wie Sie können die Log Analytics Suche REST-API in Vorgänge Management Suite (OMS) und bietet Beispiele, bei die gezeigt, wie die Befehle verwendet werden. Einige der in den Beispielen in diesem Artikel verweisen auf Betrieb Einblicken, welche den Namen der vorherigen Version der Log Analytics ist.

## <a name="overview-of-the-log-search-rest-api"></a>Übersicht über die Suche Log REST-API

Die Log Analytics Suche REST-API RESTful ist und über die Azure Ressourcenmanager-API zugegriffen werden kann. In diesem Dokument finden Sie Beispiele, ist die-API über die [ARMClient](https://github.com/projectkudu/ARMClient), ein open-Source-Tool Befehlszeile zugegriffen, die vereinfacht die Azure Ressourcenmanager-API aufrufen. Die Verwendung von ARMClient und PowerShell ist eine der viele Optionen, um die Log Analytics suchen-API zugreifen. Eine weitere Möglichkeit besteht darin, das Azure PowerShell-Modul für OperationalInsights verwenden, wozu auch die Cmdlets für den Zugriff auf Suchen. Mit diesen Tools können Sie die Rest Azure Ressourcenmanager API, damit Anrufe an OMS Arbeitsbereiche und Ausführen von Befehlen suchen, darin enthaltenen nutzen. Die API wird Suchergebnisse, um Sie im JSON-Format ausgegeben ermöglicht es Ihnen, die Suchergebnisse programmgesteuert auf verschiedene Weise verwendet.

Ressourcenmanager Azure kann über eine [Bibliothek für .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) ebenso wie eine [REST-API](https://msdn.microsoft.com/library/azure/mt163658.aspx)verwendet werden. Überprüfen Sie die verknüpfte Webseiten Weitere Informationen ein.

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Grundlegende Log Analytics Suche REST-API Lernprogramm

### <a name="to-use-the-arm-client"></a>Verwenden der Cloud-Client

1. Installieren Sie [Chocolatey](https://chocolatey.org/), welche einer geöffneten Datenquellen-Paket-Manager für Windows ist. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator an, und führen Sie dann den folgenden Befehl aus:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```

2. Installieren Sie die ARMClient, indem Sie den folgenden Befehl ausführen:

    ```
    choco install armclient
    ```

### <a name="to-perform-a-simple-search-using-the-armclient"></a>Zum Ausführen einer einfachen Suche mithilfe der ARMClient

1. Anmelden bei Ihrem Konto Microsoft oder weiterentwickelt:

    ```
    armclient login
    ```

    Eine erfolgreiche Anmeldung Listet alle Abonnements mit dem angegebenen Konto verknüpft:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```

2. Abrufen der Arbeitsbereiche Vorgänge Management Suite an:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Alle Arbeitsbereiche, die mit dem Abonnement verknüpft Ausgabe ein erfolgreicher Get-Anruf:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. Erstellen Sie Ihre Suche Variable:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Suchen Sie Ihre neue Variable für die Suche verwenden:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Melden Sie sich Analytics Suche REST-API Bezug Beispiele
In den folgenden Beispielen zeigen Sie die Verwendung der Suche-API.

### <a name="search---actionread"></a>Suche - Aktion/gelesen

**Beispiel-Url:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Anfordern:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
Die folgende Tabelle beschreibt die Eigenschaften, die verfügbar sind.

|**Eigenschaft**|**Beschreibung**|
|---|---|
|Nach oben|Die maximale Anzahl von Ergebnissen zurück.|
|Hervorheben|Vor und nach Parameter, verwendet in der Regel zum Hervorheben von übereinstimmenden Felder enthält|
|Pre|Die angegebene Zeichenfolge in die übereinstimmenden Felder als Präfix voran.|
|Bereitstellen|Fügt die angegebene Zeichenfolge an die übereinstimmenden Felder.|
|Abfrage|Die Suchabfrage zum Sammeln und Ergebnisse zurückgeben.|
|Starten|Anfang des Zeitfensters Ergebnisse aus gefunden werden sollen.|
|Ende|Ende des Zeitfensters Ergebnisse aus gefunden werden sollen.|


**Antwort:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Suchen / {ID} - Aktion/gelesen

**Anfordern des Inhalts einer Suche gespeichert:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

>[AZURE.NOTE] Wenn Sie die Suche mit dem Status 'Ausstehend' zurückgibt, kann die abrufen klicken Sie dann auf die aktualisierten Ergebnisse durch diese API vorgenommen werden. Nach 6 min wird das Ergebnis der Suche aus dem Cache abgelegt werden und HTTP nicht mehr angezeigt werden zurückgegeben. Wenn die ursprüngliche Suchabfrage sofort einen "Erfolgreichen" Status zurückgegeben wird, wird es im Cache, die bewirken, dass diese API zurückzugebenden HTTP nicht mehr angezeigt, wenn abgefragt nicht hinzugefügt werden. Der Inhalt des ein Ergebnis HTTP 200 werden in das gleiche Format wie die ursprüngliche Suchabfrage nur mit aktualisierten Werte.

### <a name="saved-searches---rest-only"></a>Gespeicherte Suchen – nur REST

**Liste der gespeicherten Suchen anfordern:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Unterstützten Methoden: GET setzen löschen.

Auflistungen unterstützt: Abrufen

Die folgende Tabelle beschreibt die Eigenschaften, die verfügbar sind.

|Eigenschaft|Beschreibung|
|---|---|
|ID|Den eindeutigen Bezeichner enthält.|
|ETag|**Für den Patch erforderlich ist**. Bei jedem Schreibvorgang aktualisiert durch Server. Wert muss gleich dem aktuellen gespeicherten Wert oder ' *' aktualisieren. 409 für alten/ungültige Werte zurückgegeben.|
|Properties.Query|**Erforderlich**. Die Suchabfrage.|
|properties.displayName|**Erforderlich**. Der Benutzername des definierten Anzeigen der Abfrage. Wenn Sie als eine Ressource Azure erstellt wird, wäre dies eine Kategorie.|
|Properties.Category|**Erforderlich**. Die benutzerdefinierte Kategorie der Abfrage. Wenn als Azure Ressource erstellt eine Kategorie wäre.|

>[AZURE.NOTE] Die Log Analytics suchen-API gibt derzeit Benutzern erstellte gespeicherte Suchen, wenn für die gespeicherte Suche in einem Arbeitsbereich hin abgefragt. Die API wird keine gespeicherte Suchen, die zu diesem Zeitpunkt von Lösungen bereitgestellten zurück. Dieses Feature wird zu einem späteren Zeitpunkt hinzugefügt werden.

### <a name="create-saved-searches"></a>Erstellen von gespeicherten Suchen

**Anfordern:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="delete-saved-searches"></a>Löschen gespeichert Suchbegriffe

**Anfordern:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Aktualisierung gespeichert Suchbegriffe

 **Anfordern:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metadaten - nur JSON

Hier ist eine Möglichkeit, die Felder für alle Typen von Log für die in dem Arbeitsbereich erfassten Daten angezeigt werden. Beispielsweise, wenn Sie möchten, dass Sie wissen, ob der Ereignistyp ein Feld mit dem Namen Computer hat, ist Klicken Sie dann dies eine Möglichkeit zum Nachschlagen und bestätigen.

**Anfrage für Felder:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Antwort:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

Die folgende Tabelle beschreibt die Eigenschaften, die verfügbar sind.

|**Eigenschaft**|**Beschreibung**|
|---|---|
|Namen|Feldname.|
|displayName|Der Anzeigename des Felds.|
|Typ|Die Art der Wert des Felds.|
|facetable|Kombination von aktuellen 'indiziert', ' gespeicherte ' und 'Vertriebsstrategie' Eigenschaften.|
|Anzeigen|Aktuelle 'anzeigen'-Eigenschaft. True, wenn das Feld Suchen angezeigt wird.|
|ownerType|Verringert in nur Dateitypen, die zu Onboarded IP Adressen gehören.|


## <a name="optional-parameters"></a>Optionale Parameter
Die folgende Informationen werden optionale Parameter zur Verfügung.

### <a name="highlighting"></a>Hervorhebung

Der Parameter "Hervorheben" ist ein optionaler Parameter, die Sie zum Anfordern des Teilsystems Suche verwenden möglicherweise eine Reihe von Datenpunkten in seine Antwort einbeziehen.

Diese Marker stehen für den Beginn und Beenden von hervorgehobenem Text, der die Konditionen in Ihrer Suchabfrage bereitgestellten entspricht.
Sie können die Start- und Endzeit Markierungen angeben, die bei der Suche zum Umbrechen von des hervorgehobenen Ausdrucks verwendet wird.

**Beispiel für Suchabfrage**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Beispiel für Ergebnis:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Beachten Sie, dass das Ergebnis oben eine Fehlermeldung enthält, die mit dem Präfix und angefügt wurden.

## <a name="computer-groups"></a>Computergruppen

Computergruppen sind spezielle gespeicherte Suchen, die eine Reihe von Computern zurückgeben.  Sie können eine Computergruppe in anderen Abfragen zum Einschränken der Ergebnisse auf den Computern in der Gruppe verwenden.  Eine Computergruppe wird als eine gespeicherte Suche mit einer Kategorie Gruppe mit einem Wert des Computers implementiert.

Es folgt eine Stichprobe Antwort für eine Gruppe.

    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
        }],
    "Version": 1
    }

### <a name="retrieving-computer-groups"></a>Abrufen von Computergruppen

Verwenden Sie die Get-Methode für die Gruppen-ID zum Abrufen einer Computergruppe.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Erstellen oder Aktualisieren einer Computergruppe

Verwenden Sie die sich Methode eine eindeutige ID suchen gespeichert zum Erstellen einer neuen Computergruppe. Wenn Sie eine vorhandene Gruppe-ID für die Computer verwenden, werden, dass eine geändert werden. Wenn Sie eine Gruppe in der Verwaltungskonsole OMS erstellt haben, wird die ID in der Gruppe und Namen erstellt.

Die Abfrage verwendet die Definition der Gruppe muss eine Gruppe von Computern für die Gruppe ordnungsgemäß zurückgeben.  Es wird empfohlen, dass Sie Ihre Abfrage mit *Beenden | Verschiedene Computer* um sicherzustellen, dass die richtigen Daten zurückgegeben werden.

Die Definition der gespeicherten Suche muss eine Kategorie mit dem Namen Gruppe mit einem Wert des Computers für die Suche als Computergruppe klassifiziert werden beinhalten.

    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson

### <a name="deleting-computer-groups"></a>Löschen von Computergruppen

Verwenden Sie die Delete-Methode für die Gruppen-ID zum Löschen einer Computergruppe an.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Log Suchbegriffe](log-analytics-log-searches.md) , zum Erstellen von Abfragen mithilfe von benutzerdefinierten Felder für die Kriterien.
