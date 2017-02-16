
<properties
    pageTitle="DocumentDB Java-API und SDK | Microsoft Azure"
    description="Umfassende Informationen Sie zu den Java-API und SDK einschließlich Release-Daten, Endtermine und zwischen den einzelnen Versionen des DocumentDB Java SDK vorgenommenen Änderungen."
    services="documentdb"
    documentationCenter="java"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB-APIs und SDKs

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-java-api-and-sdk"></a>DocumentDB Java-API und SDK

<table>
<tr><td>**SDK herunterladen**</td><td>[Maven](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>
<tr><td>**API-Dokumentation**</td><td>[Java-API-Dokumentation](http://azure.github.io/azure-documentdb-java/)</td></tr>
<tr><td>**Eigene Notizen hinzufügen können SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>
<tr><td>**Erste Schritte**</td><td>[Erste Schritte mit der Java SDK](documentdb-java-application.md)</td></tr>
<tr><td>**Aktuelle unterstützte Laufzeit**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Freigeben von Notizen

### <a name="a-name191191httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb191"></a><a name="1.9.1"/>[1.9.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.1)

  - Unterstützung für BoundedStaleness Konsistenz Ebene hinzugefügt.
  - Unterstützung für direkte Konnektivität für Vorgänge für partitionierte Websitesammlungen hinzugefügt.
  - Feste Einheiten ein Fehlers in einer Datenbank mit SQL Abfragen.
  - Feste Einheiten ein Fehlers im Sitzungscache Stelle, an der Sitzungstoken falsch festgelegt werden kann.

### <a name="a-name190190httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb190"></a><a name="1.9.0"/>[1.9.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.0)

  - Unterstützung für cross Partition parallele Abfragen hinzugefügt.
  - Unterstützung für oben/ORDER BY-Abfragen für partitionierte Websitesammlungen hinzugefügt.
  - Unterstützung für signifikante Konsistenz hinzugefügt.
  - Unterstützung für basierenden Namen Anfragen bei Verwendung von direkte Konnektivität hinzugefügt.
  - Feste Einheiten über alle Anforderung Wiederholungsversuche konsistent bleiben ActivityId vornehmen.
  - Feste Einheiten ein Fehlers im Zusammenhang mit dem Sitzungscache eine Auflistung mit demselben Namen beim erneuten erstellen.
  - Polygons und LineString DataTypes während der Websitesammlung, die Richtlinie für Geo-Zäune raumbezogener Abfragen Indizierung hinzugefügt.
  - Feste Probleme mit Java-Dokument für Java 1.8.

### <a name="a-name181181httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb181"></a><a name="1.8.1"/>[1.8.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.1)
  - Einen Fehler behoben in PartitionKeyDefinitionMap Zwischenspeichern Einzelpartition Websitesammlungen und nicht zusätzliche Abruf Key Anfragen partitionieren.
  - Feste Einheiten einen Fehler nicht zu wiederholen, wenn eine falsche Key Partitionswert bereitgestellt wird.

### <a name="a-name180180httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb180"></a><a name="1.8.0"/>[1.8.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.0)
  - Die Unterstützung für mehrere Region Datenbankkonten hinzugefügt.
  - Unterstützung für automatische Wiederholung auf reduzierten Anfragen mit Optionen zum Anpassen des max Wiederholungsversuche und max "Wiederholen" Wartezeit hinzugefügt.  Finden Sie unter RetryOptions und ConnectionPolicy.getRetryOptions().
  - Veraltete IPartitionResolver Grundlage benutzerdefinierten vorherigen Code. Verwenden Sie partitionierte Websitesammlungen für mehr Speicherplatz und Durchsatz.

### <a name="a-name171171httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb171"></a><a name="1.7.1"/>[1.7.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.1)
- Hinzugefügten "Wiederholen" Richtlinie Unterstützung für begrenzungsebene.  

### <a name="a-name170170httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb170"></a><a name="1.7.0"/>[1.7.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.0)
- Hinzugefügten Time-to live (TTL) Unterstützung für Dokumente.

### <a name="a-name160160httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb160"></a><a name="1.6.0"/>[1.6.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.6.0)
- Implementiert [partitionierten Websitesammlungen](documentdb-partition-data.md) und [benutzerdefinierter Performance Ebenen](documentdb-performance-levels.md).

### <a name="a-name151151httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb151"></a><a name="1.5.1"/>[1.5.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.1)
- Feste Einheiten einen Fehler in HashPartitionResolver generieren Hashwerte in littleEndian mit anderen SDKs konsistent sein.

### <a name="a-name150150httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb150"></a><a name="1.5.0"/>[1.5.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.0)
- Hinzufügen von Hash & Bereich partitionieren Server zu mehreren partitionsübergreifend mit Sharding Applikationen unterstützen.

### <a name="a-name140140httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb140"></a><a name="1.4.0"/>[1.4.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.4.0)
- Implementieren Sie Upsert an. Neue UpsertXXX Methoden zur Unterstützung von Features Upsert hinzugefügt.
- Implementieren-ID-basierte Routing. Keine öffentlichen API Änderungen, die alle Änderungen internen.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
- Release übersprungen, um die Versionsnummer Ausrichtung mit anderen SDKs anzuzeigen

### <a name="a-name120120httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb120"></a><a name="1.2.0"/>[1.2.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.2.0)
- Unterstützt Geodaten Index
- Überprüft Id-Eigenschaft für alle Ressourcen. IDs für Ressourcen keine ?, enthalten / #, \, Zeichen oder Ende mit einem Leerzeichen.
- ResourceResponse hinzugefügt neu Kopfzeile "Index Transformation Fortschritt".

### <a name="a-name110110httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb110"></a><a name="1.1.0"/>[1.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.1.0)
- Implementiert Indizierung Richtlinie Version 2

### <a name="a-name100100httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb100"></a><a name="1.0.0"/>[1.0.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.0.0)
- GA SDK

## <a name="release--retirement-dates"></a>Release & Endtermine
Microsoft bieten Benachrichtigung mindestens **12 Monate** vor ein SDK akzeptieren, um den Übergang auf eine neuere/unterstützte Version kontinuierliche zurückziehen.

Neue Features und Funktionalität und Optimierungen werden nur im aktuellen SDK hinzugefügt, die als solche wird empfohlen, dass Sie immer auf die neueste SDK-Version so früh wie möglich aktualisieren.

Jede Aufforderung zum DocumentDB mit einer deaktivierte SDK werden vom Dienst abgelehnt.

> [AZURE.WARNING]
Alle Java SDK DocumentDB Azure-Versionen vor Version **1.0.0** werden auf **29 Februar 2016**deaktiviert werden.

<br/>

| Version | Datum der Freigabe | Rente Datum
| ---     | ---          | ---
| [1.9.1](#1.9.1) | 28 Oktober 2016 |---
| [1.9.0](#1.9.0) | 03 Oktober 2016 |---
| [1.8.1](#1.8.1) | 30 Juni 2016 |---
| [1.8.0](#1.8.0) | 14 Juni 2016 |---
| [1.7.1](#1.7.1) | 30 April 2016 |---
| [1.7.0](#1.7.0) | 27 April 2016 |---
| [1.6.0](#1.6.0) | 29 März 2016 |---
| [1.5.1](#1.5.1) | 31 Dezember 2015 |---
| [1.5.0](#1.5.0) | 04 Dezember 2015 |---
| [1.4.0](#1.4.0) | 05 Oktober 2015 |---
| [1.3.0](#1.3.0) | 05 Oktober 2015 |---
| [1.2.0](#1.2.0) | 05 August 2015 |---
| [1.1.0](#1.1.0) | 09 Juli 2015 |---
| [1.0.1](#1.0.1) | 12 Mai 2015 |---
| [1.0.0](#1.0.0) | 07 April 2015 |---
| 0.9.5-prelease | 09 Mrz 2015 | 29 Februar 2016
| 0.9.4-prelease | 17 Februar 2015 | 29 Februar 2016
| 0.9.3-prelease | 13 Januar 2015 | 29 Februar 2016
| 0.9.2-prelease | 19 Dezember 2014 | 29 Februar 2016
| 0.9.1-prelease | 19 Dezember 2014 | 29 Februar 2016
| 0.9.0-prelease | 10 Dezember 2014 | 29 Februar 2016

## <a name="faq"></a>Häufig gestellte Fragen
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Siehe auch

Weitere Informationen zum DocumentDB finden Sie unter [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) Service-Seite.
