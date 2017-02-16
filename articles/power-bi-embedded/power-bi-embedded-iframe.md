<properties
   pageTitle="So verwenden Sie Power BI eingebettete mit weiteren | Microsoft Azure"
   description="Informationen Sie zum Verwenden von Power BI eingebettete mit weiteren "
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="how-to-use-power-bi-embedded-with-rest"></a>So verwenden Sie Power BI eingebettete mit weiteren


## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>Power BI eingebettete: Zweck und Neuigkeiten für
Übersicht über Power BI eingebettete wird beschrieben, in der offiziellen [eingebettete von Power BI-Website](https://azure.microsoft.com/services/power-bi-embedded/), aber werfen einen Blick, bevor wir die Details zu dessen Verwendung mit REST zu verschaffen.

Es ist eigentlich ziemlich einfach. Ein ISV häufig die dynamischen datenvisualisierungen [Power](https://powerbi.microsoft.com) BI in ihrer eigenen Anwendung als Bausteine für die Benutzeroberfläche verwenden möchte.

Sie wissen, Power BI-Berichten oder Kacheln in Ihre Webseite einbetten ist jedoch bereits möglich, ohne das Power BI eingebettete Azure-Dienst mithilfe der **Power BI-API**. Wenn Sie Ihre Berichte in der gleichen Organisation freigeben möchten, können Sie die Berichte mit Azure AD-Authentifizierung einbetten. Der Benutzer, der die Berichte Ansichten muss über eigene Azure AD-Konto anmelden. Wenn Sie Ihre Berichte für alle Benutzer (einschließlich externe Benutzer) freigeben möchten, können Sie einfach mit anonymem Zugriff einbetten.

Sie sehen, diese einfach einbetten Lösung, aber nicht ganz entsprechen die Anforderungen der Anwendung ISV.
Die meisten ISV Applikationen müssen die Daten für ihre eigenen Kunden, nicht unbedingt die Benutzer in der eigenen Organisation verfügbar ist. Beispielsweise, wenn Sie einige Dienst für Unternehmen A und Unternehmen B festlegen, auftreten Benutzer im Unternehmen A nur Daten für das eigene Unternehmen a Mehrere Mandanten ist d. h., für die Übermittlung erforderlich.

ISV-Anwendung bietet möglicherweise darüber hinaus eine eigene Authentifizierungsmethoden, z. B. Formulare Authentifizierung, Standardauthentifizierung usw... Klicken Sie dann muss die Einbetten von Lösung mit dieser bestehenden Authentifizierungsmethoden sicheres zusammenarbeiten. Es ist auch für Benutzer können diese ISV Applikationen ohne zusätzliche Erwerb verwenden oder zur Lizenzierung von Power BI-Abonnement erforderlich.

 **Power BI eingebettete** exakt folgenden Arten von ISV Szenarien dient. Also nun wir die schnelle Einführung in die Weise verfügen, erfahren Sie in einige details

Sie können .NET \(c#) oder Node.js SDK, zu Ihrer Anwendung mit Power BI eingebettete ganz einfach zu erstellen. Aber in diesem Artikel wird erläutert über HTTP Bewegung \(einschließlich AuthN) der Power BI ohne SDKs. Grundlegendes zu diesem Fluss aus, Sie können Ihre Anwendung **in einer beliebigen Programmiersprache**erstellen und Sie können verstanden tief das wesentliche des Power BI eingebettete.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Erstellen von Power BI-Arbeitsbereich Sammlung, und Get-Zugriffstaste \(Provisioning)
Power BI eingebettete ist eine der Azure-Dienste. Nur der ISV, Azure-Portal verwendet, wird für Nutzungsgebühren belastet \(pro stündlich Benutzer Sitzung), und der Benutzer, der den Bericht Ansichten ist nicht belastet oder sogar ein Azure-Abonnement erforderlich.
Vor Beginn der unsere Anwendungsentwicklung, müssen wir die **Power BI-Arbeitsbereich Websitesammlung** mithilfe von Azure-Portal erstellen.

Jeder Arbeitsbereich der Power BI eingebettete ist der Arbeitsbereich für die einzelnen Kunden (Mandant), und wir können viele Arbeitsbereiche in jeder Arbeitsbereich-Auflistung hinzufügen. In jeder Auflistung Arbeitsbereich wird die gleichen Zugriffstaste verwendet. In Effekt, wird die Arbeitsbereich-Auflistung die Begrenzungslinie Sicherheit für Power BI eingebettete.

![](media\power-bi-embedded-iframe\create-workspace.png)

Wenn wir mit dem Erstellen der Arbeitsbereich-Auflistung fertig sind, kopieren Sie die Zugriffstaste aus Azure-Portal aus.

![](media\power-bi-embedded-iframe\copy-access-key.png)

> [AZURE.NOTE] Wir können auch Bereitstellung die Arbeitsbereich-Auflistung und Zugriffstaste über REST-API abrufen. Weitere Informationen finden Sie unter [Power BI Ressource Anbieter-APIs](https://msdn.microsoft.com/library/azure/mt712306.aspx).

## <a name="create-pbix-file-with-power-bi-desktop"></a>Erstellen Sie .pbix Datei mit Power BI-Desktop
Als Nächstes müssen wir die Verbindung von Daten und Berichten eingebettet werden erstellen.
Für diese Aufgabe gibt es keine Programmierung oder Coderessource. Wir verwenden nur Power BI-Desktop.
In diesem Artikel wird nicht wir die Details zur Verwendung von Power BI-Desktop durchgehen. Wenn Sie einige hier Hilfe benötigen, finden Sie unter [Erste Schritte mit Power BI-Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/). Unsere beispielsweise wird nur der [Retail Analyse Stichprobe](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/)verwendet.

![](media\power-bi-embedded-iframe\power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>Erstellen eines Power BI-Arbeitsbereichs

Nachdem Sie nun die Bereitstellung alle fertig ist, kann's losgehen Erstellen eines Kunden Arbeitsbereichs in die Sammlung Arbeitsbereich über REST-APIs. Die folgenden HTTP-Beitrag anfordern (REST), ist in unseren vorhandenen Arbeitsbereich Auflistung den neuen Arbeitsbereich erstellen. In diesem Beispiel ist der Name des Arbeitsbereichs Websitesammlung **Mypbiapp**.
Wir werden nur Zugriffstaste, die wir zuvor kopiert, als **AppKey**festlegen. Es ist sehr einfach Authentifizierung!

**HTTP-Anforderung**

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-Antwort**

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

Die zurückgegebenen **WorkspaceId** wird für die folgenden weiteren API-Aufrufe verwendet. Dieser Wert muss unsere Anwendung beibehalten werden.

## <a name="import-pbix-file-into-the-workspace"></a>.Pbix Importdatei in den Arbeitsbereich
Jeder Arbeitsbereich kann eine einzelne Power BI-Desktop-Datei mit einem Dataset hosten \(einschließlich der Einstellungen für die Datenquelle) und Berichte. Wir können unsere .pbix-Datei importieren, zu dem Arbeitsbereich, wie im folgenden Code dargestellt. Wie Sie sehen können, können wir die Binärdatei mit MIME Multipart in http .pbix-Datei hochladen.

Die Uri-Fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** ist die WorkspaceId und Abfrage Parameter **DatasetDisplayName** ist der Name der Dataset zu erstellen. Erstellte Dataset enthält alle Daten im Zusammenhang Elemente in .pbix Datei wird beispielsweise als importierten Daten, den Mauszeiger zur Datenquelle, usw...

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

Diese Aufgabe importieren möglicherweise eine Weile ausgeführt. Wenn Sie fertig sind, kann unsere Anwendung den Aufgabenstatus mit Import-Id Fragen. In diesem Beispiel ist die Id importieren **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

Die folgenden Fragen Status, die mittels dieser Id importieren:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Wenn der Vorgang nicht abgeschlossen ist, könnte die HTTP-Antwort wie folgt aus:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

Wenn der Vorgang abgeschlossen ist, könnte die HTTP-Antwort Weitere wie folgt:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a>Datenquellen-Konnektivität \(und mehrere Mandanten Daten)
Während der Elemente in der Datei .pbix fast alle in unseren Arbeitsbereich importiert wurden, sind nicht die Anmeldeinformationen für Datenquellen. Daher kann keine beim **DirectQuery-Modus**verwenden, der eingebettete Bericht ordnungsgemäß angezeigt werden. Aber beim **Import-Modus**verwenden, können wir den Bericht mithilfe der vorhandenen importierten Daten anzeigen. In diesem Fall müssen wir die Anmeldeinformationen mithilfe der folgenden Schritte aus, über die restlichen Anrufe festlegen.

Zunächst muss die Gateway-Datenquelle abgerufen werden. Wir wissen, dass die Dataset- **Id** , die zuvor zurückgegebene Id ist.

**HTTP-Anforderung**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-Antwort**

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

Verwenden die zurückgegebenen Gateway-Id und die Datenquelle Id \(finden Sie im vorherigen **GatewayId** und **Id** im Ergebnis zurückgegebenen), können wir die Anmeldeinformationen für diese Datenquelle wie folgt ändern:

**HTTP-Anforderung**

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

**HTTP-Antwort**

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

In der Herstellung können wir die verschiedenen Verbindungszeichenfolge für jeden Arbeitsbereich mit REST-API einstellen. \(d.h., können wir die Datenbank für jeden Kunden getrennt wird.)

Die folgenden besteht die Verbindungszeichenfolge der Datenquelle über REST zu ändern.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Oder, können wir in Power BI eingebettete Sicherheitsstufe Zeile und können wir die Daten für jeden Benutzer in einem Bericht zu trennen. As a result, können wir Bereitstellung von einzelnen Kundenbericht mit demselben .pbix \(UI, usw..) und unterschiedlichen Datenquellen.

> [AZURE.NOTE] Wenn Sie **Importieren Modus** statt **DirectQuery-Modus**verwenden, gibt es Möglichkeit keine zum Aktualisieren von Datenmodellen über-API. Und lokale Datenquellen über Power BI-Gateway nicht in Power BI eingebettete noch nicht unterstützt. Allerdings Präsentationsarten wirklich Achten Sie auf den [Power BI-Blog](https://powerbi.microsoft.com/blog/) für was neu ist und was in Zukunft kommt frei.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Anmelde- und Hostinganbieter (einbetten) von Berichten in unsere Webseite

In der vorherigen REST-API können wir als Autorisierung Kopfzeile Zugriffstaste **AppKey** selbst verwenden. Da diese Aufrufe Back-End-serverseitig behandelt werden können, ist es sicherer.

Aber wenn wir den Bericht in unsere Webseite einbetten, diese Art von Informationen zur Sicherheit würde behandelt werden mithilfe von JavaScript \(Front-End). Klicken Sie dann muss der Wert von Autorisierung Header geschützt werden. Wenn unsere Zugriffstaste durch einen bösartiger Benutzer oder bösartiger Code erkannt wird, können sie alle Vorgänge, die mit diesem Schlüssel aufrufen.

Wenn wir den Bericht in unsere Webseite einbetten, müssen wir das berechnete Token statt Zugriffstaste **AppKey**verwenden. Unsere Anwendung muss OAuth Json Web Token erstellen \(JWT) der aus der Ansprüche und die berechnete digitale Signatur besteht. Wie unten dargestellt, wird diese OAuth JWT Token codierte Zeichenfolge Punkt getrennt.

![](media\power-bi-embedded-iframe\oauth-jwt.png)

Zuerst müssen wir den Eingabewert Vorbereiten der später angemeldet ist. Dieser Wert ist die Zeichenfolge base64 Url-codierte (rfc4648) die folgenden Json, und diese werden durch den Punkt getrennt \(.) Zeichen. Erläutern Sie später so die Berichts-Id erhalten.

> [AZURE.NOTE] Wenn wir Zeile Ebene Sicherheit Datensatzebene mit Power BI eingebettete verwenden möchten, müssen wir **Benutzernamen** und **Rollen** in der Ansprüche angeben.

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

Als Nächstes müssen wir die base64-codierte Zeichenfolge HMAC erstellen \(die Signatur) mit SHA256-Algorithmus. Diese signierte Eingabewert ist die vorhergehende Zeichenfolge.

Letzte, müssen wir die eingegebene Wert und Signatur Zeichenfolge mit Punkt kombinieren \(.) Zeichen. Die abgeschlossene Zeichenfolge ist das app-Token für das Einbetten von Bericht. Selbst wenn das app Token von einem bösartiger Benutzer gefunden wird, kann nicht die ursprüngliche Zugriffstaste ausgegeben. Diese app Token läuft schnell ab.

Hier ein Beispiel PHP für die folgenden Schritte aus:

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is the apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-the-report-into-the-web-page"></a>Schließlich den Bericht in die Webseite einbetten

Für das Einbetten von unserem Berichts, müssen wir Abrufen der Url einbetten und Berichts **-ID-** mithilfe der folgenden REST-API.

**HTTP-Anforderung**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-Antwort**

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

Wir können den Bericht in unsere Web app mit dem vorherigen app Token einbetten.
Wenn wir im nächsten Beispielcode betrachten, ist der ehemalige Teil der im vorherigen Beispiel identisch. In der letzte Teil dieses Beispiel zeigt die **EmbedUrl** \(finden Sie unter dem vorhergehenden Ergebnis) im Iframe, und Posten Sie das app-Token ist in den Iframe.

> [AZURE.NOTE] Sie müssen den Bericht-Id-Wert in eine eigene ändern. Außerdem ist aufgrund eines Fehlers in unseren Content Management-System, das Iframe-Tag in der Stichprobe Code Literal schreibgeschützt. Entfernen Sie den Enden versehene Text aus dem Tag, wenn Sie kopieren, und fügen Sie den Code Stichprobe.

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

Und so sieht unsere Ergebnis:

![](media\power-bi-embedded-iframe\view-report.png)

Zu diesem Zeitpunkt zeigt Power BI eingebettete nur den Bericht im Iframe. Aber Achten Sie auf den [Power BI-Blog](). Verbesserte zukünftigen könnten neuen clientseitigen APIs verwenden, die werden lassen Sie uns senden die Informationen in den Iframe sowie erhalten von Informationen. Interessante privates!


## <a name="see-also"></a>Siehe auch
- [Anmeldung und Autorisierung in Power BI eingebettete](power-bi-embedded-app-token-flow.md)
