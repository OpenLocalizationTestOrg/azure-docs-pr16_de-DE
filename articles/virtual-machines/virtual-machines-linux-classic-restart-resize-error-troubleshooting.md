<properties
   pageTitle="Virtueller Computer neu zu starten, oder Ändern der Größe Probleme | Microsoft Azure"
   description="Behandeln von Problemen mit klassischen Bereitstellungsprobleme mit dem Neustarten oder Ändern der Größe einer vorhandenen Linux virtuellen Computern in Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="required"
   ms.date="09/20/2016"
   ms.devlang="na"
   ms.author="delhan"/>

# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Behandeln von Problemen mit klassischen Bereitstellungsprobleme mit dem Neustarten oder Ändern der Größe einer vorhandenen Linux virtuellen Computern in Azure

> [AZURE.SELECTOR]
- [Klassische](../articles/virtual-machines/virtual-machines-linux-classic-restart-resize-error-troubleshooting.md)
- [Ressourcenmanager](../articles/virtual-machines/virtual-machines-linux-restart-resize-error-troubleshooting.md)

Wenn Sie versuchen, eine beendet Azure-virtuellen Computern (VM) beginnen, oder Ändern der Größe einer vorhandenen Azure-virtuellen Computer, ist der häufige Fehler, die auftreten eines Fehlers bei. Dieser Fehler führt, wenn das Cluster oder die Region hat keinen verfügbaren Ressourcen auf, oder die angeforderten virtueller Speicher kann nicht unterstützt.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Sammeln der Überwachungsprotokolle

Sammeln Sie zum Starten der Problembehandlung der Überwachungsprotokolle zum Identifizieren des Fehlers das Problem zugeordnet.

Klicken Sie auf **Durchsuchen**, im Portal Azure > **virtuellen Computern** > _Ihrer Linux virtuellen Computern_ > **Einstellungen** > **Überwachungsprotokolle**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problem: Fehler beim Starten eines angehaltenen virtuellen Computers

Sie versuchen, Starten eines angehaltenen virtuellen Computers aber Abrufen eines Fehlers bei.

### <a name="cause"></a>Ursache

Ihre Einladung zum Starten des beendeten virtueller Computer muss bei der ursprünglichen Cluster versucht, die Cloud-Dienst hostet. Der Cluster verfügt jedoch nicht freigeben von Speicherplatz für die Ausführung der Anforderung verfügbar.

### <a name="resolution"></a>Auflösung

* Erstellen eines neuen Cloud-Diensts, und ordnen Sie sie entweder einen Bereich oder eine bereichsbasierte virtuelle Netzwerk, aber nicht aus einer Gruppe Zugehörigkeit.

* Löschen Sie den beendeten virtuellen Computer an.

* Erstellen Sie mithilfe der Datenträger den virtuellen Computer in der neuen Cloud-Dienst neu.

* Starten Sie den neu erstellten virtuellen Computer an.

Wenn Sie eine Fehlermeldung erhalten, bei dem Versuch, einen neuen Clouddienst zu erstellen, zu einem späteren Zeitpunkt wiederholen Sie, oder ändern Sie den Bereich für den Clouddienst.

> [AZURE.IMPORTANT] Neue Cloud-Dienst hat einen neuen Namen und VIP, daher müssen Sie diese Informationen für alle die Abhängigkeiten zu ändern, verwenden diese Informationen für den vorhandenen Clouddienst.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problem: Fehler beim Ändern der Größe von einer vorhandenen virtuellen Computer

Sie versuchen, Ändern der Größe einer vorhandenen virtuellen Computer aber Abrufen eines Fehlers bei.

### <a name="cause"></a>Ursache

Die Anforderung zum Ändern der Größe des virtuellen Computer muss bei der ursprünglichen Cluster versucht, die Cloud-Dienst hostet. Cluster unterstützt jedoch nicht die angeforderten virtueller Speicher.

### <a name="resolution"></a>Auflösung

Verringern der Größe der angeforderten virtuellen Computer, und wiederholen Sie die Anforderung Größe ändern.

* Klicken Sie auf **Alle durchsuchen** > **virtuellen Computern (klassische)** > _des virtuellen Computers_ > **Einstellungen** > **Größe**. Die detaillierten Schritte finden Sie unter [Ändern der Größe des virtuellen Computers](https://msdn.microsoft.com/library/dn168976.aspx).

Wenn es nicht möglich, verringern die Größe virtueller Computer ist, gehen Sie folgendermaßen vor:

  * Erstellen Sie einen neue Cloud-Dienst, um sicherzustellen, ist es nicht zu einer Gruppe Zugehörigkeit verknüpft und nicht zugeordnete ein virtuelles Netzwerk, das mit einer Gruppe Zugehörigkeit verknüpft ist.

  * Erstellen eines neuen, größeres virtuellen Computers.

Sie können Ihre virtuellen Computer in der gleichen Cloud-Dienst konsolidieren. Wenn Ihre vorhandene Cloud-Dienst ein bereichsbasierte virtuelles Netzwerk zugeordnet ist, können Sie den neuen Clouddienst mit dem vorhandenen virtuellen Netzwerk verbinden.

Wenn Sie nicht der vorhandenen Cloud-Dienst ein bereichsbasierte virtuelles Netzwerk zugeordnet ist, müssen Sie die virtuellen Computern in den vorhandenen Clouddienst löschen, und erstellen Sie dann in der neuen Cloud-Dienst aus ihre Laufwerke neu. Es ist jedoch wichtig, denken Sie daran, dass der neuen Cloud-Dienst hat einen neuen Namen und VIP, sodass Sie diese für alle die Abhängigkeiten aktualisieren, die derzeit diese Informationen für den vorhandenen Clouddienst verwenden müssen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie beim Erstellen einer neuen Linux VM in Azure Probleme auftreten, finden Sie unter [Behandeln von Problemen bei der Bereitstellung bei der Erstellung eines neuen Linux virtuellen Computers in Azure](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
