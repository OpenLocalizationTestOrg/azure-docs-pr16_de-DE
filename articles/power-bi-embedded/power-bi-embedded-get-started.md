<properties
   pageTitle="Erste Schritte mit Microsoft Power BI eingebettete"
   description="Power BI eingebettete, interaktive Power BI-Berichten in Business Intelligence-Anwendung hinzufügen"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="get-started-with-microsoft-power-bi-embedded"></a>Erste Schritte mit Microsoft Power BI eingebettete

**Power BI eingebettete** ist eine Azure-Dienst, mit dem Entwickler interaktive Power BI-Berichte in ihre eigenen Programme hinzufügen kann. **Power BI eingebettete** funktioniert mit vorhandenen Applikationen ohne Überarbeitung, oder melden Sie sich an die Benutzer die Möglichkeit zu ändern.

Ressourcen für **Microsoft Power BI eingebettete** sind über die [Azure-Cloud-APIs](https://msdn.microsoft.com/library/mt712306.aspx)nach der Bereitstellung. In diesem Fall ist die Ressource, die Sie Bereitstellen einer **Power BI-Arbeitsbereich-Websitesammlung**.

![](media\power-bi-embedded-get-started\introduction.png)

## <a name="create-a-workspace-collection"></a>Erstellen einer Websitesammlung Arbeitsbereich
Eine **Auflistung der Arbeitsbereich** ist der obersten Ebene Azure Ressource und Container für die Inhalte, die in der Anwendung eingebettet werden. Eine **Auflistung der Arbeitsbereich** erstellt werden können, auf zwei Arten::

   -    Manuell mithilfe der Azure-Portal
   -    Programmgesteuert mithilfe der Azure Ressource Manager(ARM)-APIs

Werfen Sie durch die Schritte zum Erstellen eine **Arbeitsbereich-Websitesammlung** mithilfe der Azure-Portal an.

   1.   Öffnen, und melden Sie sich bei **Azure-Portal**: [http://portal.azure.com](http://portal.azure.com).

   2.   Klicken Sie auf **+ neu** auf den oberen Bereich.

       ![](media\power-bi-embedded-get-started\create-workspace-1.png)

   3.   Klicken Sie unter **Daten + Analytics** auf **Power BI eingebettete**.
   4.   Geben Sie in der **Erstellung Blade**die erforderlichen Informationen ein. **Preise**finden Sie unter [Power BI eingebettete Preise](http://go.microsoft.com/fwlink/?LinkID=760527).

       ![](media\power-bi-embedded-get-started\create-workspace-2.png)

   5. Klicken Sie auf **Erstellen**.

Der **Arbeitsbereich Websitesammlung** dauert kurze bereitstellen. Wenn abgeschlossen ist, wird der **Arbeitsbereich Websitesammlung Blade**geöffnet werden.

   ![](media\power-bi-embedded-get-started\create-workspace-3.png)

Die **Erstellung Blade** enthält die Informationen, dass Sie die APIs aufrufen, die Arbeitsbereiche erstellen und Bereitstellen von Inhalt zu müssen.

<a name="view-access-keys"/>
## <a name="view-power-bi-api-access-keys"></a>Ansicht Zugriffstasten Power BI-API

Eine der wichtigsten Teile der erforderlichen Informationen zum Aufrufen der Power BI REST-APIs werden die **Tastenkombinationen**. Diese werden zum Generieren der **app Token** , mit denen Ihre Anfragen API authentifizieren verwendet. Wenn Ihre **Tastenkombinationen**anzeigen möchten, klicken Sie auf **Zugriffstasten** auf das **Blade Einstellungen**. Weitere Informationen zu **app Token**finden Sie unter [Authenticating und mit Power BI eingebettete autorisieren](power-bi-embedded-app-token-flow.md).

   ![](media\power-bi-embedded-get-started\access-keys.png)

Sie können ' Beachten Sie, dass Sie zwei Tasten haben.

   ![](media\power-bi-embedded-get-started\access-keys-2.png)

Kopieren Sie diese Schlüssel und sichere Weise in Ihrer Anwendung zu speichern. Es ist sehr wichtig, dass Sie diese Schlüssel wie behandeln Sie ein Kennwort, da sie Zugriff auf alle Inhalte in Ihrer **Websitesammlung Arbeitsbereich**bieten werden.

Während der zwei Tasten aufgelistet sind, wird nur einen Schlüssel zu einem bestimmten Zeitpunkt benötigt. Der zweite Schlüssel erfolgt, sodass Sie regelmäßig Tasten neu generieren können ohne Zugriff auf den Dienst unterbrechen.

Jetzt, da Sie eine Instanz der Power BI für die Anwendung, und **Zugriffstasten**verfügen, können Sie einen Bericht in Ihrer eigenen app importieren. Bevor Sie erfahren Sie, wie Sie einen Bericht importieren, werden im nächste Abschnitt Erstellen von Power BI Datasets und Berichte können in einer app einzubetten.

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app"></a>Erstellen von Power BI Datasets und Berichte können in eine app einbetten

Jetzt, da Sie eine Instanz der Power BI für eine Anwendung erstellt haben und über **Tastenkombinationen**verfügen, müssen Sie zum Erstellen der Power BI Datasets und Berichte, die Sie einbetten möchten. Datasets und Berichte können mithilfe von **Power BI-Desktop**erstellt werden. Sie können [Power BI-Desktop für frei](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)herunterladen. Alternativ können Sie zum schnellen Einstieg die [Retail Analyse Stichprobe PBIX] (http://go.microsoft.com/fwlink/?LinkID=780547) herunterladen. Weitere Informationen zum Verwenden von **Power BI-Desktop**finden Sie unter [Erste Schritte mit Power BI-Desktop](https://powerbi.microsoft.com/en-us/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

Mit **Power BI-Desktop**verbinden Sie mit der Datenquelle durch eine Kopie der Daten in **Power BI-Desktop** importieren oder Herstellen einer Verbindung mit der Datenquelle mithilfe der **DirectQuery**direkt aus.

Hier sind die Unterschiede zwischen der Verwendung **Importieren** und **DirectQuery**aus.

|Importieren | DirectQuery
|---|---
|Tabellen, Spalten *und Daten* werden importiert oder in **Power BI-Desktop**kopiert. Bei der Arbeit mit Visualisierungen in **Power BI-Desktop** Abfragen eine Kopie der Daten. Um die Änderungen angezeigt, die auf die zugrunde liegenden Daten aufgetreten sind, müssen Sie aktualisieren oder erneut ein abgeschlossen, aktuelle Dataset zu importieren.|Nur die *Tabellen und Spalten* werden importiert oder in **Power BI-Desktop**kopiert. Bei der Arbeit mit Visualisierungen in **Power BI-Desktop** Abfragen die zugrunde liegenden Datenquelle, was bedeutet, dass Sie immer aktuelle Daten anzeigen.

Weitere Informationen zum Herstellen einer Verbindung mit einer Datenquelle finden Sie unter [Verbinden mit einer Datenquelle](power-bi-embedded-connect-datasource.md).

Nachdem Sie Ihre Arbeit in **Power BI-Desktop**gespeichert haben, wird eine Datei PBIX erstellt. Diese Datei enthält den Bericht. Darüber hinaus Datenimports enthält die PBIX des vollständigen Datasets, oder wenn Sie **DirectQuery**verwenden, die PBIX nur ein DataSetSchema enthält. Sie haben die PBIX programmgesteuert in den Arbeitsbereich mit der [Power BI-Import-API](https://msdn.microsoft.com/library/mt711504.aspx)bereitstellen.

> [AZURE.NOTE] **Power BI eingebettete** verfügt über weitere APIs das Ändern der Server und die Datenbank, die das Dataset zeigt und das Festlegen von Anmeldeinformationen Konto Dienst, der das Dataset verwendet werden, um zu Ihrer Datenbank herstellen. Finden Sie unter [Beitrag SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) und [Patch Gateway Datenquelle](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="next-steps"></a>Nächste Schritte
In den vorherigen Schritten haben Sie eine Auflistung der Arbeitsbereich und der erste Bericht und Dataset erstellt. Nun ist es an der Zeit, die Informationen zum Schreiben von Code für **Power BI eingebettete**. Um Ihnen den Einstieg zu erleichtern, wir eine Stichprobe Web app erstellt haben: [Erste Schritte mit der Stichprobe](power-bi-embedded-get-started-sample.md). Im Beispiel wird gezeigt, wie an:

  - Bereitstellen von Inhalten
      - Erstellen eines Arbeitsbereichs
      - Importieren einer PBIX-Datei
      - Aktualisieren der Verbindungszeichenfolgen und Festlegen von Anmeldeinformationen für Ihre Datensätze.

  - Sichere Einbetten eines Berichts

## <a name="see-also"></a>Siehe auch
- [Erste Schritte mit der Stichprobe](power-bi-embedded-get-started-sample.md)
- [Anmeldung und Autorisierung mit Power BI eingebettete](power-bi-embedded-app-token-flow.md)
- [Power BI-desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
