<properties 
    pageTitle="Übersicht über das Ereignis Hubs Authentifizierung und Sicherheit Modell | Microsoft Azure"
    description="Ereignis Hubs Authentifizierung und Sicherheit Übersicht über das Objektmodell."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm;clemensv" />

# <a name="event-hubs-authentication-and-security-model-overview"></a>Ereignis Hubs Authentifizierung und Sicherheit Modell (Übersicht)

Das Ereignis Hubs Sicherheitsmodell erfüllt die folgenden Anforderungen an:

- Nur Geräte, die gültige Anmeldeinformationen präsentieren können Daten an ein Ereignis Verteiler senden.
- Ein Gerät Identität kann nicht an ein anderes Gerät.
- Ein Gerät unerwünschten kann für das Senden von Daten an ein Ereignis Hub blockiert werden.

## <a name="device-authentication"></a>Geräteauthentifizierung

Das Ereignis Hubs Sicherheitsmodell basiert auf einer Kombination von [Freigegebenen Access Signatur (SAS)](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) Token und *Ereignisherausgeber*. Ein Ereignis Publisher definiert einen virtuellen Endpunkt für eine Ereignis-Hub an. Herausgeber kann nur zum Senden von Nachrichten an ein Ereignis Verteiler verwendet werden. Es ist nicht möglich, Nachrichten von einem Herausgeber erhalten.

In der Regel setzt ein Ereignis Hub eines Herausgebers pro Gerät. Alle Nachrichten, die an der Herausgeber von einem Ereignis-Hub gesendet werden sind Warteschlange innerhalb dieser Ereignis-Hub an. Herausgeber aktivieren abgestimmte Access-Steuerelement und Beschränkung.

Jedes Gerät zugeordnet ist ein eindeutiges Token, die auf dem Gerät hochgeladen wird. Die Token hergestellt werden, dass jede eindeutiges Token, das einen anderen eindeutigen Herausgeber Zugriff gewährt. Ein Gerät, das ein Token besitzt kann nur eine Publisher, aber keine anderen Publisher senden. Wenn mehrere Geräte das gleiche Token freigeben möchten, und klicken Sie dann teilt diesen Geräten ein Herausgebers.

Wird zwar nicht empfohlen, ist es möglich, Geräte mit Token ausstatten, die ein Ereignis Hub direkten Zugriff gewähren. Jedem Gerät, das diese token enthält, kann direkt in diesem Ereignis Hub Nachrichten senden. Solche ein Gerät wird nicht unterliegen begrenzungsebene sein. Darüber hinaus kann nicht das Gerät gesperrt werden, an diesem Ereignis Hub zu senden.

Alle Token, die mit einem Schlüssel SAS angemeldet sind. In der Regel werden alle Token mit demselben Schlüssel signiert. Geräte sind nicht die Taste bewusst; Dadurch wird verhindert, dass Geräte Fertigung Token.

### <a name="create-the-sas-key"></a>Erstellen Sie die SAS-Taste

Beim Erstellen eines Ereignisses Hubs Namespaces generiert Azure Ereignis Hubs 256-Bit-SAS Registrierungsschlüssel nach dem **RootManageSharedAccessKey**aus. Dieses wichtigsten gewährt senden, Abhören und Verwalten von Berechtigungen für den Namespace. Sie können zusätzliche Tasten erstellen. Es wird empfohlen, dass Sie einen Schlüssel erstellen, dass gewährt Berechtigungen an bestimmten Ereignis Hub senden. Im weiteren Verlauf dieses Themas, wird davon ausgegangen, dass Sie diese Taste mit dem Namen `EventHubSendKey`.

Im folgenden Beispiel wird einen Schlüssel senden nur Ereignis-Hub zu erstellen:

```
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create Event Hub with a SAS rule that enables sending to that Event Hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Generieren von Token

Sie können mit der SAS-Taste Token generieren. Sie müssen nur über ein Token pro Gerät erstellen. Token können mit dem folgenden Verfahren produziert werden. Alle Token werden mit dem **EventHubSendKey** Schlüssel generiert. Jedes Token, die einen eindeutigen URI zugeordnet ist.

```
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Wenn Sie diese Methode aufrufen, sollte der URI angegeben werden, als `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Für alle Token ist der URI identisch, mit Ausnahme von `PUBLISHER_NAME`, für jedes Token unterschiedlich sein sollten. Idealerweise `PUBLISHER_NAME` stellt die ID des Geräts, das die Token empfängt.

Diese Methode wird ein Token mit der folgenden Struktur erstellt:

```
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

Die token Ablaufzeit in Sekunden seit dem 1. Jan 1970 angegeben. Im folgenden finden ein Beispiel für ein Token:

```
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

In der Regel haben die Token eine Lebensdauer, die ähnelt oder die Lebensdauer des Geräts überschreitet. Wenn das Gerät die Möglichkeit, ein neues Token erhalten hat, können Token mit kürzere Lebensdauer verwendet werden.

### <a name="devices-sending-data"></a>Senden von Daten Geräte

Sobald die Token erstellt wurden, wird jedes Gerät mit einem eigenen eindeutiges Token bereitgestellt.

Wenn das Gerät Daten in einem Ereignis-Hub sendet, tags das Gerät seine Token mit der Anforderung senden. Um zu verhindern, dass einen Angreifer aus Abhören und das Token Diebstahl, muss die Kommunikation zwischen dem Gerät und dem Ereignis Hub über einen verschlüsselten Kanal auftreten.

### <a name="blacklisting-devices"></a>Sperren von Geräten

Wenn ein Token angehängtes gestohlen wird, kann der Angreifer das Gerät Identitätswechsel, deren Token gestohlen wurde. Sperren eines Geräts macht das Gerät unbrauchbar, bis das Gerät ein neues Token erhält, das einen anderen Herausgeber verwendet.

## <a name="authentication-of-back-end-applications"></a>Authentifizierung der Back-End-Anwendungen

Für die Authentifizierung, die die Daten, die auf Geräte nutzen Back-End-Applikationen setzt Ereignis Hubs ein Sicherheitsmodell, das Modell ähnlich ist, Dienstbus Themen verwendet wird. Eine Ereignis Hubs Consumer Gruppe ist ein Abonnement zu einem Thema Dienstbus entspricht. Ein Client kann Consumer Gruppen erstellen, wenn die Anforderung zum Erstellen der Gruppe Consumer ein Token beigefügt ist, dass gewährt Verwalten von Berechtigungen für den Hub Ereignis oder für den Namespace, zu dem das Ereignis Hub gehört. Ein Client kann Daten aus einer Gruppe Consumer nutzen, wenn die Anfrage empfangen ein Token, die Berechtigung empfangen auf dieser Gruppe Consumer, Ereignis-Hub oder den Namespace begleitet wird, zu dem das Ereignis Hub gehört.

Die aktuelle Version von Dienstbus unterstützt keine SAS Regeln für einzelne Abonnements. Dasselbe gilt für Ereignis Hubs Consumer Gruppen. SAS-Unterstützung wird für die beiden Features in der Zukunft hinzugefügt werden.

SAS-Taste können Sie keine der SAS-Authentifizierung für einzelne Consumer Gruppen alle Consumer Gruppen mit einem gemeinsamen Schlüssel sichern. Dieser Ansatz ermöglicht eine Anwendung, die Daten von einem beliebigen Consumer Gruppen von einem Ereignis-Hub zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Ereignis Hubs finden Sie auf den folgenden Themen:

- [Ereignis Hubs (Übersicht)]
- Eine [Lösung für messaging in der Warteschlange] Servicebuswarteschlangen verwenden.
- Eine vollständige [Beispiel-Anwendung, die Ereignis Hubs verwendet].

[Ereignis Hubs (Übersicht)]: event-hubs-overview.md
[Beispiel-Anwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[in der Warteschlange Lösung für messaging]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
