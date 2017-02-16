<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in SmarterU | Microsoft Azure" 
    description="Erfahren Sie, wie SmarterU mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Lernprogramm: Azure-Active Directory-Integration in SmarterU
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und SmarterU anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten SmarterU
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie SmarterU zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite SmarterU (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für SmarterU
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-smarteru-tutorial/IC777320.png "Szenario")

##<a name="enabling-the-application-integration-for-smarteru"></a>Aktivieren die Anwendungsintegration für SmarterU
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für SmarterU aktiviert.

###<a name="to-enable-the-application-integration-for-smarteru-perform-the-following-steps"></a>Wenn die Anwendungsintegration für SmarterU aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-smarteru-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-smarteru-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-smarteru-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-smarteru-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **SmarterU**.

    ![Anwendung fallery] (./media/active-directory-saas-smarteru-tutorial/IC777321.png "Anwendung fallery")

7.  Wählen Sie im Ergebnisbereich **SmarterU aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![SmarterU] (./media/active-directory-saas-smarteru-tutorial/IC777322.png "SmarterU")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu SmarterU mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **SmarterU** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-smarteru-tutorial/IC777323.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der SmarterU auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-smarteru-tutorial/IC777324.png "Konfigurieren Sie einmaliges Anmelden")

3.  Klicken Sie auf das **Konfigurieren einmaliges Anmelden bei SmarterU** Seite zum Herunterladen der Metadaten Ihrer, klicken Sie auf **Herunterladen von Metadaten**und dann die Daten Datei lokal als **c:\\SmarterUMetaData.cer**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-smarteru-tutorial/IC777325.png "Konfigurieren Sie einmaliges Anmelden")

4.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens SmarterU als Administrator.

5.  Klicken Sie oben auf der Symbolleiste auf **Kontoeinstellungen**.

    ![Kontoeinstellungen] (./media/active-directory-saas-smarteru-tutorial/IC777326.png "Kontoeinstellungen")

6.  Führen Sie die folgenden Schritte aus, klicken Sie auf der Seite Konto konfigurieren:

    ![Externe Autorisierung] (./media/active-directory-saas-smarteru-tutorial/IC777327.png "Externe Autorisierung")

    1.  Wählen Sie die **externen Autorisierung aktivieren**.
    2.  Wählen Sie im Abschnitt **Master Login-Steuerelement** **SmarterU** Registerkarte ein.
    3.  Wählen Sie im Abschnitt **Standard-Anmeldung des Benutzers** **SmarterU** Registerkarte ein.
    4.  Wählen Sie die **Okta aktivieren**.
    5.  Kopieren Sie den Inhalt der heruntergeladenen Metadaten-Datei, und fügen Sie ihn in das Textfeld **Okta Metadaten** .
    6.  Klicken Sie auf **Speichern**.

7.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-smarteru-tutorial/IC777328.png "Konfigurieren Sie einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei SmarterU zu ermöglichen, müssen er in SmarterU bereitgestellt werden.  
Im Falle von SmarterU ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **SmarterU** .

2.  Wechseln Sie zu **Benutzer**.

3.  Führen Sie im Abschnitt Benutzer die folgenden Schritte aus:

    ![Neuer Benutzer] (./media/active-directory-saas-smarteru-tutorial/IC777329.png "Neuer Benutzer")

    1.  Klicken Sie auf **+ Benutzer**.
    2.  Geben Sie die Attributwerte der zugehörigen des Benutzerkontos Azure AD-in die folgenden Textfelder: **Primär per e-Mail senden**, **Mitarbeiter-ID**, **Kennwort**, **Kennwort bestätigen**, **angegebenen Namen**, **Nachnamen**.
    3.  Klicken Sie auf **aktiv**.
    4.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Alle anderen SmarterU Benutzer Konto Creation Tools können oder APIs bereitgestellt durch SmarterU bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-smarteru-perform-the-following-steps"></a>Zuweisen von Benutzern zu SmarterU führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **SmarterU **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-smarteru-tutorial/IC777330.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-smarteru-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).