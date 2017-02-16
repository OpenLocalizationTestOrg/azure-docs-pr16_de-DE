<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Clever | Microsoft Azure" 
    description="Erfahren Sie, wie Clever mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clever"></a>Lernprogramm: Azure-Active Directory-Integration in Clever

Das Ziel dieses Lernprogramms ist die Integration von Azure und Clever anzeigen. Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen raffinierten Mandanten

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Clever zugewiesen haben können einzelne melden Sie sich bei der Anwendung an Ihre raffinierten Firmenwebsite (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Clever
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-clever-tutorial/IC798977.png "Szenario")
##<a name="enabling-the-application-integration-for-clever"></a>Aktivieren die Anwendungsintegration für Clever

So aktivieren Sie die Anwendungsintegration für Clever Gliedern ist das Ziel in diesem Abschnitt.

###<a name="to-enable-the-application-integration-for-clever-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Clever aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clever-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-clever-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-clever-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-clever-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Clever**.

    ![Katalog der Anwendung] (./media/active-directory-saas-clever-tutorial/IC798978.png "Katalog der Anwendung")

7.  Im Ergebnisbereich **Clever**wählen Sie aus, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Intelligente] (./media/active-directory-saas-clever-tutorial/IC798979.png "Intelligente")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Clever mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Die intelligente Anwendung erwartet die SAML-Assertionen in einem bestimmten Format, das benutzerdefinierte Attribut Zuordnungen an der Konfiguration **token Saml-Attribute** hinzufügen erforderlich ist.  
Das folgende Bildschirmabbild zeigt ein Beispiel dafür.

![Attribute] (./media/active-directory-saas-clever-tutorial/IC798980.png "Attribute")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Clever** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-clever-tutorial/IC784682.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer Clever bei, klicken Sie auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-clever-tutorial/IC798981.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Raffinierten melden Sie sich auf URL** die URL, die von den Benutzern die Anmeldung an Ihrer Anwendung raffinierten verwendet (z. B.: *https://clever.com/in/azsandbox*), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-clever-tutorial/IC798982.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Clever** zum Herunterladen der Metadaten Ihrer klicken Sie auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-clever-tutorial/IC798983.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens raffinierten als Administrator.

6.  Klicken Sie in der Symbolleiste auf **Chat anmelden**.

    ![Chat anmelden] (./media/active-directory-saas-clever-tutorial/IC798984.png "Chat anmelden")

7.  Klicken Sie auf der Seite **Login Instant** führen Sie die folgenden Schritte aus:

    ![Chat anmelden] (./media/active-directory-saas-clever-tutorial/IC798985.png "Chat anmelden")

    1.  Geben Sie die **Anmelde-URL**ein.  

        >[AZURE.NOTE] Die **Anmelde-URL** ist ein benutzerdefinierter Wert.
Sie können den tatsächlichen Wert von Ihrem Supportteam raffinierten gelangen.

    2.  Wählen Sie als **Identität System** **ADFS**aus.
    3.  Klicken Sie auf **Speichern**.

8.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-clever-tutorial/IC798986.png "Konfigurieren Sie einmaliges Anmelden")

9.  Klicken Sie im Menü oben auf **Attributen** , um das Dialogfeld **Token SAML-Attribute** zu öffnen.

    ![Attribute] (./media/active-directory-saas-clever-tutorial/IC795920.png "Attribute")

10. Um die Zuordnungen erforderliches Attribut hinzufügen möchten, führen Sie die folgenden Schritte aus:

    ![token SAML-Attribute] (./media/active-directory-saas-clever-tutorial/IC795921.png "token SAML-Attribute")

  	|Attributname|Attributwert|
  	|---|---|
  	|clever.Student.Credentials.District\_Benutzername|User.userPrincipalName|

    1.  Klicken Sie für jede Datenzeile von in der Tabelle oben auf **Benutzerattribut hinzufügen**.
    2.  Geben Sie in das Textfeld **Name Attribut** das Attribut Bankname für diese Zeile ein.
    3.  Wählen Sie in das Textfeld **Wert Attribut** für diese Zeile angezeigten Attributwert ein.
    4.  Klicken Sie auf **abgeschlossen**.

11. Klicken Sie auf **Änderungen anzuwenden**.

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzer zur Anmeldung bei Clever aktivieren zu können, müssen er in Clever bereitgestellt werden.  
Wenn Clever ist die Bereitstellung eine manuelle Aufgabe, die von Ihrem Supportteam raffinierten ausgeführt werden soll.

>[AZURE.NOTE] Alle anderen Benutzer raffinierten Konto Creation Tools können oder APIs bereitgestellt durch Clever bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-clever-perform-the-following-steps"></a>Zuweisen von Benutzern zu Clever führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Clever **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-clever-tutorial/IC798987.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-clever-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
