<properties
pageTitle="So aktualisieren Sie einen Clouddienst | Microsoft Azure"
description="Erfahren Sie, wie in Azure-Cloud-Dienste zu aktualisieren. Erfahren Sie, wie ein Update auf einen Clouddienst fortgesetzt wird, um die Verfügbarkeit sicherzustellen."
services="cloud-services"
documentationCenter=""
authors="Thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/10/2016"
ms.author="adegeo"/>

# <a name="how-to-update-a-cloud-service"></a>So aktualisieren Sie einen Clouddienst

## <a name="overview"></a>(Übersicht)
Am Fuß 10.000 ist Aktualisieren eines Cloud-Diensts, einschließlich der Rollen und den Gast OS, drei Schritten aus. Zunächst müssen der Binärdateien und von Konfigurationsdateien für die neue Cloud-Dienst oder Version des Betriebssystems hochgeladen werden. Als Nächstes reserviert Azure berechnen und Netzwerk-Ressourcen für den Clouddienst basierend auf den Anforderungen der die neue Version des Cloud-Dienst an. Schließlich führt Azure ein paralleles Update um inkrementell den Mandanten gleichzeitiger Ihrer Verfügbarkeit auf die neue Version oder Gast-Betriebssystem zu aktualisieren. Dieser Artikel beschreibt die Details der letzte Schritt – die Aktualisierung im Wechsel.

## <a name="update-an-azure-service"></a>Aktualisieren eines Azure-Diensts

Azure organisiert Ihre Rolleninstanzen in logischen Gruppen Upgrade Domänen (UD) bezeichnet. Upgrade Domänen (UD) sind logische Gruppen von Rolleninstanzen, die als Gruppe aktualisiert werden.  Azure Updates einen Cloud-service eine UD gleichzeitig anzeigt, wodurch Instanzen in anderen UDs fortsetzen Datenverkehr erstellen.

Die Standardanzahl von Upgrade Domänen ist 5. Sie können unterschiedlich viele Upgrade Domänen angeben, indem Sie das Attribut UpgradeDomainCount in des Diensts Formulardefinitionsdatei (.csdef). Weitere Informationen über das Attribut UpgradeDomainCount finden Sie unter [WebRole Schema](https://msdn.microsoft.com/library/azure/gg557553.aspx) oder [WorkerRole Schema](https://msdn.microsoft.com/library/azure/gg557552.aspx).

Wenn Sie eine in-situ-Aktualisierung von einer oder mehreren Rollen in Ihrem Dienst durchführen, aktualisiert Azure Sätze von Rolleninstanzen nach dem Upgrade Domäne, die sie angehören. Azure Updates, die alle Instanzen in einer bestimmten Upgrade Domäne – beenden können, aktualisieren können, schalten sie online – wieder klicken Sie dann auf die nächste Domäne verschoben. Durch das Beenden nur der Instanzen, die in der aktuellen Upgrade Domäne ausgeführt, sichergestellt Azure, dass eine Aktualisierung in den Einfluss möglichst zum laufenden Dienst auftritt. Weitere Informationen finden Sie weiter unten in diesem Artikel [wie das Update fortgesetzt wird](#howanupgradeproceeds) .

> [AZURE.NOTE] Während der Ausdrücke **Aktualisieren** und **Durchführen eines Upgrades** weicht Bedeutung im Kontext Azure haben, können sie Synonym für die Prozesse und eine Beschreibung der Features in diesem Dokument verwendet werden.

Definieren Sie des Diensts muss mindestens zwei Instanzen einer Rolle für diese Rolle aktualisiert werden in-situ-ohne Ausfallzeiten. Wenn nur eine Instanz von einer Rolle aus der Dienst besteht, ist der Dienst erst verfügbar, die in-situ-Aktualisierung abgeschlossen ist.

In diesem Thema werden die folgende Informationen zu Updates für Azure behandelt:

-   [Zulässige Dienst Änderungen während einer Aktualisierung](#AllowedChanges)
-   [Wie ein Upgrade fortgesetzt wird](#howanupgradeproceeds)
-   [Zurücksetzen eines Updates](#RollbackofanUpdate)
-   [Mehrere veränderliche Vorgänge in einer laufenden Bereitstellung initiieren](#multiplemutatingoperations)
-   [Verteilung der Rollen in Upgrade Domänen](#distributiondfroles)

<a name="AllowedChanges"></a>
## <a name="allowed-service-changes-during-an-update"></a>Zulässige Dienst Änderungen während einer Aktualisierung
Die folgende Tabelle zeigt die zulässigen Änderungen an einen Dienst während einer Aktualisierung an:

|Änderungen zu hosten, Services und Rollen zulässig|In-situ-update|Gestaffelter (VIP austauschen)|Löschen und erneut bereitstellen|
|---|---|---|---|
|Betriebssystemversion|Ja|Ja|Ja
|.NET Trust Ebene|Ja|Ja|Ja|
|Virtuellen Computern Größe<sup>1</sup>|Ja,<sup>2</sup>|Ja|Ja|
|Lokaler Speicher-Einstellungen|Vergrößern Sie nur<sup>2</sup>|Ja|Ja|
|Hinzufügen oder Entfernen von Rollen in einem Dienst|Ja|Ja|Ja|
|Anzahl der Instanzen von einer bestimmten Rolle|Ja|Ja|Ja|
|Zahl oder einen falschen von Endpunkten für einen Dienst|Ja,<sup>2</sup>|Nein|Ja|
|Namen und Werte der Konfiguration Einstellungen|Ja|Ja|Ja|
|Werte (aber nicht Namen) Konfiguration Einstellungen|Ja|Ja|Ja|
|Hinzufügen von neuen Zertifikaten|Ja|Ja|Ja|
|Ändern Sie die vorhandenen Zertifikate|Ja|Ja|Ja|
|Bereitstellen von neuen code|Ja|Ja|Ja|
<sup>1</sup> Größe ändern, die eine Teilmenge der verfügbaren für den Clouddienst Größen eingeschränkt.

<sup>2</sup> Erfordert Azure SDK 1.5 oder höhere Versionen.

> [AZURE.WARNING] Ändern der Größe des virtuellen Computers, werden lokale Daten gelöscht.


Die folgenden Elemente werden bei einer Aktualisierung nicht unterstützt:

-   Ändern des Namens einer Rolle. Entfernen Sie, und fügen Sie die Rolle mit den neuen Namen ein.
-   Ändern der Anzahl der Domäne aktualisieren.
-   Die Größe der lokalen Ressourcen zu verringern.

Wenn Sie andere Updates mit des Diensts Definition, wie etwa die Größe der lokale Ressource verringern herstellen, müssen Sie stattdessen eine VIP austauschen Update ausführen. Weitere Informationen finden Sie unter [Bereitstellung austauschen](https://msdn.microsoft.com/library/azure/ee460814.aspx).

<a name="howanupgradeproceeds"></a>
## <a name="how-an-upgrade-proceeds"></a>Wie ein Upgrade fortgesetzt wird
Sie können entscheiden, ob alle die Rollen in Ihrem Dienst oder eine einzelne Rolle im Dienst aktualisiert werden sollen. In beiden Fällen werden alle Instanzen der einzelnen Rollen, die gerade aktualisiert wird und in der ersten Upgrade Domäne beendet, aktualisiert und wieder online geschaltet. Sobald sie wieder online sind, sind die Instanzen in der zweiten Upgrade Domäne beendet, aktualisiert und wieder online geschaltet. Cloud-Dienst kann höchstens ein Upgrade jeweils aktiven haben. Das Upgrade wird immer für die neueste Version von Cloud-Dienst ausgeführt.

Das folgende Diagramm veranschaulicht, wie das Upgrade fortgesetzt wird, wenn Sie alle Rollen des Diensts durchführen:

![Dienst aktualisieren] (media/cloud-services-update-azure-service/IC345879.png "Dienst aktualisieren")

Dieses nächste Diagramm veranschaulicht, wie das Update fortgesetzt wird, wenn Sie nur eine einzelne Rolle aktualisieren:

![Rolle aktualisieren] (media/cloud-services-update-azure-service/IC345880.png "Rolle aktualisieren")  

> [AZURE.NOTE] Beim Aktualisieren von eines Diensts aus einer einzelnen Instanz auf mehrere Instanzen wird der Dienst zum Absturz bringen während des Upgrades aufgrund von der Möglichkeit Azure Upgrades Diensten durchgeführt werden. Der Dienst Vereinbarung zum Servicelevel bürgenden dienstverfügbarkeit gilt nur für Dienste, die mit mehr als eine Instanz bereitgestellt werden. Die folgende Liste beschreibt, wie sich die Daten auf jedem Laufwerk jedes Azure Service Upgrade Szenario auswirken:
>
>Virtueller Computer Neustart:
>
-   C: beibehalten
-   D: beibehalten
-   E:) beibehalten
>
>Portal Neustart:
>
-   C: beibehalten
-   D: beibehalten
-   E gelöscht.
>
>Portal neu abbilden:
>
-   C: beibehalten
-   D: gelöscht.
-   E gelöscht.

>In-Place-Aktualisierung:
>
-   C: beibehalten
-   D: beibehalten
-   E gelöscht.
>
>Migration von Knoten:
>
-   C: gelöscht.
-   D: gelöscht.
-   E gelöscht.

>Beachten Sie, dass in der Liste oben das Laufwerk e:) Rolle des Stammverzeichnis darstellt, und nicht hartcodierte sollten. Verwenden Sie stattdessen die Variable RoleRoot-Umgebung, um das Laufwerk darzustellen.

>Um der Ausfall minimieren beim Upgrade einer Instanz Dienst, Bereitstellen Sie einen neuen mit mehreren Instanzdienst auf den Bereitstellungsserver und führen Sie einer VIP austauschen aus.

Während einer automatische Aktualisierung wertet den Azure Fabric Controller regelmäßig die Integrität des Cloud-Dienst ermitteln, zu welchem sicher der nächsten UD durchlaufen werden kann. Diese Auswertung Dienststatus auf Basis pro Rolle ausgeführt wird und betrachtet nur Instanzen in der neuesten Version (d. h. Instanzen von UDs, die bereits durchlaufen wurde haben). Überprüft, ob eine Mindestanzahl der Rolleninstanzen, für die einzelnen Rollen, einen ausreichenden terminal Zustand erreicht haben.

### <a name="role-instance-start-timeout"></a>Timeout für Rolle Instanz starten
Der Fabric Controller wartet 30 Minuten für jede Instanz der Rolle einen Schritte Zustand erreicht. Wenn das Zeitlimit abläuft, weiterhin der Fabric Controller auf die nächste Rolleninstanz durchlaufen.

<a name="RollbackofanUpdate"></a>
## <a name="rollback-of-an-update"></a>Zurücksetzen eines Updates
Azure sorgt für Flexibilität bei der Verwaltung von Diensten während einer Aktualisierung, mit deren Hilfe Sie zusätzliche Vorgänge für einen Dienst einleiten, nachdem die ursprünglichen Update Anforderung vom Azure Fabric Controller akzeptiert wird. Zurücksetzen kann nur ausgeführt werden, wenn eine Aktualisierung (Konfiguration ändern) oder Upgrade wird in den Zustand **in Bearbeitung** für die Bereitstellung. Ein Update oder Upgrade gilt in Bearbeitung werden, wie es ist mindestens eine Instanz des Diensts die noch nicht auf die neue Version aktualisiert wurde. Klicken Sie zum Testen, ob zurücksetzen zulässig ist, überprüfen Sie den Wert der RollbackAllowed Kennzeichnung, zurückgegebene [Bereitstellung abrufen](https://msdn.microsoft.com/library/azure/ee460804.aspx) und [Erste Cloud Service Eigenschaften](https://msdn.microsoft.com/library/azure/ee460806.aspx) Vorgänge, festgelegt ist true.

> [AZURE.NOTE] Dies ist nur sinnvoll ist, rufen Sie an einem **in-situ** -Update zurücksetzen oder aktualisiert werden, da VIP austauschen Upgrades eine vollständige laufenden Instanz von Ihrem Dienst durch ein anderes ersetzt müssen.

Zurücksetzen eines Updates in Bearbeitung weist die folgenden Effekte für die Bereitstellung:

-   Alle Rolleninstanzen war noch nicht aktualisiert oder auf die neue Version aktualisiert werden nicht aktualisiert oder aktualisiert werden, da diese Instanzen bereits die Zielversion des Diensts ausgeführt werden.
-   Alle Rolleninstanzen hatte bereits aktualisiert oder Upgrade auf die neue Version des Service-Pakets (\*.cspkg) Datei oder die Dienstkonfiguration (\*.cscfg) Datei (oder beide Dateien) auf die Version vor dem upgrade dieser Dateien zurückgesetzt werden.

Dies wird durch die folgenden Funktionen funktional bereitgestellt:

-   Der [Zurücksetzen aktualisieren oder Upgrade](https://msdn.microsoft.com/library/azure/hh403977.aspx) -Operation, die auf die Aktualisierung einer Konfiguration (ausgelöst durch einen [Bereitstellung-Konfiguration ändern](https://msdn.microsoft.com/library/azure/ee460809.aspx)) oder ein Upgrade (ausgelöst durch einen [Upgrade Bereitstellung](https://msdn.microsoft.com/library/azure/ee460793.aspx)) aufgerufen werden können, wie es ist mindestens eine Instanz des Diensts die noch nicht auf die neue Version aktualisiert wurde.
-   Das Element gesperrt und die RollbackAllowed Element, das als Teil der Antwort Textkörper der [Bereitstellung abrufen](https://msdn.microsoft.com/library/azure/ee460804.aspx) und [Eigenschaften für Cloud-Dienst erhalten](https://msdn.microsoft.com/library/azure/ee460806.aspx) zurückgegeben werden:
    1.  Das Element gesperrt können Sie erkennen, wenn ein Vorgang veränderliche für eine gegebene Bereitstellung aufgerufen werden kann.
    2.  Das Element RollbackAllowed können Sie erkennen, wenn der Vorgang [Zurücksetzen Update oder ein Upgrade](https://msdn.microsoft.com/library/azure/hh403977.aspx) auf eine gegebene Bereitstellung aufgerufen werden kann.

    Um eine Zurücksetzen ausführen zu können, müssen Sie keinen der gesperrt, und die Elemente RollbackAllowed überprüfen. Es ist es ausreichend, um zu bestätigen, dass RollbackAllowed festgelegt ist true. Diese Elemente werden nur zurückgegeben, wenn diese Methoden aufgerufen werden, indem Sie mit der Anfrage-Header legen Sie auf "X-ms-Version: 2011-10-01" oder höher. Weitere Informationen zu Kopf-Versioning finden Sie unter [Service Management Versioning](https://msdn.microsoft.com/library/azure/gg592580.aspx).

Es gibt Situationen, wobei Zurücksetzen eines Updates oder Aktualisierung wird nicht unterstützt, diese werden wie folgt:

-   Verringerung der lokalen Ressourcen – Wenn die Aktualisierung die lokalen Ressourcen für eine Rolle der Azure-Plattform erhöht lässt sich nicht auf Zurücksetzen. 
-   Speicherkontingent Einschränkungen - haben, wenn die Aktualisierung wurde ein Maßstab unten Vorgang, die Sie nicht mehr möglicherweise Sie ausreichend berechnen Kontingent zum Abschließen des Vorgangs zurücksetzen. Jede Azure Abonnement umfasst ein Kontingent zugeordnet, die von allen gehosteten Dienste verwendet werden kann, die zu diesem Abonnement gehören die maximale Anzahl der Kerne angibt. Wenn die Durchführung eines angegebenen Updates Zurücksetzen Ihres Abonnements über die Kontingent und dann setzen möchten wird Zurücksetzen nicht aktiviert.
-   Racebedingung – Wenn Sie das ursprüngliche Update abgeschlossen wurde, Zurücksetzen ist nicht möglich.

Beispiel wenn das Zurücksetzen einer Aktualisierung hilfreich sein können ist, wenn Sie den Vorgang [Upgrade Bereitstellung](https://msdn.microsoft.com/library/azure/ee460793.aspx) im manuellen Modus zum Steuerelement verwenden, die die Rate, ein Bereiche in-situ-Upgrades in Ihrer Azure Service gehostet, Variationswebsites bereitgestellt wird.

Während der Installation des Upgrades [Upgrade Bereitstellung](https://msdn.microsoft.com/library/azure/ee460793.aspx) im manuellen Modus anrufen und Upgrade Domänen durchlaufen beginnen. Überwachen während des Upgrades Wenn zu einem bestimmten Zeitpunkt, beachten Sie, dass einige Rolleninstanzen in den ersten Upgrade Domänen, die Sie überprüfen reagiert haben, können Sie den Vorgang [Zurücksetzen Update oder ein Upgrade](https://msdn.microsoft.com/library/azure/hh403977.aspx) auf die Bereitstellung, die davon unberührt versetzen die Instanzen die noch nicht aktualisiert haben und Zurücksetzen Instanzen der Bereich der vorherigen Service-Paket und Konfiguration aktualisiert wurde aufrufen.

<a name="multiplemutatingoperations"></a>
## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Mehrere veränderliche Vorgänge in einer laufenden Bereitstellung initiieren
In einigen Fällen möchten Sie möglicherweise mehrere Gleichzeitiges veränderliche Vorgänge in einer laufenden Bereitstellung einleiten. Beispielsweise können Sie ein Update-Dienst ausführen und, während das Update über dem Dienst Variationswebsites wird ist, Sie möchten einige ändern, z. B. um das Update wieder Rollen, ein anderes Update anwenden oder sogar löschen die Bereitstellung. Ein Falls, in dem dies möglicherweise notwendig sein, ist ein dienstupgrade fehlerhafte Code enthält, wodurch eine Instanz der aktualisierten Rolle wiederholt abzustürzen. In diesem Fall wird der Azure Fabric Controller nicht vorzunehmen höchstwahrscheinlich in anwenden, die aktualisiert werden, da keine ausreichende Anzahl der Instanzen in der aktualisierten Domäne fehlerfrei sind. Dieser Status wird als eine *hängen Bereitstellung*bezeichnet. Sie können die Bereitstellung lösen Sie nach der Aktualisierung zurücksetzen oder Anwenden einer frischen Aktualisierung über oben auf den fehlerhaften eine.

Nachdem vom Azure Fabric Controller die ursprüngliche Anforderung zu aktualisieren, oder aktualisieren den Dienst eingegangen ist, können Sie die nachfolgenden veränderliche Vorgänge starten. D. h., müssen Sie nicht warten, bis der anfänglichen Vorgang abgeschlossen, bevor Sie einen anderen veränderliche Vorgang beginnen können.

Einleiten eines zweiten Aktualisierungsvorgangs, während die erste Aktualisierung laufenden befindet, wird ähnlich wie der Vorgang zurücksetzen ausgeführt. Ist das zweite Update im automatischen Modus, wird die erste Upgrade Domäne sofort, oftmals führenden Instanzen aus mehreren Upgrade Domänen offline an derselben Stelle in der Zeit, sondern aktualisiert werden.

Die veränderliche Vorgänge sind wie folgt: [Ändern Bereitstellungskonfiguration](https://msdn.microsoft.com/library/azure/ee460809.aspx), [Upgrade Bereitstellung](https://msdn.microsoft.com/library/azure/ee460793.aspx), [Bereitstellung Aktualisierungsstatus](https://msdn.microsoft.com/library/azure/ee460808.aspx), [Bereitstellung löschen](https://msdn.microsoft.com/library/azure/ee460815.aspx)und [Zurücksetzen aktualisieren oder aktualisieren](https://msdn.microsoft.com/library/azure/hh403977.aspx).

Zwei Vorgänge, [Bereitstellung abrufen](https://msdn.microsoft.com/library/azure/ee460804.aspx) und [Eigenschaften für Cloud-Dienst erhalten](https://msdn.microsoft.com/library/azure/ee460806.aspx), zurück, die gesperrt Kennzeichnung der untersucht werden kann, um festzustellen, ob ein Vorgang veränderliche für eine gegebene Bereitstellung aufgerufen werden kann.

Um die Version der folgenden Methoden aufzurufen, die die Kennzeichnung gesperrt zurückgibt, müssen Sie auf Anfrage-Header festlegen "X-ms-Version: 2011-10-01" oder eine höhere. Weitere Informationen zu Kopf-Versioning finden Sie unter [Service Management Versioning](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>
## <a name="distribution-of-roles-across-upgrade-domains"></a>Verteilung der Rollen in Upgrade Domänen
Azure verteilt Instanzen von einer Rolle gleichmäßig über eine festgelegte Anzahl von Upgrade-Domänen, die als Teil der Dienstdatei Definition (.csdef) konfiguriert werden können. Die maximale Anzahl von Upgrade Domänen 20 ist und die Standardeinstellung ist 5. Weitere Informationen dazu, wie Sie die Definition Dienstdatei ändern finden Sie unter [Azure Service Definition Schema (.csdef Datei)](cloud-services-model-and-package.md#csdef).

Angenommen, wenn Ihre Rolle zehn Instanzen verfügt, enthält standardmäßig jede Upgrade Domäne zwei Instanzen. Wenn Ihre Rolle 14 Instanzen verfügt, dann vier Upgrade Domänen drei Instanzen enthalten, und eine fünfte Domäne enthält zwei.

Upgrade von Domänen mit 0-basierten Index identifiziert werden: die erste Upgrade Domäne hat eine ID von 0, und die zweite Upgrade Domäne hat eine ID von 1 usw..

Das folgende Diagramm veranschaulicht, wie ein Dienst als enthält zwei Rollen verteilt werden, wenn der Dienst zwei Upgrade Domänen definiert. Der Dienst wird acht Instanzen der Webrolle und neun Instanzen von Worker-Rolle ausgeführt.

![Verteilung der Upgrade Domänen] (media/cloud-services-update-azure-service/IC345533.png "Verteilung der Upgrade Domänen")

> [AZURE.NOTE] Beachten Sie, dass Azure Steuerelementen wie Instanzen über Upgrade Domänen zugeordnet werden. Es ist nicht möglich, um anzugeben, welche Instanzen zu welcher Domäne zugeordnet werden.

## <a name="next-steps"></a>Nächste Schritte
[Zum Verwalten von Cloud-Diensten](cloud-services-how-to-manage.md)  
[Zum Überwachen der Cloud Services](cloud-services-how-to-monitor.md)  
[So konfigurieren Sie Cloud Services](cloud-services-how-to-configure.md)  
