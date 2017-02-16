<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in AnswerHub | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von AnswerHub mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Lernprogramm: Azure-Active Directory-Integration in AnswerHub

Das Ziel dieses Lernprogramms ist die Integration von Azure und [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software)anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Eine [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software) einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie AnswerHub zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite AnswerHub (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für AnswerHub
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-answerhub-tutorial/IC785165.png "Szenario")

##<a name="enabling-the-application-integration-for-answerhub"></a>Aktivieren die Anwendungsintegration für AnswerHub

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für AnswerHub aktiviert.

###<a name="to-enable-the-application-integration-for-answerhub-perform-the-following-steps"></a>Wenn die Anwendungsintegration für AnswerHub aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-answerhub-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-answerhub-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-answerhub-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-answerhub-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **AnswerHub**.

    ![Katalog der Anwendung] (./media/active-directory-saas-answerhub-tutorial/IC785166.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **AnswerHub aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![AnswerHub] (./media/active-directory-saas-answerhub-tutorial/IC785167.png "AnswerHub")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu AnswerHub mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **AnswerHub** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-answerhub-tutorial/IC785168.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der AnswerHub auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-answerhub-tutorial/IC785169.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **AnswerHub melden Sie sich In URL** ein Ihre URL dem folgenden Muster "*https://company.answerhub.com*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-answerhub-tutorial/IC785170.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei AnswerHub** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-answerhub-tutorial/IC785171.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens AnswerHub als Administrator.
    >[AZURE.NOTE] Wenn Sie Informationen zur Konfiguration von AnswerHub benötigen, wenden Sie sich an [AnswerHubs Supportteam](mailto:success@answerhub.com. ).








6.  Wechseln Sie zur **Verwaltung**.

7.  Klicken Sie auf der Registerkarte **Benutzer und Gruppen** .

8.  Klicken Sie im Navigationsbereich auf der linken Seite, im Abschnitt **Einstellungen für soziale Netzwerke** **SAML-Setup**auf.

9.  Klicken Sie auf die Registerkarte **IDP Config** .

10. Führen Sie auf der Registerkarte **IDP Config** die folgenden Schritte aus:

    ![SAML-Setup] (./media/active-directory-saas-answerhub-tutorial/IC785172.png "SAML-Setup")

    1.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **IDP Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei AnswerHub** .
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei AnswerHub** kopieren Sie des Werts **Remote Abmeldung-URL** , und klicken Sie dann in das Textfeld **IDP Abmeldung URL** einzufügen.
    3.  Kopieren Sie auf der Seite **Konfigurieren einmaliges Anmelden bei AnswerHub** im Azure klassischen Portal des Werts **Bezeichner Namensformat** , und fügen Sie ihn in das Textfeld **IDP Bezeichner Namensformat** .
    4.  Klicken Sie auf **Schlüssel und Zertifikate**.

11. Klicken Sie auf der Registerkarte Schlüssel und Zertifikate führen Sie die folgenden Schritte aus:

    ![Schlüssel und Zertifikate] (./media/active-directory-saas-answerhub-tutorial/IC785173.png "Schlüssel und Zertifikate")

    1.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    2.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **IDP öffentlicher Schlüssel (x 509-Format)** .
    3.  Klicken Sie auf **Speichern**.

12. Klicken Sie auf der Registerkarte **IDP Config** auf **Speichern**.

13. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-answerhub-tutorial/IC785174.png "Konfigurieren einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzern zur Anmeldung bei AnswerHub zu ermöglichen, müssen er in AnswerHub bereitgestellt werden.  
Im Falle von AnswerHub ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **AnswerHub** an.

2.  Wechseln Sie zur **Verwaltung**.

3.  Klicken Sie auf der Registerkarte **Benutzer und Gruppen** .

4.  Klicken Sie im Navigationsbereich auf der linken Seite, im Abschnitt **Benutzer verwalten** auf **Benutzer erstellen oder importieren**.

    ![Benutzer und Gruppen] (./media/active-directory-saas-answerhub-tutorial/IC785175.png "Benutzer und Gruppen")

5.  Geben Sie die **e-Mail-Adresse**, **Benutzername** und **Kennwort** eines gültigen Azure Active Directory-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten, und klicken Sie dann auf **Speichern**.

>[AZURE.NOTE] Alle anderen AnswerHub Benutzer Konto Creation Tools können oder APIs bereitgestellt durch AnswerHub bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-answerhub-perform-the-following-steps"></a>Zuweisen von Benutzern zu AnswerHub führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **AnswerHub **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-answerhub-tutorial/IC785176.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-answerhub-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
