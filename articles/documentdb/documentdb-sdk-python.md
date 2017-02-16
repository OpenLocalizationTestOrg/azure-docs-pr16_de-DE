<properties 
    pageTitle="DocumentDB Python-API und SDK | Microsoft Azure" 
    description="Umfassende Informationen Sie zu den Python-API und SDK einschließlich Release-Daten, Endtermine und zwischen den einzelnen Versionen des DocumentDB Python SDK vorgenommenen Änderungen." 
    services="documentdb" 
    documentationCenter="python" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB-APIs und SDKs

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-python-api-and-sdk"></a>DocumentDB Python-API und SDK

<table>
<tr><td>**SDK herunterladen**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>
<tr><td>**API-Dokumentation**</td><td>[Dokumentation zur Python-API](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>
<tr><td>**Informationen zur Installation von SDK**</td><td>[Informationen zur Installation von Python SDK](http://azure.github.io/azure-documentdb-python/)</td></tr>
<tr><td>**Eigene Notizen hinzufügen können SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>
<tr><td>**Erste Schritte**</td><td>[Erste Schritte mit der Python SDK](documentdb-python-application.md)</td></tr>
<tr><td>**Aktuelle unterstützte Plattform**</td><td>[Python 2.7](https://www.python.org/downloads/) und [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Freigeben von Notizen

### <a name="a-name200200httpspypipythonorgpypipydocumentdb200"></a><a name="2.0.0"/>[2.0.0](https://pypi.python.org/pypi/pydocumentdb/2.0.0)
- Unterstützung für Python 3.5 hinzugefügt.
- Unterstützung für Verbindungspooling über ein Modul Besprechungsanfragen hinzugefügt.
- Unterstützung für Sitzung Konsistenz hinzugefügt.
- Unterstützung für oben/ORDERBY Abfragen für partitionierte Websitesammlungen hinzugefügt.


### <a name="a-name190190httpspypipythonorgpypipydocumentdb190"></a><a name="1.9.0"/>[1.9.0](https://pypi.python.org/pypi/pydocumentdb/1.9.0)
- Richtlinie-Unterstützung für reduzierten Anfragen hinzugefügten "Wiederholen". (Reduzierten Anfragen erhalten eine Anforderung Zins zu groß Ausnahme, mit dem Fehlercode 429.) Standardmäßig versucht DocumentDB neun Mal für jede Anforderung Fehlercode 429 vorgefunden wird, RetryAfter Zeitangabe in der Kopfzeile Antwort berücksichtigt. Nacheinander Intervall feste "Wiederholen" kann nun festgelegt werden als Teil der RetryOptions-Eigenschaft auf das Objekt ConnectionPolicy Wenn Sie die RetryAfter Zeit zurückgegebene Server zwischen der Wiederholungsversuche ignorieren möchten. DocumentDB jetzt wartet maximal 30 Sekunden für jede, anfordern beschränkt (ohne Rücksicht auf "Wiederholen" Count) und die Antwort mit dem Fehlercode 429 gibt. Diesmal kann auch Überschreiben in die Eigenschaft RetryOptions ConnectionPolicy Objekt sein.

- DocumentDB gibt jetzt X-ms-Steuerung-"Wiederholen"-Anzahl und x-ms-throttle-retry-wait-time-ms, wie die Antwort Überschriften in jeder Anforderung zum Kennzeichnen der Einschränkung wiederholen zählen und der Uhrzeit der Cummulative die Anforderung zwischen der Wiederholungsversuche gewartet hat.

- Entfernt die RetryPolicy Klasse und die entsprechende Eigenschaft (Retry_policy), die in der Klasse Document_client verfügbar gemacht und eingeführt stattdessen eine RetryOptions Klasse Verfügbarmachen der RetryOptions-Eigenschaft auf ConnectionPolicy Klasse, die verwendet werden kann, einige Standardoptionen "Wiederholen" außer Kraft gesetzt werden.

### <a name="a-name180180httpspypipythonorgpypipydocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://pypi.python.org/pypi/pydocumentdb/1.8.0)
  - Die Unterstützung für mehrere Region Datenbankkonten hinzugefügt.

### <a name="a-name170170httpspypipythonorgpypipydocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://pypi.python.org/pypi/pydocumentdb/1.7.0)
- Die Unterstützung für Zeit zu Live(TTL) Feature für Dokumente hinzugefügt.

### <a name="a-name161161httpspypipythonorgpypipydocumentdb161"></a><a name="1.6.1"/>[1.6.1](https://pypi.python.org/pypi/pydocumentdb/1.6.1)
- Updates im Zusammenhang mit Server Seite Einheit Sonderzeichen in Partitionkey Pfad zulässig sind.

### <a name="a-name160160httpspypipythonorgpypipydocumentdb160"></a><a name="1.6.0"/>[1.6.0](https://pypi.python.org/pypi/pydocumentdb/1.6.0)
- Implementiert [partitionierten Websitesammlungen](documentdb-partition-data.md) und [benutzerdefinierter Performance Ebenen](documentdb-performance-levels.md). 

### <a name="a-name150150httpspypipythonorgpypipydocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://pypi.python.org/pypi/pydocumentdb/1.5.0)
- Hinzufügen von Hash & Bereich partitionieren Server zu mehreren partitionsübergreifend mit Sharding Applikationen unterstützen.

### <a name="a-name142142httpspypipythonorgpypipydocumentdb142"></a><a name="1.4.2"/>[1.4.2](https://pypi.python.org/pypi/pydocumentdb/1.4.2)
- Implementieren Sie Upsert an. Neue UpsertXXX Methoden zur Unterstützung von Features Upsert hinzugefügt.
- Implementieren-ID-basierte Routing. Keine öffentlichen API Änderungen, die alle Änderungen internen.

### <a name="a-name120120httpspypipythonorgpypipydocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://pypi.python.org/pypi/pydocumentdb/1.2.0)
- Unterstützt Geodaten Index.
- Überprüft Id-Eigenschaft für alle Ressourcen. IDs für Ressourcen keine ?, enthalten / #, \, Zeichen oder Ende mit einem Leerzeichen.
- ResourceResponse hinzugefügt neu Kopfzeile "Index Transformation Fortschritt".

### <a name="a-name110110httpspypipythonorgpypipydocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://pypi.python.org/pypi/pydocumentdb/1.1.0)
- Implementiert Indizierung Richtlinie Version 2.

### <a name="a-name101101httpspypipythonorgpypipydocumentdb101"></a><a name="1.0.1"/>[1.0.1](https://pypi.python.org/pypi/pydocumentdb/1.0.1)
- Unterstützt die Proxy-Verbindung.

### <a name="a-name100100httpspypipythonorgpypipydocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://pypi.python.org/pypi/pydocumentdb/1.0.0)
- GA SDK.

## <a name="release--retirement-dates"></a>Release & Rente Datumsangaben
Microsoft bieten Benachrichtigung mindestens **12 Monate** vor ein SDK akzeptieren, um den Übergang auf eine neuere/unterstützte Version kontinuierliche zurückziehen.

Neue Features und Funktionalität und Optimierungen werden nur im aktuellen SDK hinzugefügt, die als solche wird empfohlen, dass Sie immer auf die neueste SDK-Version so früh wie möglich aktualisieren. 

Jede Aufforderung zum DocumentDB mit einer deaktivierte SDK werden vom Dienst abgelehnt.

> [AZURE.WARNING]
Alle Versionen von Azure DocumentDB SDK für Python vor Version **1.0.0** werden auf **29 Februar 2016**deaktiviert werden. 

<br/>

| Version | Datum der Freigabe | Rente Datum 
| ---     | ---          | ---
| [2.0.0](#2.0.0) | 29 September 2016 |---
| [1.9.0](#1.9.0) | 07 Juli 2016 |---
| [1.8.0](#1.8.0) | 14 Juni 2016 |---
| [1.7.0](#1.7.0) | 26 April 2016 |---
| [1.6.1](#1.6.1) | 08 April 2016 |---
| [1.6.0](#1.6.0) | 29 März 2016 |---
| [1.5.0](#1.5.0) | 03 Januar 2016 |---
| [1.4.2](#1.4.2) | 06 Oktober 2015 |---
| [1.4.1](#1.4.1) | 06 Oktober 2015 |---
| [1.2.0](#1.2.0) | 06 August 2015 |---
| [1.1.0](#1.1.0) | 09 Juli 2015 |---
| [1.0.1](#1.0.1) | 25 Mai 2015 |---
| [1.0.0](#1.0.0) | 07 April 2015 |---
| 0.9.4-prelease | 14 Januar 2015 | 29 Februar 2016
| 0.9.3-prelease | 09 Dezember 2014 | 29 Februar 2016
| 0.9.2-prelease | 25 November 2014 | 29 Februar 2016
| 0.9.1-prelease | 23 September 2014 | 29 Februar 2016
| 0.9.0-prelease | 21 August 2014 | 29 Februar 2016

## <a name="faq"></a>Häufig gestellte Fragen
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Siehe auch

Weitere Informationen zum DocumentDB finden Sie unter [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) Service-Seite. 
