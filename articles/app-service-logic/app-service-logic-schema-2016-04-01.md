<properties 
    pageTitle="Neues Schemaversion 2016-06-01 | Microsoft Azure" 
    description="Erfahren Sie, wie die JSON-Definition für die neueste Version von apps Logik schreiben" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="new-schema-version-2016-06-01"></a>Neues Schemaversion 2016-06-01

Das neue Schema und API Version für Logik apps bietet eine Reihe von zur Verbesserung der die Zuverlässigkeit zu verbessern und Center für erleichterte Bedienung von Logik apps. Es gibt 3 wichtigsten Unterschiede:

1. Hinzufügen von Bereiche, über die Aktionen werden, die eine Auflistung von Aktionen enthalten.
1. Bedingungen und Schleifen sind herausragende Aktionen.
1. Ausführung Sortierung ausführlicher über `runAfter` Eigenschaft (welche ersetzt `dependsOn`)

Weitere Informationen zum Upgrade Ihrer Logik apps aus dem Schema 2015-08-01-Vorschau auf das Schema 2016-06-01 [schauen Sie sich im folgenden Abschnitt Aktualisierung.](#upgrading-to-2016-06-01-schema)


## <a name="1-scopes"></a>1. Bereiche

Eine über die wichtigsten Änderungen in diesem Schema wird die Hinzufügung Bereiche und die Möglichkeit, Aktionen ineinander verschachteln.  Dies ist hilfreich, wenn eine Reihe von Aktionen zusammen gruppieren oder Aktionen ineinander schachteln (beispielsweise kann eine Bedingung eine weitere Bedingung enthalten) benötigen.  Weitere Informationen zur Syntax Umfang sein können gefundenen [hier](app-service-logic-loops-and-scopes.md)jedoch eine einfache Umfang Beispiel finden Sie unter:


```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

## <a name="2-conditions-and-loops-changes"></a>2. Bedingungen und Änderungen in einer Schleife

In den vorherigen Versionen des Schemas wurden Bedingungen und Schleifen Parameter, die eine einzelne Aktion zugeordnet sind.  In diesem Schema wurde diese Einschränkung aufgehoben und jetzt Konditionen und Schleifen werden als den Typ einer Aktion.  Weitere Informationen finden Sie [in diesem Artikel](app-service-logic-loops-and-scopes.md), und ein einfaches Beispiel für eine Bedingung Aktion abgebildet:

```
{
    "If_trigger_is_foo": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'foo')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_bar": "..."
        }      
    }
}
```

## <a name="3-runafter-property"></a>3. Eigenschaft RunAfter

Das neue `runAfter` Eigenschaft ersetzen ist `dependsOn` helfen präzisere in ausführen Sortierung zulässig sind.  `dependsOn`"die Aktion ausgeführt und war erfolgreich," Synonym wurde, jedoch oft Sie müssen eine Aktion ausführen, wenn die vorherige Aktion erfolgreich ist, fehlgeschlagen ist oder übersprungen.  `runAfter`ermöglicht die Flexibilität.  Es ist ein Objekt, das alle Aktionsnamen gibt an, die sie nach dem ausgeführt werden kann, und definiert ein Array von Status, die zum Auslösen von zulässig sind.  Für Beispiel, wenn Sie möchten nach Schritte A wurde erfolgreich verlaufen ist und B ausführen erfolgreich war oder fehlschlug, würden Sie erstellen die folgenden `runAfter` Eigenschaft:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrading-to-2016-06-01-schema"></a>Upgrade auf 2016-06-01 schema

Upgrade auf das neue 2016-06-01 Schema benötigen nur wenige Schritte.  Die Unterschiede zwischen dem Schema Details finden Sie [in diesem Artikel](app-service-logic-schema-2016-04-01.md).  Der Upgradeprozess umfasst das Upgrade-Skript speichern als neue app Logik und potenziell überschreiben alten Logik app bei Bedarf ausgeführt.

1. Öffnen Sie Ihre aktuelle Logik app.
1. Klicken Sie auf der Symbolleiste auf die Schaltfläche **Aktualisierungsschema**
   
    ![][1]
   
    Die aktualisierte Definition wird zurückgegeben.  Sie kopieren und fügen Sie dies in einer Ressourcendefinition, wenn Sie benötigen, aber es **wird dringend empfohlen** , Sie verwenden die Schaltfläche " **Speichern unter** " aus, um sicherzustellen, dass alle Verbindung könnten Verweise sind in der aktualisierten Logik app zulässig.
1. Klicken Sie auf die Schaltfläche " **Speichern unter** " in der Symbolleiste des Upgrade Blades.
1. Füllen Sie den Namen und Logik app-Status, und klicken Sie auf **Erstellen** , um Ihre Upgrade Logik app bereitstellen.
1. Stellen Sie sicher, dass Ihre app aktualisierten Logik wie erwartet funktioniert.

    >[AZURE.NOTE] Wenn Sie einen manuellen oder Anforderung Trigger verwenden, wird der Rückruf-URL in Ihrer neuen Logik app geändert haben.  Verwenden Sie die neue URL zu überprüfen, ob er-durchgehende funktioniert und Sie können über Ihre vorhandene Logik app zur vorherigen URLs verpflichtet klonen.

1. *Optional* Verwenden Sie die Schaltfläche **Datenbeschriftungsreihe** in der Symbolleiste (neben dem Symbol **Update Schema** in der Abbildung oben), um Ihre vorherigen Logik app durch die neue Schemaversion zu überschreiben.  Dies ist erforderlich, nur, wenn Sie möchten, erhalten bleiben sollen die gleichen Ressourcen-ID oder Trigger URL der app Logik anfordern.

### <a name="upgrade-tool-notes"></a>Upgrade Tool Notizen

#### <a name="condition-mapping"></a>Bedingung Zuordnung

Das Tool wird bemüht, die WAHR und falsch Verzweigung Aktionen zusammen in einen Bereich in der aktualisierten Definition gruppieren zu gestalten.  Speziell für das-Designer Muster von `@equals(actions('a').status, 'Skipped')` sollte werden als eine `else` Aktion.  Erstellen Sie das Tool erkennt, dass potenziell Mustern Programm nicht erkannt werden sollen jedoch die separate Bedingungen für den True und der falsch Verzweigung.  Aktionen können neu zugewiesen werden nach einer Aktualisierung bei Bedarf.

#### <a name="foreach-with-condition"></a>Mit der Bedingung ForEach
  
Das vorherige Muster einer Foreach-Schleife mit einer Bedingung pro Element kann in das neue Schema mit der Filteraktion repliziert werden.  Dies sollte automatisch auf Upgrade auftreten.  Die Bedingung wird eine Filteraktion, bevor die Foreach-Schleife (zurückzugebenden nur ein Array von Elementen, die die Bedingung entsprechen), und diese Matrix wird in der Aktion Foreach übergeben.  Sie können ein Beispiel für diese [in diesem Artikel](app-service-logic-loops-and-scopes.md) anzeigen.

#### <a name="resource-tags"></a>Ressourcen-Kategorien

Ressource Kategorien auf Upgrade entfernt werden und müssen sie erneut für den aktualisierten Workflow festlegen.

## <a name="other-changes"></a>Andere Änderungen

### <a name="manual-trigger-renamed-to-request-trigger"></a>Manuelle umbenannt, die Anfrage Trigger auslösen

Den Typ `manual` nicht mehr unterstützt und umbenannt wurde, um `request` mit der Art der `http`.  Dies ist konsistenter mit dem Typ des Musters, die zum Erstellen der Trigger verwendet wird.

### <a name="new-filter-action"></a>Neue 'Filter' Aktion

Wenn Sie mit einem großen Arrays und müssen auf eine kleinere Menge Elemente filtern arbeiten, können Sie den neuen 'Filter' ein.  Es akzeptiert ein Array und eine Bedingung und wertet die Bedingung für jedes Element und ein Array von Elementen, die die Bedingung entsprechen zurückzukehren.

### <a name="foreach-and-until-action-restrictions"></a>ForEach und bis Aktion Einschränkungen

Die Foreach und bis Schleife sind auf eine einzelne Aktion beschränkt.

### <a name="trackedproperties-on-actions"></a>Klicken Sie auf Aktionen TrackedProperties

Aktionen können nun verfügen über eine weitere Eigenschaft (nebengeordneten zu `runAfter` und `type`) aufgerufen `trackedProperties`.  Es ist ein Objekt, das angibt, bestimmte Aktion Eingaben oder Ausgaben, in den Diagnoseprotokollen Azure werden aufgenommen werden sollen, die als Teil eines Workflows ausgegeben wird.  Beispiel:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
- [Verwenden Sie die Workflowdefinition der Logik-app](app-service-logic-author-definitions.md)
- [Erstellen einer Vorlage für Logik app Bereitstellung](app-service-logic-create-deploy-template.md)


<!-- Image references -->
[1]: ./media/app-service-logic-schema-2016-04-01/upgradeButton.png
