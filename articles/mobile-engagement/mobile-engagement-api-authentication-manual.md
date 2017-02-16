<properties 
    pageTitle="Authentifizieren Sie mit Mobile-APIs REST Engagement - Manuelles setup"
    description="Beschreibt, wie Sie Manuelles setup-Authentifizierung für Mobile Engagement REST-APIs" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Authentifizieren Sie mit Mobile-APIs REST Engagement - Manuelles setup

Dies ist eine Anlage bereitzustellende Dokumentation [Authentifizieren mit Mobile Engagement REST-APIs](mobile-engagement-api-authentication.md). Stellen Sie sicher, dass Sie sie zuerst zum Abrufen des Kontexts lesen. Eine alternative Möglichkeit, führen Sie die einmalige Einrichtung zum Einrichten der Authentifizierung für die Mobile Engagement REST-APIs verwenden des Portals Azure beschreibt. 

>[AZURE.NOTE] Die folgenden Anweisungen basieren auf diesem [Leitfaden Active Directory](../resource-group-create-service-principal-portal.md) und angepasst für was für die Authentifizierung für Mobile Engagement-APIs erforderlich ist. So finden Sie es, wenn Sie die zutreffenden Schritte im Detail zu verstehen möchten. 

1. Melden Sie sich bei Ihrem Konto Azure über das [klassische Portal](https://manage.windowsazure.com/).

2. Wählen Sie im linken Bereich aus **Active Directory** .

     ![Wählen Sie aus Active Directory][1]

3. Wählen Sie Ihre Azure-Portal **Active Directory Standard** aus. 

     ![Wählen Sie ein Verzeichnis][2]

    >[AZURE.IMPORTANT] Dieser Ansatz funktioniert nur, wenn Sie in der standardmäßigen Active Directory Ihres Kontos arbeiten und funktioniert nicht, wenn Sie dies in einem Active Directory durchführen, die Sie in Ihrem Konto erstellt haben. 

4. Wenn die Anwendungen in Ihrem Verzeichnis anzeigen möchten, klicken Sie auf on **Applications**.

     ![Anzeigen von applications][3]

5. Klicken Sie auf **Hinzufügen**. 

     ![Hinzufügen der Anwendung][4]

6. Klicken Sie auf **eine Anwendung, die zur Entwicklung von meinem Unternehmen hinzufügen**

     ![neue Anwendung][5]

6. Füllen Sie die Namen der Anwendung, und wählen Sie den Typ der Anwendung als **WEB Anwendung und/oder WEB-API** aus, und klicken Sie auf die Schaltfläche Weiter.

     ![Name-Anwendung][6]

7. Sie können eine beliebige-platzhalterprodukt-URLs für **SIGN ON URL** und der **APP-ID-URI**bereitstellen. Sie werden nicht in diesem Szenario verwendet und die URLs selbst werden nicht überprüft.  

     ![Anwendungseigenschaften][7]

8. Am Ende dieses haben Sie eine app AAD mit dem Namen aus, die Sie zuvor angegeben haben, wie im folgenden. Dies ist Ihre **AD\_APP\_Namen** , und notieren sie.  

     ![App-name][8]

9. Klicken Sie auf den Namen der Anwendung, und klicken Sie auf **Konfigurieren**.

     ![Konfigurieren der app][9]

10. Notieren Sie die CLIENT-ID, die als verwendet werden **CLIENT\_ID** für Ihre API-Anrufe. 

     ![Konfigurieren der app][10]

11. Führen Sie einen Bildlauf nach unten bis zum Abschnitt **Tasten** , und fügen Sie einen Schlüssel mit vorzugsweise 2 Jahre (Ablauf) Dauer, und klicken Sie auf **Speichern**. 

     ![Konfigurieren der app][11]


12. Kopieren Sie sofort den Wert der für den Schlüssel angezeigt wird, wie er jetzt nur angezeigt wird und nicht gespeichert, damit Sie nicht mehr angezeigt wird. Wenn Sie nicht mehr wissen müssen Sie einen neuen Product Key zu generieren. Dies ist der **CLIENT_SECRET** für Anrufe API wird. 

     ![Konfigurieren der app][12]

    >[AZURE.IMPORTANT] Schlüssel läuft am Ende der Dauer, die Sie angegeben haben, damit es erneuern Zeitpunkt andernfalls zu Ihrer Authentifizierung API erfüllen müssen nicht mehr funktionieren. Sie können auch löschen und dieser Taste neu zu erstellen, wenn Sie denken, dass geworden ist.
 
13. Klicken Sie auf **Ansicht ENDPUNKTE** Schaltfläche jetzt die wird das Dialogfeld **Endpunkte App** geöffnet. 

    ![][13]

14. Kopieren Sie im Dialogfeld App Endpunkte des **SICHERHEITSTOKENS OAUTH 2.0-ENDPUNKT**aus. 

    ![][14]

15. In der folgenden Form werden Wo finde ich die GUID in die URL Ihrer **TENANT_ID** daher Notieren sie diesen Endpunkt: 

        https://login.microsoftonline.com/<GUID>/oauth2/token

16. Nun können wir die Berechtigungen für diese app konfigurieren. Hierfür müssen Sie das [Azure-Portal](https://portal.azure.com)zu öffnen. 

17. Klicken Sie auf **Ressourcengruppen** , und suchen Sie die **Mobile Engagement** Ressourcengruppe aus.  

    ![][15]

18. Klicken Sie auf die **Mobile Engagement** Ressourcengruppe aus, und navigieren Sie zu den **Einstellungen** Blade hier. 

    ![][16]

19. Klicken Sie auf **Benutzer** in das Blade Einstellungen, und klicken Sie dann auf **Hinzufügen** , um einen Benutzer hinzuzufügen. 

    ![][17]

20. Klicken Sie auf **Wählen Sie eine Rolle**

    ![][18]

21. Klicken Sie auf **Eigentümer**

    ![][19]

22. Suchen nach den Namen der Anwendung **AD\_APP\_Namen** in das Suchfeld. Dies wird nicht hier standardmäßig angezeigt werden. Nachdem Sie es gefunden haben, wählen Sie ihn aus, und **Wählen** Sie am unteren Rand der Blade auf. 

    ![][20]

23. Klicken Sie auf das Blade **Access hinzufügen** wird es als **1 Benutzer und Gruppen 0**angezeigt. Klicken Sie auf diese Blade zum Bestätigen der Änderung auf **OK** . 

    ![][21]

Jetzt die erforderliche AAD Konfiguration abgeschlossen haben, und Sie sind alle festgelegt, dass die APIs aufrufen. 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



