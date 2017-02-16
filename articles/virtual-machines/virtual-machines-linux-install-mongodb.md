<properties
   pageTitle="Installieren MongoDB einer Linux virtuellen Computers | Microsoft Azure"
   description="Informationen Sie zum Installieren und Konfigurieren von MongoDB auf einem Computer in das Modell zur Bereitstellung von Ressourcenmanager mit Azure Linux."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/29/2016"
   ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-linux-vm-in-azure"></a>Installieren Sie und konfigurieren Sie eine Linux virtuellen Computers in Azure MongoDB
[MongoDB](http://www.mongodb.org) ist eine beliebte Source-öffnen, leistungsfähige NoSQL-Datenbank. In diesem Artikel wird gezeigt, wie installieren und Konfigurieren von MongoDB auf einer Linux VM in Azure Ressourcenmanager Bereitstellungsmodell verwenden. Beispiele dieser Details wie zu:

- [Manuell installieren Sie und konfigurieren Sie eine einfache MongoDB Instanz](#manually-install-and-configure-mongodb-on-a-vm)
- [Erstellen Sie eine einfache MongoDB Instanz mithilfe einer Vorlage Ressourcenmanager](#create-basic-mongodb-instance-on-centos-using-a-template)
- [Erstellen einer komplexen MongoDB sharded Cluster mit Replikat legt mithilfe einer Vorlage Ressourcenmanager](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="prerequisites"></a>Erforderliche Komponenten
In diesem Artikel benötigen Sie Folgendes:

- ein Azure-Konto ([kostenlose Testversion erhalten](https://azure.microsoft.com/pricing/free-trial/)).
- die [Azure CLI](../xplat-cli-install.md) in angemeldet`azure login`
- die Azure CLI *muss* in Azure Ressourcenmanager im Modus verwendet wird`azure config mode arm`


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Manuell installieren und Konfigurieren von MongoDB eines virtuellen Computers
MongoDB für Linux Distros einschließlich Red Hat [Bereitstellen von Anweisungen Installation](https://docs.mongodb.com/manual/administration/install-on-linux/) / CentOS, SuSE-, Ubuntu- und Debian. Im folgenden Beispiel wird eine `CoreOS` virtueller Computer mit einer gespeicherten am SSH-Schlüssel `.ssh/azure_id_rsa.pub`. Beantworten Sie die Anweisungen für Speicher Kontonamen, DNS-Namen und Administrator-Anmeldeberechtigungen aus:

```bash
azure vm quick-create --ssh-publickey-file .ssh/azure_id_rsa.pub --image-urn CentOS
```

Melden Sie sich bei dem virtuellen Computer mithilfe der öffentlichen IP-Adresse am Ende des vorherigen Erstellungsschritt virtueller Computer angezeigt:

```bash
ssh ops@40.78.23.145
```

Um die Installationsquellen für MongoDB hinzuzufügen, Erstellen einer `yum` Repository-Datei wie folgt:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.2.repo
```

Die MongoDB Repo Datei zur Bearbeitung zu öffnen. Fügen Sie die folgenden Zeilen hinzu:

```bash
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc
```

Installieren mit MongoDB `yum` wie folgt:

```bash
sudo yum install -y mongodb-org
```

Standardmäßig wird SELinux auf CentOS Bilder, die verhindert, dass Sie den Zugriff auf MongoDB erzwungen. Installieren Sie Richtlinie Verwaltungstools und konfigurieren Sie SELinux MongoDB wie folgt auf deren TCP-Standardport 27017 Betrieb zulässig. 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Starten Sie den Dienst MongoDB wie folgt:

```bash
sudo service mongod start
```

Überprüfen Sie die Installation MongoDB durch Herstellen einer Verbindung mit dem lokalen `mongo` Client:

```bash
mongo
```

Testen Sie jetzt die MongoDB Instanz durch Hinzufügen einiger Daten, und klicken Sie dann auf Suchen:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Konfigurieren Sie bei Bedarf MongoDB für den automatischen start bei einem Neustart des Systems:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Erstellen Sie einfache MongoDB Instanz auf CentOS mithilfe einer Vorlage
Sie können eine einfache MongoDB Instanz eines einzelnen CentOS virtuellen Computers mithilfe der folgenden Azure Schnellstart-Vorlage von Github erstellen. Diese Vorlage verwendet die benutzerdefinierte Skript-Erweiterung für Linux zum Hinzufügen eines `yum` Repository neu erstellten CentOS virtueller Computer und dann installieren MongoDB.

- [Grundlegende MongoDB Instanz auf CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `WestUS` Region. Geben Sie Ihre eigenen Werte wie folgt aus:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [AZURE.NOTE] Die CLI Azure wieder Sie eine Aufforderung in ein paar Sekunden die Bereitstellung zu erstellen, ohne die Installation und Konfiguration dauert ein paar Minuten. Überprüfen Sie den Status der Bereitstellung mit `azure group deployment show myResourceGroup`, den Namen der Ressourcengruppe entsprechend eingeben. Warten Sie, bis die `ProvisioningState` 'Erfolgreich' zeigt, bevor Sie versuchen, SSH auf den virtuellen Computer.

Nachdem die Bereitstellung abgeschlossen, SSH auf den virtuellen Computer ist. Beziehen Sie die IP-Adresse des virtuellen Computer mit der `azure vm show` -Befehl wie im folgenden Beispiel gezeigt:

```bash
azure vm show --resource-group myResourceGroup --name myVM
```

Am Ende der Ausgabe der `Public IP address` wird angezeigt. SSH zu Ihrem virtuellen Computer mit der IP-Adresse Ihre virtuellen Computer:

```bash
ssh ops@138.91.149.74
```

Überprüfen Sie die Installation MongoDB durch Herstellen einer Verbindung mit dem lokalen `mongo` Client wie folgt:

```bash
mongo
```

Testen Sie jetzt die Instanz durch Hinzufügen einiger Daten, und suchen wie folgt ein:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Erstellen Sie eine komplexe MongoDB Sharded Cluster auf CentOS mithilfe einer Vorlage
Sie können einen komplexen MongoDB sharded Cluster mit der folgenden Azure Schnellstart-Vorlage von Github erstellen. Diese Vorlage folgt die [MongoDB sharded Cluster bewährte Methoden](https://docs.mongodb.com/manual/core/sharded-cluster-components/) zum und ein hohen Verfügbarkeit. Die Vorlage erstellt zwei mehrere Shards hinweg, mit drei Knoten in jeder Replikatgruppe. Eine Config Server Replikatsatz mit drei Knoten auch erstellt, plus zwei `mongos` Routerservern Konsistenz von Applications über die mehrere Shards hinweg bereitstellen.

- [MongoDB Sharding Cluster auf CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [AZURE.WARNING] Bereitstellen von diesem komplexen MongoDB sharded Cluster erfordert mehr als 20 Prozessorkerne, welche ist in der Regel die Anzahl der Standardeinstellung Core pro Region für ein Abonnement. Öffnen Sie eine Azure Supportanfrage, Ihre Core Zähler zu erhöhen.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `WestUS` Region. Geben Sie Ihre eigenen Werte wie folgt aus:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [AZURE.NOTE] Die CLI Azure wieder Sie eine Aufforderung in ein paar Sekunden die Bereitstellung zu erstellen, ohne die Installation und Konfiguration können in Anspruch nehmen mehr als eine Stunde. Überprüfen Sie den Status der Bereitstellung mit `azure group deployment show myResourceGroup`, den Namen der Ressourcengruppe entsprechend anpassen. Warten Sie, bis die `ProvisioningState` 'Erfolgreich' vor dem Herstellen einer Verbindung mit den virtuellen Computern zeigt.


## <a name="next-steps"></a>Nächste Schritte
In diesen Beispielen verbinden Sie mit der MongoDB Instanz lokal aus dem virtuellen Computer. Wenn Sie die MongoDB-Instanz aus einem anderen virtuellen Computer oder Netzwerk verbinden möchten, stellen Sie sicher, das entsprechende [Netzwerk-Sicherheitsgruppe Regeln erstellt werden](virtual-machines-linux-nsg-quickstart.md).

Weitere Informationen zum Verwenden von Vorlagen erstellen finden Sie unter der [Ressourcenmanager Azure Übersicht](../azure-resource-manager/resource-group-overview.md).

Ressourcenmanager Azure Vorlagen mithilfe der benutzerdefinierte Skripts Erweiterung herunterladen und Ausführen von Skripts auf Ihre virtuellen Computern. Weitere Informationen finden Sie unter [Verwenden der Azure benutzerdefinierte Skripts Erweiterung mit Linux virtuellen Computern](virtual-machines-linux-extensions-customscript.md).