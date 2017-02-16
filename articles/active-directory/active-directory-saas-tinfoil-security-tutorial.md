<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Folie Sicherheit | Microsoft Azure"
    description="Erfahren Sie, wie mit Sicherheit Folie mit Azure Active Directory einmaliges Anmelden, automatisierte bereitgestellt und mehr aktiviert!." 
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

#<a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Lernprogramm: Azure-Active Directory-Integration in Folie Sicherheit
  
Ziel dieses Lernprogramms ist die Integration von Azure und Sicherheit Folie angezeigt.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Folie Sicherheit einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie mit der Folie Sicherheit zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Folie Sicherheit Firmenwebsite (Identität Anbieter initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration Folie Wertpapiers zurück
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-tinfoil-security-tutorial/IC798965.png "Konfigurieren Sie einmaliges Anmelden")

##<a name="enabling-the-application-integration-for-tinfoil-security"></a>Aktivieren die Anwendungsintegration Folie Wertpapiers zurück
  
In diesem Abschnitt Ziel ist es zu gliedernden So aktivieren Sie die Anwendungsintegration Folie Wertpapiers zurück.

###<a name="to-enable-the-application-integration-for-tinfoil-security-perform-the-following-steps"></a>Zum Aktivieren der Anwendungsintegration Folie Wertpapiers führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-tinfoil-security-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-tinfoil-security-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-tinfoil-security-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-tinfoil-security-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Sicherheit Folie**ein.

    ![Katalog der Anwendung] (./media/active-directory-saas-tinfoil-security-tutorial/IC798966.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Folie Sicherheit aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Folie-Sicherheit] (./media/active-directory-saas-tinfoil-security-tutorial/IC802771.png "Folie-Sicherheit")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren Folie Sicherheit mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für Folie Sicherheit konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite der **Folie Sicherheit** Anwendung-Integration klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-tinfoil-security-tutorial/IC798967.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Folie Sicherheit auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-tinfoil-security-tutorial/IC798968.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Folie Sicherheits Antwort-URL** die URL Ihrer Folie Security Assertion Consumer Service (ACS) (z. B.: "*https://www.tinfoilsecurity.com/saml/consume*", und klicken Sie dann auf **Weiter**.

    >[AZURE.NOTE] Sie sollten keine Abrufen der ACS-URL aus Folie Sicherheitsmetadaten (https://www.tinfoilsecurity.com/saml/metadata).

    ![Konfigurieren der App-URL] (./media/active-directory-saas-tinfoil-security-tutorial/IC798969.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Folie Sicherheit** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und klicken Sie dann speichern Zertifikatsdatei lokal als **c:\\Folie Security.cer**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-tinfoil-security-tutorial/IC798970.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Folie Sicherheit als Administrator.

6.  Klicken Sie oben auf der Symbolleiste auf **My Account**.

    ![Dashboard] (./media/active-directory-saas-tinfoil-security-tutorial/IC798971.png "Dashboard")

7.  Klicken Sie auf **Sicherheit**.

    ![Sicherheit] (./media/active-directory-saas-tinfoil-security-tutorial/IC798972.png "Sicherheit")

8.  Führen Sie auf der Seite des Konfiguration **Einmaliges Anmelden** die folgenden Schritte aus:

    ![Einmaliges Anmelden] (./media/active-directory-saas-tinfoil-security-tutorial/IC798973.png "Einmaliges Anmelden")

    1.  Wählen Sie **SAML aktivieren**.
    2.  Klicken Sie auf **Manuelle Konfiguration**.
    3.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Sicherheit Folie** kopieren Sie des Werts **SAML SSO-URL** , und fügen Sie ihn in das Textfeld **URL SAML-ablegen** .
    4.  Kopieren Sie den **Fingerabdruck** Wert aus dem exportierte Zertifikat, und fügen Sie ihn in das Textfeld **SAML Zertifikat Fingerabdruck** .  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

    5.  Kopieren Sie **Ihre Konto-ID**an.
    6.  Klicken Sie auf **Speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-tinfoil-security-tutorial/IC798974.png "Konfigurieren Sie einmaliges Anmelden")

10. Klicken Sie im Menü oben auf **Attributen** , um das Dialogfeld **Token SAML-Attribute** zu öffnen.

    ![Attribute] (./media/active-directory-saas-tinfoil-security-tutorial/IC795920.png "Attribute")

11. Um die Zuordnungen erforderliches Attribut hinzufügen möchten, führen Sie die folgenden Schritte aus:

    ![Attribute] (./media/active-directory-saas-tinfoil-security-tutorial/IC798975.png "Attribute")

    1.  Klicken Sie auf **Benutzerattribut hinzufügen**.
    2.  Geben Sie in das Textfeld **Name Attribut** **Accountid**aus.
    3.  Fügen Sie das Textfeld **Attributwert** den Wert des Konto-ID, die, den Sie im vorherigen Abschnitt kopiert haben.
    4.  Klicken Sie auf **abgeschlossen**.

12. Klicken Sie auf **Änderungen anzuwenden**.

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Folie Sicherheit zu ermöglichen, müssen er in der Folie Sicherheit bereitgestellt werden.  
Wenn Folie Sicherheit ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-get-a-user-provisioned-perform-the-following-steps"></a>Wenn einen Benutzer nach der Bereitstellung erhalten möchten, führen Sie die folgenden Schritte aus:

1.  Wenn der Benutzer einen Teil eines Enterprise-Kontos ist, müssen Sie die Folie Sicherheit Supportteam um erstellten Benutzerkontos zu erhalten.

2.  Wenn der Benutzer eine normale Folie Sicherheit SaaS-Benutzer ist, kann der Benutzer einer Collaborator an allen Standorten des Benutzers hinzufügen. Dadurch wird die ein Prozesses, um eine Einladung an die angegebenen e-Mail zum Erstellen eines neuen Folie Sicherheit Benutzerkontos zu senden.

>[AZURE.NOTE] Alle anderen Folie Sicherheit Benutzer Konto Creation Tools können oder APIs zur Verfügung gestellt von Folie Security bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-tinfoil-security-perform-the-following-steps"></a>Zuweisen von Benutzern zu Folie Sicherheit führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite Anwendung Integration **Folie Sicherheit **auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-tinfoil-security-tutorial/IC798976.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-tinfoil-security-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).