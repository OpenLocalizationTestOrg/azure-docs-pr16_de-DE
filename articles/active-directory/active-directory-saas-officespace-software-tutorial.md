<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in OfficeSpace Software | Microsoft Azure" 
    description="Erfahren Sie, wie zur Verwendung von OfficeSpace Software mit Azure Active Directory aktivieren einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Lernprogramm: Azure-Active Directory-Integration in OfficeSpace-Software
  
Ziel dieses Lernprogramms ist die Integration von Azure und OfficeSpace Software angezeigt.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein OfficeSpace Software einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie OfficeSpace Software zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite OfficeSpace Software (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für OfficeSpace-Software
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-officespace-software-tutorial/IC777764.png "Szenario")
##<a name="enabling-the-application-integration-for-officespace-software"></a>Aktivieren die Anwendungsintegration für OfficeSpace-Software
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für OfficeSpace Software aktiviert.

###<a name="to-enable-the-application-integration-for-officespace-software-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für OfficeSpace Software die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-officespace-software-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-officespace-software-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-officespace-software-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-officespace-software-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **OfficeSpace Software**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-officespace-software-tutorial/IC777765.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **OfficeSpace-Software**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![OfficeSpace-Software] (./media/active-directory-saas-officespace-software-tutorial/IC781007.png "OfficeSpace-Software")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren OfficeSpace Software-mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für OfficeSpace Software konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite Anwendung Integration **OfficeSpace Software** klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Einmaliges konfigurieren auf =] (./media/active-directory-saas-officespace-software-tutorial/IC777766.png "Einmaliges konfigurieren auf =")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der OfficeSpace Software auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-officespace-software-tutorial/IC777767.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **OfficeSpace Software melden Sie sich auf URL** die URL, die von den Benutzern verwendet, um melden Sie sich an Ihrer Anwendung OfficeSpace Software auf (z. B.: "*https://company.officespacesoftware.com*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-officespace-software-tutorial/IC775556.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei OfficeSpace Software** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-officespace-software-tutorial/IC793769.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens OfficeSpace Software als Administrator.

6.  Wechseln Sie zu **Admin \> Verbinder**.

    ![Administrator] (./media/active-directory-saas-officespace-software-tutorial/IC777769.png "Administrator")

7.  Klicken Sie auf **SAML-Autorisierung**.

    ![Verbinder] (./media/active-directory-saas-officespace-software-tutorial/IC777770.png "Verbinder")

8.  Führen Sie im Abschnitt **SAML-Autorisierung** die folgenden Schritte aus:

    ![SAML-Konfiguration] (./media/active-directory-saas-officespace-software-tutorial/IC777771.png "SAML-Konfiguration")

    1.  Im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden bei OfficeSpace Software** kopieren Sie des Werts **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Abmeldung Provider-Url** .
    2.  Im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden bei OfficeSpace Software** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **Client idp Ziel-Url** .
    3.  Kopieren Sie den **Fingerabdruck** Wert aus dem exportierte Zertifikat, und fügen Sie ihn in das Textfeld **Client idp Zertifikat Fingerabdruck** .  

        >[AZURE.TIP]
        Weitere Informationen hierzu finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

    4.  Klicken Sie auf **Einstellungen speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-officespace-software-tutorial/IC777772.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei OfficeSpace Software zu ermöglichen, müssen er in OfficeSpace Software bereitgestellt werden. Im Falle von OfficeSpace Software ist die Bereitstellung einer automatisierten Aufgabe.  
Es gibt keine Aktionselement für Sie ein.  
Benutzer werden automatisch bei Bedarf beim ersten einzelnen anmelden erstellt.

>[AZURE.NOTE]Alle anderen OfficeSpace Software Benutzer Konto Creation Tools können oder APIs zur Verfügung gestellt von OfficeSpace Software bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-officespace-software-perform-the-following-steps"></a>Zuweisen von Benutzern zu OfficeSpace Software führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite Anwendung Integration **OfficeSpace Software **auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-officespace-software-tutorial/IC777773.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-officespace-software-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).