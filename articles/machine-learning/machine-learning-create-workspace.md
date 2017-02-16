<properties
    pageTitle="Erstellen eines Arbeitsbereichs maschinellen Learning | Microsoft Azure"
    description="So erstellen Sie einen Arbeitsbereich für Azure Computer Learning Studio"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="garye;bradsev;ahgyger"/>


# <a name="create-and-share-an-azure-machine-learning-workspace"></a>Erstellen und Freigeben von einem Azure maschinellen Learning-Arbeitsbereich

Diese Menü enthält Links zu Themen zum Einrichten der verschiedenen Wissenschaft-Umgebungen durch Cortana Analytics Prozess (FESTSTELLTASTE) verwendet werden.

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Um Azure maschinellen Learning Studio verwenden zu können, müssen Sie einen Computer Learning Arbeitsbereich haben. In diesem Arbeitsbereich enthält die Tools, die Sie erstellen, verwalten und Versuche veröffentlichen müssen.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="to-create-a-workspace"></a>Zum Erstellen eines Arbeitsbereichs

1. Anmelden Sie bei der [klassischen Microsoft Azure-Portal].

> [AZURE.NOTE] Zum Anmelden, müssen Sie ein Azure-Abonnementadministrator sein. Wird der Besitzer eines Arbeitsbereichs maschinellen Learning [klassischen Microsoft Azure-Portal]nicht Zugriff gewähren. Weitere Informationen hierzu finden Sie unter [Berechtigungen der Azure-Abonnement-Administrator und Besitzer des Arbeitsbereichs](#subscriptionvsworkspace) .

2. Klicken Sie im Bereich Microsoft Azure Services auf **Computer Schulung**.

    ![Computer-Learning-Dienst][1]

3. Klicken Sie auf **+ neue** am unteren Fensterrand.
4. Klicken Sie auf **DATA SERVICES**, dann **Computer LEARNING**, dann **schnellen Erstellen**.

    ![Schnellen Erstellen eines neuen Arbeitsbereichs][3]

5. Geben Sie einen **ARBEITSBEREICHSNAMEN** für den Arbeitsbereich ein.
6. Geben Sie den Azure **Speicherort**, geben Sie ein vorhandenes Azure- **Speicher-Konto** , oder wählen Sie zum Erstellen eines neuen Kontos **Erstellen eines neuen Kontos mit Speicher** aus.
7. Klicken Sie auf **einen ML ARBEITSBEREICH erstellen**.

Nachdem Ihr Computer Learning-Arbeitsbereich erstellt wurde, sehen Sie sich, dass sie auf der Seite **Computer lernen** aufgeführt.

## <a name="sharing-an-azure-machine-learning-workspace"></a>Einen Arbeitsbereich Azure maschinellen Learning Freigabe

Nachdem ein Computer Learning-Arbeitsbereich erstellt wurde, können Sie Benutzer zu Ihrer Access-Arbeitsbereich und freigeben auf dem Arbeitsbereich und alle zugehörigen Versuche einladen. Wir unterstützen zwei Rollen Benutzer:

- **Benutzer** - Arbeitsbereich Benutzer kann erstellen, öffnen, ändern und Löschen von Datasets, Versuche und Webdienste im Arbeitsbereich.
- **Besitzer** - ein Besitzer einladen, entfernen und Benutzer mit Zugriff auf den Arbeitsbereich, darüber hinaus zu ein Benutzer Möglichkeiten auflisten kann. Er auch haben Zugriff auf Notizbücher.

### <a name="to-share-a-workspace"></a>Freigeben ein Arbeitsbereichs
1. Anmelden Sie bei [maschinellen Learning Studio]
2. Klicken Sie im Bereich Computer Learning Studio auf **EINSTELLUNGEN**
3. Klicken Sie auf **Benutzer**
4. Klicken Sie auf **Weitere Benutzer EINLADEN**

    ![Einladen von Benutzern weitere][4]

5. Geben Sie einen oder mehrere e-Mail-Adresse ein. Benötigt der Benutzer nur ein gültiges Microsoft-Konto (z. B. name@outlook.com) oder ein Organisations-Konto (aus dem Azure Active Directory).
6. Klicken Sie auf die Schaltfläche überprüfen.

Jeder Benutzer, die Sie hinzugefügt haben, erhalten eine e-Mail mit-Anweisung Freigegebener Arbeitsbereich anmelden.

Informationen zum Verwalten von dem Arbeitsbereich finden Sie unter [Verwalten ein Azure maschinellen Learning-Arbeitsbereich].
Wenn Sie ein Problem beim Erstellen des Arbeitsbereichs auftreten, lesen [Problembehandlung Leitfaden: Erstellen und Verbinden mit einem Computer Learning-Arbeitsbereich].

## <a name="a-namesubscriptionvsworkspaceaprivileges-of-azure-subscription-administrator-and-of-workspace-owner"></a><a name="subscriptionvsworkspace"></a>Berechtigungen Azure Abonnementadministrator und der Besitzer des Arbeitsbereichs

Es folgt eine Erläuterung des Unterschieds zwischen Abonnementadministrator Azure-und einen Besitzer des Arbeitsbereichs Tabelle.

| Aktionen                   | Azure Abonnementadministrator | Besitzer des Arbeitsbereichs  |
| --------------            |:------------------------:| :----------------:|
| Access [klassischen Microsoft Azure-portal]| Ja         | Nein                |
| Erstellen eines neuen Arbeitsbereichs                 | Ja         | Nein                |
| Löschen eines Arbeitsbereichs                     | Ja         | Nein                |
| Hinzufügen von Endpunkt zu einem Webdienst          | Ja         | Nein                |
| Löschen der Endpunkt von einem Webdienst     | Ja         | Nein                |
| Ändern der Parallelität von einem Webdienst   | Ja         | Nein                |
| Access [Computer Learning Studio]       | Nicht *        | Ja               |


> [AZURE.NOTE] * Azure-Abonnementadministrator automatisch hinzugefügt wird der Arbeitsbereich er als Besitzer-Arbeitsbereich erstellt. Jedoch nicht einfach wird ein Abonnementadministrator Azure-ihm Zugriff auf eine beliebige Arbeitsbereich unter diesen Abonnement gewähren.

<!-- ![List of Machine Learning workspaces][2] -->

<!--Anchors-->
[To create a workspace]: #createworkspace

<!--Image references-->
[1]: media/machine-learning-create-workspace/cw1.png
[2]: media/machine-learning-create-workspace/cw2.png
[3]: media/machine-learning-create-workspace/cw4.png
[4]: media/machine-learning-create-workspace/cw5.png


<!--Link references-->
[Einen Arbeitsbereich Azure maschinellen Learning verwalten]: machine-learning-manage-workspace.md
[Leitfaden zur Problembehandlung: Erstellen und Verbinden mit einem Computer Learning-Arbeitsbereich]: machine-learning-troubleshooting-creating-ml-workspace.md
[Maschinelle Learning Studio]: https://studio.azureml.net/  
[Klassische Microsoft Azure-portal]: https://manage.windowsazure.com/
