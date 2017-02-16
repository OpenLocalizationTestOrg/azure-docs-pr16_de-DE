<properties 
    pageTitle="Erfahren Sie mehr über BizTalk-Dienste Begrenzungsebene | Microsoft Azure" 
    description="Informationen Sie zu Schwellenwerten begrenzungsebene und resultierender Runtime-Verhalten für BizTalk-Dienste. Begrenzungsebene basiert auf arbeitsspeicherauslastung und die Anzahl der Nachrichten. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>





# <a name="biztalk-services-throttling"></a>BizTalk-Dienste: Begrenzungsebene

Azure BizTalk-Dienste implementiert Dienst begrenzungsebene basierend auf zwei Bedingungen: arbeitsspeicherauslastung und die Anzahl der Nachrichten gleichzeitig verarbeiten. Dieses Thema listet die Drosselung Schwellenwerte und beschreibt das Runtime-Verhalten, wenn eine Bedingung Drosselung auftritt.

## <a name="throttling-thresholds"></a>Schwellenwerte begrenzungsebene

Die folgende Tabelle enthält die Drosselung Quell- und Schwellenwerte:

||Beschreibung|Niedrige Schwellenwert|Hohe Schwellenwert|
|---|---|---|---|
|Arbeitsspeicher|% des gesamten System Arbeitsspeicher verfügbar/PageFileBytes. <p><p>Total verfügbare PageFileBytes beträgt ungefähr 2 Mal der Arbeitsspeicher des Systems.|60 %|70 %|
|Verarbeitung von Nachrichten|Anzahl der Nachrichten, die gleichzeitig Verarbeitung|40 * Zahl der Kerne|100 * Anzahl der Kerne|

Wenn ein hoher Schwellenwert erreicht wird, beginnt Azure BizTalk-Dienste einschränken. Begrenzungsebene Stopps an, wenn der niedrige Schwellenwert erreicht wird. Beispielsweise wird der Dienst 65 % Systemarbeitsspeicher verwenden. In diesem Fall wird der Dienst nicht einschränken. Der Dienst wird gestartet, mithilfe von 70 % Systemarbeitsspeicher. In diesem Fall der Dienst Steuerung und weiterhin einschränken, bis der Dienst 60 % (niedrig Schwellenwert) Systemarbeitsspeicher verwendet.

Azure BizTalk-Dienste verfolgt die Drosselung Status (normalen Zustand im Vergleich zu reduzierten Zustand) und der Drosselung Dauer.


## <a name="runtime-behavior"></a>Laufzeit Verhalten

Wenn Azure BizTalk Services eine Drosselungsstatus platziert wird, geschieht Folgendes:

- Begrenzungsebene ist pro Instanz der Rolle. Beispiel:<br/>
RoleInstanceA begrenzungsebene ist. RoleInstanceB ist nicht begrenzungsebene. In diesem Fall werden Nachrichten in RoleInstanceB verarbeitet, wie erwartet. Nachrichten in RoleInstanceA verworfen und der folgende Fehler fehl:<br/><br/>
**Server ist ausgelastet. Versuchen Sie es erneut.**<br/><br/>
- Alle Quellen Abruf Umfrage oder Herunterladen eine Nachricht nicht. Beispiel:<br/>
Eine Verkaufspipeline zieht Nachrichten aus einer externen FTP-Quelle. Die Instanz der Rolle ausführen die Abruf wird in einer Drosselungsstatus. In diesem Fall beendet der Verkaufspipeline zusätzliche Nachrichten herunterladen, bis die Instanz der Rolle begrenzungsebene nicht mehr.
- Eine Antwort wird an den Client gesendet, damit der Client die Nachricht erneut senden kann.
- Sie müssen warten, bis der begrenzungsebene gelöst ist. Insbesondere müssen Sie warten, bis der niedrige Schwellenwert erreicht wird.

## <a name="important-notes"></a>Wichtige Hinweise
- Begrenzungsebene kann nicht deaktiviert werden.
- Schwellenwerte begrenzungsebene kann nicht geändert werden.
- Begrenzungsebene wird die gesamte System implementiert.
- Azure SQL-Datenbankserver enthält auch integrierte begrenzungsebene.

## <a name="additional-azure-biztalk-services-topics"></a>Weitere Themen im Azure BizTalk-Dienste

-  [Installieren die Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Lernprogramme: Azure BizTalk-Dienste](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Wie kann ich mithilfe von starten Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk-Dienste](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Siehe auch
- [BizTalk-Dienste: Entwickler, Basic, Standard und Premium-Editionen Diagramm](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk-Dienste: Provisioning klassischen mithilfe von Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk-Dienste: Provisioning Statusdiagramm](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk-Dienste: Dashboard, überwachen und Dezimalstellen Registerkarten](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk-Dienste: Sichern und Wiederherstellen](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk-Dienste: Name des Herausgebers und Herausgeber Schlüssel](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
 
