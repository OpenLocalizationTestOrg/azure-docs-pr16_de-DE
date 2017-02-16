<properties
   pageTitle="Aktivieren oder Deaktivieren von Azure-virtuellen Computer für die Überwachung"
   description="Beschreibt, wie Sie aktivieren oder Deaktivieren von Azure-virtuellen Computer für die Überwachung"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="enable-or-disable-azure-vm-monitoring"></a>Aktivieren oder Deaktivieren von Azure-virtuellen Computer für die Überwachung

In diesem Abschnitt beschrieben, wie Sie aktivieren oder deaktivieren die Überwachung auf virtuellen Computern auf Azure ausgeführt wird. Standardmäßig für die Überwachung ist aktiviert auf Azure-virtuellen Computern, wenn vom [Azure-Portal](https://portal.azure.com) bereitgestellt und Überwachung Diagramme werden standardmäßig mit einem Punkt 1 Minute bereitgestellt. Sie können aktivieren oder deaktivieren die Überwachung mit dem Portal oder mit Azure Line Benutzeroberfläche für Mac, Linux und Windows (Azure CLI). 

## <a name="enable--disable-monitoring-through-the-azure-portal"></a>Aktivieren Sie / deaktivieren Sie die Überwachung über das Azure-Portal
 
Sie können die Überwachung von Ihrer Azure virtueller Computer, die Daten zu Ihrer Instanz in 1 Minute Perioden bereitstellt aktivieren. (Speicher Änderungen zu übernehmen). Ausführliche Diagnosedaten ist für den virtuellen Computer in der Portalseite Diagramme oder über die API verfügbar. Standardmäßig Azure-Portal ermöglicht Überwachung, aber Sie können deaktivieren sie wie nachstehend beschrieben. Sie können die Überwachung während der virtuellen Computers ausgeführt wird, oder in Zustand angehaltenen aktivieren.

- Öffnen Sie die Azure Portals bei ** [https://portal.azure.com](https://portal.azure.com)**

- Klicken Sie im linken Navigationsbereich auf virtuellen Computern.

- Wählen Sie in der Liste virtuellen Computern eine Instanz ausgeführte oder beendete. Blad virtuellen Computern wird geöffnet.

- Klicken Sie auf "Alle Einstellungen".

- Klicken Sie auf "Diagnose".

- Ändern der Status auf oder deaktivieren. Sie können auch diese Blade wählen Sie in der Ebene der Überwachung Details aus, die Sie für Ihre virtuellen Computern aktivieren möchten.

[Azure.Note] Die Diagnose auf ist der Standardwert beim Erstellen eines neuen virtuellen Computers

![Aktivieren Sie / deaktivieren Sie die Überwachung über das Azure-Portal.][1]


## <a name="enable--disable-monitoring-with-azure-cli"></a>Aktivieren Sie / deaktivieren Sie die Überwachung mit Azure CLI
 
So aktivieren Sie die Überwachung für eine Azure-virtuellen Computer.

- Erstellen einer Datei mit dem Namen wie PrivateConfig.json mit dem folgenden Inhalt.
        {"StorageAccountName": "Speicher Firma Daten erhalten", "StorageAccountKey": "die Taste für das Konto"}
- Führen Sie den folgenden Befehl aus Azure CLI.

        azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.0 --private-config-path PrivateConfig.json

[Azure.Note] Sie können auf eine neuere Version, wenn verfügbar von Version 2.0 ändern. 

Für weitere Details zum Konfigurieren von Überwachung Kennzahlen und Beispielen, finden Sie auf das Dokument - **[Mithilfe von Linux Diagnostic Erweiterung für Monitor Linux virtuellen Computers Leistung und Diagnose Daten](virtual-machines-linux-classic-diagnostic-extension.md).

<!--Image references-->
[1]: ./media/virtual-machines-linux-vm-monitoring/portal-enable-disable.png
 

