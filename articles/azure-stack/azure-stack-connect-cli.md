<properties
    pageTitle="Herstellen einer Verbindung mit CLI Azure Stapel mit | Microsoft Azure"
    description="Informationen Sie zum Verwenden der Plattformen line Interface (CLI) zum Verwalten und Bereitstellen von Ressourcen auf Azure Stapel"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="helaw"/>

# <a name="install-and-configure-azure-stack-cli"></a>Installieren und Konfigurieren von Azure Stapel CLI

In diesem Dokument aus führen wir Sie durch den Prozess der Verwendung von Azure Line Interface (CLI) zum Verwalten von Azure Stapel Ressourcen auf Linux und Mac-Client-Plattformen.  

## <a name="install-azure-stack-cli"></a>Installieren von Azure Stapel CLI

Wenn Sie Mac oder Linux angezeigt wird, erhalten Sie die CLI, indem Sie mit dem folgenden Befehl:
  
    `npm install -g azure-cli@0.10.4`.


## <a name="connect-to-azure-stack"></a>Verbinden mit Azure Stapel
In den folgenden Schritten konfigurieren Sie Azure CLI Verbindung zum Azure Stapel aus. Klicken Sie dann anmelden und Abonnementinformationen abzurufen.

1.  Rufen Sie den Wert für Active Directory Ressourcen-Id, indem Sie diese PowerShell:
        
          (Invoke-RestMethod -Uri https://api.azurestack.local/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]

2.  Verwenden Sie den folgenden Befehl aus CLI hinzufügen die Stapel Azure-Umgebung sicherzustellen aktualisieren *– Active Directory Ressourcen-Id* mit den Daten URL, die im vorherigen Schritt abgerufen:

           azure account env add AzureStack --resource-manager-endpoint-url "https://api.azurestack.local" --management-endpoint-url "https://api.azurestack.local" --active-directory-endpoint-url  "https://login.windows.net" --portal-url "https://portal.azurestack.local" --gallery-endpoint-url "https://portal.azurestack.local" --active-directory-resource-id "https://azurestack.local-api/" --active-directory-graph-resource-id "https://graph.windows.net/"

3.  Melden Sie sich mit dem folgenden Befehl (ersetzen *Benutzername* durch Ihren Benutzernamen):

        azure login -e AzureStack -u “<username>”

    >[AZURE.NOTE]Wenn Sie Probleme bei der Überprüfung Zertifikat wiedergegeben werden, deaktivieren Sie Zertifikat Überprüfung durch Ausführen des Befehls `set        NODE_TLS_REJECT_UNAUTHORIZED=0`.

4.  Festlegen des Azure-Konfigurationsmodus zu Azure Ressourcenmanager mit dem folgenden Befehl:

        azure config mode arm

5.  Abrufen einer Liste der Abonnements.

        azure account list     

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Vorlagen mit Azure CLI](azure-stack-deploy-template-command-line.md)

[Verbinden mit PowerShell](azure-stack-connect-powershell.md)

[Verwalten von Benutzerberechtigungen](azure-stack-manage-permissions.md)
