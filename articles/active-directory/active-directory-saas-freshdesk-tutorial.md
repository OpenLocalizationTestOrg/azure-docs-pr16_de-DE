<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Freshdesk | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Freshdesk mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Lernprogramm: Azure-Active Directory-Integration in Freshdesk
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Freshdesk anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Freshdesk
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Freshdesk zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Freshdesk (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Freshdesk
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-freshdesk-tutorial/IC776761.png "Szenario")
##<a name="enabling-the-application-integration-for-freshdesk"></a>Aktivieren die Anwendungsintegration für Freshdesk
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Freshdesk aktiviert.

###<a name="to-enable-the-application-integration-for-freshdesk-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Freshdesk aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-freshdesk-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-freshdesk-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-freshdesk-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-freshdesk-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Freshdesk**.

    ![Katalog der Anwendung] (./media/active-directory-saas-freshdesk-tutorial/IC776762.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Freshdesk aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Freshdesk] (./media/active-directory-saas-freshdesk-tutorial/IC776763.png "Freshdesk")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Freshdesk mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für Freshdesk konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Freshdesk** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-freshdesk-tutorial/IC776764.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Freshdesk auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-freshdesk-tutorial/IC776765.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Freshdesk melden Sie sich In die URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. Freshdesk.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-freshdesk-tutorial/IC776766.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Freshdesk** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und klicken Sie dann speichern Zertifikatsdatei lokal als **c:\\Freshdesk.cer**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-freshdesk-tutorial/IC776767.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Freshdesk als Administrator.

6.  Klicken Sie oben im Menü auf **Administrator**.

    ![Administrator] (./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Administrator")

7.  Klicken Sie auf der Registerkarte **General Settings** auf **Sicherheit**.

    ![Sicherheit] (./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Sicherheit")

8.  Führen Sie im Abschnitt **Sicherheit** die folgenden Schritte aus:

    ![Einmaliges Anmelden] (./media/active-directory-saas-freshdesk-tutorial/IC776770.png "Einmaliges Anmelden")

    1.  Wählen Sie für **Einzelne anmelden (SSO)** **auf**.
    2.  **SAML-SSO**auswählen.
    3.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **SAML-Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Freshdesk** .
    4.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Freshdesk** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **URL Abmelden** .
    5.  Kopieren Sie den **Fingerabdruck** Wert aus dem exportierte Zertifikat, und fügen Sie ihn in das Textfeld **Sicherheit Zertifikat Fingerabdruck** .  

        >[AZURE.TIP]Weitere Informationen hierzu finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

    6.  Klicken Sie auf **Speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-freshdesk-tutorial/IC776771.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Freshdesk zu ermöglichen, müssen er in Freshdesk bereitgestellt werden.  
Im Falle von Freshdesk ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **Freshdesk** .

2.  Klicken Sie oben im Menü auf **Administrator**.

    ![Administrator] (./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Administrator")

3.  Klicken Sie auf der Registerkarte **General Settings** auf **Agents**.

    ![Agents] (./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Agents")

4.  Klicken Sie auf **neue Agent**.

    ![Neuer Agent] (./media/active-directory-saas-freshdesk-tutorial/IC776774.png "Neuer Agent")

5.  Klicken Sie im Dialogfeld Agenteninformationen führen Sie die folgenden Schritte aus:

    ![Agent-Informationen] (./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Agent-Informationen")

    1.  Geben Sie in das Textfeld **Vollständiger Name** den Namen des Kontos Azure AD-, die Sie bereitstellen möchten.
    2.  Geben Sie in das Textfeld **E-Mail** die Azure AD-e-Mail-Adresse der Firma Azure AD-, die Sie bereitstellen möchten.
    3.  Geben Sie im Textfeld **Titel** den Titel des Kontos Azure AD-, die Sie bereitstellen möchten.
    4.  Wählen Sie **Agents Rolle**aus, und klicken Sie dann auf **zuweisen**.
    5.  Klicken Sie auf **Speichern**.
    
        >[AZURE.NOTE] Der Inhaber des Azure AD-Konto erhalten eine e-Mail-Nachricht, die enthält einen Link, um das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen Freshdesk Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Freshdesk bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-freshdesk-perform-the-following-steps"></a>Zuweisen von Benutzern zu Freshdesk führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Freshdesk **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-freshdesk-tutorial/IC776776.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-freshdesk-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).