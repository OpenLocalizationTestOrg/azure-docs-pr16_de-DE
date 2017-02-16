<properties
    pageTitle="Verwenden von MySQL-Datenbanken als PaaS Azure Stapel | Microsoft Azure"
    description="Verstehen der QuickSteps zum Bereitstellen des MySQL-Anbieters für Ressourcen und zum Bereitstellen von MySQL als Dienst auf Azure Stapel an."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="bradleyb"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>

# <a name="use-mysql-databases-as-paas-on-azure-stack"></a>Verwenden von MySQL-Datenbanken als PaaS Azure Stapel

> [AZURE.NOTE] Die folgende Informationen gilt nur für Azure Stapel TP1 Bereitstellungen.

Sie können einen MySQL Ressourcenanbieter Azure Stapel bereitstellen. Nachdem Sie den Ressourcenanbieter bereitstellen, können Sie MySQL-Server und Datenbanken über Azure Ressourcenmanager Bereitstellung Vorlagen erstellen und MySQL-Datenbanken als Dienst bereitstellen. MySQL-Datenbanken, die auf Websites häufig verwendet werden, zu viele Website Plattformen unterstützen. Nach der Bereitstellung des Anbieters für Ressourcen, können Sie WordPress Websites aus der Azure Web Apps-Plattform als ein Add-on Service (PaaS) für Azure Stapel erstellen.

## <a name="quick-steps-to-deploy-the-resource-provider"></a>QuickSteps Anbieter für die Ressourcen bereitgestellt.
Gehen Sie folgendermaßen vor, wenn Sie bereits mit Azure Stapel vertraut sind. Wenn Sie weitere Einzelheiten hierzu erfahren möchten, führen Sie die Links in jedem Bereich, oder wechseln Sie direkt zum [Bereitstellen der MySQL-Datenbank Ressource Anbieter Netzwerkadapter auf Azure Stapel Prüfung des Konzepts ist](azure-stack-mysql-rp-deploy-long.md).

1.  Stellen Sie sicher, dass diese Sie [Alle richten Sie Schritte durchführen](azure-stack-mysql-rp-deploy-long.md#set-up-steps-before-you-deploy) , bevor Sie den Ressourcenanbieter bereitstellen:

    - .NET 3.5 Framework ist bereits in der Basis Windows Server-Image eingerichtet. (Wenn Sie den Stapel Azure Bits nach Februar 23, 2016, heruntergeladen haben können Sie diesen Schritt überspringen.)
    - [Eine Version von Azure PowerShell, die mit Azure Stapel kompatibel ist, installiert ist](http://aka.ms/azStackPsh).
    - In Internet Explorer Sicherheitseinstellungen auf die ClientVM, [Internet Explorer verbesserte Sicherheit deaktiviert ist und Cookies aktiviert sind](azure-stack-mysql-rp-deploy-long.md#Turn-off-IE-enhanced-security-and-enable-cookies).

2. [Herunterladen die MySQL-Anbieter-Binärdateien Ressourcendatei](http://aka.ms/masmysqlrp) , und klicken Sie auf die ClientVM in Ihrer Azure Stapel Nachweis des Konzepts zu extrahieren.

3. [Bootstrap.cmd und die Skripts ausführen](azure-stack-mysql-rp-deploy-long.md#Bootstrap-the-resource-provider-deployment-PowerShell-and-Prepare-for-deployment).

    Eine Reihe von Skripts, die durch die beiden wichtigsten Registerkarten gruppiert ist, wird in der PowerShell Integrated Scripting Umgebung (ISE) geöffnet. Führen Sie alle die geladen Skripts in der Reihenfolge von links nach rechts auf jeder Registerkarte.

    1. Führen Sie die Skripts auf der Registerkarte **Vorbereiten** von links nach rechts, um:

        - Erstellen Sie ein Platzhalterzeichen Zertifikat zum Schutz der Kommunikation zwischen den Anbieter für Ressourcen und Azure Ressourcenmanager.
        - Zustimmen Sie MySQL-Endbenutzer-Lizenzvertrag, und Laden Sie die MySQL-Binärdateien.
        - Hochladen der Zertifikate und alle anderen Elemente zu einem Stapel Azure-Speicher-Konto an.
        - Veröffentlichen von Katalog-Paketen, damit Sie MySQL-Ressourcen durch den Katalog bereitstellen können.

        > [AZURE.IMPORTANT] Allen Skripts keinen Grund dafür legen, nachdem Sie Ihre Azure Active Directory-Mandanten senden, können die Sicherheitsstufe eine DLL blockieren, die für die Bereitstellung ausführen erforderlich ist. Um dieses Problem zu beheben, suchen Sie nach der Microsoft.AzureStack.Deployment.Telemetry.Dll im Ordner Anbieter Ressource, klicken Sie nach rechts, darauf, klicken Sie auf **Eigenschaften**, und klicken Sie dann auf der Registerkarte **Allgemein** checken Sie **Blockierung aufheben ein** .

    2. Führen Sie die Skripts in die Registerkarte **Bereitstellen** von links nach rechts, um:

        - [Bereitstellen eines virtuellen Computers (virtueller Computer)](azure-stack-mysql-rp-deploy-long.md#Deploy-the-MySQLResource-Provider-VM) , die der Ressourcenanbieter, MySQL-Server und Datenbanken, die Sie instanziiert, gehostet wird. Dieses Skript verweist auf eine JSON Parameter-Datei, die Sie mit einigen Werten zu aktualisieren, bevor Sie das Skript ausführen müssen.
        - [Registrieren ein lokales DNS-Eintrags](azure-stack-mysql-rp-deploy-long.md#Update-the-local-DNS) , den Anbieter virtueller Computer für Ihre Ressourcen zugeordnet werden.
        - [Anbieter für Ihre Ressourcen zu registrieren](azure-stack-mysql-rp-deploy-long.md#Register-the-MySQL-RP-Resource-Provider) der lokalen Ressourcenmanager mit Azure aus.

        > [AZURE.IMPORTANT] Alle Skripts wird davon ausgegangen, dass das Bild Basis-Betriebssystem die erforderlichen Komponenten (.NET 3.5 installiert, JavaScript und Cookies aktiviert wird, klicken Sie auf die ClientVM und die neueste Version von Azure PowerShell installiert) erfüllt. Wenn ein Fehler ausgegeben, wenn Sie die Skripts ausführen, überprüfen Sie, dass Sie die erforderlichen Komponenten erfüllt.

5. Bereitstellen einer MySQL-Datenbank vom Stapel Azure-Portal [testen den neuen MySQL-Anbieter für Ressourcen](/azure-stack-MySql-rp-deploy-long.md#create-your-first-mysql-database-to=test-your-deployment). Klicken Sie auf **Erstellen** &gt; **benutzerdefinierte** &gt; **MySQL-Server und die Datenbank**.

Dies sollte Anbieter Ihre MySQL-Ressourcen einrichten und Ausführen von in etwa 25 Minuten abrufen.
