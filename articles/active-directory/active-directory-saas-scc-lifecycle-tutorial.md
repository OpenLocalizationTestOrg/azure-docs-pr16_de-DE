<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in SCC Lebenszyklus | Microsoft Azure" 
    description="Erfahren Sie, wie einmaliges Anmelden, automatisierte Bereitstellung und mehr Aktivierung SCC Lebenszyklus mit Azure Active Directory mithilfe!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Lernprogramm: Azure-Active Directory-Integration in SCC Lebenszyklus
  
Ziel dieses Lernprogramms ist die Integration von Azure und SCC Lebenszyklus angezeigt.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine SCC LifeCycle einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie SCC Lebenszyklus zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite SCC Lebenszyklus (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für SCC Lebenszyklus
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Szenario")
##<a name="enabling-the-application-integration-for-scc-lifecycle"></a>Aktivieren die Anwendungsintegration für SCC Lebenszyklus
  
In diesem Abschnitt Ziel ist es zu gliedernden wie Sie die Anwendungsintegration für SCC Lebenszyklus aktivieren.

###<a name="to-enable-the-application-integration-for-scc-lifecycle-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für SCC LifeCycle die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **SCC Lebenszyklus**ein.

    ![Katalog der Anwendung] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **SCC Lebenszyklus aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![SCC Lebenszyklus] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC Lebenszyklus")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren auf SCC-Lebenszyklus mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **SCC Lebenszyklus** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der SCC Lebenszyklus auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Melden Sie sich auf URL** ein die URL untersuchten Ihre Benutzer bei Ihrer SCC Supportlebenszyklus-Anwendung unter Verwendung des folgenden Musters "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*", klicken Sie auf, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei SCC LifeCycle** auf **Metadaten herunterladen**und dann speichern Sie Metadatendatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Konfigurieren Sie einmaliges Anmelden")

5.  Weiterleiten dieser Datei Metadaten an SCC LifeCycle Supportteam.

    >[AZURE.NOTE]Einmaliges Anmelden muss durch das Supportteam SCC Lebenszyklus aktiviert werden.

6.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei SCC Lebenszyklus zu ermöglichen, müssen er in SCC Lebenszyklus bereitgestellt werden.
  
Es gibt keine Aktionselement für Ihre Benutzer bereitgestellt, die SCC Lebenszyklus konfigurieren.  
Wenn ein zugewiesene Benutzer zur Anmeldung bei SCC Lebenszyklus versucht, wird ein Konto SCC Lebenszyklus bei Bedarf automatisch erstellt.

>[AZURE.NOTE]Alle anderen SCC Lebenszyklus Benutzer Konto Creation Tools können oder APIs zur Verfügung gestellt von SCC Lebenszyklus bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-scc-lifecycle-perform-the-following-steps"></a>Zuweisen von Benutzern zu SCC Lebenszyklus führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **SCC Lebenszyklus **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).