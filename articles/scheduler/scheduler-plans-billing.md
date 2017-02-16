<properties
 pageTitle="Pläne und Abrechnung in Azure Scheduler"
 description="Pläne und Abrechnung in Azure Scheduler"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="plans-and-billing-in-azure-scheduler"></a>Pläne und Abrechnung in Azure Scheduler

## <a name="job-collection-plans"></a>Position Websitesammlungs-Pläne

Position Websitesammlungen sind die berechenbaren Entität in Azure Scheduler. Position-Sammlungen enthalten eine Anzahl von Aufträgen und drei Pläne – kostenlos, Standard und Premium – die nachfolgend beschriebenen nützlich sein.

|**Position Websitesammlung Plan**|**Maximale Anzahl der Stellen pro Websitesammlung Position**|**Max-Serie**|**Max-Position Websitesammlungen pro Abonnement**|**Grenzwerte**|
|:---|:---|:---|:---|:---|
|**Kostenlose**|5 Stellen pro Websitesammlung Position|Einmal pro Stunde. Aufträge kann nicht öfter als einmal pro Stunde ausgeführt werden.|Ein Abonnement ist bis zu 1 kostenlosen Auftrag Websitesammlung zulässig.|[Http-ausgehende Autorisierung Objekt](scheduler-outbound-authentication.md) kann nicht verwendet werden.
|**Standard**|50 Stellen pro Websitesammlung Position|Einmal pro Minute. Aufträge kann nicht öfter als einmal pro Minute ausgeführt werden.|Ein Abonnement ist bis zu 100 standard Auftrag Websitesammlungen zulässig.|Zugriff auf alle Featuresatz von Scheduler|
|**P10 Premium**|50 Stellen pro Websitesammlung Position|Einmal pro Minute. Aufträge kann nicht öfter als einmal pro Minute ausgeführt werden.|Ein Abonnement kann bis zu 10.000 P10 Premium Auftrag Websitesammlungen. Sie <a href="mailto:wapteams@microsoft.com">uns wenden Sie sich an</a> , um weitere.|Zugriff auf alle Featuresatz von Scheduler|
|**P20 Premium**|1000 Stellen pro Websitesammlung Position|Einmal pro Minute. Aufträge kann nicht öfter als einmal pro Minute ausgeführt werden.|Ein Abonnement kann bis zu 10.000 P20 Premium Auftrag Websitesammlungen. Sie <a href="mailto:wapteams@microsoft.com">uns wenden Sie sich an</a> , um weitere.|Zugriff auf alle Featuresatz von Scheduler|

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Upgrades und Downgrades Auftrag Websitesammlung Pläne

Sie können upgrade oder Downgrade für einen Auftrag Websitesammlung Plan jederzeit zwischen frei, Standard- und Premium-Pläne. Jedoch kann bei einer kostenlosen Auftrag Auflistung herunterstufen, das Downgrade für eine der folgenden Gründe fehlschlagen:

- Eine Auflistung von kostenlosen Auftrag bereits vorhanden ist, in dem Abonnement
- Eine Position in der Auflistung Position weist eine höhere Serie als für Projekte in kostenlosen Auftrag Websitesammlungen zulässig. Der maximale Serienoptionen in einer Sammlung von kostenlosen Auftrag zulässig ist einmal pro Stunde
- Mehr als 5 Aufgaben anstehen in der Auflistung Auftrag
- Ein Projekt in der Auflistung Position hat eine HTTP- oder HTTPS-Aktion, die mithilfe eines [http-ausgehende Autorisierung Objekt](scheduler-outbound-authentication.md)

## <a name="billing-and-azure-plans"></a>Abrechnung und Azure-Pläne

Abonnements unterliegen den Auftrag Websitesammlungen nicht kostenlos. Wenn Sie mehr als 100 standard Auftrag Websitesammlungen (10 standard Abrechnung Einheiten) haben, ist es eine bessere verfolgte aus, um alle Auftrag Websitesammlungen in den Plan Premium verfügen.

Wenn Sie eine Position standard-Sammlung und eine Premium Auftrag Websitesammlung verfügen, können Sie abgebucht ein standard Abrechnung Einheit _und_ eine Premium Abrechnung Einheit aus. Der Scheduler Service Rechnung basierend auf der Anzahl der aktiven Auftrag Websitesammlungen, die entweder Standard oder Premium festgelegt werden; Dies wird erläutert, in den beiden nächsten Abschnitten weiter.

## <a name="standard-billable-units"></a>Standard berechenbaren Einheiten

Eine berechenbare Standardeinheit kann bis zu 10 standard Auftrag Sammlungen enthalten. Da eine Auflistung standard Position bis zu 50 Stellen pro Websitesammlung Auftrag unterstützt, ermöglicht eine Abrechnung Standardeinheit ein Abonnement besitzen bis zu 500 Einzelvorgänge – bis zu fast 22 Millionen Auftrag Ausführungen pro Monat.

Wenn Sie zwischen 1 und 10 standard Auftrag Websitesammlungen verfügen, können Sie für 1 Abrechnung Standardeinheit Abrechnung. Wenn Sie zwischen 11 und 20 standard Auftrag Websitesammlungen haben, werden Ihnen 2 standard Abrechnung Einheiten berechnet. Wenn Sie zwischen 21 und 30 standard Auftrag Websitesammlungen haben, wird für 3 standard Abrechnung Einheiten in Rechnung gestellt werden, und so weiter aus.

## <a name="p10-premium-billable-units"></a>P10 Premium berechenbaren Einheiten

Eine P10 Premium berechenbare Einheit kann bis zu 10.000 P10 Premium Auftrag Websitesammlungen enthalten. Da eine P10 Premium Auftrag Auflistung bis zu 50 Stellen pro Websitesammlung Auftrag unterstützt, ermöglicht eine Premium Abrechnung Einheit ein Abonnement besitzen bis zu 500.000 Einzelvorgänge – bis zu fast 22 Milliarden Auftrag Ausführungen pro Monat.

Wenn Sie zwischen 1 und 10.000 Premium Auftrag Websitesammlungen haben, werden Ihnen 1 P10 Premium Abrechnung Einheit berechnet. Wenn Sie zwischen 10,001 und 20.000 Premium Auftrag Websitesammlungen haben, wird für 2 P10 Premium Abrechnung Einheiten in Rechnung gestellt werden, und so weiter aus.

Weise P10 Premium Auftrag Websitesammlungen die gleiche Funktionalität wie der standard-Position Websitesammlungen jedoch einen Umbruch Preis bereitstellen, für den Fall, dass die Anwendung viele Websitesammlungen Position erforderlich ist.

## <a name="p20-premium-billable-units"></a>P20 Premium berechenbaren Einheiten

Eine P20 Premium berechenbare Einheit kann bis zu 5.000 P20 Premium Auftrag Websitesammlungen einbeziehen. Da eine P20 Premium Auftrag Auflistung bis zu 1.000 Stellen pro Websitesammlung Auftrag unterstützt, ermöglicht eine Premium Abrechnung Einheit ein Abonnement besitzen bis zu 5.000.000 Einzelvorgänge – bis zu fast 220 Milliarden Auftrag Ausführungen pro Monat.

P20 Premium Auftrag Websitesammlungen bietet dieselben Funktionen wie P10 Premium Auftrag Websitesammlungen unterstützt aber auch eine Zahl größer Aufträge pro Websitesammlung Position und eine höhere Gesamtzahl der Einzelvorgänge insgesamt als P10 Premium zulassen, dass Sie weitere Skalierbarkeit besitzen.

## <a name="billing-and-active-status"></a>Abrechnung und aktiven Status

Position Websitesammlungen sind immer aktiv, es sei denn, Ihre gesamte Abonnement nicht mehr in einigen temporären deaktivierte Zustand aufgrund von Problemen Abrechnung angezeigt wurde. Die einzige Möglichkeit, um sicherzustellen, dass eine Auflistung Auftrag nicht fakturiert wird ist entweder die Sammlung Position zu löschen oder den Plan _Free_ festlegen.

Zwar können Sie alle Aufträge innerhalb einer Websitesammlung Position in einem einzigen Vorgang deaktivieren, nicht den den Status der Position Sammlung ändert – die Position Sammlung wird _immer noch_ in Rechnung gestellt werden. Ebenso leeren Auftrag Websitesammlungen als aktiv betrachtet und werden in Rechnung gestellt werden.

## <a name="pricing"></a>Preise

Preise, finden Sie in der [Scheduler Preise](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Siehe auch


 [Was ist der Scheduler?](scheduler-intro.md)

 [Azure Scheduler Konzepte und Terminologie Entitätshierarchie](scheduler-concepts-terms.md)

 [Erste Schritte mit Scheduler Azure-Portal](scheduler-get-started-portal.md)

 [Azure Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Bezug auf Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Azure Scheduler hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Grenzwerte für Azure Scheduler, Standardeinstellungen und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler ausgehende Authentifizierung](scheduler-outbound-authentication.md)
