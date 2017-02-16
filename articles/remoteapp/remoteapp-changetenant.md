
<properties
    pageTitle="Ändern den Azure-Active Directory-Mandanten in Azure RemoteApp | Microsoft Azure"
    description="Informationen Sie zum Ändern des Azure-Active Directory-Mandanten zugeordnet Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="change-the-azure-active-directory-tenant-in-azure-remoteapp"></a>Ändern der Azure-Active Directory-Mandanten in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp verwendet Azure Active Directory (Azure AD), damit Benutzer Zugriff auf. Nur Azure AD-Mandanten, mit denen Sie in Azure RemoteApp, wird mit der Azure-Abonnement verknüpft ist. Sie können das zugeordnete Abonnement auf **der Einstellungsseite im Portal** anzeigen. Prüfen Sie die Spalte **Verzeichnis** aus, auf der Registerkarte **Abonnements** .

> [AZURE.NOTE] Damit diese Änderung erfolgreich durchgeführt werden kann müssen Sie zuerst entfernen Sie alle Benutzer von der vorhandenen Azure Active Directory-Mandanten aus allen Azure RemoteApp Sammlungen. Hierfür, wechseln Sie zu der Azure-Portal, wechseln Sie zur Registerkarte **Azure RemoteApp** und öffnen Sie jeder Websitesammlung Azure RemoteApp. Wechseln Sie zur Registerkarte **Benutzer** und Entfernen von Benutzern, die zu Ihrem aktuellen Azure Active Directory-Mandanten gehören. Wiederholen Sie für alle vorhandenen Azure RemoteApp Websitesammlungen. Ohne auf diese Weise ist es ist nicht möglich, zu erstellen oder patch für Websitesammlungen.

Wenn Sie einen anderen Mandanten verwenden möchten, verwenden Sie diese Schritte so ändern Sie die Zuordnung mit Ihrem Abonnement:

1. Entfernen Sie im Portal alle Azure AD-Benutzer, die Sie Zugriff auf Websitesammlungen Azure RemoteApp erteilt haben. (Siehe die Notiz über die Schritte zum Zweck.)


2. Legen Sie ein Microsoft-Konto (vormals als eine Live ID bezeichnet) als Dienstadministrator an. (Feststellen nicht, ob Sie bereits über die Dienstadministrator sind? Sie können herausfinden, indem Sie auf **Einstellungen-Administratoren >**.) Nun sieht wie die ändern:
    1. Klicken Sie auf den Benutzer in der oberen rechten Ecke, und klicken Sie dann auf **Anzeigen meiner Rechnung**.
    2. Klicken Sie auf das Abonnement. Klicken Sie dann auf der neuen Seite einen Bildlauf nach unten, und klicken Sie rechts auf **Abonnementdetails bearbeiten** . (Sortieren der mittleren unten rechts, wenn diese hilft Ihnen bei der Suche).
    3. Geben Sie das Microsoft-Konto für den Benutzer, der den Dienst-Administrator sein sollen

3. Nun wird im Portal abmelden, und melden Sie sich dann wieder mit dem Microsoft-Konto, das Sie im vorherigen Schritt angegeben haben.


4. Klicken Sie auf **-> neue App Services -> Active Directory-Verzeichnis > -> benutzerdefinierte erstellen**.
5. Wählen Sie unter **Verzeichnis**aus **vorhandenen Verzeichnis verwenden**. Wir werden Sie jetzt aus dem Portal anmelden, wählen Sie **bereit sind, nun abgemeldet mir**müssen.
6. Melden Sie sich zurück in das Portal als globaler Administrator des Verzeichnisses, die Sie hinzufügen möchten. (Wenn Sie noch kein globaler Administrator nicht vorhanden sind, werden Sie nach einer Runde von anmelden, und klicken Sie dann abmelden.)
7. Sie werden aufgefordert werden bei der Anmeldung, wenn Sie Ihre vorhandene AD-Mandanten mit Ihrem Abonnement verwenden möchten. Klicken Sie auf **Weiter**, und klicken Sie dann auf **jetzt abmelden**.
5. Anschließend wieder anzumelden, und kehren Sie zu **Einstellungen -> Abonnements**. Wählen Sie Ihr Abonnement aus, und klicken Sie dann auf **Verzeichnis bearbeiten**. Wählen Sie den Azure AD-Mandanten, den Sie verwenden möchten.



Sie können jetzt verwenden der neuen Azure AD-Mandanten zum Steuern des Zugriffs mit dem Azure-Abonnement und des Benutzerzugriffs in Azure RemoteApp zu konfigurieren.
