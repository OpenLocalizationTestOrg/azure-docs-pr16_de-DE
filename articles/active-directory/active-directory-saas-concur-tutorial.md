<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Concur | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Concur mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-concur"></a>Lernprogramm: Azure-Active Directory-Integration in Concur  


Das Ziel dieses Lernprogramms ist die Integration von Azure und Concur anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten in Concur

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Concur
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-concur-tutorial/IC769766.png "Szenario")

>[AZURE.NOTE] Die Konfiguration Ihres Abonnements Concur für partnerverbundkontakte SSO über SAML ist eine separate Aufgabe Sie Concur ausführen müssen Sie sich an.

##<a name="enabling-the-application-integration-for-concur"></a>Aktivieren die Anwendungsintegration für Concur

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Concur aktiviert.

###<a name="to-enable-the-application-integration-for-concur-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Concur aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-concur-tutorial/IC700993.png "Active Directory")

2.  Aus der Liste **Verzeichnis** auswählen möchten, dass das Verzeichnis für die Verzeichnisintegration aktivieren.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-concur-tutorial/IC700994.png "Applikationen")

4.  Um den **Katalog der Anwendung**zu öffnen, klicken Sie auf **Ein App hinzufügen**, und klicken Sie dann auf **Hinzufügen, eine Anwendung für meine Organisation verwenden**.

    ![Was möchten Sie tun?] (./media/active-directory-saas-concur-tutorial/IC700995.png "Was möchten Sie tun?")

5.  Geben Sie im **Suchfeld** **Concur**.

    ![Katalog der Anwendung] (./media/active-directory-saas-concur-tutorial/IC721727.png "Katalog der Anwendung")

6.  Wählen Sie im Ergebnisbereich **Concur aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Damit übereinstimmen] (./media/active-directory-saas-concur-tutorial/IC721728.png "Damit übereinstimmen")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Concur mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

>[AZURE.NOTE] Die Konfiguration Ihres Abonnements Concur für partnerverbundkontakte SSO über SAML ist eine separate Aufgabe Sie Concur ausführen müssen Sie sich an.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Concur **Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-concur-tutorial/IC769767.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Concur auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-concur-tutorial/IC769768.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Melden Sie sich In URL überein** Ihre Concur Mandanten anmelden URL ein, und klicken Sie dann auf **Weiter**: 

    ![Konfigurieren melden Sie sich URL] (./media/active-directory-saas-concur-tutorial/IC769769.png "Konfigurieren melden Sie sich URL")

4.  Führen Sie die folgenden Schritte aus, auf der Seite **Konfigurieren einmaliges Anmelden bei Concur** .

    ![Konfigurieren melden Sie sich URL] (./media/active-directory-saas-concur-tutorial/IC769770.png "Konfigurieren melden Sie sich URL")

    1.  Klicken Sie auf herunterladen, die Metadaten, und klicken Sie dann SFI der Datendatei auf Ihren Computer.
    2.  Wenden Sie sich an das Supportteam Concur um SSO für Ihren Mandanten zu konfigurieren.
    3.  Wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.  

    >[AZURE.NOTE] Die Konfiguration Ihres Abonnements Concur für partnerverbundkontakte SSO über SAML ist eine separate Aufgabe Sie Concur ausführen müssen Sie sich an.

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

In diesem Abschnitt Ziel ist es zu gliedernden zum Aktivieren von Active Directory-Benutzerkonten zu Concur bereitgestellt.

Zum Aktivieren von apps in des Diensts, es müssen ordnungsgemäßen Einrichten und Verwenden eines Profils Web Dienstadministrator sein. Fügen Sie nicht einfach die Rolle des Administrators WS zu Ihrem vorhandenen Administratorprofil, das Sie für T & E Verwaltungsfunktionen verwenden hinzu.

Berater überein oder der Client-Administrator muss ein distinct Web Dienstadministrator Profil erstellen und der Client-Administrator muss dieses Profil verwenden, für das Web Services Administrator-Funktionen (z. B. apps aktivieren). Diese Profile getrennt von der Client tägliche T & E Administrator Administratorprofil aufzubewahren (das Admin-Profil T & E sollte nicht die Rolle WSAdmin zugewiesen haben).

Beim Erstellen des Profils für die Aktivierung der app zu verwendende Geben Sie in die Felder des Benutzerprofils Administratornamen im Client. Dadurch wird das Profil Besitz zuweisen. Nachdem die Profile erstellt wurde, muss der Client mit dieses Profil auf die Schaltfläche "*Aktivieren*" für eine App Partner innerhalb des Menüs Webdienste melden.

Aus den folgenden Gründen sollte diese Aktion nicht mit dem Profil vorgenommen werden, die sie für normale T & E Administration verwenden.

1.  Der Client hat sein, das "*Ja*" klickt klicken Sie im Dialogfeld, das angezeigt wird, nachdem Sie eine app aktiviert ist. Klicken Sie auf diese bestätigt, dass der Client für die Anwendung Partner auf ihre Daten zugreifen, damit Sie oder des Partners auf die Schaltfläche ' Ja ' klicken kann bereit ist.
2.  Wenn ein Administrator Clients, der eine app aktiviert hat der Administrator T & E mit Profil das Unternehmen verlässt (resultierender im Profil deaktiviert wird), alle apps verwenden, Profil nicht funktionieren, bis die app mit einem anderen aktiven WS Admin-Profil aktiviert ist aktiviert. Dies ist, warum Sie zum Erstellen von Profilen distinct WS Administrator überfüllt werden.
3.  Wenn ein Administrator das Unternehmen verlassen hat, kann der Name, das Profil WS Administrator zugeordnet ist werden an den Administrator Ersatz geändert gegebenenfalls ohne zu beeinträchtigen, dass die app aktivierte, da dieses Profil nicht muss deaktiviert

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **Concur** an.

2.  Wählen Sie im Menü **Verwaltung** **Webdienste**aus.

    ![Concur Mandanten] (./media/active-directory-saas-concur-tutorial/IC721729.png "Concur Mandanten")

3.  Wählen Sie auf der linken Seite aus dem Bereich **Webdienste** **Partner-Anwendung zu aktivieren**.

    ![Aktivieren Sie die Anwendung Partner] (./media/active-directory-saas-concur-tutorial/IC721730.png "Aktivieren Sie die Anwendung Partner")

4.  Wählen Sie in der Liste **Aktivieren Anwendung** **Azure Active Directory aus**, und klicken Sie dann auf **Aktivieren**.

    ![Microsoft Azure-Active Directory] (./media/active-directory-saas-concur-tutorial/IC721731.png "Microsoft Azure-Active Directory")

5.  Klicken Sie auf **Ja,** um das Dialogfeld **Aktion bestätigen** schließen.

    ![Bestätigen Sie die Aktion] (./media/active-directory-saas-concur-tutorial/IC721732.png "Bestätigen Sie die Aktion")

6.  Wählen Sie im Portal Azure klassischen **Concur** aus der Anwendungsliste auf die Seite **Concur** Dialogfeld zu öffnen.

7.  Klicken Sie zum Öffnen der Dialogseite **Konfigurieren Benutzer bereitgestellt** auf **Konfigurieren Benutzer bereitgestellt**.

8.  Geben Sie den Benutzernamen und das Kennwort Ihres Concur-Administrators, und klicken Sie dann auf **Weiter**.

9.  Klicken Sie zum Abschluss dieser Konfiguration, klicken Sie auf der Seite **Bestätigung** auf die Schaltfläche " **abgeschlossen** ".

Sie können jetzt erstellen ein Testkonto, 10 Minuten warten, und stellen Sie sicher, dass das Konto mit Concur synchronisiert wurde.
##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-concur-perform-the-following-steps"></a>Zuweisen von Benutzern zu Concur führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Concur **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-concur-tutorial/IC769771.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-concur-tutorial/IC767830.png "Ja")

Sie sollten jetzt 10 Minuten warten, und stellen Sie sicher, dass das Konto mit Concur synchronisiert wurde.

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
