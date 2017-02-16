Weiter, wenn alle Server auf dem Cluster Windows Server 2008 R2 oder Windows Server 2012 ausgeführt werden, müssen Sie sicherstellen, dass das Update [KB2854082](http://support.microsoft.com/kb/2854082) installiert ist, auf jeder der lokalen Server oder Azure-virtuellen Computern, die den Cluster gehören. Alle Server oder virtuellen Computer, der im Cluster, aber nicht in der Gruppe Verfügbarkeit sollten auch dieses Update installiert haben.

Laden Sie in der remote desktop-Sitzung für jeden einzelnen Clusterknoten [KB2854082](http://support.microsoft.com/kb/2854082) in einem lokalen Verzeichnis. Klicken Sie dann installieren Sie das Update nacheinander auf jeden einzelnen Clusterknoten ein. Wenn der Cluster-Dienst auf dem Clusterknoten aktuell ausgeführt wird, ist der Server am Ende der Update-Installation neu gestartet.

>[AZURE.WARNING] Den Cluster-Dienst beenden oder Neustarten des Servers wirkt sich auf den Quorum Integritätsstatus Ihrer Cluster und der Gruppe Verfügbarkeit und kann dazu führen, dass Ihre Cluster offline gehen. Wenn während der Installation die hohe Verfügbarkeit von Ihren Cluster beibehalten möchten, Folgendes sicher:
>
> - Der Cluster wird in optimale Quorum Gesundheit, 
> - Alle Clusterknoten online sind, bevor Sie das Update auf einem beliebigen Knoten installieren und
> - Zulassen der Update-Installations vollständig auf einem Knoten, einschließlich vollständig einen Neustart des Servers, bevor Sie das Update auf einem anderen Knoten im Cluster ausgeführt.
