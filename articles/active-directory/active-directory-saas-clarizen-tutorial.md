<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Clarizen | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Clarizen mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Lernprogramm: Azure-Active Directory-Integration in Clarizen

Das Ziel dieses Lernprogramms ist die Integration von Azure und Clarizen anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Clarizen einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Clarizen zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Clarizen (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Clarizen
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-clarizen-tutorial/IC784679.png "Szenario")
##<a name="enabling-the-application-integration-for-clarizen"></a>Aktivieren die Anwendungsintegration für Clarizen

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Clarizen aktiviert.

###<a name="to-enable-the-application-integration-for-clarizen-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Clarizen aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clarizen-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-clarizen-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-clarizen-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-clarizen-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Clarizen**.

    ![Katalog der Anwendung] (./media/active-directory-saas-clarizen-tutorial/IC784680.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Clarizen aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Clarizen] (./media/active-directory-saas-clarizen-tutorial/IC784681.png "Clarizen")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Clarizen mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Clarizen** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-clarizen-tutorial/IC784682.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Clarizen auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-clarizen-tutorial/IC784683.png "Konfigurieren Sie einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Clarizen** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-clarizen-tutorial/IC784684.png "Konfigurieren Sie einmaliges Anmelden")

4.  Melden Sie sich in einem anderen Webbrowserfenster in Ihrer Firmenwebsite **Clarizen** als Administrator (z. B.: *https://app2.clarizen.com/Clarizen/Pages/Service/Login.aspx*).

5.  Klicken Sie auf Ihren Benutzernamen ein, und klicken Sie dann auf **Einstellungen**.

    ![Einstellungen] (./media/active-directory-saas-clarizen-tutorial/IC784685.png "Einstellungen")

6.  Klicken Sie auf der Registerkarte **Globalen Einstellungen** , und neben **Partnersuche Authentifizierung**, klicken Sie dann auf **Bearbeiten**.

    ![Globaler Einstellungen] (./media/active-directory-saas-clarizen-tutorial/IC786906.png "Globaler Einstellungen")

7.  Klicken Sie im Dialogfeld **Authentifizierung Partnersuche** führen Sie die folgenden Schritte aus:

    ![Partnersuche Authentifizierung] (./media/active-directory-saas-clarizen-tutorial/IC785892.png "Partnersuche Authentifizierung")

    1.  Klicken Sie auf **Hochladen** , um Ihr heruntergeladene Zertifikat hochladen.
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Clarizen** kopieren Sie den Wert für die **Einzelnen anmelden Dienst-URL** , und klicken Sie dann in das Textfeld **Anmeldung URL** einzufügen.
    3.  Im Azure klassischen-Portal auf der Dialogseite **Konfigurieren Single Abmeldung bei Clarizen** kopieren Sie den Wert für die **Einzelnen Sign-Out Dienst-URL** , und fügen Sie ihn in das Textfeld **Sign-out URL** .
    4.  Wählen Sie **verwenden Beitrag**aus.
    5.  Klicken Sie auf **Speichern**.

8.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-clarizen-tutorial/IC784688.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzer zur Anmeldung bei Clarizen aktivieren zu können, müssen er in Clarizen bereitgestellt werden.  
Im Falle von Clarizen ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Clarizen** an.

2.  Klicken Sie auf **Personen**.

    ![Personen] (./media/active-directory-saas-clarizen-tutorial/IC784689.png "Personen")

3.  Klicken Sie auf **Benutzer einladen**.

    ![Einladen von Benutzern] (./media/active-directory-saas-clarizen-tutorial/IC784690.png "Einladen von Benutzern")

4.  Klicken Sie auf der Seite Personen einladen Dialogfeld führen Sie die folgenden Schritte aus:

    ![Einladen von Personen] (./media/active-directory-saas-clarizen-tutorial/IC784691.png "Einladen von Personen")

    1.  Geben Sie in das Textfeld **-e-Mail** die e-Mail-Adresse eines gültigen Azure Active Directory-Kontos ein, die Sie bereitstellen möchten.
    2.  Klicken Sie auf **einladen**.

    >[AZURE.NOTE] Der Inhaber des Azure Active Directory-Konto wird erhalten eine e-Mail, und führen Sie einen Link, um ihr Konto zu bestätigen, bevor sie aktiviert wurde.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Benutzer Azure AD-erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-clarizen-perform-the-following-steps"></a>Zuweisen von Benutzern zu Clarizen führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Clarizen **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-clarizen-tutorial/IC784692.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-clarizen-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
