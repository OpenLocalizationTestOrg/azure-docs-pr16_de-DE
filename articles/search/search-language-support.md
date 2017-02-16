<properties
   pageTitle="Erstellen Sie einen Index für Dokumente in mehreren Sprachen in Azure suchen | Microsoft Azure | Cloud gehosteten Suchdienst"
   description=" Azure-Suche unterstützt 56 Sprachen, Nutzung von Sprache Analyzern aus Lucene und natürlicher Sprache Verarbeitung Technologie von Microsoft."
   services="search"
   documentationCenter=""
   authors="yahnoosh"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="07/14/2016"
   ms.author="jlembicz"/>

# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Erstellen Sie einen Index für Dokumente in mehreren Sprachen in Azure-Suche
> [AZURE.SELECTOR]
- [Portal](search-language-support.md)
- [REST](https://msdn.microsoft.com/library/azure/dn879793.aspx)
- [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)

Die Leistungsfähigkeit von Sprache Analyzern solche ist so einfach wie das Festlegen einer Eigenschaft nach einem durchsuchbare Feld in der Indexdefinition. Jetzt können Sie diesen Schritt im Portal durchführen.

Nachfolgend finden Sie Screenshots der Azure-Portal Blades für Azure suchen, die Benutzern ein IndexSchema definieren zu ermöglichen. Benutzer können aus dieser Blade erstellen alle Felder und festlegen die Eigenschaft Analyzer für jede von ihnen.

> [AZURE.IMPORTANT] Sie können nur eine Sprache Analyse während Felddefinition, als in beim Erstellen eines neuen Indexes von Grund nach oben oder beim Hinzufügen eines neuen Felds in einen vorhandenen Index festlegen. Stellen Sie sicher, dass Sie alle Attribute, einschließlich des Analyzers beim Erstellen des Felds vollständig angeben. Sie können nicht mehr bearbeiten Sie die Attribute oder den Analyzer Typ ändern, nachdem Sie die Änderungen zu speichern.

## <a name="define-a-new-field-definition"></a>Definieren der Definition eines neuen Felds

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com) an, und öffnen Sie die Service-Karte vorausgesetzt, Ihr Suchdiensts.
2. Klicken Sie auf **Add Index** in der Befehlsleiste am oberen Rand der Dienst Dashboard, um einen neuen Index zu starten, oder öffnen Sie einen vorhandenen Index zum Festlegen von einer Analyzer für neue Felder, die Sie zu einer vorhandenen Index hinzufügen.
3. Das Felder Blade angezeigt wird, verwendet Optionen zum Definieren des Schemas des Indexes, einschließlich der Registerkarte Analyzer zum Auswählen einer Sprache Analyse.
4. Felddefinition zunächst in den Feldern aus einen Namen, den Datentyp auswählen und Festlegen von Attributen im Feld als vollständigen Text durchsucht, in den Suchergebnissen, verwendbar in Vertriebsstrategie Navigationsstrukturen, einer abrufbaren sortierbaren und usw. markiert. 
5. Öffnen Sie zum nächsten Feld im verschieben, bevor Sie die Registerkarte **Analyzer** aus. 

   
![][1]
*Klicken Sie zum Auswählen einer Analyzer auf die Registerkarte Analyzer das Blade (Felder)*

## <a name="choose-an-analyzer"></a>Wählen Sie eine Analyzer aus.

6. Führen Sie einen Bildlauf um das Feld finden, das Sie definieren. 
7. Wenn Sie das Feld als durchsuchbar markiert nicht geschehen ist, aktivieren Sie das Kontrollkästchen jetzt, um ihn als **durchsuchbar**zu markieren.
8. Klicken Sie auf den Bereich Analyzer, um eine Liste der verfügbaren Analyzern anzuzeigen.
9. Wählen Sie den Analyzer, die, den Sie verwenden möchten.

![][2]
*Wählen Sie die unterstützten Analyzern für jedes Feld*

Standardmäßig verwenden alle durchsuchbare Felder [Standard Lucene Analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) die Sprache unabhängig ist. Wenn Sie die vollständige Liste der unterstützten Analyzern anzeigen möchten, finden Sie unter [Unterstützung von Sprachen in Azure suchen](https://msdn.microsoft.com/library/azure/dn879793.aspx).

Sobald der Sprache Analyzer für ein Feld ausgewählt ist, wird es bei jeder Anforderung Indizierung und Suche für dieses Feld verwendet werden. Wenn eine Abfrage mit mehreren Feldern mit verschiedenen Analyzern ausgegeben wird, wird die Abfrage unabhängig vom der richtigen Analyzern für jedes Feld verarbeitet werden.

Viele Web- und Windows-Dienste dienen Benutzer auf der ganzen Welt mit verschiedenen Sprachen. Es ist möglich, einen Index für ein Szenario wie folgt zu definieren, indem Sie ein Feld für jede unterstützte Sprache zu erstellen.

![][3]
*Indexdefinition mit einem Beschreibungsfeld für die einzelnen Sprachen unterstützt*

Wenn die Sprache des Agents Ausgeben einer Abfrage bekannt ist, kann eine Suchabfrage zu einem bestimmten Feld mithilfe des Abfrageparameters **SearchFields** ausgelegte. Die folgende Abfrage wird nur in Verbindung mit der Beschreibung im Polnisch veröffentlicht werden:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2015-02-28`

Sie können Ihre Index im Portal Abfragen, fügen Sie eine Abfrage ähnlich der oben abgebildeten über **Suche-Explorer** . Suche Explorer steht aus der Befehlsleiste in der Service-Karte vorausgesetzt. Details finden Sie in der [Abfrage im Portal Azure Suchindex](search-explorer.md) .

Manchmal ist die Sprache des Agents Ausgeben einer Abfrage nicht bekannt, wobei die Abfrage für alle Felder gleichzeitig ausgestellt werden kann. Bei Bedarf kann die Einstellung für eine bestimmte Sprache ergibt mithilfe von [Profilen bewerten](https://msdn.microsoft.com/library/azure/dn798928.aspx)definiert werden. Im folgenden Beispiel werden in der Beschreibung in Englisch gefundenen höher relativ zur Übereinstimmung im Polnisch und Französisch bewertet werden:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2015-02-28`

Wenn Sie ein .NET Entwickler haben, beachten Sie, dass Sie die Sprache Analyzern mit dem [Azure Suche .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search)konfigurieren können. Die neueste Version unterstützt der Microsoft-Sprache Analyzern ebenfalls.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
