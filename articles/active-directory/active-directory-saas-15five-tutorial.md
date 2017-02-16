<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in 15Five | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von 15Five mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-15five"></a>Lernprogramm: Azure-Active Directory-Integration in 15Five

Das Ziel dieses Lernprogramms ist die Integration von Azure und 15Five anzeigen. Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine 15Five einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie 15Five zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite 15Five (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für 15Five
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-15five-tutorial/IC784667.png "Szenario")
##<a name="enabling-the-application-integration-for-15five"></a>Aktivieren die Anwendungsintegration für 15Five

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für 15Five aktiviert.

###<a name="to-enable-the-application-integration-for-15five-perform-the-following-steps"></a>Wenn die Anwendungsintegration für 15Five aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-15five-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-15five-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-15five-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-15five-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **15Five**.

    ![Katalog der Anwendung] (./media/active-directory-saas-15five-tutorial/IC784668.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **15Five aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![15Five] (./media/active-directory-saas-15five-tutorial/IC784669.png "15Five")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren 15Five mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **15Five** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-15five-tutorial/IC784670.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der 15Five auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-15five-tutorial/IC784671.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **15Five melden Sie sich In URL** ein Ihre URL dem folgenden Muster "*https://company.15Five.com*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-15five-tutorial/IC784672.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei 15Five** klicken Sie auf **Herunterladen von Metadaten**und leiten Sie dann an das Supportteam 15Five Metadaten-Datei.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-15five-tutorial/IC784673.png "Konfigurieren einmaliges Anmelden")

    >[AZURE.NOTE] Einmaliges Anmelden muss durch das Supportteam 15Five aktiviert sein.

5.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-15five-tutorial/IC784674.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzern zur Anmeldung bei 15Five zu ermöglichen, müssen er in 15Five bereitgestellt werden.  
Im Falle von 15Five ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **15Five** an.

2.  Wechseln Sie zu **Unternehmen verwalten**.

    ![Verwalten des Unternehmens] (./media/active-directory-saas-15five-tutorial/IC784675.png "Verwalten des Unternehmens")

3.  Wechseln Sie zu **Personen \> Hinzufügen von Personen**.

    ![Personen] (./media/active-directory-saas-15five-tutorial/IC784676.png "Personen")

4.  Führen Sie im Abschnitt neue Person hinzufügen die folgenden Schritte aus:

    ![Neue Person hinzufügen] (./media/active-directory-saas-15five-tutorial/IC784677.png "Neue Person hinzufügen")

    1.  Geben Sie den **Vornamen**, **Nachnamen**, **Titel**, **e-Mail-Adresse** eines gültigen Azure Active Directory-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Fertig**.

    >[AZURE.NOTE] Der Inhaber des Azure AD-Konto erhalten eine e-Mail-Nachricht, einschließlich einen Link, um das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen 15Five Benutzer Konto Creation Tools können oder APIs von 15Five zur Bereitstellung von Konten AAD Benutzer bereitgestellt.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-15five-perform-the-following-steps"></a>Zuweisen von Benutzern zu 15Five führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **15Five **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-15five-tutorial/IC784678.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-15five-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
