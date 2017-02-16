<properties 
    pageTitle="Dienstbus Relay-Beispiele Übersicht | Microsoft Azure"
    description="Kategorisiert und beschreibt Dienstbus Relay Beispiele mit Links zu den einzelnen."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/07/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-samples"></a>Beispiele für Dienstbus relay

Den Dienstbus Relay Beispielen wird die wichtigsten Features in [Dienstbus Relay](https://azure.microsoft.com/services/service-bus/). In diesem Artikel kategorisiert und eine Beschreibung der Beispiele mit Links zu den einzelnen verfügbar.

>[AZURE.NOTE] Dienstbus Beispiele werden nicht mit dem SDK installiert. Um die Beispiele zu erhalten, finden Sie auf der [Seite mit Beispielen Azure SDK](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Darüber hinaus besteht eine aktualisierte Sammlung von Dienstbus Relay Beispiele [hier](https://github.com/Azure-Samples/azure-servicebus-relay-samples) (derzeit, sie sind nicht in diesem Artikel beschriebenen).  

Beispiele messaging finden Sie unter [Dienstbus messaging Beispiele](../service-bus-messaging/service-bus-samples.md).

## <a name="service-bus-relay"></a>Dienstbus relay

In den folgenden Beispielen veranschaulichen Applications zu schreiben, die verwenden den Relaydienst Dienstbus.

Beachten Sie, dass die Relay Beispiele eine Verbindungszeichenfolge zum Zugreifen auf Ihre Dienstbus Namespaces erforderlich.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>Um eine Verbindungszeichenfolge für Azure Service Bus zu erhalten.

1. Melden Sie sich bei der [Azure-Portal](http://portal.azure.com)an.

1. Klicken Sie in der linken Spalte auf **Dienstbus**.

1. Klicken Sie auf den Namen des Namespaces in der Liste.

3. Klicken Sie in das Blade Namespace auf **Freigegebene-Richtlinien**.

4. Klicken Sie in das Blade **freigegeben-Richtlinien** auf **RootManageSharedAccessKey**.

6. Kopieren Sie die Verbindungszeichenfolge in die Zwischenablage.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>Um eine Verbindungszeichenfolge für Service Bus für Windows Server zu erhalten.

1. Führen Sie das folgende PowerShell-Cmdlet aus:

    ```
    get-sbClientConfiguration
    ```

2. Fügen Sie die Verbindungszeichenfolge in der App für die Stichprobe.

## <a name="service-bus-relay"></a>Dienstbus relay

Beispiele für die das Dienstbus Relay.

### <a name="getting-started"></a>Erste Schritte

|Beispielname|Beschreibung|Minimale SDK-Version|Verfügbarkeit|
|---|---|---|---|
|[Messaging weitergeleitet: Azure](http://code.msdn.microsoft.com/Relayed-Messaging-Windows-0d2cede3)|Veranschaulicht, wie Dienstbus Client und Dienst auf Azure ausgeführt werden. In diesem Beispiel wird die Dienstbus programmgesteuert konfiguriert. Nur Umgebung und Sicherheit Informationen werden in den Dateien gespeichert.|1.8|Microsoft Azure-Dienstbus|
|[Weitergeleitete Messaging Authentifizierung: Freigegeben geheim](http://code.msdn.microsoft.com/Relayed-Messaging-92b04c02)|Veranschaulicht, wie ein Herausgeber Namen und Herausgeber geheim mit Dienstbus authentifizieren.|1.8|Microsoft Azure-Dienstbus|
|[Per Authentifizierung weitergeleitet: WebNoAuth](http://code.msdn.microsoft.com/Relayed-Messaging-a4f0b831)|Veranschaulicht, wie eine HTTP-Webdiensts verfügbar machen, die keine Client-Benutzer-Authentifizierung erforderlich ist.|1.8|Microsoft Azure-Dienstbus|
|[Messaging Bindungen weitergeleitet: WebHttpBehavior](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-a6477ba0)|Veranschaulicht, wie die Bindung **WebHttpRelayBinding** binäre Daten über das Web programming Model zurückgeben.|1.8|Microsoft Azure-Dienstbus|
|[Weitergeleitete Messaging Bindungen: NetTcp weitergeleitet](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-2dec7692)|Veranschaulicht, wie die **NetTcpRelayBinding** Bindung.|1.8|Microsoft Azure-Dienstbus|

### <a name="exploring-features"></a>Erkunden von Funktionen

Beispiele für die verschiedenen Dienstbus Relay-Funktionen.

|Beispielname|Beschreibung|Minimale SDK-Version|Verfügbarkeit|
|---|---|---|---|
|[Weitergeleitete Messaging Authentifizierung: Einfache WebToken](http://code.msdn.microsoft.com/Relayed-Messaging-32c74392)|Veranschaulicht, wie eine einfache Web token Anmeldeinformationen bei Dienstbus authentifizieren. Im Beispiel ähnelt dem Beispiel Echo mit wenigen Änderungen. In diesem Beispiel fügt insbesondere ein Verhalten ServiceHost (Dienst) und Applikationen ChannelFactory (Client) an.|1.8|Microsoft Azure-Dienstbus|
|[Weitergeleitete Nachrichten: Lastenausgleich](http://code.msdn.microsoft.com/Relayed-Messaging-Load-bd76a9f8)|Veranschaulicht, wie Microsoft Azure-Dienstbus zum Weiterleiten von Nachrichten an mehrere Empfänger. Es zeigt mehrere Instanzen eines einfachen Diensts zum Kommunizieren mit einem Client über die **NetTcpRelayBinding** Bindung|1.8|Microsoft Azure-Dienstbus|
|[Weitergeleitete Messaging Bindungen: Netto Ereignis](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-c0176977)|Veranschaulicht, wie die **NetEventRelayBinding** Bindung auf Microsoft Azure-Dienstbus mit.|1.8|Microsoft Azure-Dienstbus|
|[Weitergeleitete Messaging Bindungen: WS2007Http Sitzung](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ef1f1fcb)|Veranschaulicht die Verwendung der **WS2007HttpRelayBinding** Bindung mit zuverlässigen Sitzungen aktiviert. Es wird gezeigt, wie Dienstbus Anmeldeinformationen in der Konfigurationsdatei statt programmgesteuert angeben.|1.8|Microsoft Azure-Dienstbus|
|[Weitergeleitete Messaging Bindungen: WS2007Http MsgSecCertificate](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-f29c9da5)|Veranschaulicht, wie die **WS2007HttpRelayBinding** Bindung mit Nachricht Sicherheit zum Sichern von End-to-End-Nachrichten während weiterhin mit Anforderung der Clients mit Dienstbus authentifizieren.|1.8|Microsoft Azure-Dienstbus|
|[Weitergeleitete Nachrichten: Metadatenaustausch](http://code.msdn.microsoft.com/Relayed-Messaging-Metadata-f122312e)|Veranschaulicht, wie einen Metadaten-Endpunkt verfügbar machen, der die Bindung Relay verwendet. **Metadatenaustausch** wird in den folgenden Relay Bindungen unterstützt: **NetTcpRelayBinding**, **NetOnewayRelayBinding**, **BasicHttpRelayBinding**und **WS2007HttpRelayBinding**.|1.8|Microsoft Azure-Dienstbus|
|[Weitergeleitete Messaging Bindungen: NetTcp direkte](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ca039161)|Konfigurieren die **NetTcpRelayBinding** Bindung, den **Hybrid** Verbindungsmodus zu unterstützen, die zuerst stellt eine weitergeleitete Verbindung her, und falls möglich, um eine direkte Verbindung zwischen einem Client und einem Dienst wechselt automatisch veranschaulicht.|1.8|Microsoft Azure-Dienstbus|
|[Weitergeleitete Messaging Bindungen: NetTcp MsgSec Benutzername](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-30542392)|Veranschaulicht, wie die **NetTcpRelayBinding** Bindung mit Nachricht Sicherheit.|1.8|Microsoft Azure-Dienstbus|
|[Weitergeleitete Messaging Bindungen: Netto unidirektionalen](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-bb5b813a)|Veranschaulicht, wie verfügbar zu machen und einen Endpunkt mithilfe der Bindung **NetOnewayRelayBinding** nutzen.|1.8|Microsoft Azure-Dienstbus|
|[Weitergeleitete Messaging Bindungen: WS2007Http einfach](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-aa4b793a)|Veranschaulicht die Verwendung der **WS2007HttpRelayBinding** Bindung. Es wird einen einfachen Dienst, der kein Sicherheitsoptionen verwendet und erfordert keine Clients authentifiziert veranschaulicht.|1.8|Microsoft Azure-Dienstbus|

## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter den folgenden Themen Übersichten-Dienst.

- [Dienstbus Relay (Übersicht)](service-bus-relay-overview.md)
- [Dienst Busarchitektur](../service-bus-messaging/service-bus-architecture.md)
- [Dienstbus-Grundlagen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)