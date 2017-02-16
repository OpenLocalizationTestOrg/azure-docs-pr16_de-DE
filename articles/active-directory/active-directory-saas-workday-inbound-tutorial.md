<properties 
    pageTitle="Lernprogramm: Konfigurieren von Arbeitstag für eingehende Synchronisierung | Microsoft Azure" 
    description="Informationen Sie zum Verwenden der Arbeitstag als Datenquelle Identität für Azure Active Directory." 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />


#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Lernprogramm: Konfigurieren von Arbeitstag für eingehende Synchronisierung


Ziel dieses Lernprogramms ist es, die Sie die Schritte anzeigen, die Sie in der Arbeitstag und Azure AD-zum Importieren von Personen aus Arbeitstag zu Azure AD ausführen müssen. 

Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure AD Premium-Abonnement
-   Einen Mandanten in Arbeitstag
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1. Aktivieren die Anwendungsintegration für Arbeitstag 


2. Erstellen eines Benutzers mit Integration system 


3. Erstellen einer Sicherheitsgruppe 


4. Zuweisen des Integration Systembenutzers zur Sicherheitsgruppe 


5. Konfigurieren von Sicherheitsoptionen für die Gruppe 


6. Aktivieren die Richtlinie Änderungen Sicherheit 


7. Konfigurieren von Benutzer importieren in Azure AD- 



##<a name="enabling-the-application-integration-for-workday"></a>Aktivieren die Anwendungsintegration für Arbeitstag
  
Ziel dieses Abschnitts ist es, wie die Telefonintegration für Arbeitstag der Anwendung gliedern.

### <a name="steps"></a>Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-workday-inbound-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-workday-inbound-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-workday-inbound-tutorial/IC749321.png "Anwendung hinzufügen")

  
5. Geben Sie in das Suchfeld ein **Arbeitstag**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-workday-inbound-tutorial/IC701021.png "Hinzufügen einer Anwendung von gallerry")

6. Im Ergebnisbereich Arbeitstag wählen Sie aus, und klicken Sie dann auf zum Hinzufügen der Anwendungs abgeschlossen.

    ![Katalog der Anwendung] (./media/active-directory-saas-workday-inbound-tutorial/IC701022.png "Katalog der Anwendung")





## <a name="creating-an-integration-system-user"></a>Erstellen eines Benutzers mit Integration system

### <a name="steps"></a>Schritte aus:


1. Geben Sie die **Arbeitstag Arbeitsfläche**, erstellen Sie Benutzer in das Suchfeld, und klicken Sie dann auf **Integration Systembenutzer erstellen**. 

    ![Benutzer erstellen] (./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "Benutzer erstellen")



2. Führen Sie die Aufgabe **Integration Systembenutzer erstellen** , durch die Angabe eines neuen Benutzers der Integration System einen Benutzernamen und ein Kennwort.  Lassen Sie erfordern neues Kennwort am nächsten Anmelden Option deaktiviert ist, da diese Benutzer programmgesteuert anmelden wird. Lassen Sie die Sitzung in Minuten mit den Standardwert von 0 (null) und des Benutzers Sitzungen Timeout vorzeitig verhindern wird. 

    ![Erstellen von Integration System-Benutzer] (./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "Erstellen von Integration System-Benutzer")
 




## <a name="creating-a-security-group"></a>Erstellen einer Sicherheitsgruppe

In diesem Lernprogramm beschriebenen Szenario müssen Sie eine Sicherheitsgruppe für beschränkten Integration System erstellen und den Benutzer zuweisen.

### <a name="steps"></a>Schritte aus:

1. Geben Sie Sicherheitsgruppe in das Suchfeld erstellen, und klicken Sie dann auf **Sicherheitsgruppe erstellen**. 

    ![CreateSecurity Gruppe] (./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity Gruppe")
 

2. Durchführen der Aufgabe Sicherheitsgruppe erstellen.  Wählen Sie Integration System Sicherheitsgruppe – ist kein Hindernis aus der Dropdownliste den Typ des Tenanted Sicherheitsgruppe zu eine Sicherheitsgruppe zu erstellen, zu denen Mitglieder explizit hinzugefügt werden. 

    ![CreateSecurity Gruppe] (./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity Gruppe")
 



## <a name="assigning-the-integration-system-user-to-the-security-group"></a>Zuweisen des Integration Systembenutzers zur Sicherheitsgruppe

### <a name="steps"></a>Schritte aus:


1. Geben Sie Sicherheitsgruppe bearbeiten in das Suchfeld ein, und klicken Sie dann auf **Sicherheitsgruppe bearbeiten**. 

    ![Sicherheitsgruppe bearbeiten] (./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "Sicherheitsgruppe bearbeiten")
 
 

2. Suchen nach, und wählen Sie die neue Integration Sicherheitsgruppe anhand des Namens. 

    ![Sicherheitsgruppe bearbeiten] (./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "Sicherheitsgruppe bearbeiten")

 

3. Den neue Integration System-Benutzer zu der neuen Sicherheitsgruppe hinzufügen. 

    ![System-Sicherheitsgruppe] (./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "System-Sicherheitsgruppe")  




## <a name="configuring-security-group-options"></a>Konfigurieren von Sicherheitsoptionen für die Gruppe

In diesem Schritt gewähren Sie den neuen Sicherheit Gruppenberechtigungen für **Abrufen** , und **setzen Sie** Vorgänge für die Objekte, die durch die folgenden Richtlinien der Domäne Sicherheit gesichert:

- Externe Konto bereitgestellt.

- Worker Daten: Öffentliche Worker-Berichte

- Worker Daten: Alle Positionen

- Worker Daten: Aktuelle Informationen Personal

- Worker Daten: Titel (geschäftlich) klicken Sie auf dieses Profil

 
### <a name="steps"></a>Schritte aus:

1. Geben Sie im Suchfeld auf die Domäne Sicherheitsrichtlinien, und klicken Sie dann auf den Link, Domäne Sicherheitsrichtlinien für funktionsübergreifendes Bereich.  

    ![Richtlinien der Domäne Sicherheit] (./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "Richtlinien der Domäne Sicherheit")  
 

2. Suchen nach System, und wählen Sie den **System** funktionsübergreifendes Bereich.  Klicken Sie auf **OK**.  

    ![Richtlinien der Domäne Sicherheit] (./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "Richtlinien der Domäne Sicherheit")  


3. Klicken Sie in der Liste der Sicherheitsrichtlinien für das System funktionsübergreifendes Bereich erweitern Sie Security Administration, und wählen Sie Sicherheitsrichtlinie der Domäne externe Konto bereitgestellt.  

    ![Richtlinien der Domäne Sicherheit] (./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "Richtlinien der Domäne Sicherheit")  


4. Klicken Sie auf **Berechtigungen bearbeiten**, und klicken Sie dann auf der Seite **Berechtigungen bearbeiten**Dialogfeld zur Liste der Sicherheitsgruppen mit **Abrufen** und **setzen** Integration Berechtigungen fügen Sie die neuen Sicherheitsgruppe hinzu. 

    ![Bearbeiten von Berechtigungen] (./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "Bearbeiten von Berechtigungen")  

 

5. Wiederholen Sie Schritt 1, oben, um zum Bildschirm zum Auswählen von Funktionsbereichen, zurückzukehren und diesmal Suche für Personal, den funktionsübergreifendes Personal-Bereich auszuwählen, und klicken Sie auf **OK**.

    ![Richtlinien der Domäne Sicherheit] (./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "Richtlinien der Domäne Sicherheit")  
 

6. Erweitern Sie in der Liste der Sicherheitsrichtlinien für den Personal funktionsübergreifendes Bereich Worker Daten: Personal, und wiederholen Sie Schritt 4 oben für jedes dieser verbleibende Sicherheitsrichtlinien:

     - Worker Daten: Öffentliche Worker-Berichte

     - Worker Daten: Alle Positionen

     - Worker Daten: Aktuelle Informationen Personal

     - Worker Daten: Titel (geschäftlich) klicken Sie auf dieses Profil


    ![Richtlinien der Domäne Sicherheit] (./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "Richtlinien der Domäne Sicherheit")  







## <a name="activating-security-policy-changes"></a>Aktivieren die Richtlinie Änderungen Sicherheit

### <a name="steps"></a>Schritte aus:

1. Geben Sie in das Suchfeld aktivieren, und klicken Sie dann auf den Link, ausstehende Änderungen für Sicherheit aktivieren. 

    ![Aktivieren] (./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "Aktivieren") 
 

2. Beginnen Sie den Vorgang steht noch aus Änderungen für Sicherheit aktivieren durch Eingeben eines Kommentars ü Zwecken, und klicken Sie dann auf **OK**. 

    ![Ausstehend Sicherheit aktivieren] (./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "Ausstehend Sicherheit aktivieren")   
 

3. Führen Sie die Aufgabe auf dem nächsten Bildschirm durch Aktivieren des Kontrollkästchens bestätigen gekennzeichnet, und klicken Sie dann auf **OK**. 

    ![Ausstehend Sicherheit aktivieren] (./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "Ausstehend Sicherheit aktivieren")  





## <a name="configuring-user-import-in-azure-ad"></a>Konfigurieren von Benutzer importieren in Azure AD-

In diesem Abschnitt Ziel ist es zu gliedernden Azure AD zum Importieren von Personen aus Arbeitstag konfigurieren.


### <a name="steps"></a>Schritte aus:


1. Klicken Sie auf der Seite **Arbeitstag** Anwendung Integration auf **Konfigurieren Benutzer importieren** , um das Dialogfeld **Provisioning konfigurieren** zu öffnen.


2. Klicken Sie auf der Seite **Einstellungen und Administrator-Anmeldeberechtigungen** führen Sie die folgenden Schritte aus, und klicken Sie dann auf **Weiter**: 

    ![Einstellungen und Administrator-Anmeldeberechtigungen] (./media/active-directory-saas-workday-inbound-tutorial/IC750995.png "Einstellungen und Administrator-Anmeldeberechtigungen")  
 
    ein. Arbeitstag Administrator Textfeld für den Benutzernamen, geben Sie den Namen des Benutzers, die Sie, in der erstellen erstellt haben im Benutzerabschnitt eine Integration-System.

    b. Geben Sie im Textfeld Kennwort Arbeitstag Administrator das Kennwort des Benutzers, die Sie, in der erstellen erstellt haben im Benutzerabschnitt eine Integration-System.

    c. Geben Sie im Textfeld Arbeitstag Mandanten URL die URL oder Ihrem Mandanten Arbeitstag aus.


3. Klicken Sie auf der Seite **Verbindung testen** klicken Sie auf **Test zu starten** , um die Verbindung zu bestätigen, und klicken Sie dann auf **Weiter**. 

    ![Verbindung testen] (./media/active-directory-saas-workday-inbound-tutorial/IC750996.png "Verbindung testen")  
 

4. Klicken Sie auf der Optionsseite **Provisioning** klicken Sie auf **Weiter**. 

    ![Bereitstellung von Optionen] (./media/active-directory-saas-workday-inbound-tutorial/IC750997.png "Bereitstellung von Optionen")


5. Klicken Sie auf **abgeschlossen**, klicken Sie im Dialogfeld **Starten bereitgestellt** . 

    ![Starten der Bereitstellung] (./media/active-directory-saas-workday-inbound-tutorial/IC750998.png "Starten der Bereitstellung")
 

Sie können jetzt wechseln Sie zum Abschnitt **Benutzer** und überprüfen Sie, ob Ihre Benutzer Arbeitstag importiert wurde.



## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
