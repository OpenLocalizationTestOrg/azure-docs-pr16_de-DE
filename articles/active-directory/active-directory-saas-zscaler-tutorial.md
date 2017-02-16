<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Zscaler | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Zscaler mit Azure Active Directory einmaliges Anmelden, automatisierte bereitgestellt und mehr aktivieren!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zscaler"></a>Lernprogramm: Azure-Active Directory-Integration in Zscaler
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Zscaler anzeigen. Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten in Zscaler
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Zscaler
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren von Proxyeinstellungen
4.  Konfigurieren der Benutzer bereitgestellt
5.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-zscaler-tutorial/IC769226.png "Szenario")

##<a name="enabling-the-application-integration-for-zscaler"></a>Aktivieren die Anwendungsintegration für Zscaler
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Zscaler aktiviert.

###<a name="to-enable-the-application-integration-for-zscaler-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Zscaler aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zscaler-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-zscaler-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-zscaler-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-zscaler-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Zscaler**.

    ![Katalog der Anwendung] (./media/active-directory-saas-zscaler-tutorial/IC769227.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Zscaler aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Zscaler] (./media/active-directory-saas-zscaler-tutorial/IC769228.png "Zscaler")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Zscaler mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie ein Zertifikat zum Zscaler hochladen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Zscaler** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-zscaler-tutorial/IC769229.png "Einmaliges Anmelden aktivieren")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Zscaler auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Einzelne konfigurieren melden Sie sich auf] (./media/active-directory-saas-zscaler-tutorial/IC769230.png "Einzelne konfigurieren melden Sie sich auf")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Zscaler melden Sie sich In URL** Geben Sie Ihren Benutzernamen in URL, die Sie von Zscaler erhalten haben, und klicken Sie dann auf **Weiter**: 

    >[AZURE.NOTE] Wenden Sie sich an das Supportteam Zscaler, wenn Sie nicht wissen, was Ihre anmelden URL ist.

    ![Konfigurieren der app-URL] (./media/active-directory-saas-zscaler-tutorial/IC769231.png "Konfigurieren der app-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Zscaler** führen Sie die folgenden Schritte aus:

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-zscaler-tutorial/IC769232.png "Konfigurieren einmaliges Anmelden")

    1.  Klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal als **c:\\Zscaler.cer**.
    2.  Kopieren Sie die **URL der Authentifizierung Anforderung** in der Zwischenablage ein.

5.  Melden Sie sich bei Ihrem Mandanten Zscaler an.

6.  Klicken Sie im Menü oben auf **Verwaltung**.

    ![Verwaltung] (./media/active-directory-saas-zscaler-tutorial/IC769486.png "Verwaltung")

7.  Klicken Sie unter **Verwalten von Administratoren und Rollen**klicken Sie auf **Benutzer Aufgabenbereich und Authentifizierung**.

    ![Administratoren und Rollen verwalten] (./media/active-directory-saas-zscaler-tutorial/IC769487.png "Administratoren und Rollen verwalten")

8.  Führen Sie im Abschnitt **Authentifizierung die Option für Ihre Organisation auswählen** die folgenden Schritte aus:

    ![Wählen Sie Authentifizierung] (./media/active-directory-saas-zscaler-tutorial/IC769488.png "Wählen Sie Authentifizierung")

    1.  Wählen Sie **authentifizieren mithilfe von SAML einmaliges Anmelden**.
    2.  Klicken Sie auf **Konfigurieren von SAML einzelnen anmelden Parameter**.

9.  Führen Sie die folgenden Schritte aus, und klicken Sie dann auf **Fertig**, auf der Seite Dialogfeld **Konfigurieren von SAML einzelnen anmelden Parameter** :

    ![Hochladen des Zertifikats] (./media/active-directory-saas-zscaler-tutorial/IC769489.png "Hochladen des Zertifikats")

    1.  Fügen Sie den Wert im Feld **Authentifizierung Anforderung URL** vom klassischen Azure-Portal im Textfeld **URL des Portals SAML an die Benutzer für die Authentifizierung gesendet werden** .
    2.  Geben Sie in das Textfeld **Attribut, Anmeldename enthält** **NameID**aus.
    3.  Hochladen von im Feld **Hochladen öffentlichen SSL-Zertifikat** das Zertifikat, das Sie vom klassischen Azure-Portal heruntergeladen haben.
    4.  Wählen Sie **Automatische SAML - Bereitstellung aktivieren**.

10. Klicken Sie auf der Dialogseite **Benutzerauthentifizierung konfigurieren** führen Sie die folgenden Schritte aus:

    ![Konfigurieren der Benutzerauthentifizierung] (./media/active-directory-saas-zscaler-tutorial/IC769490.png "Konfigurieren der Benutzerauthentifizierung")

    1.  Klicken Sie auf **Speichern**.
    2.  Klicken Sie auf **jetzt aktivieren**.

11. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-zscaler-tutorial/IC769491.png "Konfigurieren einmaliges Anmelden")

##<a name="configuring-proxy-settings"></a>Konfigurieren von Proxyeinstellungen

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>So konfigurieren Sie die Proxyeinstellungen in Internet Explorer

1.  Starten Sie **InternetExplorer**.

2.  Wählen Sie **Internetoptionen** im Menü **Extras** , um das Dialogfeld **Internetoptionen** zu öffnen.

    ![Internetoptionen] (./media/active-directory-saas-zscaler-tutorial/IC769492.png "Internetoptionen")

3.  Klicken Sie auf die Registerkarte **Verbindungen** .

    ![Verbindungen] (./media/active-directory-saas-zscaler-tutorial/IC769493.png "Verbindungen")

4.  Klicken Sie auf **LAN-Einstellungen** , um das Dialogfeld **LAN-Einstellungen** zu öffnen.

5.  Führen Sie im Abschnitt Proxy-Server die folgenden Schritte aus:

    ![Proxyserver] (./media/active-directory-saas-zscaler-tutorial/IC769494.png "Proxyserver")

    1.  Wählen Sie einen Proxyserver für LAN verwenden.
    2.  Geben Sie in das Textfeld Adresse **gateway.zscalertwo.net**ein.
    3.  Geben Sie im Textfeld Port den Wert **80**ein.
    4.  Wählen Sie **Proxyserver für lokale Adressen umgehen**aus.
    5.  Klicken Sie auf **OK** , um das Dialogfeld **Einstellungen für lokales Netzwerk (LAN)** zu schließen.

6.  Klicken Sie auf **OK** , um das Dialogfeld **Internetoptionen** zu schließen.

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Zscaler zu ermöglichen, müssen er in Zscaler bereitgestellt werden.  
Im Falle von Zscaler ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **Zscaler** .

2.  Klicken Sie auf **Verwaltung**.

    ![Verwaltung] (./media/active-directory-saas-zscaler-tutorial/IC781035.png "Verwaltung")

3.  Klicken Sie auf **Benutzermanagement**.

    ![Verwaltung der Benutzer] (./media/active-directory-saas-zscaler-tutorial/IC781036.png "Verwaltung der Benutzer")

4.  Klicken Sie auf der Registerkarte **Benutzer** auf **Hinzufügen**.

    ![Hinzufügen] (./media/active-directory-saas-zscaler-tutorial/IC781037.png "Hinzufügen")

5.  Führen Sie im Abschnitt Benutzer hinzufügen die folgenden Schritte aus:

    ![Fügen Sie Benutzer hinzu] (./media/active-directory-saas-zscaler-tutorial/IC781038.png "Fügen Sie Benutzer hinzu")

    1.  Geben Sie die **Benutzer-ID**, **Anzeigenamen des Benutzers ein**, **das Kennwort**, **Kennwort bestätigen**ein, und wählen Sie dann **Gruppen** und der **Abteilung** eines gültigen AAD-Kontos ein, die Sie bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Können Sie alle anderen Zscaler Benutzer Konto Tool zum Erstellen oder APIs bereitgestellt durch Zscaler bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-zscaler-perform-the-following-steps"></a>Zuweisen von Benutzern zu Zscaler führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Zscaler** Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-zscaler-tutorial/IC769495.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-zscaler-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
