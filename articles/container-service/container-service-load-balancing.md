<properties
   pageTitle="Laden Saldo Container in einem Cluster Azure Container Dienst | Microsoft Azure"
   description="Lastenausgleich über mehrere Container in einem Cluster Azure Container Dienst."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="DC/OS, Azure Container, Micro-Dienste"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="rogardle"/>

# <a name="load-balance-containers-in-an-azure-container-service-cluster"></a>Laden Sie Saldo Container in einem Cluster Azure Container Service

In diesem Artikel werden wir untersuchen So erstellen Sie eine interne Lastenausgleich in einer eines DC/OS verwaltete Azure Container Dienst Marathon Projektor verwenden. Dies ermöglicht Ihnen Ihre Applikationen horizontal skalieren. Es wird auch der öffentlichen und privaten Agent Cluster nutzen durch Ihre Lastenausgleich auf Öffentliche Cluster und Ihrer Anwendungscontainer auf die private Cluster platzieren Anzeigeberechtigungen.

## <a name="prerequisites"></a>Erforderliche Komponenten

[Bereitstellen einer Instanz von Azure Container Dienst](container-service-deployment.md) mit Orchestrator Typ DC/OS, und [Stellen Sie sicher, dass Ihre Kunden mit Ihren Cluster herstellen kann](container-service-connect.md). 

## <a name="load-balancing"></a>Lastenausgleich

Es gibt zwei Lastenausgleich Ebenen im Container Dienst Cluster, die, den wir erstellen, wird, ein: 

  1. Azure Lastenausgleich bietet öffentlichen Eintrag Punkt (diejenigen, die Endbenutzer erreicht wird). Dies wird automatisch von Azure Container Dienst bereitgestellt und ist, wird standardmäßig so konfiguriert, dass um Port 80, 443 und 8080 verfügbar zu machen.
  2. Lastenausgleich Marathon (Marathon Projektor) leitet eingehende Anfragen Container Instanzen, die diese Anforderungen an. Wie wir die Container unsere Webdienst bereitstellen zu skalieren, passt Marathon Projektor dynamisch an. Diese Lastenausgleich nicht standardmäßig in Ihrem Container-Dienst bereitgestellt, aber es ist sehr einfach zu installieren.

## <a name="marathon-load-balancer"></a>Lastenausgleich Marathon

Lastenausgleich Marathon neu dynamisch basierend auf den Container, die Sie bereitgestellt haben. Es ist auch flexibel in Bezug auf einen Container oder einen Agent - verloren gehen, wenn diese Situation vorliegt und Apache Mesos einfach Sie den Container an anderer Stelle starten werden Marathon Projektor passt.

Zum Installieren web Sie entweder die DC/OS verwenden können Marathon Lastenausgleich Benutzeroberfläche oder die Befehlszeile.

### <a name="install-marathon-lb-using-dcos-web-ui"></a>Installieren von Marathon Projektor mit DC/OS Web-Benutzeroberfläche

  1. Klicken Sie auf 'Universum'
  2. Suchen nach 'Marathon Projektor'
  3. Klicken Sie auf "Installieren"

![Installieren von Marathon Projektor über die DC/OS Web-Oberfläche](./media/dcos/marathon-lb-install.png)

### <a name="install-marathon-lb-using-the-dcos-cli"></a>Verwendung der DC/OS CLI Marathon Projektor installieren

Nach dem Installieren der DC/OS CLI und sicherstellen können Sie zum Cluster, führen den folgenden Befehl aus Ihrem Clientcomputer verbinden:

```bash
dcos package install marathon-lb
```

Dieser Befehl installiert Lastenausgleich automatisch auf der öffentlichen Agents Cluster.

## <a name="deploy-a-load-balanced-web-application"></a>Bereitstellen eine Last verteilt Webanwendung

Nun verfügen wir über das Paket Marathon Projektor, können wir einen Anwendungscontainer bereitstellen, den wir für den Lastenausgleich wünschen. In diesem Beispiel werden wir einen einfachen Webserver bereitstellen, mit der folgenden Konfiguration:

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}

```

  * Legen Sie den Wert der `HAProxy_0_VHOST` auf den vollqualifizierten Domänennamen des Lastenausgleich für Ihre Agents. Dies ist das Formular `<acsName>agents.<region>.cloudapp.azure.com`. Angenommen, Sie erstellen Sie einen Container Dienst Cluster mit Namen `myacs` in der Region `West US`, der vollqualifizierten Domänennamen wäre `myacsagents.westus.cloudapp.azure.com`. Sie können Suchen auch diese wonach Lastenausgleich mit "Agents" in den Namen, wenn Sie über die Ressourcen in der Ressourcengruppe gefunden haben, die Sie für den Container-Dienst in der [Azure-Portal](https://portal.azure.com)erstellt.
  * Festlegen der ServicePort an einen Port > = 10.000. Dies identifiziert den Dienst, die in diesem Container – ausgeführt wird Marathon Projektor verwendet diese Dienste identifiziert, die sie verteilen sollten auf.
  * Festlegen der `HAPROXY_GROUP` Beschriftung "externe".
  * Festlegen von `hostPort` 0. Dies bedeutet, dass Marathon beliebig einen verfügbaren Anschluss zuzuordnen.
  * Festlegen von `instances` auf die Anzahl der Instanzen, die Sie erstellen möchten. Sie können immer diese nach oben oder unten später skalieren.

Lohnt sich Noing standardmäßig Marathon auf private Cluster bereitstellen, dies bedeutet, dass es sich bei die obige Bereitstellung nur wird zugegriffen werden über Ihre Lastenausgleich, es wird in der Regel, die wir wünschen.

### <a name="deploy-using-the-dcos-web-ui"></a>Verwenden der Web-Benutzeroberfläche DC/OS bereitstellen

  1. Finden Sie auf der Seite Marathon am http://localhost/marathon (nach dem Einrichten Ihrer [SSH Tunnel](container-service-connect.md) , und klicken Sie auf`Create Appliction`
  2. In der `New Application` Dialogfeld auf `JSON Mode` in der oberen rechten Ecke
  3. Fügen Sie die oben angegebenen JSON in editor
  4. Klicken Sie auf`Create Appliction`

### <a name="deploy-using-the-dcos-cli"></a>Verwendung der DC/OS CLI bereitstellen

Bereitstellen dieser Anwendung mit der CLI DC/OS kopieren Sie einfach die oben genannten JSON in einer Datei namens `hello-web.json`, und führen Sie:

```bash
dcos marathon app add hello-web.json
```

## <a name="azure-load-balancer"></a>Azure Lastenausgleich

Standardmäßig macht Azure Lastenausgleich Ports 80, 8080 und 443. Wenn Sie verwenden eine der folgenden drei ports (wie wir im obigen Beispiel), und klicken Sie dann liegt kein, die Sie ausführen müssen. Sie sollten Ihre Agent Lastenausgleich FQDN – Treffer zu können, und jedes Mal, wenn Sie aktualisieren, wird drücken, eine der drei Webserver Roundrobin. Wenn Sie einen anderen Anschluss verwenden, müssen Sie jedoch eine Regel Round-Robert und einen Prüfpunkt auf dem Lastenausgleich für den Port hinzufügen, die Sie verwendet. Sie können dazu die [Azure CLI](../xplat-cli-azure-resource-manager.md), wobei die Befehle `azure network lb rule create` und `azure network lb probe create`. Sie können auch dazu das Azure-Portal.


## <a name="additional-scenarios"></a>Zusätzliche Szenarien

Sie könnten ein Szenario haben, in dem Sie verschiedene Domänen verwenden, um verschiedene Dienste verfügbar zu machen. Beispiel:

mydomain1.com -> LB:80 Azure -> Marathon-Lb:10001 -> Mycontainer1:33292  
mydomain2.com -> LB:80 Azure -> Marathon-Lb:10002 -> Mycontainer2:22321

Um dies zu erreichen, schauen Sie sich [virtuelle Hosts](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/), die Möglichkeit, bestimmte Marathon Projektor Pfade Domänen zugeordnet werden soll.

Alternativ können Sie verfügbar machen verschiedene Ports und ordnen Sie diese an den richtigen Dienst hinter Marathon Projektor neu zu. Beispiel:

Azure Lb:80 -> Marathon-Lb:10001 -> Mycontainer:233423  
Azure Lb:8080 -> Marathon-Lb:1002 -> Mycontainer2:33432


## <a name="next-steps"></a>Nächste Schritte

Finden Sie in der Dokumentation DC/OS weitere auf [Marathon Projektor](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/).
