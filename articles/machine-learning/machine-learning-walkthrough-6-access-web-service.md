<properties
    pageTitle="Schritt 6: Zugriff auf den Computer Learning Webdienst | Microsoft Azure"
    description="Schritt 6 von der Entwicklung eine exemplarische Vorgehensweise zur Vorhersage Lösung: Zugreifen auf einen aktiven Azure maschinellen Learning-Webdienst."
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
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>Exemplarische Vorgehensweise Schritt 6: Zugriff auf den Webdienst Azure maschinellen Schulung

Dies ist der letzte Schritt darin, die Anleitung, [Entwicklung einer Lösung Vorhersageanalytik Azure Computer interessante](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Erstellen eines Arbeitsbereichs Computer-Schulung](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Hochladen von vorhandenen Daten](machine-learning-walkthrough-2-upload-data.md)
3.  [Erstellen einer neuen experimentieren.](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Schulen Sie und ausgewertet werden Sie die Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Bereitstellen des Webdiensts](machine-learning-walkthrough-5-publish-web-service.md)
6.  **Zugriff auf den Webdienst**

----------

Im vorherigen Schritt in dieser Anleitung erfahren bereitgestellt wir eines Webdiensts, unsere Kreditkarte Risiko Vorhersagemodell verwendet. Jetzt müssen Benutzer in der Lage, Daten zu senden und empfangen Ergebnisse. 

Der Webdienst ist ein Azure Webdienst, der annehmen und Zurückgeben von Daten mithilfe von REST-APIs auf zwei Arten kann:  

-   **Anforderung/Antwort** - der Benutzer eine oder mehrere Datenzeilen Kreditkarte zum Dienst mithilfe einer HTTP-Protokoll sendet, und der Webdienst sendet einen oder mehrere Sätze von Ergebnissen.
-   **Stapel Ausführung** - der Benutzer speichert eine oder mehrere Zeilen mit Kreditkarte Daten in einer Azure Blob und dann den Blob-Speicherort an den Dienst gesendet. Der Dienst alle Zeilen der Daten in die Eingabewerte Blob bewertet, speichert die Ergebnisse in einem anderen Blob und gibt die URL des Container.  

Die schnellste und einfachste Möglichkeit zum Zugriff auf den Webdienst erfolgt über das Web-App-Vorlagen in der [Azure Web App-Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).
Diese Web app-Vorlagen können eine benutzerdefinierte Web app erstellen, die weiß, dass Ihre Webdiensts Eingabedaten und was zurückgeben. Sie müssen lediglich Zugriff auf Ihre Webdienst und Daten zu ermöglichen, und die Vorlage erledigt den Rest.

Weitere Informationen zum Verwenden der Web app-Vorlagen finden Sie unter [verbrauchen eines Webdiensts Azure maschinellen Learning mit einer Web app-Vorlage](machine-learning-consume-web-service-with-web-app-template.md).

Sie können auch den Webdienst Startcode stehen im R, c# und Python programming Sprachen mit Zugriff auf eine benutzerdefinierte Anwendung entwickeln.
Vollständige Details finden Sie unter [How to ein Webdiensts Azure maschinellen Learning nutzen, das von einem Computer Learning experimentieren veröffentlicht wurde](machine-learning-consume-web-services.md).
