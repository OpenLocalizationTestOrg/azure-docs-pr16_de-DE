<properties
   pageTitle="Problembehandlung von Fehlern bei Docker Client unter Windows mit Visual Studio | Microsoft Azure"
   description="Behandeln von Problemen, die bei der Verwendung von Visual Studio zum Erstellen und Bereitstellen von Web apps für Docker unter Windows mit Visual Studio auftreten."
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="allclark" />

# <a name="troubleshooting-visual-studio-docker-development"></a>Problembehandlung bei der Entwicklung von Visual Studio Docker

Bei der Arbeit mit Visual Studio-Tools für Docker Vorschau können Sie einige Probleme aufgrund der Art der Vorschau auftreten.
Es folgen einige häufige Probleme und Lösungen.


## <a name="unable-to-validate-volume-mapping"></a>Überprüfen Sie die Lautstärke Zuordnung kann nicht
Volumen-Zuordnung ist erforderlich, den Quellcode und Binärdateien der Anwendung für die app-Ordner im Container freizugeben.  Bestimmte Lautstärke Zuordnungen sind in der Docker-compose.dev.debug.yml und Docker-compose.dev.release.yml Dateien enthalten. Als Dateien auf Ihrem Hostcomputer geändert werden, geben die Container diese Änderungen in eine ähnliche Ordnerstruktur wieder.

Zum Aktivieren der Lautstärke Zuordnung öffnen Sie **Einstellungen...** aus das Symbol "Moby" Docker für Windows, und wählen Sie dann auf die Registerkarte **Laufwerke freigegeben** .  Stellen Sie sicher, dass den Buchstaben des Laufwerks der hostet Projekt als auch den Buchstaben des Laufwerks %USERPROFILE%, wo befindet sich sind freigegeben, indem Sie diese markieren und dann auf **Übernehmen**.

Klicken Sie zum Testen Lautstärke Zuordnung funktionsfähig ist, nachdem der Laufwerke freigegeben wurden, auffordern, entweder neu erstellen und F5 aus in Visual Studio oder versuchen Sie die folgenden Befehl:

*In einer Windows-Befehlszeile*

*[Hinweis: Dies wird davon ausgegangen, Ihre Benutzer-Ordner befindet sich auf Laufwerk "C" und es freigegeben wurde.  Wenn Sie ein anderes Laufwerk freigegeben haben, aktualisieren Sie je nach Bedarf]*
```
docker run -it -v /c/Users/Public:/wormhole busybox
```

*Im Container Linux*

```
/ # ls
```

Es sollte ein Verzeichnis auflisten über die Benutzer/Öffentliche Ordner angezeigt.
Wenn keine Dateien angezeigt werden, und Ihre /c/Users/Public Ordner nicht leer ist, ist die Lautstärke Zuordnung nicht ordnungsgemäß konfiguriert. 

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

In Wurmloch Verzeichnis auf den Inhalt finden Sie unter Ändern der `/c/Users/Public` Verzeichnis:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

**Hinweis:** *Bei der Arbeit mit Linux virtuellen Computern ist das Dateisystem Container Groß-/Kleinschreibung beachtet.*

##<a name="build--prepareforbuild-task-failed-unexpectedly"></a>Erstellen: Vorgang "PrepareForBuild" Unerwarteter Fehler bei.

Microsoft.DotNet.Docker.CommandLine.ClientException: Herstellen einer Verbindung ist ein Fehler aufgetreten:

Stellen Sie sicher, dass der standardmäßigen Docker Host ausgeführt wird. Öffnen Sie ein Eingabeaufforderungsfenster, und führen Sie:

```
docker info
```

Ist dies gibt ein Fehler dann versuchen, die **Docker für Windows** -desktop-Anwendung zu starten.  Wenn die desktop-Anwendung dann **Moby** über die Taskleiste das Symbol ausgeführt wird sollte sichtbar sein. Klicken Sie mit der rechten Maustaste auf das Symbol in der Taskleiste, und öffnen Sie die **Einstellungen**.  Klicken Sie auf der Registerkarte **Zurücksetzen** und **Restart Docker...**.

##<a name="manually-upgrading-from-version-031-to-040"></a>Manuelles Aktualisieren von Version 0.31 zu 0.40


1. Sichern des Projekts
1. Löschen Sie die folgenden Dateien im Projekt:

    ```
      Dockerfile
      Dockerfile.debug
      DockerTask.ps1
      docker-compose-yml
      docker-compose.debug.yml
      .dockerignore
      Properties\Docker.props
      Properties\Docker.targets
    ```

1. Schließen Sie die Lösung, und entfernen Sie die folgenden Zeilen aus der Datei .xproj:

    ```
      <DockerToolsMinVersion>0.xx</DockerToolsMinVersion>
      <Import Project="Properties\Docker.props" />
      <Import Project="Properties\Docker.targets" />
    ```

1. Öffnen Sie die Lösung erneut.
1. Entfernen Sie die folgenden Zeilen aus der Properties\launchSettings.json-Datei ein:

    ```
      "Docker": {
        "executablePath": "%WINDIR%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
        "commandLineArgs": "-ExecutionPolicy RemoteSigned .\\DockerTask.ps1 -Run -Environment $(Configuration) -Machine '$(DockerMachineName)'"
      }
    ```

1. Entfernen Sie die folgenden Dateien, die im Zusammenhang mit Docker von project.json in der PublishOptions:

    ```
    "publishOptions": {
      "include": [
        ...
        "docker-compose.yml",
        "docker-compose.debug.yml",
        "Dockerfile.debug",
        "Dockerfile",
        ".dockerignore"
      ]
    },
    ```

1. Deinstallieren Sie die vorherige Version und installieren Sie **Docker Support-> Hinzufügen** und dann auf Docker Tools 0.40 erneut aus dem Kontextmenü für Ihre ASP.Net Core Web oder Console-Anwendung. Dadurch wird die neue erforderlichen Docker-Elemente wieder zum Projekt hinzugefügt. 

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Ein Fehlerdialogfeld tritt auf, wenn **Docker Support-> Hinzufügen** wollen, oder Debuggen (F5) Anwendung Core ASP.NET in einem container

Wir haben gelegentlich gesehen nach der Deinstallation von, und installieren Extensions, Visual Studio MEF (Managed Extensibility Framework) Cache kann beschädigt werden. Wenn in diesem Fall, dass es verschiedene Fehler verursachen kann beim Hinzufügen von Docker Support-Dialogfenster und/oder versuchen, auszuführen oder zu debuggen (F5) Ihrer ASP.NET zentralen Anwendung. Als vorübergehende Lösung führen Sie die folgenden Schritte aus, um zu löschen und den MEF-Cache-Erneuerung.

1. Schließen Sie alle Instanzen von Visual Studio
1. %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\ öffnen
1. Löschen Sie die folgenden Ordner
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Öffnen von Visual Studio
1. Versuchen Sie erneut das Szenario 
