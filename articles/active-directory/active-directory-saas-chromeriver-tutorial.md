<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Chromeriver | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Chromeriver mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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


#<a name="tutorial-azure-active-directory-integration-with-chromeriver"></a>Lernprogramm: Azure-Active Directory-Integration in Chromeriver

Das Ziel dieses Lernprogramms ist die Integration von Azure und Chromeriver anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Eine Chromeriver einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Chromeriver zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Chromeriver (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Chromeriver
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-chromeriver-tutorial/IC802755.png "Szenario")
##<a name="enabling-the-application-integration-for-chromeriver"></a>Aktivieren die Anwendungsintegration für Chromeriver

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Chromeriver aktiviert.

###<a name="to-enable-the-application-integration-for-chromeriver-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Chromeriver aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-chromeriver-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-chromeriver-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-chromeriver-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-chromeriver-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Chromeriver**.

    ![Katalog der Anwendung] (./media/active-directory-saas-chromeriver-tutorial/IC802756.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Chromeriver aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Chromeriver mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Chromeriver** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-chromeriver-tutorial/IC802757.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Chromeriver auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-chromeriver-tutorial/IC802758.png "Konfigurieren Sie einmaliges Anmelden")

3.  Führen Sie auf der Seite **Einstellungen für die App konfigurieren** die folgenden Schritte aus:

    ![Konfigurieren der App-Einstellungen] (./media/active-directory-saas-chromeriver-tutorial/IC802759.png "Konfigurieren der App-Einstellungen")

    1.  Geben Sie im Textfeld **URL Antworten** der Chromeriver **AssertionConsumerService-URL** (z. B.: *https://qa-app.chromeriver.com/login/sso/saml/consume?customerId=911*).  

        >[AZURE.NOTE] Sie können diesen Wert von Ihrem Supportteam Chromeriver abrufen.

    2.  Klicken Sie auf **Weiter**

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Chromeriver** zum Herunterladen der Metadaten Ihrer auf **Metadaten herunterladen**und speichern Sie die Metadatendatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-chromeriver-tutorial/IC802760.png "Konfigurieren Sie einmaliges Anmelden")

5.  Senden Sie die heruntergeladene Metadatendatei an Ihr Supportteam Chromeriver ein.

    >[AZURE.NOTE]Führen Sie die eigentliche SSO-Konfiguration muss das Supportteam Chromeriver.  
    Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.

6.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-chromeriver-tutorial/IC802761.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzern zur Anmeldung bei Chromeriver zu ermöglichen, müssen er in Chromeriver bereitgestellt werden.  
Bei Chromeriver müssen die Benutzerkonten von Ihrem Supportteam Chromeriver erstellt werden.

>[AZURE.NOTE] Alle anderen Chromeriver Benutzer Konto Creation Tools können oder APIs von bereitgestellten Chromeriver Bereitstellen von Azure Active Directory-Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-chromeriver-perform-the-following-steps"></a>Zuweisen von Benutzern zu Chromeriver führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Chromeriver **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-chromeriver-tutorial/IC802762.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-chromeriver-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
