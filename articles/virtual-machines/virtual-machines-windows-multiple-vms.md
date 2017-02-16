<properties
    pageTitle="Erstellen von mehreren virtuellen Computern | Microsoft Azure"
    description="Optionen für das Erstellen von mehreren virtuellen Computern unter Windows"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="guybo"/>

# <a name="create-multiple-azure-virtual-machines"></a>Erstellen Sie mehrerer Azure-virtuellen Computern

Es gibt viele Szenarios, wenn Sie eine große Anzahl ähnlicher virtueller Computer (virtuellen Computern) erstellen müssen. Beispiele sind High Performance computing (HPC), umfangreiche Datenanalyse, skalierbare und häufig statusfreie mittleren Ebene oder Back-End-Server (z. B. Webserver.) und verteilten Datenbanken.

In diesem Artikel werden die verfügbaren Optionen zum Erstellen von mehreren virtuellen Computern in Azure. Diese Optionen hinausgehen den einfachen Fällen, wo Sie eine Reihe von virtuellen Computern manuell erstellen. Zum Erstellen von vielen virtuellen Computern skalieren nicht die Prozessen, die Sie normalerweise verwenden auch, wenn Sie mehr als ein paar virtuellen Computern erstellen müssen.

Eine Möglichkeit zum Erstellen von vielen ähnlichen virtuellen Computern besteht darin, Azure Ressourcenmanager Erstellen von _Ressourcen in einer Schleife_verwenden.

## <a name="resource-loops"></a>Ressource Schleifen

Ressource Schleifen sind Syntax Abkürzung in Azure Ressourcenmanager Vorlagen. Ressource Schleifen können eine Reihe von ähnlich konfigurierten Ressourcen in einer Schleife erstellen. Ressource Schleifen können Sie mehrere Speicherkonten, Netzwerk-Schnittstellen oder virtuellen Computern erstellen. Weitere Informationen zu Ressourcen Schleifen finden Sie in [Erstellen von virtuellen Computern in Verfügbarkeit legt Ressource Schleifen verwenden](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/).

## <a name="challenges-of-scale"></a>Probleme mit Skalierung

Auch wenn Ressourcen Schleifen leichter zu erstellen, die sich bei einer Cloud-Infrastruktur und kürzere Vorlagen, bleiben bestimmte Probleme. Wenn Sie eine Ressource Schleife zum Erstellen von 100 virtuellen Computern verwenden, müssen Sie beispielsweise Network Interface Controller (NICs) mit den entsprechenden virtuellen Computern und Speicherkonten zu koordinieren. Da die Anzahl der virtuellen Computern wahrscheinlich von der Anzahl der Speicherkonten unterschiedlich sein, müssen Sie auch andere Ressourcen Schleife an Papiergrößen, behandelt. Hierbei handelt es sich um lösbaren Probleme, aber die Komplexität nimmt erheblich mit Skalierung.

Eine weitere Herausforderung tritt auf, wenn Sie eine Infrastruktur benötigen, die elastisch skaliert. Angenommen, sollten Sie eine Infrastruktur automatisch skalieren, die automatisch erhöht oder verringert die Anzahl der virtuellen Computern als Antwort auf Arbeitsbelastung. Virtuellen Computern nicht die bieten einer integrierten Methode Number (Skalierung und Maßstab in) variieren. Wenn Sie durch das Entfernen von virtuellen Computern in skalieren, ist es schwierig, hohen Verfügbarkeit zu gewährleisten, indem Sie sicherstellen, dass die Aktualisierung und Fehlerstrukturanalyse Domänen virtuellen Computern verteilt werden.

Wenn Sie eine Ressource Schleife verwenden, wechseln mehrere Anrufe an Ressourcen erstellen schließlich zu der zugrunde liegenden Textur. Wenn mehrere Anrufe ähnliche Ressourcen erstellen, weist Azure implizite Möglichkeit, eine Verbesserung dieser Entwurf und Bereitstellung Zuverlässigkeit und Leistung zu optimieren. Dies ist die Stelle, an der _virtuellen Computern Maßstab legt_ nützlich sein.

## <a name="virtual-machine-scale-sets"></a>Virtuellen Computern skalieren Datensätze

Virtuellen Computern sind skalieren eine Ressource Azure berechnen bereitstellen und Verwalten einer Gruppe von identischen virtuellen Computern. Alle virtuellen Computern konfiguriert denselben, virtueller Computer Maßstab Sätze sind einfach skalieren und skalieren. Sie einfach ändern, die Anzahl der virtuellen Computern festlegen. Sie können auch virtueller Computer Maßstab Datensätze zu skalieren, basierend auf die Notwendigkeit, die Arbeitsbelastung konfigurieren.

Für Applikationen, die zum Berechnen von Ressourcen aus- und skalieren müssen, sind Maßstab Operationen implizit Fehlerstrukturanalyse und Aktualisieren von Domänen verteilt.

Statt abgleichen mehrere Ressourcen wie NICs und virtuellen Computern, weist ein virtuellen Computer Maßstab Satz Netzwerk, Speicher, virtuellen Computern und gemeinsame Erweiterungseigenschaften, die Sie, zentral konfigurieren können.

Eine Einführung in virtuellen Computer Maßstab Mengen finden Sie in den [virtuellen Computern Maßstab legt Produkt (Seite)](https://azure.microsoft.com/services/virtual-machine-scale-sets/). Ausführlichere Informationen finden Sie unter der [virtuellen Computern Maßstab legt Dokumentation](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/).
