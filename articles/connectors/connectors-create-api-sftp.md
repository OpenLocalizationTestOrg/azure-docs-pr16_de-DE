<properties
pageTitle="Informationen zum Verwenden des SFTP Verbinders in Ihrer apps Logik | Microsoft Azure"
description="Erstellen Sie Logik apps mit Azure-App-Dienst an. Verbinden Sie mit SFTP-API zum Senden und Empfangen von Dateien. Sie können verschiedene Vorgänge ausführen, die beispielsweise erstellen, aktualisieren, Abrufen oder Löschen von Dateien."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/20/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-sftp-connector"></a>Erste Schritte mit der SFTP Verbinder

Verwenden Sie den Verbinder SFTP zum Zugreifen auf ein SFTP-Konto zum Senden und Empfangen von Dateien an. Sie können verschiedene Vorgänge ausführen, die beispielsweise erstellen, aktualisieren, Abrufen oder Löschen von Dateien.  

Um [alle Verbinder](./apis-list.md)verwenden zu können, müssen Sie zuerst eine app Logik zu erstellen. Sie können durch [Erstellen einer Logik app jetzt](../app-service-logic/app-service-logic-create-a-logic-app.md)loslegen.

## <a name="connect-to-sftp"></a>Herstellen einer Verbindung SFTP mit

Bevor Sie Ihre app Logik Dienste zugreifen kann, müssen Sie zuerst eine *Verbindung* mit dem Dienst erstellen. Eine [Verbindung](./connectors-overview.md) stellt eine Verbindung zwischen einer app Logik und einem anderen Dienst.  

### <a name="create-a-connection-to-sftp"></a>Herstellen einer Verbindung mit SFTP

>[AZURE.INCLUDE [Steps to create a connection to SFTP](../../includes/connectors-create-api-sftp.md)]

## <a name="use-an-sftp-trigger"></a>Verwenden eines Triggers SFTP

Ein Trigger ist ein Ereignis, das zum Starten des Workflows in einer app Logik definiert verwendet werden kann. [Erfahren Sie mehr über Trigger](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

In diesem Beispiel stellen ich wird gezeigt, wie Sie mithilfe den Trigger **SFTP – Wenn eine Datei hinzugefügt oder geändert wird** um einen Logik app Workflow zu starten, wenn eine Datei hinzugefügt wird, oder auf einem Server SFTP geändert. Im Beispiel wird auch enthält Informationen zum Hinzufügen einer bedingungs, die den Inhalt der Datei neue oder geänderte überprüft und Entscheidung, um die Datei zu extrahieren, wenn deren Inhalt darauf hinzuweisen, dass es extrahiert werden sollen, bevor Sie den Inhalt verwenden. Schließlich erfahren Sie, wie Sie eine Aktion zum Extrahieren Sie den Inhalt einer Datei, und platzieren Sie den extrahierten Inhalt in einem Ordner auf dem Server SFTP hinzufügen. 

Dieser Trigger können Sie in ein Enterprise-Beispiel um einen Ordner SFTP für neue Dateien zu überwachen, die Aufträge von Kunden darstellen.  Sie können eine Aktion SFTP Verbinder, z. B. **-Dateiinhalt abrufen** klicken Sie dann den Inhalt der Reihenfolge für die weitere Verarbeitung und Speicher in Ihrer Datenbank Bestellungen abrufen verwenden.

>[AZURE.INCLUDE [Steps to create an SFTP trigger](../../includes/connectors-create-api-sftp-trigger.md)]

## <a name="add-a-condition"></a>Hinzufügen einer bedingungs

>[AZURE.INCLUDE [Steps to add a condition](../../includes/connectors-create-api-sftp-condition.md)]

## <a name="use-an-sftp-action"></a>Verwenden Sie eine Aktion SFTP

Eine Aktion ist ein Vorgang durchgeführten durch den Workflow in einer app Logik definiert. [Erfahren Sie mehr über Aktionen](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.INCLUDE [Steps to create an SFTP action](../../includes/connectors-create-api-sftp-action.md)]


## <a name="technical-details"></a>Technische Details

Hier sind die Details der Trigger, Aktionen und Antworten, die diese Verbindung unterstützt:

## <a name="sftp-triggers"></a>SFTP Trigger

SFTP besteht aus die folgenden Triggern:  

|Auslösen | Beschreibung|
|--- | ---|
|[Wenn eine Datei hinzugefügt oder geändert wird](connectors-create-api-sftp.md#when-a-file-is-added-or-modified)|Dieser Vorgang löst einen Fluss, wenn eine Datei hinzugefügt oder in einem Ordner geändert wird.|


## <a name="sftp-actions"></a>SFTP Aktionen

SFTP weist die folgenden Aktionen aus:


|Aktion|Beschreibung|
|--- | ---|
|[Abrufen von Dateimetadaten](connectors-create-api-sftp.md#get-file-metadata)|Dieser Vorgang ruft Dateimetadaten, die mit dem Datei-Id an.|
|[Update-Datei](connectors-create-api-sftp.md#update-file)|Dieser Vorgang wird den Inhalt der Datei aktualisiert.|
|[Datei löschen](connectors-create-api-sftp.md#delete-file)|Dieser Vorgang löscht eine Datei.|
|[Abrufen von Metadaten mit Pfad der Datei](connectors-create-api-sftp.md#get-file-metadata-using-path)|Dieser Vorgang ruft Dateimetadaten mithilfe des Dateipfades ab.|
|[Abrufen von Dateiinhalt unter Verwendung der Pfad](connectors-create-api-sftp.md#get-file-content-using-path)|Dieser Vorgang ruft Dateiinhalt mithilfe des Dateipfades ab.|
|[Abrufen der Inhalt der Datei](connectors-create-api-sftp.md#get-file-content)|Dieser Vorgang ruft Dateiinhalt mithilfe den Datei-Id an.|
|[Datei erstellen](connectors-create-api-sftp.md#create-file)|Dieser Vorgang hochgeladen eine Datei auf einer SFTP-Server.|
|[Kopieren einer Datei](connectors-create-api-sftp.md#copy-file)|Dieser Vorgang kopiert eine Datei auf einem Server SFTP.|
|[Der Listendateien im Ordner](connectors-create-api-sftp.md#list-files-in-folder)|Mit diesem Vorgang wird in einem Ordner enthaltenen Dateien an.|
|[Der Listendateien im Stammordner](connectors-create-api-sftp.md#list-files-in-root-folder)|Dieser Vorgang ruft die Dateien im Stammordner ab.|
|[Ordner zu extrahieren](connectors-create-api-sftp.md#extract-folder)|Dieser Vorgang extrahiert eine Archivdatei in einem anderen Ordner (Beispiel: ZIP).|
### <a name="action-details"></a>Aktionsdetails

Hier sind die Details für die Aktionen und Trigger für diesen Connector, zusammen mit ihren Antworten:



### <a name="get-file-metadata"></a>Abrufen von Dateimetadaten
Dieser Vorgang ruft Dateimetadaten, die mit dem Datei-Id an. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|ID *|Datei|Geben Sie die Datei|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|




### <a name="update-file"></a>Update-Datei
Dieser Vorgang wird den Inhalt der Datei aktualisiert. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|ID *|Datei|Geben Sie die Datei|
|Textkörper *|Der Inhalt der Datei|Inhalt der Datei zu aktualisieren|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|




### <a name="delete-file"></a>Datei löschen
Dieser Vorgang löscht eine Datei. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|ID *|Datei|Geben Sie die Datei|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist




### <a name="get-file-metadata-using-path"></a>Abrufen von Metadaten mit Pfad der Datei
Dieser Vorgang ruft Dateimetadaten mithilfe des Dateipfades ab. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Pfad *|Dateipfad|Eindeutige Pfad der Datei|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|




### <a name="get-file-content-using-path"></a>Abrufen von Dateiinhalt unter Verwendung der Pfad
Dieser Vorgang ruft Dateiinhalt mithilfe des Dateipfades ab. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Pfad *|Dateipfad|Eindeutige Pfad der Datei|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist




### <a name="get-file-content"></a>Abrufen der Inhalt der Datei
Dieser Vorgang ruft Dateiinhalt mithilfe den Datei-Id an. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|ID *|Datei|Geben Sie die Datei|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist




### <a name="create-file"></a>Datei erstellen
Dieser Vorgang hochgeladen eine Datei auf einer SFTP-Server. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Ordnerpfad *|Ordnerpfad|Eindeutige Pfad des Ordners|
|Namen *|Dateiname|Name der Datei|
|Textkörper *|Der Inhalt der Datei|Inhalt der Datei zu erstellen|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


|| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|




### <a name="copy-file"></a>Kopieren einer Datei
Dieser Vorgang kopiert eine Datei auf einem Server SFTP. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Quelle *|Quelldateipfad|Pfad zu der Quelldatei|
|Ziel *|Zieldateipfad|Pfad zu der Zieldatei, einschließlich des Dateinamens|
|Überschreiben|Überschreiben?|Überschreibt die Zieldatei aus, wenn auf "True" gesetzt|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|




### <a name="when-a-file-is-added-or-modified"></a>Wenn eine Datei hinzugefügt oder geändert wird
Dieser Vorgang löst einen Fluss, wenn eine Datei hinzugefügt oder in einem Ordner geändert wird. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Ordner ID *|Ordner|Geben Sie einen Ordner|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist




### <a name="list-files-in-folder"></a>Der Listendateien im Ordner
Mit diesem Vorgang wird in einem Ordner enthaltenen Dateien an. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|ID *|Ordner|Geben Sie den Ordner|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist



#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|




### <a name="list-files-in-root-folder"></a>Der Listendateien im Stammordner
Dieser Vorgang ruft die Dateien im Stammordner ab. 


Es sind keine Parameter für diesen Anruf

#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|




### <a name="extract-folder"></a>Ordner zu extrahieren
Dieser Vorgang extrahiert eine Archivdatei in einem anderen Ordner (Beispiel: ZIP). 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Quelle *|Der Pfad der Archivdatei Quelle|Pfad der Archivdatei|
|Ziel *|Zielordnerpfad|Pfad zum Zielordner|
|Überschreiben|Überschreiben?|Überschreibt die Zieldateien, wenn auf "True" gesetzt|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist



#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|



## <a name="http-responses"></a>HTTP-Antworten

Eine oder mehrere der folgenden HTTP Statuscodes können die Aktionen und Trigger oben zurückgegeben werden: 

|Namen|Beschreibung|
|---|---|
|200|Okay|
|202|Akzeptiert|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Es ist ein Fehler aufgetreten.|
|Standard|Fehler bei Vorgang.|







## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)