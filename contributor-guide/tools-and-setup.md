<properties
pageTitle="Installieren und Einrichten von Tools für das Schreiben in GitHub"
description="Tools und Schritte können authoring Azure Inhalt GitHub festgelegt werden."
services="contributor-guide"
documentationCenter=""
authors="tysonn"  
manager="carolz" />

<tags
ms.service="contributor-guide"
 ms.devlang=""
 ms.topic="article"
  ms.tgt_pltfrm=""
  ms.workload=""
  ms.date="01/19/2015"
  ms.author="tysonn" />

#<a name="install-and-set-up-tools-for-authoring-in-github"></a>Installieren und Einrichten von Tools für das Schreiben in GitHub

Führen Sie die Schritte in diesem Artikel zum Einrichten von Tools für die Azure technische Dokumentation beitragen. Gelegentliche und nur gelegentliche Mitwirkenden können wahrscheinlich die GitHub Benutzeroberfläche, die in Schritt 2 beschrieben.

Wenn Sie mit Git nicht vertraut sind, möchten Sie möglicherweise einige Git Terminologie überprüfen: [https://help.github.com/articles/github-glossary](https://help.github.com/articles/github-glossary). Dieser Thread StackOverflow enthält darüber hinaus ein Glossar Git in diesen Schritten treten: [http://stackoverflow.com/questions/7076164/terminology-used-by-git](http://stackoverflow.com/questions/7076164/terminology-used-by-git)

## <a name="contents"></a>Inhalt

- [Erstellen Sie ein Konto GitHub und richten Sie des eigenen Profils ein](#create-a-github-account-and-set-up-your-profile)
- [Melden Sie sich bei Disqus](#sign-up-for-disqus)
- [Prüfen Sie, ob Sie wirklich führen Sie die restlichen Schritte müssen](#determine-whether-you-really-need-to-follow-the-rest-of-these-steps)
- [Berechtigungen in GitHub](#permissions-in-github)
- [Installieren von Git für Windows](#install-git-for-windows)
- [Aktivieren der Authentifizierung mit zwei Faktoren](#enable-two-factor-authentication)
- [Installieren Sie einen Abzug-editor](#install-a-markdown-editor)
- [Atom konfigurieren](#configure-atom)
- [Verzweigung Repository und kopieren Sie sie auf Ihren computer](#fork-the-repository-and-copy-it-to-your-computer)
- [Konfigurieren Sie Ihren Benutzernamen und e-Mail-lokal](#configure-your-user-name-and-email-locally)
- [Nächste Schritte](#next-steps)

## <a name="create-a-github-account-and-set-up-your-profile"></a>Erstellen Sie ein Konto GitHub und richten Sie des eigenen Profils ein

Um zu den Azure technischen Inhalt beitragen, benötigen Sie ein Konto [GitHub](http://www.github.com) .

Wenn Sie ein Microsoft-Mitwirkender sind, müssen Sie Einrichten Ihres Kontos GitHub festlegen, damit Sie als Mitarbeiter von Microsoft eindeutig festgelegt sind. Richten Sie Ihr Profil wie folgt aus:

- **Profilbild**: ein Bild von Ihnen (erforderlich)
- **Name**: der vor- und Nachnamen ein, (erforderlich)
- **E-Mail**: Ihre Microsoft e-Mail-Adresse (optional)
- **Unternehmen**: vorbehalten (erforderlich)
- **Standort**: Liste Ihren Standort (optional)

Ihr Profil in etwa dieses Profil:

<p align="center">
 ![Beispiel für GitHub Profil](./media/tools-and-setup/githubprofile.png)

## <a name="sign-up-for-disqus"></a>Melden Sie sich bei Disqus

Jeder veröffentlichten Azure technischen Artikel hat einen Kommentar Stream vom Dienst Disqus bereitgestellt.

 ![Discus-logo](./media/tools-and-setup/discus.png)

Wenn Sie ein Microsoft-Mitarbeiter sind, und wenn Sie der Autor des oder einen Mitwirkenden führt zu einem Artikel sind, müssen Sie für Disqus registrieren, damit Sie im Kommentar Stream für den Artikel teilnehmen können.

1. Melden Sie für ein Konto bei [http://www.disqus.com/ an](http://www.disqus.com/)
2. Füllen Sie Ihr Profil wie folgt aus:

 - **Vollständiger Name**: Ihren vollständigen Namen als Ihr Microsoft-Adresse Adressbuch Angebot sowie Klammern Informationen, also der Alias angezeigten plus @MSFT. Format: *zuerst die letzten [alias@MSFT] *
 - **Standort**: Ihres Standorts
 - **Kurze Lebenslauf**: Ihren Titel

## <a name="determine-whether-you-really-need-to-follow-the-rest-of-these-steps"></a>Prüfen Sie, ob Sie wirklich führen Sie die restlichen Schritte müssen

Sie müssen nicht alle Schritte in diesem Artikel ausführen. Sortieren von Inhalten Beitrag vornehmen müssen oder soll das hängt.

###<a name="submit-a-text-only-change-to-an-existing-article"></a>Senden Sie eine nur-Text-Änderung führt zu einem vorhandenen Artikel

Wenn Sie nur müssen oder Textinhalt Updates zu einem vorhandenen Artikel vornehmen möchten, müssen Sie wahrscheinlich führen Sie die restlichen Schritte. GitHubs webbasierten Abzug-Editor können und übermitteln Sie Ihre Änderungen. Klicken Sie einfach auf den Link GitHub im Artikel, die Sie ändern möchten:

 ![Beispiel für GitHub Profil](./media/tools-and-setup/contributetogit.png)

 Klicken Sie dann auf das Symbol zum Bearbeiten in der GitHub Version des Artikels

 ![Beispiel für GitHub Profil](./media/tools-and-setup/editicon.PNG)

 Den einfach zu verwendendes Web-Editor, der Änderungen übermitteln erleichtert wird geöffnet. Sie müssen nicht die anderen in diesem Artikel beschriebenen Schritte.

###<a name="all-other-changes"></a>Alle anderen Änderungen
Die GitHub UI unterstützt die Erstellung der neuen Dateien sowie ziehen und Ablegen von Bildern. Bei der Arbeit in der Benutzeroberfläche kann verwalten Verzweigungen verwirrend jedoch, damit es in der Regel wird empfohlen, installieren Sie die Tools, und erfahren Sie, die Befehle für das Erstellen und Verwalten von Artikeln. Wenn Sie die Benutzeroberfläche verwenden möchten, finden Sie unter:

- [Erstellen von Dateien auf Github](https://github.com/blog/1327-creating-files-on-github)
- [Hochladen von Dateien auf Ihre Repositorys](https://github.com/blog/2105-upload-files-to-your-repositories)

Für die folgenden Arten von Arbeit empfehlen wir Sie installieren und erfahren, wie Sie die Tools verwenden:

 - Bereitstellen von wichtigsten Änderungen für einen Artikel
 - Erstellen und Veröffentlichen von ein neuer Beitrag
 - Hinzufügen von neuen Bildern oder Aktualisieren von Bildern
 - Aktualisieren einen Artikel über einen Zeitraum von Tagen ohne publishing Änderungen an diesen Tagen
 - Erstellen von Inhalten in einer Version, die sich an einem bestimmten Tag zu einem bestimmten Zeitpunkt zu wechseln

##<a name="permissions-in-github"></a>Berechtigungen in GitHub

Jeder mit einem Konto GitHub kann auf Azure technischen Inhalt über unsere öffentlichen Repository am [https://github.com/Azure/azure-content](https://github.com/Azure/azure-content)mitwirken. Es sind keine besonderen Berechtigungen erforderlich.

Wenn Sie ein Microsoft-PM sind oder Autor, wer arbeitet, auf Inhalt, müssen Sie in unseren privaten gemeinsam Azure Content Repository Azure-Inhalt-Kurs. Besuchen Sie [https://repos.opensource.microsoft.com](https://repos.opensource.microsoft.com ) , um die Leseberechtigungen anfordern, mit denen Beiträge über die private Repo - erleichtern melden Sie sich bei GitHub mithilfe der Schaltfläche > Azure klicken Sie auf > Klicken Sie auf **Verknüpfung ein Team** oder die **Teilnahme an ein anderes Team**, und suchen Sie nach und teilnehmen an der Gruppe **Azure-Inhalt-gelesen** .

## <a name="install-git-for-windows"></a>Installieren von Git für Windows

Installieren Sie Git aus [http://git-scm.com/download/win](http://git-scm.com/download/win)für Windows. Dieser Download Installationen Git Version Control System und es Git Bash, die Befehlszeile app, mit denen Sie für die Interaktion mit Ihrem lokalen Git Repository installiert.

Sie können die Standardeinstellungen übernehmen; Wenn Sie die Befehle in der Windows-Befehlszeile verfügbar sein sollen, wählen Sie die Option, die es ermöglicht.

<p align="center">
 ![Beispiel für GitHub Profil](./media/tools-and-setup/gitbashinstall.png)

(Notiz: Dies ist nicht identisch mit "Github für Windows". "Github für Windows" ist ein anderes Benutzeroberflächen-basierten Tool, das auch verwendet werden kann, wenn Sie auf sich selbst bis zum lesen möchten. [https://windows.github.com/](https://windows.github.com/))

## <a name="enable-two-factor-authentication"></a>Aktivieren der Authentifizierung mit zwei Faktoren

Wenn Sie in das private Content Repository arbeiten, müssen Sie für Ihr Konto GitHub Authentifizierung mit zwei Faktoren (2FA) aktivieren. Es ist im privaten Repository erforderlich.

Folgen Sie den Anweisungen in beide der folgenden GitHub Hilfethemen, um dies zu aktivieren:

- [Info Authentifizierung mit zwei Faktoren](https://help.github.com/articles/about-two-factor-authentication/)
- [Erstellen einer Access-Token für die Befehlszeile Verwendung](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)

Wenn Sie das Token erstellen, wählen Sie alle Bereiche der Benutzeroberfläche Token-Erstellung ([Details auf jeden Bereich](https://developer.github.com/v3/oauth/#scopes))

Nachdem Sie 2FA aktiviert haben, müssen Sie das Access Token statt GitHub Kennwort an der Befehlszeile eingeben, bei dem Versuch, ein Repository GitHub über die Befehlszeile zugreifen. Das Access-Token ist nicht der Code für die Authentifizierung, den Sie in einer Textnachricht erhalten, wenn Sie 2FA einrichten. Es ist eine lange Zeichenfolge, die sieht ungefähr wie folgt aus: fdd3b7d3d4f0d2bb2cd3d58dba54bd6bafcd8dee. Einige Hinweise:

- Wenn Sie Ihre Access-Token erstellen, speichern Sie es in eine Textdatei zu machen Sie es einfach zugänglich bei Bedarf.

- Später, wenn Sie das Token einfügen müssen, wissen Sie, dass es gibt zwei Möglichkeiten, fügen Sie die Befehlszeile:

 - Klicken Sie auf das Symbol in der oberen linken Ecke des Fensters Befehlszeile > Bearbeiten > einfügen.
 - Mit der rechten Maustaste des Symbol in der oberen linken Ecke des Fensters, und klicken Sie auf Eigenschaften > Optionen > QuickEdit-Modus. Dadurch wird die Befehlszeile konfiguriert, damit Sie mit der rechten Maustaste in das Fenster Befehlszeile einfügen können.

## <a name="install-a-markdown-editor"></a>Installieren Sie einen Abzug-editor

Wir erstellen Inhalt, die mit der einfachen "Abzug" Notation lieber in die Dateien als komplexen "Markup" (HTML, XML usw.). Ja, müssen Sie einen Abzug-Editor installieren.

- **Atom**: Verwenden GitHubs Atom Abzug-Editor, die meisten von uns: [http://atom.io](http://atom.io). Es ist keine Lizenz für die Verwendung von Business erforderlich. Es hat die Rechtschreibprüfung.

- **Editor**: Sie können für eine sehr einfache Option Editor verwenden.

- **Sehen**: Dies ist eine einfache, elegante, Online, und öffnen Abzug Quelleditor, die eine Vorschau bietet. Besuchen Sie [http://prose.io](http://prose.io) und autorisieren Sie der Text in Ihrem Repository zu.

- **[Visual Studio-Code](https://www.visualstudio.com/products/code-vs.aspx)** - Microsoft Eintrag in diesem Bereich.

## <a name="configure-atom"></a>Atom konfigurieren

Wenn Sie Atom verwenden, müssen Sie ein paar Punkte einrichten.

- Atom Standardwerte zur Verwendung von 2 Leerzeichen für Tabstopps, aber Abzug erwartet 4 Leerzeichen. Wenn Sie den Standardwert von zwei lassen, wird der Artikel in lokale Vorschau beeindruckend aussehen aber nicht wann importiert in Azure. Ja, um 4 Leerzeichen verwenden Atom konfigurieren – diese Einstellung finden Sie unter Datei > Einstellungen > Editor-Einstellungen > Registerkarte Länge.
- Möglicherweise gibt es auch in diesem Abschnitt weiche umbrechen auch, aktivieren möchten die bedeutet "Zeilenumbruch" in Editor entspricht.
- Klicken Sie zum Aktivieren der Abzug Vorschau auf Pakete > Abzug Vorschau > Vorschau aktivieren/deaktivieren. STRG-UMSCHALT-M können Sie die Vorschau HTML-Ansicht umschalten.

## <a name="fork-the-repository-and-copy-it-to-your-computer"></a>Verzweigung Repository und kopieren Sie sie auf Ihren computer

1. Erstellen einer Verzweigung des Repositorys in GitHub – wechseln Sie zu der oberen rechten Ecke der Seite, und klicken Sie auf die Schaltfläche Verzweigung. Wenn Sie dazu aufgefordert werden, wählen Sie Ihr Konto als den Speicherort, in dem die Verzweigung erstellt werden soll. Dadurch wird eine Kopie des Repositorys in Ihr Konto Git Hub erstellt. Im Allgemeinen müssen technische Autoren und Programm-Manager Verzweigung Azure-Inhalt-Kurs, die als "Privat" Repo. Communitymitglieder müssen Azure-Inhalt, der öffentlichen Repo Verzweigung. Sie müssen nur einmal Verzweigung. nach Ihrer ersten Einrichtung Wenn Sie Ihre Verzweigung in einen anderen Computer kopieren möchten müssen Sie nur die Befehle ausführen, die in diesem Abschnitt, um die Repo auf Ihren Computer kopieren folgen.  Falls gewünscht Gabeln beide Repositorys zu erstellen, müssen Sie eine Verzweigung für jedes Repository zu erstellen.

2. Kopieren Sie die persönliche Access Token, die Sie von [https://github.com/settings/tokens](https://github.com/settings/tokens)erhalten haben. Sie können die Standardberechtigungen für das Token übernehmen.  Speichern Sie persönliche Access Token wird in eine Textdatei zur späteren Wiederverwendung.

3. Kopieren Sie als Nächstes Repository auf Ihren Computer mit Ihren Anmeldeinformationen, die in der Befehlszeichenfolge eingebettet.  Hierzu Git Bash öffnen und ihn als Administrator ausführen. Geben Sie an der Eingabeaufforderung den folgenden Befehl ein.  Dieser Befehl erstellt ein Verzeichnis azure-content(-pr) auf Ihrem Computer.  Wenn Sie den Standardspeicherort verwenden, werden sie bei c:\users<your Windows user name>\azure-content(-pr).

Öffentliche Repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content.git

Private Repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content-pr.git

Dieser Befehl datenbeschriftungsreihe könnte beispielsweise ungefähr wie folgt aussehen:

        git clone https://smithj:b428654321d613773d423ef2f173ddf4a312345@github.com/smithj/azure-content-pr.git  

## <a name="set-remote-repository-connection-and-configure-credentials"></a>Remote-Repository Verbindung einrichten und Konfigurieren von Anmeldeinformationen

Erstellen Sie einen Bezug auf die Stamm-Repository durch Eingabe der folgenden Befehle aus. Diese Standortadministrator legt fest Verbindungen mit dem Repository in GitHub, damit Sie die neuesten Änderungen auf dem lokalen Computer abrufen und schieben Sie Ihre Änderungen wieder zu GitHub können. Dieser Befehl konfiguriert auch Ihr Token lokal so, dass Sie Ihren Namen und Ihr Kennwort jedes Mal eingeben, Sie versuchen, den Zugriff auf die übergeordneten Repo und Ihre Verzweigung auf github, besitzen.

Öffentliche Repo:

        cd azure-content
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content.git
        git fetch upstream

Private Repo:

        cd azure-content-pr
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content-pr.git
        git fetch upstream

Dies dauert in der Regel eine Weile. Nachdem Sie dies getan haben, müssen Sie Verzweigen erneut, oder geben Sie Ihre Anmeldeinformationen erneut. Sie müssen nur die Zweige auf einen lokalen Computer erneut kopieren, wenn Sie die Tools auf einem anderen Computer einrichten.


## <a name="configure-your-user-name-and-email-locally"></a>Konfigurieren Sie Ihren Benutzernamen und e-Mail-lokal

Um sicherzustellen, dass Sie korrekt als Mitwirkender aufgelistet werden, müssen Sie Ihren Benutzernamen und e-Mail-lokal Git konfigurieren.

1. Starten Sie Git Bash, und wechseln Sie in Azure-Inhalte oder Azure-Inhalt-Kurs:

   ````
   cd azure-content
   ````

 oder

   ````
   cd azure-content-pr
   ````

2. Konfigurieren Sie Ihren Benutzernamen ein, damit sie Ihren Namen übereinstimmt, wie Sie es in Ihrem Profil GitHub einrichten:

    ````
    git config --global user.name "John Doe"
    ````
3. Konfigurieren Sie Ihre e-Mail-Adresse, damit sie die primäre e-Mail in Ihrem Profil GitHub festgelegte übereinstimmt; Wenn Sie einem Mitarbeiter MSFT befinden, sollten sie Ihre MSFT e-Mail-Adresse:

    ````
    git config --global user.email "alias@example.com"
    ````
4. Typ `git config -l` und überprüfen Sie Ihre lokalen Einstellungen, um den Benutzernamen zu gewährleisten und e-Mail in der Konfiguration korrekt sind.

##<a name="next-steps"></a>Nächste Schritte

- Verstehen Sie die Art der Inhalte, die in der technischen Inhalt Repo gehört und wissen Sie, was nicht gehört. Finden Sie unter den [Inhalt Channel Anleitungen](./content-channel-guidance.md)!
- Gehen Sie [folgendermaßen erstellen oder Ändern eines Artikels und senden Sie es für die Veröffentlichung](./git-commands-for-master.md)vor.
- Kopieren Sie [die Vorlage Abzug](../markdown templates/markdown-template-for-new-articles.md) als Basis für einen neuen Artikel aus.
- Verwenden Sie [Diese Checkliste zum Abruf Anforderung überprüfen wird die Qualität Kriterien](./contributor-guide-pr-criteria.md) für die Zusammenführung aus.


###<a name="contributors-guide-navigation"></a>Mitwirkenden Leitfaden navigation

- [Artikel (Übersicht)](./../README.md)
- [Index der Artikel mit Hinweisen](./contributor-guide-index.md)



<!--Anchors-->
[Use a customer-friendly voice]: #use-a-customer-friendly-voice
[Consider localization and machine translation]: #consider-localization-and-machine-translation
[other style and voice issues to watch for]: #other-style-and-voice-issues-to-watch-for


[Create a GitHub account and set up your profile]: #create-a-github-account-and-set-up-your-profile
[Determine whether you really need to follow the rest of these steps]: #determine-whether-you-really-need-to-follow-the-rest-of-these-steps
[Permissions in GitHub]: #permissions-in-github
[Install Git for Windows]: #install-git-for-windows
[Enable two-factor authentication]: #enable-two-factor-authentication
[Install a markdown editor]: #install-a-markdown-editor
[Fork the repository and copy it to your computer]: #fork-the-repository-and-copy-it-to-your-computer
[Install git-credential-winstore]: #install-git-credential-winstore
[Sign up for Disqus]: #sign-up-for-disqus
[Configure your user name and email locally]: #configure-your-user-name-and-email-locally
[Next steps]: #next-steps
