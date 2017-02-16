<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration mit Aha! | Microsoft Azure" 
    description="Erfahren Sie, wie mit Aha! mit Azure Active Directory einmaliges Anmelden aktivieren automatisierte bereitgestellt und vieles mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-aha"></a>Lernprogramm: Azure-Active Directory-Integration mit Aha!

Ziel dieses Lernprogramms ist es, zeigen die Integration von Azure und Aha!  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Aha! Einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms, haben die Benutzer Azure AD-Sie Aha zugewiesen! werden können einzelne melden Sie sich bei der Anwendung an Ihre Aha! Firmenwebsite (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Aha!
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-aha-tutorial/IC798944.png "Szenario")
##<a name="enabling-the-application-integration-for-aha"></a>Aktivieren die Anwendungsintegration für Aha!

In diesem Abschnitt Ziel ist es zu gliedernden So aktivieren Sie die Anwendungsintegration für Aha!.

###<a name="to-enable-the-application-integration-for-aha-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Aha!, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-aha-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-aha-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-aha-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-aha-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Aha!**.

    ![Katalog der Anwendung] (./media/active-directory-saas-aha-tutorial/IC798945.png "Katalog der Anwendung")

7.  Wählen Sie im Bereich Ergebnisse **Aha!**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Aha!] (./media/active-directory-saas-aha-tutorial/IC802746.png "Aha!")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Ziel in diesem Abschnitt ist zum Aktivieren der Benutzer für Aha authentifizieren Gliedern! mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Portal Azure klassischen klicken Sie auf die **Aha!** Integration der Anwendung Seite, klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-aha-tutorial/IC798946.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf die **Wie möchten Sie Benutzer anmelden auf Aha!** Seite, wählen Sie **Microsoft Azure AD einmaliges Anmelden**aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-aha-tutorial/IC798947.png "Konfigurieren Sie einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in die **Aha! Melden Sie sich auf URL** Textfeld, geben die URL zum durch Ihre Benutzer anmelden zu Ihrem Aha! Anwendung (z. B.: "*https://company.aha.io/session/new*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-aha-tutorial/IC798948.png "Konfigurieren der App-URL")

4.  Klicken Sie auf die **Konfigurieren einmaliges Anmelden bei Aha!** Seite zum Herunterladen der Metadatendatei auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-aha-tutorial/IC798949.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich in Ihrem Aha! Firmenwebsite als Administrator.

6.  Klicken Sie auf **Einstellungen**, klicken Sie im Menü oben.

    ![Einstellungen] (./media/active-directory-saas-aha-tutorial/IC798950.png "Einstellungen")

7.  Klicken Sie auf **Konto**.

    ![Profil] (./media/active-directory-saas-aha-tutorial/IC798951.png "Profil")

8.  Klicken Sie auf **Sicherheit und einmaliges Anmelden**.

    ![Sicherheit und einmaliges Anmelden] (./media/active-directory-saas-aha-tutorial/IC798952.png "Sicherheit und einmaliges Anmelden")

9.  Wählen Sie im Abschnitt **Einmaliges Anmelden** als **Identitätsanbieter** **SAML2.0**ein.

    ![Sicherheit und einmaliges Anmelden] (./media/active-directory-saas-aha-tutorial/IC798953.png "Sicherheit und einmaliges Anmelden")

10. Führen Sie auf der Seite des Konfiguration **Einmaliges Anmelden** die folgenden Schritte aus:

    ![Einmaliges Anmelden] (./media/active-directory-saas-aha-tutorial/IC798954.png "Einmaliges Anmelden")

    1.  Geben Sie in das Textfeld **Name** einen Namen für die Konfiguration aus.
    2.  Wählen Sie für die **Verwendung von konfigurieren** **Metadaten-Datei**ein.
    3.  Um die heruntergeladene Metadatendatei hochladen möchten, klicken Sie auf **Durchsuchen**.
    4.  Klicken Sie auf **Aktualisieren**.

11. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-aha-tutorial/IC798955.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um aktivieren Azure AD-Benutzer zur Anmeldung bei Aha!, müssen er in Aha bereitgestellt werden!.  
Wenn Aha!, eine automatisierte Aufgabe bereitgestellt wird.  
Es gibt keine Aktionselement für Sie ein.
  
Benutzer werden automatisch bei Bedarf beim ersten einzelnen anmelden erstellt.

>[AZURE.NOTE] Sie können eine beliebige andere Aha! Tools für Benutzer Konto erstellen oder Aha-APIs! AAD Benutzerkonten nicht bereitstellen.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-aha-perform-the-following-steps"></a>Zuweisen von Benutzern zu Aha!, führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf die **Aha!** Integration der Anwendung Seite, klicken Sie auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-aha-tutorial/IC798956.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-aha-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
