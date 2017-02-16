<properties
    pageTitle="Erweiterte Anforderung mit Azure-API Management begrenzungsebene"
    description="Informationen Sie zum Erstellen und Anwenden von flexible Kontingent und Zins Richtlinien mit Azure-API Management einschränken."
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="advanced-request-throttling-with-azure-api-management"></a>Erweiterte Anforderung mit Azure-API Management begrenzungsebene

Möglichkeit zur schränken Sie eingehender Anfragen ist eine wichtige Rolle der Azure-API Management. Entweder durch die Kontrolle der Serviceanfragen oder mit dem gesamten Anfragen/Daten übertragen, API-Management API Anbieter ihre APIs Missbrauch schützen und Wert für die verschiedenen Ebenen der API Produkt erstellen können.

## <a name="product-based-throttling"></a>Produkt-basierten begrenzungsebene
Bis Datum, das Zins begrenzungsebene wurden Funktionen beschränken sich auf ausgelegte auf ein bestimmtes Produkt Abonnement (im Wesentlichen ein Schlüssel), im Publisher-API Verwaltungsportal definiert. Dies ist sinnvoll, für den Anbieter API Grenzwerte auf die Entwickler anwenden, die angemeldet haben, deren API verwenden, allerdings es hilft nicht, beispielsweise in einzelne Endbenutzer der API begrenzungsebene. Es ist möglich, die für einzelne Benutzer der Anwendung des Entwicklers das Kontingent für die gesamte nutzen und dann verhindern, dass andere Kunden des Entwicklers, damit Sie die Anwendung verwenden. Darüber hinaus können mehrere Kunden, die eine große Anzahl von Anfragen generieren möglicherweise Zugriff auf gelegentliche Benutzer einschränken.

## <a name="custom-key-based-throttling"></a>Benutzerdefinierten Product Key basierend begrenzungsebene
Die neuen [Zins Beschränkung durch Schlüssel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) und [Kontingent-von-Key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) Richtlinien bieten eine erheblich flexiblere Lösung zu den Datenverkehr Steuerelement. Diese neuen Richtlinien können Sie zum Definieren von Ausdrücken, um die Tasten identifizieren möchten, die zum Nachverfolgen der Datenverkehrsverwendung verwendet werden. Die Arbeitsweise Dies ist einfachste anhand eines Beispiels dargestellt. 

## <a name="ip-address-throttling"></a>IP-Adresse begrenzungsebene
Die folgenden Richtlinien einschränken eine einzelnen IP-Adresse nur 10 Anrufe pro Minute, für insgesamt 1.000.000 Anrufe und 10.000 KB Bandbreite pro Monat. 

    <rate-limit-by-key  calls="10"
              renewal-period="60"
              counter-key="@(context.Request.IpAddress)" />

    <quota-by-key calls="1000000"
              bandwidth="10000"
              renewal-period="2629800"
              counter-key="@(context.Request.IpAddress)" />

Wenn alle Clients im Internet eine eindeutige IP-Adresse verwendet haben, wird dies kann eine effektive Möglichkeit zum Einschränken der Verwendung von Benutzer sein. Es ist jedoch recht wahrscheinlich, dass mehrere Benutzer werden eine einzelne öffentliche IP-Adresse aufgrund sie Zugriff auf das Internet über ein NAT-Gerät freigeben. Trotz dies für APIs, die nicht authentifizierten Zugriff zulassen der `IpAddress` möglicherweise die beste Option.

## <a name="user-identity-throttling"></a>Benutzer Identität begrenzungsebene
Wenn ein Benutzer authentifiziert ist, und klicken Sie dann ein Drosselung Schlüssel basierend auf Informationen, die ein, die eindeutig generiert werden kann Benutzer.

    <rate-limit-by-key calls="10"
        renewal-period="60"
        counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />

In diesem Beispiel wir die Autorisierung Kopfzeile extrahieren, konvertieren sie `JWT` Objekt, und verwenden Sie den gewünschten Betreff für die Token zu identifizieren des Benutzers verwenden, die als die Rate Schlüssel einschränken. Wenn die Identität des Benutzers, in gespeichert ist der `JWT` wie eine der anderen Ansprüche, dass der Wert in seine Position verwendet werden kann.

## <a name="combined-policies"></a>Kombinierte Richtlinien
Obwohl die neuen Drosselung Richtlinien mehr Kontrolle als die vorhandenen Drosselung Richtlinien bereitstellen, besteht weiterhin Wert Kombination beider Funktionen. Begrenzungsebene nach Produkt Abonnementschlüssel ([Grenzwert Anruf Zins durch Abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) und [einsatzkontingent ein Abonnement festlegen](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) ist eine großartige Möglichkeit zum Aktivieren einer API durch Belastung basierend auf Verwendung Ebenen monetizing. Das feiner detaillierte Steuerelement nicht einschränken, indem Sie Benutzer Komplement wird und verhindert, dass eines bestimmten Benutzers Verhalten beeinträchtigt die Oberfläche eines anderen. 

## <a name="client-driven-throttling"></a>Leistungsgesteuert begrenzungsebene Client
Wenn Sie die Taste Drosselung mithilfe einer [Richtlinienausdruck](https://msdn.microsoft.com/library/azure/dn910913.aspx)definiert ist, ist es den API-Anbieter, der wie die begrenzungsebene ausgelegte auswählen ist. Ein Entwickler sollten jedoch so steuern Sie, wie sie Grenzwert für eigene bewerten Kunden. Dies konnte vom Anbieter API aktiviert werden durch eine benutzerdefinierte Kopfzeile einführen des Entwicklers Clientanwendung die EINGABETASTE, um die API kommunizieren dürfen.

    <rate-limit-by-key calls="100"
              renewal-period="60"
              counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>

Dadurch des Entwicklers Clientanwendung auswählen, wie Benutzer die Taste einschränken Rate erstellen möchten. Mit ein wenig Einfallsreichtum konnte ein Cliententwickler Ebenen für eigene durch Sätze von Tasten für Benutzer zuordnen und drehen die Verwendung der Schlüssel erstellen.

## <a name="summary"></a>Zusammenfassung
Azure-API Management bietet Zins und Angebot begrenzungsebene zum Schützen sowohl Wert an den API-Dienst hinzufügen. Die neuen Drosselung Richtlinien mit benutzerdefinierten Bereiche Regeln können Sie feiner detaillierte Kontrolle diesen Richtlinien, damit Ihre Kunden noch deutlicher Applications erstellen können. Die Beispiele in diesem Artikel werden die Verwendung dieser neuen Richtlinien von Fertigung Rate Tasten mit IP-Adressen, Benutzeridentitäten und Client generiert Werte einschränken. Es gibt jedoch viele andere Teile der Nachricht, die Benutzer-Agents, URL-Pfad Satzteile, Nachrichtengröße verwendet werden kann.

## <a name="next-steps"></a>Nächste Schritte
Liefern Sie uns bitte Ihr Feedback im Thread Disqus für dieses Thema. Es wäre hervorragend, um Informationen zu anderen möglichen Schlüsselwerte zu hören, die eine logische Wahl der Szenarien wurden.

## <a name="watch-a-video-overview-of-these-policies"></a>Schauen Sie sich einen Überblick über diese Richtlinien video
Weitere Informationen zu den in diesem Artikel behandelt [Zins Beschränkung durch Schlüssel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) und [Kontingent-von-Key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) -Richtlinien schauen Sie sich das folgende Video.

> [AZURE.VIDEO advanced-request-throttling-with-azure-api-management]
