<properties 
    pageTitle="Exemplarische Vorgehensweise: Überwachen von Microsoft Dynamics CRM mit Anwendung Einsichten" 
    description="Rufen Sie werden von Microsoft Dynamics CRM Online-Anwendung Einsichten verwenden. Exemplarische Vorgehensweise des Installationsprogramms, Abrufen von Daten, Visualisierung und exportieren." 
    services="application-insights" 
    documentationCenter=""
    authors="mazharmicrosoft" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="awills"/>
 
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Exemplarische Vorgehensweise: Aktivieren von werden für Microsoft Dynamics CRM Online-Anwendung Einsichten verwenden

In diesem Artikel wird gezeigt, wie abzurufenden Daten werden von [Microsoft Dynamics CRM Online](https://www.dynamics.com/) [Visual Studio Anwendung Einsichten](https://azure.microsoft.com/services/application-insights/)verwenden. Wir werden auf den gesamten Prozess des Hinzufügens Anwendung Einsichten Skript an Ihrer Anwendung, durchzuführen Erfassen von Daten und Visualisierung von Daten.

>[AZURE.NOTE] [Durchsuchen Sie die Lösung für die Stichprobe](https://dynamicsandappinsights.codeplex.com/).

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Hinzufügen von Anwendung Einsichten zu neuen oder vorhandenen CRM Online-Instanz 

Um eine Anwendung zu überwachen, fügen Sie eine Anwendung Einsichten SDK an Ihrer Anwendung. Das SDK sendet werden- [Anwendung Einsichten Portal](https://portal.azure.com)können unsere leistungsstarke Datenanalyse und Diagnosetools verwenden, oder die Daten in Speicher exportieren.

### <a name="create-an-application-insights-resource-in-azure"></a>Erstellen Sie eine Ressource Anwendung Einsichten in Azure

1. Erhalten Sie [ein Konto in Microsoft Azure](http://azure.com/pricing). 
2. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com) an, und fügen Sie eine neue Anwendung Einsichten Ressource. Dies ist die Stelle, an der die Daten verarbeitet und angezeigt werden.

    ![Klicken Sie auf +, Anwendung Einsichten Entwicklertools Dienste.](./media/app-insights-sample-mscrm/01.png)

    Wählen Sie als den Anwendungstyp ASP.NET aus.

3. Öffnen Sie die Registerkarte Schnellstart, und öffnen Sie das Skript Code.

    ![](./media/app-insights-sample-mscrm/03.png)

**Lassen Sie die Codepage geöffnet** , während Sie führen Sie die nächste Schritt in einem anderen Browserfenster. Sie benötigen den Code verfügbar. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Erstellen Sie eine JavaScript-Web-Ressource in Microsoft Dynamics CRM

1. Öffnen Sie Ihre CRM Online-Instanz und melden Sie sich mit Administratorrechten an.
2. Öffnen von Microsoft Dynamics CRM Einstellungen Anpassungen, Anpassen des Systems

    ![](./media/app-insights-sample-mscrm/04.png)
    
    ![](./media/app-insights-sample-mscrm/05.png)


    ![](./media/app-insights-sample-mscrm/06.png)

3. Erstellen Sie eine JavaScript-Ressource ein.

    ![](./media/app-insights-sample-mscrm/07.png)

    Geben sie einen Namen, wählen Sie **Skript (JScript)** und Text-Editor geöffnet.

    ![](./media/app-insights-sample-mscrm/08.png)
    
4. Kopieren Sie den Code aus Anwendung Einsichten aus. Sicherzustellen Sie beim Kopieren der Skript-Tags zu ignorieren. Im folgenden Bildschirmfoto finden:

    ![](./media/app-insights-sample-mscrm/09.png)

    Der Code enthält die Instrumentation-Taste, die Ihrer Anwendung Einsichten Ressource bezeichnet.

5. Speichern und veröffentlichen.

    ![](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Urkunde Formulare

1. Öffnen Sie in Microsoft CRM Online Formulars "Firma"

    ![](./media/app-insights-sample-mscrm/11.png)

2. Öffnen Sie das Formular Eigenschaften

    ![](./media/app-insights-sample-mscrm/12.png)

3. Fügen Sie JavaScript-Web-Ressource, die Sie erstellt haben.

    ![](./media/app-insights-sample-mscrm/13.png)

    ![](./media/app-insights-sample-mscrm/14.png)

4. Speichern Sie und veröffentlichen Sie Ihre Anpassungen Formular.


## <a name="metrics-captured"></a>Kennzahlen erfasst

Sie haben nun werden erfassen für das Formular eingerichtet. Immer, wenn er verwendet wird, werden die Daten an Ihrer Anwendung Einsichten Ressource gesendet.

Hier sind Beispiele für die Daten, die Sie sehen.

#### <a name="application-health"></a>Integrität der Anwendung

![](./media/app-insights-sample-mscrm/15.png)

![](./media/app-insights-sample-mscrm/16.png)

Browser-Ausnahmen:

![](./media/app-insights-sample-mscrm/17.png)

Klicken Sie auf das Diagramm, um weitere Details zu sehen:

![](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Verwendung

![](./media/app-insights-sample-mscrm/19.png)

![](./media/app-insights-sample-mscrm/20.png)

![](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Browser

![](./media/app-insights-sample-mscrm/22.png)

![](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Geografischen Standort

![](./media/app-insights-sample-mscrm/24.png)

![](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Innere Seite Ansicht Anforderung

![](./media/app-insights-sample-mscrm/26.png)

![](./media/app-insights-sample-mscrm/27.png)

![](./media/app-insights-sample-mscrm/28.png)

![](./media/app-insights-sample-mscrm/29.png)

![](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Beispiel-code

[Beispiel-Code durchsuchen](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI

Sie können sogar tieferen Analyse ausführen, wenn Sie [die Daten nach Microsoft Power BI exportieren](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Beispiel für Microsoft Dynamics CRM-Lösung

[Hier die Stichprobe-Lösung in Microsoft Dynamics CRM implementiert] (https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Weitere Informationen

* [Was ist eine Anwendung Einsichten?](app-insights-overview.md)
* [Anwendung Einsichten für Webseiten](app-insights-javascript.md)
* [Weitere Beispiele und exemplarische Vorgehensweisen](app-insights-code-samples.md)

 
