<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in AppDynamics | Microsoft Azure" 
    description="Erfahren Sie, wie AppDynamics mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-appdynamics"></a>Lernprogramm: Azure-Active Directory-Integration in AppDynamics

Das Ziel dieses Lernprogramms ist die Integration von Azure und AppDynamics anzeigen. Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine AppDynamics einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie AppDynamics zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite AppDynamics (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für AppDynamics
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-appdynamics-tutorial/IC790209.png "Szenario")
##<a name="enabling-the-application-integration-for-appdynamics"></a>Aktivieren die Anwendungsintegration für AppDynamics

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für AppDynamics aktiviert.

###<a name="to-enable-the-application-integration-for-appdynamics-perform-the-following-steps"></a>Wenn die Anwendungsintegration für AppDynamics aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-appdynamics-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-appdynamics-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-appdynamics-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-appdynamics-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **AppDynamics**.

    ![Katalog der Anwendung] (./media/active-directory-saas-appdynamics-tutorial/IC790210.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **AppDynamics aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![AppDynamics] (./media/active-directory-saas-appdynamics-tutorial/IC790211.png "AppDynamics")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu AppDynamics mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **AppDynamics** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren von einzigen Anmeldung] (./media/active-directory-saas-appdynamics-tutorial/IC790212.png "Konfigurieren von einzigen Anmeldung")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der AppDynamics auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von einzigen Anmeldung] (./media/active-directory-saas-appdynamics-tutorial/IC790213.png "Konfigurieren von einzigen Anmeldung")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **AppDynamics melden Sie sich auf URL** Geben Sie Ihre URL verwendet, die für Ihre Benutzer melden Sie sich für den Zugriff auf AppDynamics ("*https://companyname.saas.appdynamics.com*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-appdynamics-tutorial/IC790214.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei AppDynamics** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren von einzigen Anmeldung] (./media/active-directory-saas-appdynamics-tutorial/IC790215.png "Konfigurieren von einzigen Anmeldung")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens AppDynamics als Administrator.

6.  Klicken Sie in der Symbolleiste oben auf **Einstellungen**, und klicken Sie dann auf **Verwaltung**.

    ![Verwaltung] (./media/active-directory-saas-appdynamics-tutorial/IC790216.png "Verwaltung")

7.  Klicken Sie auf der Registerkarte **Authentifizierungsanbieter** .

    ![Authentifizierungsanbieter] (./media/active-directory-saas-appdynamics-tutorial/IC790224.png "Authentifizierungsanbieter")

8.  Führen Sie im Abschnitt **Authentifizierungsanbieter** die folgenden Schritte aus:

    ![SAML-Konfiguration] (./media/active-directory-saas-appdynamics-tutorial/IC790225.png "SAML-Konfiguration")

    1.  Wählen Sie unter **Authentifizierungsanbieter** **SAML**aus.
    2.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei AppDynamics** .
    3.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei AppDynamics** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **URL Abmelden** .
    4.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    5.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **Zertifikat**
    6.  Klicken Sie auf **Speichern**.
        ![Speichern] (./media/active-directory-saas-appdynamics-tutorial/IC777673.png "Speichern")

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren von einzigen Anmeldung] (./media/active-directory-saas-appdynamics-tutorial/IC790226.png "Konfigurieren von einzigen Anmeldung")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzer zur Anmeldung bei AppDynamics aktivieren zu können, müssen er in AppDynamics bereitgestellt werden.  
Im Falle von AppDynamics ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite AppDynamics.

2.  Wechseln Sie zu **Benutzer**, und klicken Sie dann auf **+** So öffnen Sie das Dialogfeld **Benutzer erstellen** .

    ![Benutzer] (./media/active-directory-saas-appdynamics-tutorial/IC790229.png "Benutzer")

3.  Führen Sie im Abschnitt **Benutzer erstellen** die folgenden Schritte aus:

    ![Erstellen Sie Benutzer] (./media/active-directory-saas-appdynamics-tutorial/IC790230.png "Erstellen Sie Benutzer")

    1.  Geben Sie den **Benutzernamen**, **Namen**, **E-Mails**, **Neues Kennwort**, **Neues Kennwort wiederholen** eines gültigen AAD ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Alle anderen AppDynamics Benutzer Konto Creation Tools können oder APIs von AppDynamics zur Bereitstellung von Azure AD-Benutzerkonten bereitgestellt.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-appdynamics-perform-the-following-steps"></a>Zuweisen von Benutzern zu AppDynamics führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **AppDynamics **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-appdynamics-tutorial/IC790231.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-appdynamics-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
