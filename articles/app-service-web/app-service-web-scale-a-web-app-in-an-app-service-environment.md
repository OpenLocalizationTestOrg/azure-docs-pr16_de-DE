<properties 
    pageTitle="Zum Skalieren einer App in einer App-Service-Umgebung" 
    description="Skalierung der app in einer App-Service-Umgebung" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="ccompy"/>

# <a name="scaling-apps-in-an-app-service-environment"></a>Anpassungsbereich für apps in einer App-Service-Umgebung #

In der App-Verwaltungsdienst Azure sind normalerweise drei Punkte, die Sie skalieren können:

- Preise plan
- Worker Größe 
- Anzahl der Instanzen.

In einer ASE besteht keine Notwendigkeit auswählen oder ändern den Plan Preisgestaltung.  Auch mithilfe von Funktionen ist es bereits bei einer Premium Preise Videofunktionen Ebene aus.  

In Bezug auf Worker Auswahl an Papiergrößen weisen der Administrator ASE die Größe der Ressource berechnen, für jeden Worker Pool verwendet werden soll.  Dies bedeutet können Sie Worker Pool 1 mit P4 berechnen Ressourcen und Worker Pool 2 mit P1 berechnen Ressourcen, bei Bedarf.  Sie haben keine Größe erfüllt sein.  Für details, um die Größe und deren Preisgestaltung finden Sie unter dem Dokument hier [Azure App Preisen][AppServicePricing].  Dies bewirkt, dass die Skalierungsoptionen für Web apps und App Service-Pläne in einer App-Service-Umgebung sein:

- Worker Ressourcenpool Auswahl
- Anzahl der Instanzen

Ändern eines Elements erfolgt über die entsprechenden Benutzeroberfläche für Ihre ASE App Service-Pläne gehostet werden.  

![][1]

Sie können nicht von der ASP abgesehen von der Anzahl der verfügbaren berechnen Ressourcen im Pool Worker skalieren, denen Ihre ASP ist.  Wenn Sie Ressourcen in diesem Pool Worker berechnen müssen müssen Sie abrufen den ASE-Administrator, um sie hinzuzufügen.  Weitere Informationen, um erneut konfigurieren Ihrer ASE, lesen Sie hier die Informationen: [wie Sie eine App-Service-Umgebung konfigurieren][HowtoConfigureASE].  Sie möchten möglicherweise auch nutzen der ASE automatisch skalieren Features hinzufügen Kapazität auf Zeitplan oder Kennzahlen basieren.  Weitere Details zum Konfigurieren von automatisch skalieren für die ASE abzurufenden Umgebung selbst finden Sie unter [Skalieren für eine App-Service-Umgebung konfigurieren][ASEAutoscale].

Sie können mehrere app berechnen Ressourcen aus verschiedenen Worker Pools mit Servicepläne erstellen oder können den gleichen Worker Pool.  Beispiel für Wenn Worker Pool 1 (10) verfügbar berechnen Ressourcen enthält, die Möglichkeit, eine app-Serviceplan verwenden (6) berechnen Ressourcen erstellen und Planen einer zweiten app Service (4) Ressourcen zu berechnen.

### <a name="scaling-the-number-of-instances"></a>Die Anzahl der Instanzen skalieren ###

Beim Erstellen Sie zuerst der Web-app in einer App-Service-Umgebung, die ihn mit 1 Instanz gestartet.  Sie können, klicken Sie dann auf weitere Instanzen berechnen zusätzlichen Ressourcen für Ihre app bereitgestellt skalieren.   

Wenn Ihre ASE ausreichend Kapazität hat ist dies recht einfach.  Sie wechseln Sie zu der App-Dienst planen, die die Websites enthält, die Sie nach oben skalieren, und wählen Sie skalieren möchten.  Daraufhin wird die Benutzeroberfläche können manuell die Skalierung der für Ihre ASP oder automatisch skalieren Regeln für Ihre ASP konfigurieren.  Manuelles Maßstab festzulegen Ihre app einfach ***skalieren, indem Sie*** ***eine Instanz***zählen, die ich manuell eingeben.  Von hier aus den Schieberegler auf die gewünschte Menge oder geben Sie es in das Feld neben dem Schieberegler ein.  

![][2] 

Die Regeln automatisch skalieren für einen ASP in einer ASE funktionieren genauso wie gewohnt.  Können ***Prozentsatz der CPU*** unter ***Skalieren durch*** auswählen und automatisch skalieren Regeln für Ihre ASP basierend auf Prozentsatz der CPU erstellen, oder Sie können komplexere Regeln verwenden ***Zeitplan und Leistung von Regeln***erstellen.  Verwenden Sie den Leitfaden zum genauere Details zum Konfigurieren von finden Sie unter Skalieren hier [Skalieren eine app im App-Verwaltungsdienst Azure][AppScale]. 


### <a name="worker-pool-selection"></a>Arbeitskollegen Ressourcenpool Auswahl ###

Wie bereits zuvor erwähnt, wird die Worker Ressourcenpool Auswahl aus der ASP-Benutzeroberfläche zugegriffen werden.  Öffnen Sie das Blade für die ASP, die Sie skalieren, und wählen Sie Worker Ressourcenpool möchten.  Alle der Worker Pools werden angezeigt, die Sie in Ihrer Umgebung der App-Dienst konfiguriert haben.  Wenn Sie nur eine Arbeitskollegen Ressourcenpool haben sehen Sie nur einen Pool aufgeführt.  Um welche Worker Ressourcenpool in Ihrer ASP ist zu ändern, wählen Sie einfach Worker Pool, den Sie Ihre App-Dienst planen wechseln möchten.  

![][3]

Vor dem Verschieben Ihrer ASP aus einem Worker Pool zu einem anderen ist es wichtig, um sicherzustellen, dass Sie ausreichend Kapazität für Ihre ASP haben werden.  In der Liste der Worker Pools nicht nur wird der Name der Worker Ressourcenpool aufgeführt aber Sie können auch anzeigen, wie viele Mitarbeiter in diesem Pool Arbeitskollegen verfügbar sind.  Stellen Sie sicher, dass es gibt genügend Instanzen enthalten Planen Ihrer App Service zur Verfügung.  Wenn Sie weitere Ressourcen im Pool Worker Sie wechseln möchten berechnen müssen, klicken Sie dann erhalten des Administrators ASE, um sie hinzuzufügen.  

> [AZURE.NOTE] Verschieben, dass eine ASP aus einem Worker Pool Haut unterbindet beginnt der apps in dieser ASP.  Dadurch können Anfragen langsamer ausgeführt wird, wie Ihre app kalt gestartet wird, klicken Sie auf die neue Ressourcen berechnen wird.  Kalt starten kann mithilfe der [Anwendung von einsatzbereiten von Videofunktionen] vermieden werden[ AppWarmup] in Azure-App-Dienst.  Das Anwendung Initialisierung Modul im Artikel beschriebenen funktioniert auch für kalt Starts, da es sich bei der Initialisierungsprozess auch aufgerufen wird, wenn apps kalt Schritte auf neu berechnen Ressourcen sind. 

## <a name="getting-started"></a>Erste Schritte

Um mit der App-Service-Umgebungen anzufangen, finden Sie unter [Wie zum Erstellen einer App Service-Umgebung][HowtoCreateASE]

Weitere Informationen über die App-Verwaltungsdienst Azure-Plattform finden Sie unter [Azure-App-Verwaltungsdienst][AzureAppService].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
