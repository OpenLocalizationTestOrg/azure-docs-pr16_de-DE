<properties
   pageTitle="Verbinden und kommunizieren mit den Diensten in Azure Service Architektur | Microsoft Azure"
   description="Informationen Sie zum beheben, verbinden und kommunizieren mit den Diensten Service-Struktur."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="msfussell"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="connect-and-communicate-with-services-in-service-fabric"></a>Verbinden und kommunizieren mit den Diensten Dienst Struktur
Dienst Struktur führt einen Dienst irgendwo in einem Dienst Fabric Cluster, in der Regel in mehreren virtuellen Computern verteilt. Sie können an einem Ort, vom Dienstbesitzer oder automatisch vom Dienst Fabric verschoben werden. Services sind nicht statisch an einem bestimmten Arbeitsplatz oder Adresse gebunden.
 
Eine Fabric Service-Anwendung umfasst Regel vielen verschiedenen Diensten, wobei jeden Dienst einen bestimmten Vorgang ausführt. Diese Dienste möglicherweise eine vollständige-Funktion, wie verschiedene Teile einer Webanwendung Rendern bilden kommunizieren. Es gibt auch Clientanwendungen, die Herstellen einer Verbindung mit und mit den Diensten kommunizieren. Dieses Dokument beschreibt zum Einrichten der Kommunikation mit und zwischen Ihrer Dienste Service-Struktur.

## <a name="bring-your-own-protocol"></a>Schalten Sie Ihre eigenen Protokoll
Erleichtert nicht Entscheidungen zu Möglichkeiten, die Ihrer Dienste, aber Fabric Service hilft des Lebenszyklus von Ihrer Dienste verwalten. Dies umfasst die Kommunikation. Wenn Ihr Dienst von Fabric Service geöffnet wird, ist, des Diensts Verkaufschance zum Einrichten von außen liegenden Tabellenblättern für eingehenden Anfragen, verwenden jeden gewünschten Protokoll oder Kommunikation-Stapel. Wird der Dienst Abhören auf eine normale **nun** Adresse mit einem beliebigen adressieren scheme, wie etwa einen URI. Mehrere Dienstinstanzen oder Replikate möglicherweise ein Hostprozesses freigeben in diesem Fall wird entweder benötigten verschiedene Ports oder verwendet einen Port Freigabe bereit, wie der HTTP-Kerneltreiber unter Windows. In beiden Fällen muss jedes Services-Instanz oder Replikat in einem Hostprozess eindeutig adressiert werden.

![Endpunkte][1]

## <a name="service-discovery-and-resolution"></a>Dienst Suche und Auflösung
In einem verteilten System möglicherweise Dienste von einem Computer zu einem anderen über einen Zeitraum verschieben. Dies kann verschiedene Ursachen haben, z. B. Ressource Lastenausgleich, Upgrades, Failovers oder Skalierung auftreten. Dies bedeutet, dass Dienst Endpunktadressen ändern, wie der Dienst auf Knoten mit unterschiedlichen IP-Adressen verschiebt und auf verschiedene Ports öffnen kann, wenn der Dienst einen dynamisch ausgewählten Anschluss verwendet.

![Verteilung der Dienste][7]

Fabric-Dienst bietet einen Suche und mit einer Auflösung von DNS-Dienst bezeichnet. Der DNS-Dienst unterhält, dass eine Tabelle, die zugeordnet Dienstinstanzen an den Endpunktadressen mit dem Namen, den sie abhören. Alle Dienstinstanzen von benannten Dienst Struktur haben eindeutige Namen Jahreszahlen URIs, z. B. `"fabric:/MyApplication/MyService"`. Der Namen des Diensts ändert sich nicht im Laufe des Diensts, sondern nur die Endpunktadressen, die beim Verschieben von Diensten zu ändern. Dies ist vergleichbar mit Websites, die Konstante URLs haben, aber in dem die IP-Adresse ändern kann. Und DNS im Internet, auf der Website-URLs in IP-Adressen aufgelöst wird ähnlich wie, Dienst Fabric hat eine Registrierungsstelle, die die Endpunktadresse Dienstnamen zugeordnet ist.

![Endpunkte][2]

Beseitigen und Herstellen einer Verbindung mit Services umfasst die folgenden Schritte aus, die in einer Schleife ausführen:

* **Beheben**: Abrufen den Endpunkt, der ein Dienst veröffentlicht wurde vom Dienst benennen.

* **Verbinden**: Verbinden auf den Dienst über welchen auf diesen Endpunkt verwendet Protokoll.

* **Wiederholen**: ein Verbindungsversuch kann ein Fehler auftreten für verschiedene Gründe, für Beispiel, wenn der Dienst verschoben wurde seit der letzten Berechnung die Endpunktadresse gelöst wurde. In diesem Fall der vorangehenden zu beheben, und verbinden Sie die Schritte müssen wiederholt werden, und diese wird wiederholt, bis die Verbindung hergestellt wurde.

## <a name="connections-from-external-clients"></a>Verbindungen aus externen clients

Herstellen einer Verbindung mit miteinander in einem Cluster im Allgemeinen Services können die Endpunkte eines anderen Diensten direkt zugreifen, da die Knoten in einem Cluster normalerweise auf demselben lokalen Netzwerk befinden. In einigen Umgebungen jedoch ein Cluster hinter einem Lastenausgleich möglicherweise, die externen eingehende Verkehr über eine begrenzte Anzahl von Ports weiterleitet. In diesen Fällen Services können weiterhin miteinander kommunizieren und Beheben von Adressen, die der DNS-Dienst verwenden, aber zusätzliche Schritte unternommen werden müssen, damit externe Clients mit Webdiensten herstellen können.

## <a name="service-fabric-in-azure"></a>Dienst Fabric in Azure

Ein Dienst Fabric Cluster in Azure wird hinter einem Lastenausgleich Azure platziert. Alle externer Datenverkehr zum Cluster muss über den Lastenausgleich übergeben. Der laden Lastenausgleich wird automatisch Weiterleitung von Datenverkehr eingehende einen bestimmten Anschluss auf einen zufällig ausgewählten *Knoten* , die denselben Port geöffnet hat. Lastenausgleich Azure nur kennt Ports öffnen auf den *Knoten*, es nicht zu Ports Öffnen durch einzelne *Services*weiß. 

![Azure Lastenausgleich und Dienst Fabric Suchtopologie][3]

Damit externen Verkehr von Port **80**annehmen möchten, müssen die folgenden Elemente beispielsweise konfiguriert sein:

1. Schreiben Sie einem Dienst im Anschluss 80 überwacht. Konfigurieren Sie der Port 80 des Diensts ServiceManifest.xml, und öffnen Sie eine Zuhörer im Dienst, beispielsweise einem lokal gehosteten Webserver.
 
    ```xml
    <Resources>
        <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>
    ```
    ```csharp
        class HttpCommunicationListener : ICommunicationListener
        {
            ...
            
            public Task<string> OpenAsync(CancellationToken cancellationToken)
            {
                EndpointResourceDescription endpoint = 
                    serviceContext.CodePackageActivationContext.GetEndpoint("WebEndpoint");

                string uriPrefix = $"{endpoint.Protocol}://+:{endpoint.Port}/myapp/";

                this.httpListener = new HttpListener();
                this.httpListener.Prefixes.Add(uriPrefix);
                this.httpListener.Start();

                string uriPublished = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

                return Task.FromResult(this.publishUri);
            }
            
            ...
        }
        
        class WebService : StatelessService
        {
            ...
            
            protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
            {
                return new[] {new ServiceInstanceListener(context => new HttpCommunicationListener(context))};
            }
            
            ...
        }
    ```
  
2. Erstellen Sie einen Dienst Fabric Cluster in Azure ein, und geben Sie Port **80** als einen benutzerdefinierten Endpunktport für den Knoten ein, der den Dienst gehostet wird. Wenn Sie mehr als eine Knoten haben, können Sie Einrichten einer *Position Einschränkung* Dienst um sicherzustellen, dass er nur den Knotentyp ausgeführt wird, die den benutzerdefinierte Endpunktport geöffnet wurde.

    ![Öffnen Sie einen Port für einen Knotentyp][4]

3. Nachdem der Cluster erstellt wurde, konfigurieren Sie Azure Lastenausgleich in der Clusters Ressourcengruppe für die Weiterleitung von Datenverkehr an Port 80. Cluster werden mithilfe der Azure-Portal zu erstellen, wird diese automatisch für jeden Anschluss benutzerdefinierte Endpunkt eingerichtet, die so konfiguriert wurde.

    ![Den Datenverkehr in Azure Lastenausgleich weiterleiten][5]

4. Lastenausgleich Azure verwendet einen Prüfpunkt, um festzustellen, ob er Datenverkehr an einen bestimmten Knoten zu senden. Der Prüfpunkt wird regelmäßig einen Endpunkt auf den einzelnen Knoten festzustellen, ob der Knoten reagiert ist oder nicht. Wenn der Prüfpunkt nach einer konfigurierten Anzahl an, wie oft die Beantwortung fehlschlägt, reagiert Lastenausgleich Datenverkehr an diesen Knoten zu senden. Cluster werden mithilfe der Azure-Portal zu erstellen, wird ein Prüfpunkt automatisch für jeden Anschluss benutzerdefinierte Endpunkt eingerichtet, die so konfiguriert wurde.

    ![Weiterleiten von Datenverkehr in Azure Lastenausgleich][8]

Es ist wichtig, denken Sie daran, den Lastenausgleich Azure als auch der Prüfpunkt nur die *Knoten*, nicht die *Dienste* auf den Knoten ausgeführte kennen. Lastenausgleich Azure sendet immer Datenverkehr an Knoten, die dem Prüfpunkt, reagieren, ist Vorsicht müssen um sicherzustellen, dass die Dienste auf den Knoten verfügbar sind, auf der Prüfpunkt reagieren.

## <a name="built-in-communication-api-options"></a>Integrierte Kommunikationsoptionen API
Das zuverlässigen Services Framework im Lieferumfang von mehreren vordefinierten Kommunikationsoptionen. Die Entscheidung darüber, die welche einen für Sie am besten geeignet sind, hängt von der Auswahl von Modell der Programmierung, im Rahmen der Kommunikation und die Programmiersprache, die in Ihrer Dienste geschrieben werden.

* **Kein bestimmtes Protokoll:**  Wenn Sie keine bestimmte Wahl der Kommunikationsframework, aber Sie etwas von erhalten möchten und schnell ausgeführt werden, dann ist die ideale Option für Sie [Remote Service](service-fabric-reliable-services-communication-remoting.md), wodurch stark-eingegebene Anrufe Remoteprozeduraufruf für zuverlässigen Diensten und zuverlässigen Akteuren dar. Dies ist die einfachste und schnellste Möglichkeit dar, erste Schritte mit Service-Kommunikation. Dienst Remote übernimmt die Auflösung von Adressen Service, Verbindung, wiederholen und Fehlerbehandlung. Beachten Sie, dass dieser Dienst Remote nur c# Applications zur Verfügung steht.

* **HTTP**: zur Kommunikation von Sprache-unabhängige HTTP bietet eine Auswahl Industriestandard mit Tools und HTTP-Servern in vielen verschiedenen Sprachen, alle unterstützten vom Dienst Fabric verfügbar. Webdienste können alle HTTP-Stapel verfügbar, einschließlich der [ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md)verwenden. Geschrieben in c# Kunden nutzen können die [ `ICommunicationClient` und `ServicePartitionClient` Klassen](service-fabric-reliable-services-communication.md) für Dienst Auflösung, HTTP-Verbindungen und Schleifen "Wiederholen".

* **WCF**: Wenn Sie vorhandenen Code, WCF als Ihre Kommunikationsframework verwendet, haben, können die `WcfCommunicationListener` für die serverseitige und `WcfCommunicationClient` und `ServicePartitionClient` Klassen für den Client. Weitere Informationen hierzu finden Sie unter in diesem Artikel über [WCF-basierten Implementierung von den Kommunikationsstapel](service-fabric-reliable-services-communication-wcf.md).

## <a name="using-custom-protocols-and-other-communication-frameworks"></a>Verwenden von benutzerdefinierten Protokolle und andere Kommunikation Framework
Services können beliebiger Protokoll oder Framework für die Kommunikation, ob sie ein benutzerdefiniertes binäre Protokoll über TCP-Sockets oder streaming Ereignisse bis [Azure Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/) oder [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)befindet. Fabric-Dienst bietet Kommunikation APIs, die Sie Ihre Kommunikationsstapel in, stecken können während die Arbeit zu ermitteln, und verbinden Sie alle von Ihnen abstrahiert werden. In diesem Artikel zum [zuverlässigen Service Kommunikationsmodell](service-fabric-reliable-services-communication.md) Weitere Informationen hierzu finden Sie unter.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Konzepte und APIs zur Verfügung, in dem [zuverlässigen Services Kommunikationsmodell](service-fabric-reliable-services-communication.md), und klicken Sie dann schneller Einstieg in [Service Remote](service-fabric-reliable-services-communication-remoting.md) , oder wechseln Sie ausführliche Informationen zum Schreiben einer Kommunikation Zuhörer [Web-API mit OWIN Self-Hosting](service-fabric-reliable-services-communication-webapi.md)verwenden.

[1]: ./media/service-fabric-connect-and-communicate-with-services/serviceendpoints.png
[2]: ./media/service-fabric-connect-and-communicate-with-services/namingservice.png
[3]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancertopology.png
[4]: ./media/service-fabric-connect-and-communicate-with-services/nodeport.png
[5]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerport.png
[7]: ./media/service-fabric-connect-and-communicate-with-services/distributedservices.png
[8]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerprobe.png
