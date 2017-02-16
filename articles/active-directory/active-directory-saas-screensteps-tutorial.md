<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in ScreenSteps | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von ScreenSteps mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Lernprogramm: Azure-Active Directory-Integration in ScreenSteps
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und ScreenSteps anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten ScreenSteps
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie ScreenSteps zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite ScreenSteps (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für ScreenSteps
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-screensteps-tutorial/IC778516.png "Szenario")
##<a name="enabling-the-application-integration-for-screensteps"></a>Aktivieren die Anwendungsintegration für ScreenSteps
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für ScreenSteps aktiviert.

###<a name="to-enable-the-application-integration-for-screensteps-perform-the-following-steps"></a>Wenn die Anwendungsintegration für ScreenSteps aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-screensteps-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-screensteps-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-screensteps-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-screensteps-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **ScreenSteps**.

    ![Katalog der Anwendung] (./media/active-directory-saas-screensteps-tutorial/IC778517.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **ScreenSteps aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![ScreenSteps] (./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu ScreenSteps mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **ScreenSteps** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-screensteps-tutorial/IC778519.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der ScreenSteps auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-screensteps-tutorial/IC778520.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **ScreenSteps melden Sie sich In die URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. ScreenSteps.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der app-URL] (./media/active-directory-saas-screensteps-tutorial/IC778521.png "Konfigurieren der app-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei ScreenSteps** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-screensteps-tutorial/IC778522.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens ScreenSteps als Administrator.

6.  Klicken Sie auf **Konto Management**.

    ![Verwaltung von Konten] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Verwaltung von Konten")

7.  Klicken Sie auf **Remote-Authentifizierung**.

    ![Remote-Authentifizierung] (./media/active-directory-saas-screensteps-tutorial/IC778524.png "Remote-Authentifizierung")

8.  Klicken Sie auf **Erstellen Authentifizierung Endpunkt**.

    ![Remote-Authentifizierung] (./media/active-directory-saas-screensteps-tutorial/IC778525.png "Remote-Authentifizierung")

9.  Führen Sie im Abschnitt **Erstellen Sie einen Endpunkt Authentifizierung** die folgenden Schritte aus:

    ![Erstellen Sie einen Endpunkt Authentifizierung] (./media/active-directory-saas-screensteps-tutorial/IC778526.png "Erstellen Sie einen Endpunkt Authentifizierung")

    1.  Geben Sie im Textfeld **Titel** einen Titel ein.
    2.  Wählen Sie in der Liste **Modus** **SAML**aus.
    3.  Klicken Sie auf **Erstellen**.

10. Führen Sie im Abschnitt **Remote Authentifizierung Endpunkt** die folgenden Schritte aus:

    ![Remote-Authentifizierung Endpunkt] (./media/active-directory-saas-screensteps-tutorial/IC778527.png "Remote-Authentifizierung Endpunkt")

    1.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Remote Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei ScreenSteps** .
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei ScreenSteps** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **URL Abmelden** .
    3.  Klicken Sie auf **Wählen Sie eine Datei**, und Laden Sie das heruntergeladene Zertifikat.
    4.  Klicken Sie auf **Aktualisieren**.

11. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-screensteps-tutorial/IC778542.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei **ScreenSteps**zu ermöglichen, müssen er in **ScreenSteps**bereitgestellt werden.  
Im Falle von **ScreenSteps**ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-account-to-screensteps-perform-the-following-steps"></a>Um ein Benutzerkonto zu ScreenSteps bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **ScreenSteps** .

2.  Klicken Sie auf **Konto Management**.

    ![Verwaltung von Konten] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Verwaltung von Konten")

3.  Klicken Sie auf **Benutzer**.

    ![Benutzer] (./media/active-directory-saas-screensteps-tutorial/IC778544.png "Benutzer")

4.  Klicken Sie auf **Benutzer erstellen**.

    ![Alle Benutzer] (./media/active-directory-saas-screensteps-tutorial/IC778545.png "Alle Benutzer")

5.  Wählen Sie aus der Liste **Benutzerrolle** eine Rolle für Ihre Benutzer aus.

6.  Geben Sie im Abschnitt Benutzerrolle**"Vorname**, **Nachname**, **E-Mail**, **Anmeldung**, **Kennwort** und **Kennwort zur Bestätigung"**eines gültigen AAD-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.

    ![Neuer Benutzer] (./media/active-directory-saas-screensteps-tutorial/IC778546.png "Neuer Benutzer")

7.  Im Abschnitt Gruppen **Authentifizierung Gruppe (SAML)**wählen Sie aus, und klicken Sie dann auf **Benutzer erstellen**.

    ![Gruppen] (./media/active-directory-saas-screensteps-tutorial/IC778547.png "Gruppen")

>[AZURE.NOTE]Alle anderen ScreenSteps Benutzer Konto Creation Tools können oder APIs bereitgestellt durch ScreenSteps bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-screensteps-perform-the-following-steps"></a>Zuweisen von Benutzern zu ScreenSteps führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **ScreenSteps **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-screensteps-tutorial/IC773094.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-screensteps-tutorial/IC778548.png "Zuweisen von Benutzern")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).