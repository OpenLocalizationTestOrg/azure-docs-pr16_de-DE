<properties
   pageTitle="Installieren von .NET auf einer Rolle der Cloud-Dienst | Microsoft Azure"
   description="Dieser Artikel beschreibt, wie .NET Framework in die Cloud-Dienst Web und Worker-Rollen manuell installieren"
   services="cloud-services"
   documentationCenter=".net"
   authors="thraka"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/10/2016"
   ms.author="adegeo"/>

# <a name="install-net-on-a-cloud-service-role"></a>Installieren von .NET auf einer Rolle der Cloud-Dienst 

Dieser Artikel beschreibt, wie Sie .NET Framework auf Cloud-Dienst Web und Worker-Rollen installieren. Diese Schritte können Sie .NET 4.6.1 unter Azure Gast OS Familie 4 installieren. Die neuesten Informationen zu Gast-BS-Versionen finden Sie unter [SDK Kompatibilitätsmatrix und Azure Gast OS frei](cloud-services-guestos-update-matrix.md).

Beim Installieren von .NET auf Ihre Websites und Worker Rollen umfasst, einschließlich der .NET Installer-Paket als Teil des Projekts Cloud und starten das Installationsprogramm als Teil der Rolle des Start-Aufgaben.  

## <a name="add-the-net-installer-to-your-project"></a>Das Installationsprogramm .NET zum Projekt hinzufügen
- Herunterladen der Web Installer für .NET Framework, die Sie installieren möchten
    - [.NET 4.6.1 web Installer](http://go.microsoft.com/fwlink/?LinkId=671729)
- Für eine Webrolle
  1. Im **Explorer Lösung**, klicken Sie unter In **Rollen** in der Cloud Service Project nach rechts auf auf Ihre Rolle und wählen Sie **Hinzufügen > Neuer Ordner**. Erstellen Sie einen Ordner mit dem Namen *bin*
  2. Klicken Sie mit der rechten Maustaste auf den Ordner **Bin** , und wählen Sie **Hinzufügen > vorhandene Element**. Wählen Sie das Installationsprogramm .NET aus, und fügen Sie es in den Ordner Bin.
- Für eine Worker-Rolle
  1. Klicken Sie mit der rechten Maustaste auf Ihre Rolle und wählen Sie **Hinzufügen > vorhandene Element**. Wählen Sie das Installationsprogramm .NET aus, und fügen sie die Rolle hinzu. 

Dateien hinzugefügt haben, auf diese Weise zum Ordner Inhalt Rolle automatisch das Cloud-Service-Paket hinzugefügt und an eine konsistente Position des virtuellen Computers bereitgestellt werden. Wiederholen Sie diesen Vorgang für alle Websites und Worker Rollen in der Cloud-Dienst, sodass alle Rollen eine Kopie des Installationsprogramms verfügen.

> [AZURE.NOTE] Sie sollten .NET 4.6.1 auf Ihre Rolle der Cloud-Dienst installieren, auch wenn die Anwendung .NET 4.6 vorgesehen ist. Die Azure Gast-BS umfasst Updates [3098779](https://support.microsoft.com/kb/3098779) und [3097997](https://support.microsoft.com/kb/3097997). .NET 4.6 auf diese Updates installieren möglicherweise Probleme verursachen, wenn Sie Ihren, ausgeführt, sodass Sie direkt .NET 4.6.1 anstelle von .NET 4.6 installieren sollten. Weitere Informationen finden Sie unter [KB 3118750](https://support.microsoft.com/kb/3118750).

![Inhalt der Rolle mit Installer-Dateien][1]

## <a name="define-startup-tasks-for-your-roles"></a>Startaufgaben für Ihre Rollen definieren
Startaufgaben können Sie Vorgänge ausführen, bevor eine Rolle gestartet wird. Installieren von .NET Framework als Teil des Vorgangs Start wird sichergestellt, dass das Framework installiert ist, bevor Sie ein Teil des Anwendungscodes wird ausgeführt. Weitere Informationen beim Start Aufgaben finden: [Start-Aufgaben in Azure ausführen](cloud-services-startup-tasks.md). 

1. Fügen Sie zu der Datei *ServiceDefinition.csdef* unter dem **WebRole** oder **WorkerRole** Knoten für alle Rollen Folgendes ein:
    
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```

    Die oben genannten Konfiguration wird Console Befehl *install.cmd* mit Administratorrechten ausgeführt, damit .NET Framework installiert werden kann. Die Konfiguration wird auch eine LocalStorage mit dem Namen *NETFXInstall*erstellt. Das Skript zum Starten des legen Sie des temporären Ordners, um diese Speicherressource lokalen verwenden, damit das .NET Framework-Installationsprogramm wird heruntergeladen und werden von dieser Ressource installiert. Es ist wichtig, legen Sie die Größe der Ressource auf mindestens 1024MB, um sicherzustellen, dass das Framework ordnungsgemäß installiert wird. Weitere Informationen zum Starten Aufgaben finden Sie unter: [Allgemeine Cloud-Dienst Startaufgaben](cloud-services-startup-tasks-common.md) 

2. Erstellen Sie eine Datei **install.cmd** Lizenz zu aller Rollen von der rechten Maustaste auf die Rolle und Auswählen von **Hinzufügen > Vorhandenes Element...**. Daher müssen alle Rollen sollte jetzt die .NET Installer-Datei als auch die Datei "install.cmd" verfügen.
    
    ![Rolle Inhalt mit allen Dateien][2]

    > [AZURE.NOTE] Verwenden Sie einen einfachen Text-Editor wie Editor, um diese Datei zu erstellen. Wenn Sie Visual Studio verwenden, um eine Textdatei erstellen und benennen Sie sie dann auf '.cmd' die Datei enthält möglicherweise noch immer eine UTF-8-Byte-Order Mark und führt zu einem Fehler Fehlern die erste Zeile des Skripts ausgeführt. Wenn Sie mit Visual Studio erstellt wurden die Datei verlassen Hinzufügen einer REM (Kommentar) zur ersten Zeile der Datei, damit er beim Ausführen der ignoriert wird. 

3. Fügen Sie das folgende Skript zu der Datei **install.cmd** aus:

    ```
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    set netfx="NDP461"
    
    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."
    
    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit
    
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
    
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
    
    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp
    
    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp
    
    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"
    
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%
    
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed
    
    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%   
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
    
    :restart
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%
    
    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%
    
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
    
    :exit
    EXIT /B 0
    ```
        
    Skript für die Installation überprüft, ob die angegebene .NET Framework-Version bereits auf dem Computer installiert ist, durch Abfragen der Registrierungs. Wenn die Version .NET nicht installiert ist, und klicken Sie dann die .net Web Installer wird gestartet. Um für Probleme beheben wird das Skript alle Aktivitäten in einer Datei namens *Startuptasklog-(Currentdatetime) txt* *InstallLogs* lokalen Speicher gehörende Kehrmatrix protokolliert.

    > [AZURE.NOTE] Das Skript enthält immer noch so .NET 4.5.2 oder .NET 4.6 Kontinuitätsgründen zu installieren. Es ist nicht erforderlich, .NET 4.5.2 manuell zu installieren, wie es bereits auf Azure Gast OS ist ein. Anstelle von .NET 4.6 installieren, sollten Sie direkt .NET 4.6.1 aufgrund von [KB 3118750](https://support.microsoft.com/kb/3118750)installieren.
      

## <a name="configure-diagnostics-to-transfer-the-startup-task-logs-to-blob-storage"></a>Konfigurieren Sie die Diagnose zum Übertragen der Start-Task-Protokolle BLOB-Speicher 
Um eine Problembehandlung bei der Installation zu vereinfachen, können Sie Azure-Diagnose, um alle Protokolldateien, die auf die Start- oder das Installationsprogramm .NET Blob-Speicher übertragen konfigurieren. Bei diesem Ansatz können Sie die Protokolle anzeigen, indem Sie einfach die Protokolldateien von Blob-Speicher herunterladen anstatt remote Desktop in die Rolle.

So konfigurieren die Diagnose Öffnen der *diagnostics.wadcfgx* aus, und fügen Sie den folgenden unter den Knoten **Verzeichnisse** : 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Dadurch wird die Azure Diagnose zum übertragen alle Dateien im Verzeichnis *Log* unter der Ressource *NETFXInstall* mit dem Konto des Diagnose-Speicher im Container Blob *Netfx-Installation* konfiguriert.

## <a name="deploying-your-service"></a>Bereitstellen von Ihrem Dienst 
Wenn Sie den Dienst bereitstellen werden die Startaufgaben ausführen und .NET Framework installieren, wenn er noch nicht installiert ist. Ihre Rollen werden im beschäftigt Zustand, während das Framework jetzt installiert wird und möglicherweise auch neu gestartet, wenn die Installation von Framework dies erforderlich macht. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Installieren von .NET Framework][]
- [So: feststellen, welche .NET Framework-Versionen installiert sind][]
- [Problembehandlung bei .NET Framework-Installationen][]

[So: feststellen, welche .NET Framework-Versionen installiert sind]: https://msdn.microsoft.com/library/hh925568.aspx
[Installieren von .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Problembehandlung bei .NET Framework-Installationen]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png

 
