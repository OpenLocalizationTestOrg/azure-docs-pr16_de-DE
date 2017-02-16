<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in ITRP | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von ITRP mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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
    ms.date="09/07/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-itrp"></a>Lernprogramm: Azure-Active Directory-Integration in ITRP
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und ITRP anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten ITRP
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie ITRP zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite ITRP (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für ITRP
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-itrp-tutorial/IC775551.png "Szenario")
##<a name="enabling-the-application-integration-for-itrp"></a>Aktivieren die Anwendungsintegration für ITRP
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für ITRP aktiviert.

###<a name="to-enable-the-application-integration-for-itrp-perform-the-following-steps"></a>Wenn die Anwendungsintegration für ITRP aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-itrp-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-itrp-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-itrp-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-itrp-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **ITRP**.

    ![Katalog der Anwendung] (./media/active-directory-saas-itrp-tutorial/IC775565.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **ITRP aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775566.png "ITRP")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu ITRP mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für ITRP konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **ITRP** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-itrp-tutorial/IC771709.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der ITRP auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-itrp-tutorial/IC775567.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **ITRP melden Sie sich In die URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. ITRP.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-itrp-tutorial/IC775568.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei ITRP** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und klicken Sie dann speichern Zertifikatsdatei lokal als **c:\\ITRP.cer**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-itrp-tutorial/IC775569.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens ITRP als Administrator.

6.  Klicken Sie oben auf der Symbolleiste auf **Einstellungen**.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775570.png "ITRP")

7.  Wählen Sie im linken Navigationsbereich **Einmaliges Anmelden**.

    ![Einmaliges Anmelden] (./media/active-directory-saas-itrp-tutorial/IC775571.png "Einmaliges Anmelden")

8.  Führen Sie im Konfigurationsabschnitt einmaliges Anmelden die folgenden Schritte aus:

    ![Einmaliges Anmelden] (./media/active-directory-saas-itrp-tutorial/IC775572.png "Einmaliges Anmelden")

    ![Einmaliges Anmelden] (./media/active-directory-saas-itrp-tutorial/IC775573.png "Einmaliges Anmelden")

    1.  Klicken Sie auf **Aktivieren**.
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei ITRP** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **Remote Abmeldung-URL** .
    3.  Kopieren Sie auf der Seite **Konfigurieren einmaliges Anmelden bei ITRP** im Azure klassischen Portal des Werts **SAML SSO-URL** , und fügen Sie ihn in das Textfeld **SAML SSO-URL** .
    4.  Kopieren Sie den **Fingerabdruck** Wert aus dem exportierte Zertifikat, und fügen Sie ihn in das Textfeld **Zertifikat Fingerabdruck** .
        
        >[AZURE.TIP]Weitere Informationen hierzu finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

    5.  Klicken Sie auf **Speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-itrp-tutorial/IC775574.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei ITRP zu ermöglichen, müssen er in ITRP bereitgestellt werden.  
Im Falle von ITRP ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **ITRP** .

2.  Klicken Sie oben auf der Symbolleiste auf **Datensätze**.

    ![Administrator] (./media/active-directory-saas-itrp-tutorial/IC775575.png "Administrator")

3.  Wählen Sie aus dem Popupmenü aus **Personen**aus.

    ![Personen] (./media/active-directory-saas-itrp-tutorial/IC775587.png "Personen")

4.  Klicken Sie auf **neue Person hinzufügen** ("+").

    ![Administrator] (./media/active-directory-saas-itrp-tutorial/IC775576.png "Administrator")

5.  Führen Sie die folgenden Schritte aus, klicken Sie im Dialogfeld neue Person hinzufügen:

    ![Benutzer] (./media/active-directory-saas-itrp-tutorial/IC775577.png "Benutzer")

    1.  Geben Sie den **Namen**, **E-Mail** mit einem gültigen AAD-Konto, die, das Sie bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Alle anderen ITRP Benutzer Konto Creation Tools können oder APIs bereitgestellt durch ITRP bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-itrp-perform-the-following-steps"></a>Zuweisen von Benutzern zu ITRP führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure AD-.

2.  Klicken Sie auf der Seite **ITRP **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-itrp-tutorial/IC775588.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-itrp-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).