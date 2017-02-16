<properties
    pageTitle="Gewusst wie: Bereitstellen von einem Webdienst in mehrere Regionen | Microsoft Azure"
    description="Schritte zum Bereitstellen (Kopieren) einen neuen Webdienst mit anderen Regionen."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="how-to-deploy-a-web-service-to-multiple-regions"></a>Gewusst wie: Bereitstellen von einem Webdienst auf mehrere Bereiche

Die neue Azure-Webdienste können Sie problemlos einen Webdienst in mehrere Regionen bereitstellen, ohne mehrere Abonnements oder Arbeitsbereiche. 

Preise ist Region bestimmte, daher Sie einen Abrechnung Plan für die einzelnen Regionen definieren müssen, in dem Sie den Webdienst bereitgestellt werden.

## <a name="to-create-a-plan-in-another-region"></a>So erstellen Sie einen Plan in einer anderen region

1. Melden Sie sich bei [Microsoft Azure maschinellen Learning-Webdiensten](https://services.azureml.net/).
2. Klicken Sie auf die Menüoption **Pläne** .
3. Klicken Sie auf die Pläne über die Seite "anzeigen" klicken Sie auf **neu**.
4. Wählen Sie aus der Dropdownliste den **Abonnement** das Abonnement in dem neue Plan gespeichert werden soll.
5. Wählen Sie aus der Dropdownliste den **Region** einen Bereich für den neuen Plan ein. Die ' Planoptionen ' für den ausgewählten Bereich werden im Abschnitt **Optionen planen** der Seite angezeigt.
6. Wählen Sie aus der Dropdownliste den **Ressourcengruppe** eine Ressourcengruppe für den Plan aus. Aus Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwalten von Azure Ressourcen über Portal](../azure-portal/resource-group-portal.md).
7. Geben Sie im **Plan Name** den Namen des Plans ein.
8. Klicken Sie unter **Planoptionen**auf der Abrechnung Ebene für den neuen Plan.
9. Klicken Sie auf **Erstellen**.


## <a name="deploying-the-web-service-to-another-region"></a>Bereitstellen von den Webdienst in eine andere region

1. Klicken Sie auf die Menüoption **Webdienste** .
2. Wählen Sie den Webdienst, der Sie eine neue Region bereitstellen.
3. Klicken Sie auf **Kopieren**.
4. Geben Sie im Feld **Name der Web-Dienst**einen neuen Namen für den Webdienst ein.
5. Geben Sie im Feld **Beschreibung Webdienst**eine Beschreibung für den Webdienst ein.
6. Wählen Sie aus der Dropdownliste den **Abonnement** das Abonnement, in dem der neuen Webdienst gespeichert werden.
7. Wählen Sie aus der Dropdownliste den **Ressourcengruppe** eine Ressourcengruppe für den Webdienst ein. Aus Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwalten von Azure Ressourcen über Portal](../azure-portal/resource-group-portal.md).
8. Wählen Sie aus der Dropdownliste den **Bereich** des Bereichs, in dem den Webdienst bereitgestellt.
9. Wählen Sie aus der Dropdownliste den **Speicher-Konto** ein Speicherkonto, in dem den Webdienst gespeichert.
10. Wählen Sie aus der Dropdownliste den **Preisplan** einen Plan in der Region, das Sie in Schritt 8 ausgewählt haben.
11. Klicken Sie auf **Kopieren**.

