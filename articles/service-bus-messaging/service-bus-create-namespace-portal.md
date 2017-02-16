<properties
    pageTitle="Erstellen eines Dienstbus Namespaces über das Azure-Portal | Microsoft Azure"
    description="Benötigen Sie einen Namespace, um mit Dienstbus zu beginnen zu können. So sieht so erstellen Sie eine mithilfe des Azure-Portals aus."
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="jotaub"/>

# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Erstellen eines Dienstbus Namespaces über das Azure-portal

Ein Namespace ist eine allgemeine Container für alle messaging-Komponenten. Mehrere Warteschlangen und Themen können in einem einzelnen Namespace befinden, und Namespaces dienen häufig als Anwendungscontainer. Es gibt derzeit zwei verschiedene Verfahren zum Erstellen eines Dienstbus Namespaces.

1.  Azure-Portal (in diesem Artikel)

2.  [Ressourcenmanager-Vorlagen][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Erstellen Sie einen Namespace Azure-Portal

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Herzlichen Glückwunsch! Sie haben nun einen Namespace Service Bus Messaging erstellt.

## <a name="next-steps"></a>Nächste Schritte

Schauen Sie sich unsere [GitHub Beispiele] [ github-samples] der anzeigen einige erweiterte Features Azure Service Bus Messaging.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples