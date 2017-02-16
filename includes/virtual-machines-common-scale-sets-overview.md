

Für Applikationen, die skalieren berechnen Ressourcen ab und in Maßstab implizit Vorgänge verteilt werden über Domänen Fehlerstrukturanalyse und aktualisieren müssen. Finden Sie eine Einführung in die virtuellen Computer skalieren Datensätze in der zuletzt verwendete [Azure Blog Ankündigung](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/)ein.

Sehen Sie sich diese Videos an, für Weitere Informationen zu virtuellen Computer Maßstab Datensätze an:

 - [Mark Russinovich spricht Azure-Skala Datensätze](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [Virtuellen Computern Maßstab legt fest, mit Guy Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Erstellen und Verwalten von virtuellen Computer Maßstab legt

Virtueller Computer Maßstab Mengen definiert werden können und über JSON-Vorlagen und [REST-APIs](https://msdn.microsoft.com/library/mt589023.aspx) nur bereitgestellt wie einzelne Azure Ressourcenmanager virtuellen Computern an. Daher, können alle Methoden zur Bereitstellung von standard Azure Ressourcenmanager verwendet werden. Weitere Informationen zu Vorlagen finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen](../articles/resource-group-authoring-templates.md).

Eine Reihe von Beispielvorlagen für virtuellen Computer Maßstab Mengen finden Sie im Schnellstart Azure Vorlagen GitHub Repository hier:

[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) - Suchen nach Vorlagen mit _Vmss_ in dem Titel.

Auf den Detailseiten für diese Vorlagen wird eine Schaltfläche angezeigt, die für die Bereitstellungsfunktion Portal links. Klicken Sie auf die Schaltfläche, und füllen Sie alle Parameter, die im Portal erforderlich sind, zum Bereitstellen von virtuellen Computer Maßstab festlegen. Wenn Sie nicht sicher, ob eine Ressource sind unterstützt oberen oder gemischte Groß-/Kleinschreibung Kleinbuchstaben Parameterwerte mit Sicherheit hat. Es gibt auch einen praktischen video Sezieren einer virtuellen Computer Maßstab festlegen Vorlage hier:

[Virtueller Computer Skalierung festlegen Vorlage Sezieren](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Aus- und festlegen Skalierung einer virtuellen Computer-Farben-Skala

Vergrößern oder verkleinern die Anzahl der virtuellen Computern in einer Gruppe von virtuellen Computer skalieren, ändern Sie die Eigenschaft _Kapazität_ und stellen Sie die Vorlage erneut bereit. Diese Vereinfachung erleichtert die eigene benutzerdefinierte Skalierung Layer Schreiben benutzerdefinierter Maßstab Ereignisse zu definieren, die von Azure automatisch skalieren nicht unterstützt werden soll.

Wenn Sie eine Vorlage zum Ändern der Kapazität bereitstellen, können Sie viel kleinere Vorlage definieren die nur die SKU und die aktualisierten Kapazität enthält. Ein Beispiel dafür ist hier gezeigten: [https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-scale-in-or-out/azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-linux-nat/azuredeploy.json).

Wenn Sie die Schritte durchzuführen, die eine Reihe von Farben-Skala zu erstellen, die automatisch skaliert wird, finden Sie unter [Automatisch skalieren Computern in einer virtuellen Computern Skalierung festlegen.](../articles/virtual-machines/virtual-machines-windows-vmss-powershell-creating.md)

## <a name="monitoring-your-vm-scale-set"></a>Legen Sie Ihren Maßstab virtueller Computer für die Überwachung

Es wird derzeit empfohlen, dass Sie der [Azure Ressource Explorer](https://resources.azure.com) verwenden, um virtuellen Computer Maßstab anzuzeigen. Virtueller Computer sind skalieren eine Ressource unter Microsoft.Compute, damit diese Website Sie sehen, indem Sie die folgenden Links:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>Virtueller Computer Skalierung festlegen Szenarien

In diesem Abschnitt werden einige typischen virtueller Computer Maßstab festlegen Szenarien aufgelistet. Einige höheren Ebene Azure-Dienste (wie Stapel Dienst Fabric, Azure Container Service) werden diese Szenarios verwendet.

 - **RDP / SSH mit dem Maßstab virtueller Computer festlegen Instanzen** – A virtueller Computer Maßstab festlegen wird innerhalb eines VNET erstellt und einzelne virtuellen Computern in sind nicht öffentliche IP-Adressen zugewiesen. Dies ist eine gute Sache, da Sie nicht im Allgemeinen den Aufwand Fußball und Verwaltung der separate IP-Adressen auf alle Ihre berechnen Raster statusfreien Ressourcen zuordnen möchten, und Sie können ganz einfach Herstellen einer Verbindung mit diesen virtuellen Computern von anderen Ressourcen in Ihrer VNET auch die öffentliche IP-Adressen wie Lastenausgleich oder eigenständigen virtuellen Computern haben.

 - **Verbinden mit virtuellen Computern mit NAT-Regeln** – Sie können erstellen eine öffentliche IP-Adresse, weisen sie einem Lastenausgleich und definieren eingehende Regeln für NAT einen Port auf die IP-Adresse, die einen Anschluss eines virtuellen Computers festlegen Maßstab virtueller Computer zugeordnet sind. Z. B.

    Öffentliche IP-Port 50000 > -> Vmss\_0 -> Port 22 öffentlichen IP - > Port 50001 -> Vmss\_1 -> Port 22 öffentlichen IP - > Port 50002 -> Vmss\_2-Anschluss 22 >

    Hier ist ein Beispiel für das Erstellen eines virtuellen Computer-Skala Satzes die NAT-Regeln verwendet wird, aktivieren Sie SSH-Verbindung zu jeder virtuellen Computer in einem Maßstab festlegen, verwenden eine einzelne öffentliche IP-Adresse: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    Hier ist ein Beispiel für das gleiche mit RDP und Windows ausführen: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - Legen Sie die **Verbindung herstellen mit virtuellen Computern mit einem "Jumpbox"** – Wenn Sie einen Maßstab virtueller Computer erstellen, und ein eigenständiges virtueller Computer in der gleichen VNET, die eigenständige virtueller Computer und virtueller Computer Maßstab festlegen, virtuellen Computern deren interne IP-Adresse mit miteinander zu verbinden können, befasst durch das VNET/Subnetz definiert. Wenn Sie eine öffentliche IP-Adresse erstellen, und sie zu der eigenständigen virtueller Computer weisen können Sie RDP oder SSH für die eigenständige virtuellen Computer, und verbinden Sie zu Ihrer virtuellen Computer Maßstab festlegen Instanzen von diesem Computer aus. Sie können zu diesem Zeitpunkt feststellen, dass ein einfacher virtuellen Computer Maßstab Satz grundsätzlich sicherer als eine einfache eigenständige virtueller Computer mit einer öffentlichen IP-Adresse in die voreingestellte Konfiguration.

    Ein Beispiel dieser Ansatz, diese Vorlage erstellt einen einfachen Mesos Cluster, die mit einem eigenständigen Master VM die einen virtueller Computer Maßstab-Set-basierten Cluster virtuellen Computern verwaltet: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **Round Robert Lastenausgleich mit dem Maßstab virtueller Computer Instanzen festlegen** - Wenn Sie die Arbeit an einem berechnungscluster virtueller Computer mit einem Ansatz "Round-Robert" übermitteln möchten, können Sie eine Azure Lastenausgleich Lastenausgleich Konfigurieren von Regeln entsprechend. Sie können auch definieren Prüfpunkte, um zu überprüfen, ob die Anwendung wird ausgeführt, indem anpingen Ports mit einem angegebenen Protokoll, Intervall und Anforderung Pfad.

    Hier ist ein Beispiel für die erstellt einen virtuellen Computer Maßstab der virtuellen Computern IIS-Webserver ausgeführt und ein Lastenausgleich den Lastenausgleich jedes virtuellen Computer empfängt verwendet. Verwendet das HTTP-Protokoll auch eine bestimmte URL jedes virtuellen Computers Signal an: [https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) - Blick auf die Ressourcenart Microsoft.Network/loadBalancers und die NetworkProfile und ExtensionProfile in der VirtualMachineScaleSet.

 - **Bereitstellen von einem berechnungscluster in einer PaaS Cluster-Manager ein virtuellen Computer Maßstab festgelegte** - virtuellen Computer skalieren, die Datensätze manchmal als nächsten Generation Worker Rolle beschrieben werden. Es ist eine gültige Beschreibung jedoch es auch ausgeführt, das Risiko von verwirrend Maßstab festlegen Features mit PaaS Version 1 Worker Rollenfeatures. In einer sinnvoll virtueller Computer skalieren Mengen stellen eine Wahr "Worker-Rolle" oder Ressource Worker, darin, dass sie eine Ressource GRG berechnen bieten Plattform/Runtime unabhängig, also anpassbare in Azure Ressourcenmanager IaaS und integriert.

    Eine PaaS Version 1 Worker-Rolle, während im Hinblick auf Plattform/Runtime-Unterstützung (nur Windows-Plattform Bilder) eingeschränkt umfasst auch Dienste wie VIP austauschen, konfigurierbare Upgradeeinstellungen, Runtime/Bereitstellung bestimmte Einstellungen für die app die sind entweder nicht _noch_ verfügbar auf virtuellen Computer skalieren Sätze oder von anderen höheren Ebenen PaaS-Diensten wie Dienst Fabric übermittelt werden. Vor diesem können Sie virtueller Computer Maßstab Sätze als Infrastruktur eigenständig die PaaS unterstützt. H. PaaS Lösungen wie Dienst Fabric oder Cluster Manager wie Mesos auf virtuellen Computer aufbauen können skalieren Sätze als Layer skalierbare berechnen.

    Hier ist ein Beispiel für einen Mesos Cluster mit der ein virtueller Computer Skalierung festlegen in der gleichen VNET als eigenständiges virtueller Computer bereitstellt. Die eigenständige virtueller Computer ist ein Mesos Master-Shape, und virtueller Computer Maßstab festlegen stellt einen Satz von untergeordnete Knoten: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json). Zukünftige Versionen des Windows [Azure-Container Dienst](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) werden weitere komplexen/abgesichert Versionen dieses Szenario basierend auf virtuellen Computer Maßstab Sätze bereitstellen.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>Virtueller Computer Maßstab festlegen Leistung und Anleitungen skalieren

- Bei der öffentlichen Vorschau erstellen nicht mehr als 500 virtuellen Computern in mehreren virtuellen Computer Maßstab Kriteriengruppen nacheinander.
- Planen der Verwendung von nicht mehr als 40 virtuellen Computern pro Storage-Konto.
- Verteilen Sie die ersten Buchstaben des Speicher-Kontonamen so weit wie möglich aus.  Beispiel VMSS Vorlagen in [Azure Schnellstart Vorlagen](https://github.com/Azure/azure-quickstart-templates/) enthalten Beispiele dazu an.
- Bei Verwendung von benutzerdefinierten virtuellen Computern, Planen Sie für nicht mehr als 40 virtuellen Computern pro virtueller Computer skalieren zurück, die in einem einzigen Speicher-Konto ein.  Sie benötigen das Bild vorab kopiert berücksichtigt Speicher Vorbemerkung Bereitstellung Maßstab festlegen können. Finden Sie unter den häufig gestellten Fragen Weitere Informationen.
- Planen der Verwendung von nicht mehr als 2048 virtuellen Computern pro VNET.  Dieser Grenzwert wird in der Zukunft erhöht.
- Die Anzahl der virtuellen Computern, die Sie erstellen können, ist durch Ihrer berechnen Core Quote in einem beliebigen Bereich eingeschränkt. Möglicherweise müssen Sie den Kundensupport zum Erhöhen der berechnen Speicherkontingent für wurde erhöht, auch wenn Sie noch heute hoher maximal Kerne für die Verwendung mit Cloud Services oder IaaS Version 1 haben. Ihr Kontingent Abfragen den folgenden Befehl aus Azure CLI ausgeführt werden kann: _Azure-virtuellen Computer Liste-Verwendung_, und die folgenden PowerShell-Befehl: _Get-AzureRmVMUsage_ (Wenn Sie mit einer Version von PowerShell unter 1.0 _Get-AzureVMUsage_verwenden).

## <a name="vm-scale-set-frequently-asked-questions"></a>Virtueller Computer Maßstab festlegen häufig gestellte Fragen

**F.** Wie viele virtuellen Computern können Sie in einer Gruppe von virtuellen Computer skalieren lassen?

**A** 100, wenn Sie Plattform Bilder verwenden, die über mehrere Speicherkonten hinweg verteilt werden können. Wenn Sie benutzerdefinierte Bilder, bis 40, zu verwenden, da benutzerdefinierte Bilder in der Vorschau auf einem einzelnen Speicherkonto beschränkt sind.

**F** welche anderen Ressource Grenzwerte für virtuellen Computer Maßstab Datensätze vorhanden?

**A** Sie können nur mehrere Maßstab Sätze pro Region während der Preview-Zeitraum nicht mehr als 500 virtuellen Computern erstellen. Die vorhandene [Grenzwerte Azure-Abonnement-Service /](../articles/azure-subscription-service-limits.md) anwenden.

**F.** Sind Sie innerhalb virtueller Computer Maßstab Mengen Daten Datenträger unterstützt?

**A** Nicht in die erste Version. Optionen zum Speichern von Daten sind:

- Azure-Dateien (SMB freigegebene Laufwerke)

- OS Laufwerk

- TEMP-Laufwerk (lokal, nicht unterstütztes Azure-Speicher)

- Externe Daten-service (z. B. remote DB, Azure-Tabellen, Azure Blobs)

**F.** Welche Azure Regionen Unterstützung für virtuellen Computer Maßstab Mengen?

**A** Die Region die Ressourcenmanager Azure unterstützt unterstützt virtueller Computer Maßstab Sätze.

**F.** Wie erstellen Sie einen virtuellen Computer Maßstab Sätze Verwendung eines benutzerdefinierten Bildes?

**A** Lassen Sie die Eigenschaft VhdContainers leer, beispielsweise:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": [https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd](https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd)
            },
            "osType": "Windows"
        }
    },


**F.** Wenn ich verringern Skalierung der meine virtueller Computer Kapazität von 20 auf 15, die virtuellen Computern entfernt werden?

**A** Virtuelle Computer werden nicht mit dem Maßstab festlegen gleichmäßig über Upgrade und Fehlerstrukturanalyse-Domänen Verfügbarkeit maximiert entfernt.

**F.** Wie sieht es, wenn ich die Kapazität von 15 auf 18 erhöhen Sie?

**A** Wenn Sie auf 18, virtueller Computer mit Index 15, 16, erhöhen wird 17 erstellt. In beiden Fällen sind die virtuellen Computern FDs und UDs verteilt.

**F.** Wenn Sie mehrere Erweiterungen in einer Gruppe von virtuellen Computer Skala verwenden, kann ich eine Sequenz Ausführung erzwingen?

**A** Nicht direkt, aber für die Erweiterung CustomScript Ihr Skript konnte warten, bis eine andere Erweiterung ausführen (z. B., indem Sie die Erweiterung für die Überwachung protokollieren – finden Sie unter [https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install\_lap.sh](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)).

**F.** Funktionieren virtueller Computer Maßstab Datensätze mit Azure Verfügbarkeit?

**A** Ja. Ein virtueller Computer skalieren ist eine implizit Verfügbarkeit mit FDs 3 und 5 UDs festlegen. Sie brauchen nichts unter VirtualMachineProfile konfigurieren. In zukünftigen Versionen virtueller Computer Maßstab Sätze sind wahrscheinlich mehrere Mandanten erstrecken, aber jetzt einen festlegen ist eine einzelne Verfügbarkeit Gruppe.
