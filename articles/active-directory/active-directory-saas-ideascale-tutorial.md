<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in IdeaScale | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von IdeaScale mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Lernprogramm: Azure-Active Directory-Integration in IdeaScale
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und IdeaScale anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine IdeaScale einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie IdeaScale zugewiesen haben einzelnen melden Sie sich bei der Anwendung, die die [Einführung in die Access-Bereich](active-directory-saas-access-panel-introduction.md)verwenden können.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für IdeaScale
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-ideascale-tutorial/IC790838.png "Szenario")
##<a name="enabling-the-application-integration-for-ideascale"></a>Aktivieren die Anwendungsintegration für IdeaScale
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für IdeaScale aktiviert.

###<a name="to-enable-the-application-integration-for-ideascale-perform-the-following-steps"></a>Wenn die Anwendungsintegration für IdeaScale aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-ideascale-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-ideascale-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-ideascale-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-ideascale-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **IdeaScale**.

    ![Katalog der Anwendung] (./media/active-directory-saas-ideascale-tutorial/IC790841.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **IdeaScale aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![IdeaScale] (./media/active-directory-saas-ideascale-tutorial/IC790842.png "IdeaScale")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu IdeaScale mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für IdeaScale konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **IdeaScale** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-ideascale-tutorial/IC790843.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der IdeaScale auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-ideascale-tutorial/IC790844.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **IdeaScale melden Sie sich auf URL** die URL, die von den Benutzern verwendet, um melden Sie sich an Ihrer Anwendung IdeaScale auf (z. B.: "*https://company.IdeaScale.com*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-ideascale-tutorial/IC790845.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei IdeaScale** zum Herunterladen der Metadaten Ihrer klicken Sie auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-ideascale-tutorial/IC790846.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens IdeaScale als Administrator.

6.  Wechseln Sie zu der **Communityeinstellungen**.

    ![Communityeinstellungen] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Communityeinstellungen")

7.  Wechseln Sie zu **Sicherheit \> Anmeldung Einstellungen einzelner**.

    ![Einzigen Anmeldung-Einstellungen] (./media/active-directory-saas-ideascale-tutorial/IC790848.png "Einzigen Anmeldung-Einstellungen")

8.  Wählen Sie als **Einmaliges Anmelden Typ** **SAML 2.0**ein.

    ![Typ der einzelnen Anmeldung] (./media/active-directory-saas-ideascale-tutorial/IC790849.png "Typ der einzelnen Anmeldung")

9.  Klicken Sie im Dialogfeld **Einstellungen für einzelne Anmeldung** führen Sie die folgenden Schritte aus:

    ![Einzigen Anmeldung-Einstellungen] (./media/active-directory-saas-ideascale-tutorial/IC790850.png "Einzigen Anmeldung-Einstellungen")

    1.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei IdeaScale** kopieren Sie den Wert der **Element-ID** , und fügen Sie ihn in das Textfeld **SAML IdP Einheiten ID** .
    2.  Kopieren Sie den Inhalt Ihrer Metadaten heruntergeladene Datei, und fügen Sie ihn in das Textfeld **SAML IdP Metadaten** .
    3.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei IdeaScale** kopieren Sie des Werts **Remote Abmeldung-URL** , und klicken Sie dann in das Textfeld **Abmeldung Erfolg URL** einzufügen.
    4.  Klicken Sie auf **Änderungen speichern**.

10. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-ideascale-tutorial/IC790851.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei IdeaScale zu ermöglichen, müssen er in IdeaScale bereitgestellt werden.  
Im Falle von IdeaScale ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **IdeaScale** an.

2.  Wechseln Sie zu der **Communityeinstellungen**.

    ![Communityeinstellungen] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Communityeinstellungen")

3.  Wechseln Sie zu **grundlegende Einstellungen \> Mitglied Management**.

4.  Klicken Sie auf **Element hinzufügen**.

    ![Mitglied Management] (./media/active-directory-saas-ideascale-tutorial/IC790852.png "Mitglied Management")

5.  Im Abschnitt neues Mitglied hinzuzufügen führen Sie die folgenden Schritte aus:

    ![Neues Mitglied hinzufügen] (./media/active-directory-saas-ideascale-tutorial/IC790853.png "Neues Mitglied hinzufügen")

    1.  Geben Sie in das Textfeld **E-Mail-Adressen** die e-Mail-Adresse eines gültigen AAD-Kontos ein, die Sie bereitstellen möchten.
    2.  Klicken Sie auf **Änderungen speichern**.

    >[AZURE.NOTE] Der Inhaber des Azure Active Directory-Konto erhalten eine e-Mail mit einem Link zu das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen IdeaScale Benutzer Konto Creation Tools können oder APIs bereitgestellt durch IdeaScale bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-ideascale-perform-the-following-steps"></a>Zuweisen von Benutzern zu IdeaScale führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **IdeaScale **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-ideascale-tutorial/IC790854.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-ideascale-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).