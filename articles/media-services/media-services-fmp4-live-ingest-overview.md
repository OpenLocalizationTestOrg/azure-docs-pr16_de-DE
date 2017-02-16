<properties 
    pageTitle="Azure fragmentierte MP4 live Media-Dienste Aufnahme Spezifikation | Microsoft Azure" 
    description="Diese Spezifikation beschreibt das Protokoll und Format für fragmentiert MP4-basierten live streaming Aufnahme für Microsoft Azure Media-Dienste. Microsoft Azure Media Services bietet live streaming Service, wodurch Kunden streamen live Ereignisse und Übertragen von Inhalten in Echtzeit mit Microsoft Azure als Cloud-Plattform. Dieses Dokument beschreibt auch bewährte Methoden beim Erstellen hochgradig redundante und robuste live Aufnahme Verfahren." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"     
    ms.author="cenkdin;juliako"/>

#<a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Azure fragmentierte MP4 live Media-Dienste Aufnahme Spezifikation

Diese Spezifikation beschreibt das Protokoll und Format für fragmentiert MP4-basierten live streaming Aufnahme für Microsoft Azure Media-Dienste. Microsoft Azure Media Services bietet live streaming Service, wodurch Kunden streamen live Ereignisse und Übertragen von Inhalten in Echtzeit mit Microsoft Azure als Cloud-Plattform. Dieses Dokument beschreibt auch bewährte Methoden beim Erstellen hochgradig redundante und robuste live Aufnahme Verfahren.


##<a name="1-conformance-notation"></a>1. Notation Konformität

Die Wörter Key "muss", sind "Darf nicht", "Erforderlich", "Gilt", "Soll nicht", "SHOULD", "Sollten nicht", "Empfohlen", "Möglicherweise" und "OPTIONAL" in diesem Dokument interpretiert werden soll, wie in RFC 2119 beschrieben.

##<a name="2-service-diagram"></a>2. Diagramm-Dienst 

Im nachstehenden Diagramm zeigt die Architektur des streaming live-Diensts in Microsoft Azure Media-Dienste:

1.  Live Encoder legt live Feeds Kanäle erstellt und nach der Bereitstellung über das Microsoft Azure Media Services SDK.
2.  Kanäle, Programme und Streaming Endpunkt in Microsoft Azure Media Services Ziehpunkt alle live streaming Funktionen Erfassung, Formatierung, einschließlich cloud DVR, Sicherheit, Skalierbarkeit und Redundanz.
3.  Optional können Kunden bereitstellen ein Layers CDN zwischen den Endpunkt Streaming und die Clientendpunkte auswählen.
4.  Client Endpunkte Stream vom Endpunkt Streaming über HTTP Adaptives Streaming Protokolle (z. B. interpolierten Streaming, Gedankenstrich, Festplatten oder HLS).

![Image1][image1]


##<a name="3-bit-stream-format--iso-14496-12-fragmented-mp4"></a>3.-Bit-Stream Format – ISO 14496-12 fragmentiert MP4

Das Format über das Netzwerk für live streaming Aufnahme, die in diesem Dokument erläutert wird [ISO-14496-12] basiert auf. Finden Sie ausführliche Erläuterung von fragmentiert MP4-Format und Erweiterungen für beide Dateien Demand [[MS-SSTR]](http://msdn.microsoft.com/library/ff469518.aspx) und live streaming Aufnahme.

###<a name="live-ingest-format-definitions"></a>Live Aufnahme Format Definitionen 

Es folgt eine Liste der spezielles Format, die Aufnahme Definitionen, die angewendet wird, um live in Microsoft Azure Media-Dienste:

1. Die 'Ftyp', LiveServerManifestBox und 'Moov' Feld müssen bei jeder Anforderung (HTTP-POST) gesendet werden.  Er muss am Anfang des Streams gesendet werden und einem beliebigen Zeitpunkt der Encoder wieder herstellen muss, um fortsetzen Stream Aufnahme.  Weitere Informationen hierzu finden Sie unter Abschnitt 6 in [1].
2. [1] Abschnitt 3.3.2 definiert eine optionale Feld mit dem Namen StreamManifestBox für live Aufnahme. Auf der Weiterleitung Logik des Microsoft Azure den Lastenausgleich Verwendung von diesem Feld wird nicht mehr unterstützt und sollte nicht vorhanden ist, wenn in Microsoft Azure Media Service Aufnahme. Wenn dieses Feld vorhanden ist, wird ignoriert Azure Media Services im Hintergrund.
3. Die TrackFragmentExtendedHeaderBox 3.2.3.2 in [1] definiert sein muss für jede Fragment vorhanden sein.
4. Version 2 der TrackFragmentExtendedHeaderBox sollte verwendet werden, um Medien Segmente mit identischen URLs in mehreren Rechenzentren generieren. Das Fragment Index-Feld ist erforderlich für Cross-Datacenter Failover Streaming Index-basierten Formate wie Apple HTTP Live Streaming (HLS) und Index-basierten MPEG-Strich.  Um Cross-Datacenter Failover zu aktivieren, muss der Index Fragment Encoder und erhöhen hinweg von 1 für jedes Fragment aufeinanderfolgende Medien auch über Encoder neu gestartet wurde oder Fehler synchronisiert werden.
5. [1] Abschnitt 3.3.6 definiert Feld mit der Bezeichnung MovieFragmentRandomAccessBox ('Mfra'), die am Ende des live-Aufnahme gesendet werden kann, um EOS (Ende des Streams) an den Kanal anzugeben. Auf der Logik Erfassung von Azure Media Services Verwendung der EOS (Ende des Streams) wird nicht mehr unterstützt, und das Feld 'Mfra' für live-Aufnahme sollte nicht gesendet werden. Wenn gesendet hat, wird ignoriert Azure Media Services im Hintergrund. Es wird empfohlen, [Channel zurücksetzen](https://msdn.microsoft.com/library/azure/dn783458.aspx#reset_channels) verwenden, um den Status des Punkts Erfassung zurücksetzen und auch empfohlen wird das [Programm beenden](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) verwenden, um eine Präsentation und Stream zu beenden.
6. Die Dauer der MP4-Fragment sollten Konstante, um Verringern der Größe der Manifeste Client und verbessern Client Download Heuristik durch die Verwendung von Tags wiederholen.  Die Dauer möglicherweise schwanken, damit keine ganze Zahl Rahmen Sätzen zu berücksichtigen.
7. Die Dauer der MP4-Fragment sollten zwischen ungefähr 2 und 6 Sekunden.
8. MP4-Fragment-Zeitstempel und Indizes (TrackFragmentExtendedHeaderBox Fragment_ Absolute_ Zeit und Fragment_index) sollte kommen in aufsteigender Reihenfolge an.  Obwohl Azure Media Services flexibel in Bezug auf doppelte Satzteile ist, gibt es sehr eingeschränkte Möglichkeit zum Satzteile entsprechend der Medienzeitachse neu anordnen.

##<a name="4-protocol-format--http"></a>4-Protokoll Format – HTTP

ISO fragmentiert MP4 live Grundlage Aufnahme für Microsoft Azure Media Services verwendet ein Standard zeitintensive HTTP POST Datenübertragung codierten Medien verpackt fragmentiert MP4-Format an den Dienst anfordern. Jede HTTP POST sendet eine vollständige fragmentiert MP4-Bit-Stream ("Stream") beginnend von beginnend mit Kopfzeile Felder (im Feld 'Ftyp', "Moov" und "Live Server Manifest Feld") und eine Abfolge von Fragmenten ('Moof' und 'Mdat' Listenfelder) fortsetzen. Lizenzinformationen finden Sie im Abschnitt 9.2 In [1], für die URL-Syntax für HTTP POST-Anforderung. Ein Beispiel für den POST-URL lautet: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

###<a name="requirements"></a>Anforderungen

Hier sind die Anforderungen an:

1. Encoder sollte die Übertragung per HTTP POST-Anforderung mit einer leeren "Text" (Länge Null Inhalt) mit der gleichen Aufnahme URL gestartet. Dies kann helfen, schnell zu erkennen, wenn Sie der Endpunkt live Erfassung gültig ist und es ist keine Authentifizierung oder anderen Bedingungen, die erforderlich. Pro HTTP-Protokoll kann der Server nicht mehr zurück HTTP-Antwort zu senden, bis die gesamte Anforderung, einschließlich Beitrag Textkörper eingegangen ist. Ausgehend von der zeitintensive Art der live Ereignis, ohne diesen Schritt, der Encoder möglicherweise nicht zu jedem Fehler beim erkennen, bis alle Daten senden abgeschlossen ist.
2. Encoder muss Fehler oder Authentifizierung Herausforderung als Ergebnis (1) behandelt werden. Wenn (1) erfolgreich ist 200 Reaktionen fortzusetzen.
3. Encoder muss einen neuen Beitrag HTTP-Besprechungsanfrage mit fragmentierten MP4 Streams beginnen.  Die Nutzlast muss mit Kopfzeile Felder gefolgt von Satzteile beginnen.  Beachten Sie, dass das Feld 'Ftyp', "Moov" und "Live Server Manifest Feld" (in dieser Reihenfolge) bei jeder Anforderung gesendet werden muss, selbst wenn der Encoder verbunden werden muss, da die vorherige Anforderung vor Ende des Streams beendet wurde. 
4. Für das Hochladen, da es nicht möglich, die Länge des gesamte Inhalte des Ereignisses live Vorhersagen ist, muss Encoder verwenden aufgeteilter Codierung übertragen.
5. Wenn das Ereignis über, nach dem Senden des letzten Fragments, muss der Encoder ordnungsgemäß aufgeteilter Codierung übertragen Nachricht Beenden der Sequenz (die meisten HTTP-Client-Stapel verarbeitet es automatisch). Encoder muss warten, für den Dienst, den Code endgültige Antwort zurück, und klicken Sie dann die Verbindung zu beenden. 
6. Verwenden Encoder müssen nicht das Nomen Events() auf, wie in 9,2 In [1] für live-Aufnahme in Microsoft Azure Media-Dienste beschrieben.
7. Wenn die HTTP POST-Anforderung beendet oder vor dem Ende des Streams mit einem Fehler TCP Timeout erreicht, muss der Encoder Emission einen neuen Beitrag Besprechungsanfrage über eine neue Verbindung und führen Sie die Anforderungen über für die zusätzliche Anforderung, dass der Encoder erneutes die beiden vorherigen MP4-Fragmenten für jeden Titel im Stream senden und fortsetzen ohne Einführung in Diskontinuitäten Stelle der Medienzeitachse muss.  Erneutes Senden die letzten beiden MP4-Fragmenten für jeden Titel wird sichergestellt, dass keine Daten verloren.  Wenn ein Stream eine Audio- und Nachverfolgen enthält und die aktuelle Beitrag Anforderung fehlschlägt, muss der Encoder Kurzum, schließen Sie wieder und erneutes Senden den letzten beiden Fragmenten für die audio-Spur, die zuvor erfolgreich gesendet wurden, und die letzten zwei Fragmente für das video nachverfolgen, die zuvor erfolgreich gesendet wurden akzeptieren, um sicherzustellen, dass es keine Daten verloren ist.  Einen "Weiter" Puffer von Medien Fragmenten, muss sendet sie erneut, wenn Sie eine neue Verbindung herstellen der Encoder aufrechterhalten.

##<a name="5-timescale"></a>5. Zeitskala 

[[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx) beschreibt die Verwendung von "Zeitskala" für SmoothStreamingMedia (Abschnitt 2.2.2.1), StreamElement (Abschnitt 2.2.2.3), StreamFragmentElement(2.2.2.6) und LiveSMIL (Abschnitt 2.2.7.3.1). Wenn der Wert der Zeitskala nicht vorhanden ist, ist der Standardwert verwendet 10.000.000 (10 MHz). Obwohl interpolierten Streaming Formatangabe nicht blockiert Verwendung anderer Zeitskala Werte, die am häufigsten Verwendungszwecke Implementierungen Encoder dieser Standardwert (10 MHz) zum Generieren Aufnahme interpolierten Streaming Daten. Aufgrund von Feature zum [Azure Medien dynamische Verpacken](media-services-dynamic-packaging-overview.md) ist es wird empfohlen, 90 kHz Zeitskala für Videodatenströme und 44,1 oder 48.1 kHz für audio-Streams verwendet. Wenn andere Zeitskala Werte für verschiedene Streams verwendet werden, muss die Ebene Zeitskala Stream gesendet werden. Näheres [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

##<a name="6-definition-of-stream"></a>6. "Stream" definition  

"Stream" ist die grundlegende Einheit für Vorgänge in live-Aufnahme live Präsentation verfassen, streaming Failover und Redundanz Szenarien behandeln. "Stream" wird als eine eindeutige fragmentiert MP4-Bit-Stream die eine einzelne nachverfolgen enthalten möglicherweise oder mehrerer Titel definiert. Eine vollständige live Präsentation konnte eine oder mehrere Streams je nach Konfiguration der live Encoder(s) enthalten. Die folgenden Beispiele veranschaulichen verschiedene Optionen Stream(s) verwenden, um eine vollständige live Präsentation verfassen.

**Beispiel:** 

Kunde möchte eine live-streaming Präsentation erstellen, die die folgenden Audio und Video eine Bitrate enthält:

Video – 3000/s, 1500/s, 750/s

Audio – 128/s

###<a name="option-1-all-tracks-in-one-stream"></a>Option 1: Alle Spuren in einem stream

Bei dieser Option ein einzelner Encoder alle Spuren von Audio und Video generiert und bündeln sie in einem fragmentiert MP4-Bit-Stream die über eine einzelne HTTP POST Verbindung gesendet wird. In diesem Beispiel ist nur eine Stream für diese Präsentation live:

![Bild2][image2]

###<a name="option-2-each-track-in-a-separate-stream"></a>Option 2: Jede Spur in einem separaten stream

Bei dieser Option die Encoder(s) setzen nur eine in jeder Fragment MP4-Bit-Stream nachverfolgen und Posten alle Streams über mehrere separate HTTP-Verbindungen. Dies konnte mit Encoder eine oder mehrere Encoder vorgenommen werden. Aus Sicht des live Aufnahme besteht diese Präsentation live vier Streams.

![image3][image3]

###<a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>Option 3: Bündeln audio nachverfolgen mit den niedrigsten Bitrate nachverfolgen video in einem stream

Bei dieser Option wählt der Kunden die audio-Spur mit den niedrigsten Bitrate video Nachverfolgen in ein Fragment MP4-Bit-Stream bündeln und lassen Sie die anderen zwei video Titel jedes seinen eigenen Stream wird. 


![image4][image4]


###<a name="summary"></a>Zusammenfassung

Was oben angezeigt wird, ist keine vollständige Liste aller möglichen Aufnahme Optionen für dieses Beispiel. Alle Gruppierung von Spuren in Streams wird als tatsächlich, durch die live-Aufnahme unterstützt. Kunden und Lieferanten Encoder können ihre eigenen Implementierungen basierend auf engineering Komplexität, Encoder-Kapazität und Redundanz und Failover Aspekte auswählen. Jedoch darauf hinzuweisen, dass in den meisten Fällen es nur eine Audio-Spur für die gesamte live-Präsentation ist, damit es wichtig ist, um die Bekömmlichkeit des Streams Erfassung sicherzustellen, die die audio-Spur enthält. Diese Aspekte häufig führt in einem eigenen Stream (wie in der Option 2) audio nachverfolgen Inbetriebnahme oder bündeln es mit den niedrigsten Bitrate video nachverfolgen (wie in der Option 3). Auch für eine bessere Redundanz und Fehlertoleranz Aufnahme senden eine Audiodatei Nachverfolgen in zwei verschiedene Streams (Option 2 mit redundanten audio Spuren) oder bündeln, die mindestens zwei audio Nachverfolgen der niedrigsten Bitrate video Spuren (Option 3 mit Audio in mindestens zwei Videodatenströme gebündelten) wird dringend für live empfohlen, in Microsoft Azure Media-Dienste.

##<a name="7-service-failover"></a>7. Failover Dienst 

Bereitstellung des live streaming ist gute Failoversupport entscheidend, für die Bereitstellung des Diensts. Microsoft Azure Media Services soll verschiedene Arten von Fehlern, einschließlich Netzwerkfehler, Serverfehler, Speicherprobleme zu behandeln. Bei Verwendung in Verbindung mit den richtigen Failover Logik von der Seite live Encoder kann Kunden einen hochgradig zuverlässigen live streaming-Dienst aus der Cloud erzielen.

In diesem Abschnitt werden Service Failover-Szenarien behandelt. In diesem Fall Fehler irgendwo innerhalb des Diensts geschieht und -Manifeste selbst als ein Fehler. Hier sind einige Vorschläge zur Durchführung Encoder für den Umgang mit Service-Failover aus:


1. Verwenden Sie ein 10 zweiten Timeout für die Herstellung der Verbindungs TCP aus.  Wenn beim Versuch zum Herstellen der Verbindung mehr als 10 Sekunden dauert, den Vorgang abzubrechen, und versuchen Sie es erneut. 
2. Verwenden Sie einen kurzen Timeout zum Senden der HTTP-Anforderung Blöcken Nachricht ein.  Wenn die Dauer der Ziel MP4-Fragment N Sekunden ist, verwenden Sie einen senden Timeout zwischen N und 2N Sekunden; Verwenden Sie beispielsweise einen Timeout von 6 bis 12 Sekunden auf, wenn die Dauer der MP4-Fragment 6 Sekunden ist.  Wenn ein Timeout auftritt, setzen Sie die Verbindung, öffnen Sie eine neue Verbindung und fortsetzen Stream Aufnahme auf die neue Verbindung. 
3. Verwalten eines parallelen Puffers, enthält die letzten zwei Fragmente für jeden Titel, die erfolgreich und vollständig den Dienst gesendet wurden.  Wenn die Anforderung HTTP POST eines Streams wird beendet oder vor dem Timeout am Ende des Streams, öffnen Sie eine neue Verbindung ein anderes HTTP POST-Anforderung beginnen, Erneutes Senden die Überschriften Stream, Erneutes Senden die letzten zwei Fragmente für jeden Titel und Fortsetzen des Streams ohne Einführung in eine Diskontinuität Stelle der Medienzeitachse.  Dadurch wird das Risiko von Datenverlusten zu verringern.
4. Es wird empfohlen, dass der Encoder keine der die Anzahl der Wiederholungsversuche zum Herstellen einer Verbindung oder streaming Beschränkung nach einer TCP Fehlers fortsetzen.
5. Nach einem Fehler TCP:
    1. Die aktuelle Verbindung geschlossen werden muss, und für einen neuen Beitrag HTTP-Besprechungsanfrage muss eine neue Verbindung erstellt werden.
    2. Die neue HTTP-Beitrag URL muss sein die URL der ursprünglichen Beitrag identisch.
    3. Die neue HTTP-Beitrag muss gehören Stream, Kopfzeilen (im Feld 'Ftyp', "Moov" und "Live Server Manifest Feld") identisch mit der Überschriften Stream in dem ursprünglichen Beitrag.
    4. Die letzten zwei Fragmente gesendet für jeden Titel erneut gesendet werden müssen, und streaming ohne Einführung in eine Diskontinuität Stelle der Medienzeitachse fortgesetzt wird.  Die MP4-Fragment Zeitstempel müssen kontinuierlich, auch über HTTP POST-Anfragen erhöhen.
6. Der Encoder sollte die HTTP POST-Anforderung beendet, wenn Daten nicht mit einer Rate entsprechen die Dauer der MP4-Fragment gesendet wird.  HTTP POST-Anforderung, die keine Daten sendet kann verhindern, dass Azure Media-Dienste schnell Trennen vom Encoder im Falle ein Update-Dienst.  Aus diesem Grund für die HTTP-POST gering (Ad Signal) Spuren sollten kurze halten, beenden, sobald das gering gefüllte Fragment gesendet wird.

##<a name="8-encoder-failover"></a>8. Failover encoder

Encoder Failover ist der zweite Failover Szenario, die für die End-to-End-live streaming Übermittlung berücksichtigt werden muss. In diesem Szenario passiert ist die Bedingung Fehler auf der Seite Encoder ein. 

![image5][image5]


Nachstehend sind erwartet aus den Endpunkt live Aufnahme Encoder Failover geschieht:

1. Eine neue Encoderinstanz sollte erstellt werden, um weiterhin streaming, wie in dem Diagramm oben (Stream für 3000 k video mit gestrichelten Linie) dargestellt.
2. Der neue Encoder muss dieselbe URL HTTP POST-Anforderungen als die fehlerhafte Instanz verwenden.
3. Die neue Encoder POST-Anforderung muss die gleichen fragmentierte MP4 Kopfzeile Felder als die fehlerhafte Instanz beinhalten.
4. Der neue Encoder muss für alle anderen laufenden Encoder für live derselben Präsentation synchronisierten Beispiele von Audio und Video mit ausgerichteten Fragment Begrenzung generieren ordnungsgemäß synchronisiert werden.
5. Der neue Stream muss semantisch entspricht, mit dem vorherigen Stream und austauschbare Kopf- und Fragment Ebene.
6. Der neue Encoder sollten Datenverluste zu minimieren.  Die Fragment_absolute_time und Fragment_index von Medien Fragmenten sollten ab dem Zeitpunkt zu erhöhen, in dem der Encoder zuletzt beendet wurde.  Die Fragment_absolute_time und Fragment_index sollten in einer zusammenhängenden Weise erhöhen, aber es ist zulässig, eine Diskontinuität bei Bedarf vorstellen.  Azure Media-Dienste ignoriert Fragmente, die sie bereits verarbeitet und erhalten, damit es besser, die Sie im Zweifelsfall Erneutes Senden als vorstellen Diskontinuitäten Stelle der Medienzeitachse Satzteile ist. 

##<a name="9-encoder-redundancy"></a>9. Redundanz encoder 

Für bestimmte kritische live Ereignisse, dass bei Bedarf auch höhere Verfügbarkeit und für Quality of Experience, es empfohlen wird, aktive redundante Encoder nahtloses Failover ohne Datenverlust erzielen einzusetzen.

![Image6][image6]

Wie in dem Diagramm oben dargestellt, gibt es zwei Gruppe der Encoder zwei Kopien der einzelnen Streams gleichzeitig verschieben, in der live-Dienst. Dieses Setup wird unterstützt, da Microsoft Azure Media-Dienste die Möglichkeit zum Filtern von doppelten Satzteile basierend auf Stream-ID und Fragment-Timestamp hat. Die resultierende live Stream und Archivierung werden ein einzelnes Kopie aller der Streams, die die beste mögliche Aggregation aus zwei Quellen ist. Beispielsweise in hypothetische extrem Fall, solange es ist eine Encoder (verfügt nicht über die identisch sein) zu einem bestimmten Zeitpunkt im Zeit für jede Stream ausführen, der sich daraus ergebende live Stream vom Dienst werden fortlaufender ohne Verlust von Daten. 

Die Anforderung für dieses Szenario ist fast identisch mit den Anforderungen in Encoder Failover Groß-/Kleinschreibung mit der Ausnahme, die die zweite Gruppe der Encoder zur gleichen Zeit wie der primären Encoder ausgeführt werden.

##<a name="10-service-redundancy"></a>10. Redundanz Dienst  

Hochgradig redundante globale Quadrat-verteilten Zufallsgröße ist manchmal erforderlich Cross-Region Sicherung Landes-/ Datenverluste verarbeitet haben. Klicken Sie auf der Suchtopologie "Encoder Redundanz" erweitern, können Kunden auswählen, eine Bereitstellung von redundanten Dienst in einem anderen Bereich sein, die mit dem 2. Satz von Encoder verbunden ist. Kunden konnte auch bei einem CDN Anbieter Bereitstellen einer Rahmen (globale Datenverkehr Manager) vor der zwei Bereitstellung von Diensten für nahtlos Clientdatenverkehr arbeiten. Identisch mit "Encoder Redundanz" Groß-/Kleinschreibung mit der einzige Ausnahme, die die zweite Gruppe der Encoder müssen auf einen anderen live Arbeitsordner Endpunkt Aufnahme sind die Anforderungen für die Encoder. Im nachstehenden Diagramm wird dieses Setup dargestellt:

![image7][image7]

##<a name="11-special-types-of-ingestion-formats"></a>11. spezielle Formattypen Aufnahme 

In diesem Abschnitt werden einige spezielle Art von live-Aufnahme-Formaten, die einige spezifischen Szenarios vorgesehen sind.

###<a name="sparse-track"></a>Gering gefüllte nachverfolgen

Bei der Übermittlung einer live-streaming Präsentation mit rich Client-Erfahrung ist häufig erforderlich, Zeit synchronisiert Ereignisse zu übertragen oder in-Band-mit dem Hauptfenster Media-Daten zeigt. Ein Beispiel dafür ist dynamische live Werbung einfügen. Diese Art von Ereignis Signalton unterscheidet sich von regulären aufgrund ihrer gering gefüllte Natur streaming Audio und Video. Kurzum, die Signaldaten normalerweise geschieht nicht kontinuierlich und das Intervall kann schwer Vorhersagen sein. Des Konzepts der gering gefüllten nachverfolgen wurde speziell für die Aufnahme und in-Band-Signaldaten übertragen.

Im folgenden finden Sie eine empfohlene Implementierung für Aufnahme gering gefüllte nachverfolgen:

1. Erstellen Sie einen separaten fragmentiert MP4-Bit-Stream, der nur gering gefüllte Titel ohne Spuren von Audio und Video enthält.
2. "Live Server Manifest im Feld" im Abschnitt 6 im Sinne [1] verwenden Sie Parameter "ParentTrackName" den Namen der übergeordneten Spur angeben. Lizenzinformationen finden Sie im Abschnitt 4.2.1.2.1.2 in [1] Weitere Details.
3. In der "Live Manifest Feld Server" ManifestOutput festgelegt werden muss auf "True".
4. Die gering gefüllte Art des Ereignisses signalisierendes wird angegeben, wird empfohlen, dass:
    1. Am Anfang des Ereignisses live sendet Encoder die ursprüngliche Kopfzeile Felder auf den Dienst, der den Dienst registrieren der gering gefüllten nachverfolgen im Clientmanifest zulassen möchten.
    2. Der Encoder sollte die HTTP POST-Anforderung beendet, wenn Daten nicht gesendet werden.  Ein langer HTTP-Beitrag, der nicht von Daten senden können Azure Media Services verhindern schnell Trennen vom Encoder bei einem Dienst Neustart aktualisieren oder Server wie der Media-Server vorübergehend in einem Empfangsvorgang auf Sockets blockiert werden. 
    3. Schließen der Encoder SHOULD während der Zeit, wenn Signaldaten nicht verfügbar ist, die HTTP POST anfordern.  Während die POST-Anforderung aktiv ist, sollte der Encoder Daten senden. 
    4. Beim Senden von gering gefüllte Satzteile kann Encoder explizite Content-Length-Header festlegen, sofern verfügbar.
    5. Beim Senden von gering gefüllte Fragment mit eine neue Verbindung sollte Encoder Starten von die Kopfzeile Kontrollkästchen gefolgt von den neuen Fragmenten senden. Dies ist der Fall behandelt, wobei Failover Vorkommnissen dazwischen und die neue gering gefüllte Verbindung zu einem neuen Server, der nicht vor der gering gefüllten nachverfolgen erkannt wurde hergestellt wird.
    6. Das Fragment gering gefüllte nachverfolgen wird zur Verfügung gestellt werden an dem Kunden bei der entsprechenden übergeordneten nachverfolgen Fragment, die gleich weist oder vergrößern Timestamp-Wert an dem Client zur Verfügung gestellt wird. Beispielsweise verfügt das gering gefüllte Fragment einen Zeitstempel der t = 1000, wird es nach dem Client angezeigt wird (vorausgesetzt, wird der Name der übergeordneten nachverfolgen) Video fragment Timestamp 1000 oder darüber hinaus, können sie das gering gefüllte Fragment t herunterladen erwartet = 1000. Bitte beachten Sie, dass das tatsächliche Signal sehr gut für eine andere Position in der Präsentation Zeitachse für ihren vorgesehenen Zweck verwendet werden kann. Im obigen Beispiel, ist es möglich, die das gering gefüllte t-Fragment = 1000 hat eine XML-Nutzlast zum Einfügen einer Anzeige in der Lage, die ein paar Sekunden ist also später.
    7. Die Nutzdaten des Fragment gering gefüllte nachverfolgen kann verschiedene Formate (z. B. XML- oder Text oder Binärzahl usw.) je nach den verschiedenen Szenarios werden. 


###<a name="redundant-audio-track"></a>Redundante Audio nachverfolgen

In einer normalen HTTP Adaptives Streaming Szenario (z. B. interpolierten Streaming oder Gedankenstrich) ist oft nur eine Audio-Nachverfolgen der gesamten Präsentation. Im Gegensatz zu video Spuren denen mehrere Ebenen von Qualität für den Kunden in Fehler aus, kann die audio-Spur einen einzelnen Punkt des Fehlers sein, wenn der Stream, der die audio-Spur enthält, Aufnahme fehlerhaft ist. 

Um dieses Problem zu beheben, unterstützt Microsoft Azure Media Services live Erfassung von redundanten audio Spuren. Die Idee ist, dass die gleiche audio-Spur in verschiedene Streams mehrmals gesendet werden kann. Während der Dienst die audio-Spur nur einmal im Clientmanifest registrieren wird, ist es redundante audio Spuren als Sicherungskopien zum Abrufen von audio Satzteile, wenn die primäre audio-Spur Problemen verwenden. Um redundante audio Spuren Aufnahme zu können, muss der Encoder:

1. Erstellen einer audio Spur in mehreren Fragment MP4-Bit-Streams. Redundanten audio Spuren muss semantisch gleichwertigen mit genau die gleichen Fragment Zeitstempel und austauschbare Kopf- und Fragment Ebene.
2. Stellen Sie sicher, dass die "Audiowiedergabe" Eintrag in das Live Server-Manifest (Abschnitt 6 [1]) werden für alle redundanten audio Spuren identisch.

Es folgt eine empfohlene Implementierung für redundante audio Spuren:

1. Jede eindeutige audio nachverfolgen allein in einem Stream zu senden.  Auch für jede dieser audio nachverfolgen Streams, wo 2nd Streams unterscheidet sich von 1. nur von der Bezeichner in der URL des HTTP-Beitrag senden ein redundantes Streams: {Protokoll} :// {Serveradresse} / {Punkt path}/Streams({identifier}) veröffentlichen.
2. Verwenden Sie separate Streams, um eine der beiden niedrigsten video Bitrate zu senden. Jeder dieser Streams sollten auch eine Kopie jeder eindeutige audio nachverfolgen enthalten.  Wenn mehrere Sprachen unterstützt werden, sollten diese Streams beispielsweise audio Spuren für die einzelnen Sprachen enthalten.
3. Formular mit separaten Server (Encoder) Instanzen codieren und senden Sie die redundante Streams in angegeben ist (1) und (2). 



##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png

 