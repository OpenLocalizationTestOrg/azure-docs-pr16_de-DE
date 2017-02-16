<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Vertrieb Sandkasten-| Microsoft Azure"
    description="Informationen Sie zum Vertrieb Sandkasten-mit Azure Active Directory verwenden, um einmaliges Anmelden, automatisierte bereitgestellt und mehr zu ermöglichen!." 
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
    ms.date="10/28/2016" 
    ms.author="jeedes" />


#<a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Lernprogramm: Azure-Active Directory-Integration in Vertrieb Sandkastenmodus
>[AZURE.TIP]Feedback, finden Sie [hier](http://go.microsoft.com/fwlink/?LinkId=521878).
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Vertrieb Sandkasten-anzeigen.  
Sandkästen bieten Ihnen die Möglichkeit, mehrere Kopien Ihrer Organisation in separaten Umgebungen für verschiedene Zwecke, wie z. B. Entwicklung, Test und Schulung, ohne dass Sie die Daten und Anwendungen in Ihrer Organisation der Vertrieb Herstellung beeinträchtigt zu erstellen.  
Weitere Informationen hierzu finden Sie unter [Übersicht der Sandkastenmodus](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)
  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eines Sandkastens in Salesforce.com
  
Wenn Sie noch nicht über eine gültige Sandkastenmodus in Salesforce.com, müssen Sie Vertrieb wenden Sie sich an.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Vertrieb Sandkastenmodus
2.  Konfigurieren einmaliges Anmelden
3.  Aktivieren Ihre Domäne
4.  Konfigurieren der Benutzer bereitgestellt
5.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Szenario")
##<a name="enabling-the-application-integration-for-salesforce-sandbox"></a>Aktivieren die Anwendungsintegration für Vertrieb Sandkastenmodus
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Vertrieb Sandkastenmodus aktiviert.

###<a name="to-enable-the-application-integration-for-salesforce-sandbox-perform-the-following-steps"></a>Zum Aktivieren der Anwendungsintegration für Vertrieb Sandkasten-führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Applikationen")

4.  Um den **Katalog der Anwendung**zu öffnen, klicken Sie auf **Ein App hinzufügen**, und klicken Sie dann auf **Hinzufügen, eine Anwendung für meine Organisation verwenden**.

    ![Was möchten Sie tun?] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Was möchten Sie tun?")

5.  Geben Sie im **Suchfeld** **Vertrieb Sandkasten-**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Katalog der Anwendung")

6.  Klicken Sie im Ergebnisbereich wählen Sie **Vertrieb Sandkasten-**und dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Vertrieb Sandkastenmodus] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Vertrieb Sandkastenmodus")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Vertrieb mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Vertrieb Sandkasten-** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Sandkastenmodus Vertrieb auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Vertrieb Sandkastenmodus] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Vertrieb Sandkastenmodus")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Melden Sie sich auf URL** den URL dem folgenden Muster `http://company.my.salesforce.com`, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Konfigurieren der App-URL")

4. Wenn Sie bereits einmaliges Anmelden für ein anderes Vertrieb Sandkasten-Instanz in Ihrem Verzeichnis konfiguriert haben, müssen Sie auch den **Bezeichner** , um den gleichen Wert wie die **URL anmelden**müssen konfigurieren. Das Feld **ID** kann durch Aktivieren des Kontrollkästchens **Erweiterte Einstellungen anzeigen** auf der Seite **Konfigurieren der App-URL** Rand des Dialogfelds gefunden werden.

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden am Vertrieb Sandkasten-** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei Ihrem Vertrieb Sandkasten-als Administrator.

6.  Klicken Sie im Menü oben auf **Setup**.

    ![Setup] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Setup")

7.  Klicken Sie im Navigationsbereich auf der linken Seite klicken Sie auf **Sicherheit Steuerelemente**, und klicken Sie dann auf **Einstellungen für einzelne anmelden**.

    ![Einstellungen für einzelne Zeichen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Einstellungen für einzelne Zeichen")

8.  Führen Sie auf den Abschnitt Einstellungen für einzelne Zeichen die folgenden Schritte aus:

    ![Einstellungen für einzelne Zeichen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Einstellungen für einzelne Zeichen")

    ein.  Wählen Sie **SAML aktiviert**.
    
    b.  Klicken Sie auf **neu**.

9.  Führen Sie in dem Abschnitt SAML einzelnen anmelden Einstellungen die folgenden Schritte aus:

    ![SAML einzelnen anmelden Einstellungen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML einzelnen anmelden Einstellungen")

    ein.  Geben Sie in das Textfeld Name den Namen der Konfiguration (z. B.: *SPSSOWAAD\_Test*).
    
    b.  Im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden am Vertrieb Sandkasten-** kopieren Sie den Wert des **Herausgebers URL** , und fügen Sie ihn in das Textfeld **Herausgeber** .
    
    c.  Geben Sie in das Textfeld **Einheiten Id** **https://test.salesforce.com** ist dies die erste Instanz des Sandkastenmodus Vertrieb, die Sie zu Ihrem Verzeichnis hinzufügen. Wenn Sie bereits eine Instanz des Sandkastenmodus Vertrieb, und klicken Sie dann für den Typ der **Element-ID** in der **Melden Sie sich auf URL**, hinzugefügt haben das in diesem Format sein soll:`http://company.my.salesforce.com`
    
    d.  Klicken Sie auf **Durchsuchen** , um die heruntergeladene Zertifikat hochladen.
    
    e.  Wählen Sie als **Typ der SAML-Identität** **Assertion enthält die Föderation-ID aus dem Benutzer-Objekt**aus.
    
    f.  Wählen Sie als **Speicherort der SAML-Identität** **Identität befindet sich im NameIdentifier Element der Betreff-Anweisung**ein.
    
    g.  Im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden am Vertrieb Sandkasten-** kopieren Sie des Werts **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Identität Anbieter Anmelde-URL** .
    
    h.  SFDC unterstützt SAML-Abmeldung nicht.  Fügen Sie zur Umgehung dieses Problems, 'https://login.windows.net/common/wsfederation?wa=wsignout1.0' es in das Textfeld **Identität Anbieter Abmelden URL** .
    
    Ich.  Wählen Sie als **Service Provider initiiert Bindung anfordern** **HTTP-Beitrag**ein.
    
    j. Klicken Sie auf **Speichern**.

10. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Konfigurieren Sie einmaliges Anmelden")

##<a name="enabling-your-domain"></a>Aktivieren Ihre Domäne
  
In diesem Abschnitt wird davon ausgegangen, dass Sie bereits eine Domäne erstellt haben.  
Weitere Informationen hierzu finden Sie unter [Domänennamen definieren](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

###<a name="to-enable-your-domain-perform-the-following-steps"></a>Um Ihre Domäne zu aktivieren, führen Sie die folgenden Schritte aus:

1.  Klicken Sie auf **Domain Management**im linken Navigationsbereich, und klicken Sie dann auf **Meine Domäne.**

    ![Meiner Domäne] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "Meiner Domäne")

    >[AZURE.NOTE]Stellen Sie sicher, dass Ihre Domäne richtig konfiguriert ist.

2.  Im Abschnitt **Einstellungen der Seite Login** klicken Sie auf **Bearbeiten**, klicken Sie dann als **Authentifizierungsdienst**, wählen Sie den Namen der SAML einzelnen anmelden Einstellung aus dem vorherigen Abschnitt, und klicken Sie abschließend auf **Speichern**.

    ![Meiner Domäne] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "Meiner Domäne")
  
Sobald Sie eine Domäne konfiguriert haben, sollten die Benutzer die Domäne URL zum Anmelden bei der Sandkastenmodus Vertrieb verwenden.  
Um den Wert, der die URL zu gelangen, klicken Sie auf das SSO-Profil, das Sie im vorherigen Abschnitt erstellt haben.
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer Bereitstellung von Active Directory-Benutzerkonten zu Vertrieb Sandkastenmodus aktiviert.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Wählen Sie im Portal Vertrieb in der oberen Navigationsleiste Ihren Namen, um Ihre Benutzermenü zu erweitern:

    ![Meine Einstellungen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Meine Einstellungen")

2.  Wählen Sie Ihre Benutzer im Menü **Meine Einstellungen** , um Ihre **Eigene** Einstellungsseite zu öffnen.

3.  Im linken Bereich klicken Sie auf **Persönliche** um die persönliche Abschnitt zu erweitern, und klicken Sie dann auf **Zurücksetzen Meine Sicherheitstoken**:

    ![Meine Einstellungen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Meine Einstellungen")

4.  Klicken Sie auf der Seite **Zurücksetzen Meine Sicherheitstoken** auf **Sicherheitstoken zurücksetzen** um eine e-Mail-Nachricht anfordern, die Ihre Sicherheitstoken Salesforce.com enthält.

    ![Neues Token] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "Neues Token")

5.  Überprüfen Sie Ihren e-Mail-Posteingang für eine e-Mail von Salesforce.com mit "**salesforce.com.com Sicherheit Bestätigung**" als Betreff ein.

6.  Überprüfen Sie diese e-Mail-Adresse, und kopieren Sie den Sicherheit token Wert.

7.  Klicken Sie im Azure klassischen-Portal auf der Seite **Vertrieb Sandkastenmodus** Anwendung Integration auf **Konfigurieren Benutzer bereitgestellt** , um das Dialogfeld **Konfigurieren Benutzer bereitgestellt** öffnen.

    ![Konfigurieren Benutzer bereitgestellt] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Konfigurieren Benutzer bereitgestellt")

8.  Geben Sie auf der Seite **Geben Sie Ihre Anmeldeinformationen Vertrieb Sandkasten-zum Aktivieren der automatischen Benutzer bereitgestellt** die folgenden Einstellungen für die Konfiguration aus:

    ![Vertrieb Sandkastenmodus] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Vertrieb Sandkastenmodus")

    ein.  Geben Sie in das Textfeld **Vertrieb Sandkasten-Administratorbenutzernamen** einen Kontonamen Vertrieb Sandkasten-, der das **System** Administratorprofil in Salesforce.com zugewiesen hat.

    b.  Geben Sie in das Textfeld **Vertrieb Sandkasten-Administratorkennworts** das Kennwort für dieses Konto ein.

    c.  Fügen Sie im Textfeld **Benutzer Sicherheitstoken** des Sicherheit token Werts ein.

    d.  Klicken Sie auf **Überprüfen** , um zu überprüfen, ob Ihre Konfiguration.

    e.  Klicken Sie auf die Schaltfläche **Weiter** , um **die Bestätigungsseite** zu öffnen.

9.  Klicken Sie auf der Seite **Bestätigung** auf **abgeschlossen** , um die Konfiguration zu speichern.
##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-salesforce-sandbox-perform-the-following-steps"></a>Zuweisen von Benutzern zu Vertrieb Sandkasten-führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Vertrieb Sandkasten- **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Ja")
  
Sie sollten jetzt 10 Minuten warten, und stellen Sie sicher, dass das Konto zum Vertrieb Sandkasten-synchronisiert wurde.
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](https://msdn.microsoft.com/library/dn308586).
