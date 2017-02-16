<properties
    pageTitle="Behandeln von Problemen mit: Erstellen und Verbinden mit einem Computer Learning Arbeitsbereich | Microsoft Azure"
    description="Lösungen für häufige Probleme in erstellen und Herstellen einer Verbindung mit einem Azure maschinellen Learning-Arbeitsbereich"
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
    ms.date="09/09/2016"
    ms.author="garye"/>


# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Leitfaden zur Problembehandlung: Erstellen und Verbinden mit einem Computer Learning-Arbeitsbereich

Dieses Handbuch bietet Lösungen für einige häufig Probleme beim Einrichten der Arbeitsbereiche Azure maschinellen Schulung.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Besitzer des Arbeitsbereichs

Wenn Sie einen neuen Computer Learning-Arbeitsbereich erstellen, die ID, die Sie im Feld Besitzer des ARBEITSBEREICHS eingeben muss ein gültiges Microsoft-Konto (vormals Windows Live ID), z. B. john-contoso@live.com oder john-contoso@hotmail.com. Es kann kein nicht-Microsoft-Konto, beispielsweise Ihre Firmen-e-Mail-Konto sein. Wenn Sie ein kostenloses Microsoft-Konto erstellen möchten, wechseln Sie zu [www.live.com](http://www.live.com).

Beachten Sie, dass das Konto ein, die Sie mit einer Azure-Portal anmelden, um den Arbeitsbereich zu erstellen nicht automatisch über die Berechtigung zum *Öffnen* dieses Arbeitsbereichs, es sei denn, Sie diesem Konto als Besitzer angeben. Zum Öffnen eines Arbeitsbereichs in Computer Learning Studio Sie müssen werden angemeldet, um die Microsoft-Account, die als Besitzer des Arbeitsbereichs definiert wurde, oder müssen Sie eine Einladung von der Besitzer des Arbeitsbereichs beitreten zu erhalten. Vom klassischen Azure-Portal können Sie jedoch *Verwalten* den Arbeitsbereich, wozu auch die Möglichkeit, ändern Sie den Besitzer und Konfigurieren von Access.

Weitere Informationen zum Verwalten von eines Arbeitsbereichs finden Sie unter [Verwalten ein Azure maschinellen Learning-Arbeitsbereich].

[Einen Arbeitsbereich Azure maschinellen Learning verwalten]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Zulässige Regionen

Computer-Schulung ist eine eingeschränkte Anzahl von Regionen derzeit verfügbar. Wenn Sie Ihr Abonnement eine der folgenden Bereiche nicht enthalten ist, wird die Fehlermeldung angezeigt, "Enthält keine Abonnements die zulässigen Regionen."

Um ein Bereich zu Ihrem Abonnement hinzugefügt werden anzufordern, wählen Sie aus dem klassischen Azure-Portal **An Microsoft Support wenden** aus, wählen Sie als den Problemtyp **Abrechnung** , und folgen Sie den Anweisungen zum Senden Ihrer Anforderung.

![Wenden Sie sich an Microsoft support][screen1]

## <a name="storage-account"></a>Speicher-Konto

Der Computer Learning-Dienst benötigt ein Speicherkonto, um Daten zu speichern. Sie können ein vorhandenes Speicherkonto verwenden, oder Sie können ein neues Speicherkonto erstellen, wenn Sie den neuen Computer Learning-Arbeitsbereich erstellen (Wenn Sie zum Erstellen eines neuen Kontos mit Speicher Kontingent haben).

<!-- These instructions no longer work, but I'm not sure what to replace them with
To see if you can create a new storage account, in the Classic Portal, go to **Settings** and then click **Usage**.
-->

![Arbeitsbereich erstellen][screen2]

Nach der neue Computer Learning-Arbeitsbereich erstellt wurde, können Sie sich anmelden maschinellen Learning Studio mithilfe des Microsoft-Kontos, das Sie als der Besitzer des Arbeitsbereichs angegeben haben. Wenn Sie die Fehlermeldung "Arbeitsbereich nicht gefunden" (ähnlich wie der folgende Screenshot) auftreten, gehen Sie folgendermaßen vor Ihrem Browsercookies löschen.

![Arbeitsbereich nicht gefunden][screen3]

**So löschen Sie den Browsercookies**

Wenn Sie Internet Explorer verwenden, klicken Sie auf die Schaltfläche **Tools** in der oberen rechten Ecke, und wählen Sie **Internetoptionen**aus.  

![Internetoptionen][screen4]

Klicken Sie unter der Registerkarte **Allgemein** auf **löschen...**

![Registerkarte Allgemein][screen5]

Stellen Sie im Dialogfeld **Browserverlauf löschen** sicher, dass **Cookies und Websitedaten** ausgewählt ist, und klicken Sie auf **Löschen**.

![Löschen von cookies][screen6]

Nachdem die Cookies gelöscht werden, starten Sie den Browser neu, und wechseln Sie dann zu der Seite [Microsoft Azure maschinellen Learning](https://studio.azureml.net) . Wenn Sie für einen Benutzernamen und Kennwort aufgefordert werden, geben Sie das Microsoft-Konto, das Sie als der Besitzer des Arbeitsbereichs angegeben haben.

Unser Ziel besteht darin die maschinelle Learning-Oberfläche wie nahtlose wie möglich machen. Bitte veröffentlichen Sie alle Kommentare und Probleme am [Computer-Schulung Azure-Forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) interessiert Sie kontinuierlich verbessern.

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
