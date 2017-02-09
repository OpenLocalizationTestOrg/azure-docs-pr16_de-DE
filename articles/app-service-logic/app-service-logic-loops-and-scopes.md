<properties
   pageTitle="Logik Apps Schleifen, Bereichen und vorgelagerte | Microsoft Azure"
   description="Logik App Schleife, Umfang und Konzepte vorgelagerte"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/14/2016"
   ms.author="jehollan"/>
   
# <a name="logic-apps-loops-scopes-and-debatching"></a>Logik Apps Schleifen, Bereichen und vorgelagerte
  
>[AZURE.NOTE] Diese Version von Artikel wendet Logik Apps 2016-04-01-Vorschau Schema und höher.  Konzepte sind für ältere Schemas vergleichbar, aber Bereiche sind nur verfügbar für dieses Schema und höher.
  
## <a name="foreach-loop-and-arrays"></a>ForEach-Schleife und Matrizen
  
Logik Apps können Sie über eine Reihe von Daten in einer Schleife, und führen eine Aktion für jedes Element.  Dies ist möglich, über die `foreach` Aktion.  Im Designer, Sie können angeben, dass hinzufügen eine für jede Schleife.  Nach der Auswahl der Matrix, die, der Sie durchlaufen möchten, können Sie beginnen, diesem Aktionen hinzuzufügen.  Sie sind derzeit auf nur eine Aktion pro Foreach-Schleife beschränkt, aber diese Einschränkung wird in den kommenden Wochen aufgehoben werden.  Nur ein Mal in der Schleife beginnen Sie auf angeben, was auf jeder Wert des Arrays erfolgen soll.

Wenn der Code-Ansicht zu verwenden, können Sie angeben eines für jede Schleife wie unten.  Dies ist ein Beispiel für eine für jede Schleife, die eine e-Mail für die einzelnen e-Mail-Adressen, die sendet 'microsoft.com' enthält:

```
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  A `foreach` Aktion können Sie auf Matrizen bis zu 5.000 Zeilen durchlaufen.  Jeder Iteration kann parallel, ausführen, damit es möglicherweise notwendig sein, Nachrichten an eine Warteschlange hinzufügen, wenn strömungsregelung benötigt wird.
  
## <a name="until-loop"></a>Bis Schleife
  
  Sie können eine Aktion oder eine Reihe von Aktionen ausführen, bis eine Bedingung erfüllt ist.  Das am häufigsten verwendete für dieses Szenario wird einen Endpunkt aufrufen, bis Sie die Antwort erhalten, die Ihnen gesuchte.  Klicken Sie im Designer können Sie angeben zum Hinzufügen einer bis Schleife.  Sie können nach dem Hinzufügen von Aktionen in der Schleife aus, die Bedingung, die als auch der Schleife festlegen Grenzwerte.  Es gibt eine Verzögerung 1 Minute Schleife verkettet.
  
  Wenn der Code-Ansicht zu verwenden, können Sie angeben einer bis Schleife wie unten.  Dies ist ein Beispiel für einen HTTP-Endpunkt aufrufen, bis im Antworttext den Wert 'Erledigt' hat.  Vervollständigt wann entweder 
  
  * HTTP-Antwort hat Status 'Abgeschlossen'
  * Es hat versucht für 1 Stunde
  * Es hat 100 Mal durchlaufen.
  
  ```
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed'),
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn und vorgelagerte

Manchmal erhalten ein Triggers möglicherweise ein Array von Elementen, die Sie verwenden möchten, debatch und Starten eines Workflows pro Element.  Hierfür kann über die `spliton` Befehl.  Wenn Ihre Trigger Swagger Nutzlast angibt, die ein Array, ist standardmäßig eine `spliton` hinzugefügt werden sollen, und starten Sie einen pro Element.  SplitOn kann nur einen Trigger hinzugefügt werden.  Dies kann manuell konfiguriert oder in der Ansicht mit Definition Code überschrieben.  Aktuell SplitOn debatch kann bis zu 5.000 Elemente Matrizen.  Kann Ihnen keine `spliton` und auch das Syncronous Antwort Muster implementieren.  Alle Workflow aufgerufen, verfügt über eine `response` Aktion zusätzlich zu `spliton` Asyncronously ausgeführt wird, und senden Sie sofort `202 Accepted` Antwort.  

SplitOn kann in der Ansicht mit Code wie im folgenden Beispiel angegeben werden muss.  Diese empfängt ein Array von Elementen und debatches für jede Zeile.

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequency": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Bereiche

Es ist möglich, eine Reihe von Aktionen, die einen Bereich mit gruppieren.  Dies ist besonders hilfreich, für die Behandlung von Ausnahmen.  Im Designer können Sie einen neuen Bereich hinzufügen und dem Hinzufügen von Aktionen in ihr.  Sie können in der Ansicht mit Code wie in der folgenden Bereiche definieren:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
