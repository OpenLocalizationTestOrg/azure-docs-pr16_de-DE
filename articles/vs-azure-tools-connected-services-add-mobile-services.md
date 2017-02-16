<properties 
   pageTitle="Hinzufügen von Mobile Dienste mithilfe von Diensten verbunden in Visual Studio | Microsoft Azure"
   description="Fügen Sie Mobile Dienste hinzu, indem Sie über das Dialogfeld Visual Studio verbunden Dienste hinzufügen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="mlhoop"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="mobile"
   ms.date="12/16/2015"
   ms.author="mlearned" />

# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Hinzufügen von Mobile-Diensten mit Visual Studio verbunden Services

Mit Visual Studio 2015 können Sie mit Azure Mobile Dienste mithilfe des Dialogfelds **Verbunden Dienst hinzufügen** verbinden. Sie können eine beliebige app, C#-Client, alle JavaScript-app oder Plattformen Cordova app verbinden. Nachdem Sie eine Verbindung herstellen, können Sie erstellen und Zugriff auf Daten, Erstellen von benutzerdefinierten APIs und geplante Aufträge oder Unterstützung für Pushbenachrichtigungen hinzuzufügen.  Der Vorgang verbundenen Dienste fügt alle entsprechenden Verweise und Verbindungscode hinzu. Sie können auch nutzen der integrierten Unterstützung für die Authentifizierung mit einer Vielzahl von beliebte Identität Phishingversuchen, wie z. B. Azure AD-Facebook, Twitter und Microsoft Accounts.

## <a name="supported-project-types"></a>Unterstützte Projekttypen

>[AZURE.NOTE] In Visual Studio 2015 ist das Hinzufügen von Azure Mobile-Dienste auf einem Windows Universal (Windows 10) Projekte mit dem Dialogfeld verbunden Dienste hinzufügen nicht unterstützt. Sie können Azure Mobile Dienste hinzufügen, indem Sie die entsprechenden Pakete mithilfe des NuGet-Paket-Managers für ein Projekt zu installieren.

Im Dialogfeld verbunden Services können Sie in den folgenden Projekttypen mit Azure Mobile Services herstellen.

- .NET Windows 8.1 Store, Telefon und universeller App Projekte

- JavaScript Windows 8.1 Store, Telefon und universeller App Projekte

- Projekte mit Visual Studio-Tools für Apache Cordova erstellt


## <a name="connect-to-azure-mobile-services-using-the-add-connected-services-dialog"></a>Verbinden Sie mit Azure Mobile Dienste mithilfe des Dialogfelds verbunden Dienste hinzufügen

1. Stellen Sie sicher, dass Sie ein Azure-Konto besitzen. Wenn Sie ein Azure-Konto besitzen, können Sie für eine [kostenlose Testversion](http://go.microsoft.com/fwlink/?LinkId=518146)registrieren.

1. Öffnen Sie das Dialogfeld **Verbunden Dienste hinzufügen** .
 - Öffnen Sie für .NET apps das Projekt in Visual Studio, öffnen Sie das Kontextmenü für den Knoten **Verweise** in der Lösung Explorer, und wählen Sie dann **Verbunden Dienst hinzufügen**
 
        ![Connecting to Azure Mobile Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)

 - Öffnen Sie für Projekte, Apache Cordova app das Projekt in Visual Studio, öffnen Sie das Kontextmenü für den Projektknoten in Lösung Explorer, und wählen Sie dann **Verbunden Dienst hinzufügen**.

1. Klicken Sie im Dialogfeld **Verbunden Dienst hinzufügen** wählen Sie **Azure Mobile Dienste**, und wählen Sie dann auf die Schaltfläche **Konfigurieren** . Sie möglicherweise aufgefordert, die in Azure melden, wenn Sie dies nicht bereits getan haben.

    ![Hinzufügen einen Azure Mobile Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)

1. Wählen Sie im Dialogfeld **Azure Mobile Dienste** einen vorhandenen mobilen Dienst Wenn dort noch besteht. Wenn Sie einen neuen Azure mobilen Dienst erstellen müssen, gehen Sie dazu. Fahren Sie andernfalls mit dem nächsten Schritt fort.

    So erstellen Sie ein neues mobilen Dienstkonto
    1. Wählen Sie den Link **Erstellen Dienst **am unteren Rand des Dialogfelds aus.
        ![Neuen mobilen verbundenen Dienst hinzufügen](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)




    2. Klicken Sie im Dialogfeld **Erstellen Mobile Service** können Sie aus der Dropdownliste **Laufzeit** einen JavaScript Back-End-mobile-Dienst oder einen mobilen .NET Back-End-Dienst auswählen. 
  
        ![Erstellen eines mobilen Diensts](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)

        Ein JavaScript-Back-End-Dienst ist einfach und leistungsfähige. Wenn Sie einen mobilen JavaScript Back-End-Dienst erstellen, der serverseitigen JavaScript-Code in der Cloud gespeichert ist, aber Sie können mithilfe der Server-Explorer oder der Azure-Verwaltungsportal Serverskripts bearbeiten. 

        Ein .NET Back-End-mobiler-Dienst bietet Ihnen die Leistungsumfang und die Flexibilität von Web-API und Entität Framework. Wenn Sie einen mobilen .NET Back-End-Dienst erstellen, wird ein Projekt für Sie erstellt und Ihre Lösung hinzugefügt. 

    1. Wählen Sie den **Bereich** des mobilen Diensts werden soll, und geben Sie einen Benutzernamen und ein Kennwort für den Server.
 
    1. Nachdem Sie alle erforderlichen Informationen eingegeben haben, wählen Sie die Schaltfläche **Erstellen** in der mobilen Dienst erstellen.
    2. Der neue mobile Service sollte in der Dienstliste auf das Dialogfeld **Azure Mobile Dienste** angezeigt werden. Wählen Sie den neuen mobilen Dienst in der Liste aus, und wählen Sie dann auf die Schaltfläche **Hinzufügen** auf den Dienst zum Projekt hinzufügen.
    

1. Überprüfen Sie die erste Schritte Seite, die angezeigt wird, und erfahren Sie, wie Ihr Projekt geändert wurde. Sobald Sie einen verbundenen Dienst hinzufügen, wird in Ihrem Browser eine Seite Erste Schritte angezeigt. Sie überprüfen die vorgeschlagene nächste Schritte und Codebeispielen, oder wechseln Sie zu der Seite geblieben, um anzuzeigen, welche Verweise zum Projekt hinzugefügt wurden, und wie Ihre Dateien Code und Konfiguration geändert wurden.

1. Starten Sie mithilfe der Codebeispielen als Leitfaden Schreiben von Code zum Zugreifen auf Ihre mobilen Service!

## <a name="how-your-project-is-modified"></a>Wie Ihr Projekt geändert wird

Wie Visual Studio Ihres Projekts ändert, hängt von den Projekttyp ab. C#-Client-apps finden Sie unter [Was ist – C#-Projekte](http://go.microsoft.com/fwlink/p/?LinkId=513119). JavaScript-Client-apps finden Sie unter [Was ist – JavaScript-Projekte](http://go.microsoft.com/fwlink/p/?LinkId=513120). Cordova-apps finden Sie unter [Was ist – Cordova Projekte](http://go.microsoft.com/fwlink/p/?LinkId=513116).


##<a name="next-steps"></a>Nächste Schritte

Stellen von Fragen und Hilfe erhalten: 

 - [Forum im MSDN: Azure Mobile-Diensten](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)

 - [Azure Mobile Dienste auf den Microsoft Azure-Teamblog](https://azure.microsoft.com/blog/topics/mobile/)

 - [Azure Mobile Dienste auf azure.microsoft.com](https://azure.microsoft.com/services/mobile-services/)

 - [Azure Mobile Services-Dokumentation unter azure.microsoft.com](https://azure.microsoft.com/documentation/services/mobile-services/)



