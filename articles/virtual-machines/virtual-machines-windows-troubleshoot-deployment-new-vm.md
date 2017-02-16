<properties
   pageTitle="Behandeln von Problemen mit Windows virtueller Computer Bereitstellung-RM | Microsoft Azure"
   description="Problembehandlung bei Ressourcenmanager Bereitstellungsprobleme beim Erstellen eines neuen Windows virtuellen Computers in Azure"
   services="virtual-machines-windows, azure-resource-manager"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue, azure-resource-manager"/>

<tags
  ms.service="virtual-machines-windows"
  ms.workload="na"
  ms.tgt_pltfrm="vm-windows"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/09/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Behandeln von Problemen mit Ressourcenmanager Bereitstellungsprobleme bei der Erstellung eines neuen Windows virtuellen Computers in Azure

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Sammeln der Überwachungsprotokolle

Sammeln Sie zum Starten der Problembehandlung der Überwachungsprotokolle zum Identifizieren des Fehlers das Problem zugeordnet. Die folgenden Links enthalten ausführliche Informationen zu den Prozess, denen Sie folgen.

[Behandeln von Problemen mit Ressourcen Gruppe Bereitstellungen mit Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md)

[Überwachen von Vorgängen mit Ressourcenmanager](../resource-group-audit.md)

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** Wenn das Betriebssystem Windows GRG ist und es Upload oder über die Einstellung GRG erfasst ist, wird nicht Fehlern vorhanden sein. Auf ähnliche Weise ist das Betriebssystem Windows spezialisierte, ist es Upload oder über die spezielle Einstellung erfasst, und es können keine Fehler nicht.

**Laden Sie Fehler an:**

**N<sup>1</sup>:** Wenn das Betriebssystem Windows GRG ist-, und es wird als hochgeladen spezialisierte, erhalten Sie einen Timeoutfehler provisioning mit dem virtuellen Computer, auf dem Bildschirm OOBE hängen.

**N<sup>2</sup>:** Wenn das Betriebssystem ist Windows spezialisierte und hochgeladen wird, wie GRG, erhalten Sie einen provisioning Fehler mit dem virtuellen Computer, auf dem Bildschirm OOBE hängen, da Sie der neue virtuellen Computer mit dem ursprünglichen Computername, Benutzername und Kennwort ausgeführt wird.

**Auflösung**

Um beide dieser Fehler zu beheben, verwenden Sie [Hinzufügen-AzureRmVhd die ursprüngliche virtuelle Festplatte hochladen](https://msdn.microsoft.com/library/mt603554.aspx), verfügbare lokal mit dieselbe Einstellung wie für das Betriebssystem (GRG-/ spezialisierte) aus. Zum Hochladen wie GRG, denken Sie daran Sysprep zum ersten Mal starten.

**Erfassen von Fehlern:**

**N<sup>3</sup>:** Wenn das Betriebssystem Windows GRG ist-, und es werden als erfasst spezialisierte, erhalten Sie einen Timeoutfehler provisioning, da die ursprünglichen virtuellen nicht verwendet werden kann, wie er als GRG markiert ist.

**N<sup>4</sup>:** Ist das Betriebssystem Windows spezialisierte, und es wird als GRG erfasst, erhalten Sie einen provisioning Fehler, da Sie der neue virtuellen Computer mit dem ursprünglichen Computername, Benutzername und Kennwort ausgeführt wird. Auch die ursprüngliche virtueller Computer kann nicht da ist es steht als spezielle.

**Auflösung**

Um beide dieser Fehler zu beheben, löschen Sie das aktuelle Bild aus dem Portal und [Erfassen von der aktuellen virtuellen Festplatten](virtual-machines-windows-vhd-copy.md) mit der gleichen Einstellung wie für das Betriebssystem (GRG-/ spezialisierte) ein.

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Problem: Benutzerdefinierte/Katalog/Marketplace-Bild; Fehler beim Zuweisen
Dieser Fehler tritt in Situationen auf, wenn die neue Anforderung der virtuellen Computer zu einem Cluster fixiert ist, die entweder keine Unterstützung für die angeforderten virtueller Speicher oder hat keinen verfügbaren Speicherplatz, um die Anfrage zu berücksichtigen.

**Ursache 1:** Der Cluster kann die angeforderten virtueller Speicher nicht unterstützen.

**Lösung 1:**

- Wiederholen Sie die Anforderung, die mit geringerer Größe virtueller Computer an.
- Wenn die Größe des die angeforderten virtueller Computer nicht geändert werden kann:
  - Beenden Sie alle virtuellen Computern Verfügbarkeit festlegen.
  Klicken Sie auf **Ressourcengruppen** > *der Ressourcengruppe* > **Ressourcen** > *Festlegen Ihrer Verfügbarkeit* > **virtuellen Computern** > *des virtuellen Computers* > **Beenden**.
  - Nachdem alle virtuellen Computern beendet haben, erstellen Sie den neuen virtuellen Computer in die gewünschte Größe aus.
  - Starten Sie den neuen virtuellen Computer zuerst, und wählen Sie alle beendeten den virtuellen Computern aus und klicken Sie auf **Start**.

**Verursachen 2:** Der Cluster hat keine Ressourcen freizugeben.

**Lösung 2:**

- Wiederholen Sie die Anforderung zu einem späteren Zeitpunkt ein.
- Wenn Sie der neue virtuellen Computer in einer anderen Verfügbarkeit Reihe werden können
  - Erstellen eines neuen virtuellen Computers in einer anderen Verfügbarkeit (in der gleichen Region) festlegen.
  - Fügen Sie den neuen virtuellen Computer mit dem gleichen virtuellen Netzwerk ein.

## <a name="next-steps"></a>Nächste Schritte
Wenn beim Starten eines beendet Windows virtuellen Computers oder Ändern der Größe einer vorhandenen Windows virtueller Computer in Azure Probleme auftreten, finden Sie unter [Behandeln von Problemen mit Ressourcenmanager Bereitstellungsprobleme mit Neustarten oder Ändern der Größe einer vorhandenen Windows-virtuellen Computern, die in Azure](virtual-machines-windows-restart-resize-error-troubleshooting.md).
