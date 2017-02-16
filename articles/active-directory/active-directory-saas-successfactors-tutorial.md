<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in SuccessFactors | Microsoft Azure"
    description="Informationen Sie zur Verwendung von SuccessFactors mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />


#<a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Lernprogramm: Azure-Active Directory-Integration in SuccessFactors
  
Ziel dieses Lernprogramms ist es zu zeigen, wie Sie SuccessFactors mit Azure Active Directory (Azure AD) integrieren.

Integration von SuccessFactors mit Azure AD bietet Ihnen die folgenden Vorteile:

- Sie können in Azure AD steuern, die auf SuccessFactors zugreifen
- Sie können Ihre Benutzer automatisch auf SuccessFactors (einmaliges Anmelden) mit ihren Konten Azure AD-angemeldete abrufen aktivieren.
- Sie können Ihre Konten an einem zentralen Ort – im klassischen Azure-Portal verwalten.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Konfigurieren von Azure AD-Integration mit SuccessFactors, benötigen Sie die folgenden Elemente:

- Ein gültiges Azure-Abonnement
- Einen Mandanten in SuccessFactors


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten.


## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei Hauptfenster Bausteine:

1. Hinzufügen von SuccessFactors aus dem Katalog
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-successfactors-from-the-gallery"></a>Hinzufügen von SuccessFactors aus dem Katalog
Um die Integration der SuccessFactors in Azure AD zu konfigurieren, müssen Sie SuccessFactors zu Ihrer Liste der verwalteten SaaS apps aus dem Katalog hinzuzufügen.

**Um SuccessFactors aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1.  Klicken Sie auf der linken Navigationsbereich im Azure klassischen Portal auf **Active Directory**.

    ![Konfigurieren einmaliges Anmelden][1]

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Konfigurieren einmaliges Anmelden][2]

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3]

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Konfigurieren einmaliges Anmelden][4]

6.  Geben Sie im **Suchfeld** **SuccessFactors**.

    ![Konfigurieren einmaliges Anmelden][5]

7.  Im Ergebnisfeld **SuccessFactors**wählen Sie aus, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Konfigurieren einmaliges Anmelden][6]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
Das Ziel der in diesem Abschnitt ist erläutert, wie Sie konfigurieren und Testen der Azure AD-einmaliges Anmelden mit SuccessFactors basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück SuccessFactors an einen Benutzer in Azure AD ist. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SuccessFactors eingerichtet werden.

Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in SuccessFactors zuweisen.

Zum Konfigurieren und Azure AD-einmaliges Anmelden mit SuccessFactors testen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer SuccessFactors Benutzer testen](#creating-a-successfactors-test-user)** : ein Gegenstück von Britta Simon in SuccessFactors haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
4. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD-einmaliges Anmelden
  
In diesem Abschnitt Azure AD-einmaliges Anmelden im klassischen Portal aktivieren und konfigurieren in Ihrer Anwendung SuccessFactors einmaliges Anmelden.

**Führen Sie die folgenden Schritte aus, um Azure AD-einmaliges Anmelden mit SuccessFactors konfigurieren:**

1.  Im Azure klassischen-Portal auf der Seite **SuccessFactors** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden][7]

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der SuccessFactors auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden][8]

3.  Klicken Sie auf der Seite **App-URL konfigurieren** führen Sie die folgenden Schritte aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden][9]

    ein. Geben Sie in das Textfeld **Melden Sie sich auf URL** eine URL, die mit einer der folgenden Muster: 

  	|                                                            |
  	| ---                                                        |
  	| `https://<company name>.successfactors.com/<company name>` |
  	| `https://<company name>.sapsf.com/<company name>`          |
  	| `https://<company name>.successfactors.eu/<company name>`  |
  	| `https://<company name>.sapsf.eu`                          |

    b. Geben Sie im Textfeld **URL Antworten** einer URL mithilfe einer der folgenden Muster: 
    
  	|                                                            |
  	| ---                                                        |
  	| `https://<company name>.successfactors.com/<company name>` |
  	| `https://<company name>.sapsf.com/<company name>`          |
  	| `https://<company name>.successfactors.eu/<company name>`  |
  	| `https://<company name>.sapsf.eu`                          |
  	| `https://<company name>.sapsf.eu/<company name>`           |

    c. Klicken Sie auf **Weiter**. 


    > [AZURE.TIP] Bitte beachten Sie, dass diese nicht die tatsächlichen Werte sind. Sie müssen diese Werte mit den tatsächlichen melden Sie sich auf URL und Antworten URL zu aktualisieren. Um diese Werte zu erhalten, wenden Sie sich an [SuccessFactors Supportteam](https://www.successfactors.com/en_us/support.html).

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei SuccessFactors** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden][10]

5.  In einem anderen Webbrowserfenster melden Sie sich bei der **Administratorportal SuccessFactors** als Administrator.

6.  Besuchen Sie die **Anwendung Sicherheit** und einheitlichen mit **Bestimmten melden Sie sich auf Feature**. 

7. Platzieren Sie einen beliebigen Wert in **Token zurücksetzen** , und klicken Sie auf **Speichern Token** zum Aktivieren von SAML-SSO.

    ![Konfigurieren der app auf einmaliges Anmelden][11]


    > [AZURE.NOTE] Dieser Wert wird nur als den Ein-/Ausschalter verwendet. Wenn Sie einen beliebigen Wert gespeichert ist, ist der SAML SSO. Wenn ein leerer Wert gespeichert wurde ist der SAML SSO deaktiviert.

8. Einer systemeigenen unter Screenshot und die folgenden Aktionen ausführen.

    ![Konfigurieren der app auf einmaliges Anmelden][12]

    ein. Aktivieren Sie das Optionsfeld **SAML Version 2 SSO**
    
    b. Festlegen der SAML bestätigen Partei Name(e.g. SAml issuer + company name).

    c. In der **SAML-Herausgeber** setzen Textfeld den Wert des **Herausgebers URL** aus Azure AD-Anwendung Kontokonfigurations-Assistenten aus.

    d. Wählen Sie die **Antwort (Kunden generiert/IdP/AP)** als **obligatorisch Signatur erforderlich**.

    e. Wählen Sie als **SAML-Kennzeichnung aktivieren** **aktiviert** .

    f. Wählen Sie **Nein** , als **Login Anforderungssignatur (AE generiert/SP/RP)**.

    g. Wählen Sie **Browser/Beitrag Profil** als **SAML-Profil**ein.

    h. Wählen Sie **Nein** aus, als **gültige Zertifikats erzwingen**.

    Ich. Kopieren Sie den Inhalt der heruntergeladenen Zertifikatsdatei, und fügen Sie ihn in das Textfeld **SAML-Überprüfung Zertifikat** .


    > [AZURE.NOTE] Der Zertifikatsinhalt muss haben beginnen Zertifikats- und Ende Zertifikat Kategorien.

9. Navigieren Sie zu der SAML-Version 2, und gehen Sie folgendermaßen vor:

    ![Konfigurieren der app auf einmaliges Anmelden][13]

    ein. Wählen Sie **Ja** als **Unterstützung SP initiiert globale Abmelden**.

    b. In der **Globalen Abmeldung Dienst-URL (LogoutRequest Ziel)** setzen Textfeld den Wert des **Remote Abmeldung URL** aus Azure AD-Anwendung Kontokonfigurations-Assistenten aus.

    c. Wählen Sie **Nein** aus, als **sp verschlüsseln müssen Sie alle NameID Element erforderlich**.

    d. Wählen Sie als **NameID Format** **nicht angegeben** .

    e. Wählen Sie **Ja** aus, als **sp-initiiert Login (AuthnRequest) aktivieren**.

    f. In der **Anforderung als unternehmensweiten Herausgeber senden** setzen Textfeld den Wert der **Remote-Anmelde-URL** aus Azure AD-Anwendung Kontokonfigurations-Assistenten.

10.  Führen Sie folgende Schritte aus, wenn Sie die Benutzernamen Login vornehmen möchten und Kleinschreibung.
    
    a.Visit **Unternehmen Einstellungen**(unten).
    
    b. Wählen Sie das Kontrollkästchen **Aktivieren nicht Fall sensible Benutzernamen**ein.

    c.Click **Speichern**.
    
    ![Konfigurieren Sie einmaliges Anmelden][29]


    > [AZURE.NOTE] Wenn Sie versuchen, diese zu aktivieren, prüft das System, wenn Sie einen doppelten SAML-Benutzernamen Erstellung. Wenn beispielsweise der Kunden Benutzernamen Benutzer1 und Benutzer1 hat. Entfernung von Groß-/Kleinschreibung wird diese Duplikate. Das System bieten Ihnen eine Fehlermeldung angezeigt, und es wird das Feature nicht aktivieren. Der Kunde muss so ändern Sie eine der die Benutzernamen, damit er anderen tatsächlich geschrieben wurde. 

11.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Applikationen][14]

12. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.

    ![Applikationen][15]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
Das Ziel der in diesem Abschnitt besteht im Erstellen eines Testbenutzers im klassischen Portal Britta Simon bezeichnet.

![Erstellen von Azure AD-Benutzer][16]

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Benutzers mit Azure AD-testen][17]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.
    
    ![Erstellen eines Benutzers mit Azure AD-testen][18]

4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**.

    ![Erstellen eines Benutzers mit Azure AD-testen][19]

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen][20]

    ein. Wählen Sie als Typ des Benutzers neuen Benutzer in Ihrer Organisation ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf **Weiter**.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus:
    
    ![Erstellen eines Benutzers mit Azure AD-testen][21]

    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. In das letzte Textfeld **Name** , Typ, **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.
    
    ![Erstellen eines Benutzers mit Azure AD-testen][22]

8. Führen Sie auf der Seite **erste temporäres Kennwort** die folgenden Schritte aus:
    
    ![Erstellen eines Benutzers mit Azure AD-testen][23]

    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.

    b. Klicken Sie auf **abgeschlossen**.  



### <a name="creating-a-successfactors-test-user"></a>Erstellen eines Testbenutzers SuccessFactors
  
Um Azure AD-Benutzern zur Anmeldung bei SuccessFactors zu ermöglichen, müssen er in SuccessFactors bereitgestellt werden.  
Im Falle von SuccessFactors ist die Bereitstellung eine manuelle Aufgabe.
  
Wenn erstellt in SuccessFactors Benutzer erhalten möchten, müssen Sie die [SuccessFactors Supportteam](https://www.successfactors.com/en_us/support.html)wenden Sie sich an.



### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

Das Ziel der in diesem Abschnitt ist für die Aktivierung der Britta Simon Azure einmaliges Anmelden verwenden, indem Sie keinen Zugriff auf SuccessFactors erteilen.
    
![Benutzer zuweisen][24]

**Um Britta Simon SuccessFactors zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.
    
    ![Benutzer zuweisen][25]

2. Wählen Sie in der Liste Applications **SuccessFactors**.
    
    ![Konfigurieren Sie einmaliges Anmelden][26]

3. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.
    
    ![Benutzer zuweisen][27]

4. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

5. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.
    
    ![Benutzer zuweisen][28]



### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.
 
Wenn Sie die Kachel SuccessFactors im Bereich Access klicken, Sie sollten automatisch an Ihrer Anwendung SuccessFactors angemeldete abrufen.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[0]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_00.png
[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_01.png
[6]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_02.png
[7]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_03.png
[8]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_04.png
[9]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_05.png
[10]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_06.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png

[16]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_00.png
[17]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png
[18]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png
[19]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png
[20]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png
[21]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_05.png
[22]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_06.png
[23]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_07.png

[24]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_07.png
[25]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_08.png
[26]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_11.png
[27]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_09.png
[28]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_10.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png
