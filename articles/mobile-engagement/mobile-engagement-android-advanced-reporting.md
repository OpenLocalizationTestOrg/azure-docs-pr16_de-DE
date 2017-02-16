<properties
    pageTitle="Erweiterte Optionen für Azure Mobile Engagement Android SDK"
    description="Beschreibt, wie Sie erweiterte reporting an Analytics Azure Mobile Engagement Android SDK erfassen führen"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-engagement-on-android"></a>Erweiterte Reporting mit Engagement auf Android-Gerät

> [AZURE.SELECTOR]
- [Universal Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Silverlight für Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

In diesem Thema werden zusätzliche reporting-Szenarien in Ihrem Android-Anwendung. Sie können diese Optionen auf die app in das [Erste Schritte](mobile-engagement-android-get-started.md) -Lernprogramm erstellt anwenden.

## <a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

Des Lernprogramms gestellten wurde absichtlich direkten und einfache, aber es werden von Erweiterte Optionen, die Sie auswählen können.

## <a name="modifying-your-activity-classes"></a>Ändern der `Activity` Klassen

In der [Erste Schritte-Lernprogramm](mobile-engagement-android-get-started.md)alle war erforderlich war, Ihre `*Activity` untergeordneten Klassen erben von der zugehörigen `Engagement*Activity` Klassen. Wenn Ihre legacy Aktivität erweiterten beispielsweise `ListActivity`, möchten Sie es erweitern vornehmen `EngagementListActivity`.

> [AZURE.IMPORTANT] Bei Verwendung von `EngagementListActivity` oder `EngagementExpandableListActivity`, vergewissern Sie sich einwählen eine `requestWindowFeature(...);` besteht, bevor Sie den Anruf an `super.onCreate(...);`, andernfalls einem Absturz.

Sie finden diese Klassen in die `src` Ordner, und Sie können in Ihr Projekt kopieren. Die Klassen sind auch in der **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternative Methode: anrufen `startActivity()` und `endActivity()` manuell

Wenn Sie nicht oder nicht überladen möchten Ihre `Activity` Klassen, können Sie stattdessen starten und beenden Sie Ihre Aktivitäten durch Aufrufen von der `EngagementAgent`des Methoden direkt.

> [AZURE.IMPORTANT] Das Android SDK nie Ruft die `endActivity()` Methode, selbst wenn die Anwendung geschlossen ist (auf Android Applikationen werden nie geschlossen). Daher ist es *dringend* empfohlen, rufen Sie die `startActivity()` Methode in der `onResume` Rückruf des *Alle* Ihrer Aktivitäten, und die `endActivity()` Methode in der `onPause()` Rückruf des *Alle* Ihre Aktivitäten. Dies ist die einzige Möglichkeit, um sicherzustellen, dass Sitzungen nicht weitergegeben werden. Wenn eine Sitzung offengelegt werden, trennt die Engagement Dienst nie das Engagement Back-End-(seit der Dienst verbundenen bleibt, solange eine Sitzung aussteht).

Hier ist ein Beispiel:

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

In diesem Beispiel ist ähnlich wie die `EngagementActivity` Klasse und seine Varianten, deren Quellcode Sie unter finden der `src` Ordner.

## <a name="using-applicationoncreate"></a>Verwenden von Application.onCreate()

Sie in setzen Code `Application.onCreate()` und in anderen Anwendung Rückrufe für alle Ihrer Anwendung Prozesse, einschließlich des Engagement Diensts ausgeführt wird. Es möglicherweise unerwünschte Seite Effekte wie nicht benötigte Arbeitsspeicher Zuweisungen und Nachrichtenfäden im Rahmen des Projekts Prozess, oder doppelte übertragenen Empfänger oder Dienste.

Wenn Sie außer Kraft setzen `Application.onCreate()`, wir empfehlen hinzufügen die folgenden Codeausschnitts am Anfang Ihrer `Application.onCreate()` (Funktion):

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

Führen Sie dieselben Schritte für `Application.onTerminate()`, `Application.onLowMemory()`, und `Application.onConfigurationChanged(...)`.

Sie können auch erweitern `EngagementApplication` statt verlängern `Application`: der Rückruf `Application.onCreate()` bedeutet das Kontrollkästchen Prozess und Anrufe aufgeführt. `Application.onApplicationProcessCreate()` nur der aktuelle Prozess nicht das Schema der Engagement Hostingdienst ist, die denselben Regeln für den anderen Rückruf anwenden.

## <a name="tags-in-the-androidmanifestxml-file"></a>Tags in der Datei AndroidManifest.xml

In der Service-Tag in der Datei AndroidManifest.xml die `android:label` Attribut ermöglicht es Ihnen, wie er im Bildschirm "Services ausgeführt" von ihrem Telefon Endbenutzern angezeigt wird, wählen Sie den Namen des Diensts Engagement. Es wird empfohlen, wenn dieses Attribut auf `"<Your application name>Service"` (z. B. `"AcmeFunGameService"`).

Angeben der `android:process` Attribut wird sichergestellt, dass der Engagement Dienst ausgeführt, in einem eigenen Prozess wird (als ausführen, Engagement im gleichen Prozess Ihrer Anwendung Ihrer primär/UI-Thread potenziell weniger reagiert).

## <a name="building-with-proguard"></a>Erstellen mit ProGuard

Wenn Sie Ihre Anwendungspaket mit ProGuard erstellen, müssen Sie einige Klassen beibehalten. Sie können den folgenden Konfiguration Codeausschnitt verwenden:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
    }
