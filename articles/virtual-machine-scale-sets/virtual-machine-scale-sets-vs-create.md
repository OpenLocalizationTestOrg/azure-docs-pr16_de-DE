<properties
    pageTitle="Bereitstellen von virtuellen Computern skalieren Set mithilfe von Visual Studio | Microsoft Azure"
    description="Bereitstellen von virtuellen Computern skalieren Datensätze mit Visual Studio und Ressourcenmanager Vorlage"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="guybo"/>

# <a name="deploy-virtual-machine-scale-set-using-visual-studio"></a>Bereitstellen von virtuellen Computern skalieren Set mithilfe von Visual Studio

In diesem Artikel wird gezeigt, wie eine Azure virtuellen Computern skalieren Set mithilfe von eine Ressource Gruppe Bereitstellung von Visual Studio bereitgestellt werden kann.


[Azure-virtuellen Computern skalieren Sätze](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) sind eine Ressource Azure berechnen zum Bereitstellen und Verwalten einer Auflistung von ähnlichen virtuellen Computern einfach integrierten Optionen für die automatische Skalierung und Lastenausgleich. Bereitstellen können und Bereitstellen virtueller Computer Maßstab Datensätze mithilfe von [Vorlagen Azure Ressource Manager (Cloud)](https://github.com/Azure/azure-quickstart-templates). Cloud-Vorlagen mit Azure CLI, PowerShell REST bereitgestellt werden können und auch direkt in Visual Studio. Visual Studio bietet eine Reihe von Beispiel Vorlagen, die als Teil eines Projekts Bereitstellen einer Ressource Azure bereitgestellt werden kann.

Ressourcengruppe Azure-Bereitstellungen sind eine Möglichkeit zum Gruppieren und eine Reihe von Azure Ressourcenübersicht bei einer einzelnen Bereitstellung veröffentlichen. Hier erfahren sie mehr über: [Erstellen und Bereitstellen von Azure-Ressourcengruppen über Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="pre-requisites"></a>Erforderliche Komponenten

Um anzufangen virtueller Computer Maßstab Datensätze in Visual Studio bereitstellen, benötigen Sie Folgendes:

- Visual Studio-2013 oder 2015
- Azure SDK 2.7, 2,8 oder 2.9

Hinweis: Diese Anleitung setzt voraus, dass Sie Visual Studio 2015 mit [Azure SDK 2,8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)verwenden.

## <a name="creating-a-project"></a>Erstellen eines Projekts

1. Erstellen eines neuen Projekts in Visual Studio 2015 durch Auswahl **Datei | Neue | Project**.

    ![Neue Datei][file_new]

2. Wählen Sie unter **Visual c# | Cloud**, wählen Sie **Azure Ressourcenmanager** zum Erstellen eines Projekts für die Bereitstellung von einer Cloud-Vorlage aus.

    ![Erstellen von Project][create_project]

3.  Wählen Sie aus der Liste der Vorlagen Linux oder Windows virtuellen Computern Skalieren festlegen Vorlage aus.

    ![Wählen Sie die Vorlage][select_Template]

4. Nachdem das Projekt erstellt wird wird PowerShell-Bereitstellungsskripts, einer Azure Ressourcenmanager Vorlage und eine Parameterdatei für den virtuellen Computern Skalierung festlegen angezeigt.

    ![Lösung-Explorer][solution_explorer]

## <a name="customize-your-project"></a>Anpassen des Projekts

Jetzt können Sie die Vorlage, um ihn nach Ihrer Anwendung wünschen, z. B. virtueller Computer Erweiterungseigenschaften hinzufügen oder Bearbeiten von Regeln für den Lastenausgleich anpassen bearbeiten. Standardmäßig werden die Skalierung festlegen Vorlagen so konfiguriert, dass die Erweiterung AzureDiagnostics bereitstellen, die auf einfache Weise automatisch skalieren Regeln hinzugefügt werden können. Es bereitstellt auch ein Lastenausgleich mit einer öffentlichen IP-Adresse, mit eingehenden NAT-Regeln, mit denen Sie eine Verbindung mit der Instanzen virtueller Computer mit SSH (Linux) oder RDP (Windows) – der Bereich der front-End-Anschluss mit 50000, beginnt, konfiguriert d.h. bei Linux, wenn Sie SSH Port 50000 für die öffentliche IP-Adresse (oder den Domänennamen) Sie weitergeleitet werden an Port 22 des den ersten virtuellen Computer in der Skalierung festlegen. Herstellen einer Verbindung mit Port 50001 wird an den zweiten virtuellen Computer Port 22 usw. weitergeleitet werden.

 Eine gute Möglichkeit zum Bearbeiten Ihrer Vorlagen mit Visual Studio besteht darin, JSON Gliederung verwenden, um den Parameter, Variablen und Ressourcen zu organisieren. Verständnis des Schemas können Visual Studio weisen Sie Fehler in der Vorlage vor der Bereitstellung.

![JSON-Explorer][json_explorer]

## <a name="deploy-the-project"></a>Bereitstellen des Projekts

6. Stellen Sie die Cloud-Vorlage in Azure zum Erstellen der Ressource virtueller Computer Skalierung festlegen. Klicken Sie mit der rechten Maustaste auf den Projektknoten, und wählen Sie **Bereitstellen | Neue Bereitstellung**.

    ![Bereitstellen der Vorlage][5deploy_Template]

7. Wählen Sie im Dialogfeld "Bereitstellen zu Ressourcengruppe" Ihr Abonnement.

    ![Bereitstellen der Vorlage][6deploy_Template]

8. Von hier aus können Sie auch eine neue Azure Ressourcengruppe für die Bereitstellung Ihrer Vorlage zu erstellen.

    ![Neue Ressourcengruppe][new_resource]

9. Als nächstes wählen Sie die Schaltfläche **Bearbeiten Parameter** Parameter eingeben, die zu Ihrer Vorlage, bestimmte Werte wie der Benutzername zu übergebenden und das Kennwort für das Betriebssystem sind erforderlich, um die Bereitstellung zu erstellen. Wenn Sie keine PowerShell-Tools für Visual Studio installiert haben, es wird empfohlen, aktivieren Sie "Speichern von Kennwörtern" akzeptieren, um eine ausgeblendete PowerShell Befehlszeile vermeiden, oder verwenden Sie die [Keyvault unterstützen](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).

    ![Parameter bearbeiten][edit_parameters]

10. Klicken Sie jetzt auf **Bereitstellen**. Im **Ausgabefenster** zeigt den Fortschritt der Bereitstellung. Beachten Sie, dass das Ausführen der Aktion ist des Skripts **AzureResourceGroup.ps1 bereitstellen** .

    ![Ausgabefenster][output_window]

## <a name="exploring-your-vm-scale-set"></a>Untersuchen der virtuellen Computer Maßstab festlegen

Nachdem die Bereitstellung abgeschlossen ist, können Sie den neuen virtuellen Computer Skalierung festlegen in der Visual Studio- **Cloud-Explorer** (Aktualisieren der Liste) anzeigen. Cloud-Explorer können Sie die Azure Ressourcen in Visual Studio bei der Entwicklung von Applications verwalten. Sie können auch in der [Azure-Portal](https://portal.azure.com) und [Azure-Explorers](https://resources.azure.com/)virtueller Computer Skalierung festlegen anzeigen.

![Cloud-Explorer][cloud_explorer]

 Im Portal bietet die beste Methode zum visuell verwalten Ihre Azure Infrastruktur mit einem Webbrowser während Azure Ressource Explorer eine einfache Möglichkeit zum Explorer bietet und Debuggen Azure Ressourcen, die einem Fenster in der Instanzansicht"" zugewiesen und auch einblenden PowerShell-Befehle für die Ressourcen, deren Eigenschaften, denen Sie ansehen. Während virtueller Computer Maßstab Datensätze in der Vorschau sind, wird der Ressource Explorer die meisten Details für Ihre virtuellen Computer Maßstab Mengen angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Sobald Sie erfolgreich virtueller Computer Maßstab Datensätze über Visual Studio bereitgestellt haben, können Sie weiteren Ihres Projekts entsprechend Ihren Anforderungen Anwendung anpassen. Einrichten beispielsweise automatisch skalieren ein, nach dem Hinzufügen einer Ressource Einblicken, Infrastruktur Ihrer Vorlage wie eigenständigen virtuellen Computern hinzufügen oder Bereitstellen von Applications Erweiterung benutzerdefinierter Skripts verwenden. Eine gute Quelle für Beispielvorlagen finden Sie im [Schnellstart-Vorlagen Azure](https://github.com/Azure/azure-quickstart-templates) GitHub Repository (Suche nach "Vmss").

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png
