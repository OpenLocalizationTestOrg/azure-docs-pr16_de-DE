<properties
    pageTitle="Hinzufügen ein neues Kontos von Azure Stapel Mandanten in Azure Active Directory | Microsoft Azure"
    description="Nach der Bereitstellung von Microsoft Azure Stapel Prüfung des Konzepts ist, müssen Sie mindestens einen Mandanten Benutzerkonto erstellen, damit Sie das Mandanten Portal durchsuchen können."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Hinzufügen eines neuen Kontos von Azure Stapel Mandanten in Azure Active Directory

Nach der [Bereitstellung der Azure Stapel Prüfung des Konzepts ist](azure-stack-run-powershell-script.md)benötigen Sie ein Benutzerkonto für den Mandanten, damit Sie können im Portal Mandanten untersuchen und Testen Sie Ihre Angebote und Pläne. Sie können einem mandantenadministratorkonto mithilfe [des Azure-Portals](#create-an-azure-stack-tenant-account-using-the-azure-portal) oder mithilfe der [PowerShell](#create-an-azure-stack-tenant-account-using-powershell)erstellen.

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Erstellen Sie ein Azure Stapel mandantenadministratorkonto über das Azure-portal

Sie müssen eine Azure-Abonnement Azure-Portal verwenden können.

1. Melden Sie sich bei [Azure](http://manage.windowsazure.com).

2.  Klicken Sie in der linken Navigationsleiste Microsoft Azure auf **Active Directory**.

3.  Klicken Sie in der Verzeichnisliste klicken Sie auf das Verzeichnis, das Sie für Azure Stapel verwenden möchten, oder Erstellen eines neuen Kontos.

4.  Klicken Sie auf dieser Seite Verzeichnis auf **Benutzer**.

5.  Klicken Sie auf **Benutzer hinzufügen**.

6.  Wählen Sie im Assistenten **Hinzufügen Benutzer** in der Liste **Typ des Benutzers** **neuen Benutzer in Ihrer Organisation**ein.

7.  Geben Sie im Feld **Benutzername** einen Namen für den Benutzer aus.

8.  In der **@** Feld, und wählen Sie den entsprechenden Eintrag.

9.  Klicken Sie auf den Pfeil nach rechts.

10.  Geben Sie in der **Benutzerprofil** -Seite des Assistenten eine **Vorname**, **Nachname**und **Anzeigename**ein.

11. Wählen Sie in der Liste **Rolle** **Benutzer**aus.

12. Klicken Sie auf den Pfeil nach rechts.

13. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

14. Kopieren Sie das **neue Kennwort**ein.

15. Melden Sie sich beim Microsoft Azure mit dem neuen Konto an. Ändern Sie das Kennwort ein.

16. Melden Sie sich bei `https://portal.azurestack.local` mit dem neuen Konto im Portal Mandanten angezeigt.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>Erstellen Sie ein Azure Stapel mandantenadministratorkonto mithilfe der PowerShell

Wenn Sie ein Azure-Abonnement besitzen, können Azure-Portal Sie ein Benutzerkonto Mandanten hinzufügen. In diesem Fall können Sie stattdessen die Azure Active Directory-Modul für Windows PowerShell verwenden.

> [AZURE.NOTE] Wenn Sie zum Bereitstellen von Azure Stapel Prüfung des Konzepts ist Microsoft Account (Live ID) verwenden, können AAD PowerShell Sie mandantenadministratorkonto erstellen. 

1.  Installieren Sie den [Microsoft Online Services - Anmeldeassistenten für IT-Experten RTW](https://www.microsoft.com/en-us/download/details.aspx?id=41950).

2.  Installieren Sie der [Azure Active Directory-Modul für Windows PowerShell (64-Bit-Version)](http://go.microsoft.com/fwlink/p/?linkid=236297) , und öffnen Sie sie.

3.  Führen Sie die folgenden Cmdlets aus:




    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack PoC
   
            $msolcred = get-credential
    
    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".
    
            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId
    
    ```

4.  Melden Sie sich beim Microsoft Azure mit dem neuen Konto an. Ändern Sie das Kennwort ein.

5.  Melden Sie sich bei `https://portal.azurestack.local` mit dem neuen Konto im Portal Mandanten angezeigt.



