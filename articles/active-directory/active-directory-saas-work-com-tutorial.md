<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Work.com | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Work.com mit Azure Active Directory einmaliges Anmelden, automatisierte bereitgestellt und mehr aktivieren!." 
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

#<a name="tutorial-azure-active-directory-integration-with-workcom"></a>Lernprogramm: Azure-Active Directory-Integration in Work.com
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Work.com anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Work.com einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, weisen AAD Benutzer, die Sie haben, Work.com Access gehört können einzelne melden Sie sich an die Anwendung zu Ihrer Firmenwebsite Work.com (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Work.com
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-work-com-tutorial/IC794105.png "Szenario")

##<a name="enabling-the-application-integration-for-workcom"></a>Aktivieren die Anwendungsintegration für Work.com
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Work.com aktiviert.

###<a name="to-enable-the-application-integration-for-workcom-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Work.com aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-work-com-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-work-com-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-work-com-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-work-com-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Work.com**.

    ![Katalog der Anwendung] (./media/active-directory-saas-work-com-tutorial/IC794106.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Work.com aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Work.com] (./media/active-directory-saas-work-com-tutorial/IC794107.png "Work.com")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Work.com mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie ein Zertifikat zum Work.com.com hochladen.

>[AZURE.NOTE] Einmaliges Anmelden um zu konfigurieren, müssen Sie einen benutzerdefinierten Domänennamen für die Work.com noch setup. Sie müssen mindestens einen Domänennamen definieren, Testen Ihren Domänennamen und auf die gesamte Organisation bereitstellen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Melden Sie sich bei Ihrem Mandanten Work.com als Administrator an.

2.  Wechseln Sie zu **Einrichten**.

    ![Setup] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

3.  Klicken Sie auf der linken Navigationsbereich im Abschnitt **Verwalten** klicken Sie auf **Domain Management** , um die zugehörigen Abschnitt zu erweitern, und klicken Sie dann auf **Meine Domäne** , um die Seite **Meine Domäne** öffnen. 

    ![Meiner Domäne] (./media/active-directory-saas-work-com-tutorial/IC767825.png "Meiner Domäne")

4.  Um den Besitz Ihrer Domäne richtig installiert wurde, stellen Sie sicher, dass es in "**Schritt 4 für Benutzer bereitgestellt**" ist, und überprüfen Sie Ihre "**Meine Domäne Einstellungen**".

    ![Ihre Benutzer bereitgestellt] (./media/active-directory-saas-work-com-tutorial/IC784377.png "Ihre Benutzer bereitgestellt")

5.  Melden Sie sich in einem anderen Webbrowserfenster Ihrer Azure klassischen-Portal an.

6.  Klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren Single Sign On** , auf der Seite **Work.com **Anwendung Integration.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-work-com-tutorial/IC794109.png "Konfigurieren Sie einmaliges Anmelden")

7.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Work.com auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-work-com-tutorial/IC794110.png "Konfigurieren Sie einmaliges Anmelden")

8.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Work.com melden Sie sich auf URL** die URL, die von den Benutzern verwendet, um melden Sie sich an Ihrer Anwendung Work.com auf (z. B.: " *http://company.my.salesforce.com*"), und klicken Sie dann auf **Weiter**: 

    ![Konfigurieren der App-URL] (./media/active-directory-saas-work-com-tutorial/IC794111.png "Konfigurieren der App-URL")

9.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Work.com** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-work-com-tutorial/IC794112.png "Konfigurieren Sie einmaliges Anmelden")

10. Melden Sie sich bei Ihrem Mandanten Work.com.

11. Wechseln Sie zu **Einrichten**.

    ![Setup] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

12. Erweitern Sie klicken Sie im Menü **Sicherheit Steuerelemente** zu, und klicken Sie dann auf **Einstellungen für einzelne Zeichen**.

    ![Einstellungen für einzelne Zeichen] (./media/active-directory-saas-work-com-tutorial/IC794113.png "Einstellungen für einzelne Zeichen")

13. Klicken Sie auf der Seite **Einstellungen für einzelne anmelden** Dialogfeld führen Sie die folgenden Schritte aus:

    ![SAML aktiviert] (./media/active-directory-saas-work-com-tutorial/IC781026.png "SAML aktiviert")

    1.  Wählen Sie **SAML aktiviert**.
    2.  Klicken Sie auf **neu**.

14. Führen Sie im Abschnitt **SAML einzelnen anmelden Einstellungen** die folgenden Schritte aus:

    ![SAML einzelne anmelden Einstellung] (./media/active-directory-saas-work-com-tutorial/IC794114.png "SAML einzelne anmelden Einstellung")

    1.  Geben Sie in das Textfeld **Name** einen Namen für die Konfiguration aus.  

        >[AZURE.NOTE] Einen Wert für **Name** bereitstellen automatisch im Textfeld **API den** Wert zu füllen.

    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Work.com** kopieren Sie den Wert des **Herausgebers URL** , und fügen Sie ihn in das Textfeld **Herausgeber** .
    3.  Um die heruntergeladene Zertifikat hochladen möchten, klicken Sie auf **Durchsuchen**.
    4.  Geben Sie in das Textfeld **Einheiten Id** **https://salesforce-work.com**aus.
    5.  Wählen Sie als **Typ der SAML-Identität** **Assertion enthält die Föderation-ID aus dem Benutzer-Objekt**aus.
    6.  Wählen Sie als **Speicherort der SAML-Identität** **Identität befindet sich im NameIdentfier Element der Betreff-Anweisung**ein.
    7.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Identität Anbieter Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Work.com** .
    8.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Work.com** kopieren Sie des Werts **Remote Abmeldung-URL** , und klicken Sie dann in das Textfeld **Identität Anbieter Abmelden URL** einzufügen.
    9.  Wählen Sie als **Service Provider initiiert Bindung anfordern** **HTTP-Beitrag**ein.
    10. Klicken Sie auf **Speichern**.

15. Ihre klassischen Work.com-Portal, klicken Sie im linken Navigationsbereich auf klicken Sie auf **Domain Management** , um die zugehörigen Abschnitt zu erweitern, und klicken Sie dann auf **Meine Domäne** , um die Seite **Meine Domäne** öffnen. 

    ![Meiner Domäne] (./media/active-directory-saas-work-com-tutorial/IC794115.png "Meiner Domäne")

16. Klicken Sie auf der Seite **My Domain** im Abschnitt **Login Seite Branding** auf **Bearbeiten**.

    ![Branding-Anmeldeseite] (./media/active-directory-saas-work-com-tutorial/IC767826.png "Branding-Anmeldeseite")

17. Klicken Sie auf der Seite **Login Seite Branding** im Abschnitt **Authentifizierungsdienst** ist der Name der **SAML SSO-Einstellungen** angezeigt. Wählen Sie ihn aus, und klicken Sie dann auf **Speichern**.

    ![Branding-Anmeldeseite] (./media/active-directory-saas-work-com-tutorial/IC784366.png "Branding-Anmeldeseite")

18. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-work-com-tutorial/IC794116.png "Konfigurieren Sie einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Für Azure Active Directory-Benutzer anmelden können müssen er Work.com bereitgestellt werden.  
Im Falle von Work.com ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator klicken Sie auf der Website Ihres Unternehmens Work.com an.

2.  Wechseln Sie zu **Einrichten**.

    ![Setup] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

3.  Wechseln Sie zu **Verwalten von Benutzern \> Benutzer**.

    ![Verwalten von Benutzern] (./media/active-directory-saas-work-com-tutorial/IC784369.png "Verwalten von Benutzern")

4.  Klicken Sie auf **Neuer Benutzer**.

    ![Alle Benutzer] (./media/active-directory-saas-work-com-tutorial/IC794117.png "Alle Benutzer")

5.  Führen Sie im Abschnitt Benutzer die folgenden Schritte aus:

    ![Benutzer bearbeiten] (./media/active-directory-saas-work-com-tutorial/IC794118.png "Benutzer bearbeiten")

    1.  Geben Sie **Nachname**, **Alias**, **E-Mail**, **Benutzername** und **Pseudonym** Attribute eines gültigen Azure Active Directory-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Wählen Sie die **Rolle**, **Benutzerlizenz** und **Profil**.
    3.  Klicken Sie auf **Speichern**.  

        >[AZURE.NOTE] Der Inhaber des Azure Active Directory-Konto erhalten eine e-Mail-Nachricht, einschließlich einen Link, um das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen Work.com Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Work.com bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-workcom-perform-the-following-steps"></a>Zuweisen von Benutzern zu Work.com führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite Work.com Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-work-com-tutorial/IC794119.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-work-com-tutorial/IC767830.png "Ja")
  
Sie sollten jetzt 10 Minuten warten, und stellen Sie sicher, dass das Konto mit Work.com.com synchronisiert wurde.
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).