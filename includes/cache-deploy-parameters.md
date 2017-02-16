
### <a name="cacheskuname"></a>cacheSKUName

Die Preise Ebene des neuen Azure Redis Cache.

    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "The pricing tier of the new Azure Redis Cache."
      }
    },

Die Vorlage definiert die Werte, die für diesen Parameter (Basic oder Standard) zulässig sind, und weist einen Standardwert (Basic), wenn kein Wert angegeben ist. Basic stellt einen einzelnen Knoten mit mehreren Größen verfügbar von in 53 GB.
Standard stellt zwei Knoten primär/Replikat mit mehreren Größen verfügbar von in 53 GB und 99,9 % Vereinbarung zum SERVICELEVEL.

### <a name="cacheskufamily"></a>cacheSKUFamily

Die Familie für das Sku.

    "cacheSKUFamily": {
      "type": "string",
      "allowedValues": [
        "C"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "The family for the sku."
      }
    },


### <a name="cacheskucapacity"></a>cacheSKUCapacity

Die Größe der neuen Azure Redis Cache Instanz. 

    "cacheSKUCapacity": {
      "type": "int",
      "allowedValues": [
        0,
        1,
        2,
        3,
        4,
        5,
        6
      ],
      "defaultValue": 0,
      "metadata": {
        "description": "The size of the new Azure Redis Cache instance. "
      }
    }


Die Vorlage definiert die Werte, die für diesen Parameter (0, 1, 2, 3, 4, 5 oder 6) zulässig sind, und weist einen Standardwert (1), wenn kein Wert angegeben ist. Diese Zahlen entsprechen folgenden Cachegröße: 0 = 250 MB, 1 = 1 GB, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB

