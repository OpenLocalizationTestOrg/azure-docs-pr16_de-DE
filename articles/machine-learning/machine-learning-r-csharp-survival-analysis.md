<properties 
    pageTitle="Survival Analyse mit Azure-Computern Learning | Microsoft Azure" 
    description="Survival Analyse Ereignis Vorkommen Wahrscheinlichkeit" 
    services="machine-learning" 
    documentationCenter="" 
    authors="zhangya" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="zhangya"/> 


#<a name="survival-analysis"></a>Survival Analyse 

Unter viele Szenarien ist das Hauptfenster Ergebnis unter Bewertung der Uhrzeit auf ein Ereignis von Interesse. Kurzum, die Frage "Wenn dieses Ereignis stattfindet?" aufgefordert wird. Beispiele, sollten Sie Situationen, in dem die Daten die verstrichene Zeit (Tage, Jahre, mit usw. beschreibt), bis das Ereignis relevante (wegen Krankheit, PhD Grad erhalten, einer Wähltastatur Fehler) auftritt. Jede Instanz in den Daten steht für ein bestimmtes Objekt (Patienten, ein Student, ein Auto usw.).


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Dieser [Webdienst]( https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) finden Sie Antworten auf die Frage "Welche ist die Wahrscheinlichkeit, dass das Ereignis relevante durch Zeit n für Objekt X? stattfindet" Durch die Bereitstellung eines Survival Analysis-Modells, ermöglicht dieser Webdienst Personen eingeben von Daten, um das Modell Schulen und zu testen. Das Hauptfenster Design der den Versuch besteht darin, die Länge der verstrichenen Zeit modellieren, bis das Ereignis relevante auftritt. 

>Dieser Webdienst könnte beispielsweise von den Benutzern – potenziell über eine mobile-app, über eine Website oder sogar auf einem lokalen Computer genutzt werden. Aber der Zweck des Webdiensts ist auch dienen als Beispiel wie Azure maschinellen Schulung zum Erstellen von Webdienste auf R Code verwendet werden können. Mit nur wenigen Codezeilen R und Mausklicks eine Schaltfläche innerhalb von Azure maschinellen Learning Studio werden einem Versuch mit R Code erstellt und als Webdienst veröffentlicht. Der Webdienst kann dann zum Azure Marketplace veröffentlicht und verbraucht Benutzer und Geräte auf der ganzen Welt ohne Installation Infrastruktur vom Autor des Webdiensts.  

##<a name="consumption-of-web-service"></a>Verbrauch des Webdiensts

In der folgenden Tabelle wird das Eingabedaten Schema des Webdiensts angezeigt. 6 Stück der Informationen als Eingabe erforderlich sind: Schulung Daten, testen die Daten, relevante Index "Uhrzeit" Uhrzeit dimension, Index "Event" Dimension und welche Variable (fortlaufender oder Faktor). Die Schulungsdaten werden mit einer Zeichenfolge dargestellt, wo die Zeilen werden durch Kommas getrennt, und die Spalten durch Semikolon getrennt werden. Die Anzahl der Features von Daten ist flexibel. Alle Elemente in die eingegebene Zeichenfolge müssen numerisch sein. In den Daten Schulung zeigt die Dimension "Time" die Anzahl der Zeiteinheiten (Tage, Jahre, mit usw.) sind seit dem Ausgangspunkt der Untersuchung (Patienten empfangen Medikamenten Behandlung Programme, ein Student Anfangs PhD Fallstudie, ein Auto, beginnen Sie mit der gesteuert werden, usw.), bis das Ereignis relevante (den Patienten zur Rückkehr zur Verwendung von Medikamenten, die Student auftritt, der das Ausmaß PhD, das Probleme bei der Wähltastatur einer Auto usw..) Tritt auf. Die Dimension "Event" gibt an, ob das Ereignis relevante am Ende der Fallstudie auftritt. Der Wert "Ereignis = 1" bedeutet, dass die das Ereignis relevante zum Zeitpunkt der Dimension "Time"; erkennbar eintritt "Ereignis = 0" bedeutet, die das Ereignis relevante nicht durch die durch die Dimension "Time" angegebene Zeit aufgetreten ist.

- Trainingdata - Zeichenfolge. Zeilen werden durch Trennzeichen getrennt und Spalten werden durch Semikolon getrennt. Jede Zeile enthält Dimension "Time", "Event" Dimension und Vorhersage Variablen.
- Testingdata - eine Zeile mit Daten, die Vorhersage Variablen für ein bestimmtes Objekt enthält.
- Time_of_interest - der verstrichenen Zeit des Zinsen n.
- Index_time - der Spaltenindex der Dimension "Time" (beginnend mit 1).
- Index_event - der Spaltenindex der Dimension "Event" (beginnend mit 1).
- Variable_types - Zeichenfolge mit Semikolons als Trennzeichen darin. 0 fortlaufende Variablen und 1 darstellt Faktor Variablen.


Die Ausgabe ist die Wahrscheinlichkeit eines Ereignisses auftreten, indem Sie eine bestimmte Uhrzeit. 

>Dieser Dienst, ist wie auf dem Azure Marketplace, gehostet ein OData-Dienst; Diese möglicherweise über POST oder GET-Methoden aufgerufen werden. 

Es gibt mehrere Methoden für die Nutzung des Diensts in einer Weise automatisierte (eine Beispiel-app ist [hier](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

###<a name="starting-c-code-for-web-service-consumption"></a>Starten Sie C#-Code für Webdiensten:
    public class Input
    {
            public string trainingdata;
            public string testingdata;
            public string timeofinterest;
            public string indextime;
            public string indexevent;
            public string variabletypes;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { trainingdata = TextBox1.Text, testingdata = TextBox2.Text, timeofinterest = TextBox3.Text, indextime = TextBox4.Text, indexevent = TextBox5.Text, variabletypes = TextBox6.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




Die Interpretation dieser Prüfung wird wie folgt aus. Das Ziel der Daten Voraussetzung besteht darin, die verstrichene Zeit bis zur Rückgabe von Medikamenten Einsatz für die Patienten modellieren, die eine der beiden Behandlung Programme erhalten. Die Ausgabe des Web Service lautet: für Patienten wird 35 Jahre alt, Probleme vorherigen Menge von Medikament Behandlung 2 Mal das Programm lange lokales Behandlung aufzeichnen und mit sowohl Heroin und Cocaine Verwendung, die Wahrscheinlichkeit, dass der Einsatz von Medikamenten zurückgeben 95.64 % nach Tag 500 ist.

##<a name="creation-of-web-service"></a>Erstellen von Webdienst

>Dieser Webdienst wurde mithilfe von Azure maschinellen Learning erstellt. Kostenlose Testversion sowie Einführung Videos zum Erstellen von Versuche und [publishing-Webdiensten](machine-learning-publish-a-machine-learning-web-service.md)finden Sie unter [azure.com/ml](http://azure.com/ml). Es folgt ein Screenshot der Versuch, die den Web-Dienst und Beispiel-Code für jede der Module in den Versuch erstellt.

Aus innerhalb von Azure Computer lernen, eine neue, leere experimentieren erstellt wurde und zwei [Ausführen R Skript] [ execute-r-script] Module auf den Arbeitsbereich angefordert wurden. Das Datenschema erstellt wurde, mit einem einfachen [R Skript ausführen][execute-r-script], die im Schema Eingabedaten für den Webdienst definiert. Dieses Modul verknüpft ist dann das zweite [R Skript ausführen] [ execute-r-script] Modul, das Arbeit Haupt-. In diesem Modul unterstützt Vorbearbeiten von Daten, das Modell Gebäude und Vorhersagen. Im Schritt Daten vorverarbeitenden die Eingabedaten durch eine lange Zeichenfolge dargestellt transformiert und in einem Datenrahmen konvertiert wurde. Im Modell Gebäude Schritt wird eine externe R Paket "survival_2.37-7.zip" zuerst für die Durchführung von Survival Analysis installiert. Dann wird die Funktion "Coxph" nach einer Reihe Datenverarbeitung Vorgänge ausgeführt. Die Details der Funktion "Coxph" für die Analyse überleben können aus der Dokumentation R gelesen werden. Im Schritt Vorhersage eine Instanz Testen in das Datenmodell ausgebildeten, mit der Funktion "Surfit" angegeben ist und die Kurve Survival für diese Instanz testen als "Kurven" Variable erstellt wird. Schließlich wird die Wahrscheinlichkeit, dass die Uhrzeit relevante abgerufen. 

###<a name="experiment-flow"></a>Experimentieren Fluss:

![Experimentieren Fluss][1]

####<a name="module-1"></a>Modul 1:

    #Data schema with example data (replaced with data from web service)
    trainingdata="53;1;29;0;0;3,79;1;34;0;1;2,45;1;27;0;1;1,37;1;24;0;1;1,122;1;30;0;1;1,655;0;41;0;0;1,166;1;30;0;0;3,227;1;29;0;0;3,805;0;30;0;0;1,104;1;24;0;0;1,90;1;32;0;0;1,373;1;26;0;0;1,70;1;36;0;0;1”
    testingdata="35;2;1;1"
    time_of_interest="500"
    index_time="1"
    index_event="2"
    
    # 0 - continuous; 1 -  factor
    variable_types="0;0;1;1"

    sampleInput=data.frame(trainingdata,testingdata,time_of_interest,index_time,index_event,variable_types)

    maml.mapOutputPort("sampleInput"); #send data to output port
    
####<a name="module-2"></a>Modul 2:

    #Read data from input port
    data <- maml.mapInputPort(1) 
    colnames(data) <- c("trainingdata","testingdata","time_of_interest","index_time","index_event","variable_types")

    # Preprocessing training data
    traindingdata=data$trainingdata
    y=strsplit(as.character(data$trainingdata),",")
    n_row=length(unlist(y))
    z=sapply(unlist(y), strsplit, ";", simplify = TRUE)
    mydata <- data.frame(matrix(unlist(z), nrow=n_row, byrow=T), stringsAsFactors=FALSE)
    n_col=ncol(mydata)

    # Preprocessing testing data
    testingdata=as.character(data$testingdata)
    testingdata=unlist(strsplit(testingdata,";"))

    # Preprocessing other input parameters
    time_of_interest=data$time_of_interest
    time_of_interest=as.numeric(as.character(time_of_interest))
    index_time = data$index_time
    index_event = data$index_event
    variable_types = data$variable_types

    # Necessary R packages
    install.packages("src/packages_survival/survival_2.37-7.zip",lib=".",repos=NULL,verbose=TRUE)
    library(survival)

    # Prepare to build model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct the execution string
    for (i in 1:len){
    if(i==len){
    if(variable_types[i]!=0){ c=paste(c, "factor(",v_predictors[i],")",sep="")}
     else{ c=paste(c, v_predictors[i])}
    }else{
    if(variable_types[i]!=0){c=paste(c, "factor(",v_predictors[i],") + ",sep="")}
    else{c=paste(c, v_predictors[i],"+")}
    }
    }
    f=paste("coxph(Surv(",d_time,",",d_event,") ~")
    f=paste(f,c)
    f=paste(f,", data=mydata )")

    # Fit a Cox proportional hazards model and get the predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find the event occurrence probability
    position_closest=which.min(abs(prob_event$time - time_of_interest))

    if(prob_event[position_closest,"time"]==time_of_interest){# exact match
    output=prob_event[position_closest,"prob"]
    }else{# not exact match
    if(time_of_interest>max(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else if(time_of_interest<min(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else{output=(prob_event[position_closest,"prob"]+prob_event[position_closest+1,"prob"])/2}
    }

    #Pull out results to send to web service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




##<a name="limitations"></a>Einschränkungen

Dieser Webdienst kann nur numerische Werte als Feature Variablen (Spalten) enthalten. Die Spalte "Ereignis" kann nur der Wert 0 oder 1 dauern. Die Spalte "Time" muss eine positive ganze Zahl sein.

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen über den Verbrauch der Webdienst oder Veröffentlichung zum Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
