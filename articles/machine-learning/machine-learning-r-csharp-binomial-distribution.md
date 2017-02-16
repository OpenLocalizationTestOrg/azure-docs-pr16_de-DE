<properties 
    pageTitle="Binomialverteilung Suite | Microsoft Azure" 
    description="Binomialverteilung Suite" 
    services="machine-learning" 
    documentationCenter="" 
    authors="ireiter" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="ireiter"/> 


#<a name="binomial-distribution-suite"></a>Binomialverteilung Suite




Die Binomialverteilung Suite ist eine Reihe von Hilfen in generieren und schreibgeschützte Binomialverteilung Stichprobe-Webdiensten ([Binomialverteilung Generator](https://datamarket.azure.com/dataset/aml_labs/bdg5), [Wahrscheinlichkeit Taschenrechner]( https://datamarket.azure.com/dataset/aml_labs/bdp4), [Taschenrechner Quantile]( https://datamarket.azure.com/dataset/aml_labs/bdq5)). Die Dienste zulassen Generieren einer Sequenz Binomialverteilung beliebiger Länge, Quantiles berechnen von out-of angegebenen Wahrscheinlichkeit und Berechnung zurück aus einem angegebenen Quantile. Die Dienste gibt unterschiedliche Ausgaben basierend auf den ausgewählten Dienst aus (siehe Beschreibung unten). Die Suite Binomialverteilung basiert auf R Funktionen Qbinom, Rbinom und Pbinom, die in das Paket R Stats enthalten sind. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Diese Webdienste konnte von Benutzer – potenziell direkt auf der Marketplace, bis eine mobile-app, über eine Website oder sogar auf einem lokalen Computer, beispielsweise genutzt werden. Aber der Zweck des Webdiensts ist auch dienen als Beispiel wie Azure maschinellen Schulung zum Erstellen von Webdienste auf R Code verwendet werden können. Mit nur wenigen Codezeilen R und Mausklicks eine Schaltfläche innerhalb von Azure maschinellen Learning Studio werden einem Versuch mit R Code erstellt und als Webdienst veröffentlicht. Der Webdienst kann dann zum Azure Marketplace veröffentlicht und verbraucht Benutzer und Geräte auf der ganzen Welt – keine Infrastruktur Setup vom Autor des Webdiensts erforderlich ist.

##<a name="consumption-of-web-service"></a>Verbrauch des Webdiensts
Die Binomialverteilung Suite umfasst die folgenden 3-Dienste.

###<a name="binomial-distribution-quantile-calculator"></a>Binomialverteilung Quantile Taschenrechner
Dieser Dienst 4 Argumente einer Verteilung Normal akzeptiert und die zugehörigen Quantile berechnet.
Die übergebenen Argumente sind:

- p - ein einzelnes aggregiert Wahrscheinlichkeit von mehreren versuchen Erfolge sind.  
- Größe - die Anzahl der Versuche.
- WAHRSCHBEREICH - die Wahrscheinlichkeit eines Erfolgs in eine Testversion.
- Seite – L am unteren Rand der Verteilung U für den oberen Rand der Verteilung. 

Die Ausgabe des Diensts wird die berechnete Quantile, die mit der angegebenen Wahrscheinlichkeit verknüpft ist.

###<a name="binomial-distribution-probability-calculator"></a>Die Wahrscheinlichkeit Taschenrechner einer binomialverteilten Zufallsvariablen zurück
Dieser Dienst akzeptiert 4 Argumente der als Binomialverteilung zurück und die zugehörige Wahrscheinlichkeit berechnet.
Die übergebenen Argumente sind:

- f - ein einzelnes Quantile der ein Ereignis mit einer binomialverteilten Zufallsvariablen zurück. 
- Größe - die Anzahl der Versuche.
- WAHRSCHBEREICH - die Wahrscheinlichkeit eines Erfolgs in eine Testversion.
- Seite – L für den unteren Rand der Verteilung, U am oberen Rand der Verteilung oder E, die eine einzelne Zahl der Erfolge entspricht.

Die Ausgabe des Diensts wird die berechnete Wahrscheinlichkeit, die der angegebenen Quantile zugeordnet ist.

###<a name="binomial-distribution-generator"></a>Generator einer binomialverteilten Zufallsvariablen zurück
Dieser Dienst akzeptiert 3 Argumente der als Binomialverteilung zurück und generiert eine zufällige Folge von Zahlen, die binomially verteilt werden. Die folgenden Argumente sollte innerhalb der Anforderung darauf bereitgestellt werden:

- n - Anzahl der Beobachtungen. 
- Größe - Anzahl von versuchen Erfolge sind.
- WAHRSCHBEREICH - Wahrscheinlichkeit eines Erfolgs.

Die Ausgabe der Dienst ist eine Abfolge von Länge n mit einer Binomialverteilung anhand der Größe und WAHRSCHBEREICH Argumente.

>Dieser Dienst, ist wie auf dem Azure Marketplace, gehostet ein OData-Dienst; Diese möglicherweise über POST oder GET-Methoden aufgerufen werden. 

Es gibt mehrere Methoden für die Nutzung des Diensts in einer Weise automatisierte (Apps im folgenden Beispiel: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [Wahrscheinlichkeit Taschenrechner](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [Taschenrechner Quantile](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)). 

###<a name="starting-c-code-for-web-service-consumption"></a>Starten Sie C#-Code für Webdiensten:

###<a name="binomial-distribution-quantile-calculator"></a>Binomialverteilung Quantile Taschenrechner
    public class Input
    {
            public string p;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void main()
    {
            var input = new Input() { p = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="binomial-distribution-probability-calculator"></a>Die Wahrscheinlichkeit Taschenrechner einer binomialverteilten Zufallsvariablen zurück
    public class Input
    {
            public string q;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = " PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="binomial-distribution-generator"></a>Generator einer binomialverteilten Zufallsvariablen zurück
    public class Input
    {
            public string n;
            public string size;
            public string p;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, size = TextBox2.Text, p = TextBox3.Text };
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

###<a name="binomial-distribution-quantile-calculator"></a>Binomialverteilung Quantile Taschenrechner

![Arbeitsbereich erstellen][4]

####<a name="module-1"></a>Modul 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
####<a name="module-2"></a>Modul 2:

    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }

    if (param$prob < 0 ) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 0
    } else if (param$prob > 1) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 1
    }

    quantile = qbinom(param$p,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    quantile

    if (param$side == 'U'){
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = F)
    band=subset(df,x>quantile)
    } else if (param$side =='L') {
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = T)
    band=subset(df,x<=quantile)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(quantile)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");


###<a name="binomial-distribution-probability-calculator"></a>Die Wahrscheinlichkeit Taschenrechner einer binomialverteilten Zufallsvariablen zurück

![Arbeitsbereich erstellen][5]

####<a name="module-1"></a>Modul 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port


####<a name="module-2"></a>Modul 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    prob = pbinom(param$q,size=param$size,prob=param$prob)
    prob.eq = dbinom(param$q,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    prob

    if (param$side == 'U'){
    prob = 1 - prob
    band=subset(df,x>param$q)
    } else if (param$side =='E') {
    prob = prob.eq
    band=subset(df,x==param$q)
    } else if (param$side =='L') {
    prob = prob
    band=subset(df,x<=param$q)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

###<a name="binomial-distribution-generator"></a>Generator einer binomialverteilten Zufallsvariablen zurück

![Arbeitsbereich erstellen][6]

####<a name="module-1"></a>Modul 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data to output port

####<a name="module-2"></a>Modul 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Einschränkungen 
Dies sind sehr einfache Beispiele der Binomialverteilung umgibt. Wie aus dem obigen Beispielcode gesehen werden können, wird die kleinen Aufspüren von Fehlern implementiert.

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen über den Verbrauch der Webdienst oder Veröffentlichung zum Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).


[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png
 
