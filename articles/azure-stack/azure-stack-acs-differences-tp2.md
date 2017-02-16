
<properties
    pageTitle="Konsistente Azure Speicher: Unterschiede und Aspekte | Microsoft Azure"
    description="Informationen über die Unterschiede aus Azure Speicherplatz und andere Aspekte beim Bereitstellen von Azure konsistent Speicher aus."
    services="azure-stack"
    documentationCenter=""
    authors="MChadalapaka"
    manager="siroy"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="mchad"/>

# <a name="azure-consistent-storage-differences-and-considerations"></a>Konsistente Azure Speicher: Unterschiede und Aspekte

Konsistente Azure Speicher ist Speicher Cloud-Dienste in Microsoft Azure Stapel festlegen. Konsistente Azure Speicher bietet Blob, Tabelle, Warteschlange und Verwaltungsfunktionen mit Azure einheitlichen Semantik Konto an. In diesem Artikel werden die bekannten Azure konsistent Speicherunterschiede mit Azure-Speicher zusammengefasst. Es werden auch andere Aspekte zu beachten, wenn Sie die aktuell öffentlich zugängliche Vorschauversion von Microsoft Azure Stapel bereitstellen zusammengefasst.

<span id="Concepts" class="anchor"><span id="_Toc386544169" class="anchor"><span id="_Toc389466742" class="anchor"><span id="_Ref428966996" class="anchor"><span id="_Toc433223853" class="anchor"></span></span></span></span></span>
## <a name="known-differences"></a>Bekannten unterschieden

Diese Technical Preview-Version von Azure konsistent Speicher verfügt nicht 100 % Feature Unstimmigkeit mit Azure-Speicher für die API-Versionen, die unterstützt werden. Bekannte Funktionsunterschiede Folgendes enthalten:

-   Bestimmte Kontotypen Speicher sind noch nicht verfügbar, z. B. Standard\_RAGRS und Standard\_GRS.

-   Premium\_LRS Speicherkonten bereitgestellt werden können. Es gibt jedoch derzeit keine Leistungsgrenzwerte oder Garantien.

-   Azure Dateien Funktionalität ist noch nicht verfügbar.

-   Die erste Seite Bereiche-API unterstützt nicht das Abrufen von Seiten, die Momentaufnahmen der Seitenblobs unterschiedlich sein.

-   Die erste Seite Bereiche-API gibt Seiten, die 4 KB Genauigkeit aufweisen.

-   Partitionsschlüssel und Zeile Key in der Azure-konsistente Speicher Tabelle Implementierung sind jeweils auf 400 Zeichen (d. h., 800 Byte) eingeschränkt.

-   BLOB-Namen in der Azure-konsistente Speicher Blob-Service-Implementierung sind auf 880 Zeichen (d. h., 1760 Byte) eingeschränkt.

-   Die konsistente Azure Speicher Implementierung des Mandanten Speicher-Verwendungsdaten reporting Speicher Verwendung Meter bereitstellt, mit denen in Azure mit derselben Semantik und Einheiten identisch sind. Aktuell, allerdings der Speichertransaktionen Verwendung Meter IaaS Transaktionen nicht enthält, und der Datenübertragung Verwendung Meter unterscheidet die Bandbreite Verwendung von internen oder externen Netzwerkverkehr auf einen Bereich Azure Stapel nicht.

-   Bestimmte Unterschiede bestehen in den Bereich der Funktionalität für Speicher Verwaltbarkeit. Sie nicht z. B. Kontotyp ändern oder benutzerdefinierte Domänen haben. Darüber hinaus wird nur API Ebene Konsistenz für die Premium angeboten\_LRS Speicher Kontotyp.

## <a name="deployment-considerations"></a>Aspekte beim Bereitstellen

-   **Testen.** Stellen Sie nicht konsistent Azure Speicher in Herstellung Umgebungen, die die aktuelle Microsoft Azure Stapel Technical Preview-Version verwenden. Diese Version ist nur Testzwecken in einer Übung testumgebung gedacht.

-   Optimale **Leistung**. Die Microsoft Azure Stapel Technical Preview-Version von Azure konsistent Speicher ist nicht vollständig Performance-optimierten.

-   **Skalierbarkeit.** Die Microsoft Azure Stapel Technical Preview-Version von Azure konsistent Speicher ist nicht für Skalierbarkeit optimiert.

## <a name="version-support-considerations"></a>Version Support Aspekte

Mit dieser Preview-Version von Azure konsistent Speicher werden die folgenden Versionen unterstützt:

> Azure-Speicher-Client-Bibliothek: [Microsoft Azure-Speicher 6.x .NET SDK](http://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)
>
> Azure Speicher Datendienste: [2015-04-05 REST-API Version](https://msdn.microsoft.com/library/azure/mt705637.aspx)
>
> Azure Storage Management Services: [2015-05-01-Vorschau](https://msdn.microsoft.com/library/azure/mt163683.aspx)
> [2015-06-15](https://msdn.microsoft.com/library/azure/mt163683.aspx)
## <a name="next-steps"></a>Nächste Schritte

-   [Einführung in Azure konsistent Speicher](azure-stack-storage-overview.md)
