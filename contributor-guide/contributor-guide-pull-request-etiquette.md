# <a name="pull-request-etiquette-and-best-practices-for-microsoft-contributors-to-azure-documentation"></a>Ziehen Sie die Anfrage Etikette sowie optimale Methoden für Mitwirkende Microsoft Azure-Dokumentation

Um die Änderungen in der Dokumentation veröffentlicht werden, senden Sie Abruf Anfragen aus der Verzweigung. Jeder Abruf Anforderung wurde vor zusammenzuführenden überprüft werden. Lesen Sie diesen Artikel, um zu verstehen, wie Sie mit Abruf Anforderung Bearbeitern funktionieren sollten, und wie Sie Abruf Anfragen erstellen können sind, schneller und um zu überprüfen, damit die Warteschlange Abruf besser allen passt.

## <a name="working-with-pull-request-reviewers"></a>Arbeiten mit Abruf Anforderung Bearbeiter

Hier sind die Grundlagen, die Sie zum Arbeiten mit Abruf Anforderung Bearbeitern wissen müssen. 

- <b>Einführung in die Rolle des Bearbeiters Anforderung abrufen. Der Bearbeiter:</b>
  - Damit ist sichergestellt die grundlegende Qualität des Inhalts
  - Verhindert, dass Regressionen im repository
  - Stellt Feedback vor dem Zusammenführen

  Abruf Anforderung Bearbeitern befinden sich in einer Rolle Inhalt Governance. Die primäre Absicht ist nicht so einfach zusammen welchen so schnell wie möglich übermittelt wird. Erwarten Sie Feedback, das Sie Updates, insbesondere für neue und stark überarbeiteten Artikel treffen werden muss.

- <b>Planen Sie mit Ihrem Abruf Anforderung Bearbeiter im voraus:</b>
  - Für den Abruf von hoher Priorität Besprechungsanfragen
  - Abruf Anfragen zeitlich zu vom behoben Versionen
  - Abruf Besprechungsanfragen, die ändern, oder fügen viele Dateien

- <b>Vereinbarung zum SERVICELEVEL für den Abruf anfordern überprüfen</b>

  In der privaten Repository versucht jedes Mal Abruf Anforderung die Warteschlange mit der Bezeichnung sofort auf Seriendruck Abruf gibt das Team überprüfen die Anfrage Abruf innerhalb von 12 Geschäftszeiten (M – F, 8-17 Uhr) und ebenfalls Feedback oder zusammengeführt werden, wenn kein Feedback erforderlich ist. Diese Vereinbarung zum SERVICELEVEL gilt der Vorgang, überprüfen den Kurs, die es nicht zusammengeführt. Wenn sie [die Kriterien für das Zusammenführen von](contributor-guide-pr-criteria.md)erfüllen werden PRs zusammengeführt. 

## <a name="make-the-pull-request-queue-work-better-for-everyone"></a>Stellen Sie der Warteschlange Abruf besser für jeden arbeiten

Es gibt zwei grundlegende Fakten in der Warteschlange Kurs:

- Abruf Besprechungsanfragen, die im Bereich klein sind und sehr ähnlich Änderungen enthalten, dauern weniger Zeit, um zu überprüfen. 
- Abruf Besprechungsanfragen, die im Bereich groß sind oder andere, gemischte Arten von Änderungen enthalten, dauert länger überprüfen.

Sie können die Warteschlange Abruf besser arbeiten, indem Sie die folgenden bewährten gestalten:

- Eigenständige kleinere Updates zu vorhandenen Artikeln neue Beiträge oder Haupt-schreibt. Klicken Sie auf diese Änderungen in separaten arbeiten Verzweigungen arbeiten. 

- Wenn Sie den Artikel oder Bilder löschen, mischen Sie nicht den Löschvorgang mit neuen Inhalt Ergänzungen oder Updates. Den Änderungen/neuen Inhalt in einem separaten arbeiten Zweig zu behandeln.

- Planen Sie für Versionen oder die Umgestaltung von Inhalten im voraus mit Ihrem Kurs Bearbeiter. Möglicherweise vertretene Hilfe zum Erstellen einer Version Verzweigung oder zum Koordinieren von Zusammenführen Zeiten mit Zeiten veröffentlichen, damit Ihr Inhalt zum richtigen Zeitpunkt veröffentlicht wird.

- Wenn Sie versuchen, in der ACOM Repo (ie, Änderungen linken Navigationsbereich Start Seiten, leitet oder Learning Karten) mit Änderungen im Repository Azure-Inhalt-Kurs vorgenommenen Aktualisierungen zu koordinieren, müssen Sie im voraus, dass die Arbeit mit Ihrem Kurs Bearbeiter koordinieren. Andernfalls riskieren Sie eine große Anzahl fehlerhaften Verknüpfungen.

## <a name="criteria-for-expedited-pull-requests"></a>Kriterien für beschleunigte Abruf Besprechungsanfragen

- Wenden Sie sich an Azdocprs zu beschleunigen PRs nur, wenn Sie unbedingt erforderlich. Können Sie anfordern, geliefert Kurs Behandlung für rote Zone, Legal, Datenschutz und Sicherheitsprobleme; für Kunden wirklich fehlerhafte Erfahrung; und Geschäftsleitung der Benutzerberechtigungen. 
- Inhalt für die Funktion Versionen nicht geeignet ist, für die Behandlung von expedited - Feature Release Inhalt erfordert Planung im Vorfeld oder müssen Sie durch die standardmäßige Prioritätswarteschlange behandelt werden.


## <a name="in-a-hurry-submit-prs-that-can-be-accepted-automatically"></a>Eilig? Übermitteln PRs, die automatisch akzeptiert werden können

Verwenden Sie die PRMerger Automatisierungsregeln, um mehrere Ihrer täglichen PRs automatisch zusammengeführt abzurufen.

PRMerger kann Ihre Kurs automatisch, wenn übernehmen:
* Er wirkt sich auf 10 Dateien oder weniger.
* Sie enthält 15 Commit oder weniger.
* Weniger als 20 % des vornehmen.
* Selektoren/Switchers werden nicht aktualisiert.
* Keine Dateien gelöscht oder hinzugefügt.
* Keine Bilder sind neu, geändert oder gelöscht.

Wenn Ihre Anforderung Abruf nicht diese Kriterien erfüllt, wird die Bezeichnung "PRmerger kann nicht zusammenführen" automatisch, damit Sie wissen, dass es überprüfen erfordert von zugewiesen ein Kurs Prüfer.

### <a name="need-to-make-a-lot-of-little-changes"></a>Viele kleinen Änderungen vornehmen müssen?

Optimieren Ihrer in Übereinstimmung mit den oben genannten PRMerger Automatisierungsregeln, und gehen Sie folgendermaßen vor:
* Übermitteln Sie Artikel mit light Änderungen zusammen in einer Kurs mit 10 oder weniger Dateien.
* Erstellen einer separaten Kurs nach Artikeln welche Bilder oder Selektoren ändern. Setzt personenbezogenen überprüfen.
* Erstellen einer separaten Kurs neue oder gelöschte Artikel. Setzt personenbezogenen überprüfen.

## <a name="related"></a>Zusammenhang

- [Qualitätskriterien für Abruf Anforderung überprüfen](contributor-guide-pr-criteria.md)

- [Ziehen Sie die Anfrage Kommentar Automatisierung](contributor-guide-pull-request-comments.md)
