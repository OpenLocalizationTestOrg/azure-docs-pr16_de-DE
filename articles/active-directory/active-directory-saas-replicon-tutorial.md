<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Replicon | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Replicon mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-replicon"></a>Lernprogramm: Azure-Active Directory-Integration in Replicon
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Replicon anzeigen. Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Replicon
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Replicon zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Replicon (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Replicon
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-replicon-tutorial/IC777798.png "Szenario")
##<a name="enabling-the-application-integration-for-replicon"></a>Aktivieren die Anwendungsintegration für Replicon
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Replicon aktiviert.

###<a name="to-enable-the-application-integration-for-replicon-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Replicon aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-replicon-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-replicon-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-replicon-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Replicon**.

    ![Katalog der Anwendung] (./media/active-directory-saas-replicon-tutorial/IC777799.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Replicon aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Replicon] (./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Replicon mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Replicon** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-replicon-tutorial/IC777801.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Replicon auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-replicon-tutorial/IC777802.png "Konfigurieren einmaliges Anmelden")

3.  Führen Sie auf der Seite **Konfigurieren der App-URL** die folgenden Schritte aus:

    ![Konfigurieren der app-URL] (./media/active-directory-saas-replicon-tutorial/IC777803.png "Konfigurieren der app-URL")

    1.  Geben Sie in das Textfeld **Replicon melden Sie sich auf URL** die URL Ihrer Replicon Mandanten (z. B.: *https://na2.replicon.com/company/saml2/sp-sso/post*).
    2.  Geben Sie Ihre Replicon **AssertionConsumerService** URL in das Textfeld **Replicon Antwort-URL** (z. B.: *https://global.replicon.com/! / saml2/Unternehmen/Sso/Beitrag*).  

        >[AZURE.NOTE] Abrufen der URL können Sie aus den Metadaten Replicon am:         **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.

    3.  Klicken Sie auf **Weiter**

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Replicon** zum Herunterladen der Metadaten Ihrer klicken Sie auf **Herunterladen von Metadaten**, und klicken Sie dann speichern Sie die Metadaten auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-replicon-tutorial/IC777804.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Replicon als Administrator.

6.  Zum Konfigurieren der SAML 2.0, führen Sie die folgenden Schritte aus:

    ![Aktivieren von SAML-Authentifizierung] (./media/active-directory-saas-replicon-tutorial/IC777805.png "Aktivieren von SAML-Authentifizierung")

    1.  Klicken Sie zum Anzeigen des Dialogfelds **EnableSAML Authentication2** fügen Sie folgenden Ihre URL ein an, nach dem Schlüssel des Unternehmens:  
        **/Services/SecurityService1.svc/Help/Test/EnableSAMLAuthentication2**  
        Die nachstehende Abbildung zeigt das Schema der die vollständige URL aus:  
        **https://Na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**
    2.  Klicken Sie auf die **+** zu den **v20Configuration** Abschnitt zu erweitern.
    3.  Klicken Sie auf die **+** zu den **MetaDataConfiguration** Abschnitt zu erweitern.
    4.  Klicken Sie auf **Datei auswählen**, um Ihre Identität Anbieter Metadaten XML-Datei aus, und klicken Sie auf **Absenden**.

7.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-replicon-tutorial/IC778418.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Replicon zu ermöglichen, müssen er in Replicon bereitgestellt werden.  
Im Falle von Replicon ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  In einem Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Replicon als Administrator.

2.  Wechseln Sie zu **Administration \> Benutzer**.

    ![Benutzer] (./media/active-directory-saas-replicon-tutorial/IC777806.png "Benutzer")

3.  Klicken Sie auf **+ Benutzer hinzufügen**.

    ![Fügen Sie Benutzer hinzu] (./media/active-directory-saas-replicon-tutorial/IC777807.png "Fügen Sie Benutzer hinzu")

4.  Führen Sie im Abschnitt **Benutzerprofil** die folgenden Schritte aus:

    ![Benutzerprofil] (./media/active-directory-saas-replicon-tutorial/IC777808.png "Benutzerprofil")

    1.  Geben Sie in das Textfeld **Benutzername** die Azure AD-e-Mail-Adresse des Benutzers Azure AD-, die Sie bereitstellen möchten.
    2.  Wählen Sie als **Authentifizierungstyp** **SSO**aus.
    3.  Geben Sie in das Textfeld **Abteilung** der Abteilung des Benutzers ein.
    4.  Wählen Sie als **Typ des Mitarbeiters** **Administrator**aus.
    5.  Klicken Sie auf **Speichern von Benutzerprofilen**.

>[AZURE.NOTE]Alle anderen Replicon Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Replicon bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-replicon-perform-the-following-steps"></a>Zuweisen von Benutzern zu Replicon führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Replicon **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-replicon-tutorial/IC777809.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-replicon-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).