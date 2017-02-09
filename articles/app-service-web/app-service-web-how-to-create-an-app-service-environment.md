<properties 
    pageTitle="So erstellen Sie eine App-Service-Umgebung" 
    description="Erstellung Fluss Beschreibung für die app-Service-Umgebungen" 
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
    ms.date="09/22/2016" 
    ms.author="ccompy"/>

# <a name="how-to-create-an-app-service-environment"></a>So erstellen Sie eine App-Service-Umgebung #

### <a name="overview"></a>(Übersicht) ###

App-Service-Umgebungen (ASE) sind eine Möglichkeit, Premium Service Azure-App-Dienst, der eine erweiterte Konfiguration Videofunktionen übermittelt, die in der Stempel mit mehreren Mandanten ist nicht verfügbar.  Das Feature ASE bereitstellt im Wesentlichen die App-Verwaltungsdienst Azure in virtuelle Netzwerk des Kunden.  Um ein besseres Verständnis der Funktionen App Service-Umgebungen, lesen Sie die [Was ist eine App-Service-Umgebung] zu erhalten[ WhatisASE] Dokumentation.

### <a name="before-you-create-your-ase"></a>Bevor Sie Ihre ASE erstellen ###

Es ist wichtig, für die Aspekte bewusst sein, die Sie nicht ändern können.  Sind diese Aspekte, die Sie zu Ihrer ASE ändern können, nachdem sie erstellt wurde:

- Speicherort
- Abonnement
- Ressourcengruppe
- VNet verwendet
- Subnetz verwendet 
- Subnetz Größe

Wenn Sie eine VNet auswählen und Angeben von einem Subnetz, stellen Sie sicher ist es für alle zukünftiges Wachstum ausreicht.  

### <a name="creating-an-app-service-environment"></a>Erstellen einer App-Service-Umgebung ###

Es gibt zwei Methoden zum Zugreifen auf der Erstellung ASE Benutzeroberfläche aus.  Sind möglich durch Suchen in der Azure Marketplace für ***App-Service-Umgebung*** gefunden oder neu geschieht -> Web + Mobile-App-Service-Umgebung >.  So erstellen Sie eine ASE

1. Geben Sie den Namen Ihrer ASE.  Der Name, der für die ASE angegeben ist werden von den apps, die in der ASE erstellt verwendet werden.  Ist die ASE Namen Appsvcenvdemo wäre der Unterdomänennamen. *appsvcenvdemo.p.azurewebsites.net*.  Wenn Sie also eine *Mytestapp* -app erstellt würden dann sie am *mytestapp.appsvcenvdemo.p.azurewebsites.net*adressiert werden.  Sie können nicht den Namen Ihrer ASE Leerraum verwenden.  Wenn Sie Großbuchstaben Zeichen im Namen verwenden, werden der Domänenname die gesamte Kleinbuchstaben Version des Namens.  Wenn Sie eine ILB verwenden klicken Sie dann Ihren Namen ASE wird nicht in Ihre Subdomain verwendet jedoch wird stattdessen während der Erstellung ASE ausdrücklich angegeben

    ![][1]

2. Wählen Sie Ihr Abonnement.  Das Abonnement für Ihre ASE verwendet wird auch eine, die alle apps in dieser ASE mit erstellt werden.  Sie können keine Ihrer ASE einer VNet versehen, der in ein anderes Abonnement

3. Wählen Sie aus, oder geben Sie eine neue Ressourcengruppe.  Die Ressourcengruppe für Ihre ASE verwendet muss identisch sein, die für Ihre VNet verwendet wird.  Wenn Sie eine bereits vorhandene VNet auswählen, wird Ihre VNet, damit entspricht die Ressource Gruppenauswahl für Ihre ASE aktualisiert.

    ![][2]

4. Treffen Sie Ihre Auswahl virtuelles Netzwerk und einen Speicherort ein.  Sie können zum Erstellen einer neuen VNet, oder wählen eine bereits vorhandene VNet auswählen.  Wenn Sie eine neue VNet auswählen können Sie einen Namen und einen Speicherort angeben. Die neue VNet haben die Adresse Bereich 192.268.250.0/23 und einem Subnetz namens **Standard** , die als 192.168.250.0/24 definiert ist.  Sie können auch einfach einen bereits vorhandenen Classic oder Ressource-Manager VNet auswählen.  Die Auswahl des VIP bestimmt, wenn Ihre ASE direkt aus dem Internet (extern) zugegriffen werden kann, oder wenn es sich um eine interne laden Lastenausgleich (ILB) verwendet.  Lernen sie mehr über lesen Sie [verwenden eine interne Lastenausgleich mit einer App-Service-Umgebung][ILBASE].  Wenn Sie eine externe VIP auswählen können Sie mit wie vielen externe IP-Adressen auswählen das System mit Zwecken IPSSL erstellt wird.  Wenn Sie die Internal auswählen, müssen Sie die Unterdomäne angeben, die Ihre ASE verwendet werden.  ASEs kann in virtuelle Netzwerke bereitgestellt werden, die *beiden* öffentlichen Adressbereiche *oder* RFC1918 Adresse Leerzeichen (d. h. verwenden Private Adressen).  Um ein virtuelles Netzwerk mit einem Bereich Öffentliche Adresse verwenden zu können, müssen Sie die VNet im Voraus erstellen.  Wenn Sie eine bereits vorhandene VNet auswählen, müssen Sie ein neues Subnetz beim Erstellen der ASE zu erstellen.  **Sie können keiner zuvor erstellten Subnetz im Portal verwenden.  Sie können eine ASE mit einem bereits vorhandenen Subnetz erstellen, wenn Sie Ihre ASE mithilfe einer Ressource-Manager Vorlage erstellen.**  So erstellen Sie eine ASE aus eine Vorlage verwenden die Informationen, [erstellen eine App-Service-Umgebung aus Vorlage] [ ILBAseTemplate] und hier [Erstellen Sie eine ILB App-Service-Umgebung aus Vorlage][ASEfromTemplate].

### <a name="details"></a>Details ###

Eine ASE wird mit 2 Front endet und 2 Kollegen erstellt.  Der Front endet dienen als die Endpunkte HTTP-/HTTPS, und senden Sie den Datenverkehr für die Worker, denen die Rollen handelt, die Ihrer apps zu hosten.   Sie können passen Sie die Menge nach der Erstellung ASE und können sogar einrichten automatisch skalieren Regeln auf diese Ressourcenpools.  Weitere Informationen hierzu um manuelle Skalierung Management und Überwachung einer App-Service-Umgebung finden Sie hier: [eine App-Service-Umgebung konfigurieren][ASEConfig] 

Nur die eine ASE kann im von der ASE verwendeten Subnetz vorhanden sein.  Das Subnetz kann nicht für einen anderen Wert als den ASE verwendet werden.

### <a name="after-app-service-environment-creation"></a>Nach der Erstellung der App-Service-Umgebung ###

Sie können nach der Erstellung ASE anpassen:

- Menge der Front endet (minimale: 2)
- Menge von Arbeitskräften (minimale: 2)
- Menge von IP-Adressen für IP SSL verfügbar
- Berechnen von Ressourcen Größen von vorne endet oder Kollegen verwendet (Front-End-Mindestgröße ist P2)


Es werden weitere Details, um die manuelle Skalierung, Management und Überwachung der App-Service-Umgebungen hier: [eine App-Service-Umgebung konfigurieren][ASEConfig] 

Informationen zu automatische Skalierung eine Führungslinie hier vorhanden ist: [automatisch skalieren für eine App-Service-Umgebung konfigurieren][ASEAutoscale]

Es gibt zusätzliche Abhängigkeiten, die nicht für die Anpassung, wie etwa die Datenbanken und Speicher verfügbar sind.  Dies sind die von Azure behandelt und im Zusammenhang mit dem System.  Der Systemspeicher unterstützt bis zu 500 GB für die gesamte App-Service-Umgebung und die Datenbank durch Azure angepasst wird, durch die Skalierung des Systems nach Bedarf.


## <a name="getting-started"></a>Erste Schritte
Alle Artikel und wie-des für App-Service-Umgebungen in der [Infodatei für Service-Umgebungen Anwendung](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Um mit der App-Service-Umgebungen anzufangen, finden Sie unter [Einführung App-Service-Umgebungen][WhatisASE]

Weitere Informationen über die App-Verwaltungsdienst Azure-Plattform finden Sie unter [Azure-App-Verwaltungsdienst][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/
