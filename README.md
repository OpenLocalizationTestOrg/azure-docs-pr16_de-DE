# <a name="azure-technical-documentation-contributor-guide"></a>Azure technische Dokumentation Mitwirkender Leitfaden

Sie haben das GitHub Repository gefunden werden, das die Quelle für die technischen Unterlagen aufnimmt, die zum Azure-Dokumentation Center unter [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation)veröffentlicht wird.

Dieses Repository enthält auch dabei helfen, Sie zu unseren technischen Unterlagen beitragen.  Eine Liste der Artikel in den Mitwirkenden Leitfaden finden Sie unter [den Index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).

## <a name="contribute-to-azure-documentation"></a>Eigene Notizen hinzufügen können Azure-Dokumentation

Vielen Dank für Ihr Interesse in Azure-Dokumentation!

* [Methoden zum mitwirken](#ways-to-contribute)
* [Verhaltensregeln](#code-of-conduct)
* [Über Ihre Beiträge in Azure Inhalt](#about-your-contributions-to-azure-content)
* [Repository Organisation](#repository-organization)
* [Verwenden von GitHub, Git und dieses repository](#use-github-git-and-this-repository)
* [So Abzug verwenden, um Ihr Thema zu formatieren](#how-to-use-markdown-to-format-your-topic)
* [Feedback, Kommentare und support](./contributor-guide/feedback-and-comments.md)
* [Weitere Ressourcen](#more-resources)
* [Index alle Mitwirkenden Leitfaden Artikeln](./contributor-guide/contributor-guide-index.md) (öffnet die neue Seite)

## <a name="ways-to-contribute"></a>Methoden zum mitwirken 

Sie können auf verschiedene Weise zur [Azure-Dokumentation](http://azure.microsoft.com/documentation/) beitragen:

* Mitwirken Sie auf eine [Forumsdiskussion](http://social.msdn.microsoft.com/Forums/windowsazure/home).
* Übermitteln von Disqus Kommentaren am unteren Rand der Artikel.
* Sie können ganz einfach zu technischen Artikeln, die in der Benutzeroberfläche GitHub mitwirken. Entweder finden Sie im Artikel in diesem Repository, oder besuchen Sie den Artikel auf [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation) , und klicken Sie auf den Link im Artikel, der auf die Quelle GitHub für den Artikel verweist.
* Wenn Sie zu einem vorhandenen Artikel wesentliche Änderungen erstellen möchten, müssen hinzufügen oder Ändern von Bildern oder einen Beitrag einen neuen Beitrag, Sie dieses Repository verzweigen, installieren Git Bash, Abzug Wähltastatur, und erfahren Sie, einige Git Befehle.

##<a name="code-of-conduct"></a>Verhaltensregeln

Dieses Projekt hat das [Microsoft Open Quelle Verhaltensregeln](https://opensource.microsoft.com/codeofconduct/)eingeführt. Weitere Informationen finden Sie unter den [Code durchführen FAQ](https://opensource.microsoft.com/codeofconduct/faq/) oder Kontakt [opencode@microsoft.com](mailto:opencode@microsoft.com) mit zusätzlichen Fragen oder Kommentare.

##<a name="about-your-contributions-to-azure-content"></a>Informationen zu Ihrer Beiträge zu Azure Inhalt

###<a name="minor-corrections"></a>Kleinere Korrekturen

Kleinere Korrekturen oder Clarifications, die Sie zur Dokumentation und Code-Beispiele in dieser Repo übermitteln fallen unter die [Azure Website Nutzungsbedingungen (rechtlichen)](http://azure.microsoft.com/support/legal/website-terms-of-use/).


###<a name="larger-submissions"></a>Größere Übermittlungen

Wenn Sie eine Anforderung Abruf mit neuen oder wesentlichen Änderungen an Dokumentation und Codebeispielen übermitteln, senden wir der Person einen Kommentar in GitHub werden Sie aufgefordert, eine online Beitrag Lizenz Vertrag (CLA) zu senden, wenn Sie in einer dieser Gruppen sind:

* Mitglieder der Gruppe Microsoft Open Technologien.
* Mitwirkenden, die nicht für Microsoft arbeiten.

Wir benötigen Sie das online-Formular ausführen, bevor wir Ihre Abruf Anfrage annehmen können.

Vollständige Details sind unter [http://azure.github.io/guidelines/#cla](http://azure.github.io/guidelines/#cla)verfügbar.

## <a name="repository-organization"></a>Repository Organisation

Der Inhalt im Repository Azure-Inhalt folgt die Organisation Dokumentation auf [Azure.Microsoft.com](http://azure.microsoft.com). Dieses Repository enthält zwei Root-Ordner:

### <a name="articles"></a>\articles

Der Ordner *\articles* enthält Dokumentation Artikel als Abzug Dateien mit der Erweiterung *.md* formatiert.

Artikel im Stammverzeichnis in Azure.Microsoft.com veröffentlicht werden, in den Pfad *http://azure.microsoft.com/documentation/articles/ {Artikel-Name-ohne – Geschäftsführer} /*.

* **Dateinamen-Artikel:** Finden Sie unter [unseren Datei benennen Anleitungen](./contributor-guide/file-names-and-locations.md).

Artikel innerhalb ihrer eigenen-Ordner werden in den Pfad zu Azure.Microsoft.com veröffentlicht *http://azure.microsoft.com/documentation/articles/service-folder/ {Artikel-Namen – ohne-Geschäftsführer} /*

* **Medien Unterordner:** Der *\articles* Ordner enthält den Ordner *\media* für Root Directory Artikel Mediendateien, innerhalb der Unterordner mit den Bildern für jeden Artikel sind.  Die Ordner Dienst enthalten einen separaten Medienordner für den Artikeln innerhalb jeder-Ordner. Die Artikel Bildordner sind in der Datei Artikel minus die Erweiterung *.md* gleichnamigen.

### <a name="includes"></a>\Includes

Erstellen Sie wieder verwendbaren Inhalt Abschnitte, in einen oder mehrere Artikel aufgenommen werden sollen. Finden Sie unter [Benutzerdefiniert Erweiterungen in unseren technischen Inhalten verwendet werden](./contributor-guide/custom-markdown-extensions.md).

### <a name="markdown-templates"></a>\markdown Vorlagen

Dieser Ordner enthält unsere Abzug standard-Vorlage mit grundlegenden Abzug Formatierung, die Sie für einen Artikel müssen.

### <a name="contributor-guide"></a>\contributor-Guide

Dieser Ordner enthält Artikel, die unseren Mitwirkenden Leitfaden gehören.  

## <a name="use-github-git-and-this-repository"></a>Verwenden von GitHub, Git und dieses repository

Informationen zum mitwirken, wie Sie mithilfe der GitHub UI kleinen Änderungen mitwirken und so Verzweigen und das Repository für weitere signifikante Spenden klonen, finden Sie unter [Installieren und Einrichten von Tools für das Schreiben in GitHub](./contributor-guide/tools-and-setup.md).

Wenn Sie GitBash installieren, und wählen Sie Lokales arbeiten, werden die Schritte zum Erstellen einer neuen lokalen arbeiten Verzweigung, Änderung vornimmt, und übermitteln, dass die Änderungen in den Hauptfenster Zweig zurück [Git](./contributor-guide/git-commands-for-master.md) Befehle zum Erstellen eines neuen Artikels oder aktualisieren einen vorhandenen Artikel aufgelistet.

### <a name="branches"></a>Verzweigungen

Es empfiehlt sich, dass Sie lokale Arbeiten Verzweigungen erstellen, die auf einen bestimmten Bereich der Änderung ausgerichtet. Jede Verzweigung sollten auf einem einzigen Konzept/Artikel sowohl zu optimieren Workflow und reduzieren die Möglichkeit, Zusammenführungskonflikte beschränkt.  Die folgenden Aktivitäten werden von den gewünschten Bereich für einen neuen Zweig:

* Ein neuer Beitrag (und die zugehörigen Bilder)
* Rechtschreibung und Grammatik Bearbeitungen auf einen Artikel.
* Anwenden einer Änderung der einzelnen Formatierung für eine große Gruppe von Artikeln (z. B. neue copyright Fußzeile) an.

## <a name="how-to-use-markdown-to-format-your-topic"></a>So Abzug verwenden, um Ihr Thema zu formatieren

Alle Artikel in diesem Repository verwenden GitHub flavored Abzug.  Hier ist eine Liste der Ressourcen ein.

- [Abzug-Grundlagen](https://help.github.com/articles/markdown-basics/)

- [Druckbare Abzug Spickzettel](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

- Die Liste der Abzug Editoren finden Sie unter [Tools und Thema einrichten](./contributor-guide/tools-and-setup.md#install-a-markdown-editor).

## <a name="article-metadata"></a>Artikel Metadaten

Artikel Metadaten können bestimmte Funktionalitäten auf der Website azure.microsoft.com, wie Autor Abschreibung, Mitwirkender Abschreibung, Breadcrumb, Artikel Beschreibungen und SEO Optimierungen sowie reporting Microsoft verwendet, um die Leistung des Inhalts ausgewertet werden soll. Ja, ist die Metadaten wichtig! [Hier die Anleitung für die Metadaten Ihrer sicherzustellen rechts abgeschlossen ist](./contributor-guide/article-metadata.md).

### <a name="labels"></a>Etiketten

Ziehen Sie Besprechungsanfragen helfen Sie uns Abruf Anforderung Workflows verwalten und helfen, damit Sie wissen, was mit Abruf Anforderung geht sind automatisierte Etiketten zugewiesen:

* Beitrag-Lizenzvertrag Zusammenhang
    * CLA nicht erforderlich: die Änderung ist geringfügigen und sind nicht erforderlich, dass Sie eine CLA signieren.
    * CLA erforderlich: der Bereich der Änderung relativ umfangreich ist und setzt voraus, dass Sie eine CLA anmelden.
    * CLA angemeldet: der Mitwirkende angemeldet der CLA aus, damit die Anfrage Abruf zur Überprüfung jetzt nach vorne verschieben kann.
* Spalte Etiketten: wie Etiketten PnP, moderne Apps und TDC helfen die Abruf-Anfragen durch die interne Organisation kategorisiert, die die Anforderung Abruf überprüfen muss.
* Ändern an verfassen gesendet werden: der Autor wurde der Anforderung ausstehend Abruf benachrichtigt.

## <a name="more-resources"></a>Weitere Ressourcen

Finden Sie unter den [Index unsere Mitwirkender des Handbuchs](./contributor-guide/contributor-guide-index.md) für alle Anleitungen-Themen.
