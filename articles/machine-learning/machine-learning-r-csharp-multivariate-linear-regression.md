<properties 
    pageTitle="Multivariate lineare Regression | Microsoft Azure" 
    description="Multivariate lineare Regression" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jaymathe" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="jaymathe"/> 


#<a name="multivariate-linear-regression"></a>Multivariate lineare Regression   
 

 
Angenommen Sie, Sie haben ein Dataset und möchte schnell eine abhängige Variable (y) Vorhersagen für den einzelnen (i) basierend auf unabhängigen Variablen. Lineare Regression ist eine beliebte statistische Methode für solche Vorhersagen verwendet. Hier wird angenommen, dass y abhängige Variable ein fortlaufender Wert sein.  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

Ein einfaches Szenario könnte die Stelle, an der die Recherche-Assistenten will Vorhersagen von der Gewichtung einer Person (y), basierend auf deren Höhe (X). Ein erweitertes Szenario könnte, wo die Recherche-Assistenten verfügt über weitere Informationen für die einzelnen (z. B. Stärke, Geschlecht, Rasse) und versucht, die Stärke der Person Vorhersagen. Dieser [Webdienst]( https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) passt lineare Regressionsmodell auf die Daten und gibt den geschätzten Wert (y) für jede der Beobachtungen für die Daten aus.

>Dieser Webdienst könnte beispielsweise von den Benutzern – potenziell über eine mobile-app, über eine Website oder sogar auf einem lokalen Computer genutzt werden. Aber der Zweck des Webdiensts ist auch dienen als Beispiel wie Azure maschinellen Schulung zum Erstellen von Webdienste auf R Code verwendet werden können. Mit nur wenigen Codezeilen R und Mausklicks eine Schaltfläche innerhalb von Azure maschinellen Learning Studio werden einem Versuch mit R Code erstellt und als Webdienst veröffentlicht. Der Webdienst kann dann zum Azure Marketplace veröffentlicht und verbraucht Benutzer und Geräte auf der ganzen Welt ohne Installation Infrastruktur vom Autor des Webdiensts.  

##<a name="consumption-of-web-service"></a>Verbrauch des Webdiensts  
Dieser Webdienst gibt die geschätzte Werte für die abhängige Variable basierend auf die unabhängige Variable für alle von der Beobachtungen aus. Der Webdienst erwartet Endbenutzer zum Eingeben von Daten als eine Zeichenfolge zurück, wobei Zeilen werden durch Kommas (,) getrennt und Spalten werden durch ein Semikolon (;) getrennt. Der Webdienst erwartet 1 Zeile jeweils und die erste Spalte werden die abhängige Variable erwartet. Ein Beispieldataset könnte wie folgt aussehen:

![Beispieldaten][1]

Beobachtungen ohne eine abhängige Variable sollte für y als "NV" eingegeben werden. Die Dateneingabe für das oben angegebenen Dataset die folgende Zeichenfolge wäre: "10 5; 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2; 1, NV; 3; 4". Die Ausgabe ist der geschätzten Wert für jede Zeile basierend auf die unabhängige Variable. 

>Dieser Dienst, ist wie auf dem Azure Marketplace, gehostet ein OData-Dienst; Diese möglicherweise über POST oder GET-Methoden aufgerufen werden. 

Es gibt mehrere Methoden für die Nutzung des Diensts in einer Weise automatisierte (eine Beispiel-app ist [hier](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Starten Sie C#-Code für Webdiensten:

    public class Input
    {
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
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


Aus innerhalb von Azure Computer lernen, eine neue, leere experimentieren erstellt wurde und zwei [Ausführen R Skript] [ execute-r-script] Module auf den Arbeitsbereich angefordert wurden. Dieser Webdienst wird eine Azure maschinellen Learning Experimentieren mit einem zugrunde liegenden R Skript ausgeführt. Es gibt 2 Webparts zu diesem Versuch: Schemadefinition, und Schulung Modell + bewerten. Das erste Modul definiert die erwartete Struktur des Eingabe-Dataset, in dem die erste Variable ist die abhängige Variable und die restlichen Variablen unabhängig sind. Das zweite Modul passt ein Modells generische lineare Regression für die Eingabedaten.  
  
![Experimentieren Fluss][3]

####<a name="module-1"></a>Modul 1:
 
####<a name="schema-definition"></a>Schemadefinition  
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

####<a name="module-2"></a>Modul 2:
####<a name="lm-modeling"></a>LM Modellierung   
    data <- maml.mapInputPort(1) # class: data.frame  
  
    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  
 
##<a name="limitations"></a>Einschränkungen
Dies ist ein einfaches Beispiel eines Webdiensts mehrere lineare Regression. Wie aus dem obigen Beispielcode gesehen werden können, wird keine Aufspüren von Fehlern implementiert, und der Dienst wird davon ausgegangen, dass alles ein fortlaufender Variable (keine kategorisierten Features zulässig) ist als den Dienst nur Eingaben numerische Werte zum Zeitpunkt der Erstellung der dieser Webdienst. Darüber hinaus übernimmt der Dienst derzeit begrenzte Datengröße, Fälligkeitsdatum der Anforderung/Antwort Art der Web Service-Anruf und der Fakultät, dass das Modell jedes Mal anpassen wird ist, die der Webdienst aufgerufen wird. 

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen über den Verbrauch der Webdienst oder Veröffentlichung zum Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
