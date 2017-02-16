<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Intacct | Microsoft Azure" 
    description="Erfahren Sie, wie Intacct mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-intacct"></a>Lernprogramm: Azure-Active Directory-Integration in Intacct
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Intacct anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Intacct
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Intacct zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Intacct (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Intacct
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-intacct-tutorial/IC790030.png "Szenario")
##<a name="enabling-the-application-integration-for-intacct"></a>Aktivieren die Anwendungsintegration für Intacct
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Intacct aktiviert.

###<a name="to-enable-the-application-integration-for-intacct-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Intacct aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-intacct-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-intacct-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-intacct-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-intacct-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Intacct**.

    ![Katalog der Anwendung] (./media/active-directory-saas-intacct-tutorial/IC790031.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Intacct aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Intacct] (./media/active-directory-saas-intacct-tutorial/IC790032.png "Intacct")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Intacct mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Intacct** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-intacct-tutorial/IC790033.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Intacct auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-intacct-tutorial/IC790034.png "Konfigurieren Sie einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Intacct melden Sie sich auf URL** Geben Sie Ihre URL dem folgenden Muster "*https://Intacct.com/company*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-intacct-tutorial/IC790035.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Intacct** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-intacct-tutorial/IC790036.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Intacct als Administrator.

6.  Klicken Sie auf der Registerkarte **Unternehmen** , und klicken Sie dann auf **Firmeninfos**.

    ![Unternehmen] (./media/active-directory-saas-intacct-tutorial/IC790037.png "Unternehmen")

7.  Klicken Sie auf der Registerkarte **Sicherheit** , und klicken Sie dann auf **Bearbeiten**.

    ![Sicherheit] (./media/active-directory-saas-intacct-tutorial/IC790038.png "Sicherheit")

8.  Führen Sie im Abschnitt **für einmaliges Anmelden (SSO)** die folgenden Schritte aus:

    ![Einmaliges Anmelden] (./media/active-directory-saas-intacct-tutorial/IC790039.png "Einmaliges Anmelden")

    1.  Wählen Sie **für einmaliges Klicken Sie auf Aktivieren**.
    2.  Wählen Sie als **Identität Anbietertyp** **SAML 2.0**ein.
    3.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Intacct** kopieren Sie den Wert des **Herausgebers URL** , und fügen Sie ihn in das Textfeld **URL des Herausgebers** .
    4.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Intacct** .
    5.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.
        
        >[AZURE.TIP]Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    6.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **Zertifikat**
    7.  Klicken Sie auf **Speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-intacct-tutorial/IC790040.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Intacct zu ermöglichen, müssen er in Intacct bereitgestellt werden.  
Im Falle von Intacct ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **Intacct** .

2.  Klicken Sie auf der Registerkarte **Unternehmen** , und klicken Sie dann auf **Benutzer**.

    ![Benutzer] (./media/active-directory-saas-intacct-tutorial/IC790041.png "Benutzer")

3.  Klicken Sie auf der Registerkarte ' **Hinzufügen** '

    ![Hinzufügen] (./media/active-directory-saas-intacct-tutorial/IC790042.png "Hinzufügen")

4.  Führen Sie im Abschnitt **Benutzerinformationen** die folgenden Schritte aus:

    ![Benutzerinformationen] (./media/active-directory-saas-intacct-tutorial/IC790043.png "Benutzerinformationen")

    1.  Geben Sie die **Benutzer-ID**, der **Nachname**, **Vorname**, die **e-Mail-Adresse**, den **Titel** und das **Mobiltelefon** ein Azure AD-Konto, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Wählen Sie die **admininistratorberechtigungen** eines Kontos Azure AD-, die Sie bereitstellen möchten.
    3.  Klicken Sie auf **Speichern**.
        
        >[AZURE.NOTE] Der Inhaber des AAD Konto wird erhalten eine e-Mail, und führen Sie einen Link, um ihr Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen Intacct Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Intacct bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Benutzer Azure AD-erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-intacct-perform-the-following-steps"></a>Zuweisen von Benutzern zu Intacct führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Intacct **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-intacct-tutorial/IC790044.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-intacct-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).