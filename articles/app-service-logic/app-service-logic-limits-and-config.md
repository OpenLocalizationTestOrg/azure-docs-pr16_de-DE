<properties
    pageTitle="Logik App Grenzwerte und Konfiguration | Microsoft Azure"
    description="Übersicht über die Beschränkungen Service und Konfigurationswerte für Logik Apps verfügbar."
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="logic-app-limits-and-configuration"></a>Logik App Grenzwerte und Konfiguration

Nachstehend sind Informationen zu den aktuellen Grenzwerte und Details zur Konfiguration für Azure Logik Apps.

## <a name="limits"></a>Grenzwerte

### <a name="http-request-limits"></a>HTTP-Anforderung Beschränkungen

Dies sind die Grenzwerte für einen einzelnen HTTP-Anforderung und/oder Connector-Anruf

#### <a name="timeout"></a>Timeout

|Namen|Grenzwert|Notizen|
|----|----|----|
|Timeout für die Anforderung|1 minute|Ein [Muster asynchronen](app-service-logic-create-api-app.md) oder [bis Schleife](app-service-logic-loops-and-scopes.md) können Bedarf kompensieren|

#### <a name="message-size"></a>Nachrichtengröße

|Namen|Grenzwert|Notizen|
|----|----|----|
|Nachrichtengröße|50 MB|Einige Verbinder und APIs möglicherweise nicht 50 MB unterstützen.  Anforderung Trigger unterstützt bis zu 25MB|
|Ausdruck Auswertung Grenzwert|131.072 Zeichen|`@concat()`, `@base64()`, `string` kann nicht länger als dies|

#### <a name="retry-policy"></a>Wiederholen Sie die Richtlinie

|Namen|Grenzwert|Notizen|
|----|----|----|
|Wiederholungsversuche|4|Konfigurieren können mit der [Richtlinienparameter wiederholen](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Wiederholen Sie die max-Verzögerung|1 Stunde|Konfigurieren können mit der [Richtlinienparameter wiederholen](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Wiederholen Sie min Verzögerung|20 Min.|Konfigurieren können mit der [Richtlinienparameter wiederholen](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|

### <a name="run-duration-and-retention"></a>Dauer des Testlaufs und Aufbewahrungsrichtlinien

Dies sind die Grenzwerte für eine einzelne Logik app ausführen.

|Namen|Grenzwert|Notizen|
|----|----|----|
|Führen Sie die Dauer|90 Tage||
|-Aufbewahrung|90 Tage|Dies ist von der Enduhrzeit ausführen|
|Min Serienintervall|15 Sekunden||
|Max Serienintervall|500 Tage||


### <a name="looping-and-debatching-limits"></a>Endloswiedergabe und vorgelagerte Beschränkungen

Dies sind die Grenzwerte für eine einzelne Logik app ausführen.

|Namen|Grenzwert|Notizen|
|----|----|----|
|ForEach-Elemente|5.000|Die [Abfrage Aktion](../connectors/connectors-native-query.md) können Sie größere Arrays Bedarf filtern|
|Bis zu Iterationen|10.000||
|SplitOn Elemente|5.000||
|ForEach Parallelität|20|Sie können festlegen, um einen sequenziellen Foreach durch Hinzufügen von `"operationOptions": "Sequential"` zu den `foreach` Aktion|


### <a name="throughput-limits"></a>Grenzwerte für Durchsatz

Dies sind die Grenzwerte für eine Instanz der einzelnen Logik app. 

|Namen|Grenzwert|Notizen|
|----|----|----|
|Trigger pro Sekunde|100|Workflows können auf mehrere apps verteilen werden, je nach Bedarf|

### <a name="definition-limits"></a>Definition Beschränkungen

Dies sind die Grenzwerte für eine einzelne Logik app Definition.

|Namen|Grenzwert|Notizen|
|----|----|----|
|Aktionen in ForEach|1|Sie können geschachtelte Workflows, um dies zu erweitern, je nach Bedarf hinzufügen.|
|Aktionen pro workflow|60|Sie können geschachtelte Workflows, um dies zu erweitern, je nach Bedarf hinzufügen.|
|Zulässige Aktion Verschachtelungsebene|5|Sie können geschachtelte Workflows, um dies zu erweitern, je nach Bedarf hinzufügen.|
|Je nach Region pro Abonnement Zahlungen|1000||
|Trigger pro workflow|10||
|Max Zeichen pro Ausdruck|8192 fest||
|Max `trackedProperties` Umfang in Zeichen|16.000|
|`action`/`trigger`Grenzwert für Namen|80||
|`description`maximale Zeichenfolgenlänge|256||
|`parameters`Grenzwert|50||
|`outputs`Grenzwert|10||

## <a name="configuration"></a>Konfiguration

### <a name="ip-address"></a>IP-Adresse

Anrufe von einem [Verbinder](../connectors/apis-list.md) werden die IP-Adresse, die unten angegebene stammen.

Anrufe aus einer Logik app direkt (d. h. über [HTTP](../connectors/connectors-native-http.md) oder [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)) können von einem beliebigen der [Azure Datacenter IP-Bereiche](https://www.microsoft.com/en-us/download/details.aspx?id=41653)stammen.

|Logik App Region|Ausgehende IP|
|-----|----|
|Australien OST|40.126.251.213|
|Australien oder|40.127.80.34|
|Brasilien Süd|191.232.38.129|
|Zentrale Indien|104.211.98.164|
|USA – zentral|40.122.49.51|
|Ostasien|23.99.116.181|
|Ostasiatische US|191.237.41.52|
|Ostasiatische USA 2|104.208.233.100|
|Japan OST|40.115.186.96|
|Japan "Westen"|40.74.130.77|
|Nord-zentralen US|65.52.218.230|
|North Europa|104.45.93.9|
|Süd zentralen US|104.214.70.191|
|Oder Asien|13.76.231.68|
|Süd Indien|104.211.227.225|
|Westen Europa|40.115.50.13|
|Westen Indien|104.211.161.203|
|Westen US|104.40.51.248|


## <a name="next-steps"></a>Nächste Schritte  

- Um mit Logik Apps anzufangen, führen Sie das Lernprogramm [Erstellen Sie eine App Logik](app-service-logic-create-a-logic-app.md) aus.  
- [Allgemeine Beispiele anzeigen und Szenarien](app-service-logic-examples-and-scenarios.md)
- [Sie können Automatisieren von Geschäftsprozessen mit Logik Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Erfahren Sie, wie Ihre Systeme mit Logik Apps integriert werden soll.](http://channel9.msdn.com/Events/Build/2016/P462)