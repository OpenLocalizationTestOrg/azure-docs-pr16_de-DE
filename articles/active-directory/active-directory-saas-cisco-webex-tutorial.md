<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Cisco Webex | Microsoft Azure" 
    description="Erfahren Sie, wie Cisco Webex mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Lernprogramm: Azure-Active Directory-Integration in Cisco Webex

Das Ziel dieses Lernprogramms ist die Integration von Azure und Cisco Webex anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Cisco Webex

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Cisco Webex zugewiesen haben können einzelne melden Sie sich bei der Anwendung an Ihre Cisco Webex Firmenwebsite (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Cisco Webex
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Szenario")
##<a name="enabling-the-application-integration-for-cisco-webex"></a>Aktivieren die Anwendungsintegration für Cisco Webex

So aktivieren Sie die Anwendungsintegration für Cisco Webex Gliedern ist das Ziel in diesem Abschnitt.

###<a name="to-enable-the-application-integration-for-cisco-webex-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Cisco Webex aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Cisco Webex**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Cisco Webex aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Cisco Webex] (./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Cisco Webex mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Cisco Webex** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Cisco Webex auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Konfigurieren einmaliges Anmelden")

3.  Klicken Sie auf der Seite **App-URL konfigurieren** führen Sie die folgenden Schritte aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der app-URL] (./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "Konfigurieren der app-URL")

    1.  Geben Sie in das Textfeld **Optimierung auf URL** die URL Ihrer Cisco Webex Mandanten (z. B.: *http://contoso.webex.com*).
    2.  Geben Sie Ihre **Cisco Webex AssertionConsumerService URL** in das Textfeld **Cisco Webex Antwort-URL** (z. B.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Cisco Webex** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Cisco Webex als Administrator.

6.  Klicken Sie im Menü oben auf **Websiteverwaltung**.

    ![Website verwalten] (./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Website verwalten")

7.  Klicken Sie im Abschnitt **Website verwalten** **SSO-Konfiguration**auf.

    ![SSO-Konfiguration] (./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO-Konfiguration")

8.  Führen Sie im Abschnitt Partnersuche Web SSO-Konfiguration die folgenden Schritte aus:

    ![Partnersuche SSO-Konfiguration] (./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Partnersuche SSO-Konfiguration")

    1.  Wählen Sie aus der Liste **Föderation Protokoll** **SAML 2.0**ein.
    2.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    3.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, und kopieren Sie den Inhalt der es.
    4.  Klicken Sie auf **SAML-Metadaten importieren**, und fügen Sie Ihre Base-64-codierte Zertifikat.
    5.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Cisco Webex** kopieren Sie den Wert des **Herausgebers URL** , und fügen Sie ihn in das Textfeld **Herausgeber für SAML (IdP-ID)** .
    6.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Kunden SSO Service Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Cisco Webex** .
    7.  Wählen Sie aus der Liste **NameID Format** **e-Mail-Adresse**ein.
    8.  Geben Sie in das Textfeld **AuthnContextClassRef** **Urn: Oasis: Namen: Tc: SAML:2.0:ac:classes:Password**ein.
    9.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Cisco Webex** kopieren Sie des Werts **Remote Abmeldung-URL** , und klicken Sie dann in das Textfeld **Kunden SSO-Dienst abmelden URL** einzufügen.
    10. Klicken Sie auf **Aktualisieren**.

9.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Cisco Webex** wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **abgeschlossen**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzer zur Anmeldung bei Cisco Webex aktivieren zu können, müssen er in Cisco Webex bereitgestellt werden.  
Wenn Cisco Webex ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **Cisco Webex** .

2.  Wechseln Sie zu **Verwalten von Benutzern \> Benutzer hinzufügen**.

    ![Hinzufügen von Benutzern] (./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Hinzufügen von Benutzern")

3.  Klicken Sie auf Benutzer hinzufügen im Abschnitt führen Sie die folgenden Schritte aus:

    ![Benutzer hinzufügen] (./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Benutzer hinzufügen")

    1.  Wählen Sie als **Kontotyp** **Host**aus.
    2.  Geben Sie die Informationen eines vorhandenen Benutzers Azure AD-in die folgenden Textfelder: **Vorname, Nachname**, **Benutzernamen**, **E-Mails**, **Ihr Kennwort ein**, **Bestätigen das Kennwort ein**.
    3.  Klicken Sie auf **Hinzufügen**.

>[AZURE.NOTE] Alle anderen Cisco Webex Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Cisco Webex bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Benutzer Azure AD-erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-cisco-webex-perform-the-following-steps"></a>Zuweisen von Benutzern zu Cisco Webex führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Cisco Webex **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
