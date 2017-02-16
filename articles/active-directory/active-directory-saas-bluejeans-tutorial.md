<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in BlueJeans | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von BlueJeans mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-ad-integration-with-bluejeans"></a>Lernprogramm: Azure AD-Integration in BlueJeans

Das Ziel dieses Lernprogramms ist die Integration von Azure und BlueJeans anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Eine BlueJeans einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie BlueJeans zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite BlueJeans (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für BlueJeans
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-bluejeans-tutorial/IC785860.png "Szenario")
##<a name="enabling-the-application-integration-for-bluejeans"></a>Aktivieren die Anwendungsintegration für BlueJeans

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für BlueJeans aktiviert.

###<a name="to-enable-the-application-integration-for-bluejeans-perform-the-following-steps"></a>Wenn die Anwendungsintegration für BlueJeans aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bluejeans-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-bluejeans-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-bluejeans-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-bluejeans-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **BlueJeans**.

    ![Katalog der Anwendung] (./media/active-directory-saas-bluejeans-tutorial/IC785861.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **BlueJeans aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![BlueJeans] (./media/active-directory-saas-bluejeans-tutorial/IC785862.png "BlueJeans")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu BlueJeans mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **BlueJeans** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-bluejeans-tutorial/IC785863.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der BlueJeans auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-bluejeans-tutorial/IC785864.png "Konfigurieren Sie einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **BlueJeans melden Sie sich auf URL** Geben Sie Ihre URL dem folgenden Muster "*https://company.BlueJeans.com*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-bluejeans-tutorial/IC785865.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei BlueJeans** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-bluejeans-tutorial/IC785866.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens **BlueJeans** als Administrator.

6.  Wechseln Sie zu **ADMIN \> gruppieren Einstellungen \> Sicherheit**.

    ![Administrator] (./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Administrator")

7.  Führen Sie im Abschnitt **Sicherheit** die folgenden Schritte aus:

    ![SAML für einmaliges Anmelden] (./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML für einmaliges Anmelden")

    1.  Wählen Sie **SAML für einmaliges Anmelden**.
    2.  Wählen Sie **die automatische Bereitstellung aktivieren**.

8.  Wechseln Sie auf die folgenden Schritte aus:

    ![Zertifikatspfad] (./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Zertifikatspfad")

    1.  Klicken Sie auf **Datei auswählen**, und Laden Sie das heruntergeladene Zertifikat.
    2.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei BlueJeans** .
    3.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei BlueJeans** kopieren Sie den Wert für die **URL für das Kennwort ändern** , und fügen Sie ihn in das Textfeld **Kennwort ändern URL** .
    4.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei BlueJeans** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **URL Abmelden** .

9.  Wechseln Sie auf die folgenden Schritte aus:

    ![Speichern der Änderungen] (./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Speichern der Änderungen")

    1.  Geben Sie in das Textfeld **Benutzer-Id** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**aus.
    2.  Geben Sie in das Textfeld **E-Mail** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**aus.
    3.  Klicken Sie auf **Änderungen speichern**.

10. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-bluejeans-tutorial/IC785876.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzern zur Anmeldung bei BlueJeans zu ermöglichen, müssen er in BlueJeans bereitgestellt werden.  
Im Falle von BlueJeans ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **BlueJeans** an.

2.  Wechseln Sie zu **ADMIN \> Verwalten von Benutzern \> Benutzer hinzufügen**.

    ![Administrator] (./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Administrator")

    >[AZURE.IMPORTANT] Die Registerkarte **Benutzer hinzufügen** ist nur verfügbar, wenn die **Registerkarte Sicherheit**, **Aktivieren Sie die automatische Bereitstellung** nicht aktiviert ist.

3.  Führen Sie im Abschnitt **Benutzer hinzufügen** die folgenden Schritte aus:

    ![Fügen Sie Benutzer hinzu] (./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Fügen Sie Benutzer hinzu")

    1.  Geben Sie einen **BlueJeans Benutzernamen**, eine **e-Mail-Adresse**, eine **BlueJeans Besprechung-ID**, einen **Kenncode Moderator**, einen **Vollständigen Namen**, das **Unternehmen** eines gültigen AAD-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Benutzer hinzufügen**.

>[AZURE.NOTE] Alle anderen BlueJeans Benutzer Konto Creation Tools können oder APIs bereitgestellt durch BlueJeans bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-bluejeans-perform-the-following-steps"></a>Zuweisen von Benutzern zu BlueJeans führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **BlueJeans **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-bluejeans-tutorial/IC785887.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-bluejeans-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
