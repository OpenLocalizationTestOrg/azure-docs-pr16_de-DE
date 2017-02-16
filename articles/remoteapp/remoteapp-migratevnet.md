<properties
    pageTitle="Zum Migrieren von einem RemoteApp VNET zu einer Azure VNET | Microsoft Azure"
    description="Informationen Sie zum Migrieren von einem RemoteApp VNET zu einer Azure VNET"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="how-to-migrate-a-hybrid-collection-from-a-remoteapp-vnet-to-an-azure-vnet"></a>Zum Migrieren von einer Websitesammlungs Hybrid aus einem RemoteApp VNET zu einer Azure VNET

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Herzlichen Glückwunsch! Wir haben Sie bereitzustellende Hybrid RemoteApp Websitesammlungen direkt in Ihrer vorhandenen Azure virtuelle Netzwerke (VNETs) statt erstellen RemoteApp-spezifische VNETs aktiviert. Auf diese Weise können Sie nutzen Sie die neuesten VNET Funktionen (wie ExpressRoute), und geben Sie Ihre Sammlungen Hybrid direkten Netzwerkzugriff zu anderen Azure-Diensten und virtuellen Computern, die auf die VNET bereitgestellt.  (Dadurch wird eine bessere Leistung und einfacher Setup als VNET-VNET-Konfigurationen).


Einmal angenommen, dass Sie bereits ein Hybrid RemoteApp Auflistung mit dem Namen *OriginalCollection* mit ein RemoteApp VNET namens *RemoteAppVNET*erstellt haben. Hier sind die Schritte zum Migrieren sie zu einer neuen Azure VNET *AzureVNET*bezeichnet.

1.  Erstellen Sie auf der Registerkarte **Netzwerke** im [Verwaltungsportal](http://manage.windowsazure.com/)eines VNET *AzureVNET*, mit der gleichen Speicherort, DNS-Konfiguration und Adresse Leerzeichen (für mindestens eine der *AzureVNET* Subnetze) wie für *RemoteAppVNET*bezeichnet.
2.  Konfigurieren von *AzureVNET* entweder Host oder Netzwerkkonnektivität und Active Directory-Bereitstellung, die *OriginalCollection* Domäne zu verbunden ist.
3.  Klicken Sie auf der Registerkarte **RemoteApps** erstellen Sie eine neue RemoteApp Websitesammlung *Neue Websitesammlung*bezeichnet. (Verwenden Sie die Option **Erstellen mit VNET** aus, die keine **Symbolleiste erstellen**ein.)
3.  Konfigurieren von *NewCollection* mit einem Subnetz in *AzureVNET*bereitgestellt werden.
4.  Konfigurieren von *NewCollection* , um die dasselbe Bild und Verknüpfungsinformationen Domäne wie für *OriginalCollection*zu verwenden.
5.  *NewCollection* wird nach ein paar Stunden in Ihrer Websitesammlung mit einem aktiven Zustand angezeigt.

Jetzt, wenn Sie keine Benutzerinformationen aus der ursprünglichen Auflistung der neuen Auflistung migrieren müssen, führen Sie diese Schritte weiter:

6.  Löschen Sie *OriginalCollection*.
7.  Löschen Sie *RemoteAppVNET*.

Und schon!

Auch wenn Sie Benutzerinformationen aus der ursprünglichen Auflistung der neuen Auflistung migrieren müssen, führen Sie diese Schritte weiter:

6.  Senden eine e-Mail an [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) mit Ihrer Azure-Abonnement-ID, den Namen der ursprünglichen Auflistung und den Namen der neuen Auflistung, und bitten Sie ihn, um Ihre Benutzerinformationen zu migrieren.
7.  Innerhalb von 2 Arbeitstagen wird das Team RemoteApp Benutzerzugriffsliste und alle Benutzerdokumente und benutzereinstellungen verschieben aus der ursprünglichen Sammlung in die neue Auflistung.
8.  Löschen Sie *OriginalCollection*.
9.  Löschen Sie *RemoteAppVNET*.

Und jetzt war's schon!

Wenn Sie Fragen haben oder spezielle Hilfe benötigen, e-Mail- [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).
