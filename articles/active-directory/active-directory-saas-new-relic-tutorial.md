<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration mit neuen Relic | Microsoft Azure" 
    description="Erfahren Sie, wie mit neuen Relic mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktiviert!" 
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

#<a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Lernprogramm: Azure-Active Directory-Integration mit neuen Relic
  
Ziel dieses Lernprogramms ist zum Einrichten einmaliges Anmelden zwischen Azure Active Directory und neue Relic veranschaulichen.
  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Eine neue Relic einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, ist die Azure Active Directory-Benutzer, die Sie neue Relic zugewiesen haben einmaliges Anmelden mithilfe des Access-Bedienfelds AAD möglich.

1.  Aktivieren die Anwendungsintegration für neue Relic
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-new-relic-tutorial/IC797030.png "Szenario")
##<a name="enabling-the-application-integration-for-new-relic"></a>Aktivieren die Anwendungsintegration für neue Relic
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für neue Relic aktiviert.

###<a name="to-enable-the-application-integration-for-new-relic-perform-the-following-steps"></a>Wenn die Anwendungsintegration für neue Relic aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-new-relic-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-new-relic-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-new-relic-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-new-relic-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Neue Relic**ein.

    ![Katalog der Anwendung] (./media/active-directory-saas-new-relic-tutorial/IC797031.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Neue Relic**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Neue Relic] (./media/active-directory-saas-new-relic-tutorial/IC797032.png "Neue Relic")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
In diesem Abschnitt wird beschrieben, wie Benutzer für neue Relic mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden authentifizieren aktivieren wird.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren Single Sign On** , im Azure klassischen-Portal auf der Seite **Neue Relic** Anwendung Integration.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-new-relic-tutorial/IC769534.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der neuen Relic auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-new-relic-tutorial/IC797033.png "Konfigurieren Sie einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Neue Relic melden Sie sich auf URL** Geben Sie die URL, die von den Benutzern verwendet, um melden Sie sich an Ihrer Anwendung neue Relic auf, und klicken Sie dann auf **Weiter**. 

    Die app-URL ist die URL Ihrer neuen Relic Mandanten (z. B.: *https://rpm.newrelic.com*):

    ![Konfigurieren der App-URL] (./media/active-directory-saas-new-relic-tutorial/IC797034.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei neuen Relic** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-new-relic-tutorial/IC797035.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich auf der Website Ihres Unternehmens **Neue Relic** als Administrator.

6.  Klicken Sie auf **Konten-Einstellungen**, klicken Sie im Menü oben.

    ![Kontoeinstellungen] (./media/active-directory-saas-new-relic-tutorial/IC797036.png "Kontoeinstellungen")

7.  Klicken Sie auf der Registerkarte **Sicherheit und Authentifizierung** , und klicken Sie dann auf die Registerkarte **für einmaliges Anmelden** .

    ![Einmaliges Anmelden] (./media/active-directory-saas-new-relic-tutorial/IC797037.png "Einmaliges Anmelden")

8.  Klicken Sie auf der Seite SAML Dialogfeld führen Sie die folgenden Schritte aus:

    ![SAML] (./media/active-directory-saas-new-relic-tutorial/IC797038.png "SAML")

    1.  Klicken Sie auf **Datei auswählen** , um Ihre heruntergeladenen Azure Active Directory-Zertifikat zu laden.
    2.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Remote Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei neuen Relic** .
    3.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei neuen Relic** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **URL Start Abmelden** .
    4.  Klicken Sie auf **Änderungen speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-new-relic-tutorial/IC797039.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure Active Directory-Benutzern zur Anmeldung bei neuen Relic zu ermöglichen, müssen er in der neuen Relic bereitgestellt werden.  
Wenn neue Relic ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-account-to-new-relic-perform-the-following-steps"></a>Um ein Benutzerkonto zu neuen Relic bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer **Neuen Relic** Firmenwebsite an.

2.  Klicken Sie auf **Konten-Einstellungen**, klicken Sie im Menü oben.

    ![Kontoeinstellungen] (./media/active-directory-saas-new-relic-tutorial/IC797040.png "Kontoeinstellungen")

3.  Klicken Sie im Bereich **Konto** auf der linken Seite klicken Sie auf **Zusammenfassung**, und klicken Sie dann auf **Benutzer hinzufügen**.

    ![Kontoeinstellungen] (./media/active-directory-saas-new-relic-tutorial/IC797041.png "Kontoeinstellungen")

4.  Führen Sie die folgenden Schritte aus, klicken Sie im Dialogfeld **aktive Benutzer** :

    ![Aktive Benutzer] (./media/active-directory-saas-new-relic-tutorial/IC797042.png "Aktive Benutzer")

    1.  Geben Sie in das Textfeld **-e-Mail** die e-Mail-Adresse eines gültigen Azure Active Directory-Benutzers, die Sie bereitstellen möchten.
    2.  Wählen Sie als **Rolle** **Benutzer**aus.
    3.  Klicken Sie auf **Hinzufügen, diesen Benutzer**.

>[AZURE.NOTE]Alle anderen neuen Relic Benutzer Konto Creation Tools können oder APIs, die neue Relic bereitstellen AAD Benutzerkonten bereitstellt.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-new-relic-perform-the-following-steps"></a>Zuweisen von Benutzern zu neuen Relic führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Neue Relic** Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-new-relic-tutorial/IC797043.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-new-relic-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).




