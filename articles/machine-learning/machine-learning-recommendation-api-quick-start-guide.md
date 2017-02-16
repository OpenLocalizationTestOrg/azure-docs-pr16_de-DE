<properties 
    pageTitle="Schnellstarthandbuch: Computer Learning Empfehlungen-API | Microsoft Azure" 
    description="Azure maschinellen Learning Empfehlungen – Schnellstarthandbuch" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-machine-learning-recommendations-api"></a>Schnellstarthandbuch für den maschinellen Learning Empfehlungen-API

>[AZURE.NOTE]Starten Sie mithilfe von Empfehlungen-API kognitive Service statt dieser Version. Kognitive Empfehlungen Dienst ersetzt werden soll diesen Dienst, und alle neuen Features werden es entwickelt werden. Es hat neue Funktionen wie Batchverarbeitung Support, ein besseres API-Explorer, eine übersichtlichere API einbinden, konsistenter beim Registrieren/Abrechnung Erfahrung usw. aus.
> Weitere Informationen zum [Migrieren zu den neuen kognitive Dienst](http://aka.ms/recomigrate)


Dieses Dokument beschreibt, wie zur integrierten Ihres Dienst oder einer Anwendung mit Microsoft Azure maschinellen Learning Empfehlungen. Weitere Informationen hierzu finden Sie auf der Empfehlungen-API im [Katalog](http://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="general-overview"></a>Allgemeine Übersicht

Wenn Azure maschinellen Learning Empfehlungen verwenden möchten, müssen Sie die folgenden Schritte durchführen:

* Erstellen eines Modells – ein Modell ist ein Container für Ihre Verwendungsdaten, Katalogdaten und Modell Empfehlungen.
* Importieren von Katalogdaten - Katalog enthält Metadateninformationen über die Elemente an. 
* Importieren Verwendungsdaten - Verwendungsdaten können 2 Arten (oder beides) hochgeladen werden:
    * Durch Hochladen einer Datei, die die Verwendungsdaten enthält.
    * Per Daten Acquisition Ereignisse.
    Normalerweise hochladen Sie eine Datei für die Verwendung und möglicherweise ein Modell anfänglichen Empfehlungen (bootstrap) erstellen und diese verwenden, bis das System genügend Daten mithilfe des Acquisition Datenformats sammelt.
* Erstellen eines Modells Empfehlungen: Dies ist eine asynchrone Operation, in der das System Empfehlungen nimmt die Verwendungsdaten und erstellt ein Empfehlungen-Modell. Dieser Vorgang kann einige Minuten oder mehrere Stunden dauern, abhängig von der Größe der Daten und die eigene Konfigurationsparameter dauern. Wenn Sie den Build auslösen, erhalten Sie eine eigene ID. Verwenden Sie diese um zu aktivieren, wenn der Buildvorgang beendet ist, bevor Sie anfangen, Empfehlungen nutzen.
* Nutzen Sie Empfehlungen - Get-Empfehlungen für ein bestimmtes Element oder eine Liste mit Elementen an.

Die oben aufgeführten Schritte sind über die Azure maschinellen Learning Empfehlungen-API fertig.  Sie können eine Stichprobe-Anwendung, die einzelnen Schritte aus implementiert Herunterladen der [Katalog sowie.](http://1drv.ms/1xeO2F3)

##<a name="limitations"></a>Einschränkungen

* Die maximale Anzahl von Datenmodellen pro Abonnement ist 10.
* Die maximale Anzahl von Elementen, die ein Katalog enthalten kann ist 100,000.
* Die maximale Anzahl von Punkten Verwendung, die aufbewahrt werden, ist ~ 5.000.000. Die älteste wird gelöscht werden, wenn neue hochgeladen oder gemeldet werden.
* Die maximale Größe von Daten, die in Beitrag (z. B. Katalogdaten importieren, importieren Verwendungsdaten) gesendet werden können, ist 200MB.
* Ist die Anzahl der Transaktionen pro Sekunde für die Erstellung eines Empfehlungen-Modells, die nicht aktiv ist ~ 2TPS. Die Erstellung eines Empfehlungen-Modells, das aktiv ist kann für 20TPS enthalten, nach oben.

##<a name="integration"></a>Integration

###<a name="authentication"></a>Authentifizierung
Microsoft Azure Marketplace unterstützt Grundlagen "oder" OAuth Authentifizierungsmethode. Sie können einfach die Taste Konto suchen, durch den Tasten in der Marketplace unter [Einstellungen für Ihr Konto](https://datamarket.azure.com/account/keys)navigieren. 
####<a name="basic-authentication"></a>Standardauthentifizierung
Fügen Sie der Kopfzeile Autorisierung hinzu:

    Authorization: Basic <creds>
               
    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  
    
Konvertieren in Base64 (c#)

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);
    
Konvertieren in Base64 (JavaScript)

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);
    



###<a name="service-uri"></a>Dienst-URI
Der Dienst Stamm-URI für die Azure maschinellen Learning Empfehlungen APIs ist [hier.](https://api.datamarket.azure.com/amla/recommendations/v2/)

Der vollständige Dienst-URI wird mithilfe der OData-Spezifikation Elemente ausgedrückt.

###<a name="api-version"></a>API-version
Jede API Anruf haben, am Ende, einen Abfrageparameter aufgerufen ApiVersion, die auf "1.0" festgelegt werden soll.

###<a name="ids-are-case-sensitive"></a>IDs Groß-/Kleinschreibung
IDs, die APIs zurückgegebene Groß-/Kleinschreibung und sollte als solche verwendet werden, wenn als Parameter in weiteren API-Aufrufe übergeben. Modell-IDs und Katalog-IDs sind z. B. Groß-/Kleinschreibung beachtet.

###<a name="create-a-model"></a>Erstellen eines Modells
Erstellen eine Besprechungsanfrage "Modell erstellen":

| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelName   |   Nur Buchstaben (A-Z, a-Z), Zahlen (0-9), Bindestriche (--) und Unterstrichs (_) zulässig sind.<br>Die Maximallänge: 20 |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | KEINE |


**Antwort**:

HTTP-Status Code: 200

- `feed/entry/content/properties/id`-Enthält die Modell-ID.
**Hinweis**: Modell-ID wird Groß-/Kleinschreibung beachtet.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


###<a name="import-catalog-data"></a>Importieren von Daten im Katalog

Wenn Sie mehrere Katalogdateien in der gleichen Modell mit mehreren Anrufe hochladen, werden wir die neuen Katalogelemente einfügen. Vorhandene Elemente werden mit den ursprünglichen Werten bleiben.

| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId |   Eindeutiger Bezeichner des Modells (Groß-/Kleinschreibung wird beachtet)  |
| Dateiname | Textbasierte Bezeichner des Katalogs.<br>Nur Buchstaben (A-Z, a-Z), Zahlen (0-9), Bindestriche (--) und Unterstrichs (_) zulässig sind.<br>Die Maximallänge: 50 |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | Daten des Katalogs. Format:<br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th>Namen</th><th>Obligatorisch</th><th>Typ</th><th>Beschreibung</th></tr><tr><td>Listenelement-Id</td><td>Ja</td><td>Alphanumerische, maximale Länge 50</td><td>Eindeutige ID eines Elements</td></tr><tr><td>Elementnamen</td><td>Ja</td><td>Alphanumerische, maximale Länge von 255 Zeichen</td><td>Elementnamen</td></tr><tr><td>Element-Kategorie</td><td>Ja</td><td>Alphanumerische, maximale Länge von 255 Zeichen</td><td>Kategorie, zu dem dieses Element (z. B. Cooking Bücher, Filmen...) gehört</td></tr><tr><td>Beschreibung</td><td>Nein</td><td>Alphanumerische, maximale Länge 4000</td><td>Beschreibung dieses Artikels</td></tr></table><br>Maximale Dateigröße ist 200MB.<br><br>Beispiel:<br><pre>2406e770-769c-4189-89de-1c9283f93a96, Clara Callan, Adressbuch<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, den Chatroom vergessen: eine Idee (Byzantium Buch), die Bücher<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, Adressbuch<br>552a1940-21e4-4399-82bb-594b46d7ed54, Führung der Raubtiere, Adressbuch</pre> |


**Antwort**:

HTTP-Status Code: 200

- `Feed\entry\content\properties\LineCount`-Zeilenanzahl akzeptiert.
- `Feed\entry\content\properties\ErrorCount`-Anzahl der Zeilen, die nicht aufgrund eines Fehlers eingefügt wurden.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
        <subtitle type="text">Import catalog file</subtitle>
        <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
     </entry>
    </feed>


###<a name="import-usage-data"></a>Importieren von Verwendungsdaten

####<a name="uploading-a-file"></a>Hochladen einer Datei
In diesem Abschnitt wird gezeigt, wie Daten zur Verwendung mit einer Datei hochladen. Sie können diese API mit Verwendungsdaten mehrfach aufrufen. Für alle Anrufe werden alle Verwendungsdaten gespeichert werden.

| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId |   Eindeutiger Bezeichner des Modells (Groß-/Kleinschreibung wird beachtet) |
| Dateiname | Textbasierte Bezeichner des Katalogs.<br>Nur Buchstaben (A-Z, a-Z), Zahlen (0-9), Bindestriche (--) und Unterstrichs (_) zulässig sind.<br>Die Maximallänge: 50 |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | Verwendungsdaten. Format:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Namen</th><th>Obligatorisch</th><th>Typ</th><th>Beschreibung</th></tr><tr><td>Benutzer-Id</td><td>Ja</td><td>Alphanumerische</td><td>Eindeutige ID eines Benutzers nachschlagen</td></tr><tr><td>Listenelement-Id</td><td>Ja</td><td>Alphanumerische, maximale Länge 50</td><td>Eindeutige ID eines Elements</td></tr><tr><td>Zeit</td><td>Nein</td><td>Datum im Format: JJJJ/MM/TTThh (z. B. 2013/06/20T10:00:00)</td><td>Zeitpunkt der Daten</td></tr><tr><td>Ereignis</td><td>Nein, wenn bereitgestellt hat, und klicken Sie dann muss auch Datum setzen</td><td>Eine der folgenden Optionen:<br>• Klicken Sie auf<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Kauf</td><td></td></tr></table><br>Maximale Dateigröße ist 200MB.<br><br>Beispiel:<br><pre>149452, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951, 1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Antwort**:

HTTP-Status Code: 200

- `Feed\entry\content\properties\LineCount`-Zeilenanzahl akzeptiert.
- `Feed\entry\content\properties\ErrorCount`-Anzahl der Zeilen, die nicht aufgrund eines Fehlers eingefügt wurden.
- `Feed\entry\content\properties\FileId`-Datei Bezeichner enthält.


OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add bulk usage data (usage file)</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
    </entry>
    </feed>


####<a name="using-data-acquisition"></a>Verwenden von Daten acquisition
In diesem Abschnitt veranschaulicht, wie Ereignisse in Echtzeit an Azure maschinellen Learning Empfehlungen, in der Regel von der Website zu senden.

| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
|Anforderungstexts| Dateneingabe für jedes Ereignis Ereignis, die Sie senden möchten. Sie sollten für Benutzer oder Browser derselben Sitzung dieselbe ID im Feld SessionId senden. (Siehe Beispiel für Ereignis Textkörper unten).|


- Beispiel für das Ereignis 'Auf':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Beispiel für das Ereignis 'RecommendationClick':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RecommendationClick</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Beispiel für das Ereignis 'AddShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Beispiel für das Ereignis 'RemoveShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RemoveShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Beispiel für das Ereignis 'Kaufen':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>

- Beispiel '2 Ereignisse auf' und 'AddShopCart' senden:

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        <ItemName>itemName</ItemName>
        <ItemDescription>item description</ItemDescription>
        <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
        </EventData>
        </Event>

**Antwort**: HTTP-Status Code: 200

###<a name="build-a-recommendation-model"></a>Erstellen eines Empfehlungen-Modells

| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells (Groß-/Kleinschreibung wird beachtet)  |
| userDescription | Textbasierte Bezeichner des Katalogs. Beachten Sie, wenn Sie Leerzeichen verwenden Sie mit % 20 stattdessen codiert werden müssen. Finden Sie unter oben genannten Beispiel.<br>Die Maximallänge: 50 |
| apiVersion | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

Dies ist eine asynchrone API. Sie erhalten eine eigene ID als Antwort. Sie erhalten Sie, wenn der Build beendet wurde, rufen Sie die API "Get erstellt Status der ein Model", und suchen Sie diese Generator-ID in der Antwort. Beachten Sie, dass ein Build zu Stunden abhängig von der Größe der Daten Minuten dauern kann.

Sie können keine Empfehlungen bis zum den Build nutzen endet.

Status für gültige erstellen:

- Erstellen – Modell erstellt wurde.
- In der Warteschlange – Modellbuild ausgelöst wurde, und es wird in der Warteschlange.
- Gebäude – Modell wird erstellt.
- Erfolg – erstellen wurde erfolgreich beendet.
- Fehler – beendet erstellen mit einem Fehler an.
- Abgebrochen – wurde Build abgebrochen.
- Absagen – Build abgebrochen wird.


Beachten Sie, dass die ID erstellen unter den folgenden Pfad gefunden werden kann:`Feed\entry\content\properties\Id`

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
    </entry>
    </feed>

###<a name="get-build-status-of-a-model"></a>Erstellen eines Modells Status abrufen

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27`|



|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId         |   Eindeutiger Bezeichner des Modells (Groß-/Kleinschreibung wird beachtet)    |
|   onlyLastBuild   |   Gibt an, ob den Build Verlauf des Modells oder nur den Status des letzten Build zurückgeben. |
|   apiVersion      |   1.0                                 |


**Antwort**:

HTTP-Status Code: 200

Die Antwort enthält einen Eintrag pro erstellen. Jeder Eintrag weist die folgenden Daten:

- `feed/entry/content/properties/UserName`– Name des Benutzers.
- `feed/entry/content/properties/ModelName`– Name des Modells.
- `feed/entry/content/properties/ModelId`– Modellieren Sie eindeutige ID.
- `feed/entry/content/properties/IsDeployed`– Gibt an, ob die erstellen (auch bekannt als bereitgestellt wird aktive Build).
- `feed/entry/content/properties/BuildId`– Erstellen Sie eindeutige ID ein.
- `feed/entry/content/properties/BuildType`-Typ von der erstellen.
- `feed/entry/content/properties/Status`– Erstellen Sie Status. Kann eine der folgenden: Fehler, Gebäude, in Warteschlange, Cancelling, abgebrochen, Erfolg
- `feed/entry/content/properties/StatusMessage`– Detaillierte Statusmeldung (gilt nur für bestimmte Staaten).
- `feed/entry/content/properties/Progress`– Erstellen Sie den Fortschritt (%).
- `feed/entry/content/properties/StartTime`– Erstellen Sie Startzeit.
- `feed/entry/content/properties/EndTime`– Erstellen Sie Endzeit.
- `feed/entry/content/properties/ExecutionTime`– Erstellen Sie Dauer.
- `feed/entry/content/properties/ProgressStep`– Details der aktuellen Stufe, der erstellen wird ausgeführt wird.

Status für gültige erstellen:
- Erstellt – wurde Build Anforderungseintrag erstellt.
- In der Warteschlange – Buildanforderung ausgelöst wurde und sie in der Warteschlange ist.
- Gebäude – erstellen wird ausgeführt.
- Erfolg – erstellen wurde erfolgreich beendet.
- Fehler – beendet erstellen mit einem Fehler an.
- Abgebrochen – wurde Build abgebrochen.
- Absagen – Build abgebrochen wird.

Gültige Werte für Typ erstellen:
- Rang - Rang erstellen. (Erstellen Sie Details für Rang, Lizenzinformationen finden Sie im Dokument "Dokumentation maschinellen Learning Empfehlungen API").
- Empfehlungen - Empfehlungen erstellen.
- FBT - häufig gekauft zusammen erstellen.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


###<a name="get-recommendations"></a>Lassen Sie sich

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|



|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells (Groß-/Kleinschreibung wird beachtet) |
| Artikelnummern ein. | Durch Trennzeichen getrennte Liste der Elemente empfehlen.<br>Die Maximallänge: 1024 |
| numberOfResults | Anzahl der Ergebnisse erforderlich |
| includeMetatadata | Eine zukünftige Verwendung, immer false |
| apiVersion | 1.0 |

**Antwort:**

HTTP-Status Code: 200

Die Antwort enthält einen Eintrag pro Element empfohlen. Jeder Eintrag weist die folgenden Daten:

- `Feed\entry\content\properties\Id`-Empfohlene Element-ID an.
- `Feed\entry\content\properties\Name`-Name des Elements.
- `Feed\entry\content\properties\Rating`-Bewertung von empfohlen; höherer Zahl bedeutet höhere KONFIDENZ.
- `Feed\entry\content\properties\Reasoning`-Empfehlungen Schlussfolgerungen (z. B. Empfehlungen erläuterungen).

OData-XML

Im folgenden Beispielantwort umfasst 10 empfohlene Elemente:

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
    </feed>

###<a name="update-model"></a>Aktualisierungsmodell
Sie können die Beschreibung Modell oder im aktiven Generator-ID aktualisieren.
*Aktiven Generator-ID* – jeder Build für jedes Modell verfügt über eine eigene ID. Die aktive Generator-ID wird dem ersten erfolgreichen Build für jedes neue Modell. Nachdem Sie haben eine ID aktiven erstellen, und führen Sie Sie für das gleiche Modell zusätzliche erstellt, müssen Sie explizit es als Standard-Generator-ID festlegen, wenn Sie möchten. Wenn Sie Empfehlungen, nutzen, wenn Sie nicht die Generator-ID angeben, die Sie verwenden möchten, wird automatisch eine Standard verwendet werden.

Dieses Verfahren ermöglicht es Ihnen - Nachdem Sie ein Modell Empfehlungen Herstellung - zum Erstellen neuer Modelle und testen, bevor Sie diese auf der Herstellung heraufstufen haben.

| HTTP-Methode | URI |
|:--------|:--------|
|SETZEN     |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27`|


|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| ID | Eindeutiger Bezeichner des Modells (Groß-/Kleinschreibung wird beachtet) |
| apiVersion | 1.0 |
|||
| Anforderungstexts | `<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br>Beachten Sie, dass die XML-Tags, Beschreibung und ActiveBuildId optional sind. Wenn Sie keine Beschreibung oder ActiveBuildId festlegen möchten, entfernen Sie das gesamte Tag. |

**Antwort**:

HTTP-Status Code: 200

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Update an Existing Model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
    <rights type="text" />
     <updated>2014-10-05T10:27:17Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

##<a name="legal"></a>Rechtliche Hinweise
Dieses Dokument wird bereitgestellt "als-wird". Informationen und Ansichten, ausgedrückt in diesem Dokument, einschließlich URL und andere Verweise auf Internetwebsites, können ohne vorherige Ankündigung geändert werden. Einige Beispiele, die in diesen Beispielen werden nur Abbildung und fiktives sind. Keine Assoziierung oder Verbindung vorgesehen ist, oder sollte abgeleitet werden. Dieses Dokument stellt Sie keine Rechte an geistiges Eigentum in ein Microsoft-Produkt nicht bereit. Können Sie kopieren und verwenden Sie dieses Dokument zu internen Referenzzwecken. © Microsoft 2014. Alle Rechte vorbehalten. 
 
