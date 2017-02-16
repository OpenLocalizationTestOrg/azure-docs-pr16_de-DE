<properties
 pageTitle="Scheduler Grenzwerte Druck- und Standardeinstellungen"
 description="Scheduler Grenzwerte Druck- und Standardeinstellungen"
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

# <a name="scheduler-limits-and-defaults"></a>Scheduler Grenzwerte Druck- und Standardeinstellungen

## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Kontingente Scheduler, Grenzwerte, Standardeinstellungen und Drosselungen

[AZURE.INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>Die Kopfzeile X-ms-Anforderung-id

Jeder Anforderung anhand der Scheduler-Dienst gibt eine Antwort Kopfzeile mit dem Namen**X-ms-Anforderung-Id**an. Diese Kopfzeile enthält einen undurchsichtig zurück, der die Anforderung eindeutig.

Wenn eine Anforderung tritt immer ein Fehler, und Sie haben bestätigt, dass die Anforderung ordnungsgemäß formuliert wird, können Sie diesen Wert verwenden, den Fehler an Microsoft gemeldet. Enthalten Sie in den Bericht den Wert von X-ms-Anforderung-Id, die ungefähre Uhrzeit, die die Anforderung vorgenommen wurde, die den Bezeichner das Abonnement, Position Websitesammlung und/oder Position und den Typ des Vorgangs, die die Anfrage zu übermitteln.

## <a name="see-also"></a>Siehe auch


 [Was ist der Scheduler?](scheduler-intro.md)

 [Azure Scheduler Konzepte und Terminologie Entitätshierarchie](scheduler-concepts-terms.md)

 [Erste Schritte mit Scheduler Azure-Portal](scheduler-get-started-portal.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [Azure Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Bezug auf Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Azure Scheduler hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Azure Scheduler ausgehende Authentifizierung](scheduler-outbound-authentication.md)
