<properties 
    pageTitle="Einer normalverteilten Zufallsvariablen Web Service Suite | Microsoft Azure" 
    description="Einer normalverteilten Zufallsvariablen Web Service-Suite" 
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

#<a name="normal-distribution-suite"></a>Einer normalverteilten Zufallsvariablen Suite



Die Normalverteilung-Suite ist eine Reihe von Beispiel-Webdiensten ([Generator]( https://datamarket.azure.com/dataset/aml_labs/ndg7), [Taschenrechner Quantile]( https://datamarket.azure.com/dataset/aml_labs/ndq5), [Wahrscheinlichkeit Taschenrechner]( https://datamarket.azure.com/dataset/aml_labs/ndp5)), die Ihnen helfen, in das Erstellen und normale Verteilung behandeln. Den Diensten können Generieren einer normalverteilten Zufallsvariablen Sequenz beliebiger Länge, Quantiles aus einer angegebenen Wahrscheinlichkeit berechnen und Wahrscheinlichkeit von einer angegebenen Quantile berechnen. Die Dienste gibt unterschiedliche Ausgaben basierend auf den ausgewählten Dienst aus (siehe Beschreibung unten). Die einer normalverteilten Zufallsvariablen Suite basiert auf R Funktionen Qnorm, Rnorm und Pnorm, die in R Stats Paket enthalten sind.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Dieser Webdienst könnte beispielsweise von den Benutzern – potenziell über eine mobile-app, über eine Website oder sogar auf einem lokalen Computer genutzt werden. Aber der Zweck des Webdiensts ist auch dienen als Beispiel wie Azure maschinellen Schulung zum Erstellen von Webdienste auf R Code verwendet werden können. Mit nur wenigen Codezeilen R und Mausklicks eine Schaltfläche innerhalb von Azure maschinellen Learning Studio werden einem Versuch mit R Code erstellt und als Webdienst veröffentlicht. Der Webdienst kann dann zum Azure Marketplace veröffentlicht und verbraucht Benutzer und Geräte auf der ganzen Welt ohne Installation Infrastruktur vom Autor des Webdiensts.  
 

##<a name="consumption-of-web-service"></a>Verbrauch des Webdiensts
Die Suite einer normalverteilten Zufallsvariablen umfasst die folgenden 3-Dienste.

###<a name="normal-distribution-quantile-calculator"></a>Einer normalverteilten Zufallsvariablen Quantile Taschenrechner
Dieser Dienst 4 Argumente einer Verteilung Normal akzeptiert und die zugehörigen Quantile berechnet.

Die übergebenen Argumente sind:

* p - ein einzelnes Wahrscheinlichkeit eines Ereignisses mit einer normalverteilten Zufallsvariablen. 
* Mittel - das Mittel einer normalverteilten Zufallsvariablen zurück.
* SD - die Standardabweichung einer normalverteilten Zufallsvariablen. 
* Seite – L für den unteren Rand der Verteilung und U für den oberen Rand der Verteilung.

Die Ausgabe des Diensts wird die berechnete Quantile, die mit der angegebenen Wahrscheinlichkeit verknüpft ist.

###<a name="normal-distribution-probability-calculator"></a>Rechner für einer normalverteilten Zufallsvariablen zurück
Dieser Dienst 4 Argumente einer Verteilung Normal akzeptiert und die zugehörige Wahrscheinlichkeit berechnet.

Die übergebenen Argumente sind:

* f - ein einzelnes Quantile der ein Ereignis mit einer normalverteilten Zufallsvariablen. 
* Mittel - das Mittel einer normalverteilten Zufallsvariablen zurück.
* SD - die Standardabweichung einer normalverteilten Zufallsvariablen. 
* Seite – L für den unteren Rand der Verteilung und U für den oberen Rand der Verteilung.

Die Ausgabe des Diensts wird die berechnete Wahrscheinlichkeit, die der angegebenen Quantile zugeordnet ist.

###<a name="normal-distribution-generator"></a>Generator einer normalverteilten Zufallsvariablen
Dieser Dienst akzeptiert 3 Argumente einer Verteilung Normal und generiert eine zufällige Folge von Zahlen, die normalverteilt sind. Die folgenden Argumente sollte innerhalb der Anforderung darauf bereitgestellt werden:

* n – die Anzahl der Beobachtungen. 
* Mittel - das Mittel einer normalverteilten Zufallsvariablen zurück.
* SD - die Standardabweichung einer normalverteilten Zufallsvariablen. 

Die Ausgabe der Dienst ist eine Abfolge von Länge n mit einer normalen Verteilung anhand der Mittelwert und sd Argumente.

>Dieser Dienst, ist wie auf dem Azure Marketplace, gehostet ein OData-Dienst; Diese möglicherweise über POST oder GET-Methoden aufgerufen werden. 

Es gibt mehrere Methoden für die Nutzung des Diensts in einer Weise automatisierte (Apps im folgenden Beispiel: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [Wahrscheinlichkeit Taschenrechner](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Taschenrechner Quantile](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).

###<a name="starting-c-code-for-web-service-consumption"></a>Starten Sie C#-Code für Webdiensten:

###<a name="normal-distribution-quantile-calculator"></a>Einer normalverteilten Zufallsvariablen Quantile Taschenrechner
    public class Input
    {
            public string p;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { p = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="normal-distribution-probability-calculator"></a>Rechner für einer normalverteilten Zufallsvariablen zurück
    public class Input
    {
            public string q;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="normal-distribution-generator"></a>Generator einer normalverteilten Zufallsvariablen
    public class Input
    {
            public string n;
            public string mean;
            public string sd;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text };
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
>Dieser Webdienst wurde mithilfe von Azure maschinellen Learning erstellt. Kostenlose Testversion sowie Einführung Videos zum Erstellen von Versuche und [publishing-Webdiensten](machine-learning-publish-a-machine-learning-web-service.md)finden Sie unter [azure.com/ml](http://azure.com/ml). 

Es folgt ein Screenshot der Versuch, die den Web-Dienst und Beispiel-Code für jede der Module in den Versuch erstellt.

###<a name="normal-distribution-quantile-calculator"></a>Einer normalverteilten Zufallsvariablen Quantile Taschenrechner

Experimentieren Fluss:

![Experimentieren Fluss][2]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }
    q = qnorm(param$p,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    q = 2* param$mean - q
    } else if (param$side =='L') {
    q = q
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(q)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-probability-calculator"></a>Rechner für einer normalverteilten Zufallsvariablen zurück
Experimentieren Fluss:

![Experimentieren Fluss][3]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    prob = pnorm(param$q,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    prob = 1 - prob
    } else if (param$side =='B') {
    prob = ifelse(prob<=0.5,prob * 2, 1)
    } else if (param$side =='L') {
    prob = prob
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-generator"></a>Generator einer normalverteilten Zufallsvariablen
Experimentieren Fluss:

![Experimentieren Fluss][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Einschränkungen 

Dies sind sehr einfache Beispiele umgebenden Wahrscheinlichkeiten einer normalverteilten Zufallsvariablen. Wie aus dem obigen Beispielcode gesehen werden können, wird die kleinen Aufspüren von Fehlern implementiert.

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen über den Verbrauch der Webdienst oder Veröffentlichung zum Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png
 
