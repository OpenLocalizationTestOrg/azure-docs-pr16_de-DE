<properties 
    pageTitle="So verwenden Sie den Inspektor API verfolgen Anrufe bei Azure-API Verwaltung" 
    description="Informationen Sie zum Verfolgen von Anrufen mit der API Inspektor in Azure-API Management." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>So verwenden Sie den Inspektor API verfolgen Anrufe bei Azure-API Verwaltung

Management-API bietet ein API Inspektor-Tool, um Ihnen bei der für das Debuggen und Problembehandlung der APIs. Inspektor API programmgesteuert verwendet werden können, und können auch direkt von der Entwicklerportal verwendet werden. 

Zusätzlich zu Tracing Operationen verfolgt API Inspektor auch Testen der [Richtlinienausdruck](https://msdn.microsoft.com/library/azure/dn910913.aspx) aus. Eine Demo finden Sie unter [Cloud Deckblatt Folge 177: Weitere API Verwaltungsfunktionen](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) und schnelle Vorlauf auf 21:00.

Dieses Handbuch bietet eine Verwendung der Funktion API Inspektor verwenden.

>[AZURE.NOTE] API Inspektor Spuren sind nur generiert und für Besprechungsanfragen mit Abonnement Tasten, die [dem Administratorkonto](api-management-howto-create-groups.md) gehören zur Verfügung gestellt.

## <a name="trace-call"></a> Verwenden API Inspektor zum Verfolgen eines Anrufs

Um API Inspektor verwenden zu können, fügen ein **Ocp-Apim-Spur: WAHR** Kopfzeile einem Anruf Vorgang anfordern, herunter, und prüfen Sie die Spur verwenden die URL, die von der Antwort-Header **Ocp-Apim-Spur-Speicherort** angegeben. Dies kann programmgesteuert erfolgen, und kann auch direkt von der Entwicklerportal vorgenommen werden.

In diesem Lernprogramm erfahren so verwenden Sie den Inspektor API auf Spur Vorgänge mithilfe der grundlegenden Rechner-API im beim Abrufen Schritte [Verwalten Ihrer ersten API](api-management-get-started.md) Lernprogramm konfiguriert ist. Wenn Sie dieses Lernprogramm abgeschlossen haben dies dauert nur ein paar Minuten der grundlegenden Taschenrechner API importieren oder eine andere API Ihrer Wahl wie die Echo-API können. Jede Instanz der API Management-Dienst gehört zum vorkonfiguriertes mit einer Echo-API, die mit experimentieren und Informationen zu API Management verwendet werden können. Die Echo-API gibt wieder jeden Eingabe an sie gesendet wird. Wenn Sie es verwenden möchten, können Sie alle HTTP-Verb aufrufen, und der Rückgabewert ist einfach, was Sie gesendet. 



Um anzufangen, klicken Sie auf **Entwicklerportal** im klassischen Azure-Portal für Ihre API Verwaltungsdienst. Vorgänge können direkt von der Entwicklerportal bietet eine bequeme Möglichkeit zum Anzeigen und testen die Vorgänge einer API aufgerufen werden.

>Wenn Sie noch eine Instanz der API Management-Dienst erstellt haben, finden Sie unter [Erstellen einer API Management Service Instanz][] in das [Erste Schritte mit Azure-API Management][] Lernprogramm.

![Entwicklerportal Management-API][api-management-developer-portal-menu]

Klicken Sie im oberen Menü **APIs** auf, und klicken Sie dann auf **Grundlegende Taschenrechner**.

![Echo-API][api-management-api]

Klicken Sie auf, **Probieren Sie es** , um den Vorgang **Hinzufügen, zwei Zahlen** zu wiederholen.

![Probieren Sie es][api-management-open-console]

Übernehmen Sie den Standardnamen Parameterwerte, und wählen Sie die Taste Abonnement für das Produkt aus, die, das Sie aus dem Dropdown- **Abonnement-Taste** verwenden möchten.

Standardmäßig in die **Ocp-Apim-Spur** -Entwicklerportal ist Header bereits auf **true**festgelegt. Diese Kopfzeile konfiguriert, und zwar unabhängig davon, ob eine Spur generiert wird.

![Senden][api-management-http-get]

Klicken Sie auf **Senden** , um den Vorgang aufzurufen.

![Senden][api-management-send-results]

In der Antwort werden die Kopfzeilen einer **Ocp-Apim-Spur-Speicherort** mit einem Wert ähnlich wie im folgenden Beispiel.

    ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742

Die Ablaufverfolgung kann aus der angegebenen Position heruntergeladen und überprüft werden, wie im nächsten Schritt gezeigt werden.

## <a name="inspect-trace"> </a>Der Spur prüfen

So prüfen Sie die Werte in der Spur, laden Sie die Spur-Datei aus dem **Ocp-Apim-Spur-Speicherort** -URL ein. Es ist eine Textdatei im JSON-Format, und Einträge ähnlich wie im folgenden Beispiel enthält.

    {
        "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
        "traceEntries": {
            "inbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0725926",
                    "data": {
                        "request": {
                            "method": "GET",
                            "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "Connection",
                                    "value": "Keep-Alive"
                                },
                                {
                                    "name": "Host",
                                    "value": "contoso5.azure-api.net"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "mapper",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0726213",
                    "data": {
                        "configuration": {
                            "api": {
                                "from": "/calc",
                                "to": {
                                    "scheme": "http",
                                    "host": "calcapi.cloudapp.net",
                                    "port": 80,
                                    "path": "/api",
                                    "queryString": "",
                                    "query": {},
                                    "isDefaultPort": true
                                }
                            },
                            "operation": {
                                "method": "GET",
                                "uriTemplate": "/add?a={a}&b={b}"
                            },
                            "user": {
                                "id": 1,
                                "groups": [
                                    "Administrators",
                                    "Developers"
                                ]
                            },
                            "product": {
                                "id": 1
                            }
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0727522",
                    "data": {
                        "message": "Request is being forwarded to the backend service.",
                        "request": {
                            "method": "GET",
                            "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "X-Forwarded-For",
                                    "value": "33.52.215.35"
                                }
                            ]
                        }
                    }
                }
            ],
            "outbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1960601",
                    "data": {
                        "response": {
                            "status": {
                                "code": 200,
                                "reason": "OK"
                            },
                            "headers": [
                                {
                                    "name": "Pragma",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Length",
                                    "value": "124"
                                },
                                {
                                    "name": "Cache-Control",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Type",
                                    "value": "application/xml; charset=utf-8"
                                },
                                {
                                    "name": "Date",
                                    "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                                },
                                {
                                    "name": "Expires",
                                    "value": "-1"
                                },
                                {
                                    "name": "Server",
                                    "value": "Microsoft-IIS/8.5"
                                },
                                {
                                    "name": "X-AspNet-Version",
                                    "value": "4.0.30319"
                                },
                                {
                                    "name": "X-Powered-By",
                                    "value": "ASP.NET"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1961112",
                    "data": {
                        "message": "Response headers have been sent to the caller. Starting to stream the response body."
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1963155",
                    "data": {
                        "message": "Response body streaming to the caller is complete."
                    }
                }
            ]
        }
    }

## <a name="next-steps"> </a>Nächste Schritte

-   Schauen Sie sich eine Vorführung der Richtlinienausdrücke in Spur [Cloud Deckblatt Folge 177: Weitere API Verwaltungsfunktionen](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Vorspulen Sie, 21:00 Uhr anschauen angezeigt.

>[AZURE.VIDEO episode-177-more-api-management-features-with-vlad-vinogradsky]

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Erste Schritte mit Azure-API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management-Dienst]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




 