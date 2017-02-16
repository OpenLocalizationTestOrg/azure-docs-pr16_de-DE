<properties
   pageTitle="Behandeln von Problemen mit Windows virtueller Computer Bereitstellung-klassischen | Microsoft Azure"
   description="Problembehandlung bei klassischen Bereitstellungsprobleme beim Erstellen eines neuen Windows virtuellen Computers in Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
  ms.service="virtual-machines-windows"
  ms.workload="na"
  ms.tgt_pltfrm="vm-windows"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/06/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Behandeln von Problemen mit klassischen Bereitstellungsprobleme bei der Erstellung eines neuen Windows virtuellen Computers in Azure

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-selectors-include.md)]

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Sammeln der Überwachungsprotokolle

Sammeln Sie zum Starten der Problembehandlung der Überwachungsprotokolle zum Identifizieren des Fehlers das Problem zugeordnet.

Klicken Sie auf **Durchsuchen**, im Portal Azure > **virtuellen Computern** > *Ihrem Windows-Computer* > **Einstellungen** > **Überwachungsprotokolle**.

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** Wenn das Betriebssystem Windows GRG ist und es Upload oder über die Einstellung GRG erfasst ist, wird nicht Fehlern vorhanden sein. Auf ähnliche Weise ist das Betriebssystem Windows spezialisierte, ist es Upload oder über die spezielle Einstellung erfasst, und es können keine Fehler nicht.

**Laden Sie Fehler an:**

**N<sup>1</sup>:** Wenn das Betriebssystem Windows GRG ist-, und es wird als hochgeladen spezialisierte, erhalten Sie einen Timeoutfehler provisioning mit dem virtuellen Computer, auf dem Bildschirm OOBE hängen.

**N<sup>2</sup>:** Wenn das Betriebssystem ist Windows spezialisierte und hochgeladen wird, wie GRG, erhalten Sie einen provisioning Fehler mit dem virtuellen Computer, auf dem Bildschirm OOBE hängen, da Sie der neue virtuellen Computer mit dem ursprünglichen Computername, Benutzername und Kennwort ausgeführt wird.

**Lösung:**

Um beide dieser Fehler zu beheben, laden Sie die ursprüngliche virtuelle Festplatte, verfügbar auf-Prem, mit der gleichen Einstellung wie für das Betriebssystem (GRG-/ spezialisierte) hoch. Zum Hochladen wie GRG, denken Sie daran Sysprep zum ersten Mal starten. Weitere Informationen finden Sie unter [Erstellen und Hochladen einer Windows Server virtuellen in Azure](virtual-machines-windows-classic-createupload-vhd.md) .

**Erfassen von Fehlern:**

**N<sup>3</sup>:** Wenn das Betriebssystem Windows GRG ist-, und es werden als erfasst spezialisierte, erhalten Sie einen Timeoutfehler provisioning, da die ursprünglichen virtuellen nicht verwendet werden kann, wie er als GRG markiert ist.

**N<sup>4</sup>:** Ist das Betriebssystem Windows spezialisierte, und es wird als GRG erfasst, erhalten Sie einen provisioning Fehler, da Sie der neue virtuellen Computer mit dem ursprünglichen Computername, Benutzername und Kennwort ausgeführt wird. Auch die ursprüngliche virtueller Computer kann nicht da ist es steht als spezielle.

**Lösung:**

Um beide dieser Fehler zu beheben, löschen Sie das aktuelle Bild aus dem Portal und [Erfassen von der aktuellen virtuellen Festplatten](virtual-machines-windows-classic-capture-image.md) mit der gleichen Einstellung wie für das Betriebssystem (GRG-/ spezialisierte) ein.

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Problem: Benutzerdefinierte / Katalog / Marketplace-Bild Fehler beim Zuweisen
Dieser Fehler tritt in Situationen auf, wenn der Anforderung des neue virtuellen Computer zu einem Cluster, die gesendet wird entweder hat keinen verfügbaren Speicherplatz, um die Anfrage zu berücksichtigen, oder die virtueller Speicher angefordert wird nicht unterstützt. Es ist nicht möglich, mischen Sie verschiedene Reihe von virtuellen Computern in der gleichen Cloud-Dienst. Also wird soll zum Erstellen eines neuen virtuellen Computers einer anderen Größe als was Ihre Cloud-Dienst unterstützen kann, die Anfrage berechnen fehl.

Je nach der Einschränkungen der Cloud-Dienst, den Sie verwenden, um den neuen virtuellen Computer erstellen, können einen durch eine von zwei Situationen verursacht Fehler auftreten.

**Ursache 1:** Cloud-Dienst zu einem bestimmten Cluster fixiert ist, oder ist es an eine Gruppe für die Zugehörigkeit verknüpft und daher zu einem bestimmten Cluster standardmäßig angehefteten. So neu berechnen Ressource anfordert, da Zugehörigkeit Gruppe sind versucht, in demselben Cluster, in dem die vorhandenen Ressourcen gehostet werden. Jedoch dieselbe Zuordnungseinheit möglicherweise nicht unterstützen die angeforderten virtueller Speicher oder nicht genügend Speicherplatz, was zu einem Fehler Zuteilung haben. Dies trifft, ob die neuen Ressourcen über einen neuen Clouddienst oder einem vorhandenen Clouddienst erstellt werden.

**Lösung 1:**

- Erstellen eines neuen Cloud-Diensts, und ordnen Sie sie entweder einen Bereich oder eine bereichsbasierte virtuelles Netzwerk.
- Erstellen eines neuen virtuellen Computers in der neuen Cloud-Dienst an.
  Wenn Sie eine Fehlermeldung erhalten, bei dem Versuch, einen neuen Clouddienst zu erstellen, zu einem späteren Zeitpunkt wiederholen Sie, oder ändern Sie den Bereich für den Clouddienst.

> [AZURE.IMPORTANT] Wenn Sie versuchten, erstellen einen neuen virtuellen Computer in einem vorhandenen Clouddienst aber konnten nicht und zum Erstellen eines neuen Cloud-Diensts für Ihre neue virtuellen Computer haben, können Sie zum Konsolidieren von Ihrer virtuellen Computer in der gleichen Cloud-Dienst auswählen. Klicken Sie hierzu löschen Sie die virtuellen Computern in den vorhandenen Clouddienst, und erfassen sie von ihren Festplatten in der neuen Cloud-Dienst. Es ist jedoch wichtig, denken Sie daran, dass der neuen Cloud-Dienst hat einen neuen Namen und VIP, sodass Sie diese für alle die Abhängigkeiten aktualisieren, die derzeit diese Informationen für den vorhandenen Clouddienst verwenden müssen.

**Verursachen 2:** Cloud-Dienst ist ein virtuelles Netzwerk, das zu einer Gruppe Zugehörigkeit verknüpft ist, sodass es zu einem bestimmten Cluster standardmäßig fixiert ist zugeordnet. Alle neuen berechnen Ressource anfordert, da Zugehörigkeit Gruppe sind daher versucht, in demselben Cluster, in dem die vorhandenen Ressourcen gehostet werden. Jedoch dieselbe Zuordnungseinheit möglicherweise nicht unterstützen die angeforderten virtueller Speicher oder nicht genügend Speicherplatz, was zu einem Fehler Zuteilung haben. Dies trifft, ob die neuen Ressourcen über einen neuen Clouddienst oder einem vorhandenen Clouddienst erstellt werden.

**Lösung 2:**

- Erstellen eines neuen regionalen virtuellen Netzwerks.
- Erstellen Sie den neuen virtuellen Computer in das neue virtuelle Netzwerk ein.
- [Verbinden Ihrer vorhandenen virtuelles Netzwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) in das neue virtuelle Netzwerk. Finden Sie weitere Informationen zu [regionalen virtuelle Netzwerke](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/)aus. Alternativ können Sie [Ihre Zugehörigkeit basierende Gruppe virtuelle Netzwerk zu einem regionalen virtuellen Netzwerk migrieren](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), und klicken Sie dann den neuen virtuellen Computer zu erstellen.

## <a name="next-steps"></a>Nächste Schritte
Wenn beim Starten eines beendet Windows virtuellen Computers oder Ändern der Größe einer vorhandenen Windows virtueller Computer in Azure Probleme auftreten, finden Sie unter [Behandeln von klassischen Bereitstellungsprobleme mit dem Neustarten oder Ändern der Größe einer vorhandenen Windows-virtuellen Computern, die in Azure](windows/classic/virtual-machines-windows-classic-restart-resize-error-troubleshooting.md).
