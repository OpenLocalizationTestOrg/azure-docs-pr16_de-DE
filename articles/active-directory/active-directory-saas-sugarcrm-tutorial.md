<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration-Integration in SugarCRM | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von SugarCRM mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-sugarcrm"></a>Lernprogramm: Azure-Active Directory-Integration-Integration in SugarCRM
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Zucker CRM anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Zucker CRM einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Zucker CRM zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Zucker CRM (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Zucker CRM
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-sugarcrm-tutorial/IC795881.png "Szenario")

##<a name="enabling-the-application-integration-for-sugar-crm"></a>Aktivieren die Anwendungsintegration für Zucker CRM
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Zucker CRM aktiviert.

###<a name="to-enable-the-application-integration-for-sugar-crm-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Zucker CRM aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sugarcrm-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-sugarcrm-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-sugarcrm-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-sugarcrm-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Zucker CRM**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-sugarcrm-tutorial/IC795882.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Zucker CRM aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Zucker CRM] (./media/active-directory-saas-sugarcrm-tutorial/IC795883.png "Zucker CRM")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren Zucker CRM mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikat in Ihrem Mandanten Zucker CRM hochladen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Zucker CRM** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-sugarcrm-tutorial/IC795884.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Zucker CRM auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-sugarcrm-tutorial/IC795885.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Zucker CRM melden Sie sich auf URL** die URL, die von den Benutzern die Anmeldung an Ihrer Anwendung Zucker CRM verwendet (z. B.: "*http://company.sugarondemand.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-sugarcrm-tutorial/IC795886.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Zucker CRM** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-sugarcrm-tutorial/IC796918.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Zucker CRM als Administrator.

6.  Wechseln Sie zu **Administrator**.

    ![Administrator] (./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Administrator")

7.  Klicken Sie im Abschnitt **Verwaltung** auf **Kennwort Management**.

    ![Verwaltung] (./media/active-directory-saas-sugarcrm-tutorial/IC795889.png "Verwaltung")

8.  Wählen Sie **SAML-Authentifizierung aktivieren**.

    ![Verwaltung] (./media/active-directory-saas-sugarcrm-tutorial/IC795890.png "Verwaltung")

9.  Führen Sie im Abschnitt **SAML-Authentifizierung** die folgenden Schritte aus:

    ![SAML-Authentifizierung] (./media/active-directory-saas-sugarcrm-tutorial/IC795891.png "SAML-Authentifizierung")

    1.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Zucker CRM** .
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Zucker CRM** kopieren Sie des Werts **Remote Anmelde-URL** , und klicken Sie dann in das Textfeld **SLO URL** einzufügen.
    3.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    4.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie das gesamte Zertifikat in das Textfeld **X 509-Zertifikat** .
    5.  Klicken Sie auf **Speichern**.

10. Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Zucker CRM** wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **abgeschlossen**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-sugarcrm-tutorial/IC796919.png "Konfigurieren Sie einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Zucker CRM zu ermöglichen, müssen er Zucker CRM bereitgestellt werden.  
Bei Zucker CRM ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Zucker CRM** an.

2.  Wechseln Sie zu **Administrator**.

    ![Administrator] (./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Administrator")

3.  Klicken Sie im Abschnitt **Verwaltung** auf **User Management**.

    ![Verwaltung] (./media/active-directory-saas-sugarcrm-tutorial/IC795893.png "Verwaltung")

4.  Wechseln Sie zu **Benutzer \> erstellen neuen Benutzer**.

    ![Erstellen neuen Benutzer] (./media/active-directory-saas-sugarcrm-tutorial/IC795894.png "Erstellen neuen Benutzer")

5.  Klicken Sie auf der Registerkarte **Benutzerprofil** führen Sie die folgenden Schritte aus:

    ![Neuer Benutzer] (./media/active-directory-saas-sugarcrm-tutorial/IC795895.png "Neuer Benutzer")

    1.  Geben Sie in die verknüpfte Textfelder Benutzernamen, Nachname und e-Mail-Adresse eines gültigen Azure Active Directory-Benutzers ein.

6.  Wählen Sie als **Status** **aktiv**.

7.  Klicken Sie auf die Registerkarte Kennwort führen Sie die folgenden Schritte aus:

    ![Neuer Benutzer] (./media/active-directory-saas-sugarcrm-tutorial/IC795896.png "Neuer Benutzer")

    1.  Geben Sie das Kennwort in das Textfeld verwandte ein.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Alle anderen Zucker CRM Benutzer Konto Creation Tools können oder APIs zur Verfügung gestellt von Zucker CRM bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-sugar-crm-perform-the-following-steps"></a>Zuweisen von Benutzern zu Zucker CRM führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Zucker CRM** Integration Anwendung auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-sugarcrm-tutorial/IC795897.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-sugarcrm-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).