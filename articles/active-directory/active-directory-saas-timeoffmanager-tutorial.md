<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in TimeOffManager | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von TimeOffManager mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Lernprogramm: Azure-Active Directory-Integration in TimeOffManager
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und TimeOffManager anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine TimeOffManager einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie TimeOffManager zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite TimeOffManager (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für TimeOffManager
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-timeoffmanager-tutorial/IC795909.png "Szenario")

##<a name="enabling-the-application-integration-for-timeoffmanager"></a>Aktivieren die Anwendungsintegration für TimeOffManager
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für TimeOffManager aktiviert.

###<a name="to-enable-the-application-integration-for-timeoffmanager-perform-the-following-steps"></a>Wenn die Anwendungsintegration für TimeOffManager aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-timeoffmanager-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-timeoffmanager-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-timeoffmanager-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-timeoffmanager-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **TimeOffManager**.

    ![Katalog der Anwendung] (./media/active-directory-saas-timeoffmanager-tutorial/IC795910.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **TimeOffManager aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![TimeOffManager] (./media/active-directory-saas-timeoffmanager-tutorial/IC795911.png "TimeOffManager")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu TimeOffManager mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikat in Ihrem Mandanten TimeOffManager hochladen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **TimeOffManager** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-timeoffmanager-tutorial/IC795912.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der TimeOffManager auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-timeoffmanager-tutorial/IC795913.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **TimeOffManager Antwort-URL** die URL Ihrer TimeOffManager AssertionConsumerService (z. B.: "*Beispiel: https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company\_Id = IC34216*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-timeoffmanager-tutorial/IC795914.png "Konfigurieren der App-URL")

    Sie können die Antwort-URL aus der TimeOffManager für einmaliges Anmelden Einstellung Seite erhalten.

    ![Einstellungen für einzelne Zeichen] (./media/active-directory-saas-timeoffmanager-tutorial/IC795915.png "Einstellungen für einzelne Zeichen")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei TimeOffManager** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-timeoffmanager-tutorial/IC795916.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens TimeOffManager als Administrator.

6.  Wechseln Sie zu **Konto \> Konto Optionen \> anmelden Einstellungen einzelner**.

    ![Einstellungen für einzelne Zeichen] (./media/active-directory-saas-timeoffmanager-tutorial/IC795917.png "Einstellungen für einzelne Zeichen")

7.  Führen Sie im Abschnitt **Einstellungen für einzelne Zeichen** die folgenden Schritte aus:

    ![Einstellungen für einzelne Zeichen] (./media/active-directory-saas-timeoffmanager-tutorial/IC795918.png "Einstellungen für einzelne Zeichen")

    ein.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    b.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie das gesamte Zertifikat in das Textfeld **X 509-Zertifikat** .
    
    c.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei TimeOffManager** kopieren Sie den Wert des **Herausgebers URL** , und fügen Sie ihn in das Textfeld **Idp Herausgeber** .
    
    d.  Kopieren Sie auf der Seite **Konfigurieren einmaliges Anmelden bei TimeOffManager** im Azure klassischen Portal des Werts **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **IdP Endpunkt-URL** .
    
    e.  Als **SAML erzwingen**wählen Sie **Nein**aus.
    

    f.  Als **Benutzer automatisch erstellen**wählen Sie **Ja**aus.
    
    g.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei TimeOffManager** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **URL Abmelden** .
    
    h.  Klicken Sie auf **Änderungen speichern**.

8.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei TimeOffManager** wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **abgeschlossen**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-timeoffmanager-tutorial/IC795919.png "Konfigurieren Sie einmaliges Anmelden")

9.  Klicken Sie im Menü oben auf **Attributen** , um das Dialogfeld **Token SAML-Attribute** zu öffnen.

    ![Attribute] (./media/active-directory-saas-timeoffmanager-tutorial/IC795920.png "Attribute")

10. Um die Zuordnungen erforderliches Attribut hinzufügen möchten, führen Sie die folgenden Schritte aus:

    ![token SAML-Attribute] (./media/active-directory-saas-timeoffmanager-tutorial/123.png "token SAML-Attribute")

  	|Attributname|Attributwert|
  	|---|---|
  	|E-Mail|User.Mail|
  	|Vorname|User.GivenName|
  	|Nachname|User.Surname|

    ein.  Klicken Sie für jede Datenzeile von in der Tabelle oben auf **Benutzerattribut hinzufügen**.

    b.  Geben Sie in das Textfeld **Name Attribut** das Attribut Bankname für diese Zeile ein.

    c.  Wählen Sie in das Textfeld **Wert Attribut** für diese Zeile angezeigten Attributwert ein.

    d.  Klicken Sie auf **abgeschlossen**.

11. Klicken Sie auf **Änderungen anzuwenden**.

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei TimeOffManager zu ermöglichen, müssen er TimeOffManager bereitgestellt werden.  
TimeOffManager unterstützt nur in der Zeit Benutzer bereitgestellt. Es gibt keine Aktionselement für Sie ein.  
Die Benutzer werden automatisch während der erstmaligen Anmeldung mit einmaliges auf hinzugefügt.

>[AZURE.NOTE] Alle anderen TimeOffManager Benutzer Konto Creation Tools können oder APIs bereitgestellt durch TimeOffManager bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-timeoffmanager-perform-the-following-steps"></a>Zuweisen von Benutzern zu TimeOffManager führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **TimeOffManager** Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-timeoffmanager-tutorial/IC795922.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-timeoffmanager-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
