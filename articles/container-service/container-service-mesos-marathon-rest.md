<properties
   pageTitle="Azure Container Container Servicemanagement über die REST-API | Microsoft Azure"
   description="Über die Marathon REST-API bereitstellen Sie Container zu einem Cluster Azure Container Dienst Mesos."
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Container, Micro-Dienste Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-rest-api"></a>Container Management durch die REST-API

DC/OS bietet es sich um eine Umgebung für die Bereitstellung und gruppierte Auslastung, während die zugrunde liegenden Hardware zu skalieren. Auf DC/OS ist es ein Framework, das Planen und Ausführen von berechnen Auslastung verwaltet.

Zwar Framework für viele beliebte Auslastung verfügbar sind, beschreibt dieses Dokument, wie Sie erstellen und Container Bereitstellungen mit Marathon skalieren. Vor dem Durcharbeiten in diesen Beispielen benötigen Sie einen DC/OS Cluster, der in Azure Container Dienst konfiguriert ist. Sie müssen außerdem remote Connectivity mit diesem Cluster haben. Weitere Informationen zu diesen Elementen finden Sie unter den folgenden Artikeln:

- [Bereitstellen eines Azure Container Dienst Clusters](container-service-deployment.md)
- [Herstellen einer Verbindung mit einer Azure Container Dienst cluster](container-service-connect.md)

Nachdem Sie mit dem Azure Container Dienst Cluster verbunden sind, können Sie die DC/OS und verwandte REST-APIs über Http://localhost:local-Anschluss zugreifen. Die Beispiele in diesem Dokument wird davon ausgegangen, dass Sie auf Port 80 Tunnel sind. Beispielsweise der Endpunkt Marathon erreichbar `http://localhost/marathon/v2/`. Weitere Informationen zu den verschiedenen APIs finden Sie unter der Dokumentation Mesosphere für die [Marathon-API](https://mesosphere.github.io/marathon/docs/rest-api.html) und die [Chronos-API](https://mesos.github.io/chronos/docs/api.html)und der Apache Dokumentation zur [Mesos Scheduler-API](http://mesos.apache.org/documentation/latest/scheduler-http-api/).

## <a name="gather-information-from-dcos-and-marathon"></a>Sammeln Sie Informationen in DC/OS und Marathon

Bevor Sie mit dem DC/OS Cluster Container bereitstellen, sammeln Sie einige Informationen zu den DC/OS Cluster, beispielsweise die Namen und den aktuellen Status der DC/OS Agents. Passen Sie dazu die Abfrage die `master/slaves` Endpunkt der DC/OS REST-API. Wenn alles gut geht, sehen Sie eine Liste der DC/OS-Agents und verschiedene Eigenschaften für jede.

```bash
curl http://localhost/mesos/master/slaves
```

Verwenden Sie nun die Marathon `/apps` Endpunkt aktuellen Anwendung Bereitstellungen mit dem DC/OS Cluster überprüfen. Wenn es sich um einen neuen Cluster handelt, wird ein leeres Array für apps angezeigt.

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Bereitstellen eines Containers Docker formatierten

Sie bereitstellen Docker formatierten Container bis Marathon mithilfe einer JSON-Datei, die die beabsichtigte Bereitstellung beschrieben. Im folgende Beispiel wird den Nginx Container, Bindung Port 80 des DC/OS-Agents an Port 80 des Containers bereitstellen. Beachten Sie auch, dass die Eigenschaft 'AcceptedResourceRoles' auf 'Slave_public' festgelegt ist. Dies wird den Container Agent öffentlich zugänglichen Agent Maßstab festlegen bereitgestellt.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Um einen Container Docker formatierten bereitstellen, erstellen Sie JSON-Datei, oder verwenden Sie die bereitgestellten bei [Azure Container Service Demo](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json)Beispiel. Speichern Sie es in einem zugänglichen Speicherort aus. Weiter, um den Container bereitzustellen, führen Sie den folgenden Befehl ein. Geben Sie den Namen der JSON-Datei ein.

```
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

Die Ausgabe ist ähnlich wie der folgende aus:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Wenn Sie für Applikationen Marathon Abfragen, wird dieser neue Anwendung nun in der Ausgabe angezeigt.

```
curl localhost/marathon/v2/apps
```

## <a name="scale-your-containers"></a>Ihre Container skalieren

Sie können Sie auch die Marathon-API zu skalieren oder zu skalieren in Anwendung Bereitstellungen. Im vorherigen Beispiel bereitgestellt Sie eine Instanz der Anwendung. Lassen Sie uns skalieren diese sofort zu drei Instanzen einer Anwendung. Hierzu erstellen Sie eine JSON-Datei mithilfe des folgenden JSON-Texts, und in einem zugänglichen Speicherort speichern.

```json
{ "instances": 3 }
```

Führen Sie den folgenden Befehl aus, um die Anwendung zu skalieren.

>[AZURE.NOTE] Der URI werden http://localhost/marathon/v2/apps/, und klicken Sie dann die ID der Anwendung zu skalieren. Wenn Sie das Beispiel-Nginx verwenden, das der URI http://localhost/marathon/v2/apps/nginx wäre hier bereitgestellt wird.

```json
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Schließlich Abfrage den Endpunkt Marathon für Applikationen. Sie sehen, dass es jetzt drei der Nginx Container.

```
curl localhost/marathon/v2/apps
```

## <a name="use-powershell-for-this-exercise-marathon-rest-api-interaction-with-powershell"></a>Verwenden Sie für diese Übung PowerShell: Marathon REST-API Interaktion mit PowerShell

Sie können diese dieselben Aktionen ausführen, mithilfe von PowerShell-Befehlen auf einem Windows-System.

Führen Sie den folgenden Befehl zum Sammeln von Informationen zu den DC/OS Cluster, wie z. B. Agent und Agent-Status.

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Sie bereitstellen Docker formatierten Container bis Marathon mithilfe einer JSON-Datei, die die beabsichtigte Bereitstellung beschrieben. Im folgende Beispiel wird den Nginx Container, Bindung Port 80 des DC/OS-Agents an Port 80 des Containers bereitstellen.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Erstellen Sie eigener JSON-Datei, oder verwenden Sie die bereitgestellten bei [Azure Container Service Demo](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json)Beispiel. Speichern Sie es in einem zugänglichen Speicherort aus. Weiter, um den Container bereitzustellen, führen Sie den folgenden Befehl ein. Geben Sie den Namen der JSON-Datei ein.

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Sie können Sie auch die Marathon-API zu skalieren oder zu skalieren in Anwendung Bereitstellungen. Im vorherigen Beispiel bereitgestellt Sie eine Instanz der Anwendung. Lassen Sie uns skalieren diese sofort zu drei Instanzen einer Anwendung. Hierzu erstellen Sie eine JSON-Datei mithilfe des folgenden JSON-Texts, und in einem zugänglichen Speicherort speichern.

```json
{ "instances": 3 }
```

Führen Sie den folgenden Befehl aus, um die Anwendung zu skalieren.

> [AZURE.NOTE] Der URI werden http://localhost/marathon/v2/apps/, und klicken Sie dann die ID der Anwendung zu skalieren. Wenn Sie das bereitgestellte Nginx Beispiel hier verwenden, würde der URI http://localhost/marathon/v2/apps/nginx sein.

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen finden Sie über die Mesos HTTP-Endpunkte]( http://mesos.apache.org/documentation/latest/endpoints/).
- [Weitere Informationen finden Sie über die Marathon REST-API]( https://mesosphere.github.io/marathon/docs/rest-api.html).
