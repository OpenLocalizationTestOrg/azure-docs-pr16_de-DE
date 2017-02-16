<properties
   pageTitle="Docker und verfassen auf einem virtuellen Computer | Microsoft Azure"
   description="Kurze Einführung in das Arbeiten mit verfassen und Docker auf in Azure-virtuellen Computern Linux"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="iainfou"/>

# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-on-an-azure-virtual-machine"></a>Erste Schritte mit Docker und verfassen definieren und Ausführen einer Anwendungs mehrere Container auf eine Azure-virtuellen Computern

Erste Schritte mit Docker und [Verfassen](http://github.com/docker/compose) definieren und Ausführen einer komplexen Anwendungs auf einem Computer in Azure Linux. Mit verfassen verwenden Sie eine einfache Textdatei, um eine Anwendung aus mehreren Docker Container definieren. Sie dann Drehfeld von Ihrer Anwendung in einem einzigen Befehl, mit der alles für die Bereitstellung Ihrer Umgebung definierte unterhält. 

Als Beispiel zeigt in diesem Artikel, wie schnell Einrichten eines Blogs WordPress mit einer Back-End-MariaDB SQL-Datenbank auf einer VM Ubuntu. Verfassen können Sie auch komplexere Applikationen einrichten.


## <a name="step-1-set-up-a-linux-vm-as-a-docker-host"></a>Schritt 1: Einrichten einer Linux VM als Host Docker

Können verschiedene Azure Verfahren und verfügbaren Bilder oder Ressourcenmanager Vorlagen in der Azure Marketplace zum Erstellen einer Linux VM und als Host Docker einrichten. Beispielsweise finden Sie unter [Docker virtueller Computer die Erweiterung Ihrer Umgebung für die Bereitstellung verwenden](virtual-machines-linux-dockerextension.md) , um schnell ein Ubuntu VM mithilfe einer [Vorlage Schnellstart](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)mit der Erweiterung Azure Docker virtueller Computer zu erstellen. 

Wenn Sie die Erweiterung Docker virtuellen Computer verwenden, Ihre virtuellen Computer wird automatisch als Host Docker eingerichtet und verfassen ist bereits installiert. Das Beispiel in diesem Artikel wird gezeigt, wie die [für Mac, Linux, und Windows Azure Line-Benutzeroberfläche](../xplat-cli-install.md) (Azure CLI) verwenden in Ressourcenmanager Modus, um den virtuellen Computer zu erstellen.

Der grundlegende Befehl aus dem vorherigen Dokument erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `West US` Speicherort und einen virtuellen bereitstellt, mit der Erweiterung Azure Docker virtueller Computer installiert:

```bash
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

## <a name="step-2-verify-that-compose-is-installed"></a>Schritt 2: Stellen Sie sicher, dass verfassen installiert ist

Nach dem Abschluss der bereitstellungs SSH auf Ihrer neuen Docker Host unter Verwendung von DNS benennen Sie die bereitgestellten während der Bereitstellung. Sie können `azure vm show -g myDockerResourceGroup -n myDockerVM` zum Anzeigen von Details zu Ihrer virtuellen Computer, einschließlich der DNS-Name.

Um zu überprüfen, dass Verfassen des virtuellen Computers installiert ist, führen Sie den folgenden Befehl aus:

```bash
docker-compose --version
```

Sie sehen eine Ausgabe ähnlich `docker-compose 1.6.2, build 4d72027`.

>[AZURE.TIP] Wenn Sie eine andere Methode zum Erstellen eines Docker Hosts und verfassen selbst installieren müssen verwendet, finden Sie in der [Dokumentation verfassen](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="step-3-create-a-docker-composeyml-configuration-file"></a>Schritt 3: Erstellen einer Docker-compose.yml Konfigurationsdatei

Als Nächstes erstellen Sie ein `docker-compose.yml` -Datei, die nur Konfiguration eine Textdatei ist, um den Container Docker zum Ausführen des virtuellen Computers zu definieren. Die Datei gibt das Bild für jeden Container ausgeführt (oder dies möglicherweise einen Build aus einem Dockerfile)-, die notwendigen Umgebungsvariablen und Abhängigkeiten, Ports und die Links zwischen Container. Details zur Yml Dateisyntax finden Sie unter [Verfassen Datei verweisen](http://docs.docker.com/compose/yml/).

Erstellen der `docker-compose.yml` Datei wie folgt:

```bash
touch docker-compose.yml
```

Verwenden Sie einen Texteditor einiger Daten zu der Datei hinzufügen. Im folgenden Beispiel wird die `vi` Editor:

```bash
vi docker-compose.yml
```

Fügen Sie im folgenden Beispiel wird in der Textdatei an. Diese Konfiguration verwendet Bilder aus der [Registrierung DockerHub](https://registry.hub.docker.com/_/wordpress/) , um WordPress (der open-Source Bloggen und Inhalt Managementsystem) und eine verknüpfte Back-End-MariaDB SQL-Datenbank zu installieren. Geben Sie Ihre eigenen `MYSQL_ROOT_PASSWORD` wie folgt:

```bash
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="step-4-start-the-containers-with-compose"></a>Schritt 4: Starten Sie den Container mit verfassen

In demselben Verzeichnis wie Ihre `docker-compose.yml` Datei, und führen Sie den folgenden Befehl (je nach Ihrer Umgebung müssen Sie möglicherweise ausführen `docker-compose` mit `sudo`.):

```bash
docker-compose up -d

```

Dieser Befehl startet den angegebenen Docker Container `docker-compose.yml`. Es dauert eine oder zwei Minuten für diesen Schritt ausführen. Sie sehen die Ausgabe ähnlich wie im folgenden Beispiel:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

>[AZURE.NOTE] Achten Sie darauf, dass die Option **-d** beim Start verwenden, damit die Container kontinuierlich im Hintergrund ausgeführt werden.

Um sicherzustellen, dass die von den Container sind, geben Sie ein `docker-compose ps`. Sie sollte ungefähr wie folgt angezeigt:

```bash
Name             Command             State              Ports
-------------------------------------------------------------------------
wordpress_db_1     /docker-           Up                 3306/tcp
             entrypoint.sh
             mysqld
wordpress_wordpr   /entrypoint.sh     Up                 0.0.0.0:80->80
ess_1              apache2-for ...                       /tcp
```

Sie können jetzt mit WordPress direkt auf den virtuellen Computer auf Port 80 herstellen. Öffnen Sie einen Webbrowser, und geben Sie den DNS-Namen für Ihre virtuellen Computer (z. B. `http://myresourcegroup.westus.cloudapp.azure.com`). Die WordPress sollte nun angezeigt werden, wo Sie die Installation abschließen können und erste Schritte mit der Anwendung Startbildschirm.

![WordPress-Startbildschirm][wordpress_start]


## <a name="next-steps"></a>Nächste Schritte

* Fahren Sie mit der [Erweiterung-Benutzerhandbuch Docker virtueller Computer](https://github.com/Azure/azure-docker-extension/blob/master/README.md) für weitere Optionen zum Docker und Verfassen Ihrer Docker virtuellen Computer konfigurieren. Beispielsweise ist eine Möglichkeit zum Verfassen Yml Datei (Konvertierung in JSON) setzen direkt in die Konfiguration der Erweiterung Docker virtuellen Computer ein.
* Schauen Sie sich die [Befehlszeile Bezug verfassen](http://docs.docker.com/compose/reference/) und [Benutzerhandbuch](http://docs.docker.com/compose/) Weitere Beispiele für das Erstellen und Bereitstellen von apps mit mehreren Container.
* Verwenden Sie entweder eine Vorlage Azure Ressourcenmanager Ihrer eigenen oder eine beigetragen aus der [Community](https://azure.microsoft.com/documentation/templates/)eine Azure-virtuellen Computer mit Docker und eine Anwendung mit verfassen eingerichtet bereitstellen. Beispielsweise verwendet die Vorlage [Bereitstellen eines Blogs WordPress mit Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) Docker und verfassen WordPress schnell mit einer MySQL-Back-End-auf einer VM Ubuntu bereitstellen.
* Versuchen Sie es mit einem [Docker Swarm](virtual-machines-linux-docker-swarm.md) Cluster Integration Docker verfassen. Szenarien finden Sie unter [Mit Punktschwarms verfassen verwenden](https://docs.docker.com/compose/swarm/) .

<!--Image references-->

[wordpress_start]: ./media/virtual-machines-linux-docker-compose-quickstart/WordPress.png
