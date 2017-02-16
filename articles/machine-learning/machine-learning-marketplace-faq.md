<properties 
    pageTitle="Häufig gestellte Fragen: Veröffentlichen und Verwenden von maschinellen Learning apps in Azure Marketplace | Microsoft Azure" 
    description="Häufig gestellte Fragen" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bharaths" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/11/2016" 
    ms.author="bharaths"/> 

#<a name="publishing-and-using-machine-learning-apps-in-the-azure-marketplace-faq"></a>Veröffentlichen und Verwenden von Computer-Learning-apps in dem Azure Marketplace: häufig gestellte Fragen

##<a name="questions-about-consuming-from-marketplace"></a>Fragen zu Verarbeitung von Marketplace


**1. Warum erhalte ich die folgende Fehlermeldung angezeigt, nachdem ich für den Webdienst Eingaben vornehmen:**

**Die Anforderung, die in einer Back-End-Timeout oder Back-End-Fehler geführt haben. Das Team untersucht das Problem. Es tut uns leid, die zu entschuldigen. (500)**

Der Eingabeparameter möglicherweise nicht die erforderlichen Format für den bestimmten Webdienst entsprechen. Lizenzinformationen finden Sie die entsprechende Dokumentation Verknüpfung zum Suchen nach das richtige Format für den Eingabeparameter und die Einschränkungen für diesen Webdienst.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**2. wenn ich den API Link für den Webdienst zu, die ich auf der Seite "Durchsuchen dieses Dataset" und einfügen, die es in einem anderen Browserfenster kopieren sehe, welche Anmeldeinformationen sollte verwenden ich, um die Ergebnisse zuzugreifen, und wie sehe ich sie?**

Sie sollten Ihr Konto Marketplace als den Benutzernamen und die primäre kontoschlüssel als Kennwort verwenden. Die primäre kontoschlüssel finden Sie auf der Seite **Durchsuchen dieses Dataset** unter der Beschreibung des Webdiensts (klicken Sie auf die Schaltfläche " **Anzeigen** "). Das Ergebnis möglicherweise im Browser angezeigt wird, oder es möglicherweise zum Herunterladen zur Verfügung, je nachdem, welcher Browser verwenden Sie.

**3. Warum erhalte ich die folgende Fehlermeldung angezeigt, nachdem ich die Eingabe für den Webdienst auf der Seite "Durchsuchen dieses Dataset" eingegeben haben:** 

**Während der Bearbeitung Ihrer Anfrage ist ein unerwarteter Fehler aufgetreten. Versuchen Sie es erneut.**

Eine oder mehrere Eingabeparameter des Web Service möglicherweise die maximale Länge überschritten haben, wenn den Webdienst auf der Marketplace-Seite **Durchsuchen dieses Dataset** Verarbeitung. Die Dienste können mit einem mehr Länge Eingabe mithilfe von HTTP POST Methoden aufgerufen werden. Beispiele finden Sie unter [Beispiel Lösungen R auf Computer Learning verwenden und auf Marketplace veröffentlicht](machine-learning-r-csharp-web-service-examples.md).

**4 Warum werden nicht in der "EXPLORER-API" Registerkarte Int im Store im klassischen Azure-Portal nichts angezeigt?** 

Dies ist ein bekanntes Problem mit dem Azure klassischen Portal Marketplace. Das Team ist arbeiten, um dieses Problem zu beheben. 


##<a name="questions-about-publishing-from-azure-machine-learning-on-marketplace"></a>Fragen zu für die Veröffentlichung von Azure Computer Learning auf Marketplace

**1. Warum sind meine Transaktionen Logos oder die Bilder nicht für mein Webdienst auffrischen?** 

Logos und Bilder in der Veröffentlichungsportal zwischengespeichert werden, und es kann bis zu 10 Tagen für den neuen Logo oder das Bild auf dem Portal aktualisieren dauern.

**2 Warum ist meine Webdienst Marketplace eine Fehlermeldung mit die Registerkarte "Details"?**

Es ist ein bekanntes Problem der Marketplace beim Herstellen einer Verbindung zu Azure Computer lernen für Dienstleistung. Das Team ist arbeiten, um dieses Problem zu beheben.

**3. Warum funktioniert der R Beispiel-Code in den Azure maschinellen Learning-Webdiensten nicht für die Webdienste auf Marketplace?**

Die Authentifizierungssysteme unterscheiden sich bei der Verbindung mit Azure Computer Learning-Webdiensten direkt zum Herstellen einer Verbindung mit diese Webdienste über die Marketplace verglichen. Die Dienste Marketplace OData-Dienste sind, und sie können mit GET oder POST-Methoden aufgerufen werden können. 

**4. warum Support-Links von meinem Webdienst sind bietet nicht ordnungsgemäß für einige der Meine Angebote aktualisieren?**

Support-Links, sind global pro Publisher, nicht für einzelne Angebot. 

**5. wie Veröffentlichen ich einen Webdienst mit Stapel Eingabemodus auf Marketplace?**

Der Stapel Eingabemodus wird in Marketplace-Webdiensten derzeit nicht unterstützt.

**6 an wen sollte ich mich wenden, um Hilfe zu erhalten, wenn ich Fragen darüber, wie Sie eine Publisher Daten haben, oder ich während der Veröffentlichung Problem vorliegt?**

Wenden Sie sich an das Team Azure Marketplace am <datamarketbd@microsoft.com> Weitere Informationen.





 
