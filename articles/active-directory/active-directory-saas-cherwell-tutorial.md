<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Cherwell | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Cherwell mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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
    ms.date="10/14/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-cherwell"></a>Lernprogramm: Azure-Active Directory-Integration in Cherwell

Das Ziel dieses Lernprogramms ist die Integration von Azure und Cherwell anzeigen. Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Cherwell einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Cherwell zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Cherwell (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Cherwell
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-cherwell-tutorial/IC798988.png "Szenario")
##<a name="enabling-the-application-integration-for-cherwell"></a>Aktivieren die Anwendungsintegration für Cherwell

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Cherwell aktiviert.

###<a name="to-enable-the-application-integration-for-cherwell-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Cherwell aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cherwell-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-cherwell-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-cherwell-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-cherwell-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Cherwell**.

    ![Cherwell] (./media/active-directory-saas-cherwell-tutorial/IC798989.png "Cherwell")

7.  Wählen Sie im Ergebnisbereich **Cherwell aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

    ![Cherwell](./media/active-directory-saas-cherwell-tutorial/IC798996.png "Cherwell")

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Cherwell mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Cherwell** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-cherwell-tutorial/IC798990.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Cherwell auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-cherwell-tutorial/IC798991.png "Konfigurieren Sie einmaliges Anmelden")

3.  Führen Sie auf der Seite **Konfigurieren der App-URL** die folgenden Schritte aus:

    ![Konfigurieren der App-URL] (./media/active-directory-saas-cherwell-tutorial/IC798992.png "Konfigurieren der App-URL")

    ein.  Geben Sie in das Textfeld **Melden Sie sich auf URL** die URL, die von den Benutzern verwendet werden, melden Sie sich bei Ihrem **Cherwell** (z. B.: *https://\<Firmennamen\>.cherwellondemand.com/cherwellclient*).

    b.  Klicken Sie auf **Weiter**

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Cherwell** führen Sie die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-cherwell-tutorial/IC798993.png "Konfigurieren Sie einmaliges Anmelden")

    ein.  Klicken Sie auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat lokal auf Ihrem Computer.

    b.  Kopieren Sie die **Identität Provider-URL**ein.

    c.  Kopieren Sie die **URL der Dienst für einmaliges Anmelden**.

    d.  Klicken Sie auf **Weiter**.

5.  Senden Sie das heruntergeladene Zertifikat, die **Identität Provider-URL** und der **Einzelnen anmelden Service URL** an Ihr Supportteam Cherwell.

    >[AZURE.NOTE] Führen Sie die eigentliche SSO-Konfiguration muss das Supportteam Cherwell.
Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.

6.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-cherwell-tutorial/IC798994.png "Konfigurieren Sie einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzern zur Anmeldung bei Cherwell zu ermöglichen, müssen er in Cherwell bereitgestellt werden.  
Bei Cherwell müssen die Benutzerkonten von Ihrem Supportteam Cherwell erstellt werden.

>[AZURE.NOTE] Alle anderen Cherwell Benutzer Konto Creation Tools können oder APIs von bereitgestellten Cherwell Bereitstellen von Azure Active Directory-Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-cherwell-perform-the-following-steps"></a>Zuweisen von Benutzern zu Cherwell führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Cherwell** Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-cherwell-tutorial/IC798995.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-cherwell-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
