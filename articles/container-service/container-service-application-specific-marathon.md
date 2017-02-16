<properties
   pageTitle="Anwendung oder benutzerspezifische Marathon Dienst | Microsoft Azure"
   description="Erstellen einer Anwendung oder benutzerspezifische Marathon service"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Container, Marathon, Micro-Dienste DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>

# <a name="create-an-application-or-user-specific-marathon-service"></a>Erstellen einer Anwendung oder benutzerspezifische Marathon service

Azure Container-Dienst bietet eine Reihe von master-Servern auf denen wir Apache Mesos und Marathon vorkonfiguriert. Diese können verwendet werden, um die Anwendung auf dem Cluster koordinieren, aber es empfiehlt sich nicht, die master-Server für diesen Zweck verwenden. Protokollierung in die master-Server selbst und als Änderungen – beispielsweise erfordert optimiert die Konfiguration des Marathon-Visualisierung eindeutige master-Servern, die ein wenig unterscheiden sich von den standardmäßigen und größere und unabhängig verwaltet werden müssen. Darüber hinaus möglicherweise durch eine Teamwebsite erforderliche Konfiguration nicht die optimale Konfiguration für ein anderes Team.

In diesem Artikel werden wir erläutert, wie eine Anwendung oder benutzerspezifische Marathon Dienst hinzufügen werden.

Da dieser Dienst an einen einzelnen Benutzer oder ein Team angehören wird, kann es sich in keiner Weise konfigurieren, die sie wünschen. Darüber hinaus wird Azure Container Service sichergestellt, dass der Dienst weiterhin ausgeführt. Wenn der Dienst fehlschlägt, starten Azure Container Dienst für Sie. In den meisten Fällen wird nicht Sie auch feststellen, dass es Ausfallzeiten hatte.

## <a name="prerequisites"></a>Erforderliche Komponenten

[Bereitstellen einer Instanz von Azure Container Dienst](container-service-deployment.md) mit Orchestrator Typ DC/OS, und [Stellen Sie sicher, dass Ihre Kunden mit Ihren Cluster herstellen kann](container-service-connect.md). Darüber hinaus kann die folgenden Schritte aus.

[AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>Erstellen einer Anwendung oder benutzerspezifische Marathon service

Beginnen Sie mit der Erstellung einer JSON-Konfigurationsdatei, die den Namen der Anwendungsdienst definiert werden, die Sie erstellen möchten. Hier verwenden wir die `marathon-alice` als Framework Namen. Speichern Sie die Datei als ungefähr wie folgt `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Als Nächstes verwenden Sie die CLI DC/OS, installieren die Instanz Marathon mit den Optionen, die in der Konfigurationsdatei festgelegt werden.

```bash
dcos package install --options=marathon-alice.json marathon
```

Sie sollten jetzt finden Sie unter Ihre `marathon-alice` Dienst, der auf der Registerkarte Dienste der Benutzeroberfläche DC/OS ausgeführt. Kann ich die Benutzeroberfläche `http://<hostname>/service/marathon-alice/` , wenn Sie es direkt zugreifen möchten.

## <a name="set-the-dcos-cli-to-access-the-service"></a>Festlegen der DC/OS CLI den Zugriff auf Dienste

Sie können optional Konfigurieren Ihrer DC/OS CLI Zugriff auf diese neue Dienstleistung durch Festlegen der `marathon.url` -Eigenschaft auf die `marathon-alice` Instanz wie folgt:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Sie können überprüfen, welche Instanz von Marathon, mit denen Ihre CLI gegen arbeitet der `dcos config show` Befehl. Sie können mit dem master Marathon Dienst mit dem Befehl Wiederherstellen `dcos config unset marathon.url`.
