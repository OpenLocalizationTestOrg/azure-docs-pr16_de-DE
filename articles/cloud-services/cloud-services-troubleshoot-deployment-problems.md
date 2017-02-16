<properties
 pageTitle="Behandeln von Problemen mit der Cloud Bereitstellung Dienstprobleme | Microsoft Azure"
 description="Es gibt einige häufig auftretende Probleme, die, denen Sie auftreten können, wenn Sie einen Clouddienst für Azure bereitstellen. Dieser Artikel enthält Lösungen für einige Felder."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-deployment-problems"></a>Behandeln von Problemen mit der Bereitstellungsprobleme mit Cloud-Dienst

Wenn Sie ein Cloud-Service-Anwendungspaket Azure bereitstellen, erhalten Sie Informationen zur Bereitstellung von im Bereich **Eigenschaften** im Azure-Portal. Sie können die Details in diesem Bereich verwenden, um Probleme mit der Cloud-Dienst bei der Problembehandlung helfen, und Sie können diese Informationen zur Unterstützung von Azure bereitstellen, wenn Sie eine neue Supportanfrage öffnen.

Finden Sie im Bereich **Eigenschaften** wie folgt:

* Klicken Sie auf die Bereitstellung von Ihrem Cloud-Dienst im Portal Azure, klicken Sie auf **Alle Einstellungen**, und klicken Sie dann auf **Eigenschaften**.
* Im Portal Azure klassischen klicken Sie auf die Bereitstellung von Ihrem Cloud-Dienst, klicken Sie auf **DASHBOARD**, befindet sich in der unteren rechten Ecke der Seite (unter **den ersten Blick**). Achten Sie darauf, dass keine Bezeichnung "Eigenschaften" in diesem Bereich vorhanden ist.

> [AZURE.NOTE] Sie können den Inhalt **im Eigenschaftenbereich** in die Zwischenablage kopieren, indem Sie auf das Symbol in der oberen rechten Ecke des Bereichs.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Problem: Ich kann nicht auf Meine Website zugreifen, aber meine Bereitstellung wird gestartet, und alle Rolleninstanzen bereit sind

Der Website-URL-Link im Portal angezeigt enthält nicht den Port. Der Standardport für Websites ist 80. Wenn eine Anwendung in einen anderen Anschluss ausführen so konfiguriert ist, müssen Sie die richtige Port-Nummer zur URL hinzufügen, wenn die Website zugreifen.

1. Klicken Sie auf die Bereitstellung von Ihrem Cloud-Dienst, der Azure-Portal.
2. Aktivieren Sie im Bereich **Eigenschaften** des Portals Azure die Ports für die Rolleninstanzen (unter **Eingabe Endpunkte**) ein.
3. Wenn der Port nicht 80 ist, fügen Sie den richtigen Anschlusswert zur URL beim Zugriff auf der Anwendungs. Geben Sie die URL, gefolgt von einem Doppelpunkt (:)), gefolgt von der Portnummer, ohne Leerzeichen, um einen Standardport anzugeben.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Problem: Meine Rolleninstanzen ohne mich jegliche wiederverwendet

Reparatur Dienst geschieht automatisch auf, wenn Azure Problem Knoten ermittelt und verschiebt Rolleninstanzen daher an neue Knoten. In diesem Fall möglicherweise Ihre Rolleninstanzen Wiederverwendung automatisch angezeigt. So können Sie herausfinden, wenn Dienst Reparatur aufgetreten ist:

1. Klicken Sie auf die Bereitstellung von Ihrem Cloud-Dienst, der Azure-Portal.
2. Im Bereich **Eigenschaften** des Portals Azure überprüfen Sie die Informationen und feststellen Sie, ob Service Reparatur der Zeit stattgefunden hat, die Sie die Rollen Wiederverwendung beobachtet.

Rollen werden auch ungefähr einmal pro Monat während Host-OS und Updates Gast-BS Papierkorb.  
Weitere Informationen finden Sie im Blogbeitrag [Rolle Instanz Neustart fällig auf OS-Upgrades](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Problem: Ich kann VIP vertauscht und erhalten eine Fehlermeldung

Eine VIP austauschen ist nicht zulässig, wenn ein Bereitstellungsupdate ausgeführt wird. Von Bereitstellungsupdates automatisch ausgeführt werden sollen wenn:

* Neue Gast-Betriebssystem verfügbar ist, und Sie für automatische Updates konfiguriert sind.
* Reaktionsgruppendienst Reparatur erfolgt.

Um herauszufinden, ob Sie automatische hindert Aktualisierung Sie eine VIP austauschen ausführen:

1. Klicken Sie auf die Bereitstellung von Ihrem Cloud-Dienst, der Azure-Portal.
2. Prüfen Sie den Wert des **Status**des Portals Azure im Bereich **Eigenschaften** aus. Wenn es **bereit**ist, überprüfen Sie, dass die **letzten Vorgang** , um festzustellen, ob eine zuletzt, die Vorkommnissen verhindern, dass die VIP austauschen möglicherweise.
3. Wiederholen Sie die Schritte 1 und 2 für die Bereitstellung der Herstellung.
4. Ist eine automatische Aktualisierung in Bearbeitung, warten Sie auf Fertig stellen, bevor Sie versuchen, führen Sie die VIP austauschen.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Problem: Eine Instanz der Rolle ist zwischen Schritte, Initialisierung, gebucht und weiterspielen Endloswiedergabe.

Diese Bedingung könnte ein Problem mit Ihrer Anwendung Code, Paket oder Konfiguration Datei anzugeben. In diesem Fall sollten Sie den Status ändern alle paar Minuten sehen und Azure-Portal möglicherweise etwas wie **Wiederverwendung**"," **gebucht**"oder" **Initialisierung**angenommen. Dies zeigt an, dass es ist etwas mit der Anwendung, die die Instanz der Rolle Ausführung planmäßigen ist falsch.

Weitere Informationen zum Beheben dieses Problems finden Sie im Blogbeitrag [Azure PaaS berechnen Diagnosedaten](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) und [häufige Probleme, die dazu führen, dass die Rollen in den Papierkorb](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Problem: Meine Anwendung funktioniert nicht

1. Klicken Sie im Azure-Portal auf die Instanz der Rolle.
2. Beachten Sie im Bereich **Eigenschaften** des Portals Azure Lösung des Problems Folgendes ein:
   * Wenn die Rolleninstanz zuletzt beendet hat (Sie können den Wert der überprüfen **Abbrechen Count**), die Bereitstellung aktualisiert werden konnte. Warten Sie, um festzustellen, ob die Instanz der Rolle von Lebensläufen eigenständig funktionieren.
   * Wenn die Instanz der Rolle **gebucht**ist, überprüfen Sie Ihre Anwendungscode, um festzustellen, ob das Ereignis [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) behandelt wird. Möglicherweise müssen Sie hinzufügen oder korrigieren einige Code, der das Ereignis behandelt.
   * Die Daten Diagnose und zur Problembehandlung Szenarien in den Blogbeitrag [Azure PaaS berechnen Diagnosedaten](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)durchgehen.

>[AZURE.WARNING] Wenn Sie Ihre Cloud-Dienst wiederverwenden, Zurücksetzen Sie die Eigenschaften für die Bereitstellung, löschen die Informationen für das ursprüngliche Problem effektiv.

## <a name="next-steps"></a>Nächste Schritte

Weitere [Artikel zur Fehlerbehebung](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) für Cloud Services anzeigen

Behebung von Cloud-Dienst Rolle mithilfe von Azure PaaS Computer Diagnosedaten finden Sie unter [Kevin Williamsons Blog Reihe](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
