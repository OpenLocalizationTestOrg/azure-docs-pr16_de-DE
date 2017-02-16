<properties
    pageTitle="Vergleich von benutzerdefinierten Bilder und Formeln in Kursen DevTest | Microsoft Azure"
    description="Erfahren Sie die Unterschiede zwischen benutzerdefinierten Bilder und Formeln als virtueller Computer basiert, damit Sie entscheiden können, welche eine Ihrer Umgebung am besten geeignet ist."
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

# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a>Vergleich von benutzerdefinierten Bilder und Formeln in DevTest Einheiten

## <a name="overview"></a>(Übersicht)
Sowohl [benutzerdefinierte Bilder](./devtest-lab-create-template.md) und [Formeln](./devtest-lab-manage-formulas.md) können als Basis für die [neue virtuellen Computern erstellt](./devtest-lab-add-vm-with-artifacts.md)verwendet werden. Jedoch ist der wichtigste Unterschied zwischen benutzerdefinierten Bilder und Formeln an, dass ein benutzerdefiniertes Bild einfach ein Bild auf eine virtuelle Festplatte basiert ist, während eine Formel ein Bild basierend auf einer virtuellen Festplatte *im Erweiterung* vorkonfiguriert Einstellungen - wie virtueller Speicher, virtuelles Netzwerk und Subnetz, Elemente und usw. ist. Diese vorkonfigurierten Einstellungen werden mit Standardwerten eingerichtet, die zum Zeitpunkt der Erstellung virtueller Computer überschrieben werden können. In diesem Artikel werden einige der (Experten) vor- und Nachteile (aufzulisten) bei der Verwendung von benutzerdefinierten Bilder im Vergleich mit Formeln erläutert.
 
## <a name="custom-image-pros-and-cons"></a>Benutzerdefiniertes Bild vor- und Nachteile
Benutzerdefinierte Bilder Bereitstellen einer eine statische, unveränderliche Möglichkeit zum Erstellen von virtuellen Computern aus eine gewünschte Umgebung. 

**Experten**
- Bereitstellen von virtuellen Computer aus ein benutzerdefiniertes Bild ist schnell wie nichts geändert wird, nachdem Sie der virtuellen Computer aus dem Bild erstellt wird, ist. Kurzum, gibt es keine Einstellungen zum Anwenden von benutzerdefinierten Bilds ungeändert einfach ein Bild ohne Einstellungen. 
- Virtuellen Computern, die ein einzelnes benutzerdefiniertes Bild Dokumentvorlagen sind identisch.

**Aufzulisten**
- Wenn Sie einen Aspekt des benutzerdefinierten Bildes aktualisieren müssen, muss das Bild neu erstellt werden.  

## <a name="formula-pros-and-cons"></a>Formel vor- und Nachteile
Formeln bieten eine dynamische Möglichkeit zum Erstellen von virtuellen Computern aus die gewünschten Konfiguration-Einstellungen.

**Experten**
- Im laufenden Betrieb über Elemente können Änderungen in der Umgebung erfasst werden. Beispielsweise, wenn Sie einen virtuellen Computer mit den neuesten Bits aus der Verkaufspipeline veröffentlichte Version installiert oder den neuesten Code aus Ihrem Repository eintragen, können Sie einfach ein angeben, die die neuesten Bits bereitstellt oder trägt den aktuellen Code in die Formel zusammen mit einer Basis ' Ziel '. Wenn diese Formel zum Erstellen von virtuellen Computern verwendet wird, werden der neueste Bits-Code auf dem virtuellen Computer bereitgestellt/eingetragen. 
- Formeln können Standardeinstellungen definieren, die benutzerdefinierte Bilder – beispielsweise virtueller Computer Größen und virtuelles Netzwerk-Einstellungen nicht möglich. 
- Die Einstellungen in einer Formel gespeichert werden als Standardwerte angezeigt, aber Sie können geändert werden, wenn Sie der virtuellen Computer erstellt wurde. 

**Aufzulisten**
- Erstellen eines virtuellen Computers aus der Formel kann mehr Zeit als das Erstellen eines virtuellen Computers aus ein benutzerdefiniertes Bild optimieren.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwandte von Blogbeiträgen

- [Benutzerdefinierte Bilder oder Formeln?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

