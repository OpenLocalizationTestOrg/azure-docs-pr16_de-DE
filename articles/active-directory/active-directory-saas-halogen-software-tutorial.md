<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Halogenlampen Software"
    description="Informationen Sie zum Konfigurieren der einmaligen Anmeldens zwischen Azure Active Directory und Halogenlampen Software."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Lernprogramm: Azure-Active Directory-Integration in Halogenlampen Software

Ziel dieses Lernprogramms ist zu veranschaulichen Halogenlampen Software mit Azure Active Directory (Azure AD) integriert werden soll.

Integration Halogenlampen Software mit Azure AD bietet Ihnen die folgenden Vorteile: 

- Sie können in Azure AD steuern, wer auf Halogenlampen Software zugreifen kann 
- Sie können Ihre Benutzer automatisch auf Halogenlampen Software (einmaliges Anmelden) angemeldete Abrufen mit ihren Azure AD-Konten aktivieren.
- Sie können Ihre Konten an einem zentralen Ort – im klassischen Azure-Portal verwalten.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Zum Konfigurieren von Azure AD-Integration mit Halogenlampen Software, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine Halogenlampen Software einmaligen Anmeldung aktiviert Abonnement


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten. 

 
## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können. 

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei Hauptfenster Bausteine:

1. Hinzufügen von Halogenlampen Software aus dem Katalog 
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-halogen-software-from-the-gallery"></a>Hinzufügen von Halogenlampen Software aus dem Katalog
Zum Konfigurieren der Integration von Halogenlampen Software in Azure AD müssen Sie Halogenlampen Software zu Ihrer Liste der verwalteten SaaS apps aus dem Katalog hinzuzufügen.

**Um Halogenlampen Software aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite. 

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie in das Suchfeld **Halogenlampen Software**aus.

    ![Applikationen][5]

7. Wählen Sie im Ergebnisbereich **Halogenlampen Software aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
Das Ziel der in diesem Abschnitt ist erläutert, wie Sie konfigurieren und Testen der Azure AD-einmaliges Anmelden mit Halogenlampen Software auf Grundlage eines Testbenutzers "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück Halogenlampen Software an einen Benutzer in Azure AD ist. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer Halogenlampen Software hergestellt werden.

Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** Halogenlampen Software zuweisen.
 
Zum Konfigurieren und Azure AD-einmaliges Anmelden mit Halogenlampen Software testen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einzelnen einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer Halogenlampen Software testen Benutzer](#creating-a-halogen-software-test-user)** : ein Gegenstück von Britta Simon Halogenlampen Software haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
5. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfigurieren von Azure AD einzelnes einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist Azure AD-einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in Ihrer Anwendung Halogenlampen Software.


**So konfigurieren Sie Azure AD-einmaliges Anmelden mit Halogenlampen Software die folgenden Schritte aus:**

1. Im Azure klassischen-Portal auf der Seite Anwendung Integration **Halogenlampen Software** klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .

    ![Konfigurieren Sie einmaliges Anmelden][8]

2. Klicken Sie auf der Seite **Wie möchten Sie Benutzer Halogenlampen Software auf bei** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][9]

3. Klicken Sie auf der Dialogseite **App-Einstellungen konfigurieren** , gehen Sie folgendermaßen vor:  ![App-Einstellungen konfigurieren][10]
 
     ein. Geben Sie in das Textfeld **Melden Sie sich auf URL** Ihre untersuchten Ihre Benutzer bei Ihrer Halogenlampen Software-Anwendung unter Verwendung des folgenden Musters, klicken Sie auf URL: *https://global.hgncloud.com/fabrikam/welcome.jsp*

     b. Klicken Sie auf **Weiter**.
 
4. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Halogenlampen Software** klicken Sie auf **Herunterladen von Metadaten**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
    
    ![Was ist Azure AD-Verbindung herstellen][11]

5. In einem anderen Browserfenster melden Sie sich für den Zugriff auf Ihre **Software Halogenlampen** Anwendung als Administrator.

6. Klicken Sie auf der Registerkarte **Optionen** . 

    ![Was ist Azure AD-Verbindung herstellen][12]


7. Klicken Sie im linken Navigationsbereich auf **SAML-Konfiguration**. 

    ![Was ist Azure AD-Verbindung herstellen][13]

8. Führen Sie auf der Seite **SAML-Konfiguration** die folgenden Schritte aus:  ![Neuigkeiten Azure AD-verbinden][14]

    ein. Wählen Sie als **Eindeutigen Bezeichner** **NameID**aus.

    b. Wählen Sie als **Eindeutigen Bezeichner Karten zu** **Benutzernamen**ein.

    c. Um die heruntergeladene Metadatendatei hochladen möchten, klicken Sie auf **Durchsuchen** , um die Datei auszuwählen, und klicken Sie dann **Datei hochladen**.

    d. Klicken Sie auf **Test ausführen**, um die Konfiguration zu testen. 

    > [AZURE.NOTE] Sie müssen Sie warten für die Meldung "*der SAML-Test ist abgeschlossen. Schließen Sie die in diesem Fenster*". Klicken Sie dann schließen Sie das geöffnete Browserfenster. Das Kontrollkästchen **SAML aktivieren** ist nur verfügbar, wenn der Test abgeschlossen wurde.

    e. Wählen Sie **SAML aktivieren**.
    
    f. Klicken Sie auf **Änderungen speichern**. 


9. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen. 

    ![Was ist Azure AD-Verbindung herstellen][15]

10. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.  

    ![Was ist Azure AD-Verbindung herstellen][16]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
Das Ziel der in diesem Abschnitt besteht im Erstellen eines Testbenutzers aufgerufen Britta Simon im klassischen Azure-Portal.

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Was ist Azure AD-Verbindung herstellen][100] 

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.

    ![Was ist Azure AD-Verbindung herstellen][101] 

4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**. 

    ![Was ist Azure AD-Verbindung herstellen][102] 

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus:

    ![Was ist Azure AD-Verbindung herstellen][103] 
 
    ein. Wählen Sie als **Typ des Benutzers** **neuen Benutzer in Ihrer Organisation**ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf Weiter.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus: 

    ![Was ist Azure AD-Verbindung herstellen][104] 

    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. In der **Nachname** Txtbox, Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

    ![Was ist Azure AD-Verbindung herstellen][105]  

8. Klicken Sie auf der Seite **erste temporäres Kennwort** führen Sie die folgenden Schritte aus:

    ![Was ist Azure AD-Verbindung herstellen][106]   

    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.
    b. Klicken Sie auf **abgeschlossen**.   
  
 
### <a name="creating-a-halogen-software-test-user"></a>Erstellen eines Testbenutzers Halogenlampen Software

Das Ziel der in diesem Abschnitt ist zum Erstellen eines Benutzers Britta Simon Halogenlampen Software bezeichnet.

**Führen Sie die folgenden Schritte aus, um einen Benutzer namens Britta Simon Halogenlampen Software zu erstellen:**

1. Melden Sie sich auf Ihrer Anwendung **Halogenlampen Software** als Administrator an.

2. Klicken Sie auf der Registerkarte **Benutzer Center** , und klicken Sie dann auf **Benutzer erstellen**.

    ![Was ist Azure AD-Verbindung herstellen][300]  

3. Klicken Sie auf der Seite **Neuer Benutzer** Dialogfeld führen Sie die folgenden Schritte aus:

    ![Was ist Azure AD-Verbindung herstellen][301]

    ein. Geben Sie im Textfeld **Vorname** **Britta**aus. 
  
    b. Geben Sie im Textfeld **Nachname** **Simon**aus.
  
    c. Geben Sie in das Textfeld für den **Benutzernamen** **Benutzernamen im Portal Azure klassischen Brita Simon auch**ein.
  
    d. Geben Sie ein Kennwort in das Textfeld **Kennwort** für Britta.
  
    e. Klicken Sie auf **Speichern**.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

Das Ziel der in diesem Abschnitt ist für die Aktivierung der Britta Simon Azure einmaliges Anmelden verwenden, indem Sie keinen Zugriff auf Software Halogenlampen erteilen.

![Was ist Azure AD-Verbindung herstellen][200]

**Um Britta Simon Halogenlampen Software zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal Azure klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Was ist Azure AD-Verbindung herstellen][201]

2. Wählen Sie in der Liste Applikationen **Halogenlampen Software**aus.

    ![Was ist Azure AD-Verbindung herstellen][202]

1. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Was ist Azure AD-Verbindung herstellen][203]

1. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

    ![Was ist Azure AD-Verbindung herstellen][204]

2. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Was ist Azure AD-Verbindung herstellen][205]



### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.

Wenn Sie die Kachel Halogenlampen Software im Bereich Access klicken, Sie sollten automatisch an Ihrer Anwendung Halogenlampen Software angemeldete abrufen.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_04.png
[5]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_05.png
[6]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_06.png
[7]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_07.png
[8]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_08.png
[9]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_09.png
[10]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_10.png
[11]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_11.png
[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png
[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png
[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png
[15]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_15.png
[16]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_16.png
[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_100.png 
[101]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_101.png 
[102]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_102.png 
[103]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_103.png 
[104]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_104.png 
[105]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_105.png 
[106]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_106.png 
[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_200.png 
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_201.png 
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_203.png
[204]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_204.png
[205]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_205.png
[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png
[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png