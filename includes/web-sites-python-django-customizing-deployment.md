Azure wird Stellen Sie fest, dass die Anwendung mithilfe von Python **, wenn beide der folgenden Bedingungen erfüllt sind**:

- Requirements.txt-Datei im Stammordner
- Alle .py-Datei im Stammordner oder eine runtime.txt, die angibt, Python

Wenn dies der Fall ist, wird sie ein Python Bereitstellungsskript verwenden, das die standard Synchronisation von Dateien sowie zusätzliche Python Vorgänge wie ausführt:

- Automatische Verwaltung von virtuellen Umgebung
- Installation von in requirements.txt mit Pip aufgelisteten Pakete
- Die Erstellung der entsprechenden web.config basierend auf der ausgewählten Python-Version.
- Statische Dateien für Applikationen Django sammeln

Sie können bestimmte Aspekte der Standard-Bereitstellungsschritte steuern, ohne das Skript anpassen.

Wenn Sie alle Python bestimmte Bereitstellungsschritte überspringen möchten, können Sie diese leere Datei erstellen:

    \.skipPythonDeployment

Wenn Sie eine Sammlung von statischen Dateien für eine Anwendung Django überspringen möchten:

    \.skipDjango 

Für mehr Kontrolle über die Bereitstellung können Sie das standardmäßige Bereitstellungsskript überschreiben, indem Sie die folgenden Dateien erstellen:

    \.deployment
    \deploy.cmd

Die [Azure Line Benutzeroberfläche][] können Sie um die Dateien zu erstellen.  Verwenden Sie diesen Befehl aus Ihrem Projektordner an:

    azure site deploymentscript --python

Wenn diese Dateien nicht vorhanden ist, wird Azure temporäre Bereitstellungsskript erstellt und ausgeführt.  Es ist das Element, das Sie mit den oben angegebenen Befehl erstellen.

[Azure Line-Benutzeroberfläche]: http://azure.microsoft.com/downloads/
