<properties
   pageTitle="Deaktivieren, aktivieren oder Löschen eines Profils Datenverkehr Manager | Microsoft Azure"
   description="Dieser Artikel hilft Ihnen bei der Arbeit mit Ihren Profilen Datenverkehr-Manager."
   services="traffic-manager"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="disable-enable-or-delete-a-profile"></a>Deaktivieren, aktivieren oder Löschen eines Profils


Sie können ein vorhandenes Datenverkehr Manager Profil deaktivieren, damit er auf den konfigurierten Endpunkten nicht benutzeranforderungen verweist. Wenn Sie den Datenverkehr Manager Profil, das Profil selbst und die Informationen in das Profil deaktivieren bleibt intakt und die Benutzeroberfläche Datenverkehr Manager bearbeitet werden kann. Wenn Sie das Profil erneut aktivieren möchten, können Sie ganz einfach tun, damit in der Azure-Portal und Verweise fortgesetzt werden. Wenn Sie ein Profil Datenverkehr Manager im Portal Azure erstellen, wird sie automatisch aktiviert.

## <a name="to-disable-a-profile"></a>So deaktivieren Sie ein Profil

1. Ändern des DNS-Eintrags auf dem Internet-DNS-Server und mit dem entsprechenden Datensatztyp Zeiger auf einen anderen Namen oder die IP-Adresse von einer bestimmten Stelle im Internet. Kurzum, Ändern des DNS-Eintrags auf dem Internet-DNS-Server so, dass sie einen CNAME-Eintrag nicht mehr verwendet, der auf den Domänennamen Ihres Profils Datenverkehr Manager verweist.
1. Datenverkehr wird gestoppt weitergeleitet werden die Endpunkte durch den Datenverkehr Manager einräumen.
1. Wählen Sie das Profil, das Sie deaktivieren möchten. Markieren Sie das Profil, auf der Seite Datenverkehr Manager aus das Profil durch Klicken auf die Spalte neben dem Profilnamen ein. Klicken Sie nicht auf den Namen des Profils oder den Pfeil neben dem Namen des, wie Sie auf der Einstellungsseite für das Profil Vorgangsdauer.
1. Klicken Sie nachdem Sie das Profil ausgewählt haben auf Deaktivieren am unteren Rand der Seite.

## <a name="to-enable-a-profile"></a>Aktivieren ein Profils

1. Wählen Sie das Profil, das Sie aktivieren möchten. Markieren Sie das Profil, auf der Seite Datenverkehr Manager aus das Profil durch Klicken auf die Spalte neben dem Profilnamen ein. Klicken Sie nicht auf den Namen des Profils oder den Pfeil neben dem Namen des, wie Sie auf der Einstellungsseite für das Profil Vorgangsdauer.
1. Klicken Sie nachdem Sie das Profil ausgewählt haben auf aktivieren, am unteren Rand der Seite.
1. Ändern des DNS-Eintrags auf dem Internet-DNS-Server mit den Typ CNAME-Eintrag, der den Namen Ihres Unternehmens in den Domänennamen Ihres Profils Datenverkehr Manager zugeordnet ist. Weitere Informationen finden Sie unter [Punkt einer Unternehmen Internet-Domäne zu einer Domäne den Datenverkehr-Manager](traffic-manager-point-internet-domain.md).
1. Datenverkehr wird weitergeleitet werden die Endpunkte erneut starten.

## <a name="delete-a-profile"></a>Löschen eines Profils


1. Stellen Sie sicher, dass der DNS-Eintrag auf dem Internet-DNS-Server nicht mehr einen CNAME-Eintrag verwendet, der auf den Domänennamen Ihres Profils Datenverkehr Manager verweist.
1. Wählen Sie das Profil, das Sie löschen möchten. Markieren Sie das Profil, auf der Seite Datenverkehr Manager aus Profils durch
1. Klicken Sie auf die Spalte neben dem Profil. Klicken Sie nicht auf den Namen des Profils oder den Pfeil neben dem Namen des, wie Sie auf der Einstellungsseite für das Profil Vorgangsdauer.
1. Klicken Sie nachdem Sie das Profil ausgewählt haben auf am unteren Rand der Seite löschen.

## <a name="next-steps"></a>Nächste Schritte

[Datenverkehr Manager – deaktivieren oder Aktivieren von außen liegenden Tabellenblättern](disable-or-enable-an-endpoint.md)

[Konfigurieren von Failover routing Methode](traffic-manager-configure-failover-routing-method.md)

[Konfigurieren der Funktion RUNDEN Robert routing Methode](traffic-manager-configure-round-robin-routing-method.md)

[Konfigurieren der Leistung routing Methode](traffic-manager-configure-performance-routing-method.md)

[Problembehandlung Datenverkehr Manager heruntergestuft Zustand](traffic-manager-troubleshooting-degraded.md)

