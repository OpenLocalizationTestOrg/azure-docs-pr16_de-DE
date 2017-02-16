<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Zscaler eine | Microsoft Azure" 
    description="Erfahren Sie, wie Sie mithilfe einer Zscaler mit Azure Active Directory einmaliges Anmelden, automatisierte bereitgestellt und mehr aktiviert!." 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zscaler-one"></a>Lernprogramm: Azure-Active Directory-Integration in Zscaler 1

Ziel dieses Lernprogramms ist die Integration von Azure und eine ZScaler angezeigt.  
 Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:  

-   Ein gültiges Azure-Abonnement
-   Eine ZScaler ein einmaliges Anmelden aktiviert Abonnement  

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie für einen ZScaler zugewiesen haben können einzelne melden Sie sich bei der Anwendung an Ihre ZScaler einer Firmenwebsite (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).  

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:  

1.  Aktivieren die Integration der Anwendung für eine ZScaler
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren von Proxyeinstellungen
4.  Konfigurieren der Benutzer bereitgestellt
5.  Zuweisen von Benutzern  

![Szenario] (./media/active-directory-saas-zscaler-one-tutorial/IC800214.png "Szenario")  

##<a name="enabling-the-application-integration-for-zscaler-one"></a>Aktivieren die Integration der Anwendung für eine ZScaler

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für eine ZScaler aktiviert.  

###<a name="to-enable-the-application-integration-for-zscaler-one-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für eine ZScaler die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.  

    ![Active Directory] (./media/active-directory-saas-zscaler-one-tutorial/IC700993.png "Active Directory")  

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.  

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .  

    ![Applikationen] (./media/active-directory-saas-zscaler-one-tutorial/IC700994.png "Applikationen")  

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.  

    ![Anwendung hinzufügen] (./media/active-directory-saas-zscaler-one-tutorial/IC749321.png "Anwendung hinzufügen")  

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.  

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-zscaler-one-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")  

6.  Geben Sie im **Suchfeld** **ZScaler ein**.  

    ![Katalog der Anwendung] (./media/active-directory-saas-zscaler-one-tutorial/IC800215.png "Katalog der Anwendung")  

7.  Wählen Sie im Ergebnisbereich **Eine ZScaler**aus, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.  

    ![ZScaler 1] (./media/active-directory-saas-zscaler-one-tutorial/IC800216.png "ZScaler 1")  

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer mit einem ZScaler authentifiziert mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikat zu Ihrem ZScaler einen Mandanten hochladen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)  

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren Single Sign On** , im Azure klassischen-Portal auf der Seite von **ZScaler eine** Anwendung Integration.  

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-zscaler-one-tutorial/IC800217.png "Konfigurieren Sie einmaliges Anmelden")  

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer eine ZScaler bei auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.  

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-zscaler-one-tutorial/IC800218.png "Konfigurieren Sie einmaliges Anmelden")  

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **ZScaler ein melden Sie sich auf URL** ein die URL, die von den Benutzern, melden Sie sich für den Zugriff auf Ihre ZScaler eine Anwendung verwendet, und klicken Sie dann auf **Weiter**.  

    ![Konfigurieren der App-URL] (./media/active-directory-saas-zscaler-one-tutorial/IC800219.png "Konfigurieren der App-URL")  

    >[AZURE.NOTE]Sie können den tatsächlichen Wert für Ihre Umgebung von Ihrem Supportteam ZScaler eine abrufen, bei Bedarf.  

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei einem ZScaler** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.  

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-zscaler-one-tutorial/IC800220.png "Konfigurieren Sie einmaliges Anmelden")  

5.  In einem anderen Webbrowserfenster melden Sie sich bei Ihrem ZScaler einer Firmenwebsite als Administrator.  

6.  Klicken Sie im Menü oben auf **Verwaltung**.  

    ![Verwaltung] (./media/active-directory-saas-zscaler-one-tutorial/IC800206.png "Verwaltung")  

7.  Klicken Sie unter **Verwalten von Administratoren und Rollen**klicken Sie auf **Benutzer verwalten und Authentifizierung**.  

    ![Verwalten von Benutzern und Authentifizierung] (./media/active-directory-saas-zscaler-one-tutorial/IC800207.png "Verwalten von Benutzern und Authentifizierung")  

8.  Führen Sie im Abschnitt **Wählen Sie Authentifizierung die Optionen für Ihre Organisation** die folgenden Schritte aus:  

    ![Authentifizierung] (./media/active-directory-saas-zscaler-one-tutorial/IC800208.png "Authentifizierung")  

    1.  Wählen Sie **authentifizieren mithilfe von SAML einmaliges Anmelden**.  
    2.  Klicken Sie auf **Konfigurieren von SAML einzelnen anmelden Parameter**.  

9.  Führen Sie die folgenden Schritte aus, und klicken Sie dann auf **Fertig**, auf der Seite Dialogfeld **Konfigurieren von SAML einzelnen anmelden Parameter** :  

    ![Einmaliges Anmelden] (./media/active-directory-saas-zscaler-one-tutorial/IC800209.png "Einmaliges Anmelden")  

    1.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei einem ZScaler** kopieren Sie den Wert für die **Authentifizierung anfordern URL** , und fügen Sie ihn in das Textfeld **URL des Portals SAML an die Benutzer für die Authentifizierung gesendet werden** .  
    2.  Geben Sie in das Textfeld **Attribut, Anmeldename enthält** **NameID**aus.  
    3.  Wenn Ihr heruntergeladene Zertifikat hochladen möchten, klicken Sie auf **Zscaler Pem**.  
    4.  Wählen Sie **Automatische SAML - Bereitstellung aktivieren**.  

10. Klicken Sie auf der Dialogseite **Benutzerauthentifizierung konfigurieren** führen Sie die folgenden Schritte aus:  

    ![Verwaltung] (./media/active-directory-saas-zscaler-one-tutorial/IC800210.png "Verwaltung")  

    1.  Klicken Sie auf **Speichern**.  
    2.  Klicken Sie auf **jetzt aktivieren**.  

11. Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei einer ZScaler** wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie dann auf **abgeschlossen**.  

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-zscaler-one-tutorial/IC800221.png "Konfigurieren Sie einmaliges Anmelden")  

##<a name="configuring-proxy-settings"></a>Konfigurieren von Proxyeinstellungen

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>So konfigurieren Sie die Proxyeinstellungen in Internet Explorer

1.  Starten Sie **InternetExplorer**.  

2.  Wählen Sie **Internetoptionen** im Menü **Extras** , um das Dialogfeld **Internetoptionen** zu öffnen.  

    ![Internetoptionen] (./media/active-directory-saas-zscaler-one-tutorial/IC769492.png "Internetoptionen")  

3.  Klicken Sie auf die Registerkarte **Verbindungen** .  

    ![Verbindungen] (./media/active-directory-saas-zscaler-one-tutorial/IC769493.png "Verbindungen")  

4.  Klicken Sie auf **LAN-Einstellungen** , um das Dialogfeld **LAN-Einstellungen** zu öffnen.  

5.  Führen Sie im Abschnitt Proxy-Server die folgenden Schritte aus:  

    ![Proxyserver] (./media/active-directory-saas-zscaler-one-tutorial/IC769494.png "Proxyserver")  

    1.  Wählen Sie einen Proxyserver für LAN verwenden.  
    2.  Geben Sie in das Textfeld Adresse **gateway.zscalerone.net**ein.  
    3.  Geben Sie im Textfeld Port den Wert **80**ein.  
    4.  Wählen Sie **Proxyserver für lokale Adressen umgehen**aus.  
    5.  Klicken Sie auf **OK** , um das Dialogfeld **Einstellungen für lokales Netzwerk (LAN)** zu schließen.  

6.  Klicken Sie auf **OK** , um das Dialogfeld **Internetoptionen** zu schließen.  

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzer bei einer ZScaler anmelden aktivieren, müssen sie für einen ZScaler bereitgestellt werden.  
 Im Falle einer einzigen ZScaler ist die Bereitstellung eine manuelle Aufgabe.  

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem **Zscaler einen** Mandanten.  

2.  Klicken Sie auf **Verwaltung**.  

    ![Verwaltung] (./media/active-directory-saas-zscaler-one-tutorial/IC781035.png "Verwaltung")  

3.  Klicken Sie auf **Benutzermanagement**.  

    ![Hinzufügen] (./media/active-directory-saas-zscaler-one-tutorial/IC781037.png "Hinzufügen")  

4.  Klicken Sie auf der Registerkarte **Benutzer** auf **Hinzufügen**.  

    ![Hinzufügen] (./media/active-directory-saas-zscaler-one-tutorial/IC781037.png "Hinzufügen")  

5.  Führen Sie im Abschnitt Benutzer hinzufügen die folgenden Schritte aus:  

    ![Fügen Sie Benutzer hinzu] (./media/active-directory-saas-zscaler-one-tutorial/IC781038.png "Fügen Sie Benutzer hinzu")  

    1.  Geben Sie die **Benutzer-ID**, **Anzeigenamen des Benutzers ein**, **das Kennwort**, **Kennwort bestätigen**ein, und wählen Sie dann **Gruppen** und der **Abteilung** eines gültigen AAD-Kontos ein, die Sie bereitstellen möchten.  
    2.  Klicken Sie auf **Speichern**.  

>[AZURE.NOTE]Alle anderen ZScaler ein Benutzer Konto Creation Tools können oder APIs, die eine ZScaler bereitstellen AAD Benutzerkonten bereitstellt.  

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.  

###<a name="to-assign-users-to-zscaler-one-perform-the-following-steps"></a>Zuweisen von Benutzern zu einer ZScaler führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.  

2.  Klicken Sie auf der Seite von **ZScaler eine** Anwendung Integration auf **Benutzer zuweisen**.  

    ![Zuweisen von Benutzern] (./media/active-directory-saas-zscaler-one-tutorial/IC800222.png "Zuweisen von Benutzern")  

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.  

    ![Ja] (./media/active-directory-saas-zscaler-one-tutorial/IC767830.png "Ja")  

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).  
