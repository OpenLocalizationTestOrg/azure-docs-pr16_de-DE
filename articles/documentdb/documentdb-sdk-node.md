<properties
    pageTitle="DocumentDB Node.js-API und SDK | Microsoft Azure"
    description="Umfassende Informationen Sie zu den Node.js-API und SDK einschließlich Release-Daten, Endtermine und zwischen den einzelnen Versionen des DocumentDB Node.js SDK vorgenommenen Änderungen."
    services="documentdb"
    documentationCenter="nodejs"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB-APIs und SDKs

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

##<a name="documentdb-nodejs-api-and-sdk"></a>DocumentDB Node.js-API und SDK

<table>
<tr><td>**SDK herunterladen**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>
<tr><td>**API-Dokumentation**</td><td>[Dokumentation zur Node.js API](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>
<tr><td>**Informationen zur Installation von SDK**</td><td>[Informationen zur Installation](http://azure.github.io/azure-documentdb-node/)</td></tr>
<tr><td>**Eigene Notizen hinzufügen können SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>
<tr><td>**Beispiele**</td><td>[Node.js Codebeispielen](documentdb-nodejs-samples.md)</td></tr>
<tr><td>**Erste Schritte-Lernprogramm**</td><td>[Erste Schritte mit dem Node.js SDK](documentdb-nodejs-get-started.md)</td></tr>
<tr><td>**Web app-Lernprogramm**</td><td>[Erstellen einer Node.js Webanwendung mit DocumentDB](documentdb-nodejs-application.md)</td></tr>
<tr><td>**Aktuelle unterstützte Plattform**</td><td>[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/)<br/>[Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/>[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)</td></tr>
</table></br>

##<a name="release-notes"></a>Freigeben von Notizen

###<a name="a-name11001100a"></a><a name="1.10.0"/>1.10.0</a>

- Unterstützung für cross Partition parallele Abfragen hinzugefügt.
- Unterstützung für oben/ORDER BY-Abfragen für partitionierte Websitesammlungen hinzugefügt.

###<a name="a-name190190a"></a><a name="1.9.0"/>1.9.0</a>

- Richtlinie-Unterstützung für reduzierten Anfragen hinzugefügten "Wiederholen". (Reduzierten Anfragen erhalten eine Anforderung Zins zu groß Ausnahme, mit dem Fehlercode 429.) Standardmäßig versucht DocumentDB neun Mal für jede Anforderung Fehlercode 429 vorgefunden wird, RetryAfter Zeitangabe in der Kopfzeile Antwort berücksichtigt. Nacheinander Intervall feste "Wiederholen" kann nun festgelegt werden als Teil der RetryOptions-Eigenschaft auf das Objekt ConnectionPolicy Wenn Sie die RetryAfter Zeit zurückgegebene Server zwischen der Wiederholungsversuche ignorieren möchten. DocumentDB jetzt wartet maximal 30 Sekunden für jede, anfordern beschränkt (ohne Rücksicht auf "Wiederholen" Count) und die Antwort mit dem Fehlercode 429 gibt. Diesmal kann auch Überschreiben in die Eigenschaft RetryOptions ConnectionPolicy Objekt sein.

- DocumentDB gibt jetzt X-ms-Steuerung-"Wiederholen"-Anzahl und x-ms-throttle-retry-wait-time-ms, wie die Antwort Überschriften in jeder Anforderung zum Kennzeichnen der Einschränkung wiederholen zählen und der Uhrzeit der Cummulative die Anforderung zwischen der Wiederholungsversuche gewartet hat.

- Die RetryOptions-Klasse wurde hinzugefügt, Verfügbarmachen der RetryOptions-Eigenschaft für die ConnectionPolicy-Klasse, die verwendet werden kann, einige Standardoptionen "Wiederholen" außer Kraft gesetzt.

###<a name="a-name180180a"></a><a name="1.8.0"/>1.8.0</a>

 - Die Unterstützung für mehrere Region Datenbankkonten hinzugefügt.

###<a name="a-name170170a"></a><a name="1.7.0"/>1.7.0</a>

- Die Unterstützung für Zeit zu Live(TTL) Feature für Dokumente hinzugefügt.

###<a name="a-name160160a"></a><a name="1.6.0"/>1.6.0</a>

- Implementiert [partitionierten Websitesammlungen](documentdb-partition-data.md) und [benutzerdefinierter Performance Ebenen](documentdb-performance-levels.md).

###<a name="a-name156156a"></a><a name="1.5.6"/>1.5.6</a>

- Feste RangePartitionResolver.resolveForRead Fehler, keine Links aufgrund einer schlechten verketten Ergebnisliste zurückgegeben wurde.

###<a name="a-name155155a"></a><a name="1.5.5"/>1.5.5</a>

- Feste HashParitionResolver resolveForRead(): Wenn keine Partitionsschlüssel angegeben wurde Ausnahme ausgelöst wird, anstelle eine Liste aller registrierten Links zurückgeben.

###<a name="a-name154154a"></a><a name="1.5.4"/>1.5.4</a>

- Updates Emission [#100](https://github.com/Azure/azure-documentdb-node/issues/100) - HTTPS-Agent dedizierter: Ändern des globalen Agents für DocumentDB Zwecke vermeiden. Verwenden Sie einen dedizierten Agent für alle von der Bibliothek der Serviceanfragen.

###<a name="a-name153153a"></a><a name="1.5.3"/>1.5.3</a>

- Updates ' Problemdetails ' [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - ordnungsgemäß Ziehpunkt Striche Medien IDs.

###<a name="a-name152152a"></a><a name="1.5.2"/>1.5.2</a>

- Updates Emission [#95](https://github.com/Azure/azure-documentdb-node/issues/95) - EventEmitter Zuhörer die Warnung verloren gehen.

###<a name="a-name151151a"></a><a name="1.5.1"/>1.5.1</a>

- Updates Emission [#92](https://github.com/Azure/azure-documentdb-node/issues/90) - Ordner umbenennen Hash zu hashende für Betriebssysteme Groß-/Kleinschreibung beachtet.

### <a name="a-name150150a"></a><a name="1.5.0"/>1.5.0</a>

- Implementieren Sharding Unterstützung durch Hash & Bereich Partition Server hinzufügen.

### <a name="a-name140140a"></a><a name="1.4.0"/>1.4.0</a>

- Implementieren Sie Upsert an. Neue UpsertXXX Methoden auf DocumentClient.

### <a name="a-name130130a"></a><a name="1.3.0"/>1.3.0</a>

- Übersprungen, um die Ausrichtung mit anderen SDKs Versionsnummern anzuzeigen.

### <a name="a-name122122a"></a><a name="1.2.2"/>1.2.2</a>

- Teilen f legt Wrapper an neue Repository fest.
- Paketdatei für Npm Registrierung aktualisieren.

### <a name="a-name121121a"></a><a name="1.2.1"/>1.2.1</a>

- Implementiert ID basierend Routing.
- Behebt Problem [#49](https://github.com/Azure/azure-documentdb-node/issues/49) - aktuelle Eigenschaftenkonflikte mit Methode current().

### <a name="a-name120120a"></a><a name="1.2.0"/>1.2.0</a>

- Unterstützung für Geodaten Index hinzugefügt.
- Überprüft Id-Eigenschaft für alle Ressourcen. Keine IDs für Ressourcen enthalten?, / #, & #47; & #47; Zeichen oder Ende mit einem Leerzeichen.
- ResourceResponse hinzugefügt neu Kopfzeile "Index Transformation Fortschritt".

### <a name="a-name110110a"></a><a name="1.1.0"/>1.1.0</a>

- Implementiert Indizierung Richtlinie Version 2.

### <a name="a-name103103a"></a><a name="1.0.3"/>1.0.3</a>

- Problem [#40] (https://github.com/Azure/azure-documentdb-node/issues/40) – implementiert Eslint und mühselige Konfigurationen Core und Versprechen SDK.

### <a name="a-name102102a"></a><a name="1.0.2"/>1.0.2</a>

- Problem [Nr. 45](https://github.com/Azure/azure-documentdb-node/issues/45) - Versprechen Wrapper enthält keine Kopfzeile mit zurück.

### <a name="a-name101101a"></a><a name="1.0.1"/>1.0.1</a>

- Implementiert Möglichkeit, die Abfrage für Konflikte durch Hinzufügen von ReadConflicts, ReadConflictAsync und QueryConflicts.
- Aktualisierte API-Dokumentation.
- Emission [#41](https://github.com/Azure/azure-documentdb-node/issues/41) - client.createDocumentAsync zurück.

### <a name="a-name100100a"></a><a name="1.0.0"/>1.0.0</a>

- GA SDK.

## <a name="release--retirement-dates"></a>Release & Endtermine
Microsoft bieten Benachrichtigung mindestens **12 Monate** vor ein SDK akzeptieren, um den Übergang auf eine neuere/unterstützte Version kontinuierliche zurückziehen.

Neue Features und Funktionalität und Optimierungen werden nur im aktuellen SDK hinzugefügt, die als solche wird empfohlen, dass Sie immer auf die neueste SDK-Version so früh wie möglich aktualisieren.

Jede Aufforderung zum DocumentDB mit einer deaktivierte SDK werden vom Dienst abgelehnt.

> [AZURE.WARNING]
Alle Versionen von Azure DocumentDB SDK für Node.js vor Version **1.0.0** werden auf **29 Februar 2016**deaktiviert werden.

<br/>

| Version | Datum der Freigabe | Rente Datum
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 03 Oktober 2016 |---
| [1.9.0](#1.9.0) | 07 Juli 2016 |---
| [1.8.0](#1.8.0) | 14 Juni 2016 |---
| [1.7.0](#1.7.0) | 26 April 2016 |---
| [1.6.0](#1.6.0) | 29 März 2016 |---
| [1.5.6](#1.5.6) | 08 März 2016 |---
| [1.5.5](#1.5.5) | 02 Februar 2016 |---
| [1.5.4](#1.5.4) | 01 Februar 2016 |---
| [1.5.2](#1.5.2) | 26 Januar 2016 |---
| [1.5.2](#1.5.2) | 22 Januar 2016 |---
| [1.5.1](#1.5.1) | 4 Januar 2016 |---
| [1.5.0](#1.5.0) | 31 Dezember 2015 |---
| [1.4.0](#1.4.0) | 06 Oktober 2015 |---
| [1.3.0](#1.3.0) | 06 Oktober 2015 |---
| [1.2.2](#1.2.2) | 10 September 2015 |---
| [1.2.1](#1.2.1) | 15 August 2015 |---
| [1.2.0](#1.2.0) | 05 August 2015 |---
| [1.1.0](#1.1.0) | 09 Juli 2015 |---
| [1.0.3](#1.0.3) | 04 Juni 2015 |---
| [1.0.2](#1.0.2) | 23 Mai 2015 |---
| [1.0.1](#1.0.1) | 15 Mai 2015 |---
| [1.0.0](#1.0.0) | 08 April 2015 |---
| 0.9.4-Prerelease | 06 April 2015 | 29 Februar 2016
| 0.9.3-Prerelease | 14 Januar 2015 | 29 Februar 2016
| 0.9.2-Prerelease | 18 Dezember 2014 | 29 Februar 2016
| 0.9.1-Prerelease | 22 August 2014 | 29 Februar 2016
| 0.9.0-Prerelease | 21 August 2014 | 29 Februar 2016


## <a name="faq"></a>Häufig gestellte Fragen
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Siehe auch

Weitere Informationen zum DocumentDB finden Sie unter [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) Service-Seite.
