<properties
   pageTitle="Verwenden des HTTP Zuhörer und Verbinder in Logik Apps | Microsoft Azure-App-Verwaltungsdienst "
   description="So erstellen und konfigurieren Sie die Zuhörer HTTP und HTTP Aktion Verbinder oder API-app und in einer app Logik in Azure-App-Dienst verwenden"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="08/31/2016"
   ms.author="prkumar"/>


# <a name="get-started-with-the-http-listener-and-http-action-and-add-it-to-your-logic-app"></a>Erste Schritte mit dem Zuhörer HTTP und HTTP-Aktion, und fügen Sie es zu Ihrer Anwendung Logik

> [AZURE.NOTE]Wir werden die Unterstützung für diesen Connector endet, da deren Funktionalität jetzt standardmäßig als **manuellen Trigger enthalten ist** bei der Erstellung der neuen Logik apps.  Es empfiehlt sich, alle Ihre Logik apps zu aktualisieren, die diesen Connector verwenden.  
> Diese Version des Artikels gilt Logik apps 2014-12-01-Schema Vorschauversion aus.

Verbinden Sie direkt auf HTTP-Ressourcen zum Empfangen von HTTP-Anfragen sowie HTTP-Web-Anfragen konfigurieren. Es gibt einige Szenarien, wo Sie möglicherweise für die Arbeit mit direkter HTTP-Verbindungen, einschließlich müssen:

1.  Eine Logik App entwickeln möchte, die ein Web oder ein mobiler Benutzer interaktive front-End unterstützt.
2.  Zum Abrufen und Verarbeiten von Daten von einem Webdienst, der einen außerhalb des Feld Verbinder aufweist.
3.  Für die Durchführung von Aktionen, die bereits als Webdienst verfügbar gemachten, aber nicht als app API verfügbar sind.

Für diese Szenarios gibt es zwei Optionen:

1. **HTTP-Zuhörer**: diesen Connector fungiert als Trigger und wartet auf HTTP-Anfragen auf einen Endpunkt konfiguriert. Wenn für den Endpunkt konfiguriert ein Anruf empfangen wird, eine neue Instanz des Flusses auslöst und übergibt die Daten in die Anfrage des Ablaufs für die Verarbeitung von erhaltenen. Sie können auch konfiguriert werden automatisch auf die eingehende Anforderung reagieren, wenn illustrieren gestartet hat, oder können Sie eine Antwort auf Grundlage der Ausführung Fluss erstellen und Senden einer Antwort an den Anrufer.
2. **HTTP-Aktion**: Dies können Sie die Anforderung einer Web an einem beliebigen Endpunkt verfügbar im Internet konfigurieren, ruft wieder eine Antwort, und es kann für weitere Aktionen im Fluss zu nutzen.

Logik apps können auslösen basierend auf einer Vielzahl von Datenquellen und Angebot Verbinder zum Abrufen und Verarbeiten von Daten als Teil des Ablaufs. Sie können Ihre Geschäftsdaten Workflow und Prozess als Teil dieser Workflow innerhalb einer App Logik HTTP-Connector hinzufügen. 

## <a name="creating-an-http-listener-for-your-logic-app"></a>Erstellen einer HTTP-Zuhörer für Ihre App Logik
Ein Verbinder innerhalb einer Logik app erstellt werden kann oder direkt aus dem Azure Marketplace erstellt werden. So erstellen Sie einen Verbinder aus dem Marketplace  

1. Wählen Sie in der Azure Startboard **Marketplace**aus.
2. Suchen Sie nach "HTTP", wählen Sie HTTP Zuhörer aus, und wählen Sie **Erstellen**.
3.  Konfigurieren der HTTP-Zuhörer wie folgt aus:  
![][1]

4.  Wenn die paketeinstellungen einrichten, sehen Sie die folgende Option, ob die Zuhörer sollte automatisch Antworten oder ist es erforderlich, um eine explizite Antwort zu senden. Legen Sie diese auf **False** , Ihre eigene Antwort zu senden:  
![][2]

5.  Klicken Sie auf **OK** , um zu erstellen.
6.  Sobald die app-API Instanz erstellt wurde, öffnen Sie die Einstellungen, um die Sicherheit zu konfigurieren. Die HTTP-Zuhörer unterstützt derzeit Standardauthentifizierung. Sie können dies konfigurieren, verwenden die Sicherheitsoption beim Öffnen der Zuhörer HTTP:  
![][3]
  
    **Bekannte Probleme** *Der Sicherheit Einstellungen anzeigen "Keine" als Standardwert, obwohl darauf nicht definiert. Müssen Sie die Einstellung ändern, um Basic und zurück zur keine vor dem Speichern, um sicherzustellen, dass die Zuhörer HTTP richtig konfiguriert ist.*  

7. Legen Sie schließlich die Einstellungen der App API auf öffentliche (anonym), damit externe Clients den Endpunkt zugreifen können. Diese Einstellung steht unter "alle Einstellungen > ApplicationSettings" der App-API Zuhörer HTTP:![][10]

Wenn das erledigt ist, können Sie nun eine app Logik zum Verwenden der HTTP-Zuhörer erstellen.

## <a name="using-the-http-listener-in-your-logic-app"></a>Verwenden die Zuhörer HTTP in Ihrer App Logik
Nachdem Ihre app API erstellt wurde, können Sie jetzt die HTTP-Zuhörer als Trigger für Ihre App Logik verwenden. Dazu müssen Sie:

4.  Erstellen Sie eine neue Logik App.
5.  Öffnen Sie "Trigger und Aktionen" zum Öffnen der Logik Apps-Designer, und konfigurieren Ihre Fluss. Die HTTP-Zuhörer wird im Katalog aufgeführt. Wählen Sie ihn aus.
6.  Sie können nun festlegen, die HTTP-Methode und die relative URL, zu denen Sie die Zuhörer zum Auslösen des Ablaufs benötigen:  
![][4]  
![][5]

7.  Um der vollständige URI zu gelangen, klicken Sie mit der Doppelklicken auf die Zuhörer HTTP zum Anzeigen von zugehörigen Einstellungen Konfiguration, und kopieren Sie die URL für den "Host" der app API:  
![][6]
8.  Sie können jetzt wie folgt in die HTTP-Anforderung in andere Aktionen im Fluss empfangenen Daten verwenden:  
![][7]  
![][8]
9.  Abschließend um eine Antwort zu senden, fügen Sie ein anderes HTTP Zuhörer hinzu, und wählen Sie die Aktion HTTP-Antwort senden. Legen Sie die ID anfordern auf RequestID vom die Zuhörer HTTP, und füllen Sie die Antwort Textkörper und HTTP-Status, die Sie wieder zurückkehren möchten:  
![][9]

## <a name="using-the-http-action"></a>Mithilfe der Aktion HTTP
Die Aktion HTTP von Logik Apps unterstützt, und setzt voraus eine app API erste Möglichkeit zur gemeinsamen Nutzung benutzerspezifisch erstellt werden. Sie können Aktion HTTP an einer beliebigen Stelle in Ihrer App Logik einfügen und wählen Sie die URI, Überschriften und Textkörper für den Anruf.
Die Aktion HTTP unterstützt mehrere Optionen zum Client Seite Sicherheit. Sehen Sie die [Sicherheitsoptionen für Client-Seite](../scheduler/scheduler-outbound-authentication.md)ein.

Die Ausgabe der HTTP-Aktion ist Überschriften und Textkörper, die weiteren unterhalb verwendet werden kann in den Datenfluss ähnlich wie die Ausgabe anderer Aktionen und Verbinder genutzt wird.

## <a name="do-more-with-your-connector"></a>Führen Sie den Verbinder
Jetzt, da der Verbinder erstellt wurde, können Sie es mit einem Business Workflow mit einer Logik App hinzufügen. Finden Sie unter [Was Logik Apps werden?](app-service-logic-what-are-logic-apps.md).

Zeigen Sie die Swagger REST-API-Referenz bei [Verbindern und API Apps Bezug](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Sie können auch die Leistung von Statistiken und Steuerelement Sicherheit des Verbinders überprüfen. Finden Sie unter [Verwalten und Überwachen Ihrer integrierten API Apps und Verbinder](app-service-logic-monitor-your-connectors.md).

> [AZURE.NOTE] Wenn Sie mit Azure Logik Apps Schritte, bevor Sie für ein Azure-Konto anmelden möchten, wechseln Sie zu [Versuchen Logik App](https://tryappservice.azure.com/?appservice=logic). Sie können eine kurzlebige Starter Logik app sofort im App-Dienst erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

<!--Image references-->
[1]: ./media/app-service-logic-connector-http/1.png
[2]: ./media/app-service-logic-connector-http/2.png
[3]: ./media/app-service-logic-connector-http/3.png
[4]: ./media/app-service-logic-connector-http/4.png
[5]: ./media/app-service-logic-connector-http/5.png
[6]: ./media/app-service-logic-connector-http/6.png
[7]: ./media/app-service-logic-connector-http/7.png
[8]: ./media/app-service-logic-connector-http/8.png
[9]: ./media/app-service-logic-connector-http/9.png
[10]: ./media/app-service-logic-connector-http/10.png
