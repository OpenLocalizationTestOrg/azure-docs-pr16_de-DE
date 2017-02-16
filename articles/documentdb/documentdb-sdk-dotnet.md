<properties 
    pageTitle="DocumentDB .NET API und SDK | Microsoft Azure" 
    description="Umfassende Informationen Sie zu den .NET API und SDK einschließlich Release-Daten, Endtermine und zwischen den einzelnen Versionen des DocumentDB .NET SDK vorgenommenen Änderungen." 
    services="documentdb" 
    documentationCenter=".net" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB-APIs und SDKs 

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-net-api-and-sdk"></a>DocumentDB .NET API und SDK

<table>
<tr><td>**SDK herunterladen**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>
<tr><td>**API-Dokumentation**</td><td>[Dokumentation zur .NET-API](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>
<tr><td>**Beispiele**</td><td>[.NET Codebeispielen](documentdb-dotnet-samples.md)</td></tr>
<tr><td>**Erste Schritte**</td><td>[Erste Schritte mit DocumentDB .NET SDK](documentdb-get-started.md)</td></tr>
<tr><td>**Web app-Lernprogramm**</td><td>[Web-Anwendungsentwicklung mit DocumentDB](documentdb-dotnet-application.md)</td></tr>
<tr><td>**Aktuelle unterstützten framework**</td><td>[Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Freigeben von Notizen

> [AZURE.IMPORTANT] Ab Version 1.9.2 Release erhalten System.NotSupportedException Sie bei der Abfrage partitionierter Websitesammlungen. Um diesen Fehler zu vermeiden, sicherstellen Sie, dass Ihre Hostprozess 64-Bit. Für Projekte, ausführbare kann dies erfolgen durch Deaktivieren der Option "Bevorzugen 32-Bit-Version" im Fenster für die Projekteigenschaften, klicken Sie auf der Registerkarte erstellen.

### <a name="a-name11001100httpswwwnugetorgpackagesmicrosoftazuredocumentdb1100"></a><a name="1.10.0"/>[1.10.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.10.0)

  - Hinzugefügten direkte Konnektivität Unterstützung für partitionierte Websitesammlungen.
  - Verbesserte Leistung für die begrenzt Veraltung Konsistenz ein.
  - Polygons und LineString DataTypes während der Websitesammlung, die Richtlinie für Geo-Zäune raumbezogener Abfragen Indizierung hinzugefügt.
  - Erweiterte LINQ-Unterstützung für StringEnumConverter, IsoDateTimeConverter und UnixDateTimeConverter während der Übersetzung Prädikate
  - Verschiedene SDK Updates.

### <a name="a-name195195httpswwwnugetorgpackagesmicrosoftazuredocumentdb195"></a><a name="1.9.5"/>[1.9.5 erreicht werden soll](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.5)

  - Feste ein Problem, das die folgenden NotFoundException: die gelesen Sitzung ist nicht verfügbar für die Eingabewerte Sitzungstoken. In einigen Fällen ist diese Ausnahme aufgetreten, bei der Abfrage für den gelesen-Bereich eines Kontos Geo verteilt.
  - Bereitgestellt, die ResponseStream-Eigenschaft in der Klasse ResourceResponse, die womit direkten Zugriff auf den zugrunde liegenden Stream aus einer Antwort kann.

### <a name="a-name194194httpswwwnugetorgpackagesmicrosoftazuredocumentdb194"></a><a name="1.9.4"/>[1.9.4](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.4)

  - Geändert hat die Klassen ResourceResponse, FeedResponse, StoredProcedureResponse und MediaResponse, um die entsprechenden öffentliche Schnittstelle implementieren, sodass für Bereitstellung (EDT auf alle) leistungsgesteuert Test Mocks erstellt werden können.
  - Feste ein Problem, das falsch formatierte Partition Key Kopfzeile verursacht, wenn ein benutzerdefiniertes JsonSerializerSettings Objekt für Serialisieren der Daten verwenden.

### <a name="a-name193193httpswwwnugetorgpackagesmicrosoftazuredocumentdb193"></a><a name="1.9.3"/>[1.9.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.3)

  - Feste ein Problem, das zeitintensive Abfragen treten Fehler verursacht: Autorisierungstoken ist in der aktuellen Uhrzeit ungültig.
  - Feste ein Problem, das die ursprüngliche SqlParameter aus cross Partition oben/Order by Abfragen entfernt.

### <a name="a-name192192httpswwwnugetorgpackagesmicrosoftazuredocumentdb192"></a><a name="1.9.2"/>[1.9.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.2)

  - Unterstützung für parallele Abfragen für partitionierte Websitesammlungen hinzugefügt.
  - Unterstützung für cross Partition oben und ORDER BY-Abfragen für partitionierte Websitesammlungen hinzugefügt.
  - Dadurch die fehlende Verweise auf DocumentDB.Spatial.Sql.dll und Microsoft.Azure.Documents.ServiceInterop.dll, die erforderlich sind, wenn Sie ein Projekt DocumentDB mit einem Verweis auf die DocumentDB Nuget-Paket verweisen.
  - Dadurch die Möglichkeit, Parameter unterschiedlichen Typs verwenden, wenn Sie benutzerdefinierte Funktionen in LINQ zu verwenden. 
  - Einen Fehler für global repliziert Konten behoben, wurden Upsert Anrufe geleitet Speicherorte statt schreiben Speicherorte lesen.
  - Hinzugefügte Methoden auf die Schnittstelle IDocumentClient, die fehlende wurden: 
      - UpsertAttachmentAsync-Methode, MediaStream und Optionen als Parameter akzeptiert
      - CreateAttachmentAsync-Methode, die Optionen als Parameter akzeptiert
      - CreateOfferQuery-Methode, die QuerySpec als Parameter annimmt.
  - Unversiegelten öffentlichen Klassen, die in der IDocumentClient Oberfläche angezeigt werden.

### <a name="a-name180180httpswwwnugetorgpackagesmicrosoftazuredocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.8.0)
  - Die Unterstützung für mehrere Region Datenbankkonten hinzugefügt.
  - Unterstützung für "Wiederholen" auf reduzierten Besprechungsanfragen hinzugefügt.  Benutzer kann die Anzahl der Wiederholungsversuche und die maximale Wartezeit anpassen, indem Sie die Eigenschaft ConnectionPolicy.RetryOptions konfigurieren.
  - Eine neue IDocumentClient Schnittstelle, die die Signaturen aller DocumenClient Eigenschaften und Methoden definiert wird hinzugefügt.  Als Bestandteil der diese Änderung geändert auch Erweiterungsmethoden, die IQueryable und IOrderedQueryable an Methoden für die DocumentClient-Klasse selbst zu erstellen.
  - Die Konfigurationsoption zum Festlegen der ServicePoint.ConnectionLimit für einen angegebenen DocumentDB Endpunkt Uri hinzugefügt.  Verwenden Sie ConnectionPolicy.MaxConnectionLimit, um den Standardwert zu ändern, der auf 50 festgelegt ist.
  - Veraltete IPartitionResolver und deren Implementierung.  Unterstützung für IPartitionResolver ist veraltet. Es wird empfohlen, unterteilt Websitesammlungen für höhere Speicher und Durchsatz zu verwenden.


### <a name="a-name171171httpswwwnugetorgpackagesmicrosoftazuredocumentdb171"></a><a name="1.7.1"/>[1.7.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.1)
  - Hinzugefügt, eine Überladung URI basierend auf ExecuteStoredProcedureAsync Methode, die RequestOptions als Parameter akzeptiert.
  
### <a name="a-name170170httpswwwnugetorgpackagesmicrosoftazuredocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.0)
  - Hinzugefügten Time-to live (TTL) Unterstützung für Dokumente.

### <a name="a-name163163httpswwwnugetorgpackagesmicrosoftazuredocumentdb163"></a><a name="1.6.3"/>[1.6.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.3)
  - Einen Fehler behoben in Nuget Verpackung des .NET SDK für das Verpacken es als Teil einer Lösung Azure-Cloud-Dienst.
  
### <a name="a-name162162httpswwwnugetorgpackagesmicrosoftazuredocumentdb162"></a><a name="1.6.2"/>[1.6.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.2)
  - Implementiert [partitionierten Websitesammlungen](documentdb-partition-data.md) und [benutzerdefinierter Performance Ebenen](documentdb-performance-levels.md). 

### <a name="a-name153153httpswwwnugetorgpackagesmicrosoftazuredocumentdb153"></a><a name="1.5.3"/>[1.5.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.3)
  - **[Feste]** Abfragen DocumentDB Endpunkt löst: ' System.Net.Http.HttpRequestException: Fehler beim Kopieren von Inhalt in einem Stream.

### <a name="a-name152152httpswwwnugetorgpackagesmicrosoftazuredocumentdb152"></a><a name="1.5.2"/>[1.5.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.2)
  - Erweiterte LINQ unterstützen, einschließlich der neue Operatoren für Paging-, bedingte Ausdrücke und Vergleich im Bereich.
    - Optimieren der Operator SELECT TOP-Verhalten in LINQ aktivieren
    - CompareTo-Operator zum Vergleichen von Zeichenfolgen Bereich aktivieren
    - Bedingte (?) und zusammengefügten Operatoren (?)
  - **[Feste]** ArgumentOutOfRangeException beim Kombinieren der Modell Projektion mit Where-In in Linq-Abfrage.  [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151httpswwwnugetorgpackagesmicrosoftazuredocumentdb151"></a><a name="1.5.1"/>[1.5.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.1)
 - **[Feste]** Wenn wählen Sie nicht die letzten Ausdruck ist der LINQ-Anbieter davon ausgegangen, dass keine Projektion und gefertigt wählen * falsch.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150httpswwwnugetorgpackagesmicrosoftazuredocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.0)
 - Implementiert Upsert, hinzugefügt UpsertXXXAsync Methoden
 - Verbesserte Leistung für alle Anfragen
 - LINQ-Anbieter-Unterstützung für bedingte, zusammengefügten und CompareTo Methoden für Zeichenfolgen
 - **[Feste]** LINQ-Anbieter--> Methode implementieren enthält, klicken Sie auf Liste, um den gleichen SQL-Code als IEnumerable und Array generieren
 - **[Feste]** BackoffRetryUtility verwendet die gleichen HttpRequestMessage erneut statt Erstellen einer neuen auf "Wiederholen"
 - **[Veraltet]** UriFactory.CreateCollection--> sollte jetzt UriFactory.CreateDocumentCollection verwenden
 
### <a name="a-name141141httpswwwnugetorgpackagesmicrosoftazuredocumentdb141"></a><a name="1.4.1"/>[1.4.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.1)
 - **[Feste]** Lokalisierungsprobleme, wenn Sie nicht En Kultur Informationen, wie z. B. nl-NL usw. verwenden. 
 
### <a name="a-name140140httpswwwnugetorgpackagesmicrosoftazuredocumentdb140"></a><a name="1.4.0"/>[1.4.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.0)
  - Routing auf Grundlage der ID
    - Neue UriFactory Helper zur Unterstützung bei erstellen ID-basierten Ressourcenlinks
    - Neue überladenen auf DocumentClient URI erstellen
  - IsValid() und IsValidDetailed() in LINQ für Geodaten hinzugefügt
  - Erweiterte LINQ-Anbieter-support
    - **Mathematik** - Abs, ARCCOS, ARCSIN, ARCTAN, Ceiling, Cos, Exp, Floor, Log, Log10, Pow, Round, melden Sie sich, Sin, Sqrt Tan, kürzen
    - **Zeichenfolge** - verketten, EndsWith IndexOf "," Anzahl "," ToLower "," TrimStart enthält, ersetzen, umdrehen möchten, TrimEnd, StartsWith, Teilzeichenfolge ToUpper
    - **Matrix** - verketten, enthält, zählen
    - **IN** -operator

### <a name="a-name130130httpswwwnugetorgpackagesmicrosoftazuredocumentdb130"></a><a name="1.3.0"/>[1.3.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.3.0)
  - Hinzugefügte Unterstützung für Indizierung Richtlinien ändern
    - Neue ReplaceDocumentCollectionAsync-Methode in DocumentClient
    - Neue Eigenschaft IndexTransformationProgress ResourceResponse<T> für prozentuale Fortschritts von Index Richtlinie Änderungen
    - DocumentCollection.IndexingPolicy ist jetzt veränderbare
  - Hinzugefügte Unterstützung für räumliche Indizierung und Abfrage
    - Neuer Microsoft.Azure.Documents.Spatial Namespace für räumliche Typen serialisieren/Deserialisieren wie Punkt und Polygon
    - Neue SpatialIndex Klasse für GeoJSON Daten gehörende Kehrmatrix DocumentDB Indizierung
  - **[Fest]** : falsche SQL-Abfrage aus Linq Ausdruck [#38](https://github.com/Azure/azure-documentdb-net/issues/38) generiert

### <a name="a-name120120httpswwwnugetorgpackagesmicrosoftazuredocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.2.0)
- Abhängigkeit von Newtonsoft.Json v5.0.7 
- Änderungen an die Order By unterstützen
  - LINQ-Anbieter-Unterstützung für OrderBy() oder OrderByDescending()
  - IndexingPolicy zur Unterstützung von Order By 
  
        **NB: Possible breaking change** 
  
        If you have existing code that provisions collections with a custom indexing policy, then your existing code will need to be updated to support the new IndexingPolicy class. If you have no custom indexing policy, then this change does not affect you.

### <a name="a-name110110httpswwwnugetorgpackagesmicrosoftazuredocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.1.0)
- Unterstützung für Partitionierung von Daten mithilfe von die neuen Klassen HashPartitionResolver und RangePartitionResolver sowie die IPartitionResolver
- Datenvertragsserialisierung
- GUID Unterstützung in LINQ-Anbieter
- Unterstützung für UDFs in LINQ

### <a name="a-name100100httpswwwnugetorgpackagesmicrosoftazuredocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.0.0)
- GA SDK

> [AZURE.NOTE]
Es wurde eine Änderung NuGet-Paket namens zwischen Vorschau und benennen. Wir vom **Microsoft.Azure.Documents.Client** in **Microsoft.Azure.DocumentDB** verschoben wird
<br/>


### <a name="a-name09x-preview09x-previewhttpswwwnugetorgpackagesmicrosoftazuredocumentsclient"></a><a name="0.9.x-preview"/>[0.9.x-Preview](https://www.nuget.org/packages/Microsoft.Azure.Documents.Client)
- Vorschau SDKs [veraltet]

## <a name="release--retirement-dates"></a>Release & Endtermine
Microsoft bieten Benachrichtigung mindestens **12 Monate** vor ein SDK akzeptieren, um den Übergang auf eine neuere/unterstützte Version kontinuierliche zurückziehen.

Neue Features und Funktionalität und Optimierungen werden nur im aktuellen SDK hinzugefügt, die als solche wird empfohlen, dass Sie immer auf die neueste SDK-Version so früh wie möglich aktualisieren. 

Jede Aufforderung zum DocumentDB mit einer deaktivierte SDK werden vom Dienst abgelehnt.

> [AZURE.WARNING]
Alle Versionen des DocumentDB Azure SDK für .NET vor Version **1.0.0** werden auf **29 Februar 2016**deaktiviert werden. 
 
<br/>
 
| Version | Datum der Freigabe | Rente Datum 
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 27 September 2016 |---
| [1.9.5 erreicht werden soll](#1.9.5) | 01 September 2016 |---
| [1.9.4](#1.9.4) | 24 August 2016 |---
| [1.9.3](#1.9.3) | 15 August 2016 |---
| [1.9.2](#1.9.2) | 23 Juli 2016 |---
| 1.9.1 | Nicht mehr unterstützt |---
| 1.9.0 | Nicht mehr unterstützt |---
| [1.8.0](#1.8.0) | 14 Juni 2016 |---
| [1.7.1](#1.7.1) | 06 Mai 2016 |---
| [1.7.0](#1.7.0) | 26 April 2016 |---
| [1.6.3](#1.6.3) | 08 April 2016 |---
| [1.6.2](#1.6.2) | 29 März 2016 |---
| [1.5.3](#1.5.3) | 19 Februar 2016 |---
| [1.5.2](#1.5.2) | 14 Dezember 2015 |---
| [1.5.1](#1.5.1) | 23 November 2015 |---
| [1.5.0](#1.5.0) | 05 Oktober 2015 |---
| [1.4.1](#1.4.1) | 25 August 2015 |---
| [1.4.0](#1.4.0) | 13 August 2015 |---
| [1.3.0](#1.3.0) | 05 August 2015 |---
| [1.2.0](#1.2.0) | 06 Juli 2015 |---
| [1.1.0](#1.1.0) | 30 April 2015 |---
| [1.0.0](#1.0.0) | 08 April 2015 |---
| [0.9.3-prelease](#0.9.x-preview) | 12 März 2015 | 29 Februar 2016
| [0.9.2-prelease](#0.9.x-preview) | Januar 2015 | 29 Februar 2016
| [.9.1-prelease](#0.9.x-preview) | 13 Oktober 2014 | 29 Februar 2016
| [0.9.0-prelease](#0.9.x-preview) | 21 August 2014 | 29 Februar 2016

## <a name="faq"></a>Häufig gestellte Fragen
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Siehe auch

Weitere Informationen zum DocumentDB finden Sie unter [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) Service-Seite. 
