<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Dropbox für Unternehmen | Microsoft Azure" 
    description="Erfahren Sie, wie Dropbox für Unternehmen mit Azure Active Directory zu verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Lernprogramm: Azure-Active Directory-Integration in Dropbox für Unternehmen
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Dropbox für Unternehmen anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Test in Dropbox für Unternehmen
  
Am Ende dieses Lernprogramms, des Unternehmens-Website (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie für Business einzelnen melden Sie sich bei der Anwendung an Ihre Dropbox für Unternehmen kann Dropbox zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Dropbox für Unternehmen
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769508.png "Szenario")



##<a name="enabling-the-application-integration-for-dropbox-for-business"></a>Aktivieren die Anwendungsintegration für Dropbox für Unternehmen
  
In diesem Abschnitt Ziel ist es zu gliedernden So aktivieren Sie die Anwendungsintegration für Dropbox für Unternehmen.

###<a name="to-enable-the-application-integration-for-dropbox-for-business-perform-the-following-steps"></a>Um die Anwendungsintegration für Dropbox for Business zu aktivieren, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Dropbox für Unternehmen**.

    ![Katalog der Anwendung] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701010.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Dropbox for Business**und dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Dropbox für Unternehmen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701011.png "Dropbox für Unternehmen")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Dropbox for Business mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

Als Teil dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikat in Ihrem Dropbox für Business Mandanten hochladen. Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Integrationsseite von **Dropbox for Business** -Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749323.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Dropbox for Business auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749327.png "Konfigurieren einmaliges Anmelden")

3.  Führen Sie auf der Seite **Konfigurieren der App-URL** die folgenden Schritte aus:

    ein. Melden Sie sich für den Zugriff auf Ihre Dropbox für Business-Mandanten. 

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769509.png "Konfigurieren einmaliges Anmelden")

    b. Klicken Sie im Navigationsbereich auf der linken Seite auf **Administrator Console**. 

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769510.png "Konfigurieren einmaliges Anmelden")

    c. Klicken Sie auf die **Verwaltungskonsole**klicken Sie im linken Navigationsbereich auf **Authentifizierung** . 

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769511.png "Konfigurieren einmaliges Anmelden")

    d. Klicken Sie im Abschnitt **einmaliges Anmelden** wählen Sie **einmaliges Anmelden aktivieren**, und klicken Sie dann auf **Weitere** um diesen Abschnitt zu erweitern.  

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769512.png "Konfigurieren einmaliges Anmelden")

    e. Kopieren Sie die URL neben **Benutzer können sich anmelden, indem Sie ihre e-Mail-Adresse eingeben, oder sie können direkt zu wechseln**. 

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769513.png "Konfigurieren einmaliges Anmelden")

    f. Fügen Sie auf dem Portal Azure klassischen im Textfeld URL **DropBox für Unternehmen melden Sie sich** die URL ein. 

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769514.png "Konfigurieren einmaliges Anmelden")  



4. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Dropbox für Unternehmen** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.  

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769515.png "Konfigurieren einmaliges Anmelden")


5. Führen Sie auf Ihrer Dropbox für Business Mandanten, im Abschnitt **einmaliges Anmelden** auf der Seite **Authentifizierung** die folgenden Schritte aus: 

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Konfigurieren einmaliges Anmelden")

    ein. Klicken Sie auf **erforderlich**.

    b. Kopieren Sie den Wert für die **Anmeldung Seiten-URL** , und fügen Sie ihn in das Textfeld **URL anmelden** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Dropbox for Business** .


    c. Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat. 

    > [AZURE.TIP] Weitere Informationen hierzu finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).


    d. Klicken Sie auf die Schaltfläche **"Zertifikat auswählen"** , und navigieren Sie zu Ihrem **Base-64-codierte Zertifikatsdatei**.


    e. Klicken Sie auf die Schaltfläche **"Änderungen speichern"** , um die Konfiguration auf Ihre DropBox für Business Mandanten abzuschließen.


6. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen. 

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749329.png "Konfigurieren einmaliges Anmelden")



##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer Bereitstellung von Active Directory-Benutzerkonten zu Dropbox für Unternehmen aktiviert.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1. Klicken Sie im Azure klassischen-Portal auf der Seite Anwendung Integration von **Dropbox for Business** auf **Konfigurieren Benutzer bereitgestellt** , um das Dialogfeld **Konfigurieren Benutzer bereitgestellt** öffnen.

2. Klicken Sie auf die aktivieren Benutzer bereitgestellt zu DropBox für Business-Seite, auf aktivieren Benutzer bereitgestellt, um die Anmeldung bei Dropbox zum Verknüpfen mit Azure AD-Dialogfeld zu öffnen.  

    ![Benutzer bereitgestellt] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769517.png "Benutzer bereitgestellt")

3. Klicken Sie im Dialogfeld **Anmelden bei Dropbox zum Verknüpfen mit Azure AD** melden Sie sich an Ihrem Dropbox für Business-Mandanten. 

    ![Benutzer bereitgestellt] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769518.png "Benutzer bereitgestellt")



4. Klicken Sie auf **Zulassen** Azure AD Zugriff auf Dropbox erteilen. 

    ![Benutzer bereitgestellt] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769519.png "Benutzer bereitgestellt")



5. Wenn Sie die Konfiguration abgeschlossen haben, klicken Sie auf die Schaltfläche **abgeschlossen** .  

    ![Benutzer bereitgestellt] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769520.png "Benutzer bereitgestellt")




##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-dropbox-for-business-perform-the-following-steps"></a>Zuweisen von Benutzern zu Dropbox für Unternehmen führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite Anwendung Integration von **Dropbox for Business **auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769521.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC767830.png "Ja")
  


Sie sollten jetzt 10 Minuten warten, und stellen Sie sicher, dass das Konto mit Dropbox for Business synchronisiert wurde.

Als ersten Überprüfungsschritt können Sie den Bereitstellung Status auf **Dashboard** auf der Seite **Dropbox für Business** Anwendung Integration in Azure klassischen Portal prüfen.

![Zuweisen von Benutzern] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769522.png "Zuweisen von Benutzern")


Ein erfolgreich abgeschlossener Benutzer bereitgestellt Kreis wird durch einen verknüpften Status angezeigt.

![Zuweisen von Benutzern] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769523.png "Zuweisen von Benutzern")


Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung.
Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).




## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)