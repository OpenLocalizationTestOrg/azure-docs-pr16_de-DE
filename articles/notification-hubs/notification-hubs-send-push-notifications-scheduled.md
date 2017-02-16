<properties
    pageTitle="So geplante Benachrichtigungen senden | Microsoft Azure"
    description="In diesem Thema werden Benachrichtigungen geplant mit Azure Benachrichtigung Hubs verwenden."
    services="notification-hubs"
    documentationCenter=".net"
    keywords="Pushbenachrichtigungen, der Pushbenachrichtigung, Planung von Pushbenachrichtigungen"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="how-to-send-scheduled-notifications"></a>Gewusst wie: Senden Benachrichtigungen geplant


##<a name="overview"></a>(Übersicht)

Wenn Sie ein Szenario haben, in dem Sie eine Benachrichtigung zu einem bestimmten Zeitpunkt in der Zukunft senden möchten, aber haben keinen auf einfache Weise Reaktivieren von Back-End-Code zum Senden der Benachrichtigung. Standard-Ebene Benachrichtigung Hubs unterstützt ein Feature, das Sie Benachrichtigungen von 7 Tage in der Zukunft planen kann.

Wenn eine Benachrichtigung gesendet wird, verwenden Sie die [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) Klasse einfach im Infobereich Hubs SDK fest, wie im folgenden Beispiel gezeigt:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Darüber hinaus können Sie eine zuvor geplante Benachrichtigung mit seiner NotificationId kündigen:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Es gibt keine Grenzwerte für die Anzahl der geplanten Benachrichtigungen, die Sie senden können.