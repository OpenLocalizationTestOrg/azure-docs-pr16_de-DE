
Wenn das Problem Azure nicht in diesem Artikel behandelt wird, finden Sie auf den [Azure-Foren auf MSDN und Stapel überlaufen](https://azure.microsoft.com/support/forums/). Sie können das Problem auf diese Foren oder zu Posten @AzureSupport auf Twitter. Darüber hinaus können Sie eine Supportanfrage Azure-Datei, indem Sie auf der Website [Azure unterstützen](https://azure.microsoft.com/support/options/) **Unterstützung** auswählen.

## <a name="general-troubleshooting-steps"></a>Allgemeine Schritte zur Problembehandlung
### <a name="troubleshoot-common-allocation-failures-in-the-classic-deployment-model"></a>Problembehandlung bei häufig auftretenden Zuteilung Fehler im Bereitstellungsmodell klassischen

Diese Schritte können viele Zuteilung Fehler beim virtuellen-beheben:

- Ändern der Größe des virtuellen Computer auf einen anderen virtuellen Computer Größe.<br>
  Klicken Sie auf **Alle durchsuchen** > **virtuellen Computern (klassische)** > des virtuellen Computers > **Einstellungen** > **Größe**. Die detaillierten Schritte finden Sie unter [Ändern der Größe des virtuellen Computers](https://msdn.microsoft.com/library/dn168976.aspx).

- Löschen Sie alle virtuellen Computern aus der Cloud-Dienst und erstellen Sie virtuelle Computer erneut.<br>
  Klicken Sie auf **Alle durchsuchen** > **virtuellen Computern (klassische)** > des virtuellen Computers > **Löschen**. Klicken Sie dann auf **neu** > **berechnen** > [virtuellen Computerabbild].

### <a name="troubleshoot-common-allocation-failures-in-the-azure-resource-manager-deployment-model"></a>Problembehandlung bei häufig auftretenden Zuteilung Fehler im Bereitstellungsmodell Azure Ressourcenmanager

Schritte können Sie viele Zuteilung Fehler beim virtuellen-beheben:

- Beenden (Freigeben) alle virtuellen Computern in der gleichen Verfügbarkeit festlegen, und starten Sie jeweils erneut.<br>
  So beenden Sie die: Klicken Sie auf **Ressourcengruppen** > der Ressourcengruppe > **Ressourcen** > Ihrer Verfügbarkeit festlegen > **virtuellen Computern** > des virtuellen Computers > **Beenden**.

    Nachdem alle virtuellen Computern beendet haben, wählen Sie den ersten virtuellen Computer aus, und klicken Sie auf **Start**.

## <a name="background-information"></a>Hintergrundinformationen
### <a name="how-allocation-works"></a>Funktionsweise der Verteilung
Die Server in Azure Rechenzentren werden in Cluster aufgeteilt. Normalerweise, eine Anforderung Zuteilung in mehrere Cluster versucht wird, doch es ist möglich, dass bestimmte Einschränkungen aus der Anforderung Zuteilung erzwingen, dass die Azure-Plattform die Anfrage in nur eine Cluster versucht. In diesem Artikel werden wir dies als Referenz "an einem Cluster angeheftet." Diagramm 1 unter veranschaulicht die Groß-/Kleinschreibung von einer normalen Zuteilung, die in mehrere Cluster versucht wird. Diagramm 2 zeigt einen Fall einer Zuordnung, die auf Cluster 2 angehefteten hat, da dies ist, wo die vorhandene Cloud-Dienst CS_1 oder Verfügbarkeit Menge gehostet wird.
![Zuteilung Diagramm](./media/virtual-machines-common-allocation-failure/Allocation1.png)

### <a name="why-allocation-failures-happen"></a>Warum geschieht Zuteilung Fehlern
Wenn eine Anforderung Zuteilung zu einem Cluster fixiert ist, ist es eine höhere Wahrscheinlichkeit von kostenlosen Ressourcen findet, da der verfügbaren Ressourcenpool kleiner ist. Darüber hinaus, wenn Anforderung Zuteilung zu einem Cluster fixiert ist, aber die Art der Ressource, die Sie angefordert wird durch die Cluster nicht unterstützt, Anforderung tritt auch, wenn der Cluster kostenlose Ressourcen hat. Diagramm 3 unter veranschaulicht der Groß-/Kleinschreibung, da nur Candidate Cluster nicht kostenlose Ressourcen verfügt, die eine fixierte Zuordnung aus. Diagramm 4 veranschaulicht den Fall, in dem eine fixierte Zuordnung schlägt fehl, da nur Candidate Cluster die angeforderten virtueller Speicher nicht unterstützt, obwohl der Cluster kostenlose Ressourcen hat.

![Fehler beim angeheftete zuweisen](./media/virtual-machines-common-allocation-failure/Allocation2.png)

## <a name="detailed-troubleshoot-steps-specific-allocation-failure-scenarios-in-the-classic-deployment-model"></a>Ausführliche Behandeln von Problemen mit Schritte besondere Zuteilung Fehlerszenarien im Bereitstellungsmodell klassischen
Hier werden häufige Zuteilung Szenarien, die dazu führen, dass eine Anforderung Zuteilung fixiert werden. Wir werden jedes Szenarios weiter unten in diesem Artikel befassen.

- Ändern der Größe eines virtuellen Computers oder Hinzufügen von virtuellen Computern oder Rolleninstanzen zu einem vorhandenen Clouddienst
- Starten Sie die teilweise beendete (freigegeben) virtuellen Computern
- Starten Sie neu vollständig beendete (freigegeben) virtuellen Computern
- Staging/Fertigung Bereitstellungen (Plattform als Dienst nur)
- Zugehörigkeit Gruppe (VM-Dienst in der Nähe)
- Virtuelles Netzwerk Zugehörigkeit basierende Gruppe

Wenn Sie eine Zuteilung Fehlermeldung, finden Sie unter Wenn die beschriebenen Szenarios mit Ihrer Fehler anwenden. Verwenden Sie den von der Azure-Plattform zurückgegebenen Zuteilung Fehler, um das entsprechende Szenario zu identifizieren. Wenn Ihre Anforderung fixiert ist, entfernen Sie einige der festen Einschränkungen auf Ihre Einladung zu weitere Cluster, wodurch erhöht die Wahrscheinlichkeit eines Erfolgs Zuteilung öffnen.

Im Allgemeinen solange der Fehler "die angeforderten virtueller Speicher wird nicht unterstützt" nicht angezeigt wird, können Sie immer zu einem späteren Zeitpunkt wiederholen wie genügend Ressourcen im Cluster Anforderung gerecht werden kann freigegeben wurde. Wenn das Problem ist, dass die angeforderten virtueller Speicher nicht unterstützt wird, versuchen Sie eine andere virtueller Computer Größe aus. Andernfalls ist die einzige Option die feste Einschränkung zu entfernen.

Zwei häufige Fehlerszenarien beziehen sich auf Gruppen. In der Vergangenheit eine Gruppe Zugehörigkeit Nähe des virtuellen Computern/Dienst Instanzen bieten verwendet wurde, oder es wurde verwendet, um die Erstellung eines virtuellen Netzwerks aktivieren. Mit der Einführung der regionalen virtuelle Netzwerke sind Gruppen die nicht mehr erforderlich, um ein virtuelles Netzwerk erstellen. Mit Verringerung der Netzwerkwartezeit Azure Infrastruktur geändert empfohlen Zugehörigkeit Gruppen für Näherung VM-Dienst verwenden.

Diagramm 5 unter bietet die Taxonomie Szenarien Zuteilung (angeheftete).
![Fixierte Zuordnung Taxonomie](./media/virtual-machines-common-allocation-failure/Allocation3.png)

> [AZURE.NOTE] Der Fehler in jedem Szenario Zuteilung aufgeführt ist eine kurze Form. Schlagen Sie in der [Fehlermeldung Zeichenfolge suchen](#Error string lookup) detaillierte Fehlerzeichenfolgen.

## <a name="allocation-scenario-resize-a-vm-or-add-vms-or-role-instances-to-an-existing-cloud-service"></a>Zuteilung Szenario: Ändern der Größe eines virtuellen Computers oder Hinzufügen von virtuellen Computern oder Rolleninstanzen zu einem vorhandenen Clouddienst
**Fehler**

Upgrade_VMSizeNotSupported oder GeneralError

**Ursache Cluster Anheftung**

Eine Anforderung zum Ändern der Größe eines virtuellen Computers oder Hinzufügen eines virtuellen Computers oder eine Instanz der Rolle zu einer vorhandenen Cloud-Dienst muss bei der ursprünglichen Cluster versucht werden, die den vorhandenen Clouddienst hostet. Erstellen einen neuen Clouddienst ermöglicht die Azure-Plattform an einen anderen Cluster zu finden, die kostenlose Ressourcen oder die Größe virtueller Computer, die Sie angefordert unterstützt.

**Dieses Problem zu umgehen**

Wenn der Fehler Upgrade_VMSizeNotSupported * ist, versuchen Sie eine andere virtueller Computer Größe. Erstellen Sie Wenn in unterschiedlichen Größen virtueller Computer nicht möglich ist, aber wenn sie eine andere virtuelle IP-Adresse (VIP) verwendet werden, einen neuen Clouddienst, um den neuen virtuellen Computer hosten, und fügen Sie den neuen Clouddienst das regionalen virtuelle Netzwerk, in dem die vorhandenen virtuellen Computern ausgeführt werden. Wenn Ihre vorhandene Cloud-Dienst ein Landes-/ virtuelles Netzwerk nicht verwendet, können Sie weiterhin erstellen ein neues virtuelles Netzwerks für den neuen Clouddienst, und Verbinden Ihrer [vorhandenen virtuelles Netzwerk in das neue virtuelle Netzwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Finden Sie weitere Informationen zu [regionalen virtuelle Netzwerke](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/)aus.

Wenn des Fehlers GeneralError * ist, ist es wahrscheinlich, dass die Art der Ressource (beispielsweise einer bestimmten virtueller Speicher) wird durch den Cluster unterstützt, aber der Cluster hat keinen kostenlose Ressourcen zum Zeitpunkt. Ähnlich wie der obigen Szenario hinzufügen die gewünschten berechnen Ressource durch die Erstellung eines neuen Cloud-Diensts (Beachten Sie, dass neue Cloud-Dienst verwenden, um eine andere VIP muss), und verwenden Sie ein Landes-/ virtuelles Netzwerk in der Cloud-Webdiensten herstellen.

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>Zuteilung Szenario: Restart teilweise beendet (freigegeben) virtuellen Computern

**Fehler**

GeneralError *

**Ursache Cluster Anheftung**

Teilweise zur Freigabe bedeutet, dass Sie (freigegeben) virtuellen Computern eine oder mehr, aber nicht alle in einen Clouddienst angehalten. Wenn Sie beenden (Freigeben) ein virtueller Computer, die zugeordneten Ressourcen freigegeben werden. Einen Neustart von diesen beendet (freigegeben) virtuellen Computer ist deshalb eine neue Zuteilung Anforderung aus. Einen Neustart von virtuellen Computern in einer teilweise freigegeben Cloud-Dienst entspricht dem Hinzufügen von virtuellen Computern zu einem vorhandenen Clouddienst. Die Anfrage Zuteilung muss bei der ursprünglichen Cluster versucht werden, die den vorhandenen Clouddienst hostet. Erstellen einen anderen Cloud-Service ermöglicht die Azure-Plattform an einen anderen Cluster zu finden, die kostenlose oder die virtuellen Computer Größe, die Sie angefordert unterstützt.

**Dieses Problem zu umgehen**

Wenn sie zu eine andere VIP verwenden, löschen die beendeten (freigegeben) virtuellen Computern (aber halten Sie die zugehörigen Datenträger) zulässig ist und die virtuellen Computern wieder über einen anderen Clouddienst hinzufügen. Verwenden Sie ein Landes-/ virtuelles Netzwerk, um Ihre Cloud-Webdiensten herstellen:
- Wenn Ihre vorhandene Cloud-Dienst ein Landes-/ virtuelles Netzwerk verwendet, fügen Sie einfach neue Cloud-Dienst mit dem gleichen virtuellen Netzwerk.
- Wenn Ihre vorhandene Cloud-Dienst ein Landes-/ virtuelles Netzwerk nicht verwendet, erstellen Sie ein neues virtuelles Netzwerk für das neue Cloud-Dienst, und [Verbinden Ihrer vorhandene virtuelle Netzwerk in das neue virtuelle Netzwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/)aus. Finden Sie weitere Informationen zu [regionalen virtuelle Netzwerke](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/)aus.

## <a name="allocation-scenario-restart-fully-stopped-deallocated-vms"></a>Zuteilung Szenario: Neustart vollständig beendet (freigegeben) virtuellen Computern
**Fehler**

GeneralError *

**Ursache Anheftung cluster**

Vollständige zur Freigabe bedeutet, die Sie nicht mehr freigegeben () alle virtuellen Computern, aus der Cloud-Dienst. Die Zuteilung Anfragen diese virtuelle Computer neu starten, müssen bei der ursprünglichen Cluster herangezogen, die Cloud-Dienst hostet. Erstellen eines neuen Cloud-Diensts ermöglicht die Azure-Plattform an einen anderen Cluster zu finden, die kostenlose Ressourcen oder die virtuellen Computer Größe, die Sie angefordert unterstützt.

**Dieses Problem zu umgehen**

Wenn sie zu eine andere VIP verwenden, löschen die ursprünglichen beendeten (freigegeben) virtuellen Computern (aber halten Sie die zugehörigen Datenträger) zulässig ist, und löschen den entsprechenden Clouddienst (die zugeordneten berechnen Ressourcen wurden bereits freigegeben, wenn Sie (freigegeben) nicht mehr im virtuellen Computern). Erstellen Sie einen neuen Clouddienst die virtuellen Computern wieder hinzufügen.

## <a name="allocation-scenario-stagingproduction-deployments-platform-as-a-service-only"></a>Zuteilung Szenario: Staging/Fertigung Bereitstellungen (Plattform als Dienst nur)
**Fehler**

New_General* oder New_VMSizeNotSupported*

**Ursache Anheftung cluster**

Das staging und der Herstellung Bereitstellung der Cloud-Dienst werden im selben Cluster gehostet werden. Wenn Sie die zweite Bereitstellung hinzufügen, wird die entsprechende Zuteilung Anforderung im selben Cluster versucht, die die erste Bereitstellung hostet.

**Dieses Problem zu umgehen**

Löschen Sie der ersten Bereitstellung und den ursprünglichen Clouddienst und erneut bereitstellen Sie Cloud-Dienst. Diese Aktion kann die erste Bereitstellung landen in einem Cluster, der genügend Ressourcen an beide Bereitstellungen anpassen oder in einem Cluster, der die Größen virtueller Computer unterstützt, die Sie angefordert.

## <a name="allocation-scenario-affinity-group-vmservice-proximity"></a>Zuteilung Szenario: Zugehörigkeit Gruppe (VM-Dienst in der Nähe)
**Fehler**

New_General* oder New_VMSizeNotSupported*

**Ursache Cluster Anheftung**

Eine zu berechnen, dass eine Gruppe für die Zugehörigkeit zugeordnete Ressource zu einem Cluster verknüpft ist. Neue berechnen Ressource anfordert, da Zugehörigkeit Gruppe werden im selben Cluster, in dem die vorhandenen Ressourcen gehostet sind, übermitteln. Dies trifft, ob die neuen Ressourcen über einen neuen Clouddienst oder einem vorhandenen Clouddienst erstellt werden.

**Dieses Problem zu umgehen**

Wenn eine Gruppe für die Zugehörigkeit nicht erforderlich ist, führen Sie nicht verwenden Sie eine Gruppe für die Zugehörigkeit oder berechnen Ressourcen in mehreren Gruppen die Gruppe.

## <a name="allocation-scenario-affinity-group-based-virtual-network"></a>Zuteilung Szenario: virtuelles Netzwerk Zugehörigkeit basierende Gruppe
**Fehler**

New_General* oder New_VMSizeNotSupported*

**Ursache Cluster Anheftung**

Bevor Landes-/ virtuelle Netzwerke eingeführt wurden, müssen Sie eine Gruppe für die Zugehörigkeit ein virtuelles Netzwerk zuordnen. Daher berechnen Ressourcen in einer Gruppe Zugehörigkeit platziert werden durch die gleichen Einschränkung gebunden in beschriebenen der "Zuteilung Szenario: Zugehörigkeit Gruppe (VM-Dienst in der Nähe)" im Abschnitt oben. Die berechnen Ressourcen werden auf einen einzigen Cluster verknüpft.

**Dieses Problem zu umgehen**

Wenn Sie eine Gruppe für die Zugehörigkeit nicht benötigen, erstellen Sie ein neues Landes-/ virtuelles Netzwerk für die neuen Ressourcen, die Sie hinzufügen möchten, und [Verbinden Sie das vorhandene virtuelle Netzwerk in das neue virtuelle Netzwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Finden Sie weitere Informationen zu [regionalen virtuelle Netzwerke](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/)aus.

Alternativ können Sie [Ihre Zugehörigkeit basierende Gruppe virtuelle Netzwerk zu einem regionalen virtuellen Netzwerk migrieren](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), und fügen Sie die gewünschten Ressourcen erneut hinzu.

## <a name="detailed-troubleshooting-steps-specific-allocation-failure-scenarios-in-the-azure-resource-manager-deployment-model"></a>Detaillierte Schritte besondere Zuteilung Fehlerszenarien im Bereitstellungsmodell Azure Ressourcenmanager Problembehandlung
Hier werden häufige Zuteilung Szenarien, die dazu führen, dass eine Anforderung Zuteilung fixiert werden. Wir werden jedes Szenarios weiter unten in diesem Artikel befassen.

- Ändern der Größe eines virtuellen Computers oder Hinzufügen von virtuellen Computern oder Rolleninstanzen zu einem vorhandenen Clouddienst
- Starten Sie die teilweise beendete (freigegeben) virtuellen Computern
- Starten Sie erneut vollständig beendete (freigegeben) virtuellen Computern

Wenn Sie eine Zuteilung Fehlermeldung, finden Sie unter Wenn die beschriebenen Szenarios mit Ihrer Fehler anwenden. Verwenden Sie den von der Azure-Plattform zurückgegebenen Zuteilung Fehler, um das entsprechende Szenario zu identifizieren. Wenn Ihre Anfrage zu einem vorhandenen Cluster fixiert ist, entfernen Sie einige der festen Einschränkungen auf Ihre Einladung zu weitere Cluster, wodurch erhöht die Wahrscheinlichkeit eines Erfolgs Zuteilung öffnen.

Im Allgemeinen solange der Fehler "die angeforderten virtueller Speicher wird nicht unterstützt" nicht angezeigt wird, können Sie immer einem späteren Zeitpunkt wiederholen wie genügend Ressourcen im Cluster Anforderung gerecht werden kann freigegeben wurden. Wenn das Problem ist, dass die angeforderten virtueller Speicher nicht unterstützt wird, finden Sie unter problemumgehungen.

## <a name="allocation-scenario-resize-a-vm-or-add-vms-to-an-existing-availability-set"></a>Zuteilung Szenario: Ändern der Größe eines virtuellen Computers oder Hinzufügen von virtuellen Computern zu einem bestehenden Satz der Verfügbarkeit
**Fehler**

Upgrade_VMSizeNotSupported* oder GeneralError*

**Ursache Anheftung cluster**

Eine Anforderung zum Ändern der Größe eines virtuellen Computers oder Hinzufügen eines virtuellen Computers zu einem bestehenden Satz von Verfügbarkeit muss bei der ursprünglichen Cluster versucht werden, die den vorhandenen Verfügbarkeit Satz hostet. Erstellen Sie neue Verfügbarkeit ermöglicht die Azure-Plattform an einen anderen Cluster zu finden, die kostenlose Ressourcen oder die virtuellen Computer Größe, die Sie angefordert unterstützt.

**Dieses Problem zu umgehen**

Wenn der Fehler Upgrade_VMSizeNotSupported * ist, versuchen Sie eine andere virtueller Computer Größe. Wenn in unterschiedlichen Größen virtueller Computer nicht möglich ist, beenden Sie alle virtuellen Computern Verfügbarkeit festlegen. Sie können dann die Größe des virtuellen Computers ändern, die den virtuellen Computer zu einem Cluster zuzuordnen, die die gewünschte Größe des virtuellen Computer unterstützt.

Wenn der Fehlercode GeneralError * lautet, ist es wahrscheinlich, dass die Art der Ressource (beispielsweise einer bestimmten virtueller Speicher) wird durch den Cluster unterstützt, aber der Cluster hat keinen kostenlose Ressourcen zum Zeitpunkt. Wenn Sie der virtuellen Computer in einer anderen Verfügbarkeit Reihe werden kann, Erstellen eines neuen virtuellen Computers in einer anderen Verfügbarkeit (in der gleichen Region) festlegen. Diese neuen virtuellen Computer kann dann mit dem gleichen virtuellen Netzwerk hinzugefügt werden.  

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>Zuteilung Szenario: Restart teilweise beendet (freigegeben) virtuellen Computern
**Fehler**

GeneralError *

**Ursache Cluster Anheftung**

Teilweise zur Freigabe bedeutet, dass Sie nicht (freigegeben) einen oder mehrere mehr, aber nicht alle virtuellen Computern in einer Verfügbarkeit festlegen. Wenn Sie beenden (Freigeben) ein virtueller Computer, die zugeordneten Ressourcen freigegeben werden. Einen Neustart von diesen beendet (freigegeben) virtuellen Computer ist deshalb eine neue Zuteilung Anforderung aus. Einen Neustart von virtuellen Computern in einer Menge teilweise freigegeben Verfügbarkeit entspricht dem Hinzufügen von virtuellen Computern zu einem bestehenden Satz von Verfügbarkeit. Die Anfrage Zuteilung muss bei der ursprünglichen Cluster versucht werden, die den vorhandenen Verfügbarkeit Satz hostet.

**Dieses Problem zu umgehen**

Beenden Sie alle virtuellen Computern im Feld Verfügbarkeit vor dem Neustart der ersten Phase festlegen. Dadurch wird sichergestellt, dass ein neue Zuteilung Versuch ausgeführt wird und ein neuer Cluster ausgewählt werden kann, die verfügbaren Kapazität enthält.

## <a name="allocation-scenario-restart-fully-stopped-deallocated"></a>Zuteilung Szenario: Neustart vollständig beendet (freigegeben)
**Fehler**

GeneralError *

**Ursache Cluster Anheftung**

Vollständige zur Freigabe bedeutet, den Sie angehalten freigegeben () alle virtuellen Computern in einer Verfügbarkeit festlegen. Die Anfrage Zuteilung diese virtuelle Computer neu starten, werden alle Cluster adressieren, die die gewünschte Größe zu unterstützen.

**Dieses Problem zu umgehen**

Wählen Sie eine neue virtueller Speicher zugewiesen werden. Wenn dies nicht funktioniert, versuchen Sie es später erneut.

## <a name="error-string-lookup"></a>Fehlermeldung Zeichenfolge suchen
**New_VMSizeNotSupported***

"Das virtueller Computer Größe (oder die Kombination von virtuellen Computer Größen) erforderlich, indem Sie diese Bereitstellung kann aufgrund der Bereitstellung Anforderung Einschränkungen bereitgestellt werden. Falls möglich, versuchen Sie gemütlich Einschränkungen wie virtuelle Netzwerk Bindungen, in einen gehosteten Service für keine anderen Bereitstellung darin und in eine andere Gruppe für die Zugehörigkeit oder keine Gruppe Zugehörigkeit bereitstellen oder bereitstellen zu einem anderen Bereich."

**New_General***

"Fehler bei Zuteilung; Einschränkungen in Anforderung erfüllen können nicht genutzt werden. Die angeforderten neue Service-Bereitstellung an eine Gruppe Zugehörigkeit gebunden ist oder das Ziel eines virtuellen Netzwerks oder eine vorhandene Bereitstellung unter diesen gehosteten Dienst vorhanden ist. Einer der folgenden Situationen schränkt die neue Bereitstellung auf bestimmte Azure Ressourcen. Später erneut versuchen Sie, oder verringern Sie der virtueller Speicher oder die Anzahl der Rolleninstanzen. Alternativ, falls möglich die oben genannten Einschränkungen entfernen oder versuchen Sie es in ein anderes Region bereitstellen."

**Upgrade_VMSizeNotSupported***

"Aktualisierung die Bereitstellung nicht möglich. Die angeforderten virtueller Speicher Funktionen Länge und LÄNGEB möglicherweise nicht in die Unterstützung der vorhandenen bereitstellungs Ressourcen zur Verfügung. Bitte versuchen Sie es später erneut, versuchen Sie es mit einem anderen virtueller Speicher oder kleinere Anzahl der Rolleninstanzen oder erstellen Sie eine Bereitstellung unter einer leeren gehosteten Dienst mit einer neuen Gruppe für die Zugehörigkeit oder keine Zugehörigkeit Gruppe Bindung."

**GeneralError***

"Der Server ist einen internen Fehler aufgetreten. Wiederholen Sie die Anfrage." Oder "Fehler bei einer Zuteilung für den Dienst werden."
