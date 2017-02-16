<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Citrix GoToMeeting | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Citrix GoToMeeting mit Azure Active Directory einmaliges Anmelden, automatisierte bereitgestellt und mehr aktivieren!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a>Lernprogramm: Azure-Active Directory-Integration in Citrix GoToMeeting  
Gilt für: Azure

>[AZURE.TIP]Feedback, finden Sie [hier](http://go.microsoft.com/fwlink/?LinkId=522412).

Das Ziel dieses Lernprogramms ist die Integration von Azure und Citrix GoToMeeting anzeigen. Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten in Citrix GoToMeeting

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Citrix GoToMeeting
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Konfiguration] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768996.png "Konfiguration")



##<a name="enabling-the-application-integration-for-citrix-gotomeeting"></a>Aktivieren die Anwendungsintegration für Citrix GoToMeeting

So aktivieren Sie die Anwendungsintegration für Citrix GoToMeeting Gliedern ist das Ziel in diesem Abschnitt.

###<a name="to-enable-the-application-integration-for-citrix-gotomeeting-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Citrix GoToMeeting aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung aus dem Katalog] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749322.png "Hinzufügen einer Anwendung aus dem Katalog")

6.  Geben Sie im **Suchfeld** **Citrix GoToMeeting**aus.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701006.png "Citrix GoToMeeting")

7.  Wählen Sie im Ergebnisbereich **Citrix GoToMeeting aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701012.png "Citrix GoToMeeting")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Citrix GoToMeeting mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikat in Ihrem Mandanten Citrix GoToMeeting hochladen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Klicken Sie auf der Seite **Citrix GoToMeeting** Integration Anwendung auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren eines einzelnen melden Sie sich auf** öffnen.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768997.png "Einmaliges Anmelden aktivieren")

2.  Wählen Sie auf der Seite **Wie möchten Sie Benutzer bei der Citrix GoToMeeting auf** **Microsoft Azure AD einmaliges Anmelden**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768998.png "Konfigurieren einmaliges Anmelden")


3. Klicken Sie auf der Seite **Konfigurieren der App-Einstellungen** auf **Weiter**. 

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689981.png "Einmaliges Anmelden aktivieren")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Citrix GoToMeeting** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768999.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Browserfenster melden Sie sich in Ihrem Citrix Organisation Center ([https://account.citrixonline.com/organization/administration/](https://account.citrixonline.com/organization/administration/)).

6. Klicken Sie auf der Registerkarte **Identitätsanbieter** , und gehen Sie folgendermaßen vor:  

    ![SAML-setup] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML-setup")

    ein. Wählen Sie **manuell**

    
    b. Kopieren Sie den Wert für die **Anmeldung Seiten-URL** im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Citrix GoToMeeting** , und fügen Sie ihn in das Textfeld **Anmeldung Seiten-URL** . 

    
    c. Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Citrix GoToMeeting** kopieren Sie den Wert **Sign-Out Seiten-URL** , und fügen Sie ihn in das Textfeld **Abmeldung Seiten-URL** .

    
    d. Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Citrix GoToMeeting** kopieren Sie den Wert der **Element-ID** , und fügen Sie ihn in das Textfeld **Identität Anbieter Einheiten ID** .

   
    e. Wenn Ihr heruntergeladene Zertifikat hochladen möchten, klicken Sie auf **Hochladen**.

    
    f. Klicken Sie auf **Speichern**.

6.  Klicken Sie im Portal Azure klassischen wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769000.png "Konfigurieren einmaliges Anmelden")


7. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.

    ![SAML-setup] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689982.png "SAML-setup")





##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Das Ziel der in diesem Abschnitt ist zum Aktivieren der Bereitstellung von Active Directory-Benutzerkonten zu Citrix GoToMeeting gliedern.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der Seite **Citrix GoToMeeting** Anwendung Integration auf **Konfigurieren Benutzer bereitgestellt** , um das Dialogfeld **Konfigurieren Benutzer bereitgestellt** öffnen.

    ![Konfigurieren Benutzer bereitgestellt] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769001.png "Konfigurieren Benutzer bereitgestellt")

2.  Klicken Sie auf der Seite **Einstellungen und Administrator-Anmeldeberechtigungen** führen Sie die folgenden Schritte aus:

    ![Konfigurieren Benutzer bereitgestellt] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769002.png "Konfigurieren Benutzer bereitgestellt")

    ein. Geben Sie in das Textfeld **Citrix GoToMeeting Admin-Benutzername** den Benutzernamen eines Administrators ein.

    
    b. In das Textfeld **Citrix GoToMeeting Administratorkennworts** , das Kennwort des Administrators.

    
    c. Klicken Sie auf **Weiter**.

3.  Klicken Sie auf der Seite **Bestätigung** auf das Häkchen, um die Konfiguration zu speichern.

4.  Klicken Sie auf die Schaltfläche **Überprüfen** , um zu überprüfen, ob Ihre Konfiguration.


##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-citrix-gotomeeting-perform-the-following-steps"></a>Zuweisen von Benutzern zu Citrix GoToMeeting führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Citrix GoToMeeting** Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769003.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC767830.png "Ja")

Sie sollten jetzt 10 Minuten warten, und stellen Sie sicher, dass das Konto mit Dropbox for Business synchronisiert wurde.

Als ersten Überprüfungsschritt können Sie den Bereitstellung Status auf Dashboard in der D auf der Seite **Citrix GoToMeeting** Anwendung Integration in Azure klassischen Portal prüfen.

![Dashboard] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769004.png "Dashboard")

Ein erfolgreich abgeschlossener Benutzer bereitgestellt Kreis wird durch einen verknüpften Status angezeigt:

![Integrationsstatus] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769005.png "Integrationsstatus")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung.

Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](https://msdn.microsoft.com/library/dn308586).
