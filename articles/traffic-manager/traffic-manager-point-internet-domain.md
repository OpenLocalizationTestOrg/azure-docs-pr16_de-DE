<properties
    pageTitle="Zeigen Sie eine Firma Internet-Domäne auf einen Domänennamen Datenverkehr Manager | Microsoft Azure"
    description="In diesem Artikel helfen Ihnen die zeigen Sie den Namen Ihres Unternehmens zu einem Domänennamen Datenverkehr-Manager."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="point-a-company-internet-domain-to-an-azure-traffic-manager-domain"></a>Zeigen Sie eine Firma Internet-Domäne auf eine Domäne Azure Datenverkehr Manager

Beim Erstellen eines Profils Datenverkehr Manager weist Azure automatisch einen DNS-Namen für das Profil ein. Um einen Namen aus der DNS-Zone verwenden möchten, erstellen Sie einen CNAME DNS-Eintrag, der den Domänennamen Ihres Profils Datenverkehr Manager zugeordnet ist. Sie können den Domänennamen Datenverkehr Manager im Abschnitt **Allgemein** auf der Seite Konfiguration des Profils Datenverkehr Manager suchen.

Um den Datenverkehr Manager DNS Namen contoso.trafficmanager.net Namen www.contoso.com verweisen erstellen Sie beispielsweise den folgenden DNS-Eintrag:

    www.contoso.com IN CNAME contoso.trafficmanager.net

Alle Datenverkehr Anfragen mit *www.contoso.com* Abrufen an *contoso.trafficmanager.net*weitergeleitet.

>[AZURE.IMPORTANT] Können nicht zeigen Sie auf die Domäne den Datenverkehr Manager eine Domäne der zweiten Ebene, wie etwa *"contoso.com"*. DNS-Protokollstandards zulassen CNAME-Einträge für die zweite Ebene Domänennamen nicht.

## <a name="next-steps"></a>Nächste Schritte

- [Weiterleitung Methoden Datenverkehr-Manager](traffic-manager-routing-methods.md)
- [Datenverkehr-Manager – deaktivieren, aktivieren oder Löschen eines Profils](disable-enable-or-delete-a-profile.md)
- [Datenverkehr Manager – deaktivieren oder Aktivieren von außen liegenden Tabellenblättern](disable-or-enable-an-endpoint.md)
