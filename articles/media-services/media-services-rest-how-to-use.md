<properties 
    pageTitle="Media Services REST-API Übersicht | Microsoft Azure" 
    description="Media Services REST-API (Übersicht)" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako"/>


# <a name="media-services-rest-api-overview"></a>Media Services REST-API (Übersicht) 

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Microsoft Azure Media Services ist ein Dienst, der OData-basiertem HTTP-Anfragen akzeptiert und wieder in ausführlichen JSON oder Atom + Pub reagieren können. Da Media-Dienste Azure Entwurfsrichtlinien entspricht, gibt es eine Reihe von erforderlichen HTTP-Header, die jeder Kunde muss bei einer Verbindung mit Media-Dienste sowie eine Reihe von optionalen Header, die verwendet werden können. Den folgenden Abschnitten werden die Überschriften und HTTP-Verben wann können erstellen Besprechungsanfragen und Antworten von Media-Dienste empfangen.

##<a name="considerations"></a>Aspekte 

Unter folgenden Aspekten angewendet werden, wenn der REST verwenden.

- Bei der Abfrage Elemente besteht maximal 1000 Elemente gleichzeitig zurückgegeben werden, da Öffentliche REST Version 2 Abfrageergebnisse auf 1000 Ergebnisse begrenzt. Sie müssen **Überspringen** und **Optimieren** (.NET) verwenden / **oben** (REST) als in [diesem Beispiel .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) und [in diesem Beispiel REST-API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities)beschrieben. 

- Bei Verwendung von JSON und Angabe, dass das Schlüsselwort **__metadata** in der Besprechungsanfrage verwenden (z. B. Bezüge auf ein verknüpftes Objekt) müssen Sie die Kopfzeile **akzeptieren** auf [JSON ausführlichen Format](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) festlegen (siehe folgendes Beispiel). OData versteht nicht die **__metadata** -Eigenschaft in der Besprechungsanfrage, es sei denn, Sie zu ausführlichen festlegen.  

        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
        
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 
        

## <a name="standard-http-request-headers-supported-by-media-services"></a>Standardmäßiger HTTP-Anforderungsheader von Medienservices unterstützt werden

Für jede Anruf, die Sie in der Media-Dienste vornehmen, es gibt eine Reihe von erforderlichen Überschriften, die Sie in der Besprechungsanfrage enthalten müssen, und auch eine Reihe von optionalen Header möglicherweise enthalten sein sollen. Die folgende Tabelle enthält die erforderlichen Header:


Kopfzeile|Typ|Wert
---|---|---
Autorisierung|Person|Person wird nur zulässigen Autorisierung. Der Wert muss auch das Access Token ACS umfassen.
X-ms-version|Dezimalzahl|2.11
DataServiceVersion|Dezimalzahl|3.0
MaxDataServiceVersion|Dezimalzahl|3.0



>[AZURE.NOTE] Da Media-Dienste OData verwendet, um die zugrunde liegenden Objekt Metadatenrepository über REST-APIs verfügbar zu machen, sollte die Kopfzeilen DataServiceVersion und MaxDataServiceVersion in eine beliebige Anforderung enthalten sein; Wenn dies nicht der Fall ist, nimmt jedoch klicken Sie dann aktuell Media-Dienste, dass der Wert DataServiceVersion verwenden 3.0 ist.

Im folgenden finden eine Reihe von optionalen Header:

Kopfzeile|Typ|Wert
---|---|---
Datum|RFC 1123 Datum|Timestamp der Anforderung
Annehmen|Inhaltstyp|Die angeforderten Inhaltstyp für die Antwort wie die folgende:<p> -Anwendung/Json; Odata = ausführliche<p> -Anwendung/Atom + Xml<p> Antworten möglicherweise einen anderen Inhaltstyp, wie etwa einen Abruf Blob, wird eine erfolgreiche Antwort Blob Streams als Nutzlast enthalten.
Annehmen-Codierung|GZIP, verkleinern|GZIP und DEFLATE Codierung, falls zutreffend. Hinweis: Für große Ressourcen, Media-Dienste möglicherweise ignorieren diese Kopfzeile und nicht komprimierte Daten zurück.
Annehmen Sprache|"En", "en" enden und So weiter.|Gibt die bevorzugte Sprache für die Antwort an.
Annehmen-Zeichensatz|Zeichensatz Typ wie "UTF-8"|Standardmäßig ist die UTF-8.
X-HTTP-Methode|HTTP-Methode|Ermöglicht Clients oder Firewalls, die HTTP-Methoden, wie sich oder löschen diese Methoden, Tunnel über eine GET-Anruf nicht unterstützen.
Inhaltstyp|Inhaltstyp|Inhaltstyp des Anforderungstexts in sich oder Beitrag anfordert.
Client-Anforderung-id|Zeichenfolge|Ein Anrufer defined Wert, der die angegebene Anforderung identifiziert. Wenn angegeben, wird dieser Wert in der Antwortnachricht als eine Möglichkeit zum Zuordnen der Anforderung enthalten. <p><p>**Wichtige**<p>Werte am 2096b gekürzt werden soll (2k).

## <a name="standard-http-response-headers-supported-by-media-services"></a>Standardmäßigen HTTP-Header von Medienservices unterstützt werden

Im folgenden finden eine Reihe von Überschriften, die Sie je nach der Ressource, die Sie anfordern wurden und die Aktion, die Sie ausführen wollten zurückgegeben werden kann.


Kopfzeile|Typ|Wert
---|---|---
Anfrage-id|Zeichenfolge|Ein eindeutiger Bezeichner für den aktuellen Vorgang, Dienst generiert.
Client-Anforderung-id|Zeichenfolge|Ein Bezeichner angegeben Anrufer in der ursprünglichen Anforderung, falls vorhanden.
Datum|RFC 1123 Datum|Das Datum, das die Anforderung verarbeitet wurde.
Inhaltstyp|Variiert|Der Inhaltstyp der Antwort Stelle.
Inhalt-Codierung|Variiert|Gzip oder verkleinern, je nach Bedarf.


## <a name="standard-http-verbs-supported-by-media-services"></a>Standardmäßigen HTTP-Verben von Medienservices unterstützt werden

Im folgenden finden eine vollständige Liste der HTTP-Verben, die beim Bereitstellen von HTTP anfordert verwendet werden können:


Verb|Beschreibung
---|---
Erhalten|Gibt den aktuellen Wert eines Objekts.
Bereitstellen|Erstellt ein Objekt auf der Grundlage der Daten zur Verfügung gestellt, oder ein Befehls übermittelt.
SETZEN|Ersetzt ein Objekt oder ein benanntes Objekt (falls zutreffend) erstellt.
LÖSCHEN|Löscht ein Objekt.
ZUSAMMENFÜHREN|Ein vorhandenes Objekt aktualisiert mit benannten geänderten.
KOPF|Gibt die Metadaten eines Objekts für eine GET-Antwort an.

##<a name="limitation"></a>Einschränkung

Bei der Abfrage Elemente besteht maximal 1000 Elemente gleichzeitig zurückgegeben werden, da Öffentliche REST Version 2 Abfrageergebnisse auf 1000 Ergebnisse begrenzt. Sie müssen **Überspringen** und **Optimieren** (.NET) verwenden / **oben** (REST) als in [diesem Beispiel .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) und [in diesem Beispiel REST-API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities)beschrieben. 


## <a name="discovering-media-services-model"></a>Entdecken Media Services-Modell

Um Media-Dienste Personen leichter auffindbar zu machen, kann der Vorgang $metadata verwendet werden. Sie können Sie alle Typen gültige Entität, Elementeigenschaften, Zuordnungen, Funktionen, Aktionen und usw. abrufen. Im folgenden Beispiel wird veranschaulicht, wie den URI zu erstellen: https://media.windows.net/API/$ Metadaten.

Sie sollten anhängen "? version=2.x-api" bis zum Ende des URIS, wenn Sie die Metadaten in einem Browser anzeigen möchten, oder die Kopfzeile X-ms-Version nicht in Ihrer Anfrage schließen.



##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





 
