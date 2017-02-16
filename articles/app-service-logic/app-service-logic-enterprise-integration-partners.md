<properties 
    pageTitle="Erfahren Sie mehr über Partner und Enterprise Integration Pack | Microsoft Azure-App-Verwaltungsdienst | Microsoft Azure" 
    description="Informationen Sie zum Verwenden von Partner mit der apps Enterprise Integration Pack und Logik" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="learn-about-partners-and-enterprise-integration-pack"></a>Erfahren Sie mehr über Partner und Pack für Enterprise-Integration

## <a name="overview"></a>(Übersicht)
Vor dem ein Partners erstellen können, müssen Sie und die Organisation, der Sie eine Geschäftsbeziehung unterhalten möchten Informationen gemeinsam nutzen, mit deren Hilfe Sie sowohl identifizieren und überprüfen Sie die Nachrichten, die vom miteinander gesendet werden. Nachdem Sie diese Diskussion haben und Sie mit Ihrer geschäftlichen Beziehung zu beginnen können, können Sie zum Erstellen eines *Partners* in Ihr Konto Integration.

## <a name="what-is-a-partner"></a>Was ist ein Partner?
Partner sind die Personen, die in Business-To-Business (B2B) messaging und Transaktionen teilnehmen. 

## <a name="how-are-partners-used"></a>Wie werden Partner verwendet?
Partner werden verwendet, um die Vereinbarungen zu erstellen. Ein Vertrag definiert die Details der Nachrichten, die zwischen Partner ausgetauscht werden sollen. 

Bevor Sie einen Vertrag erstellen können, müssen Sie mindestens zwei Partner bei Ihrem Konto Integration hinzugefügt haben. Einer der Partner eine Vereinbarung muss Ihrer Organisation sein. Der Partner, der Ihre Organisation darstellt wird als **Host Partner**bezeichnet. Der zweite Partner würde die anderen Organisation darstellen, mit der Ihre Organisation Nachrichten austauscht. Der zweite Partner wird als **Gast Partner**bezeichnet. Des Partners Gast kann ein anderes Unternehmen, oder sogar eine Abteilung innerhalb der eigenen Organisation handeln.  

Nachdem Sie die Partner hinzugefügt haben, verwenden Sie diese Partner zur Verfügung, um einen Vertrag erstellen. 

Empfangen Sie, und senden Sie die Einstellungen ausgerichteten aus der Sicht des Partners gehostet werden. Die Einstellungen für empfangen eine Vereinbarung beispielsweise bestimmen, wie der gehostete Partner von einem Partner Gast gesendete Nachrichten erhält. Die Einstellungen Senden der Vereinbarung ebenso anzugeben, wie der gehostete Partner Nachrichten an den Partner Gast sendet.

## <a name="how-to-create-a-partner"></a>Zum Erstellen eines Partners?
Vom Azure-Portal:  
1. Wählen Sie **Durchsuchen**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Geben Sie in das Suchfeld der Filters **Integration** , und wählen Sie **Integration Konten** aus der Liste der Suchergebnisse     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Wählen Sie das **Konto Integration** , dem Sie die Partner hinzufügen möchten  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4.  Wählen Sie die Kachel für **Partner**  
![](./media/app-service-logic-enterprise-integration-partners/partner-1.png)  
5. Wählen Sie die Schaltfläche **Hinzufügen** in den Partner Blade, das geöffnet wird  
![](./media/app-service-logic-enterprise-integration-partners/partner-2.png)  
6. Geben Sie einen **Namen** für Ihr Partner, und klicken Sie dann der **Kennung **, wählen Sie schließlich Geben Sie einen **Wert**. Der Wert wird verwendet, um Dokumente zu identifizieren, die in Ihrer apps gehören.  
![](./media/app-service-logic-enterprise-integration-partners/partner-3.png)  
7. Wählen Sie *das Glockensymbol Benachrichtigung, um den Fortschritt der Partner Erstellungsprozess anzuzeigen* .  
![](./media/app-service-logic-enterprise-integration-partners/partner-4.png)  
8. Wählen Sie die Kachel für **Partner zur Verfügung** . Dabei wird die Kachel aktualisiert und sollten Sie die Anzahl der Partner erhöhen angezeigt wird, wurde den neuen Partner über die entsprechenden erfolgreich hinzugefügt.    
![](./media/app-service-logic-enterprise-integration-partners/partner-5.png)  
10. Nachdem Sie die Kachel Partner ausgewählt haben, wird auch den neu hinzugefügten Partner angezeigt, in dem Partner Blade angezeigt.    
![](./media/app-service-logic-enterprise-integration-partners/partner-6.png)  

## <a name="how-to-edit-a-partner"></a>Zum Bearbeiten eines Partners

Wie folgt vor, um eines Partners zu bearbeiten, das bereits in Ihrem Konto Integration:  
1. Wählen Sie die Kachel für **Partner**  
2. Wählen Sie den Partner zu bearbeiten, wenn das Blade Partner angezeigt wird  
3. Klicken Sie auf die Kachel **Update Partner** nehmen Sie die Änderungen, die Sie benötigen  
4. Wenn Sie Ihre Änderungen sind, wählen Sie den Link **Speichern** , sonst aus, wählen Sie den Link **verwerfen** , um die Änderungen verwerfen.  
![](./media/app-service-logic-enterprise-integration-partners/edit-1.png)  

## <a name="how-to-delete-a-partner"></a>Zum Löschen eines Partners
1. Wählen Sie die Kachel für **Partner**  
2. Wählen Sie den Partner zu bearbeiten, wenn das Blade Partner angezeigt wird  
3. Wählen Sie den Link **Löschen**    
![](./media/app-service-logic-enterprise-integration-partners/delete-1.png)   

## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr über agreements] (./app-service-logic-enterprise-integration-agreements.md "Erfahren Sie mehr über Enterprise-Integration Verträgen")  


