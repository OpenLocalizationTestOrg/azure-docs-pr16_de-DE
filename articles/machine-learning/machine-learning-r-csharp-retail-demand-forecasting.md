<properties 
    pageTitle="Prognostizieren - legt + STL | Microsoft Azure" 
    description="Prognostizieren - legt + STL" 
    services="machine-learning" 
    documentationCenter="" 
    authors="xueshanz" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="yijichen"/> 

#<a name="forecasting---ets--stl"></a>Prognostizieren - legt + STL  

Dieser [Webdienst]( https://datamarket.azure.com/dataset/aml_labs/demand_forecast) implementiert saisonale Trend Gliederung (STL) und Exponentielles Glätten (legt) Modelle zu erzeugen, basierte auf den zurückliegenden Daten, die vom Benutzer eingegebene Vorhersagen. Wird die Anforderung für ein bestimmtes Produkt dieses Jahr erhöht? Kann ich meine Artikelumsätze für die Jahreszeit Weihnachten Vorhersagen, damit ich meine Inventory eine effektive Planung können? Prognose Datenmodelle sind bereit für die Adresse solche Fragen. Ausgehend von der vergangenen Daten, untersuchen Sie diese Modelle ausgeblendeten Trends und saisonbedingten zukünftigen Trends Vorhersagen. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
 
>Dieser Webdienst könnte beispielsweise von den Benutzern – potenziell über eine mobile-app, über eine Website oder sogar auf einem lokalen Computer genutzt werden. Aber der Zweck des Webdiensts ist auch dienen als Beispiel wie Azure maschinellen Schulung zum Erstellen von Webdienste auf R Code verwendet werden können. Mit nur wenigen Codezeilen R und Mausklicks eine Schaltfläche innerhalb von Azure maschinellen Learning Studio werden einem Versuch mit R Code erstellt und als Webdienst veröffentlicht. Der Webdienst kann dann zum Azure Marketplace veröffentlicht und verbraucht Benutzer und Geräte auf der ganzen Welt ohne Installation Infrastruktur vom Autor des Webdiensts.  
 
##<a name="consumption-of-web-service"></a>Verbrauch des Webdiensts 

Dieser Dienst 4 Argumente akzeptiert und die Vorhersagen berechnet.
Die übergebenen Argumente sind:

* Häufigkeit – Gibt die Häufigkeit unformatierten Daten (täglich/wöchentlich/monatlich/Quarterly/jährlich) an.
* Voreingestellter - SCHÄTZER zukünftiger Zeitrahmen an.
* Datum: Fügen Sie in der neuen Datenreihe Zeitdaten für Zeit hinzu.
* Wert – in die neue Uhrzeit Reihe liegenden Datenwerte hinzufügen.

Die Ausgabe der Dienst ist die berechneten Werte für die Wettervorhersage angezeigt werden.
 
Beispieleingabe könnte: 

* Häufigkeit – 12
* Voreingestellter - 12
* Datum - 1/15/2012 2/15/2012; 3/15/2012; 4/15/2012; 5/15/2012; 6/15/2012; 7/15/2012; 8 / 15/2012 9/15/2012; 10/15/2012; 11/15/2012; 12/15/2012; 1/15/2013 2/15/2013; 3/15/2013; 4/15/2013; 5/15/2013; 6/15/2013; 7/15/2013; 8 / 15/2013 9/15/2013; 10/15/2013; 11/15/2013; 12/15/2013. 1/15/2014 2/15/2014; 3/15/2014; 4/15/2014; 5/15/2014; 6/15/2014; 7/15/2014; 8 / 15/2014; 9/15/2014
* Value - 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509

>Dieser Dienst, ist wie auf dem Azure Marketplace, gehostet ein OData-Dienst; Diese möglicherweise über POST oder GET-Methoden aufgerufen werden. 

Es gibt mehrere Methoden für die Nutzung des Diensts in einer Weise automatisierte (eine Beispiel-app ist [hier](http://microsoftazuremachinelearning.azurewebsites.net/StlEtsForecasting.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Starten Sie C#-Code für Webdiensten:

    public class Input
    {
            public string frequency;
            public string horizon;
            public string date;
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };         var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


##<a name="creation-of-web-service"></a>Erstellen von Webdienst 

>Dieser Webdienst wurde mithilfe von Azure maschinellen Learning erstellt. Kostenlose Testversion sowie Einführung Videos zum Erstellen von Versuche und [publishing-Webdiensten](machine-learning-publish-a-machine-learning-web-service.md)finden Sie unter [azure.com/ml](http://azure.com/ml). Es folgt ein Screenshot der Versuch, die den Web-Dienst und Beispiel-Code für jede der Module in den Versuch erstellt.

Aus innerhalb von Azure Computer lernen, wurde eine neue, leere experimentieren erstellt. Beispieleingabedaten hochgeladen wurde mit einem Datenschema vordefinierte. Mit den Datenschema verknüpft ist ein [Ausführen R Skript] [ execute-r-script] Modul, durch die generiert STL und legt Prognostizieren von Datenmodellen mithilfe von Stl, 'legt,' und 'Planung' Funktionen von R. 

###<a name="experiment-flow"></a>Experimentieren Fluss:

![Experimentieren Fluss][2]

####<a name="module-1"></a>Modul 1:
 
    # Add in the CSV file with the data in the format shown below 
![Beispieldaten][3]   

####<a name="module-2"></a>Modul 2:

    # Data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)
    
    # Preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]
    
    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)
    
    # Fit a time series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- stl(train_ts,  s.window="periodic")
    train_model <- forecast(fit1, h = data$horizon, method = 'ets')
    plot(train_model)
    
    # Produce forcasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")
    
    # Data output
    maml.mapOutputPort("data.forecast");

##<a name="limitations"></a>Einschränkungen 

Dies ist ein einfaches Beispiel für legt + STL prognostizieren. Wie aus dem obigen Beispielcode gesehen werden können, keine Aufspüren von Fehlern implementiert wird und der Dienst wird davon ausgegangen, dass alle Variablen fortlaufender-Positive Werte sind, und die Häufigkeit sollte eine ganze Zahl größer als 1. Die Länge der die dar Datum und der Wert muss dieselbe sein, und die Länge der Zeit Serie sollten größer als 2 * Häufigkeit. Variable für das Datum sollte das Format ' mm/tt/jjjj' eingehalten werden.

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen über den Verbrauch der Webdienst oder Veröffentlichung zum Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img1.png
[2]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img2.png
[3]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
