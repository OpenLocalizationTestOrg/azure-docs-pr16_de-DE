<properties
    pageTitle="Azure Mobile Engagement Web SDK Upgrade Verfahren | Microsoft Azure"
    description="Die neuesten Updates und Verfahren für das Web SDK für Azure Mobile Engagement"
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
    ms.date="06/07/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a>Azure Mobile Engagement Web SDK Upgrade-Verfahren

Wenn Sie bereits eine frühere Version des Azure Mobile Engagement Web SDK in die Webanwendung integriert haben, müssen Sie die folgenden Punkte berücksichtigen, wenn Sie das SDK aktualisieren.

Wenn Sie mehrere Versionen des Mobile Engagement Web SDK übersprungen haben, müssen Sie während des Upgrades mehrere Verfahren abschließen zu können. Beispielsweise, wenn Sie in 1.6.0 1.4.0 migrieren, zuerst führen Sie die Schritte zum Aktualisieren von 1.4.0 auf 1.5.0. Befolgen Sie die Verfahren zum Aktualisieren von 1.5.0 auf 1.6.0 an.

Egal Version, die Sie aktualisieren aus eine frühere Version von der Datei Azure-engagement.js ersetzen, mit der neuesten Version der Datei.

## <a name="upgrade-from-121-to-200"></a>Aktualisieren von 1.2.1 auf 2.0.0

In diesem Abschnitt beschrieben, wie eine Mobile Engagement Web SDK Integration von Capptain Dienst angebotenen Capptain SAS, um eine app Azure Mobile Engagement migrieren. Wenn Sie von einer früheren Version migrieren, wenden Sie sich die Capptain-Website, um zuerst zur 1.2.1 migrieren, und wenden Sie die folgenden Verfahren.

Diese Version des Mobile Engagement Web SDK unterstützt keine Samsung Smartphone, Opera TV, WebOS oder das Feature Reichweite.

>[AZURE.IMPORTANT] Capptain und Azure Mobile Engagement sind nicht denselben Dienst. Im folgende Verfahren werden nur zum Migrieren der app Client hervorgehoben. Migrieren der Mobile Engagement Web SDK in der app wird nicht Ihre Daten migrieren von einem Server Capptain auf einem Server Mobile Engagement.

### <a name="javascript-files"></a>JavaScript-Dateien

Ersetzen Sie die Datei Capptain-sdk.js mit der Datei Azure-engagement.js und aktualisieren Sie Skript Importvorgänge entsprechend zu.

### <a name="remove-capptain-reach"></a>Entfernen von Capptain Reichweite

Diese Version des Mobile Engagement Web SDK unterstützt nicht das Feature Reichweite. Wenn Sie in Ihrer Anwendung Capptain Reichweite integriert, müssen Sie ihn entfernen.

Entfernen Sie des Importvorgangs erreichen CSS auf Ihrer Seite, und löschen Sie die zugehörigen CSS-Datei (standardmäßig Capptain-reach.css).

Löschen Sie die folgenden Ressourcen für Reichweite: das Schließen Bild (standardmäßig Capptain-close.png) und das Markensymbol (Capptain--Benachrichtigungssymbol, standardmäßig).

Die Benutzeroberfläche erreicht haben, für das innerhalb der app-Benachrichtigungen zu entfernen. Das Standardlayout sieht wie folgt aus:

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

Entfernen der Benutzeroberfläche erreichen für Text und Web Ankündigungen und Umfragen. Das Standardlayout sieht wie folgt aus:

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

Entfernen der `reach` Objekt aus der Konfiguration, sofern vorhanden. Es sieht wie folgt aus:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Entfernen Sie alle anderen Reichweite Anpassung, z. B. Kategorien.

### <a name="remove-deprecated-apis"></a>Entfernen Sie veraltete APIs

Einige APIs von Capptain sind im SDK Web Engagement Mobile nicht mehr unterstützt.

Entfernen Sie alle Anrufe an die folgenden APIs: `agent.connect`, `agent.disconnect`, `agent.pause`, und `agent.sendMessageToDevice`.

Entfernen Sie alle Instanzen von den folgenden Rückruf aus der Konfiguration Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, und `onPushMessageReceived`.

### <a name="configuration"></a>Konfiguration

Mobile Engagement wird mithilfe eine Verbindungszeichenfolge SDK Bezeichnern, beispielsweise die Bezeichner der Anwendung konfiguriert.

Ersetzen Sie die Anwendung-ID, mit der Verbindungszeichenfolge. Notiz, die das globale Objekt für die SDK-Konfiguration von ändert `capptain` auf `azureEngagement`.

Vor der Migration:

    window.capptain = {
      appId: ...,
      [...]
    };

Nach der Migration:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

Die Verbindungszeichenfolge für eine Anwendung wird im Portal Azure angezeigt.

### <a name="javascript-apis"></a>JavaScript-APIs

Globale JavaScript-Objekt `window.capptain` wurde umbenannt `window.azureEngagement` , aber Sie können die `window.engagement` Alias für API-Aufrufe. Sie können den Alias SDK-Konfiguration definieren.

Beispielsweise `capptain.deviceId` wird `engagement.deviceId`, `capptain.agent.startActivity` wird `engagement.agent.startActivity`und so weiter.
