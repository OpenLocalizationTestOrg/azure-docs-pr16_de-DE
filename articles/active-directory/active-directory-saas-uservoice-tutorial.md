<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in UserVoice | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von UserVoice mit Azure Active Directory einmaliges Anmelden, automatisierte bereitgestellt und mehr aktivieren!." 
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

#<a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Lernprogramm: Azure-Active Directory-Integration in UserVoice
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und UserVoice anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten UserVoice
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie UserVoice zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite UserVoice (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für UserVoice
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-uservoice-tutorial/IC777514.png "Szenario")

##<a name="enabling-the-application-integration-for-uservoice"></a>Aktivieren die Anwendungsintegration für UserVoice
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für UserVoice aktiviert.

###<a name="to-enable-the-application-integration-for-uservoice-perform-the-following-steps"></a>Wenn die Anwendungsintegration für UserVoice aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-uservoice-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-uservoice-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-uservoice-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-uservoice-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **UserVoice**.

    ![Katalog der Anwendung] (./media/active-directory-saas-uservoice-tutorial/IC777513.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **UserVoice aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![UserVoice] (./media/active-directory-saas-uservoice-tutorial/IC777810.png "UserVoice")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu UserVoice mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für UserVoice konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **UserVoice** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-uservoice-tutorial/IC777515.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der UserVoice auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-uservoice-tutorial/IC777516.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **UserVoice melden Sie sich In die URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. UserVoice.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-uservoice-tutorial/IC777517.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei UserVoice** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und klicken Sie dann speichern Zertifikatsdatei lokal als **c:\\UserVoice.cer**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-uservoice-tutorial/IC777518.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens UserVoice als Administrator.

6.  Klicken Sie oben auf der Symbolleiste klicken Sie auf Einstellungen, und wählen Sie dann im Menü Web-Portal.

    ![Einstellungen] (./media/active-directory-saas-uservoice-tutorial/IC777519.png "Einstellungen")

7.  Klicken Sie auf der Registerkarte **Web-Portal** im Abschnitt **Benutzerauthentifizierung** auf **Bearbeiten** , um die Dialogseite **Benutzerauthentifizierung bearbeiten** zu öffnen.

    ![Web-portal] (./media/active-directory-saas-uservoice-tutorial/IC777520.png "Web-portal")

8.  Klicken Sie auf der Dialogseite **Benutzerauthentifizierung bearbeiten** führen Sie die folgenden Schritte aus:

    ![Bearbeiten der Benutzerauthentifizierung] (./media/active-directory-saas-uservoice-tutorial/IC777521.png "Bearbeiten der Benutzerauthentifizierung")

    1.  Klicken Sie auf **Einmaliges Anmelden (SSO)**.
    2.  Kopieren Sie auf der Seite **Konfigurieren einmaliges Anmelden bei UserVoice** im Azure klassischen Portal des Werts **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **SSO Remote Anmeldung** .
    3.  Kopieren Sie auf der Seite **Konfigurieren einmaliges Anmelden bei UserVoice** im Azure klassischen Portal des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das **Textfeld SSO Remote Sign-Out**.
    4.  Kopieren Sie den **Fingerabdruck** Wert aus dem exportierte Zertifikat, und klicken Sie dann in der **aktuellen Zertifikat SHA1 Fingerabdruck** Textfeld einzufügen.  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

    5.  Klicken Sie auf **Authentifizierungseinstellungen speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-uservoice-tutorial/IC777522.png "Konfigurieren einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei UserVoice zu ermöglichen, müssen er in UserVoice bereitgestellt werden.  
Im Falle von UserVoice ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **UserVoice** .

2.  Wechseln Sie zu **Einstellungen**.

    ![Einstellungen] (./media/active-directory-saas-uservoice-tutorial/IC777811.png "Einstellungen")

3.  Klicken Sie auf **Allgemein**.

4.  Klicken Sie auf **Agents und Berechtigungen**.

    ![Agents und Berechtigungen] (./media/active-directory-saas-uservoice-tutorial/IC777812.png "Agents und Berechtigungen")

5.  Klicken Sie auf **Administratoren hinzufügen**.

    ![Hinzufügen von Administratoren] (./media/active-directory-saas-uservoice-tutorial/IC777813.png "Hinzufügen von Administratoren")

6.  Klicken Sie im Dialogfeld **Administratoren einladen** führen Sie die folgenden Schritte aus:

    ![Einladen von Administratoren] (./media/active-directory-saas-uservoice-tutorial/IC777814.png "Einladen von Administratoren")

    1.  Geben Sie in die TextBox-e-Mails die e-Mail-Adresse der Firma, die Sie bereitstellen möchten, und klicken Sie dann auf **Hinzufügen**.
    2.  Klicken Sie auf **einladen**.

>[AZURE.NOTE] Alle anderen UserVoice Benutzer Konto Creation Tools können oder APIs bereitgestellt durch UserVoice bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-uservoice-perform-the-following-steps"></a>Zuweisen von Benutzern zu UserVoice führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **UserVoice **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-uservoice-tutorial/IC777523.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-uservoice-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).