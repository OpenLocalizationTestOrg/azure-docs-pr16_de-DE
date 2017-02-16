<properties
   pageTitle="Bereitstellen eines neuen Webdienst"
   description="Der Workflow der Bereitstellung von einer Cloud-basierten Webdienst"
   services="machine-learning"
   documentationCenter=""
   authors="vDonGlover"
   manager="raymondl"
   editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>

# <a name="deploy-a-new-web-service"></a>Bereitstellen eines neuen Webdiensts

Microsoft Azure Computer jetzt learning bietet Webdienste, die auf [Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md) basieren für neue Planoptionen Abrechnung und Bereitstellen von Ihren Webdienst auf mehrere Bereiche.

Allgemeine Workflows für die Bereitstellung von eines Webdiensts mit Microsoft Azure maschinellen Learning-Webdiensten lautet:

* Erstellen einer Vorhersage experimentieren.
* es bereitstellen
* Konfigurieren Sie deren Namen
* Abrechnung plan
* Testen Sie
* Nutzen Sie es aus.

Die folgende Grafik zeigt den Workflow an.

![Workflow für Web Service-Bereitstellung][1]
 
## <a name="deploy-web-service-from-studio"></a>Webdienst aus Studio bereitstellen 

Ein Versuch als neue Webdienst bereitstellen. Melden Sie sich bei der maschinellen Learning Studio, und erstellen Sie einen neuen Vorhersage Webdienst. 

**Hinweis**: Wenn Sie bereits eine experimentieren als klassische Webdienst bereitgestellt haben können nicht Sie es als neue Webdienst bereitstellen.
 
Klicken Sie auf am unteren Rand des Zeichenbereichs experimentieren **Ausführen** , und klicken Sie dann auf **Webdienst bereitstellen** und **Webdienst bereitstellen [New]**. Die Bereitstellungsseite des Computers Learning Webdienst-Managers wird geöffnet.

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a>Maschinelle Learning Web-Dienst-Manager bereitstellen experimentieren Seite
Geben Sie einen Namen für den Webdienst, klicken Sie auf der Seite bereitstellen experimentieren.
Wählen Sie einen Preisgestaltung Plan aus. Wenn Sie einen vorhandenen Preisgestaltung Plan, dass Sie ihn auswählen können haben, müssen andernfalls Sie einen neuen Preisplan für den Dienst erstellen. 

1.  In den **Preisplan** Dropdown-Liste, wählen Sie einen vorhandenen Plan aus, oder wählen Sie die Option **neuen Plan auswählen** .
2.  Geben Sie unter **Plan Name**einen Namen ein, der den Plan auf Ihrer Rechnung identifiziert.
3.  Wählen Sie eine der **Monatlichen planen Ebenen**. Beachten Sie, dass die Region ein Plan Ebenen Standard für die Pläne für Ihre Standardregion und Ihre Webdienst bereitgestellt wird.

Klicken Sie auf **Bereitstellen** und die Schnellstart-Seite für Ihre Web-Dienst wird geöffnet.

## <a name="quickstart-page"></a>Schnellstart-Seite
Schnellstart die Webseite-Dienst bietet Ihnen Hinweise und Zugriff auf die am häufigsten verwendeten Aufgaben, die Sie ausführen möchten, nachdem Sie einen neuen Webdienst erstellt. Hier können Sie einfach die **Testseite** und die **verbrauchen** Seite zugreifen.

## <a name="testing-your-web-service"></a>Testen Ihrer Webdiensts

Klicken Sie von der Seite Schnellstart unter Allgemeine Aufgaben auf Webdienst testen.   

So prüfen Sie den Webdienst als eine Anforderung / Antwort-Dienst (RRS):

* Klicken Sie auf der Menüleiste auf **Test** .
* Klicken Sie auf **Anforderung-Antwort**.
* Geben Sie geeignete Werte für die Eingabewerte Spalten Ihrer experimentieren.
* Klicken Sie auf Testen **Anforderung / Antwort**.

Sie Ergebnisse werden auf der rechten Seite der Seite angezeigt.

Klicken Sie zum Testen von eines Webdiensts Stapel Ausführung Service (l) verwenden Sie eine CSV-Datei:

* Klicken Sie auf der Menüleiste auf **Test** .
* Klicken Sie auf **Stapel**.
* Klicken Sie auf Durchsuchen, und navigieren Sie zu Ihrer Daten Beispieldatei, unter der Eingabe.
* Klicken Sie auf **Testen**.

Der Status des Tests wird unter **Testen Stapel Projekte**angezeigt.

## <a name="consuming-your-web-service"></a>Verwenden der Webdiensts

Wenn als Webdienst bereitgestellt, bieten Azure maschinellen Learning Versuche ein REST-API, die von einer Vielzahl von Geräten und Plattformen genutzt werden können. Dies ist, weil die einfache REST-API akzeptiert und reagiert mit JSON Nachrichten formatiert. Das Azure maschinellen Learning-Portal enthält Code, der zum Aufrufen des Webdiensts in R, c# und Python verwendet werden kann.
 
Klicken Sie auf der Seite Verbrauch finden Sie:

* Der API-Schlüssel und URI für die Verarbeitung von Webdienst in apps.
* Starten Sie Excel und Web app-Vorlagen zum Starten eines Ihrer Verbrauchsprozess aus.
* Beispiel-Code in c#, Python und R zu Ihnen den Einstieg.

Weitere Informationen zum Verwenden von Webdiensten finden Sie unter [so einen Webdienst Azure maschinellen Learning nutzen, der von einem Computer Learning experimentieren bereitgestellt wurde](machine-learning-consume-web-services.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden von Webdiensten finden Sie unter:

[Wie Sie einen Webdienst Azure maschinellen Learning nutzen, der von einem Computer Learning experimentieren bereitgestellt wurde](machine-learning-consume-web-services.md)

[Azure maschinellen Learning-Webdiensten: Bereitstellung und Verbrauch](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
