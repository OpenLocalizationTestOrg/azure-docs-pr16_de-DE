1. Im Visual Studio- **Lösung-Explorer**mit der rechten Maustaste in des Projekts, und wählen Sie **Hinzufügen > Docker Support** aus dem Kontextmenü.

    ![Hinzufügen von Docker Support-Kontextmenü](media/vs-azure-tools-docker-add-docker-support/docker-support-context-menu.png)

1. Hinzufügen von Docker Unterstützung zu einer ASP.NET Core führt Project Web das Hinzufügen von mehreren Docker-bezogene Dateien des Projekts, einschließlich Dateien Docker verfassen, Bereitstellung von Windows PowerShell-Skripts und Docker Eigenschaftendateien hinzugefügt werden. 

    ![Docker Dateien zum Projekt hinzugefügt](media/vs-azure-tools-docker-add-docker-support/docker-files-added.png)
    
> [AZURE.NOTE]Wenn die [Docker für Windows Beta](https://beta.docker.com)verwenden, öffnen Sie Properties\Docker.props, entfernen Sie den Standardwert, und starten Sie Visual Studio für den Wert wirksam werden.
> 
> ```
> <DockerMachineName Condition="'$(DockerMachineName)'=='' "></DockerMachineName>
> ```
