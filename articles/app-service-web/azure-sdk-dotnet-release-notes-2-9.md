<properties 
   pageTitle="Azure SDK für .NET 2,9 – Versionsinformationen" 
   description="Azure SDK für .NET 2,9 – Versionsinformationen" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

# <a name="azure-sdk-for-net-29-release-notes"></a>Azure SDK für .NET 2,9 – Versionsinformationen

##<a name="overview"></a>(Übersicht)

Dieses Dokument enthält die Versionsinformationen für das Azure SDK für .NET 2.9 Release. 

Ausführliche Informationen zu Updates, die in dieser Version finden Sie unter der [Azure SDK 2.9 Ankündigung bereitstellen](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Anzeigen der Vorschau Azure SDK 2.9 für Visual Studio 2015 Update 2 und Visual Studio "15"
 
Dieses Update umfasst die folgenden Updates:

- Problem im Zusammenhang mit API Client Generation in ruhen lassen, die Zeichenfolge "Unbekannten Typ" als den Namen des Ordners Code Generation und/oder den Namen des Namespace angezeigt würde, Drop in den generierten Code.
- Problem im Zusammenhang mit geplanten WebJobs, in dem die Authentifizierungsinformationen an dem Scheduler provisioning Prozess zu übergebenden weiß nicht wurde.

Dieses Update umfasst die folgende neue Features:

- Unterstützung für sekundäre App-Dienste in der Registerkarte "Dienstleistungen" des Dialogfelds "App-Dienst provisioning". 

##<a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Azure-Daten Lake Tools für Visual Studio 2015 Update 2
 
Diese Updates umfassen Folgendes:

- **Azure Daten dem Tools** für Visual Studio wird nun mit dem Azure SDK für .NET Version zusammengeführt. Das Tool wird automatisch installiert, wenn Sie Azure SDK installieren. 

    Das Tool wird regelmäßig aktualisiert, wechseln Sie [hier](http://aka.ms/datalaketool) können Sie die Aktualisierungen zu gelangen.

- **Server-Explorer** bietet nun die Möglichkeit zum Anzeigen aller und einige U SQL Metadaten Personen erstellen. Weitere Informationen finden Sie in [diesem](https://azure.microsoft.com/documentation/services/data-lake-analytics/) Blog.


##<a name="hdinsight-tools"></a>HDInsight-Tools 

**HDInsight Tools** für Visual Studio unterstützt jetzt HDInsight Version 3.3, einschließlich mit Tez Diagramme und andere Sprache behebt.


##<a name="azure-resource-manager"></a>Azure Ressourcenmanager 

Diese Version bietet [KeyVault](../resource-manager-keyvault-parameter.md) Unterstützung für Cloud-Vorlagen.

##<a name="see-also"></a>Siehe auch

[Azure SDK 2.9 Ankündigung Beitrag](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)
