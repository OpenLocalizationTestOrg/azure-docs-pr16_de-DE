<properties
    pageTitle="Integration von Azure Mobile Engagement Web SDK | Microsoft Azure"
    description="Die neuesten Updates und Verfahren zum Microsoft Azure Mobile Engagement Web SDK"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="02/29/2016"
    ms.author="piyushjo" />

#<a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Integrieren von Azure Mobile Engagement in einer Webanwendung

> [AZURE.SELECTOR]
- [Windows-Dienst](mobile-engagement-windows-store-integrate-engagement.md)
- [Silverlight für Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Die Verfahren in diesem Artikel werden die einfachste Methode zum Aktivieren der Analytics und Überwachungsfunktionen in Azure Mobile Engagement in der Webanwendung beschrieben.

Führen Sie die Schritte aus, um das Protokoll meldet aktivieren, die erforderlich sind, um alle Statistiken zu Benutzer, Sitzungen, Aktivitäten, stürzt ab und Technicals zu berechnen. Für die Anwendung-abhängige Statistik, wie Ereignisse, Fehler und Projekte müssen Sie mithilfe der Azure Mobile Engagement-API manuell Log Berichte aktivieren. Weitere Informationen erfahren Sie, [wie Sie erweiterte Mobile Projektdauer tagging-API in einer Webanwendung verwenden](mobile-engagement-web-use-engagement-api.md).

## <a name="introduction"></a>Einführung

[Im Web Azure mobilen Engagement SDK herunterladen](http://aka.ms/P7b453).
Die Mobile Engagement Web SDK wird als eine einzelne JavaScript-Datei geliefert Azure-engagement.js, Sie haben in jeder Seite Ihrer Website oder Web-Anwendung aufnehmen möchten.

> [AZURE.IMPORTANT] Bevor Sie dieses Skript ausführen, müssen Sie einen Code oder Skripts Ausschnitt ausführen, den Sie schreiben Mobile Engagement für eine Anwendung zu konfigurieren.

## <a name="browser-compatibility"></a>Browserkompatibilität

Die Mobile Engagement Web SDK verwendet systemeigene JSON Codierung und Decodierung, sowie Domain-übergreifende AJAX-Anfragen (auf der Spezifikation W3C CORS verlassen). Es ist mit den folgenden Browsern kompatibel:

* Microsoft Kante 12 +
* InternetExplorer 10 und
* Firefox 3.5 +
* Chrome 4 +
* Safari 6 +
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Konfigurieren von mobilen Engagement

Schreiben Sie ein Skript, das eine globale erstellt `azureEngagement` JavaScript-Objekt, wie im folgenden Beispiel gezeigt. Da es möglicherweise in Ihrer Website mit Vielfachen Seiten, wird angenommen, dass dieses Skript in jeder Seite enthalten ist. In diesem Beispiel wird das JavaScript-Objekt namens `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

Die `connectionString` Wert für eine Anwendung Azure-Portal angezeigt wird.

> [AZURE.NOTE] `appVersionName`und `appVersionCode` sind optional. Wir empfehlen jedoch Sie diese so konfigurieren, dass Analytics Versionsinformationen verarbeitet werden können.

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Mobile Engagement Skripts in Ihre Seiten aufnehmen
Hinzufügen von Mobile Engagement Skripts auf Ihre Seiten auf eine der folgenden Methoden:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Oder:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Alias

Nachdem das Skript Mobile Engagement Web SDK geladen wurde, wird den **Engagement** Alias Zugriff auf die APIs SDK erstellt. Sie können diesen Alias SDK-Konfiguration definieren. Dieser Alias wird als Referenz in dieser Dokumentation verwendet.

Beachten Sie, dass wenn das standardmäßige Alias einen mit einem anderen globale Variable auf Ihrer Seite Konflikt, Sie sie in der Konfiguration wie folgt definieren können, bevor Sie die Mobile Engagement Web SDK laden:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Grundlegende Berichterstattung

Grundlegende Berichterstattung in Mobile Engagement behandelt Sitzung Ebene Statistiken, wie z. B. Statistiken über Benutzer, Sitzungen, Aktivitäten und stürzt ab.

### <a name="session-tracking"></a>Sitzung nachverfolgen

Ist eine Sitzung Mobile Engagement in eine Abfolge von Aktivitäten unterteilt, die jeweils durch einen Namen identifiziert.

In einer klassischen Website empfiehlt es sich, dass Sie eine andere Aktivität auf jeder Seite der Website deklarieren. Für eine Website oder Web-Anwendung, bei der die aktuelle Seite nie geändert wird, sollten Sie die Aktivitäten in kleinerem Maßstab, z. B. innerhalb der Seite zu verfolgen.

Rufen Sie in beiden Fällen starten, oder ändern die aktuellen Aktivitäten von Benutzern, die `engagement.agent.startActivity` (Funktion). Beispiel:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

Der Mobile Engagement Server endet automatisch eine offene Sitzung innerhalb von drei Minuten nach dem Schließen der Anwendungsseite.

Sie können auch Sie können beenden eine Sitzung manuell durch Aufrufen von `engagement.agent.endActivity`. Dadurch werden die aktuellen Aktivitäten von Benutzern auf 'im Leerlauf 'gesetzt.  Beenden die Sitzung 10 Sekunden später, wenn eine neue Verbindung für `engagement.agent.startActivity` die Sitzung von Lebensläufen.

Sie können die Verzögerung von 10 Sekunden in der globalen Engagement-Objekt, wie folgt konfigurieren:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [AZURE.NOTE] Können keine `engagement.agent.endActivity` in der `onunload` Rückruf, weil Sie zu diesem Zeitpunkt AJAX-Aufrufe sind nicht möglich.

## <a name="advanced-reporting"></a>Erweiterte Berichte

Wenn Sie anwendungsspezifische Ereignisse, Fehler und Einzelvorgänge melden möchten, müssen Sie optional die Mobile Engagement-API verwenden. Sie zugreifen auf das Mobile Engagement API, über die `engagement.agent` Objekt.

Sie können alle erweiterten Funktionen in Mobile Engagement in der Mobile Engagement-API zugreifen. Die API wird im Artikel [zum Verwenden der erweiterten Mobile Engagement tagging-API in einer Webanwendung](mobile-engagement-web-use-engagement-api.md)ausführlich beschrieben.

## <a name="customize-the-urls-used-for-ajax-calls"></a>Passen Sie die URLs für AJAX-Aufrufe verwendet

Sie können URLs anpassen, die die Mobile Engagement Web SDK verwendet. Um das Protokoll-URL (den Endpunkt des SDK für die Protokollierung) neu zu definieren, können Sie beispielsweise die Konfiguration wie folgt überschreiben:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Wenn Ihre Funktionen URL eine Zeichenfolge, die zurück mit beginnt `/`, `//`, `http://`, oder `https://`, das Standardschema wird nicht verwendet. Standardmäßig die `https://` Schema für diese URLs verwendet wird. Wenn Sie das Standardschema anpassen möchten, überschreiben Sie die Konfiguration, wie folgt aus:

    window.azureEngagement = {
      ...
      urls: {
        ...      
        scheme: '//'
      }
    };
