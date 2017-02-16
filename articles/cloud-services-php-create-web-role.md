<properties
    pageTitle="Web und Worker Rollen von PHP | Microsoft Azure"
    description="Ein Leitfaden zum Erstellen von Web- und Worker Rollen von PHP in einer Azure-Cloud-Dienst und Konfigurieren von PHP-Laufzeit."
    services=""
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

#<a name="how-to-create-php-web-and-worker-roles"></a>Zum Erstellen von PHP Web- und Worker Rollen

## <a name="overview"></a>(Übersicht)

In diesem Handbuch wird gezeigt, wie Sie auf Web oder Arbeitskollegen Rollen von PHP in einer Windows-Umgebung Development erstellen, wählen Sie eine bestimmte Version von PHP aus der "integrierten" Versionen zur Verfügung, ändern Sie die Konfiguration von PHP, Erweiterungen aktivieren und schließlich auf Azure bereitstellen. Es werden auch so konfigurieren Sie eine Rolle Web oder Worker, um eine PHP-Laufzeit (mit benutzerdefinierten Konfiguration und Extensions) verwenden, die Sie bereitstellen.

## <a name="what-are-php-web-and-worker-roles"></a>Was sind Web und Worker Rollen von PHP?

Azure bietet drei Modelle für die Ausführung von Applications zu berechnen: Azure-App-Verwaltungsdienst, Azure-virtuellen Computern und Azure-Cloud-Dienste. Alle drei Modelle unterstützen PHP. Cloud Services, wozu Web- und Worker Rollen bietet *Plattform als Service (PaaS)*. In einen Clouddienst bietet eine Webrolle einen dedizierten Host Front-End-Webanwendungen Webserver (Internetinformationsdienste). Eine Worker-Rolle kann unabhängig von der Interaktion mit dem Benutzer oder Eingabe asynchrone langer oder ständige Aufgaben ausführen.

Weitere Informationen zu diesen Optionen finden Sie unter [Berechnen von Azure bereitgestellte Hostinganbieter Optionen](./cloud-services/cloud-services-choose-me.md).

## <a name="download-the-azure-sdk-for-php"></a>Laden Sie das Azure SDK für PHP

Das [Azure SDK für PHP] besteht aus mehreren Komponenten. In diesem Artikel werden zwei davon verwenden: Azure PowerShell und Azure-Emulatoren. Diese beiden Komponenten können über die Microsoft Web Platform Installer installiert werden. Weitere Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](powershell-install-configure.md).

## <a name="create-a-cloud-services-project"></a>Cloud Services-Projekt erstellen

Der erste Schritt beim Erstellen einer Web oder Arbeitskollegen Rolle von PHP ist ein Azure Service-Projekt erstellen. ein Azure Service-Projekt fungiert als logischer Container für Web- und Worker Rollen und die [Definition des Diensts (.csdef)] und [Dienstkonfiguration (.cscfg)] Projektdateien enthält.

Zum Erstellen eines neuen Projekts von Azure Service führen Sie Azure PowerShell als Administrator aus, und führen Sie den folgenden Befehl aus:

    PS C:\>New-AzureServiceProject myProject

Dieser Befehl erstellt ein neues Verzeichnis (`myProject`), dem Sie Web- und Worker Rollen hinzufügen können.

## <a name="add-php-web-or-worker-roles"></a>Hinzufügen von PHP Web oder Arbeitskollegen Rollen

Wenn eine Web-Rolle von PHP zu einem Projekt hinzufügen möchten, führen Sie den folgenden Befehl aus im Stammverzeichnis des Projekts ein:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Verwenden Sie für eine Worker-Rolle diesen Befehl aus:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [AZURE.NOTE] Die `roleName` Parameter ist optional. Wenn sie ausgelassen wird, wird der Rollenname automatisch generiert werden. Die erste Webrolle erstellt werden `WebRole1`, werden die zweite `WebRole2`und so weiter. Die erste Worker-Rolle erstellt werden `WorkerRole1`, werden die zweite `WorkerRole2`usw..

## <a name="specify-the-built-in-php-version"></a>Geben Sie die integrierte Version von PHP

Wenn Sie eine Web oder Arbeitskollegen Rolle von PHP zu einem Projekt hinzufügen, werden die Projektdateien Konfiguration geändert, sodass PHP auf jede Instanz Web oder Arbeitskollegen Ihrer Anwendung installiert sein wird, wenn es bereitgestellt wird. Um die Version von PHP anzuzeigen, die standardmäßig installiert werden, führen Sie den folgenden Befehl aus:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

Die Ausgabe der obigen Befehls sieht etwa wie nachfolgend gezeigt. In diesem Beispiel die `IsDefault` auf die Kennzeichnung festgelegt ist `true` für PHP 5.3.17, gibt an, dass er angezeigt wird, die standardmäßige PHP-Version installiert ist.

    Runtime Version     PackageUri                      IsDefault
    ------- -------     ----------                      ---------
    Node 0.6.17         http://nodertncu.blob.core...   False
    Node 0.6.20         http://nodertncu.blob.core...   True
    Node 0.8.4          http://nodertncu.blob.core...   False
    IISNode 0.1.21      http://nodertncu.blob.core...   True
    Cache 1.8.0         http://nodertncu.blob.core...   True
    PHP 5.3.17          http://nodertncu.blob.core...   True
    PHP 5.4.0           http://nodertncu.blob.core...   False

Sie können die Version von PHP Runtime an die Versionen von PHP festlegen, die aufgelistet werden. Um beispielsweise die Version von PHP festlegen (für eine Rolle mit dem Namen `roleName`) 5.4.0, verwenden Sie den folgenden Befehl aus:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [AZURE.NOTE] Verfügbare Versionen von PHP möglicherweise in der Zukunft ändern.

## <a name="customize-the-built-in-php-runtime"></a>Passen Sie die integrierte Laufzeit von PHP

Sie haben die vollständige Kontrolle über die Konfiguration von PHP-Laufzeit, die installiert ist, wenn Sie die Schritte oben, einschließlich der Änderung der `php.ini` Einstellungen und Aktivierung von Erweiterungen.

Um die integrierten Laufzeit von PHP anpassen möchten, gehen Sie folgendermaßen vor:

1. Hinzufügen eines neues Ordners, mit dem Namen `php`, an der `bin` Verzeichnis von Ihrer Webrolle. Fügen sie für eine Worker-Rolle die Rolle des Stammverzeichnisses hinzu.
2. In der `php` Ordner, erstellen Sie einen anderen Ordner namens `ext`. Setzen eine `.dll` Erweiterungsdateien (z. B. `php_mongo.dll`), die Sie in diesem Ordner aktivieren möchten.
3. Hinzufügen eines `php.ini` legen Sie die `php` Ordner. Aktivieren Sie alle benutzerdefinierten Erweiterungen, und legen Sie alle Richtlinien von PHP in dieser Datei. Angenommen, Sie aktivieren möchten `display_errors` auf und Aktivieren der `php_mongo.dll` Erweiterung, die den Inhalt Ihrer `php.ini` lauten wie folgt:

        display_errors=On
        extension=php_mongo.dll

> [AZURE.NOTE] Änderungen an den Einstellungen, die Sie explizit ist, nicht in festgelegt der `php.ini` Datei, die Sie bereitstellen wird automatisch auf ihre Standardwerte festgelegt werden. Weiter, beachten Sie, dass Sie eine vollständige hinzufügen können `php.ini` Datei.

## <a name="use-your-own-php-runtime"></a>Verwenden Sie Ihrer eigenen Laufzeit von PHP
In einigen Fällen statt eine integrierte Laufzeit von PHP auswählen und konfigurieren ihn wie oben beschrieben, können Sie eigene Runtime von PHP zur Verfügung stellen möchten. Beispielsweise können Sie die gleiche Laufzeit von PHP in einer Rolle Web oder Arbeitskollegen verwenden, die Sie in Ihrer Entwicklungsumgebung verwenden. Dies vereinfacht stellen Sie sicher, dass die Anwendung in Ihrem Unternehmen Verhalten nicht geändert werden.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Konfigurieren einer Webrolle, um eigene Laufzeit von PHP zu verwenden.

Zum Konfigurieren einer Webrolle, um eine Laufzeit von PHP verwenden, die Sie bereitstellen, gehen Sie folgendermaßen vor:

1. Erstellen Sie ein Azure Service-Projekt, und fügen Sie einer Web-Rolle von PHP aus, wie zuvor in diesem Artikel beschrieben.
2. Erstellen einer `php` Ordner in den `bin` Ordner, die im Stammverzeichnis der Web-Rolle ist, und fügen Sie anschließend Ihre PHP-Laufzeit (alle Binärdateien, Konfigurationsdateien, Unterordner usw.) die `php` Ordner.
3. (OPTIONAL) Wenn Ihre Runtime von PHP die [Microsoft-Treiber für PHP für SQL Server]verwendet[sqlsrv drivers], müssen Sie so konfigurieren Sie Ihre Webrolle zum Installieren von [SQL Server Native Client 2012] [ sql native client] Wenn es zulässig ist. Fügen Sie hierzu den [sqlncli.msi X64 Installer] an, die `bin` Ordner im Stammverzeichnis der Web-Rolle. Das Skript zum Starten des beschrieben, die im nächsten Schritt wird das Installationsprogramm im Hintergrund ausgeführt, wenn die Rolle bereitgestellt wird. Wenn Ihre Runtime von PHP kein Microsoft-Drivers für PHP für SQL Server verwendet, können Sie die folgende Zeile aus dem Skript angezeigt, die im nächsten Schritt entfernen:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Definieren eine Startaufgabe, die [Internet Information Services (IIS)] konfiguriert[ iis.net] Ihre Runtime von PHP zu verwenden, zur Behandlung von Besprechungsanfragen für `.php` Seiten. Öffnen Sie hierzu die `setup_web.cmd` Datei (in der `bin` Datei des Stammverzeichnisses Ihrer Webserverrolle) in einem Text-Editor und Ersetzen Sie den Inhalt mithilfe des folgenden Skripts:

        @ECHO ON
        cd "%~dp0"

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
        SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"

5. Ihre Webserverrolle Stammverzeichnis Ihrer Anwendungsdateien hinzufügen. Dies ist der Webserver Stammverzeichnis wird.

6. Veröffentlichen Ihrer Anwendung wie im Abschnitt [Veröffentlichen Ihrer Anwendung](#how-to-publish-your-application) beschrieben.

> [AZURE.NOTE] Die `download.ps1` Skript (in der `bin` Ordner des Stammverzeichnisses die Web-Rolle) gelöscht werden können, haben die für die Verwendung Ihrer eigenen Laufzeit von PHP oben beschriebenen Schritte ausgeführt.

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>Konfigurieren einer Rolle Worker, um eigene Laufzeit von PHP verwenden

Gehen Sie folgendermaßen vor, um eine Worker-Rolle aus, um eine Laufzeit von PHP verwenden, die Sie bereitstellen zu konfigurieren:

1. Erstellen Sie ein Azure Service-Projekt, und fügen Sie eine Worker-Rolle von PHP aus, wie zuvor in diesem Artikel beschrieben.
2. Erstellen einer `php` Ordner im Stammverzeichnis der Worker-Rolle, und fügen Sie anschließend Ihre PHP-Laufzeit (alle Binärdateien, Konfigurationsdateien, Unterordner usw.) die `php` Ordner.
3. (OPTIONAL) Wenn Ihre Runtime von PHP [Microsoft-Treiber für PHP für SQL Server]verwendet[sqlsrv drivers], müssen Sie so konfigurieren Sie Ihre Arbeitskollegen Rolle zum Installieren von [SQL Server Native Client 2012] [ sql native client] Wenn es zulässig ist. Hierfür die [sqlncli.msi X64 Installer] Worker-Rolle Stammverzeichnis hinzugefügt. Das Skript zum Starten des beschrieben, die im nächsten Schritt wird das Installationsprogramm im Hintergrund ausgeführt, wenn die Rolle bereitgestellt wird. Wenn Ihre Runtime von PHP kein Microsoft-Drivers für PHP für SQL Server verwendet, können Sie die folgende Zeile aus dem Skript angezeigt, die im nächsten Schritt entfernen:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Definieren eine Startaufgabe, die addiert Ihrer `php.exe` ausführbare Worker-Rolle Pfad Umgebung Variablen, wenn die Rolle bereitgestellt wird. Öffnen Sie hierzu die `setup_worker.cmd` Datei (im Stammverzeichnis der Worker-Rolle) in einem Text-Editor, und Ersetzen Sie den Inhalt mithilfe des folgenden Skripts:

        @echo on

        cd "%~dp0"

        echo Granting permissions for Network Service to the web root directory...
        icacls ..\ /grant "Network Service":(OI)(CI)W
        if %ERRORLEVEL% neq 0 goto error
        echo OK

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        setx Path "%PATH%;%~dp0php" /M

        if %ERRORLEVEL% neq 0 goto error

        echo SUCCESS
        exit /b 0

        :error

        echo FAILED
        exit /b -1

5. Der Worker-Rolle Stammverzeichnis Ihrer Anwendungsdateien hinzufügen.

6. Veröffentlichen Ihrer Anwendung wie im Abschnitt [Veröffentlichen Ihrer Anwendung](#how-to-publish-your-application) beschrieben.

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>Führen Sie die Anwendung in den Emulatoren Datenverarbeitung und Speicher

Die Azure Emulatoren bereitstellen eine lokale Umgebung, in der Sie vor der Bereitstellung in der Cloud Azure-Anwendung testen können. Es gibt einige Unterschiede zwischen der Emulatoren und Azure-Umgebung aus. Zum besseren Verständnis finden Sie unter [Verwenden der Azure Speicheremulator zum Entwickeln und Testen](./storage/storage-use-emulator.md).

Beachten Sie, dass Sie PHP installiert lokal an, um die Serveremulator verwenden müssen. Die Serveremulator wird Ihrer lokale Installation von PHP verwendet, Sie die Anwendung ausführen zu können.

Um Ihr Projekt in den Emulatoren ausführen zu können, führen Sie den folgenden Befehl aus Stammverzeichnis Ihres Projekts aus:

    PS C:\MyProject> Start-AzureEmulator

Die Ausgabe ähnlich wie folgt werden angezeigt:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

Sie können Ihrer Anwendung im Emulator ausgeführt wird, indem Sie einen Webbrowser öffnen und an die lokale Adresse in die Ausgabe der anzeigen (`http://127.0.0.1:81` im obigen Beispiel).

Um die Emulatoren beenden möchten, führen Sie diesen Befehl aus:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Veröffentlichen Sie die Anwendung

Wenn Sie die Anwendung veröffentlichen, müssen Sie zuerst Importieren Ihrer veröffentlichungseinstellungen mithilfe des [Import-AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) -Cmdlets. Dann können Sie die Anwendung veröffentlichen, mithilfe des [Veröffentlichen-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) Cmdlets. Informationen zum Anmelden finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](powershell-install-configure.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie im [Developer Center von PHP](/develop/php/).

[Azure SDK für PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[Dienstdefinition (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[Dienstkonfiguration (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[SQLNCLI.msi X64 installer]: http://go.microsoft.com/fwlink/?LinkID=239648
