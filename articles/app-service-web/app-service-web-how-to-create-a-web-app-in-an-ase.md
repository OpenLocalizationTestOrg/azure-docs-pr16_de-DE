<properties
    pageTitle="Erstellen Sie eine Web-app in einer App-Service-Umgebung"
    description="Erfahren Sie, wie Sie Web apps und app Dienstpläne in einer App-Service-Umgebung zu erstellen."
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="create-a-web-app-in-an-app-service-environment"></a>Erstellen Sie eine Web-app in einer App-Service-Umgebung

## <a name="overview"></a>(Übersicht)

In diesem Lernprogramm erfahren, wie in einer [App-Service-Umgebung](app-service-app-service-environment-intro.md) (ASE) Web apps und App-Service-Pläne erstellen. 

> [AZURE.NOTE] Wenn Sie erfahren, wie eine Web app erstellen möchten, aber nicht in einer App-Service-Umgebung erledigen müssen, finden Sie unter [Erstellen einer .NET Web app](web-sites-dotnet-get-started.md) oder eine der zugehörigen Lernprogramme für andere Sprachen und Framework.

## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Lernprogramm wird davon ausgegangen, dass Sie eine App-Service-Umgebung erstellt haben. Wenn Sie die noch nicht getan haben, finden Sie unter [Erstellen einer App-Service-Umgebung](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Erstellen Sie eine Web-app

1. Klicken Sie im [Portal Azure](https://portal.azure.com/)auf **Neu > Web + Mobile > Web App**. 

    ![][1]

2. Wählen Sie Ihr Abonnement.  

    Achten Sie darauf, um eine app in Ihrem App-Service-Umgebung zu erstellen, müssen Sie im selben Abonnement verwenden, das Sie beim Erstellen der Umgebung verwendet, wenn Sie mehrere Abonnements haben. 

3. Wählen Sie aus, oder erstellen Sie eine Ressourcengruppe aus.

    *Ressourcengruppen* aktivieren Sie zum Verwalten von Azure Ressourcenübersicht als Einheit und sind nützlich, wenn die *Steuerung des Benutzerzugriffs rollenbasierte* (RBAC) Regeln für Ihre apps einrichten. Weitere Informationen finden Sie unter [Verwalten von Azure Ressourcen][ResourceGroups]. 

4. Wählen Sie aus, oder erstellen Sie eine App Serviceplan.

    *App-Service-Pläne* werden verwaltet Sätze von Web apps.  Normalerweise beim Preise auswählen, wird der Preis belastet mit der App-Serviceplan statt mit der einzelnen apps angewendet. In einer ASE, die Sie für die für die ASE reserviert Computinginstanzen Zahlen statt, was Sie mit Ihrem ASP aufgelistet haben.  Um die Anzahl der Instanzen von einer Web-app, die Sie von der Instanzen von der App-Verwaltungsdienst skalieren zu skalieren planen, und er wirkt sich auf alle Web apps in diesem Plan.  Einige Features wie Website Steckplätze oder VNET Integration besitzen außerdem Menge Einschränkungen innerhalb des Plans.  Weitere Informationen finden Sie unter [Übersicht über die App-Verwaltungsdienst Azure-Pläne](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)

    Identifizieren von der App-Dienst Plänen in Ihrer ASE anhand der Position, die unter dem Namen Plan erwähnt wird.  

    ![][5]

    Wenn Sie eine App Serviceplan zu verwenden, die bereits in Ihrem App-Service-Umgebung möchten, wählen Sie diesen aus. Wenn Sie einen neuen App-Serviceplan erstellen möchten, finden Sie im folgenden Abschnitt dieses Lernprogramms, [Erstellen Sie eine App Serviceplan in einer App-Service-Umgebung](#createplan).

5. Geben Sie den Namen für Ihre Web-app, und klicken Sie dann auf **Erstellen**. 

    Wenn Ihre ASE einer externen VIP verwendet ist die URL einer app in einer ASE: [*Sitename*]. [*Name der Ihre App-Service-Umgebung*]. [*Sitename*] anstelle von p.azurewebsites.net. azurewebsites.net
    
    Wenn Ihre ASE dieses ASE ist eine interne VIP und dann die URL der app verwendet: [*Sitename*]. [*Unterdomäne angegeben, während der Erstellung ASE*]   
    Nachdem Sie Ihre ASP während der Erstellung ASE ausgewählt haben, sehen Sie die Unterdomäne unter **Namen** aktualisieren

## <a name="a-namecreateplana-create-an-app-service-plan"></a><a name="createplan"></a>Erstellen eines App-Serviceplan

Wenn Sie eine App Serviceplan in einer App-Service-Umgebung erstellen, unterscheiden sich Ihre Arbeitskollegen Auswahlmöglichkeiten keine freigegebenen Kollegen in einem ASE vorhanden sind.  Die Kollegen, die Sie verwenden müssen, sind diejenigen, die die ASE durch den Administrator zugewiesen wurden  Dies bedeutet, dass zum Erstellen eines neuen Plans Sie weitere Kollegen Ihre ASE Worker Pool als die Gesamtzahl der Instanzen über alle Ihre Pläne bereits in dieser Worker Pool zugewiesenen haben müssen.  Wenn Sie über genügend Kollegen in Ihrer ASE Worker Ressourcenpool Ihres Plans erstellen besitzen, müssen Sie für die Arbeit mit Ihrem Administrator ASE abrufen hinzugefügt.

Ein weiterer Unterschied mit App-Service-Pläne, die von einer App-Service-Umgebung gehostet ist der fehlende Preise Auswahl.  Wenn Sie eine App-Service-Umgebung verfügen, die Sie Zahlen für berechnen Ressourcen, die vom System verwendet und haben keine zusätzliche Gebühren für die Pläne in dieser Umgebung.  Beim Erstellen eines App-Serviceplan wählen Sie normalerweise eine Preisgestaltung Planen der Ihrer Rechnungen bestimmt.  Eine App-Service-Umgebung ist im Wesentlichen einem privaten Ort, wo Sie Inhalt erstellen können.  Sie Zahlen für die Umgebung und nicht um den Inhalt zu hosten.

So erstellen Sie eine App Serviceplan während der Erstellung einer Web-app, wie im vorherigen Abschnitt des Lernprogramms erläutert anzeigen die folgenden Anweisungen

1. Klicken Sie in der Auswahl Plan Benutzeroberfläche auf **Neu erstellen** , und geben Sie einen Namen für den Plan aus, wie gewohnt außerhalb einer ASE verwenden.

2. Wählen Sie die ASE, die Sie von Ihrem Speicherort Datumsauswahl verwenden möchten.

    Da eine App-Service-Umgebung im Wesentlichen eine private Bereitstellung handelt, der unter Standort angezeigt wird. 

    ![][2]

    Nach der Auswahl eines ASE im Auswahltool für Speicherort aktualisiert die Erstellung der App-Service-Plan Benutzeroberfläche an.  Die Position zeigt jetzt dem Namen des Systems ASE und der Region steckt im und die Preisgestaltung Plan Datumsauswahl wird mit einer Worker Ressourcenpool Datumsauswahl ersetzt.  

    ![][3]

### <a name="selecting-a-worker-pool"></a>Auswählen einer Worker Ressourcenpool

Normalerweise gibt in Azure-App-Dienst und außerhalb einer App-Service-Umgebung, es 3 Größen berechnen, die mit der Auswahl eines Plans dedizierten Kurs verfügbar sind.  In ähnlicher Weise für eine ASE können bis zu 3 Speicherpools Worker definieren und den Schriftgrad berechnen, die für diesen Pool Worker verwendet wird.  Was dies bedeutet für Mandanten, der die ASE wird, stattdessen einen Preisgestaltung Plan mit berechnen Größe für Ihre App-Serviceplan auswählen, die Sie auswählen, was ein *Worker Ressourcenpool*aufgerufen wird.  

Die Auswahl der Worker Ressourcenpool zeigt Benutzeroberfläche berechnen Größe für diesen Worker Pool unter dem Namen verwendet.  Die verfügbare Menge bezieht sich auf wie vielen zu berechnen, dass Instanzen zur Verwendung in diesem Pool verfügbar sind.  Der gesamte Pool möglicherweise mehrere Instanzen als diese Anzahl, aber dieser Wert bezieht sich auf einfach wie viele nicht verwendet werden.  Wenn Sie Ihre App-Service-Umgebung zum Hinzufügen von Informationen zu berechnen, dass [Ihre App-Service-Umgebung konfigurieren](app-service-web-configure-an-app-service-environment.md)Ressourcen finden Sie unter anpassen müssen.

![][4]

In diesem Beispiel wird nur zwei Worker Pools verfügbar. Hierfür der Administrator ASE Hosts nur in diesen zwei Arbeits-Pools zugeordnet ist.  Die dritte würde Vorliegens virtuellen Computern reserviert darin angezeigt.  

## <a name="after-web-app-creation"></a>Nach der Erstellung der Web-app

Es gibt ein paar Überlegungen zum Ausführen von Web apps sowie zum Verwalten von App-Service-Pläne in einem ASE, die berücksichtigt werden müssen.  

Der Besitzer der ASE wie bereits zuvor erwähnt, die Größe des Systems verantwortlich ist und daher stehen auch dafür sorgen, dass es ausreichender Kapazität für die gewünschte App Dienstpläne gehostet wird. Falls keine Kollegen verfügbaren sind, werden Sie nicht zum Erstellen der App-Serviceplan sein.  Dies ist auch True, wenn die Skalierung der Web-App.  Wenn Sie weitere Instanzen benötigen, müssen Sie Ihre App-Service-Umgebung Administrator Weitere Kollegen hinzufügen zu erhalten.

Nach dem Erstellen der Web app und die App-Serviceplan ist es eine gute Idee, es nach oben skalieren.  In einer ASE müssen immer mindestens 2 Instanzen von der App-Serviceplan für Fehlertoleranz für Ihre apps sein.  Eine App Serviceplan in einer ASE Skalierung entspricht dem normalen, bis der App-Serviceplan Benutzeroberfläche.  Weitere Informationen zu skalieren, [wie Sie eine Web-app in einer App-Service-Umgebung skalieren](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: http://azure.microsoft.com/documentation/articles/resource-group-portal/
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
