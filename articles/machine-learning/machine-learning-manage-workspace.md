<properties
    pageTitle="Verwalten von einem Computer Learning Arbeitsbereich | Microsoft Azure"
    description="Verwalten des Zugriffs auf Azure maschinellen Learning Arbeitsbereiche, bereitstellen und Verwalten von ML API-Webdiensten"
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
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="manage-an-azure-machine-learning-workspace"></a>Einen Arbeitsbereich Azure maschinellen Learning verwalten

>[AZURE.NOTE] Die Verfahren in diesem Artikel gelten für Azure maschinellen Learning klassischen-Webdiensten. Informationen zum Verwalten von Webdienste im Portal maschinellen Learning-Webdiensten finden Sie unter [Verwalten von einem Webdienst im Portal Azure maschinellen Learning Web Services verwenden](machine-learning-manage-new-webservice.md).

Verwenden das klassische Azure-Portal an, können Sie Ihre Computer Learning Arbeitsbereiche zu verwalten:

- Überwachen des Arbeitsbereichs wie verwendet wird
- Konfigurieren Sie den Arbeitsbereich, um den Zugriff gewähren oder verweigern
- Verwalten von-Webdiensten im Arbeitsbereich erstellt
- Löschen des Arbeitsbereichs

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Darüber hinaus bietet die Registerkarte Dashboard einen Überblick über die Verwendung der Arbeitsbereich und eine kurze Übersicht über Ihre Arbeitsbereichsinformationen.  

> [AZURE.TIP] In Azure maschinellen Learning Studio, klicken Sie auf die Registerkarte **WEB SERVICES** können Sie hinzufügen, aktualisieren und Löschen von einem Computer Learning-Webdienst.

So verwalten Sie einen Arbeitsbereich

1.  Melden Sie sich mit Ihrem Microsoft Azure-Konto [Azure klassischen Portal](https://manage.windowsazure.com/) : Verwenden Sie das Konto aus, das das Abonnement Azure zugeordnet ist.
2.  Klicken Sie im Bereich Microsoft Azure Services auf **Computer Schulung**.
3.  Klicken Sie auf den Arbeitsbereich, die, den Sie verwalten möchten.

Die Arbeitsbereichsseite besteht aus drei Registerkarten:

- **DASHBOARD** - ermöglichen Ihnen das Anzeigen der Verwendung der Arbeitsbereich und Informationen
- **Konfigurieren** - ermöglicht Ihnen den Zugriff auf den Arbeitsbereich verwalten
- **Webdienste** - ermöglicht es Ihnen, Web Services verwalten, die aus diesem Arbeitsbereich veröffentlicht wurden

## <a name="to-monitor-how-the-workspace-is-being-used"></a>Überwachen des Arbeitsbereichs wie verwendet wird

Klicken Sie auf die Registerkarte **DASHBOARD** .

Aus dem Dashboard können Sie allgemeine Verwendung Ihres Arbeitsbereichs anzeigen und erhalten eine kurze Übersicht über Arbeitsbereichsinformationen.

- Das Diagramm **berechnen** zeigt die berechnen-Ressourcen, die von den Arbeitsbereich verwendet wird. Sie können die Anzeige von relativen oder absoluten Werten ändern, und Sie können den Zeitrahmen, die im Diagramm angezeigten ändern.
- **Übersicht über die Verwendung** zeigt Azure-Speicher von den Arbeitsbereich verwendet wird.
- **Den ersten Blick** bietet eine Zusammenfassung der Arbeitsbereichsinformationen und nützliche Links.

> [AZURE.NOTE] Der Link zum **Anmelden bei ML Studio** öffnet maschinellen Learning Studio mithilfe der Microsoft Account in angemeldet sind. Die Account Microsoft Azure klassischen-Portal anmelden, zum Erstellen eines Arbeitsbereichs Inanspruchnahme verfügt über die Berechtigung zum Öffnen dieses Arbeitsbereichs nicht automatisch. Um einen Arbeitsbereich zu öffnen, Sie müssen werden angemeldet, um die Microsoft-Account, die als Besitzer des Arbeitsbereichs definiert wurde, oder müssen Sie eine Einladung von der Besitzer des Arbeitsbereichs beitreten zu erhalten.


## <a name="to-grant-or-suspend-access-for-users"></a>Erteilen oder Zugriff für Benutzer anhalten ##

Klicken Sie auf die Registerkarte **Konfigurieren** .

Auf der Konfigurationsregisterkarte können Sie folgende Aktionen ausführen:

- Brechen Sie Zugriff auf den Computer Learning-Arbeitsbereich auf verweigern. Benutzer werden nicht mehr im Arbeitsbereich in Computer Learning Studio zu öffnen. Klicken Sie auf zulassen, um den Zugriff wiederherzustellen.

Um weitere Konten verwalten, die Zugriff auf den Arbeitsbereich maschinellen Learning Studio haben, klicken Sie auf **Anmelden bei ML Studio** auf der Registerkarte **DASHBOARD** (siehe Anmerkung vorangehende hinsichtlich **Anmelden bei ML Studio**). Den Arbeitsbereich im Computer Learning Studio geöffnet. Weitere Schritte klicken Sie auf die Registerkarte **EINSTELLUNGEN** und dann auf **Benutzer**. Sie können **Mehr Benutzer EINLADEN** , um Benutzern Zugriff auf den Arbeitsbereich, oder wählen Sie einen Benutzer aus, und klicken Sie auf **Entfernen**klicken.


## <a name="to-manage-web-services-in-this-workspace"></a>In diesem Arbeitsbereich Webdienste verwalten

Klicken Sie auf der Registerkarte **Webdienste** .

Zeigt eine Liste der Webdienste veröffentlicht aus diesem Arbeitsbereich an.
Klicken Sie zum Verwalten von eines Webdiensts auf den Namen in der Liste, um den Dienst Webseite zu öffnen.

Möglicherweise müssen Sie ein Webdienst einen oder mehrere Endpunkte definiert.

- Sie können mehrere Endpunkte zusätzlich zu den Endpunkt "Standard" definieren. Um den Endpunkt hinzuzufügen, klicken Sie auf **Verwalten Endpunkte** am unteren Rand des Dashboards um Azure maschinellen Learning-Webdienste-Portal zu öffnen.

- So löschen Sie einen Endpunkt (Sie können nicht den Endpunkt "Standard" löschen), klicken Sie auf das Kontrollkästchen am Anfang der Endpunktzeile, und klicken Sie auf **Löschen**. Dadurch wird den Endpunkt vom Webdienst entfernt.

    > [AZURE.NOTE] Wenn die Anwendung den Webdienst-Endpunkt verwendet wird, wenn der Endpunkt gelöscht wird, erhalten die Anwendung einen Fehler das nächste Mal versucht wird, auf den Dienst zugreifen.

Klicken Sie auf den Namen einer Webdienst-Endpunkt um ihn zu öffnen. 

Aus dem Dashboard können Sie allgemeine Verwendung des Web Service über einen Zeitraum anzeigen. Sie können den Zeitraum aus dem Zeitraum Dropdownmenü in der oberen rechten Ecke der Verwendung Diagramme anzeigen auswählen. Das Dashboard zeigt die folgende Informationen:

- **Anfragen im Zeitverlauf** zeigt ein Diagramm Schritt für die Anzahl der Anfragen über den ausgewählten Zeitraum an. Es kann helfen, festzustellen, ob Sie Spitzen Verwendung auftrat.
- **Anforderung-Antwort Anfragen** zeigt die Gesamtzahl der Anforderung / Antwort-Anrufe, die der Dienst erhalten haben, über den ausgewählten Zeitraum und wie viele fehlgeschlagen ist.
- **Durchschnittliche Anforderung-Antwort zu berechnen** , zeigt einen Mittelwert der Zeit zum Ausführen der empfangenen Besprechungsanfragen erforderlich sind.
- **Stapel Anfragen** zeigt die Gesamtzahl der Stapel Anfragen der Dienst erhalten haben, über den ausgewählten Zeitraum und wie viele fehlgeschlagen ist.
- **Durchschnittliche Wartezeit für Position** zeigt einen Mittelwert der Zeit zum Ausführen der empfangenen Besprechungsanfragen erforderlich sind.
- **Fehler** zeigt die aggregate Anzahl der aufgetretene auf Anrufe an den Webdienst.
- **Kosten für Services** zeigt die Gebühren für den den Plan, der mit dem Dienst verbunden ist.

Von der Seite konfigurieren können Sie die folgenden Eigenschaften aktualisieren:

* **Beschreibung** können Sie eine Beschreibung für den Webdienst eingeben. Beschreibung ist ein erforderliches Feld.
* **Protokollierung** , können Sie aktivieren oder Deaktivieren der Protokollierung auf den Endpunkt zurück. Weitere Informationen zur Protokollierung finden Sie unter aktivieren [für maschinelle Learning-Webdiensten Protokollierung](machine-learning-web-services-logging.md).
* **Aktivieren von Beispieldaten** können Sie Beispieldaten bereitstellen, die Sie zum Testen des Anforderung / Antwort-Diensts verwenden können. Wenn Sie den Webdienst in Computer Learning Studio erstellt, die Beispieldaten aus den Daten der verwendeten zum Schulen von Modell stammt. Wenn Sie den Dienst programmgesteuert erstellt haben, werden die Daten aus den Beispieldaten übernommen, die Sie als Teil des JSON-Pakets bereitgestellt.

[consume]: machine-learning-consume-web-services.md
[marketplace]: machine-learning-publish-web-service-to-azure-marketplace.md
