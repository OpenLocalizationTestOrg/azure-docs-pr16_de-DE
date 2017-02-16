<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Arbeitstag | Microsoft Azure" 
    description="Erfahren Sie, wie einmaliges Anmelden, automatisierte bereitgestellt und mehr Aktivierung Arbeitstag mit Azure Active Directory mithilfe!." 
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
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-workday"></a>Lernprogramm: Azure-Active Directory-Integration in Arbeitstag
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Arbeitstag anzeigen. Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten in Arbeitstag
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Arbeitstag
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Konfigurieren der Benutzer bereitgestellt

![Szenario] (./media/active-directory-saas-workday-tutorial/IC782919.png "Szenario")

##<a name="enabling-the-application-integration-for-workday"></a>Aktivieren die Anwendungsintegration für Arbeitstag
  
Ziel dieses Abschnitts ist es, wie die Anwendung die Telefonintegration für Vertrieb gliedern.

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für Arbeitstag der folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-workday-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-workday-tutorial/IC700994.png "Applikationen")

4.  Um den **Katalog der Anwendung**zu öffnen, klicken Sie auf **Ein App hinzufügen**, und klicken Sie dann auf **Hinzufügen, eine Anwendung für meine Organisation verwenden**.

    ![Was möchten Sie tun?] (./media/active-directory-saas-workday-tutorial/IC700995.png "Was möchten Sie tun?")

5.  Geben Sie im **Suchfeld** **Arbeitstag**aus.

    ![Arbeitstag] (./media/active-directory-saas-workday-tutorial/IC701021.png "Arbeitstag")

6.  Wählen Sie im Ergebnisbereich **Arbeitstag aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Arbeitstag] (./media/active-directory-saas-workday-tutorial/IC701022.png "Arbeitstag")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Arbeitstag mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren Single Sign On** , auf der Seite **Arbeitstag** Anwendung Integration.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-workday-tutorial/IC782920.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Arbeitstag auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-workday-tutorial/IC782921.png "Konfigurieren einmaliges Anmelden")

3.  Klicken Sie auf der Seite **App-URL konfigurieren** führen Sie die folgenden Schritte aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-workday-tutorial/IC782957.png "Konfigurieren der App-URL")

    ein. Geben Sie in das Textfeld **Melden Sie sich auf URL** die URL zum Anmelden bei dem folgenden Muster Arbeitstag von Ihren Benutzern ein:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b.  Geben Sie in das Textfeld **Arbeitstag Antworten URL** der Arbeitstag Antworten URL dem folgenden Muster:`https://impl.workday.com/<tenant>/login-saml.htmld`

    >[AZURE.NOTE]Ihre Antwort-URL muss eine Unterdomäne verfügen (z. B.: Www, wd2, wd3, wd3-Impl, wd5, wd5-Impl). 
    >Funktioniert mit ungefähr wie "*http://www.myworkday.com*", "*http://myworkday.com*" jedoch nicht. 
 
4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden am Arbeitstag** um Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-workday-tutorial/IC782922.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Arbeitstag als Administrator.

6.  Wechseln Sie zu **Menü \> Arbeitsfläche**.

    ![Arbeitsfläche] (./media/active-directory-saas-workday-tutorial/IC782923.png "Arbeitsfläche")

7.  Wechseln Sie zu **Kontoverwaltung**.

    ![Kontoverwaltung] (./media/active-directory-saas-workday-tutorial/IC782924.png "Kontoverwaltung")

8.  Wechseln Sie zum **Bearbeiten des Mandanten Setup – Sicherheit**.

    ![Bearbeiten des Mandanten Sicherheit] (./media/active-directory-saas-workday-tutorial/IC782925.png "Bearbeiten des Mandanten Sicherheit")

9.  Führen Sie im Abschnitt **Umleitung URLs** die folgenden Schritte aus:

    ![Umleitung URLs] (./media/active-directory-saas-workday-tutorial/IC7829581.png "Umleitung URLs")

    ein. Klicken Sie auf **Zeile hinzufügen**.

    b. Geben Sie im Textfeld **Umleiten Anmelde-URL,** und das Textfeld **Umleiten Mobile-URL** die **Arbeitstag Mandanten URL** , die Sie auf der Seite **Konfigurieren der App-URL** des Portals Azure klassischen eingegeben haben.
    
    c. Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden am Arbeitstag** kopieren Sie die **Einzelnen Sign-Out Dienst-URL**, und klicken Sie dann in das Textfeld **Abmeldung umleiten URL** einzufügen.

    d.  Geben Sie den Umgebungsnamen **Umgebung** Textfeld ein.  


    >[AZURE.NOTE] Der Wert für das Attribut Umgebung ist der Wert des Mandanten URLs verknüpft:
    >
    >-   Wenn Sie den Domänennamen für den Mandanten Arbeitstag URL mit Impl beginnt (z. B.: *https://impl.workday.com/\<Mandanten\>/login-saml2.htmld*), das Attribut **Umgebung** muss auf Implementierung festgelegt werden.
    >-   Wenn Sie den Namen der Domäne mit etwas anderem gestartet wird, müssen Sie Arbeitstag, um den Wert **Umgebung** zu erhalten.

10. Führen Sie im Abschnitt **SAML-Setup** die folgenden Schritte aus:

    ![SAML-Setup] (./media/active-directory-saas-workday-tutorial/IC782926.png "SAML-Setup")

    ein.  Wählen Sie **SAML-Authentifizierung aktivieren**.

    b.  Klicken Sie auf **Zeile hinzufügen**.

11. Führen Sie im Abschnitt SAML-Identitätsanbieter die folgenden Schritte aus:

    ![SAML-Identitätsanbieter] (./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML-Identitätsanbieter")

    ein. Geben Sie in das Textfeld Identität Anbietername einen Anbieternamen (z. B.: *SPInitiatedSSO*).

    b. Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden am Arbeitstag** kopieren Sie des Werts **Identität Anbieter-ID** , und fügen Sie ihn in das Textfeld **Herausgeber** .

    c. Die Option **Aktivieren Workday Initialted Abmelden**.

    d. Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden am Arbeitstag** kopieren Sie den Wert für die **Einzelnen Sign-Out Dienst-URL** , und klicken Sie dann in das Textfeld **Abmeldung anfordern URL** einzufügen.


    e. Klicken Sie auf **Identität Anbieter öffentlicher Schlüsselzertifikat**, und klicken Sie dann auf **Erstellen**. 

    ![Erstellen] (./media/active-directory-saas-workday-tutorial/IC782928.png "Erstellen")

    f. Klicken Sie auf **X509 Erstellen öffentlicher Schlüssel**. 
        
    ![Erstellen] (./media/active-directory-saas-workday-tutorial/IC782929.png "Erstellen")


1. Führen Sie im Abschnitt **Ansicht X509 öffentlicher Schlüssel** die folgenden Schritte aus: 

    ![Ansicht X509 öffentlicher Schlüssel] (./media/active-directory-saas-workday-tutorial/IC782930.png "Ansicht X509 öffentlicher Schlüssel") 

    ein. Geben Sie in das Textfeld **Name** einen Namen für das Zertifikat (z. B.: *PPE\_SP*).
        
    b. Geben Sie in das Textfeld **Gültig ab** des gültigen Attributwert, der das Zertifikat ein.
    
    c.  Geben Sie in das Textfeld **Gültig bis** die gültigen Attribut Wert, der das Zertifikat ein.
        
    >[AZURE.NOTE] Sie können die gültigen von Datum und die gültigen aus dem heruntergeladenen Zertifikat Datum, indem Sie darauf doppelklicken erhalten. Die Datumsangaben werden unter der Registerkarte **Details** aufgeführt.

    d. Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

    >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    e.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, und kopieren Sie den Inhalt der es.
    
    f.  Fügen Sie den Inhalt der Zwischenablage in das Textfeld **Zertifikat** .
    
    g.  Klicken Sie auf **OK**.

12.  Führen Sie die folgenden Schritte aus: 

    ![SSO-Konfiguration] (./media/active-directory-saas-workday-tutorial/IC7829351111.png "SSO-Konfiguration")

    ein.  Aktivieren der **X509 Private Schlüssel Paar**.

    b.  Geben Sie in das Textfeld **Service Provider-ID** **http://www.workday.com**aus.

    c.  Wählen Sie **Aktivieren initiiert SP SAML-Authentifizierung**aus.

    d.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden am Arbeitstag** kopieren Sie den Wert für die **Einzelnen anmelden Dienst-URL** , und fügen Sie ihn in das Textfeld **IdP SSO-Dienst-URL** .
     
    e. Wählen Sie aus, **nicht nach SP initiiert Authentifizierung Anforderung verkleinern**.

    f. Wählen Sie unter **Anfordern Signatur Authentifizierungsmethode** **SHA256**aus. 
        
    ![Authentifizierung anfordern Signaturmethode] (./media/active-directory-saas-workday-tutorial/IC782932.png "Authentifizierung anfordern Signaturmethode") 
 
    g. Klicken Sie auf **OK**. 
        
    ![OK] (./media/active-directory-saas-workday-tutorial/IC782933.png "OK")

12. Klicken Sie im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden am Arbeitstag** auf **Weiter**. 

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-workday-tutorial/IC782934.png "Konfigurieren einmaliges Anmelden")

13. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**. 

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-workday-tutorial/IC782935111.png "Konfigurieren einmaliges Anmelden")



##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um einen Testbenutzer nach der Bereitstellung in Arbeitstag zu gelangen, müssen Sie die Arbeitstag-Supportteam.  
Das Supportteam Arbeitstag wird den Benutzer erstellt.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-workday-perform-the-following-steps"></a>Zuweisen von Benutzern zu Arbeitstag führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Arbeitstag **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-workday-tutorial/IC782935.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-workday-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).