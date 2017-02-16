<properties
   pageTitle="Öffentliche und Private DC/OS Agent Pool ACS | Microsoft Azure"
   description="Wie funktionieren die öffentlichen und privaten Agent Pools mit einem Cluster Azure Container Dienst."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Container, Micro-Dienste Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="timlt"/>

# <a name="dcos-agent-pools-for-azure-container-service"></a>DC/OS Agent Pools für Container Azure Service

DC/OS Azure Container Dienst teilt Agents in öffentlichen oder privaten Pools an. Eine Bereitstellung kann entweder Pool Auswirkungen Accessibility zwischen Computern in Ihrem Dienst Container vorgenommen werden. Die Computer können mit dem Internet (öffentlich) verfügbar gemacht oder internen (privat) beibehalten werden. In diesem Artikel bietet einen kurzen Überblick, warum es ein öffentlicher und privater Pool.

### <a name="private-agents"></a>Private-agents

Private Agent-Knoten, die über ein Netzwerk nicht geroutet ausgeführt werden. Dieses Netzwerk ist nur verfügbar, aus der Zone Administrator oder über den vorstehend öffentliche Zone. Standardmäßig startet DC/OS apps auf private Agent-Knoten. Weitere Informationen zur Netzwerk-Sicherheit finden Sie in der [Dokumentation DC/OS](https://dcos.io/docs/1.7/administration/securing-your-cluster/) .

### <a name="public-agents"></a>Öffentliche-agents

Öffentliche Agent Knoten DC/OS apps und Dienste über ein Netzwerk öffentlich zugängliche ausführen. Weitere Informationen zur Netzwerk-Sicherheit finden Sie in der [Dokumentation DC/OS](https://dcos.io/docs/1.7/administration/securing-your-cluster/) .

## <a name="using-agent-pools"></a>Agent Pools verwenden

Standardmäßig bereitstellt **Marathon** jeder neue Anwendung zu den *privaten* Agent-Knoten. Sie müssen die Anwendung in den *öffentlichen* Knoten während der Erstellung der Anwendung explizit bereitstellen. Wählen Sie die Registerkarte **Optional** aus, und geben Sie **Slave_public** für den Wert für die **Ressource Rollen akzeptiert** . Dieses Verfahren wird dokumentierten [hier](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) und in der Dokumentation [DC\OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) .

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie weitere Informationen zum [Verwalten Ihrer DC/OS Container](container-service-mesos-marathon-ui.md).

Erfahren Sie, wie [die Firewall öffnen](container-service-enable-public-access.md) von Azure zum Zulassen öffentlichen Zugriffs auf Ihre DC/OS Container bereitgestellt.