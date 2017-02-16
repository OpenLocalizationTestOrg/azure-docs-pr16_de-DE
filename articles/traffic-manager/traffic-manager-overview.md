<properties
    pageTitle="Was Datenverkehr Manager ist | Microsoft Azure"
    description="In diesem Artikel wird Ihnen helfen zu verstehen, was den Datenverkehr Manager ist, und gibt an, ob es sich um die Wahl der richtigen Datenverkehr Weiterleitung für eine Anwendung ist"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-traffic-manager"></a>Übersicht über den Datenverkehr-Manager

Microsoft Azure Datenverkehr-Manager können Sie die Verteilung der den Benutzerdatenverkehr für Endpunkte in verschiedenen Rechenzentren zu steuern. Endpunkte, die von den Datenverkehr Manager unterstützten einschließen Azure-virtuellen Computern, Web Apps, und cloud Services. Sie können auch den Datenverkehr Manager mit externen, nicht Azure Endpunkten verwenden.

Datenverkehr Manager verwendet Domain Name System (DNS), um das direkte Client-Anfragen an den am besten geeigneten Endpunkt basierend auf einer [Datenverkehr-routing Methode](traffic-manager-routing-methods.md) und die Integrität des die Endpunkte. Datenverkehr Manager bietet einen Zellbereich Datenverkehr-routing Methoden zu anderen Anwendung Erfordernisse, Endpunkt [Überwachen](traffic-manager-monitoring.md)des Systemzustands und automatisches Failover entsprechend. Datenverkehr Manager ist flexibel in Bezug auf Fehler, einschließlich den Ausfall der eine ganze Azure Region.

## <a name="traffic-manager-benefits"></a>Vorteile der Datenverkehr-Manager

Datenverkehr Manager können Ihnen helfen:

- **Verbessern der Verfügbarkeit von wichtigen Anwendung**

    Datenverkehr Manager bietet hohen Verfügbarkeit für die Anwendung, durch die Endpunkte für die Überwachung und automatisches Failover bereitstellen, wenn Sie ein Endpunkt-fällt aus.

- **Verbesserung der Reaktionszeiten für Applikationen leistungsfähige**

    Azure ermöglicht Ihnen, die Cloud Services oder Websites in Rechenzentren rund um den Globus ausführen. Datenverkehr Manager verbessert Anwendung Reaktionszeiten Datenverkehr an den Endpunkt mit den niedrigsten Netzwerkwartezeit für den Client ab, indem Sie ein.

- **Ausführen der Dienst Wartung ohne Ausfallzeiten**

    Sie können Vorgänge der geplanten Wartung auf Ihre Applikationen Substantiven ausführen. Datenverkehr Manager leitet den Datenverkehr in alternativen Endpunkte während die Wartung ausgeführt wird.

- **Kombinieren von lokalen und Cloud-basierten Anwendungen**

    Datenverkehr Manager unterstützt externe, nicht Azure Endpunkte, aktivieren es mit Hybrid verwendet werden cloud und lokale Bereitstellungen, einschließlich der "Burst-zu-Cloud," "Migrieren zu Cloud," und "Failover Cloud" Szenarien.

- **Verteilen von Datenverkehr für großen, komplexen Bereitstellungen**

    Verwenden [verschachtelte Datenverkehr Manager Profile](traffic-manager-nested-profiles.md), können Datenverkehr-routing Methoden zum Erstellen von anspruchsvolle und flexibler Regeln, um Unterstützung für größere und komplexere Bereitstellungen den Anforderungen kombiniert werden.

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Funktionsweise der Datenverkehr-Manager](traffic-manager-how-traffic-manager-works.md).

- Informationen Sie zum Entwickeln hoher Verfügbarkeit von Applications [Datenverkehr Manager Endpunkt Überwachung](traffic-manager-monitoring.md)verwenden.

- Erfahren Sie mehr zu den [Datenverkehr-routing Methoden](traffic-manager-routing-methods.md) von Datenverkehr Manager unterstützt.

- [Erstellen eines Profils Datenverkehr-Manager](traffic-manager-manage-profiles.md).

