<properties 
    pageTitle="Rechner Learning Empfehlungen API Dokumentation | Microsoft Azure" 
    description="Azure maschinellen Learning Empfehlungen API-Dokumentation für ein Empfehlungen-Engine verfügbar in Microsoft Azure Marketplace." 
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
    ms.author="LuisCa"/>

#<a name="azure-machine-learning-recommendations-api-documentation"></a>Learning Empfehlungen API Dokumentation Azure-Computern

>[AZURE.NOTE]Starten Sie mithilfe von Empfehlungen-API kognitive Service statt dieser Version. Kognitive Empfehlungen Dienst ersetzt werden soll diesen Dienst, und alle neuen Features werden es entwickelt werden. Es hat neue Funktionen wie Batchverarbeitung Support, ein besseres API-Explorer, eine übersichtlichere API einbinden, konsistenter beim Registrieren/Abrechnung Erfahrung usw. aus.
> Weitere Informationen zum [Migrieren zu den neuen kognitive Dienst](http://aka.ms/recomigrate)

Dieses Dokument Seitenansichtsmodus Microsoft Azure maschinellen Learning Empfehlungen-APIs.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="1-general-overview"></a>1. allgemeine Übersicht
Dieses Dokument ist eine API-Referenz. Sie sollten mit dem Dokument "Azure maschinellen Learning Empfehlungen – Schnellstart" beginnen.

Die Azure maschinellen Learning Empfehlungen-API kann in der folgenden logischen Gruppen unterteilt werden:

- <ins>Einschränkungen</ins> - Empfehlungen-API Einschränkungen.
- <ins>Allgemeine Informationen</ins> : Informationen zu Authentifizierung, Dienst-URI und Versioning.
- <ins>Grundlegende Modell</ins> - APIs, die Ihnen ermöglichen, führen Sie die grundlegende Vorgänge in dem Modell (z. B. erstellen, aktualisieren und löschen ein Modells).
- <ins>Erweiterte Modell</ins> - APIs, mit denen Sie datenerkenntnissen auf dem Modell erweiterte abrufen können.
- <ins>Modell Business Regeln</ins> - APIs, die Sie zum Verwalten von Business-Regeln auf das Modell Empfehlungen Ergebnisse aktivieren.
- <ins>Katalog</ins> - APIs, mit denen Sie grundlegende Vorgänge in einem Modell Katalog ausführen können. Katalog enthält Metadateninformationen zu den Artikeln Datenseite Verwendung.
- <ins>Feature</ins> - APIs, mit deren Hilfe Einsichten Element in den Katalog und wie Sie diese Informationen verwenden, um eine bessere Empfehlungen erstellen können.
- <ins>Verwendungsdaten</ins> - APIs, mit denen Sie grundlegende Vorgänge für die Verwendung Modelldaten ausführen können. Von Verwendungsdaten in die grundlegende Form besteht aus Zeilen, die Paare von & #60; Benutzer-ID & #62; enthalten, & #60; ItemId & #62;.
- <ins>Erstellen von</ins> -APIs, aktivieren Sie zum Auslösen Erstellung eines Modells oder führen Sie grundlegende Vorgänge, die mit diesem Build verknüpft sind. Nachdem Sie wertvolle Verwendungsdaten haben, können Sie die Erstellung eines Modells auslösen.
- <ins>Empfehlungen</ins> - APIs, mit denen Sie Empfehlungen nutzen, nachdem Sie das Erstellen eines Modells endet.
- <ins>Benutzerdaten</ins> - APIs, die Sie zum Abrufen von Informationen für die Verwendung Benutzerdaten aktivieren.
- <ins>Benachrichtigungen</ins> - APIs, mit denen Sie Benachrichtigungen zu Problemen im Zusammenhang mit Ihrem API Operationen erhalten können. (Beispielsweise, erstellen Sie einen Bericht Verwendungsdaten über den Erwerb von Daten und die meisten Ereignisse Verarbeitung schlägt fehl. Eine Benachrichtigung Fehler wird ausgelöst werden.)

##<a name="2-limitations"></a>2. Einschränkungen

- Die maximale Anzahl von Datenmodellen pro Abonnement ist 10.
- Die maximale Anzahl von Builds pro Modell ist 20.
- Die maximale Anzahl von Elementen, die ein Katalog enthalten kann ist 100,000.
- Die maximale Anzahl von Punkten Verwendung, die aufbewahrt werden, ist ~ 5.000.000. Die älteste wird gelöscht werden, wenn neue hochgeladen oder gemeldet werden.
- Die maximale Größe von Daten, die in Beitrag (z. B. Katalogdaten importieren, importieren Verwendungsdaten) gesendet werden können, ist 200MB.
- Die maximale Anzahl von Elementen, die für gestellt werden, wenn die erste Empfehlungen 150 ist.

##<a name="3-apis---general-information"></a>3. APIs - allgemeine Informationen

###<a name="31-authentication"></a>3.1. Authentifizierung
Führen Sie die Microsoft Azure Marketplace-Richtlinien zur Authentifizierung. Der Marketplace unterstützt Grundlagen "oder" OAuth Authentifizierungsmethode.

###<a name="32-service-uri"></a>3,2. Dienst-URI
Der Dienst Stamm-URI für die Azure maschinellen Learning Empfehlungen APIs ist [hier.](https://api.datamarket.azure.com/amla/recommendations/v3/)

Der vollständige Dienst-URI wird mithilfe der OData-Spezifikation Elemente ausgedrückt.  

###<a name="33-api-version"></a>3.3. API-version
Jede API Anruf haben, am Ende, einen Abfrageparameter aufgerufen ApiVersion, die auf 1.0 festgelegt werden soll.

###<a name="34-ids-are-case-sensitive"></a>3.4. IDs Groß-/Kleinschreibung
IDs, die APIs zurückgegebene Groß-/Kleinschreibung und sollte als solche verwendet werden, wenn als Parameter in weiteren API-Aufrufe übergeben. Modell-IDs und Katalog-IDs sind z. B. Groß-/Kleinschreibung beachtet.

##<a name="4-recommendations-quality-and-cold-items"></a>4. Empfehlungen Qualität und Kalt Elemente

###<a name="41-recommendation-quality"></a>4.1. Empfehlungen Qualität

Erstellen eines Modells Empfehlungen ist in der Regel genügend an, damit das System, um Empfehlungen bereitzustellen. Dennoch hängt Empfehlungen Qualität der Verwendung verarbeitet und den Schutz des Katalogs. Beispielsweise wenn Sie viele kalt Elemente (ohne signifikante Verwendung) haben, kann das System Probleme beim Bereitstellen einer Empfehlungen für solche ein Element, oder verwenden solche ein Elements als eine empfohlene sind. Um das Element kalt Problem zu umgehen, kann das System die Verwendung von Metadaten der Elemente, um die Empfehlungen zu verbessern. Diese Metadaten wird als Features bezeichnet. Standardfeatures sind ein Adressbuch Autor oder eine Film Akteur. Über den Katalog in Form von Schlüssel/Wert-Zeichenfolgen werden Features bereitgestellt. Die Formatierung der Katalogdatei finden Sie im [Abschnitt Geschäftsdatenkatalog importieren](#81-import-catalog-data). 

###<a name="42-rank-build"></a>4.2. Rang erstellen

Features des Modells Empfehlungen verbessern können, aber Sie hierzu erfordert die Verwendung von aussagekräftigen Features. Für diesen Zweck, die ein neuer Build eingeführt wurde - ein Rang erstellen. Dieser Build wird die Nützlichkeit Features Rangfolge. Ein aussagekräftiger Feature ist ein Feature mit einem Rang Faktor von 2 und nach oben.
Nach dem Kennenlernen, welche Features benötigt werden, die lösen Sie ein Empfehlungen erstellen, mit der Liste (oder Unterliste) aussagekräftige Features aus aus. Es ist möglich, diese Features zur Verbesserung der sowohl warm Elemente und Kalt Elemente zu verwenden. Damit diese für warm Elemente, verwendet die `UseFeatureInModel` erstellen Parameter eingerichtet werden sollte. Zur Verwendung von Features für kalt Elemente, die `AllowColdItemPlacement` Parameter erstellen aktiviert werden soll.
Hinweis: Es ist nicht möglich, aktivieren `AllowColdItemPlacement` ohne aktivieren `UseFeatureInModel`.

###<a name="43-recommendation-reasoning"></a>4.3. Empfehlungen Logik

Empfehlungen Logik ist ein weiterer Aspekt der Featureverwendung. Tatsächlich um, die Azure maschinellen Learning Empfehlungen-Engine Features können Sie Empfehlungen erläuterungen (auch bekannt als bereitstellen Schlussfolgerungen), um weitere Vertrauen in die empfohlenen Elemente vom Consumer Empfehlungen führenden.
So aktivieren Sie die Logik der `AllowFeatureCorrelation` und `ReasoningFeatureList` Parameter sollten Setup vor dem Anfordern einer Empfehlungen erstellen.


##<a name="5-model-basic"></a>5. Basic Modell

###<a name="51-create-model"></a>5.1. Modell erstellen
Erstellt eine Anforderung "Modell erstellen".

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

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

###<a name="52-get-model"></a>5.2. Erste Modell
Erstellt eine Anforderung "Get-Modell".

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br>Beispiel:<br>`<rootURI>/GetModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   ID  |   Eindeutiger Bezeichner des Modells (Groß-/Kleinschreibung wird beachtet) |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

Die Modelldaten finden Sie unter den folgenden Elementen:

- `feed/entry/content/properties/Id`-Eindeutige ID Modell
- `feed/entry/content/properties/Name`-Modell Name.
- `feed/entry/content/properties/Date`-Erstellungsdatum der Modell.
- `feed/entry/content/properties/Status`-Modell Status. Eine der folgenden Optionen:
    - Erstellt - Modell wird erstellt und enthält keine Katalog und die Verwendung.
    - ReadyForBuild - Modell erstellt und Katalog und die Verwendung enthält.
- `feed/entry/content/properties/HasActiveBuild`– Gibt an, wenn das Modell erfolgreich erstellt wurde.
- `feed/entry/content/properties/BuildId`-Aktive Generator-ID Modell
- `feed/entry/content/properties/Mpr`-Mittelwert Quantil Rangfolge modellieren (MPR - ModelInsight Weitere Informationen finden Sie unter).
- `feed/entry/content/properties/UserName`-Modell internen Benutzernamen ein.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get A List Of All Models</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T14:35:51Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
        <d:Name m:type="Edm.String">vah-11111</d:Name>
        <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
        <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
        <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
        <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
        <d:Description m:type="Edm.String">short description</d:Description>
        <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
      </m:properties>
    </content>
      </entry>
    </feed>

###<a name="53-get-all-models"></a>5.3. Alle Modelle abrufen
Ruft alle Modelle des aktuellen Benutzers ab.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetAllModels?apiVersion=%271.0%27`<br>Beispiel:<br>`<rootURI>/GetAllModels?apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

- `feed/entry/content/properties/Id`-Eindeutige ID Modell
- `feed/entry/content/properties/Name`-Modell Name.
- `feed/entry/content/properties/Date`-Erstellungsdatum der Modell.
- `feed/entry/content/properties/Status`-Modell Status. Eine der folgenden Optionen:
  - Erstellt - Modell wird erstellt und enthält keine Katalog und die Verwendung.
  - ReadyForBuild - Modell erstellt und Katalog und die Verwendung enthält.
- `feed/entry/content/properties/HasActiveBuild`– Gibt an, wenn das Modell erfolgreich erstellt wurde.
- `feed/entry/content/properties/BuildId`-Aktive Generator-ID Modell
- `feed/entry/content/properties/Mpr`-Modellieren MPR (Weitere Informationen finden Sie unter ModelInsight).
- `feed/entry/content/properties/UserName`-Modell internen Benutzernamen ein.
- `feed/entry/content/properties/UsageFileNames`-Modell verwendete Dateien durch Kommas getrennte Liste.
- `feed/entry/content/properties/CatalogId`-Modell Katalog-ID an.
- `feed/entry/content/properties/Description`-Beschreibung der Modell.
- `feed/entry/content/properties/CatalogFileName`-Name der Katalog Modell.

OData-XML


    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get A List Of All Models</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-28T14:35:51Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
                    <d:Name m:type="Edm.String">vah-11111</d:Name>
                    <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
                    <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
                    <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
                    <d:BuildId m:type="Edm.String">-1</d:BuildId>
                    <d:Mpr m:type="Edm.String">0</d:Mpr>
                    <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
                    <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
                    <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
                    <d:Description m:type="Edm.String">short description</d:Description>
                    <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
                </m:properties>
            </content>
        </entry>
    </feed>

###<a name="54-update-model"></a>5.4. Aktualisierungsmodell

Sie können die Beschreibung Modell oder im aktiven Generator-ID aktualisieren.<br>
<ins>Aktiven Generator-ID</ins> – jeder Build für jedes Modell verfügt über eine eigene ID. Die aktive Generator-ID wird dem ersten erfolgreichen Build für jedes neue Modell. Nachdem Sie haben eine ID aktiven erstellen, und führen Sie Sie für das gleiche Modell zusätzliche erstellt, müssen Sie explizit es als Standard-Generator-ID festlegen, wenn Sie möchten. Wenn Sie Empfehlungen, nutzen, wenn Sie nicht die Generator-ID angeben, die Sie verwenden möchten, wird automatisch eine Standard verwendet werden.<br>
Dieses Verfahren ermöglicht es Ihnen - Nachdem Sie ein Modell Empfehlungen Herstellung - zum Erstellen neuer Modelle und testen, bevor Sie diese auf der Herstellung heraufstufen haben.


| HTTP-Methode | URI |
|:--------|:--------|
|SETZEN     |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br>Beispiel:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   ID      | Eindeutiger Bezeichner des Modells (Groß-/Kleinschreibung wird beachtet)  |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | `<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`<Description>New Description</Description>`<br>`<ActiveBuildId>-1</ActiveBuildId>`<br>` </ModelUpdateParams>`<br><br>Beachten Sie, dass die XML-Tags, Beschreibung und ActiveBuildId optional sind. Wenn Sie keine Beschreibung oder ActiveBuildId festlegen möchten, entfernen Sie das gesamte Tag.|

**Antwort**:

HTTP-Status Code: 200

###<a name="55-delete-model"></a>5.5. Modell löschen
Löscht ein vorhandenes Modell-ID

| HTTP-Methode | URI |
|:--------|:--------|
|LÖSCHEN     |`<rootURI>/DeleteModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br>Beispiel:<br>`<rootURI>/DeleteModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   ID  |   Eindeutiger Bezeichner des Modells (Groß-/Kleinschreibung wird beachtet) |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Delete Model by Id</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T10:39:33Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">DeleteModelByIdEntity</title>
    <updated>2014-10-28T10:39:33Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:string m:type="Edm.String"></d:string>
      </m:properties>
    </content>
      </entry>
    </feed>

##<a name="6-model-advanced"></a>6. erweiterte Modell

###<a name="61-model-data-insight"></a>6.1. Modell Daten Einblick
Gibt statistische Daten auf der Registerkarte Verwendungsdaten, denen mit diesem Modell erstellt wurde.

Nur für Empfehlungen-Generator verfügbar.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetDataInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Beispiel:<br>`<rootURI>/GetDataInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId |   Eindeutiger Bezeichner des Modells |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

Die Daten werden als eine Auflistung von Eigenschaften zurückgegeben.

- `feed/entry/id/content/properties/key`-Enthält den Namen der Eigenschaft.
- `feed/entry/id/content/properties/value`-Wert der Eigenschaft enthält.

In der nachfolgenden Tabelle wird den Wert, der jedem Schlüssel dargestellt.

|Schlüssel|Beschreibung|
|:-----|:----|
| AvgItemLength | Durchschnittliche Anzahl der distinct Benutzer pro Element. |
| AvgUserLength | Durchschnittliche Anzahl der eindeutige Elemente pro Benutzer. |
| DensificationNumberOfItems | Die Anzahl von Elementen nach Kürzen von Elementen, die Einheitsformular werden nicht möglich. |
| DensificationNumberOfUsers | Anzahl der Verwendung von Punkten nach Kürzen von Benutzern und Elemente, die Einheitsformular werden können nicht. |
| DensificationNumberOfRecords | Anzahl der Verwendung von Punkten nach Kürzen von Benutzern und Elemente, die Einheitsformular werden können nicht. |
| MaxItemLength | Anzahl der einzelnen Benutzer für die am häufigsten verwendeten Element. |
| MaxUserLength | Maximale Anzahl von unterschiedlichen Elemente für einen Benutzer. |
| MinItemLength | Maximale Anzahl von unterschiedlichen Benutzern für ein Element. |
| MinUserLength | Minimale Anzahl der unterschiedlichen Elemente für einen Benutzer. |
| RawNumberOfItems | Die Anzahl von Elementen in der Verwendung Dateien. |
| RawNumberOfUsers | Anzahl der Verwendung Punkte, bevor Sie eine beliebige Archivs. |
| RawNumberOfRecords | Anzahl der Verwendung Punkte, bevor Sie eine beliebige Archivs. |
| SamplingNumberOfItems | N/V |
| SamplingNumberOfRecords | N/V |
| SamplingNumberOfUsers | N/V |

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get data insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgItemLength</d:Key>
        <d:Value m:type="Edm.String">18.936</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgUserLength</d:Key>
        <d:Value m:type="Edm.String">51.879</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfItems</d:Key>
        <d:Value m:type="Edm.String">2,000</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfRecords</d:Key>
        <d:Value m:type="Edm.String">37,872</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
        <m:properties>
            <d:Key m:type="Edm.String">DensificationNumberOfUsers</d:Key>
            <d:Value m:type="Edm.String">730</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxItemLength</d:Key>
                <d:Value m:type="Edm.String">4,704</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxUserLength</d:Key>
                <d:Value m:type="Edm.String">190</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinItemLength</d:Key>
                <d:Value m:type="Edm.String">8</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinUserLength</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="62-model-insight"></a>6.2. Modell Einblick
Gibt einen Einblick modellieren, klicken Sie auf die aktive erstellen oder (falls vorhanden) auf einen bestimmten Build.

Nur für Empfehlungen-Generator verfügbar.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |Erstellen Sie mit aktiv ID aus:<br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/GetModelInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br>Erstellen Sie mit bestimmten ID aus:<br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId |   Eindeutiger Bezeichner des Modells |
|   buildId |   Optional - Nummer, die einen erfolgreichen Build identifiziert. |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

Die Daten werden als eine Auflistung von Eigenschaften zurückgegeben.

- `feed/entry/id/content/properties/key`
- `feed/entry/id/content/properties/value`


In der nachfolgenden Tabelle wird den Wert, der jedem Schlüssel dargestellt.

| Schlüssel | Beschreibung |
|:---- |:----|
| CatalogCoverage | Welcher Teil der Katalog mit Verwendungsmustern Einheitsformular werden kann. Die restlichen Elemente benötigen Content-basierte Features. |
| MPR | Mittelwert Quantil Rang das Modell. Unteren ist es besser. |
| NumberOfDimensions | Anzahl der Maße für die Matrix Factorization Algorithmus. |


OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get model insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">CatalogCoverage</d:Key>
                <d:Value m:type="Edm.String">1.000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">Mpr</d:Key>
                <d:Value m:type="Edm.String">0.367</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">NumberOfDimensions</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="63-get-model-sample"></a>6.3. Erste Modell Stichprobe
Ruft eine Stichprobe des Modells Empfehlungen ab.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetModelSample?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Beispiel:<br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br>Erstellen Sie mit bestimmten ID aus:<br>`<rootURI>/GetModelSample?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`<br>Beispiel:<br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&buildId=%271500068%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId |   Eindeutiger Bezeichner des Modells |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

OData-XML

Im unformatierten Text-Format wird als Antwort zurückgegeben:

<pre>
Ebene 1---655fc 955-a5a3-4a26-9723-3090859cb27b, betroffenen: eine neuartige 655fc 955-a5a3-4a26-9723-3090859cb27b, betroffenen: eine neuartige Bewertung: 0.5215 3f471802-f84f-44a0 - 99c 8-6d2e7418eec1, Schwarz Hawk unten: einen Textabschnitt eines modernen War Bewertung: 0.5151 07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Bewertung: 0.5148 6afc18e4 8c2a - 43d 1-9021-57543d6b11d8, Imajica Bewertung: 0.5146 e4cc5e69-3567-43ab-b00f-f0d8d0506870, drücken Sie Liste Bewertung: 0.514 56b61441-0eed-46cc-a8f6-112775b81892, lebenswichtiger in Shanghai 56b61441-0eed-46cc-a8f6-112775b81892, lebenswichtiger in Shanghai Bewertung: 0.5218 53156702-cc0c-443d-b718-6fb74b2491d3, Sohn des \ Bewertung: 0.5212 fb8cf7a6-8719-46ee - 97d 4-92f931d77a3a, Rauch und spiegelt: kurze Fictions und Illusionen Bewertung : 0.5188 8f5fe006-79e4-4679-816b-950989d1db4b, A Ort haben ich nie (modernen American Idee) Bewertung wurde: 0.5156 d8db4583-cc0f-49ce-bc95-b7fa3491623f, Glück: eine neuartige Bewertung: 0.5156 50471eec-9aeb-4900-84 d 7-21567ab18546, wenn die Buddha behoben: ein Handbuch für die Suche nach Love auf eine festliche Pfad cfe922a1-7ca0-4f8d-ad9d-b7cc87bfe0ef, gute Wahl geheimen Informationen von der Ya-Ya Sisterhood: eine neuartige Bewertung: 0.5266 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, die Poisonwood Bibel: eine neuartige Bewertung: 4dd0d7dc3a19-0.5252 973f8cbd-0846-4f6b - 9d 28, Schweine im Himmel Bewertung: 0.5244 e2cbf7ad-0636-4117-8b30-298da6df7077, tierisch Träume Bewertung : 0.5227 6c818fd3-5a09-417d-9ab4-7ffe090f0fef, Geständnisse eines hässlich Stepsister: eine neuartige Bewertung: 0.5222 5e97148f-Defb - 4d 74-af2d-80f4763bf531, Tiefe Ende der Ozean (Oprahs Adressbuch Club) 5e97148f-Defb - 4d 74-af2d-80f4763bf531, die Bewertung Ozean (Oprahs Adressbuch Club) Tiefe Ende: 0.537 5dcbac37-2946-4f2a-a0b3-bbe710f9409a, von Insel: eine neuartige Bewertung: 0.5277 bc5b69db-733b-4346-Adde-3927544258f7, Downtown Bewertung: 0.5275 31fe5c63 3e5a - 48d 0-802b-d3b0f989a634, einen schönen Tag: eine Geschichte Blutdruckmessung und Sweatsocks Bewertung: 0.5252 0adf981a b65b - 4c 11-b36b-78aca2f948a2, der perfekten Storm : Ein True Geschichte der Männer gegen die Bewertung Beringsee: 0.5238 68f97068-ae1a-4163-9e94-396b800b743d, Modoc: True Geschichte den größten Elefanten, die jemals 68f97068-ae1a-4163-9e94-396b800b743d, Modoc vorhanden war: WAHR Geschichte den größten Elefanten, die Bewertung jemals vorhanden war: 0.5379 6724862e-e4e7-4022-9614-1468d8b902ff, kleines House auf die Bewertung Prairie: 0.5345 cdedb837-1620-496d - 94c 4-6ccfed888320, kleines House in der große Woods Bewertung: 0.5325 382164ba-406b-4187-b726-d7a54b9d790d, die Tao of Pooh Bewertung: 0.5309 6a068d6a-bb74-4ba3-b3f2-a956c4f9d1b5 , Auf Bank Plum Creek Bewertung: 0.5285 37ef8e74-e348-44e5-Aabc-1d7f9efcb25b, Männer sind von Mars Frauen sind Venus: einen praktischen Leitfaden für die Kommunikation verbessern und die erste, was die in Ihrer Beziehungen 37ef8e74-e348-44e5-Aabc-1d7f9efcb25b, Mars Männer sind, sind Frauen Venus: einen praktischen Leitfaden für die Kommunikation verbessern und was erste in Ihre Bewertung Beziehungen werden soll: 0.5397 f2be16d4-5faf - 4d 32-ab83-7ba74d29261e, berücksichtigen aller richtige Nachtruhe Textabschnitte : Moderne Geschichten für unsere Nutzungsdauer und-Zeiten Bewertung: 0.5207 ef732c5c-334b-4d6b-ab82-7255eb7286d0, Honor zwischen Diebe Bewertung: 0.5195 0b209b8c-7cdd-47fd-b940-05c7ff7c60fc, die zugewiesen Struktur Bewertung: 0.5194 883b360f-8b42-407f-b977-2f44ad840877, furchteinflößendes Geschichten können in der dunkel feststellen: von American Volkskunst (furchteinflößendes Geschichten) Bewertung gesammelt: 0.5184 ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Männer im Büro: der einbezogen von Baseball d008dae9, c73a, 40a1, 9a9b, 96d5cf546f36, die Gulag Archipels 1918-1956: einem Versuch in literarische Untersuchung ich-II-Bewertung: 0.5416 ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Männer im Büro : Der einbezogen Baseball Bewertung: 0.5403 49dec30e-0adb-411a-b186-48eaabf6f8bc, Fatherland Bewertung: 0.5394 cc7964fd-d30f-478e-a425-93ddbdf094ed, magische das Sammeln: Bereich vol 1 Bewertung: 0.5379 8a1e9f36-97af-4614-bed9-24e3940a05f3, weitere Sniglets: ein beliebiges Wort, die nicht im Wörterbuch angezeigt wird, aber Bewertung sollte: 0.5377 12a6d988-be21-4a09-8143-9d5f4261ba16, ein Traum der Eagles 07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Bewertung: 0.5417 e4cc5e69-3567-43ab-b00f-f0d8d0506870, drücken Sie Liste Bewertung: 0.5416 1f1a34c4-9781-49f5-a3cc-acec3ae3c71d, die Familie Bewertung: 0.5371 56daeffe - 7d 48-43cd-8ef8-7dffd0c103d3, Kilogramm Klasse Bewertung: 0.5366 b2fe511e-5cb9-4a56-b823-2801e63e6a96, rechtliche Angebots Bewertung : 0.5366 df87525b-e435-4bd6-8701-4e60ad344e28, suchen Fisch 56d 33036-Dfda-46b9-8e2a-76cb03921bb0, die X-Dateien: Grund NULL Bewertung: 0.5417 0780cde8-6529-4e1d-b6c6-082c1b80e596, zwölf Rot Herrings Bewertung: 0.5416 df87525b-e435-4bd6-8701-4e60ad344e28, suchen Fisch Bewertung: 0.5408 400fe331 2c 35-490c-Adbc-b28b4b73d56c, müssen wir feststellen der Präsident? Bewertung: 0.5383 f86ad7d0 - 5c 03-42b3-Aebf-13d44aec8b30, Graustufen F1 Bewertung: 0.5358 de1f62a4-89e6 - 44d 2-Aaee-992a4bf093f1, die Zuordnung, die die ganzen Welt geändert: William Smith und das Geburtsdatum der moderne Geologie de1f62a4-89e6 - 44d 2-Aaee-992a4bf093f1, die Zuordnung, die die ganzen Welt geändert: William Smith und das Geburtsdatum der moderne Geologie Bewertung: 0.5422 b303538f-e2c6-4a2c-b425-8d21e684fc3e, meine Onkel Oswald Bewertung: 0.5385 34b84627-48af-4a4c - 96c 4-b26fb3863f56, Mitternacht Garten von funktionierenden und bösartige Bewertung: 0.5379 306cbaa7-b1a8-4142-9 d 55-e11b5018a7a8, die Straße Anwältin Bewertung : 0.5376 e53b4baa - 8c 09-45c 4-95c 0-b6a26b98770b, entgehen Smillas Gefühl für Snow Bewertung: 0.5367

<a name="level-2"></a>Ebene 2
---------------
352aaea1-6b12-454d-a3d5-46379d9e4eb2, das nur Schwein (Hillerman Tony) 352aaea1-6b12-454d-a3d5-46379d9e4eb2, die nur Schwein (Hillerman Tony) im: 0.5425 74c 49398-bc10-4af5-a658-a996a1201254, untergeordneten Elementen des der Storm (Peters Elizabeth) Bewertung: 0.5387 9ba80080-196e-43fd-8025-391d963f77e7, das beweglich Mädchen Bewertung: 0.5372 e68f81d5-7745-4cc7-b943-fedb8fcc2ced, Killer Lächeln (Scottoline Lisa) Bewertung: 0.5353 b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Angebot Bewertung: 0.5332 c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon Tage 0adf981a-b65b - 4c 11-b36b-78aca2f948a2, der perfekten Storm: A wahr Geschichte der Männer anhand der Beringsee Bewertung: 0.5433 c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon Tage Bewertung : 0.543 a00ae6ad-4a7f-4211-9836-75ce8834eb11, Sniglets (Snig'lit: ein beliebiges Wort, das im Wörterbuch nicht angezeigt wird jedoch sollte) Bewertung: 0.5327 6f6e192e 0d 64-49ca-9b63-f09413ea1ee6, richtige berücksichtigen aller Feiertage Geschichten: für eine Augenblick Yuletide Jahreszeit Bewertung: 0.5307 798051a8 147d - 4d 46-b0dc-e836325029e6, Alter der UNSCHULD (MOVIE TIE-IN) Bewertung: 0.5301 73f3e25a-e996-4162-9ed8-ff3d34075650, O Pionieren! (Pinguin des 20. Jahrhundert Klassiker) cba8163f-6536-436b-8130-47b4a43c827f, eine (der offiziellen Leitfaden für die X-Dateien vol 2) Bewertung vertrauen: 0.5434 5708e4cb-2492-49 C 0-94a8-cc413eec5d89, kleine Götter (Discworld Büchern (Taschenbuch)) Bewertung: 0.5406 73f3e25a-e996-4162-9ed8-ff3d34075650, O Pionieren! (Pinguin des 20. Jahrhundert Klassiker) Bewertung: 0.5403 d885b0bd-ae4b-452d-bdf2-faa90197dbc9, die Farbe des magische Bewertung: 0.539 b133a9c4-4784-4db3-b100-d0d6dffb94d2, der Wahrheitswert liegt außerhalb des Bewertung dort (der offiziellen Leitfaden für die X-Dateien vol 1): 0.5367 271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: oder kann ich feststellen, warum der Gefluegelte Whale 271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke Experten sind: oder kann ich feststellen, warum der Gefluegelte Whale Bewertung Experten sind: 0.5445 2de1c354-90ff - 47c 5-a0db-1bad7d88ef94, die Salaryman des Frau (Kinder Gewalt Reihe) Bewertung: 0.5329 d279416e 19c 0-43f8-9ec9-a585947879ca, macht Ihre Einstellung Bewertung : 0.5316 c8f854d7-3de3-4b23-8217-f4f851670fd4, von der Cootie Mädchen Rache: eine Robert Hudson Mysterium (Robert Hudson Geheimnisse (Taschenbuch)) Bewertung: 0.5305 8ef4751c-7074-409e-a3ac-d49b222fc864, Stelle, an der die frei Dinge Bewertung sind: 0.5289 9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, deren Augen Gott 9ad1b620, 0a7b, 4543, 8673, 66d4c3bcb2f1 heraus wurden, deren Augen wurden Gott Bewertung heraus: 0.5446 da45c4d5-aba1-413b-a9bd-50df98b1e1d2, die Bewertung Bean Bäume: 0.5389 65ecbdd1 131c - 40c 3-a3d6-d86ca281377a, Gott des kleine Punkte im: 0.5387 c78743bf-7947-4a0c-8db7-8a3bfe69ba70, die Bewertung Stein Tagebücher: 4dd0d7dc3a19-0.5355 973f8cbd-0846-4f6b - 9d 28, Schweine im Himmel Bewertung : 0.5344 5f17d90a-2604-4fe8-8977-1a280b9098b1, für das Geld (Stephanie Plum Büchern (Taschenbuch)) 5f17d90a-2604-4fe8-8977-1a280b9098b1, eine für die Bewertung Geld (Stephanie Plum Büchern (Taschenbuch)): 0.5446 57169b2b-9a8a-486 b-9aac-1ed98ce57168, endgültigen Rechtsmittel Bewertung: 0.5332 efcb1bc4-7278-4a8f-b491-befde02070d6, Moment der eindeutige und aktuelle Bewertung: 0.5329 1efa91a2-993b - 4c 43-9f5c-3454fc12612d, Brennen Faktor Bewertung: 0.5309 24c 59962-458a-4ec8-b95d-d694e861919c, im Mitford (die Mitford Jahre) Bewertung zu Hause: 0.5303 4fd48c46-1a20 - 4c 57-bc7f-a02ef123dc52, als Art vorgenommen Fachkenntnisse: die Junge, die als ein kleines Mädchen 4fd48c46-1a20-4c57-bc7f-a02ef123dc52 ausgelöst wurde , Wie Art ihm vorgenommen: die Junge, wer wurde ausgelöst als ein kleines Mädchen Bewertung: 0.5449 cd5f2c03-20cb-43be-a1fb-3b4233e63222, Schweine im Himmel: 0.5329 19985fdb-d07a-4a25-ae4a-97b9cb61e5d1, in der Zeit Cholera (Pinguin hervorragende Bücher des 20. Jahrhunderts) Bewertung Love: 0.5267 15689d 09-c711-4844-84 d 8-130a90237b26 Beschriftung Canto Bewertung: 0.5245 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, das Poisonwood Bibel: A Roman Bewertung: 0.5235 98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Speicher f874b5a3 5d 40-4436-94ff-0fa1c090ddf5, der so auch überschreitet (A Scribner klassisch) im : 0.5451 98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Speicher Bewertung: 0.5442 0ce0014a-9a48-4013-a08a-7f2c11877930, H.M.S. Nicht gelesenen Bewertung: 0.5421 15316ca6-1e38-425f-893d-691944a47000, weitere furchteinflößendes Geschichten zu erzählen In der dunkel Bewertung: 0.5409 329d 5682-3dc3-4206-8aa2-eef4b1032258, Buchstaben aus der Erde Bewertung: 6f669bb1b0a9-0.54 5b9445d5-c072 - 419c - 8d 49, Tochter von Fortune: eine Tochter von Fortune Roman (Oprahs Adressbuch Club (Hardcover)) 5b9445d5-c072 - 419c - 8d 49-6f669bb1b0a9,: eine Bewertung Roman (Adressbuch Club (Hardcover) des Oprah): 0.5462 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, die Poisonwood Bibel: eine neuartige Bewertung: 0.5372 604eb3bd-6026-4f51-Bffd-9fb54f180400, familiäre Bilder: eine neuartige Bewertung: 0.5341 8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Bewertung normale Zeit (Oprahs Adressbuch Club (Taschenbuch)) : 0.5334 da45c4d5-aba1-413b-a9bd-50df98b1e1d2, die Bean Strukturen Bewertung: 0.5319 d5358189-d70f-4e35-8add-34b83b4942b3, Schweine im Himmel d5358189, d70f, 4e35, 8add, 34b83b4942b3, Schweine im Himmel Bewertung: 0.5491 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, die Poisonwood Bibel: eine neuartige Bewertung: 0.5401 c78743bf-7947-4a0c-8db7-8a3bfe69ba70, die Bewertung Stein Tagebücher: 0.5393 8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in normale Zeit (Oprahs Adressbuch Club (Taschenbuch)) Bewertung: 4dd0d7dc3a19-0.5382 973f8cbd-0846-4f6b - 9d 28, Schweine im Himmel Bewertung: 0.5367

</pre>


##<a name="7-model-business-rules"></a>7. Business-Regeln Modell

Dies sind die Typen von Regeln unterstützt:
- <strong>Sperrliste</strong> - Sperrliste können Sie eine Liste der Elemente bereitstellen, die nicht in den Ergebnissen Empfehlungen zurückgegeben werden sollen. 

- <strong>FeatureBlockList</strong> - Feature Sperrliste können Sie Elemente auf der Grundlage der Werte seiner Features blockieren.

*Nicht mehr als 1000 Elemente in einer Regel für die einzelnen Sperrliste senden oder ein Anruf möglicherweise Timeout. Wenn Sie mehr als 1000 Elemente blockieren müssen, können Sie mehrere Sperrliste anrufen.*

- <strong>Upsale</strong> - Upsale können Sie Elemente in den Ergebnissen Empfehlungen zurück zu erzwingen.

- <strong>Weiße Liste</strong> - weiße Liste können Sie nur Empfehlungen aus einer Liste von Elementen vorschlagen.

- <strong>FeatureWhiteList</strong> - Feature weiße Liste können Sie nur Elemente empfiehlt sich, die bestimmte Features Werte enthalten.

- <strong>PerSeedBlockList</strong> - pro Startwert Blockliste ermöglicht es Ihnen, pro Element eine Liste von Elementen bereitzustellen, die als Empfehlungen Ergebnisse zurückgegeben werden kann.




###<a name="71-get-model-rules"></a>7.1. Abrufen der Modell-Regeln

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetModelRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Beispiel:<br>`<rootURI>/GetModelRules?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId |   Eindeutiger Bezeichner des Modells |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

- `feed/entry/content/properties/Id`-Eindeutiger Bezeichner für diese Regel.
- `feed/entry/content/properties/Type`-Typ der Regel.
- `feed/entry/content/properties/Parameter`-Regel Parameter.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get a list of rules for a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T12:58:57Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000043</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1000"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000044</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1001"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="72-add-rule"></a>7.2. Regel hinzufügen

| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/AddRule?apiVersion=%271.0%27`|
|KOPFZEILE   |`"Content-Type", "text/xml"`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | 
<ins>Bei jedem Element-Ids für Business Regeln bereitstellen, stellen Sie sicher, verwenden Sie die externe Id des Elements (der gleichen Id, die Sie in der Katalogdatei verwendet)</ins><br>
<ins>So fügen Sie eine Regel Sperrliste hinzu:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>BlockList</Type><Value>{"ItemsToExclude":["2406E770-769C-4189-89DE-1C9283F93A96","3906E110-769C-4189-89DE-1C9283F98888"]}</Value></ApiFilter>`<br><br><ins>
<ins>So fügen Sie eine Regel FeatureBlockList hinzu:</ins><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureBlockList</Type><Value>{"Name":"Movie_category","Values":["Adult","Drama"]}</Value></ApiFilter>`<br><br><ins>So fügen Sie eine Regel Upsale hinzu:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>Upsale</Type><Value>{"ItemsToUpsale":["2406E770-769C-4189-89DE-1C9283F93A96"],"NumberOfItemsToUpsale":5}</Value></ApiFilter>`<br><br>
<ins>So fügen Sie eine Regel weißen Liste hinzu:</ins><br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>WhiteList</Type><Value>{"ItemsToInclude":["2406E770-769C-4189-89DE-1C9283F93A96","1116E770-769C-4189-89DE-1C9283F88888"]}</Value></ApiFilter>`<br><br><ins>
<ins>So fügen Sie eine Regel FeatureWhiteList hinzu:</ins><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureWhiteList</Type><Value>{"Name":"Movie_rating","Values":["PG13"]}</Value></ApiFilter>`<br><br><ins>So fügen Sie eine Regel PerSeedBlockList hinzu:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>PerSeedBlockList</Type><Value>{"SeedItems":["9949"],"ItemsToExclude":["9862","8158","8244"]}</Value></ApiFilter>`|


**Antwort**:

HTTP-Status Code: 200

Die API gibt die neu erstellte Regel mit entsprechenden Details an. Die Eigenschaft Regeln kann aus den folgenden Pfaden abgerufen werden:

- `feed/entry/content/properties/Id`-Eindeutiger Bezeichner für diese Regel.
- `feed/entry/content/properties/Type`-Typ der Regel: Sperrliste oder Upsale.
- `feed/entry/content/properties/Parameter`-Regel Parameter.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add A Rule</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T11:13:28Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AddARuleEntity</title>
        <updated>2014-11-05T11:13:28Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000041</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1002"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="73-delete-rule"></a>7.3. Regel löschen

| HTTP-Methode | URI |
|:--------|:--------|
|LÖSCHEN     |`<rootURI>/DeleteRule?modelId=%27<model_id>%27&filterId=%27<filter_Id>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`DeleteRule?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&filterId=%271000011%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId |   Eindeutiger Bezeichner des Modells |
|   filterId    |   Eindeutiger Bezeichner des Filters |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

###<a name="74-delete-all-rules"></a>7.4. Alle Regeln löschen

| HTTP-Methode | URI |
|:--------|:--------|
|LÖSCHEN     |`<rootURI>/DeleteAllRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`DeleteAllRules?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId |   Eindeutiger Bezeichner des Modells |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

##<a name="8-catalog"></a>8. Katalog

###<a name="81-import-catalog-data"></a>8.1. Katalog importieren von Daten

Wenn Sie mehrere Katalogdateien in der gleichen Modell mit mehreren Anrufe hochladen, werden wir die neuen Katalogelemente einfügen. Vorhandene Elemente werden mit den ursprünglichen Werten bleiben. Daten im Katalog kann nicht mit dieser Methode aktualisiert werden.

Die Daten im Katalog sollte das folgende Format folgen:

- Ohne Features-`<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- Mit Features-`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

Hinweis: Die maximale Dateigröße ist 200MB.

**Format-details**

| Namen | Obligatorisch | Typ |  Beschreibung |
|:---|:---|:---|:---|
| Listenelement-Id |Ja | [A-Z], [a-Z], [0-9], [_] & #40; Unterstrich & #41; [-] & #40; Gedankenstrich & #41;<br> Die Maximallänge: 50 | Eindeutige ID eines Elements. |
| Elementnamen | Ja | Jedes beliebige alphanumerischen Zeichen<br> Die Maximallänge: 255 | Der Name des Elements. | 
| Element-Kategorie | Ja | Jedes beliebige alphanumerischen Zeichen <br> Die Maximallänge: 255 | Kategorie, zu dem dieses Element (z. B. Cooking Bücher, Filmen...) gehört. kann leer sein. |
| Beschreibung | Nein, es sei denn, Features sind präsentieren (können aber leer sein) | Jedes beliebige alphanumerischen Zeichen <br> Die Maximallänge: 4000 | Beschreibung dieses Artikels. |
| Featureliste | Nein | Jedes beliebige alphanumerischen Zeichen <br> Die Maximallänge: 4000; Die maximale Anzahl von Features: 20 | Durch Trennzeichen getrennte Liste der Featurenamen = Wert Features, die zum Modell Empfehlungen; verbessern verwendet werden kann Siehe Abschnitt [Erweiterte Themen](#2-advanced-topics) . |


| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|
|KOPFZEILE   |`"Content-Type", "text/xml"`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId |   Eindeutiger Bezeichner des Modells  |
| Dateiname | Textbasierte Bezeichner des Katalogs.<br>Nur Buchstaben (A-Z, a-Z), Zahlen (0-9), Bindestriche (--) und Unterstrichs (_) zulässig sind.<br>Die Maximallänge: 50 |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | Beispiel (mit Features):<br/>2406e770-769c-4189-89de-1c9283f93a96, Clara Callan, Adressbuch, die Beschreibung Adressbuch verfassen = Richard Wright, Publisher Harper Flamingo (Kanada) = Jahr = 2001<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, der vergessen Raum: A Idee (Byzantium Buch), Adressbuch, Verfassen = Nick Bantock, Publisher = Harpercollins, Jahr = 1997<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, Adressbuch, Verfassen = Timothy Findley, Publisher (Kanada) HarperFlamingo = Jahr = 2001<br>552a1940-21e4-4399-82bb-594b46d7ed54, Führung der Raubtiere, Adressbuch, die Beschreibung Adressbuch verfassen = Magnus Mills, Publisher Arcade Publishing, = Jahr 1998 =</pre> |


**Antwort**:

HTTP-Status Code: 200

Die-API gibt einen Bericht zu importieren.
- `feed\entry\content\properties\LineCount`-Zeilenanzahl akzeptiert.
- `feed\entry\content\properties\ErrorCount`-Anzahl der Zeilen, die nicht aufgrund eines Fehlers eingefügt wurden.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Import catalog file</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ImportCatalogFileEntity2</title>
        <updated>2014-10-05T06:58:04Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:LineCount m:type="Edm.String">4</d:LineCount>
                <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="82-get-catalog"></a>8.2. Abgerufen Sie Katalog werden
Ruft alle Katalogelementen an.
Der Katalog werden, die jeweils eine Seite abgerufen. Wenn Sie die Elemente an einem bestimmten Index abrufen möchten, können Sie den $skip Odata-Parameter verwenden. Beispielsweise wenn Sie Elemente ab Position 100 erhalten möchten, fügen Sie den Parameter $skip = 100 ein, um die Anfrage.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetCatalog?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`GetCatalog?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId |   Eindeutiger Bezeichner des Modells |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

Die Antwort enthält einen Eintrag pro Katalogelement. Jeder Eintrag weist die folgenden Daten:

- `feed/entry/content/properties/ExternalId`-Katalog Element externen-ID von den Kunden bereitgestellt.
- `feed/entry/content/properties/InternalId`-Katalogs interne Element-ID, das Schema, die Azure maschinellen Learning Empfehlungen generiert hat.
- `feed/entry/content/properties/Name`-Katalog Elementnamen.
- `feed/entry/content/properties/Category`-Katalog Element Category.
- `feed/entry/content/properties/Description`-Element Beschreibung des Katalogs.
- `feed/entry/content/properties/Metadata`-Katalogs Elementmetadaten.


OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get All Catalog Items</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">552A1940-21E4-4399-82BB-594B46D7ED54</d:ExternalId>
                <d:InternalId m:type="Edm.String">060db26a-e6a6-464e-bb52-436d2da82a50</d:InternalId>
                <d:Name m:type="Edm.String">Restraint of Beasts</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:ExternalId>
                <d:InternalId m:type="Edm.String">209d7bfe-2eb9-4455-92a3-7c867a41a74a</d:InternalId>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">3BB5CB44-D143-4BDD-A55C-443964BF4B23</d:ExternalId>
                <d:InternalId m:type="Edm.String">913ff79b-059b-4d4d-8042-6fa4cc1391dd</d:InternalId>
                <d:Name m:type="Edm.String">Spadework</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">21BF8088-B6C0-4509-870C-E1C7AC78304A</d:ExternalId>
                <d:InternalId m:type="Edm.String">ea65e4fa-768c-40b4-92c3-69d3e8178691</d:InternalId>
                <d:Name m:type="Edm.String">The Forgetting Room: A Fiction (Byzantium Book)</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="83-get-catalog-items-by-token"></a>8.3. Abrufen von Katalogelementen durch Token

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetCatalogItemsByToken?modelId=%27<modelId>%27&token=%27<token>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`GetCatalogItemsByToken?modelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&token=%27Cla%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId |   Eindeutiger Bezeichner des Modells |
|   Token   |   Der Name der Katalogelement Token. Sollte mindestens 3 Zeichen enthalten. |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

Die Antwort enthält einen Eintrag pro Katalogelement. Jeder Eintrag weist die folgenden Daten:

- `feed/entry/content/properties/InternalId`-Katalogs interne Element-ID, das Schema, die Azure maschinellen Learning Empfehlungen generiert hat.
- `feed/entry/content/properties/Name`-Katalog Elementnamen.
- `feed/entry/content/properties/Rating`-(für eine zukünftige Verwendung)
- `feed/entry/content/properties/Reasoning`-(für eine zukünftige Verwendung)
- `feed/entry/content/properties/Metadata`-(für eine zukünftige Verwendung)
- `feed/entry/content/properties/FormattedRating`-(für eine zukünftige Verwendung)

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get Catalog items that contain a token</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:48:19Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">CatalogItemsThatContainATokenEntity</title>
            <updated>2014-10-29T11:48:19Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                    <d:Name m:type="Edm.String">Clara Callan</d:Name>
                    <d:Rating m:type="Edm.Double">0</d:Rating>
                    <d:Reasoning m:type="Edm.String"></d:Reasoning>
                    <d:Metadata m:type="Edm.String"></d:Metadata>
                    <d:FormattedRating m:type="Edm.Double" m:null="true"></d:FormattedRating>
                </m:properties>
            </content>
        </entry>
    </feed>

##<a name="9-usage-data"></a>9. Verwendungsdaten
###<a name="91-import-usage-data"></a>9.1. Importieren von Verwendungsdaten
####<a name="911-uploading-file"></a>9.1.1. Datei hochladen
In diesem Abschnitt wird gezeigt, wie Daten zur Verwendung mit einer Datei hochladen. Sie können diese API mit Verwendungsdaten mehrfach aufrufen. Für alle Anrufe werden alle Verwendungsdaten gespeichert werden.

| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId |   Eindeutiger Bezeichner des Modells  |
| Dateiname | Textbasierte Bezeichner des Katalogs.<br>Nur Buchstaben (A-Z, a-Z), Zahlen (0-9), Bindestriche (--) und Unterstrichs (_) zulässig sind.<br>Die Maximallänge: 50 |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | Verwendungsdaten. Format:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Namen</th><th>Obligatorisch</th><th>Typ</th><th>Beschreibung</th></tr><tr><td>Benutzer-Id</td><td>Ja</td><td>[A-Z], [a-Z], [0-9], [_] & #40; Unterstrich & #41; [-] & #40; Gedankenstrich & #41;<br> Die Maximallänge: 255 </td><td>Eindeutiger Bezeichner eines Benutzers nachschlagen.</td></tr><tr><td>Listenelement-Id</td><td>Ja</td><td>[A-Z], [a-Z], [0-9], [& #95;] & #40; Unterstrich & #41; [-] & #40; Gedankenstrich & #41;<br> Die Maximallänge: 50</td><td>Eindeutige ID eines Elements.</td></tr><tr><td>Zeit</td><td>Nein</td><td>Datum im Format: JJJJ/MM/TTThh (z. B. 2013/06/20T10:00:00)</td><td>Zeitpunkt der Daten.</td></tr><tr><td>Ereignis</td><td>Nein; Wenn bereitgestellt hat, und klicken Sie dann muss auch Datum setzen</td><td>Eine der folgenden Optionen:<br>• Klicken Sie auf<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Kauf</td><td></td></tr></table><br>Maximale Dateigröße: 200MB<br><br>Beispiel:<br><pre>149452, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951, 1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Antwort**:

HTTP-Status Code: 200

- `Feed\entry\content\properties\LineCount`-Zeilenanzahl akzeptiert.
- `Feed\entry\content\properties\ErrorCount`-Anzahl der Zeilen, die nicht aufgrund eines Fehlers eingefügt wurden.
- `Feed\entry\content\properties\FileId`-Datei Bezeichner enthält.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add bulk usage data (usage file)</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
    </entry>
    </feed>


####<a name="912-using-data-acquisition"></a>9.1.2. Verwenden von Daten Acquisition
In diesem Abschnitt veranschaulicht, wie Ereignisse in Echtzeit an Azure maschinellen Learning Empfehlungen, in der Regel von der Website zu senden.

| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27`|
|KOPFZEILE   |`"Content-Type", "text/xml"`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
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
                    <PurchaseItem>
                        <ItemId>ABBF8081-C5C0-4F09-9701-E1C7AC78304A</ItemId>
                        <Count>1</Count>
                    </PurchaseItem>
                    <PurchaseItem>
                        <ItemId>21BF8088-B6C0-4509-870C-11C0AC7F304B</ItemId>
                        <Count>3</Count>
                    </PurchaseItem>
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

###<a name="92-list-model-usage-files"></a>9.2. Liste Modell verwendete Dateien
Metadaten aller Modell Verwendung Dateien abgerufen.
Die Verwendung-Dateien, die eine Seite nacheinander abgerufen. Jede Seite enthält 100 Elemente. Wenn Sie die Elemente an einem bestimmten Index abrufen möchten, können Sie den $skip Odata-Parameter verwenden. Beispielsweise wenn Sie Elemente ab Position 100 erhalten möchten, fügen Sie den Parameter $skip = 100 ein, um die Anfrage.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/ListModelUsageFiles?forModelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/ListModelUsageFiles?forModelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   forModelId  |   Eindeutiger Bezeichner des Modells |
|   apiVersion      | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

Die Antwort enthält einen Eintrag pro Datei Verwendung. Jeder Eintrag weist die folgenden Daten:

- `feed\entry\content\properties\Id`-Datei-ID Verwendung
- `feed\entry\content\properties\Length`-Verwendung Dateilänge in MB.
- `feed\entry\content\properties\DateModified`-Datum die Verwendung Datei erstellt wurde.
- `feed\entry\content\properties\UseInModel`-Gibt an, ob die Datei Verwendung im Modell verwendet wird.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of model's usage files info</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
            <updated>2014-10-30T09:40:25Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">4c067b42-e975-4cb2-8c98-a6ab80ed6d63</d:Id>
                    <d:Length m:type="Edm.Double">0</d:Length>
                    <d:DateModified m:type="Edm.String">10/30/2014 9:19:57 AM</d:DateModified>
                    <d:UseInModel m:type="Edm.String">true</d:UseInModel>
                </m:properties>
            </content>
        </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">3126d816-4e80-4248-8339-1ebbdb9d544d</d:Id>
                <d:Length m:type="Edm.Double">0.001</d:Length>
                <d:DateModified m:type="Edm.String">10/30/2014 9:21:44 AM</d:DateModified>
                <d:UseInModel m:type="Edm.String">true</d:UseInModel>
            </m:properties>
        </content>
    </entry>
</feed>

###<a name="93-get-usage-statistics"></a>9.3. Erste Verwendungsstatistiken
Ruft Verwendungsstatistik ab.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetUsageStatistics?modelId=%27<modelId>%27& startDate=%27<date>%27&endDate=%27<date>%27&eventTypes=%27<types>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/GetUsageStatistics?modelId=%271d20c34f-dca1-4eac-8e5d-f299e4e4ad66%27&startDate=%272014%2F10%2F17T00%3A00%3A00%27&endDate=%272014%2F11%2F16T00%3A00%3A00%27&eventTypes=%271%2C2%2C3%2C4%2C5%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells  |
| startDate |   Den Anfangstermin an. Format: JJJJ/MM/TTThh |
| Enddatum | Enddatum ein. Format: JJJJ/MM/TTThh |
| ' EventTypes ' |  Durch Trennzeichen getrennte Textzeichenfolge Ereignistypen oder Null alle Ereignisse zu erhalten.  |
| apiVersion | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

Eine Auflistung von Schlüssel/Wert-Elemente. Können Sie jeweils enthält die Summe der Ereignisse für einen bestimmten Ereignistyp, gruppiert nach Stunde.

- `feed\entry[i]\content\properties\Key`-Enthält die Zeit (gruppiert nach Stunde) und den Ereignistyp.
- `feed\entry[i]\content\properties\Value`-Zählen von Gesamt-Ereignis.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get usage statistics</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-18T11:39:16Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;Click</d:Event>
                <d:Value m:type="Edm.String">5</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;RecommendationClick</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/1/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/5/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="94-get-usage-file-sample"></a>9.4. Erste Verwendung Datei Stichprobe
Ruft die erste 2KB des Verwendung der Inhalt der Datei an.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetUsageFileSample?modelId=%27<modelId>%27& fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/GetUsageFileSample?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fileId=%274c067b42-e975-4cb2-8c98-a6ab80ed6d63%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells  |
| "FileID" |  Eindeutiger Bezeichner der Modelldatei Verwendung  |
| apiVersion | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

Im unformatierten Text-Format wird als Antwort zurückgegeben:
<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15 , WAHR, 1 235588, 21BF8088-B6C0-4509-870C-E1C7AC78304A, 2014/11/02T13:40:15, WAHR, 1 158254, 21BF8088-B6C0-4509-870 C-E1C7AC78304A, 2014/11/02T13:40:15, WAHR, 1 271195, 21BF8088-B6C0-4509-870 C-E1C7AC78304A, 2014/11/02T13:40:15, WAHR, 1 141157, 21BF8088-B6C0-4509-870 C-E1C7AC78304A, 2014/11/02T13:40:15, WAHR, 1 171118, 3BB5CB44-D143-4BDD-A55C-443964BF4B23, 2014/11/02T13:40:15, WAHR, 1 225087, 3BB5CB44-D143-4BDD-A55C-443964BF4B23, 2014/11/02T13:40:15, WAHR, 1
</pre>


###<a name="95-get-model-usage-file"></a>9.5. Erste Verwendung Modelldatei
Ruft den vollständigen Inhalt der Datei Verwendung.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetModelUsageFile?mid=%27<modelId>%27& fid=%27<fileId>%27&download=%27<download_value>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/GetModelUsageFile?mid=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fid=%273126d816-4e80-4248-8339-1ebbdb9d544d%27&download=%271%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| Teil | Eindeutiger Bezeichner des Modells  |
| FID | Eindeutiger Bezeichner der Modelldatei Verwendung |
| Herunterladen | 1 |
| apiVersion | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

Im unformatierten Text-Format wird als Antwort zurückgegeben:
<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15 ,True,1 235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 244881,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 50547,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 213090,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15 ,True,1 260655,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 72214,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 189334,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 36326,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 189336,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 189334,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1 260655,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1 162100,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1 54946,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15 , WAHR, 1 260965, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, WAHR, 1 102758, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, WAHR, 1 112602, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, WAHR, 1 163925, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, WAHR, 1 262998, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, WAHR, 1 144717, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, WAHR, 1
</pre>

###<a name="96-delete-usage-file"></a>9.6. Verwendung Datei löschen
Löscht die angegebene Verwendung Modelldatei an.

| HTTP-Methode | URI |
|:--------|:--------|
|LÖSCHEN     |`<rootURI>/DeleteUsageFile?modelId=%27<modelId>%27&fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/DeleteUsageFile?modelId=%270f86d698-d0f4-4406-a684-d13d22c47a73%27&fileId=%27f2e0b09d-be5c-46b2-9ac2-c7f622e5e1a5%27&apiVersion=%271.0%27`|

| Parameternamen    |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells  |
| "FileID" | Eindeutiger Bezeichner des zu löschenden der Datei |
| apiVersion | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200


###<a name="97-delete-all-usage-files"></a>9.7. Löschen Sie alle Dateien, Verwendung
Löscht alle Modell Verwendung Dateien an.

| HTTP-Methode | URI |
|:--------|:--------|
|LÖSCHEN     |`<rootURI>/DeleteAllUsageFiles?modelId=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/DeleteAllUsageFiles?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&apiVersion=%271.0%27`|

| Parameternamen    |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells  |
| apiVersion | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

##<a name="10-features"></a>10. Features von
Dieser Abschnitt listet die zum Abrufen von Informationen zu Features, wie die importierten Features und deren Werte, deren Rang aus, und wenn diese Rang zugewiesen wurde. Features werden als Teil der Daten im Katalog importiert, und klicken Sie dann ihre Rang zugeordnet wird, wenn ein Rang Build abgeschlossen ist.
Feature Rang kann je nach der Muster Verwendungsdaten und Art der Elemente ändern. Allerdings für konsistente Verwendung/Elemente, der Rang sollten nur kleine Fluktuationen.
Der Rang der Features ist eine nicht Negative Zahl an. Die Zahl 0 bedeutet, dass das Feature nicht eingestuft wurde (Wenn Sie diese API vor dem Abschluss der den ersten Rang Build Aufrufen geschieht). Das Datum, an dem der Rang zugeordnet wurde, heißt die Aktualität Punktzahl.

###<a name="101-get-features-info-for-last-rank-build"></a>10.1. Abrufen von Features Informationen (für die letzte Rang Build)
Die Featureinformationen, einschließlich Positionierung, für den letzten erfolgreichen Rang Build abgerufen.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten      |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&apiVersion=%271.0%27`

| Parameternamen    |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells  |
|samplingSize| Anzahl der Werte für das jeweilige Feature entsprechend der im Katalog vorhandenen Daten aufnehmen möchten. <br/>Mögliche Werte sind:<br> -1 - alle Beispiele. <br>0 - keine werden. <br>N - N Beispiele für jedes Featurenamen zurück.|
| apiVersion | 1.0 |
|||
| Anforderungstexts | KEINE |


**Antwort**:

HTTP-Status Code: 200

Die Antwort enthält eine Liste der Features Informationen Posten. Jeder Eintrag enthält:

- `feed/entry/content/m:properties/d:Name`-Feature Name.
- `feed/entry/content/m:properties/d:RankUpdateDate`-Datum mit der der Rang bei dieser Funktion, auch bekannt als zugewiesen wurde Punktzahl Aktualität Feature. Ein zurückliegende Datum ("0001-01-01T00:00:00') bedeutet, dass kein Rang Build durchgeführt wurde.
- `feed/entry/content/m:properties/d:Rank`-Feature Rang (Pufferzeiten). Rang von 2.0 und von wird ein guter Feature angesehen.
- `feed/entry/content/m:properties/d:SampleValues`-Durch Trennzeichen getrennte Liste von Werten auf die angeforderte werden Größe.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get the features of a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-01-08T13:15:02Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Author</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Publisher</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1"/>
        <contenttype="application/xml">
        <m:properties>
            <d:Name m:type="Edm.String">Year</d:Name>
            <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
            <d:Rank m:type="Edm.String">0</d:Rank>
            <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
        </m:properties>
        </content>
    </entry>
</feed>


###<a name="102-get-features-info-for-specific-rank-build"></a>10.2. Abrufen von Features Informationen (für bestimmte Rang Build)

Die Featureinformationen, einschließlich der Rangfolge für einen bestimmten Rang Build abgerufen.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten      |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&rankBuildId=<rankBuildId>&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&rankBuildId=1000551&apiVersion=%271.0%27`

| Parameternamen    |   Gültige Werte    |
|:--------          |:--------          |
| modelId | Eindeutiger Bezeichner des Modells  |
|samplingSize| Anzahl der Werte für das jeweilige Feature entsprechend der im Katalog vorhandenen Daten aufnehmen möchten.<br/> Mögliche Werte sind:<br> -1 - alle Beispiele. <br>0 - keine werden. <br>N - N Beispiele für jedes Featurenamen zurück.|
|rankBuildId| Eindeutiger Bezeichner des den Rang Build oder-1 für den letzten Rang build|
| apiVersion | 1.0 |
|||
| Anforderungstexts | KEINE |


**Antwort**:

HTTP-Status Code: 200

Die Antwort enthält eine Liste der Features Informationen Posten. Jeder Eintrag enthält:

- `feed/entry/content/m:properties/d:Name`-Feature Name.
- `feed/entry/content/m:properties/d:RankUpdateDate`-Datum mit der der Rang bei dieser Funktion, auch bekannt als zugewiesen wurde Punktzahl Aktualität Feature. Ein zurückliegende Datum ("0001-01-01T00:00:00') bedeutet, dass kein Rang Build durchgeführt wurde.
- `feed/entry/content/m:properties/d:Rank`-Feature Rang (Pufferzeiten). Rang von 2.0 und von wird ein guter Feature angesehen.
- `feed/entry/content/m:properties/d:SampleValues`-Durch Trennzeichen getrennte Liste von Werten auf die angeforderte werden Größe.

OData

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get the features of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:54:22Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Author</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">3.38867426</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Publisher</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.67839336</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Year</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.12352145</d:Rank>
                    <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
                </m:properties>
            </content>
        </entry>
    </feed>


##<a name="11-build"></a>11. Erstellen von

  In diesem Abschnitt wird erläutert, dass die verschiedenen APIs auf Builds beziehen. Es gibt 3 Arten von Builds: ein Empfehlungen erstellen, eine Rang erstellen und eine FBT (häufig zusammen gekauft) erstellen.

Die Empfehlungen-Generator Zweck ist ein Empfehlungen Modell für Vorhersagen verwendet generieren. Vorhersagen (für diese Art von Generator) stammen, es gibt zwei Arten:
* I2I – auch bekannt als Elementen Empfehlungen - angegeben, ein Element oder eine Liste von Elementen, die diese Option wird eine Liste von Elementen Vorhersagen, die vermutlich hoher relevante vorhanden sind.
* U2I – auch bekannt als Benutzer zu Element Empfehlungen – der angegebene Benutzer-Id (und optional eine Liste von Elementen) wird diese Option eine Liste mit Elementen Vorhersagen zu wahrscheinlich hoher Zinsen für den angegebenen Benutzer (und der weiteren Auswahl von Elementen) aufweisen. Die Empfehlungen U2I basieren auf des Verlaufs von Elementen, die für den Benutzer auf die Uhrzeit relevante wurden, die das Modell erstellt wurde.

Rang erstellen wird eine technische erstellen, die Sie wissen möchten, die Nützlichkeit Ihrer Features kann. Um das beste Ergebnis für ein Empfehlungen-Modell, das im Zusammenhang mit Features zu gelangen, sollten Sie in der Regel, die folgende Schritte durchführen:
- Lösen Sie der einen Rang Build aus (es sei denn, die Bewertung der Features unveränderliche ist), und warten Sie, bis Sie das Feature Punktzahl erhalten.
- Rufen Sie den Rang der Features, indem Sie die [Features Informationen](#101-get-features-info-for-last-rank-build) API aufrufen.
- Konfigurieren eines Empfehlungen erstellen mit den folgenden Parametern:
    - `useFeatureInModel`– Legen Sie auf True.
    - `ModelingFeatureList`– Legen Sie auf eine kommagetrennte Liste der Features, die mit einem Faktor von 2.0 oder mehr (nach dem Rang, die Sie im vorherigen Schritt abgerufen).
    - `AllowColdItemPlacement`– Legen Sie auf True.
    - Optional können Sie festlegen `EnableFeatureCorrelation` true und `ReasoningFeatureList` zur Liste der Features für erläuterungen (normalerweise der gleichen Liste der Features, die in Modellierung oder eine Unterliste) verwendet werden soll.
- Auslösen des Builds Empfehlungen mit den Parametern konfigurierten.

Hinweis: Wenn Sie keine Parameter konfigurieren (z. B. die Empfehlungen-Generator ohne Parameter aufrufen) oder die Verwendung von Features nicht explizit deaktivieren (z. B. `UseFeatureInModel` auf False gesetzt), Einrichten des Systems im Zusammenhang mit dem Feature Parameter erläutert Werte oben, für den Fall, dass ein Rang Build vorhanden ist.

Es gibt keine Beschränkung auf ein Rang Build und ein Build Empfehlungen für das gleiche Modell gleichzeitig ausgeführt. Sie können keine trotzdem zwei Versionen von desselben Typs im gleichen Modell parallel ausführen.

Erstellen einer FBT (häufig zusammen gekauft) ist noch eine andere Empfehlungen Algorithmus aufgerufen manchmal "vorsichtig" Recommender, was gute für Kataloge, die nicht in der Natur einheitlichen sind (einheitlichen: Bücher, Filme, einige Lebensmittel, Weise; nicht einheitlichen: Computer und Geräte, Domain-übergreifende, sehr unterschiedlichen).

Hinweis: Wenn die Verwendung-Dateien, die Sie hochgeladen das optionale Feld "Ereignistyp" enthalten wird dann für FBT Modellierung nur "Erwerben" Ereignisse verwendet werden. Wenn keine Ereignistyp bereitgestellt wird, dass alle Ereignisse als erwerben berücksichtigt werden.


####<a name="111-build-parameters"></a>11.1 erstellen Sie Parameter

Jedes Typs erstellen kann über einen Satz von Parametern (unten gezeigten) konfiguriert werden. Wenn Sie die Parameter nicht konfiguriert haben, wird das System automatisch Werte in die Parameter entsprechend den Informationen, die zum Zeitpunkt vorhanden, dass Sie eine eigene auslösen Attribut.

#####<a name="1111-usage-condenser"></a>11.1.1. Verwendung Schliff
Benutzer oder Elemente mit Verwendung folgende Punkte möglicherweise lauter als Informationen enthalten. Das System versucht, Vorhersagen die minimale Anzahl der Verwendung Punkte pro Benutzer/Element in einem Datenmodell verwendet werden. Dieser Wert wird innerhalb des durch die Parameter ItemCutoffLowerBound und ItemCutoffUpperBound für Elemente definierten Bereichs und des durch die Parameter UserCutOffLowerBound und UserCutoffUpperBound für Benutzer definierten Bereichs liegen. Der Effekt Schliff auf Elemente oder Benutzer kann durch Festlegen der entsprechenden Grenzen mindestens eine 0 (null) minimiert werden.

#####<a name="1112-rank-build-parameters"></a>11.1.2. Rang erstellen Parameter

In der nachfolgenden Tabelle wird die Generator-Parameter für einen Rang Build dargestellt.

|Schlüssel|Beschreibung|Typ|Gültiger Wert|
|:-----|:----|:----|:---|
|NumberOfModelIterations | Die Anzahl der Iterationen, die das Modell führt, wird durch die Gesamtzeit berechnen und die Genauigkeit Modell wiedergegeben. Je höher der Wert, der eine höhere Genauigkeit erhalten Sie, aber die Zeit berechnen dauert länger.| Ganze Zahl | 10-50 |
| NumberOfModelDimensions | Die Anzahl der Dimensionen bezieht sich auf die Anzahl der "Features" Modell versucht, zu den Daten zu suchen. Erhöhen der Anzahl von Dimensionen ermöglicht besser Feinabstimmung der Ergebnisse in kleinere Cluster. Jedoch verhindert zu viele Dimensionen Modell Korrelationen zwischen Elementen suchen. | Ganze Zahl | 10-40 |
|ItemCutOffLowerBound| Definiert das Element Untergrenze für tief. Finden Sie unter Verwendung Schliff oben. | Ganze Zahl | 2 oder mehr (0 deaktivieren Schliff) |
|ItemCutOffUpperBound| Definiert die Obergrenze Element für tief. Finden Sie unter Verwendung Schliff oben. | Ganze Zahl | 2 oder mehr (0 deaktivieren Schliff) |
|UserCutOffLowerBound| Definiert die Benutzer Untergrenze für tief. Finden Sie unter Verwendung Schliff oben. | Ganze Zahl | 2 oder mehr (0 deaktivieren Schliff) |
|UserCutOffUpperBound| Definiert die Benutzer Obergrenze für tief. Finden Sie unter Verwendung Schliff oben. | Ganze Zahl | 2 oder mehr (0 deaktivieren Schliff) |

#####<a name="1113-recommendation-build-parameters"></a>11.1.3. Empfehlungen-Generator-Parameter
In der nachfolgenden Tabelle wird die Generator-Parameter für Empfehlungen Build dargestellt.

|Schlüssel|Beschreibung|Typ|Gültiger Wert|
|:-----|:----|:----|:---|
|NumberOfModelIterations | Die Anzahl der Iterationen, die das Modell führt, wird durch die Gesamtzeit berechnen und die Genauigkeit Modell wiedergegeben. Je höher der Wert, der eine höhere Genauigkeit erhalten Sie, aber die Zeit berechnen dauert länger.| Ganze Zahl | 10-50 |
| NumberOfModelDimensions | Die Anzahl der Dimensionen bezieht sich auf die Anzahl der "Features" Modell versucht, zu den Daten zu suchen. Erhöhen der Anzahl von Dimensionen ermöglicht besser Feinabstimmung der Ergebnisse in kleinere Cluster. Jedoch verhindert zu viele Dimensionen Modell Korrelationen zwischen Elementen suchen. | Ganze Zahl | 10-40 |
|ItemCutOffLowerBound| Definiert das Element Untergrenze für tief. Finden Sie unter Verwendung Schliff oben. | Ganze Zahl | 2 oder mehr (0 deaktivieren Schliff) |
|ItemCutOffUpperBound| Definiert die Obergrenze Element für tief. Finden Sie unter Verwendung Schliff oben. | Ganze Zahl | 2 oder mehr (0 deaktivieren Schliff) |
|UserCutOffLowerBound| Definiert die Benutzer Untergrenze für tief. Finden Sie unter Verwendung Schliff oben. | Ganze Zahl | 2 oder mehr (0 deaktivieren Schliff) |
|UserCutOffUpperBound| Definiert die Benutzer Obergrenze für tief. Finden Sie unter Verwendung Schliff oben. | Ganze Zahl | 2 oder mehr (0 deaktivieren Schliff) |
| Beschreibung | Erstellen Sie eine Beschreibung ein. | Zeichenfolge | Text, maximal 512 Zeichen |
| EnableModelingInsights | Ermöglicht Ihnen, Kennzahlen im Modell Empfehlungen zu berechnen. | Boolesch | Wahrheitswert |
| UseFeaturesInModel | Zeigt an, ob Funktionen verwendet werden können, damit Sie das Modell Empfehlungen verbessern können. | Boolesch | Wahrheitswert |
| ModelingFeatureList | Durch Trennzeichen getrennte Liste der Featurenamen die eigene Empfehlungen verwendet werden, um die Empfehlungen zu verbessern. | Zeichenfolge | Bereitstellen von Namen, bis zu 512 Zeichen |
| AllowColdItemPlacement | Zeigt an, ob die Empfehlungen kalt Elemente auch über Features Ähnlichkeit Pushbenachrichtigungen sollte. | Boolesch | Wahrheitswert |
| EnableFeatureCorrelation | Zeigt an, ob Features in Logik verwendet werden können. | Boolesch | Wahrheitswert |
| ReasoningFeatureList | Durch Trennzeichen getrennte Liste der Featurenamen für Schlussfolgerungen Sätze (z. B. Empfehlungen erläuterungen) verwendet werden soll.  | Zeichenfolge | Bereitstellen von Namen, bis zu 512 Zeichen |
| EnableU2I | Der individuelle Empfehlungen auch bekannt als zulassen U2I (Element Empfehlungen Benutzer). | Boolesch | Wahrheitswert (standardmäßig WAHR) |

#####<a name="1114-fbt-build-parameters"></a>11.1.4. FBT Buildparameter
In der nachfolgenden Tabelle wird die Generator-Parameter für Empfehlungen Build dargestellt.

|Schlüssel|Beschreibung|Typ|Gültiger Wert (Standard)|
|:-----|:----|:----|:---|
|FbtSupportThreshold | Wie vorsichtig ist das Modell. Anzahl der gemeinsame Vorkommen von Elementen für die Modellierung berücksichtigt werden.| Ganze Zahl | 3-50 (6) |
|FbtMaxItemSetSize | Umschließt die Anzahl der Elemente in einer Gruppe von häufig verwendeten an.| Ganze Zahl | 2-3 (2) |
|FbtMinimalScore | Minimale Punktzahl, die ein häufig verwendeten Satz in die zurückgegebenen Ergebnisse enthalten sein soll. Je höher, desto besser.| Double | 0 und höher (0) |
|FbtSimilarityFunction | Definiert die Funktion Ähnlichkeit vom Build verwendet werden. Heben Sie bevorzugt Serendipity, gemeinsame Vorkommen bevorzugt Vorhersagbarkeit und Jaccard ist eine übersichtliche Kompromisse zwischen den beiden. | Zeichenfolge | Cooccurrence, heben Sie, Jaccard (heben Sie) |


###<a name="112-trigger-a-recommendation-build"></a>11.2. Auslösen eines Empfehlungen erstellen

  Standardmäßig wird diese API die Erstellung eines Empfehlungen-Modells auslösen. Zum Auslösen eines Rang erstellen (um Features Punktzahl), die Generator-API Variante mit Parameter erstellen verwendet werden soll.


| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27`|
|KOPFZEILE   |`"Content-Type", "text/xml"`(Wenn der Textbereich anfordern senden)|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells  |
| userDescription | Textbasierte Bezeichner des Katalogs. Beachten Sie, wenn Sie Leerzeichen verwenden Sie mit % 20 stattdessen codiert werden müssen. Finden Sie unter oben genannten Beispiel.<br>Die Maximallänge: 50 |
| apiVersion | 1.0 |
|||
| Anforderungstexts | Wenn leer wird der Build mit den Parametern der Standard-Generator ausgeführt werden.<br><br>Wenn Sie die Parameter erstellen festlegen möchten, senden Sie die Parameter in den Textkörper wie im folgenden Beispiel als XML. (Siehe Abschnitt "Build Parameter" für eine Erläuterung der Parameter).`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>` |

**Antwort**:

HTTP-Status Code: 200

Dies ist eine asynchrone API. Sie erhalten eine eigene ID als Antwort. Sie erhalten Sie, wenn der Build beendet wurde, rufen Sie die API "Get erstellt Status der ein Model", und suchen Sie diese Generator-ID in der Antwort. Beachten Sie, dass ein Build zu Stunden abhängig von der Größe der Daten Minuten dauern kann.

Sie können keine Empfehlungen bis zum den Build nutzen endet.

Status für gültige erstellen:

- Erstellen - Generator Anforderung erstellt wurde.
- In der Warteschlange - Generator Anforderung gesendet wurde, und es ist in der Warteschlange.
- Baustein - Generator wird ausgeführt.
- Erfolg - Generator wurde erfolgreich beendet.
- Fehler - Workflow erstellen mit einem Fehler beendet.
- Abgebrochen - abgebrochen erstellen.
- Absagen - Anforderung einer Abbrechen für den Build gesendet wurde.


Beachten Sie, dass die ID erstellen unter den folgenden Pfad gefunden werden kann:`Feed\entry\content\properties\Id`

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

###<a name="113-trigger-build-recommendation-rank-or-fbt"></a>11.3. Trigger erstellen (empfohlen, Rang oder FBT)

| HTTP-Methode | URI |
|:--------|:--------|
|Bereitstellen     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&buildType=%27<buildType>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&buildType=%27Ranking%27&apiVersion=%271.0%27`|
|KOPFZEILE   |`"Content-Type", "text/xml"`(Wenn der Textbereich anfordern senden)|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells  |
| userDescription | Textbasierte Bezeichner des Katalogs. Beachten Sie, wenn Sie Leerzeichen verwenden Sie mit % 20 stattdessen codiert werden müssen. Finden Sie unter oben genannten Beispiel.<br>Die Maximallänge: 50 |
| Buildtyp | Typ des Aufrufen der erstellen: <br/> -'Empfehlungen' für Empfehlungen erstellen <br> -'Rangfolgen' für Rang erstellen <br/> -'Fbt' für FBT erstellen
| apiVersion | 1.0 |
|||
| Anforderungstexts | Wenn leer wird der Build mit den Parametern der Standard-Generator ausgeführt werden.<br><br>Wenn Sie eigene Parameter festlegen möchten, senden Sie sie in den Textkörper wie im folgenden Beispiel als XML. (Siehe Abschnitt "Build Parameter" für eine Erläuterung und die vollständige Liste der Parameter).`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>` |

**Antwort**:

HTTP-Status Code: 200

Dies ist eine asynchrone API. Sie erhalten eine eigene ID als Antwort. Sie erhalten Sie, wenn der Build beendet wurde, rufen Sie die API "Get erstellt Status der ein Model", und suchen Sie diese Generator-ID in der Antwort. Beachten Sie, dass ein Build zu Stunden abhängig von der Größe der Daten Minuten dauern kann.

Sie können keine Empfehlungen bis zum den Build nutzen endet.

Status für gültige erstellen:

- Erstellen - Modell erstellt wurde.
- In der Warteschlange - Modellbuild ausgelöst wurde, und es wird in der Warteschlange.
- Baustein - Modell wird erstellt.
- Erfolg - Generator wurde erfolgreich beendet.
- Fehler - Workflow erstellen mit einem Fehler beendet.
- Abgebrochen - abgebrochen erstellen.
- Absagen - Generator abgebrochen wird.

Beachten Sie, dass die ID erstellen unter den folgenden Pfad gefunden werden kann:`Feed\entry\content\properties\Id`

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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




###<a name="114-get-builds-status-of-a-model"></a>11.4. Get erstellt Status eines Modells
Builds und deren Status für ein angegebenes Modell abgerufen.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27`|


|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   modelId         |   Eindeutiger Bezeichner des Modells  |
|   onlyLastBuild   |   Gibt an, ob den Build Verlauf des Modells oder nur den Status des letzten Build zurückgeben  |
|   apiVersion      |   1.0                                 |


**Antwort**:

HTTP-Status Code: 200

Die Antwort enthält einen Eintrag pro erstellen. Jeder Eintrag weist die folgenden Daten:

- `feed/entry/content/properties/UserName`-Namen des Benutzers.
- `feed/entry/content/properties/ModelName`-Name des Modells.
- `feed/entry/content/properties/ModelId`-Modelle eindeutige ID an.
- `feed/entry/content/properties/IsDeployed`-, Ob die erstellen (auch bekannt als bereitgestellt wird aktive Build).
- `feed/entry/content/properties/BuildId`– Erstellen Sie eindeutige ID ein.
- `feed/entry/content/properties/BuildType`-Typ von der erstellen.
- `feed/entry/content/properties/Status`-Erstellen Sie Status. Kann eine der folgenden: Fehler, Gebäude, in Warteschlange, Cancelling, abgebrochen, Erfolg.
- `feed/entry/content/properties/StatusMessage`-Detaillierte Statusmeldung (gilt nur für bestimmte Staaten).
- `feed/entry/content/properties/Progress`-Status (%) erstellen.
- `feed/entry/content/properties/StartTime`-Generator Startzeit an.
- `feed/entry/content/properties/EndTime`-Erstellen Sie Endzeit.
- `feed/entry/content/properties/ExecutionTime`-Erstellen Sie Dauer.
- `feed/entry/content/properties/ProgressStep`-Details der aktuellen Stufe der eine laufende Erstellung.

Status für gültige erstellen:
- Erstellt - wurde Eintrag für die Anfrage erstellen erstellt.
- In der Warteschlange - Generator Anforderung ausgelöst wurde und sie in der Warteschlange ist.
- Baustein - Generator ist in Bearbeitung.
- Erfolg - Generator wurde erfolgreich beendet.
- Fehler - Workflow erstellen mit einem Fehler beendet.
- Abgebrochen - abgebrochen erstellen.
- Absagen - Generator abgebrochen wird.

Gültige Werte für Typ erstellen:
- Rang - Rang erstellen.
- Empfehlungen - Empfehlungen erstellen.


OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T17:51:10Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T17:51:10Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


###<a name="115-get-builds-status"></a>11.5. Get erstellt Status
Ruft erstellen Status aller Modelle eines Benutzers nachschlagen.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=<bool>&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=true&apiVersion=%271.0%27`|


|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
|   onlyLastBuild   |   Gibt an, ob den Build Verlauf des Modells oder nur den Status des letzten Build zurückgeben. |
|   apiVersion      |   1.0                                 |


**Antwort**:

HTTP-Status Code: 200

Die Antwort enthält einen Eintrag pro erstellen. Jeder Eintrag weist die folgenden Daten:

- `feed/entry/content/properties/UserName`-Namen des Benutzers.
- `feed/entry/content/properties/ModelName`-Name des Modells.
- `feed/entry/content/properties/ModelId`-Modelle eindeutige ID an.
- `feed/entry/content/properties/IsDeployed`-, Ob der Build bereitgestellt wird.
- `feed/entry/content/properties/BuildId`– Erstellen Sie eindeutige ID ein.
- `feed/entry/content/properties/BuildType`-Typ von der erstellen.
- `feed/entry/content/properties/Status`-Erstellen Sie Status. Kann eine der folgenden: Fehler, Gebäude, in Warteschlange, abgebrochen, Cancelling, Erfolg.
- `feed/entry/content/properties/StatusMessage`-Detaillierte Statusmeldung (gilt nur für bestimmte Staaten).
- `feed/entry/content/properties/Progress`-Status (%) erstellen.
- `feed/entry/content/properties/StartTime`-Generator Startzeit an.
- `feed/entry/content/properties/EndTime`-Erstellen Sie Endzeit.
- `feed/entry/content/properties/ExecutionTime`-Erstellen Sie Dauer.
- `feed/entry/content/properties/ProgressStep`-Details der aktuellen Stufe der eine laufende Erstellung.

Status für gültige erstellen:
- Erstellt - wurde Eintrag für die Anfrage erstellen erstellt.
- In der Warteschlange - Generator Anforderung ausgelöst wurde und sie in der Warteschlange ist.
- Baustein - Generator ist in Bearbeitung.
- Erfolg - Generator wurde erfolgreich beendet.
- Fehler - Workflow erstellen mit einem Fehler beendet.
- Abgebrochen - abgebrochen erstellen.
- Absagen - Generator abgebrochen wird.


Gültige Werte für Typ erstellen:
- Rang - Rang erstellen.
- Empfehlungen - Empfehlungen erstellen.


OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T18:41:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T18:41:21Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


###<a name="116-delete-build"></a>11.6. Löschen von erstellen
Löscht eine erstellen.

HINWEIS: <br>Erstellen einer aktiven kann nicht gelöscht werden. Das Modell sollte an einen anderen aktiven Build aktualisiert werden, bevor Sie es löschen.<br>Sie können eine eigene in Bearbeitung nicht löschen. Sie sollten die erstellen zuerst durch Aufrufen <strong>Erstellen abbrechen</strong>Abbrechen.

| HTTP-Methode | URI |
|:--------|:--------|
|LÖSCHEN     |`<rootURI>/DeleteBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/DeleteBuild?buildId=%271500068%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| buildId | Eindeutiger Bezeichner des der erstellen. |
| apiVersion | 1.0 |

**Antwort:**

HTTP-Status Code: 200

###<a name="117-cancel-build"></a>11.7. Abbrechen erstellen
Bricht ab einer erstellen, der im Status erstellen.

| HTTP-Methode | URI |
|:--------|:--------|
|SETZEN     |`<rootURI>/CancelBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/CancelBuild?buildId=%271500076%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| buildId | Eindeutiger Bezeichner des der erstellen. |
| apiVersion | 1.0 |

**Antwort:**

HTTP-Status Code: 200

###<a name="118-get-build-parameters"></a>11,8. Abrufen von Parametern erstellen
Ruft Parameter zu erstellen.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetBuildParameters?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/GetBuildParameters?buildId=%271000653%27&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| buildId | Eindeutiger Bezeichner des der erstellen. |
| apiVersion | 1.0 |

**Antwort:**

HTTP-Status Code: 200

Diese API gibt eine Auflistung von Schlüssel/Wert-Elementen. Jedes Element stellt einen Parameter und dessen Wert:
- `feed/entry/content/properties/Key`-Erstellen Sie Parametername.
- `feed/entry/content/properties/Value`-Erstellen Sie Parameterwert.

In der nachfolgenden Tabelle wird den Wert, der jedem Schlüssel dargestellt.

|Schlüssel|Beschreibung|Typ|Gültiger Wert|
|:-----|:----|:----|:---|
|NumberOfModelIterations | Die Anzahl der Iterationen, die das Modell führt, wird durch die Gesamtzeit berechnen und die Genauigkeit Modell wiedergegeben. Je höher der Wert, der eine höhere Genauigkeit erhalten Sie, aber die Zeit berechnen dauert länger.| Ganze Zahl | 10-50 |
| NumberOfModelDimensions | Die Anzahl der Dimensionen bezieht sich auf die Anzahl der "Features" Modell versucht, zu den Daten zu suchen. Erhöhen der Anzahl von Dimensionen ermöglicht besser Feinabstimmung der Ergebnisse in kleinere Cluster. Jedoch verhindert zu viele Dimensionen Modell Korrelationen zwischen Elementen suchen. | Ganze Zahl | 10-40 |
|ItemCutOffLowerBound| Definiert das Element Untergrenze für tief. Finden Sie unter Verwendung Schliff oben. | Ganze Zahl | 2 oder mehr (0 deaktivieren Schliff) |
|ItemCutOffUpperBound| Definiert die Obergrenze Element für tief. Finden Sie unter Verwendung Schliff oben. | Ganze Zahl | 2 oder mehr (0 deaktivieren Schliff) |
|UserCutOffLowerBound| Definiert die Benutzer Untergrenze für tief. Finden Sie unter Verwendung Schliff oben. | Ganze Zahl | 2 oder mehr (0 deaktivieren Schliff) |
|UserCutOffUpperBound| Definiert die Benutzer Obergrenze für tief. Finden Sie unter Verwendung Schliff oben. | Ganze Zahl | 2 oder mehr (0 deaktivieren Schliff) |
| Beschreibung | Erstellen Sie eine Beschreibung ein. | Zeichenfolge | Text, maximal 512 Zeichen |
| EnableModelingInsights | Ermöglicht Ihnen, Kennzahlen im Modell Empfehlungen zu berechnen. | Boolesch | Wahrheitswert |
| UseFeaturesInModel | Zeigt an, ob Funktionen verwendet werden können, damit Sie das Modell Empfehlungen verbessern können. | Boolesch | Wahrheitswert |
| ModelingFeatureList | Durch Trennzeichen getrennte Liste der Featurenamen die eigene Empfehlungen verwendet werden, um die Empfehlungen zu verbessern. | Zeichenfolge | Bereitstellen von Namen, bis zu 512 Zeichen |
| AllowColdItemPlacement | Zeigt an, ob die Empfehlungen kalt Elemente auch über Features Ähnlichkeit Pushbenachrichtigungen sollte. | Boolesch | Wahrheitswert |
| EnableFeatureCorrelation | Zeigt an, ob Features in Logik verwendet werden können. | Boolesch | Wahrheitswert |
| ReasoningFeatureList | Durch Trennzeichen getrennte Liste der Featurenamen für Schlussfolgerungen Sätze (z. B. Empfehlungen erläuterungen) verwendet werden soll.  | Zeichenfolge | Bereitstellen von Namen, bis zu 512 Zeichen |


OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get build parameters</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:50:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UseFeaturesInModel</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">AllowColdItemPlacement</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableFeatureCorrelation</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableModelingInsights</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelIterations</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
            <titletype="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelDimensions</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <linkrel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ModelingFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ReasoningFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">Description</d:Key>
                    <d:Value m:type="Edm.String">rankBuild</d:Value>
                </m:properties>
            </content>
        </entry>
    </feed>

##<a name="12-recommendation"></a>12. Empfehlungen
###<a name="121-get-item-recommendations-for-active-build"></a>12.1. Abrufen von Element Empfehlungen (für aktive Build)

Lassen Sie sich von der aktiven Build vom Typ "Empfohlen" oder "Fbt" ausgehend von einer Liste der Basis (Eingabewerte) Elemente.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells |
| Artikelnummern ein. | Durch Trennzeichen getrennte Liste der Elemente empfehlen. <br>Ist der aktive Build vom Typ FBT können Sie nur ein Element senden. <br>Die Maximallänge: 1024 |
| numberOfResults | Anzahl der Ergebnisse erforderlich <br> Max: 150 |
| includeMetatadata | Eine zukünftige Verwendung, immer false |
| apiVersion | 1.0 |

**Antwort:**

HTTP-Status Code: 200


Die Antwort enthält einen Eintrag pro Element empfohlen. Jeder Eintrag weist die folgenden Daten:
- `Feed\entry\content\properties\Id`-Empfohlene Element-ID an.
- `Feed\entry\content\properties\Name`-Name des Elements.
- `Feed\entry\content\properties\Rating`-Bewertung von empfohlen; höherer Zahl bedeutet höhere KONFIDENZ.
- `Feed\entry\content\properties\Reasoning`-Empfehlungen Schlussfolgerungen (z. B. Empfehlungen erläuterungen).

Im folgenden Beispielantwort enthält 10 empfohlene Elemente.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
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

###<a name="122-get-item-recommendations-of-a-specific-build"></a>12.2. Lassen Sie sich von Element (für einen bestimmten Build)

Lassen Sie sich von einem bestimmten Build vom Typ "Empfohlen" oder "Fbt" ein.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=1234&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells |
| Artikelnummern ein. | Durch Trennzeichen getrennte Liste der Elemente empfehlen. <br>Ist der aktive Build vom Typ FBT können Sie nur ein Element senden. <br>Die Maximallänge: 1024 |
| numberOfResults | Anzahl der Ergebnisse erforderlich <br> Max: 150  |
| includeMetatadata | Eine zukünftige Verwendung, immer false
| buildId | die Generator-Id für diese Anforderung Empfehlungen verwenden |
| apiVersion | 1.0 |

**Antwort:**

HTTP-Status Code: 200


Die Antwort enthält einen Eintrag pro Element empfohlen. Jeder Eintrag weist die folgenden Daten:
- `Feed\entry\content\properties\Id`-Empfohlene Element-ID an.
- `Feed\entry\content\properties\Name`-Name des Elements.
- `Feed\entry\content\properties\Rating`-Bewertung von empfohlen; höherer Zahl bedeutet höhere KONFIDENZ.
- `Feed\entry\content\properties\Reasoning`-Empfehlungen Schlussfolgerungen (z. B. Empfehlungen erläuterungen).

Finden Sie ein Beispiel Antwort 12.1

###<a name="123-get-fbt-recommendations-for-active-build"></a>12.3. Abrufen von FBT Empfehlungen (für aktive Build)

Lassen Sie sich von der aktiven Build vom Typ "Fbt" eines Elements Startwert (Eingabe) abhängig.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=<double>&includeMetadata=false&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells |
| itemId | Element empfehlen. <br>Die Maximallänge: 1024 |
| numberOfResults | Anzahl der Ergebnisse erforderlich <br>Max: 150 |
| minimalScore | Minimale Punktzahl, die ein häufig verwendeten Satz in die zurückgegebenen Ergebnisse enthalten sein soll |
| includeMetatadata | Eine zukünftige Verwendung, immer false |
| apiVersion | 1.0 |

**Antwort:**

HTTP-Status Code: 200


Die Antwort enthält einen Eintrag pro empfohlene Element festlegen (eine Gruppe von Elementen, wenn Sie in der Regel zusammen mit den Startwert/Eingabemethoden-Artikel gekauft werden). Jeder Eintrag weist die folgenden Daten:
- `Feed\entry\content\properties\Id1`-Empfohlene Element-ID an.
- `Feed\entry\content\properties\Name1`-Name des Elements.
- `Feed\entry\content\properties\Id2`Empfohlene Element 2nd - ID (optional).
- `Feed\entry\content\properties\Name2`-Name des 2. Elements (optional).
- `Feed\entry\content\properties\Rating`-Bewertung von empfohlen; höherer Zahl bedeutet höhere KONFIDENZ.
- `Feed\entry\content\properties\Reasoning`-Empfehlungen Schlussfolgerungen (z. B. Empfehlungen erläuterungen).

Im folgenden Beispielantwort enthält 3 empfohlene Element Sätze.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemId='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">159</d:Id1>
        <d:Name1 m:type="Edm.String">159</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '159'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">52</d:Id1>
        <d:Name1 m:type="Edm.String">52</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.533343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '52'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">35</d:Id1>
        <d:Name1 m:type="Edm.String">35</d:Name1>
        <d:Id2 m:type="Edm.String">102</d:Id2>
        <d:Name2 m:type="Edm.String">102</d:Name2>
        <d:Rating m:type="Edm.Double">0.523343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '35' and '102'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
    </feed>

###<a name="124-get-fbt-recommendations-of-a-specific-build"></a>12.4. Lassen Sie sich von FBT (für einen bestimmten Build)

Lassen Sie sich von einem bestimmten Build vom Typ "Fbt" ein.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=0.1&includeMetadata=false&buildId=1234&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells |
| itemId | Element empfehlen. <br>Die Maximallänge: 1024 |
| numberOfResults | Anzahl der Ergebnisse erforderlich <br>Max: 150 |
| minimalScore | Minimale Punktzahl, die ein häufig verwendeten Satz in die zurückgegebenen Ergebnisse enthalten sein soll |
| includeMetatadata | Eine zukünftige Verwendung, immer false |
| buildId | die Generator-Id für diese Anforderung Empfehlungen verwenden |
| apiVersion | 1.0 |

**Antwort:**

HTTP-Status Code: 200


Die Antwort enthält einen Eintrag pro empfohlene Element festlegen (eine Gruppe von Elementen, wenn Sie in der Regel zusammen mit den Startwert/Eingabemethoden-Artikel gekauft werden). Jeder Eintrag weist die folgenden Daten:
- `Feed\entry\content\properties\Id1`-Empfohlene Element-ID an.
- `Feed\entry\content\properties\Name1`-Name des Elements.
- `Feed\entry\content\properties\Id2`Empfohlene Element 2nd - ID (optional).
- `Feed\entry\content\properties\Name2`-Name des 2. Elements (optional).
- `Feed\entry\content\properties\Rating`-Bewertung von empfohlen; höherer Zahl bedeutet höhere KONFIDENZ.
- `Feed\entry\content\properties\Reasoning`-Empfehlungen Schlussfolgerungen (z. B. Empfehlungen erläuterungen).

Finden Sie ein Beispiel Antwort in 12.3

###<a name="125-get-user-recommendations-for-active-build"></a>12.5. Lassen Sie die Benutzer sich (für aktive Build)

Lassen Sie Benutzer sich von einer Build vom Typ "Empfohlen" werden als aktiven Build gekennzeichnet.

Die API wird eine Liste der geschätzten Artikel in Übereinstimmung mit den Verlauf Verwendung des Benutzers zurück.

Hinweise: 
 1. Es gibt keine Benutzer Empfehlungen für FBT erstellen.
 2. Ist der aktiven Erstellen von FBT dieser Methode wird ein Fehler zurückgegeben.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells |
| Benutzer-ID  | Eindeutiger Bezeichner des Benutzers |
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

Finden Sie ein Beispiel Antwort 12.1

###<a name="126-get-user-recommendations-with-item-list-for-active-build"></a>12.6. Abrufen von Empfehlungen für Benutzer mit Elementliste (für aktive Build)

Lassen Sie sich Benutzer, der einen Build vom Typ "Empfohlen" werden als aktiven Erstellen von Elementen mit einer zusätzlichen gekennzeichnet

Die API wird eine Liste der geschätzten Artikel in Übereinstimmung mit den Verlauf Verwendung des Benutzers und zusätzliche bereitgestellten Elemente zurück.

Hinweise: 
 1. Es gibt keine Benutzer Empfehlungen für FBT erstellen.
 2. Ist der aktiven Erstellen von FBT dieser Methode wird ein Fehler zurückgegeben.


| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%2C1000%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|

|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells |
| Benutzer-ID  | Eindeutiger Bezeichner des Benutzers |
| itemsIds | Durch Trennzeichen getrennte Liste der Elemente empfehlen. Die Maximallänge: 1024 |
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

Finden Sie ein Beispiel Antwort 12.1

###<a name="127-get-user-recommendations--of-a-specific-build"></a>12,7. Lassen Sie sich der Benutzer (für einen bestimmten Build)

Lassen Sie Benutzer sich von einem bestimmten Build vom Typ "Empfohlen".

Die API wird eine Liste der geschätzten Artikel in Übereinstimmung mit den Verlauf Verwendung des Benutzers (in einen bestimmten Build verwendet) zurück.

Hinweis: Es gibt keine Benutzer Empfehlungen für FBT erstellen.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27`|


|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells |
| Benutzer-ID  | Eindeutiger Bezeichner des Benutzers |
| numberOfResults | Anzahl der Ergebnisse erforderlich |
| includeMetatadata | Eine zukünftige Verwendung, immer false |
| buildId | die Generator-Id für diese Anforderung Empfehlungen verwenden |
| apiVersion | 1.0 |

**Antwort:**

HTTP-Status Code: 200


Die Antwort enthält einen Eintrag pro Element empfohlen. Jeder Eintrag weist die folgenden Daten:
- `Feed\entry\content\properties\Id`-Empfohlene Element-ID an.
- `Feed\entry\content\properties\Name`-Name des Elements.
- `Feed\entry\content\properties\Rating`-Bewertung von empfohlen; höherer Zahl bedeutet höhere KONFIDENZ.
- `Feed\entry\content\properties\Reasoning`-Empfehlungen Schlussfolgerungen (z. B. Empfehlungen erläuterungen).

Finden Sie ein Beispiel Antwort 12.1


###<a name="128-get-user-recommendations-with-item-list-of-a-specific-build"></a>12,8. Lassen Sie die Benutzer sich mit Elementliste (von einer bestimmten Build)

Abrufen von Benutzer Empfehlungen für einen bestimmten Build vom Typ "Empfohlen" und die Liste der anderen Elemente.

Die API wird eine Liste der geschätzten Artikel in Übereinstimmung mit den Verlauf Verwendung des Benutzers und die zusätzliche Liste von Elementen zurück.

Hinweis: Tthere ist keine Benutzer Empfehlungen für FBT erstellen.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27`|



|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells |
| Benutzer-ID  | Eindeutiger Bezeichner des Benutzers |
| Artikelnummern ein. | Durch Trennzeichen getrennte Liste der Elemente empfehlen. Die Maximallänge: 1024 |
| numberOfResults | Anzahl der Ergebnisse erforderlich |
| includeMetatadata | Eine zukünftige Verwendung, immer false |
| buildId | die Generator-Id für diese Anforderung Empfehlungen verwenden |
| apiVersion | 1.0 |

**Antwort:**

HTTP-Status Code: 200


Die Antwort enthält einen Eintrag pro Element empfohlen. Jeder Eintrag weist die folgenden Daten:
- `Feed\entry\content\properties\Id`-Empfohlene Element-ID an.
- `Feed\entry\content\properties\Name`-Name des Elements.
- `Feed\entry\content\properties\Rating`-Bewertung von empfohlen; höherer Zahl bedeutet höhere KONFIDENZ.
- `Feed\entry\content\properties\Reasoning`-Empfehlungen Schlussfolgerungen (z. B. Empfehlungen erläuterungen).

Finden Sie ein Beispiel Antwort 12.1

##<a name="13-user-usage-history"></a>13. Verlauf der Nutzung der Benutzer
Nachdem ein Empfehlungen Modell erstellt wurde erlaubt das System zum Abrufen des Verlaufs Benutzer (Elemente, die einem bestimmten Benutzer zugeordnet sind) für das Erstellen verwendet.
Diese API abzurufenden des Verlaufs Benutzer zulassen

Hinweis: der Benutzer Verlauf ist nur für Empfehlungen Builds derzeit verfügbar.

###<a name="131-retrieve-user-history"></a>13.1 Rufen Sie Benutzer Verlauf ab
Rufen Sie die Liste des Elements im aktiven Build oder in der angegebenen Build für die angegebene Benutzer-Id verwendet.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     | Erhalten Sie den Verlauf Benutzer für die aktive erstellen.<br/>`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&apiVersion=%271.0%27`<br/><br/>Abrufen des Verlaufs Benutzer für die angegebene build`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`<br/><br/>Beispiel:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277`|


|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Die eindeutige ID des Modells.|
| Benutzer-ID | Die eindeutige ID des Benutzers.
| buildId | Optionale Parameter, um anzugeben, in welchem Build des Verlaufs Benutzer Abruf werden sollen zulassen
| apiVersion | 1.0 |


**Antwort:**

HTTP-Status Code: 200

Die Antwort enthält einen Eintrag pro Element empfohlen. Jeder Eintrag weist die folgenden Daten:
- `Feed\entry\content\properties\Id`-Empfohlene Element-ID an.
- `Feed\entry\content\properties\Name`-Name des Elements.
- `Feed\entry\content\properties\Rating`-N/V.
- `Feed\entry\content\properties\Reasoning`-N/V.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get User History</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-05-26T15:32:47Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">CatalogItemsThatContainATokenEntity</title>
        <updated>2015-05-26T15:32:47Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Rating m:type="Edm.Double">0</d:Rating>
                <d:Reasoning m:type="Edm.String"/>
                <d:Metadata m:type="Edm.String"/>
                <d:FormattedRating m:type="Edm.Double" m:null="true"/>
            </m:properties>
        </content>
    </entry>
</feed>

##<a name="14-notifications"></a>14. Benachrichtigungen
Azure maschinellen Learning Empfehlungen erstellt Benachrichtigungen aus, wenn beständigen Fehler im System auftreten. Es gibt 3 Arten von Benachrichtigungen aus:
1.  Generator Fehler – diese Benachrichtigung wird für jede Buildfehler ausgelöst.
2.  Daten Acquisition Verarbeitung Fehler – diese Benachrichtigung wird ausgelöst, wenn mehr als 100 Fehler in den letzten 5 Minuten in die Verarbeitung von Verwendung Ereignissen pro Modell müssen.
3.  Fehler bei der Empfehlungen - Verbrauch – diese Benachrichtigung wird ausgelöst, wenn mehr als 100 Fehler in den letzten 5 Minuten in die Verarbeitung von Empfehlungen Anforderungen pro Modell müssen.


###<a name="141-get-notifications"></a>14.1. Erhalten von Benachrichtigungen
Ruft alle Benachrichtigungen für alle Modelle oder ein einzelnes Modell ab.

| HTTP-Methode | URI |
|:--------|:--------|
|Erhalten     |`<rootURI>/GetNotifications?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Erste alle Benachrichtigungen für alle Modelle:<br>`<rootURI>/GetNotifications?apiVersion=%271.0%27`<br><br>Beispiel für den Abruf von Benachrichtigungen für ein bestimmtes Modell:<br>`<rootURI>/GetNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%277`|


|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Optionaler Parameter. Wenn nicht angegeben, erhalten Sie alle Benachrichtigungen für alle Modelle. <br>Gültiger Wert: eindeutige ID des Modells.|
| apiVersion | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort:**

HTTP-Status Code: 200

OData-XML

    The response includes one entry per notification. Each entry has the following data:
        * feed\entry\content\properties\UserName - Internal user name identification.
        * feed\entry\content\properties\ModelId - Model ID.
        * feed\entry\content\properties\Message - Notification message.
        * feed\entry\content\properties\DateCreated - Date that this notification was created in UTC format.
        * feed\entry\content\properties\NotificationType - Notification types. Values are BuildFailure, RecommendationFailure, and DataAquisitionFailure.

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of Notifications for a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-04T13:24:23Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'" />
        <entry>
                <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfNotificationsForAUserEntity</title>
            <updated>2014-11-04T13:24:23Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:UserName m:type="Edm.String">515506bc-3693-4dce-a5e2-81cb3e8efb56@dm.com</d:UserName>
                    <d:ModelId m:type="Edm.String">967136e8-f868-4258-9331-10d567f87fae</d:ModelId>
                    <d:Message m:type="Edm.String">Build failed for user</d:Message>
                    <d:DateCreated m:type="Edm.String">2014-11-04T13:23:14.383</d:DateCreated>
                    <d:NotificationType m:type="Edm.String">BuildFailure</d:NotificationType>
                </m:properties>
            </content>
        </entry>
    </feed>

###<a name="142-delete-model-notifications"></a>14.2. Löschen von Benachrichtigungen Modell
Löscht alle gelesenen Benachrichtigungen für ein Modell.

| HTTP-Methode | URI |
|:--------|:--------|
|LÖSCHEN     |`<rootURI>/DeleteModelNotifications?modelId=%<model_id>%27&apiVersion=%271.0%27`<br><br>Beispiel:<br>`<rootURI>/DeleteModelNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%27`|


|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| modelId | Eindeutiger Bezeichner des Modells |
| apiVersion | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200

###<a name="143-delete-user-notifications"></a>14.3. Löschen von Benachrichtigungen für Benutzer
Löscht alle Benachrichtigungen für alle Modelle.

| HTTP-Methode | URI |
|:--------|:--------|
|LÖSCHEN     |`<rootURI>/DeleteUserNotifications?apiVersion=%271.0%27`|


|   Parameternamen  |   Gültige Werte                        |
|:--------          |:--------                              |
| apiVersion | 1.0 |
|||
| Anforderungstexts | KEINE |

**Antwort**:

HTTP-Status Code: 200




##<a name="15-legal"></a>15 rechtliche Hinweise
Dieses Dokument wird bereitgestellt "als-wird". In diesem Dokument, einschließlich URL und andere Verweise auf Websites im Internet angegebenen Informationen und Ansichten können ohne vorherige Ankündigung geändert werden.<br><br>
Einige Beispiele, die in diesen Beispielen werden nur Abbildung und fiktives sind. Keine Assoziierung oder Verbindung vorgesehen ist, oder sollte abgeleitet werden.<br><br>
Dieses Dokument stellt Sie keine Rechte an geistiges Eigentum in ein Microsoft-Produkt nicht bereit. Können Sie kopieren und verwenden Sie dieses Dokument zu internen Referenzzwecken.<br><br>
© Microsoft 2015. Alle Rechte vorbehalten.
 
