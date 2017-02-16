<properties
   pageTitle="Virtueller Computer neu zu starten, oder Ändern der Größe Probleme | Microsoft Azure"
   description="Behandeln von Problemen mit Ressourcenmanager Bereitstellungsprobleme mit dem Neustarten oder Ändern der Größe einer vorhandenen Linux virtuellen Computern in Azure"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.devlang="na"
   ms.workload="required"
   ms.date="09/09/2016"
   ms.author="delhan"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Behandeln von Problemen mit Ressourcenmanager Bereitstellungsprobleme mit dem Neustarten oder Ändern der Größe einer vorhandenen Linux virtuellen Computern in Azure

Wenn Sie versuchen, eine beendet Azure-virtuellen Computern (VM) beginnen, oder Ändern der Größe einer vorhandenen Azure-virtuellen Computer, ist der häufige Fehler, die auftreten eines Fehlers bei. Dieser Fehler führt, wenn das Cluster oder die Region hat keinen verfügbaren Ressourcen auf, oder die angeforderten virtueller Speicher kann nicht unterstützt.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Sammeln der Überwachungsprotokolle

Sammeln Sie zum Starten der Problembehandlung der Überwachungsprotokolle zum Identifizieren des Fehlers das Problem zugeordnet. Die folgenden Links enthalten ausführliche Informationen zu den Prozess:

[Problembehandlung bei Ressource Gruppe Bereitstellungen mit Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md)

[Überwachen von Vorgängen mit Ressourcenmanager](../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problem: Fehler beim Starten eines angehaltenen virtuellen Computers

Sie versuchen, Starten eines angehaltenen virtuellen Computers aber Abrufen eines Fehlers bei.

### <a name="cause"></a>Ursache

Ihre Einladung zum Starten des beendeten virtueller Computer muss bei der ursprünglichen Cluster versucht, die Cloud-Dienst hostet. Der Cluster verfügt jedoch nicht freigeben von Speicherplatz für die Ausführung der Anforderung verfügbar.

### <a name="resolution"></a>Auflösung

*   Beenden Sie alle virtuellen Computern Verfügbarkeit festlegen, und starten Sie jeden virtuellen Computer.

  1. Klicken Sie auf **Ressourcengruppen** > _der Ressourcengruppe_ > **Ressourcen** > _Festlegen Ihrer Verfügbarkeit_ > **virtuellen Computern** > _des virtuellen Computers_ > **Beenden**.

  2. Nachdem alle virtuellen Computern beendet haben, wählen Sie alle beendeten den virtuellen Computern aus, und klicken Sie auf Start.

*   Wiederholen Sie die Restart-Anforderung zu einem späteren Zeitpunkt ein.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problem: Fehler beim Ändern der Größe von einer vorhandenen virtuellen Computer

Sie versuchen, Ändern der Größe einer vorhandenen virtuellen Computer aber Abrufen eines Fehlers bei.

### <a name="cause"></a>Ursache

Die Anforderung zum Ändern der Größe des virtuellen Computer muss bei der ursprünglichen Cluster versucht, die Cloud-Dienst hostet. Cluster unterstützt jedoch nicht die angeforderten virtueller Speicher.

### <a name="resolution"></a>Auflösung

* Wiederholen Sie die Anforderung mit geringerer Größe virtueller Computer.

* Wenn die Größe des angeforderten den virtuellen Computer geändert werden kann:

  1. Beenden Sie alle virtuellen Computern Verfügbarkeit festlegen.

    * Klicken Sie auf **Ressourcengruppen** > _der Ressourcengruppe_ > **Ressourcen** > _Festlegen Ihrer Verfügbarkeit_ > **virtuellen Computern** > _des virtuellen Computers_ > **Beenden**.

  2. Nachdem alle virtuellen Computern beendet haben, Ändern der Größe des gewünschten virtuellen Computer zu vergrößern.
  3. Wählen Sie den geänderter Größe virtuellen Computer aus und klicken Sie auf **Start**, und beginnen Sie jede der beendeten virtuellen Computern.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie beim Erstellen einer neuen Linux VM in Azure Probleme auftreten, finden Sie unter [Behandeln von Problemen bei der Bereitstellung bei der Erstellung eines neuen Linux virtuellen Computers in Azure](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
