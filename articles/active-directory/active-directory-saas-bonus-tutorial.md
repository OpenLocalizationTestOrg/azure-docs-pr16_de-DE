<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Bonus.ly | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Bonus.ly mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Lernprogramm: Azure-Active Directory-Integration in Bonus.ly

Das Ziel dieses Lernprogramms ist die Integration von Azure und Bonus.ly anzeigen. Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Test in Bonus.ly

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Bonus.ly
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-bonus-tutorial/IC773679.png "Szenario")
##<a name="enabling-the-application-integration-for-bonusly"></a>Aktivieren die Anwendungsintegration für Bonus.ly

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Bonus.ly aktiviert.

###<a name="to-enable-the-application-integration-for-bonusly-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Bonus.ly aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-bonus-tutorial/IC773680.png "Einmaliges Anmelden aktivieren")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-bonus-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-bonus-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-bonus-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Bonus.ly**.

    ![Katalog der Anwendung] (./media/active-directory-saas-bonus-tutorial/IC773681.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Bonus.ly aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773682.png "Bonusly")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Bonus.ly mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für Bonus.ly konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Bonus.ly** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-bonus-tutorial/IC749323.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Bonus.ly auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-bonus-tutorial/IC773683.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Bonus.ly Mandanten URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. Bonus.ly*", und klicken Sie dann auf **Weiter**: 

    ![Konfigurieren der app-URL] (./media/active-directory-saas-bonus-tutorial/IC773684.png "Konfigurieren der app-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Bonus.ly** , klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal als **c:\\Bonusly.cer**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-bonus-tutorial/IC773685.png "Konfigurieren einmaliges Anmelden")

5.  Melden Sie sich in einem anderen Browserfenster Ihrem Mandanten **Bonus.ly** an.

6.  Klicken Sie oben auf der Symbolleiste klicken Sie auf **Einstellungen**, und wählen Sie **Integration und apps**.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773686.png "Bonusly")

7.  Wählen Sie unter **Single Sign On** **SAML**aus.

8.  Klicken Sie auf der Seite **SAML** Dialogfeld führen Sie die folgenden Schritte aus:

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773687.png "Bonusly")

    1.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **IdP SSO Ziel-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Bonus.ly** .
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Bonus.ly** kopieren Sie den Wert für die **Herausgeber-ID** , und fügen Sie ihn in das Textfeld **IdP Herausgeber** .
    3.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **IdP Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Bonus.ly** .
    4.  Kopieren Sie den **Fingerabdruck** Wert aus dem exportierte Zertifikat, und fügen Sie ihn in das Textfeld **Zertifikat Fingerabdruck** .

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

9.  Klicken Sie auf **Speichern**.

10. Klicken Sie im Portal klassischen Microsoft Azure wählen Sie die Konfiguration Bestätigung, und klicken Sie dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-bonus-tutorial/IC773689.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzern zur Anmeldung bei Bonus.ly zu ermöglichen, müssen er in Bonus.ly bereitgestellt werden.  
Im Falle von Bonus.ly ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  In einem Webbrowserfenster melden Sie sich in Ihrem Mandanten Bonus.ly.

2.  Klicken Sie auf **Einstellungen**

    ![Einstellungen] (./media/active-directory-saas-bonus-tutorial/IC781041.png "Einstellungen")

3.  Klicken Sie auf der Registerkarte **Benutzer und Boni** .

    ![Benutzer und Boni] (./media/active-directory-saas-bonus-tutorial/IC781042.png "Benutzer und Boni")

4.  Klicken Sie auf **Verwalten von Benutzern**.

    ![Verwalten von Benutzern] (./media/active-directory-saas-bonus-tutorial/IC781043.png "Verwalten von Benutzern")

5.  Klicken Sie auf **Benutzer hinzufügen**.

    ![Fügen Sie Benutzer hinzu] (./media/active-directory-saas-bonus-tutorial/IC781044.png "Fügen Sie Benutzer hinzu")

6.  Klicken Sie im Dialogfeld **Benutzer hinzufügen** führen Sie die folgenden Schritte aus:

    ![Fügen Sie Benutzer hinzu] (./media/active-directory-saas-bonus-tutorial/IC781045.png "Fügen Sie Benutzer hinzu")

    1.  Geben Sie den "**-e-Mail**, **Vorname**, **Nachname**" eines gültigen AAD-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

    >[AZURE.NOTE] Der Inhaber des AAD Konto erhalten eine e-Mail-Nachricht, die enthält einen Link, um das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen Bonus.ly Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Bonus.ly bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-bonusly-perform-the-following-steps"></a>Zuweisen von Benutzern zu Bonus.ly führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite Bonus.ly Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-bonus-tutorial/IC773690.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-bonus-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
