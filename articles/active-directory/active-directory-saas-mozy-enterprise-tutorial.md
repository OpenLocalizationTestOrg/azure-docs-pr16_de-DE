<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Mozy Enterprise | Microsoft Azure" 
    description="Erfahren Sie, wie Mozy Enterprise mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a>Lernprogramm: Azure-Active Directory-Integration in Mozy Enterprise
  
Ziel dieses Lernprogramms ist die Integration von Azure und Mozy Enterprise angezeigt.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Mozy Enterprise
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Mozy Enterprise zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Mozy Enterprise (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Mozy Enterprise
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777308.png "Szenario")
##<a name="enabling-the-application-integration-for-mozy-enterprise"></a>Aktivieren die Anwendungsintegration für Mozy Enterprise
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Mozy Enterprise aktiviert.

###<a name="to-enable-the-application-integration-for-mozy-enterprise-perform-the-following-steps"></a>Um die Anwendungsintegration für Mozy Enterprise zu aktivieren, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mozy-enterprise-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-mozy-enterprise-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-mozy-enterprise-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-mozy-enterprise-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Mozy Enterprise**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777309.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Mozy Enterprise**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Mozy Enterprise] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777310.png "Mozy Enterprise")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Mozy Enterprise mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikat in Ihrem Mandanten Mozy Enterprise hochladen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Mozy Enterprise** Application Integration klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-mozy-enterprise-tutorial/IC771709.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer Mozy Enterprise bei, klicken Sie auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777311.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Mozy Enterprise melden Sie sich In URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. MozyEnterprise.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der app-URL] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777312.png "Konfigurieren der app-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Mozy Enterprise** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777313.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Mozy Enterprise als Administrator.

6.  Klicken Sie im Abschnitt **Konfiguration** auf **Authentifizierungsrichtlinie**.

    ![Authentifizierungsrichtlinie] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777314.png "Authentifizierungsrichtlinie")

7.  Führen Sie in dem Abschnitt **Richtlinie Authentifizierung** die folgenden Schritte aus:

    ![Authentifizierungsrichtlinie] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777315.png "Authentifizierungsrichtlinie")

    1.  Wählen Sie als **Anbieter** **Verzeichnisdienst** ein.
    2.  Wählen Sie **LDAP-Pushbenachrichtigungen verwenden**.
    3.  Klicken Sie auf der Registerkarte **SAML-Authentifizierung** .
    4.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Mozy Enterprise** kopieren Sie den Wert für die **Authentifizierung anfordern URL** , und fügen Sie ihn in das Textfeld **URL Authentifizierung** .
    5.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Mozy Enterprise** kopieren Sie des Werts **Identität Anbieter-ID** , und fügen Sie ihn in das Textfeld **SAML-Endpunkt** .
    6.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP]Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    7.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie das gesamte Zertifikat in das Textfeld **SAML-Zertifikat** .
    8.  Wählen Sie **Aktivieren SSO für Administratoren, mit deren Netzwerkanmeldeinformationen anmelden**aus.
    9.  Klicken Sie auf **Änderungen speichern**.

8.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Mozy Enterprise** wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **abgeschlossen**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777316.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzer zur Anmeldung bei Mozy Enterprise zu aktivieren, müssen er in Mozy Enterprise bereitgestellt werden.  
Im Falle von Mozy Enterprise ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **Mozy Enterprise** an.

2.  Klicken Sie auf **Benutzer**, und klicken Sie dann auf **Neuen Benutzer hinzufügen**.

    ![Benutzer] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777317.png "Benutzer")

    >[AZURE.NOTE]Die Option für die **Neuen Benutzer hinzufügen** , wird nur angezeigt, nur, wenn **Mozy** als Anbieter unter **Authentifizierungsrichtlinie**ausgewählt ist. Wenn SAML-Authentifizierung konfiguriert ist werden dann Benutzer automatisch bei ihrer ersten Anmeldung durch einmaliges Klicken Sie auf hinzugefügt.

3.  Führen Sie die folgenden Schritte aus, klicken Sie im Benutzerdialogfeld neuen:

    ![Hinzufügen von Benutzern] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777318.png "Hinzufügen von Benutzern")

    1.  Wählen Sie eine Gruppe aus der Liste **Wählen Sie eine Gruppe** aus.
    2.  Wählen Sie in der Liste **Was des Benutzers geben Sie** einen Typ aus.
    3.  Geben Sie in das Textfeld für den **Benutzernamen** den Namen des Benutzers Azure AD-aus.
    4.  Geben Sie in das Textfeld **-e-Mail** die e-Mail-Adresse des Benutzers Azure AD-aus.
    5.  Wählen Sie die **Benutzer folgendermaßen: e-Mail senden**aus.
    6.  Klicken Sie auf **Benutzer hinzufügen**.

    >[AZURE.NOTE]Nach dem Erstellen des Benutzers, wird eine e-Mail-Nachricht an den Azure AD-Benutzer gesendet werden, die enthält einen Link, um das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE]Alle anderen Mozy Enterprise Benutzer Konto Creation Tools können oder APIs bereitgestellt von Mozy Enterprise bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
 
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-mozy-enterprise-perform-the-following-steps"></a>Zuweisen von Benutzern zu Mozy Enterprise führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Mozy Enterprise **Application Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-mozy-enterprise-tutorial/IC777319.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-mozy-enterprise-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).