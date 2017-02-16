<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in DocuSign | Microsoft Azure"
    description="Informationen Sie zum einmaligen Anmeldens zwischen Azure Active Directory und DocuSign konfigurieren."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Lernprogramm: Azure-Active Directory-Integration in DocuSign

Das Ziel dieses Lernprogramms ist die Integration von Azure und DocuSign anzeigen.
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

- Ein gültiges Azure-Abonnement
- Einen Mandanten in DocuSign



In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1. [Aktivieren die Anwendungsintegration für DocuSign](#enabling-the-application-integration-for-docusign) 


2. [Konfigurieren einmaliges Anmelden](#configuring-single-sign-on) 


3. [Konfigurieren der Bereitstellung](#configuring-account-provisioning) 


4. [Zuweisen von Benutzern](#assigning-users) 

    ![Konfigurieren einmaliges Anmelden][0]
 

## <a name="enabling-the-application-integration-for-docusign"></a>Aktivieren die Anwendungsintegration für DocuSign

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für DocuSign aktiviert.

### <a name="to-enable-the-application-integration-for-docusign-perform-the-following-steps"></a>Wenn die Anwendungsintegration für DocuSign aktivieren möchten, führen Sie die folgenden Schritte aus:

1. Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Konfigurieren einmaliges Anmelden][1]

2. Wählen Sie aus der Liste Verzeichnis Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Konfigurieren einmaliges Anmelden][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3]

5. Auf der Seite Was möchten Sie Dialogfeld kann, klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Konfigurieren einmaliges Anmelden][4]


6. Geben Sie im Suchfeld **DocuSign**ein.

    ![Konfigurieren einmaliges Anmelden][5]

7. Wählen Sie im Ergebnisbereich **DocuSign aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Konfigurieren einmaliges Anmelden][6]


## <a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu DocuSign mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.


### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1. Klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds konfigurieren Single Sign On, im Azure klassischen-Portal auf der Seite **DocuSign Anwendungsintegration** .

    ![Konfigurieren einmaliges Anmelden][7]

2. Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der DocuSign auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf Weiter.

    ![Konfigurieren einmaliges Anmelden][8]

3. Führen Sie auf der Seite **Einstellungen für die App konfigurieren** die folgenden Schritte aus:

    ![Konfigurieren einmaliges Anmelden][61]

    ein. Geben Sie im Textfeld **URL anmelden** `https://account.docusign.com/*`.  

    b. Geben Sie in das Textfeld **Bezeichner** `https://account.docusign.com/*`.  
   
    c. Klicken Sie auf **Weiter**. 


    > [AZURE.TIP] Melden Sie sich auf URL und die ID-Werte sind nur Platzhalter. Anleitungen zum Abrufen der tatsächlichen Werte für Ihre Umgebung fallen weiter unten in diesem Thema.
 

4. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei DocuSign** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden][10]


5. In einem anderen Webbrowserfenster melden Sie sich bei der **Administratorportal DocuSign** als Administrator.


6. Klicken Sie im Navigationsmenü auf der linken Seite auf **Domänen**.

    ![Konfigurieren einmaliges Anmelden][51]

7. Klicken Sie im rechten Bereich auf **Domäne anfordern**.

    ![Konfigurieren einmaliges Anmelden][52]

8. Klicken Sie im Dialogfeld **Anfordern einer Domäne** in das Textfeld **Domain Name** Geben Sie Ihre Unternehmensdomäne, und klicken Sie dann auf **anfordern**. Stellen Sie sicher, dass Sie die Domäne überprüfen und der Status aktiv ist.

    ![Konfigurieren einmaliges Anmelden][53]

9. Klicken Sie im Menü auf der linken Seite auf **Identitätsanbieter**  

    ![Konfigurieren einmaliges Anmelden][54]

10. Klicken Sie im rechten Bereich auf **Identitätsanbieter hinzufügen**. 
    
    ![Konfigurieren einmaliges Anmelden][55]

11. Führen Sie auf der Einstellungsseite **Anbieter Identität** die folgenden Schritte aus:

    ![Konfigurieren einmaliges Anmelden][56]


    ein. Geben Sie in das Textfeld **Name** einen eindeutigen Namen für Ihre Konfiguration aus. Verwenden Sie bitte keine Leerzeichen.

    b. Im Portal Azure klassischen kopieren Sie die URL des Herausgebers, und fügen Sie ihn in das Textfeld **Identität Anbieter Herausgeber** .

    c. Im Portal Azure klassischen kopieren Sie die **Remote-Anmelde-URL**, und fügen Sie ihn in das Textfeld **Identität Anbieter Anmelde-URL** .

    d. Im Portal Azure klassischen kopieren Sie der **Remote Abmeldung-URL**, und klicken Sie dann in das Textfeld **Identität Anbieter Abmelden URL** einzufügen.

    e. Wählen Sie **Die AuthN Anforderung signieren**.

    f. Wählen Sie als **AuthN senden Anforderung durch** **Bereitstellen**.

    g. Wählen Sie als **Abmeldung Anforderung durch Senden** **Bereitstellen**. 


12. Wählen Sie im Abschnitt **Benutzerdefinierte Attribut Zuordnung** das Feld, das Sie mit Azure AD anfordern zuordnen möchten. In diesem Beispiel wird das anfordern **Emailaddress** mit dem Wert des **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**zugeordnet. Dies ist die Standardnamen Anfordern von Azure AD-für e-Mail-Anspruch. 

    > [AZURE.NOTE] Verwenden der entsprechenden **Benutzer-ID** zuordnen den Benutzer aus Azure AD Docusign Benutzer Zuordnung an. Wählen Sie die richtigen Feld aus, und geben Sie den entsprechenden Wert basierend auf Ihrer organisationseinstellungen.

    ![Konfigurieren einmaliges Anmelden][57]

13. Im Abschnitt **Identität Anbieter Zertifikat** klicken Sie auf **Hinzufügen**, und Laden Sie das Zertifikat, das Sie vom klassischen Azure AD-Portal heruntergeladen haben.   

    ![Konfigurieren einmaliges Anmelden][58]

14. Klicken Sie auf **Speichern**.

15. Im Abschnitt **Identitätsanbieter** klicken Sie auf **Aktionen**, und klicken Sie dann auf **Endpunkte**.   

    ![Konfigurieren einmaliges Anmelden][59]



10. Klicken Sie im Portal Azure klassischen kehren Sie zu der Seite **Einstellungen für die App konfigurieren** . 

16. **Verwaltungsportal von DocuSign**im Abschnitt **Ansicht SAML 2.0 Endpunkte** führen Sie aus, die folgenden Schritte aus:

    ![Konfigurieren einmaliges Anmelden][60]

    ein. Kopieren Sie die **Service Provider Herausgeber-URL**, und fügen Sie ihn in das Textfeld **Bezeichner** im klassischen Azure-Portal.

    b. Kopieren Sie die **Service Provider Anmelde-URL**, und fügen Sie in das Textfeld **Melden Sie sich auf URL** im klassischen Azure-Portal.

    c.  Klicken Sie auf **Schließen**  


10. Klicken Sie im Portal Azure klassischen klicken Sie auf **Weiter**. 


15. Klicken Sie im Portal Azure klassischen wählen Sie die **Bestätigung der einzelnen anmelden Konfiguration**, und klicken Sie dann auf **Weiter**.

    ![Applikationen][14]

10. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.

    ![Applikationen][15]
 

## <a name="configuring-account-provisioning"></a>Konfigurieren der Bereitstellung

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer Bereitstellung von Active Directory-Benutzerkonten zu DocuSign aktiviert.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1. Klicken Sie im **Azure klassischen Portal**auf der Seite **DocuSign Anwendungsintegration** auf **Konfigurieren Konto bereitgestellt** , um das Dialogfeld konfigurieren Benutzer bereitgestellt öffnen.

    ![Konfigurieren der Bereitstellung][30]

2. Klicken Sie auf der Seite **Einstellungen und Administrator-Anmeldeberechtigungen** zum automatischen Benutzer bereitgestellt aktivieren möchten, geben Sie die Anmeldeinformationen eines Kontos DocuSign mit der Berechtigung, und klicken Sie dann auf **Weiter**. 

    ![Konfigurieren der Bereitstellung][31]

3. Klicken Sie im Dialogfeld **Verbindung testen** klicken Sie auf **Test zu starten**, und bei einem erfolgreichen Test, klicken Sie auf **Weiter**.

    ![Konfigurieren der Bereitstellung][32]

3. Klicken Sie auf der Seite **Bestätigung** auf **abgeschlossen**.

    ![Konfigurieren der Bereitstellung][33]
 

## <a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

### <a name="to-assign-users-to-docusign-perform-the-following-steps"></a>Zuweisen von Benutzern zu DocuSign führen Sie die folgenden Schritte aus:

1. Erstellen Sie ein Testkonto im **Azure klassischen Portal**.

2. Klicken Sie auf der Seite **DocuSign Anwendungsintegration** auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern][40]
 

3. Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Zuweisen von Benutzern][41]


Sie sollten jetzt 10 Minuten warten, und stellen Sie sicher, dass das Konto mit DocuSign synchronisiert wurde.

Als ersten Überprüfungsschritt können Sie den Bereitstellung Status auf Dashboard in der D auf der Seite DocuSign Anwendung Integration in Azure klassischen Portal prüfen.

![Zuweisen von Benutzern][42]

Ein erfolgreich abgeschlossener Benutzer bereitgestellt Kreis wird durch einen verknüpften Status angezeigt:

![Zuweisen von Benutzern][43]


Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung.

Weitere Details über den Bereich Access finden Sie unter Einführung in die Access-Systemsteuerung.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_00.png
[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_01.png
[6]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_02.png
[7]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_03.png
[8]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_04.png
[9]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_05.png
[10]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_06.png

[14]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_10.png
[15]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_11.png

[30]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png
[31]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_12.png
[32]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_13.png
[33]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_14.png



[40]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_15.png
[41]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_16.png
[42]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_17.png
[43]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_18.png

[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png