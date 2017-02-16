<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Panorama9 | Microsoft Azure" 
    description="Erfahren Sie, wie Panorama9 mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panorama9"></a>Lernprogramm: Azure-Active Directory-Integration in Panorama9
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Panorama9 anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Panorama9 einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Panorama9 zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Panorama9 (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Panorama9
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-panorama9-tutorial/IC790016.png "Szenario")
##<a name="enabling-the-application-integration-for-panorama9"></a>Aktivieren die Anwendungsintegration für Panorama9
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Panorama9 aktiviert.

###<a name="to-enable-the-application-integration-for-panorama9-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Panorama9 aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-panorama9-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-panorama9-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-panorama9-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-panorama9-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Panorama9**.

    ![Katalog der Anwendung] (./media/active-directory-saas-panorama9-tutorial/IC790017.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Panorama9 aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Panorama9] (./media/active-directory-saas-panorama9-tutorial/IC790018.png "Panorama9")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Panorama9 mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für Panorama9 konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Panorama9** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-panorama9-tutorial/IC790019.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Panorama9 auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-panorama9-tutorial/IC790020.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Panorama9 melden Sie sich auf die URL** den URL, der von Ihren Benutzern die Anmeldung bei Panorama9 verwendet (z. B.: "*https://dashboard.panorama9.com/saml/access/3262*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-panorama9-tutorial/IC790021.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Panorama9** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer speichern.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-panorama9-tutorial/IC790022.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Panorama9 als Administrator.

6.  Klicken Sie in der Symbolleiste oben auf **Verwalten**, und klicken Sie dann auf **Erweiterungen**.

    ![Extensions] (./media/active-directory-saas-panorama9-tutorial/IC790023.png "Extensions")

7.  Klicken Sie im Dialogfeld **Erweiterungen** klicken Sie auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden] (./media/active-directory-saas-panorama9-tutorial/IC790024.png "Einmaliges Anmelden")

8.  Führen Sie im Abschnitt **Einstellungen** die folgenden Schritte aus:

    ![Einstellungen] (./media/active-directory-saas-panorama9-tutorial/IC790025.png "Einstellungen")

    1.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Panorama9** kopieren Sie den Wert für die **Einzelnen anmelden Dienst-URL** , und fügen Sie ihn in das Textfeld **Identität Provider-URL** .
    2.  Kopieren Sie den **Fingerabdruck** Wert aus dem exportierte Zertifikat, und fügen Sie ihn in das Textfeld **Zertifikat Fingerabdruck** .  

        >[AZURE.TIP]Weitere Informationen hierzu finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

    3.  Klicken Sie auf **Speichern**.

9.  Klicken Sie im Portal Azure AD-klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-panorama9-tutorial/IC790026.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Panorama9 zu ermöglichen, müssen er in Panorama9 bereitgestellt werden.  
Im Falle von Panorama9 ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Panorama9** an.

2.  Klicken Sie auf **Verwalten**, und klicken Sie dann auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer] (./media/active-directory-saas-panorama9-tutorial/IC790027.png "Benutzer")

3.  Klicken Sie auf **+**.

4.  Führen Sie im Abschnitt Benutzer die folgenden Schritte aus:

    ![Benutzer] (./media/active-directory-saas-panorama9-tutorial/IC790028.png "Benutzer")

    1.  Geben Sie in das Textfeld **-e-Mail** die e-Mail-Adresse eines gültigen Azure Active Directory-Benutzers, die Sie bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE]Alle anderen Panorama9 Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Panorama9 bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-panorama9-perform-the-following-steps"></a>Zuweisen von Benutzern zu Panorama9 führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Panorama9** Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-panorama9-tutorial/IC790029.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-panorama9-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).