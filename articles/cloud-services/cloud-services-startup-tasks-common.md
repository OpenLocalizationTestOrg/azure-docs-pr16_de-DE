<properties 
pageTitle="Allgemeine Startaufgaben in Cloud Services | Microsoft Azure" 
description="Enthält einige Beispiele für allgemeine Startaufgaben, die Sie in der Cloud Services-Web-Rolle oder Worker-Rolle ausführen möchten." 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="10/17/2016" 
ms.author="adegeo"/>

# <a name="common-cloud-service-startup-tasks"></a>Allgemeine Aufgaben der Cloud-Dienst starten

Dieser Artikel enthält einige Beispiele für allgemeine Startaufgaben, die Sie in der Cloud-Dienst ausführen möchten. Startaufgaben können Sie Vorgänge ausführen, bevor eine Rolle gestartet wird. Vorgänge, die Sie möglicherweise ausführen möchten einschließen eine Komponente installieren, COM-Komponenten registrieren, Registrierungsschlüsseln festlegen oder Starten eines Prozesses zeitintensiven. 

Finden Sie [in diesem Artikel](cloud-services-startup-tasks.md) zu verstehen, wie Vorgänge beim Start funktionieren und speziell so erstellen Sie die Einträge, die einen Vorgang Start definieren.

>[AZURE.NOTE] Startaufgaben sind nicht anwendbar auf virtuellen Computern, die nur für Cloud-Dienst Web und Worker-Rollen.


## <a name="define-environment-variables-before-a-role-starts"></a>Definieren der Umgebungsvariablen vor Beginn eine Rolle

Wenn Sie Umgebungsvariablen definiert für einen bestimmten Vorgang benötigen, verwenden Sie das Element [-Umgebung, die] innerhalb des [Task] -Elements.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
                <Environment>
                    <Variable name="MyEnvironmentVariable" value="MyVariableValue" />
                </Environment>
            </Task>
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

Variablen können auch einen [gültigen Azure XPath-Wert](cloud-services-role-config-xpath.md) in Bezug auf einen Beitrag zur Bereitstellung. Anstelle von der `value` Attribut, [RoleInstanceValue] untergeordnetes Element definieren.

```xml
<Variable name="PathToStartupStorage">
    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
</Variable>
```


## <a name="configure-iis-startup-with-appcmdexe"></a>Konfigurieren von IIS-Start mit AppCmd.exe

Das Befehlszeile [AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) -Tool kann zum Verwalten von IIS-Einstellungen beim Start auf Azure verwendet werden. *AppCmd.exe* bietet komfortablen Befehlszeile Zugriff zu Konfiguration von Einstellungen zur Verwendung in "Startaufgaben" auf Azure. Verwendung von *AppCmd.exe*, können Einstellungen der Website werden hinzugefügt, geändert oder entfernt für Applikationen und Websites.

Es gibt jedoch einige Punkte, die bei der Verwendung von *AppCmd.exe* als beim Start Aufgaben achten sollten:

- Startaufgaben können mehr als einmal zwischen nach einem Neustart ausgeführt werden. Beispielsweise eine Rolle beim-Freigabe.
- Wenn eine Aktion *AppCmd.exe* nur einmal durchgeführt werden, möglicherweise einen Fehler verursacht. Bei dem Versuch, einen Abschnitt *Web.config* zweimal hinzufügen konnte beispielsweise einen Fehler generiert werden.
- Startaufgaben fehl, wenn sie eine nicht-NULL Beendigungscode oder **Errorlevel**zurückgeben. Beispielsweise, wann *AppCmd.exe* einen Fehler generiert.

Es empfiehlt sich zu überprüfen, ob die **Errorlevel** nach Aufrufen *AppCmd.exe*, welche ist einfach, wenn Sie den Anruf an *AppCmd.exe* mit einer Datei *.cmd* umbrechen. Wenn Sie eine Antwort bekannte **Errorlevel** erkennen, können Sie ignoriert oder wieder zu übergeben.

Zurückgegebene *AppCmd.exe* Errorlevel in der Datei winerror.h aufgelistet sind, und auch auf der [MSDN-](https://msdn.microsoft.com/library/windows/desktop/ms681382.aspx)angezeigt werden.

### <a name="example-of-managing-the-error-level"></a>Beispiele für die Verwaltung der Ebene zurück

In diesem Beispiel addiert einen Abschnitt Komprimierung und einen Eintrag Komprimierung für JSON zu der Datei *Web.config* mit Fehlerbehandlung und Protokollierung.

Die entsprechenden Abschnitten der Datei [ServiceDefinition.csdef] sind hier gezeigt, die das Attribut [ExecutionContext](https://msdn.microsoft.com/library/azure/gg557552.aspx#Task) Einstellung enthalten `elevated` *AppCmd.exe* über die erforderlichen Berechtigungen zum Ändern der Einstellungen in der Datei *Web.config* zu gewähren:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

Die Batchdatei *Startup.cmd* verwendet *AppCmd.exe* eines Abschnitts Komprimierung und einen Eintrag Komprimierung für JSON zur Datei *Web.config* hinzufügen. Erwarteten **Errorlevel** 183 wird mithilfe der Überprüfen auf NULL festgelegt. Befehlszeile EXE-Programm. Unerwartete Errorlevels werden in StartupErrorLog.txt protokolliert.

    REM   *** Add a compression section to the Web.config file. ***
    %windir%\system32\inetsrv\appcmd set config /section:urlCompression /doDynamicCompression:True /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1
    
    REM   ERRORLEVEL 183 occurs when trying to add a section that already exists. This error is expected if this
    REM   batch file were executed twice. This can occur and must be accounted for in a Azure startup
    REM   task. To handle this situation, set the ERRORLEVEL to zero by using the Verify command. The Verify
    REM   command will safely set the ERRORLEVEL to zero.
    IF %ERRORLEVEL% EQU 183 DO VERIFY > NUL
    
    REM   If the ERRORLEVEL is not zero at this point, some other error occurred.
    IF %ERRORLEVEL% NEQ 0 (
        ECHO Error adding a compression section to the Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
        GOTO ErrorExit
    )
    
    REM   *** Add compression for json. ***
    %windir%\system32\inetsrv\appcmd set config  -section:system.webServer/httpCompression /+"dynamicTypes.[mimeType='application/json; charset=utf-8',enabled='True']" /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1
    IF %ERRORLEVEL% EQU 183 VERIFY > NUL
    IF %ERRORLEVEL% NEQ 0 (
        ECHO Error adding the JSON compression type to the Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
        GOTO ErrorExit
    )
    
    REM   *** Exit batch file. ***
    EXIT /b 0
    
    REM   *** Log error and exit ***
    :ErrorExit
    REM   Report the date, time, and ERRORLEVEL of the error.
    DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
    TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
    ECHO An error occurred during startup. ERRORLEVEL = %ERRORLEVEL% >> "%TEMP%\StartupLog.txt" 2>&1
    EXIT %ERRORLEVEL%


## <a name="add-firewall-rules"></a>Hinzufügen von Firewallregeln

In Azure gibt es effektiv zwei Firewalls. Die erste Firewall steuert Verbindungen zwischen den virtuellen Computern und außen. Diese Firewall wird durch die [Endpunkte] Element in der Datei [ServiceDefinition.csdef] gesteuert.

Die zweite Firewall steuert Verbindungen zwischen des virtuellen Computers und den Prozessen innerhalb dieser virtuellen Computern an. Diese Firewall gesteuert werden kann, indem Sie die `netsh advfirewall firewall` Befehlszeile Tool.

Azure erstellt Firewall-Regeln für die Schritte in der Rollen Prozesse an. Wenn Sie einen Dienst oder ein Programm starten, erstellt Azure beispielsweise automatisch die notwendigen Firewall-Regeln, um diesen Dienst zur Kommunikation mit dem Internet zu ermöglichen. Wenn Sie einen Dienst, der von einem anderen Prozess außerhalb Ihres Aufgabenbereichs (wie etwa ein COM +-Dienst oder ein Windows-Task geplant) gestartet wird erstellen, müssen Sie jedoch eine Firewall-Regel, um auf diesen Dienst zugreifen dürfen manuell zu erstellen. Diese Firewall-Regeln können mithilfe einer Startaufgabe erstellt werden.

Eine Startaufgabe, die eine Firewall-Regel erstellt wird, müssen eine[Aufgabe] des **erhöhten** [ExecutionContext]. Die Datei [ServiceDefinition.csdef] den folgenden Start Vorgang hinzufügen.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="AddFirewallRules.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

Wenn Sie die Firewall-Regel hinzufügen möchten, müssen Sie die entsprechende verwenden `netsh advfirewall firewall` Befehle in Ihrer Batchdatei starten. In diesem Beispiel ist die Startaufgabe Sicherheit und Verschlüsselung für TCP-80 erforderlich.

    REM   Add a firewall rule in a startup task.
    
    REM   Add an inbound rule requiring security and encryption for TCP port 80 traffic.
    netsh advfirewall firewall add rule name="Require Encryption for Inbound TCP/80" protocol=TCP dir=in localport=80 security=authdynenc action=allow >> "%TEMP%\StartupLog.txt" 2>&1
    
    REM   If an error occurred, return the errorlevel.
    EXIT /B %errorlevel%


## <a name="block-a-specific-ip-address"></a>Bestimmte IP-Adressen

Sie können eine Azure Rolle Webzugriff auf eine Gruppe von angegebenen IP-Adressen durch Ändern der IIS-Datei **web.config** einschränken. Sie benötigen ferner eine Befehlsdatei verwenden, die **IP** -Abschnitt der Datei **ApplicationHost.config** die Sperre aufhebt.

Erstellen Sie eine Befehlsdatei, die am Anfang der Rolle ausgeführt wird, um die **IP** -Abschnitt der Datei **ApplicationHost.config** entsperren möchten gehen Sie wie folgt. Erstellen Sie einen Ordner auf der Stammebene Ihrer Web Rolle **Start** aufgerufen und erstellen Sie in diesem Ordner, eine Batchdatei namens **startup.cmd**. Fügen Sie diese Datei zum Projekt Visual Studio und Festlegen der Eigenschaften auf **Immer kopieren** um sicherzustellen, dass es in Ihrem Paket enthalten ist.

Die Datei [ServiceDefinition.csdef] den folgenden Start Vorgang hinzufügen.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WebRole name="WebRole1">
        ...
        <Startup>
            <Task commandLine="startup.cmd" executionContext="elevated" />
        </Startup>
    </WebRole>
</ServiceDefinition>
```

Fügen Sie zu der Datei **startup.cmd** diesen Befehl ein:

    @echo off
    @echo Installing "IPv4 Address and Domain Restrictions" feature 
    powershell -ExecutionPolicy Unrestricted -command "Install-WindowsFeature Web-IP-Security"
    @echo Unlocking configuration for "IPv4 Address and Domain Restrictions" feature 
    %windir%\system32\inetsrv\AppCmd.exe unlock config -section:system.webServer/security/ipSecurity

Dieser Vorgang bewirkt, dass die Batchdatei **startup.cmd** jedes Mal, wenn die Web-Rolle Initialisierung um sicherzustellen, dass die erforderlichen **IP** -Abschnitts vorher aufgehoben wird, ausgeführt werden soll.

Ändern Sie im [Abschnitt system.webServer](http://www.iis.net/configreference/system.webserver/security/ipsecurity#005) abschließend **Ihre Webserverrolle Webkonfigurationsdatei zum Hinzufügen einer Liste von IP-Adressen, die Zugriff gewährt werden, wie im folgenden Beispiel gezeigt** :

Dieses Beispiel Config **ermöglicht** allen IP-Adressen Zugriff auf den Server, mit Ausnahme der beiden definiert

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are granted access-->
    <ipSecurity>
        <!--The following IP addresses are denied access-->
        <add allowed="false" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="false" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

Dieses Beispiel Config **verweigert** werden alle IP-Adressen aus den Zugriff auf dem Server, eine Ausnahme bilden jedoch die beiden definiert.

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are denied access-->
    <ipSecurity allowUnlisted="false">
        <!--The following IP addresses are granted access-->
        <add allowed="true" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="true" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

## <a name="create-a-powershell-startup-task"></a>Erstellen einer PowerShell Start-Aufgabe

Windows PowerShell-Skripts können nicht direkt aus der Datei [ServiceDefinition.csdef] aufgerufen werden, aber sie können innerhalb einer Batchdatei Start aus aufgerufen werden.

PowerShell (standardmäßig) wird nicht signierte Skripts nicht ausgeführt. Es sei denn, Sie Ihr Skript signieren, müssen Sie zum Ausführen von nicht signierte Skripts PowerShell konfigurieren. Um nicht signierte Skripts ausführen zu können, muss der **ExecutionPolicy** auf **nicht eingeschränkt**festgelegt werden. Die Einstellung **ExecutionPolicy** , mit denen Sie basiert auf der Version von Windows PowerShell.

    REM   Run an unsigned PowerShell script and log the output
    PowerShell -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1
        
    REM   If an error occurred, return the errorlevel.
    EXIT /B %errorlevel%


Wenn Sie eine Gast OS verwenden, die ausgeführt wird können PowerShell 2.0 oder 1.0 erzwingen Version 2 ausführen, und wenn nicht verfügbar ist, verwenden Sie Version 1.

    REM   Attempt to set the execution policy by using PowerShell version 2.0 syntax.
    PowerShell -Version 2.0 -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

    REM   If PowerShell version 2.0 isn't available. Set the execution policy by using the PowerShell
    IF %ERRORLEVEL% EQU -393216 (

       PowerShell -Command "Set-ExecutionPolicy Unrestricted" >> "%TEMP%\StartupLog.txt" 2>&1
       PowerShell .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1
    )

    REM   If an error occurred, return the errorlevel.
    EXIT /B %errorlevel%

## <a name="create-files-in-local-storage-from-a-startup-task"></a>Erstellen von Dateien in der lokalen Speicher von einem Vorgang Start

Eine lokale Speicherressource können zum Speichern von Dateien, die von der Startaufgabe, die später von der Anwendung zugegriffen wird erstellt.

Zum Erstellen der Ressource lokaler Speicher Hinzufügen eines Abschnitts [LocalResources] zu der Datei [ServiceDefinition.csdef] , und fügen Sie das [LocalStorage] untergeordnetes Element. Geben Sie der lokale Speicherressource einen eindeutigen Namen und eine geeignete Größe für die Startaufgabe.

Um eine lokale Speicherressource in der Startaufgabe verwenden zu können, müssen Sie eine Umgebungsvariable-, die zum Verweisen auf des lokalen Speicherorts für die Ressource zu erstellen. Dann können die Startaufgabe und die Anwendung lesen und Schreiben von Dateien, die der Speicherressource lokalen.

Die entsprechenden Abschnitten der Datei **ServiceDefinition.csdef** lauten wie folgt:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WorkerRole name="WorkerRole1">
    ...
        
    <LocalResources>
      <LocalStorage name="StartupLocalStorage" sizeInMB="5"/>
    </LocalResources>
        
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
          <Variable name="PathToStartupStorage">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
  </WorkerRole>
</ServiceDefinition>
```

Beispielsweise wird diese Batchdatei **Startup.cmd** der **PathToStartupStorage** Umgebungsvariablen erstellen die Datei **MyTest.txt** auf den lokalen Speicherort.

    REM   Create a simple text file.

    ECHO This text will go into the MyTest.txt file which will be in the    >  "%PathToStartupStorage%\MyTest.txt"
    ECHO path pointed to by the PathToStartupStorage environment variable.  >> "%PathToStartupStorage%\MyTest.txt"
    ECHO The contents of the PathToStartupStorage environment variable is   >> "%PathToStartupStorage%\MyTest.txt"
    ECHO "%PathToStartupStorage%".                                          >> "%PathToStartupStorage%\MyTest.txt"

    REM   Exit the batch file with ERRORLEVEL 0.

    EXIT /b 0

Sie können mithilfe der Methode [GetLocalResource](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) Ordner lokale Speicher aus dem Azure SDK zugreifen.

```csharp
string localStoragePath = Microsoft.WindowsAzure.ServiceRuntime.RoleEnvironment.GetLocalResource("StartupLocalStorage").RootPath;

string fileContent = System.IO.File.ReadAllText(System.IO.Path.Combine(localStoragePath, "MyTestFile.txt"));
```


## <a name="run-in-the-emulator-or-cloud"></a>Führen Sie im Emulator oder cloud

Sie können veranlassen, dass die Startaufgabe verschiedene Schritte ausführen, wenn es in der Cloud, im Vergleich zu in der Serveremulator funktioniert. Beispielsweise, wenn Sie eine neue Kopie Ihrer SQL-Daten verwenden Sie nur während der Ausführung im Emulator möchten. Oder möchten Sie möglicherweise einige Leistung Optimierungen für die Cloud führen, die Sie nicht tun, wenn im Emulator ausgeführt müssen.

Diese Möglichkeit für andere Aktionen auf der Serveremulator und der Cloud kann durch Erstellen einer Umgebungsvariable in der Datei [ServiceDefinition.csdef] erfolgen. Sie testen dann diese Umgebungsvariable-, die nach einem Wert in der Startaufgabe ein.

Um die Umgebungsvariable zu erstellen, fügen Sie die [Variable]/[RoleInstanceValue] Element, und erstellen Sie einen XPath-Wert der `/RoleEnvironment/Deployment/@emulated`. Der Wert der Umgebungsvariablen **"% ComputeEmulatorRunning"** ist `true` unter der Serveremulator und `false` Wenn in der Cloud ausführen.


```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WorkerRole name="WorkerRole1">

    ...

    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>

  </WorkerRole>
</ServiceDefinition>
```

Der Vorgang kann jetzt überprüfen Sie die **"% ComputeEmulatorRunning"** Umgebungsvariable um anderen basierend auf Aktionen, ob die Rolle in der Cloud oder den Emulator ausgeführt wird. Hier ist eine .cmd Shellskript, die für diese Umgebungsvariable überprüft.

    REM   Check if this task is running on the compute emulator.

    IF "%ComputeEmulatorRunning%" == "true" (
        REM   This task is running on the compute emulator. Perform tasks that must be run only in the compute emulator.
        
    ) ELSE (
        REM   This task is running on the cloud. Perform tasks that must be run only in the cloud.
        
    )


## <a name="detect-that-your-task-has-already-run"></a>Erkennen Sie, dass die Aufgabe bereits ausgeführt wurde

Die Rolle möglicherweise wiederverwenden, ohne einen Neustart des Computers bewirken, dass Ihre Vorgänge Start erneut ausführen. Es gibt keine kennzeichnen, um anzugeben, dass eine Aufgabe bereits Hostinganbieter des virtuellen Computers ausgeführt wurde. Möglicherweise müssen Sie einige Aufgaben, in dem es spielt keine Rolle, dass sie mehrfach ausführen. Sie können jedoch möglicherweise in einer Situation ausführen, in dem Sie benötigen, um zu verhindern, dass einen Vorgang mehr als einmal ausführen.

Die einfachste Methode zum erkennen, dass eine Aufgabe bereits ausgeführt wurde, wird zum Erstellen einer Datei in den Ordner **"% Temp%"** , wenn der Vorgang erfolgreich ist, und suchen Sie dafür am Anfang des Vorgangs. Hier ist ein Beispiel für Cmd Shell-Skript, die das für Sie ein.

    REM   If Task1_Success.txt exists, then Application 1 is already installed.
    IF EXIST "%RoleRoot%\Task1_Success.txt" (
      ECHO Application 1 is already installed. Exiting. >> "%TEMP%\StartupLog.txt" 2>&1
      GOTO Finish
    )

    REM   Run your real exe task
    ECHO Running XYZ >> "%TEMP%\StartupLog.txt" 2>&1
    "%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

    IF %ERRORLEVEL% EQU 0 (
      REM   The application installed without error. Create a file to indicate that the task
      REM   does not need to be run again.

      ECHO This line will create a file to indicate that Application 1 installed correctly. > "%RoleRoot%\Task1_Success.txt"
      
    ) ELSE (
      REM   An error occurred. Log the error and exit with the error code.

      DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
      TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
      ECHO  An error occurred running task 1. Errorlevel = %ERRORLEVEL%. >> "%TEMP%\StartupLog.txt" 2>&1

      EXIT %ERRORLEVEL%
    )

    :Finish

    REM   Exit normally.
    EXIT /B 0

## <a name="task-best-practices"></a>Bewährte Methoden für die Aufgabe
Hier sind einige bewährte Methoden, die sollten Sie beim Konfigurieren der Aufgabe für Ihre Rolle Web oder Arbeitskollegen befolgen.

### <a name="always-log-startup-activities"></a>Melden Sie immer Start Aktivitäten

Visual Studio bietet keinen Debugger um schrittweise Stapeldateien, dass er gute können Sie so viele Daten zu gelangen, klicken Sie auf den Vorgang Stapel Dateien wie möglich ist. Protokollierung der Ausgabe Stapelverarbeitungsdateien, sowohl **Stdout** und **Stderr**können bieten Ihnen wichtige Informationen bei dem Versuch, Debuggen und Beheben von Stapelverarbeitungsdateien. Fügen Sie Text hinzu, um die Datei StartupLog.txt im Verzeichnis auf die **"% Temp%"** Umgebungsvariable zeigt sowohl **Stdout** und **Stderr** Anmeldung, `>>  "%TEMP%\\StartupLog.txt" 2>&1` an das Ende von bestimmter Zeilen, die Sie melden möchten. Wenn Sie beispielsweise setup.exe im Verzeichnis **% PathToApp1Install %** ausführen:

    "%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

Zur Vereinfachung der Xml können Sie *eine Befehlsdatei Wrapper* erstellen, die Ruft alle Vorgänge sowie Protokollierung starten und damit ist sichergestellt, dass jede untergeordnete-Aufgabe die gleichen Umgebungsvariablen teilt. Dies möglicherweise eine 

Möglicherweise erachten Sie es lästig zu verwendende `>> "%TEMP%\StartupLog.txt" 2>&1` am Ende der einzelnen Vorgänge starten. Sie können die Aufgabe Protokollierung erzwingen, durch die Erstellung eines Wrappers, das Protokollierung für Sie behandelt. Dieser Wrapper Ruft die real Batchdatei, die Sie ausführen möchten. Alle Ausgabe die Ziel-Batchdatei wird in der Datei *Startuplog.txt* umgeleitet werden.

Im folgenden Beispiel wird gezeigt, wie alle Ausgaben aus einer Batchdatei Start umleiten. In diesem Beispiel wird die Datei ServerDefinition.csdef eine Startaufgabe, die *logwrap.cmd*ruft erstellt. *logwrap.cmd* ruft *Startup2.cmd*, die Ausgabe in die umleiten **"% Temp%"\\StartupLog.txt**.

ServiceDefinition.cmd:

```xml
<Startup>
    <Task commandLine="logwrap.cmd startup2.cmd" executionContext="limited" taskType="simple" />
</Startup>
```

**logwrap.cmd:**

```cmd
@ECHO OFF

REM   logwrap.cmd calls passed in batch file, redirecting all output to the StartupLog.txt log file.

ECHO [%date% %time%] == START logwrap.cmd ============================================== >> "%TEMP%\StartupLog.txt" 2>&1
ECHO [%date% %time%] Running %1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   Call the child command batch file, redirecting all output to the StartupLog.txt log file.
START /B /WAIT %1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   Log the completion of child command.
ECHO [%date% %time%] Done >> "%TEMP%\StartupLog.txt" 2>&1

IF %ERRORLEVEL% EQU 0 (

   REM   No errors occurred. Exit logwrap.cmd normally.
   ECHO [%date% %time%] == END logwrap.cmd ================================================ >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO.  >> "%TEMP%\StartupLog.txt" 2>&1
   EXIT /B 0
   
) ELSE (

   REM   Log the error.
   ECHO [%date% %time%] An error occurred. The ERRORLEVEL = %ERRORLEVEL%.  >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO [%date% %time%] == END logwrap.cmd ================================================ >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO.  >> "%TEMP%\StartupLog.txt" 2>&1
   EXIT /B %ERRORLEVEL%
   
)
```

**Startup2.cmd:**

```cmd
@ECHO OFF

REM   This is the batch file where the startup steps should be performed. Because of the
REM   way Startup2.cmd was called, all commands and their outputs will be stored in the
REM   StartupLog.txt file in the directory pointed to by the TEMP environment variable.

REM   If an error occurs, the following command will pass the ERRORLEVEL back to the
REM   calling batch file.

ECHO [%date% %time%] Some log information about this task
ECHO [%date% %time%] Some more log information about this task

EXIT %ERRORLEVEL%
```

Beispiel für die Ausgabe in der Datei **StartupLog.txt** :

```txt
[Mon 10/17/2016 20:24:46.75] == START logwrap.cmd ============================================== 
[Mon 10/17/2016 20:24:46.75] Running command1.cmd 
[Mon 10/17/2016 20:24:46.77] Some log information about this task
[Mon 10/17/2016 20:24:46.77] Some more log information about this task
[Mon 10/17/2016 20:24:46.77] Done 
[Mon 10/17/2016 20:24:46.77] == END logwrap.cmd ================================================ 
```

>[AZURE.TIP] Die Datei **StartupLog.txt** befindet sich die *C:\Resources\temp\\{Rollenbezeichner} \RoleTemp* Ordner.

### <a name="set-executioncontext-appropriately-for-startup-tasks"></a>Festlegen der ExecutionContext ordnungsgemäß für den Start-Vorgänge

Festlegen von Berechtigungen für den Start-Vorgang ordnungsgemäß. Manchmal müssen Startaufgaben mit erhöhten ausführen, obwohl die Rolle normale Rechte ausgeführt wird.

Das[Vorgang] [ExecutionContext]-Attribut legt die Berechtigungsstufe des Vorgangs Start. Verwenden von `executionContext="limited"` bedeutet, dass die Startaufgabe weist dieselben Berechtigungen wie die Rolle. Verwenden von `executionContext="elevated"` bedeutet die Startaufgabe verfügt Administratorrechte, wodurch den Vorgang Start Administratoraufgaben ausführen, ohne dass Administratorrechte für Ihre Rolle.

Ein Beispiel für eine Aufgabe Autostart, die erhöhten erfordert ist eine Startaufgabe, die **AppCmd.exe** zum Konfigurieren von IIS verwendet. **AppCmd.exe** erfordert `executionContext="elevated"`.

### <a name="use-the-appropriate-tasktype"></a>Verwenden Sie die entsprechenden taskType

Das[Vorgang] [TaskType]Attribut bestimmt, dass die Methode der Start-Vorgang ausgeführt wird. Es gibt drei Werte: **einfach**, **Hintergrund**und **Vordergrund**. Die Hintergrund- und Vordergrundfarben Aufgaben asynchrone gestartet werden, und klicken Sie dann die einfachen Aufgaben synchron ausgeführt werden einzeln nacheinander.

Bei **einfachen** Startaufgaben können Sie die Reihenfolge, in der die Aufgaben ausführen, durch die Reihenfolge festlegen, in denen die Aufgaben in der Datei ServiceDefinition.csdef aufgeführt sind. Wenn eine **einfache** Aufgabe mit einem Beendigungscode ungleich null endet, klicken Sie dann die Startprozedur beendet wird und die Rolle nicht gestartet.

Der Unterschied zwischen **Start Hintergrundaufgaben** und **Vordergrund** Startaufgaben ist, dass **Vordergrund** Aufgaben, die Rolle behalten erst am Ende des Vorgangs **Vordergrund** ausgeführt. Dies bedeutet auch, dass, wenn die Aufgabe **Vordergrund** hängt oder stürzt ab, die Rolle wird nicht Papierkorb, bis die Aufgabe **Vordergrund** erzwungen wird geschlossen. Aus diesem Grund werden **Hintergrundaufgaben** für Start asynchrone Vorgänge empfohlen, es sei denn, Sie dieses Feature des Vorgangs **Vordergrund** benötigen.

### <a name="end-batch-files-with-exit-b-0"></a>Ende Stapelverarbeitungsdateien mit beenden/b 0

Die Rolle wird nur gestartet, **Errorlevel** aus jeder der Startaufgabe einfache 0 (null) ist. Nicht alle Programme **Errorlevel** (Beendigungscode) ordnungsgemäß festgelegt, damit die Batchdatei mit enden soll eine `EXIT /B 0` Wenn alles richtig ist.

Eine fehlende `EXIT /B 0` am Ende eines Stapels Start-Datei ist eine häufige Ursache Rollen aus, die nicht gestartet werden.

>[AZURE.NOTE] Ich habe die geschachtelte Stapel bemerkt Dateien manchmal hängen sich bei Verwendung der `/B` Parameter. Sie möchten sicherstellen, dass dieses Problem hängt nicht geschieht, wenn einer anderen Batchdatei Ihrer aktuellen Batchdatei Anrufe, wie wenn Sie das [Protokoll Wrapper](#always-log-startup-activities)verwenden. Sie können Auslassen der `/B` Parameter in diesem Fall.

### <a name="expect-startup-tasks-to-run-more-than-once"></a>Erwarten Sie beim Start Vorgänge mehr als einmal ausführen

Nicht alle Rolle archiviert einschließen ein Neustart des Computers, jedoch alle Rolle archiviert alle Start-Vorgänge ausgeführt. Dies bedeutet, dass beim Startaufgaben mehrmals zwischen nach einem Neustart ohne Probleme ausgeführt werden müssen. Dies wird im [vorherigen Abschnitt](#detect-that-your-task-has-already-run)erläutert.

### <a name="use-local-storage-to-store-files-that-must-be-accessed-in-the-role"></a>Verwenden Sie zum Speichern von Dateien, die erfolgen müssen in der Rolle lokalen Speicher

Wenn kopieren oder erstellen Sie eine Datei während der Startaufgabe, die für Ihre Rolle dann zugegriffen werden soll, muss die Datei in einem lokalen Speicher positioniert sein. Finden Sie im [vorhergehenden Abschnitt](#create-files-in-local-storage-from-a-startup-task).

## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie die Cloud [Service-Modell und Paket](cloud-services-model-and-package.md)

Erfahren Sie mehr über die Funktionsweise von [Aufgaben](cloud-services-startup-tasks.md) .

[Erstellen und Bereitstellen von](cloud-services-how-to-create-deploy-portal.md) der Cloud-Service-Paket.


[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Aufgabe]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Startup]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Umgebung]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Variable]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
[Endpunkte]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Endpoints
[LocalStorage]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalStorage
[LocalResources]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalResources
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
