<properties 
    pageTitle="Azure RemoteApp - testen Ihr Netzwerk Bandbreite Verwendung mit anhand einiger allgemeinen Szenarien | Microsoft Azure"
    description="Erfahren Sie, wie etwa häufige Verwendungsszenarien, die Ihnen helfen können Indexeigenschaften Bandbreite Netzwerk für Azure RemoteApp ermitteln."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />
    
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Azure RemoteApp - testen Ihr Netzwerk Bandbreite Verwendung mit anhand einiger allgemeinen Szenarien

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Wie [Schätzung Azure RemoteApp Netzwerk Bandbreite](remoteapp-bandwidth.md)Verwendung besprochen überprüft die beste Methode zum Ermitteln, was der Einfluss der Azure RemoteApp mit Ihrem Netzwerk einige Verwendung ausgeführt wird. Führen Sie diese Tests für einen festgelegten Zeitraum, und messen Sie die Bandbreite für jedes Szenario erforderlich. Wenn Sie die Möglichkeit haben, können Sie auch messen das Netzwerk Paket Verlust und Netzwerk Zittern um die Netzwerk-Muster zu verstehen, die in Ihrer Umgebung erstellt wird.

    
Wenn die Bandbreite Verwendung auswerten, beachten Sie, dass es sich bei Verwendung zwischen verschiedenen Benutzern in Ihrem Unternehmen variiert. Text Leser und Autoren nutzen beispielsweise normalerweise weniger Bandbreite als Benutzer aus, für die Arbeit mit Video. Die besten Ergebnisse erzielen Sie untersuchen Sie Ihren eigenen Benutzer Anforderungen, und erstellen Sie eine Kombination aus den folgenden Szenarien, die die Benutzer in Ihrem Unternehmen am besten darstellt. Denken Sie daran, [Überprüfen Sie die Faktoren, die Einfluss Bandbreite Verwendung und Benutzer kommen](remoteapp-bandwidthexperience.md) –, mit denen Sie die ideale Tests identifizieren.

Zuerst erfahren Sie mehr über die Tests, wählen Sie die Mischung aus, und führen Sie diese aus. In der folgenden Tabelle können Sie die um Leistung zu verfolgen.

>[AZURE.NOTE] Wenn Sie Ihrer eigenen Netzwerktests nicht, oder Sie verfügen nicht über die Zeit dafür, schauen Sie sich unsere [Standard-Netzwerk Bandbreite schätzt/Empfehlungen](remoteapp-bandwidthguidelines.md). Ihre mit möglicherweise, jedoch variieren, damit Sie *können* eigene Tests ausführen, Sie sollten.


## <a name="the-usage-tests"></a>Die Verwendung tests
Jede dieser Tests für verschiedene viel Zeit ausführen und Testen der verschiedenen Funktionen/Features, die Bandbreite beanspruchen. Denken Sie daran, der Mischung aus Test auswählen, dass am besten Unternehmen einzelner Benutzer entspricht.
 
### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>Geschäftsleitung/komplexen PowerPoint - 900-1000 Sekunden ausführen

Ein Benutzer bietet zwischen 45 bis 50 hoher Auflösung an Folien mithilfe von Microsoft Office PowerPoint in den Vollbildmodus. Bilder, Übergänge (mit Animationen) und Hintergründe mit Farbverlauf, die für Ihr Unternehmen typische sind, sollte die Folien enthalten. Der Benutzer muss mindestens 20 Sekunden auf jeder Folie verbringen.
    
Dieses Szenario erstellt bursty Datenverkehr, beim Übergang von einer Folie zur nächsten Folie in der Präsentation.
    
### <a name="simple-powerpoint---run-for-620-seconds"></a>Einfache PowerPoint - ~ 620 Sekunden ausführen

Ein Benutzer bietet eine einfache PowerPoint-Datei mit maximal 30 Folien mithilfe von Microsoft Office PowerPoint im Vollbildmodus an. Folien werden weitere Text ankommt als in der Geschäftsleitung/komplexen PowerPoint-Szenario und einfacher Hintergründe und Bilder (Schwarz Diagramme) haben. 
    
### <a name="internet-explorer---run-for-250-seconds"></a>InternetExplorer - ~ 250 Sekunden ausführen

Ein Benutzer durchsucht im Web mit Internet Explorer. Der Benutzer navigiert und führt einen Bildlauf durch, bis eine Mischung aus Text, Bilder natürlich und einige Schema Diagrammen. Die Webseiten gespeichert, auf dem lokalen Laufwerk des Servers Remote Desktop Session Host (RD Session Host) als ein. MHT-Datei. Der Benutzer einen Bildlauf, Bild-auf, Bild-ab, nach oben und nach-unten-Taste mit variierenden Abständen für jeden Schlüssel/Bildlaufleiste verwenden:
    
    - Nach unten – 250 Tastenanschlägen sehr 500 ms
    - Bild-auf - 36 Tastenanschlägen alle 1000 ms
    - Nach-unten Sie-75 Tastenanschlägen alle 100 ms
    - Bild-ab - 20 Tastenanschlägen alle 500 ms
    - Bis - Tastatureingaben 120 jeder 300 ms
    
### <a name="pdf-document---simple---run-for-610-seconds"></a>PDF-Dokument – einfache - ~ 610 Sekunden ausführen
Ein Benutzer liest und durchsucht ein PDF-Dokument auf verschiedene Weise mithilfe von Adobe Acrobat Reader. Tabellen, einfachen Grafiken und Schriftarten für mehrere Text sollte das Dokument bestehen. Das Dokument ist 35-40 Seiten lang. Der Benutzer ein Bildlauf durchgeführt, mit zwei unterschiedlichen Zinssätzen, Abwärtskompatibilität und weiterleitet, in vier unterschiedliche Zoomgrößen (Seite zum Anpassen an die Breite anpassen (100 %) und ein weiteres Ihrer Wahl). Das vergrößern: Damit ist sichergestellt, dass der Text (Schriftart) in verschiedenen Größen wiedergegeben. Bildlauf ist nach unten mithilfe von Bild-auf, Bild-ab, nach oben und nach-unten-Taste mit variierenden Abständen für jede Bildlaufleiste.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>PDF-Dokument - gemischten - und-Los ~ 320 Sekunden
Ein Benutzer liest und durchsucht ein PDF-Dokument auf verschiedene Weise mithilfe von Adobe Acrobat Reader. Das Dokument besteht aus hoher Qualität Bilder (einschließlich Fotos), einfache Diagramme, Tabellen und mehrere Schriftarten für Text. Der Benutzer ein Bildlauf durchgeführt, mit zwei unterschiedlichen Zinssätzen, Abwärtskompatibilität und weiterleitet, in vier unterschiedliche Zoomgrößen (Seite zum Anpassen an die Breite anpassen (100 %) und ein weiteres Ihrer Wahl). Das vergrößern: Damit ist sichergestellt, dass der Text (Schriftart) in verschiedenen Größen wiedergegeben. Bildlauf ist nach unten mithilfe von Bild-auf, Bild-ab, nach oben und nach-unten-Taste mit variierenden Abständen für jede Bildlaufleiste.

### <a name="flash-video-playback---run-for-180-seconds"></a>Videowiedergabe in Flash - ~ 180 Sekunden ausführen
Ein Benutzer Ansichten einer Adobe Flash-codierte Video in eine Webseite eingebettet wird. Die Webseite wird in der lokalen Festplatte des Servers RD Session Host gespeichert. Das Video wird durch einen eingebetteten Player-Plug-Ins in Internet Explorer wiedergegeben.

Dieses Szenario emuliert Benutzer Rich-Inhalt mit multimedia Webseiten anzeigen. Die meisten Daten sollte Feld bis VOBR.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Word remote eingeben – ~ 1800 Sekunden ausführen
Ein Benutzer eingibt ein Dokument über eine RDP-Sitzung an. Tastatureingaben werden clientseitig über die RDP-Sitzung zu einem Dokument in Microsoft Word gesendet in der remote-Sitzung ausgeführt. Der Eingabe Prozentsatz ist ein Zeichen jeder 250 ms (total 7050 Zeichen). 

Dies ist eine der häufigsten Szenarios für ein Information Worker. Dieses Szenario überprüft der Reaktionszeiten eines Benutzers Nachschlagen eines Prozessor moderne Arbeit eingeben. Dieses Szenario ist vertrauliche gerade kleinen Änderungen Bandbreite Verwendung.

## <a name="tracking-the-test-results"></a>Nachverfolgen der Testergebnisse

In der folgenden Tabelle können für die folgenden Szenarien in Ihrer Umgebung ausgewertet werden soll. Die unten angegebenen Daten ist nur für Abbildung – es möglicherweise erheblich abweicht, was Sie beobachten. 

Zur Vereinfachung wird davon ausgegangen, dass alle Szenarien werden getestet mit der gleichen Auflösung 1920 x 1080 Pixel und TCP Transport in einem Netzwerk mit Wartezeit (Verzögerung) unter 200 ms und Netzwerk in das Endnotenzeichen 120 ms + 1 Jitter %.

Über die Tabelle:
- **Mittelwert auftreten** , enthält die Netzwerk-Bandbreite, in dem Benutzerproduktivität wird nicht erheblich beeinträchtigt aber nicht die gelegentliche Video- oder Audioclip Probleme aufgeführt. Das System ist schnell wiederherstellen, indem Sie die dynamische Logik nutzen können. Die Bandbreite Netzwerk schätzt Versuch, um die Qualität der benutzererfahrung zu gewährleisten.
 - **Noticeable Probleme (Seitenumbruch Punkt)** enthält die Bandbreite, wo Benutzer möglicherweise signifikante Probleme in ihrer Erfahrung beachten und ihre Produktivität für messbare Zeiträume beeinträchtigt wird. Zu diesem Zeitpunkt die RDP Algorithmen kämpfen und können nicht gewährleisten des Benutzers für Quality of Experience wegen unzureichender Bandbreite.
 - **Empfohlen** enthält die Netzwerk-Bandbreite für gut oder hervorragend Erfahrung empfohlen. Es ist in der Regel einen Schritt größer als der Wert in die entsprechende Spalte **Mittelwert auftreten** .
 - **Notizen** gehören Beobachtungen und Kommentare.
 
| Test                  | Durchschnittliche Erfahrung | Auffällig Probleme (Seitenumbruch Punkt) | Empfohlene Bandbreite | Notizen                                                              |
|-----------------------|--------------------|---------------------------------|-------------------------------|--------------------------------------------------------------------|
| Geschäftsleitung/komplexen PPT | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s bevorzugte    | Bei 1 MB/s verloren viele Animationen                                   |
| Einfache PPT            | 5 MB/s              | 256 KB/s                         | 10 MB/s                        | Laden die Folien mit 256 KB/s mit auffällig Verzögerung                   |
| InternetExplorer     | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s bevorzugte    | Bei 1 MB/s Webvideos sind verschwommen und unvollständig, schnell durchführen eines Bildlaufs gibt es Probleme |
| Einfache PDF-Datei            | 1 MB/s              | 256 KB/s                         | 5 MB/s                         | Bei 256 KB/s dauert es eine Weile zum Laden der Seite                       |
| Gemischte PDF-Datei             | 1 MB/s             | 256 KB/s                         | 5 MB/s                         | Bei 256 KB/s gelangen Sie mit die Seite viel Zeit zum Laden    |
| Flash Videowiedergabe  | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s bevorzugte    | Das Video mit 1 MB/s ist körnig und einige Frames werden gelöscht.           |
| Word remote eingeben    | 256 KB/s            | 128 KB/s                         | 1 MB/s                         | Bei 256 KB/s möglicherweise Benutzer die Zeit zwischen Tastatureingaben fest.             |

Um die Bandbreite pro Benutzer auszuwerten, erstellen Sie eine Kombination der obigen Szenarios und den entsprechenden Anteil erforderlichen Bandbreite aus. Wählen Sie die höchste Zahl für Ihre Szenarios erforderlich. Da Benutzer fast nie das System alleine verwenden, sollten Sie einige reservieren für Benutzer, die gleichzeitig in einem Netzwerk arbeiten.
     
## <a name="learn-more"></a>Weitere Informationen
- [Schätzen Sie Azure RemoteApp Netzwerk Bandbreite Verwendung](remoteapp-bandwidth.md)

- [Azure RemoteApp - treten wie Bandbreite und die Qualität des Arbeit zusammen auf?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp Netzwerkbandbreite - Richtlinien (Wenn Sie Ihre eigenen überprüfen können)](remoteapp-bandwidthguidelines.md)