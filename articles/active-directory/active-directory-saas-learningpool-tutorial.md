<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Learningpool | Microsoft Azure" 
    description="Erfahren Sie, wie Learningpool mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-learningpool"></a>Lernprogramm: Azure-Active Directory-Integration in Learningpool
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Learningpool anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Learningpool einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Learningpool zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Learningpool (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Learningpool
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-learningpool-tutorial/IC791166.png "Szenario")
##<a name="enabling-the-application-integration-for-learningpool"></a>Aktivieren die Anwendungsintegration für Learningpool
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Learningpool aktiviert.

###<a name="to-enable-the-application-integration-for-learningpool-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Learningpool aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-learningpool-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-learningpool-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-learningpool-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-learningpool-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Learningpool**.

    ![Katalog der Anwendung] (./media/active-directory-saas-learningpool-tutorial/IC795073.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Learningpool aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Learningpool] (./media/active-directory-saas-learningpool-tutorial/IC809577.png "Learningpool")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Learningpool mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.
  
Die Learningpool-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format, das benutzerdefinierte Attribut Zuordnungen an der Konfiguration **token Saml-Attribute** hinzufügen erforderlich ist.  
Das folgende Bildschirmabbild zeigt ein Beispiel dafür.

![Token SAML-Attribute] (./media/active-directory-saas-learningpool-tutorial/IC795074.png "Token SAML-Attribute")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Klicken Sie im Azure klassischen Portal **Learningpool** Anwendung Integration in die Seite, das Menü im oberen Bereich auf **Attributen** , um das Dialogfeld **Token SAML-Attribute** zu öffnen.

    ![Attribute] (./media/active-directory-saas-learningpool-tutorial/IC795075.png "Attribute")

2.  Um die Zuordnungen erforderliches Attribut hinzufügen möchten, führen Sie die folgenden Schritte aus:

    ###  

  	|Attributname                |Attributwert            |
  	|------------------------------|---------------------------|

     Urn: oid:1.2.840.113556.1.4.221 | User.userPrincipalName
  	|-------------------------------|--------------------------|  
     Urn: oid:2.5.4.42|User.GivenName   
  	|Urn: oid:0.9.2342.19200300.100.1.3|User.Mail
  	|Urn: oid:2.5.4.4|User.Surname

    1.  Klicken Sie für jede Datenzeile von in der Tabelle oben auf **Benutzerattribut hinzufügen**.
    2.  Geben Sie in das Textfeld **Name Attribut** das Attribut Bankname für diese Zeile ein.
    3.  Wählen Sie aus der Liste **Attributwert** für diese Zeile angezeigten Attributwert ein.
    4.  Klicken Sie auf **abgeschlossen**.

3.  Klicken Sie auf **Änderungen anzuwenden**.

4.  Klicken Sie in Ihrem Browser auf **zurück** , wenn Sie das Dialogfeld **Schnellstart** wieder öffnen.

5.  Klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren von Singel anmelden] (./media/active-directory-saas-learningpool-tutorial/IC795076.png "Konfigurieren von Singel anmelden")

6.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Learningpool auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-learningpool-tutorial/IC795077.png "Konfigurieren Sie einmaliges Anmelden")

7.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Learningpool melden Sie sich auf URL** die URL, die von den Benutzern verwendet, um melden Sie sich an Ihrer Anwendung Learningpool auf (z. B.: https://parliament.preview.learningpool.com/auth/shibboleth/index.php), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-learningpool-tutorial/IC795078.png "Konfigurieren der App-URL")

8.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Learningpool** zum Herunterladen der Metadaten Ihrer klicken Sie auf **Herunterladen von Metadaten**, und klicken Sie dann speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-learningpool-tutorial/IC795079.png "Konfigurieren Sie einmaliges Anmelden")

9.  Weiterleiten dieser Datei Metadaten an Ihr Supportteam für Learningpool an.

    >[AZURE.NOTE]Einmaliges Anmelden muss durch das Supportteam Learningpool aktiviert sein.

10. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-learningpool-tutorial/IC795080.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Learningpool zu ermöglichen, müssen er in Learningpool bereitgestellt werden.
  
Es gibt keine Aktionselement für Ihre Benutzer zu Learningpool provisioning konfigurieren.  
Benutzer müssen sich von Ihrem Supportteam Learningpool erstellt werden.

>[AZURE.NOTE]Alle anderen Learningpool Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Learningpool bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-learningpool-perform-the-following-steps"></a>Zuweisen von Benutzern zu Learningpool führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Learningpool **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-learningpool-tutorial/IC795081.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-learningpool-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).