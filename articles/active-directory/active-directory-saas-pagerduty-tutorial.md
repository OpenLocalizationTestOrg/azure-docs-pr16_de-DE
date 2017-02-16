<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Pagerduty | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Pagerduty mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Lernprogramm: Azure-Active Directory-Integration in Pagerduty
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Pagerduty anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Pagerduty
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Pagerduty zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Pagerduty (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Pagerduty
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-pagerduty-tutorial/IC778528.png "Szenario")
##<a name="enabling-the-application-integration-for-pagerduty"></a>Aktivieren die Anwendungsintegration für Pagerduty
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Pagerduty aktiviert.

###<a name="to-enable-the-application-integration-for-pagerduty-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Pagerduty aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Verwaltungsportal Azure, klicken Sie auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-pagerduty-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-pagerduty-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-pagerduty-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-pagerduty-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Pagerduty**.

    ![Katalog der Anwendung] (./media/active-directory-saas-pagerduty-tutorial/IC778529.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Pagerduty aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![PagerDuty] (./media/active-directory-saas-pagerduty-tutorial/IC778530.png "PagerDuty")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Pagerduty mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Pagerduty** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-pagerduty-tutorial/IC778531.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Pagerduty auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-pagerduty-tutorial/IC778532.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Pagerduty melden Sie sich In die URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. Pagerduty.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von app-url] (./media/active-directory-saas-pagerduty-tutorial/IC778533.png "Konfigurieren von app-url")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Pagerduty** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-pagerduty-tutorial/IC778534.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Pagerduty als Administrator.

6.  Klicken Sie auf **Konten-Einstellungen**, klicken Sie im Menü oben.

    ![Kontoeinstellungen] (./media/active-directory-saas-pagerduty-tutorial/IC778535.png "Kontoeinstellungen")

7.  Klicken Sie auf **einmaliges Anmelden**.

    ![Einmaliges Anmelden] (./media/active-directory-saas-pagerduty-tutorial/IC778536.png "Einmaliges Anmelden")

8.  Führen Sie die folgenden Schritte aus, klicken Sie auf der Seite aktivieren einmaliges Anmelden (SSO):

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-pagerduty-tutorial/IC778537.png "Einmaliges Anmelden aktivieren")

    1.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    2.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **X 509-Zertifikat**
    3.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Anmelde-URL** , im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden bei Pagerduty** .
    4.  Im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden bei Pagerduty** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **URL Abmelden** .
    5.  Aktivieren Sie das Kontrollkästchen Sie **auf einmaliges Anmelden**.
    6.  Klicken Sie auf **Änderungen speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-pagerduty-tutorial/IC778538.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Pagerduty zu ermöglichen, müssen er in Pagerduty bereitgestellt werden.  
Im Falle von Pagerduty ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **Pagerduty** .

2.  Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

3.  Klicken Sie auf **Benutzer hinzufügen**.

    ![Hinzufügen von Benutzern] (./media/active-directory-saas-pagerduty-tutorial/IC778539.png "Hinzufügen von Benutzern")

4.  Klicken Sie im Dialogfeld **Ihr Team einladen** Geben Sie **vor- und Nachname** und die **e-Mail-** Adresse des Benutzers Azure AD-, die Sie bereitstellen, klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Senden eingeladen werden**möchten.

    ![Laden Sie Ihr team] (./media/active-directory-saas-pagerduty-tutorial/IC778540.png "Laden Sie Ihr team")

    >[AZURE.NOTE] Alle hinzugefügten Benutzer erhalten eine Einladung zu einem PagerDuty Konto erstellen.

>[AZURE.NOTE] Alle anderen Pagerduty Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Pagerduty bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-pagerduty-perform-the-following-steps"></a>Zuweisen von Benutzern zu Pagerduty führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Pagerduty **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-pagerduty-tutorial/IC778541.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-pagerduty-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).