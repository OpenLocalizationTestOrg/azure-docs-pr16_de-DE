<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Lucidchart | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Lucidchart mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lucidchart"></a>Lernprogramm: Azure-Active Directory-Integration in Lucidchart
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Lucidchart anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Eine Lucidchart einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Lucidchart zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Lucidchart (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Lucidchart
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-lucidchart-tutorial/IC791183.png "Szenario")
##<a name="enabling-the-application-integration-for-lucidchart"></a>Aktivieren die Anwendungsintegration für Lucidchart
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Lucidchart aktiviert.

###<a name="to-enable-the-application-integration-for-lucidchart-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Lucidchart aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-lucidchart-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-lucidchart-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-lucidchart-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-lucidchart-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Lucidchart**.

    ![Katalog der Anwendung] (./media/active-directory-saas-lucidchart-tutorial/IC791184.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Lucidchart aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Lucidchart] (./media/active-directory-saas-lucidchart-tutorial/IC791185.png "Lucidchart")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Lucidchart mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Lucidchart** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-lucidchart-tutorial/IC791186.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Lucidchart auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-lucidchart-tutorial/IC791187.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Lucidchart melden Sie sich auf URL** die URL, die von den Benutzern verwendet, um melden Sie sich an Ihrer Anwendung Lucidchart auf (z. B.: "*https://chart2.office.lucidchart.com/saml/sso/azure*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-lucidchart-tutorial/IC791188.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Lucidchart** zum Herunterladen der Metadaten Ihrer klicken Sie auf **Metadaten herunterladen**, und speichern Sie die Datendatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-lucidchart-tutorial/IC791189.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Lucidchart als Administrator.

6.  Klicken Sie im Menü oben auf **Teamwebsite**.

    ![Team] (./media/active-directory-saas-lucidchart-tutorial/IC791190.png "Team")

7.  Klicken Sie auf **Anwendung \> verwalten SAML**.

    ![Verwalten von SAML] (./media/active-directory-saas-lucidchart-tutorial/IC791191.png "Verwalten von SAML")

8.  Führen Sie auf der Seite **Einstellungen der SAML-Authentifizierung** Dialogfeld die folgenden Schritte aus:

    1.  Wählen Sie **SAML-Authentifizierung aktivieren**, und klicken Sie dann auf **Optional**.
        ![Einstellungen der SAML-Authentifizierung] (./media/active-directory-saas-lucidchart-tutorial/IC791192.png "Einstellungen der SAML-Authentifizierung")
    2.  Klicken Sie in das Textfeld **Domäne** ein Geben Sie Ihre Domäne, und klicken Sie dann auf **Zertifikat ändern**.
        ![Zertifikat ändern] (./media/active-directory-saas-lucidchart-tutorial/IC791193.png "Zertifikat ändern")
    3.  Öffnen Sie die heruntergeladene Metadatendatei, kopieren Sie den Inhalt aus, und fügen Sie ihn in das Textfeld **Metadaten hochladen** .
        ![Hochladen von Metadaten] (./media/active-directory-saas-lucidchart-tutorial/IC791194.png "Hochladen von Metadaten")
    4.  Wählen Sie **automatisch Hinzufügen neuer Benutzer an das Team**aus, und klicken Sie dann auf **Änderungen speichern**.
        ![Speichern der Änderungen] (./media/active-directory-saas-lucidchart-tutorial/IC791195.png "Speichern der Änderungen")

9.  Wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-lucidchart-tutorial/IC791196.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Es gibt keine Aktionselement für Ihre Benutzer zu Lucidchart provisioning konfigurieren.  
Wenn Sie ein zugeordneten Benutzer versucht, die bei Verwendung der Access-Systemsteuerung Lucidchart anzumelden, überprüft Lucidchart an, ob der Benutzer vorhanden ist.  
Wenn es noch kein Benutzerkonto verfügbar ist, wird es von Lucidchart automatisch erstellt.
##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-lucidchart-perform-the-following-steps"></a>Zuweisen von Benutzern zu Lucidchart führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Lucidchart **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-lucidchart-tutorial/IC791197.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-lucidchart-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).