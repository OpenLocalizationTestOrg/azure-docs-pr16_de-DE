<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Kintone | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Kintone mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kintone"></a>Lernprogramm: Azure-Active Directory-Integration in Kintone
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Kintone anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Kintone einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Kintone zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Kintone (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Kintone
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-kintone-tutorial/IC785859.png "Szenario")
##<a name="enabling-the-application-integration-for-kintone"></a>Aktivieren die Anwendungsintegration für Kintone
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Kintone aktiviert.

###<a name="to-enable-the-application-integration-for-kintone-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Kintone aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kintone-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-kintone-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-kintone-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-kintone-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Kintone**.

    ![Katalog der Anwendung] (./media/active-directory-saas-kintone-tutorial/IC785867.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Kintone aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Kintone] (./media/active-directory-saas-kintone-tutorial/IC785871.png "Kintone")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Kintone mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Kintone** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-kintone-tutorial/IC785872.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Kintone auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-kintone-tutorial/IC785873.png "Konfigurieren Sie einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Kintone melden Sie sich auf URL** Geben Sie Ihre URL dem folgenden Muster "*https://company.kintone.com*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-kintone-tutorial/IC785875.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Kintone** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-kintone-tutorial/IC785878.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens **Kintone** als Administrator.

6.  Klicken Sie auf **Einstellungen**.

    ![Einstellungen] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Einstellungen")

7.  Klicken Sie auf **Benutzer und Systemadministration**.

    ![Benutzer und Systemadministration] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Benutzer und Systemadministration")

8.  Klicken Sie unter **System Administration \> Sicherheit** klicken Sie auf **Login**.

    ![Anmeldung] (./media/active-directory-saas-kintone-tutorial/IC785881.png "Anmeldung")

9.  Klicken Sie auf **Aktivieren SAML-Authentifizierung**.

    ![SAML-Authentifizierung] (./media/active-directory-saas-kintone-tutorial/IC785882.png "SAML-Authentifizierung")

10. Führen Sie im Abschnitt SAML-Authentifizierung die folgenden Schritte aus:

    ![SAML-Authentifizierung] (./media/active-directory-saas-kintone-tutorial/IC785883.png "SAML-Authentifizierung")

    1.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Kintone** .
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Kintone** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **URL Abmelden** .
    3.  Klicken Sie auf **Durchsuchen** , um Ihr heruntergeladene Zertifikat hochladen.
    4.  Klicken Sie auf **Speichern**.

11. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-kintone-tutorial/IC785884.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Kintone zu ermöglichen, müssen er in Kintone bereitgestellt werden.  
Im Falle von Kintone ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Kintone** an.

2.  Klicken Sie auf **Einstellungen**.

    ![Einstellungen] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Einstellungen")

3.  Klicken Sie auf **Benutzer und Systemadministration**.

    ![Benutzer und Systemadministration] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Benutzer und Systemadministration")

4.  Klicken Sie unter **Benutzer-Administration**klicken Sie auf **Abteilungen und Benutzer**.

    ![Abteilung und Benutzer] (./media/active-directory-saas-kintone-tutorial/IC785888.png "Abteilung und Benutzer")

5.  Klicken Sie auf **Neuer Benutzer**.

    ![Neue Benutzer] (./media/active-directory-saas-kintone-tutorial/IC785889.png "Neue Benutzer")

6.  Führen Sie im Abschnitt **Neuer Benutzer** die folgenden Schritte aus:

    ![Neue Benutzer] (./media/active-directory-saas-kintone-tutorial/IC785890.png "Neue Benutzer")

    1.  Geben Sie einen **Anzeigenamen ein**, **Benutzernamen**, **Neues Kennwort**, **Kennwort bestätigen**, **E-Mail-Adresse** und andere Details eines gültigen AAD-Kontos ein, die Sie in der zugehörigen Texboxes bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Alle anderen Kintone Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Kintone bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-kintone-perform-the-following-steps"></a>Zuweisen von Benutzern zu Kintone führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Kintone **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-kintone-tutorial/IC785891.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-kintone-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).