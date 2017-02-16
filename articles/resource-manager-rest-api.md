<properties
   pageTitle="Ressourcenmanager REST APIs | Microsoft Azure"
   description="Übersicht über die Ressourcenmanager REST APIs Authentifizierung und die Verwendung der Beispiele"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="navale;tomfitz;"/>
   
# <a name="resource-manager-rest-apis"></a>Ressourcenmanager REST-APIs

> [AZURE.SELECTOR]
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Portal](./azure-portal/resource-group-portal.md) 
- [REST-API](resource-manager-rest-api.md)

Hinter jeder Anruf zu Azure Ressourcenmanager, hinter jeder Vorlage bereitgestellten hinter jeder konfigurierten Speicher-Konto besteht eine oder mehrere Anrufe an die Ressourcenmanager Azure REST-API. In diesem Thema Arbeitszeit diesen APIs und wie Sie diese ohne alle SDK gar aufrufen können. Dies kann sehr hilfreich, wenn Sie Vollzugriff auf alle Anfragen Azure oder wenn das SDK für die gewünschte Sprache nicht verfügbar ist oder nicht unterstützen, Vorgänge, dass Sie ausführen möchten.

In diesem Artikel wird nicht Navigieren in jeder API, die in Azure verfügbar gemacht werden, aber lieber einige beispielhaft, wie Sie fortfahren und verbinden zu, verwenden. Wenn Sie die Grundlagen verstehen, dann können Sie fortfahren und lesen den [Azure Ressourcenmanager REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn790568.aspx) finden ausführliche Informationen zu den Rest der APIs verwenden.

## <a name="authentication"></a>Authentifizierung
Authentifizierung für Cloud wird durch Azure Active Directory (AD) behandelt. Und an eine beliebige API Sie zunächst mit Azure AD erhalten Sie eine Authentifizierungstoken authentifizieren müssen herstellen, die Sie können an jeder Anforderung übergeben, klicken Sie auf. Wie wir reinen Anruf direkt an die REST-APIs beschreiben, werden wir auch setzen voraus, dass nicht mit einem Kennwort normalen Benutzernamen authentifizieren, wo möglicherweise ein pop-Einrichtung-Bildschirm zur Eingabe aufgefordert, Benutzername und Kennwort und vielleicht sogar anderen Authentifizierungsmechanismen in zwei Faktor Authentifizierungsszenarien verwendet, werden soll. Daher erstellen wir was aufgerufen wird eine Azure AD-Anwendung und einem Dienst Tilgungsanteile, die verwendet werden, melden Sie sich mit. Aber denken Sie daran, dass mehrere Authentifizierungsverfahren zur Unterstützung von Azure AD und aller Folien einsetzen, um die Authentifizierungstoken abzurufen, die wir für nachfolgende API Anforderungen müssen.
Führen Sie Schritt-für-Schritt-Anleitung [Erstellen Azure AD-Anwendung und Dienst Prinzip](./resource-group-create-service-principal-portal.md) .

### <a name="generating-an-access-token"></a>Generieren einer Access-Token 
Authentifizierung über Azure AD erfolgt durch an Azure AD Standardtelefonnummern am login.microsoftonline.com. Damit Sie authentifizieren müssen die folgende Informationen:

* Azure AD-Mandanten-ID (der Name des dieser Azure AD verwendetem anmelden, häufig die gleiche Ihr Unternehmen, jedoch nicht erforderlich)
* ID der Anwendung (während Schritt beim Erstellen des Azure AD-Anwendung geöffnet)
* Kennwort (die Sie beim Erstellen der Azure AD-Anwendung ausgewählt haben)

In den unter HTTP-Anforderung Vergewissern Sie sich mit den richtigen Werten "Azure AD-Mandanten-ID", "Anwendung-ID" und "Kennwort" ersetzen.

**Generische HTTP-Anforderung:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... wird (wenn Authentifizierung erfolgreich) dazu führen, dass eine ähnliche Antwort auf diese:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(Die Access_token in der obigen Antwort wurden zum Erhöhen der Lesbarkeit gekürzt wurden)

**Generieren von Access Token Bash verwenden:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Generieren von Access Token mithilfe der PowerShell aus:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Die Antwort enthält eine Access-Token, Informationen, wie lange die Token gültig ist und Informationen darüber, welche Ressourcen die Token für verwenden können.
Das Access-Token, die, das Sie in den vorherigen HTTP-Anruf erhalten, muss in für alle Anforderung in die Cloud-API als eine Kopfzeile mit dem Namen "Autorisierung" mit dem Wert "Person YOUR_ACCESS_TOKEN" übergeben werden. Beachten Sie den Abstand zwischen "Person" und Ihre Access-Token.

Wie Sie aus dem oben genannten HTTP-Ergebnis sehen können, ist das Token für einen bestimmten Zeitraum während der sollten Sie zwischenspeichern und erneut verwenden die gleiche Token gültig. Auch wenn es Azure AD für jeden Anruf API authentifizieren möglich ist, wäre es nicht sehr effizient.

## <a name="calling-arm-rest-apis"></a>Anrufen Cloud REST-APIs

[Azure Ressourcenmanager REST-APIs werden im folgenden beschrieben](https://msdn.microsoft.com/library/azure/dn790568.aspx) , und es ist außerhalb des Gültigkeitsbereichs für dieses Lernprogramm aus, um die Verwendung der einzelnen und jedes Dokument ein. Dieser Dokumentation wird nur wenige APIs verwenden, um die grundlegende Verwendung erläutert die APIs und anschließend verweisen wir Sie in der offiziellen Dokumentation.

### <a name="list-all-subscriptions"></a>Liste aller Abonnements

Einer der am einfachsten Vorgänge, die Sie ausführen können wird die verfügbaren Abonnements aufgeführt, die Sie zugreifen können. In den unter anfordern, können Sie sehen, wie Access Token in Form einer Überschrift weitergegeben wird.

(Ersetzen Sie mit der tatsächlichen Access Token YOUR_ACCESS_TOKEN.)

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

... und daher, erhalten Sie eine Liste der Abonnements, die für den Zugriff auf diesen Dienst Tilgungsanteile zulässig ist

(Abonnement IDs unten wurden zur Verbesserung der Lesbarkeit gekürzt)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Liste aller Ressourcengruppen in einem bestimmten Abonnement

Alle Ressourcen, die mit der Cloud-APIs verfügbare Funktionen sind in einer Ressourcengruppe geschachtelt. Wir werden Abfrage Cloud für vorhandene Ressourcengruppen in unseren Abonnement mithilfe der unter HTTP anfordern erhalten. Beachten Sie, wie die Abonnement-ID als Teil der URL diesmal übergeben wird.

(Ersetzen Sie YOUR_ACCESS_TOKEN und SUBSCRIPTION_ID mit der tatsächlichen Access Token und Abonnement-ID)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Die Antwort, die Sie erhalten hängt davon ab, ob Sie keine Ressourcengruppe definiert haben und wenn Ja, wie viele.

(Abonnement IDs unten wurden zur Verbesserung der Lesbarkeit gekürzt)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Bisher wir haben nur wurde Abfragen die Cloud-APIs Informationen, es ist Zeit wir einige Ressourcen stattdessen erstellen, und lassen Sie uns zunächst die einfachste hiervon all, eine Ressourcengruppe. Die folgende HTTP-Anforderung erstellt eine neue Ressourcengruppe einer Region/Speicherort Ihrer Wahl und ein oder mehrere Tags hinzugefügt (im Beispiel unten tatsächlich nur Fügt ein Tag).

(Ersetzen Sie YOUR_ACCESS_TOKEN, SUBSCRIPTION_ID, RESOURCE_GROUP_NAME mit Ihrem tatsächlichen Access Token, Abonnement-ID und die Namen der zu erstellenden Ressourcengruppe)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Wenn der Vorgang erfolgreich ist, erhalten Sie eine ähnliche Antwort auf diese.

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Sie haben eine Ressourcengruppe erfolgreich in Azure erstellt. Herzlichen Glückwunsch!

### <a name="deploy-resources-to-a-resource-group-using-an-arm-template"></a>Bereitstellen von Ressourcen zu einer Ressourcengruppe mithilfe einer Vorlage Cloud

Mit der Cloud können Sie mithilfe von Vorlagen des Cloud Ressourcen bereitstellen. Eine Vorlage Cloud definiert mehrere Ressourcen und deren abhängigen Dateien. Für diesen Abschnitt wir gerade nehmen an, dass Sie mit der Cloud-Vorlagen vertraut sind wird nur erfahren Sie, wie den Anruf API zum Starten der Bereitstellung eines führen. Eine detaillierte Dokumentation Cloud-Vorlagen finden Sie hier.

Bereitstellung von einer Cloud-Vorlage nicht viel unterscheiden sich, wie Sie andere APIs anzurufen. Ein wichtiger Aspekt ist, dass die Bereitstellung von einer Vorlage kann ganz sehr lange dauern, je nachdem, was sich innerhalb der Vorlage verwenden, ist die API wird nur zurückgerufen werden, und es liegt an Sie als Entwickler Abfrage für den Status der Bereitstellung akzeptieren, um herauszufinden, wann die Bereitstellung abgeschlossen ist.

In diesem Beispiel werden wir eine öffentlich zugänglicher Cloud-Vorlage verfügbar auf [GitHub](https://github.com/Azure/azure-quickstart-templates)verwenden. Die Vorlage, die wir verwenden möchten wird eine Linux VM in der Region Westen US bereitstellen. Auch wenn dieser Vorlage die Vorlage verfügbar in einem öffentlichen Repository wie GitHub hat, können Sie auch die vollständige Vorlage als Teil der Anforderung übergeben auswählen. Beachten Sie, dass wir Parameterwerte als Teil der Anforderung bereitstellen, die innerhalb der verwendeten Vorlage verwendet wird.

(Ersetzen Sie SUBSCRIPTION_ID RESOURCE_GROUP_NAME DEPLOYMENT_NAME YOUR_ACCESS_TOKEN GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME ADMIN_USER_NAME ADMIN_PASSWORD und DNS_NAME_FOR_PUBLIC_IP je nach Anforderung Werte)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Die sehr lange JSON-Antwort für diese Anforderung wurde weggelassen, müssen, um zu dieser Dokumentation Lesbarkeit zu verbessern. Die Antwort enthält Informationen zur Bereitstellung mit Vorlagen, die Sie soeben erstellt haben.

