<properties
   pageTitle="Reverse Proxy Fabric Service | Microsoft Azure"
   description="Verwenden Sie für die Kommunikation mit Microservices von innerhalb und außerhalb der Cluster Service-Fabric reverse proxy"
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/04/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-reverse-proxy"></a>Dienst Fabric Reverse Proxy

Der Dienst Fabric Reverse Proxy ist eine reverse Proxy-Dienst integriert in Struktur, mit dem adressieren Microservices im Cluster Fabric Service, die HTTP-Endpunkte verfügbar machen kann.

## <a name="microservices-communication-model"></a>Microservices Kommunikationsmodell

Microservices Dienst Struktur in der Regel eine Teilmenge der virtuellen Computers im Cluster ausgeführt und können aus einem virtuellen Computer in ein anderes verschiedenen Gründen verschieben. Damit die Endpunkte für Microservices dynamisch ändern können. Das normale Muster für die Kommunikation mit dem Microservice ist die auflösen Schleife unten,

1. Beheben Sie den Speicherort des Diensts zunächst über den Dienst benennen.
2. Verbinden mit dem Dienst.
3. Ermitteln Sie die Ursache der Verbindungsfehler und den Speicherort des Diensts bei Bedarf erneut aufgelöst.

Dieser Vorgang umfasst im Allgemeinen umbrechen clientseitige Kommunikation Bibliotheken in einer Schleife, die Richtlinien für die Auflösung und "Wiederholen" implementiert werden.
Weitere Informationen zu diesem Thema finden Sie unter [Kommunikation mit den Diensten](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-via-sf-reverse-proxy"></a>Kommunikation über AE reverse proxy
Dienst Fabric reverse Proxy für alle Knoten im Cluster ausgeführt wird. Es führt durch den gesamten Dienst, Auflösung im Auftrag eines Kunden und leitet die Clientanforderung weiter. Damit können Clients, die auf dem Cluster nur keine clientseitige HTTP-Kommunikation Bibliotheken Sie Kommunikation mit dem Zieldienst über die AE reverse Proxy lokal auf demselben Knoten ausgeführt.

![Interne Kommunikation][1]

## <a name="reaching-microservices-from-outside-the-cluster"></a>Eingang Microservices von außerhalb des Clusters
Das standardmäßige externe Kommunikationsmodell für Microservices ist **Teilnahme** , jeden Dienst standardmäßig nicht direkt von externen Clients zugegriffen werden. Der [Lastenausgleich Azure](../load-balancer/load-balancer-overview.md) ist ein Netzwerk Begrenzungslinie zwischen Microservices und externe Clients, die führt Netzwerk Adressübersetzung und leitet externe Anfragen zu internen **nun** Endpunkte. Um eine Microservice Endpunkt für externe Clients direkt barrierefrei zu machen, muss Lastenausgleich Azure zuerst Verkehr an jeden Port, der vom Dienst im Cluster konfiguriert sein. Darüber hinaus die meisten Microservices (Einschüchterung dynamische Microservices) nicht auf allen Knoten im Cluster live und können sie zwischen Knoten bei einem Failover verschieben, damit in diesen Fällen Lastenausgleich Azure effektiv den Zielknoten der Replikate ermitteln kann befinden um den Datenverkehr auf weiterleiten.

### <a name="reaching-microservices-via-the-sf-reverse-proxy-from-outside-the-cluster"></a>Eingang Microservices über den reverse Proxy AE von außerhalb der cluster

Anstelle des Diensts für den einzelnen Ports Azure Lastenausgleich konfigurieren, kann nur der AE Reverse Proxy-Port in der Azure-Lastenausgleich konfiguriert sein. Dadurch können Clients außerhalb der Cluster Dienste innerhalb der Cluster über den reverse Proxy ohne eine zusätzliche Konfigurationen erreicht haben.

![Externe Kommunikation][0]

>[AZURE.WARNING] Konfigurieren der reverse Proxy-Port auf dem Lastenausgleich, macht alle micro Dienste im Cluster, die einen http-Endpunkt addressible von außerhalb der Cluster sein verfügbar machen.


## <a name="uri-format-for-addressing-services-via-the-reverse-proxy"></a>Zum Adressieren von Diensten über den reverse Proxy-URI-format

Reverse Proxy verwendet ein bestimmtes URI-Format, um die Servicepartition zu identifizieren, an die eingehende Anfrage weitergeleitet werden sollen:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&Timeout=<timeout_in_seconds>
```

 - **http(s)://:** Der reverse Proxy kann HTTP oder HTTPS-Verkehr akzeptieren konfiguriert werden. Bei HTTPS-Datenverkehr tritt auf, SSL-Beendigung am reverse Proxy. Besprechungsanfragen, die vom reverse Proxy auf Dienste im Cluster weitergeleitet werden, sind über http.
 - **Cluster FQDN | interne IP-Adresse:** Für externe Clients kann der reverse Proxy konfiguriert sein, damit sie über die Cluster-Domäne (z. B. mycluster.eastus.cloudapp.azure.com) erreichbar ist. Standardmäßig, die auf den einzelnen Knoten der reverse Proxy ausgeführt wird, kann also für interne Datenverkehr es auf lokalen Host oder auf eine beliebige IP internen Knoten (z. B. 10.0.0.1) erreicht werden.
 - **Port:** Der Port, der für den reverse Proxy angegeben wurde. Beispiel: 19008.
 - **ServiceInstanceName:** Dies ist der Name des vollständigen bereitgestellte Dienst-Instanz des Diensts Sie versuchen, sans erreichen der "Fabric: /" Farbschema. Um beispielsweise Dienst zu erreichen *Fabric: / Anwendung/Myservice/*, würden Sie *Anwendung/Myservice*verwenden.
 - **Suffix Pfad:** Dies ist der tatsächliche URL-Pfad für den Dienst, dem Sie in eine Verbindung herstellen möchten. Beispiel: *Myapi/Werte/hinzufügen/3*
 - **PartitionKey:** Dies ist für einen partitionierten Dienst der berechnete Partitionsschlüssel der Partition, die Sie erreichen möchten. Beachten Sie, dass dies *nicht* der Partitions-ID-GUID. Für diesen Parameter ist nicht für einzelne Partitionsschemas-Dienste erforderlich.
 - **PartitionKind:** Der Dienst Partitionsschema. Dies kann 'Int64Range' oder 'Mit dem Namen' sein. Für diesen Parameter ist nicht für einzelne Partitionsschemas-Dienste erforderlich.
 - **Timeout:**  Gibt das Timeout für die HTTP-Anforderung vom reverse Proxy zum Dienst für die Clientanfrage erstellt. Der Standardwert hierfür ist 60 Sekunden. Dies ist ein optionaler Parameter.

### <a name="example-usage"></a>Beispiel für Verwendung

Werfen Sie Dienst, beispielsweise **Fabric: / Anwendung/MyService** , öffnet eine HTTP Zuhörer auf den folgenden URL:

```
http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Mit den folgenden Ressourcen:

 - `/index.html`
 - `/api/users/<userId>`

Wenn der Dienst das einzelne Partitionierungsschema verwendet, die *PartitionKey* und *PartitionKind* Zeichenfolge Abfrageparameter sind nicht erforderlich, und der Dienst über das Gateway als erreicht werden kann:

 - Extern:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService`
 - Intern:`http://localhost:19008/MyApp/MyService`

Wenn der Dienst das Partitionierungsschema Uniform Int64 verwendet, müssen die *PartitionKey* und *PartitionKind* Zeichenfolge Abfrageparameter verwendet werden, zu einem Teil der Dienst erreicht haben:

 - Extern:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
 - Intern:`http://localhost:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Um die vom Dienst verfügbar gemachten Ressourcen erreicht haben, platzieren Sie einfach Pfad der Ressource nach dem Dienstnamen in der URL ein:

 - Extern:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
 - Intern:`http://localhost:19008/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

Das Gateway leiten dann diese Anfragen des Diensts-URL:

 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Spezielle Behandlung für das Freigeben von Port Services

Das Gateway-Anwendung versucht, erneut eine Adresse Dienst aufgelöst und wiederholen Sie die Anforderung aus, wenn ein Dienst erreicht werden kann. Dies ist einer der Hauptvorteile des Gateways, wie Client-Code nicht mit einer Auflösung von einem eigenen Dienst implementieren und Beheben von Schleife muss.

Im Allgemeinen bei ein Dienst nicht erreichbar ist bedeutet dies, dass der Dienstinstanz oder Replikat als Teil des normalen Lebenszyklus in einen anderen Knoten verschoben wurde. In diesem Fall kann das Gateway erhalten eine Netzwerk Verbindung zurück, die angibt, dass ein Endpunkt nicht mehr auf die ursprünglich gelöst Adresse geöffnet ist.

Jedoch Replikate oder Dienstinstanzen können freigeben ein Hostprozesses und möglicherweise auch einen Port, wenn Sie von einem HTTP-basierten Webserver gehostet freigeben, einschließlich:

 - [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
 - [ASP.NET Core WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
 - [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

In diesem Fall ist es wahrscheinlich, dass der Webserver steht in der Hostprozess und Beantworten von Besprechungsanfragen, aber die Dienstinstanz gelöst oder Replikat nicht mehr verfügbar ist, auf dem Host ist. In diesem Fall wird das Gateway eine Antwort HTTP 404 vom Webserver erhalten. HTTP 404 weist daher zwei unterschiedlichen Bedeutung:

 1. Die Serviceadresse korrekt ist, aber die vom Benutzer angeforderten Ressource ist nicht vorhanden.
 2. Die Adresse der Dienst ist falsch und die Ressource, die vom Benutzer angeforderten möglicherweise tatsächlich auf einen anderen Knoten vorhanden.

Im ersten Fall ist dies eine normale HTTP 404, die einen Benutzerfehler betrachtet. Im zweiten Fall, der Benutzer hat angefordert, eine Ressource, die vorhanden ist, jedoch das Gateway wurde nicht finden, da der Dienst selbst in diesem Fall muss das Gateway lösen erneut auf die Adresse, und versuchen Sie es erneut verschoben wurde, kann.

Das Gateway benötigt daher eine Möglichkeit diesen beiden Fällen unterscheiden. Damit diese Unterscheidung zu machen, ist ein Hinweis vom Server erforderlich.

 - Standardmäßig die Anwendungsgateway wird davon ausgegangen Fall #2 und versucht, erneut aufgelöst und Emission erneut auf die Anfrage.
 - Um Fall #1 mit dem Gateway Anwendung anzugeben, sollte der Dienst den folgenden HTTP-Antwort-Header zurück:

`X-ServiceFabric : ResourceNotFound`

HTTP-Antwortheader gibt eine normale HTTP 404 Situation in der die angeforderte Ressource ist nicht vorhanden, und das Gateway keinen Versuch, die Adresse Dienst erneut zu beheben.

## <a name="setup-and-configuration"></a>Installation und Konfiguration
Der Dienst Fabric Reverse Proxy kann für den Cluster über die [Vorlage Azure Ressourcenmanager](./service-fabric-cluster-creation-via-arm.md)aktiviert werden.

Nachdem Sie die Vorlage für den Cluster haben, die Sie bereitstellen möchten (entweder über die Beispielvorlagen oder durch Erstellen einer benutzerdefinierten Ressource-Manager Vorlage) kann Reverse Proxy in der Vorlage aktiviert werden, indem Sie die folgenden Schritte aus.

1. Definieren Sie einen Port für den reverse Proxy im [Abschnitt Parameter](../resource-group-authoring-templates.md) der Vorlage ein.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19008,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Geben Sie den Port für jede der Nodetype Objekte im **Cluster** [im Abschnitt Typ von Ressourcen](../resource-group-authoring-templates.md)

    Für ApiVersion vor ' 2016-09-01' der Port wird durch den Parameter identifiziert ***HttpApplicationGatewayEndpointPort*** benennen

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "httpApplicationGatewayEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

    Für ApiVersion des am oder nach dem ' 2016-09-01' der Port wird durch den Parameter identifiziert ***ReverseProxyEndpointPort*** benennen

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

3. Zur Behebung des reverse Proxys von außerhalb der Azure Cluster die Einrichtung die **Azure laden Lastenausgleich Regeln** für den Port angegeben, die in Schritt 1 aus.

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. Fügen Sie das Zertifikat auf die Eigenschaft HttpApplicationGatewayCertificate im **Cluster** [im Abschnitt Typ von Ressourcen](../resource-group-authoring-templates.md) zum Konfigurieren SSL-Zertifikate für den Port für den Reverse Proxy hinzu

    Für ApiVersion vor ' 2016-09-01' das Zertifikat wird durch den Parameter identifiziert ***HttpApplicationGatewayCertificate*** benennen

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "httpApplicationGatewayCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```
    Für ApiVersion des am oder nach dem ' 2016-09-01' das Zertifikat wird durch den Parameter identifiziert ***ReverseProxyCertificate*** benennen
    
    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

## <a name="next-steps"></a>Nächste Schritte
 - Sehen Sie ein Beispiel für HTTP-Kommunikation zwischen Dienste in einem [Projekt auf GitHUb Beispiel](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Remoteprozeduraufruf Anrufe mit zuverlässigen Services Remote](service-fabric-reliable-services-communication-remoting.md)

 - [Web-API, OWIN zuverlässigen Dienste verwendet.](service-fabric-reliable-services-communication-webapi.md)

 - [WCF-Kommunikation mithilfe von zuverlässigen Diensten](service-fabric-reliable-services-communication-wcf.md)


[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
