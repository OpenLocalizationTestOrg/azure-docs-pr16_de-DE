<properties 
    pageTitle="Cluster-Modell | Microsoft Azure" 
    description="Cluster-Modell" 
    services="machine-learning" 
    documentationCenter="" 
    authors="FrancescaLazzeri" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="lazzeri"/> 


#<a name="cluster-model"></a>Cluster-Modell    

Wie können wir Gruppen Kreditkarte Inhaber Verhaltensweisen Vorhersagen, um Belastung Kreditkartenunternehmen zu verringern? Wie können wir Persönlichkeitsabhängige Eigenschaften von Mitarbeitern Gruppen definieren, um die Leistung bei der Arbeit? Wie können Ärzte Patienten in Gruppen basierend auf den Merkmalen von deren Krankheiten zu klassifizieren? Grundsätzlich können alle dieser Fragen über Cluster Analysefunktionen beantwortet werden.   


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
   
Cluster Analyse wird eine Reihe von Beobachtungen in zwei oder mehr gegenseitig unbekannten Gruppen basierend auf Kombinationen von Variablen klassifiziert. Cluster Analyse dient zum Ermitteln eines Systems der Beobachtungen, in der Regel Personen oder ihrer Merkmale in Gruppen, organisieren, freigeben Mitglieder der Gruppen Eigenschaften gemeinsam. Dieser [Service](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) verwendet den K-bedeutet, dass Methoden, eine häufig verwendete Cluster Methode, um Cluster beliebige Daten in Gruppen an. Dieser Webdienst übernimmt die Daten und die Anzahl der k Cluster als Eingabe und erzeugt Vorhersagen von denen der einzelnen Beobachtungen gehört k Gruppen. 

>Dieser Webdienst könnte beispielsweise von den Benutzern – potenziell über eine mobile-app, über eine Website oder sogar auf einem lokalen Computer genutzt werden. Aber der Zweck des Webdiensts ist auch dienen als Beispiel wie Azure maschinellen Schulung zum Erstellen von Webdienste auf R Code verwendet werden können. Mit nur wenigen Codezeilen R und Mausklicks eine Schaltfläche innerhalb von Azure maschinellen Learning Studio werden einem Versuch mit R Code erstellt und als Webdienst veröffentlicht. Der Webdienst kann dann zum Azure Marketplace veröffentlicht und verbraucht Benutzer und Geräte auf der ganzen Welt ohne Installation Infrastruktur vom Autor des Webdiensts.  

##<a name="consumption-of-web-service"></a>Verbrauch des Webdiensts   
Dieser Webdienst gruppiert die Daten in eine Reihe von k Gruppen und gibt die gruppenzuordnung für jede Zeile aus. Der Webdienst erwartet Endbenutzer zum Eingeben von Daten als eine Zeichenfolge zurück, wobei Zeilen werden durch Kommas (,) getrennt und Spalten werden durch ein Semikolon (;) getrennt. Der Webdienst erwartet 1 Zeile jeweils ein. Ein Beispieldataset könnte wie folgt aussehen:

![Beispieldaten][1]

Nehmen Sie an, dass der Benutzer diese Daten in 3 gegenseitig Gruppen aufteilen möchten. Die Dateneingabe für das oben angegebenen Dataset folgenden wäre: Wert = "10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4"; k = "3". Die Ausgabe ist die geschätzte Gruppenmitgliedschaft für jede Zeile an.

>Dieser Dienst, ist wie auf dem Azure Marketplace, gehostet ein OData-Dienst; Diese möglicherweise über POST oder GET-Methoden aufgerufen werden. 

Es gibt mehrere Methoden für die Nutzung des Diensts in einer Weise automatisierte (eine Beispiel-app ist [hier](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Starten Sie C#-Code für Webdiensten:

    public class Input
    {
            public string value;
            public string k;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
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

Aus innerhalb von Azure Computer lernen, eine neue, leere experimentieren erstellt wurde und zwei [Ausführen R Skript] [ execute-r-script] Module herausgezogen sind, sodass Sie auf den Arbeitsbereich. Das Datenschema erstellt wurde, mit einem einfachen [R Skript ausführen][execute-r-script]. Klicken Sie dann das Schema der Cluster im Abschnitt Modell erneut mit einem [Ausführen R Skript]erstellt verknüpfte wurde[execute-r-script]. Im [Ausführen R Skript] [ execute-r-script] für das Clustermodell verwendet, nutzt der Webdienst klicken Sie dann die Funktion "k-bedeutet", also vordefinierten in das [R Skript ausführen] [ execute-r-script] von Azure maschinellen Schulung.    
   

     
![Experimentieren Fluss][3]

####<a name="module-1"></a>Modul 1: 
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)
    
    maml.mapOutputPort("mydata");     
    

####<a name="module-2"></a>Modul 2:
    # Map 1-based optional input ports to variables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");
   
 
##<a name="limitations"></a>Einschränkungen
Dies ist ein sehr einfaches Beispiel für einen Cluster Webdienst. Wie aus dem obigen Beispielcode gesehen werden können, wird keine Aufspüren von Fehlern implementiert, und der Dienst wird davon ausgegangen, dass alles ein fortlaufender Variable (keine kategorisierten Features zulässig) ist als den Dienst nur Eingaben numerische Werte zum Zeitpunkt der Erstellung der dieser Webdienst. Darüber hinaus übernimmt der Dienst derzeit begrenzte Datengröße, Fälligkeitsdatum der Anforderung/Antwort Art der Web Service-Anruf und der Fakultät, dass das Modell jedes Mal anpassen wird ist, die der Webdienst aufgerufen wird. 

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen über den Verbrauch der Webdienst oder Veröffentlichung zum Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
