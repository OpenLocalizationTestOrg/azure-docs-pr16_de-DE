<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Onit | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Onit mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-onit"></a>Lernprogramm: Azure-Active Directory-Integration in Onit
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Onit anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Onit einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Onit zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Onit (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Onit
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-onit-tutorial/IC791166.png "Szenario")
##<a name="enabling-the-application-integration-for-onit"></a>Aktivieren die Anwendungsintegration für Onit
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Onit aktiviert.

###<a name="to-enable-the-application-integration-for-onit-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Onit aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-onit-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-onit-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-onit-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-onit-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Onit**.

    ![Katalog der Anwendung] (./media/active-directory-saas-onit-tutorial/IC791167.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Onit aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Onit] (./media/active-directory-saas-onit-tutorial/IC795325.png "Onit")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Onit mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für Onit konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).
  
Die Onit-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format, das benutzerdefinierte Attribut Zuordnungen an der Konfiguration **token Saml-Attribute** hinzufügen erforderlich ist.  
Das folgende Bildschirmabbild zeigt ein Beispiel dafür.

![Einmaliges Anmelden] (./media/active-directory-saas-onit-tutorial/IC791168.png "Einmaliges Anmelden")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Klicken Sie im Azure klassischen Portal **Onit** Anwendung Integration in die Seite, das Menü im oberen Bereich auf **Attributen** , um das Dialogfeld **Token SAML-Attribute** zu öffnen.

    ![Attribute] (./media/active-directory-saas-onit-tutorial/IC791169.png "Attribute")

2.  Um die Zuordnungen erforderliches Attribut hinzufügen möchten, führen Sie die folgenden Schritte aus:

    
  	|Attributname|Attributwert|
  	|---|---|
  	|Namen|User.userPrincipalName|
  	|E-Mail|User.Mail|

    1.  Klicken Sie für jede Datenzeile von in der Tabelle oben auf **Benutzerattribut hinzufügen**.
    2.  Geben Sie in das Textfeld **Name Attribut** das Attribut Bankname für diese Zeile ein.
    3.  Wählen Sie aus der Liste **Attributwert** für diese Zeile angezeigten Attributwert ein.
    4.  Klicken Sie auf **abgeschlossen**.

3.  Klicken Sie auf **Änderungen anzuwenden**.

4.  Klicken Sie in Ihrem Browser auf **zurück** , wenn Sie das Dialogfeld **Schnellstart** wieder öffnen.

5.  Klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-onit-tutorial/IC791170.png "Konfigurieren Sie einmaliges Anmelden")

6.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Onit auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-onit-tutorial/IC791171.png "Konfigurieren Sie einmaliges Anmelden")

7.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Onit melden Sie sich auf URL** die URL, die von den Benutzern verwendet, um melden Sie sich an Ihrer Anwendung Onit auf (z. B.: "*https://ms-sso-test.onit.com*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-onit-tutorial/IC791172.png "Konfigurieren der App-URL")

8.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Onit** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-onit-tutorial/IC791173.png "Konfigurieren Sie einmaliges Anmelden")

9.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Onit als Administrator.

10. Klicken Sie im Menü oben auf **Verwaltung**.

    ![Verwaltung] (./media/active-directory-saas-onit-tutorial/IC791174.png "Verwaltung")

11. Klicken Sie auf **Edit Unternehmen**.

    ![Bearbeiten von Unternehmen] (./media/active-directory-saas-onit-tutorial/IC791175.png "Bearbeiten von Unternehmen")

12. Klicken Sie auf der Registerkarte **Sicherheit** .

    ![Unternehmensinformationen bearbeiten] (./media/active-directory-saas-onit-tutorial/IC791176.png "Unternehmensinformationen bearbeiten")

13. Führen Sie auf der Registerkarte **Sicherheit** die folgenden Schritte aus:

    ![Einmaliges Anmelden] (./media/active-directory-saas-onit-tutorial/IC791177.png "Einmaliges Anmelden")

    1.  Wählen Sie als **Authentication Strategy** **Single Sign On und das Kennwort**ein.
    2.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Idp Ziel-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Onit** .
    3.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Onit** kopieren Sie des Werts **Remote Abmeldung-URL** , und klicken Sie dann in das Textfeld **Idp Abmeldung URL** einzufügen.
    4.  Kopieren Sie den **Fingerabdruck** Wert aus dem exportierte Zertifikat, und fügen Sie ihn in das Textfeld **Idp Zertifikat Fingerabdruck (SHA1)** .  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

    5.  Wählen Sie als **Typ SSO** **SAML**aus.
    6.  Geben Sie einen Schaltflächentext, den Sie in das Textfeld **SSO Login Schaltflächentext** .
    7.  Wählen Sie **Melden Sie sich mit SSO: für die folgenden Domänen-Benutzer erforderlich**, geben Sie die e-Mail-Adresse eines Benutzers nachschlagen Test in der verknüpften Textfeld, und klicken Sie dann auf **Aktualisieren**.
        ![Bearbeiten von Unternehmen] (./media/active-directory-saas-onit-tutorial/IC791178.png "Bearbeiten von Unternehmen")

14. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-onit-tutorial/IC791179.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Onit zu ermöglichen, müssen er in Onit bereitgestellt werden.  
Im Falle von Onit ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator klicken Sie auf der Website Ihres Unternehmens **Onit** an.

2.  Klicken Sie auf **Benutzer hinzufügen**.

    ![Verwaltung] (./media/active-directory-saas-onit-tutorial/IC791180.png "Verwaltung")

3.  Führen Sie auf der Seite Dialogfeld **Benutzer hinzufügen** die folgenden Schritte aus:

    ![Fügen Sie Benutzer hinzu] (./media/active-directory-saas-onit-tutorial/IC791181.png "Fügen Sie Benutzer hinzu")

    1.  Geben Sie den **Namen** und die **E-Mail-Adresse** eines gültigen AAD-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Erstellen**.  

        >[AZURE.NOTE] Der Kontobesitzer erhalten eine e-Mail-Nachricht, einschließlich einen Link, um das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen Onit Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Onit bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-onit-perform-the-following-steps"></a>Zuweisen von Benutzern zu Onit führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Onit **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-onit-tutorial/IC791182.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-onit-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).