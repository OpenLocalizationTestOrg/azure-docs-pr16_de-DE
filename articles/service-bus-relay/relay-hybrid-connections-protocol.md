<properties 
    pageTitle="Azure Relay Hybrid Verbindungen Protocol | Microsoft Azure"
    description="Zure Relay Hybrid Verbindungen Protokoll Guide."
    services="service-bus"
    documentationCenter="na"
    authors="clemensv"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="sethm" />

# <a name="azure-relay-hybrid-connections-protocol"></a>Azure Relay Hybrid Verbindungen Protocol

Azure-Relay ist eine der Säulen Hauptfunktionen der Azure-Dienstbus-Plattform. Das Relay des neuen "Hybrid Verbindungen" Videofunktionen ist eine sichere, offene Protokoll Entwicklung, basierend auf HTTP und WebSockets.

Es ersetzt die ehemalige, gleichmäßig mit der Bezeichnung "BizTalk-Dienste" Features, die auf einem Fundament eigene Protokoll erstellt wurde. Die Integration der Hybrid Verbindungen in Azure App Services funktionieren weiterhin als-ist.

"Hybrid Verbindungen" ermöglicht Herstellen der binäre Stream bidirektionale Kommunikation zwischen zwei vernetzten Clientanwendungen, bei dem eine oder beide der beteiligten hinter NATs oder Firewalls befinden können.

Dieses Dokument beschreibt die clientseitige Interaktionen mit der Hybrid Verbindungen Relay zum Herstellen einer Verbindung-Clients im Zuhörer und Absender Rollen und wie Listener neue Verbindungen akzeptiert.

## <a name="interaction-model"></a>Interaktionsmodell

Das Hybrid Verbindungen Relay verbindet zwei Parteien können, indem Sie einen Punkt Rendezvous in der Azure Cloud, die sowohl Parteien können ermitteln und hinsichtlich des Netzwerks eigene Herstellen einer Verbindung mit. Dieser Punkt Rendezvous heißt "Hybrid Verbindung" in diese und andere Dokumentation, in die APIs und Azure-Portal. Der Endpunkt Hybrid Verbindungen wird als "Service" im weiteren Verlauf dieses Dokuments bezeichnet werden.

Das Interaktionsmodell leans über das Verzeichnis eingesetzten viele andere Netzwerk-APIs:

Es gibt ein befinden, der zuerst gibt Readiness eingehende Verbindungen verarbeitet und anschließend beim Eintreffen akzeptiert. Auf der Seite befindet sich ein Verbindungslinien Client, der die Zuhörer, gewartet hat die Verbindung mit für das Einrichten eines Pfads bidirektionale Kommunikation akzeptiert eine Verbindung herstellt. "Verbinden", "Spielen", "Akzeptieren" sind denselben Konditionen, die Sie auf den meisten Sockets APIs gefunden werden.

Alle weitergeleitete Kommunikationsmodell weist jede Partei ausgehende Verbindungen rechts einen Endpunkt, stellen Sie die zudem ist die "Zuhörer" eines "Clients" Zielsprache verwendet und anderen überladenen Terminologie möglicherweise auch verursachen; die präzise uns daher für Hybrid Verbindungen verwendete Terminologie sieht wie folgt aus:

Die Programme auf beiden Seiten einer Verbindung werden "Kunden", bezeichnet, da sich Clients zum Dienst handelt. Der Client, der wartet und akzeptiert Verbindungen ist die "Zuhörer" oder unter dem Gesichtspunkt sind die "Zuhörer Rolle" sein. Der Client, der eine neue Verbindung in Richtung eines Zuhörer über den Dienst initiiert heißt "Absender" oder "Absender Rolle".

## <a name="listener-interactions"></a>Zuhörer Interaktionen

Die Zuhörer hat vier Interaktionen mit dem Dienst an. alle Kabel Details werden weiter unten in diesem Dokument im Referenzabschnitt beschrieben.

### <a name="listen"></a>Abhören

Um Readiness zum Dienst anzugeben, der eine Zuhörer erstellt bereit zum Akzeptieren von Verbindungen, eine ausgehende Web Sockets-Verbindung. Der Verbindungshandshake führt den Namen einer Hybrid Verbindung konfiguriert im Namespace Relay und ein Sicherheitstoken, das die "spielen" für die Übertragung von Recht auf, die einen Namen aus. Wenn die Web Sockets vom Dienst akzeptiert wird, die Registrierung ist abgeschlossen und definierte Web Sockets wird als "Kanal Steuerelement" für die Aktivierung alle nachfolgender Interaktionen beibehalten. Dienst können bis zu 25 gleichzeitige Listener eine Verbindung Hybrid an. Wenn Sie 2 oder mehr aktive Listener vorhanden sind, werden eingehende Verbindungen diese in zufälliger Reihenfolge verteilt werden; ' wissenschaftsmesse ' Verteilung ist nicht unbedingt.

### <a name="accept"></a>Annehmen

Sobald eine neue Verbindung-Dienst ein Absenders geöffnet wird, wird der Dienst auswählen und benachrichtigen einer der aktiven Listener, die in Verbindung. Die Benachrichtigung wird über den Kanal öffnen Steuerelement auf dem Zuhörer gesendet, als JSON-Nachricht mit der URL des Web Sockets-Endpunkts an, die die Zuhörer zu für das Akzeptieren der Verbindungs herstellen muss.

Die URL kann und muss direkt von der Zuhörer ohne zusätzlichen Aufwand verwendet werden; die codierte Informationen gilt nur für einen kurzen Zeitraum, im Wesentlichen für lange der Absender sind, für die Verbindung definierte End-to-End sein, aber bis zu einem Maximum von 30 Sekunden warten ist. Die URL kann nur für eine erfolgreiche Verbindung mit verwendet werden. Sobald die Web Sockets-Verbindung mit der URL Rendezvous eingerichtet wurde, wird alle weiterer Aktivitäten mit diesem Web Sockets von und an den Absender, ohne Eingriff oder Interpretation vom Dienst weitergeleitet.

### <a name="renew"></a>Erneuern 

Das Sicherheitstoken, das verwendet werden muss, um die Zuhörer registrieren und Verwalten des Steuerelement-Kanals möglicherweise ablaufen, während die Zuhörer aktiv ist. Token Ablauf wirkt sich nicht auf die laufende Verbindungen, aber es bewirkt, dass den Steuerelement Channel vom Dienst am oder unmittelbar zurückzunehmen, nachdem die Instant Ablauf gelöscht werden. Die Aktion "Erneuern" ist eine JSON-Nachricht, die die Zuhörer senden können, das mit dem Steuerelement Kanal verknüpfte Token ersetzen, damit der Steuerelement-Kanal für längere Zeiträume beibehalten werden kann.

### <a name="ping"></a>Ping 

Wenn der Steuerelement-Kanal sehr lange Zwischenstufen auf die Möglichkeit, im Leerlauf bleibt möglicherweise wie Laden Balancers oder NATs die TCP-Verbindung entfernen. Die Aktion "Ping" vermieden werden, die durch eine kleine Datenmenge auf dem Kanal, der daran alle im Netzwerk weiterleiten, die die Verbindung aktiv sein soll erinnert, und es dient auch als Verfügbarkeit Test für die Zuhörer senden. Wenn der Ping fehlschlägt, der Steuerelement-Kanal sollte installiertes berücksichtigt werden, und schließen Sie die Zuhörer wieder sollte.

## <a name="sender-interaction"></a>Interaktion der Absender

Der Absender hat nur eine einzige Interaktion mit dem Dienst, er eine Verbindung herstellt.

### <a name="connect"></a>Verbinden

Die Aktion "Verbinden" wird geöffnet, ein Sockets Web-Dienst, den Namen der Verbindung Hybrid bereitstellen und ein (optional, aber standardmäßig erforderlich) Sicherheitstoken erworbenes Berechtigung "Senden", in der Abfragezeichenfolge. Der Dienst wird anschließend interagieren mit dem Zuhörer auf die oben beschriebenen Weise und lassen die Zuhörer eine Rendezvous-Verbindung zu erstellen, die mit diesem Web Sockets zugeordnet werden soll. Nachdem die Web Sockets zugestimmt haben, werden alle Weitere Interaktionen auf Web Sockets daher mit einem verbundenen Zuhörer sein.

## <a name="interaction-summary"></a>Interaktion der Zusammenfassung

Das Ergebnis dieses Interaktion Modells ist, dass der Absender Client außerhalb des Handshakes mit eines "Säubern" Web Sockets stammt, dem mit einem Zuhörer verbunden ist, und, die keine weiteren Preambles oder Vorbereitung benötigt. Dadurch wird praktisch jedem vorhandenen Web Sockets-Client-Implementierung leicht Hybrid Verbindungen Dienst nutzen, indem Sie einfach eine ordnungsgemäß erstellte URL in ihren Web Sockets-Client-Ebene angeben.

Die Verbindung Rendezvous Web Sockets, die die Zuhörer durch die Interaktion akzeptieren abruft ist auch übersichtliche und alle vorhandenen Web Sockets Server-Implementierung mit einigen minimale zusätzliche Abstraktion, mit der unterschieden zwischen "akzeptieren" Vorgänge auf deren Framework des lokalen Netzwerk zu überwachen und Hybrid-Verbindungen remote "akzeptieren" Vorgänge übergeben werden kann.

## <a name="protocol-reference"></a>Protokoll Bezug

In diesem Abschnitt werden die Details der oben beschriebenen Protokoll Interaktionen an.

Alle Web Socketverbindungen wurden Port 443 als ein Upgrade von HTTPS-1.1 vorgenommen die häufig nach einige Web Sockets Framework oder API abstrahiert werden. Die zugehörige Beschreibung Implementierung neutrale, verbleibt ohne einen bestimmten Rahmen vorschlagen.

## <a name="listener-protocol"></a>Zuhörer Protokoll

Das Protokoll Zuhörer besteht aus zwei Verbindung Gesten und drei Nachrichtenvorgänge.

### <a name="listener-control-channel-connection"></a>Zuhörer Steuerelement Channel-Verbindung

Der Steuerelement-Kanal wird geöffnet, bei der Erstellung einer Web Sockets Verbindungs mit:

*Wss: / / {Namespace-Adresse} /* *$servicebus* */* *Hybridconnection /**{Path}? Sb-Hc-Action =... und Sb-Hc-Id =... und Sb-Hc-Token =... "*

Die *Namespace-Adresse* ist den vollqualifizierten Domänennamen des Namespace Azure-Relay, der die Verbindung Hybrid, in der Regel des Formulars hostet {*Name}. servicebus.windows.net.*

Die Zeichenfolge Parameter Abfrageoptionen sind wie folgt

| Parameter        | Erforderlich? | Beschreibung                                                                                                                                                                                        |
|--------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB-Hc-Aktion | Ja       | Die Rolle des Zuhörer muss der Parameter **Sb-Hc-Action = Abhören**                                                                                                                                |
| {Path}       | Ja       | Der URL-codierte Namespacepfad der vorkonfigurierten in Verbindung mit dieser Zuhörer auf registrieren. Dieser Ausdruck wird angefügt, um zu den festen * **$servicebus**/**Hybridconnection /*** Pfadteil. |
| SB-Hc-token  | Ja\*     | Die Zuhörer muss Bereitstellen einer gültigen und URL-codierte Service Bus freigegeben Access Token für den Namespace oder Hybrid Verbindung aus, die für die Übertragung von **Abhören** rechts.                                           |
| SB-Hc-id     | Nein        | Diese Client bereitgestellte optional ID ermöglicht End-to-End-diagnostic Tracing.                                                                                                                             |

Wenn die Web Sockets-Verbindung aufgrund der Hybrid Verbindung Pfad nicht registriert, oder ein Token ungültige oder fehlende oder ein anderer Fehler fehlschlägt, wird das Fehler Feedback mithilfe des normalen HTTP 1.1 Status Feedback-Modells bereitgestellt werden. Die Beschreibung des Status wird eine Fehler Verlauf-Id enthalten, die zur Unterstützung von Azure kommuniziert werden kann:

| Code | Fehler          | Beschreibung                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Nicht gefunden      | Der **Pfad** des Hybrid Verbindung ist ungültig oder die base-URL ist ungültig |
| 401  | Nicht autorisierte   | Das Sicherheitstoken ist fehlende oder falsch formatierte oder ist ungültig                  |
| 403  | Verboten      | Das Sicherheitstoken gilt nicht für diesen Pfad für diese Aktion          |
| 500  | Interner Fehler | Leider nicht geklappt im Dienst                                    |

Wenn die Web Sockets-Verbindung absichtlich vom Dienst beendet wird, nachdem es anfangs eingerichtet wurde, den Grund für dadurch mithilfe eines entsprechenden Web Sockets Protokoll Fehlercodes sowie eine beschreibende Fehlermeldung angezeigt, die auch eine Verlauf-Id angegeben wird mitgeteilt wird. Der Dienst wird nicht ab dem Steuerelement Kanal herunter, ohne dass einen Fehler auftritt. Säubern beenden ist Client gesteuert.

| WS Status | Beschreibung                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Der Pfad Hybrid Verbindung wurde gelöscht oder deaktiviert wurde.                           |
| 1008      | Das Sicherheitstoken ist abgelaufen, und die Autorisierungsrichtlinie wird daher verletzt. |
| 1011      | Leider nicht geklappt innerhalb der Dienst.                                           |

### <a name="accept-handshake"></a>Handshake annehmen 

Die Benachrichtigung akzeptieren wird vom Dienst an die Zuhörer über den zuvor definierte Steuerelement-Kanal als JSON-Nachricht in einem Web Sockets Textrahmen gesendet. Es gibt keine Antwort auf diese Nachricht aus.

Die Nachricht enthält ein JSON-Objekt mit der Bezeichnung "Annehmen", die die folgenden Eigenschaften zu diesem Zeitpunkt definiert:

-   **Adresse** – die URL-Zeichenfolge für die Herstellung der Web Sockets zum Dienst verwendet werden, um eine eingehende Verbindung anzunehmen.

-   **Id** – die eindeutige ID für diese Verbindung. Wenn die Id, die vom Absender Client bereitgestellt wurde, ist der Wert des Absenders bereitgestellt hat, andernfalls handelt es sich um einen Wert vom System generiert werden.

-   **ConnectHeaders** – alle HTTP-Header, die an den Endpunkt Relay vom Absender bereitgestellten wozu auch die Sec-WebSocket-Protokoll und die Sec-WebSocket-Erweiterungen Überschriften.

| Nachricht annehmen                                                                     |
|------------------------------------------------------------------------------------|
```json
{                                                                                  
    "accept" : {                                                                                    
        "address" : "wss://168.61.148.205:443/$servicebus/hybridconnection/{path}?...",                                                                          
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736\_G0\_G1",                         
        "connectHeaders" : {                                                                
            "Host" : "…",                                                                       
            "Sec-WebSocket-Protocol" : "…",                                                     
            "Sec-WebSocket-Extensions" : "…"                                                    
        }                                                                                     
    }
}
```

Der URL in das JSON-Nachricht bereitgestellt wird von der Zuhörer zum Web Sockets zum annehmen oder Ablehnen des Absenders Sockets herzustellen.

#### <a name="accepting-the-socket"></a>Akzeptieren des Sockets

Wenn Sie annehmen, stellt die Zuhörer eine WebSocket-Verbindung mit der angegebenen Adresse her.

Denken Sie, dass für den Zeitraum Vorschau die Adresse URI möglicherweise eine absolut erforderlichen IP-Adresse und das TLS-Zertifikat vom Server bereitgestellten Überprüfung auf die Adresse fehl schlägt. Dies wird bei der Vorschau behoben werden.

Wenn die Meldung "akzeptieren" Kopfzeile "Sec-WebSocket-Protokoll" enthält, wird davon ausgegangen, dass die Zuhörer, wenn sie dieses Protokoll unterstützt nur die WebSocket akzeptiert und, dass sie die Kopfzeile legt fest, wie die WebSocket hergestellt wird.

Dasselbe gilt für die Kopfzeile "Sec-WebSocket-Erweiterungen". Wenn das Framework eine Erweiterung unterstützt, sollte die Kopfzeile auf die Antwort des *Servers* Seite des Handshakes erforderlich "Sec-WebSocket-Erweiterungen" für die Erweiterung festgelegt werden.

Die URL muss als verwendet werden – ist für das Einrichten des Sockets annehmen, aber die folgenden Parameter enthält:

| Parameter        | Erforderlich? | Beschreibung                                                                                                                                                                                                                                                                                   |
|--------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB-Hc-Aktion | Ja       | Für das Akzeptieren eines Sockets muss der Parameter **Sb-Hc-Action = annehmen**                                                                                                                                                                                                                          |
| {Path}       | Ja       | Der URL-codierte Namespacepfad der vorkonfigurierten in Verbindung mit dieser Zuhörer auf registrieren. Dieser Ausdruck wird angefügt, um zu den festen * **$servicebus**/**Hybridconnection /*** Pfadteil.                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The path expression MAY be extended with a suffix and a query string expression that follows the registered name after a separating forward slash.                                                                                                                                             
                                                                                                                                                                                                                                                                                                                           
                            This allows the sender client to pass dispatch arguments to the accepting listener when it is not possible to include HTTP headers.                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The expectation is that the listener framework will parse out the fixed path portion and the registered name from the path and make the remainder, possibly without any query string arguments prefixed by “sb-“, available to the application for deciding whether to accept the connection.  
                                                                                                                                                                                                                                                                                                                           
                            For more detail see the “Sender Protocol” section below.                                                                                                                                                                                                                                       |
| SB-Hc-Id | No        | Beschreibung der oben **Id** wird angezeigt.                                                                                                                                                                                                                                                              |

Ist ein Fehler, zu der Dienst möglicherweise wie folgt Antworten:

| Code | Fehler          | Beschreibung                         |
|------|----------------|-------------------------------------|
| 403  | Verboten      | Die URL ist ungültig.               |
| 500  | Interner Fehler | Leider nicht geklappt im Dienst |

Nachdem die Verbindung hergestellt wurde, wird der Server Web Sockets deaktivieren, wenn des Absenders Web Sockets nach unten oder mit dem folgenden Status wird

| WS Status | Beschreibung                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Die Verbindung schließt die Clients des Absenders                                        |
| 1001      | Der Pfad Hybrid Verbindung wurde gelöscht oder deaktiviert wurde.                           |
| 1008      | Das Sicherheitstoken ist abgelaufen, und die Autorisierungsrichtlinie wird daher verletzt. |
| 1011      | Leider nicht geklappt innerhalb der Dienst.                                           |

#### <a name="rejecting-the-socket"></a>Ablehnen von Sockets

Ablehnen von Sockets nach verborgenen die Meldung "akzeptieren" erfordert einen ähnlichen Handshake, damit der Statuscode und Status Beschreibung den Grund für die Ablehnung Kommunikation wieder an den Absender versendet werden können.

Hier die Protokoll Entwurf Wahl besteht darin, einen WebSocket-Handshake verwenden (die zum Beenden eines definierten Fehlerstatus vorgesehen ist), damit die Zuhörer Clientimplementierungen können weiterhin auf kein WebSocket-Client verlassen und brauchen eine zusätzliche einsetzen, bare HTTP-Client.

Zum abweisen des Sockets Client nimmt die Adresse URI aus der Nachricht "Annehmen" und fügt an zwei Abfrageparameter Zeichenfolge an:

| Parameter             | Erforderlich? | Beschreibung                             |
|-------------------|-----------|-----------------------------------------|
| statusCode        | Ja       | HTTP-Status Codezahl                |
| statusDescription | Ja       | Menschen lesbaren Grund für die Ablehnung |

Der resultierende URI wird dann zum Herstellen einer Verbindung WebSocket; Beachten Sie erneut, die das TLS-Zertifikat nicht möglicherweise die Adresse bei der Vorschau entsprechen, damit die Überprüfung deaktiviert auftritt.

Wenn ordnungsgemäß ausgefüllt haben, tritt diese Handshake absichtlich mit einem HTTP-Fehlercode 410, da keine WebSocket eingerichtet wurde. Falls ein Fehler auftritt, stehen diese Optionen aus:

| Code | Fehler          | Beschreibung                         |
|------|----------------|-------------------------------------|
| 403  | Verboten      | Die URL ist ungültig.               |
| 500  | Interner Fehler | Leider nicht geklappt im Dienst |

### <a name="listener-token-renewal"></a>Zuhörer token Erneuerung

Wenn die Zuhörer des Token gerade abläuft, kann es es per Textnachricht Rahmen zum Dienst über den Kanal definierte Steuerelement ersetzen. Die Nachricht enthält ein JSON-Objekt, das mit dem Namen "RenewToken", das die folgende Eigenschaft zu diesem Zeitpunkt definiert:

-   **token** – eine gültige, URL-codierte Service Bus freigegeben Access Token für den Namespace oder Hybrid Verbindung aus, die für die Übertragung von rechts **Abhören** .

| RenewToken Nachricht                                                                                                                                                    
|------------------------------------------------------------------------------------------------|
```json
{
    "renewToken" : {      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fHybridConnection1%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
    }                                                                                                                                                            
}
```                                                                                                                                                                      

Wenn die token Validierung fehlschlägt, der Zugriff verweigert wird und Cloud-Dienst ein Steuerelement Channel Websocket mit einem Fehler schließt, gibt es keine Antwort.

| WS Status | Beschreibung                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1008      | Das Sicherheitstoken ist abgelaufen, und die Autorisierungsrichtlinie wird daher verletzt. |

<a name="sender-protocol"></a>Absender-Protokoll
---------------

Das Protokoll Absender ist effektiv wie eine Zuhörer eingerichtet wird. Das Ziel ist die maximale Transparenz des End-to-End-Web Sockets. Herstellen einer Verbindung mit die Adresse entspricht dem für den Zuhörer, aber "Aktion" unterscheidet sich und das Token benötigt eine andere Berechtigung:

*Wss: / / {Namespace-Adresse} /* *$servicebus* */* *Hybridconnection /**{Path}? Sb-Hc-Action =... und Sb-Hc-Id =... \[& Sbc-Hc-Token =... \]*

Die *Namespace-Adresse* ist den vollqualifizierten Domänennamen des Namespace Azure-Relay, der die Verbindung Hybrid, in der Regel des Formulars hostet {*Name}. servicebus.windows.net.*

Die Anforderung, enthalten möglicherweise beliebigen zusätzlichen HTTP-Header, auch die Anwendung definiert. Alle bereitgestellten Header Datenfluss, um die Zuhörer und befindet sich auf das Objekt "ConnectHeader" der Nachricht Steuerelement "akzeptieren".

Die Zeichenfolge Parameter Abfrageoptionen sind wie folgt

| Parameter        | Erforderlich? | Beschreibung                                                                                                                                                                                                       |
|--------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB-Hc-Aktion | Ja       | Die Rolle des Zuhörer muss der Parameter **Aktion = verbinden**                                                                                                                                                    |
| {Path}       | Ja       | Der URL-codierte Namespacepfad der vorkonfigurierten in Verbindung mit dieser Zuhörer auf registrieren.                                                                                                               
                                                                                                                                                                                                                                               
                            The path expression MAY be extended with a suffix and a query string expression to communicate further                                                                                                             
                                                                                                                                                                                                                                               
                            If the Hybrid Connection is registered under the path “hyco”, the path expression can be “**hyco/**suffix?param=value&…” followed by the query string parameters defined here. A complete expression may then be:  
                                                                                                                                                                                                                                               
                            *wss://{ns}/**$servicebus**/**hybridconnection/*** **hyco/**suffix?param=value*& sb-hc-action=…&sb-hc-id=…\[&sbc-hc-token=…\]*                                                                                     
                                                                                                                                                                                                                                               
                            The path expression is passed through to the listener in the address URI contained in the “accept” control message.                                                                                                |
| SB-Hc-Token | Yes\*     | Die Zuhörer muss bieten eine gültige, URL-codierte Service Bus freigegeben Access Token für den Namespace oder Hybrid Verbindung aus, die für die Übertragung von rechts **Senden** .                                                            | | SB-Hc-Id | No        | Optionale ID, mit der End-to-End-diagnostic Tracing ermöglicht und ist für die Zuhörer während des Handshakes akzeptieren zur Verfügung gestellt.                                                                                       |

Wenn die Web Sockets-Verbindung aufgrund der Hybrid Verbindung Pfad nicht registriert, oder ein Token ungültige oder fehlende oder ein anderer Fehler fehlschlägt, wird das Fehler Feedback mithilfe des normalen HTTP 1.1 Status Feedback-Modells bereitgestellt werden. Die Beschreibung des Status wird eine Fehler Verlauf-Id enthalten, die zur Unterstützung von Azure kommuniziert werden kann:

| Code | Fehler          | Beschreibung                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Nicht gefunden      | Der **Pfad** des Hybrid Verbindung ist ungültig oder die base-URL ist ungültig |
| 401  | Nicht autorisierte   | Das Sicherheitstoken ist fehlende oder falsch formatierte oder ist ungültig                  |
| 403  | Verboten      | Das Sicherheitstoken gilt nicht für diesen Pfad für diese Aktion          |
| 500  | Interner Fehler | Leider nicht geklappt im Dienst                                    |

Wenn die Web Sockets-Verbindung absichtlich vom Dienst beendet wird, nachdem es anfangs eingerichtet wurde, den Grund für dadurch mithilfe eines entsprechenden Web Sockets Protokoll Fehlercodes sowie eine beschreibende Fehlermeldung angezeigt, die auch eine Verlauf-Id angegeben wird mitgeteilt wird.

| WS Status | Beschreibung                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1000      | Die Zuhörer fahren des Sockets ab.                                                 |
| 1001      | Der Pfad Hybrid Verbindung wurde gelöscht oder deaktiviert wurde.                           |
| 1008      | Das Sicherheitstoken ist abgelaufen, und die Autorisierungsrichtlinie wird daher verletzt. |
| 1011      | Leider nicht geklappt innerhalb der Dienst.                                           |

## <a name="next-steps"></a>Nächste Schritte:

- [Relay häufig gestellte Fragen](relay-faq.md)
- [Erstellen Sie einen namespace](relay-create-namespace-portal.md)
- [Erste Schritte mit .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Erste Schritte mit Knoten](relay-hybrid-connections-node-get-started.md)