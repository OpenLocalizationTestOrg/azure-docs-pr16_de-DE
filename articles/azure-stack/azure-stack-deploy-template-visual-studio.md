<properties
    pageTitle="Bereitstellen von Vorlagen mit Visual Studio in Azure Stapel | Microsoft Azure"
    description="Informationen Sie zum Bereitstellen von Vorlagen mit Visual Studio in Azure Stapel."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Bereitstellen von Vorlagen in Azure Stapel mit Visual Studio

Verwenden Sie Visual Studio für die Bereitstellung von Azure Ressourcenmanager Vorlagen auf die Azure Stapel Prüfung des Konzepts ist.

Ressourcenmanager Vorlagen bereitstellen und alle Ressourcen für eine Anwendung in einem einzigen, koordinierte Vorgang bereitstellen.

1.  Öffnen Sie Visual Studio 2015 Update 1 ein.

2.  Klicken Sie auf **Datei**, klicken Sie auf **neu**, und klicken Sie im Dialogfeld **Neues Projekt** auf **Azure Ressourcengruppe**.

3.  Geben Sie einen **Namen** für das neue Projekt ein, und klicken Sie dann auf **OK**.

4.  Klicken Sie im Dialogfeld **Azure-Vorlage auswählen** klicken Sie auf **Windows-Computer**, und klicken Sie dann auf **OK**.

  In Ihrem neuen Projekt können Sie eine Liste der verfügbaren Vorlagen anzeigen, indem Sie den Knoten **Vorlagen** in **Lösung Explorer** -Fenster erweitern.

5.  Klicken Sie im **Explorer Lösung** mit der rechten Maustaste in des Namens Ihres Projekts, klicken Sie auf **Bereitstellen**, und klicken Sie auf **Neue Bereitstellung**.

6.  Wählen Sie im Dialogfeld **Bereitstellen Ressourcengruppe** in der Dropdownliste **Abonnements** Ihres Abonnements Microsoft Azure Stapel.

7.  Klicken Sie in der Liste **Ressourcengruppe** wählen Sie eine vorhandene Ressourcengruppe aus, oder Erstellen eines neuen Kontos.

8.  Wählen Sie in der Liste **Speicherort der Ressource Gruppe** einen Speicherort aus, und klicken Sie dann auf **Bereitstellen**.

9.  Geben Sie im Dialogfeld **Parameter bearbeiten** Werte für den Parameter (die Vorlage variieren), und klicken Sie dann auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Vorlagen für die Befehlszeile](azure-stack-deploy-template-command-line.md)
