<properties
    pageTitle="Hinzufügen der Microsoft Translator Logik apps | Microsoft Azure"
    description="Übersicht über die Microsoft Translator Verbinder mit REST-API Parameter"
    services=""
    suite=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-microsoft-translator-connector"></a>Erste Schritte mit der Microsoft Translator Verbinder
Verbinden Sie mit Microsoft Translator zum Übersetzen von Text Erkennen einer Sprache und mehr. Mit Microsoft Translator können Sie folgende Aktionen ausführen: 

- Erstellen Sie Ihrer Business Fluss basierend auf den Daten, die Sie von Microsoft Translator zu gelangen. 
- Verwenden von Aktionen zum Übersetzen von Text Erkennen einer Sprache und mehr. Diese Aktionen erhalten eine Antwort, und nehmen Sie die Ausgabe dann für andere Aktionen verfügbar. Beim Erstellen eine neue Datei in Dropbox können Sie den Text in der Datei in eine andere Sprache, die mit Microsoft Translator übersetzen.

Um einen Vorgang in Logik apps hinzuzufügen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Microsoft Translator umfasst die folgenden Aktionen aus. Es gibt keine Trigger aus.

Trigger | Aktionen
--- | ---
Keine | <ul><li>Erkennen der Sprache</li><li>Text in Sprache</li><li>Text übersetzen</li><li>Abrufen von Sprachen</li><li>Abrufen von Speech Sprachen</li></ul>

Alle Connectors unterstützen die Daten in JSON und XML-Formaten.


## <a name="create-a-connection-to-microsoft-translator"></a>Herstellen einer Verbindung mit Microsoft Translator

>[AZURE.INCLUDE [Steps to create a connection to Microsoft Translator](../../includes/connectors-create-api-microsofttranslator.md)]


## <a name="swagger-rest-api-reference"></a>Swagger REST-API-Referenz
Version gilt: 1.0.

### <a name="detect-language"></a>Erkennen der Sprache    
Erkennt Quelle Sprache von Text angegeben.  
```GET: /Detect```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Abfrage|Zeichenfolge|Ja|Abfrage|keine |Text, dessen Sprache angegeben wird|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="text-to-speech"></a>Text in Sprache    
Konvertiert einen angegebenen Text in Sprache als Audiodatenstrom Welle Format an.  
```GET: /Speak```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Abfrage|Zeichenfolge|Ja|Abfrage|keine |Umwandeln von Text in|
|Sprache|Zeichenfolge|Ja|Abfrage|keine |Sprachcode generieren Sprache (Beispiel: "En-us')|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="translate-text"></a>Text übersetzen    
Wandelt Text in eine bestimmte Sprache Microsoft Translator verwenden.  
```GET: /Translate```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Abfrage|Zeichenfolge|Ja|Abfrage|keine |Übersetzen von Text|
|languageTo|Zeichenfolge|Ja|Abfrage| keine|Zielsprache Code (Beispiel: "Französisch")|
|languageFrom|Zeichenfolge|Nein|Abfrage|keine |Quelle Sprache. Wenn nicht angegeben, versucht Microsoft Translator automatisch erkennen. (Beispiel: En)|
|Kategorie|Zeichenfolge|Nein|Abfrage|Allgemeine |Übersetzung Kategorie (Standard: "Allgemein")|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="get-languages"></a>Abrufen von Sprachen    
Ruft alle Microsoft Translator unterstützten Sprachen.  
```GET: /TranslatableLanguages```

Es gibt keine Parameter für diesen Anruf. 

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="get-speech-languages"></a>Abrufen von Speech Sprachen    
Ruft die Sprachen für die Sprachsynthese zur Verfügung.  
```GET: /SpeakLanguages``` 

Es gibt keine Parameter für diesen Anruf.

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|

## <a name="object-definitions"></a>Objektdefinitionen

#### <a name="language-language-model-for-microsoft-translator-translatable-languages"></a>Sprache: Sprachmodell für Microsoft Translator übersetzt Sprachen

|Eigenschaftsname | Datentyp | Erforderlich|
|---|---|---|
|Code|Zeichenfolge|Nein|
|Namen|Zeichenfolge|Nein|


## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

Kehren Sie zu der [Liste der APIs](apis-list.md).


<!--References-->
[5]: https://datamarket.azure.com/developer/applications/
[6]: ./media/connectors-create-api-microsofttranslator/register-your-application.png
