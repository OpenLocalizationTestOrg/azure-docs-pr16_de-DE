<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Envoy | Microsoft Azure" 
    description="Erfahren Sie, wie Envoy mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-envoy"></a>Lernprogramm: Azure-Active Directory-Integration in Envoy
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Envoy anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Envoy
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Envoy zugewiesen haben können einzelne melden Sie sich bei der Anwendung an Ihre Envoy Firmenwebsite (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Envoy
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-envoy-tutorial/IC776759.png "Szenario")
##<a name="enabling-the-application-integration-for-envoy"></a>Aktivieren die Anwendungsintegration für Envoy
  
So aktivieren Sie die Anwendungsintegration für Envoy Gliedern ist das Ziel in diesem Abschnitt.

###<a name="to-enable-the-application-integration-for-envoy-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Envoy aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-envoy-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-envoy-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-envoy-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-envoy-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Envoy**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-envoy-tutorial/IC776760.png "Katalog der Anwendung")

7.  Im Ergebnisbereich **Envoy**wählen Sie aus, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Envoy] (./media/active-directory-saas-envoy-tutorial/IC776777.png "Envoy")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Envoy mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für Envoy konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Envoy** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-envoy-tutorial/IC776778.png "Einmaliges Anmelden aktivieren")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer Envoy bei, klicken Sie auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-envoy-tutorial/IC776779.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Envoy melden Sie sich In die URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. Envoy.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der app-URL] (./media/active-directory-saas-envoy-tutorial/IC776780.png "Konfigurieren der app-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Envoy** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und klicken Sie dann speichern Zertifikatsdatei lokal als **c:\\Envoy.cer**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-envoy-tutorial/IC776781.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Envoy als Administrator.

6.  Klicken Sie oben auf der Symbolleiste auf **Einstellungen**.

    ![Envoy] (./media/active-directory-saas-envoy-tutorial/IC776782.png "Envoy")

7.  Klicken Sie auf die **Firma**.

    ![Unternehmen] (./media/active-directory-saas-envoy-tutorial/IC776783.png "Unternehmen")

8.  Klicken Sie auf **SAML**.

    ![SAML] (./media/active-directory-saas-envoy-tutorial/IC776784.png "SAML")

9.  Führen Sie im Konfigurationsabschnitt **SAML-Authentifizierung** die folgenden Schritte aus:

    ![SAML-Authentifizierung] (./media/active-directory-saas-envoy-tutorial/IC776785.png "SAML-Authentifizierung")

    >[AZURE.NOTE] Der Wert für die ID HQ wird automatisch durch die Anwendung generiert.

    1.  Kopieren Sie den **Fingerabdruck** Wert aus dem exportierte Zertifikat, und fügen Sie ihn in das Textfeld **Fingerabdruck** .  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Envoy** kopieren Sie des Werts **SAML SSO-URL** , und fügen Sie ihn in das Textfeld **Identität Provider HTTP SAML-URL** .
    3.  Klicken Sie auf **Änderungen speichern**.

10. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-envoy-tutorial/IC776786.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Es gibt keine Aktionselement für Ihre Benutzer zu Envoy provisioning konfigurieren.  
Wenn ein zugewiesene Benutzer versucht, die bei Verwendung der Access-Systemsteuerung Envoy anzumelden, überprüft Envoy an, ob der Benutzer vorhanden ist.  
Wenn es noch kein Benutzerkonto verfügbar ist, wird es von Envoy automatisch erstellt.
##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-envoy-perform-the-following-steps"></a>Zuweisen von Benutzern zu Envoy führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Envoy **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-envoy-tutorial/IC776787.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-envoy-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).