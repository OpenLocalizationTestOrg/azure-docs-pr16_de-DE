<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in SilkRoad Nutzungsdauer Suite | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren der einmaligen Anmeldens zwischen Azure Active Directory und SilkRoad Nutzungsdauer Suite."
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
    ms.date="09/19/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Lernprogramm: Azure-Active Directory-Integration in SilkRoad Nutzungsdauer Suite

Ziel dieses Lernprogramms ist zu veranschaulichen SilkRoad Nutzungsdauer Suite mit Azure Active Directory (Azure AD) integriert werden soll.  
Integrieren von Azure AD SilkRoad Nutzungsdauer Suite bietet Ihnen die folgenden Vorteile: 

- Sie können in Azure AD steuern, wer auf SilkRoad Nutzungsdauer Suite zugreifen kann 
- Sie können Ihre Benutzer automatisch auf SilkRoad Nutzungsdauer Suite (einmaliges Anmelden) angemeldete Abrufen mit ihren Azure AD-Konten aktivieren.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Zum Konfigurieren von Azure AD-Integration mit SilkRoad Nutzungsdauer Suite, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine SilkRoad Nutzungsdauer Suite einmaligen Anmeldung aktiviert Abonnement


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten. 

 
## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei Hauptfenster Bausteine:

1. Hinzufügen von SilkRoad Nutzungsdauer Suite aus dem Katalog 
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-silkroad-life-suite-from-the-gallery"></a>Hinzufügen von SilkRoad Nutzungsdauer Suite aus dem Katalog
Zum Konfigurieren der Integration von SilkRoad Nutzungsdauer Suite in Azure AD müssen Sie SilkRoad Nutzungsdauer Suite zu Ihrer Liste der verwalteten SaaS apps aus dem Katalog hinzuzufügen.

**Wenn SilkRoad Nutzungsdauer Suite aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **SilkRoad Nutzungsdauer Suite**aus.

    ![Applikationen][5]

7. Wählen Sie im Ergebnisbereich **SilkRoad Nutzungsdauer Suite**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Applikationen][50]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
Das Ziel der in diesem Abschnitt ist erläutert, wie Sie konfigurieren und Testen der Azure AD-einmaliges Anmelden mit SilkRoad Nutzungsdauer Suite basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück SilkRoad Nutzungsdauer Suite an einen Benutzer in Azure AD ist. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SilkRoad Nutzungsdauer Suite eingerichtet werden.  
Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in SilkRoad Nutzungsdauer Suite zuweisen.
 
Zum Konfigurieren und Azure AD-einmaliges Anmelden mit SilkRoad Nutzungsdauer Suite testen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer SilkRoad Nutzungsdauer Suite Benutzer testen](#creating-a-silkroad-life-suite-test-user)** : ein Gegenstück von Britta Simon in SilkRoad Nutzungsdauer Suite haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
5. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist Azure AD-einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in Ihrer Anwendung SilkRoad Nutzungsdauer Suite.

**Führen Sie die folgenden Schritte aus, um Azure AD-einmaliges Anmelden mit SilkRoad Nutzungsdauer Suite konfigurieren:**

5. Melden Sie sich für den Zugriff auf Ihre SilkRoad Firmenwebsite als Administrator. 


    > [AZURE.NOTE] Um Zugriff auf die Anwendung SilkRoad Nutzungsdauer Suite Authentifizierung zum Konfigurieren von Föderation mit Microsoft Azure AD zu erhalten, wenden Sie sich an SilkRoad Support oder Ihrer Vertreter SilkRoad Dienste.


6. Wechseln Sie zu **Dienstanbieter**, und klicken Sie dann auf **Details Föderation**. 

    ![Azure AD einmaliges Anmelden][10] 


1. Klicken Sie auf **Föderation Metadaten herunterladen**, und klicken Sie dann speichern Sie die Metadatendatei auf Ihrem Computer.

    ![Azure AD einmaliges Anmelden][11] 

3. Im Azure klassischen-Portal auf der Seite **SilkRoad Nutzungsdauer Suite** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .

    ![Konfigurieren Sie einmaliges Anmelden][6] 

2. Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der SilkRoad Nutzungsdauer Suite auf** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][7] 

3. Führen Sie auf der Seite Dialogfeld **Konfigurieren der App-Einstellungen** die folgenden Schritte aus:

    ![Azure AD einmaliges Anmelden][8] 
 
    ein. Geben Sie in das Textfeld **Melden Sie sich auf URL** die URL, die von den Benutzern die Anmeldung bei Ihrer Website SilkRoad Nutzungsdauer Suite verwendet (z. B.: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).

    b. Öffnen der heruntergeladenen **Silkroad** -Metadaten-Datei.

    c. Suchen Sie nach der Kategorie **AssertionConsumerService** , und kopieren Sie das Attribut **Speicherort** .         

    ![Azure AD einmaliges Anmelden][21] 
   
    d. Fügen Sie den Wert in das Textfeld **Antwort-URL** ein.
 
    e. Klicken Sie auf **Weiter**.
 
4. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei SilkRoad Nutzungsdauer Suite** führen Sie die folgenden Schritte aus:

    ![Azure AD einmaliges Anmelden][9] 

    ein. Klicken Sie auf Zertifikat herunterladen, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.




1. Klicken Sie in der Anwendung **SilkRoad** **Authentifizierung-Datenquellen**aus.

    ![Azure AD einmaliges Anmelden][12] 



1. Klicken Sie auf **Authentifizierungsquelle hinzufügen**. 

    ![Azure AD einmaliges Anmelden][13] 



1. Führen Sie im Abschnitt **Authentifizierungsquelle hinzufügen** die folgenden Schritte aus: 

    ![Azure AD einmaliges Anmelden][14] 

    ein. Klicken Sie unter **Option 2 - Metadaten-Datei**klicken Sie auf **Durchsuchen** , um die heruntergeladene Metadatendatei hochladen.

    b. Klicken Sie auf **Dateidaten mithilfe von Identitätsanbieter erstellen**.



1. Klicken Sie im Abschnitt **Authentifizierung Quellen** auf **Bearbeiten**. 

    ![Azure AD einmaliges Anmelden][15] 


1. Führen Sie die folgenden Schritte aus, klicken Sie im Dialogfeld **Authentifizierungsquelle bearbeiten** : 

    ![Azure AD einmaliges Anmelden][16] 

    ein. Als **aktiviert**wählen Sie **Ja**aus.

    b. Geben Sie in das Textfeld **IdP Beschreibung** eine Beschreibung für die Konfiguration (z. B.: *Azure AD SSO*).

    c. Geben Sie in das Textfeld **IdP Name** einen Namen, die auf Ihre Konfiguration spezifisch sind (z. B.: *Azure SP*).

    d. Klicken Sie auf **Speichern**.


6. Deaktivieren Sie alle anderen Authentifizierung Quellen. 

    ![Azure AD einmaliges Anmelden][17]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** im Azure klassischen Portal klicken Sie auf **Weiter**.  

    ![Azure AD einmaliges Anmelden][18]

1. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.

    ![Azure AD einmaliges Anmelden][19]


### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
Das Ziel der in diesem Abschnitt besteht im Erstellen eines Testbenutzers aufgerufen Britta Simon im klassischen Azure-Portal.

![Erstellen von Azure AD-Benutzer][20]

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)  

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) 
 
4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**. 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus: 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)  

    ein. Wählen Sie als Typ des Benutzers neuen Benutzer in Ihrer Organisation ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf **Weiter**.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus: 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png) 
 
    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. In das letzte Textfeld **Name** , Typ, **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) 
 
8. Führen Sie auf der Seite **erste temporäres Kennwort** die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png) 
  
    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.

    b. Klicken Sie auf **abgeschlossen**.   

  
 
### <a name="creating-a-silkroad-life-suite-test-user"></a>Erstellen eines Testbenutzers SilkRoad Nutzungsdauer Suite

Das Ziel der in diesem Abschnitt ist zum Erstellen eines Benutzers Britta Simon in SilkRoad Nutzungsdauer Suite bezeichnet. Brittas müssen eine SSO-ID (auch als eine *AuthParam*bezeichnet), die Brittas **Emailaddress** in Azure AD entspricht.

**Führen Sie die folgenden Schritte aus, um einen Benutzer namens Britta Simon in SilkRoad Nutzungsdauer Suite zu erstellen:**

1. Bitten Sie Ihrem Supportteam SilkRoad Nutzungsdauer Suite zum Erstellen eines Benutzers, das als **SSO-ID-** Attribut den gleichen Wert wie die **Emailaddress** von Britta Simon in Azure AD enthält.



### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

Das Ziel der in diesem Abschnitt ist für die Aktivierung der Britta Simon Azure einmaliges Anmelden verwenden, indem Sie keinen Zugriff auf SilkRoad Nutzungsdauer Suite erteilen.

![Benutzer zuweisen][200] 

**Um Britta Simon SilkRoad Nutzungsdauer Suite zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal Azure klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste Applications **SilkRoad Nutzungsdauer Suite**ein.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

2. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.  
Wenn Sie die Kachel SilkRoad Nutzungsdauer Suite im Bereich Access klicken, Sie sollten automatisch an Ihrer Anwendung SilkRoad Nutzungsdauer Suite angemeldete abrufen.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png





