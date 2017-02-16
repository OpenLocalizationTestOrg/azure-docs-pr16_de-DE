<properties
   pageTitle="Ressourcenmanager Vorlage Funktionen | Microsoft Azure"
   description="Beschreibt die Funktionen in einer Vorlage Azure Ressourcenmanager zum Abrufen von Werten und Arbeiten mit Zeichenfolgen und numerische Werte abrufen von Informationen zur Bereitstellung verwenden."
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
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-template-functions"></a>Azure Ressourcenmanager Vorlagenfunktionen

In diesem Thema werden die Funktionen, die Sie in einer Vorlage Ressourcenmanager Azure verwenden können.

Vorlagenfunktionen und deren Parameter Groß-und Kleinschreibung beachtet. Ressourcenmanager aufgelöst beispielsweise **variables('var1')** und **VARIABLES('VAR1')** als identisch. Wenn ausgewertet, es sei denn, die Funktion Gesetze Groß-/Kleinschreibung, (z. B. ToUpper oder ToLower) ändert, wird die Funktion der Groß-/Kleinschreibung beibehalten. Bestimmte Ressourcentypen möglicherweise Groß-/Kleinschreibung Anforderungen unabhängig davon, wie die Funktion ausgewertet werden.

## <a name="numeric-functions"></a>Numerische Funktionen

Ressourcenmanager bietet die folgenden Funktionen für die Arbeit mit ganzen Zahlen:

- [Hinzufügen](#add)
- [copyIndex](#copyindex)
- [div](#div)
- [Ganzzahl](#int)
- [Rest](#mod)
- [mul](#mul)
- [Sub](#sub)


<a id="add" />
### <a name="add"></a>Hinzufügen

**Fügen Sie hinzu (operand1 operand2)**

Gibt die Summe der beiden angegebenen ganzen Zahlen zurück.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| Operand1                           |   Ja    | Erste ganze Zahl hinzufügen.
| operand2                           |   Ja    | Zweite Ganzzahl hinzufügen.

Im folgenden Beispiel werden zwei Parameter.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to add"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to add"
        }
      }
    },
    ...
    "outputs": {
      "addResult": {
        "type": "int",
        "value": "[add(parameters('first'), parameters('second'))]"
      }
    }

<a id="copyindex" />
### <a name="copyindex"></a>copyIndex

**copyIndex(offset)**

Gibt den aktuellen Index einer Schleife Iteration zurück. 

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| Versatz                           |   Nein    | Der Betrag für die aktuelle Iteration Wert hinzufügen.

Diese Funktion ist immer mit einem Objekt **Kopieren** verwendet. Eine vollständige Beschreibung, wie Sie **CopyIndex**verwenden finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen in Azure Ressourcenmanager](resource-group-create-multiple.md).

Im folgenden Beispiel wird eine Schleife kopieren und den Indexwert, der im Lieferumfang von des Namens. 

    "resources": [ 
      { 
        "name": "[concat('examplecopy-', copyIndex())]", 
        "type": "Microsoft.Web/sites", 
        "copy": { 
          "name": "websitescopy", 
          "count": "[parameters('count')]" 
        }, 
        ...
      }
    ]


<a id="div" />
### <a name="div"></a>div

**Div (operand1, operand2)**

Gibt die Division ganzer Zahlen aus zwei bereitgestellten ganzen Zahlen zurück.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| Operand1                           |   Ja    | Ganze Zahl dividiert wird.
| operand2                           |   Ja    | Ganze Zahl, die dividiert wird verwendet wird. 0 kann nicht sein.

Im folgende Beispiel wird eine Parameter von einem weiteren Parameter.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "divResult": {
        "type": "int",
        "value": "[div(parameters('first'), parameters('second'))]"
      }
    }

<a id="int" />
### <a name="int"></a>Ganzzahl

**int(valueToConvert)**

Wandelt den angegebenen Wert ganze Zahl zurück.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| valueToConvert                     |   Ja    | Der Wert in Integer zu konvertieren. Der Typ des Werts kann nur Zeichenfolge oder eine ganze Zahl sein.

Im folgende Beispiel konvertiert den Benutzer bereitgestellte Parameterwert ganze Zahl zurück.

    "parameters": {
        "appId": { "type": "string" }
    },
    "variables": { 
        "intValue": "[int(parameters('appId'))]"
    }


<a id="mod" />
### <a name="mod"></a>Rest

**MOD (operand1, operand2)**

Gibt den Rest der Division ganzer Zahlen mithilfe der zwei angegebenen Zahlen zurück.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| Operand1                           |   Ja    | Ganze Zahl dividiert wird.
| operand2                           |   Ja    | Ganze Zahl, die verwendet wird, dividiert wird, muss von 0 abweichen.

Im folgende Beispiel gibt den Rest der Division von einem weiteren Parameter einen Parameter.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "modResult": {
        "type": "int",
        "value": "[mod(parameters('first'), parameters('second'))]"
      }
    }

<a id="mul" />
### <a name="mul"></a>mul

**Mul (operand1, operand2)**

Gibt die Multiplikation aus zwei bereitgestellten ganzen Zahlen zurück.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| Operand1                           |   Ja    | Erste ganze Zahl multipliziert werden soll.
| operand2                           |   Ja    | Zweite ganze Zahl multipliziert werden soll.

Im folgende Beispiel multipliziert einen Parameter, indem Sie einen weiteren Parameter.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to multiply"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to multiply"
        }
      }
    },
    ...
    "outputs": {
      "mulResult": {
        "type": "int",
        "value": "[mul(parameters('first'), parameters('second'))]"
      }
    }

<a id="sub" />
### <a name="sub"></a>Sub

**Sub (operand1, operand2)**

Gibt die Subtraktion aus zwei bereitgestellten ganzen Zahlen zurück.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| Operand1                           |   Ja    | Ganze Zahl, die von subtrahiert wird.
| operand2                           |   Ja    | Ganze Zahl, die subtrahiert wird.

Im folgende Beispiel subtrahiert einen Parameter aus einem anderen Parameter.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer subtracted from"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer to subtract"
        }
      }
    },
    ...
    "outputs": {
      "subResult": {
        "type": "int",
        "value": "[sub(parameters('first'), parameters('second'))]"
      }
    }

## <a name="string-functions"></a>Zeichenfolgenfunktionen

Ressourcenmanager bietet die folgenden Funktionen für die Arbeit mit Zeichenfolgen an:

- [Base64](#base64)
- [Verketten](#concat)
- [Länge](#lengthstring)
- [padLeft](#padleft)
- [Ersetzen](#replace)
- [Überspringen](#skipstring)
- [Teilen](#split)
- [Zeichenfolge](#string)
- [Teilzeichenfolge](#substring)
- [Übernehmen](#takestring)
- [toLower](#tolower)
- [toUpper](#toupper)
- [Kürzen](#trim)
- [uniqueString](#uniquestring)
- [URI](#uri)


<a id="base64" />
### <a name="base64"></a>Base64

**Base64 (InputString)**

Gibt die Darstellung base64 die eingegebene Zeichenfolge zurück.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| inputString                        |   Ja    | Der Zeichenfolgenwert als base64 Darstellung zurück.

Im folgenden Beispiel wird gezeigt, wie die base64-Funktion verwenden.

    "variables": {
      "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
      "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

<a id="concat" />
### <a name="concat---string"></a>Verketten - Zeichenfolge

**Verketten (Zeichenfolge1, Zeichenfolge2, Zeichenfolge3,...)**

Mehrere Zeichenfolgenwerte kombiniert, und gibt die verkettete Zeichenfolge zurück. 

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| Zeichenfolge1:                        |   Ja    | Ein Zeichenfolgenwert zu verketten.
| zusätzliche Zeichenfolgen             |   Nein     | Zeichenfolgenwerte zu verketten.

Diese Funktion kann eine beliebige Anzahl von Argumenten in Anspruch nehmen und kann entweder Zeichenfolgen oder Arrays für die Parameter übernehmen. Ein Beispiel für Arrays zu verketten, finden Sie unter [Verketten - Matrix](#concatarray).

Im folgenden Beispiel wird veranschaulicht, wie mehrere Zeichenfolgenwerte verkettete Zeichenfolge zur Rückkehr zu kombinieren.

    "outputs": {
        "siteUri": {
          "type": "string",
          "value": "[concat('http://', reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
        }
    }


<a id="lengthstring" />
### <a name="length---string"></a>Länge - Zeichenfolge

**Length(String)**

Gibt die Anzahl der Zeichen in einer Zeichenfolge zurück.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| Zeichenfolge                        |   Ja    | Der Zeichenfolgenwert für den Abruf von der Anzahl von Zeichen verwendet.

Ein Beispiel für die Verwendung von Länge mit einer Matrix zurück, finden Sie unter [Länge - Array](#length).

Im folgende Beispiel gibt die Anzahl der Zeichen in einer Zeichenfolge zurück. 

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "nameLength": "[length(parameters('appName'))]"
    }
        

<a id="padleft" />
### <a name="padleft"></a>padLeft

**PadLeft (ValueToPad, TotalLength PaddingCharacter)**

Gibt eine Zeichenfolge rechtsbündig ausgerichtet durch Hinzufügen von Zeichen nach links, bis die angegebene Gesamtlänge zu erreichen.
  
| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| valueToPad                         |   Ja    | Die Zeichenfolge oder eine Ganzzahl zu rechts auszurichten.
| totalLength                        |   Ja    | Die Gesamtzahl der Zeichen in die zurückgegebene Zeichenfolge.
| paddingCharacter                   |   Nein     | Das Zeichen für Links den Abstand bis die Gesamtlänge erreicht ist. Der Standardwert ist ein Leerzeichen.

Im folgenden Beispiel wird veranschaulicht, wie auf der Wähltastatur des Benutzer bereitgestellte Parameterwert durch Hinzufügen des Zeichens 0 (null), bis die Zeichenfolge 10 Zeichen erreicht. Wenn der ursprünglichen Parameterwert mehr als 10 Zeichen ist, werden keine Zeichen hinzugefügt.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "paddedAppName": "[padLeft(parameters('appName'),10,'0')]"
    }

<a id="replace" />
### <a name="replace"></a>Ersetzen

**Ersetzen (OriginalString, OldCharacter, NewCharacter)**

Gibt eine neue Zeichenfolge mit allen Instanzen des Zeichens in der angegebenen Zeichenfolge durch ein anderes Zeichen ersetzt.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| originalString                     |   Ja    | Die Zeichenfolge, die alle Instanzen des Zeichens durch ein anderes Zeichen ersetzt hat.
| oldCharacter                       |   Ja    | Das Zeichen aus der ursprünglichen Zeichenfolge entfernt werden soll.
| newCharacter                       |   Ja    | Das Zeichen anstelle des Zeichens entfernte hinzufügen.

Im folgenden Beispiel wird gezeigt, wie alle Striche aus der Benutzer bereitgestellte Zeichenfolge entfernen.

    "parameters": {
        "identifier": { "type": "string" }
    },
    "variables": { 
        "newidentifier": "[replace(parameters('identifier'),'-','')]"
    }

<a id="skipstring" />
### <a name="skip---string"></a>Überspringen - Zeichenfolge
**Überspringen (OriginalValue, NumberToSkip)**

Gibt eine Zeichenfolge zurück, bei dem alle Zeichen nach einer festgelegten Anzahl der Zeichenfolge an.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| originalValue                      |   Ja    | Die Zeichenfolge für überspringen verwendet werden soll.
| numberToSkip                       |   Ja    | Die Anzahl von Zeichen zu überspringen. Wenn dieser Wert kleiner oder gleich 0 ist, werden alle Zeichen der Zeichenfolge zurückgegeben. Wenn sie größer als die Länge der Zeichenfolge ist, wird eine leere Zeichenfolge zurückgegeben. 

Ein Beispiel für die Verwendung von überspringen mit einer Matrix zurück finden Sie unter [Überspringen - Matrix](#skip).

Im folgende Beispiel überspringt die angegebene Anzahl von Zeichen in der Zeichenfolge an.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for skipping"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[skip(parameters('first'),parameters('second'))]"
      }
    }


<a id="split" />
### <a name="split"></a>Teilen

**Teilen (InputString, DelimiterString)**

**Teilen (InputString, DelimiterArray)**

Gibt ein Array von Zeichenfolgen, die die untergeordneten Zeichenfolgen für die eingegebene Zeichenfolge enthält, die durch die angegebenen Trennzeichen getrennt werden.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| inputString                        |   Ja    | Die Zeichenfolge zu teilen.
| Trennzeichen                          |   Ja    | Das Trennzeichen, das verwenden, kann eine einzelne Zeichenfolge oder ein Array von Zeichenfolgen sein.

Im folgenden Beispiel wird die eingegebene Zeichenfolge mit einem Komma.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "stringPieces": "[split(parameters('inputString'), ',')]"
    }

Im nächste Beispiel wird die eingegebene Zeichenfolge durch ein Komma oder ein Semikolon geteilt.

    "variables": {
      "stringToSplit": "test1,test2;test3",
      "delimiters": [ ",", ";" ]
    },
    "resources": [ ],
    "outputs": {
      "exampleOutput": {
        "value": "[split(variables('stringToSplit'), variables('delimiters'))]",
        "type": "array"
      }
    }

<a id="string" />
### <a name="string"></a>Zeichenfolge

**String(valueToConvert)**

Konvertiert den angegebenen Wert in einer Zeichenfolge an.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| valueToConvert                     |   Ja    | Der Wert in der Zeichenfolge konvertiert. Sämtliche Typen von Werten kann einschließlich Objekte und Arrays konvertiert werden.

Im folgende Beispiel werden Benutzer bereitgestellten Parameterwerte in Zeichenfolgen konvertiert.

    "parameters": {
      "jsonObject": {
        "type": "object",
        "defaultValue": {
          "valueA": 10,
          "valueB": "Example Text"
        }
      },
      "jsonArray": {
        "type": "array",
        "defaultValue": [ "a", "b", "c" ]
      },
      "jsonInt": {
        "type": "int",
        "defaultValue": 5
      }
    },
    "variables": { 
      "objectString": "[string(parameters('jsonObject'))]",
      "arrayString": "[string(parameters('jsonArray'))]",
      "intString": "[string(parameters('jsonInt'))]"
    }

<a id="substring" />
### <a name="substring"></a>Teilzeichenfolge

**Teilzeichenfolge (StringToParse, StartIndex; Länge)**

Gibt eine Teilzeichenfolge, die mit der angegebenen Zeichenposition beginnt und die angegebene Anzahl von Zeichen enthält.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| stringToParse                     |   Ja    | Die ursprüngliche Zeichenfolge aus der die Teilzeichenfolge extrahiert wird.
| startIndex                         | Nein      | Die nullbasierte Zeichen Startposition nach der Teilzeichenfolge.
| Länge                             | Nein      | Die Anzahl der Zeichen für die Teilzeichenfolge.

Im folgende Beispiel extrahiert die ersten drei Zeichen aus einem Parameter.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "prefix": "[substring(parameters('inputString'), 0, 3)]"
    }

<a id="takestring" />
### <a name="take---string"></a>Übernehmen - Zeichenfolge
**Übernehmen (OriginalValue, NumberToTake)**

Gibt eine Zeichenfolge mit der angegebenen Anzahl von Zeichen ab Beginn der Zeichenfolge an.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| originalValue                      |   Ja    | Die Zeichenfolge, machen Sie die Zeichen aus.
| numberToTake                       |   Ja    | Die Anzahl der Zeichen werden müssen. Wenn dieser Wert kleiner oder gleich 0 ist, wird eine leere Zeichenfolge zurückgegeben. Wenn sie größer als die Länge der angegebenen Zeichenfolge ist, werden alle Zeichen der Zeichenfolge zurückgegeben.

Ein Beispiel für die Verwendung von übernehmen mit einer Matrix zurück finden Sie unter [Ausführen - Matrix](#take).

Im folgenden Beispiel wird die angegebene Anzahl von Zeichen aus der Zeichenfolge an.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for taking"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[take(parameters('first'), parameters('second'))]"
      }
    }

<a id="tolower" />
### <a name="tolower"></a>toLower

**toLower(stringToChange)**

Die angegebene Zeichenfolge konvertiert in Kleinbuchstaben.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| stringToChange                     |   Ja    | Die Zeichenfolge in Kleinbuchstaben konvertiert.

Im folgende Beispiel werden Benutzer bereitgestellte Parameterwert in Kleinbuchstaben konvertiert.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "lowerCaseAppName": "[toLower(parameters('appName'))]"
    }

<a id="toupper" />
### <a name="toupper"></a>toUpper

**toUpper(stringToChange)**

Konvertiert die angegebene Zeichenfolge in Großbuchstaben.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| stringToChange                     |   Ja    | Die Zeichenfolge in Großbuchstaben konvertiert.

Im folgende Beispiel wandelt den Benutzer bereitgestellte Parameterwert in Großbuchstaben.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "upperCaseAppName": "[toUpper(parameters('appName'))]"
    }

<a id="trim" />
### <a name="trim"></a>Kürzen

**Kürzen (StringToTrim)**

Entfernt alle führende und nachfolgende Leerzeichen aus der angegebenen Zeichenfolge an.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| stringToTrim                       |   Ja    | Die Zeichenfolge kürzen.

Im folgende Beispiel schneidet die Leerzeichen aus dem Benutzer bereitgestellte Parameterwert.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "trimAppName": "[trim(parameters('appName'))]"
    }

<a id="uniquestring" />
### <a name="uniquestring"></a>uniqueString

**UniqueString (BaseString,...)**

Erstellt eine deterministische Hashzeichenfolge basierend auf den Werten, die als Parameter an. 

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| baseString      |   Ja    | Die Zeichenfolge, die in der Hash-Funktion verwendet, um eine eindeutige Zeichenfolge zu erstellen.
| Je nach Bedarf weitere Parameter    | Nein       | Sie können beliebig viele Zeichenfolgen Bedarf den Wert zu erstellen, der angibt, die Ebene der Eindeutigkeit hinzufügen.

Diese Funktion ist hilfreich, wenn Sie einen eindeutigen Namen für eine Ressource erstellen müssen. Geben Sie ein Parameterwerte, die den Bereich der Eindeutigkeit für das Ergebnis zu beschränken. Sie können angeben, ob der Name nach unten bis zum Abonnement, Ressourcengruppe oder Bereitstellung eindeutig ist. 

Der zurückgegebene Wert ist nicht an eine zufällige Zeichenfolge, aber lieber das Ergebnis einer Hashfunktion. Der zurückgegebene Wert ist 13 Zeichen lang sein. Es ist nicht global eindeutig. Sie möchten möglicherweise kombinieren den Wert mit dem Präfix aus Ihrer Benennungskonvention einen Namen zu erstellen, der sinnvoll ist. Im folgenden Beispiel wird das Format der zurückgegebene Wert an. Der tatsächliche Wert wird natürlich durch die bereitgestellten Parameter variieren.

    tcvhiyu5h2o5o

Den folgenden Beispielen wird so UniqueString verwenden, um einen eindeutigen Wert für häufig verwendete Ebenen zu erstellen.

Unique ausgelegte Abonnement

    "[uniqueString(subscription().subscriptionId)]"

Unique ausgelegte Ressourcengruppe

    "[uniqueString(resourceGroup().id)]"

Unique ausgelegte mit Bereitstellung für eine Ressourcengruppe

    "[uniqueString(resourceGroup().id, deployment().name)]"
    
Das folgende Beispiel zeigt, wie Sie einen eindeutigen Namen für ein Speicherkonto basierend auf Ihrer Ressourcengruppe erstellen (innerhalb dieser Ressourcengruppe der Name nicht ist eindeutig If erstellt die gleiche Weise).

    "resources": [{ 
        "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]", 
        "type": "Microsoft.Storage/storageAccounts", 
        ...



<a id="uri" />
### <a name="uri"></a>URI

**URI (BaseUri, RelativeUri)**

Erstellt einen absoluten URI durch Kombinieren der BaseUri und die Zeichenfolge RelativeUri an.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| baseUri                            |   Ja    | Die Basis-Uri-Zeichenfolge.
| relativeUri                        |   Ja    | Der relativen Uri-Zeichenfolge, die die Basis-Uri-Zeichenfolge hinzugefügt werden soll.

Der Wert für den Parameter **BaseUri** kann eine bestimmte Datei enthalten, aber nur der grundlegende Pfad wird beim Erstellen der URI verwendet. Beispielsweise übergeben **http://contoso.com/resources/azuredeploy.json** als BaseUri Parameter ergibt ein Basis-URI- **http://contoso.com/resources/**.

Im folgenden Beispiel wird das Erstellen eines Links zu einer verschachtelten Vorlage basierend auf dem Wert der übergeordneten Vorlage.

    "templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"

## <a name="array-functions"></a>Array-Funktionen

Ressourcenmanager bietet verschiedene Funktionen für die Arbeit mit Array von Werten an.

- [Verketten](#concatarray)
- [Länge](#length)
- [Überspringen](#skip)
- [Übernehmen](#take)

Ein Array von Zeichenfolgenwerten nach einem Wert getrennt können, finden Sie unter [Teilen](#split).

<a id="concatarray" />
### <a name="concat---array"></a>Verketten - Matrix

**Verketten (Matrix1, array2; array3;...)**

Kombiniert mehrere Arrays und gibt das verkettete Array zurück. 

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| Matrix1                        |   Ja    | Ein Array zu verketten.
| zusätzliche Matrizen zurück.             |   Nein     | Arrays zu verketten.

Diese Funktion kann eine beliebige Anzahl von Argumenten in Anspruch nehmen und kann entweder Zeichenfolgen oder Arrays für die Parameter übernehmen. Ein Beispiel für Zeichenfolgenwerte Verketten finden Sie unter [Verketten - Zeichenfolge](#concat).

Im folgenden Beispiel wird gezeigt, wie zum Kombinieren von zwei Matrizen zurück.

    "parameters": {
        "firstarray": {
            type: "array"
        }
        "secondarray": {
            type: "array"
        }
     },
     "variables": {
         "combinedarray": "[concat(parameters('firstarray'), parameters('secondarray'))]
     }
        

<a id="length" />
### <a name="length---array"></a>Länge - Matrix

**Length(Array)**

Gibt die Anzahl der Elemente in einem Array zurück.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| Matrix                        |   Ja    | Die Matrix für den Abruf von der Anzahl von Elementen verwenden.

Diese Funktion können mit einem Array Sie die Anzahl der Iterationen angeben, wenn Ressourcen zu erstellen. Im folgenden Beispiel würde der Parameter **SiteNames** auf ein Array von Namen beim Erstellen der Websites zu verwendende verweisen.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

Weitere Informationen zum Verwenden dieser Funktion mit einem Array finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen in Azure Ressourcenmanager](resource-group-create-multiple.md). 

Ein Beispiel für die Länge mit einem Zeichenfolgenwert verwenden finden Sie unter [Länge - Zeichenfolge](#lengthstring).

<a id="skip" />
### <a name="skip---array"></a>Fahren Sie – array
**Überspringen (OriginalValue, NumberToSkip)**

Gibt ein Array mit allen Elementen nach einer festgelegten Anzahl in der Matrix zurück.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| originalValue                      |   Ja    | Die Matrix für überspringen verwendet werden soll.
| numberToSkip                       |   Ja    | Die Anzahl von Elementen zu überspringen. Wenn dieser Wert kleiner oder gleich 0 ist, werden alle Elemente in der Matrix zurückgegeben. Wenn sie größer als die Länge des Arrays ist, wird ein leeres Array zurückgegeben. 

Ein Beispiel für eine Zeichenfolge mit überspringen finden Sie unter [Überspringen - Zeichenfolge](#skipstring).

Im folgende Beispiel überspringt die angegebene Anzahl von Elementen in der Matrix.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for skipping"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[skip(parameters('first'), parameters('second'))]"
      }
    }

<a id="take" />
### <a name="take---array"></a>Nehmen – array
**Übernehmen (OriginalValue, NumberToTake)**

Gibt ein Array mit der angegebenen Anzahl von Elementen vom Anfang des Arrays.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| originalValue                      |   Ja    | Die Matrix werden müssen, die Elemente aus.
| numberToTake                       |   Ja    | Die Anzahl von Elementen zu übernehmen. Wenn dieser Wert kleiner oder gleich 0 ist, wird ein leeres Array zurückgegeben. Wenn es größer als die Gesamtlänge der angegebenen Matrix ist, werden alle Elemente in der Matrix zurückgegeben.

Ein Beispiel für eine Zeichenfolge mit übernehmen finden Sie unter [Ausführen - Zeichenfolge](#takestring).

Im folgenden Beispiel wird die angegebene Anzahl von Elementen aus der Matrix.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for taking"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[take(parameters('first'),parameters('second'))]"
      }
    }

## <a name="deployment-value-functions"></a>Bereitstellung Wert Funktionen

Ressourcenmanager bietet die folgenden Funktionen zum Abrufen der Werte aus der Vorlage und Werte, die im Zusammenhang mit der Bereitstellung Abschnitte:

- [Bereitstellung](#deployment)
- [Parameter](#parameters)
- [Variablen](#variables)

Um die Werte von Ressourcen zu gelangen, finden Sie unter Ressourcengruppen oder Abonnements, [Ressourcen Funktionen](#resource-functions).

<a id="deployment" />
### <a name="deployment"></a>Bereitstellung

**Freigabe deklarieren()**

Gibt Informationen zu den aktuellen Bereitstellungsvorgang.

Diese Funktion gibt das Objekt, das während der Bereitstellung weitergegeben wird. Die Eigenschaften im zurückgegebenen Objekt variieren basierend auf, ob das Bereitstellungsobjekt als Link oder als Inline-Objekt übergeben wird. 

Wenn das Bereitstellungsobjekt bei Verwendung des Parameters **- TemplateFile** verweisen auf eine lokale Datei in Azure PowerShell inline-, z. B. übergeben wird, weist das zurückgegebene Objekt folgende Format an:

    {
        "name": "",
        "properties": {
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [
                ],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

Wenn das Objekt als Link, übergeben ist wie bei Verwendung des Parameters **- TemplateUri** auf remote-Objekt verweisen, das Objekt in folgendem Format zurückgegeben wird. 

    {
        "name": "",
        "properties": {
            "templateLink": {
                "uri": ""
            },
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

Im folgenden Beispiel wird veranschaulicht, wie mit Freigabe deklarieren() in eine andere Vorlage basierend auf dem URI der übergeordneten Vorlage verknüpfen.

    "variables": {  
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
    }  

<a id="parameters" />
### <a name="parameters"></a>Parameter

**Parameter (ParameterName)**

Gibt einen Parameterwert an. Der angegebenen Parameternamen muss im Abschnitt Parameter der Vorlage definiert werden.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| parameterName                      |   Ja    | Der Name des Parameters an, um zurückzukehren.

Im folgenden Beispiel wird eine vereinfachte Nutzung der Funktion Parameter.

    "parameters": { 
      "siteName": {
          "type": "string"
      }
    },
    "resources": [
       {
          "apiVersion": "2014-06-01",
          "name": "[parameters('siteName')]",
          "type": "Microsoft.Web/Sites",
          ...
       }
    ]

<a id="variables" />
### <a name="variables"></a>Variablen

**Variablen (Variablenname)**

Gibt den Wert der Variablen an. Der angegebene Variablennamen muss im Variablenabschnitt der Vorlage definiert werden.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| Variable Name                      |   Ja    | Der Name der Variablen zurück.

Im folgende Beispiel wird einen Variablen Wert verwendet.

    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
      {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
      }
    ],

## <a name="resource-functions"></a>Ressourcen-Funktionen

Ressourcenmanager bietet die folgenden Funktionen für den Abruf von Ressourcenwerte ein:

- [ListKeys- und Liste {Wert}](#listkeys)
- [Anbieter](#providers)
- [Bezug](#reference)
- [resourceGroup](#resourcegroup)
- [resourceId](#resourceid)
- [Abonnement](#subscription)

Wenn Sie die um Werte von Parametern, Variablen oder der aktuellen Bereitstellung zu gelangen, finden Sie unter [Bereitstellung Wert Funktionen](#deployment-value-functions).

<a id="listkeys" />
<a id="list" />
### <a name="listkeys-and-listvalue"></a>ListKeys- und Liste {Wert}

**ListKeys (Ressourcenname oder ResourceIdentifier, ApiVersion)**

**Liste {Wert} (Ressourcenname oder ResourceIdentifier, ApiVersion)**

Gibt die Werte für einen beliebigen Ressourcen, die die Listenoperation unterstützt. Die häufigste Verwendung ist **ListKeys**. 
  
| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| Ressourcenname oder resourceIdentifier |   Ja    | Eindeutiger Bezeichner für die Ressource.
| apiVersion                         |   Ja    | API-Version der Ressource Laufzeitzustand.

Alle Vorgänge, die beginnt **mit** verwendet eine Funktion in der Vorlage ein. Die verfügbaren Vorgänge enthalten, nicht nur **ListKeys**, sondern auch Operationen wie **Listen-**, **ListAdminKeys**und **ListStatus**. Um zu bestimmen, welche Ressourcentypen über eine Listenoperation verfügen, verwenden Sie den folgenden PowerShell-Befehl ein.

    Get-AzureRmProviderOperation -OperationSearchString *  | where {$_.Operation -like "*list*"} | FT Operation

Oder rufen Sie die Liste mit Azure CLI. Im folgende Beispiel ruft alle Vorgänge für **Apiapps**und den JSON-Programm [Jq](http://stedolan.github.io/jq/download/) verwendet, um nur die Liste Vorgänge zu filtern.

    azure provider operations show --operationSearchString */apiapps/* --json | jq ".[] | select (.operation | contains(\"list\"))"

Die ResourceId kann angegeben werden, mithilfe der [Funktion ResourceId](#resourceid) oder mit dem Format **{ProviderNamespace} / {ResourceType} / {Ressourcenname}**.

Im folgenden Beispiel wird gezeigt, wie die primären und sekundären Schlüssel von einem Speicherkonto im Abschnitt Ausgaben zurück.

    "outputs": { 
      "listKeysOutput": { 
        "value": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2016-01-01')]", 
        "type" : "object" 
      } 
    } 

Das zurückgegebene Objekt in ListKeys weist das folgende Format:

    {
      "keys": [
        {
          "keyName": "key1",
          "permissions": "Full",
          "value": "{value}"
        },
        {
          "keyName": "key2",
          "permissions": "Full",
          "value": "{value}"
        }
      ]
    }

<a id="providers" />
### <a name="providers"></a>Anbieter

**Anbieter (ProviderNamespace, [ResourceType])**

Gibt Informationen zu einer Ressourcenanbieter und deren unterstützten Ressourcentypen an. Wenn Sie einen Ressourcentyp nicht angeben, gibt die Funktion alle unterstützten Datentypen für die Ressourcenanbieter.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| providerNamespace                  |   Ja    | Namespace des Anbieters
| resourceType                       |   Nein     | Die Art der Ressource innerhalb des angegebenen Namespaces.

Im folgenden Format wird jedes unterstützter Typ zurückgegeben. Anordnung der Matrix ist nicht unbedingt.

    {
        "resourceType": "",
        "locations": [ ],
        "apiVersions": [ ]
    }

Im folgenden Beispiel wird gezeigt, wie die Funktion Anbieter verwenden:

    "outputs": {
        "exampleOutput": {
            "value": "[providers('Microsoft.Storage', 'storageAccounts')]",
            "type" : "object"
        }
    }

<a id="reference" />
### <a name="reference"></a>Bezug

**Verweis (Ressourcenname oder ResourceIdentifier; [ApiVersion])**

Gibt ein Objekt, das eine andere Ressource Laufzeitzustand darstellt.

| Parameter                          | Erforderlich | Beschreibung
| :--------------------------------: | :------: | :----------
| Ressourcenname oder resourceIdentifier |   Ja    | Namen oder eindeutiger Bezeichner einer Ressource.
| apiVersion                         |   Nein     | API-Version der angegebenen Ressource. Fügen Sie diesen Parameter aus, wenn die Ressource innerhalb derselben Vorlage nicht bereitgestellt wird.

Die **Verweis** -Funktion seinen Wert aus einem Laufzeitzustand abgeleitet und daher nicht im Variablenabschnitt verwendet werden. Sie können Ausgaben Abschnitt einer Vorlage verwendet werden.

Mithilfe der Funktion Verweis deklarieren Sie implizit, dass eine Ressource von einer anderen Ressource abhängt, wenn die Resource innerhalb derselben Vorlage bereitgestellt wird. Sie müssen nicht auch die Eigenschaft **DependsOn** verwenden. Die Funktion wird nicht ausgewertet, bis die Resource Bereitstellung durchgeführt wurde.

Im folgende Beispiel verweist auf ein Speicherkonto, das in der gleichen Vorlage bereitgestellt wird.

    "outputs": {
        "NewStorage": {
            "value": "[reference(parameters('storageAccountName'))]",
            "type" : "object"
        }
    }

Im folgende Beispiel verweist auf eine Speicher-Konto, die nicht in dieser Vorlage bereitgestellt wird, aber innerhalb der gleichen Ressourcengruppe wie die Ressourcen bereitgestellt wird vorhanden ist.

    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }

Sie können einen bestimmten Wert aus der zurückgegebene Objekt, wie etwa die Blob-Endpunkt-URI, abrufen, wie im folgenden Beispiel dargestellt.

    "outputs": {
        "BlobUri": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

Im folgende Beispiel verweist auf einem Speicherkonto in einer anderen Ressourcengruppe.

    "outputs": {
        "BlobUri": {
            "value": "[reference(resourceId(parameters('relatedGroup'), 'Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

Die Eigenschaften für das Objekt aus der **Verweis** -Funktion zurückgegebene variieren je nach Ressourcenart. Erstellen Sie eine einfache Vorlage, die das Objekt im Abschnitt **gibt** zurückgibt, zum Anzeigen der Eigenschaftennamen und die Werte für einen Ressourcentyp. Wenn Sie eine vorhandene Ressource dieses Typs haben, gibt Ihre Vorlage nur das Objekt ohne neuen Ressourcen bereitstellen. Wenn Sie nicht über eine vorhandene Ressource dieses Typs verfügen, Ihre Vorlage nur diese Art bereitstellt, und gibt das Objekt. Klicken Sie dann andere Vorlagen, die zum Abrufen von Werten dynamisch während der Bereitstellung müssen fügen Sie diese Eigenschaften hinzu. 

<a id="resourcegroup" />
### <a name="resourcegroup"></a>resourceGroup

**resourceGroup()**

Gibt ein Objekt, die aktuelle Ressourcengruppe darstellt. 

Das zurückgegebene Objekt befindet sich in folgendem Format:

    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
      "name": "{resourceGroupName}",
      "location": "{resourceGroupLocation}",
      "tags": {
      },
      "properties": {
        "provisioningState": "{status}"
      }
    }

Im folgende Beispiel wird den Speicherort der Ressource Gruppe so weisen Sie den Speicherort für eine Website verwendet.

    "resources": [
       {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/sites",
          "name": "[parameters('siteName')]",
          "location": "[resourceGroup().location]",
          ...
       }
    ]

<a id="resourceid" />
### <a name="resourceid"></a>resourceId

**ResourceId ([SubscriptionId], [ResourceGroupName], ResourceType, resourceName1, [resourceName2]...)**

Gibt den eindeutigen Bezeichner einer Ressource an. 
      
| Parameter         | Erforderlich | Beschreibung
| :---------------: | :------: | :----------
| subscriptionId    |   Nein     | Standardwert ist das aktuelle Abonnement. Geben Sie diesen Wert, wenn eine Ressource in einem anderen Abonnement abgerufen werden müssen.
| resourceGroupName |   Nein     | Standardwert ist die aktuelle Ressourcengruppe. Geben Sie diesen Wert, wenn eine Ressource in einer anderen Ressourcengruppe abgerufen werden müssen.
| resourceType      |   Ja    | Typ der Ressource, einschließlich Namespace von Anbieter.
| resourceName1     |   Ja    | Name der Ressource.
| resourceName2     |   Nein     | Nächste Ressource Namen Segment Wenn Ressourcen geschachtelt ist.

Sie können diese Funktion verwenden, wenn der Ressourcenname mehrdeutigen oder nicht bereitgestellte innerhalb der gleichen Vorlage ist. Der Bezeichner wird im folgenden Format zurückgegeben:

    /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/{resourceProviderNamespace}/{resourceType}/{resourceName}

Im folgenden Beispiel wird gezeigt, wie die Ressource Ids für eine Website und eine Datenbank abrufen. Die Website, die in einer Ressourcengruppe mit dem Namen **MyWebsitesGroup** vorhanden ist, und die Datenbank vorhanden ist, in der aktuellen Ressourcengruppe für diese Vorlage.

    [resourceId('myWebsitesGroup', 'Microsoft.Web/sites', parameters('siteName'))]
    [resourceId('Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]
    
Häufig müssen Sie diese Funktion verwenden, wenn ein Speicher-Konto oder virtuelle Netzwerk in eine alternative Ressourcengruppe verwenden. Die Speicher-Konto oder virtuelles Netzwerk möglicherweise aus mehreren Ressourcengruppen verwendet werden. Daher möchten Sie nicht beim Löschen einer einzelnen Ressourcengruppe löschen. Im folgende Beispiel wird gezeigt, wie eine Ressource aus einer externen Ressourcengruppe einfach verwendet werden kann:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
          "virtualNetworkName": {
              "type": "string"
          },
          "virtualNetworkResourceGroup": {
              "type": "string"
          },
          "subnet1Name": {
              "type": "string"
          },
          "nicName": {
              "type": "string"
          }
      },
      "variables": {
          "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
          "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
      },
      "resources": [
      {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[parameters('nicName')]",
          "location": "[parameters('location')]",
          "properties": {
              "ipConfigurations": [{
                  "name": "ipconfig1",
                  "properties": {
                      "privateIPAllocationMethod": "Dynamic",
                      "subnet": {
                          "id": "[variables('subnet1Ref')]"
                      }
                  }
              }]
           }
      }]
    }

<a id="subscription" />
### <a name="subscription"></a>Abonnement

**Subscription()**

Gibt Details zu dem Abonnement im folgenden Format an.

    {
        "id": "/subscriptions/#####",
        "subscriptionId": "#####",
        "tenantId": "#####"
    }

Im folgenden Beispiel wird die Abonnement-Funktion im Abschnitt Ausgaben bezeichnet. 

    "outputs": { 
      "exampleOutput": { 
          "value": "[subscription()]", 
          "type" : "object" 
      } 
    } 


## <a name="next-steps"></a>Nächste Schritte
- Eine Beschreibung der Abschnitte in einer Vorlage Azure Ressourcenmanager finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen](resource-group-authoring-templates.md)
- Wenn Sie mehrere Vorlagen verbinden möchten, finden Sie unter [Verwenden von verknüpften Vorlagen Azure Ressourcenmanager](resource-group-linked-templates.md)
- Eine angegebene Anzahl von Zeiten durchlaufen einen Typ von Ressourcen zu erstellen, finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen in Azure Ressourcenmanager](resource-group-create-multiple.md)
- Wenn Sie erfahren, wie die Vorlage bereitstellen, die Sie erstellt haben, finden Sie unter [Bereitstellen einer Anwendung mit Ressourcenmanager Azure-Vorlage](resource-group-template-deploy.md)

