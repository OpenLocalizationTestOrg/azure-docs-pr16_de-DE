<properties 
    pageTitle="Übersicht Dienstbus messaging Beispiele | Microsoft Azure"
    description="Kategorisiert und Dienstbus Beispiele mit Links zu den einzelnen messaging beschreibt."
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

# <a name="service-bus-messaging-samples"></a>Dienstbus messaging Beispiele

Den messaging Dienstbus-Beispielen wird die wichtigsten Features in [Dienstbus messaging](https://azure.microsoft.com/services/service-bus/) (Cloud-Dienst) und [Service Bus für Windows Server](https://msdn.microsoft.com/library/dn282144.aspx). In diesem Artikel kategorisiert und eine Beschreibung der Beispiele mit Links zu den einzelnen verfügbar.

>[AZURE.NOTE] Dienstbus Beispiele werden nicht mit dem SDK installiert. Um die Beispiele zu erhalten, finden Sie auf der [Seite mit Beispielen Azure SDK](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Darüber hinaus besteht eine aktualisierte Sammlung von Dienstbus messaging Beispiele [hier](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) (derzeit, sie sind nicht in diesem Artikel beschriebenen).  

Beispiele für Relay finden Sie unter [Dienstbus Relay Beispiele](../service-bus-relay/service-bus-relay-samples.md).

## <a name="service-bus-messaging"></a>Dienstbus messaging

In den folgenden Beispielen veranschaulichen, wie Applications zu schreiben, die Dienstbus messaging verwenden.

Beachten Sie, dass die messaging Beispiele eine Verbindungszeichenfolge zum Zugreifen auf Ihre Dienstbus Namespaces erforderlich.

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

### <a name="getting-started-samples"></a>Erste Schritte-Beispiele

In diesen Beispielen werden per Grundfunktionen beschrieben.

|Beispielname|Beschreibung|Minimale SDK-Version|Verfügbarkeit|
|---|---|---|---|
|[Erste Schritte: Messaging mit Warteschlangen](http://code.msdn.microsoft.com/Getting-Started-Brokered-aa7a0ac3)|Veranschaulicht, wie mit Microsoft Azure-Dienstbus zum Senden und Empfangen von Nachrichten aus einer Warteschlange verwenden.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Erste Schritte: Messaging mit Themen](http://code.msdn.microsoft.com/Getting-Started-Brokered-614d42e5)|Veranschaulicht, wie Microsoft Azure-Dienstbus zum Senden und Empfangen von Nachrichten über ein Thema mit mehreren Abonnements.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|

### <a name="exploring-features"></a>Erkunden von Funktionen

Den folgenden Beispielen wird die verschiedene Features von Dienstbus.

|Beispielname|Beschreibung|Minimale SDK-Version|Verfügbarkeit|
|---|---|---|---|
|[HTTP-Token-Anbieter](http://code.msdn.microsoft.com/Service-Bus-HTTP-Token-38f2cfc5)|Veranschaulicht die verschiedenen Methoden zur Authentifizierung eines Clients HTTP/REST mit Dienstbus an.|2.1|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Service Bus HTTP-Client](http://code.msdn.microsoft.com/Service-Bus-HTTP-client-fe7da74a)|Veranschaulicht, wie Nachrichten zu senden und Empfangen von Dienstbus über HTTP (REST /).|2.3|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Service Bus automatische Weiterleitung](http://code.msdn.microsoft.com/Service-Bus-Autoforwarding-b9df470b)|Automatisches Weiterleiten von Nachrichten von einer Warteschlange, Abonnements oder für unzustellbare Nachrichten in einer anderen Warteschlange oder Thema veranschaulicht. Darüber hinaus wird das Senden einer Nachricht in eine Warteschlange oder ein Thema über eine Warteschlange durchstellen veranschaulicht.|2.3|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Vermittelten Messaging: Beispiel für einen WCF-Kanal Sitzung](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-0a526451)|Veranschaulicht, wie Microsoft Azure-Dienstbus Kanäle Windows Communication Foundation (WCF) verwenden. Das Beispiel zeigt die Verwendung von WCF-Kanäle zum Senden und Empfangen von Nachrichten über eine Warteschlange Dienstbus an. Das Beispiel zeigt sowohl Sitzung und nicht Sitzung Kommunikation über die Dienstbus an.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Vermittelte Messaging: Transaktionen.](http://code.msdn.microsoft.com/Brokered-Messaging-8cd41d1e)|Veranschaulicht, wie die Microsoft Azure-Dienstbus messaging-Funktionen in einem Transaktionsbereich akzeptieren, um sicherzustellen, dass NDRs Blattnamen Messaging Vorgänge automatisch.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Vermittelten Messaging: Verwendung von REST Management Vorgänge](http://code.msdn.microsoft.com/Brokered-Messaging-569cff88)|Veranschaulicht, wie auf Verwendung von REST Dienstbus Management-Vorgänge durchführen.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Anbieter für Ressourcen REST-APIs](http://code.msdn.microsoft.com/Service-Bus-Resource-5d887203)|Veranschaulicht, wie die neuen Service Bus RDFE REST APIs Namespaces und messaging Einheiten verwalten.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Vermittelten Messaging: WCF-Dienst Sitzung-Beispiel](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-db4262c2)|Veranschaulicht, wie Microsoft Azure-Dienstbus mithilfe des WCF-Service-Modells. Das Beispiel zeigt die Verwendung von WCF-Service-Modells Sitzung-basierte Kommunikation über eine Dienstbus Warteschlange zufriedenzustellen.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Vermittelten Messaging: Anforderungsantwort](http://code.msdn.microsoft.com/Brokered-Messaging-Request-2b4ff5d8)|Veranschaulicht, wie die Microsoft Azure-Dienstbus und die Anforderung/Antwort-Funktionen nutzen. Das Beispiel zeigt die einfache Clients und Kommunikation über eine Dienstbus Warteschlange-Servern.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Vermittelten Messaging: Warteschlange für unzustellbare](http://code.msdn.microsoft.com/Brokered-Messaging-Dead-22536dd8)|Veranschaulicht, wie mit Microsoft Azure-Dienstbus und die Funktionalität für messaging "Warteschlange für unzustellbare" verwenden. Das Beispiel zeigt eine einfache Absender und Empfänger, die über eine Warteschlange Dienstbus kommunizieren.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Vermittelten Messaging: Zurückgestellte Nachrichten](http://code.msdn.microsoft.com/Brokered-Messaging-ccc4f879)|Veranschaulicht, wie die Nachricht Rückstellung-Funktion von Microsoft Azure-Dienstbus verwenden. Das Beispiel zeigt eine einfache Absender und Empfänger, die über eine Warteschlange Dienstbus kommunizieren.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Vermittelten Messaging: Sitzung Nachrichten](http://code.msdn.microsoft.com/Brokered-Messaging-Session-41c43fb4)|Veranschaulicht, wie mit Microsoft Azure-Dienstbus und die Sitzung Messaging-Funktionen verwenden. Das Beispiel zeigt die einfachen Absender und Empfänger Kommunikation über eine Warteschlange Dienstbus.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Vermittelten Messaging: Anforderung-Antwort-Thema](http://code.msdn.microsoft.com/Brokered-Messaging-Request-6759a36e)|Implementieren das Anforderung/Antwort Muster mit Microsoft Azure-Dienstbus Themen und Abonnements veranschaulicht. Das Beispiel zeigt die einfache Clients und Server, die über ein Thema Dienstbus kommunizieren.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Vermittelten Messaging: Antwortwarteschlange](http://code.msdn.microsoft.com/Brokered-Messaging-Request-0ce8fcaf)|Veranschaulicht, wie Microsoft Azure-Dienstbus und die Anforderung/Antwort-Funktionen nutzen. Das Beispiel zeigt einfache Clients und Kommunikation über zwei Servicebuswarteschlangen-Servern.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Vermittelten Messaging: Erkennung von Duplikaten](http://code.msdn.microsoft.com/Brokered-Messaging-c0acea25)|Veranschaulicht, wie mit Microsoft Azure-Dienstbus doppelter Nachrichten Erkennung mit Warteschlangen verwenden. Erstellt zwei Warteschlangen, mit der Erkennung von Duplikaten aktiviert und andere eine ohne Erkennung von Duplikaten.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Vermittelten Messaging: Asynchronen Messaging](http://code.msdn.microsoft.com/Brokered-Messaging-Async-211c1e74)|Verwenden Sie zum Senden und Empfangen von Nachrichten aus einer Warteschlange asynchrone Microsoft Azure-Dienstbus veranschaulicht. Die Warteschlange bietet Entkoppelter, asynchrone Kommunikation zwischen einem Absender und eine beliebige Anzahl von Empfängern mit (hier, einem einzelnen Empfänger).|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Vermittelten Messaging: Erweiterte Filter](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)|Veranschaulicht, wie mit Microsoft Azure-Dienstbus Veröffentlichen/Abonnieren erweiterte Filter verwenden. Es wird ein Thema und 3 Abonnements mit anderen Filterdefinitionen erstellt, sendet Nachrichten mit dem Thema und empfängt alle Nachrichten von Abonnements.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Vermittelten Messaging: Nachrichten Prefetch](http://code.msdn.microsoft.com/Brokered-Messaging-be2dac1d)|Veranschaulicht, wie die Microsoft Azure-Dienstbus Nachrichten Prefetch-Funktion verwenden. Es veranschaulicht, wie das Nachrichten Prefetch-Feature beim empfangen.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|

## <a name="service-bus-reference-tools"></a>Dienstbus Referenztools

Den folgenden Beispielen wird die verschiedenen anderen Features des Diensts.

|Beispielname|Beschreibung|Minimale SDK-Version|Verfügbarkeit|
|---|---|---|---|
|[Service Bus-Explorer](http://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)|Der Dienst Bus Explorer können Benutzer eine Verbindung mit einem Dienstbus Dienstnamespace und messaging Personen auf einfache Weise verwalten. Das Tool stellt erweiterte Features wie importieren/exportieren Funktionalität und die Möglichkeit, per Einheiten und Relay-Dienste zu testen.|1.8|Microsoft Azure-Dienstbus; Dienstbus für WindowsServer|
|[Autorisierung: SBAzTool](http://code.msdn.microsoft.com/Authorization-SBAzTool-6fd76d93)|In diesem Beispiel wird das Erstellen und Verwalten von Service Identitäten in Microsoft Azure Active Directory Access Control (auch bekannt als Steuerelement-Dienst oder ACS-) für die Verwendung mit Dienstbus veranschaulicht.|N/V|Microsoft Azure-Dienstbus|

## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter den folgenden Themen Übersichten-Dienst.

- [Übersicht über Dienstbus messaging](service-bus-messaging-overview.md)
- [Dienst Busarchitektur](service-bus-architecture.md)
- [Dienstbus-Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
