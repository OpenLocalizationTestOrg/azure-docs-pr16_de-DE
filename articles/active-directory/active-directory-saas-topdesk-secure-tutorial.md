<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in TOPdesk - Secure | Microsoft Azure"
    description="Informationen zum Verwenden von TOPdesk - Secure mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!." 
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

#<a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Lernprogramm: Azure-Active Directory-Integration in TOPdesk - Sicherheit
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und TOPdesk - Secure anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Ein TOPdesk - Secure einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie TOPdesk - Secure zugewiesen haben können einzelne melden Sie sich bei der Anwendung an Ihre TOPdesk - Secure Firmenwebsite (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für TOPdesk - Secure
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Szenario")

##<a name="enabling-the-application-integration-for-topdesk---secure"></a>Aktivieren die Anwendungsintegration für TOPdesk - Secure
  
In diesem Abschnitt Ziel ist es zu gliedernden wie Sie die Anwendungsintegration für TOPdesk - Secure aktivieren.

###<a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a>Zum Aktivieren der Anwendungsintegration für TOPdesk - Secure, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **TOPdesk - Secure**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Katalog der Anwendung")

7.  Im Bereich Ergebnisse wählen Sie **TOPdesk - Secure aus**und dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![TOPdesk - Sicherheit] (./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - Sicherheit")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt besteht darin, wie Sie Benutzer zur TOPdesk - Authentifizierung aktivieren Gliedern Secure mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden.  
Konfigurieren des einmaligen Anmeldens für TOPdesk - Secure erfordert ein Logo Symboldatei hochladen. Um die Symboldatei zu erhalten, wenden Sie sich an das Supportteam TOPdesk.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Melden Sie sich auf der Website Ihres Unternehmens **TOPdesk - Secure** als Administrator an.

2.  Klicken Sie auf **Einstellungen**, klicken Sie im Menü **TOPdesk** .

    ![Einstellungen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Einstellungen")

3.  Klicken Sie auf **Login-Einstellungen**.

    ![Anmeldung-Einstellungen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Anmeldung-Einstellungen")

4.  Erweitern Sie im Menü **Login Einstellungen** , und klicken Sie dann auf **Allgemein**.

    ![Allgemeine] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Allgemeine")

5.  Führen Sie im Abschnitt **Secure** des Abschnitts **Login SAML** -Konfiguration die folgenden Schritte aus:

    ![Technical Settings] (./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "Technical Settings")

    1.  Klicken Sie auf **herunterladen** , um die öffentliche Metadatendatei herunterladen, und klicken Sie dann auf Ihrem Computer lokal speichern.
    2.  Öffnen Sie die Metadatendatei, und suchen Sie den Knoten **AssertionConsumerService** .
        ![Assertion Consumer-Dienst] (./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Assertion Consumer-Dienst")
    3.  Kopieren Sie den Wert **AssertionConsumerService** ein.  

        >[AZURE.NOTE] Sie können den Wert im Abschnitt **Konfigurieren der App-URL** später in diesem Lernprogramm benötigen.

6.  In einem anderen Webbrowserfenster melden Sie sich bei Ihrem **Azure klassischen Portal** als Administrator.

7.  Klicken Sie auf der Seite **TOPdesk - sichere** Integration Anwendung auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Konfigurieren Sie einmaliges Anmelden")

8.  Auf der Seite **Wie möchten Sie Benutzer bei der TOPdesk - Secure auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Konfigurieren Sie einmaliges Anmelden")

9.  Führen Sie auf der Seite **Konfigurieren der App-URL** die folgenden Schritte aus:

    ![Konfigurieren der App-URL] (./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "Konfigurieren der App-URL")

    1.  Geben Sie in das Textfeld **TOPdesk - Secure melden Sie sich auf URL** die URL, die von den Benutzern verwendet werden, melden Sie sich bei Ihrem TOPdesk - sicheren Anwendung (z. B.: "*https://qssolutions.topdesk.net*").
    2.  Fügen Sie der **TOPdesk - Secure AssertionConsumerService URL** in das Textfeld **TOPdesk – öffentliche Antwort-URL** (z. B.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Klicken Sie auf **Weiter**.

10. Auf der Seite **Konfigurieren einmaliges Anmelden bei TOPdesk - Secure** um die Metadatendatei herunterladen möchten, klicken Sie auf **Herunterladen von Metadaten**, und speichern Sie die Datei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Konfigurieren Sie einmaliges Anmelden")

11. Führen Sie die folgenden Schritte aus, um eine Zertifikatsdatei zu erstellen:

    ![Zertifikat] (./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "Zertifikat")

    1.  Öffnen Sie die heruntergeladene Metadatendatei ein.
    2.  Erweitern Sie den **RoleDescriptor** Knoten mit einem **xsi: Type** der **eingezogen: ApplicationServiceType**.
    3.  Kopieren Sie den Wert des **X509Certificate** Knotens.
    4.  Speichern Sie den kopierten **X509Certificate** Wert lokal auf Ihrem Computer in einer Datei ein.

12. Klicken Sie auf Ihrer TOPdesk - Secure Firmenwebsite, klicken Sie im Menü **TOPdesk** auf **Einstellungen**.

    ![Einstellungen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Einstellungen")

13. Klicken Sie auf **Login-Einstellungen**.

    ![Anmeldung-Einstellungen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Anmeldung-Einstellungen")

14. Erweitern Sie im Menü **Login Einstellungen** , und klicken Sie dann auf **Allgemein**.

    ![Allgemeine] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Allgemeine")

15. Klicken Sie im Abschnitt **öffentlichen** auf **Hinzufügen**.

    ![Hinzufügen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Hinzufügen")

16. Führen Sie auf der Dialogseite **SAML-Konfigurations-Assistent** die folgenden Schritte aus:

    ![SAML-Konfigurations-Assistent] (./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML-Konfigurations-Assistent")

    1.  Klicken Sie auf **Durchsuchen**, um Ihre heruntergeladene Metadatendatei, unter **Metadaten Föderation**hochzuladen.
    2.  Klicken Sie auf **Durchsuchen**, um Ihre Zertifikatsdatei, klicken Sie unter **Zertifikat (RSA)**, hochzuladen.
    3.  Klicken Sie auf **Durchsuchen**, um die Logodatei hochladen, die Sie vom Supportteam TOPdesk unter **-Logo-Symbols**erhalten haben.
    4.  Geben Sie in das Textfeld **Name-Attribut Benutzer** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**ein.
    5.  Geben Sie im Textfeld **Anzeigename** einen Namen für Ihre Konfiguration aus.
    6.  Klicken Sie auf **Speichern**.

17. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Konfigurieren Sie einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzer zur Anmeldung bei TOPdesk - Sprachoptionen müssen sicher sind, diese in TOPdesk - Secure bereitgestellt werden.  
Wenn TOPdesk - ist sicher, provisioning eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich auf der Website Ihres Unternehmens **TOPdesk - Secure** als Administrator an.

2.  Klicken Sie im Menü oben auf **TOPdesk \> neu \> Support-Dateien \> Operator**.

    ![Operator] (./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Operator")

3.  Klicken Sie im Dialogfeld **Neuer Operator** führen Sie die folgenden Schritte aus:

    ![Neuer Operator] (./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "Neuer Operator")

    1.  Klicken Sie auf der Registerkarte Allgemein.
    2.  Geben Sie im Textfeld **Nachname** der Abschnitt **Allgemein** den Nachnamen eines gültigen Azure Active Directory-Kontos ein, die Sie bereitstellen möchten.
    3.  Wählen Sie im Abschnitt **Speicherort** einer **Website** für das Konto ein.
    4.  Geben Sie im Textfeld **Benutzername** des Abschnitts **TOPdesk Login** einen Benutzernamen für Ihre Benutzer ein.
    5.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Sie können eine beliebige andere TOPdesk - Secure Benutzer Konto Creation Tools oder TOPdesk - zur Bereitstellung von Benutzerkonten AAD Secure-APIs verwenden.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a>Klicken Sie zum Zuweisen von Benutzern zu TOPdesk - Secure, führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **TOPdesk - sichere **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).