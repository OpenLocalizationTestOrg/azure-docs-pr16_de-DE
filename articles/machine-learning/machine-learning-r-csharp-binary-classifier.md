<properties 
    pageTitle="Binäre Klassifizierung | Microsoft Azure" 
    description="Binäre Klassifizierung" 
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



#<a name="binary-classifier"></a>Binäre Klassifizierung

Angenommen Sie, Sie haben ein Dataset und eine binäre abhängige Variable basierend auf die unabhängigen Variablen vorhersagen möchten. 'Logistische Regression' ist eine beliebte statistische Methode für solche Vorhersagen verwendet. Hier die abhängige Variable binäre oder dichotomous, und p ist die Wahrscheinlichkeit eines Vorhandensein von die Eigenschaft von Interesse. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Ein einfaches Szenario könnte, wo eine Recherche-Assistenten will Vorhersagen, ob ein potenzieller Student ein Angebots Aufnahme zu einer Universität basierend auf Informationen (Durchschnittsnote liegt in Oberstufe, familiäre Einkünfte, resident Bundesland gender) akzeptiert wird. Das geschätzte Ergebnis ist die Wahrscheinlichkeit, dass die potenzieller Student Aufnahme Ihr Angebot annehmen. Diesen [Webdienst]( https://datamarket.azure.com/dataset/aml_labs/log_regression) passt logistische Regressionsmodell auf die Daten und gibt den Wahrscheinlichkeitswert (y) für jede der Beobachtungen für die Daten aus.  
  
>Dieser Webdienst könnte beispielsweise von den Benutzern – potenziell über eine mobile-app, über eine Website oder sogar auf einem lokalen Computer genutzt werden. Aber der Zweck des Webdiensts ist auch dienen als Beispiel wie Azure maschinellen Schulung zum Erstellen von Webdienste auf R Code verwendet werden können. Mit nur wenigen Codezeilen R und Mausklicks eine Schaltfläche innerhalb von Azure maschinellen Learning Studio werden einem Versuch mit R Code erstellt und als Webdienst veröffentlicht. Der Webdienst kann dann zum Azure Marketplace veröffentlicht und verbraucht Benutzer und Geräte auf der ganzen Welt Zeichnungsblatts keine Infrastruktur vom Autor des Webdiensts.  
  

##<a name="consumption-of-web-service"></a>Verbrauch des Webdiensts  
Dieser Webdienst gibt die geschätzte Werte für die abhängige Variable basierend auf die unabhängige Variable für alle von der Beobachtungen aus. Der Webdienst erwartet den Endbenutzer auf Eingabedaten als eine Zeichenfolge zurück, wobei Zeilen werden durch Semikolon (;) getrennt und Spalten werden durch Semikolon (;) getrennt. Der Webdienst erwartet 1 Zeile jeweils und die erste Spalte werden die abhängige Variable erwartet. Ein Beispieldataset könnte wie folgt aussehen:

![Beispieldaten][1]

Beobachtungen ohne eine abhängige Variable sollte für y als "NV" eingegeben werden. Die Dateneingabe für das oben angegebenen Dataset die folgende Zeichenfolge wäre: "1; 5; 2,1; 1; 6,0; 5.3; 2.1,0; 5; 5,0; 3; 4,1; 2; 1; NV; 3; 4". Die Ausgabe ist der geschätzten Wert für jede Zeile basierend auf die unabhängige Variable. 

>Diesen Dienst, ist wie auf dem Azure Marketplace, gehostet ein OData-Dienst. Diese möglicherweise über POST oder GET-Methoden aufgerufen werden. 

Es gibt mehrere Methoden für die Nutzung des Diensts in einer Weise automatisierte (eine Beispiel-app ist [hier](http://microsoftazuremachinelearning.azurewebsites.net/BinaryClassifier.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Starten Sie C#-Code für Webdiensten an:

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

Aus innerhalb von Azure Computer lernen, eine neue, leere experimentieren erstellt wurde und zwei [Ausführen R Skript] [ execute-r-script] Module herausgezogen sind, sodass Sie auf den Arbeitsbereich. Dieser Webdienst wird eine Azure maschinellen Learning Experimentieren mit einem zugrunde liegenden R Skript ausgeführt. Es gibt 2 Webparts zu diesem Versuch: Schemadefinition, und Schulung Modell + bewerten. Das erste Modul definiert die erwartete Struktur des Eingabe-Dataset, in dem die erste Variable ist die abhängige Variable und die restlichen Variablen unabhängig sind. Das zweite Modul passt eine generische logistische Regressionsmodell für die Eingabedaten an.    

![Experimentieren Fluss][2]

####<a name="module-1"></a>Modul 1:

    #Schema definition  
    data <- data.frame(value = "1;2;3,1;5;6,0;8;9", stringsAsFactors=FALSE) 
    maml.mapOutputPort("data");  

####<a name="module-2"></a>Modul 2:
    #GLM modeling   
    data <- maml.mapInputPort(1) # class: data.frame  
    
    data.split <- strsplit(data[1,1], ",")[[1]] 
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE) 
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE) 
    data.split <- as.data.frame(t(data.split)) data.split <- 
    data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    
    model <- glm(data.split$V1 ~., family='binomial', data=data.split)  
    out <- data.frame(predict(model,data.split, type="response")) 
    pred1 <- as.data.frame(out) 
    group <- array(1:nrow(pred1)) 
    for (i in 1:nrow(pred1))  
        {
        if(as.numeric(pred1[i,])>0.5) {group[i]=1} 
        else {group[i]=0}
        } 
    pred2 <- as.data.frame(group) 
    maml.mapOutputPort("pred2");  


##<a name="limitations"></a>Einschränkungen
Dies ist ein einfaches Beispiel von einem Webdienst binäre Klassifizierung. Wie aus dem obigen Beispielcode gesehen werden können, wird keine Aufspüren von Fehlern implementiert und Dienst wird davon ausgegangen, dass alles in eine Binärzahl/fortlaufender Variable (keine kategorisierten Features zulässig) ist als der Dienst nur Eingaben numerischen Werte zum Zeitpunkt der Erstellung der dieser Webdienst. Darüber hinaus übernimmt der Dienst derzeit begrenzte Datengröße, Fälligkeitsdatum der Anforderung/Antwort Art der Web Service-Anruf und der Fakultät, dass das Modell jedes Mal anpassen wird ist, die der Webdienst aufgerufen wird. 

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen über den Verbrauch der Webdienst oder Veröffentlichung zum Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-binary-classifier/binary1.png
[2]: ./media/machine-learning-r-csharp-binary-classifier/binary2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
