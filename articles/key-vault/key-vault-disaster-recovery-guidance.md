<properties
    pageTitle="Was zu tun ist im Falle einer Azure service Unterbrechung, die Azure-Taste Tresor wirkt sich auf | Microsoft Azure"
    description="Erfahren Sie, was zu tun ist im Falle einer Unterbrechung Azure Service, die Azure-Taste Tresor auswirkt."
    services="key-vault"
    documentationCenter=""
    authors="adamglick"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="key-vault"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="sumedhb;aglick"/>


# <a name="azure-key-vault-availability-and-redundancy"></a>Azure-Taste Tresor Verfügbarkeit und Redundanz

Azure-Taste Tresor verfügt über mehrere Sicherheitsebenen redundant sicherzustellen, dass Ihre Tasten und Kennwörter an Ihrer Anwendung verfügbar bleiben, auch wenn einzelne Komponenten des Diensts fehlschlägt.

Den Inhalt Ihrer Key Tresor werden in der Region und einer sekundären Region mindestens 150 Meilen abwesend, aber innerhalb der gleichen Geography repliziert. Auf diese Weise hohe Zuverlässigkeit Tasten und vertraulichen Daten beibehalten.

Wenn einzelne Komponenten innerhalb des Diensts Key Tresor fehl, Schritt dienen Anforderung um sicherzustellen, dass es keine Verringerung der Funktionalität ist alternative Komponenten in der Region in. Sie müssen keine bezüglich dies ausgelöst. Es geschieht automatisch und für Sie transparent werden.

In dem seltene Fall, dass eine ganze Azure Region nicht verfügbar ist werden die Anforderungen, die Sie der Azure-Taste Tresor in diesem Bereich vornehmen automatisch weitergeleiteten (*über Fehler bei*) sekundäre Region. Wenn die primäre Region wieder verfügbar ist, Anfragen weitergeleitet werden (*konnte nicht wieder*) zurück, die primäre Region. In diesem Fall müssen Sie nicht Maßnahmen ergreifen, da dies automatisch geschieht.

Es sind ein paar Punkte beachten:

* Fällt einer Region kann es für den Dienst über treten ein paar Minuten dauern. Besprechungsanfragen, die während dieses Zeitraums vorgenommen werden können ein Fehler auftreten, bis das Failover abgeschlossen ist.
* Nach Abschluss ein Failovers ist Ihre Key Tresor im schreibgeschützten Modus. Sind Besprechungsanfragen, die in diesem Modus unterstützt werden:
 * Liste Key Depots
 * Abgerufen Sie Eigenschaften der wichtigsten Depots werden
 * Liste Kennwörter
 * Abrufen von vertraulichen Daten
 * Liste Tasten
 * Abrufen von Tasten (Eigenschaften)
 * Verschlüsseln
 * Entschlüsseln
 * Umbrechen
 * Entpacken
 * Vergewissern Sie sich
 * Melden Sie sich
 * Sicherung
* Nachdem ein Failover wieder fehlgeschlagen ist, sind alle Anforderungstypen (einschließlich Lese- *und* Schreibzugriff Anfragen) verfügbar.
