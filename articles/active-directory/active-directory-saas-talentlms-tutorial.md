<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in TalentLMS | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von TalentLMS mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-talentlms"></a>Lernprogramm: Azure-Active Directory-Integration in TalentLMS
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und TalentLMS anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten TalentLMS
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie TalentLMS zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite TalentLMS (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für TalentLMS
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-talentlms-tutorial/IC777289.png "Szenario")

##<a name="enabling-the-application-integration-for-talentlms"></a>Aktivieren die Anwendungsintegration für TalentLMS
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für TalentLMS aktiviert.

###<a name="to-enable-the-application-integration-for-talentlms-perform-the-following-steps"></a>Wenn die Anwendungsintegration für TalentLMS aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-talentlms-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-talentlms-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-talentlms-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-talentlms-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **TalentLMS**.

    ![Katalog der Anwendung] (./media/active-directory-saas-talentlms-tutorial/IC777290.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **TalentLMS aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![TalentLMS] (./media/active-directory-saas-talentlms-tutorial/IC777291.png "TalentLMS")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu TalentLMS mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert. .  
Einmaliges Anmelden für TalentLMS konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **TalentLMS** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-talentlms-tutorial/IC777292.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der TalentLMS auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-talentlms-tutorial/IC777293.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **TalentLMS melden Sie sich In die URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. TalentLMSapp.com*", und klicken Sie dann auf **Weiter**.

    ![Melden Sie sich auf URL] (./media/active-directory-saas-talentlms-tutorial/IC777294.png "Melden Sie sich auf URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei TalentLMS** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und klicken Sie dann speichern Zertifikatsdatei lokal als **c:\\TalentLMS.cer**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-talentlms-tutorial/IC777295.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens TalentLMS als Administrator.

6.  Klicken Sie im Abschnitt **Konto und Einstellungen** auf die Registerkarte **Benutzer** .

    ![Konto und Einstellungen] (./media/active-directory-saas-talentlms-tutorial/IC777296.png "Konto und Einstellungen")

7.  Klicken Sie auf **Einmaliges Anmelden (SSO)**,

8.  Führen Sie im Abschnitt einmaliges Anmelden die folgenden Schritte aus:

    ![Einmaliges Anmelden] (./media/active-directory-saas-talentlms-tutorial/IC777297.png "Einmaliges Anmelden")

    1.  Wählen Sie aus der Liste **SSO-Integration** **SAML 2.0**ein.
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei TalentLMS** kopieren Sie des Werts **Identität Anbieter-ID** , und fügen Sie ihn in das Textfeld **Identitätsanbieter (IdP)** .
    3.  Kopieren Sie den **Fingerabdruck** Wert aus dem exportierte Zertifikat, und fügen Sie ihn in das Textfeld **Zertifikat Fingerabdruck** .

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

    4.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei TalentLMS** kopieren Sie des Werts **Remote Anmelde-URL** , und klicken Sie dann in das Textfeld **Remote URL** einzufügen.
    5.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei TalentLMS** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **Abmeldung Remote-URL** .
    6.  Geben Sie in das Textfeld **TargetedID** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**
    7.  Geben Sie im Textfeld **Vorname** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    8.  Geben Sie im Textfeld **Nachname** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    9.  Geben Sie in das Textfeld **E-Mail** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**
    10. Klicken Sie auf **Speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-talentlms-tutorial/IC777298.png "Konfigurieren Sie einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei TalentLMS zu ermöglichen, müssen er in TalentLMS bereitgestellt werden.  
Im Falle von TalentLMS ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **TalentLMS** .

2.  Klicken Sie auf **Benutzer**, und klicken Sie dann auf **Benutzer hinzufügen**.

3.  Führen Sie auf der Seite Dialogfeld **Benutzer hinzufügen** die folgenden Schritte aus:

    ![Fügen Sie Benutzer hinzu] (./media/active-directory-saas-talentlms-tutorial/IC777299.png "Fügen Sie Benutzer hinzu")

    1.  Geben Sie die Attributwerte der zugehörigen des Benutzerkontos Azure AD-in die folgenden Textfelder: **Vorname**, **Nachname**, **e-Mail-Adresse**.
    2.  Klicken Sie auf **Benutzer hinzufügen**.

>[AZURE.NOTE] Alle anderen TalentLMS Benutzer Konto Creation Tools können oder APIs bereitgestellt durch TalentLMS bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-talentlms-perform-the-following-steps"></a>Zuweisen von Benutzern zu TalentLMS führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **TalentLMS **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-talentlms-tutorial/IC777300.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-talentlms-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).