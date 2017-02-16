<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Zendesk | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Zendesk mit Azure Active Directory einmaliges Anmelden, automatisierte bereitgestellt und mehr aktivieren!." 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Lernprogramm: Azure-Active Directory-Integration in Zendesk
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Zendesk anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Zendesk
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Zendesk zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Zendesk (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Zendesk
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-zendesk-tutorial/IC773083.png "Szenario")

##<a name="enabling-the-application-integration-for-zendesk"></a>Aktivieren die Anwendungsintegration für Zendesk
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Zendesk aktiviert.

###<a name="to-enable-the-application-integration-for-zendesk-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Zendesk aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Verwaltungsportal Azure, klicken Sie auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zendesk-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-zendesk-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-zendesk-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-zendesk-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Zendesk**.

    ![Katalog der Anwendung] (./media/active-directory-saas-zendesk-tutorial/IC773084.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Zendesk aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Zendesk] (./media/active-directory-saas-zendesk-tutorial/IC773085.png "Zendesk")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Zendesk mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für Zendesk konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren Single Sign On** , im Azure AD-Portal auf der Seite **Zendesk** Anwendung Integration.

    ![Einmaliges Anmelden] (./media/active-directory-saas-zendesk-tutorial/IC773086.png "Einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Zendesk auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-zendesk-tutorial/IC773087.png "Konfigurieren einmaliges Anmelden")

3.  Führen Sie auf der Seite **Konfigurieren der App-URL** die folgenden Schritte aus:

    ![Konfigurieren der app-URL] (./media/active-directory-saas-zendesk-tutorial/IC773088.png "Konfigurieren der app-URL")
  
    ein. Geben Sie in das Textfeld **Zendesk melden Sie sich In die URL** den URL dem folgenden Muster:`https://<tenant-name>.zendesk.com`

    b. Klicken Sie auf **Weiter**.



4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Zendesk** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Compiter.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-zendesk-tutorial/IC777534.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Zendesk als Administrator.

6.  Klicken Sie auf **Administrator**.

7.  Klicken Sie im linken Navigationsbereich klicken Sie auf **Einstellungen**, und klicken Sie dann auf **Sicherheit**.

    ![Sicherheit] (./media/active-directory-saas-zendesk-tutorial/IC773089.png "Sicherheit")

8.  Klicken Sie auf der Registerkarte **Sicherheit** auf die Registerkarte **Administrator und Agents** .

9.  Wählen Sie **einmaliges Anmelden (SSO) und SAML**, und wählen Sie dann auf **SAML**.

10. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Zendesk** im Azure AD-Portal kopieren Sie des Werts **SAML SSO-URL** , und fügen Sie ihn in das Textfeld **SAML SSO-URL** .

11. Im Azure AD-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Zendesk** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **Remote Abmeldung-URL** .

    ![Einmaliges Anmelden] (./media/active-directory-saas-zendesk-tutorial/IC773090.png "Einmaliges Anmelden")

12. Kopieren Sie den **Fingerabdruck** Wert aus dem exportierte Zertifikat, und fügen Sie ihn in das Textfeld **Zertifikat Fingerabdruck** .

    >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

13. Klicken Sie auf **Speichern**.

14. Klicken Sie im Portal Azure AD-wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-zendesk-tutorial/IC773093.png "Konfigurieren einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei **Zendesk**zu ermöglichen, müssen er in **Zendesk**bereitgestellt werden.  
Im Falle von **Zendesk**ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-account-to-zendesk-perform-the-following-steps"></a>Um ein Benutzerkonto zu Zendesk bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **Zendesk** .

2.  Wählen Sie die Registerkarte **Kundenliste** aus.

3.  Wählen Sie die Registerkarte **Benutzer** aus, und klicken Sie auf **Hinzufügen**.

    ![Benutzer hinzufügen] (./media/active-directory-saas-zendesk-tutorial/IC773632.png "Benutzer hinzufügen")

4.  Geben Sie die e-Mail-Adresse eines vorhandenen Azure AD-Kontos Sie bereitstellen möchten, und klicken Sie dann auf **Speichern**.

    ![Neuer Benutzer] (./media/active-directory-saas-zendesk-tutorial/IC773633.png "Neuer Benutzer")

>[AZURE.NOTE] Alle anderen Zendesk Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Zendesk bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-zendesk-perform-the-following-steps"></a>Zuweisen von Benutzern zu Zendesk führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure AD-.

2.  Klicken Sie auf der Seite **Zendesk **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-zendesk-tutorial/IC773094.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-zendesk-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
