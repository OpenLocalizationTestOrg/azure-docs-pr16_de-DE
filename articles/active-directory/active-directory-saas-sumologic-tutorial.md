<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in SumoLogic | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von SumoLogic mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Lernprogramm: Azure-Active Directory-Integration in SumoLogic
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und SumoLogic anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten SumoLogic
  
Am Ende dieses Lernprogramms, des Unternehmens Sie SumoLogicwill zugewiesen haben können einzelne melden Sie sich an die Anwendung zu Ihrer SumoLogic werden Benutzer Azure AD-Website (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für SumoLogic
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-sumologic-tutorial/IC778549.png "Szenario")

##<a name="enabling-the-application-integration-for-sumologic"></a>Aktivieren die Anwendungsintegration für SumoLogic
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für SumoLogic aktiviert.

###<a name="to-enable-the-application-integration-for-sumologic-perform-the-following-steps"></a>Wenn die Anwendungsintegration für SumoLogic aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sumologic-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-sumologic-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-sumologic-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-sumologic-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Sumologic**.

    ![Katalog der Anwendung] (./media/active-directory-saas-sumologic-tutorial/IC778550.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **SumoLogic aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![SumoLogic] (./media/active-directory-saas-sumologic-tutorial/IC778551.png "SumoLogic")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu SumoLogic mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikat in Ihre SumoLogictenant hochladen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **SumoLogic** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-sumologic-tutorial/IC778552.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der SumoLogic auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-sumologic-tutorial/IC778553.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **SumoLogic melden Sie sich In die URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. SumoLogic.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Aoo URL] (./media/active-directory-saas-sumologic-tutorial/IC778554.png "Konfigurieren Aoo URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei SumoLogic** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-sumologic-tutorial/IC778555.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens SumoLogic als Administrator.

6.  Wechseln Sie zu **verwalten \> Sicherheit**.

    ![Verwalten] (./media/active-directory-saas-sumologic-tutorial/IC778556.png "Verwalten")

7.  Klicken Sie auf **SAML**.

    ![Globale Sicherheitseinstellungen] (./media/active-directory-saas-sumologic-tutorial/IC778557.png "Globale Sicherheitseinstellungen")

8.  Aus der Liste **Wählen Sie eine Variante aus, oder erstellen Sie einen neuen** **Azure AD**wählen Sie aus, und klicken Sie dann auf **Konfigurieren**.

    ![Konfigurieren von SAML 2.0] (./media/active-directory-saas-sumologic-tutorial/IC778558.png "Konfigurieren von SAML 2.0")

9.  Führen Sie die folgenden Schritte aus, klicken Sie im Dialogfeld **Konfigurieren von SAML 2.0** :

    ![Konfigurieren von SAML 2.0] (./media/active-directory-saas-sumologic-tutorial/IC778559.png "Konfigurieren von SAML 2.0")

    1.  Geben Sie im Textfeld **Name der Konfiguration** **Azure AD**aus.
    2.  Wählen Sie **Debuggen Modus**aus.
    3.  Im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden bei SumoLogic** kopieren Sie den Wert des **Herausgebers URL** , und fügen Sie ihn in das Textfeld **Herausgeber** .
    4.  Im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden bei SumoLogic** kopieren Sie den Wert für die **Authentifizierung anfordern URL** , und klicken Sie dann in das Textfeld **Authn anfordern URL** einzufügen.
    5.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    6.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie das gesamte Zertifikat in das Textfeld **X 509-Zertifikat** .
    7.  Wählen Sie als **E-Mail-Attribut** **verwenden SAML-Betreff**ein.
    8.  Wählen Sie **SP initiiert Login-Konfiguration**aus.
    9.  Geben Sie in das Textfeld **Benutzername Pfad** **Azure**ein.
    10. Klicken Sie auf **Speichern**.

10. Im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden bei SumoLogic** wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **abgeschlossen**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-sumologic-tutorial/IC778560.png "Konfigurieren einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei SumoLogic zu ermöglichen, müssen er SumoLogic bereitgestellt werden.  
Im Falle von SumoLogic ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **SumoLogic** .

2.  Wechseln Sie zu **verwalten \> Benutzer**.

    ![Benutzer] (./media/active-directory-saas-sumologic-tutorial/IC778561.png "Benutzer")

3.  Klicken Sie auf **Hinzufügen**.

    ![Benutzer] (./media/active-directory-saas-sumologic-tutorial/IC778562.png "Benutzer")

4.  Führen Sie die folgenden Schritte aus, klicken Sie im Dialogfeld **Neuen Benutzer** :

    ![Neuer Benutzer] (./media/active-directory-saas-sumologic-tutorial/IC778563.png "Neuer Benutzer")

    1.  Geben Sie die verwandte Informationen des Kontos Azure AD-, die Sie in die Textfelder **Vorname**, **Nachname** und **e-Mail-** bereitstellen möchten.
    2.  Wählen Sie eine Rolle aus.
    3.  Wählen Sie als **Status** **aktiv**.
    4.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Alle anderen SumoLogic Benutzer Konto Creation Tools können oder APIs bereitgestellt durch SumoLogic bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-sumologic-perform-the-following-steps"></a>Zuweisen von Benutzern zu SumoLogic führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **SumoLogic** Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-sumologic-tutorial/IC778564.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-sumologic-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).