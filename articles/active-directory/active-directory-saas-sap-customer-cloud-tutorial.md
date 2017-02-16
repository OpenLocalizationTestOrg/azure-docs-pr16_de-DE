<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration mit SAP-Cloud für Kunden | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren der einmaligen Anmeldens zwischen Azure Active Directory und SAP-Cloud für Kunden."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a>Lernprogramm: Azure-Active Directory-Integration mit SAP-Cloud für Kunden

In diesem Lernprogramm erfahren Sie, wie SAP-Cloud für Kunden mit Azure Active Directory (Azure AD) integriert werden soll.

Integrieren von SAP-Cloud für Kunden mit Azure AD bietet Ihnen die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf SAP-Cloud für Kunden hat
- Sie können Ihre Benutzer automatisch angemeldet-SAP-Cloud für Kunden (einmaliges Anmelden) mit ihren Azure AD-Konten auf erste aktivieren.
- Sie können Ihre Konten an einem zentralen Ort – im klassischen Azure-Portal verwalten.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Konfigurieren von Azure AD-Integration mit SAP-Cloud für Kunden, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Ein SAP-Cloud für Kunden einmaligen Anmeldung aktiviert Abonnement


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten.


## <a name="scenario-description"></a>Szenario Beschreibung
In diesem Lernprogramm testen Sie Azure AD-einmaliges Anmelden in einer testumgebung.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei Hauptfenster Bausteine:

1. Hinzufügen von SAP-Cloud für Kunden aus dem Katalog
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a>Hinzufügen von SAP-Cloud für Kunden aus dem Katalog
Um die Integration von SAP-Cloud für Kunden in Azure AD konfigurieren zu können, müssen Sie SAP-Cloud für Kunden aus dem Katalog zu Ihrer Liste der verwalteten SaaS apps hinzufügen.

**SAP-Cloud für Kunden aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie in das Suchfeld ein **SAP-Cloud für Kunden**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_01.png)

7. Wählen Sie im Ergebnisbereich **SAP-Cloud für Kunden**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Active Directory](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_02.png)



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden

In diesem Abschnitt Konfigurieren und Testen Azure AD-einmaliges Anmelden mit SAP-Cloud für Kunden auf Basis eines Testbenutzers "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück SAP-Cloud für Kunden in Azure AD einem Benutzer ist. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SAP-Cloud für Kunden eingerichtet werden.

Zum Konfigurieren und Azure AD-einmaliges Anmelden mit SAP-Cloud für Kunden testen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen eines SAP-Cloud für Kunden Benutzer testen](#creating-an-sap-business-bydesign-test-user)** : ein Gegenstück von Britta Simon in SAP-Cloud für Kunden haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
4. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD-einmaliges Anmelden

In diesem Abschnitt Azure AD-einmaliges Anmelden im klassischen Portal aktivieren und konfigurieren in der Cloud SAP für Kunden Anwendung einmaliges Anmelden. 


**Azure AD-einmaliges Anmelden mit SAP-Cloud für Kunden so konfigurieren Sie die folgenden Schritte aus:**


1. Im klassischen Azure Portal **SAP-Cloud für Kunden** Anwendung Integration in die Seite, klicken Sie im Menü oben klicken Sie auf **Attribute**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_80.png) 


2. Klicken Sie in der Liste der Attribute SAML-token Attributen wählen Sie das Attribut Nameidentifier, und klicken Sie dann auf **Bearbeiten**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_84.png) 

3. Klicken Sie im Dialogfeld **Benutzerattribut bearbeiten** führen Sie die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_85.png) 


    ein. Wählen Sie aus der Liste **Attributwert** der **ExtractMailPrefix()** Fuction ein.
    
    b. Wählen Sie in der Liste **E-Mail** Benutzer-Attribut, die, das Sie für Ihre Implementierung verwenden möchten. 
    Angenommen, wenn EmployeeID als eindeutige Benutzer-ID verwenden möchten, und Sie den Attributwert in der ExtensionAttribute2 gespeichert haben, wählen Sie dann user.extensionattribute2. 

    c. Klicken Sie auf **abgeschlossen**. 
    

4. Klicken Sie im Portal klassischen auf der Seite Anwendung Integration **SAP-Cloud für Kunden** auf **Konfigurieren einmaliges Anmelden**.
     
    ![Konfigurieren Sie einmaliges Anmelden][6] 

5. Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der SAP-Cloud für Kunden auf** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_03.png) 

6. Führen Sie auf der Seite Dialogfeld **Konfigurieren der App-Einstellungen** die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_04.png) 

    ein. Geben Sie in das Textfeld **Melden Sie sich auf URL** die URL für Kunden-Anwendung, die mit dem folgenden Muster durch Ihre Benutzer melden Sie sich für den Zugriff auf Ihre SAP-Cloud verwendet:`https://<server name>.crm.ondemand.com`
    
    b. Klicken Sie auf **Weiter**
 
7. Führen Sie auf der Seite **Konfigurieren einmaliges Anmelden bei SAP-Cloud für Kunden** die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_05.png)

    ein. Klicken Sie auf **Herunterladen von Metadaten**aus, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


8. Um SSO konfiguriert zu erhalten, führen Sie die folgenden Schritte aus:

    ein. Melden Sie sich in der Cloud SAP für Kunden-Portal mit Administratorrechten an.

    b. Navigieren Sie zu der **Anwendung und Benutzer Management allgemeine Aufgabe** , und klicken Sie auf der Registerkarte **Identitätsanbieter** .

    c. Klicken Sie auf **Neue Identitätsanbieter** , und wählen Sie die Metadaten-XML-Datei, die Sie vom klassischen Azure-Portal heruntergeladen haben. Durch das Importieren von Metadaten aus, uploads das System automatisch die erforderlichen Signaturzertifikat und Verschlüsselungszertifikats an.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_54.png)

    d. Azure AD erfordert das Element Assertion Consumer Dienst-URL in der SAML-Anforderung, sollten Sie das Kontrollkästchen **Assertion Consumer Service-URL enthalten** .

    e. Klicken Sie auf **einmaliges Anmelden aktivieren**.

    f. Die Änderungen zu speichern.

    g. Klicken Sie auf der Registerkarte **Meine System** .

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_52.png)

    h. Kopieren Sie die **SSO-URL** , und fügen Sie ihn in das Textfeld **Azure AD melden Sie sich auf die URL** .

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_53.png)

    Ich. Geben Sie an, ob der Mitarbeiter manuell zwischen-Befehl mit Benutzer-ID und Ihr Kennwort oder SSO durch Auswählen des **Manuellen Identität Anbieter Auswahl**auswählen kann.

    j. Geben Sie im Abschnitt **SSO-URL** die URL, die durch Ihre Mitarbeiter verwendet werden soll, um das System anzumelden. 
    In der Liste **URL gesendet, die Mitarbeiter** können Sie zwischen den folgenden Optionen auswählen:
    
    **Nicht-SSO-URL**
 
    Das System sendet nur die normalen System-URL für den Mitarbeiter. Der Mitarbeiter kann nicht melden Sie sich über SSO, muss und Trennzeichen Kennwort verwenden oder stattdessen das Zertifikat.

    **SSO-URL** 

    Das System sendet nur die SSO-URL für den Mitarbeiter. Der Mitarbeiter kann sich über SSO anmelden. Anforderung der Authentifizierung wird durch die IdP umgeleitet.

    **Automatische Auswahl**
 
    Wenn SSO nicht aktiviert ist, sendet das System den normalen System-URL für den Mitarbeiter an. Wenn SSO aktiv ist, prüft das System, ob der Mitarbeiter ein Kennwort geschützt ist. Wenn ein Kennwort verfügbar ist, werden sowohl SSO-URL und nicht SSO URL an den Mitarbeiter gesendet. Wenn der Mitarbeiter kein Kennwort hat, wird nur die SSO-URL für den Mitarbeiter gesendet.

    k. Die Änderungen zu speichern.

9. Im Portal klassischen wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

10. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.  
 
    ![Azure AD einmaliges Anmelden][11]


### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
In diesem Abschnitt erstellen Sie einen Testbenutzer aufgerufen Britta Simon im klassischen Azure-Portal an.


![Erstellen von Azure AD-Benutzer][20]

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-sapcloudforcustomer-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-sapcloudforcustomer-tutorial/create_aaduser_03.png) 

4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-sapcloudforcustomer-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus:  ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-sapcloudforcustomer-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ des Benutzers neuen Benutzer in Ihrer Organisation ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf **Weiter**.

6.  Klicken Sie auf der Seite **Benutzerprofil** -Dialogfeld führen Sie die folgenden Schritte aus: ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-sapcloudforcustomer-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. In das letzte Textfeld **Name** , Typ, **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-sapcloudforcustomer-tutorial/create_aaduser_07.png) 

8. Klicken Sie auf der Seite **erste temporäres Kennwort** führen Sie die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-sapcloudforcustomer-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-an-sap-cloud-for-customer-test-user"></a>Erstellen eines SAP-Cloud für Kunden Testbenutzers

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in SAP-Cloud für Kunden. Arbeiten Sie mit SAP-Cloud für Kunden Supportteam um die Benutzer in der Cloud SAP für Kunden Plattform hinzuzufügen. 

> [AZURE.NOTE] Stellen Sie sicher, dass NameID Wert mit dem Username-Feld in der Cloud SAP für Kunden Plattform entsprechen sollen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

In diesem Abschnitt aktivieren Sie Britta Simon Azure einmaliges Anmelden verwenden, indem Sie keinen Zugriff auf SAP-Cloud für Kunden erteilen.

![Benutzer zuweisen][200] 

**Um SAP-Cloud für Kunden Britta Simon zuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste Applikationen **SAP-Cloud für Kunden**aus.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_50.png) 

3. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

5. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

In diesem Abschnitt Testen Sie Ihre Azure AD-einzelne anmelden Konfiguration mit der Access-Systemsteuerung.

Wenn Sie die SAP-Cloud für Kunden Kachel im Bereich Access klicken, Sie sollten automatisch angemeldet-in der Cloud SAP-für Kunden Anwendung auf abrufen.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_205.png
