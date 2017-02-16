# <a name="steps-to-follow-when-you-retire-or-change-the-name-of-an-acom-technical-article"></a>Die folgenden Schritte beim zurückziehen oder Ändern des Namens einer ACOM technischen Artikel

Diese Anleitung gilt für KMU, die als Autor eines Artikels aufgeführt sind, die über den Abschnitt technische Dokumentation azure.microsoft.com deaktiviert werden muss. Die Schritte gelten auch, wenn eine Datei umbenannt wird.

Wenn Sie ein Mitglied unserer Azure Community und Sie denken, dass ein Artikel aus irgendeinem Grund deaktiviert werden soll, Schreiben Sie einen Kommentar im Kommentar Disqus Stream für die Artikel, um dem Autor mitgeteilt, dass etwas mit dem Artikel falsch ist.

SME Autoren müssen verschiedene Schritte zum Inhalt, sodass die Benutzer der Website eine schlechte Erfahrung besitzen, wenn wir Inhalt von der Website zurückziehen ordnungsgemäß zu deaktivieren. Im Artikel löschen oder Ändern des Namens sollte zuletzt ausgeführt werden, die passiert!

## <a name="step-1-set-the-article-to-no-indexno-follow-and-republish-it-recommended"></a>Schritt 1: Legen Sie im Artikel auf Nein-Index/Nein-folgen und erneut veröffentlichen Sie (empfohlen)

Erstes zu tun ist zentral im Artikel als keine-Index/Nein-folgen einige Wochen, bevor Sie tatsächlich löschen. Dies ist die beste Methode "Pre Arbeit" für zurückziehen Inhalt angesehen. Dadurch entfernen im Artikel suchen-Engine Indizes, damit Personen im Artikel suchen gefunden wird nicht. [Finden Sie unter dem internen-Wiki Details.](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx)

## <a name="step-2-manage-inbound-links-required"></a>Schritt 2: Verwalten von eingehender Links (erforderlich)

Ermitteln Sie, ob alle Links zu Inhalten, die nicht von Microsoft eingehenden vorhanden sind. Häufig, Blogs, Foren und andere Inhalte im Web verweist auf Artikel. Häufig, beim Arbeiten mit Blogbesitzer dieser Hyperlinks ändern können, und Sie entfernen oder Links vom Beiträge im Forum aktualisieren können. Web Analytics-Tools können Sie feststellen, ob es sich bei hoher Auslastung eingehenden Verknüpfungen, die Sie möglicherweise auf diese Weise verwalten müssen.

## <a name="step-3-remove-all-crosslinks-to-the-article-from-the-technical-content-repository-required"></a>Schritt 3: Entfernen Sie alle Querverbindungen im Artikel über das technische Content Repository (erforderlich)

Verlassen Sie sich nicht auf die Umleitung zur Querverbindungen aus anderen Artikeln erledigen. Aktualisieren oder Entfernen der Cross im Artikel verweist, löschen oder umbenennen möchten, im Besitz von anderen Personen in den Artikeln einschließlich.

1. Stellen Sie sicher, arbeiten Sie in einer auf dem neuesten Stand lokalen Verzweigung – ausführen `git pull upstream master` (oder die entsprechende Variation anhand dieser Befehl.

2.  Prüfen Sie den Ordner Azure-Inhalt-Kurs/Artikeln und der Azure-Inhalt-Kurs/enthält Ordner für alle Artikel und enthält die Verknüpfung im Artikel, die, den Sie nicht mehr verwenden möchten, und Entfernen der Querverbindungen oder Ersetzen Sie sie durch eine entsprechenden neuen Querverbindungen. Sie können eine suchen und Ersetzen Programm, um die Querverbindungen zu finden, wenn Sie eine installiert haben. Wenn Sie diese nicht wünschen, können Sie Windows PowerShell kostenlos! So sieht so PowerShell verwenden, um die Querverbindungen zu ermitteln:

 ein. Starten Sie Windows PowerShell.

 b. Ändern der PowerShell dazu aufgefordert werden in den Ordner Azure-Inhalt-Pr\articles:

 `cd azure-content-pr\articles`

 c. Geben Sie diesen Befehl, mit denen alle Dateien aufgelistet werden, die einen Verweis auf die Artikel enthalten, die Sie löschen möchten:

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  Wenn Sie es vorziehen, die Liste der Dateinamen in eine Textdatei (in diesem Fall, benannten psoutput.txt) zu senden, können Sie folgende Schritte ausführen:

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. Fügen Sie hinzu und übernehmen Sie alle Ihre Änderungen zu, drücken Sie diese, um Ihre Verzweigung und erstellen Sie eine Anforderung Abruf, um Ihre Änderungen aus der Verzweigung in der Gestaltungsvorlage Verzweigung des Hauptfenster Repositorys zu verschieben.

## <a name="step-4-update-the-fwlink-tool-required"></a>Schritt 4: Aktualisieren des FW-Tools (erforderlich)

Aktivieren Sie das FW-Tool für alle FWLinks, die zum Artikel zum verweisen möglicherweise ein. Zeigen Sie alle FWLinks am Ersatz Inhalt; Wenn Sie nicht auf den Alias, die den Link besitzt sind, ordnen Sie ihn. Wenn der Besitzer den Link wird nicht aktualisiert werden, die Datei ein Ticket mit MSCOM auf den Link geändert haben. Weitere Informationen - [internen Wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx).

## <a name="step-5-remove-all-crosslinks-to-the-article-from-other-pages-on-azuremicrosoftcom-and-create-a-redirect-for-the-retired-page-if-appropriate-required"></a>Schritt 5: Entfernen Sie alle Querverbindungen im Artikel aus anderen Seiten auf azure.microsoft.com und erstellen Sie eine Umleitung für die Seite deaktiviert, wenn der entsprechende (erforderlich)

Sie müssen mit der Person zusammenarbeiten, die verwaltet und aktualisiert die Startseite Dokumentation des Diensts für dieses Webpart. Wenn Sie nicht wissen, wer die betreffende Person ist, wenden Sie sich an Ihren Inhalt Team-Partner. Die Person, die verwaltet und aktualisiert die Startseite Dokument müssen zwei Schritte ausführen:

1. Scannen Sie in Visual Studio die **gesamte** ACOM Web Lösung für Querverweisen zu der Datei zu deaktivieren. Entfernen Sie der Querverweise oder mit einer aktualisierten Querverweis zu ersetzen. Sie müssen die HTML-Links als auch die zugehörige Ressourcenzeichenfolgen für die HTML-Links zu entfernen. Weitere Informationen - finden Sie unter den [internen Wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Create%20or%20edit%20a%20service%20landing%20page%20or%20left%20nav.aspx)

2. Wenn ein Artikel Ersatz vorhanden ist, erstellen Sie eine Umleitung ein. Weitere Informationen - finden Sie unter den [internen Wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx).

3. Überprüfen Sie die Änderungen in das Repository ein.

## <a name="step-6-retire-the-article"></a>Schritt 6: Zurückziehen Sie im Artikel

Nachdem haben Sie die vorherigen Schritte ausgeführt haben und diese Änderungen live sind, können Sie den Artikel aus dem Repository löschen. 

**Wichtige:** Wenn Sie Dateien löschen möchten, müssen Sie verwenden die `git add --all` Befehl.

## <a name="step-7-remove-links-from-msdn-required"></a>Schritt 7: Entfernen von Links aus MSDN (erforderlich)

Überprüfen Sie das Inhalte f & a-Tool für fehlerhaften Verknüpfungen mit dem Thema veralteten oder umbenannte und entfernen/Fix die Links in alle MSDN-Themen betroffen.

## <a name="step-8-remove-cached-pages-from-search-engines-only-if-absolutely-necessary"></a>Schritt 8: Entfernen von zwischengespeicherten Seiten aus Suchmaschinen (nur, wenn unbedingt erforderlich)

Führen Sie diese nur, wenn der Inhalt schnell aufgrund gesetzlichen oder schwerwiegenden Kundenproblemen entfernt werden muss. Pro best Practices aus Google sollte normaler Priorität Seite löschen nur von natürlich Search Engine Prozesse behandelt werden. Wechseln Sie zu dieser Webseiten zwischengespeicherte Webseiten von Suchmaschinen zu entfernen:

[Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>Mitwirkenden Leitfaden für links

- [Artikel (Übersicht)](./../README.md)
- [Index der Artikel mit Hinweisen](./contributor-guide-index.md)
