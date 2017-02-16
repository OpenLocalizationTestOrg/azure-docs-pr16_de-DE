<properties
    pageTitle="So erhalten Sie eine Azure AD-Mandanten | Microsoft Azure"
    description="So erhalten Sie eine Azure Active Directory-Mandanten für die Registrierung und Erstellen von."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/28/2015"
    ms.author="dastrock"/>

# <a name="how-to-get-an-azure-active-directory-tenant"></a>So erhalten Sie eine Azure Active Directory-Mandanten

In Azure Active Directory (Azure AD) wird einen [Mandanten](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) Supportmitarbeiter einer Organisation an.  Es ist eine dedizierte Instanz des Diensts Azure AD-, die eine Organisation erhält und zugegriffen werden, wenn es für ein Microsoft-Cloud-Dienst wie Azure, Microsoft Intune oder Office 365 anmeldet.  Jede Azure AD-Mandanten ist distinct und getrennt von anderen Azure AD-Mandanten.  

Ein Mandanten befinden sich die Benutzer in einem Unternehmen und Informationen dazu benötigen – ihre Kennwörter, Benutzerprofildaten, Berechtigungen und So weiter.  Sie enthält auch Gruppen,-Anwendungen und andere Informationen in Verbindung mit einer Organisation und deren Sicherheit Gruppenrichtlinien.

Damit Azure AD-Benutzer an Ihrer Anwendung anmelden können, müssen Sie die Anwendung in einen Mandanten eigene registrieren.  Veröffentlichen einer Anwendung in einem Azure AD-Mandanten ist ein **kostenlos**.  Tatsächlich erstellt die meisten Entwickler verschiedene Mandanten Applikationen für experimentieren, Entwicklung, und das staging und zu Testzwecken verwenden.  Organisationen, die melden Sie sich für und Ihrer Anwendung nutzen, können optional Lizenzen erwerben, wenn er erweitert Directory-Funktionen nutzen möchten.

Ja, wie gehen Sie dazu, wie Sie einem Azure AD-Mandanten?  Der Prozess möglicherweise eine etwas andere, wenn Sie:

- [Haben Sie ein vorhandenes Office 365-Abonnement](#use-an-existing-office-365-subscription)
- [Haben Sie ein vorhandenes Azure-Abonnement verknüpft ist, mit einem Microsoft-Account](#use-an-msa-azure-subscription)
- [Haben Sie ein vorhandenes Azure-Abonnement verknüpft ist, mit einem organisationskonto](#use-an-organizational-azure-subscription)
- [Haben Sie keine der oben und ganz neu starten möchten](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Verwenden eines vorhandenen Office 365-Abonnements
Wenn Sie ein vorhandenes Office 365-Abonnement verfügen, besitzen Sie bereits eine Azure AD-Mandanten! Sie können [Azure-Portal](https://portal.azure.com) mit Ihrem Office 365-Konto anmelden und erste Schritte mit Azure AD.

## <a name="use-an-msa-azure-subscription"></a>Verwenden eines MSA Azure-Abonnements
Wenn Sie vorher für ein Abonnement Azure mit der einzelnen Microsoft Account angemeldet haben, besitzen Sie bereits einen Mandanten!  Wenn Sie an der [Azure-Portal](https://portal.azure.com)angemeldet haben, werden Sie automatisch zu Ihrem Mandanten Standard angemeldet sein. Sie sind kostenlos dieses Mandanten verwenden, je nach Bedarf -, aber Sie einem organisationsinterne Administratorkonto erstellen möchten.

Gehen Sie dazu folgendermaßen vor.  Sie können alternativ erstellen Sie einen neuen Mandanten, und erstellen einen Administrator in dieser Mandanten folgen einen ähnlichen Prozess.

1.  Melden Sie sich bei der [Azure-Portal](https://portal.azure.com) mit Ihrem Konto einzelne
2.  Navigieren Sie zum Abschnitt "Azure Active Directory" des Portals (finden Sie in der linken Navigationsleiste, klicken Sie unter **Weitere Dienste**)
3.  Sie automatisch bei "Standard Verzeichnis" angemeldet sein sollte, andernfalls können Sie Verzeichnisse durchsuchen, indem Sie auf Ihren Kontonamen in der oberen rechten Ecke wechseln.
4.  Wählen Sie aus dem Abschnitt **Schnellaufgaben** **Hinzufügen eines Benutzers**aus.
5.  Geben Sie im Formular Benutzer hinzufügen die folgenden Details:

    - Name: (Wählen Sie einen entsprechenden Wert)
    - Benutzername: (Wählen Sie einen Benutzernamen für dieses Administrator)
    - Profil: (füllen Sie die entsprechenden Werte für Vornamen, Nachnamen, Position und Abteilung)
    - Rolle: Globaler Administrator

6.  Wenn Sie das Hinzufügen von Formular ausgefüllt haben und das temporäre Kennwort für den neuen administrativen Benutzer erhalten, müssen Sie dieses Kennwort aufzeichnen, wie Sie mit diesen neuen Benutzer anmelden, um das Kennwort ändern müssen. Sie können das Kennwort auch direkt mit dem Benutzer, die mit einer alternativen e-Mail-Nachricht senden.
7.  Klicken Sie auf **Erstellen** , um den neuen Benutzer zu erstellen.
8.  Wenn Sie das temporäre Kennwort ändern möchten, melden Sie sich bei [https://login.microsoftonline.com](https://login.microsoftonline.com) mit dieser neuen Benutzerkonto und Ändern des Kennworts bei der Anforderung.


## <a name="use-an-organizational-azure-subscription"></a>Verwenden eines organisationsinterne Azure-Abonnements
Wenn Sie vorher für ein Abonnement Azure mit Organisations-Konto angemeldet haben, besitzen Sie bereits einen Mandanten!  Im [Portal Azure](https://portal.azure.com)sollten Sie einen Mandanten suchen, wenn Sie auf "Weitere Dienste" und "Azure Active Directory." navigieren  Sie können dieses Mandanten verwenden, je nach Bedarf. 


## <a name="start-from-scratch"></a>Ganz neu beginnen
Wenn Sie alle vorstehend genannten Kauderwelsch Ihnen, kein Problem.  Besuchen Sie einfach [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) für Azure mit einer neuen Organisation anmelden.  Nachdem Sie den Vorgang abgeschlossen haben, müssen Sie eigene sehr Azure AD-Mandanten mit den Namen der Domäne, die Sie während der Anmeldung von ausgewählt haben.  Im [Portal Azure](https://portal.azure.com)finden Ihrem Mandanten Sie navigieren zu "Azure Active Directory" in der linken NAV.

Als Teil des Prozesses zum Azure anmelden werden Sie aufgefordert, Kreditkarteninformationen anzugeben.  Ohne Sicherheitsrisiko - fortgesetzt werden kann werden Sie nicht in Rechnung gestellt für die Veröffentlichung von Applications in Azure AD oder Erstellen von neuen Mandanten.
