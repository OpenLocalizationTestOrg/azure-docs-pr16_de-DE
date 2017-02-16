<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in FM: Systeme | Microsoft Azure" 
    description="Informationen zum Verwenden von FM: Systeme mit Azure Active Directory aktivieren einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-fm-systems"></a>Lernprogramm: Azure-Active Directory-Integration in FM: Betriebssysteme
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und FM:Systems anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine FM:Systems einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie FM:Systems zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite FM:Systems (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für FM:Systems
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-fm-systems-tutorial/IC795899.png "Szenario")
##<a name="enabling-the-application-integration-for-fmsystems"></a>Aktivieren die Anwendungsintegration für FM:Systems
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für FM:Systems aktiviert.

###<a name="to-enable-the-application-integration-for-fmsystems-perform-the-following-steps"></a>Wenn die Anwendungsintegration für FM:Systems aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-fm-systems-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-fm-systems-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-fm-systems-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-fm-systems-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **FM:Systems**.

    ![Katalog der Anwendung] (./media/active-directory-saas-fm-systems-tutorial/IC795900.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **FM:Systems aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![FM: Systeme] (./media/active-directory-saas-fm-systems-tutorial/IC800213.png "FM: Systeme")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren FM:Systems mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **FM:Systems** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-fm-systems-tutorial/IC790810.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der FM:Systems auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-fm-systems-tutorial/IC795901.png "Konfigurieren Sie einmaliges Anmelden")

3.  Führen Sie auf der Seite **Konfigurieren der App-URL** die folgenden Schritte aus:

    ![Konfigurieren der App-URL] (./media/active-directory-saas-fm-systems-tutorial/IC795902.png "Konfigurieren der App-URL")

    1.  Geben Sie in das Textfeld **FM:Systems melden Sie sich auf die URL** Ihrer FM:Systems **Antwort-URL** (z. B.: *https://dpr.fmshosted.com/fminteract/ConsumerService2.aspx*).  

        >[AZURE.WARNING] Sie können diesen Wert aus Ihrer FM abrufen: Systeme Supportteam.

    2.  Klicken Sie auf **Weiter**

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei FM:Systems** zum Herunterladen der Metadaten Ihrer klicken Sie auf **Herunterladen von Metadaten**, und klicken Sie dann speichern Sie die Metadaten auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-fm-systems-tutorial/IC795903.png "Konfigurieren Sie einmaliges Anmelden")

5.  Senden Sie die Metadatendatei heruntergeladene zu Ihrem FM: Systeme Supportteam.

    >[AZURE.NOTE] Ihre FM: Systeme Supportteam muss die tatsächliche SSO-Konfiguration vornehmen.
Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.

6.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-fm-systems-tutorial/IC795904.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzer zur Anmeldung bei FM:Systems aktivieren zu können, müssen er in FM:Systems bereitgestellt werden.  
Im Falle von FM:Systems ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  In einem Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens FM:Systems als Administrator.

2.  Wechseln Sie zu **System Administration \> Sicherheit verwalten \> Benutzer \> Benutzerliste**.

    ![Systemadministration] (./media/active-directory-saas-fm-systems-tutorial/IC795905.png "Systemadministration")

3.  Klicken Sie auf **neuen Benutzer erstellen**.

    ![Erstellen neuen Benutzer] (./media/active-directory-saas-fm-systems-tutorial/IC795906.png "Erstellen neuen Benutzer")

4.  Führen Sie im Abschnitt **Benutzer erstellen** die folgenden Schritte aus:

    ![Erstellen Sie Benutzer] (./media/active-directory-saas-fm-systems-tutorial/IC795907.png "Erstellen Sie Benutzer")

    1.  Geben Sie den Benutzernamen, das Kennwort und seine Bestätigung, die e-Mail-Adresse und die Mitarbeiter-ID eines gültigen Azure Active Directory-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Weiter**.

>[AZURE.NOTE] Alle anderen FM:Systems Benutzer Konto Creation Tools können oder APIs von FM:Systems zur Bereitstellung von Konten AAD Benutzer bereitgestellt.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-fmsystems-perform-the-following-steps"></a>Zuweisen von Benutzern zu FM:Systems führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **FM:Systems **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-fm-systems-tutorial/IC795908.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-fm-systems-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).