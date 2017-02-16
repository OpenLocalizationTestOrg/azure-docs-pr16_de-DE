<properties 
   pageTitle="Nachverfolgen von B2B Nachrichten in Ihrer apps Logik in Azure-App-Verwaltungsdienst | Microsoft Azure" 
   description="In diesem Thema behandelt das Nachverfolgen von B2B Verarbeitung" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="track-b2b-messages"></a>Nachverfolgen von Nachrichten B2B

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

## <a name="b2b-tracking-information"></a>B2B Verlaufsinformationen
B2B Kommunikation umfasst Verarbeitung zwischen Partnern mit der Nachricht. Die Beziehungen werden als Agreements zwischen zwei Partnern definiert. Nachdem die Verbindung hergestellt wurde, muss es eine Möglichkeit zum Überwachen, wenn die Kommunikation geschieht, wie erwartet. 

Wir haben Nachricht Verlauf für die folgenden Szenarien B2B implementiert: AS2, EDIFACT und X12.

## <a name="as2"></a>AS2
Nachdem Sie eine Instanz der App-API AS2 erstellt haben, wechseln Sie zu dieser Instanz, und wählen Sie die **Überwachung**. Sie können hierin anzeigen und alle AS2 Verlaufsinformationen filtern:  

![][1]  

## <a name="edifact"></a>EDIFACT
Nachdem Sie eine Instanz der App-API EDIFACT erstellt haben, wechseln Sie zu dieser Instanz, und wählen Sie die **Überwachung**. Sie können in diesem Dokument anzeigen und Filtern alle EDIFACT, Nachverfolgen von Informationen. Darüber hinaus können Sie die Interchange Ebene, Gruppenebene anzeigen und Transaktion einrichten Ebene Daten in einer Einzelansicht. 

Wenn Blattnamen als Teil des EDIFACT Vereinbarungen in der zugeordneten Trading Partner Management API-app erstellt wurden, werden mit der Abschnitt Batching alle diese Stapel aufgelistet. Sie können einen Stapel auf die aktive Nachricht (falls vorhanden) und auch die Informationen für den abgeschlossenen finden Sie unter auswählen:  

![][2]      

## <a name="x12"></a>X12
Nach dem Erstellen einer Instanz von einer X12 API-App, navigieren Sie auf diese Instanz, und wählen Sie die **Überwachung**. Sie können hierin anzeigen und alle die X12 Nachverfolgen von Informationen zu filtern. Darüber hinaus können Sie die Interchange Ebene, Gruppenebene anzeigen und Transaktion einrichten Ebene Daten in einer Einzelansicht.

Wenn Blattnamen erstellt haben, werden als Teil des X12 Vereinbarungen in der zugehörigen Trading Partner Management API-Anwendung, und klicken Sie dann auf Abschnitt Batching Listet alle diese Stapel. Sie können einen Stapel auf die aktive Nachricht (falls vorhanden) und auch die Informationen für die abgeschlossene Stapel finden Sie unter auswählen.

<!--Image references-->
[1]: ./media/app-service-logic-track-b2b-messages/AS2Tracking.png
[2]: ./media/app-service-logic-track-b2b-messages/EDIFACTTracking.png
