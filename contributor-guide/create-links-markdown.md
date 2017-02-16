<properties
   pageTitle="Erstellen von Links in Abzug Artikel" description="Erläutert, wie Sie in Abzug Querverbindungen Fehlercode." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="02/03/2015" ms.author="tysonn" />

# <a name="linking-guidance-for-azure-technical-content"></a>Hinweise zur Azure technische Inhalte verknüpfen

### <a name="links-from-one-acom-article-to-another"></a>Links aus einem ACOM Artikel zu einem anderen

Verwenden Sie die folgende Syntax für den Link zum Erstellen einer Verknüpfung mit eingebetteten aus einem ACOM technische Artikel zu einem anderen ACOM technischen Artikel:  

- Ein Artikel in einem Dienst Directory Links auf einen anderen Beitrag im selben Dienstverzeichnis:

  `[link text](article-name.md)`

- Ein Artikel Links aus einem Dienst Unterverzeichnis im Stammverzeichnis zu:

  `[link text](../article-name.md)`

- Ein in der Stammwebsitesammlung Directory Link führt zu einem Artikel in einem Unterverzeichnis Service-Artikel: 

  `[link text](./service-directory/article-name.md)`

- Ein Artikel in einem Dienst Unterverzeichnis Links zu einem Artikel in einem anderen Dienst Unterverzeichnis:

  `[link text](../service-directory/article-name.md)`
 

## <a name="links-to-anchors"></a>Links zu Anker werden

Sie verfügen nicht über Anker erstellen – sie werden automatisch generiert bei Zeit für alle H2 Überschriften veröffentlichen. Sie nur müssen das einzige ist Links zu den Abschnitten H2 erstellen.

- Zum Erstellen einer Verknüpfung zu einer Überschrift innerhalb des gleichen Artikels:

  `[link](#the-text-of-the-H2-section-separated-by-hyphens)`  
  `[Create cache](#create-cache)`

- Zum Erstellen einer Verknüpfung zu einem Anker in einem anderen Artikel im gleichen Unterverzeichnis:

  `[link text](article-name.md#anchor-name)`
  `[Configure your profile](media-services-create-account.md#configure-your-profile)`

- Zum Erstellen einer Verknüpfung zu einem Anker in einem anderen Dienst Unterverzeichnis:

  `[link text](../service-directory/article-name.md#anchor-name)`
  `[Configure your profile](../service-directory/media-services-create-account.md#configure-your-profile)`

Eine Möglichkeit zum Erstellen von Links in Ihrer Artikel zu automatisch generierten Anker Links automatisieren ist [MarkdownAnchorLinkGenerator - eines Tools zum Anker Links für ACOM im richtigen Format generieren](https://github.com/Azure/Azure-CSI-Content-Tools/tree/master/Tools/ACOMMarkdownAnchorLinkGenerator).

## <a name="links-from-includes"></a>Enthält Links aus

Da gehören Dateien in ein anderes Verzeichnis befinden, mehr relative Pfade verwenden, wie unten dargestellt werden sollen. Um einen Artikel aus einer Datendatei einschließen verknüpfen, verwenden Sie dieses Format ein:

    [link text](../articles/service-folder/article-name.md)
    
Weitere Informationen zum Verwenden einer Include-Datei in die [benutzerdefinierte Abzug Erweiterungen Richtlinien](custom-markdown-extensions.md#includes).

## <a name="links-in-selectors"></a>Links in Selektoren

Wenn Sie Selektoren, die in einer einschließen eingebettet wurden verfügen, verwenden Sie diese Art von verknüpfen: 

    > [AZURE. ANSICHTSAUSWAHL-Liste (Dropdown1 | Dropdown2)]     -  [(Text1 | Beispiel1)](../articles/service-folder/article-name1.md)
    - [(Text1 | Example2)](../articles/service-folder/article-name2.md)
    - [(Text2 | Beispiel3)](../articles/service-folder/article-name3.md)
    - [(Text2 | Example4)](../articles/service-folder/article-name4.md)


## <a name="reference-style-links"></a>Bezugsart links

Verweis Formatvorlage Links können Sie um den Quellinhalt leichter lesbar zu machen. Weiterführende Verweise Formatvorlage ersetzen die Syntax der eingebetteten Link durch vereinfachte Syntax, die Sie lange URLs am Ende dieses Artikels wechseln kann. Hier Daring Fireball Beispiel:

Text eines eingebetteten:

    I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].

Erstellen einer Verknüpfung Verweise am Ende dieses Artikels:

    <!--Reference links in article-->
    [1]: http://google.com/
    [2]: http://search.yahoo.com/  
    [3]: http://search.msn.com/

Stellen Sie sicher, dass Sie den Abstand nach dem Doppelpunkt, bevor Sie den Link einfügen. Wenn Sie mit anderen technischen Artikeln, verknüpfen, wenn Sie vergessen, das Leerzeichen enthalten, wird der Link im veröffentlichten Artikel fehlerhaft. 

## <a name="link-to-acom-pages-that-are-not-part-of-the-technical-documentation-set"></a>Link zu ACOM Seiten, die nicht Bestandteil der technischen Unterlagen sind

Link zu einer Seite auf ACOM (beispielsweise einer Seite Preisgestaltung, Vereinbarung zum SERVICELEVEL Seite oder Sonstiges, der keiner Dokumentation Artikel), verwenden Sie eine absolute URL, aber nicht das Gebietsschema angeben. In diesem Fall, Links funktionieren in GitHub, und klicken Sie auf der Website gerenderten:

    [link text](http://azure.microsoft.com/pricing/details/virtual-machines/)


## <a name="link-to-msdn-or-technet"></a>Link zu MSDN- oder TechNet

Wenn Sie mit MSDN- oder TechNet zu verknüpfen müssen, verwenden Sie den vollständigen Link zum Thema und entfernen Sie die En-uns Gebietsschema aus den Link. 

### <a name="use-friendly-link-text-for-all-links"></a>Verwenden Sie für alle Links angezeigten Linktext

Sollten die Wörter, die Sie in einen Link einfügen geeignet – Kurzum, sie sollten normalen englischen Wörtern oder den Titel der Seite, die Sie eine Verknüpfung herstellen. Verwenden Sie nicht "hier klicken". Es für SEO fehlerhaft ist, und das Ziel nicht ausreichend beschreiben.

**Richtig:**

- `For more information, see the [contributor guide index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`

- `For more details, see the [SET TRANSACTION ISOLATION LEVEL](https://msdn.microsoft.com/library/ms173763.aspx) reference.`

**Falsch:**

- `For more details, see [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).`

- `For more information, click [here](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`


## <a name="fwlinks"></a>FWLinks

Vermeiden Sie FWLinks (unsere Umleitung-System) in azure.microsoft.com Inhalte an. Sie sollten nur als letzte Möglichkeit verwendet werden, wenn Sie eine Verknüpfung für eine Seite zu erstellen, deren URL, die Sie noch nicht wissen müssen. Sie sind fast nie tatsächlich benötigt. Für ACOM definieren Sie den Dateinamen, damit Sie sofort erfahren, wie er im Voraus angezeigt wird. Für eine Bibliothek Thema, die noch nicht veröffentlicht ist, können Sie einen Link erstellen, der im Thema GUID verwendet, damit Sie nicht mit einer FWLink.

Wenn Sie ein FW auf einer Webseite verwenden müssen, fügen Sie den Parameter P, damit sie eine dauerhafte Umleitung wird:

    http://go.microsoft.com/fwlink/p/?LinkId=389595

Wenn Sie die Ziel-URL in das Tool FW einfügen, denken Sie daran, die das Gebietsschema ist Ihr Ziel Link ACOM, oder die MSDN- oder TechNet-Bibliothek zu entfernen.

## <a name="remember-the-azure-library-chrome"></a>Vergessen Sie nicht die Azure Bibliothek Chrome!
Wenn Sie mit einem Thema Azure-Bibliothek verknüpfen, die unter [diesem Knoten](https://msdn.microsoft.com/library/azure)befindet sich möchten, denken Sie daran, die Azure angeben Chrome in den Link (/ Azure /). Die Azure Chrome teilt die Navigationsoptionen ACOM und zeigt nur den Azure Inhalt der MSDN-Bibliothek. Ein Link ordnungsgemäß Suchbegriffs sieht wie folgt aus:

    http://msdn.microsoft.com/library/azure/dd163896.aspx

Andernfalls wird die Seite in der MSDN-Standardansicht, mit der gesamte MSDN-Struktur angezeigt gerendert werden.

### <a name="contributors-guide-links"></a>Mitwirkenden Leitfaden für Links

- [Artikel (Übersicht)](./../README.md)
- [Index der Artikel mit Hinweisen](./contributor-guide-index.md)

<!--image references-->
[1]: ./media/create-tables-markdown/table-markdown.png
[2]: ./media/create-tables-markdown/break-tables.png
