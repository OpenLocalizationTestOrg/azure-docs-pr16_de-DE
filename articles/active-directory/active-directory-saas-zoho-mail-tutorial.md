<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Zoho E-Mail | Microsoft Azure" 
    description="Erfahren Sie, wie mit Zoho E-Mail mit Azure Active Directory einmaliges Anmelden, automatisierte bereitgestellt und mehr aktiviert!." 
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
    ms.date="09/09/2016" 
    ms.author="markvi" />

#<a name="tutorial-azure-active-directory-integration-with-zoho-mail"></a>Lernprogramm: Azure-Active Directory-Integration mit Zoho Nachrichten
  
Ziel dieses Lernprogramms ist die Integration von Azure und Zoho e-Mail-Nachrichten angezeigt.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Zoho E-Mail
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Zoho E-Mail zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Zoho Mail (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Zoho Nachrichten
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Szenario")

##<a name="enabling-the-application-integration-for-zoho-mail"></a>Aktivieren die Anwendungsintegration für Zoho Nachrichten
  
In diesem Abschnitt Ziel ist es zu gliedernden wie Sie die Anwendungsintegration für Zoho Nachrichten aktivieren.

###<a name="to-enable-the-application-integration-for-zoho-mail-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für Zoho Nachrichten die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Zoho E-Mail**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Zoho E-Mail**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Zoho E-Mail] (./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho E-Mail")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Zoho E-Mail mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **E-Mail Zoho** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Zoho E-Mail auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Konfigurieren Sie einmaliges Anmelden")

3.  Führen Sie auf der Seite **Konfigurieren der App-URL** die folgenden Schritte aus:

    ![Konfigurieren der App-URL] (./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "Konfigurieren der App-URL")

    ein. Geben Sie in das Textfeld **Zoho E-Mail melden Sie sich auf URL** den URL dem folgenden Muster:`http://<company name>.ZohoMail.com`

    b. Klicken Sie auf **Weiter**.


4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Zoho E-Mail** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Zoho E-Mail als Administrator.

6.  Wechseln Sie zu der **Systemsteuerung**.

    ![Systemsteuerung] (./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Systemsteuerung")

7.  Klicken Sie auf der Registerkarte **SAML-Authentifizierung** .

    ![SAML-Authentifizierung] (./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "SAML-Authentifizierung")

8.  Führen Sie im Abschnitt **Details der SAML-Authentifizierung** die folgenden Schritte aus:

    ![Details der SAML-Authentifizierung] (./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "Details der SAML-Authentifizierung")

    1.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden durchsuchenden Zoho** kopieren Sie des Werts **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Anmelde-URL** .
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden durchsuchenden Zoho** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **URL Abmelden** .
    3.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden durchsuchenden Zoho** kopieren Sie den Wert für die **URL für das Kennwort ändern** , und fügen Sie ihn in das Textfeld **URL für das Kennwort ändern** .
    4.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    5.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **PublicKey** .
    6.  Wählen Sie als **Algorithmus** **RSA**aus.
    7.  Klicken Sie auf **OK**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Konfigurieren Sie einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzer zur Anmeldung bei Zoho E-Mail aktivieren möchten, müssen diese in eine E-Mail Zoho bereitgestellt werden.  
Wenn Zoho E-Mail ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Zoho E-Mail** an.

2.  Wechseln Sie zu **Systemsteuerung \> E-Mail und Dokumente für**.

3.  Wechseln Sie zu **Benutzerdetails \> Benutzer hinzufügen**.

    ![Fügen Sie Benutzer hinzu] (./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Fügen Sie Benutzer hinzu")

4.  Klicken Sie im Dialogfeld **Benutzer hinzufügen** führen Sie die folgenden Schritte aus:

    ![Fügen Sie Benutzer hinzu] (./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Fügen Sie Benutzer hinzu")

    1.  Geben Sie den **Vornamen**, **Nachnamen**, **E-Mail-ID**, **Kennwort** eines gültigen Azure Active Directory-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **OK**.  

        >[AZURE.NOTE] Der Inhaber des Azure Active Directory-Konto erhalten Sie eine e-Mail mit einem Link zu das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen Zoho E-Mail Benutzer Konto Creation Tools können oder APIs bereitgestellt per Post Zoho bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-zoho-mail-perform-the-following-steps"></a>Zuweisen von Benutzern zu Zoho Mail führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **E-Mail Zoho **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).