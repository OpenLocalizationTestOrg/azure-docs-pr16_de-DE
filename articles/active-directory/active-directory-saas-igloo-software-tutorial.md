<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Igloo Software | Microsoft Azure" 
    description="Erfahren Sie, wie zur Verwendung von Igloo Software mit Azure Active Directory aktivieren einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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
    ms.date="10/20/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Lernprogramm: Azure-Active Directory-Integration in Igloo-Software
  
Ziel dieses Lernprogramms ist die Integration von Azure und Igloo Software angezeigt.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein [Igloo Software](http://www.igloosoftware.com/) für einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Igloo Software zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Igloo Software (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Igloo-Software
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-igloo-software-tutorial/IC783961.png "Szenario")
##<a name="enabling-the-application-integration-for-igloo-software"></a>Aktivieren die Anwendungsintegration für Igloo-Software
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Igloo Software aktiviert.

###<a name="to-enable-the-application-integration-for-igloo-software-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für Igloo Software die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-igloo-software-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-igloo-software-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-igloo-software-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-igloo-software-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Igloo Software**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-igloo-software-tutorial/IC783962.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Igloo-Software**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Igloo] (./media/active-directory-saas-igloo-software-tutorial/IC783963.png "Igloo")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren Igloo Software-mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikat in Ihrem Mandanten zentralen Desktop hochladen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite Anwendung Integration **Igloo Software** klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-igloo-software-tutorial/IC783964.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Igloo Software auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Microsoft Azure AD einmaliges Anmelden] (./media/active-directory-saas-igloo-software-tutorial/IC783965.png "Microsoft Azure AD einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Igloo Software melden Sie sich In URL** ein Ihre URL dem folgenden Muster "*https://company.igloocommunities.com/?signin*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-igloo-software-tutorial/IC773625.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Igloo Software** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-igloo-software-tutorial/IC783966.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Igloo Software als Administrator.

6.  Wechseln Sie zu der **Systemsteuerung**.

    ![Systemsteuerung] (./media/active-directory-saas-igloo-software-tutorial/IC799949.png "Systemsteuerung")

7.  Klicken Sie auf der Registerkarte **Mitgliedschaft** auf **Sign In Einstellungen**.

    ![Melden Sie sich in den Einstellungen] (./media/active-directory-saas-igloo-software-tutorial/IC783968.png "Melden Sie sich in den Einstellungen")

8.  Klicken Sie im Abschnitt SAML-Konfiguration auf **SAML-Authentifizierung konfigurieren**.

    ![SAML-Konfiguration] (./media/active-directory-saas-igloo-software-tutorial/IC783969.png "SAML-Konfiguration")

9.  Führen Sie im Abschnitt **Allgemeine Konfiguration** der folgenden Schritte aus:

    ![Allgemeine Konfiguration] (./media/active-directory-saas-igloo-software-tutorial/IC783970.png "Allgemeine Konfiguration")

    1.  Geben Sie im Textfeld **Name der Verbindung** einen benutzerdefinierten Namen für die Konfiguration aus.
    2.  Kopieren Sie des Werts **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **IdP Anmelde-URL** , im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden bei Igloo Software** .
    3.  Im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden bei Igloo Software** kopieren Sie des Werts **Remote Abmeldung-URL** , und klicken Sie dann in das Textfeld **IdP Abmeldung URL** einzufügen.
    4.  Wählen Sie als **Antwort abmelden und HTTP-Typ anfordern** **Bereitstellen**.
    5.  Erstellen Sie eine Textdatei aus dem heruntergeladene Zertifikat.
        
        >[AZURE.TIP]Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    6.  Entfernen Sie der ersten Zeile und der letzten Zeile aus der Text Dateiversion des Zertifikats, kopieren Sie Zertifikat verbliebenen Text und fügen Sie ihn in das Textfeld **Öffentlichen Zertifikat** .

10. Führen Sie in der **Antwort und Authentifizierungskonfiguration**die folgenden Schritte aus:

    ![Antwort und Authentifizierungskonfiguration] (./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Antwort und Authentifizierungskonfiguration")

    1.  Wählen Sie als **Identitätsanbieter** **Microsoft ADFS**aus.
    2.  Wählen Sie als **Typ ID** **E-Mail-Adresse**ein.
    3.  Geben Sie in das Textfeld **E-Mail-Attribut** **Emailaddress**aus.
    4.  Geben Sie im Textfeld **Vorname Attribut** **Vorname**ein.
    5.  Geben Sie in das Textfeld **Last Name-Attribut** **Nachnamen**aus.

11. Durchführen von die folgenden Schritte aus, um die Konfiguration abzuschließen:

    ![Klicken Sie auf Sign in die Erstellung des Benutzers] (./media/active-directory-saas-igloo-software-tutorial/IC783972.png "Klicken Sie auf Sign in die Erstellung des Benutzers")

    1.  Wählen Sie als **die Erstellung des Benutzers anmelden** **Erstellen Sie einen neuen Benutzer in Ihrer Website, wenn sie sich anmelden**.
    2.  Wählen Sie unter **Anmelden auf Einstellungen** **verwenden SAML Schaltfläche Bildschirm "Anmelden"**aus.
    3.  Klicken Sie auf **Speichern**.

12. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-igloo-software-tutorial/IC783973.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Es gibt keine Aktionselement für Ihre Benutzer bereitgestellt Igloo Software-konfigurieren.  
Wenn ein zugewiesene Benutzer zur Anmeldung bei Igloo Software mithilfe des Bedienfelds Access versucht, überprüft Igloo Software an, ob der Benutzer vorhanden ist.  
Wenn es noch kein Benutzerkonto verfügbar ist, wird es automatisch von Igloo Software erstellt.
##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Benutzer Azure AD-erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-igloo-software-perform-the-following-steps"></a>Zuweisen von Benutzern zu Igloo Software führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite Anwendung Integration **Igloo Software **auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-igloo-software-tutorial/IC783974.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-igloo-software-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).