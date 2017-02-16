<properties pageTitle="Git Befehle zum Erstellen eines neuen Artikels oder aktualisieren einen vorhandenen Artikel" description="Schritte zum Arbeiten mit der Azure technische Inhalte GitHub Repositorys." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="01/16/2015" ms.author="tysonn" />

# <a name="git-commands-for-creating-a-new-article-or-updating-an-existing-article"></a>Git Befehle zum Erstellen eines neuen Artikels oder aktualisieren einen vorhandenen Artikel


## <a name="standard-process-working-from-master"></a>Standard-Prozess (beginnend mit Master)
Führen Sie die Schritte in diesem Artikel, um eine lokale Arbeiten Verzweigung auf Ihrem Computer zu erstellen, damit Sie einen neuen Artikel für den Abschnitt technische Dokumentation von azure.microsoft.com erstellen oder einen vorhandenen Artikel aktualisieren können.

1. Starten Sie Git Bash (oder die Befehlszeile Tool können Sie Git).

 **Hinweis:** Wenn Sie im öffentlichen Repository arbeiten, ändern Sie Azure-Inhalt-Kurs in Azure-Inhalte in alle Befehle.

2. Wechseln Sie zum Azure-Inhalt-Kurs:

        cd azure-content-pr
3. Schauen Sie sich den master Zweig:

        git checkout master

4. Erstellen einer frisch lokalen arbeiten Niederlassung aus dem master Zweig abgeleitet:

        git pull upstream master:<working branch>


5. Wechseln Sie in der neuen Verzweigung arbeiten:

        git checkout <working branch>

6. Lassen Sie Ihre Verzweigung fest, ob Sie die lokale Arbeiten Zweigstelle erstellt haben:

        git push origin <working branch>

7. Ihr neuen Beitrag erstellen oder Ändern eines vorhandenen Artikels. Verwenden Sie Windows Explorer öffnen und neue Abzug Dateien erstellen und Atom (http://atom.io) als Abzug Editor verwenden. Nachdem Sie erstellt oder Ihrer Artikel und Bilder geändert haben, wechseln Sie zu dem nächsten Schritt fort.

8. Hinzufügen und die vorgenommenen Änderungen zu übernehmen:

        git add .
        git commit –m "<comment>"
        
   Oder nur die speziellen hinzuzufügenden Dateien Sie geändert:

        git add <file path>
        git commit –m "<comment>"

   Wenn Sie Dateien gelöscht haben, müssen Sie dies zu verwenden:
   
        git add --all
        git commit -m "<comment>"

9. Aktualisieren Sie den Zweig für Ihren lokalen Arbeiten mit den Änderungen in der übergeordneten aus:

        git pull upstream master

10. Drücken Sie die Änderungen an der Verzweigung GitHub:

        git push origin <working branch>

12. Wenn Sie den Inhalt in den übergeordneten master Zweig für das staging von Gültigkeitsregeln, und/oder Veröffentlichung, und übermitteln Sie bereit sind in der GitHub UI, erstellen Sie eine Anforderung Abruf aus Ihrer Verzweigung in den master Zweig.

13. Wenn Sie ein Mitarbeiter sind arbeiten in der privaten Repository die Änderungen, die Sie senden, werden automatisch bereitgestellt und ein Link staging bezieht sich auf die Anfrage Abruf. Überprüfen Sie die bereitgestellte Inhalte, und melden Sie sich Deaktivieren der Abruf Anforderung Kommentare durch Hinzufügen des Kommentars **Signieren # deaktivieren** .  Dies zeigt an, ob die Änderungen werden live abgelegt werden.  Wenn Sie nicht die Abruf Anfrage angenommen werden – möchten, wenn Sie nur die Änderungen - staging sind fügen Sie die Notiz **Hold # deaktivieren** der Anforderung Abruf hinzu.

14. Der Abruf Anforderung Acceptor prüft Abruf Anforderung, stellt Feedback und/oder Abruf Anforderung akzeptiert. 

15. Überprüfen Sie Ihre veröffentlichten Artikel oder Änderungen am optional

 http://Azure.Microsoft.com/Documentation/articles/*name-of-your-article-without-the-MD-extension*

## <a name="publishing"></a>Veröffentlichen

- Artikel sind bei ungefähr 10:00 Uhr und 3:00 Uhr veröffentlicht pazifischen Raum Zeit, Montag bis Freitag. Sie können nach Artikeln online nach dem Veröffentlichen angezeigt werden bis zu 30 Minuten dauern. Denken Sie daran, dass Abruf Anforderung wurde von einem Abruf Anforderung Bearbeiter zusammengeführt werden, bevor die Änderungen in der nächsten geplanten Veröffentlichung ausführen berücksichtigt werden können. Sie arbeiten mit sich der Abruf Anforderung Bearbeiter im voraus, um sicherzustellen, dass die Anforderung einer Abruf für eine bestimmte Veröffentlichung ausführen zusammengeführt werden sollen. Andernfalls werden PRs in der Reihenfolge überprüft, die sie übermittelt wurden.

- Wenn Sie ein Mitarbeiter arbeiten in der privaten Repository gespeichert sind, werden alle Abruf Anfragen Gültigkeitsregeln, die die müssen beseitigt werden, bevor die Anfrage Abruf zusammengeführt werden kann. 

## <a name="working-with-release-branches"></a>Arbeiten mit Version Verzweigungen

Wenn Sie bei einer Version Verzweigung arbeiten, ist die beste Methode zum Erstellen einer lokalen arbeiten Niederlassung aus dem Release Zweig dieser Befehlssyntax verwenden:

    git checkout upstream/<upstream branch name> -b <local working branch name>

Dadurch wird die lokale Zweigstelle direkt von der übergeordneten Zweig zur Vermeidung von lokalen Zusammenführen erstellt.

