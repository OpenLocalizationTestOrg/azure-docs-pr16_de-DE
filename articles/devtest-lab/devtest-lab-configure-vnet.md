<properties
    pageTitle="Konfigurieren ein virtuelles Netzwerks in Azure DevTest Kursen | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren einer vorhandenen virtuellen Netzwerk und Subnetz, und verwenden Sie diese in einen virtuellen Computer mit Azure DevTest Labs"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="tarcher"/>

# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>Konfigurieren eines virtuellen Netzwerks in Azure DevTest Einheiten

Wie erläutert im Artikel [Hinzufügen eines virtuellen Computers mit Elemente mit einem Kurs](devtest-lab-add-vm-with-artifacts.md), beim Erstellen eines virtuellen Computers in einer Übung, können Sie ein konfigurierten virtuelles Netzwerk angeben. Ein Szenario hierfür ist, wenn Sie müssen den Zugriff auf Ihre Corpnet-Ressourcen aus Ihrer virtuellen Computer mit dem virtuellen Netzwerk, das mit ExpressRoute oder Website-zu-Standort VPN konfiguriert wurde. In den folgenden Abschnitten veranschaulichen, wie vorhandenen virtuellen Netzwerks in einer Übung virtuelles Netzwerk Einstellungen hinzufügen, sodass es wählen Sie beim Erstellen von virtuellen Computern zur Verfügung steht.

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a>Konfigurieren eines virtuellen Netzwerks für eine Übung über das Azure-portal
Die folgenden Schritte führen Sie durch Hinzufügen eines vorhandenen virtuellen Netzwerk (und Subnetz) mit einem Kurs, damit sie beim Erstellen eines virtuellen Computers in der gleichen Kurs verwendet werden kann. 

1. Melden Sie sich mit dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)aus.

1. Wählen Sie **Weitere Dienste**, und wählen Sie dann in der Liste **DevTest Labs** .

1. Wählen Sie aus der Liste der Labs die gewünschten Übung aus. 

1. Wählen Sie in der Übung des Blade **Konfiguration**aus.

1. Wählen Sie in der Übung die **Konfiguration** Blade **virtuelle Netzwerke**aus.

1. Klicken Sie auf das Blade **virtuelle Netzwerke** wird eine Liste der virtuellen Netzwerke so konfiguriert, dass für die aktuelle Übung als auch das virtuelle Standard-Netzwerk, das für Ihre Übung erstellt wird. 

1. Wählen Sie **+ Hinzufügen**.

    ![Fügen Sie ein vorhandenes virtuelles Netzwerk mit Ihrem Kurs](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
    
1. Wählen Sie auf das Blade **virtuellen Netzwerk** **[Wählen Sie virtuelle Netzwerk]**aus.

    ![Wählen Sie ein vorhandenes virtuelles Netzwerk](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
    
1. Wählen Sie auf der Blade **virtuelles Netzwerk auswählen** das gewünschte virtuelle Netzwerk aus. Das Blade zeigt die virtuellen Netzwerke, die unter der gleichen Region in das Abonnement als die Übung sind.  

1. Nach der Auswahl eines virtuellen Netzwerks, werden Sie an die **virtuellen Netzwerk** Blade zurückgegeben und mehrere Felder aktiviert sind.  

    ![Wählen Sie ein vorhandenes virtuelles Netzwerk](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)

1. Geben Sie eine Beschreibung für das virtuelle Netzwerk / Übung Kombination.

1. Damit ein Subnetz in Kurs Erstellung virtueller Computer verwendet werden kann, wählen Sie **IN virtuellen Computern-CREATION verwenden**.

1. Um öffentliche IP-Adressen in einem Subnetz zulässig sind, wählen Sie **Öffentliche IP-Adresse zulassen**.

1. Geben Sie im Feld **Maximale virtuellen Computern pro Benutzer** die maximalen virtuellen Computern pro Benutzer für jedes Subnetz an. Wenn Sie eine uneingeschränkte Anzahl von virtuellen Computern möchten, lassen Sie dieses Feld leer.

1. Wählen Sie **Speichern**aus.

1. Jetzt, da das virtuelle Netzwerk konfiguriert ist, können sie beim Erstellen eines virtuellen Computers ausgewählt werden. Um Informationen zum Erstellen eines virtuellen Computers, und geben ein virtuelles Netzwerk, finden Sie im Artikel [Hinzufügen eines virtuellen Computers mit Elemente mit einem Kurs](devtest-lab-add-vm-with-artifacts.md). 

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das gewünschte virtuelle Netzwerk mit Ihrem Kurs hinzugefügt haben, besteht der nächste Schritt zum [Hinzufügen eines virtuellen Computers mit Ihrem Kurs](devtest-lab-add-vm-with-artifacts.md).