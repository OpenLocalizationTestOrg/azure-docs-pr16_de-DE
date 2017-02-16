<properties
   pageTitle="Webdienst Skalierung | Microsoft Azure"
   description="Erfahren Sie, wie einen Webdienst skalieren, indem zunehmender Parallelität und Hinzufügen von neuen Endpunkten."
   services="machine-learning"
   documentationCenter=""
   authors="neerajkh"
   manager="srikants"
   editor="cgronlun"
   keywords="Azure maschinellen learning, Web Services, Operationalization, Skalierung, Endpunkt, Parallelität"
   />
<tags
   ms.service="machine-learning"
   ms.devlang="NA"
   ms.workload="data-services"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.date="10/05/2016"
   ms.author="neerajkh"/>

# <a name="scaling-a-web-service"></a>Skalierung von einem Webdienst

>[AZURE.NOTE] In diesem Thema werden die Techniken zu einem klassischen maschinellen Learning-Webdienst verfügbar. 

Standardmäßig jeden veröffentlichten Webdienst 20 gleichzeitige Anforderungen Unterstützung konfiguriert ist und so weit wie 200 gleichzeitige Anforderungen werden kann. Während das Azure klassische Portal eine Möglichkeit zum Festlegen dieser Wert bereitstellt, Azure maschinellen Learning optimiert automatisch die Einstellung für optimale Leistung für Ihren Webdienst bereitstellen und der Portalseite Wert wird ignoriert. 

Wenn Sie planen, rufen Sie die API mit einer höheren Last als Wert Max gleichzeitige Anrufe 200 unterstützt, sollten Sie mehrere Endpunkte auf den gleichen Webdienst erstellen. Sie können Ihre laden dann zufällig für alle Konten verteilen.

## <a name="add-new-endpoints-for-same-web-service"></a>Hinzufügen von neuen Endpunkten für den gleichen Webdienst

Die Skalierung eines Webdiensts ist eine häufige Aufgabe. Einige Gründe für die skalieren sind maximal 200 gleichzeitige Anforderungen unterstützen, höhere Verfügbarkeit über mehrere Endpunkte oder separate Endpunkte für den Webdienst bereitstellt. Sie können die Dezimalstellen erhöhen, indem Sie zusätzliche Endpunkte für den gleichen Webdienst bis [Azure klassischen Portal](https://manage.windowsazure.com/) oder im Portal [Azure maschinellen Learning-Webdienst](https://services.azureml.net/) hinzufügen.

Weitere Informationen zum Hinzufügen von neuen Endpunkten finden Sie unter [Erstellen von Endpunkten](machine-learning-create-endpoint.md).

Lassen Sie denken Sie daran, die mit hoher Parallelität Anzahl beeinträchtigen, wenn Sie nicht die API mit einem entsprechend hohen Stundensatz anrufen werden können. Möglicherweise gelegentliche Zeitlimit und/oder Spitzen in der Wartezeit angezeigt, wenn Sie ein wenig auf eine API für hoher Auslastung konfiguriert belasten.

Die synchroner APIs werden in der Regel in Situationen verwendet, wo eine niedrige Wartezeit gewünscht wird. Hier Wartezeit impliziert die Zeit, die sie für die-API zum Ausführen einer Anforderung akzeptiert und nicht für alle Netzwerkprobleme zu berücksichtigen. Angenommen, Sie haben eine API mit einer 50-ms Wartezeit. Um die verfügbare Kapazität mit Einschränkung Ebene hoch und Max gleichzeitige Anrufe vollständig nutzen = 20; Sie müssen diese API 20 * 1000 aufrufen / 50 = 400 times pro Sekunde. Erweitern diese weiteren, ermöglicht eine Max gleichzeitige Anrufe von 200 rufen Sie die API 4000 Zeiten pro Sekunde, unter der Voraussetzung einer 50-ms Wartezeit.

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
