<properties 
    pageTitle="Differenz in Proportionen testen | Microsoft Azure" 
    description="Differenz in Proportionen testen" 
    services="machine-learning" 
    documentationCenter="" 
    authors="aniedea" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="aniedea"/> 


#<a name="difference-in-proportions-test"></a>Differenz in Proportionen testen


Unterscheiden sich zwei Proportionen statistisch? Angenommen, ein Benutzer möchte Vergleichen von zwei Filme, um festzustellen, ob ein Film einen erheblich höheren Anteil der hat 'gefällt mir' wann im Vergleich zu anderen. Mit einer Stichproben könnte ein statistisch signifikante Unterschied zwischen die Proportionen 0,50 und 0.51. Mit einem kleinen Beispiel es ist möglicherweise nicht genügend Daten, um festzustellen, ob diese Proportionen tatsächlich unterscheiden. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Dieser [Webdienst]( https://datamarket.azure.com/dataset/aml_labs/prop_test) führt eine Hypothese der Differenz in zwei Proportionen je nach Benutzereingabe die Anzahl der Erfolge und der Gesamtzahl der Versuche für die Gruppen 2 Vergleich. In einem möglichen Szenario konnte diesen Webdienst von innerhalb einer app Film Vergleich, ein anderer Benutzer dem Benutzer, ob eine der Filme wirklich 'befunden ist' öfter als andere, basierend auf Film Bewertungen aufgerufen werden.

>Dieser Webdienst könnte beispielsweise von den Benutzern – potenziell über eine mobile-app, über eine Website oder sogar auf einem lokalen Computer genutzt werden. Aber der Zweck des Webdiensts ist auch dienen als Beispiel wie Azure maschinellen Schulung zum Erstellen von Webdienste auf R Code verwendet werden können. Mit nur wenigen Codezeilen R und Mausklicks eine Schaltfläche innerhalb von Azure maschinellen Learning Studio werden einem Versuch mit R Code erstellt und als Webdienst veröffentlicht. Der Webdienst kann dann zum Azure Marketplace veröffentlicht und verbraucht Benutzer und Geräte auf der ganzen Welt ohne Installation Infrastruktur vom Autor des Webdiensts.


##<a name="consumption-of-web-service"></a>Verbrauch des Webdiensts

Dieser Dienst akzeptiert 4 Argumente und enthält eine Hypothese testen der Proportionen.

Die übergebenen Argumente sind:

* Successes1 - Anzahl der Erfolgsereignisse in der Stichprobe 1.
* Successes2 - Anzahl der Erfolgsereignisse in der Stichprobe 2.
* Total1 - Größe Probe 1.
* Summe2 - Größe Probe 2.

Die Ausgabe des Diensts ist das Ergebnis des die Hypothese testen sowie die Chi-Quadrat-Verteilung, df, einseitige Prüfstatistik und in der Stichprobe 1/2 und Konfidenzintervall Grenzen proportional machen.

>Dieser Dienst, ist wie auf dem Azure Marketplace, gehostet ein OData-Dienst; Diese möglicherweise über POST oder GET-Methoden aufgerufen werden. 

Es gibt mehrere Methoden für die Nutzung des Diensts in einer Weise automatisierte (eine Beispiel-app ist [hier](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Starten Sie C#-Code für Webdiensten:

    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
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

Aus innerhalb von Azure Computer lernen, eine neue, leere experimentieren erstellt wurde mit zwei [Ausführen R Skript] [ execute-r-script] Module. Im ersten Modul, die das Schema definiert ist, wird mit während der zweiten Modul den Befehl prop.test innerhalb R die Hypothese für 2 Proportionen ausgeführt. 


###<a name="experiment-flow"></a>Experimentieren Fluss:

![Experimentieren Fluss][2]


####<a name="module-1"></a>Modul 1:
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data to output port
    dataset1 = maml.mapInputPort(1) #read data from input port
    

####<a name="module-2"></a>Modul 2:

    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"The proportions are different!",
    "The proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port
    

##<a name="limitations"></a>Einschränkungen 

Dies ist ein einfaches Beispiel für einen Test Differenzen in 2 Proportionen. Wie aus dem obigen Beispielcode gesehen werden können, wird keine Aufspüren von Fehlern implementiert, und der Dienst wird davon ausgegangen, dass alle Variablen fortlaufend sind.

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen über den Verbrauch der Webdienst oder Veröffentlichung zum Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
