<properties
   pageTitle="Einrichten Ihrer Entwicklungsumgebung unter Mac OS X | Microsoft Azure"
   description="Installieren Sie die Laufzeit, SDK und Tools, und erstellen Sie einen lokale Entwicklung Cluster. Nach Abschluss der diese Installation werden Sie zum Erstellen von Applications unter Mac OS X bereit."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="seanmck"/>

# <a name="set-up-your-development-environment-on-mac-os-x"></a>Richten Sie Ihrer Entwicklungsumgebung unter Mac OS X ein

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

Sie können Service Fabric Applikationen auf Linux-Cluster mithilfe von Mac OS X ausführen erstellen. In diesem Artikel beschrieben, wie Sie Ihrem Mac für die Entwicklung einrichten.

## <a name="prerequisites"></a>Erforderliche Komponenten

Dienst Fabric wird nicht systembedingt OS X ausgeführt. Zum Ausführen eines lokalen Dienst Fabric Clusters stellen wir eine vorkonfiguriertes Ubuntu virtuellen Computern, die mit Vagrant und VirtualBox. Bevor Sie beginnen, müssen Sie folgende Schritte ausführen:

- [Vagrant (v1.8.4 oder höher)](http://wwww.vagrantup.com/downloads)
- [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

## <a name="create-the-local-vm"></a>Erstellen Sie den lokalen virtueller Computer

Führen Sie folgende Schritte aus, um den lokalen virtuellen Computer mit einem Dienst Fabric Cluster mit 5 Knoten zu erstellen:

1. Die Vagrantfile Repo Klonen

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```

2. Navigieren Sie zu der lokalen datenbeschriftungsreihe von der repo

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```

3. (Optional) Ändern der Standardeinstellungen für die virtuellen Computer

    Standardmäßig wird der lokale virtuellen Computer wie folgt konfiguriert:

    - 3 GB Arbeitsspeicher
    - Privater Hostnetzwerk so konfiguriert, dass mit IP-192.168.50.50 Pass-Through-des Datenverkehrs vom Mac Host aktivieren

    Sie können eine der folgenden Einstellungen ändern oder andere Konfiguration der virtuellen Computer in der Vagrantfile hinzufügen. Den [Vagrant Dokumentation](http://www.vagrantup.com/docs) für die vollständige Liste der Optionen anzuzeigen.

4. Erstellen Sie den virtuellen Computer

    ```bash
    vagrant up
    ```

    Dieser Schritt downloads der vorkonfigurierten virtuellen Computer Bild-, Boot, die sie lokal, und richten Sie eine lokale Dienst Struktur darin cluster. Sie erwarten, es ein paar Minuten dauern. Wenn Setup erfolgreich abgeschlossen ist, sehen Sie eine Nachricht in der Ausgabe, die angibt, dass der Cluster gestartet wird.

    ![Cluster-Setup startet den folgenden virtuellen Computer bereitgestellt][cluster-setup-script]

5. Test, der der Cluster ordnungsgemäß durch Navigieren zum Dienst Fabric Explorer am Http://192.168.50.50:19080/Explorer (vorausgesetzt, dass Sie die standardmäßigen privates Netzwerk IP-Adresse beibehalten) eingerichtet wurde.

    ![Dienst Fabric Explorer auf dem Host Mac angezeigt][sfx-mac]


## <a name="install-the-service-fabric-plugin-for-eclipse-neon-optional"></a>Installieren Sie den Dienst Fabric-Plug-In für "Ellipse" Neon (optional)

Fabric-Dienst bietet ein Plug-In für die Ellipse Neon IDE, die das Erstellen und Bereitstellen von Java Services vereinfachen können.

1. In "Ellipse" Stellen Sie sicher, dass Sie Buildship Version 1.0.17 oder höher installiert sein. Sie können die Versionen der installierten Komponenten überprüfen, indem Sie auf **Hilfe > Installation Details**. Sie können mithilfe der Anweisungen Buildship aktualisieren [hier][buildship-update].

2. Wenn Sie den Dienst Fabric-Plug-In installiert haben, wählen Sie aus **Hilfe > neue Software installieren**

3. Geben Sie im Textfeld "Arbeiten mit": http://dl.windowsazure.com/eclipse/servicefabric.

4. Klicken Sie auf Hinzufügen.

    !["Ellipse" Neon-Plug-In für Fabric Service][sf-eclipse-plugin-install]

5. Wählen Sie den Dienst Fabric-Plug-in, und klicken Sie auf Weiter.

6. Führen Sie die Schritte der Installations und dem Endbenutzer-Lizenzvertrag.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen Sie Ihrer erste Fabric Service-Anwendung für Linux](service-fabric-create-your-first-linux-application-with-java.md)

<!-- Links -->

- [Erstellen Sie einen Dienst Fabric Cluster im Azure-Portal](service-fabric-cluster-creation-via-portal.md)
- [Erstellen Sie einen Dienst Fabric Cluster mithilfe der Azure Ressourcenmanager](service-fabric-cluster-creation-via-arm.md)
- [Grundlagen des Datenmodells der Fabric Service-Anwendung](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
