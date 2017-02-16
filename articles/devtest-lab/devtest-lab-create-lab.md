<properties
    pageTitle="Erstellen einer Übung in Azure DevTest Kursen | Microsoft Azure"
    description="Erstellen Sie eine Übung in Azure DevTest Kursen für virtuellen Computern"
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
    ms.topic="get-started-article"
    ms.date="09/12/2016"
    ms.author="tarcher"/>

# <a name="create-a-lab-in-azure-devtest-labs"></a>Erstellen einer Übung in Azure DevTest Einheiten

## <a name="prerequisites"></a>Erforderliche Komponenten

Um eine Übung zu erstellen, müssen Sie folgende Aktionen ausführen:

- Ein Azure-Abonnement. Weitere Informationen zu Language Pack für Azure-Optionen finden Sie unter [Azure erwerben](https://azure.microsoft.com/pricing/purchase-options/) oder [kostenlose Testversion für einen Monat](https://azure.microsoft.com/pricing/free-trial/). Sie müssen der Besitzer des Abonnements, die Übung zu erstellen.

## <a name="steps-to-create-a-lab-in-azure-devtest-labs"></a>Schritte zum Erstellen einer Übung in Azure DevTest Einheiten

Die folgenden Schritte veranschaulichen, wie das Azure-Portal zu verwenden, um eine Übung in Azure DevTest Kursen zu erstellen. 

1. Melden Sie sich mit dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)aus.

1. Wählen Sie **Weitere Dienste**aus, und wählen Sie dann in der Liste **DevTest Labs** .

1. Wählen Sie auf der Blade **DevTest Labs** **Hinzufügen**aus.

    ![Hinzufügen einer Übung](./media/devtest-lab-create-lab/add-lab-button.png)

1. Klicken Sie auf das **Erstellen einer DevTest Übung** Blade:

    1. Geben Sie einen **Übung Namen** für die neue Übung aus.
    
    1. Wählen Sie das **Abonnement** , mit der Übung zugeordnet werden soll.
    
    1. Wählen Sie einen **Speicherort** zum Speichern der Übung aus.
    
    1. Wählen Sie **Automatische-war(en)** angeben, wenn Sie aktivieren möchten – und legen Sie die Parameter für - virtuellen Computern ist die automatische beendet alle der Übung.
    
    1. Wählen Sie den **Typ von Speicher** zu den Datenträger Speicherplatz für die Übung der virtuelle Computer anzugeben. 
    
    1. Wählen Sie auf **Erstellen**.

    ![Erstellen einer Blades Übung](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Übung erstellt haben, werden hier einige weitere mögliche Schritte aus:

- [Sicherer Zugriff mit einem Kurs](devtest-lab-add-devtest-user.md).

- [Übung Richtlinien festlegen](devtest-lab-set-lab-policy.md).

- [Erstellen einer Vorlage Übung](devtest-lab-create-template.md).

- [Erstellen von benutzerdefinierten Elemente für Ihre virtuellen Computer](devtest-lab-artifact-author.md).

- [Hinzufügen eines virtuellen Computers mit Elemente mit einem Kurs](devtest-lab-add-vm-with-artifacts.md).