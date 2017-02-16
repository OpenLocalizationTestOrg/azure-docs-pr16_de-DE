<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in TeamSeer | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von TeamSeer mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Lernprogramm: Azure-Active Directory-Integration in TeamSeer
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und TeamSeer anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten TeamSeer
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie TeamSeer zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite TeamSeer (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für TeamSeer
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-teamseer-tutorial/IC789618.png "Szenario")

##<a name="enabling-the-application-integration-for-teamseer"></a>Aktivieren die Anwendungsintegration für TeamSeer
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für TeamSeer aktiviert.

###<a name="to-enable-the-application-integration-for-teamseer-perform-the-following-steps"></a>Wenn die Anwendungsintegration für TeamSeer aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-teamseer-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-teamseer-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-teamseer-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-teamseer-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **TeamSeer**.

    ![Katalog der Anwendung] (./media/active-directory-saas-teamseer-tutorial/IC789619.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **TeamSeer aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![TeamSeer] (./media/active-directory-saas-teamseer-tutorial/IC789620.png "TeamSeer")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu TeamSeer mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **TeamSeer** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-teamseer-tutorial/IC789621.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der TeamSeer auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-teamseer-tutorial/IC789628.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **TeamSeer melden Sie sich In URL** ein Ihre URL dem folgenden Muster "*http://www.teamseer.com/companyid*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-teamseer-tutorial/IC789629.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei TeamSeer** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-teamseer-tutorial/IC789630.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens TeamSeer als Administrator.

6.  Wechseln Sie zu **HR Administrator**.

    ![HR-Administrator] (./media/active-directory-saas-teamseer-tutorial/IC789634.png "HR-Administrator")

7.  Klicken Sie auf **Setup**.

    ![Setup] (./media/active-directory-saas-teamseer-tutorial/IC789635.png "Setup")

8.  Klicken Sie auf, **Richten Sie die Details der SAML-Anbieter**.

    ![SAML-Einstellungen] (./media/active-directory-saas-teamseer-tutorial/IC789636.png "SAML-Einstellungen")

9.  Führen Sie im Abschnitt Details SAML-Anbieter die folgenden Schritte aus:

    ![SAML-Einstellungen] (./media/active-directory-saas-teamseer-tutorial/IC789637.png "SAML-Einstellungen")

    1.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei TeamSeer** kopieren Sie den Wert für die **Einzelnen anmelden Dienst-URL** , und klicken Sie dann in das Textfeld **URL** einzufügen.
    2.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    3.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **IdP öffentlichen Zertifikat** .

10. Führen Sie zum Abschließen der SAML-Anbieter-Konfigurations die folgenden Schritte aus:

    ![SAML-Einstellungen] (./media/active-directory-saas-teamseer-tutorial/IC789638.png "SAML-Einstellungen")

    1.  Geben Sie in der Liste der **E-Mail-Adressen testen**der Testbenutzer e-Mail-Adresse ein.
    2.  Geben Sie in das Textfeld **Herausgeber** die Herausgeber-URL des Dienstanbieters ein.
    3.  Klicken Sie auf **Speichern**.

11. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-teamseer-tutorial/IC789639.png "Konfigurieren Sie einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei TeamSeer zu ermöglichen, müssen er in ShiftPlanning bereitgestellt werden.  
Im Falle von TeamSeer ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **TeamSeer** an.

2.  Führen Sie die folgenden Schritte aus:

    ![HR-Administrator] (./media/active-directory-saas-teamseer-tutorial/IC789640.png "HR-Administrator")

    1.  Wechseln Sie zu **HR Administrator \> Benutzer**.
    2.  Klicken Sie auf **den neuen Benutzer-Assistenten ausführen**.

3.  Führen Sie im Abschnitt **Benutzerdetails** die folgenden Schritte aus:

    ![Benutzerdetails] (./media/active-directory-saas-teamseer-tutorial/IC789641.png "Benutzerdetails")

    1.  Geben Sie den **Vornamen**, **Nachnamen**, **Benutzernamen (e-Mail-Adresse)** eines gültigen AAD-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Weiter**.

4.  Folgen Sie den auf Bildschirm Anweisungen für einen neuen Benutzer hinzufügen, und klicken Sie auf **Fertig stellen**.

>[AZURE.NOTE] Alle anderen TeamSeer Benutzer Konto Creation Tools können oder APIs von TeamSeer zur Bereitstellung von Azure AD-Benutzerkonten bereitgestellt.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-teamseer-perform-the-following-steps"></a>Zuweisen von Benutzern zu TeamSeer führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **TeamSeer **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-teamseer-tutorial/IC789642.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-teamseer-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).