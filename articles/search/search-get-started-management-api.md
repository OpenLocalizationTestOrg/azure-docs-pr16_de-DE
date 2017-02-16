<properties 
    pageTitle="Erste Schritte mit Azure Suche Management REST-API | Microsoft Azure | Cloud gehosteten Suchdienst" 
    description="Verwalten Sie Ihrer gehostete Cloud Azure-Suchdienst mithilfe einer Management REST-API" 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

# <a name="get-started-with-azure-search-management-rest-api"></a>Erste Schritte mit Azure Suche Management REST-API
> [AZURE.SELECTOR]
- [Portal](search-manage.md)
- [PowerShell](search-manage-powershell.md)
- [REST-API](search-get-started-management-api.md)

Die Verwaltung Azure Suche REST API ist eine Alternative zum Ausführen von Verwaltungsaufgaben im Portal. Servicemanagement gehören erstellen oder löschen den Dienst, dieselbe Skalierung des Diensts und zur Verwaltung von Schlüsseln. In diesem Lernprogramm verfügt über eine Beispiel-Clientanwendung, die die Servicemanagement API veranschaulicht. Darüber hinaus Konfigurationsschritte erforderlich, um das Beispiel in Ihrer Umgebung lokale Entwicklung auszuführen.

Um dieses Lernprogramms abgeschlossen haben, müssen Sie:

- Visual Studio 2012 oder 2013
- Beispiel-Client-Anwendung zum Herunterladen

Im Verlauf Abschließen des Lernprogramms, zwei Services bereitgestellt werden: Azure suchen und Azure Active Directory (AD). Erstellen Sie darüber hinaus eine AD-Anwendung, die in Azure Vertrauensstellung zwischen der Clientanwendung und den Endpunkt des Ressource-Manager stellt her.

Sie benötigen ein Azure-Konto zum Bearbeiten dieses Lernprogramms.


##<a name="download-the-sample-application"></a>Herunterladen der Stichprobe-Anwendungs

In diesem Lernprogramm basiert auf einer Windows-Console-Anwendung in c# geschrieben wurde, die Sie bearbeiten können, und führen Sie in Visual Studio 2012 oder 2013

Sie können die Clientanwendung auf Github bei [Azure Suche .NET Management API Demo](https://github.com/Azure-Samples/search-dotnet-management-api/)suchen.


##<a name="configure-the-application"></a>Konfigurieren Sie die Anwendung

Bevor Sie die Beispiel-Anwendung ausführen können, müssen Sie Authentifizierung aktivieren, damit aus der Clientanwendung an den Endpunkt der Ressource-Manager gesendete Serviceanfragen akzeptiert werden können. Der Anforderung zum Authentifizierung für stammen mit [Azure Ressourcenmanager](https://msdn.microsoft.com/library/azure/dn790568.aspx), also die Grundlage für alle Vorgänge im Zusammenhang mit dem Portal angefordert über eine API, einschließlich der Empfänger im Zusammenhang mit Servicemanagement suchen. Die Servicemanagement API für Suchmaschinen Azure ist einfach eine Erweiterung des Azure-Managers und seine Abhängigkeiten erbt.  

Azure Ressourcenmanager erfordert Azure-Active Directory-Dienst als deren Identitätsanbieter. 

Wenn Sie um eine Access-Token zu erhalten, mit der Anfragen Ressourcenmanager erreichen können, enthält die Clientanwendung einen Codeabschnitt, der Active Directory-Anrufe an. Das Codesegment sowie die vorbereitende Schritte zur Verwendung des Codesegments aus diesem Artikel übernommen wurden: [Azure Ressourcenmanager authentifizieren Serviceanfragen](http://msdn.microsoft.com/library/azure/dn790557.aspx).

Sie können die Anweisungen in der oben aufgeführte Link, oder führen Sie die Schritte in diesem Dokument an, wenn Sie es vorziehen, um anhand des Lernprogramms Schritt für Schritt zu wechseln.

In diesem Abschnitt werden Sie die folgenden Aufgaben ausführen:

1. Erstellen eines AD-Diensts
1. Erstellen einer AD-Anwendung
1. Konfigurieren Sie die AD-Anwendung, indem Sie registrieren die Details der Beispiel-Clientanwendung, die Sie heruntergeladen haben
1. Laden Sie die Beispiel-Clientanwendung mit Werten, die es verwendet wird, um die Autorisierung für Anfragen zu erhalten

> [AZURE.NOTE] Diese Links bieten Hintergrund mit Azure Active Directory für die Authentifizierung von Client-Anfragen im Ressourcenmanager: [Azure Ressourcenmanager](http://msdn.microsoft.com/library/azure/dn790568.aspx), [Azure Ressourcenmanager authentifizieren Anfragen](http://msdn.microsoft.com/library/azure/dn790557.aspx)und [Azure Active Directory](http://msdn.microsoft.com/library/azure/jj673460.aspx).

###<a name="create-an-active-directory-service"></a>Erstellen eines Active Directory-Diensts

1. Melden Sie sich bei der [Azure-Portal](https://manage.windowsazure.com).

2. Führen Sie einen Bildlauf nach unten im linken Navigationsbereich, und klicken Sie auf **Active Directory**.

4. Klicken Sie auf **neu** , um die **App-Dienste**öffnen | **Active Directory**. In diesem Schritt erstellen Sie einen neuen Active Directory-Dienst. Dieser Dienst hostet die AD-Anwendung, dass Sie ein paar Schritte ab jetzt definiert werden. Erstellen eine neue Dienstleistung hilft isolieren das Lernprogramm aus anderen Programmen, die Sie möglicherweise bereits in Azure gehostet werden.

5. Klicken Sie auf **Verzeichnis** | **benutzerdefinierte erstellen**.

6. Geben Sie einen Namen, Domäne und geografischen Position ein. Die Domäne muss eindeutig sein. Klicken Sie auf das Häkchen, um den Dienst zu erstellen.

     ![][5]

###<a name="create-a-new-ad-application-for-this-service"></a>Erstellen einer neuen AD-Anwendung für diesen Dienst

1. Wählen Sie den "SearchTutorial" Active Directory-Dienst, den Sie soeben erstellt haben.

2. Klicken Sie im oberen Menü auf **Applications**. 
 
3. Klicken Sie auf **eine Anwendung hinzufügen**. Eine AD-Anwendung speichert Informationen zu den Clientanwendungen, die sie als Identitätsanbieter verwendet wird.  
 
4. Wählen Sie **eine Anwendung, die zur Entwicklung von meinem Unternehmen hinzufügen**. Diese Option bietet Registrierung Einstellungen für Applikationen, die nicht in der Galerie veröffentlicht werden. Da die Clientanwendung nicht Teil der Galerie ist, ist dies die richtige Wahl für dieses Lernprogramm aus.

     ![][6]
 
5. Geben Sie einen Namen ein, beispielsweise "Azure-Suche-Manager" aus.

6. Wählen Sie für Anwendungstyp **Native Client-Anwendung** aus. Dies ist die richtige für die Stichprobe Anwendung; Dies geschieht in einer Windows-Client (Konsole) Anwendung keiner Webanwendung sein.

     ![][7]
 
7. Geben Sie unter URI umleiten "http://localhost/Azure-Search-Manager-App" ein. Folgt ein URI, welche Azure Active Directory des Benutzer-Agents als Antwort auf die Anforderung einer OAuth 2.0 Autorisierung umgeleitet werden. Der Wert muss kein physische Endpunkt, sondern muss ein gültiger URI sein. 

    Für die Zwecke dieses Lernprogramms der Wert kann nichts sein, aber welchen Geben Sie eine bereitzustellende Informationen für die administrative Verbindung in diesem Beispiel werden. 
 
7. Klicken Sie auf das Häkchen, um die Active Directory-Anwendung zu erstellen. Es sollte "Azure-Suche-Manager-App" im linken Navigationsbereich angezeigt.

###<a name="configure-the-ad-application"></a>Konfigurieren Sie die AD-Anwendung
 
9. Klicken Sie auf der Anwendung, die AD "Azure-Suche-Manager-App", die Sie soeben erstellt haben. Sollte angezeigt werden, dass es im linken Navigationsbereich aufgeführt.

10. Klicken Sie im oberen Menü auf **Konfigurieren** .
 
11. Führen Sie einen Bildlauf nach unten zu den Berechtigungen, und wählen Sie **Azure Management-API**. In diesem Schritt Sie die-API (in diesem Fall die Azure Ressourcenmanager-API) angeben, dass die Clientanwendung benötigt Zugriff auf, zusammen mit den gewährten werden muss.

12. In delegierte Berechtigungen, klicken Sie auf die Dropdown-Liste, und wählen Sie **Access Azure Service Management (Preview**).
 
     ![][8]
 
13. Die Änderungen zu speichern. 

Lassen Sie die Anwendung Konfigurationsseite geöffnet. Im nächsten Schritt werden Sie Werte von dieser Seite kopieren und geben sie in der Stichprobe Anwendung.

###<a name="load-the-sample-application-program-with-registration-and-subscription-values"></a>Laden Sie das Beispiel Anwendungsprogramm mit Registrierung und Abonnement Werten

In diesem Abschnitt bearbeiten Sie die Lösung in Visual Studio gültige Werte, die vom im Portals ersetzen.
Die Werte, die Sie hinzufügen möchten, die am oberen Rand Program.cs angezeigt werden:

        private const string TenantId = "<your tenant id>";
        private const string ClientId = "<your client id>";
        private const string SubscriptionId = "<your subscription id>";
        private static readonly Uri RedirectUrl = new Uri("<your redirect url>");

Wenn Sie noch nicht [die Stichprobe Anwendung von Github heruntergeladen](https://github.com/Azure-Samples/search-dotnet-management-api/)haben, benötigen Sie es für diesen Schritt.

1. Öffnen Sie die **ManagementAPI.sln** in Visual Studio aus.

2. Öffnen Sie Program.cs.

3. Geben Sie `ClientId`. Die Anwendung AD Konfiguration Seite nach links aus dem vorherigen Schritt öffnen, kopieren Sie die Client-ID aus der Seite AD Anwendung Konfiguration im Portal und fügen Sie ihn in Program.cs.

4. Geben Sie `RedirectUrl`. Kopieren Sie umleiten URI aus der gleichen Portalseite, und fügen Sie ihn in Program.cs.

    ![][9]

5. Bereitstellen`TenantID.` 
    - Wechseln Sie zurück zu Active Directory | SearchTutorial (Dienst). 
    - Klicken Sie in der oberen Leiste auf **Anwendungen** . 
    - Klicken Sie auf **Ansicht Endpunkte** am unteren Rand der Seite. 
    - Kopieren Sie den OAUTH 2.0 Autorisierung Endpunkt am Ende der Liste an. 
    - Fügen Sie den Endpunkt in TenantID, durch das Verkürzen des Werts aller URI-Parameter außer den Mandanten-ID an.

    Angegebenen "https://login.windows.net/55e324c7-1656-4afe-8dc3-43efcd4ffa50/oauth2/authorize?api-version=1.0", löschen Sie alles mit Ausnahme von "55e324c7-1656-4afe-8dc3-43efcd4ffa50".

    ![][10]

6. Geben Sie `SubscriptionID`.
    - Wechseln Sie zu der Hauptseite des Portals.
    - Klicken Sie auf **Einstellungen** am unteren Rand des Navigationsbereichs links.
    - Klicken Sie auf der Registerkarte Abonnements kopieren Sie die Abonnement-ID, und fügen Sie ihn in Program.cs.

7. Speichern Sie und erstellen Sie die Lösung.


##<a name="explore-the-application"></a>Untersuchen der Anwendungs

Fügen Sie eine fortzuschreiten bei der ersten Methode Anruf, so, dass Sie über das Programm wechseln können. Drücken Sie **F5** , um die Anwendung ausführen, und drücken Sie **F11** , um den Code schrittweise.

Die Anwendung Beispiel erstellt einen kostenlosen Azure Suchdienst für ein vorhandenes Azure-Abonnement. Wenn Sie kostenlose Dienstleistung für Ihr Abonnement bereits vorhanden ist, tritt die Stichprobe Anwendung. Nur eine kostenlose Suchdienst pro Abonnement zulässig ist.

1. Öffnen Sie Program.cs aus der Lösung Explorer, und wechseln Sie zu der Funktion Main (String [] void). 
 
3. Beachten Sie, dass **ExecuteArmRequest** verwendet wird, um Anfragen an den Endpunkt Azure Ressourcenmanager ausführen `https://management.azure.com/subscriptions` für ein angegebenes `subscriptionID`. Diese Methode ist im Rahmen des Programms zum Ausführen von Vorgängen mit dem Azure Ressourcenmanager API oder Suche Management-API verwendet.

3. Anfragen zu Azure Ressourcenmanager müssen authentifiziert und autorisiert werden. Dies geschieht mithilfe der **GetAuthorizationHeader** -Methode aufgerufen, von der **ExecuteArmRequest** -Methode aus [Azure Ressourcenmanager authentifizieren Anfragen](http://msdn.microsoft.com/library/azure/dn790557.aspx)übernommen. Beachten Sie die **GetAuthorizationHeader** ruft `https://management.core.windows.net` einer Access-Token abrufen.

4. Aufgefordert werden, melden Sie sich mit einen Benutzernamen und Ihr Kennwort ein, das für Ihr Abonnement gültig ist.

5. Als Nächstes ist ein neuer Azure Suchdienst mit den Ressourcenmanager Azure-Anbieter registriert. Dies ist wiederum die **ExecuteArmRequest** Methode, mit dem den Suchdienst auf Azure für Ihr Abonnement über erstellt diesmal `providers/Microsoft.Search/register`. 

6. Der Rest des Programms wird die [Azure Suche Management REST-API](http://msdn.microsoft.com/library/dn832684.aspx)verwendet. Beachten Sie, dass die `api-version` für diese API von der Ressourcenmanager Azure-api-Version ist. Beispielsweise `/listAdminKeys?api-version=2014-07-31-Preview` zeigt die `api-version` der Azure Suche Management REST-API.

    Die nächste Reihe von Vorgängen Abrufen die Dienstdefinition nur erstellt, die Admin-api-Schlüssel, erneut generiert und ruft Tasten, ändert sich die Partition und Replikat und schließlich löscht den Dienst an.

    Wenn die Anzahl der Dienst Replikat oder Partition zu ändern, wird davon ausgegangen, dass diese Aktion fehl, wenn Sie die kostenlose Edition arbeiten. Nur die standard Edition umso zusätzliche Partitionen und der wiederzuverwenden.

    Löschen des Diensts ist den letzten Vorgang.

##<a name="next-steps"></a>Nächste Schritte

Nachdem Sie nach Abschluss dieses Lernprogramms, sollten Sie weitere Informationen zu Servicemanagement oder Authentifizierung mit Active Directory-Dienst:

- Weitere Informationen zu integrieren von einer Clientanwendung in Active Directory. Finden Sie unter [Integration von Applications in Azure-Active Directory](http://msdn.microsoft.com/library/azure/dn151122.aspx).
- Informationen Sie zu anderen Servicemanagement in Azure. Finden Sie unter [Verwalten von Ihrer Dienste](http://msdn.microsoft.com/library/azure/dn578292.aspx).

<!--Anchors-->
[Download the sample application]: #Download
[Configure the application]: #config
[Explore the application]: #explore
[Next Steps]: #next-steps

<!--Image references-->
[5]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-Service.PNG
[6]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-App.PNG
[7]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-App-prop.PNG
[8]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-ConfigPermissions.PNG
[9]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-ConfigPage.PNG
[10]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-OAuthEndpoint.PNG

<!--Link references-->
[Manage your search solution in Microsoft Azure]: search-manage.md
[Azure Search development workflow]: search-workflow.md
[Create your first azure search solution]: search-create-first-solution.md
[Create a geospatial search app using Azure Search]: search-create-geospatial.md


 
