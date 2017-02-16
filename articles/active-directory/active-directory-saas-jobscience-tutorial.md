<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Jobscience | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Jobscience mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Lernprogramm: Azure-Active Directory-Integration in Jobscience
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Jobscience anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Abonnement Jobscience einmaliges Anmelden aktiviert
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Jobscience zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Jobscience (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Jobscience
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-jobscience-tutorial/IC784341.png "Szenario")
##<a name="enabling-the-application-integration-for-jobscience"></a>Aktivieren die Anwendungsintegration für Jobscience
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Jobscience aktiviert.

###<a name="to-enable-the-application-integration-for-jobscience-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Jobscience aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-jobscience-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-jobscience-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-jobscience-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-jobscience-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Jobscience**.

    ![Katalog der Anwendung] (./media/active-directory-saas-jobscience-tutorial/IC784342.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Jobscience aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Jobscience] (./media/active-directory-saas-jobscience-tutorial/IC784357.png "Jobscience")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Jobscience mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für Jobscience konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite Jobscience an.

2.  Wechseln Sie zu **Einrichten**.

    ![Setup] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")

3.  Klicken Sie auf der linken Navigationsbereich im Abschnitt **Verwalten** klicken Sie auf **Domain Management** , um die zugehörigen Abschnitt zu erweitern, und klicken Sie dann auf **Meine Domäne** , um die Seite **Meine Domäne** öffnen. 

    ![Meiner Domäne] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Meiner Domäne")

4.  Um den Besitz Ihrer Domäne richtig installiert wurde, stellen Sie sicher, dass es in "**Schritt 4 für Benutzer bereitgestellt**" ist, und überprüfen Sie Ihre "**Meine Domäne Einstellungen**".

    ![Ihre Benutzer bereitgestellt] (./media/active-directory-saas-jobscience-tutorial/IC784377.png "Ihre Benutzer bereitgestellt")

5.  Melden Sie sich in einem anderen Webbrowserfenster Ihrer Azure klassischen-Portal an.

6.  Klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren Single Sign On** , auf der Seite **Jobscience** Anwendung Integration.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-jobscience-tutorial/IC784360.png "Konfigurieren Sie einmaliges Anmelden")

7.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Jobscience auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-jobscience-tutorial/IC784361.png "Konfigurieren Sie einmaliges Anmelden")

8.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Jobscience melden Sie sich In URL** ein Ihre URL dem folgenden Muster "*http://company.my.salesforce.com*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-jobscience-tutorial/IC784362.png "Konfigurieren der App-URL")

9.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Jobscience** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-jobscience-tutorial/IC784363.png "Konfigurieren Sie einmaliges Anmelden")

10. Klicken Sie auf die Jobscience Firmenwebsite klicken Sie auf **Sicherheit Steuerelemente**, und klicken Sie dann auf **Einstellungen für einzelne Zeichen**.

    ![Sicherheit-Steuerelemente] (./media/active-directory-saas-jobscience-tutorial/IC784364.png "Sicherheit-Steuerelemente")

11. Führen Sie im Abschnitt **Einstellungen für einzelne Zeichen** die folgenden Schritte aus:

    ![Einstellungen für einzelne Zeichen] (./media/active-directory-saas-jobscience-tutorial/IC781026.png "Einstellungen für einzelne Zeichen")

    1.  Wählen Sie **SAML aktiviert**.
    2.  Klicken Sie auf **neu**.

12. Klicken Sie im Dialogfeld **SAML einzelnen anmelden Einstellung zu bearbeiten** führen Sie die folgenden Schritte aus:

    ![SAML einzelne anmelden Einstellung] (./media/active-directory-saas-jobscience-tutorial/IC784365.png "SAML einzelne anmelden Einstellung")

    1.  Geben Sie in das Textfeld **Name** einen Namen für die Konfiguration aus.
    2.  Im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden bei Jobscience** kopieren Sie den Wert des **Herausgebers URL** , und fügen Sie ihn in das Textfeld **Herausgeber**
    3.  Geben Sie in das Textfeld **Einheiten Id** **https://salesforce-jobscience.com**
    4.  Klicken Sie auf **Durchsuchen** , um Ihr Zertifikat Azure AD-hochladen.
    5.  Wählen Sie als **Typ der SAML-Identität** **Assertion enthält die Föderation-ID aus dem Benutzer-Objekt**aus.
    6.  Wählen Sie als **Speicherort der SAML-Identität** **Identität befindet sich im NameIdentfier Element der Betreff-Anweisung**ein.
    7.  Im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden bei Jobscience** kopieren Sie des Werts **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Identität Anbieter Anmelde-URL**
    8.  Im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden bei Jobscience** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **Identität Anbieter Abmelden URL**
    9.  Klicken Sie auf **Speichern**.

13. Klicken Sie auf der linken Navigationsbereich im Abschnitt **Verwalten** klicken Sie auf **Domain Management** , um die zugehörigen Abschnitt zu erweitern, und klicken Sie dann auf **Meine Domäne** , um die Seite **Meine Domäne** öffnen. 

    ![Meiner Domäne] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Meiner Domäne")

14. Klicken Sie auf der Seite **My Domain** im Abschnitt **Login Seite Branding** auf **Bearbeiten**.

    ![Branding-Anmeldeseite] (./media/active-directory-saas-jobscience-tutorial/IC767826.png "Branding-Anmeldeseite")

15. Klicken Sie auf der Seite **Login Seite Branding** im Abschnitt **Authentifizierungsdienst** ist der Name der **SAML SSO-Einstellungen** angezeigt. Wählen Sie ihn aus, und klicken Sie dann auf **Speichern**.

    ![Branding-Anmeldeseite] (./media/active-directory-saas-jobscience-tutorial/IC784366.png "Branding-Anmeldeseite")

16. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-jobscience-tutorial/IC784367.png "Konfigurieren Sie einmaliges Anmelden")
  
Klicken Sie zum Abrufen des SP-initiiert für einmaliges Anmelden Anmelde-URL **Single Sign On Einstellungen** im Abschnitt **Sicherheit Steuerelemente** im Menü auf.

![Sicherheit-Steuerelemente] (./media/active-directory-saas-jobscience-tutorial/IC784368.png "Sicherheit-Steuerelemente")
  
Klicken Sie auf das SSO-Profil, das Sie in den vorstehenden Schritt erstellt haben.  
Diese Seite zeigt die Single Sign auf URL für Ihr Unternehmen (z. B. *https://companyname.my.salesforce.com?so=companyid*).
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Jobscience zu ermöglichen, müssen er in Jobscience bereitgestellt werden.  
Im Falle von Jobscience ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Jobscience** an.

2.  Wechseln Sie zu Setup

    ![Setup] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")

3.  Wechseln Sie zu **Verwalten von Benutzern \> Benutzer**.

    ![Benutzer] (./media/active-directory-saas-jobscience-tutorial/IC784369.png "Benutzer")

4.  Klicken Sie auf **Neuer Benutzer**.

    ![Alle Benutzer] (./media/active-directory-saas-jobscience-tutorial/IC784370.png "Alle Benutzer")

5.  Klicken Sie im Dialogfeld **Benutzer bearbeiten** führen Sie die folgenden Schritte aus:

    ![Benutzer bearbeiten] (./media/active-directory-saas-jobscience-tutorial/IC784371.png "Benutzer bearbeiten")

    1.  Geben Sie Vorname, Nachname, Alias, e-Mails, Namen und einen Benutzereigenschaften des Benutzers Azure AD-, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

    >[AZURE.NOTE] Der Inhaber des Azure AD-Konto erhalten eine e-Mail-Nachricht, die enthält einen Link, um das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen Jobscience Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Jobscience bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-jobscience-perform-the-following-steps"></a>Zuweisen von Benutzern zu Jobscience führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Jobscience **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-jobscience-tutorial/IC784372.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-jobscience-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).