<properties 
    pageTitle="Erstellen Sie benutzerdefinierte Elemente für Ihre DevTest Labs virtuellen Computer | Microsoft Azure"
    description="Informationen Sie zum Erstellen Ihrer eigenen Elemente für die Verwendung mit DevTest Labs"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="tarcher"/>

#<a name="create-custom-artifacts-for-your-devtest-labs-vm"></a>Erstellen Sie benutzerdefinierte Elemente für Ihre DevTest Labs virtuellen Computer

> [AZURE.VIDEO how-to-author-custom-artifacts] 

## <a name="overview"></a>(Übersicht)
**Elemente** werden zum Bereitstellen und konfigurieren die Anwendung, nachdem ein virtueller Computer bereitgestellt wird. Ein Element besteht aus einer Datei Element und andere Skriptdateien, die in einem Ordner in einem Git Repository gespeichert werden. Element Definitionsdateien bestehen aus JSON und Ausdrücke, die Sie verwenden können, um anzugeben, was Sie auf einem virtuellen Computer installieren möchten. Beispielsweise können Sie den Namen der Element, Befehl ausführen und Parameter, die zur Verfügung gestellt werden, wenn der Befehl ausgeführt wird, definieren. Sie können zu anderen Skriptdateien innerhalb der Element-Definition-Datei anhand des Namens verweisen.

##<a name="artifact-definition-file-format"></a>Element Definition-Dateiformat
Das folgende Beispiel zeigt die Abschnitte, die die grundlegende Struktur einer Datei Definition zusammensetzt.

    {
      "$schema": "https://raw.githubusercontent.com/Azure/azure-devtestlab/master/schemas/2015-01-01/dtlArtifacts.json",
      "title": "",
      "description": "",
      "iconUri": "",
      "targetOsType": "",
      "parameters": {
        "<parameterName>": {
          "type": "",
          "displayName": "",
          "description": ""
        }
      },
      "runCommand": {
        "commandToExecute": ""
      }
    }

| Name des Elements | Erforderlich? | Beschreibung
| ------------ | --------- | -----------
| $schema      | Nein        | Speicherort der Schemadatei JSON, mit dem die Gültigkeit der Definitionsdatei testen kann.
| Titel        | Ja       | Name der das Element in den Kurs angezeigt.
| Beschreibung  | Ja       | Beschreibung der das Element in den Kurs angezeigt.
| iconUri      | Nein        | URI des Symbols in den Kurs angezeigt.
| targetOsType | Ja       | Das Betriebssystem von den virtuellen Computer, auf Element installiert wird. Unterstützte Optionen sind: Windows und Linux.
| Parameter   | Nein        | Werte, die bereitgestellt werden, wenn der Befehl der Element-Installation auf einem Computer ausgeführt wird. Auf diese Weise in Ihre Element anpassen können.
| AusführenBefehl   | Ja       | Element installieren Befehl, der für einen virtuellen Computer ausgeführt wird.

###<a name="artifact-parameters"></a>Element-Parameter

Im Abschnitt Parameter der Datei geben Sie die Werte, die ein Benutzer eingeben kann, bei der Installation ein. Sie können diese Werte im Element installieren Befehl verweisen.

Sie definieren Parameter wird die folgende Struktur.

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| Name des Elements | Erforderlich? | Beschreibung
| ------------ | --------- | -----------
| Typ         | Ja       | Typ des Parameterwerts. Finden Sie in der Liste unter für die zulässigen Typen:
| DisplayName Ja       | Der Name des Parameters an, die ein Benutzer in der Kurs angezeigt wird.
| Beschreibung  | Ja       | Beschreibung des Parameters an, die in den Kurs angezeigt wird.

Zulässigen Typen sind verfügbar:

- Zeichenfolge – eine gültige JSON-Zeichenfolge
- Int – eine gültige JSON ganze Zahl
- boolesche Typen – jeder gültige JSON Boolean
- Matrix – eine gültige JSON-Matrix

##<a name="artifact-expressions-and-functions"></a>Element-Ausdrücken und Funktionen

Ausdruck können und Befehl zum Installieren von Funktionen, um das Element zu erstellen.
Ausdrücke in Klammern gesetzt werden ([und]), und Sie werden ausgewertet, wenn das Element installiert ist. Ausdrücke können an beliebiger Stelle in einer JSON-Zeichenfolgenwert und immer eine andere JSON-Wert zurückgegeben. Wenn Sie eine literale Zeichenfolge verwenden, die mit einer Klammer beginnt müssen [, verwenden Sie zwei Klammern [[.
In der Regel verwenden Sie Ausdrücke mit Funktionen Erstellen eines Werts ein. Wie werden in JavaScript Funktion Anrufe als functionName(arg1,arg2,arg3) formatiert

Die folgende Liste enthält häufige Funktionen.

- Parameters(Parametername) - gibt einen Parameterwert, der bereitgestellt wird, wenn der Befehl Element ausgeführt wird.
- Verketten (arg1, arg2, arg3,...) – werden mehrere Zeichenfolgenwerte kombiniert. Diese Funktion kann eine beliebige Anzahl von Argumenten dauern.

Im folgenden Beispiel wird die Verwendung von Funktionen und Ausdruck zum Erstellen eines Werts.

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -File startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

##<a name="create-a-custom-artifact"></a>Erstellen eines benutzerdefinierten Element

Erstellen Sie Ihre benutzerdefinierten Element, indem Sie folgende Schritte:

1. Installieren von einem JSON-Editor – Sie benötigen einen JSON-Editor für die Arbeit mit Element Definitionsdateien. Es empfiehlt sich, mithilfe von [Visual Studio-Code](https://code.visualstudio.com/), der für Windows, Linux und OS X verfügbar ist.

1. Abrufen einer Stichprobe artifactfile.json: Auschecken Azure DevTest Labs Team bei unseren [GitHub Repository](https://github.com/Azure/azure-devtestlab) , in dem wir eine umfangreiche Bibliothek von Elemente erstellt haben, die Ihnen helfen, erstellte Elemente erstellen Ihrer eigenen Elemente. Herunterladen einer Datei Element, und nehmen Sie Änderungen an Ihrer eigenen Elemente erstellt werden soll.

1. Stellen Sie die Verwendung der IntelliSense - Nutzung IntelliSense gültigen Elemente angezeigt, die zum Erstellen einer Datei Element verwendet werden können. Sie können auch die verschiedenen Optionen für Werte eines Elements anzeigen. Beispielsweise IntelliSense zeigen, werden zwei Optionen von Windows oder Linux Wenn das **TargetOsType** Element bearbeiten.

1. Speichern Sie das Element in einem Repository git
    1. Erstellen Sie ein eigenes Verzeichnis für jedes Element, in dem der Name des Verzeichnisses Artefaktnamen identisch ist.
    1. Speichern Sie die Element Definition-Datei (artifactfile.json) im Verzeichnis, die, das Sie erstellt haben.
    1. Speichern Sie die Skripts, die aus dem Element installieren Befehl verwiesen werden.

    Hier ist ein Beispiel für einen Ordner "Elemente" kann so aussehen:

    ![Element Git Repo Beispiel](./media/devtest-lab-artifact-author/git-repo.png)

1. Fügen Sie das Elemente Repository mit den Kurs - finden Sie im Artikel, [Git Element Repository mit einem Kurs hinzufügen](devtest-lab-add-artifact-repo.md).

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwandte von Blogbeiträgen
- [Behandeln von Problemen mit weiß nicht Elemente in AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs)
- [Teilnehmen an eines virtuellen Computers zu vorhandenen Active Directory-Domäne in Azure testen Entwicklungslabor Cloud-Vorlage verwenden](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie [mit einem Kurs Repository Git Element hinzufügen](devtest-lab-add-artifact-repo.md).
