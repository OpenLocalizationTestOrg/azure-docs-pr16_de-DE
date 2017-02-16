<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in NetDocuments | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von NetDocuments mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Lernprogramm: Azure-Active Directory-Integration in NetDocuments
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und NetDocuments anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten NetDocuments
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie NetDocuments zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite NetDocuments (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für NetDocuments
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-netdocuments-tutorial/IC795040.png "Szenario")
##<a name="enabling-the-application-integration-for-netdocuments"></a>Aktivieren die Anwendungsintegration für NetDocuments
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für NetDocuments aktiviert.

###<a name="to-enable-the-application-integration-for-netdocuments-perform-the-following-steps"></a>Wenn die Anwendungsintegration für NetDocuments aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-netdocuments-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-netdocuments-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-netdocuments-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-netdocuments-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **NetDocuments**.

    ![Katalog der Anwendung] (./media/active-directory-saas-netdocuments-tutorial/IC795041.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **NetDocuments aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![NetDocuments] (./media/active-directory-saas-netdocuments-tutorial/IC795042.png "NetDocuments")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu NetDocuments mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für NetDocuments konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **NetDocuments** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-netdocuments-tutorial/IC795043.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der NetDocuments auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-netdocuments-tutorial/IC795044.png "Konfigurieren Sie einmaliges Anmelden")

3.  Führen Sie auf der Seite **Konfigurieren der App-URL** die folgenden Schritte aus:

    ![Konfigurieren der App-URL] (./media/active-directory-saas-netdocuments-tutorial/IC795045.png "Konfigurieren der App-URL")

    1.  Geben Sie in das Textfeld **Melden Sie sich auf URL** Ihre URL von Ihren Benutzern verwendet werden, sich an Ihrer Anwendung NetDocuments auf (z. B.: "*https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=CA-JI1BG3H1*").
    2.  Geben Sie in das Textfeld **NetDocuments Antwort-URL** den gleichen Wert, die Sie, in eingegeben haben der das Textfeld **Melden Sie sich auf URL** .  

        >[AZURE.NOTE]Finden Sie den richtigen Wert am Ende im Dialogfeld **Identität Partnersuche** (Siehe den Screenshot für Schritt 9).

    3.  Klicken Sie auf **Weiter**

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei NetDocuments** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-netdocuments-tutorial/IC795046.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens NetDocuments als Administrator.

6.  Wechseln Sie zu **Administrator**.

7.  Klicken Sie auf **Hinzufügen und Entfernen von Benutzern und Gruppen**.

    ![Repository] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Repository")

8.  Klicken Sie auf **konfigurieren, erweiterte Authentifizierungsoptionen**.

    ![Konfigurieren erweiterter Authentifizierungsoptionen] (./media/active-directory-saas-netdocuments-tutorial/IC795048.png "Konfigurieren erweiterter Authentifizierungsoptionen")

9.  Führen Sie auf **Der Partnersuche Identität** im Dialogfeld die folgenden Schritte aus:

    ![Partnersuche Identitty] (./media/active-directory-saas-netdocuments-tutorial/IC795049.png "Partnersuche Identitty")

    1.  Wählen Sie als **verbundene Identität Servertyp**aus **Active Directory Federation Services**.
    2.  Klicken Sie auf **Datei auswählen**, um die heruntergeladene Metadatendatei hochzuladen.
    3.  Klicken Sie auf **OK**.

10. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-netdocuments-tutorial/IC795050.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei NetDocuments zu ermöglichen, müssen er in NetDocuments bereitgestellt werden.  
Im Falle von NetDocuments ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Optimierung bei der Website Ihres Unternehmens **NetDocuments** als Administrator.

2.  Klicken Sie oben im Menü auf **Administrator**.

    ![Administrator] (./media/active-directory-saas-netdocuments-tutorial/IC795051.png "Administrator")

3.  Klicken Sie auf **Hinzufügen und Entfernen von Benutzern und Gruppen**.

    ![Repository] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Repository")

4.  Geben Sie in das Textfeld **E-Mail-Adresse** die e-Mail-Adresse eines gültigen Azure Active Directory-Kontos Sie bereitstellen möchten, und klicken Sie dann auf **Benutzer hinzufügen**.

    ![E-Mail-Adresse] (./media/active-directory-saas-netdocuments-tutorial/IC795053.png "E-Mail-Adresse")

    >[AZURE.NOTE]Der Inhaber des Azure Active Directory-Konto erhalten eine e-Mail-Nachricht, die enthält einen Link, um das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE]Alle anderen NetDocuments Benutzer Konto Creation Tools können oder APIs bereitgestellt durch NetDocuments bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-netdocuments-perform-the-following-steps"></a>Zuweisen von Benutzern zu NetDocuments führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **NetDocuments **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-netdocuments-tutorial/IC795054.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-netdocuments-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).