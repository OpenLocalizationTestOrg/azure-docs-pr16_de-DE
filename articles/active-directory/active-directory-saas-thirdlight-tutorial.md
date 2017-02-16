<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Thirdlight | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Thirdlight mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Lernprogramm: Azure-Active Directory-Integration in Thirdlight
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Thirdlight anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Thirdlight einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Thirdlight zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Thirdlight (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Thirdlight
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-thirdlight-tutorial/IC805836.png "Szenario")

##<a name="enabling-the-application-integration-for-thirdlight"></a>Aktivieren die Anwendungsintegration für Thirdlight
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Thirdlight aktiviert.

###<a name="to-enable-the-application-integration-for-thirdlight-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Thirdlight aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-thirdlight-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-thirdlight-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-thirdlight-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-thirdlight-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Thirdlight**.

    ![Katalog der Anwendung] (./media/active-directory-saas-thirdlight-tutorial/IC805837.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Thirdlight aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![ThirdLight] (./media/active-directory-saas-thirdlight-tutorial/IC805838.png "ThirdLight")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Thirdlight mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für Thirdlight konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Thirdlight** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-thirdlight-tutorial/IC805839.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Thirdlight auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-thirdlight-tutorial/IC805840.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Thirdlight melden Sie sich In URL** Ihre URL, die von den Benutzern verwendet, um an Ihrer Anwendung Thirdlight melden Sie sich auf (z. B.: "*http://azuresso2.thirdlight.com/*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-thirdlight-tutorial/IC805841.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Thirdlight** zum Herunterladen der Metadaten Ihrer klicken Sie auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-thirdlight-tutorial/IC805842.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Thirdlight als Administrator.

6.  Wechseln Sie zu **Konfiguration \> System Administration**, und klicken Sie dann auf **SAML2**.

    ![Systemadministration] (./media/active-directory-saas-thirdlight-tutorial/IC805843.png "Systemadministration")

7.  Führen Sie im Abschnitt Konfiguration SAML2 die folgenden Schritte aus:

    ![SAML einmaliges Anmelden] (./media/active-directory-saas-thirdlight-tutorial/IC805844.png "SAML einmaliges Anmelden")

    1.  Wählen Sie **SAML2 einmaliges Anmelden aktivieren**.
    2.  Wählen Sie als **Quelle für IdP Metadaten** **Laden IdP Metadaten aus XML**.
    3.  Öffnen Sie die heruntergeladene Metadatendatei, kopieren Sie den Inhalt, und fügen Sie ihn in das Textfeld **IdP Metadaten XML** .
    4.  Klicken Sie auf **Speichern SAML2 Settings**.

8.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-thirdlight-tutorial/IC805845.png "Konfigurieren Sie einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Thirdlight zu ermöglichen, müssen er in Thirdlight bereitgestellt werden.  
Im Falle von Thirdlight ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Thirdlight** an.

2.  Wechseln Sie zur Registerkarte **Benutzer** .

3.  Wählen Sie **Benutzer und Gruppen**.

4.  Klicken Sie auf die Schaltfläche **neue Benutzer hinzufügen** .

5.  Geben Sie **den Benutzernamen, Namen oder Beschreibung, e-Mail-, Auswählen einer Voreinstellung oder der neuen Mitglieder** eines gültigen AAD Kontos gewünschten bereitstellen.

6.  Klicken Sie auf **Erstellen**.

>[AZURE.NOTE] Alle anderen Thirdlight Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Thirdlight bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-thirdlight-perform-the-following-steps"></a>Zuweisen von Benutzern zu Thirdlight führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Thirdlight **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-thirdlight-tutorial/IC805846.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-thirdlight-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).