<properties
    pageTitle="Erstellen Sie eine App Logik | Microsoft Azure"
    description="Informationen Sie zum Erstellen einer Verbindung von Diensten SaaS Logik-App"
    authors="jeffhollan"
    manager="dwrede"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="create-a-new-logic-app-connecting-saas-services"></a>Erstellen Sie eine neue Verbindung von Diensten SaaS Logik-app

In diesem Thema wird veranschaulicht, wie, ein paar Minuten können Sie mit [Azure Logik Apps](app-service-logic-what-are-logic-apps.md)loslegen können. Wir werden eines einfachen Workflows durchzuführen, in dem Sie interessante Tweets zu Ihren e-Mails senden können.

Wenn Sie dieses Szenario verwenden zu können, müssen Sie folgende Aktionen ausführen:

- Ein Azure-Abonnement
- Twitter-Konto
- Ein Outlook.com- oder gehostete Office 365-Postfach

## <a name="create-a-new-logic-app-to-email-you-tweets"></a>Erstellen Sie eine neue Logik app, um Sie Tweets per e-Mail senden

1. Klicken Sie im [Azure Portals Dashboard](https://portal.azure.com)die Option **neu**aus. 
2. In der Suchleiste 'Logik app' Suchen Sie, und wählen Sie dann auf **App Logik**. Sie können auch wählen Sie **neu**, **Web + Mobile**, und wählen **Logik-App**. 
3. Geben Sie einen Namen für Ihre app Logik, wählen Sie einen Speicherort, Ressourcengruppe aus, und wählen Sie **Erstellen**.  Wenn Sie die **Pin zum Dashboard** auswählen öffnet die app Logik automatisch einmal bereitgestellt.  
4. Nach dem Öffnen der app Logik zum ersten Mal können Sie aus einer Vorlage auswählen zu starten.  Klicken Sie jetzt auf **Leere Logik App** , um diese ganz neu erstellen. 
1. Das erste Element, die zu erstellenden ist die auslösen.  Dies ist das Ereignis, das die Logik app gestartet werden kann.  Suchen nach **twitter** in das Suchfeld auslösen, und wählen Sie ihn aus.
7. Jetzt wird in einen Suchbegriff ausgelöst auf Eingabe.  Die **Häufigkeit** und **Intervall** legt fest, wie häufig Ihre app Logik überprüft für neue Tweets (und alle Tweets während die Spanne Uhrzeit zurück).
    ![Twitter-Suche](./media/app-service-logic-create-a-logic-app/twittersearch.png)

5. Wählen Sie die Schaltfläche **neue Schritt** , und wählen Sie dann **eine Aktion hinzufügen** oder **Hinzufügen einer Bedingung**
6. Wenn Sie **eine Aktion hinzufügen**auswählen, können Sie den [verfügbaren Connectors](../connectors/apis-list.md) zum Auswählen einer Aktion suchen. Sie können beispielsweise **Outlook.com – senden von e-Mail-** Nachrichten über eine outlook.com-Adresse senden auswählen:  
    ![Aktionen](./media/app-service-logic-create-a-logic-app/actions.png)

7. Jetzt müssen Sie die Parameter für die e-Mail ausgefüllt werden sollen:  ![Parameter](./media/app-service-logic-create-a-logic-app/parameters.png)

8. Schließlich können Sie auswählen, **Speichern** , um Ihre app Logik stellen live.

## <a name="manage-your-logic-app-after-creation"></a>Verwalten Sie Ihre app Logik nach der Erstellung

Nachdem Sie nun Ihre app Logik ausgeführt wird. Es regelmäßige für Tweets mit dem Suchbegriff eingegeben. Wenn einen übereinstimmenden Tweet gefunden wird, wird es Sie eine e-Mail-Nachricht senden. Schließlich werden Sie finden Sie unter So deaktivieren Sie die app oder finden Sie unter wie funktioniert.

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com)

1. Klicken Sie auf **Durchsuchen** , klicken Sie auf der linken Seite des Bildschirms, und wählen Sie **Logik Apps**.

2. Klicken Sie auf die neue Logik app, die Sie gerade erstellt haben, um den aktuellen Status und weitere allgemeine Informationen finden Sie unter.

3. Klicken Sie auf **Bearbeiten**, um Ihre neue Logik app zu bearbeiten.

5. Um die app zu deaktivieren, klicken Sie in der Befehlsleiste auf **Deaktivieren** .

1. Anzeigen von ausführen und der Trigger Verläufe zu überwachen, wenn Ihre app Logik ausgeführt wird.  Klicken Sie auf **Aktualisieren** , um die neuesten Daten anzuzeigen.

In weniger als 5 Minuten konnten Sie zum Einrichten einer einfachen Logik app in der Cloud ausgeführt. Weitere Informationen zur Verwendung von Logik Apps-Features finden Sie unter [verwenden Logik app-Features]. Weitere Informationen zu den Logik App Definitionen selbst finden Sie unter [Logik App Definitionen verfassen](app-service-logic-author-definitions.md).

<!-- Shared links -->
[Azure portal]: https://portal.azure.com
[Verwenden von Logik app-Funktionen]: app-service-logic-create-a-logic-app.md
