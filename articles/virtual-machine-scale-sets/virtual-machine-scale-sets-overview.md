<properties
    pageTitle="Virtuellen Computern Maßstab legt Übersicht | Microsoft Azure"
    description="Weitere Informationen zu virtuellen Computern skalieren Datensätze"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

# <a name="virtual-machine-scale-sets-overview"></a>Übersicht des virtuellen Computern Umfang Datensätze

Virtuellen Computern sind skalieren eine Ressource mit Azure zu berechnen, die Sie zum Bereitstellen und Verwalten einer Gruppe von identischen virtuellen Computern verwenden können. Alle virtuellen Computern konfiguriert denselben virtuellen Computer skalieren, die Datensätze, zur Unterstützung von WAHR automatisch skalieren vorgesehen sind – keine vordefinierte Bereitstellung von virtuellen Computern ist erforderlich – und als solche vereinfacht umfangreiche Dienste verwendet, große berechnen, big Data und Sammelartikeleinheit Auslastung erstellen.

Für Applikationen, die skalieren berechnen Ressourcen ab und in Maßstab implizit Vorgänge verteilt werden über Domänen Fehlerstrukturanalyse und aktualisieren müssen. Finden Sie eine Einführung in die virtuellen Computer Maßstab Datensätze in der [Azure-Blog Ankündigung](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/)ein.

Sehen Sie sich diese Videos an, für Weitere Informationen zu virtuellen Computer Maßstab Datensätze an:

 - [Mark Russinovich spricht Azure-Skala Datensätze](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [Virtuellen Computern Maßstab legt fest, mit Guy Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Erstellen und Verwalten von virtuellen Computer Maßstab legt

Sie können virtuellen Computer Skalierung festlegen im [Portal Azure](https://portal.azure.com) erstellen, indem Sie _neue_ auswählen und dann mit der Eingabe in "Skalierung" in der Suchleiste. "Virtuellen Computern Skalieren festlegen" wird in den Ergebnissen angezeigt. Von dort aus können Sie die erforderlichen Felder anpassen und Bereitstellen einer Gruppe skalieren eingeben. 

Virtueller Computer skalieren kann auch definiert und mithilfe von JSON-Vorlagen und [REST-APIs](https://msdn.microsoft.com/library/mt589023.aspx) nur bereitgestellt wie einzelne Azure Ressourcenmanager virtuellen Computern an. Daher, können alle Methoden zur Bereitstellung von standard Azure Ressourcenmanager verwendet werden. Weitere Informationen zu Vorlagen finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen](../resource-group-authoring-templates.md).

Eine Reihe von Beispielvorlagen für virtuellen Computer Maßstab Mengen finden Sie im Schnellstart Azure Vorlagen GitHub Repository [hier.](https://github.com/Azure/azure-quickstart-templates) (Suchen Sie nach Vorlagen mit _Vmss_ in dem Titel)

Auf den Detailseiten für diese Vorlagen wird eine Schaltfläche angezeigt, die für die Bereitstellungsfunktion Portal links. Klicken Sie auf die Schaltfläche, und füllen Sie alle Parameter, die im Portal erforderlich sind, zum Bereitstellen von virtuellen Computer Maßstab festlegen. Wenn Sie nicht sicher, ob eine Ressource sind unterstützt oberen oder gemischte Groß-/Kleinschreibung Kleinbuchstaben Parameterwerte mit Sicherheit hat. Es gibt auch einen praktischen video Sezieren einer virtuellen Computer Maßstab festlegen Vorlage hier:

[Virtueller Computer Skalierung festlegen Vorlage Sezieren](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Aus- und festlegen Skalierung einer virtuellen Computer-Farben-Skala

Vergrößern oder verkleinern die Anzahl der virtuellen Computern in einer Gruppe von virtuellen Computer skalieren, ändern Sie die Eigenschaft _Kapazität_ und stellen Sie die Vorlage erneut bereit. Diese Vereinfachung erleichtert die eigene benutzerdefinierte Skalierung Layer Schreiben benutzerdefinierter Maßstab Ereignisse zu definieren, die von Azure automatisch skalieren nicht unterstützt werden soll.

Wenn Sie eine Vorlage zum Ändern der Kapazität bereitstellen, können Sie viel kleinere Vorlage definieren die nur die SKU und die aktualisierten Kapazität enthält. Ein Beispiel dafür dargestellt ist [hier.](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing)

Wenn Sie die Schritte durchzuführen, die eine Reihe von Farben-Skala zu erstellen, die automatisch skaliert wird, finden Sie unter [Automatisch skalieren Computern in einer virtuellen Computern Skalierung festlegen.](virtual-machine-scale-sets-windows-autoscale.md)

## <a name="monitoring-your-vm-scale-set"></a>Legen Sie Ihren Maßstab virtueller Computer für die Überwachung

Der [Azure-Portal](https://portal.azure.com) Listen Maßstab Sätze und zeigt grundlegende Eigenschaften sowie Auflistung virtuellen Computern festlegen. Für weitere Details können Sie der [Azure-Explorers](https://resources.azure.com) verwenden, um virtuellen Computer Maßstab anzuzeigen. Virtueller Computer sind skalieren eine Ressource unter Microsoft.Compute, damit diese Website Sie sehen, indem Sie die folgenden Links:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>Virtueller Computer Skalierung festlegen Szenarien

In diesem Abschnitt werden einige typischen virtueller Computer Maßstab festlegen Szenarien aufgelistet. Einige höheren Ebene Azure-Dienste (wie Stapel Dienst Fabric, Azure Container Service) werden diese Szenarios verwendet.

 - **RDP / SSH mit dem Maßstab virtueller Computer festlegen Instanzen** – A virtueller Computer Maßstab festlegen wird innerhalb eines VNET erstellt und einzelne virtuellen Computern Skalieren festlegen werden nicht öffentliche IP-Adressen zugewiesen. Dies ist eine gute Sache, da Sie nicht im Allgemeinen den Fußball und Verwaltung Aufwand für das separate öffentliche IP-Adressen auf alle in Ihrem Analyseraster berechnen statusfreien Ressourcen zuordnen möchten, und Sie können ganz einfach Herstellen einer Verbindung mit diesen virtuellen Computern von anderen Ressourcen in Ihrer VNET auch die öffentliche IP-Adressen wie Lastenausgleich oder eigenständigen virtuellen Computern haben.

 - **Verbinden mit virtuellen Computern mit NAT-Regeln** – Sie können erstellen eine öffentliche IP-Adresse, weisen sie einem Lastenausgleich und definieren eingehende Regeln für NAT einen Port auf die IP-Adresse, die einen Anschluss eines virtuellen Computers festlegen Maßstab virtueller Computer zugeordnet sind. Beispiel:
 
    Datenquelle | Port Quelle | Ziel | Ziel-Port
    --- | --- | --- | ---
    Öffentliche IP-Adresse | Port 50000 | Vmss\_0 | Anschluss 22
    Öffentliche IP-Adresse | Port 50001 | Vmss\_1 | Anschluss 22
    Öffentliche IP-Adresse | Port 50002 | Vmss\_2 | Anschluss 22

    Hier ist ein Beispiel für das Erstellen eines virtuellen Computer-Skala Satzes die NAT-Regeln verwendet wird, aktivieren Sie SSH-Verbindung zu jeder virtuellen Computer in einem Maßstab festlegen, verwenden eine einzelne öffentliche IP-Adresse: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    Hier ist ein Beispiel für das gleiche mit RDP und Windows ausführen: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - Legen Sie die **Verbindung herstellen mit virtuellen Computern mit einem "Jumpbox"** – Wenn Sie einen Maßstab virtueller Computer erstellen, und ein eigenständiges virtueller Computer in der gleichen VNET, die eigenständige virtueller Computer und virtueller Computer Maßstab festlegen, virtuellen Computern deren interne IP-Adresse mit miteinander zu verbinden können, befasst durch das VNET/Subnetz definiert. Wenn Sie eine öffentliche IP-Adresse erstellen, und sie zu der eigenständigen virtueller Computer weisen können Sie RDP oder SSH für die eigenständige virtueller Computer und dann von diesem Computer aus Herstellen einer Verbindung mit Ihrer virtuellen Computer Maßstab festlegen Instanzen. Sie können zu diesem Zeitpunkt feststellen, dass ein einfacher virtuellen Computer Maßstab Satz grundsätzlich sicherer als eine einfache eigenständige virtueller Computer mit einer öffentlichen IP-Adresse in die voreingestellte Konfiguration.

    [Ein Beispiel dieser Ansatz erstellt diese Vorlage einen einfachen Mesos Cluster, die mit einem eigenständigen Master VM der von einem virtuellen Computer Maßstab-Set-basierten Cluster virtueller Computer verwaltet.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **Lastenausgleich mit dem Maßstab virtueller Computer Instanzen festlegen** - Wenn Sie die Arbeit an einem berechnungscluster virtueller Computer mit einem Ansatz "Round-Robert" übermitteln möchten, können Sie eine Azure Lastenausgleich mit den Lastenausgleich Regeln entsprechend konfigurieren. Sie können Prüfpunkte, um zu überprüfen, ob die Anwendung wird ausgeführt, indem anpingen definieren Ports mit einem angegebenen Protokoll, Intervall und Anforderung Pfad. Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/) unterstützt auch Sätze skalieren sowie weitere anspruchsvolle Lastenausgleich Szenarien.

    [Hier ist ein Beispiel für die erstellt einen virtuellen Computer Maßstab der virtuellen Computern IIS-Webserver ausgeführt und ein Lastenausgleich den Lastenausgleich jedes virtuellen Computer empfängt verwendet. Außerdem wird das HTTP-Protokoll verwendet, eine bestimmte URL jedes virtuellen Computers Signal an.](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) (schauen Sie sich die Ressourcenart Microsoft.Network/loadBalancers und die NetworkProfile und ExtensionProfile in der VirtualMachineScaleSet)

 - **Bereitstellen von einem berechnungscluster in einer PaaS Cluster-Manager ein virtuellen Computer Maßstab festgelegte** - virtuellen Computer skalieren, die Datensätze manchmal als nächsten Generation Worker Rolle beschrieben werden. Es ist eine gültige Beschreibung jedoch es auch ausgeführt, das Risiko von verwirrend Maßstab festlegen Features mit PaaS Version 1 Worker Rollenfeatures. In einer sinnvoll virtueller Computer-Skala Mengen stellen eine Wahr "Worker-Rolle" oder Arbeitskollegen Ressource, darin, dass sie eine Ressource GRG berechnen bieten Plattform/Runtime unabhängig, also anpassbare in Azure Ressourcenmanager IaaS und integriert.

    Eine PaaS Version 1 Worker-Rolle, während im Hinblick auf Plattform/Runtime-Unterstützung (nur Windows-Plattform Bilder) eingeschränkt umfasst auch Dienste wie VIP austauschen, konfigurierbare Upgradeeinstellungen, Runtime/Bereitstellung bestimmte Einstellungen für die app die sind entweder nicht _noch_ verfügbar auf virtuellen Computer skalieren Sätze oder von anderen höheren Ebenen PaaS-Diensten wie Dienst Fabric übermittelt werden. Mit diesem in können Sie virtueller Computer Maßstab Menge als Infrastruktur genauer zu betrachten die PaaS unterstützt. H. PaaS Lösungen wie Dienst Fabric oder Cluster Manager wie Mesos auf virtuellen Computer aufbauen können skalieren Sätze als Layer skalierbare berechnen.

    [Ein Beispiel dieser Ansatz erstellt diese Vorlage einen einfachen Mesos Cluster, die mit einem eigenständigen Master VM der von einem virtuellen Computer Maßstab-Set-basierten Cluster virtueller Computer verwaltet.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json) Zukünftige Versionen des Windows [Azure-Container Dienst](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) werden weitere komplexen/abgesichert Versionen dieses Szenario basierend auf virtuellen Computer Maßstab Sätze bereitstellen.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>Virtueller Computer Maßstab festlegen Leistung und Anleitungen skalieren

- Erstellen Sie jeweils nicht mehr als 500 virtuellen Computern mehrerer virtueller Computer Maßstab gebildeten.
- Planen von maximal 20 virtuellen Computern pro Storage-Konto (es sei denn, Sie die _Overprovision_ -Eigenschaft auf "False" festlegen, in diesem Fall Sie können wechseln bis zu 40).
- Verteilen Sie die ersten Buchstaben des Speicher-Kontonamen so weit wie möglich aus.  Beispiel VMSS Vorlagen in [Azure Schnellstart Vorlagen](https://github.com/Azure/azure-quickstart-templates/) enthalten Beispiele dazu an.
- Bei Verwendung von benutzerdefinierten virtuellen Computern, Planen Sie für nicht mehr als 40 virtuellen Computern pro virtueller Computer skalieren zurück, die in einem einzigen Speicher-Konto ein.  Sie benötigen das Bild vorab kopiert berücksichtigt Speicher Vorbemerkung Bereitstellung Maßstab festlegen können. Finden Sie unter den häufig gestellten Fragen Weitere Informationen.
- Planen der Verwendung von nicht mehr als 4096 virtuellen Computern pro VNET.
- Die Anzahl der virtuellen Computern, die Sie erstellen können ist durch das Kontingent Core in der Region eingeschränkt in die Sie bereitstellen möchten. Möglicherweise müssen Sie den Kundensupport zum Erhöhen der berechnen Speicherkontingent für wurde erhöht, auch wenn Sie noch heute hoher maximal Kerne für die Verwendung mit Cloud Services oder IaaS Version 1 haben. Ihr Kontingent Abfragen den folgenden Befehl aus Azure CLI ausgeführt werden kann: `azure vm list-usage`, und die folgenden PowerShell-Befehl: `Get-AzureRmVMUsage` (Wenn eine Version von PowerShell unter 1.0 verwenden `Get-AzureVMUsage`).

## <a name="vm-scale-set-frequently-asked-questions"></a>Virtueller Computer Maßstab festlegen häufig gestellte Fragen

**F.** Wie viele virtuellen Computern können Sie in einer Gruppe von virtuellen Computer skalieren lassen?

**A** 100, wenn Sie Plattform Bilder verwenden, die über mehrere Speicherkonten hinweg verteilt werden können. Wenn Sie benutzerdefinierte Bilder, bis zu 40 (wenn die _Overprovision_ -Eigenschaft auf "false" 20 standardmäßig festgelegt ist), seit verwenden sind benutzerdefinierte Bilder auf einer einzelnen Speicher-Konto aktuell beschränkt.

**F** welche anderen Ressource Grenzwerte für virtuellen Computer Maßstab Datensätze vorhanden?

**A** Sie sind nicht mehr als 500 virtuellen Computern in mehreren Maßstab Mengen pro Region während eines Zeitraums 10 Minuten erstellen beschränkt. Die vorhandene [Grenzwerte Azure-Abonnement-Service /](../azure-subscription-service-limits.md) anwenden.

**F.** Sind Sie innerhalb virtueller Computer Maßstab Mengen Daten Datenträger unterstützt?

**A** Nicht in die erste Version. Optionen zum Speichern von Daten sind:

- Azure-Dateien (SMB freigegebene Laufwerke)

- OS Laufwerk

- TEMP-Laufwerk (lokal, nicht unterstütztes Azure-Speicher)

- Azure Datendienst (z. B. Azure Tabellen, Azure Blobs)

- Externe Daten-Dienst (z. B. remote DB)

**F.** Welche Azure Regionen unterstützen virtueller Computer Maßstab Datensätze?

**A** Die Region die Ressourcenmanager Azure unterstützt unterstützt virtueller Computer Maßstab Sätze.

**F.** Wie erstellen Sie einen virtuellen Computer Maßstab festlegen, verwenden ein benutzerdefiniertes Bild?

**A** Lassen Sie die Eigenschaft VhdContainers leer, beispielsweise:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": "https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd"
            },
            "osType": "Windows"
        }
    },


**F.** Wenn ich verringern Skalierung der meine virtueller Computer Kapazität von 20 auf 15, die virtuellen Computern entfernt werden?

**A** Virtuelle Computer werden nicht mit dem Maßstab festlegen gleichmäßig über Upgrade und Fehlerstrukturanalyse-Domänen Verfügbarkeit maximiert entfernt. Virtueller Computer mit der höchsten IDs werden zuerst entfernt.

**F.** Wie sieht es, wenn ich die Kapazität von 15 auf 18 erhöhen Sie?

**A** Wenn Sie eine Kapazität auf 18 erhöhen, werden 3 neue virtuellen Computern erstellt werden. Jedes Mal wird die virtuellen Computer Instanz-Id aus dem vorherigen höchsten Wert (z. B. 20, 21, 22) erhöht. Virtuellen Computern sind FDs und UDs verteilt.

**F.** Wenn Sie mehrere Erweiterungen in einer Gruppe von virtuellen Computer Skala verwenden, kann ich eine Sequenz Ausführung erzwingen?

**A** Nicht direkt, aber für die Erweiterung CustomScript Ihr Skript konnte warten, bis eine andere Erweiterung ([z. B., indem Sie für die Überwachung des Erweiterung Log](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)) ausführen. Zusätzliche Anleitungen Erweiterung Abfolge finden Sie im Blogbeitrag: [Erweiterung Abfolge von Azure-virtuellen Computer Maßstab Sätze](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**F.** Funktionieren virtueller Computer Maßstab Datensätze mit Azure Verfügbarkeit?

**A** Ja. Ein virtueller Computer skalieren ist eine implizit Verfügbarkeit mit 5 FDs und 5 UDs festlegen. Sie brauchen nichts unter VirtualMachineProfile konfigurieren. In zukünftigen Versionen virtueller Computer Maßstab Sätze sind wahrscheinlich mehrere Mandanten erstrecken, aber jetzt einen festlegen ist eine einzelne Verfügbarkeit Gruppe.
