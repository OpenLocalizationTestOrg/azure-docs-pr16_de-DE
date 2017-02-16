<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Pufferzeit | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Pufferzeit mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-slack"></a>Lernprogramm: Azure-Active Directory-Integration in Pufferzeit
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Pufferzeit anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Pufferzeit einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Pufferzeit zugewiesen haben können einzelne melden Sie sich bei der Anwendung an Ihre Pufferzeit Firmenwebsite (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Pufferzeit
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-slack-tutorial/IC794980.png "Szenario")

##<a name="enabling-the-application-integration-for-slack"></a>Aktivieren die Anwendungsintegration für Pufferzeit
  
In diesem Abschnitt Ziel ist es zu gliedernden wie Sie die Anwendungsintegration für Pufferzeit zu aktivieren.

###<a name="to-enable-the-application-integration-for-slack-perform-the-following-steps"></a>Um die Anwendungsintegration für Pufferzeit zu aktivieren, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-slack-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-slack-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-slack-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-slack-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Pufferzeit**ein.

    ![Katalog der Anwendung] (./media/active-directory-saas-slack-tutorial/IC794981.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Pufferzeit aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Szenario] (./media/active-directory-saas-slack-tutorial/IC796925.png "Szenario")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Pufferzeit mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Klicken Sie auf der Seite Anwendung Integration **Pufferzeit** im Azure klassischen Portal auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren Single Sign On** .

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-slack-tutorial/IC794982.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie, wie Benutzer bei der Pufferzeit auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-slack-tutorial/IC794983.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Pufferzeit melden Sie sich In URL** die URL Ihrer Pufferzeit Mandanten (z. B.: "*https://azuread.slack.com*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-slack-tutorial/IC794984.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Pufferzeit** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-slack-tutorial/IC794985.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Pufferzeit als Administrator.

6.  Wechseln Sie zu **an Microsoft Azure AD \> Einstellungen der Teamwebsite**.

    ![Team-Einstellungen] (./media/active-directory-saas-slack-tutorial/IC794986.png "Team-Einstellungen")

7.  Im Abschnitt **Team Einstellungen** klicken Sie auf der Registerkarte **Authentifizierung** , und klicken Sie dann auf **Einstellungen ändern**.

    ![Team-Einstellungen] (./media/active-directory-saas-slack-tutorial/IC794987.png "Team-Einstellungen")

8.  Klicken Sie im Dialogfeld **Einstellungen der SAML-Authentifizierung** führen Sie die folgenden Schritte aus:

    ![SAML-Einstellungen] (./media/active-directory-saas-slack-tutorial/IC794988.png "SAML-Einstellungen")

    1.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Pufferzeit** im Azure klassischen Portal kopieren Sie des Werts **SAML SSO-URL** , und fügen Sie ihn in das Textfeld **SAML 2.0 Endpunkt (HTTP)** .
    2.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Pufferzeit** im Azure klassischen Portal kopieren Sie den Wert für die **URL des Herausgebers** , und fügen Sie ihn in das Textfeld **Identität Anbieter Herausgeber** .
    3.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.
    
        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    4.  Öffnen des Base-64-codierten Zertifikats in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **Öffentlichen Zertifikat** .
    5.  Deaktivieren Sie die **Benutzer können ihre e-Mail-Adresse zu ändern**.
    6.  Wählen Sie **Benutzern erlauben, wählen Sie ihre eigenen Benutzernamen**ein.
    7.  Wählen Sie als **Authentifizierung für Ihr Team von verwendet werden muss** **Es ist optional**.
    8.  Klicken Sie auf **Konfiguration speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-slack-tutorial/IC794989.png "Konfigurieren Sie einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Pufferzeit zu ermöglichen, müssen er in Pufferzeit bereitgestellt werden.
  
Es gibt keine Aktionselement für Ihre Benutzer bereitgestellt werden, um die Pufferzeit zu konfigurieren.  
Wenn ein zugewiesene Benutzer zur Anmeldung bei Pufferzeit versucht, wird ein Pufferzeit-Konto bei Bedarf automatisch erstellt.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-slack-perform-the-following-steps"></a>Zuweisen von Benutzern zu Pufferzeit führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite Anwendung Integration **Pufferzeit **auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-slack-tutorial/IC794990.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-slack-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).