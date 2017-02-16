<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration mit SAP-HANA Cloud-Plattform | Microsoft Azure" 
    description="Erfahren Sie, wie Sie mit SAP-HANA Cloud-Plattform mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktiviert!" 
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

#<a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Lernprogramm: Azure-Active Directory-Integration mit SAP-HANA Cloud-Plattform
  
Ziel dieses Lernprogramms ist die Integration von Azure und SAP-HANA Cloud-Plattform angezeigt.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Konto SAP-HANA Cloud-Plattform
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie SAP-HANA Cloud-Plattform zugewiesen haben einzelnen melden Sie sich bei der Anwendung, die die [Einführung in die Access-Bereich](active-directory-saas-access-panel-introduction.md)verwenden können.

>[AZURE.IMPORTANT]Sie müssen Ihre eigene Anwendung bereitstellen oder einer Anwendung für Ihr Konto SAP-HANA Cloud-Plattform zum Testen des einmaligen Anmeldens auf abonnieren. In diesem Lernprogramm wird eine Anwendung in das Konto bereitgestellt.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für SAP-HANA Cloud-Plattform
2.  Konfigurieren einmaliges Anmelden
3.  Zuweisen einer Rolle zu einem Benutzer
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Szenario")
##<a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a>Aktivieren die Anwendungsintegration für SAP-HANA Cloud-Plattform
  
Ziel dieses Abschnitts ist es, wie die Anwendung die Telefonintegration für SAP-HANA Cloud gliedern.

###<a name="to-enable-the-application-integration-for-sap-hana-cloud-platform-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für SAP-HANA Cloud-Plattform der folgenden Schritte aus:

1.  Klicken Sie im Verwaltungsportal Azure, klicken Sie auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **SAP-HANA Cloud-Plattform**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Katalog der Anwendung")

7.  Im Bereich Ergebnisse wählen Sie **SAP-HANA Cloud-Plattform aus**und dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![SAP-Hana] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP-Hana")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer für SAP-HANA Cloud-Plattform mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden authentifizieren aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikat in Ihrem Mandanten SAP-HANA Cloud-Plattform hochladen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Integrationsseite **HANA Cloud-Plattform SAP-** Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der SAP-HANA Cloud-Plattform auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Konfigurieren Sie einmaliges Anmelden")

3.  Melden Sie sich in einem anderen Webbrowserfenster klicken Sie auf das SAP-HANA Cloud-Plattform Cockpit am https://account an. \<Querformat Host\>.ondemand.com/cockpit (z. B.: *https://account.hanatrial.ondemand.com/cockpit*).

4.  Klicken Sie auf der Registerkarte **vertrauen** .

    ![Vertrauen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Vertrauen")

5.  Führen Sie im Abschnitt Verwaltung vertrauen die folgenden Schritte aus:

    ![Abrufen von Metadaten] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Abrufen von Metadaten")

    1.  Klicken Sie auf der Registerkarte **Lokale Dienstanbieter** .
    2.  Wenn die HANA Cloud-Plattform SAP-Metadaten-Datei herunterladen möchten, klicken Sie auf **Metadaten erhalten**.

6.  Klicken Sie im aktiven Azure klassischen-Portal auf der Seite **Konfigurieren der App-URL** führen Sie die folgenden Schritte aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Konfigurieren der App-URL")

    1.  Geben Sie in das Textfeld **Melden Sie sich auf URL** die URL, die von den Benutzern verwendet werden, melden Sie sich bei Ihrer Anwendung **SAP-HANA Cloud-Plattform** aus. Dies ist die URL-Konto-spezifische einer geschützten Ressource in Ihrer Anwendung SAP-HANA Cloud-Plattform. Die URL basiert auf dem folgenden Muster: https:// *\<ApplicationName\>\<Kontoname\>.\< Querformat Host\>.ondemand.com/\<Pfad\_auf\_geschützte\_Ressource\> * (z. B.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)

        >[AZURE.NOTE]Dies ist die URL in der HANA Cloud-Plattform SAP-Anwendung, die den Benutzer authentifiziert erforderlich sind.

    2.  Öffnen Sie die heruntergeladene HANA Cloud-Plattform SAP-Metadaten-Datei, und suchen Sie dann auf die Kategorie **Ns3:AssertionConsumerService** .
    3.  Kopieren Sie den Wert für das Attribut **Speicherort** , und fügen Sie ihn in das Textfeld **SAP-HANA Cloud Plattform Antworten URL** .

7.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei SAP-HANA Cloud-Plattform** zum Herunterladen der Metadaten Ihrer klicken Sie auf **Metadaten herunterladen**, und speichern Sie die Datei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Konfigurieren Sie einmaliges Anmelden")

8.  Führen Sie auf das SAP-HANA Cloud-Plattform Cockpit im Abschnitt **Lokale Dienstanbieter** die folgenden Schritte aus:

    ![Management vertrauen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Management vertrauen")

    1.  Klicken Sie auf **Bearbeiten**.
    2.  Als **Konfigurationstyp**die Option **Benutzerdefiniert**aus.
    3.  Als **Lokalen Anbieternamen**übernehmen Sie den Standardwert ein.
    4.  Um einen **Schlüssel signieren** und ein Paar aus **Signaturzertifikat** Schlüssel zu generieren, klicken Sie auf **Generieren Schlüsselpaar**.
    5.  Wählen Sie als **Hauptbenutzer Verteilung** **deaktiviert**.
    6.  Wählen Sie als **Force Authentifizierung** **deaktiviert**.
    7.  Klicken Sie auf **Speichern**.

9.  Klicken Sie auf der Registerkarte **Vertrauenswürdige Identitätsanbieter** , und klicken Sie dann auf **Vertrauenswürdige Identitätsanbieter hinzufügen**.

    ![Management vertrauen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Management vertrauen")

    >[AZURE.NOTE]Um die Liste der vertrauenswürdigen Identitätsanbieter verwalten zu können, müssen Sie den Typ der Konfiguration Benutzerdefiniert im Abschnitt lokale Dienstanbieter ausgewählt haben. Für Konfiguration Standardtyp müssen Sie eine Vertrauensstellung nicht bearbeitbaren und implizite in der SAP-ID-Dienst an. Für keine verfügen Sie möglicherweise keine Trust-Einstellungen.

10. Klicken Sie auf der Registerkarte **Allgemein** , und klicken Sie dann auf **Durchsuchen** , um die heruntergeladene Metadatendatei hochladen.

    ![Management vertrauen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Management vertrauen")

    >[AZURE.NOTE] Nach dem Hochladen der Metadatendatei, werden die Werte für **einzelne anmelden URL**, **Einzelne Abmeldung URL** und **Signaturzertifikat** automatisch ausgefüllt.

11. Klicken Sie auf der Registerkarte **Attribute** .

12. Führen Sie auf der Registerkarte **Attribute** die folgenden Schritte aus:

    ![Attribute] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Attribute")

    1.  Fügen Sie durch Klicken auf **Add Assertion-Based Attribut**aus die folgenden Assertion-basierten Attribute hinzu:

        |Assertion Attribut| Der Tilgungsanteile Attribut|
        |-------------------|--------------------|
        |http://Schemas.xmlsoap.org/ws/2005/05/Identity/Claims/givenName|   Vorname|--------------------|--------------------|
        |http://Schemas.xmlsoap.org/ws/2005/05/Identity/Claims/Surname|        Nachname|-----------|
        |http://Schemas.xmlsoap.org/ws/2005/05/Identity/Claims/EmailAddress|E-Mail|

    >[AZURE.NOTE]Die Konfiguration der Attribute hängt davon ab, wie die Anwendung(en) auf HCP werden entwickelt, d. h. welche Attribute erwarten in der SAML-Antwort, und klicken Sie unter welche Name (Tilgungsanteile Attribut) sie dieses Attribut in den Code zugreifen.
    >  
    >ein.  Das **Attribut standardmäßig** in den Screenshot dient nur Abbildung zu. Es ist nicht erforderlich, um dem Szenario arbeiten zu gestalten.  
    >
    >b.  Die Namen und Werte für das **Attribut der Tilgungsanteile** dem Screenshot, hängen davon ab, wie die Anwendung entwickelt wird. Es ist möglich, dass die Anwendung andere Zuordnung erforderlich ist.

13. Im Azure klassischen-Portal auf der Seite des Dialog **Konfigurieren einmaliges Anmelden bei SAP-HANA Cloud-Plattform** wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **abgeschlossen**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Konfigurieren Sie einmaliges Anmelden")
  
Als optionalen Schritt können Sie Gruppen Assertion-basierten für Ihre Azure Active Directory-Identitätsanbieter konfigurieren.

>[AZURE.NOTE]Gruppen können auf SAP-HANA Cloud-Plattform Sie eine oder mehrere Rollen in Ihrer SAP-HANA Cloud-Plattform-Anwendung, durch die Werte von Attributen der SAML 2.0 Assertion bestimmt dynamisch einem oder mehreren Benutzern zuweisen. Wenn die Assertion das Attribut enthält beispielsweise "*Vertrag temporäre =*", können Sie alle betroffenen Benutzer der Gruppe "*temporäre*" hinzugefügt werden soll. Die Gruppe "*temporäre*" enthalten möglicherweise eine oder mehrere Rollen aus einer oder mehrerer Anmeldungen in Ihr Konto SAP-HANA Cloud-Plattform bereitgestellt.
>  
>Verwenden Sie Gruppen Assertion-basierten, wenn Sie viele Benutzer, die eine oder mehrere Rollen in Ihr Konto HANA Cloud-Plattform SAP-Anwendung Masse zuordnen möchten. Wenn Sie nur eine einzelne oder kleine Anzahl von Benutzern (a) bestimmten Rollen zuweisen, es empfiehlt sich direkt auf der Registerkarte "**Autorisierungs**" das SAP-HANA Cloud-Plattform Cockpit zuordnen, möchten.

##<a name="assigning-a-role-to-a-user"></a>Zuweisen einer Rolle zu einem Benutzer
  
Um Azure AD-Benutzer zur Anmeldung bei SAP-HANA Cloud-Plattform zu aktivieren, müssen Sie die Rollen in der SAP-HANA Cloud-Plattform zu zuweisen.

###<a name="to-assign-a-role-to-a-user-perform-the-following-steps"></a>Wenn Sie einem Benutzer eine Rolle zuweisen möchten, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich auf Ihre **SAP-HANA Cloud-Plattform** Cockpit aus.

2.  Führen Sie die folgenden Schritte aus:

    ![Autorisierungs] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Autorisierungs")

    1.  Klicken Sie auf **Autorisierung**.
    2.  Klicken Sie auf die Registerkarte **Benutzer** .
    3.  Geben Sie in das Textfeld **Benutzer** ein die e-Mail-Adresse des Benutzers ein.
    4.  Klicken Sie auf **zuweisen** , um den Benutzer einer Rolle zuzuweisen.
    5.  Klicken Sie auf **Speichern**.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Benutzer Azure AD-erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-sap-hana-cloud-platform-perform-the-following-steps"></a>Zuweisen von Benutzern zu SAP-HANA Cloud-Plattform führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **HANA Cloud-Plattform SAP-** Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).