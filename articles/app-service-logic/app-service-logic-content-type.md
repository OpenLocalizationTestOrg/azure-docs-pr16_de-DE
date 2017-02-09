<properties
   pageTitle="Logik apps Inhalt geben Behandlung | Microsoft Azure"
   description="Verstehen Sie, wie Apps Logik mit Inhaltstypen am Entwurf und Laufzeit befasst sich"
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
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-content-type-handling"></a>Logik Apps Inhaltstyp Behandlung

Es gibt viele verschiedene Arten von Inhalten, die über eine Logik-App – einschließlich JSON, XML, unformatierte Dateien und binäre Daten übertragen zu können.  Während alle Inhaltstypen unterstützt werden, einige systembedingt von der Logik Apps-Engine verstanden werden, und andere erfordern möglicherweise Umwandlung oder Konvertierungen je nach Bedarf.  Im folgende Artikel wird beschrieben, wie das Modul für die verschiedene Inhaltstypen verarbeitet und wie diese ordnungsgemäß verarbeitet werden können je nach Bedarf.

## <a name="content-type-header"></a>Inhaltstyp-Header

Um einfache beginnen, sehen wir uns den beiden `Content-Types` keine erfordern alle oder explizite Umwandlung zur Verwendung in einer App Logik - `application/json` und `text/plain`.

### <a name="applicationjson"></a>Anwendung/json

Das Workflow-System basiert auf der `Content-Type` Kopfzeile von HTTP Ruft die entsprechende Behandlung zu bestimmen.  Eine Besprechungsanfrage mit dem Inhaltstyp `application/json` gespeichert und als JSON-Objekt verarbeitet werden.  Darüber hinaus kann JSON-Inhalte ohne Umwandlung standardmäßig analysiert werden.  Damit eine Anforderung mit den Inhaltstyp-Header `application/json ` wie folgt aus:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

in einem Workflow mit einem Ausdruck wie analysiert werden konnten `@body('myAction')['foo'][0]` zum Abrufen eines Werts (in diesem Fall `bar`).  Keine weiteren Umwandlung erforderlich ist.  Wenn Sie mit Daten, die JSON ist aber keine Kopfzeile angegeben haben arbeiten, Sie können manuell wandeln Sie ihn bei der Verwendung von JSON der `@json()` (Funktion) (zum Beispiel: `@json(triggerBody())['foo']`).

### <a name="textplain"></a>Text/nur

Ähnlich wie `application/json`, HTTP-Nachrichten erhalten, die mit den `Content-Type` Kopfzeile der `text/plain` des unformatierten Formular gespeichert werden soll.  Darüber hinaus, wenn in einer nachfolgenden Aktionen ohne Umwandlung enthalten die Anfrage gelangen mit einem `Content-Type`: `text/plain` Kopfzeile.  Arbeiten mit einer Flatfile möglicherweise Sie beispielsweise den folgenden HTTP-Inhalt angezeigt:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

as `text/plain`.  Wenn Sie in die nächste Aktion Sie sie als Textkörper einer anderen Anforderung gesendet (`@body('flatfile')`), hätte die Anforderung einer `text/plain` Inhaltstyp-Header.  Wenn Sie mit Daten, die nur-Text ist, aber keine Kopfzeile angegeben haben arbeiten, Sie können manuell Umwandlung in Text mithilfe der `@string()` (Funktion) (zum Beispiel: `@string(triggerBody())`)

### <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Anwendung/Xml und Application/Octet-Stream und Konverter-Funktionen

Die App-Engine Logik behält stets den `Content-Type` , die auf dem HTTP-Anforderung oder die Antwort empfangen wurde.  Was dies bedeutet ist, wenn Sie ein Inhaltstyp mit eingeht `Content-Type` von `application/octet-stream`, einschließlich, die in einer nachfolgenden Aktion mit keine Umwandlung führt zu Fehlern in einer ausgehenden Anforderung mit `Content-Type`: `application/octet-stream`.  Auf diese Weise die-Engine können Guaruntee Daten nicht verloren, wie es in der gesamten Workflows verschoben wird.  Jedoch der Aktionszustand (Eingaben und Ausgaben) werden gespeichert ein JSON-Objekt wie er überall auf den Workflow fließt.  Dies bedeutet, dass einige Datentypen beibehalten, die-Engine werde ich den Inhalt in eine binäre base64-codierte Zeichenfolge mit entsprechenden Metadaten, die beide behält konvertieren `$content` und `$content-type` – die automatisch konvertiert werden.  Sie können auch manuell konvertieren zwischen Inhaltstypen sich Konverter Funktionen verwenden:

* `@json()`-Wandelt Daten`application/json`
* `@xml()`-Wandelt Daten`application/xml`
* `@binary()`-Wandelt Daten`application/octet-stream`
* `@string()`-Wandelt Daten`text/plain`
* `@base64()`-Inhalte in eine base64-Zeichenfolge konvertiert
* `@base64toString()`– Konvertiert eine base64-codierte Zeichenfolge in`text/plain`
* `@base64toBinary()`– Konvertiert eine base64-codierte Zeichenfolge in`application/octet-stream`
* `@encodeDataUri()`-codiert eine Zeichenfolge als Byte-Array DataUri
* `@decodeDataUri()`-eine DataUri in einem Array von Bytes decodiert

Wenn Sie eine HTTP-Anforderung mit erhalten beispielsweise `Content-Type`: `application/xml` von:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Kann ich umgewandelt und verwenden Sie später mit ungefähr wie folgt `@xml(triggerBody())`, oder innerhalb einer Funktion wie `@xpath(xml(triggerBody()), '/CustomerName')`.

### <a name="other-content-types"></a>Andere Inhaltstypen

Andere Inhaltstypen werden unterstützt und mit einer App Logik funktionieren, aber manuell abrufen von Nachrichtentext, die beim Decodieren erfordern möglicherweise die `$content`.  Beispielsweise, wenn ich das Deaktivieren von auslösen wurden einer `application/x-www-url-formencoded` Anforderung, die vergeblich wie folgt aus:

```
CustomerName=Frank&Address=123+Avenue
```

Da dies nicht nur-Text- oder JSON Sie in der Aktion als gespeichert werden werden:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Wo `$content` ist die Nutzlast codierte als Zeichenfolge base64 alle Daten beibehalten.  Da es zurzeit keine systemeigene für Formulardaten Funktion, kann ich diese Daten innerhalb eines Workflows trotzdem verwenden, indem Sie manuell Zugriff auf die Daten mit einer Funktion, wie `@string(body('formdataAction'))`.  Wenn ich meine ausgehenden Besprechungsanfrage zu außerdem unerwünscht der `application/x-www-url-formencoded` Inhaltstyp-Header, ich konnte nur Lizenz zum Textkörper Aktion ohne Umwandlung wie `@body('formdataAction')`.  Dies nur funktioniert jedoch Textkörper ist der einzige Parameter in die `body` Eingabe.  Wenn Sie versuchen, gehen Sie wie folgt `@body('formdataAction')` innerhalb von einer `application/json` anfordern, erhalten Sie einen Laufzeitfehler als codierten Textkörper Sendezeitpunkt.
