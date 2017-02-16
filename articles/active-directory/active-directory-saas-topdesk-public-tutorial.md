<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in TOPdesk - öffentlichen | Microsoft Azure" 
    description="Erfahren Sie, wie TOPdesk - öffentlichen mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren verwenden!" 
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

#<a name="tutorial-azure-directory-integration-with-topdesk---public"></a>Lernprogramm: Azure Verzeichnisintegration mit TOPdesk - öffentlichen

Das Ziel dieses Lernprogramms ist die Integration von Azure und TOPdesk - öffentlichen anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Ein TOPdesk - öffentlichen einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, Azure AD-Benutzer, die Ihnen zugewiesen haben, zu TOPdesk – öffentlichen domänenumgebung können einzelne melden Sie sich bei der Anwendung an Ihre TOPdesk - öffentlichen Firmenwebsite (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für TOPdesk - öffentlichen
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-topdesk-public-tutorial/IC790613.png "Szenario")

##<a name="enabling-the-application-integration-for-topdesk---public"></a>Aktivieren die Anwendungsintegration für TOPdesk - öffentlichen
  
Das Ziel der in diesem Abschnitt ist zu gliedernden So aktivieren Sie die Anwendungsintegration für TOPdesk - öffentlich.

###<a name="to-enable-the-application-integration-for-topdesk---public-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für TOPdesk - öffentlich ist, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-topdesk-public-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-topdesk-public-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-topdesk-public-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-topdesk-public-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **TOPdesk - öffentlichen**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-topdesk-public-tutorial/IC790614.png "Katalog der Anwendung")

7.  Im Bereich Ergebnisse wählen Sie **TOPdesk - öffentlichen aus**und dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Öffentliche TOPdesk] (./media/active-directory-saas-topdesk-public-tutorial/IC791317.png "Öffentliche TOPdesk")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren TOPdesk - öffentlichen mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Konfigurieren des einmaligen Anmeldens für TOPdesk - öffentlichen erfordert ein Logo Symboldatei hochladen. Um die Symboldatei zu erhalten, wenden Sie sich an das Supportteam TOPdesk.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Melden Sie sich als Administrator klicken Sie auf der Website Ihres Unternehmens **TOPdesk - öffentlichen** an.

2.  Klicken Sie auf **Einstellungen**, klicken Sie im Menü **TOPdesk** .

    ![Einstellungen] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Einstellungen")

3.  Klicken Sie auf **Login-Einstellungen**.

    ![Anmeldung-Einstellungen] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Anmeldung-Einstellungen")

4.  Erweitern Sie im Menü **Login Einstellungen** , und klicken Sie dann auf **Allgemein**.

    ![Allgemeine] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Allgemeine")

5.  Führen Sie im **öffentlichen** Abschnitt des Abschnitts **Login SAML** -Konfiguration die folgenden Schritte aus:

    ![Technical Settings] (./media/active-directory-saas-topdesk-public-tutorial/IC790601.png "Technical Settings")

    1.  Klicken Sie auf **herunterladen** , um die öffentliche Metadatendatei herunterladen, und klicken Sie dann auf Ihrem Computer lokal speichern.
    2.  Öffnen Sie die Metadatendatei, und suchen Sie den Knoten **AssertionConsumerService** .
        ![AssertionConsumerService] (./media/active-directory-saas-topdesk-public-tutorial/IC790619.png "AssertionConsumerService")
    3.  Kopieren Sie den Wert **AssertionConsumerService** ein.  

        >[AZURE.NOTE] Sie können den Wert im Abschnitt **Konfigurieren der App-URL** später in diesem Lernprogramm benötigen.

6.  In einem anderen Webbrowserfenster melden Sie sich bei Ihrem **Azure klassischen Portal** als Administrator.

7.  Klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren Single Sign On** , auf der Seite **TOPdesk - öffentlichen** Anwendung Integration.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-topdesk-public-tutorial/IC790620.png "Konfigurieren Sie einmaliges Anmelden")

8.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der TOPdesk - öffentlichen auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-topdesk-public-tutorial/IC790621.png "Konfigurieren Sie einmaliges Anmelden")

9.  Führen Sie auf der Seite **Konfigurieren der App-URL** die folgenden Schritte aus:

    ![Konfigurieren der App-URL] (./media/active-directory-saas-topdesk-public-tutorial/IC790622.png "Konfigurieren der App-URL")

    1.  Geben Sie in das Textfeld **TOPdesk - öffentliche melden Sie sich auf URL** die URL, die von den Benutzern verwendet werden, melden Sie sich bei Ihrem TOPdesk - Anwendung öffentlichen (z. B.: "*https://qssolutions.topdesk.net*").
    2.  Fügen Sie in das Textfeld **TOPdesk – öffentliche Antwort-URL** ein, die **TOPdesk - URL für Öffentliche AssertionConsumerService** (z. B.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Klicken Sie auf **Weiter**.

10. Auf der Seite **Konfigurieren einmaliges Anmelden bei TOPdesk - öffentlichen** um die Metadatendatei herunterladen möchten, klicken Sie auf **Herunterladen von Metadaten**, und speichern Sie die Datei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-topdesk-public-tutorial/IC790623.png "Konfigurieren Sie einmaliges Anmelden")

11. Führen Sie die folgenden Schritte aus, um eine Zertifikatsdatei zu erstellen:

    ![Zertifikat] (./media/active-directory-saas-topdesk-public-tutorial/IC790606.png "Zertifikat")

    1.  Öffnen Sie die heruntergeladene Metadatendatei ein.
    2.  Erweitern Sie den **RoleDescriptor** Knoten mit einem **xsi: Type** der **eingezogen: ApplicationServiceType**.
    3.  Kopieren Sie den Wert des **X509Certificate** Knotens.
    4.  Speichern Sie den kopierten **X509Certificate** Wert lokal auf Ihrem Computer in einer Datei ein.

12. Klicken Sie auf Ihrer TOPdesk - öffentlichen Firmenwebsite, klicken Sie im Menü **TOPdesk** auf **Einstellungen**.

    ![Einstellungen] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Einstellungen")

13. Klicken Sie auf **Login-Einstellungen**.

    ![Anmeldung-Einstellungen] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Anmeldung-Einstellungen")

14. Erweitern Sie im Menü **Login Einstellungen** , und klicken Sie dann auf **Allgemein**.

    ![Allgemeine] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Allgemeine")

15. Klicken Sie im Abschnitt **öffentlichen** auf **Hinzufügen**.

    ![SAML-Anmeldung] (./media/active-directory-saas-topdesk-public-tutorial/IC790625.png "SAML-Anmeldung")

16. Führen Sie auf der Dialogseite **SAML-Konfigurations-Assistent** die folgenden Schritte aus:

    ![SAML-Konfigurations-Assistent] (./media/active-directory-saas-topdesk-public-tutorial/IC790608.png "SAML-Konfigurations-Assistent")

    1.  Klicken Sie auf **Durchsuchen**, um Ihre heruntergeladene Metadatendatei, unter **Metadaten Föderation**hochzuladen.
    2.  Klicken Sie auf **Durchsuchen**, um Ihre Zertifikatsdatei, klicken Sie unter **Zertifikat (RSA)**, hochzuladen.
    3.  Klicken Sie auf **Durchsuchen**, um die Logodatei hochladen, die Sie vom Supportteam TOPdesk unter **-Logo-Symbols**erhalten haben.
    4.  Geben Sie in das Textfeld **Name-Attribut Benutzer** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**ein.
    5.  Geben Sie im Textfeld **Anzeigename** einen Namen für Ihre Konfiguration aus.
    6.  Klicken Sie auf **Speichern**.

17. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-topdesk-public-tutorial/IC790627.png "Konfigurieren Sie einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzer zur Anmeldung bei TOPdesk - Sprachoptionen veröffentlichen, klicken sie in TOPdesk - öffentlichen bereitgestellt werden.  
Wenn TOPdesk - veröffentlichen, einen manuellen Vorgang bereitgestellt wird.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator klicken Sie auf der Website Ihres Unternehmens **TOPdesk - öffentlichen** an.

2.  Klicken Sie im Menü oben auf **TOPdesk \> neu \> Support-Dateien \> Person**.

    ![Person] (./media/active-directory-saas-topdesk-public-tutorial/IC790628.png "Person")

3.  Klicken Sie im Dialogfeld neue Person führen Sie die folgenden Schritte aus:

    ![Neue Person] (./media/active-directory-saas-topdesk-public-tutorial/IC790629.png "Neue Person")

    1.  Klicken Sie auf der Registerkarte Allgemein.
    2.  Geben Sie im Textfeld Nachname den Nachnamen des ein gültiges Azure Active Directory-Konto, die, das Sie bereitstellen möchten.
    3.  Wählen Sie eine **Website** für das Konto ein.
    4.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Alle anderen TOPdesk - Tools für öffentliche Benutzer Konto erstellen können oder APIs von TOPdesk - öffentlichen bereitstellen AAD Benutzerkonten bereitgestellt.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-topdesk---public-perform-the-following-steps"></a>Zuweisen von Benutzern zu TOPdesk - öffentlich ist, führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **TOPdesk - öffentlichen **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-topdesk-public-tutorial/IC790630.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-topdesk-public-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).