<properties
 pageTitle="Leitfaden für Entwickler - Abfragesprache | Microsoft Azure"
 description="Azure IoT Hub Entwicklertools Leitfaden – Beschreibung der Abfragesprache zum Abrufen von Informationen im Vergleich, Methoden und Aufträge aus Ihrer IoT Hub verwendet"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="reference---query-language-for-twins-and-jobs"></a>Verweis - Abfragesprache für Projekte und im Vergleich

## <a name="overview"></a>(Übersicht)

IoT Hub bietet eine leistungsstarke SQL-ähnliche Sprache zum Abrufen von Informationen zu dem [Gerät im Vergleich] [ lnk-twins] und [Aufträge][lnk-jobs]. Dieser Artikel erläutert:

* Eine Einführung in die wichtigsten Features von IoT-Hub-Abfragesprache und
* Die detaillierte Beschreibung der Sprache.

## <a name="getting-started-with-twin-queries"></a>Erste Schritte mit zwei Abfragen

[Gerät im Vergleich] [ lnk-twins] können beliebige JSON-Objekte als Kategorien und Eigenschaften enthalten. IoT Hub können Abfrage Gerät im Vergleich als ein einzelnes JSON-Dokument alle zwei Informationen enthalten sind.
Davon aus, z. B., dass Ihre IoT Hub im Vergleich die folgende Struktur haben:

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                "location": {                                                      
                    "region": "US",                                                  
                    "plant": "Redmond43"                                             
                }                                                                  
            },                                                                   
            "properties": {                                                      
                "desired": {                                                       
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300                                          
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                     
                    },                                                               
                    "$version": 4                                                    
                },                                                                 
                "reported": {                                                      
                    "connectivity": {                                                
                        "type": "cellular"                            
                    },                                                               
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300,                                         
                        "status": "Success"                                            
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                
                    },                                                               
                    "$version": 7                                                    
                }                                                                  
            }                                                                    
        }

IoT Hub macht das Gerät im Vergleich als Dokument Auflistung **Geräte**bezeichnet.
Die folgende Abfrage ruft also die gesamte Gruppe von Gerät im Vergleich:

        SELECT * FROM devices

> [AZURE.NOTE] [IoT Hub SDKs] [ lnk-hub-sdks] Seitennavigation großen Ergebnisliste unterstützen.

IoT Hub können im Vergleich mit beliebigen Bedingungen Filtern abrufen. Beispielsweise

        SELECT * FROM devices
        WHERE tags.location.region = 'US'

Ruft die im Vergleich mit dem **location.region** Tag an **uns**festlegen.
Boolesche Operatoren und arithmetische Vergleiche werden ebenfalls, z. B. unterstützt.

        SELECT * FROM devices
        WHERE tags.location.region = 'US'
            AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60

Ruft alle im Vergleich befindet sich in den USA, die so konfiguriert ist, um werden nicht so häufig Minute zu senden. Als verwenden können ist es auch möglich, Matrixkonstanten in Verbindung mit dem **IN** und **NIN** (nicht im) Operatoren verwenden. Beispielsweise

        SELECT * FROM devices
        WHERE property.reported.connectivity IN ['wired', 'wifi']

Ruft alle im Vergleich, die Wifi gemeldet oder Connectivity verknüpft. Schlagen Sie in der [WHERE-Klausel] [ lnk-query-where] Abschnitt für den vollständigen Verweis die Filterfunktionen.

Gruppieren und Aggregationen werden ebenfalls unterstützt. Beispielsweise

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Gibt die Anzahl der Geräte in jeder werden Konfigurationsstatus.

        [
            {
                "numberOfDevices": 3,
                "status": "Success"
            },
            {
                "numberOfDevices": 2,
                "status": "Pending"
            },
            {
                "numberOfDevices": 1,
                "status": "Error"
            }
        ]

Im oben genannten Beispiel veranschaulicht eine Situation, in dem drei Geräte gemeldet erfolgreiche Konfiguration, zwei werden weiterhin die Konfiguration anwenden und eine hat einen Fehler gemeldet.

### <a name="c-example"></a>Beispiel für c#

Die Abfragefunktionalität wird vom [C#-Dienst SDK] bereitgestellt[ lnk-hub-sdks] in der Klasse **RegistryManager** .
Hier ist ein Beispiel für eine einfache Abfrage ein:

        var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
        while (query.HasMoreResults)
        {
            var page = await query.GetNextAsTwinAsync();
            foreach (var twin in page) 
            {
                // do work on twin object
            }
        }

Beachten Sie, wie der Instanziierung des **Abfrage** -Objekts mit einer Seitengröße (maximal 1000), und klicken Sie dann mehrere Seiten durch Aufrufen der Methoden **GetNextAsTwinAsync** mehrmals abgerufen werden können.
Es ist wichtig, beachten Sie, dass das Abfrageobjekt Vielfache macht **nächsten\***, je nach der Deserialisierung-Option erforderlich, indem Sie die Abfrage, d. h. zwei oder beruflichen Position Objekte oder einfarbigen Json beim Verwenden von Projektionen verwendet werden soll.

### <a name="node-example"></a>Beispiel für Knoten

Die Abfragefunktionalität wird durch den [Knoten Service SDK] bereitgestellt[ lnk-hub-sdks] in der **Registrierung** -Objekt.
Hier ist ein Beispiel für eine einfache Abfrage ein:

        var query = registry.createQuery('SELECT * FROM devices', 100);
        var onResults = function(err, results) {
            if (err) {
                console.error('Failed to fetch the results: ' + err.message);
            } else {
                // Do something with the results
                results.forEach(function(twin) {
                    console.log(twin.deviceId);
                });

                if (query.hasMoreResults) {
                    query.nextAsTwin(onResults);
                }
            }
        };
        query.nextAsTwin(onResults);

Beachten Sie, wie der Instanziierung des **Abfrage** -Objekts mit einer Seitengröße (maximal 1000), und klicken Sie dann mehrere Seiten durch Aufrufen der Methoden **NextAsTwin** mehrmals abgerufen werden können.
Es ist wichtig, beachten Sie, dass das Abfrageobjekt Vielfache macht **nächsten\***, je nach der Deserialisierung-Option erforderlich, indem Sie die Abfrage, d. h. zwei oder beruflichen Position Objekte oder einfarbigen Json beim Verwenden von Projektionen verwendet werden soll.

### <a name="limitations"></a>Einschränkungen

Derzeit Projektionen sind nur bei Verwendung von Aggregationen unterstützt, d. h. Abfragen nicht aggregiertes können nur mit `SELECT *`. Darüber hinaus Aggregation werden nur unterstützt in Verbindung mit gruppieren.

## <a name="getting-started-with-jobs-queries"></a>Erste Schritte mit Aufträge Abfragen

[Aufträge] [ lnk-jobs] bieten eine Möglichkeit zum Ausführen von Vorgängen auf Geräte. Jedes Gerät und enthält die Informationen der Einzelvorgänge, die Teil in einer Websitesammlung **Aufträge**genannt ist.
Logisch,

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                ...                                                              
            },                                                                   
            "properties": {                                                      
                ...                                                                 
            },
            "jobs": [
                { 
                    "deviceId": "myDeviceId",
                    "jobId": "myJobId",    
                    "jobType": "scheduleTwinUpdate",            
                    "status": "completed",                    
                    "startTimeUtc": "2016-09-29T18:18:52.7418462",
                    "endTimeUtc": "2016-09-29T18:20:52.7418462",
                    "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
                    "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
                    "outcome": {
                        "deviceMethodResponse": null   
                    }                                         
                },
                ...
            ]                                                             
        }

Derzeit ist dieser Websitesammlung als **devices.jobs** in der Abfragesprache IoT Hub abfragbare.

Um alle Aufträge (vergangener und geplanten) zu gelangen, die ein einzelnes Gerät beeinflussen, können Sie beispielsweise die folgende Abfrage verwendet:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'

Beachten Sie, wie diese Abfrage des Geräte-spezifischen Status (und oftmals der Antwort direkte Methode) für jedes Projekt zurückgegeben bietet.
Es ist es möglich, mit beliebigen boolesche Bedingungen, klicken Sie auf alle Eigenschaften der Objekte in der Auflistung **devices.jobs** zu filtern.
Beispielsweise die folgende Abfrage:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'
            AND devices.jobs.jobType = 'scheduleTwinUpdate'
            AND devices.jobs.status = 'completed'
            AND devices.jobs.createdTimeUtc > '2016-09-01'

Ruft alle fertigen und Aktualisieren von Projekten für Gerät **MyDeviceId** , die nach September 2016 erstellt wurden.

Es ist es möglich, die pro Gerät Aufgabenergebnisse eines einzelnen Auftrags abzurufen.

        SELECT * FROM devices.jobs
        WHERE devices.jobs.jobId = 'myJobId'

### <a name="limitations"></a>Einschränkungen
Abfragen auf **devices.jobs** können derzeit nicht unterstützt werden:

* Projektionen, d. h. nur `SELECT *` möglich ist;
* Regeln, die auf das Gerät und zusätzlich zu den Auftragseigenschaften wie oben gezeigt verweisen.
* Unterschiedlichsten Aggregationen, z. B. Anzahl, Mittelwert, Gruppieren nach.

## <a name="basics-of-an-iot-hub-query"></a>Grundlagen von einer Abfrage IoT-Hub

Jeder Abfrage IoT Hub besteht aus einer SELECT-Anweisung und FROM-Klauseln sowie WHERE (optional) und GROUP BY Klauseln. Klicken Sie auf eine Sammlung von JSON-Dokumenten, z. B. Gerät im Vergleich wird jeder Abfrage ausführen. Die FROM-Klausel zeigt an, der Gruppe von Dokumenten auf (**Geräte** oder **devices.jobs**) durchlaufen werden. Klicken Sie dann den Filter in die WHERE-Klausel angewendet wird. Im Falle von Aggregationen, die Ergebnisse dieses Schritts gruppiert sind, als in die GROUP BY-Klausel und für jede Gruppe angegeben, wird eine Zeile generiert wie in der SELECT-Klausel angegeben sind.

        SELECT <select_list>
        FROM <from_specification>
        [WHERE <filter_condition>]
        [GROUP BY <group_specification>]

## <a name="from-clause"></a>FROM-Klausel

Die **von < From_specification >** -Klausel kann nur zwei Werte annehmen: **von Geräten**Abfrage Gerät im Vergleich oder **von devices.jobs**Abfrage Auftrag pro Gerät Details.

## <a name="where-clause"></a>WHERE-Klausel

Die **, in dem < Filter_condition >** -Klausel ist optional. Es gibt an, die Filterausdrücke, die die JSON-Dokumente in die Sammlung von erfüllen müssen, damit das Ergebnis enthalten sein. Die angegebenen Bedingungen auf "true" ein, um in das Ergebnis eingeschlossen werden muss jedes Dokument JSON ausgewertet werden.

Die zulässigen Bedingungen werden im Abschnitt [Ausdrücke und Bedingungen]beschriebenen[lnk-query-expressions].

## <a name="select-clause"></a>SELECT-Klausel

Die SELECT-Klausel (**< Select_list > auswählen**) ist obligatorisch und gibt an, welche Werte werden aus der Abfrage abgerufen werden. Es gibt an, die JSON-Werte verwendet, um neue JSON-Objekte generieren für jedes Element der gefilterten (und optional gruppierten) Teilmenge der Websitesammlung von einer Projektionsphase generiert ein neues JSON-Objekt, erstellt mit den Werten in der SELECT-Klausel angegeben werden.

Dies ist die Grammatik der SELECT-Klausel:

        SELECT [TOP <max number>] <projection list>

        <projection_list> ::=
            '*'
            | <projection_element> AS alias [, <projection_element> AS alias]+

        <projection_element> :==
            attribute_name
            | <projection_element> '.' attribute_name
            | <aggregate>

        <aggregate> :==
            count(<projection_element>) | count()
            | avg(<projection_element>) | avg()
            | sum(<projection_element>) | sum()
            | min(<projection_element>) | min()
            | max(<projection_element>) | max()

eine Eigenschaft des Dokuments in der Auflistung von JSON, wobei **Attribute_name** bezeichnet. Einige Beispiele für SELECT-Klausel finden Sie in die [Erste Schritte mit zwei Abfragen] [ lnk-query-getstarted] Abschnitt.

Derzeit Auswahl Klauseln anders als **auswählen \* ** werden nur in aggregieren Abfragen auf im Vergleich unterstützt.

## <a name="group-by-clause"></a>GROUP BY-Klausel

Die **GROUP BY < Group_specification >** -Klausel ist ein optionaler Schritt, der nach dem Filter angegeben haben, klicken Sie in der WHERE-Klausel und vor der in der angegebenen Projektion ausgeführt werden kann. Dokumente basierend auf dem Wert, der ein Attribut gruppiert. Diese Gruppen dienen zum Generieren aggregierte Werte in der SELECT-Klausel angegeben sind.

Ein Beispiel für eine Abfrage mit GROUP BY ist:

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Die Syntax der formale für GROUP BY ist:

        GROUP BY <group_by_element>
        <group_by_element> :==
            attribute_name
            | < group_by_element > '.' attribute_name

eine Eigenschaft des Dokuments in der Auflistung von JSON, wobei **Attribute_name** bezeichnet. 

Die GROUP BY-Klausel wird derzeit nur unterstützt, bei der Abfrage im Vergleich.

## <a name="expressions-and-conditions"></a>Ausdrücke und Bedingungen

Auf hoher Ebene, einen *Ausdruck*:

* Wertet eine Instanz eines Typs JSON (d. h. Boolesch, Zahl, Zeichenfolge, Array oder Objekt), und
* Wird durch Bearbeiten von Daten aus dem Gerät JSON-Dokument und Konstanten integrierten Operatoren und Funktionen in Kürze definiert.

*Bedingungen* sind Ausdrücke, die in einen booleschen Wert, d. h. eine beliebige Konstante anders als booleschen Wert **true** , als **falsch** (einschließlich **null**, **nicht definierten**, jede Instanz Objekt oder einem Array, eine beliebige Zeichenfolge und klar der boolesche **falsch gilt**) ausgewertet wird.

Die Syntax für Ausdrücke lautet:

        <expression> ::=
            <constant> |
            attribute_name |
            unary_operator <expression> |
            <expression> binary_operator <expression> |
            <create_array_expression> |
            '(' <expression> ')'

        <constant> ::=
            <undefined_constant>
            | <null_constant> 
            | <number_constant> 
            | <string_constant> 
            | <array_constant> 

        <undefined_constant> ::= undefined
        <null_constant> ::= null
        <number_constant> ::= decimal_literal | hexadecimal_literal
        <string_constant> ::= string_literal
        <array_constant> ::= '[' <constant> [, <constant>]+ ']'

wobei Folgendes gilt:

| Symbol | Definition |
| -------------- | -----------------|
| attribute_name | Jede Eigenschaft des Dokuments in der Auflistung von JSON. |
| UNARY_OPERATOR | Alle unärer Operator gemäß Abschnitt Operatoren. |
| binary_operator | Binäre Operatoren gemäß Abschnitt Operatoren. |
| decimal_literal | Eine Pufferzeiten in Dezimalform ausgedrückt. |
| hexadecimal_literal | Eine Zahl, durch die Zeichenfolge "0 X" gefolgt von einer Zeichenfolge aus hexadezimalen Ziffern ausgedrückt. |
| string_literal | Zeichenfolgenliteralen werden Unicode-Zeichenfolgen durch eine Folge von 0 (null) oder mehr Unicode-Zeichen oder Escape-Zeichenfolgen dargestellt werden. Zeichenfolgenliteralen in einfache Anführungszeichen gesetzt werden (Apostroph: ') oder doppelte Anführungszeichen (Anführungszeichen: "). Escapezeichen zulässig: `\'`, `\"`, `\\`, `\uXXXX` für Unicode-Zeichen durch 4 hexadezimale Ziffern definiert. |

### <a name="operators"></a>Operatoren

Die folgenden Operatoren werden unterstützt:

| Familie | Operatoren |
| -------------- | -----------------|
| Arithmetische | +,-,*,/,% |
| Logisch | UND, ODER, NICHT |
| Vergleich |  =,! =, <>,, < =, > =, <> |

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Ausführen von Abfragen in Ihrer apps mit [IoT Hub SDKs][lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#getting-started-with-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md