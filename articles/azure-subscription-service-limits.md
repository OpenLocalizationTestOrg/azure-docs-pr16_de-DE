<properties
    pageTitle="Microsoft Azure-Abonnement und Beschränkungen Service, Kontingente und Einschränkungen"
    description="Enthält eine Liste von häufig Azure-Abonnement und Beschränkungen Service, Kontingente und Einschränkungen. Informationen zum Vergrößern Grenzwerte zusammen mit Maximalwerte umfasst."
    services=""
    documentationCenter=""
    authors="rothja"
    manager="jeffreyg"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="btardif"/>

# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure-Abonnement und Beschränkungen Service, Kontingente und Einschränkungen

## <a name="overview"></a>(Übersicht)

Dieses Dokument gibt einige der am häufigsten verwendeten Microsoft Azure Grenzwerte an. Dies befasst derzeit alle Azure Services sich nicht. Im Laufe der Zeit werden diese Grenzwerte erweitert und aktualisiert, um weitere der Plattform bedecken.

Besuchen Sie [Azure Preise Übersicht](https://azure.microsoft.com/pricing/) , um weitere Informationen zur Azure-Preisen an. Vorhanden ist, können Sie die Kosten, die [Preise Taschenrechner](https://azure.microsoft.com/pricing/calculator/) mit schätzen oder indem Sie die Preisgestaltung Detailseite für einen Dienst (beispielsweise [Windows virtuellen Computern](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).

> [AZURE.NOTE] Wenn Sie die Beschränkung über die **Beschränkung Standard**potenzieren möchten, können Sie [eine Supportanfrage online Kunden kostenlos zu öffnen](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). Die Grenzwerte können nicht über die **Obergrenze** Wert in den folgenden Tabellen ausgelöst werden. Ist keine **Obergrenze** Spalte, muss die angegebene Ressource nicht anpassbare Grenzwerte.

## <a name="limits-and-the-azure-resource-manager"></a>Grenzwerte und Azure Ressourcenmanager

Es ist jetzt möglich, mehrere Azure Ressourcen in einer einzelnen Azure Ressourcengruppe kombinieren. Wenn Ressourcen Kontaktgruppen verwenden, werden Grenzwerte angezeigt, die nur ein Mal globale wurden auf einer regionalen Ebene mit Azure-Manager verwaltet. Weitere Informationen zu Azure Ressourcengruppen finden Sie unter [Übersicht Azure Ressourcenmanager](azure-resource-manager/resource-group-overview.md).

In der nachstehenden Grenzwerte hat eine neue Tabelle hinzugefügt wurde, um alle Unterschiede zwischen den Grenzwerten entsprechen, wenn Ressourcenmanager Azure verwenden. Beispielsweise ist ein **Abonnement Grenzwerte** und einer Tabelle **Abonnement Grenzwerte - Ressourcenmanager Azure** . Wenn maximal auf beiden Szenarien angewendet wird, wird es nur in der ersten Tabelle angezeigt. Sofern nicht anders angegeben, sind in allen Regionen Grenzwerte global.

> [AZURE.NOTE] Es ist wichtig, um hervorzuheben, dass Kontingente für Ressourcen in Azure Ressourcengruppen pro Region Ihres Abonnements zugreifen können, und sind nicht pro Abonnement, wie der Dienst Management Kontingente sind. Verwenden Sie uns Core Kontingente als Beispiel für ein. Wenn Sie ein Kontingent erhöhen mit Unterstützung für Kerne anfordern müssen, müssen Sie wie viele Kerne entschieden haben, in welche Regionen verwenden, und nehmen die Anforderung eine bestimmte Ressourcengruppe Azure Core Kontingente für die Beträge und Regionen, die gewünschte möchten. Daher, wenn Sie 30 Kerne in Europa Westen verwenden Sie es; die Anwendung ausführen müssen Sie sollten insbesondere 30 Kerne in Europa Westen anfordern. Jedoch in keiner anderen Region vergrößern Core Kontingent wird nicht angezeigt, – nur Westen Europe wird das Kontingent 30-Core haben.
<!-- -->
Daher möglicherweise Sie es sinnvoll, erwägen Sie die Entscheidung, was Ihre Azure Ressourcengruppe Kontingente für Ihre Arbeitsbelastung in einem-Region sein müssen und Anfordern von diesen Betrag in jeder Region, in dem Sie Bereitstellung in Betracht ziehen. Lesen Sie weitere Hilfe zum Ermitteln von Ihrem aktuellen Kontingente für bestimmte Regionen [Behandeln von Problemen bei der Bereitstellung](./resource-manager-common-deployment-errors.md) .

## <a name="service-specific-limits"></a>Beschränkungen Service-spezifische

- [Active Directory](#active-directory-limits)
- [API Management](#api-management-limits)
- [App-Dienst](#app-service-limits)
- [Application Gateway](#application-gateway-limits)
- [Anwendung Einsichten](#application-insights-limits)
- [Automatisierung](#automation-limits)
- [Azure Redis Cache](#azure-redis-cache-limits)
- [Azure RemoteApp](#azure-remoteapp-limits)
- [Sicherung](#backup-limits)
- [Stapelverarbeitung](#batch-limits)
- [BizTalk-Dienste](#biztalk-services-limits)
- [CDN](#cdn-limits)
- [Cloud-Dienste](#cloud-services-limits)
- [Daten Factory](#data-factory-limits)
- [Daten Lake Analytics](#data-lake-analytics-limits)
- [DNS-EINTRÄGE](#dns-limits)
- [DocumentDB](#documentdb-limits)
- [Ereignis Hubs](#event-hubs-limits)
- [IoT-Hub](#iot-hub-limits)
- [Key Tresor](#key-vault-limits)
- [Media-Dienste](#media-services-limits)
- [Mobile Engagement](#mobile-engagement-limits)
- [Mobile-Diensten](#mobile-services-limits)
- [Für die Überwachung](#monitoring-limits)
- [Mehrstufige Authentifizierung](#multi-factor-authentication)
- [Netzwerke](#networking-limits)
- [Benachrichtigungsdienst-Hub](#notification-hub-service-limits)
- [Betrieb Einsichten](#operational-insights-limits)
- [Ressourcengruppe](#resource-group-limits)
- [Scheduler](#scheduler-limits)
- [Suchen](#search-limits)
- [Dienstbus](#service-bus-limits)
- [Website-Wiederherstellung](#site-recovery-limits)
- [SQL-Datenbank](#sql-database-limits)
- [Speicher](#storage-limits)
- [StorSimple System](#storsimple-system-limits)
- [Stream Analytics](#stream-analytics-limits)
- [Abonnement](#subscription-limits)
- [Datenverkehr-Manager](#traffic-manager-limits)
- [Virtuellen Computern](#virtual-machines-limits)
- [Virtuellen Computern skalieren Datensätze](#virtual-machine-scale-sets-limits)


### <a name="subscription-limits"></a>Grenzwerte für Abonnements
#### <a name="subscription-limits"></a>Grenzwerte für Abonnements
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Grenzwerte für Abonnement - Azure Ressourcenmanager

Die folgenden Grenzwerte gelten bei Verwendung des Azure Ressourcenmanager und Azure Ressourcengruppen. Grenzwerte, die nicht mit Azure-Manager geändert haben, werden nicht unter aufgeführt. Lizenzinformationen finden Sie in der vorherigen Tabelle für diese Grenzwerte.

Informationen zum Behandeln von Grenzwerte auf Ressourcenmanager Besprechungsanfragen finden Sie unter [Begrenzungsebene Ressourcenmanager Serviceanfragen](resource-manager-request-limits.md).

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### <a name="resource-group-limits"></a>Ressourcengruppe Beschränkungen

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Grenzwerte für virtuellen Computern
#### <a name="virtual-machine-limits"></a>Grenzwerte für virtuellen Computern
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### <a name="virtual-machines-limits---azure-resource-manager"></a>Grenzwerte für virtuellen Computern - Azure Ressourcenmanager

Die folgenden Grenzwerte gelten bei Verwendung des Azure Ressourcenmanager und Azure Ressourcengruppen. Grenzwerte, die nicht mit Azure-Manager geändert haben, werden nicht unter aufgeführt. Lizenzinformationen finden Sie in der vorherigen Tabelle für diese Grenzwerte.

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Grenzwerte für virtuellen Computern skalieren Datensätze

[AZURE.INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="networking-limits"></a>Grenzwerte für Netzwerke

[AZURE.INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Grenzwerte für Netzwerke
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Application Gateway beschränkt.

[AZURE.INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="traffic-manager-limits"></a>Grenzwerte für den Datenverkehr-Manager

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS-Grenzwerte

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Speicherlimits

Weitere Details auf Speichergrenzwerte-Konto finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](../articles/storage/storage-scalability-targets.md).

#### <a name="storage-service-limits"></a>Speicherlimits-Dienst

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

#### <a name="virtual-machine-disk-limits"></a>Speichergrenze virtuellen Computern

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Weitere Details finden Sie unter [virtuellen Computern Größen](../articles/virtual-machines/virtual-machines-linux-sizes.md) .

**Standard Speicherkonten**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

**Premium Speicher-Konten**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>Speicherlimits-Anbieter für Ressourcen

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### <a name="cloud-services-limits"></a>Grenzwerte für Cloud-Dienste

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### <a name="app-service-limits"></a>Grenzwerte für App-Dienst
Die folgenden Grenzwerte für die App-Dienst ist Grenzwerte für Web Apps, Mobile-Apps, Apps-API und Logik Apps enthalten.

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Scheduler Beschränkungen

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Grenzwerte für Stapel

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

###<a name="biztalk-services-limits"></a>Grenzwerte für BizTalk-Dienste
Die folgende Tabelle listet die Grenzwerte für Azure Biztalk-Dienste.

[AZURE.INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]


### <a name="documentdb-limits"></a>Grenzwerte für DocumentDB

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]

Kontingente aufgeführt, die mit einem Sternchen (*) [Durch Kontaktieren des Supports Azure angepasst werden kann](./documentdb/documentdb-increase-limits.md).

### <a name="mobile-engagement-limits"></a>Grenzwerte für Mobile Engagement

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### <a name="search-limits"></a>Suchgrenzwerte

Preisgestaltung Ebenen fest, Kapazität und Grenzen der Suchdienst. Ebenen umfassen:

- *Kostenlos* mit mehreren Mandanten Dienst für andere Azure Abonnenten, richtet sich Auswertung und kleine Entwicklungsprojekte freigegeben.
- *Grundlegende* bietet dedizierte computing-Ressourcen für Produktionsarbeitslasten mit Maßen, mit bis zu drei Replikaten für hoch verfügbare Abfrage Auslastung.
- *Standard (S1, S2, S3, S3 HD)* ist für größere Produktionsarbeitslasten. Mehrere Ebenen befinden sich innerhalb der standard-Ebene, damit Sie eine Ressourcenkonfiguration für bestimmte Szenarien auswählen können.

**Grenzwerte pro Abonnement**

[AZURE.INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Grenzwerte pro Suchdienst**

[AZURE.INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Detailliertere Informationen zu anderen den Grenzwerten, einschließlich der Größe des Dokuments Abfragen pro Sekunde, Tasten, Besprechungsanfragen und Antworten, finden Sie unter [Service Grenzwerte in Azure suchen](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Grenzwerte für Media-Dienste

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN Beschränkungen

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Grenzwerte für Mobile-Dienste

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitoring-limits"></a>Grenzwerte für die Überwachung

[AZURE.INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Hub Benachrichtigungsdienst beschränkt.

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Ereignis Hubs Beschränkungen

[AZURE.INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Dienstbus Beschränkungen

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>Grenzwerte für IoT-Hub

[AZURE.INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>Daten Factory Beschränkungen

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Daten Lake Analytics Beschränkungen
[AZURE.INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="stream-analytics-limits"></a>Stream Analytics Beschränkungen
[AZURE.INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Active Directory begrenzt

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### <a name="azure-remoteapp-limits"></a>Azure RemoteApp Beschränkungen

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>Grenzwerte für StorSimple System

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### <a name="operational-insights-limits"></a>Betrieb Einsichten Beschränkungen

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Zusätzliche Einschränkungen

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Website Wiederherstellung Beschränkungen

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Anwendung Einsichten Beschränkungen

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>Grenzwerte für Management-API

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Grenzwerte für Azure Redis Cache

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Taste Tresor Beschränkungen

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Mehrstufige Authentifizierung
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Automatisierung Beschränkungen
[AZURE.INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>Grenzwerte für SQL-Datenbank

Grenzwerte für SQL-Datenbank finden Sie unter [SQL-Datenbank Ressource Grenzwerte](sql-database/sql-database-resource-limits.md).

## <a name="see-also"></a>Siehe auch

[Grundlegendes zu Azure Grenzwerte und erhöht](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Virtuellen Computern und Cloud-Dienst Größen für Azure](virtual-machines/virtual-machines-linux-sizes.md)

[Größen für Cloud-Dienste](cloud-services/cloud-services-sizes-specs.md)
