Mit Azure Ressourcenmanager, definieren Sie Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt als Parameter bezeichnet, der alle Parameterwerte enthält.
Sie sollten einen Parameter für diese Werte definieren, die variiert basierend auf das Projekt, das Sie bereitstellen oder basierend auf der Umgebung, die, der Sie bereitstellen. Legen Sie keine Parameter für Werte, die immer unverändert bleiben soll. Jeden Parameterwert wird in der Vorlage verwendet, um die Ressourcen zu definieren, die bereitgestellt werden. 

Wenn Parameter definieren möchten, verwenden Sie das Feld **AllowedValues** , um anzugeben, welche Werte ein Benutzer, während der Bereitstellung angeben kann. Verwenden Sie das Feld **Standardwert** zum Zuweisen des Werts an den Parameter, wenn kein Wert, während der Bereitstellung angegeben wird.

Wir beschreiben Sie jeden Parameter in der Vorlage.

### <a name="sitename"></a>siteName

Der Name der Web-app, die Sie erstellen möchten.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName

Der Name des der App-Serviceplan für das Web app-hosting verwenden.
    
    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>SKU

Die Preise Ebene für den Plan Hostinganbieter.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

Die Vorlage definiert die Werte, die für diesen Parameter zulässig sind, und weist einen Standardwert (S1) aus, wenn kein Wert angegeben ist.

### <a name="workersize"></a>workerSize

Die Größe der Instanz des Hostinganbieter Plans (klein, Mittel oder groß).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }
    
Die Vorlage definiert die Werte, die für diesen Parameter (0, 1 oder 2) berechtigt sind, und weist einen Standardwert (0), wenn kein Wert angegeben ist. Die Werte entsprechen kleine, mittlere und große.
