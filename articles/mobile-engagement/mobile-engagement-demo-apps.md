<properties
    pageTitle="Azure Mobile Engagement demoApp | Microsoft Azure"
    description="Beschreibt, wo Sie herunterladen können, wie verwenden und die Vorteile von Azure Mobile Engagement demoApp"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/10/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-demo-app"></a>Azure Mobile Engagement Demo-app

Wir haben eine Azure Mobile Engagement demoApp für **iOS**, **Android**und **Windows** -Plattformen mit deren Hilfe Sie nützliche Ressourcen suchen, und erfahren mehr über Mobile Engagement veröffentlicht.

Die app können Sie Folgendes ausführen:

- Finden Sie hilfreiche Links zu Mobile Engagement Ressourcen wie Videos, Dokumentation, Support-Forum und wo auslösen Featureanfragen problemlos.
- Erfahrung Stichprobe Benachrichtigungen, die durch Mobile Engagement abzurufenden Ideen für Ihre eigenen mobilen Anwendungen unterstützt werden.
- Wie Mobile Engagement in Ihrer eigenen app implementiert untersuchen verwenden Sie eine Implementierung Bezug. Sie können zu erfahren:

    - Analytics-Daten zu sammeln.
    - Erweiterte Benachrichtigungsszenarien Typen z. B. *interstitielles Vollbildmodus* oder *Popup*zu implementieren.
    - Implementieren Sie Umfragen und Umfragen.
    - Automatische Pushbenachrichtigungen Daten und Pushbenachrichtigungen Szenarien zu implementieren.   

## <a name="app-installation"></a>App-installation
Diese app ist in den folgenden app speichern verfügbar:

- **Windows-universeller demoApp**:

    - Herunterladen der app im [Windows-App zu speichern](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).
    - Die app wurde als Windows 10 universeller app entwickelt. Der Quellcode ist auf [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows)verfügbar.

- **demo iOS-app**:

    - Herunterladen der app im [Apple zu speichern](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090).
    - Die app wurde in iOS Swift entwickelt. Der Quellcode ist auf [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios)verfügbar.

- **Android demoApp**:

    - Herunterladen der app bei [Google Play speichern](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement).
    - Der Quellcode ist auf [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android)verfügbar.

![Windows-universeller Demo-app][1]

![demo-app für iOS][2]
![Android demoApp][3]


## <a name="usage"></a>Verwendung

Sie können diese app wie folgt verwenden:

**Herunterladen der app auf Ihrem Gerät aus der Anwendung speichern (zuvor bereitgestellten) Links:**

>[AZURE.IMPORTANT] Sie benötigen keiner Azure Firma oder müssen die app mit einem Back-End verbinden. Die app funktioniert unabhängig.

- Nachdem Sie die app auf Ihrem Gerät haben, können Sie über die Links im Menü links, um die nützlichen Ressourcen zu Mobile Engagement finden Sie unter wechseln.
- Damit Sie immer über die neuesten Produktupdates aktualisiert haben haben wir den [Dienst des RSS-Feeds](https://aka.ms/azmerssfeed) in dieser Anwendung hinzugefügt.
- Sie können auch wechseln, bis die Benachrichtigung Beispielszenarien auf den Typ der Benachrichtigungen auftreten, die von Mobile Engagement für jede Plattform unterstützt werden. Diese Benachrichtigungen können werden erfahrener lokal –, die ist, können Sie die Schaltflächen auf der Bildschirme, um Sie mit der Benachrichtigungen anzeigen, die zum Senden der Benachrichtigungen aus der Mobile Engagement Plattform identisch ist klicken.

![Menü der App für Windows][4]

![Menü der App für iOS][5]
![App-Menü für Android][6]

**Herunterladen des Quellcodes aus den GitHub Links (früher bereitgestellt):**

- Nachdem Sie den Quellcode heruntergeladen haben, öffnen Sie sie in der jeweiligen Entwicklungsumgebung – XCode für iOS, Android Studio für Android und Visual Studio für Windows.
- Als Nächstes befolgen unsere [grundlegenden SDK Integration Schritte](mobile-engagement-windows-store-dotnet-get-started.md) Sie, dass Sie die Verbindung herstellen, diese app zu seiner eigenen Mobile Engagement Back-End-Instanz sind.
    - Sie müssen eine Verbindungszeichenfolge in der app zu konfigurieren.
    - Sie müssen außerdem die Pushbenachrichtigungen Benachrichtigung Plattform für Ihre app zu konfigurieren.
- Sie sehen, dass die app ähneln mit Mobile Engagement instrumentiert wird. Daher, wie Sie nach der Verbindung mit dem Back-End die app zu öffnen, werden Sie kann der Benutzer Sitzung, Aktivitäten, Ereignisse, usw., auf der Registerkarte **Monitor** sehen werden.
- Sie können auch Benachrichtigungen an diese app aus Ihrer eigenen Mobile Engagement Instanz, anstelle von lokalen Benachrichtigungen senden sein.
    - Hier können Sie das Menüelement **erhalten Sie die Geräte-ID** in der app mit Ihrem Gerät als Testgerät hinzufügen. So können Sie eine Geräte-ID, die Sie dann als Testgerät mit Ihrer Plattform Back-End-Instanz registrieren.

    ![Geräte-ID unter Windows][7]

    ![Geräte-ID unter iOS][8]
    ![Geräte-ID auf Android-Gerät][9]

## <a name="key-features-of-the-demo-app"></a>Hauptfeatures der demoApp

- Mit dieser app erwähnt, haben Sie alle wichtigen Ressourcen für Mobile Engagement in Ihrer Hand. Sie können über die Links im linken Menü wechseln.

- Erleben Sie-von-app-Benachrichtigungen für jede Plattform. Diese Benachrichtigungen können übermittelt werden als **nur Benachrichtigung**, in dem Klicken auf die Benachrichtigung einfach von einem systemeigenen Bildschirm der Anwendung öffnet (mithilfe von **Tiefe verknüpfen**) – oder eine **Web Ankündigung**, wo Sie vorführen zusätzliche HTML-Inhalt vom Mobile Engagement Back-End beim Klicken auf die Benachrichtigung angezeigt.

    ![-Von-app-Benachrichtigungen][29]

- Unter iOS müssen Sie die app, um Pushbenachrichtigungen Out-von-app oder im externen System finden Sie unter zu schließen. Sie können schauen Sie sich die Implementierung zum Hinzufügen von **interaktive Schaltflächen**, wie diejenigen, die diese Benachrichtigung Out-von-app für *Feedback-* und *Freigeben* (so, dass der Benutzer die eigentliche Benachrichtigung rechts Aktion ausführen kann) hinzugefügt wurden.

    ![-Von-app-Benachrichtigungen unter iOS][11] ![Benachrichtigung-von-app-Anzeige auf iOS][14]

- Auf Android werden die Optionen, die unterstützt werden mehrzeiligen Text (**Text Groß**) oder ein Bild Benachrichtigung (**Überblick**) auf die Benachrichtigung, sowie die **interaktive Schaltflächen** hinzufügen (wie vom iOS unterstützt).

    ![-Von-app-Benachrichtigungen auf Android-Gerät][12] ![-Von-app-Benachrichtigung anzeigen auf Android-Gerät][15]

- Auf Windows-10 können Sie sehen, wie die Benachrichtigungen auf dem PC aussehen. Diese Benachrichtigung wird auch in der Windows-10- **Benachrichtigung Center**. Es gibt keine Unterstützung für **interaktive Schaltflächen** im Moment im Windows SDK hinzufügen.

    ![-Von-app-Benachrichtigungen unter Windows][10] ![Anzeigen von Out-von-app unter Windows][13]

- Erleben Sie Standard "in app" Benachrichtigungen für jede Plattform. Dies ist eine Benutzeroberfläche in zwei Schritten, wo eine **Benachrichtigung** Fenster zum ersten Mal angezeigt. Wenn Sie darauf klicken, angezeigt wird es als Vollbild **Ankündigung**, wie im folgenden Screenshot angezeigt. Der Inhalt dieser Ankündigung entnommen Ihre Mobile Engagement Back-End-Instanz aus. Das SDK enthält Vorlagen für Benachrichtigungen und Ankündigungen. Sie können diese einfach anpassen, siehe diese demoApp mit der Erweiterung unserer Logo und eine Farbe ändern möchten.  

    ![In der app-Benachrichtigungen unter Windows][16]

    ![In der app-Benachrichtigungen unter iOS][17]  ![In der app-Benachrichtigungen auf Android-Gerät][18]

    **iOS**, **Android**

- Das Feature " **Kategorie** " eines Auftrags für Mobile können Sie auch diese Standard-Benutzeroberfläche anpassen. In der demoApp haben wir zwei gängigen Verfahren zum Ändern der Benutzeroberfläche der Benachrichtigungen veranschaulicht. Beachten Sie, dass das Feature "Kategorie" im Windows SDK noch nicht unterstützt wird.

    **Vollbildmodus interstitielles:**

    ![Benachrichtigung über rein innerhalb der app - interstitielles Kategorie][30]

    ![Interstitielles Kategorie unter iOS][21]  ![Interstitielles Kategorie auf Android-Gerät][22]

    **Popupbenachrichtigung:**

    ![Benachrichtigung über rein innerhalb der app - Popupmenü Kategorie][31]

    ![Popupbenachrichtigung unter iOS][19]   ![Popupbenachrichtigung auf Android-Gerät][20]

**iOS**, **Android**

- Mobile Engagement unterstützt auch einen speziellen Typ von **Umfragen**aufgerufen innerhalb der app-Benachrichtigung an. So können Sie Ihren Benutzern segmentierter app schnelle Umfragen versenden. Sie können die Fragen und die Optionen für jede Frage wie in den folgenden Screenshot hinzufügen. Klicken Sie dann abrufen eine Benachrichtigung in der app, um die app-Benutzer erscheint auch als.   

    ![Umfrage Benachrichtigungen][32]

    ![Umfrage unter Windows][26]

    ![Umfrage unter iOS][27]   ![Umfrage auf Android-Gerät][28]

**iOS**, **Android**

- Mobile Engagement unterstützt auch im Hintergrund **Daten** Pushbenachrichtigungen zu senden. Mit dieser Benachrichtigungen, senden Sie Daten aus dem Dienst (wie die JSON-Daten im folgenden Beispiel), die Sie in Ihrer app zu behandeln und einige agieren können. Ein Beispiel ist wie wir den Preis eines Elements Selektives ändern möchten, mithilfe von Pushbenachrichtigungen Daten.

    ![Der Pushbenachrichtigung von Daten][33]

    ![Daten der Pushbenachrichtigung unter Windows][23]

    ![Daten der Pushbenachrichtigung unter iOS][24]  ![Daten der Pushbenachrichtigung auf Android-Gerät][25]

**iOS**, **Android**

> [AZURE.NOTE] Sie können detaillierte eine schrittweise Anleitung zum eine der folgenden Benachrichtigungen, indem Sie **hier klicken, um Anweisungen zum Senden dieser Benachrichtigungen Mobile Engagement Plattform** auf einem beliebigen Bildschirm der Stichprobe Benachrichtigung anzeigen.


[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png
