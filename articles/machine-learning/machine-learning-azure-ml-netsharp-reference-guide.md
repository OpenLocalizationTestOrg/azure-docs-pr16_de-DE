<properties 
    pageTitle="Leitfaden für die Netz-neuronale Netzwerke Spezifikationssprache | Microsoft Azure" 
    description="Syntax für die Netz-neuronale Netzwerke Spezifikationssprache zusammen mit Beispiele zum Erstellen eines benutzerdefinierten Trainingsfällen Modells in Microsoft Azure ML Netz verwenden#" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jeannt" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="jeannt"/>



# <a name="guide-to-net-neural-network-specification-language-for-azure-machine-learning"></a>Leitfaden Sie für Netz-Trainingsfällen Spezifikationssprache für Azure Computer-Schulung

## <a name="overview"></a>(Übersicht)
Netz Nr. ist eine Sprache, die entwickelt von Microsoft, das zum Definieren von Trainingsfällen Architekturen für Trainingsfällen Module in Microsoft Azure maschinellen Learning verwendet wird. In diesem Artikel lernen Sie:  

-   Grundlegende Konzepte im Zusammenhang mit neuronale Netzwerke
-   Trainingsfällen Anforderungen und so die primären Komponenten definieren
-   Die Syntax und Schlüsselwörter der Netz-Spezifikationssprache
-   Beispiele für benutzerdefinierte neuronale Netzwerke erstellt Netz# 
    
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

## <a name="neural-network-basics"></a>Grundlagen von Trainingsfällen
Eine Struktur Trainingsfällen besteht aus ***Knoten*** , die in den ***Layer***, und gewichtete ***Verbindungen*** (oder ***Kanten***) angeordnet werden zwischen den Knoten. Die Verbindungen gerichtete sind und jede Verbindung verfügt über eine ***Quelle*** und einem ***Ziel*** -Knoten.  

Jeden ***trainable Layer*** (ein ausgeblendetes oder einem Layer Ausgabe) verfügt über eine oder mehrere ***Verbindung bündeln***. Bündeln Verbindung besteht aus einer Quelle Layer und Angaben zu den Verbindungen aus, die diesem Layer Quelle. Alle Verbindungen in angegebenen bündeln freigeben den gleichen ***Quelle Layer*** und den gleichen ***Ziel Layer***. In Netz # gilt als Verbindung bündeln als das Paket des Ziel Layer zugehörigen.  
 
Netz Nr. unterstützt verschiedene Arten von Verbindung bündeln, dem Sie die Möglichkeit Eingaben anpassen können ausgeblendete Ebenen zugeordnet ist und die Ausgaben zugeordnet sind.   

Die Standard- oder standard-Paket ist eine **vollständige Paket**, in der einzelnen Knoten in der Quellebene mit jeder Knoten in der Zielebene verbunden ist.  

Darüber hinaus unterstützt Netz Nr., die folgenden vier Arten von erweiterte Verbindung Pakete:  

-   **Bündeln gefiltert**. Der Benutzer kann ein Prädikat mit den jeweiligen Speicherorten von der Quelle Layer und das Ziel Layer Knoten definieren. Knoten verbunden sind, wenn das Prädikat als WAHR ausgewertet wird.
-   **Convolutional Pakete**. Der Benutzer kann kleine Themenwelten Knoten in der Quellebene definieren. Jeder Knoten in der Zielebene ist mit einer Höhe von etwa Knoten in der Quellebene verbunden.
-   **Verbindungspooling bündeln** und **Antwort Normalisierung Pakete**. Diese ähneln convolutional bündeln darin, dass der Benutzer kleine Themenwelten Knoten in der Quellebene definiert werden. Der Unterschied besteht darin, dass die-Stärken der Ränder in dieser Pakete nicht trainable sind. In diesem Fall wird eine vordefinierte Funktion auf Knoten Quellwerte zum Bestimmen des Werts der Ziel-Knoten angewendet.  

Mit Netz Nr. definieren die Struktur der einen Trainingsfällen ermöglicht die, definieren komplexe Datenstrukturen, z. B. tief neuronale Netzwerke oder Convolutions von beliebigen Dimensionen, der bekanntermaßen Learning auf Daten, wie etwa Bild-, Audio- oder Videodateien zu verbessern.  

## <a name="supported-customizations"></a>Unterstützte Anpassungen
Die Architektur von Trainingsfällen Modelle, die Sie in Azure maschinellen Learning erstellen kann Arbeiten mit Netz Nr. angepasst werden. Sie können:  

-   Ausgeblendete Ebenen erstellen und die Anzahl der Knoten in jede Ebene steuern.
-   Geben Sie an, wie Ebenen sind miteinander verbunden sein.
-   Definieren Sie spezielle Connectivity Datenstrukturen, z. B. Convolutions und Stärke bündeln Freigabe ein.
-   Geben Sie die von anderen Aktivierungsfunktionen an.  

Details der Syntax Sprache Spezifikation finden Sie unter [Spezifikation Struktur](#Structure-specifications).  
 
Beispiele für einige allgemeine Computer learning Aufgaben aus Simplex und komplexe neuronale Netzwerke definieren finden Sie unter [Beispiele](#Examples-of-Net#-usage).  

## <a name="general-requirements"></a>Allgemeine Vorschriften
-   Es muss genau ein Ausgabe-Schicht, mindestens eine Eingabe Ebene und 0 (null) oder mehrerer ausgeblendete Folien. 
-   Jede Ebene hat eine feste Anzahl von Knoten, im Prinzip in ein rechteckiges Array von beliebigen Dimensionen angeordnet. 
-   Eingabe Ebenen haben keine zugeordneten ausgebildeten Parameter und den Punkt, bei dem Instanzdaten im Netzwerk eingibt, darstellen. 
-   Trainable Ebenen (ein- und ausgeblendeten Folien) verfügen über zugeordnete ausgebildeten Parameter,-Stärken und Biases genannt. 
-   Die Quell- und Zielfelder Knoten muss in separaten Ebenen. 
-   Verbindungen muss acyclische; Kurzum, kann keine Kette von führenden wieder zum ursprünglichen Knoten Verbindungen vorhanden sein.
-   Die Ausgabe Ebene eines Layers Quelle der Verbindung bündeln nicht möglich.  

## <a name="structure-specifications"></a>Spezifikationen Struktur
Importspezifikation Struktur Trainingsfällen besteht aus drei Abschnitte: die **Deklaration von Konstanten**, die **Deklaration Layer**, die **Verbindungsdeklaration**. Es gibt auch ein Abschnitt optional **Deklaration freigeben** . In den Abschnitten können in einer beliebigen Reihenfolge angegeben werden muss.  

## <a name="constant-declaration"></a>Deklaration von Konstanten 
Eine Konstante Deklaration ist optional. Es bietet die Möglichkeit, die an anderer Stelle in der Definition Trainingsfällen verwendeten Werte definieren. Die Deklaration-Anweisung besteht aus einem Identifier gefolgt von einem Gleichheitszeichen und ein Werteausdruck.   

Die folgende Anweisung wird beispielsweise eine Konstante **X**definiert:  


    Const X = 28;  

Wenn Sie gleichzeitig zwei oder mehr Konstanten definieren, setzen Sie die Namen von Bezeichnern und Werte in geschweifte Klammern und trennen Sie dann durch Semikolons. Beispiel:  

    Const { X = 28; Y = 4; }  

Die rechte Seite des jeder Zuordnung Ausdruck kann eine ganze Zahl, eine reelle Zahl, einen booleschen Wert (True oder False) oder einen mathematischen Ausdruck sein. Beispiel:  

    Const { X = 17 * 2; Y = true; }  

## <a name="layer-declaration"></a>Schicht-Deklaration
Die Deklaration Layer ist erforderlich. Die Größe und die Quelle für den Layer, einschließlich der Verbindung bündeln und Attribute definiert. Die Deklaration-Anweisung beginnt mit dem Namen der Ebene (Eingabemethoden, ausgeblendet oder ausgeben), gefolgt von der Maße der Ebene (eines Tupels der positive ganze Zahlen). Beispiel:  

    input Data auto;
    hidden Hidden[5,20] from Data all;
    output Result[2] from Hidden all;  

-   Das Produkt der Dimensionen ist die Anzahl der Knoten in der Ebene an. In diesem Beispiel gibt es zwei Dimensionen [5,20], was bedeutet, dass in den Layer 100 Knoten vorhanden sind.
-   Die Ebenen in einer beliebigen Reihenfolge deklariert werden können: Wenn mehrere Eingabewerte Layer definiert ist, muss die Reihenfolge, in der sie deklariert sind, die Reihenfolge der Features der eingegebenen Daten übereinstimmen.  


Festlegen, dass die Anzahl der Knoten in einer Ebene automatisch bestimmt werden soll, verwenden Sie das Schlüsselwort **automatisch** aus. **Auto** -Schlüsselwort weist verschiedene Effekte, je nach den Layer:  

-   In der Deklaration eines Eingabewerte Layer ist die Anzahl der Knoten die Anzahl der Features der eingegebenen Daten.
-   In der Deklaration eines ausgeblendeten Layer ist die Anzahl der Knoten die Nummer, die für die **Anzahl der ausgeblendeten Knoten**durch den Parameterwert angegeben ist. 
-   In der Deklaration eines Ausgabe Layer ist die Anzahl der Knoten für zwei-Klasse Einstufung, 1 für Regression und die Anzahl der Ausgabeknoten für multiclass Klassifizierung gleich 2.   

Die folgende Netzwerk Definition kann beispielsweise die Größe aller Ebenen automatisch bestimmt werden soll:  

    input Data auto;
    hidden Hidden auto from Data all;
    output Result auto from Hidden all;  


Eine Ebene Deklaration für eine trainable Ebene (die ausgeblendeten oder Ausgabe Ebenen) kann optional die Ausgabe-Funktion (auch als eine Aktivierungsfunktion bezeichnet), die standardmäßig verwendet einschließen **Sigmoidfunktion** für Klassifizierung-Modelle und **lineare** Regression Modelle. (Auch wenn Sie die Standardeinstellung verwenden, können Sie explizit die Aktivierungsfunktion angeben gegebenenfalls aus Gründen der Übersichtlichkeit.)

Die folgenden Ausgabefunktionen werden unterstützt:  

-   sigmoide
-   lineare
-   Softmax
-   rlinear
-   Quadrat
-   Wurzel
-   srlinear
-   Abs
-   TANHYP 
-   brlinear  

Die folgende Deklaration wird beispielsweise die **Softmax** -Funktion verwendet:  

    output Result [100] softmax from Hidden all;  

## <a name="connection-declaration"></a>Verbindungsdeklaration
Unmittelbar nach dem Definieren des trainable Layers, deklarieren Sie Verbindungen zwischen den Layer aus, die Sie definiert haben. Die Verbindung Paket Deklaration beginnt mit dem Schlüsselwort **aus**, gefolgt vom Namen für das Paket der Quell-als auch die Art der Verbindung Paket zu erstellen.   

Derzeit gibt fünf Arten von Verbindung bündeln unterstützt:  

-   **Vollständige** Pakete, von dem Schlüsselwort **Alle** angegeben
-   **Gefiltert** bündeln, Schlüsselwort **, wo**, gefolgt von einer Prädikatsausdruck erkennbar
-   **Convolutional** bündeln, das Schlüsselwort **convolve**, gefolgt von den Attributen Faltung erkennbar
-   **Pooling** bündeln, die Schlüsselwörter **max Pool** oder **Mittelwert Ressourcenpool** erkennbar
-   **Antwort Normalisierung** bündeln, von dem Schlüsselwort **Antwort Regel** angegeben      

## <a name="full-bundles"></a>Vollständige bündeln  

Vollständige Verbindung bündeln bietet eine Verbindung von jedem Knoten in der Quellebene an jeden Knoten in der Zielebene an. Dies ist die standardmäßige Netzwerkverbindungstyp.  

## <a name="filtered-bundles"></a>Gefilterte Pakete
Importspezifikation gefilterten Verbindung-Paket enthält ein stark Prädikat, ausgedrückt Syntax, wie ein C#-Lambda-Ausdruck. Im folgende Beispiel werden zwei gefilterten Pakete definiert:  

    input Pixels [10, 20];
    hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;  

-   In der Prädikate für _ByRow_ **s** ist, einen Parameter, einen Index in das rechteckige Array der Knoten der Eingabewerte Ebene, _Pixel_darstellt und **d** ist, einen Parameter, einen Index in der Matrix von Knoten für den ausgeblendeten Layer, _ByRow_darstellt. Der Typ der sowohl **s** und **d** ist ein Tupel von ganzen Länge zwei. Im Prinzip **s** Bereiche über alle Paare von ganzen Zahlen mit _0 < = s [0] < 10_ und _0 < = s-[1] < 20_, und **d** Bereiche über alle Paare von ganzen Zahlen, mit _0 < = d [0] < 10_ und _0 < = d[1] < 12_. 
-   Klicken Sie auf der rechten Seite des Ausdrucks Prädikat ist es eine Bedingung. In diesem Beispiel für jeden Wert **s** und **d** so, dass die Bedingung zutrifft, befindet sich eine Kante aus der Quelle Layer Knoten zum Ziel Layer Knoten. Daher wird diese Filter-Ausdruck angezeigt, dass das Paket eine Verbindung aus dem Knoten durch **s** , um den Knoten definiert durch **d** in allen Fällen enthält, wo s [0] d [0] gleich ist, definiert.  

Optional können Sie eine Reihe von-Stärken für gefilterten bündeln angeben. Der Wert für das Attribut **-Stärken** muss eines Tupels unverankerte Punkt Werte mit einer Länge, die die Anzahl der Verbindungen, die definiert werden, indem Sie das Paket übereinstimmt. Standardmäßig werden-Stärken zufällig generiert.  

Gewichtungswerte werden vom Ziel Knotenindex gruppiert. D. h., sind der erste Zielknoten an K Quellknoten angeschlossen ist, die ersten _K_ Elemente des Tupels **-Stärken** der-Stärken für den ersten Zielknoten in der Reihenfolge der Datenquellen Index. Dasselbe gilt für die verbleibenden Zielknoten.  

Es ist möglich,-Stärken direkt als Konstante Werte anzugeben. Wenn Sie die-Stärken zuvor gespeichert haben, können Sie diese beispielsweise als Konstanten unter Verwendung der Syntax angeben:

    const Weights_1 = [0.0188045055, 0.130500451, ...]


## <a name="convolutional-bundles"></a>Convolutional bündeln
Wenn die Schulungsdaten eine einheitlichen Struktur hat, werden Daten auf hoher Ebene Features erlernen häufig convolutional Verbindungen verwendet. Beispielsweise können in Bild-, Audio- oder video-Daten, Anzahl der örtlich oder zeitlich Dimensionen relativ einheitlichen sein.  

Convolutional bündeln einsetzen rechteckige **Kernels** , die durch die Dimensionen verschoben werden. Im Wesentlichen definiert jeder Kernel eine Reihe von-Stärken angewendet, die in der lokalen Themenwelten, als **Kernel Applikationen**bezeichnet. Jede Anwendung Kernel entspricht einem Knoten in der Quellebene, die als **zentralen Knoten**bezeichnet wird. Die von einem Kernel-Stärken werden von vielen Verbindungen gemeinsam verwendet. In ein convolutional Paket jede Kernel rechteckige ist und alle Kernel-Programme sind die gleiche Größe.  

Convolutional bündeln unterstützen die folgenden Attribute:

**InputShape** definiert die Anzahl der Dimensionen der Quellebene für die Zwecke dieses convolutional Paket ein. Der Wert muss ein Tupel von positive Zahlen sein. Das Produkt der ganzen Zahlen muss gleich der Anzahl der Knoten in der Quellebene, aber andernfalls es muss nicht die Anzahl der Dimensionen für den Layer Quelle deklariert entsprechen. Die Länge der dieses Tupel wird **für das Paket convolutional Stelligkeitswert** . (In der Regel bezieht sich Stelligkeit auf die Anzahl der Argumente oder Operanden, die eine Funktion ausführen können.)  

Um die Form und Speicherorte des Kerns zu definieren, verwenden Sie die Attribute **KernelShape**, **Schritt**, **Textabstand**, **LowerPad**und **UpperPad**aus:   

-   **KernelShape**: (erforderlich) definiert die Anzahl der Dimensionen der einzelnen Kernel für das Paket convolutional. Der Wert muss ein Tupel von positive ganze Zahlen mit einer Länge sein, die der Stelligkeit für das Paket entspricht. Jeder Bestandteil dieses Tupel darf nicht größer als die entsprechenden Komponenten von **InputShape**sein. 
-   **Schritt**: (optional) definiert die verschiebbaren Schritt Größen der Faltung (einen Schrittgröße für jede Dimension), die den Abstand zwischen den zentralen Knoten ist. Der Wert muss ein Tupel von positive ganze Zahlen mit einer Länge sein, die der Stelligkeit für das Paket ist. Jeder Bestandteil dieses Tupel darf nicht größer als die entsprechenden Komponenten von **KernelShape**sein. Der Standardwert ist ein Tupel mit allen Komponenten gleich eins. 
-   **Freigabe**: (optional) die Stärke für jede Dimension, der die Faltung Freigabe definiert. Der Wert kann ein einzelner Wert des Typs Boolean oder eines Tupels mit booleschen Werten mit einer Länge, die gleich der Stelligkeit des das Paket sein. Ein einzelner Wert des Typs Boolean erweiterten benutzerspezifisch eines Tupels der richtigen Länge mit allen Komponenten der angegebenen Wert gleich. Der Standardwert ist ein Tupel, aller WAHR Werte besteht. 
-   **MapCount**: (optional) die Anzahl der Features für das Paket convolutional maps definiert. Der Wert kann eine einzelne positive ganze Zahl oder eines Tupels der positive ganze Zahlen mit einer Länge, die gleich der Stelligkeit des das Paket sein. Ein einzelnes-Wert benutzerspezifisch eines Tupels der richtigen Länge mit den ersten Komponenten der angegebenen Wert gleich erweitert, und alle verbleibenden Komponenten eine gleich. Der Standardwert ist eine. Die Gesamtzahl der Funktion Karten wird das Produkt Komponenten des Tupels. Die Verarbeitung von diesem Gesamtzahl über die Komponenten bestimmt, wie die Funktion Karte Werte in die Zielknoten gruppiert werden. 
-   **Gewicht**: (optional) die ursprünglichen-Stärken für das Paket definiert. Der Wert muss ein Tupel von beweglich Punktwerte mit einer Länge, die die Anzahl der Kernels ist die Anzahl der-Stärken pro Kernel, wie weiter unten in diesem Artikel definiert sein. Die standardmäßige-Stärken werden zufällig generiert.  

Es gibt zwei Sätze von Eigenschaften, die Abstand, die Eigenschaften gegenseitig steuern:

-   **Textabstand**: (optional) legt fest, ob die Eingabe mit einem **Standardschema für den Abstand**aufgefüllt werden soll. Der Wert kann ein einzelner Wert des Typs Boolean, oder eines Tupels mit booleschen Werten mit einer Länge, die gleich der Stelligkeit des das Paket sind möglich. Ein einzelner Wert des Typs Boolean erweiterten benutzerspezifisch eines Tupels der richtigen Länge mit allen Komponenten der angegebenen Wert gleich. Wenn der Wert für eine Dimension wahr ist, wird die Quelle, sodass die zentralen Knoten des Kerns vor- und Nachnamen in dieser Dimension die ersten und letzten Knoten in dieser Dimension in den Layer Quelle sind logisch in dieser Dimension mit NULL-Werte von Zellen zur Unterstützung von Applications Weitere aufgefüllt. Auf diese Weise die Anzahl der in jeder Dimension "-platzhalterprodukt" Knoten wird automatisch bestimmt, um genau passt _(InputShape [d] – 1) / Schritt [d] + 1_ Kernels in den Layer erhält Quelle. Wenn der Wert für eine Dimension falsch ist, werden die Kernels definiert, so, dass die Anzahl der Knoten auf jeder Seite, die sich links sind gleich (bis zu einem Differenz von 1) befindet. Des Standardwerts für dieses Attribut ist ein Tupel mit allen Komponenten gleich falsch.
-   **UpperPad** und **LowerPad**: (optional) bereitstellen, mehr Kontrolle über die Größe des Abstands zu verwenden. **Wichtige:** Diese Attribute definiert werden können, wenn die oben genannte Eigenschaft **Textabstand** ist ***nicht*** definiert. Die Werte sollten Tupel mit Länge ganzen Zahlen, die der Stelligkeit für das Paket sind. Wenn diese Attribute angegeben sind, werden die oberen und unteren enden jeder Dimension der Eingabewerte Ebene "-platzhalterprodukt" Knoten hinzugefügt. Die Anzahl der Knoten hinzugefügt, die den oberen und unteren enden in jeder Dimension wird durch **LowerPad**[i] und **UpperPad**[i] bestimmt. Um sicherzustellen, dass Kernels nur für Knoten "real" und "-platzhalterprodukt" Knoten nicht entsprechen, müssen die folgenden Bedingungen erfüllt sein:
    -   Jede Komponente der **LowerPad** muss kleiner als KernelShape [d] / 2. 
    -   Jede Komponente der **UpperPad** muss nicht größer als KernelShape [d] / 2. 
    -   Des Standardwerts für diese Attribute ist ein Tupel mit allen Komponenten gleich 0 an. 

Die Einstellung **Textabstand** = WAHR ermöglicht, wie viel Abstand wie benötigt wird, um die "Mitte" des Kernels innerhalb "Real" Eingabemethoden beibehalten möchten. Hiermit ändern Sie die mathematische etwas für die Ausgabegröße computing. Im Allgemeinen wird die Ausgabegröße _D_ als berechnet _D = (I - K) / S + 1_, wo _ich_ die Eingabewerte Größe ist _K_ wird die Größe des Kernel, _S_ ist die Schrittweite und _/_ ist die Division ganzer Zahlen (Runden in Richtung 0). Wenn Sie festlegen, dass UpperPad = [1, 1], die Eingabewerte Größe _ich_ ist effektiv 29, und somit _D = (29-5) / 2 + 1 = 13_. Jedoch, wenn **Textabstand** = WAHR, im Wesentlichen _ich_ ruft Deaktivieren von von _K – 1_; Daher _D = ((28 + 4) – 5) / 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14_. Durch Angeben von Werten für **UpperPad** und **LowerPad** erhalten Sie viel mehr Kontrolle über die Abstand als wenn Sie nur die **Textabstand** festlegen = wahr.

Weitere Informationen zu convolutional Netzwerken und deren Anwendung finden Sie unter folgenden Artikeln:  

-   [http://deeplearning.NET/Tutorial/lenet.HTML](http://deeplearning.net/tutorial/lenet.html )
-   [http://Research.Microsoft.com/pubs/68920/icdar03.PDF](http://research.microsoft.com/pubs/68920/icdar03.pdf) 
-   [http://people.csail.mit.edu/jvb/Papers/cnn_tutorial.PDF](http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf)  

## <a name="pooling-bundles"></a>Verbindungspooling bündeln
Ein **Paket Verbindungspooling** Geometrie ähnliche convolutional Connectivity gilt, aber es vordefinierte Funktionen zum Knoten Quellwerte leiten Sie den Wert für Ziel Knoten verwendet. Daher müssen Pool bündeln kein trainable Zustand (-Stärken oder Biases). Pool bündeln unterstützen die convolutional Attribute mit Ausnahme der **Freigabe**, **MapCount**und **-Stärken**.  

In der Regel, die die Kernels zusammengefasst, indem Sie angrenzende Pool Einheiten überlappen nicht. Schrittweite [d] ist KernelShape [d] in jeder Dimension gleich, die Ebene, die für Ihren Kunden traditionelle lokalen Pool Layer, die häufig in convolutional neuronalen Netzwerken eingesetzt wird. Jeder Zielknoten berechnet das Maximum oder den Mittelwert der zugehörigen Kernel in der Quellebene Aktivitäten.  

Im folgenden Beispiel wird veranschaulicht, Pool bündeln: 

    hidden P1 [5, 12, 12]
      from C1 max pool {
        InputShape  = [ 5, 24, 24];
        KernelShape = [ 1,  2,  2];
        Stride      = [ 1,  2,  2];
      }  

-   Der Stelligkeit für das Paket ist 3 (die Länge des die Tupel **InputShape**, **KernelShape**und **Schritt**). 
-   Ist die Anzahl der Knoten in der Quellebene _5 *24* 24 = 2880_. 
-   Dies ist eine traditionelle lokalen Pool Ebene, da **KernelShape** und **Schritt** gleich sind. 
-   Ist die Anzahl der Knoten in der Zielebene _5 *12* 12 = 1440_.  
    
Weitere Informationen zu Pool Layer finden Sie unter folgenden Artikeln:  

-   [http://www.cs.Toronto.edu/~hinton/absps/imagenet.PDF](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (Im Abschnitt 3.4)
-   [http://cs.Nyu.edu/~koray/Publis/lecun-iscas-10.PDF](http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf) 
-   [http://cs.Nyu.edu/~koray/Publis/jarrett-iccv-09.PDF](http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)
    
## <a name="response-normalization-bundles"></a>Antwort Normalisierung bündeln
**Antwort Normalisierung** ist einem lokalen Normalisierung Farbschema, die zuerst durch Geoffrey Hinton, eingeführt wurde und Al, in das Papier [ImageNet Classiﬁcation mit Tiefe Convolutional neuronalen Netzwerken](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf). Antwort Normalisierung wird verwendet, um die Generalisierung in neuronale Netze zu unterstützen. Wenn eine Neuron eine Aktivierung sehr hoher Ebene ausgelöst wird, unterdrückt eine lokale Antwort Normalisierung Ebene die Aktivierung der angrenzenden Neuronen Ebene an. Dies ist mithilfe von drei Parameter (***α***, ***β***und ***k***) und eine convolutional Struktur (oder Netzwerk-Umgebung Form). Jeder Neuron in den Layer Ziel ***y*** entspricht einem Neuron ***X*** in der Quellebene. Die Aktivierung Ebene von ***y*** wird angegeben, indem Sie die folgende Formel ein, wobei ***f*** eine Neuron der Aktivierung Ebene und ***n x*** ist der Kernel (oder die Menge an, die die Neuronen etwa ***x***enthält), durch die folgende convolutional Struktur definiert:  

![][1]  

Antwort Normalisierung bündeln unterstützen die convolutional Attribute mit Ausnahme der **Freigabe**, **MapCount**und **-Stärken**.  
 
-   Wenn Kernel Neuronen in der gleichen Karte als ***x***enthält, wird das Schema Normalisierung als **derselben Karte Normalisierung**bezeichnet. Um dieselbe Karte Normalisierung zu definieren, müssen die erste Koordinate in **InputShape** den Wert 1.
-   Wenn der Kernel Neuronen in derselben räumliche Position wie ***x enthält***, aber die Neuronen in anderen Karten sind, wird das Schema Normalisierung **über Karten Normalisierung**aufgerufen. Diese Art von Antwort Normalisierung implementiert eine Form der seitliche untersuchen inspiriert vom Typ gefunden in real Neuronen, induzierten Risikos für große Aktivierung Ebenen zwischen Neuron Ausgaben berechnet auf verschiedenen Karten erstellen. Um über Karten Normalisierung zu definieren, muss die erste Koordinate eine ganze Zahl größer als 1 und nicht größer als die Anzahl der Karten, und die restlichen die Koordinaten müssen den Wert 1.  

Da es sich bei Antwort Normalisierung bündeln eine vordefinierte Funktion auf Knoten Quellwerte zum Bestimmen des Werts der Ziel-Knoten anwenden, haben sie keinen trainable Zustand (-Stärken oder Biases).   

**Benachrichtigen**: Neuronen, die die zentralen Knoten des Kerns sind die Knoten in der Zielebene entsprechen. Wenn KernelShape [d] ungerade ist, wird beispielsweise _KernelShape [d] / 2_ entspricht dem zentralen Kernel-Knoten. _KernelShape [d]_ auch der zentrale Knoten ist, am _KernelShape [d] / 2-1_. Daher ist **Textabstand**[d] falsch, die erste und die letzte _KernelShape [d] / 2_ Knoten haben keinen entsprechende Knoten in der Zielebene. Um dies zu vermeiden, definieren als **Textabstand** [WAHR, WAHR,..., WAHR].  

Zusätzlich zu den vier Attributen, die zuvor beschriebenen unterstützen Antwort Normalisierung bündeln auch die folgenden Attributen:  

-   **Alpha**: (erforderlich): Legt eine Gleitkommazahl Wert, ***α*** in der vorherigen Formel entspricht. 
-   **Beta**: (erforderlich): Legt eine Gleitkommazahl Wert, ***β*** in der vorherigen Formel entspricht. 
-   **Versatz**: (optional) gibt eine Gleitkommazahl Wert, ***k*** in der vorherigen Formel entspricht. Wird als Standardwert 1.  

Im folgende Beispiel wird mit diesen Attributen Antwort Normalisierung bündeln definiert:  

    hidden RN1 [5, 10, 10]
      from P1 response norm {
        InputShape  = [ 5, 12, 12];
        KernelShape = [ 1,  3,  3];
        Alpha = 0.001;
        Beta = 0.75;
      }  

-   Die Quellebene umfasst fünf Karten, jede mit Aof Dimension des 12 x 12 in Knoten des 1440 summieren. 
-   Der Wert **KernelShape** gibt an, dass dies einen gleichen Karte Normalisierung Layer, wobei der Umgebung Rechteck 3 x 3. 
-   Der Standardwert des **Füllzeichens** ist falsch, daher hat der Layer Ziel nur 10 Knoten in jeder Dimension. Zum Einschließen von einem Knoten in der Zielebene, die jeder Knoten in der Quellebene entspricht, fügen Sie den Abstand = ["True", "Wahr", "Wahr"]; und ändern Sie die Größe des RN1 [5, 12 12].  

## <a name="share-declaration"></a>Freigeben der Datensatzdeklaration 
Netz Nr. unterstützt optional mehrere Pakete mit freigegebenen-Stärken definieren. Wenn ihre Strukturen gleich sind, können die-Stärken von einem beliebigen zwei bündeln freigegeben werden. Die folgende Syntax definiert Pakete mit freigegebenen-Stärken:  

    share-declaration:
        share    {    layer-list    }
        share    {    bundle-list    }
       share    {    bias-list    }
    
    layer-list:
        layer-name    ,    layer-name
        layer-list    ,    layer-name
    
    bundle-list:
       bundle-spec    ,    bundle-spec
        bundle-list    ,    bundle-spec
    
    bundle-spec:
       layer-name    =>     layer-name
    
    bias-list:
        bias-spec    ,    bias-spec
        bias-list    ,    bias-spec
    
    bias-spec:
        1    =>    layer-name
    
    layer-name:
        identifier  

Beispielsweise gibt die folgende freigeben-Deklaration Layernamen, gibt an, dass sowohl-Stärken und Biases freigegeben werden sollen:  

    Const {
      InputSize = 37;
      HiddenSize = 50;
    }
    input {
      Data1 [InputSize];
      Data2 [InputSize];
    }
    hidden {
      H1 [HiddenSize] from Data1 all;
      H2 [HiddenSize] from Data2 all;
    }
    output Result [2] {
      from H1 all;
      from H2 all;
    }
    share { H1, H2 } // share both weights and biases  

-   Die Eingabewerte Features werden in zwei gleich gleicher Breite von Ebenen unterteilt. 
-   Ausgeblendeten Ebenen berechnen dann höhere Ebene zwei Ebenen von Funktionen. 
-   Die Freigabe-Deklaration gibt an, dass auf die gleiche Weise aus ihrer jeweiligen Eingaben _H1_ und _H2_ berechnet werden müssen.  
 
Alternativ kann diese mit zwei separaten freigeben Deklarationen wie folgt angegeben werden:  

    share { Data1 => H1, Data2 => H2 } // share weights  

<!-- -->

    share { 1 => H1, 1 => H2 } // share biases  

Sie können das kurze Formular nur, wenn die Ebenen einer einzelnen Paket enthalten. Im Allgemeinen ist die Freigabe möglich, nur, wenn die relevante Struktur identisch ist, was bedeutet, dass sie die gleiche Größe, dieselbe convolutional Geometrie usw. haben.  

## <a name="examples-of-net-usage"></a>Beispiele für den Einsatz Netz Nr.
Dieser Abschnitt enthält einige Beispiele für Verwendungsmöglichkeiten Netz Nr., um ausgeblendete Ebenen hinzugefügt werden soll, definieren, wie die ausgeblendete Ebenen mit anderen Ebenen interagieren und convolutional Netzwerke erstellen.   

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Definieren einer einfachen benutzerdefinierten Trainingsfällen: "Hallo Welt"-Beispiel
Dieses einfache Beispiel veranschaulicht, wie ein Modell Trainingsfällen erstellen, die eine einzelne ausgeblendete Folie enthält.  

    input Data auto;
    hidden H [200] from Data all;
    output Out [10] sigmoid from H all;  

Das Beispiel veranschaulicht einige grundlegende Befehle wie folgt aus:  

-   Die erste Zeile definiert den Eingabewerte Layer (mit dem Namen " _Daten_"). Wenn Sie das Schlüsselwort **Auto** verwenden, enthält die Trainingsfällen automatisch alle Features Spalten in die Eingabewerte Beispiele. 
-   Die zweite Zeile wird die verborgene Ebene erstellt. Der Name _H_ wird die ausgeblendete Layer zugewiesen die 200 Knoten verfügt. Diese Schicht ist die Eingabewerte Layer vollständig verbunden.
-   Die dritte Zeile definiert den Ausgabe Layer (mit der Bezeichnung _O_), der 10 Ausgabeknoten enthält. Wenn die Trainingsfällen für Klassifizierung verwendet wird, ist eine Ausgabeknoten pro Klasse. Das Schlüsselwort **Sigmoidfunktion** gibt an, dass die Ausgabe-Funktion auf die Ausgabe Ebene angewendet wird.   

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Definieren Sie mehrere ausgeblendete Ebenen: Computer Sehschwäche Beispiel
Im folgenden Beispiel wird veranschaulicht, wie eine etwas komplexer Trainingsfällen mit mehreren benutzerdefinierten Ausgeblendete Ebenen definieren.  

    // Define the input layers 
    input Pixels [10, 20];
    input MetaData [7];
    
    // Define the first two hidden layers, using data only from the Pixels input
    hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;
    
    // Define the third hidden layer, which uses as source the hidden layers ByRow and ByCol
    hidden Gather [100] 
    {
      from ByRow all;
      from ByCol all;
    }
    
    // Define the output layer and its sources
    output Result [10]  
    {
      from Gather all;
      from MetaData all;
    }  

In diesem Beispiel veranschaulicht verschiedene Features der Spezifikationssprache neuronale Netzwerke:  

-   Die Struktur hat zwei Eingabewerte Layer _Pixel_ und _Metadaten_.
-   Die _Pixel_ -Ebene ist eine Quellebene für zwei Verbindung bündeln, mit dem Ziel Ebenen, _ByRow_ und _ByCol_.
-   Die Ebenen _Sammeln_ und _Ergebnis_ sind Ziel Ebenen in mehreren Verbindung bündeln.
-   Der Ausgabe Ebene, _Ergebnis_, handelt es sich um eine Zielebene in zwei Verbindung bündeln; eine mit der zweiten Ebene ausgeblendeten (sammeln) als Ziel Layer und anderen mit der Eingabe Ebene (Metadaten) als Ziel Layer.
-   Die ausgeblendeten Ebenen, _ByRow_ und _ByCol_, geben Sie gefilterten Connectivity mithilfe von Prädikatsausdrücke an. Genauer, den Knoten in _ByRow_ am [x, y] mit den Knoten in _Pixel_ , die erste Koordinate des Knotens, gleich der erste Index Koordinate aufweisen, verbunden ist X. Entsprechend den Knoten in _ByCol am [x, y] mit den Knoten in _Pixel_ , den zweiten Index innerhalb einer der zweite Koordinate des Knotens, koordinieren aufweisen, verbunden ist y.  

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Definieren ein convolutional Netzwerks für multiclass Klassifizierung: Ziffer Spracherkennung Beispiel
Die Definition Folgendes Netzwerk dient dazu, Zahlen erkannt und veranschaulicht einige erweiterten Techniken für einen Trainingsfällen anpassen.  

    input Image [29, 29];
    hidden Conv1 [5, 13, 13] from Image convolve 
    {
       InputShape  = [29, 29];
       KernelShape = [ 5,  5];
       Stride      = [ 2,  2];
       MapCount    = 5;
    }
    hidden Conv2 [50, 5, 5]
    from Conv1 convolve 
    {
       InputShape  = [ 5, 13, 13];
       KernelShape = [ 1,  5,  5];
       Stride      = [ 1,  2,  2];
       Sharing     = [false, true, true];
       MapCount    = 10;
    }
    hidden Hid3 [100] from Conv2 all;
    output Digit [10] from Hid3 all;  


-   Die Struktur hat eine einzelne Ebene Eingabe-, _Bild_.
-   Das Schlüsselwort **convolve** zeigt an, dass die Ebenen, die mit dem Namen _Conv1_ und _Conv2_ convolutional Ebenen sind. Jede dieser Deklarationen Layer folgt eine Liste der Attribute Faltung.
-   Das Netz hat eine dritte Ebene, _Hid3_, ausgeblendet, die vollständig mit der zweiten verborgenen Ebene _Conv2_verbunden ist.
-   Die Ausgabe-Schicht _Ziffer_, ist nur für den dritten ausgeblendete Layer _Hid3_verbunden. Das Schlüsselwort **Alle** gibt an, dass die Ausgabe Ebene vollständig mit _Hid3_verbunden ist.
-   Der Stelligkeit, der die Faltung ist drei (die Länge des die Tupel **InputShape**, **KernelShape**, **Schritt**und **Freigabe**). 
-   Die Anzahl der pro Kernel-Stärken ist _1 + **KernelShape**\[0] * * *KernelShape**\[1]* **KernelShape**\[2] = 1 + 1 *5* 5 = 26. Oder 26 * 50 = 1300_.
-   Sie können die Knoten in jeder ausgeblendete Ebene wie folgt berechnen:
    -   **NodeCount**\[0] = (5 – 1) / 1 + 1 = 5.
    -   **NodeCount**\[1] = (13-5) / 2 + 1 = 5. 
    -   **NodeCount**\[2] = (13-5) / 2 + 1 = 5. 
-   Die Gesamtzahl der Knoten kann berechnet werden, mithilfe der Ebene, der Anzahl der deklarierten Dimensionen [50, 5, 5] wie folgt: _ **MapCount** * * *NodeCount**\[0]* **NodeCount**\[1] * * *NodeCount**\[2] = 10* 5 *5* 5_
-   Da **Freigabe**[d] falsch ist nur für _d == 0_, ist die Anzahl der Kerneln _ **MapCount** * * *NodeCount**\[0] = 10* 5 = 50_. 


## <a name="acknowledgements"></a>Empfangsbestätigungen

Die Netz-Sprache zum Anpassen der neuronale Netzwerke-Architektur wurde von Shon Katzenberger (Architekt, Computer Learning) und Alexey Kamenev (Softwareentwickler, Microsoft Research) bei Microsoft entwickelt. Es wird intern für Projekte und Applikationen von Bild Erkennung bis hin zu Text Analytics learning Computer verwendet. Weitere Informationen finden Sie unter [Neuronale Netze in Azure ML - Einführung Netz Nr.](http://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)


[1]:./media/machine-learning-azure-ml-netsharp-reference-guide/formula_large.gif
 
