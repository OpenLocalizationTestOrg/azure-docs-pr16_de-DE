<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Jitbit Helpdesk | Microsoft Azure" 
    description="Erfahren Sie, wie Jitbit Helpdesk mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Lernprogramm: Azure-Active Directory-Integration in Jitbit Helpdesk
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Jitbit Helpdesk anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Jitbit Helpdesk
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie an den Jitbit Helpdesk zugewiesen haben können einzelne melden Sie sich bei der Anwendung an Ihre Jitbit Helpdesk Firmenwebsite (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Jitbit Helpdesk
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777676.png "Szenario")
##<a name="enabling-the-application-integration-for-jitbit-helpdesk"></a>Aktivieren die Anwendungsintegration für Jitbit Helpdesk
  
In diesem Abschnitt Ziel ist es zu gliedernden So aktivieren Sie die Anwendungsintegration für Jitbit Helpdesk.

###<a name="to-enable-the-application-integration-for-jitbit-helpdesk-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für Jitbit Helpdesk die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Jitbit Helpdesk**.

    ![Katalog der Anwendung] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777677.png "Katalog der Anwendung")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Jitbit Helpdesk aus**und dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![JitBit] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC781008.png "JitBit")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer an den Jitbit Helpdesk authentifizieren mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert. .  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

>[AZURE.IMPORTANT] Um einmaliges Anmelden auf Ihrem Mandanten Jitbit Helpdesk konfigurieren können, müssen Sie zuerst den Jitbit Helpdesk technischen Support zum Abrufen dieses Feature aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Jitbit Helpdesk** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777678.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer melden Sie sich an den Jitbit Helpdesk auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777679.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Jitbit Helpdesk melden Sie sich In URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. Jitbit.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der app-URL] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777528.png "Konfigurieren der app-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden bei Jitbit Helpdesk** , wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und klicken Sie dann speichern Zertifikatsdatei lokal als **c:\\Jitbit Helpdesk.cer**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777680.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Jitbit Helpdesk als Administrator.

6.  Klicken Sie oben auf der Symbolleiste auf **Verwaltung**.

    ![Verwaltung] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Verwaltung")

7.  Klicken Sie auf **Allgemeine Einstellungen**.

    ![Benutzer, Unternehmen und Berechtigungen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Benutzer, Unternehmen und Berechtigungen")

8.  Führen Sie im Konfigurationsabschnitt **Authentifizierungseinstellungen** die folgenden Schritte aus:

    ![Authentifizierungseinstellungen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777683.png "Authentifizierungseinstellungen")

    1.  Wählen Sie **Anmelden aktivieren SAML 2.0 einzelne** Anmeldung für einmaliges Anmelden (SSO) mit **OneLogin**verwenden.
    2.  Im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden bei Jitbit Helpdesk** kopieren Sie des Werts **Service Provider (SP) initiiert Endpunkt** , und klicken Sie dann in das Textfeld **Endpunkt-URL** einzufügen.
    3.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.
        
        >[AZURE.TIP]Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    4.  Öffnen Sie Ihre Base-64-codierte Zertifikat, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **X 509-Zertifikat**
    5.  Klicken Sie auf **Änderungen speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777684.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Jitbit Helpdesk zu ermöglichen, müssen er in Jitbit Helpdesk bereitgestellt werden.  
Wenn Jitbit Helpdesk ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **Jitbit Helpdesk** .

2.  Klicken Sie im Menü oben auf **Verwaltung**.

    ![Verwaltung] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Verwaltung")

3.  Klicken Sie auf **Benutzer, Unternehmen und Berechtigungen**.

    ![Benutzer, Unternehmen und Berechtigungen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Benutzer, Unternehmen und Berechtigungen")

4.  Klicken Sie auf **Benutzer hinzufügen**.

    ![Benutzer hinzufügen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777685.png "Benutzer hinzufügen")

5.  Geben Sie im Abschnitt erstellen, die Daten für das Konto ein Azure AD-bereitstellen in die folgenden Textfelder soll: **Username**, **E-Mails**, **Vorname**, **Nachname**

    ![Erstellen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777686.png "Erstellen")

6.  Klicken Sie auf **Erstellen**.

>[AZURE.NOTE] Alle anderen Jitbit Helpdesk Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Jitbit Helpdesk bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-jitbit-helpdesk-perform-the-following-steps"></a>Um Benutzer an den Jitbit Helpdesk zuweisen möchten, führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Jitbit Helpdesk **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777687.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).