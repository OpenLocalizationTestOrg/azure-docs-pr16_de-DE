<properties
    pageTitle="Veröffentlichen von Applications HDInsight | Microsoft Azure"
    description="Informationen Sie zum Erstellen und Veröffentlichen von Applications HDInsight."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/18/2016"
    ms.author="jgao"/>

# <a name="publish-hdinsight-applications-into-the-azure-marketplace"></a>Veröffentlichen von Applications HDInsight, in dem Azure Marketplace

Eine HDInsight-Anwendung ist eine Anwendung, die Benutzer in einem HDInsight Linux-basierten Cluster installieren können. Diese Anwendung können von Microsoft, unabhängigen Software-Anbietern (ISV) oder sich selbst entwickelt werden. In diesem Artikel erfahren Sie, wie die Anwendung HDInsight in dem Azure Marketplace veröffentlicht werden.  Allgemeine Informationen zu veröffentlichen, in dem Azure Marketplace finden Sie unter [Veröffentlichen ein Angebots zu Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md).

HDInsight Applikationen verwenden Sie das Modell *Wieder abrufen Ihrer eigenen Lizenz (BYOL)* , wobei Anwendung-Anbieter ist verantwortlich für die Anwendung an Endbenutzer-Lizenzierung und Endbenutzer nur für die Ressourcen, die sie, die wie etwa die Cluster HDInsight und seine virtuellen Computern/Knoten erstellen Azure berechnet werden. Zu diesem Zeitpunkt erfolgt Rechnung für die Anwendung selbst nicht über Azure.

Andere Anwendung HDInsight Zusammenhang Artikel:

- [HDInsight Installieren von Applications](hdinsight-apps-install-applications.md): erfahren, wie Sie eine Anwendung HDInsight zu Ihrer Cluster zu installieren.
- [Installieren von benutzerdefinierten HDInsight Applications](hdinsight-apps-install-custom-applications.md): Informationen zum Installieren und Testen der benutzerdefinierte HDInsight Applikationen.

 
## <a name="prerequisites"></a>Erforderliche Komponenten

Um Ihre benutzerdefinierte Anwendung zu Marketplace übermitteln, müssen Sie erstellt und Ihrer benutzerdefinierte Anwendung getestet haben. Finden Sie unter den folgenden Artikeln:

- [Installieren von benutzerdefinierten HDInsight Applications](hdinsight-apps-install-custom-applications.md): Informationen zum Installieren und Testen der benutzerdefinierte HDInsight Applikationen.

Sie müssen außerdem haben registrieren Ihr Konto Entwicklertools. Finden Sie unter [Veröffentlichen ein Angebots zu Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md) und [Microsoft Developer Konto erstellen](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Definieren der Anwendung

Umfasst zwei Schritte für die Veröffentlichung von Applications zu Azure Marketplace.  Zuerst definieren Sie eine Datei **createUiDef.json** , um anzugeben, welche Cluster Ihrer Anwendung mit kompatibel ist. ein, und klicken Sie dann veröffentlichen Sie die Vorlage aus dem Azure-Portal. Im folgenden finden eine createUiDef.json-Beispieldatei.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


|Feld  | Beschreibung   | Mögliche Werte|
|-------|---------------|----------------|
|Datentypen  | Die Clustertypen, denen die Anwendung mit kompatibel ist.    |Hadoop, HBase, Sturm, Spark (oder eine beliebige Kombination dieser)|
|Ebenen  | Die Cluster-Ebenen, denen die Anwendung mit kompatibel ist.    |Standard, Premium (oder beide)|
|Versionen|  Die HDInsight Clustertypen, denen die Anwendung mit kompatibel ist.    |3.4|

## <a name="package-application"></a>Paket-Anwendung

Erstellen Sie eine Zip-Datei, die alle erforderliche Dateien für die Installation von Ihrer HDInsight Applikationen enthält. Sie benötigen die Zip-Datei in der [Anwendung veröffentlichen](#publish-application).

- [createUiDefinition.json](#define-application).
- mainTemplate.json. Zeigen Sie ein Beispiel bei [benutzerdefinierte HDInsight Applikationen installieren](hdinsight-apps-install-custom-applications.md)aus.

    >[AZURE.IMPORTANT] Der Namen der Anwendung installieren Skriptnamen muss für einen bestimmten Cluster mit folgendem Format ein eindeutig sein. Darüber hinaus eine installieren und deinstallieren Skriptaktionen Idempotent sein, d. h., die Skripts aufgerufen werden kann repeatly während der gleichen ergebnislos.
    
    >   Name":" [verketten ("Farbton-Installation-v0 ',' – ', uniquestring('applicationName')]"
        
    >Beachten Sie, dass es gibt drei Webparts auf den Skriptnamen an:
        
    >   1. Ein Skript Name-Präfix, das den Anwendungsnamen oder einen Namen für die Anwendung relevant umfassen.
    >   2. A "-" die Lesbarkeit zu verbessern.
    >   3. Eine eindeutige Zeichenfolge-Funktion mit dem Anwendungsnamen als Parameter.

    >   Ein Beispiel ist die oben angegebenen Enden von zunehmend: Farbton-Installation-v0-4wkahss55hlas in der Liste der Aktion dauerhaften Skript. Ein Beispiel für JSON gefährliche Fracht finden Sie unter [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).

- Alle erforderlichen Skripts.

> [AZURE.NOTE] Die Anwendungsdateien (einschließlich Dateien von Web Apps ist es eine) für eine öffentlich zugängliche Endpunkt gefunden werden kann.

## <a name="publish-application"></a>Veröffentlichen Sie die Anwendung

Führen Sie die folgenden Schritte aus, um eine HDInsight Anwendung veröffentlichen aus:

1. Melden Sie sich auf die [Azure Veröffentlichungsportal](https://publish.windowsazure.com/).
2. Klicken Sie auf **Lösungsvorlagen** von links, um eine neue Lösung Vorlage erstellen.
3. Geben Sie einen Titel ein, und klicken Sie dann auf **eine neue Lösung Vorlage erstellen**.
3. Klicken Sie auf **Erstellen Developer Center zu berücksichtigen und Azure-Programm teilnehmen** , um Ihr Unternehmen zu registrieren, falls dies noch nicht erfolgt.  Finden Sie unter [Erstellen eines Microsoft Developer-Kontos](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
4. Klicken Sie auf **einige Topologien Schritte definieren**. Eine Lösungsvorlage ist "Parent" alle zugehörigen Topologien. Sie können mehrere Topologien in einem Angebot/Lösung Vorlage definieren. Wenn ein Angebot ans Staging verschoben wird, wird es mit allen zugehörigen Topologien abgelegt. 
4. Geben Sie einen Namen für die Suchtopologie, und klicken Sie dann auf das Pluszeichen (+).
5. Geben Sie eine neue Version, und klicken Sie dann auf das Pluszeichen (+).
6. Hochladen der Zip-Datei in das [Paket Anwendung](#package-application)vorbereitet.  
7. Klicken Sie auf **Zertifizierung anfordern**. Das Microsoft-Zertifizierung-Team wird, überprüfen Sie die Dateien und Zertifizieren der Suchtopologie.

## <a name="next-steps"></a>Nächste Schritte

- [HDInsight Installieren von Applications](hdinsight-apps-install-applications.md): erfahren, wie Sie eine Anwendung HDInsight zu Ihrer Cluster zu installieren.
- [Installieren von benutzerdefinierten HDInsight Applications](hdinsight-apps-install-custom-applications.md): erfahren Sie, wie eine dauerhaften veröffentlichte HDInsight Anwendung mit HDInsight bereitstellen.
- [Anpassen von Linux-basierten HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster-linux.md): erfahren Sie, wie Skript-Aktion verwenden, um zusätzliche Applikationen zu installieren.
- [Hadoop erstellen Linux-basierten Cluster in mithilfe von Vorlagen Ressourcenmanager Azure HDInsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md): erfahren Sie, wie Ressourcenmanager Vorlagen zum Erstellen von HDInsight Cluster aufrufen.
- [Leere Kante Knoten in HDInsight verwenden](hdinsight-apps-use-edge-node.md): erfahren, wie Sie einen leeren Kantenknoten für den Zugriff auf HDInsight Cluster, HDInsight Applikationen testen und Hosten von Applications HDInsight zu verwenden.

