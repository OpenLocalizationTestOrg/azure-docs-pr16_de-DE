<properties
   pageTitle="Azure Ressourcenmanager Anforderung Grenzwerte | Microsoft Azure"
   description="Beschreibt, wie mit begrenzungsebene mit Azure Ressourcenmanager Anforderungen, wenn Abonnement Grenzwerte erreicht haben."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="tomfitz"/>

# <a name="throttling-resource-manager-requests"></a>Ressourcenmanager Anfragen begrenzungsebene

Für jedes Abonnement und Mandanten Ressourcenmanager Grenzwerte Anfragen auf 15.000 pro Stunde lesen und Schreiben von Besprechungsanfragen in 1.200 pro Stunde. Wenn die Anwendung oder das Skript diese Grenzwerte erreicht haben, müssen Sie schränken Sie Ihre Anfragen. In diesem Thema wird gezeigt, wie die verbleibenden Anfragen bestimmt werden kann, die Sie vor der Grenzwert erreicht haben und wie zu reagieren, wenn Sie die erreicht haben.

Wenn Sie den Höchstwert erreichen, erhalten Sie den HTTP-Statuscode **429 zu viele Anfragen**.

Die Anzahl der Anfragen ist entweder Ihres Abonnements oder Ihrem Mandanten beschränkt. Wenn mehrere, gleichzeitige Applikationen Ausführen einer Anforderung in Ihrem Abonnement diese Applications-Anfragen werden hinzugefügt zusammen, um die Anzahl der verbleibenden Anfragen zu bestimmen.

Abonnement ausgelegte Anfragen werden der beteiligt sein, Ihre Abonnement-Id, wie etwa beim Abrufen der Ressource übergeben in Ihrem Abonnement gruppiert. Mandanten ausgelegte Besprechungsanfragen enthalten nicht Ihre Abonnement-Id, wie etwa gültige Azure Speicherorte abrufen.

## <a name="remaining-requests"></a>Verbleibende Anfragen

Sie können die Anzahl der verbleibenden Anfragen festlegen, indem Antwort-Header. Jede Anforderung umfasst Werte für die Anzahl der verbleibenden Lese- und Schreibzugriff Anforderungen an. Die folgende Tabelle beschreibt die Antwort Überschriften, die Sie für diese Werte prüfen können:

| Antwort-header | Beschreibung |
| --------------- | ----------- |
| x-ms-ratelimit-Remaining-Subscription-Reads | Abonnement ausgelegte liest verbleibende |
| x-ms-ratelimit-Remaining-Subscription-Writes | Abonnement ausgelegte schreibt verbleibende |
| x-ms-ratelimit-Remaining-Tenant-Reads | Mandanten ausgelegte liest verbleibende |
| x-ms-ratelimit-Remaining-Tenant-Writes | Mandanten ausgelegte schreibt verbleibende |
| x-ms-ratelimit-Remaining-Subscription-Resource-Requests | Abonnement ausgelegte Ressource Typ Anfragen verbleibende.<br /><br />Dieser Wert Kopfzeile wird nur zurückgegeben, wenn ein Dienst für die standardmäßige maximale überschrieben hat. Ressourcenmanager fügt diesen Wert anstelle der Abonnement liest oder schreibt hinzu. |
| x-ms-ratelimit-Remaining-Subscription-Resource-Entities-Read | Abonnement ausgelegte Ressource Typ Websitesammlung Anfragen verbleibende.<br /><br />Dieser Wert Kopfzeile wird nur zurückgegeben, wenn ein Dienst für die standardmäßige maximale überschrieben hat. Dieser Wert stellt die Anzahl der verbleibenden Websitesammlung Anfragen (Ressourcen). |
| x-ms-ratelimit-Remaining-Tenant-Resource-Requests | Mandanten ausgelegte Ressource Typ Anfragen verbleibende.<br /><br />Diese Kopfzeile wird nur für Anfragen Mandanten Ebene hinzugefügt, und nur dann, wenn ein Dienst wurde für die standardmäßige maximale überschrieben. Ressourcenmanager fügt diesen Wert anstelle der Mandanten liest oder schreibt hinzu. |
| x-ms-ratelimit-Remaining-Tenant-Resource-Entities-Read | Mandanten ausgelegte Ressource Typ Websitesammlung Anfragen verbleibende.<br /><br />Diese Kopfzeile wird nur für Anfragen Mandanten Ebene hinzugefügt, und nur dann, wenn ein Dienst wurde für die standardmäßige maximale überschrieben. |

## <a name="retrieving-the-header-values"></a>Zum Abrufen der Werte für die Kopfzeile

Abrufen von diesen Werten Kopfzeile im Code oder Skript unterscheidet sich nicht als jeder Wert, der Kopfzeile abrufen. 

Beispielsweise rufen in **c#**, Sie den Kopfzeile Wert aus einem **HttpWebResponse** Objekt mit dem Namen **Antwort** mit den folgenden Code:

    response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)

In **PowerShell**rufen Sie den Headerwert aus einem aufrufen WebRequest Vorgang.

    $r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
    $r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
    
Oder möchten Sie die verbleibenden Anfragen für das Debuggen, anzeigen, können Sie angeben, wenn die **-Debuggen** Parameter auf der **PowerShell** -Cmdlet.

    Get-AzureRmResourceGroup -Debug
    
Gibt eine große Datenmenge, einschließlich des folgenden Antwort Werts zurück:

    ...
    DEBUG: ============================ HTTP RESPONSE ============================

    Status Code:
    OK

    Headers:
    Pragma                        : no-cache
    x-ms-ratelimit-remaining-subscription-reads: 14999
    ...

In **Azure CLI**rufen Sie den Kopfzeile-Wert mithilfe der Option ausführlicheren aus.

    azure group list -vv --json

Gibt eine große Datenmenge, einschließlich des folgenden Objekts zurück:

    ...
    silly: returnObject
    {
      "statusCode": 200,
      "header": {
        "cache-control": "no-cache",
        "pragma": "no-cache",
        "content-type": "application/json; charset=utf-8",
        "expires": "-1",
        "x-ms-ratelimit-remaining-subscription-reads": "14998",
        ...

## <a name="waiting-before-sending-next-request"></a>Warten vor dem Senden der nächsten Anforderung

Wenn Sie die Anfrage Beschränkung erreicht haben, gibt Ressourcenmanager des **429** HTTP Statuscodes und einer **"Wiederholen" nach** Wert in der Kopfzeile. Die **' Wiederholen ' nach** Wert gibt die Anzahl der Sekunden Ihrer Anwendung warten sollte (oder Sleep) vor dem Senden der nächsten Anforderung. Wenn Sie eine Besprechungsanfrage senden, bevor der Wert "Wiederholen" abgelaufen ist, die Anforderung wird nicht verarbeitet und ein neuen Wert für "Wiederholen" zurückgegeben.
