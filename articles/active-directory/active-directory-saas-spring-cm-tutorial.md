<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Frühling CM | Microsoft Azure" 
    description="Erfahren Sie, wie Frühling CM mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-spring-cm"></a>Lernprogramm: Azure-Active Directory-Integration in Frühling CM
  
Ziel dieses Lernprogramms ist zum Einrichten einmaliges Anmelden zwischen Azure Active Directory und SpringCM veranschaulichen.
  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine SpringCM einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, ist die Azure Active Directory-Benutzer, die Sie SpringCM zugewiesen haben einmaliges Anmelden mithilfe des Access-Bedienfelds AAD möglich.

1.  Aktivieren die Anwendungsintegration für SpringCM
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-spring-cm-tutorial/IC797044.png "Szenario")

##<a name="enabling-the-application-integration-for-springcm"></a>Aktivieren die Anwendungsintegration für SpringCM
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für SpringCM aktiviert.

###<a name="to-enable-the-application-integration-for-springcm-perform-the-following-steps"></a>Wenn die Anwendungsintegration für SpringCM aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-spring-cm-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-spring-cm-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-spring-cm-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-spring-cm-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **SpringCM**.

    ![Katalog der Anwendung] (./media/active-directory-saas-spring-cm-tutorial/IC797045.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **SpringCM aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![SpringCM] (./media/active-directory-saas-spring-cm-tutorial/IC797046.png "SpringCM")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
In diesem Abschnitt werden, wie Benutzer authentifizieren SpringCM mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **SpringCM** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-spring-cm-tutorial/IC797047.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der SpringCM auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-spring-cm-tutorial/IC797048.png "Konfigurieren einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **SpringCM melden Sie sich auf URL** Geben Sie die URL, die von den Benutzern verwendet, um melden Sie sich an Ihrer Anwendung SpringCM auf, und klicken Sie dann auf **Weiter**. 

    Die app-URL ist die URL Ihrer SpringCM Mandanten (z. B.: *https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=16826*):

    ![Konfigurieren der App-URL] (./media/active-directory-saas-spring-cm-tutorial/IC797049.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei SpringCM** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.

    ![Konfigurieren von einzigen Anmeldung] (./media/active-directory-saas-spring-cm-tutorial/IC797050.png "Konfigurieren von einzigen Anmeldung")

5.  In einem anderen Webbrowserfenster melden Sie sich auf der Website Ihres Unternehmens **SpringCM** als Administrator.

6.  Klicken Sie im Menü oben klicken Sie auf **GEHE zu**, klicken Sie auf **Einstellungen**, und klicken Sie dann im Abschnitt **Kontoeinstellungen** auf **SAML-SSO**.

    ![SAML-SSO] (./media/active-directory-saas-spring-cm-tutorial/IC797051.png "SAML-SSO")

7.  Führen Sie im Abschnitt Konfiguration des Anbieters Identität die folgenden Schritte aus:

    ![Die Konfiguration des Anbieters Identität] (./media/active-directory-saas-spring-cm-tutorial/IC797052.png "Die Konfiguration des Anbieters Identität")

    1.  Wenn Ihr heruntergeladene Azure Active Directory-Zertifikat hochladen möchten, klicken Sie auf **Zertifikat zum Auswählen eines Herausgebers** oder **Herausgeber-Zertifikat ändern**.
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei SpringCM** kopieren Sie den Wert des **Herausgebers URL** , und fügen Sie ihn in das Textfeld **Herausgeber** .
    3.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei SpringCM** kopieren Sie des Werts **Singel anmelden Dienst-URL** , und fügen Sie ihn in das Textfeld **Service Provider (SP) initiiert Endpunkt** .
    4.  Als **SAML aktiviert**die Option **Aktivieren**aus.
    5.  Klicken Sie auf **Speichern**.

8.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren von einzigen Anmeldung] (./media/active-directory-saas-spring-cm-tutorial/IC797053.png "Konfigurieren von einzigen Anmeldung")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure Active Directory-Benutzern zur Anmeldung bei SpringCM zu ermöglichen, müssen er in SpringCM bereitgestellt werden.  
Im Falle von SpringCM ist die Bereitstellung eine manuelle Aufgabe.

>[AZURE.NOTE] Weitere Informationen hierzu finden Sie unter [Erstellen und Bearbeiten eines Benutzers SpringCM](http://knowledge.springcm.com/create-and-edit-a-springcm-user)

###<a name="to-provision-a-user-account-to-springcm-perform-the-following-steps"></a>Um ein Benutzerkonto zu SpringCM bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **SpringCM** an.

2.  Klicken Sie auf **Gehe zu**, und klicken Sie dann auf **Adressbuch**.

    ![Erstellen Sie Benutzer] (./media/active-directory-saas-spring-cm-tutorial/IC797054.png "Erstellen Sie Benutzer")

3.  Klicken Sie auf **Benutzer erstellen**.

4.  Wählen Sie eine **Benutzerrolle**aus.

5.  Wählen Sie die **Aktivierung E-Mail senden**aus.

6.  Geben Sie die Vorname, Nachname und e-Mail-Adresse des ein gültiges Azure Active Directory-Benutzerkonto, die, das Sie in die verknüpfte Textfelder bereitstellen möchten.

7.  Fügen Sie den Benutzer zu einer **Sicherheitsgruppe**an.

8.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Alle anderen SpringCM Benutzer Konto Creation Tools können oder APIs bereitgestellt durch SpringCM bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-springcm-perform-the-following-steps"></a>Zuweisen von Benutzern zu SpringCM führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **SpringCM** Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-spring-cm-tutorial/IC797055.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-spring-cm-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).




