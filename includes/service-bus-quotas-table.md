Die folgende Tabelle enthält Kontingentinformationen zu Dienstbus messaging. Ereignis Hubs Grenzwerte einbezogen werden in dieser Tabelle, aber für Weitere Informationen zum Ereignis Hubs, finden Sie unter [Ereignis Hubs Preise](https://azure.microsoft.com/pricing/details/event-hubs/). Informationen zur Preisgestaltung und andere Kontingente für Dienstbus finden Sie unter Übersicht über die [Service Bus Preise](https://azure.microsoft.com/pricing/details/service-bus/) .

|Speicherkontingent Namen|Bereich|Typ|Verhalten beim überschritten|Wert|
|---|---|---|---|---|
| Maximale Anzahl von Namespaces pro Azure-Abonnement|Namespace|Statische|Weitere Anforderungen zusätzliche Namespaces werden vom Portal abgelehnt.|100|
|Größe der Warteschlange/Thema|Entität|Bei der Erstellung der Warteschlange/Thema definiert.|Eingehende Nachrichten abgelehnt, und es wird eine Ausnahme durch den einen Code empfangen werden.|1, 2, 3, 4 oder 5 GB.<br /><br />Wenn [Partitionierung](service-bus-partitioning.md) aktiviert ist, ist die maximale Warteschlange/Thema Größe 80 GB.|
|Anzahl der aktiven Verbindungen auf einen namespace|Namespace|Statische|Nachfolgende Anforderungen für zusätzliche Verbindungen abgelehnt, und es wird eine Ausnahme durch den einen Code empfangen werden. REST Vorgänge zählen nicht in Richtung aktiven TCP-Verbindungen.|NetMessaging: 1.000<br /><br />AMQP: 5.000|
|Anzahl der aktiven Verbindungen auf eine Entität Warteschlange/Thema/Abonnement|Entität|Statische|Nachfolgende Anforderungen für zusätzliche Verbindungen abgelehnt, und es wird eine Ausnahme durch den einen Code empfangen werden. REST Vorgänge zählen nicht in Richtung aktiven TCP-Verbindungen.|Die Beschränkung der aktiven Verbindungen pro Namespace Pfeilspitze endet.|
|Anzahl gleichzeitiger empfangen Besprechungsanfragen für eine Entität Warteschlange/Thema/Abonnement|Entität|Statische|Erhalten von nachfolgende Anfragen abgelehnt, und es wird eine Ausnahme durch den einen Code empfangen werden. Dieses Kontingent gilt die kombinierte Anzahl gleichzeitiger receive-Operationen über alle Abonnements zu einem Thema.|5.000|
|Anzahl von Themen/Queues pro Dienstnamespace|Gesamte System|Statische|Nachfolgende Anforderungen für die Erstellung einer neuen Thema oder Warteschlange für den Dienstnamespace werden abgelehnt. Daher werden über das [Azure-Portal][]konfiguriert ist, eine Fehlermeldung generiert. Wenn Sie von der Management-API aufgerufen wird, wird eine Ausnahme durch den einen Code empfangen werden.|10.000<br /><br />Die Gesamtzahl der Themen sowie Warteschlangen in einem Dienstnamespace muss kleiner als oder gleich 10.000 sein.<br/>Dies gilt nicht für Premium wie, alle Elemente formatiert sind.|
|Anzahl der partitionierten Themen/Queues pro Dienstnamespace|Gesamte System|Statische|Nachfolgende Anforderungen für die Erstellung einer neuen partitionierten Thema oder Warteschlange für den Dienstnamespace werden abgelehnt. Daher werden über das [Azure-Portal][]konfiguriert ist, eine Fehlermeldung generiert. Wenn Sie von der Management-API aufgerufen wird, wird eine **QuotaExceededException-** Ausnahme durch den einen Code empfangen werden.|Standard- und Standard Ebenen - 100<br />Premium - 1.000<br/><br />Jede partitionierte Warteschlange oder Thema ermittelt in Richtung Kontingents 10.000 Elemente pro Namespace.|
|Maximale Größe von keinem messaging Entität Pfad: Warteschlange oder Thema|Entität|Statische|-|260 Zeichen|
|Maximale Größe von einen beliebigen messaging Entitätsnamen: Namespace, Abonnement, Abonnementregel oder Ereignis-Hub|Entität|Statische|-|50 Zeichen|
|Maximale Größe von Ereignis Hubs Ereignis|Gesamte System|Statische|-|256 KB|
|Nachrichtengröße für eine Person Warteschlange/Thema/Abonnement|Gesamte System|Statische|Eingehende Nachrichten, die diese Kontingente überschreiten abgelehnt, und es wird eine Ausnahme durch den einen Code empfangen werden.|Maximale Nachrichtengröße: 256KB ([Standard Ebene](../articles/service-bus/service-bus-premium-messaging.md)) / 1 MB ([Premium Ebene](../articles/service-bus/service-bus-premium-messaging.md)). <br /><br />**Notiz** Aufgrund von Belastung des Systems wird diese Beschränkung normalerweise etwas weniger.<br /><br />Maximale Kopfzeilengröße: 64KB<br /><br />Maximale Anzahl der Kopfzeile Eigenschaften im Eigenschaftenfeld Sammlung: **Byte/int. MaxValue**<br /><br />Maximale Größe der Eigenschaft in der Eigenschaftensammlung: keine explizite bestehen. Beschränkung durch maximal Kopfzeilengröße.|
|Größe der Nachricht-Eigenschaft für eine Person Warteschlange/Thema/Abonnement|Gesamte System|Statische|Eine Ausnahme **SerializationException** generiert.|Eigenschaft für maximale Größe jeder Eigenschaft ist 32K. Kumulierte Größe aller Eigenschaften darf 64 K nicht überschreiten. Dies gilt für die gesamte Kopfzeile der [BrokeredMessage](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.aspx), die beide hat Benutzereigenschaften als auch Systemeigenschaften (z. B. [SequenceNumber](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.sequencenumber.aspx), [Beschriftung](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.label.aspx), [Nachrichten-ID](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx)usw.).|
|Anzahl von Abonnements pro Thema|Gesamte System|Statische|Erstellen zusätzliche Abonnements nach dem Thema weitere Anforderungen werden abgelehnt. Daher wird, wenn über das Portal konfiguriert ist, eine Fehlermeldung angezeigt werden. Wenn Sie von der Management-API aufgerufen wird eine Ausnahme durch den einen Code empfangen werden.|2.000|
|Anzahl der SQL-Filter pro Thema|Gesamte System|Statische|Nachfolgende Anforderungen für die Erstellung von zusätzliche Filter auf dem Thema abgelehnt, und es wird eine Ausnahme durch den einen Code empfangen werden.|2.000|
|Anzahl der Korrelations-Filter pro Thema|Gesamte System|Statische|Nachfolgende Anforderungen für die Erstellung von zusätzliche Filter auf dem Thema abgelehnt, und es wird eine Ausnahme durch den einen Code empfangen werden.|100,000|
|Größe des SQL-Filter/Aktionen|Gesamte System|Statische|Nachfolgende Anforderungen für die Erstellung von zusätzliche Filter abgelehnt, und es wird eine Ausnahme durch den einen Code empfangen werden.|Maximale Länge der Zeichenfolge der Filters-Bedingung: 1024 (1-Alpha).<br /><br />Maximale Länge der Regel Aktionszeichenfolge: 1024 (1-Alpha).<br /><br />Maximale Anzahl der pro Regelaktion Ausdrücke: 32.|
|Anzahl der [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) Regeln pro Namespace, Warteschlange oder Thema|Entität, namespace|Statische|Nachfolgende Anforderungen für die Erstellung von weiteren Regeln abgelehnt, und es wird eine Ausnahme durch den einen Code empfangen werden.|Maximale Anzahl von Regeln: 12. <br /><br /> Regeln, die auf einen Namespace Dienstbus konfigurierten gelten für alle Warteschlangen und Themen in diesem Namespace.

[Azure-portal]: https://portal.azure.com