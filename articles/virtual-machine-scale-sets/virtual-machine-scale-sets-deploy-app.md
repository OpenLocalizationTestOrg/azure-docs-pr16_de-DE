<properties
    pageTitle="Bereitstellen einer App auf virtuellen Computern skalieren Sätzen | Microsoft Azure"
    description="Bereitstellen einer app auf virtuellen Computern skalieren Sätzen"
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
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="guybo"/>

# <a name="deploy-an-app-on-virtual-machine-scale-sets"></a>Bereitstellen einer App auf virtuellen Computern skalieren Sätzen

Eine Anwendung auf virtuellen Computer Skalierung festlegen wird in der Regel in drei verschiedene Arten bereitgestellt:

- Installieren neuen Software auf ein Bild Plattform zum Zeitpunkt der Bereitstellung. Ein Bild Plattform in diesem Zusammenhang ist das Betriebssystem aus dem Azure Marketplace, wie Ubuntu 16.04, Windows Server 2012 R2 usw..

Sie können neuen Software auf ein Plattform Bild mit der [Erweiterung virtueller Computer](../virtual-machines/virtual-machines-windows-extensions-features.md)installieren. Eine Erweiterung virtueller Computer ist Software, die ausgeführt wird, wenn ein virtueller Computer bereitgestellt wird. Sie können keinen Code ausführen, die, den Sie zum Zeitpunkt der Bereitstellung mit der Erweiterung benutzerdefinierter Skripts zufrieden sind. [Hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale) ist ein Beispiel für Azure Ressourcenmanager Vorlage mit zwei virtuellen Computer-Erweiterungen: einer Linux benutzerdefinierte Skripts Erweiterung Apache und PHP installieren und einer Diagnoseprotokollen Erweiterung Ausgeben von Azure automatisch skalieren verwendete Performance-Daten.

Ein Vorteil hiervon ist, Sie verfügen über ein Maß Trennung zwischen den Code Ihrer Anwendung und das Betriebssystem und Ihrer Anwendung separat verwalten können. Natürlich könnte bedeutet, dass es stehen auch weitere gleitenden Teile und Zeitpunkt der Bereitstellung virtueller Computer länger, wenn es ist sehr für das Skript zum Herunterladen und konfigurieren.

**Wenn Sie vertraulichen Informationen in den Befehl benutzerdefinierte Skripts Erweiterung (beispielsweise ein Kennwort) übergeben, achten Sie darauf, geben Sie die `commandToExecute` in der `protectedSettings` Attribut der benutzerdefinierte Skripts Erweiterung statt der `settings` Attribut.**

- Erstellen Sie ein benutzerdefiniertes Bild virtueller Computer, das sowohl das Betriebssystem und die Anwendung in einer einzelnen virtuellen enthält. Hier besteht aus den Maßstab festlegen eine Reihe von virtuellen Computern von einem Bild erstellt haben, die Sie beibehalten müssen kopiert haben. Dieser Ansatz ist keine zusätzliche Konfiguration zum Zeitpunkt der Bereitstellung virtueller Computer erforderlich. Jedoch in den `2016-03-30` Version virtueller Computer Maßstab Mengen (und frühere Versionen), sind die OS Datenträger für den virtuellen Computern in den Maßstab festlegen mit einem einzelnen Speicherkonto beschränkt. Sie können auf diese Weise kann bis zu 40 virtuellen Computern in einer Gruppe von Farben-Skala: Legen Sie im Gegensatz zu den 100 virtuellen Computer pro Maßstab Beschränkung mit Plattform Bildern. Weitere Informationen hierzu finden Sie unter [Skalieren festlegen Entwurf Überblick](./virtual-machine-scale-sets-design-overview.md) .

- Bereitstellen Sie einer Plattform oder ein benutzerdefiniertes Bild im Grunde also ein Host Container, und installieren Sie die Anwendung als eine oder mehrere Container, die Sie mit einer Verwaltungstool Orchestrator- oder Konfiguration verwalten. Das Schöne an diesem Ansatz ist, dass Sie Ihre Cloud-Infrastruktur aus der Anwendungsebene abstrahiert haben und separat verwalten können.

## <a name="what-happens-when-a-vm-scale-set-scales-out"></a>Was passiert bei einer virtuellen Computer Skalierung festlegen, skaliert?

Wenn Sie eine oder mehrere virtuelle Computer ein Maßstab festlegen, indem Sie die Kapazität hinzufügen –, ob manuell oder über automatisch skalieren – wird die Anwendung automatisch installiert. Beispiel ist die Skalierung festlegen Erweiterungen definiert verfügt, führen Sie diese auf eines neuen virtuellen Computers jedes Mal, wenn sie erstellt wurde. Maßstab festlegen auf ein benutzerdefiniertes Bild basiert, ist eine neue virtueller Computer eine Kopie des benutzerdefinierten Quellbilds aus. Wenn die Skalierung eingerichtet virtuellen Computern sind Container Hosts, und klicken Sie dann möglicherweise müssen Sie beim Startcode den Container in eine benutzerdefinierte Skripts Erweiterung laden oder eine Erweiterung möglicherweise einen Agent, der mit einem Cluster Orchestrator (z. B. Azure Container Service) registriert installieren.

## <a name="how-do-you-manage-application-updates-in-vm-scale-sets"></a>Wie verwalte Sie Anwendungsupdates virtueller Computer Maßstab gebildeten?

Führen Sie drei Hauptmethoden Anwendung Updates virtueller Computer Maßstab gebildeten über die drei Methoden zur vorherigen Anwendung Bereitstellung:

* Mit der Erweiterung virtueller Computer aktualisieren. Alle virtuellen Computer-Erweiterungen, die für einen virtuellen Computer Skalierung festlegen werden jedes Mal, wenn ein neuer virtueller Computer bereitgestellt wird, eine vorhandene virtueller Computer ausgeführt definiert sind, einem neuen Image versehen oder eine Erweiterung virtuellen Computer aktualisiert wird. Wenn Sie die Anwendung aktualisieren müssen, direkt aktualisieren einer Anwendung durch Extensions ist ein realisierbar Ansatz – Sie einfach die Definition aktualisieren. Eine einfache Möglichkeit dazu ist durch Ändern der FileUris auf die neue Software verweisen.

* Der Ansatz unveränderlich benutzerdefiniertes Bild. Wenn Sie die Anwendung (oder die app-Komponenten) in ein Bild virtuellen Computer integrieren können Sie konzentrieren auf den Aufbau einer zuverlässigen Verkaufspipeline zum Erstellen, testen und Bereitstellung der Bilder zu automatisieren. Sie können Ihre Architektur zur Erleichterung schnelle Austauschen eines Satzes bereitgestellte Maßstab in Betrieb entwerfen. Ein Beispiel für gute dieser Ansatz ist der [Azure Spinnaker Treiber arbeiten](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

Packer und Terraform auch Ressourcenmanager Azure unterstützen, damit Sie können auch Ihre Bilder "als Code" definieren und in Azure zu erstellen, verwenden die virtuelle Festplatte in einer Gruppe skalieren. Jedoch würde dadurch also für Marketplace Bilder problematisch werden, wo Erweiterungen/benutzerdefinierte Skripts wichtiger geworden, da Sie die Bits von Marketplace direkt bearbeiten nicht.

* Container zu aktualisieren. Abstrakte das Anwendung Lifecycle Management auf eine Ebene oberhalb der Cloud-Infrastruktur, beispielsweise indem kapselnden Applikationen und app-Komponenten in Containern und verwalten diese Container durch Container Orchestrators und wie Verwaltungsangestellte/Marionette Konfigurations-Manager.

Skalierung festlegen virtuellen Computern aus, und klicken Sie dann die Konvertierung in eine unveränderliche Unterlage für die Container und ist nur gelegentliche Sicherheit und OS-bezogene Updates erforderlich. Wie erwähnt, ist der Container Azure Service ein guter Beispiel bei diesem Ansatz und erstellen einen Dienst versehen.

## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Wie bereit Sie über Domänen aktualisieren ein Update OS?

Nehmen Sie an, dass das Bild OS Beibehaltung der Ausführung virtueller Computer Skalierung festlegen aktualisiert werden sollen. Eine Möglichkeit dazu ist, aktualisieren Sie den virtuellen Computer ein virtueller Computer jeweils Bilder. Sie können dies mit PowerShell oder CLI Azure tun. Es gibt separate Befehle zum Aktualisieren des virtuellen Computer Skalierung festlegen-Modells (wie seine Konfiguration definiert ist), und klicken Sie auf "manuelle Aktualisierung"-Aufrufe auf einzelne virtuellen Computern ausführen.

[Hier](https://github.com/gbowerman/vmsstools) ist ein Beispiel für Python-Skript, das den Prozess der Aktualisierung der Domäne ein Update virtueller Computer Skalierung festlegen nacheinander Automatisierung. (Einschränkung: Es ist eine Prüfung des Konzepts als eine gesicherte einsatzbereit Lösung mehrere – möglicherweise einige Fehler bei der Überprüfung usw. hinzufügen möchten.).
