<properties
    pageTitle="Behandeln von Problemen mit automatisch skalieren mit virtuellen Computern skalieren | Microsoft Azure"
    description="Behandeln von Problemen mit automatisch skalieren mit virtuellen Computern skalieren. Grundlegendes zu häufige Probleme aufgetreten und wie Sie sie auflösen."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="guybo"/>

# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Problembehandlung bei automatisch skalieren Skala mit virtuellen Computern

**Problem** : Sie haben eine automatische Skalierung Infrastruktur in Azure-Ressourcenmanager mit virtueller Computer Maßstab Sätze – beispielsweise durch Bereitstellen einer Vorlage wie folgt erstellt: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale – Sie skalieren Regeln definiert haben und funktioniert hervorragend, außer dass unabhängig davon, wie viel Laden Sie die virtuellen Computern setzen, automatisch skalieren wird nicht.

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

Einige zu berücksichtigende Faktoren gehören:

- Wie viele Kerne verfügt jedes virtuellen Computer, und Laden Sie jede Core?
 Die oben genannte Beispiel Azure Schnellstart Vorlage enthält ein Skript do_work.php, der eine einzelne Core lädt. Wenn Sie einen virtuellen größer als ein einzelnes Core virtueller Speicher wie Standard_A1 oder D1 verwenden müssen Sie diese Last mehrmals ausführen. Überprüfen Sie, wie viele Ihrer virtuellen Computern Kerne durch [Größen für Windows-virtuellen Computern in Azure](../virtual-machines/virtual-machines-windows-sizes.md) überprüfen

- Wie viele virtuelle Computer in der virtuellen Computer Skalierung festlegen möchten, verwenden Sie Arbeit an einzelnen?

    Eine Skala, Ereignis erfolgt nur dann, wenn Sie der Mittelwert CPU über **Alle** den virtuellen Computern in einer Gruppe von Farben-Skala über einen Zeitraum internen definiert die Regeln automatisch skalieren den Schwellenwert überschreitet.

- Haben Sie alle Ereignisse Maßstab verpasst?

    Überprüfen Sie, dass der Überwachungsprotokolle für Maßstab Ereignisse im Azure-Portal an. Enthielt vielleicht ein Maßstab nach oben und ein Maßstab ab dem verpasst wurde. Sie können nach "Skalierung" filtern...

    ![Überwachungsprotokolle][audit]

- Unterscheiden sich der Maßstab in und zu verkleinern Schwellenwerte ausreichend?

    Angenommen Sie, Sie legen Sie eine Regel skalieren, wenn Mittelwert CPU größer als 50 % über 5 Minuten, und klicken Sie mit dem Maßstab ist in Wenn Mittelwert CPU weniger als 50 % ist. Dies würde ein "Flügelschlagen" Problem verursachen, wenn CPU-Auslastung in der Nähe dieser Schwellenwert, mit Skalierung Aktionen beständig, erhöhen oder Verringern der Größe des Satzes ist. Aus diesem Grund versucht der Dienst automatisch skalieren zu verhindern, dass "schlagenden", die als nicht dieselbe Skalierung manifest können. Vergewissern Sie sich daher Ihre Skalierung und Maßstab in Schwellenwerte unterscheiden sich ausreichend um einige Leerzeichen zwischen Skalierung zu ermöglichen.

- Schreiben Sie eine eigene JSON-Vorlage?

    Es ist einfach zu Fehler machen, beginnen Sie also mit einer Vorlage, wie eine über dem auf bewährte zu arbeiten, und nehmen kleine ändert. 

- Können Sie manuell vergrößern oder skalieren?

    Versuchen Sie, Erneutes Bereitstellen virtueller Computer Skalierung festlegen Ressource für eine andere "Speicherkapazität"-Einstellung, um die Anzahl der virtuellen Computern manuell ändern. Eine Beispielvorlage Zweck lautet diese folgendermaßen: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – möglicherweise müssen Sie die Vorlage zu stellen Sie sicher, dass die Computer genauso mithilfe von der Skalierung festlegen bearbeiten. Wenn Sie die Anzahl der virtuellen Computern erfolgreich manuell ändern können, wissen Sie, dass die Ursache des Problems isoliert automatisch skalieren geht.

- Überprüfen Sie Ihre Microsoft.Compute/virtualMachineScaleSet, und Microsoft.Insights Ressourcen [Azure-Explorers](https://resources.azure.com/)

    Dies ist zur Problembehandlung unverzichtbar die Sie den Status Ihrer Azure Ressourcenmanager Ressourcen anzeigt. Klicken Sie auf Ihr Abonnement, und prüfen Sie die Problembehandlung der Ressourcengruppe. Klicken Sie unter der Anbieter für Ressourcen berechnen schauen Sie sich die virtuellen Computer Maßstab legen Sie Sie erstellt haben, und überprüfen Sie, welche Ansicht Instanz, die Sie den Status einer Bereitstellung anzeigt. Aktivieren Sie auch die Instanzansicht von virtuellen Computern in der virtuellen Computer Skalierung festlegen. Wechseln Sie in der Microsoft.Insights-Anbieter für Ressourcen und überprüfen Sie, dass die Regeln automatisch skalieren gut aussehen.

- Ist die Diagnose Erweiterung arbeiten und Ausgeben von Performance-Daten?

    __Aktualisieren:__ Azure automatisch skalieren wurde verbessert, um eine basiert auf dem Host Kennzahlen Verkaufspipeline verwenden die Erweiterung Diagnose installiert werden nicht mehr erforderlich ist. Dies bedeutet das nächste, das nicht mehr in den folgenden Abschnitten angewendet werden, wenn Sie eine automatische Skalierung-Anwendung, die mithilfe der neuen Verkaufspipeline erstellen. Beispiele für Azure Vorlagen, die mit der Verkaufspipeline Host konvertiert wurden: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale, https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale. 

    Basiert auf dem Host Kennzahlen für automatisch skalieren verwenden, ist es besser, aus den folgenden Gründen:

    - Weniger gleitenden Teile als ohne Erweiterung Diagnose installiert werden müssen.
    - Einfachere Vorlagen. Eine vorhandene Maßstab festlegen Vorlage lediglich Einsichten automatisch skalieren Regeln hinzufügen.
    - Weitere zuverlässigen, Berichterstattung und schneller der neuen virtuellen Computern starten.

    Die einzige Gründe eine Diagnoseprotokollen Erweiterung weiterhin verwenden möchten wäre benötigten Arbeitsspeicher Diagnose reporting/Skalierung. Basiert auf dem Host Kennzahlen melden nicht Speicher.

    Führen Sie, denken Sie daran nur im weiteren Verlauf dieses Artikels, wenn Sie weiterhin diagnostische Erweiterungen für Ihre automatische Skalierung verwenden möchten.

    Automatisch skalieren in Azure Ressourcenmanager arbeiten können (aber nicht mehr) mithilfe eines virtuellen Computers Erweiterung die Erweiterung Diagnose bezeichnet wird. Es gibt die von Leistungsdaten mit einem Speicherkonto, das Sie in der Vorlage definieren aus. Diese Daten werden dann vom Dienst Azure Monitor aggregiert.

    Wenn der Dienst Einsichten Daten aus der virtuellen Computern gelesen werden kann, wird davon ausgegangen, senden Sie eine e-Mail – beispielsweise wäre die virtuellen Computern ab, daher sollten Sie Ihre e-Mail-Adresse (die eine, die Sie beim Erstellen des Azure-Kontos angegeben).

    Sie können auch wechseln, und prüfen Sie die Daten selbst. Schauen Sie sich Azure-Speicherkonto mithilfe eines Cloud-Explorers. Für ein Beispiel für die Verwendung der [Visual Studio-Cloud-Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), melden Sie sich an, und wählen Sie das Abonnement Azure verwenden, und den Namen des Kontos Diagnose Speicher verwiesen wird, in der Diagnose Erweiterung Definition in der Bereitstellungsvorlage..

    ![Cloud-Explorer][explorer]

    Hier sehen Sie, eine Reihe von Tabellen, in dem die Daten aus jeder virtueller Computer gespeichert wird. Aufzeichnen Linux und die CPU-Metrik beispielhaft, suchen Sie die letzte Zeilen. Der Visual Studio-Cloud-Explorer unterstützt eine Abfragesprache, damit die Ausführung einer Abfrage wie "Zeitstempel Gt Datetime'2016-02-02T21:20:00Z'" um sicherzustellen, dass Sie die neuesten Ereignisse (Ausgangspunkt Mal in UTC) erhalten. Werden die Daten, denen die in angezeigt werden, dass es den Maßstab Regeln entsprechen Sie einrichten? Im folgenden Beispiel Schritte CPU für maschinelle 20 zunehmender auf 100 %, über die letzten 5 Minuten...

    ![Speichertabellen][tables]

    Wenn die Daten nicht vorhanden ist, impliziert dies, dass das Problem mit der Diagnose Erweiterung in den virtuellen Computern ausgeführt wird. Wenn die Daten vorhanden ist, impliziert dies, dass es liegt ein Problem mit dem Maßstab Regeln oder mit dem Dienst Einsichten. Überprüfen Sie [Azure Status](https://azure.microsoft.com/status/).

    Nachdem Sie, wenn Sie weiterhin Probleme werden automatisch skalieren könnten Sie den Foren auf [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp)oder [Stapeln Überlauf](http://stackoverflow.com/questions/tagged/azure)oder eine Supportanfrage melden Sie sich über die folgenden Schritte aus, gewesen sind. Bereiten Sie die Vorlage und einer Ansicht der Leistungsdaten gemeinsam nutzen.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
