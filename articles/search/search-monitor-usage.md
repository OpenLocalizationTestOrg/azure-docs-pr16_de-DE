<properties 
   pageTitle="Überwachen der Verwendung und Statistiken in einem Azure-Suchdienst | Microsoft Azure | Cloud gehosteten Suchdienst" 
   description="Nachverfolgen von Ressourcen Verbrauch und Index Größe für einen gehosteten Cloud Suchdienst auf Microsoft Azure Azure Suche." 
   services="search" 
   documentationCenter="" 
   authors="HeidiSteen" 
   manager="jhubbard" 
   editor=""
   tags="azure-portal"/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required" 
   ms.date="05/17/2016"
   ms.author="heidist"/>

# <a name="monitor-usage-and-statistics-in-an-azure-search-service"></a>Überwachen der Verwendung und Statistiken in einem Azure-Suchdienst

Nachverfolgen von den Wertzuwachs Indizes und Dokumentgröße hilft Ihnen anpassen die vorausschauende Kapazität, drücken Sie anschließend die Obergrenze, die Sie für Ihren Dienst hergestellt haben. 

Zum Überwachen der Ressource: Einsatz hinzu, zählt und Statistiken des Diensts problemlos angezeigt werden im [Portal Azure](https://portal.azure.com), aber auch erhalten Sie die Informationen programmgesteuert Wenn Sie einen benutzerdefinierten Dienst Verwaltungstool erstellen. In diesem Artikel werden die Schritte für beide Verfahren behandelt.

Sie können auch das neue Suche Datenverkehr Analytics-Feature für Einblicke mit Aktivität Ebene der Index verwenden. Besuchen Sie die [Suche Datenverkehr Analytics für Suchmaschinen Azure](search-traffic-analytics.md) Schritte.

##<a name="view-counts-and-metrics-in-the-portal"></a>Zählt und Kriterien im Portal anzeigen 

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com). 

2. Öffnen Sie das Dienst Dashboard Ihrer Azure Suchdiensts. Kacheln für den Dienst finden Sie auf der Homepage, oder Sie können navigieren Sie zu dem Dienst aus auf die JumpBar durchsuchen. Eine schrittweise Anleitung finden Sie unter [Erstellen einer Dienst](search-create-service-portal.md) .

Im Abschnitt Verwendung enthält, eine Anzeige, die Sie darüber informiert werden zurzeit welcher Teil der verfügbaren Ressourcen verwendet wird.

  ![][1]

Denken Sie daran, dass der gemeinsamen Dienste maximal eine Replikation ist und jeder partitionieren. Darüber hinaus 10.000 Dokumente können nur in total oder 50 MB Daten unterstützt, je nachdem, was nicht erfolgt ist.

##<a name="get-index-statistics-using-the-rest-api"></a>Verwenden die REST-API Indexstatistik abrufen

Bieten sowohl die REST-API von Azure suchen und .NET SDK programmgesteuerten Zugriff auf Kriterien Service.  Wenn Sie einen Index aus Azure SQL-Datenbank oder DocumentDB, eine weitere API laden [Indexer](https://msdn.microsoft.com/library/azure/dn946891.aspx) verwenden kann die Zahlen zu erhalten, die Sie benötigen. 

  + [Indexstatistik abrufen](https://msdn.microsoft.com/library/azure/dn798942.aspx)
  + [Anzahl von Dokumenten](https://msdn.microsoft.com/library/azure/dn798924.aspx)
  + [Abrufen von Indizierungsstatus](https://msdn.microsoft.com/library/azure/dn946884.aspx)

## <a name="next-steps"></a>Nächste Schritte

Prüfen Sie [Grenzwerte und Kapazität](search-limits-quotas-capacity.md) Ermittlung die Kombination von Partitionen und der benötigten Wenn vorhandenen Kapazität ausreicht. 

Weitere Informationen zum Verwalten von Diensten finden Sie unter [Verwalten der Suchdienst auf Microsoft Azure](search-manage.md) .

<!--Image references-->
[1]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG




 
