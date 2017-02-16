<properties
    pageTitle="Erstellen von Bildern in Abzug"
    description="Erläutert, wie Sie Bilder in Abzug gemäß den Richtlinien für den Azure Repositorys festlegen zu erstellen."
    services=""
    solutions=""
    documentationCenter=""
    authors="kenhoff"
    manager="ilanas"
    editor="tysonn"/>

<tags
    ms.service="contributor-guide"
    ms.devlang=""
    ms.topic="article"
    ms.tgt_pltfrm=""
    ms.workload=""
    ms.date="06/25/2015"
    ms.author="kenhoff" />

# <a name="create-images-in-markdown"></a>Erstellen von Bildern in Abzug

## <a name="image-folder-creation-and-link-syntax"></a>Abbildung Ordner erstellen und Link-syntax

Für einen neuen Artikel müssen Sie zum Erstellen eines Ordners in folgendem Speicherort:

    /articles/<service-directory>/media/<article-name>/

Beispiel:

    /articles/app-service/media/app-service-enterprise-multichannel-apps/

Nachdem Sie den Ordner und hinzugefügten Bilder zu erstellen, verwenden Sie folgende Syntax Bilder in Ihrem Artikel zu erstellen:

```
![Alt image text](./media/article-name/your-image-filename.png)
```
Beispiel:

Finden Sie [die Vorlage Abzug](../markdown%20templates/markdown-template-for-new-articles.md) ein Beispiel für ein.  Den Bezug Bildlinks in dieser Vorlage Abzug werden am unteren Rand der Vorlage werden soll.

## <a name="guidelines-specific-to-azuremicrosoftcom"></a>Speziell für azure.microsoft.com Richtlinien

Screenshots sind derzeit aufgefordert werden, wenn es nicht möglich, reproduzieren Schritte enthalten ist. Gehen Sie wie folgt Schreiben von Inhalten, um der Inhalt hervorgehoben kann, ohne die Screenshots bei Bedarf.

Verwenden Sie die folgenden Richtlinien beim Erstellen und ClipArt-Dateien, einschließlich:
- Freigeben Sie ClipArt-Dateien nicht über Dokumente. Kopieren Sie die Datei, die Sie benötigen, und es für Ihre bestimmten Thema Medienordner hinzufügen. Freigeben zwischen Dateien ist überfordert, weil es einfacher zu veraltet entfernen ist, Inhalte und Bilder, die die Repo säubern behält.

- Dateiformate: Verwenden von PNG-Dateien - höherer Qualität sind und deren Qualität während der Bereich des verwalten. Andere Dateiformate beibehalten sowie deren Qualität nicht. Das JPEG-Format ist zulässig, aber nicht bevorzugte.  Keine animierten GIF-Dateien.

- Verwenden der standardmäßigen Breite in Paint bereitgestellten Rote Rechtecke (5 px) die Aufmerksamkeit auf bestimmte Elemente in Screenshots.  

    Beispiel:

    ![Dies ist ein Beispiel für ein rotes Quadrat als einer Beschriftung verwendet.](./media/create-images-markdown/gs13noauth.png)

- Wenn es sinnvoll ist, sollten Sie gerne Bilder zuschneiden, damit die Elemente der Benutzeroberfläche in vollständiger Größe angezeigt werden. Stellen Sie sicher, dass im Kontext jedoch für Benutzer, löschen ist.

- Vermeiden Sie Leerzeichen an den Kanten von Screenshots. Wenn Sie einen Screenshot so, die weißen Hintergrund an den Kanten lässt zuschneiden, fügen Sie einen einzelnen graue Pixel-Rahmen um das Bild hinzu.  Wenn Paint verwenden zu können, führen Sie die hellere grauen in der Standard-Farbpalette (0xC3C3C3) aus. Wenn einige andere Grafik app verwenden zu können, ist die RGB-Farbe R195, G195, 195 aus. Sie können einfach ein graues Rahmens um ein Bild hinzufügen, in Visio - Aktion, wählen Sie das Bild, wählen Sie die Linie, und vergewissern Sie sich die die richtige Farbe festgelegt ist, und ändern Sie dann die Linienstärke auf 1 1/2 pt.  Screenshots sollte einen grauen Rahmen von 1 Pixel breiten haben, damit weiße Bereichen des Screenshots nicht in die Webseite verwischen.

    Beispiel:

    ![Dies ist ein Beispiel für einen grauen Rahmen um Abstände zu.](./media/create-images-markdown/agent.png)
    
    Ein Tool Hinzufügen des erforderlichen Rahmens zu Bildern automatisieren helfen finden Sie unter [AddACOMBorder tool - zum Hinzufügen des erforderlichen 1 Pixel grauen Rahmens ACOM Bilder automatisieren](https://github.com/Azure/Azure-CSI-Content-Tools/tree/master/Tools/AddACOMImageBorder).

- Konzeptionelle Darstellung mit einem Leerzeichen einen grauen Rahmen ist erforderlich.  

    Beispiel:

    ![Dies ist ein Beispiel für eine konzeptionelle mit Leerzeichen und ohne graue Rahmen aus.](./media/create-images-markdown/ic727360.png)

- Versuchen Sie nicht, nehmen Sie ein Bild zu breit.  Bilder werden automatisch geändert werden, sofern sie zu groß sind. Das Ändern der Größe manchmal aufgrund der Unschärfe, damit es empfiehlt sich, dass Sie die Breite der Ihre Bilder, um 780 beschränken px und Manuelles Ändern der Größe von Bildern vor dem Absenden, falls erforderlich.

- Befehl Ausgaben in Screenshots anzeigen.  Wenn Ihr Beitrag Schritte enthält, in dem der Benutzer in einer Shell arbeitet, ist es sinnvoll, Screenshots Ausgabe des Befehls angezeigt. Beschränken die Verwaltungsshell Breite von etwa 72 Zeichen im Allgemeinen wird in diesem Fall sichergestellt, dass das Bild in der 780 px Breite Richtlinie bleibt. Bevor Sie an einen Screenshot der Ausgabe an, die Größe des Fensters, sodass nur die relevanten Befehl und die Ausgabe (optional mit einer leeren Zeile auf beiden Seiten) dargestellt wird.

- Nutzen Sie die gesamte Screenshots von Windows, falls möglich. Wenn einen Screenshot in einem Browserfenster aufzeichnen, Ändern der Größe im Browserfenster zu 780 px breit oder weniger und beibehalten die Höhe des Browserfensters als möglichst kurz beispielsweise, die Ihrer Anwendung in das Fenster passt.

    Beispiel:

    ![Dies ist ein Beispiel für einen Screenshot des Browser-Fenster.](./media/create-images-markdown/helloworldlocal.png)

- Gehen Sie vorsichtig vor, mit welchen Informationen in Screenshots eingeblendet wird.  Interner Firmeninformationen oder persönliche Informationen nicht an.

- Verwenden Sie in konzeptkunst oder Diagramme die offizielle Symbole in der Cloud und Enterprise-Symbol und Symbol festlegen. Eine öffentliche Gruppe ist unter http://aka.ms/CnESymbols verfügbar.

- Ersetzen Sie persönliche oder private Informationen in Screenshots mit Platzhaltertext in spitzen Klammern eingeschlossen. Dies umfasst Benutzernamen, Abonnement-IDs und andere verwandte Informationen ein. Namen von persönlichen können mit einer [fictious Namen genehmigt](https://aka.ms/ficticiousnames)(nur Mitarbeiter-Link) ersetzt werden. Verwenden Sie Tipps und Tricks-Stifte oder Marker verdecken oder verwischen persönliche oder private Informationen nicht in Paint.

  Die folgende Abbildung wurde ordnungsgemäß aktualisiert, um die tatsächliche **Abonnement-ID** mit Platzhalterinformationen zu ersetzen:

  ![Private Informationen ersetzt mit Platzhalter](./media/create-images-markdown/placeholder-in-screenshot-correct.png)

### <a name="contributors-guide-links"></a>Mitwirkenden Leitfaden für Links

- [Artikel (Übersicht)](./../README.md)
- [Index der Artikel mit Hinweisen](./contributor-guide-index.md)
