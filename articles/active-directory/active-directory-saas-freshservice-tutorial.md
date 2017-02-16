<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in FreshService | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von FreshService mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Lernprogramm: Azure-Active Directory-Integration in FreshService
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und FreshService anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Eine FreshService einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie FreshService zugewiesen haben einzelnen melden Sie sich bei der Anwendung, die die [Einführung in die Access-Bereich](active-directory-saas-access-panel-introduction.md)verwenden können.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für FreshService
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-freshservice-tutorial/IC790807.png "Szenario")
##<a name="enabling-the-application-integration-for-freshservice"></a>Aktivieren die Anwendungsintegration für FreshService
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für FreshService aktiviert.

###<a name="to-enable-the-application-integration-for-freshservice-perform-the-following-steps"></a>Wenn die Anwendungsintegration für FreshService aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-freshservice-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-freshservice-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-freshservice-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-freshservice-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **FreshService**.

    ![Katalog der Anwendung] (./media/active-directory-saas-freshservice-tutorial/IC790808.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **FreshService aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Freshservice] (./media/active-directory-saas-freshservice-tutorial/IC790809.png "Freshservice")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu FreshService mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für FreshService konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **FreshService** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-freshservice-tutorial/IC790810.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der FreshService auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-freshservice-tutorial/IC790811.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **FreshService melden Sie sich auf die URL** Ihrer URL untersuchten Ihre Benutzer bei der Anwendung Freshdesk, klicken Sie auf (z. B.: "*http://democompany.freshservice.com/*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-freshservice-tutorial/IC790812.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei FreshService** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-freshservice-tutorial/IC790813.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens FreshService als Administrator.

6.  Klicken Sie oben im Menü auf **Administrator**.

    ![Administrator] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Administrator")

7.  Klicken Sie im **Portal für Kunden**auf **Sicherheit**.

    ![Sicherheit] (./media/active-directory-saas-freshservice-tutorial/IC790815.png "Sicherheit")

8.  Führen Sie im Abschnitt **Sicherheit** die folgenden Schritte aus:

    ![Einmaliges Anmelden] (./media/active-directory-saas-freshservice-tutorial/IC790816.png "Einmaliges Anmelden")

    1.  **Einmaliges OnON**zu wechseln.
    2.  **SAML-SSO**auswählen.
    3.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **SAML-Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei FreshService** .
    4.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei FreshService** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **URL Abmelden** .
    5.  Kopieren Sie den **Fingerabdruck** Wert aus dem exportierte Zertifikat, und fügen Sie ihn in das Textfeld **Sicherheit Zertifikat Fingerabdruck** .
    
        >[AZURE.TIP]Weitere Informationen hierzu finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-freshservice-tutorial/IC790817.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei FreshService zu ermöglichen, müssen er in FreshService bereitgestellt werden.  
Im Falle von FreshService ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **FreshService** an.

2.  Klicken Sie oben im Menü auf **Administrator**.

    ![Administrator] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Administrator")

3.  Klicken Sie im Abschnitt **Benutzermanagement** auf **anfordern**.

    ![Anfordern] (./media/active-directory-saas-freshservice-tutorial/IC790818.png "Anfordern")

4.  Klicken Sie auf **neue Auftraggeber**.

    ![Neue anfordern] (./media/active-directory-saas-freshservice-tutorial/IC790819.png "Neue anfordern")

5.  Führen Sie im Abschnitt **Neue Auftraggeber** die folgenden Schritte aus:

    ![Neue Auftraggeber] (./media/active-directory-saas-freshservice-tutorial/IC790820.png "Neue Auftraggeber")

    1.  Geben Sie **Vorname** und **e-Mail-** Attribute eines gültigen Azure Active Directory-Kontos gewünschten Bereitstellen in den verknüpften Textfeldern.
    2.  Klicken Sie auf **Speichern**.

    >[AZURE.NOTE] Der Inhaber des Azure Active Directory-Konto erhalten eine e-Mail einschließlich einen Link, um das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen FreshService Benutzer Konto Creation Tools können oder APIs bereitgestellt durch FreshService bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-freshservice-perform-the-following-steps"></a>Zuweisen von Benutzern zu FreshService führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **FreshService **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-freshservice-tutorial/IC790821.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-freshservice-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).