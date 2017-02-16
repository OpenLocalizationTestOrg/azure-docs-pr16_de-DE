<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration mit SciQuest verbringen Leiter | Microsoft Azure"
    description="Informationen Sie zum einmaligen Anmeldens zwischen Azure Active Directory und SciQuest verbringen Leiter konfigurieren."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a>Lernprogramm: Azure-Active Directory-Integration mit SciQuest verbringen Leiter

Ziel dieses Lernprogramms ist zu veranschaulichen SciQuest verbringen Leiter mit Azure Active Directory (Azure AD) integriert werden soll.  
Integration von SciQuest verbringen Leiter mit Azure AD bietet Ihnen die folgenden Vorteile: 

- Sie können in Azure AD steuern, die an SciQuest verbringen Leiter zugreifen können 
- Sie können Ihre Benutzer automatisch an SciQuest verbringen Leiter (einmaliges Anmelden) angemeldete Abrufen mit ihren Azure AD-Konten aktivieren.
- Sie können Ihre Konten an einem zentralen Ort – im klassischen Azure-Portal verwalten.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Zum Konfigurieren von Azure AD-Integration mit SciQuest verbringen Leiter, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine SciQuest verbringen Leiter einmaligen Anmeldung aktiviert Abonnement


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten. 

 
## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei Hauptfenster Bausteine:

1. Hinzufügen von SciQuest verbringen Leiter aus dem Katalog 
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-sciquest-spend-director-from-the-gallery"></a>Hinzufügen von SciQuest verbringen Leiter aus dem Katalog
Zum Konfigurieren der Integration von SciQuest verbringen Leiter in Azure AD müssen Sie SciQuest verbringen Leiter zu Ihrer Liste der verwalteten SaaS apps aus dem Katalog hinzuzufügen.

**Wenn SciQuest verbringen Leiter aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie in das Suchfeld **SciQuest verbringen Leiter**aus.

    ![Applikationen][5]

7. Im Bereich Ergebnisse wählen Sie **SciQuest verbringen Leiter aus**und dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Applikationen][6]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
Das Ziel der in diesem Abschnitt ist erläutert, wie Sie konfigurieren und Testen der Azure AD-einmaliges Anmelden mit SciQuest verbringen Leiter basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück SciQuest verbringen Leiter an einen Benutzer in Azure AD ist. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SciQuest verbringen Leiter eingerichtet werden.  
Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in SciQuest verbringen Leiter zuweisen.
 
Zum Konfigurieren und Azure AD-einmaliges Anmelden mit SciQuest verbringen Leiter testen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einzelnen einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer SciQuest verbringen Leiter Benutzer testen](#creating-a-halogen-software-test-user)** : ein Gegenstück von Britta Simon in SciQuest verbringen Leiter haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
5. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfigurieren von Azure AD-Single einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist Azure AD-einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in Ihrer Anwendung SciQuest verbringen Leiter.

**Führen Sie die folgenden Schritte aus, um Azure AD-einmaliges Anmelden mit SciQuest verbringen Leiter konfigurieren:**

1. Im Azure klassischen-Portal auf der Seite **SciQuest verbringen Leiter** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .

    ![Konfigurieren Sie einmaliges Anmelden][8]

2. Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der SciQuest verbringen Leiter auf** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][9]

3. Führen Sie auf der Seite Dialogfeld **Konfigurieren der App-Einstellungen** die folgenden Schritte aus: 

    ![Konfigurieren der App-Einstellungen][10]
 
     3.1. Geben Sie in das Textfeld **Melden Sie sich auf URL** Ihre untersuchten Ihre Benutzer bei Ihrer SciQuest verbringen Leiter-Anwendung unter Verwendung des folgenden Musters, klicken Sie auf URL: *https://.* sciquest.com/.**

     3,2. Geben Sie im Textfeld **URL Antworten** denselben Wert, den Sie in das Textfeld **Melden Sie sich auf URL** eingegeben haben. 

     3.3. Klicken Sie auf **Weiter**.
 
4. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei SciQuest verbringen Leiter** klicken Sie auf **Herunterladen von Metadaten**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.

    ![Was ist Azure AD-Verbindung herstellen][11]

5. Kontakt SciQuest unterstützt diese Authentifizierungsmethode mit den oben angegebenen heruntergeladenen Metadaten zu aktivieren.

6. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen. 

    ![Was ist Azure AD-Verbindung herstellen][15]

10. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.  

    




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

8. Führen Sie auf der Seite **erste temporäres Kennwort** die folgenden Schritte aus:

    ![Was ist Azure AD-Verbindung herstellen][106]   

    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.
  
    b. Klicken Sie auf **abgeschlossen**.   
  
 
### <a name="creating-a-sciquest-spend-director-test-user"></a>Erstellen eines Testbenutzers SciQuest verbringen Leiter

Das Ziel der in diesem Abschnitt ist zum Erstellen eines Benutzers Britta Simon in SciQuest verbringen Leiter bezeichnet.

Sie müssen wenden Sie sich an Ihr Supportteam SciQuest verbringen Leiter, und geben sie die Details zu Ihrem Testkonto Bezugsarten erstellt.

Alternativ können Sie auch in-Time nutzen bereitgestellt, die ein einzelnes anmelden Feature, das von SciQuest verbringen Leiter unterstützt wird.  
Wenn in-Time-Bereitstellung aktiviert ist, werden Benutzer automatisch von SciQuest verbringen Leiter während einem Versuch anmelden erstellt, wenn er nicht vorhanden ist. Dieses Feature entfallen manuell anmelden Gegenstück einzelne Benutzer erstellt.

Zum Abrufen von in-Time-Bereitstellung aktiviert, müssen Sie wenden Sie sich an Ihr Supportteam SciQuest verbringen Leiter Abwesenheit.
  

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

Das Ziel der in diesem Abschnitt ist für die Aktivierung der Britta Simon Azure einmaliges Anmelden verwenden, indem Sie ihre Access an SciQuest verbringen Leiter erteilen.

![Was ist Azure AD-Verbindung herstellen][200]

**Um Britta Simon SciQuest verbringen Leiter zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal Azure klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Was ist Azure AD-Verbindung herstellen][201]

2. Wählen Sie in der Liste Applications **SciQuest verbringen Leiter**aus.

    ![Was ist Azure AD-Verbindung herstellen][202]

1. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Was ist Azure AD-Verbindung herstellen][203]

1. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

    ![Was ist Azure AD-Verbindung herstellen][204]

2. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Was ist Azure AD-Verbindung herstellen][205]



### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.  
Wenn Sie die Kachel SciQuest verbringen Leiter im Bereich Access klicken, Sie sollten automatisch an Ihrer Anwendung SciQuest verbringen Leiter angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_20.png

