<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Feld | Microsoft Azure" 
    description="Erfahren Sie, wie das mit Azure Active Directory zu verwenden, um einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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




#<a name="tutorial-azure-active-directory-integration-with-box"></a>Lernprogramm: Azure-Active Directory-Integration in Feld


  
Ziel dieses Lernprogramms ist die Integration von Azure und im Feld angezeigt.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Im Dialogfeld einen Test-Mandanten
  
Am Ende dieses Lernprogramms, werden einzelne melden Sie sich bei der Anwendung an Ihre Feld Firmenwebsite (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md)können die Azure AD-Benutzer, die Sie im Feld zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Feld
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren von Benutzer- und Gruppe bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-box-tutorial/IC769537.png "Szenario")



##<a name="enabling-the-application-integration-for-box"></a>Aktivieren die Anwendungsintegration für Feld
  
In diesem Abschnitt Ziel ist es zu gliedernden zum Feld für die Integration der Anwendung zu aktivieren.

###<a name="to-enable-the-application-integration-for-box-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration Feld für die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-box-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-box-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-box-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-box-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie in das **Suchfeld** **ein**.

    ![Katalog der Anwendung] (./media/active-directory-saas-box-tutorial/IC701023.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **im Feld**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Feld] (./media/active-directory-saas-box-tutorial/IC701024.png "Feld")



##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer im Feld Authentifizierung mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert. Sie müssen als Teil dieses Verfahrens Metadaten zu Box.com hochladen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **im Feld** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-box-tutorial/IC769538.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Feld auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-box-tutorial/IC769539.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Im Mandanten URL** die URL Ihrer Feld Mandanten (z. B.: https://<mydomainname>. box.com), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der app-URL] (./media/active-directory-saas-box-tutorial/IC669826.png "Konfigurieren der app-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden Feld** zum Herunterladen der Metadaten Ihrer auf **Metadaten herunterladen**und dann auf die Datendatei lokal auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-box-tutorial/IC669824.png "Konfigurieren einmaliges Anmelden")

5.  Weiterleiten die im Feld Metadatendatei zum Supportteam. Das Support-Team muss konfiguriert einmaliges Anmelden für Sie.

6.  Wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-box-tutorial/IC769540.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Das Ziel der in diesem Abschnitt ist zum Aktivieren der Bereitstellung von Active Directory-Benutzerkonten im Feld Gliederungsebene.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1. Klicken Sie im Azure klassischen-Portal auf der Seite **im Feld** Anwendung Integration auf **Konfigurieren Benutzer bereitgestellt** , um das Dialogfeld **Konfigurieren Benutzer bereitgestellt** öffnen. 

    ![Aktivieren der automatischen Benutzer bereitgestellt] (./media/active-directory-saas-box-tutorial/IC769541.png "Aktivieren der automatischen Benutzer bereitgestellt")

2. Klicken Sie auf der Seite **Benutzer bereitgestellt, die im Feld aktivieren** auf **aktivieren Benutzer bereitgestellt**. 

    ![Aktivieren der automatischen Benutzer bereitgestellt] (./media/active-directory-saas-box-tutorial/IC769544.png "Aktivieren der automatischen Benutzer bereitgestellt")

3. Klicken Sie auf der Seite **Melden Sie sich im Feld Zugriff gewähren** Geben Sie die erforderlichen Anmeldeinformationen, und klicken Sie dann auf **Autorisieren**. 

    ![Aktivieren der automatischen Benutzer bereitgestellt] (./media/active-directory-saas-box-tutorial/IC769546.png "Aktivieren der automatischen Benutzer bereitgestellt")


4. Klicken Sie auf **Gewähren des Zugriffs auf das Feld** , um diesen Vorgang zu autorisieren und klassischen Azure-Portal zurück. 

    ![Aktivieren der automatischen Benutzer bereitgestellt] (./media/active-directory-saas-box-tutorial/IC769549.png "Aktivieren der automatischen Benutzer bereitgestellt")


5. Klicken Sie auf der Seite " **Bereitstellung"** zulassen die Kontrollkästchen **Objekttypen bereitstellen** , auszuwählen, und zwar unabhängig davon, ob Objekte gruppieren im Feld sowie Benutzerobjekte ein bereitgestellt werden.  Weitere Informationen finden Sie unter "Zuweisen von Benutzern und Gruppen im Abschnitt" unter.


6. Um die Konfiguration fertig sind, klicken Sie auf die Schaltfläche abgeschlossen. 

    ![Aktivieren der automatischen Benutzer bereitgestellt] (./media/active-directory-saas-box-tutorial/IC769551.png "Aktivieren der automatischen Benutzer bereitgestellt")



##<a name="assigning-a-test-user"></a>Zuweisen eines Testbenutzers
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-box-perform-the-following-steps"></a>Zuweisen von Benutzern zu Feld führen Sie die folgenden Schritte aus:

1. Erstellen Sie ein Testkonto im Portal Azure klassischen.

2. Klicken Sie auf der Seite **im Feld **Anwendung Integration auf **Benutzer zuweisen**. 

    ![Zuweisen von Benutzern] (./media/active-directory-saas-box-tutorial/IC769552.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen. 

    ![Ja] (./media/active-directory-saas-box-tutorial/IC767830.png "Ja")
  
Sie sollten jetzt 10 Minuten warten, und stellen Sie sicher, dass das Konto im Feld synchronisiert wurde.

Als ersten Überprüfungsschritt können Sie den Bereitstellung Status auf Dashboard in der D auf der Seite im Feld Anwendung Integration in Azure klassischen Portal prüfen.

![Dashboard] (./media/active-directory-saas-box-tutorial/IC769553.png "Dashboard")

Ein erfolgreich abgeschlossener Benutzer bereitgestellt Kreis wird durch einen verknüpften Status angezeigt:

![Integrationsstatus] (./media/active-directory-saas-box-tutorial/IC769555.png "Integrationsstatus")


In Ihrem Mandanten Feld sind synchronisierte Benutzer unter **Verwaltete Benutzer** in der **Verwaltungskonsole**aufgelistet.

![Integrationsstatus] (./media/active-directory-saas-box-tutorial/IC769556.png "Integrationsstatus")


##<a name="assigning-users-and-groups"></a>Zuweisen von Benutzern und Gruppen

Die **Feld > Benutzer und Gruppen** Registerkarte im klassischen Azure-Portal können Sie angeben, welche Benutzer und Gruppen Zugriff auf Feld erteilt werden soll. Zuweisen von Benutzern oder Gruppen bewirkt, dass die folgenden Elemente ausgeführt:

* Azure AD gestattet dem zugeordneten Benutzer (entweder durch direkte Zuordnung oder Gruppenmitgliedschaft) im Feld Authentifizierung an. Wenn ein Benutzer nicht zugeordnet ist, klicken Sie dann Azure AD lässt, damit das Feld anmelden und gibt einen Fehler zurück, auf der Anmeldeseite von Azure AD-.

* Kachel app für Feld wird des Benutzers [Anwendung Startprogramm für das](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)hinzugefügt.

* Wenn die automatische Bereitstellung aktiviert ist, werden die zugeordneten Benutzer und/oder Gruppen zur Bereitstellung Warteschlange automatisch bereitgestellt werden hinzugefügt.

    * Wenn nur Benutzerobjekte bereitgestellt werden konfiguriert wurden, klicken Sie dann alle direkt zugewiesen Benutzer in der Warteschlange provisioning platziert werden, und alle Benutzer, die Mitglieder alle zugeordneten Gruppen sind, werden in der Warteschlange provisioning platziert werden. 
    
    * Gruppieren von Objekten nach der Bereitstellung werden konfiguriert wurden, werden alle zugeordneten Gruppenobjekte bereitgestellt, in das Feld sowie alle Benutzer, die Mitglieder dieser Gruppen sind. Die Gruppen- und Mitgliedschaften werden beibehalten, in das Feld geschrieben wird.
    
Können die **Attribute > Single Sign On** Registerkarte so konfigurieren, welche Benutzerattribute (oder Ansprüche) bei SAML-basierten Authentifizierung, die im Feld angezeigt werden und die **Attribute > Provisioning** Registerkarte so konfigurieren, wie Benutzer- und Attributen aus Azure AD Feld Datenfluss während der Bereitstellung von Vorgängen. Weitere Informationen unter Ressourcen anzeigen.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Ausgestellt in das SAML-Token Ansprüche anpassen](active-directory-saml-claims-customization.md)
* [Bereitgestellt: Anpassen von Zuordnungen Attribut](active-directory-saas-customizing-attribute-mappings.md)
* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
