<properties 
    pageTitle="Lernprogramm: Konfigurieren von Arbeitstag für eingehende Synchronisierung | Microsoft Azure" 
    description="Erfahren Sie, wie eingehende Synchronisierung mit Azure Active Directory verwenden, um einmaliges Anmelden, automatisierte Bereitstellung und mehr zu ermöglichen!" 
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
    ms.date="04/06/2016" 
    ms.author="jeedes" />

#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Lernprogramm: Konfigurieren von Arbeitstag für eingehende Synchronisierung
>[AZURE.NOTE]Azure Active Directory (AD) Premium ist für Kunden in China mithilfe der weltweiten Instanz von Azure AD verfügbar.    
Azure AD-Premium ist im Microsoft Azure-Dienst von 21Vianet in China betrieben wird derzeit nicht unterstützt.    

Ziel dieses Lernprogramms ist es, die Sie die Schritte anzeigen, die Sie in der Arbeitstag und Microsoft Azure AD zum Importieren von Personen aus Arbeitstag an Microsoft Azure AD ausführen müssen.    
 Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:  

-   Ein gültiges Azure-Abonnement  
-   Einen Mandanten in Arbeitstag  

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:  

1.  Aktivieren die Anwendungsintegration für Arbeitstag  
2.  Erstellen eines Benutzers mit Integration system  
3.  Erstellen einer Sicherheitsgruppe  
4.  Zuweisen des Integration Systembenutzers zur Sicherheitsgruppe  
5.  Konfigurieren von Sicherheitsoptionen für die Gruppe  
6.  Aktivieren die Richtlinie Änderungen Sicherheit  
7.  Konfigurieren von Benutzer importieren in Microsoft Azure AD  

##<a name="enabling-the-application-integration-for-workday"></a>Aktivieren die Anwendungsintegration für Arbeitstag

Ziel dieses Abschnitts ist es, wie die Anwendung die Telefonintegration für Vertrieb gliedern.    

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für Arbeitstag der folgenden Schritte aus:

1.  Klicken Sie im Verwaltungsportal Azure, klicken Sie auf der linken Navigationsbereich auf **Active Directory**.    

    ![Active Directory] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700993.png "Active Directory")  

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.    

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .    

    ![Applikationen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700994.png "Applikationen")  

4.  Um den **Katalog der Anwendung**zu öffnen, klicken Sie auf **Ein App hinzufügen**, und klicken Sie dann auf **Hinzufügen, eine Anwendung für meine Organisation verwenden**.    

    ![Was möchten Sie tun?] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700995.png "Was möchten Sie tun?")  

5.  Geben Sie im **Suchfeld** **Arbeitstag**aus.    

    ![Arbeitstag] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701021.png "Arbeitstag")  

6.  Wählen Sie im Ergebnisbereich **Arbeitstag aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.    

    ![Arbeitstag] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701022.png "Arbeitstag")  

##<a name="creating-an-integration-system-user"></a>Erstellen eines Benutzers mit Integration system

1.  Geben Sie in der **Arbeitstag Arbeitsfläche**in das Suchfeld ein **Benutzer erstellen** , und klicken Sie dann auf den Link, **Integration Systembenutzer erstellen**.     

    ![Erstellen Sie Benutzer] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750979.png "Erstellen Sie Benutzer")  

2.  Führen Sie die Aufgabe Integration Systembenutzer erstellen, durch die Angabe eines neuen Benutzers der Integration System einen Benutzernamen und ein Kennwort.  Lassen Sie erfordern neues Kennwort am nächsten Anmelden Option deaktiviert ist, da diese Benutzer programmgesteuert anmelden wird.    
    Lassen Sie die Sitzung in Minuten mit den Standardwert von 0 (null) und des Benutzers Sitzungen Timeout vorzeitig verhindern wird.    

    ![Erstellen von Integration System-Benutzer] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750980.png "Erstellen von Integration System-Benutzer")  

##<a name="creating-a-security-group"></a>Erstellen einer Sicherheitsgruppe

In diesem Lernprogramm beschriebenen Szenario müssen Sie eine Sicherheitsgruppe für beschränkten Integration System erstellen und den Benutzer zuweisen.    

1.  Geben Sie Sicherheitsgruppe erstellen, in das Suchfeld ein, und klicken Sie dann auf den Link, Sicherheitsgruppe erstellen.     

    ![CreateSecurity Gruppe] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750981.png "CreateSecurity Gruppe")  

2.  Durchführen der Aufgabe Sicherheitsgruppe erstellen.  Wählen Sie Integration System Sicherheitsgruppe – ist kein Hindernis aus der Dropdownliste den Typ des Tenanted Sicherheitsgruppe zu eine Sicherheitsgruppe zu erstellen, zu denen Mitglieder explizit hinzugefügt werden.     

    ![CreateSecurity Gruppe] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750982.png "CreateSecurity Gruppe")  

##<a name="assigning-the-integration-system-user-to-the-security-group"></a>Zuweisen des Integration Systembenutzers zur Sicherheitsgruppe

1.  Geben Sie in das Suchfeld Sicherheitsgruppe bearbeiten, und klicken Sie dann auf den Link, **Sicherheitsgruppe bearbeiten**.     

    ![Sicherheitsgruppe bearbeiten] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750983.png "Sicherheitsgruppe bearbeiten")  

2.  Suchen nach, und wählen Sie die neue Integration Sicherheitsgruppe anhand des Namens    

    ![Sicherheitsgruppe bearbeiten] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750984.png "Sicherheitsgruppe bearbeiten")  

3.  Den neue Integration System-Benutzer zu der neuen Sicherheitsgruppe hinzufügen.       

    ![System-Sicherheitsgruppe] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750985.png "System-Sicherheitsgruppe")  

##<a name="configuring-security-group-options"></a>Konfigurieren von Sicherheitsoptionen für die Gruppe

In diesem Schritt gewähren Sie den neuen Sicherheit Gruppenberechtigungen für abrufen und sich Vorgänge für die Objekte, die durch die folgenden Richtlinien der Domäne Sicherheit gesichert:  

-   Externe Konto bereitgestellt.  
-   Worker Daten: Öffentliche Worker-Berichte  
-   Worker Daten: Alle Positionen  
-   Worker Daten: Aktuelle Informationen Personal  
-   Worker Daten: Titel (geschäftlich) klicken Sie auf dieses Profil  

&nbsp;  

1.  Geben Sie im Suchfeld auf die Domäne Sicherheitsrichtlinien, und klicken Sie dann auf den Link, Domäne Sicherheitsrichtlinien für funktionsübergreifendes Bereich.     

    ![Richtlinien der Domäne Sicherheit] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750986.png "Richtlinien der Domäne Sicherheit")  

2.  Suchen nach System, und wählen Sie den System funktionsübergreifendes Bereich.  Klicken Sie auf die Schaltfläche gekennzeichnet, OK.     

    ![Richtlinien der Domäne Sicherheit] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750987.png "Richtlinien der Domäne Sicherheit")  

3.  Klicken Sie in der Liste der Sicherheitsrichtlinien für das System funktionsübergreifendes Bereich erweitern Sie Security Administration, und wählen Sie Sicherheitsrichtlinie der Domäne externe Konto bereitgestellt.     

    ![Richtlinien der Domäne Sicherheit] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750988.png "Richtlinien der Domäne Sicherheit")  

4.  Klicken Sie auf die Schaltfläche Berechtigungen bearbeiten, und klicken Sie dann auf dem Bildschirm Berechtigungen bearbeiten in der Liste der Sicherheitsgruppen mit Berechtigungen für die Integration von abrufen und sich fügen Sie die neuen Sicherheitsgruppe hinzu.     

    ![Bearbeiten von Berechtigungen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750989.png "Bearbeiten von Berechtigungen")  

5.  Wiederholen Sie Schritt 1, oben, um zum Bildschirm zum Auswählen von Funktionsbereichen, zurückzukehren und diesmal, suchen Sie nach Personal, wählen Sie den funktionsübergreifendes Personal-Bereich, und klicken Sie auf die Schaltfläche gekennzeichnet, OK.    

    ![Richtlinien der Domäne Sicherheit] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750990.png "Richtlinien der Domäne Sicherheit")  

6.  Erweitern Sie in der Liste der Sicherheitsrichtlinien für den Personal funktionsübergreifendes Bereich Worker Daten: Personal, und wiederholen Sie Schritt 4 oben für jedes dieser verbleibende Sicherheitsrichtlinien:    

    -   Worker Daten: Öffentliche Worker-Berichte  
    -   Worker Daten: Alle Positionen  
    -   Worker Daten: Aktuelle Informationen Personal  
    -   Worker Daten: Titel (geschäftlich) klicken Sie auf dieses Profil    

    ![Richtlinien der Domäne Sicherheit] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750991.png "Richtlinien der Domäne Sicherheit")  

##<a name="activating-security-policy-changes"></a>Aktivieren die Richtlinie Änderungen Sicherheit

1.  Geben Sie in das Suchfeld aktivieren, und klicken Sie dann auf den Link, ausstehende Änderungen für Sicherheit aktivieren.    

    ![Aktivieren] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750992.png "Aktivieren")  

2.   Beginnen Sie den Vorgang ausstehende Änderungen für Sicherheit aktivieren durch Eingeben eines Kommentars ü Zwecken, und klicken Sie dann auf die Schaltfläche gekennzeichnet, OK aus.      

    ![Ausstehend Sicherheit aktivieren] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750993.png "Ausstehend Sicherheit aktivieren")  

3.  Führen Sie die Aufgabe auf dem nächsten Bildschirm durch Aktivieren des Kontrollkästchens gekennzeichnet bestätigen und dann auf die Schaltfläche gekennzeichnet, OK aus.     

    ![Ausstehend Sicherheit aktivieren] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750994.png "Ausstehend Sicherheit aktivieren")  

##<a name="configuring-user-import-in-microsoft-azure-ad"></a>Konfigurieren von Benutzer importieren in Microsoft Azure AD

Das Ziel dieses Abschnitts ist so konfigurieren Sie Microsoft Azure AD zum Importieren von Personen aus Arbeitstag gliedern.    

###<a name="to-configure-user-import-in-microsoft-azure-ad-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, um Benutzer importieren in Microsoft Azure AD konfigurieren:

1.  Klicken Sie auf der Seite **Arbeitstag** Anwendung Integration auf **Konfigurieren Benutzer importieren** , um das Dialogfeld **Provisioning konfigurieren** zu öffnen.    

2.  Klicken Sie auf der Seite **Einstellungen und Administrator-Anmeldeberechtigungen** führen Sie die folgenden Schritte aus, und klicken Sie dann auf Weiter:    

    ![Einstellungen und Administrator-Anmeldeberechtigungen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750995.png "Einstellungen und Administrator-Anmeldeberechtigungen")    

    1.  Geben Sie in das Textfeld **Arbeitstag Admin-Benutzername** den Namen des Benutzers, den Sie im Abschnitt [Erstellen eines Benutzers mit Integration System](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) erstellt haben.    
    2.  Geben Sie das Kennwort des Benutzers, den Sie im Abschnitt [Erstellen eines Benutzers mit Integration System](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) erstellt haben, in das Textfeld **Arbeitstag Administratorkennworts** .    
    3.  Geben Sie in das Textfeld **Arbeitstag Mandanten URL** die URL oder Ihrem Mandanten Arbeitstag aus.    

3.  Klicken Sie auf der Seite **Verbindung testen** klicken Sie auf **Test zu starten** , um die Verbindung zu bestätigen, und klicken Sie dann auf **Weiter**.    

    ![Verbindung testen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750996.png "Verbindung testen")  

4.  Klicken Sie auf der Optionsseite **Provisioning** klicken Sie auf **Weiter**.    

    ![Bereitstellung von Optionen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750997.png "Bereitstellung von Optionen")  

5.  Klicken Sie auf **abgeschlossen**, klicken Sie im Dialogfeld **Starten bereitgestellt** .    

    ![Starten der Bereitstellung] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750998.png "Starten der Bereitstellung")  

Sie können jetzt wechseln Sie zum Abschnitt **Benutzer** und überprüfen Sie, ob Ihre Benutzer Arbeitstag importiert wurde.    
