<properties
    pageTitle="Verwalten von einem Webdienst mithilfe des Portals Azure maschinellen Learning Web Serivces | Microsoft Azure"
    description="Verwalten des Zugriffs auf Azure maschinellen Learning Arbeitsbereiche, bereitstellen und Verwalten von ML API-Webdiensten"
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>


# <a name="manage-a-web-service-using-the-azure-machine-learning-web-services-portal"></a>Verwalten von einem Webdienst im Portal Azure maschinellen Learning Web Services verwenden

Sie können Computer Learning neuen und klassischen Webdienste mit Microsoft Azure maschinellen Learning-Webdienste-Portal verwalten. Da klassischen Webdienste und neue Webdienste auf verschiedenen zugrunde liegenden Technologien basieren, müssen Sie für jede von ihnen weicht Verwaltungsfunktionen.

Im Portal maschinellen Learning-Webdiensten können Sie folgende Aktionen ausführen:

- Überwachen Sie, wie der Webdienst verwendet wird.
- Konfigurieren die Beschreibung, aktualisieren Sie die Taste für das Web service (nur neue), aktualisieren Sie Ihre Speicher Konto Key (nur neue) Protokollierung aktivieren, und aktivieren oder Deaktivieren von Beispieldaten.
- Löschen Sie den Webdienst.
- Erstellen, löschen oder aktualisieren Abrechnung Pläne (nur neu).
- Hinzufügen und Löschen von Endpunkten (nur klassisch)

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="manage-new-web-services"></a>Neue Webdienste verwalten

So verwalten Sie Ihre neue Webdienste

1.  Melden Sie sich mit Ihrem Konto Microsoft Azure-Portal an [Microsoft Azure maschinellen Learning-Webdiensten](https://services.azureml.net/quickstart) : Verwenden Sie das Konto aus, das das Abonnement Azure zugeordnet ist.
2.  Klicken Sie im Menü auf **Webdienste**.

Zeigt eine Liste der bereitgestellten Webdienste für Ihr Abonnement an. 

Klicken Sie zum Verwalten von eines Webdiensts auf Webdienste. Von der Seite Webdienste können Sie folgende Aktionen ausführen:

- Klicken Sie auf den Webdienst, um ihn zu verwalten.
- Klicken Sie auf der Abrechnung Planen für den Webdienst zu aktualisieren.
- Löschen von einem Webdienst.
- Kopieren von einem Webdienst, und stellen Sie es in eine andere Region.

Wenn Sie einen Webdienst klicken, wird die Dienst Schnellstart-Webseite geöffnet. Die Dienst Schnellstart-Webseite besteht aus zwei Menüoptionen zur Verfügung, mit die Sie Ihren Webdienst verwalten können:

- **DASHBOARD** - ermöglicht das Anzeigen der Verwendung von Diensten.
- **Konfigurieren** - können Sie beschreibenden Text hinzufügen, aktualisieren Sie den Schlüssel für das Speicherkonto verknüpft ist, mit dem Webdienst, und aktivieren oder Deaktivieren von Beispieldaten.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Überwachung, wie der Webdienst verwendet wird ###

Klicken Sie auf die Registerkarte **DASHBOARD** .

Aus dem Dashboard können Sie allgemeine Verwendung des Web Service über einen Zeitraum anzeigen. Sie können den Zeitraum aus dem Zeitraum Dropdownmenü in der oberen rechten Ecke der Verwendung Diagramme anzeigen auswählen. Das Dashboard zeigt die folgende Informationen:

- **Anfragen im Zeitverlauf** zeigt ein Diagramm Schritt für die Anzahl der Anfragen über den ausgewählten Zeitraum an. Es kann helfen, festzustellen, ob Sie Spitzen Verwendung auftrat.
- **Anforderung-Antwort Anfragen** zeigt die Gesamtzahl der Anforderung / Antwort-Anrufe, die der Dienst erhalten haben, über den ausgewählten Zeitraum und wie viele fehlgeschlagen ist.
- **Durchschnittliche Anforderung-Antwort zu berechnen** , zeigt einen Mittelwert der Zeit zum Ausführen der empfangenen Besprechungsanfragen erforderlich sind.
- **Stapel Anfragen** zeigt die Gesamtzahl der Stapel Anfragen der Dienst erhalten haben, über den ausgewählten Zeitraum und wie viele fehlgeschlagen ist.
- **Durchschnittliche Wartezeit für Position** zeigt einen Mittelwert der Zeit zum Ausführen der empfangenen Besprechungsanfragen erforderlich sind.
- **Fehler** zeigt die aggregate Anzahl der aufgetretene auf Anrufe an den Webdienst.
- **Kosten für Services** zeigt die Gebühren für den den Plan, der mit dem Dienst verbunden ist.

### <a name="configuring-the-web-service"></a>Konfigurieren des Webdiensts ###

Klicken Sie auf die Menüoption **Konfigurieren** .

Sie können die folgenden Eigenschaften aktualisieren:

* **Beschreibung** können Sie eine Beschreibung für den Webdienst eingeben.
* **Titel** ermöglicht es Ihnen, einen Titel für den Webdienst eingeben.
* **Tasten** können Sie Ihre primären und sekundären API Keys drehen.
* **Kontoschlüssel Speicher** können Sie die Taste für das Speicherkonto zugeordnet des Webdiensts geändert zu aktualisieren. 
* **Aktivieren von Beispieldaten** können Sie Beispieldaten bereitstellen, die Sie zum Testen des Anforderung / Antwort-Diensts verwenden können. Wenn Sie den Webdienst in Computer Learning Studio erstellt, die Beispieldaten aus den Daten der verwendeten zum Schulen von Modell stammt. Wenn Sie den Dienst programmgesteuert erstellt haben, werden die Daten aus den Beispieldaten übernommen, die Sie als Teil des JSON-Pakets bereitgestellt.

### <a name="managing-billing-plans"></a>Verwalten der Abrechnung Pläne ###

Klicken Sie auf die Menüoption **Pläne** aus dem Web Services Schnellstart. Klicken Sie auf den Plan mit bestimmten Webdienst verknüpft ist dieser Plan verwalten.

* **Neu** können Sie einen neuen Plan zu erstellen.
* **Hinzufügen/Entfernen Plan Instanz** können Sie einen vorhandenen Plan zum Hinzufügen von Kapazität "Skalieren".
* **Upgrade/DownGrade** können Sie "zum Hinzufügen von Kapazität einen vorhandenen Plan Skalieren von".
* **Löschen** , können Sie einen Plan löschen.

Klicken Sie auf einen Plan, um deren Dashboard anzuzeigen. Das Dashboard bietet Ihnen eine Momentaufnahme oder Plan Verwendung über einen ausgewählten Zeitraum. Um den Zeitraum anzeigen auszuwählen, klicken Sie auf den Dropdownpfeil **Punkt** in der oberen rechten Ecke Dashboard. 

Das Plan Dashboard bietet die folgende Informationen:

* **Beschreibung der Plan** zeigt Informationen zu den Kosten und Kapazität, die mit dem Plan verknüpft ist.
* **Plan Verwendung** zeigt die Anzahl der Transaktionen und Berechnen von Stunden, die für den Plan belastet wurde haben.
* **Webdienste** zeigt die Anzahl der Webdienste, die diesem Plan verwendet wird.
* **Top Web Service durch Anrufe** zeigt die obersten vier-Webdiensten, die Aufrufe ausführen, die für den Plan belastet werden.
* **Top-Webdiensten durch berechnen Std.** zeigt die obersten vier Webdienste mit berechnen Ressourcen, die für den Plan belastet werden.

## <a name="manage-classic-web-services"></a>Klassische Webdienste verwalten

> [AZURE.NOTE] Die Verfahren in diesem Abschnitt sind für das Verwalten von Classic-Webdiensten über das Portal Azure maschinellen Learning-Webdiensten relevant. Informationen zum Verwalten von klassischen-Webdienste über den Computer Learning Studio und Azure klassischen Portal finden Sie unter [Verwalten ein Azure maschinellen Learning-Arbeitsbereich](machine-learning-manage-workspace.md).

So verwalten Sie Ihre klassischen Webdienste

1.  Melden Sie sich mit Ihrem Konto Microsoft Azure-Portal an [Microsoft Azure maschinellen Learning-Webdiensten](https://services.azureml.net/quickstart) : Verwenden Sie das Konto aus, das das Abonnement Azure zugeordnet ist.
2.  Klicken Sie im Menü auf **Klassische Webdienste**.

Klicken Sie zum Verwalten von einem Webdienst klassischen auf **Klassische Webdienste**. Von der Seite klassischen Webdienste können Sie folgende Aktionen ausführen:

- Klicken Sie auf den Webdienst zum Anzeigen der zugeordneten Endpunkte.
- Löschen von einem Webdienst.

Wenn Sie einen klassischen Webdienst verwalten, verwalten Sie jeder der Endpunkte getrennt. Wenn Sie einen Webdienst-Webdiensten Seite klicken, wird die Liste der Dienst zugeordneten Endpunkte geöffnet. 

Sie können auf der Seite klassischen Webdienst-Endpunkt hinzufügen und Löschen von Endpunkten-Dienst. Weitere Informationen zum Hinzufügen von Endpunkten finden Sie unter [Erstellen von Endpunkten](machine-learning-create-endpoint.md).

Klicken Sie auf einen der Endpunkte zum Dienst Schnellstart Webseite zu öffnen. Es gibt zwei Menüoptionen zur Verfügung, mit die Sie Ihren Webdienst verwalten können, klicken Sie auf der Seite Schnellstart:

- **DASHBOARD** - ermöglicht das Anzeigen der Verwendung von Diensten.
- **Konfigurieren** - können Sie einen beschreibenden Text hinzugefügt werden soll, aktivieren Fehlerprotokolle aus, und deaktivieren, aktualisieren Sie die Taste für die Speicherung Konto verknüpft ist, mit dem Webdienst aktivieren und Deaktivieren von Beispieldaten.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Überwachung, wie der Webdienst verwendet wird ###

Klicken Sie auf die Registerkarte **DASHBOARD** .

Aus dem Dashboard können Sie allgemeine Verwendung des Web Service über einen Zeitraum anzeigen. Sie können den Zeitraum aus dem Zeitraum Dropdownmenü in der oberen rechten Ecke der Verwendung Diagramme anzeigen auswählen. Das Dashboard zeigt die folgende Informationen:

- **Anfragen im Zeitverlauf** zeigt ein Diagramm Schritt für die Anzahl der Anfragen über den ausgewählten Zeitraum an. Es kann helfen, festzustellen, ob Sie Spitzen Verwendung auftrat.
- **Anforderung-Antwort Anfragen** zeigt die Gesamtzahl der Anforderung / Antwort-Anrufe, die der Dienst erhalten haben, über den ausgewählten Zeitraum und wie viele fehlgeschlagen ist.
- **Durchschnittliche Anforderung-Antwort zu berechnen** , zeigt einen Mittelwert der Zeit zum Ausführen der empfangenen Besprechungsanfragen erforderlich sind.
- **Stapel Anfragen** zeigt die Gesamtzahl der Stapel Anfragen der Dienst erhalten haben, über den ausgewählten Zeitraum und wie viele fehlgeschlagen ist.
- **Durchschnittliche Wartezeit für Position** zeigt einen Mittelwert der Zeit zum Ausführen der empfangenen Besprechungsanfragen erforderlich sind.
- **Fehler** zeigt die aggregate Anzahl der aufgetretene auf Anrufe an den Webdienst.
- **Kosten für Services** zeigt die Gebühren für den den Plan, der mit dem Dienst verbunden ist.

### <a name="configuring-the-web-service"></a>Konfigurieren des Webdiensts ###

Klicken Sie auf die Menüoption **Konfigurieren** .

Sie können die folgenden Eigenschaften aktualisieren:

* **Beschreibung** können Sie eine Beschreibung für den Webdienst eingeben. Beschreibung ist ein erforderliches Feld.
* **Protokollierung** , können Sie aktivieren oder Deaktivieren der Protokollierung auf den Endpunkt zurück. Weitere Informationen zur Protokollierung finden Sie unter aktivieren [für maschinelle Learning-Webdiensten Protokollierung](machine-learning-web-services-logging.md).
* **Aktivieren von Beispieldaten** können Sie Beispieldaten bereitstellen, die Sie zum Testen des Anforderung / Antwort-Diensts verwenden können. Wenn Sie den Webdienst in Computer Learning Studio erstellt, die Beispieldaten aus den Daten der verwendeten zum Schulen von Modell stammt. Wenn Sie den Dienst programmgesteuert erstellt haben, werden die Daten aus den Beispieldaten übernommen, die Sie als Teil des JSON-Pakets bereitgestellt.

## <a name="grant-or-suspend-access-to-web-services-for-users-in-the-portal"></a>Erteilen oder den Zugriff auf Webdienste für Benutzer im Portal anhalten

Verwenden des klassischen Azure-Portals an, können Sie zulassen oder bestimmten Benutzern den Zugriff verweigert.

### <a name="access-for-users-of-new-web-services"></a>Zugriff für Benutzer der neuen Webdienste

Wenn andere Benutzer für die Arbeit mit Ihrer Webdienste im Portal Azure maschinellen Learning-Webdiensten aktivieren möchten, müssen Sie sie für Ihr Abonnement Azure als co-Administratoren hinzufügen.

Melden Sie sich mit Ihrem Microsoft Azure-Konto [Azure klassischen Portal](https://manage.windowsazure.com/) : Verwenden Sie das Konto aus, das das Abonnement Azure zugeordnet ist.

1. Klicken Sie im Navigationsbereich klicken Sie auf **Einstellungen**und dann auf **Administratoren**.
2. Klicken Sie am unteren Rand des Fensters auf **Hinzufügen**. 
3. Geben Sie die e-Mail-Adresse der Person, die Sie als Administrator gemeinsame hinzufügen, und wählen Sie dann das Abonnement, das Sie für den Zugriff auf den gemeinsame Administrator möchten möchten, klicken Sie im Dialogfeld ADD A CO-ADMINISTRATOR.
4. Klicken Sie auf **Speichern**.

### <a name="access-for-users-of-classic-web-services"></a>Zugriff für Benutzer von klassischen Web services

So verwalten Sie einen Arbeitsbereich

Melden Sie sich mit Ihrem Microsoft Azure-Konto [Azure klassischen Portal](https://manage.windowsazure.com/) : Verwenden Sie das Konto aus, das das Abonnement Azure zugeordnet ist.

1. Klicken Sie im Bereich Microsoft Azure Services auf **Computer Schulung**.
1. Klicken Sie auf den Arbeitsbereich, die, den Sie verwalten möchten.
1. Klicken Sie auf die Registerkarte **Konfigurieren** .

Auf der Konfigurationsregisterkarte können Sie Zugriff auf den Computer Learning-Arbeitsbereich anhalten, indem Sie auf **Verweigern**. Benutzer werden nicht mehr im Arbeitsbereich in Computer Learning Studio zu öffnen. Klicken Sie auf **Zulassen**, um den Zugriff wiederherzustellen.

Für bestimmte Benutzer:

Um weitere Konten verwalten, die Zugriff auf den Arbeitsbereich maschinellen Learning Studio haben, klicken Sie auf **Anmelden bei ML Studio** auf der Registerkarte **DASHBOARD** . Den Arbeitsbereich im Computer Learning Studio geöffnet. Weitere Schritte klicken Sie auf die Registerkarte **EINSTELLUNGEN** und dann auf **Benutzer**. Sie können **Mehr Benutzer EINLADEN** , um Benutzern Zugriff auf den Arbeitsbereich, oder wählen Sie einen Benutzer aus, und klicken Sie auf **Entfernen**klicken.

> [AZURE.NOTE] Der Link zum **Anmelden bei ML Studio** öffnet maschinellen Learning Studio mithilfe der Microsoft Account in angemeldet sind. Die Account Microsoft Azure klassischen-Portal anmelden, zum Erstellen eines Arbeitsbereichs Inanspruchnahme verfügt über die Berechtigung zum Öffnen dieses Arbeitsbereichs nicht automatisch. Um einen Arbeitsbereich zu öffnen, Sie müssen werden angemeldet, um die Microsoft-Account, die als Besitzer des Arbeitsbereichs definiert wurde, oder müssen Sie eine Einladung von der Besitzer des Arbeitsbereichs beitreten zu erhalten.
