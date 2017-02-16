<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Bime | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Bime mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bime"></a>Lernprogramm: Azure-Active Directory-Integration in Bime

Das Ziel dieses Lernprogramms ist die Integration von Azure und Bime anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Bime

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Bime zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Bime (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Bime
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-bime-tutorial/IC775552.png "Szenario")
##<a name="enabling-the-application-integration-for-bime"></a>Aktivieren die Anwendungsintegration für Bime

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Bime aktiviert.

###<a name="to-enable-the-application-integration-for-bime-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Bime aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bime-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-bime-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-bime-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-bime-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Bime**.

    ![Katalog der Anwendung] (./media/active-directory-saas-bime-tutorial/IC775553.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Bime aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Bime] (./media/active-directory-saas-bime-tutorial/IC775554.png "Bime")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Bime mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für Bime konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Bime** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-bime-tutorial/IC771709.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Bime auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-bime-tutorial/IC775555.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Bime melden Sie sich In die URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. Bimeapp.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-bime-tutorial/IC775556.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Bime** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und klicken Sie dann speichern Zertifikatsdatei lokal als **c:\\Bime.cer**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-bime-tutorial/IC775557.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Bime als Administrator.

6.  Klicken Sie auf der Symbolleiste für den **Administrator**, und klicken Sie dann auf **Konto**.

    ![Administrator] (./media/active-directory-saas-bime-tutorial/IC775558.png "Administrator")

7.  Führen Sie die folgenden Schritte aus, klicken Sie auf der Seite Konto konfigurieren:

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-bime-tutorial/IC775559.png "Konfigurieren Sie einmaliges Anmelden")

    1.  Wählen Sie **Aktivieren SAML-Authentifizierung**aus.
    2.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Remote Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Bime** .
    3.  Kopieren Sie den **Fingerabdruck** Wert aus dem exportierte Zertifikat, und fügen Sie ihn in das Textfeld **Zertifikat Fingerabdruck** .  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

    4.  Klicken Sie auf **Speichern**.

8.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-bime-tutorial/IC775560.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzern zur Anmeldung bei Bime zu ermöglichen, müssen er in Bime bereitgestellt werden.  
Im Falle von Bime ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **Bime** .

2.  Klicken Sie auf der Symbolleiste für den **Administrator**, und klicken Sie dann auf **Benutzer**.

    ![Administrator] (./media/active-directory-saas-bime-tutorial/IC775561.png "Administrator")

3.  Klicken Sie in der **Liste der Benutzer**auf **Neuen Benutzer hinzufügen** ("+").

    ![Benutzer] (./media/active-directory-saas-bime-tutorial/IC775562.png "Benutzer")

4.  Führen Sie auf der Seite **Benutzerdetails** Dialogfeld die folgenden Schritte aus:

    ![Benutzerdetails] (./media/active-directory-saas-bime-tutorial/IC775563.png "Benutzerdetails")

    1.  Geben Sie den Vornamen, Nachnamen, Login, eine E-Mail mit einem gültigen AAD-Konto, die, das Sie bereitstellen möchten.
    2.  Klicken Sie auf Speichern.

>[AZURE.NOTE] Alle anderen Bime Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Bime bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-bime-perform-the-following-steps"></a>Zuweisen von Benutzern zu Bime führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Bime **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-bime-tutorial/IC775564.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-bime-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
