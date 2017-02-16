<properties
    pageTitle="Neuigkeiten Azure suchen | Microsoft Azure | Cloud gehosteten Suchdienst"
    description="Azure Suche ist ein Suchdienst gehostete Cloud vollständig verwaltet. Weitere Informationen finden Sie in dieser Features (Übersicht)."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="what-is-azure-search"></a>Was ist Azure suchen?

Azure Suche ist eine Cloud-Suche-als-Service-Lösung, die delegiert Server und Infrastructure Management an Microsoft, verlassen Sie mit einem sofort einsatzbereite-Dienst, den Sie für Ihre Daten zu füllen und verwenden Sie dann auf Suche zur Web oder mobilen Anwendung hinzuzufügen. Azure suchen können Sie einfach eine robuste Suchfunktion den Clientanwendungen mithilfe einer einfachen [REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) oder [.NET SDK](search-howto-dotnet-sdk.md) ohne Suche Infrastruktur verwalten oder ein Experte auf Suche hinzufügen.

## <a name="give-your-users-a-powerful-search-experience"></a>Geben Sie Ihre Benutzer eine leistungsfähige Suchfunktion

**Leistungsfähige Abfragen** können mithilfe der [einfachen Abfragesyntax](https://msdn.microsoft.com/library/azure/dn798920.aspx), seiner logischen Operatoren, Ausdruck Suchoperatoren, Suffixoperatoren, der Rangfolge Operatoren formuliert werden. Darüber hinaus können die [Abfragesyntax Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) fuzzy-Suche, Suche, Ausdruck verstärken und reguläre Ausdrücke aktivieren. Azure-Suche unterstützt auch benutzerdefinierte lexikalische Analyzern Ihrer Anwendung verarbeitet komplexe Suchabfragen phonetischen übereinstimmenden verwenden und reguläre Ausdrücke zulässig.

**Sprache unterstützen** ist [für 56 verschiedene Sprachen enthalten](https://msdn.microsoft.com/library/azure/dn879793.aspx). Verwenden sowohl die Lucene Analyzern Microsoft Analyzern (eingeschränkt nach Jahren natürlicher Sprache in Office und Bing processing), können Azure suchen Text in Ihrer Anwendung Suchfeld intelligente sprachspezifischen Linguistics einschließlich Zeitformen, Geschlecht, unregelmäßige Pluralform Nomen (z. B. ' Maus' im Vergleich zu 'Maus'), Word heben Zinsperiode zurück, Word neuesten (für Sprachen ohne Leerzeichen) und weitere verarbeitet analysieren.

**Suchvorschläge** können bei Autocompleted suchen und Abfragen während der Eingabe aktiviert sein. Geben Sie als Benutzer [Basis: Taggenau Dokumente in Ihrem Index vorgeschlagen werden](https://msdn.microsoft.com/library/azure/dn798936.aspx) teilweise Eingabemethoden suchen.

**Drücken Sie die Hervorhebung** [ermöglicht](https://msdn.microsoft.com/library/azure/dn798927.aspx) Benutzern das Ausschnitt des Texts in jedes Ergebnis angezeigt, die die Übereinstimmungen bereits für ihre Abfrage enthält. Sie können wählen, welche Felder hervorgehobenen Codeausschnitte zurückzukehren.

Seite mit den Suchergebnissen mit Azure Search ist einfach **facettierten Navigationsbereich** hinzugefügt. Verwenden [nur einen einzelnen Abfrageparameter](https://msdn.microsoft.com/library/azure/dn798927.aspx), zurückgegeben werden Azure-Suche kann die erforderlichen Informationen zum Erstellen einer facettierten Suchfunktion in Ihrem app Benutzeroberfläche, damit Ihre Benutzer Drilldowns und Filtern von Suchergebnissen (z. B. Katalogelemente nach Filtern Preis-Bereich oder Marke).

**Geo-geografische** Support können Sie intelligente verarbeiten, Filtern und Anzeigen von geografischen Standorten. Suche Azure ermöglicht Benutzern zum Durchsuchen von Daten in der Nähe der ein Suchergebnis an einem bestimmten Speicherort oder auf eine bestimmte geografische Region basieren. In diesem Video wird erläutert, wie es funktioniert: [Channel 9: Azure suchen und Geodaten Daten](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data).

**Filter** können verwendet werden, um einfach facettierten Navigation in der Benutzeroberfläche Ihrer Anwendung einbinden, verbessern die Abfrage Formulierung und Filter basierend auf Benutzer oder Entwickler-angegebenen Kriterien. Erstellen Sie leistungsfähige Filter mithilfe der [OData-Syntax](https://msdn.microsoft.com/library/azure/dn798921.aspx)ein.

## <a name="empower-your-developers-with-an-easy-to-use-service"></a>Befähigungen Sie der Entwickler mit einer einfach zu verwendendes service

**Hohe Verfügbarkeit** sichergestellt eine extrem zuverlässigen Dienst Suchfunktion. Wenn skaliert ordnungsgemäß, [Azure-Suche bietet eine 99,9 % Vereinbarung zum SERVICELEVEL](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Als End-to-End-Lösung, Azure Suche **vollständig verwaltete** erfordert absolut keine Infrastruktur-Verwaltung. Der Dienst kann einfach an Ihre Bedürfnisse durch Skalierung in zwei Dimensionen weitere Speicherung von Dokumenten, höhere Abfrage lädt oder beides verarbeitet angepasst werden.

**Integration von Daten** mithilfe von [Indexern](https://msdn.microsoft.com/library/azure/dn946891.aspx) ermöglicht Azure-Suche zum Durchforsten von Azure SQL-Datenbank, Azure DocumentDB oder [Azure Blob-Speicher](search-howto-indexing-azure-blob-storage.md) zum Synchronisieren Ihrer Suchindex Inhalte mit Ihrem primären Datenspeicher automatisch an.

**Dokument zum Ermitteln von Kennwörtern** ist verfügbar (aktuell in der Seitenansicht) [zu lesen ist und Indizieren gängiger Dateiformate](search-howto-indexing-azure-blob-storage.md) einschließlich Microsoft Office sowie PDF- und HTML-Dokumente.

**Suche Datenverkehr Analytics** sind [einfach gesammelt und analysiert](search-traffic-analytics.md) zum Entsperren Einsichten aus, was Benutzer in das Suchfeld eingeben.

**Einfache bewerten** ist ein wesentlicher Vorteil der Azure-Suche. [Bewerten Profile](https://msdn.microsoft.com/library/azure/dn798928.aspx) dienen zur Organisationen zur Modell Relevanz als Funktion der Werte in den Dokumenten selbst zulässig sind. Beispielsweise sollten neuer Produkte oder nach einem Rabatt Produkte höhere in den Suchergebnissen angezeigt werden. Sie können auch erstellen Punktzahl Profile mithilfe von Kategorien für individuelle bewerten basierend auf Kunden suchen Einstellungen, die Sie erfasst und separat gespeichert haben.

**Sortierung** ist für mehrere Felder über das IndexSchema angeboten und dann zum Zeitpunkt der Abfrage mit einem einzigen Suchvorgang Parameter umgeschaltet.

**Paging** und Ihre Suchergebnisse begrenzungsebene ist [mit dem Steuerelement abstimmen recht einfach](search-pagination-page-layout.md) , die Suche Azure über Ihre Suchergebnisse bietet.  

**Suche Explorer** können Sie Problem Abfragen für alle Ihre Indizes rechts von Azure-Portal Ihres Kontos so einfach Abfragen testen und verfeinern Punktzahl Profile.

## <a name="how-it-works"></a>So funktioniert es

### <a name="1-provision-service"></a>1. Dienst Bereitstellen von
Sie können von einem Azure-Suchdienst mithilfe der [Azure-Portal](https://portal.azure.com/) oder die [Azure Ressource Management API](https://msdn.microsoft.com/library/azure/dn832684.aspx)drehen.

Je nachdem, wie Sie den Suchdienst konfigurieren verwenden Sie entweder die kostenlose Ebene der Dienst, der für andere Abonnenten Azure Suche freigegeben werden, oder eine [Ebene bezahlt](https://azure.microsoft.com/pricing/details/search/) , die nur von Ihrem Dienst zu verwendende Ressourcen zuordnet. Beim Bereitstellen von Ihrem Diensts, wählen Sie auch den Bereich des das Data Center, das den Dienst hostet.

Je nach der Ebene des Diensts wählen Sie aus, den Dienst in zwei Dimensionen skaliert werden kann: 1) hinzufügen Replikate Wachstum Ihrer Kapazität behandeln beanspruchen Abfrage lädt und 2) Partitionen zum Hinzufügen von Speicherplatz für weitere Dokumente hinzufügen. Speicherung und Abfrage Dokumentdurchsatz separat behandelt, können Sie Ihre Suchdienst für Ihre Bedürfnisse anpassen.

### <a name="2-create-index"></a>2 Index erstellen
Bevor Sie Ihre Inhalte auf Ihrer Azure Suchdienst hochladen können, müssen Sie zuerst eine Azure Suchindex definieren. Ein Index ist wie eine Datenbanktabelle, die die Daten enthält, und kann Suchabfragen übernehmen. Sie definieren das IndexSchema die Struktur der Dokumente zuordnen, ähnlich wie mit Feldern in einer Datenbank suchen möchten.

Das Schema der diese Indizes kann entweder im Portal Azure oder programmgesteuert [mithilfe des SDK .NET](search-howto-dotnet-sdk.md) oder [REST-API](https://msdn.microsoft.com/library/azure/dn798941.aspx)erstellt werden. Nachdem der Index definiert ist, können Sie dann Ihre Daten mit dem Azure Suchdienst hochladen, wo sie später indiziert ist.

### <a name="3-index-data"></a>3. Daten Index
Nachdem Sie die Felder und Attributen des Indexes definiert haben, können Sie den Inhalt in den Index hochladen. Ein Modell Pushbenachrichtigungen oder Abruf können Sie Daten auf den Index hochladen.

Das Modell Abruf wird über Indexer bereitgestellt, die auf Anforderung oder geplante Updates (siehe [Indizierungsvorgänge (Azure Suche Dienst REST API)](https://msdn.microsoft.com/library/azure/dn946891.aspx)), sodass Sie einfach Daten und Änderungen von Daten aus einer Azure DocumentDB, Azure SQL-Datenbank, Azure BLOB-Speicher oder SQL Server auf eine Azure-virtuellen Computer gehostet Aufnahme für konfiguriert werden können.

Das Modell Pushbenachrichtigungen wird über die SDK oder den REST-APIs verwendet zum Senden von aktualisierter Dokumenten an einen Index bereitgestellt. Sie können Daten aus nahezu jeder Dataset das JSON-Format mit drücken. Finden Sie unter [Hinzufügen, aktualisieren oder Löschen von Dokumenten](https://msdn.microsoft.com/library/azure/dn798930.aspx) oder [zum Verwenden von .NET SDK)](search-howto-dotnet-sdk.md) um Unterstützung für die Daten geladen.

### <a name="4-search"></a>4. Suchen von
Nachdem Sie Ihre Azure Suchindex aufgefüllt haben, können Sie jetzt [Suchabfragen Problem](https://msdn.microsoft.com/library/azure/dn798927.aspx) zu Ihrer Verwendung von einfachen HTTP-Anfragen mit REST-API oder .NET SDK-Endpunkt an.

## <a name="try-it-now-for-free"></a>Probieren Sie es jetzt (kostenlos!)
Sie können noch heute Azure-Suche versuchen! Wenn Sie bereits über ein Azure-Konto besitzen, können Sie [Bereitstellen von einem Dienst in der kostenlosen Ebene](search-create-service-portal.md)an.

Wenn Sie besitzen ein Azure-Konto, dass Sie versuchen können, eine kostenlose, 60-minütigen Sitzung mit keine melden Sie erforderlich an. Wechseln Sie zu der [Azure-App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/p/?LinkId=618214) , und wählen Sie "Web App". Wählen Sie dann die Vorlage "ASP.NET + Azure suchen", um anzufangen.
