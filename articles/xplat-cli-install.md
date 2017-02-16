<properties
    pageTitle="Die Benutzeroberfläche von Azure Line installieren | Microsoft Azure"
    description="Installieren der Azure Line Interface (CLI) für Mac, Linux und Windows verwenden Azure services"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="squillace"
    services="virtual-machines-linux,virtual-network,storage,azure-resource-manager"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="rasquill"/>
    
# <a name="install-the-azure-cli"></a>Installieren der Azure CLI

> [AZURE.SELECTOR]
- [PowerShell](powershell-install-configure.md)
- [Azure CLI](xplat-cli-install.md)

Installieren Sie schnell Azure Line Interface (CLI Azure) um eine Reihe von Open Source Shell-basierten Befehle zum Erstellen und Verwalten von Ressourcen in Microsoft Azure verwenden. Sie haben mehrere Optionen, um diese Plattform-Tools auf Ihrem Computer zu installieren: 

* **Npm-Paket** – Npm (das Paketmanager für JavaScript) zum Installieren des neuesten Azure CLI-Pakets auf Ihrem Linux Verteilung oder OS ausführen. Erfordert node.js und Npm auf Ihrem Computer.
* **Installer** - Download ein für eine einfache Installation auf Mac oder Windows Installer.
* **Docker Container** – verwenden der neuesten CLI in einem sofort-und-Los Docker Container. Erfordert Docker Host auf dem Computer an.
    
Weitere Optionen und Hintergrund finden Sie auf das Projekt Repository [GitHub](https://github.com/azure/azure-xplat-cli). 

Sobald die CLI Azure ist installiert, [Verbinden es mit Ihrem Azure-Abonnement](xplat-cli-connect.md) und die **Azure** -Befehle aus der Benutzeroberfläche Line (Bash, Terminal, Eingabeaufforderung usw.) zum Arbeiten mit Ihrer Azure Ressourcen führen.



## <a name="option-1-install-an-npm-package"></a>Option 1: Installieren eines Pakets npm

Um die CLI aus einem Npm Paket installiert haben, stellen Sie sicher, dass Sie heruntergeladen und installiert die [neueste Node.js und Npm](https://nodejs.org/en/download/package-manager/). Führen Sie dann die **Npm installieren** , um das Azure-Cli-Paket zu installieren: 

    npm install -g azure-cli

Klicken Sie auf Linux-Versionen müssen Sie gegebenenfalls **Sudo** zu verwenden, um den Befehl __Npm__ erfolgreich wie folgt ausführen:

    sudo npm install -g azure-cli

> [AZURE.NOTE]Wenn Sie installieren oder Node.js und Npm auf Ihrem Linux Verteilung oder OS aktualisieren müssen, empfehlen wir, dass Sie die neueste Version von Node.js LTS (4.x) installiert haben. Wenn Sie eine ältere Version verwenden, erhalten Sie möglicherweise Fehler bei der Installation. 

Wenn Sie es vorziehen, laden Sie die neuesten Linux [Tar-Datei] [ linux-installer] für das Paket Npm lokal. Installieren Sie das Paket heruntergeladenen Npm klicken Sie dann wie folgt (auf Linux-Versionen, die Sie eventuell verwenden **Sudo**):

    npm install -g <path to downloaded tar file>

## <a name="option-2-use-an-installer"></a>Option 2: Verwenden Sie ein Installationsprogramm

Wenn Sie einen Mac oder Windows-Computer verwenden, sind die folgenden CLI Installationsprogramm zum Download zur Verfügung:

* [Mac OS X Installationsprogramm][mac-installer]

* [Windows MSI][windows-installer] 

>[AZURE.TIP]Klicken Sie auf Windows können Sie auch das [Web Platform Installer](https://go.microsoft.com/?linkid=9828653) , um die CLI installieren herunterladen. Dieses Installationsprogramm bietet Ihnen die Möglichkeit, Befehlszeile Tools und zusätzliche Azure SDK nach der Installation von der CLI installieren. 


## <a name="option-3-use-a-docker-container"></a>Option 3: Verwenden eines Containers Docker

Wenn Sie Ihren Computer als Host [Docker](https://docs.docker.com/engine/understanding-docker/) eingerichtet haben, können Sie die neuesten Azure CLI in einem Container Docker ausführen. Führen Sie den folgenden Befehl (auf Linux-Versionen, die Sie eventuell verwenden **Sudo**):

```
docker run -it microsoft/azure-cli
```


## <a name="run-azure-cli-commands"></a>Ausführen von Azure CLI-Befehle
Nach der Installation von der CLI Azure Ausführen des Befehls **Azure** aus der Befehlszeile Benutzeroberfläche (Bash, Terminal, Eingabeaufforderung usw.) Um den Hilfebefehl ausführen zu können, geben Sie beispielsweise Folgendes ein:

```
azure help
```
> [AZURE.NOTE]Klicken Sie auf einige Linux-Versionen, erhalten Sie möglicherweise eine Fehlermeldung ähnlich wie `/usr/bin/env: ‘node’: No such file or directory`. Dieser Fehler stammt aus zuletzt verwendete Installationen von Node.js am /usr/bin/nodejs installiert wird. Um das Problem zu lösen, erstellen Sie einen symbolic Link /usr/bin/node, indem Sie diesen Befehl ausführen:

```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Um die Version der CLI Azure anzuzeigen, die Sie installiert haben, geben Sie Folgendes ein:

```
azure --version
```

Jetzt sind Sie bereit sind! Die CLI-Befehle für die Arbeit mit Ihrer eigenen Ressourcen, [Herstellen einer Verbindung Ihres Azure Abonnements über die Befehlszeile Azure mit](xplat-cli-connect.md)zu gelangen.

>[AZURE.NOTE] Bei Verwendung von Azure CLI wird eine Meldung mit der Frage, ob Sie Microsoft das Erfassen von Verwendungsinformationen zulassen möchten. Teilnahme an der steht Ihnen frei. Wenn Sie zur Teilnahme entscheiden, Sie können die zu einem beliebigen Zeitpunkt durch Ausführen `azure telemetry --disable`. Führen Sie zum Aktivieren der Teilnahme zu einem beliebigen Zeitpunkt `azure telemetry --enable`.


## <a name="update-the-cli"></a>Aktualisieren der CLI

Microsoft stellt häufig aktualisierte Versionen der CLI Azure. Installieren Sie die Komponenten, die mit Hilfe des Installers für Ihr Betriebssystem erneut zu, oder führen Sie den neuesten Docker Container. Oder, wenn Sie die neuesten Node.js und Npm installiert haben, aktualisieren, indem Sie die folgenden (auf Linux-Versionen, die Sie eventuell verwenden **Sudo**) eingeben.

```
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Aktivieren Sie die Registerkarte Abschluss

Registerkarte Vervollständigung von CLI-Befehlen wird für Mac und Linux unterstützt.

Wenn es in Zsh aktivieren möchten, führen Sie Folgendes aus:

```
echo '. <(azure --completion)' >> .zshrc
```

Wenn es in Bash aktivieren möchten, führen Sie Folgendes aus:

```
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Nächste Schritte 

* [Verbinden über die Befehlszeile zu Ihrem Abonnement Azure](xplat-cli-connect.md) erstellen und Verwalten von Azure Ressourcen.

* Erfahren Sie mehr über die CLI Azure, Quellcode, Melden von Problemen, herunterladen, oder zum Projekt beitragen, finden Sie auf der [GitHub Repository für die CLI Azure](https://github.com/azure/azure-xplat-cli).

* Wenn Sie Fragen zur Nutzung der Azure CLI oder Azure haben, besuchen Sie die [Foren Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).

* Wenn Sie möchten, können Sie auch die Python-basierten [Azure CLI 2.0 Vorschau](https://github.com/azure/azure-cli)versuchen.

[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: virtual-machines-command-line-tools.md
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
