<properties
    pageTitle="Entwerfen von virtuellen Computern Maßstab für die Skalierung legt | Microsoft Azure"
    description="Informationen Sie dazu, wie Ihre virtuellen Computern skalieren Mengen für die Skalierung Entwurf"
    keywords="Linux virtuellen Computern, legt Maßstab virtuellen Computern" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="gatneil"/>

# <a name="designing-vm-scale-sets-for-scale"></a>Entwerfen virtueller Computer Maßstab für die Skalierung legt

In diesem Thema wird erläutert, gibt für virtuellen Computern skalieren Mengen. Informationen dazu, was virtuellen Computern skalieren Sätze sind finden Sie unter [Virtuellen Computern skalieren Sätze Overview](virtual-machine-scale-sets-overview.md).


## <a name="storage"></a>Speicher

Eine Reihe von Maßstab verwendet Speicherkonten zum Speichern von OS Datenträger der virtueller Computer festlegen. Es empfiehlt sich das Verhältnis der 20 virtuellen Computern pro Storage-Konto oder weniger. Es empfiehlt sich auch, dass Sie die ersten Zeichen des den Kontonamen Speicher Alphabets verteilt. Ausführen, damit hilft beim Laden auf verschiedenen internen Systeme ausdehnen. Beispielsweise in der folgenden Vorlage, wir verwenden der Funktion Ressourcenmanager Vorlage UniqueString Präfix Hashes generieren, die Speicher Kontonamen vorangestellt werden: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat).


## <a name="overprovisioning"></a>Bereitstellung überproportional vieler

Beginnend mit der API "2016 03 30"-Version wird standardmäßig virtueller Computer Maßstab Sätze "Bereitstellung überproportional vieler" virtuellen Computern. Mit Bereitstellung überproportional vieler aktiviert, die Skalierung dreht sich tatsächlich von mehr als Sie aufgefordert, den virtuellen Computern festlegen und dann löscht die zusätzlichen virtuellen Computern, die letzten erstellt wird. Bereitstellung überproportional vieler wird provisioning Erfolg Sätzen verbessert. Sie sind nicht für diese zusätzlichen virtuellen Computern Abrechnung und Richtung Ihrer Grenzwerte Kontingent nicht gezählt.

Während der Bereitstellung überproportional vieler provisioning Erfolg Sätzen verbessern kann, kann es verwirrend Verhalten für eine Anwendung, die nicht für die Verarbeitung von virtuellen Computern unangemeldete verschwinden von verursachen. Zum Aktivieren der Bereitstellung überproportional vieler deaktivieren, sicherzustellen, dass die folgende Zeichenfolge in der Vorlage: "Overprovision": "false". Weitere Informationen hierzu finden Sie in der [Dokumentation zur virtuellen Computer Maßstab festlegen REST-API](https://msdn.microsoft.com/library/azure/mt589035.aspx).

Wenn Sie die Bereitstellung überproportional vieler deaktivieren, Sie können mit einem größeren Verhältnis von virtuellen Computern pro Storage Konto abwesend abrufen, aber nicht empfohlen, übergeht über 40.


## <a name="limits"></a>Grenzwerte
Ein Maßstab festlegen auf ein benutzerdefiniertes Bild (eine von Ihnen erstellte) erstellt muss alle OS Datenträger virtuelle Festplatten in einem Speicherkonto erstellen. Daher beträgt die maximale Anzahl von virtuellen Computern in einer Gruppe von Farben-Skala auf ein benutzerdefiniertes Bild erstellt empfohlen 20. Wenn Sie die Bereitstellung überproportional vieler deaktivieren, können Sie bis zu 40 wechseln.

Ein Maßstab festlegen auf ein Bild Plattform integriert ist derzeit auf 100 virtuellen Computern (Wir empfehlen 5 Speicherkonten für diese Skala) beschränkt.

Für weitere virtuelle Computer als diese Grenzwerte zulassen möchten, müssen Sie mehrere Maßstab Sätze bereitstellen, wie in [dieser Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale)dargestellt.