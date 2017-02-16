<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Citrix ShareFile | Microsoft Azure" 
    description="Erfahren Sie, wie Sie mit Citrix ShareFile mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktiviert!" 
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

#<a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Lernprogramm: Azure-Active Directory-Integration in Citrix ShareFile

Das Ziel dieses Lernprogramms ist die Integration von Azure und Citrix ShareFile anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Citrix ShareFile

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Citrix ShareFile zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Citrix ShareFile (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Citrix ShareFile
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773620.png "Szenario")
##<a name="enabling-the-application-integration-for-citrix-sharefile"></a>Aktivieren die Anwendungsintegration für Citrix ShareFile

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Citrix ShareFile aktiviert.

###<a name="to-enable-the-application-integration-for-citrix-sharefile-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Citrix ShareFile aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Citrix ShareFile**ein.

    ![Katalog der Anwendung] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773621.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Citrix ShareFile aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Citrix ShareFile] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773622.png "Citrix ShareFile")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Citrix ShareFile mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite Anwendung Integration **Citrix ShareFile** klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773623.png "Einmaliges Anmelden aktivieren")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei Citrix ShareFile, klicken Sie auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773624.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Citrix ShareFile melden Sie sich auf URL** den URL dem folgenden Muster `https://<tenant-name>.shareFile.com`, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773625.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Citrix ShareFile** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![ConfigureSingle einmaliges Anmelden] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773626.png "ConfigureSingle einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei Ihrem **Citrix ShareFile** Firmenwebsite als Administrator.

6.  Klicken Sie oben auf der Symbolleiste auf **Administrator**.

7.  Wählen Sie im linken Navigationsbereich **Konfigurieren einmaliges Anmelden**.

    ![Kontoverwaltung] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773627.png "Kontoverwaltung")

8.  Klicken Sie auf die **einmaliges Anmelden / SAML 2.0-Konfiguration** Dialogseite klicken Sie unter **Grundlegende Einstellungen**, gehen Sie folgendermaßen vor:

    ![Einmaliges Anmelden] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773628.png "Einmaliges Anmelden")

    1.  Klicken Sie auf **SAML aktivieren**.
    2.  Kopieren Sie die Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Citrix ShareFile** den **Element-ID** -Wert, und fügen Sie ihn in die **Your IDP Herausgeber / Einheiten ID** Textfeld.
    3.  Kopieren Sie den Wert **Remote Anmelde-URL** im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Citrix ShareFile** , und fügen Sie ihn in das Textfeld **Anmelde-URL** .
    4.  Im Azure klassischen-Portal auf **das Konfigurieren einmaliges Anmelden bei Citrix ShareFile** Dialogseite kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **URL Abmelden** .
    5.  Klicken Sie neben dem Feld **X 509-Zertifikat** auf **Ändern** , und Laden Sie das Zertifikat, das Sie von der klassischen Azure AD-Portal heruntergeladen haben.
        ![Grundlegende Einstellungen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773629.png "Grundlegende Einstellungen")

9.  Klicken Sie auf die Citrix ShareFile Verwaltungsportal auf **Speichern** .

10. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773630.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzern zur Anmeldung bei Citrix ShareFile zu ermöglichen, müssen er in Citrix ShareFile bereitgestellt werden.  
Im Falle von Citrix ShareFile ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem **Citrix ShareFile** Mandanten.

2.  Klicken Sie auf **Verwalten von Benutzern \> Verwalten von Benutzern Start \> + Mitarbeiter erstellen**.

    ![Erstellen der Mitarbeiter] (./media/active-directory-saas-citrix-sharefile-tutorial/IC781050.png "Erstellen der Mitarbeiter")

3.  Geben Sie die **E-Mail**, **vor-** und **Nachname** , der eine gültige Azure AD-Konto löschen, die Sie bereitstellen möchten.

    ![Grundlegende Informationen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC799951.png "Grundlegende Informationen")

4.  Klicken Sie auf **Benutzer hinzufügen**.

    >[AZURE.NOTE] Der Inhaber des AAD Konto wird erhalten eine e-Mail, und führen Sie einen Link, um ihr Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen Citrix ShareFile Benutzer Konto Creation Tools können oder APIs bereitgestellt von Citrix ShareFile bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-citrix-sharefile-perform-the-following-steps"></a>Zuweisen von Benutzern zu Citrix ShareFile führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Citrix ShareFile **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773631.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-citrix-sharefile-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
