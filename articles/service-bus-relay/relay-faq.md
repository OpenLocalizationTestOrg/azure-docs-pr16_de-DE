<properties 
    pageTitle="Häufig gestellte Fragen zu Relay | Microsoft Azure"
    description="Finden Sie Antworten auf einige häufig gestellte Fragen zur Azure-Relay."
    services="service-bus"
    documentationCenter="na"
    authors="jtaubensee"
    manager=""
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub" />

# <a name="relay-faq"></a>Relay häufig gestellte Fragen

In diesem Artikel finden Sie Antworten auf einige häufig gestellte Fragen zum Microsoft Azure Relay. Sie können auch die [Azure unterstützen häufig gestellte Fragen zu](http://go.microsoft.com/fwlink/?LinkID=185083) allgemeinen Azure Preise und Support-Informationen besuchen. Dazu gehören die folgenden Themen:

- [Allgemeine Fragen zur Azure-Relay](#general-questions)
- [Preise](#pricing)
- [Kontingente](#quotas)
- [Abonnement und Namespace management](#subscription-and-namespace-management)
- [Behandlung von Problemen](#troubleshooting)

## <a name="general-questions"></a>Allgemeine Fragen

### <a name="what-is-azure-relay"></a>Was ist Azure Relay?

[Relay Service](relay-what-is-it.md) ermöglicht das Hosten transparent und WCF-Dienste von überall darauf zugreifen. Kurzum, dadurch Hybrid Applications, die in einer Azure Datacenter und in einer lokalen Enterprise-Umgebung ausgeführt werden.

### <a name="what-is-a-relay-namespace"></a>Was ist ein Relay Namespace?

Einen [Namespace](Relay-create-namespace-portal.md) stellt einen Bereiche Container zum Adressieren Relay Ressourcen innerhalb Ihrer Anwendung bereit. Erstellen eine unbedingt Relay verwenden und den ersten Schritten unter Erste Schritte werden.

## <a name="pricing"></a>Preise

In diesem Abschnitt finden Sie Antworten auf einige häufig gestellte Fragen zu der Struktur Preise Relay. Sie können auch die [Azure-Support-FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) für allgemeine Microsoft Azure Preisinformationen besuchen. Umfassende Informationen zu Relay Preise finden Sie unter [Dienstbus Preise Details](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="how-do-you-charge-for-relay"></a>Wie ansetzen Sie für Relay?

Vollständige Informationen Relay Preise finden Sie [Dienstbus Preise Details][Pricing overview]. Zusätzlich zu den Preisen notiert haben sind Sie zugeordneten Datenübertragung für Ausgang außerhalb des Data Center in Rechnung gestellt in denen die Anwendung bereitgestellt wird.

### <a name="what-usage-of-relay-is-subject-to-data-transfer"></a>Welche Verwendung der Relay Datenübertragung ist?

Relay enthält 5 GB Daten eingehende pro Monat, und Abonnements. Es ist kostenlos zusätzliche Azure eingehende/Ausgang von Relay verwendeten Daten.

Die Daten Gebühr für die Weiterleitung für eingehende von Absendern nur ist, als Relay Listener nicht anfallen eine Belastung Daten. Beispielsweise, wenn Sie 1 GB senden, werden Sie nur für 1 GB Abrechnung, obwohl eine Zuhörer auch 1 GB erhalten und möglicherweise außerhalb des Azure-Datencentern.

### <a name="how-are-relay-hours-calculated"></a>Wie werden Relay Stunden berechnet?

Relay Stunden sind die kumulative Zeitspanne in Rechnung gestellt während der einzelnen Relay "Öffnen" während eines bestimmten Abrechnungszeitraum. Relay ist implizit instanziiert und bei einem bestimmten Namespace geöffnet wird, wenn ein Relay Zuhörer zuerst an diese Adresse eine Verbindung herstellt. Nur, wenn die Adresse der letzten Zuhörer trennt das Relay geschlossen. Daher verbindet Zwecken Abrechnung Relay "Öffnen" ab dem Zeitpunkt als verwendet die erste Relay Zuhörer, zum Zeitpunkt der letzten Relay Zuhörer trennt die Namespace. Zählung ausnehmen Relay als geöffnet, wenn eine oder mehrere Relay Listener mit dessen Adresse Dienstbus verbunden sind.

### <a name="what-if-i-have-more-than-one-listener-connected-to-a-given-relay"></a>Was geschieht, wenn ich mehr als eine Verbindung zu einem angegebenen Relay Zuhörer habe?

In einigen Fällen möglicherweise ein einzelnes Relay mehrere verbundenen haben. Eine Relay gilt als "Öffnen", wenn mindestens ein Relay Zuhörer verbunden ist. Offenes Relay zusätzliche Listener hinzufügen führt weitere Relay Stunden. Die Anzahl der Relay Absender (Clients, die aufrufen oder Senden von Nachrichten an Relays) mit verbundenen Relay auch hat keine Auswirkung auf die Berechnung der Relay Stunden.

### <a name="how-is-the-messages-meter-calculated-for-wcf-relays"></a>Wie wird die Anzeige von Nachrichten für WCF Relays berechnet?

**Dies gilt nur für WCF Relays und kein Hybrid Verbindungen (Kosten)**

Berechenbare Nachrichten werden im Allgemeinen für Relays mit demselben Verfahren für vermittelten Personen (Warteschlangen, Themen und Abonnements) oben beschriebenen berechnet. Es gibt jedoch einige wichtige Unterschiede:

Senden, dass eine Nachricht zu einem Dienst Bus Relay behandelt wird, wie ein "vollständige bis" an die Zuhörer Relay, die senden statt der Relay Service Bus des Empfängers der Nachricht empfängt, gefolgt von einer Übermittlung an die Zuhörer Relay. Daher eine Besprechungsanfrage Antworten Formatvorlage Dienst aufrufen (von bis zu 64 KB) anhand einer Relay Zuhörer führt zu Fehlern im zwei berechenbaren Nachrichten: eine berechenbare Nachricht für die Anfrage und einen berechenbaren Nachricht für die Antwort (die Antwort Voraussetzung ist ebenfalls \<= 64 KB). Dies unterscheidet sich von einer Warteschlange verwenden, um zwischen einem Client und einem Dienst zu vermitteln. In diesem Fall müssten das gleiche Anforderung-Antwort Muster eine Besprechungsanfrage senden an die Warteschlange, gefolgt von einer zum Abrufen aus/Übermittlungs-aus der Warteschlange an den Dienst, gefolgt von einer Antwort senden an eine andere Warteschlange und eine zum Abrufen aus/Übermittlung aus dieser Warteschlange an den Client. Mit dem gleichen (\<= 64 KB) Größe Annahmen in der gesamten, das vermittelte Warteschlange Muster somit ergibt vier berechenbaren Nachrichten doppelt so viele Relay mit demselben Muster implementieren in Rechnung gestellt. Es gibt natürlich Vorteile bei der Verwendung von Warteschlangen dieses Muster, z. B. Zuverlässigkeit erzielen und Abgleich zu laden. Diese Vorteile möglicherweise die zusätzliche Ausgaben Blocksatz

Relays, die geöffnet werden, verwenden die NetTCPRelay WCF-Bindung von Nachrichten nicht als einzelne Nachrichten, sondern als Stream der Daten im System entdeckt gehandhabt. Also nur dem Absender oder den Zuhörer Einblick in den einzelnen die Rahmung haben Nachrichten senden/empfangen mithilfe dieser Bindung. Auf diese Weise für Relays mithilfe der NetTCPRelay Bindung, alle Daten wird behandelt als Stream berechenbare Nachrichten bei der Berechnung. In diesem Fall berechnet Dienstbus die Gesamtmenge Daten gesendet oder empfangen über jede einzelne Relay auf Basis 5 Minuten und Dividieren die Summe von 64 KB sein müssen, um zu bestimmen, die Anzahl der berechenbaren Nachrichten für die betreffende während dieses Zeitraums Relay.

## <a name="quotas"></a>Kontingente

|Speicherkontingent Namen|Bereich|Typ|Verhalten beim überschritten|Wert|
|---|---|---|---|---|
|Gleichzeitige Listenern in einem relay|Entität|Statische|Nachfolgende Anforderungen für zusätzliche Verbindungen abgelehnt, und es wird eine Ausnahme durch den einen Code empfangen werden.|25|
|Gleichzeitige Relay Listener|Gesamte System|Statische|Nachfolgende Anforderungen für zusätzliche Verbindungen abgelehnt, und es wird eine Ausnahme durch den einen Code empfangen werden.|2.000|
|Gleichzeitige Relay Verbindungen pro alle Relay Endpunkte in einem Dienstnamespace|Gesamte System|Statische|-|5.000|
|Relay Endpunkte pro Dienstnamespace|Gesamte System|Statische|-|10.000|
|Nachrichtengröße für [NetOnewayRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.netonewayrelaybinding.aspx) und [NetEventRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.neteventrelaybinding.aspx) weiterleitet.|Gesamte System|Statische|Eingehende Nachrichten, die diese Kontingente überschreiten abgelehnt, und es wird eine Ausnahme durch den einen Code empfangen werden.|64KB
|Nachrichtengröße für [HttpRelayTransportBindingElement](https://msdn.microsoft.com/library/microsoft.servicebus.httprelaytransportbindingelement.aspx) und [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) weiterleitet.|Gesamte System|Statische|-|Unbegrenzte|

### <a name="does-relay-have-any-usage-quotas"></a>Hat Relay alle Verwendung von Kontingenten

Standardmäßig setzt Microsoft-Dienst für alle Cloud ein Aggregat monatliche einsatzkontingent über alle Abonnements eines Kunden berechnet wird. Da wir wissen, dass Sie möglicherweise mehr als diese Grenzwerte müssen, wenden Sie sich Kundendienst zu einem beliebigen Zeitpunkt, damit wir, Ihren Anforderungen wissen und diese Grenzwerte entsprechend anpassen können. Sind die Aggregat Verwendung von Kontingenten für Dienstbus wie folgt:

- 5 Milliarden Nachrichten
- 2 Millionen Relay Stunden

Während wir behalten, führen Sie das Recht ein Kundenkontos zu deaktivieren, das in einem bestimmten Monat seine Verwendung von Kontingenten überschritten hat, wird eine E-mail-Benachrichtigung und mehrere Versuche, um einen Kunden zu kontaktieren, bevor Sie eine Aktion. Diese Kontingente überschreiten Kunden bleibt Gebühren verantwortlich, die die Kontingente überschreiten.

#### <a name="naming-restrictions"></a>Benennen von Einschränkungen

Namen für einen Namespace Relay kann nur zwischen 6-50 Zeichen lang sein.

## <a name="subscription-and-namespace-management"></a>Abonnement und Namespace management

### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Wie migriere ich einen Namespace zu einem anderen Azure-Abonnement?

Sie können PowerShell Befehle verwenden (finden Sie im Artikel [hier](../service-bus-messaging/service-bus-powershell-how-to-provision.md#migrate-a-namespace-to-another-azure-subscription)) einen Namespace aus einer Azure-Abonnement auf einen anderen zu verschieben. Um den Vorgang ausführen zu können, muss der Namespace bereits aktiv sein. Auch muss der Benutzer, die in die Befehle auf sowohl Quell- und Zielwebsites Abonnements als Administrator angemeldet sein.

## <a name="troubleshooting"></a>Behandlung von Problemen

### <a name="what-are-some-of-the-exceptions-generated-by-azure-relay-apis-and-their-suggested-actions"></a>Was sind einige der Ausnahmen von Azure Relay-APIs und ihre vorgeschlagenen Aktionen generiert?

Die [Relay Ausnahmen] [ Relay exceptions] diesem Artikel werden einige Ausnahmen mit vorgeschlagenen Aktionen.

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Was ist mit einer freigegebenen Access-Signatur und welche Sprachen unterstützen Generieren einer Signatur?

Freigegebene Access Signaturen handelt es sich um ein Authentifizierungsmechanismus basierend auf SHA – 256 secure Hashes oder URIs. Informationen dazu, wie Sie Ihre eigenen Signaturen in Knoten, PHP, Java und C generieren\#, finden Sie im Artikel [Access Signaturen freigegeben][] .

[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Relay exceptions]: relay-exceptions.md
[Freigegebene Access-Signaturen]: service-bus-sas-overview.md

## <a name="next-steps"></a>Nächste Schritte:

- [Erstellen Sie einen namespace](relay-create-namespace-portal.md)
- [Erste Schritte mit .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Erste Schritte mit Knoten](relay-hybrid-connections-node-get-started.md)