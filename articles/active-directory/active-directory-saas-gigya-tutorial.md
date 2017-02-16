<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Gigya | Microsoft Azure" 
    description="Erfahren Sie, wie Gigya mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-gigya"></a>Lernprogramm: Azure-Active Directory-Integration in Gigya
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Gigya anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Gigya für einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Gigya zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Gigya (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Gigya
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-gigya-tutorial/IC789512.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="enabling-the-application-integration-for-gigya"></a>Aktivieren die Anwendungsintegration für Gigya
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Gigya aktiviert.

###<a name="to-enable-the-application-integration-for-gigya-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Gigya aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-gigya-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-gigya-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-gigya-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-gigya-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Gigya**.

    ![Katalog der Anwendung] (./media/active-directory-saas-gigya-tutorial/IC789513.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Gigya aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Gigya] (./media/active-directory-saas-gigya-tutorial/IC789527.png "Gigya")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Gigya mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Gigya** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-gigya-tutorial/IC789528.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Gigya auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-gigya-tutorial/IC789529.png "Konfigurieren Sie einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Gigya melden Sie sich auf URL** Geben Sie Ihre URL dem folgenden Muster "*http://company.gigya.com*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-gigya-tutorial/IC789530.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Gigya** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-gigya-tutorial/IC789531.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Gigya als Administrator.

6.  Wechseln Sie zu **Einstellungen \> SAML-Login**, und klicken Sie dann auf die Schaltfläche **Hinzufügen** .

    ![SAML-Anmeldung] (./media/active-directory-saas-gigya-tutorial/IC789532.png "SAML-Anmeldung")

7.  Führen Sie im Abschnitt **SAML-Anmeldung** die folgenden Schritte aus:

    ![SAML-Konfiguration] (./media/active-directory-saas-gigya-tutorial/IC789533.png "SAML-Konfiguration")

    1.  Geben Sie in das Textfeld **Name** einen Namen für die Konfiguration aus.
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Gigya** kopieren Sie den Wert des **Herausgebers URL** , und fügen Sie ihn in das Textfeld **Herausgeber** .
    3.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Gigya** kopieren Sie den Wert für die **Einzelnen anmelden Dienst-URL** , und fügen Sie ihn in das Textfeld **Einzelne anmelden Service URL** .
    4.  Kopieren Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Gigya** im Azure klassischen Portal des Werts **Bezeichner Namensformat** , und fügen Sie ihn in das Textfeld **Name-ID-Format** .
    5.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.
        
        >[AZURE.TIP]Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    6.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **X 509-Zertifikat** .
    7.  Klicken Sie auf **Einstellungen speichern**.

8.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-gigya-tutorial/IC789534.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Gigya zu ermöglichen, müssen er in Gigya bereitgestellt werden.  
Im Falle von Gigya ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Gigya** an.

2.  Wechseln Sie zu **Admin \> Benutzer veralten**, und klicken Sie dann auf **Benutzer einladen**.

    ![Verwalten von Benutzern] (./media/active-directory-saas-gigya-tutorial/IC789535.png "Verwalten von Benutzern")

3.  Klicken Sie im Dialogfeld Benutzer einladen führen Sie die folgenden Schritte aus:

    ![Einladen von Benutzern] (./media/active-directory-saas-gigya-tutorial/IC789536.png "Einladen von Benutzern")

    1.  Geben Sie in das Textfeld **E-Mail** den e-Mail-Alias ein gültiges Azure Active Directory-Konto, die, das Sie bereitstellen möchten.
    2.  Klicken Sie auf **Benutzer einladen**.
    
        >[AZURE.NOTE] Der Inhaber des Azure Active Directory-Konto erhalten eine e-Mail-Nachricht, die enthält einen Link, um das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen Gigya Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Gigya bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-gigya-perform-the-following-steps"></a>Zuweisen von Benutzern zu Gigya führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Gigya **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-gigya-tutorial/IC789537.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-gigya-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).