<properties
    pageTitle="Hinzufügen ein virtuellen Computers mit Elemente mit einem Kurs in Azure DevTest Kursen | Microsoft Azure"
    description="Informationen Sie zum Hinzufügen eines virtuellen Computers mit Elemente in Azure DevTest Einheiten"
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
    ms.date="08/30/2016"
    ms.author="tarcher"/>

# <a name="add-a-vm-with-artifacts-to-a-lab-in-azure-devtest-labs"></a>Hinzufügen eines virtuellen Computers mit Elemente mit einem Kurs in Azure DevTest Einheiten

> [AZURE.VIDEO how-to-create-vms-with-artifacts-in-a-devtest-lab]

Sie erstellen ein virtuellen Computers in einem Kurs aus einer *Basis* , die ein [benutzerdefiniertes Bild](./devtest-lab-create-template.md), [Formel](./devtest-lab-manage-formulas.md)oder [Marketplace-Bild](./devtest-lab-configure-marketplace-images.md)ist ein.

DevTest Labs *Elemente* können Sie angeben, *Aktionen* , die ausgeführt werden, wenn Sie der virtuellen Computer erstellt wird. 

Element-Aktionen können Verfahren, wie z. B. laufenden Windows PowerShell-Skripts Bash Befehle ausgeführt, und Installieren von Software ausführen. 

Element- *Parameter* können Sie das Element, für das spezifische Szenario anpassen.

In diesem Artikel wird das Erstellen eines virtuellen Computers in Ihrem Kurs mit Elemente veranschaulicht.

## <a name="add-a-vm-with-artifacts"></a>Hinzufügen eines virtuellen Computers mit Elemente

1. Melden Sie sich mit dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)aus.

1. Wählen Sie **Weitere Dienste**, und wählen Sie dann in der Liste **DevTest Labs** .

1. Wählen Sie aus der Liste der Labs der Übung, in dem Sie den virtuellen Computer erstellen möchten.  

1. Wählen Sie in der Übung des **Übersicht** Blade **+ virtuellen Computern**aus.  
    ![Virtueller Computer-Schaltfläche "hinzufügen"](./media/devtest-lab-add-vm-with-artifacts/devtestlab-home-blade-add-vm.png)

1. Wählen Sie auf der Blade **Auswählen einer Basis** einer Basis für den virtuellen Computer aus.

1. Geben Sie einen Namen für den neuen virtuellen Computer auf das Blade **virtuellen Computers** in das Textfeld **Name des virtuellen Computers** .

    ![Übung virtueller Computer blade](./media/devtest-lab-add-vm-with-artifacts/devtestlab-lab-vm-blade.png)

1. Geben Sie einen **Benutzernamen** , der Administratorrechte des virtuellen Computers erteilt werden soll.  

1. Wenn Sie ein Kennwort in Ihrem *geheimen Store*gespeichert verwenden möchten, wählen Sie die **geheimen Daten aus meiner geheimen Store verwenden**, und geben Sie einen Schlüsselwert, der Ihre Schlüssel (Kennwort) entspricht. Geben Sie ein Kennwort andernfalls einfach in das Textfeld ein, **Geben Sie einen Wert**mit der Bezeichnung.
 
1. Wählen Sie die **Größe des virtuellen Computers** , und wählen Sie eine der vordefinierten Elemente, die die Prozessorkerne, RAM und der Festplattengröße für den virtuellen Computer zu erstellen, angeben.

1. Wählen Sie **virtuellen Netzwerk** aus, und wählen Sie das gewünschte virtuelle Netzwerk aus.

1. Wählen Sie **Subnetz** aus, und wählen Sie Subnetz.

1. Wenn die Richtlinie Übung öffentliche IP-Adressen für das ausgewählte Subnetz zulassen festgelegt ist, geben Sie an, ob Sie die IP-Adresse öffentlich sein, indem Sie **Ja** oder **Nein**auswählen möchten. Diese Option ist andernfalls deaktiviert und als **keine**ausgewählt. 

1. Wählen Sie **Elemente** aus – wählen Sie aus der Liste der Elemente – und konfigurieren Sie die Elemente, die Sie zu dem Basis Bild hinzufügen möchten. 
**Hinweis:** Wenn Sie neu bei DevTest Labs oder Konfigurieren von Elemente sind, gehen Sie zum Abschnitt [Hinzufügen einer vorhandenen Element zu eines virtuellen Computers](#add-an-existing-artifact-to-a-vm) , und klicken Sie dann hier zurückzukehren Sie, klicken Sie abschließend.

1. Wenn Sie anzeigen oder kopieren Sie die Vorlage Azure Ressourcenmanager, gehen Sie zum Abschnitt [Azure Ressourcenmanager speichern Vorlage](#save-arm-template) und dieser Seite zurückkehren, klicken Sie abschließend möchten.

1. Wählen Sie **Erstellen** , um den angegebenen virtuellen Computer mit dem Kurs hinzufügen.

1. Das Blade Übung zeigt den Status der Erstellung des virtuellen Computers; zuerst wurde als **Erstellen**, klicken Sie dann als nach dem virtuellen Computer **ausgeführt wird** gestartet.

1. Wechseln Sie zum Abschnitt [Nächsten Schritten fort](#next-steps) . 

## <a name="add-an-existing-artifact-to-a-vm"></a>Hinzufügen eines vorhandenen Artefakts zu eines virtuellen Computers

Beim Erstellen eines virtuellen Computers, können Sie vorhandene Elemente hinzufügen. Lektionen enthält Elemente aus öffentlichen DevTest Labs Element Repository als auch Elemente, die Sie erstellt und eigene Element Repository hinzugefügt haben.
Finden Sie im Artikel, [erfahren Sie, wie Sie Ihre eigenen Elemente für die Verwendung mit DevTest Labs verfassen](devtest-lab-artifact-author.md), wie Sie Elemente erstellen um zu ermitteln.

1. Wählen Sie auf der Blade **virtuellen Computers** **Elemente**aus. 

1. Wählen Sie in der Blade **Elemente hinzufügen** das gewünschte Element aus.  

    ![Hinzufügen von Elemente blade](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifact-blade.png)

1. Geben Sie die erforderlichen Parameterwerte und eine optionale Parameter, die Sie benötigen.  

1. Wählen Sie **Hinzufügen** , fügen Sie das Element und zurückkehren zur das Blade **Elemente hinzufügen** aus.

1. Fahren Sie Elemente für Ihre virtuellen Computer nach Bedarf.

1. Nachdem Sie Ihre Elemente hinzugefügt haben, können Sie das [Ändern der Reihenfolge, in der die Elemente ausgeführt werden](#change-the-order-in-which-artifacts-are-run). Sie können auch zum [anzeigen oder Ändern eines Artefakts](#view-or-modify-an-artifact)zurückkehren.

## <a name="change-the-order-in-which-artifacts-are-run"></a>Ändern der Reihenfolge, in der Elemente ausgeführt werden

Standardmäßig werden die Aktionen für die Elemente in der Reihenfolge ausgeführt, in denen sie den virtuellen Computer hinzugefügt werden. Die folgenden Schritte veranschaulichen, wie die Reihenfolge zu ändern, in der die Elemente ausgeführt werden.

1. Am oberen Rand der Blade **Elemente hinzufügen** wählen Sie den Link, der angibt, der Anzahl der Elemente, die den virtuellen Computer hinzugefügt wurden.

    ![Anzahl der Elemente virtueller Computer hinzugefügt](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)

1. Um die Reihenfolge festzulegen, in der die Elemente ausgeführt werden, Drag & drop der Elemente in der gewünschten Reihenfolge. **Hinweis:** Wenn Sie Probleme beim Ziehen des Element haben, stellen Sie sicher, dass Sie von der linken Seite des Artefakts ziehen. 

1. Wählen Sie **OK** , wenn Sie fertig.  

## <a name="view-or-modify-an-artifact"></a>Anzeigen oder Ändern eines Artefakts

Die folgenden Schritte veranschaulichen, wie anzeigen oder ändern die Parameter ein:

1. Am oberen Rand der Blade **Elemente hinzufügen** wählen Sie den Link, der angibt, der Anzahl der Elemente, die den virtuellen Computer hinzugefügt wurden.

    ![Anzahl der Elemente virtueller Computer hinzugefügt](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)

1. Wählen Sie das Element, das Sie anzeigen oder bearbeiten möchten, klicken Sie auf das Blade **Elemente ausgewählt** .  

1. Klicken Sie auf das **Element hinzufügen** Blade nehmen Sie benötigten vor, und wählen Sie **OK** , um das **Element hinzufügen** Blade schließen aus.

1. Wählen Sie **OK** , um das **Ausgewählte Elemente** Blade schließen aus.

## <a name="save-azure-resource-manager-template"></a>Ressourcenmanager Azure-Vorlage speichern

Eine Vorlage Azure Ressourcenmanager bietet deklarativ definieren eine Bereitstellung wiederholbare. Die folgenden Schritte erläutert, wie zum Speichern der Ressourcenmanager Azure-Projektvorlage für den virtuellen Computer erstellt wird.
Nach dem Speichern, können Sie die Vorlage Ressourcenmanager Azure bereitstellen [Neuer virtueller Computer mit Azure PowerShell](../azure-resource-manager/resource-group-overview.md#template-deployment)verwenden.

1. Wählen Sie auf der Blade **virtuellen Computers** **Ansicht Cloud-Vorlage**aus.

1. Wählen Sie in der **Ansicht Azure Ressourcenmanager Vorlage Blade**Text für die Vorlage ein.

1. Kopieren des markierten Texts in die Zwischenablage.

1. Wählen Sie **OK** , um die **Ansicht Azure Ressourcenmanager Vorlage Blade**schließen aus.

1. Öffnen Sie einen Text-Editor ein.

1. Fügen Sie den Text für die Vorlage aus der Zwischenablage ein.

1. Speichern Sie die Datei zur späteren Verwendung.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nächste Schritte

- Nachdem Sie der virtuellen Computer erstellt wurde, können Sie mit dem virtuellen Computer verbinden, **Verbinden** des virtuellen Computers Blade die Option auswählen.
- Erfahren Sie, wie Sie [benutzerdefinierte Elemente für Ihre DevTest Labs virtuellen Computer erstellen](devtest-lab-artifact-author.md).
- Untersuchen der [DevTest Labs Cloud Schnellstart Vorlagenkatalog](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)