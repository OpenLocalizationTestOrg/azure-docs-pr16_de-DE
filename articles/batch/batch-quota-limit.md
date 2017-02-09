<properties
    pageTitle="Stapelverarbeitung Dienstkontingente und Grenzwerte | Microsoft Azure"
    description="Erfahren Sie mehr über Standard Azure Stapel Kontingente, Grenzwerte und Einschränkungen und erhöht das Kontingent anfordern"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/10/2016"
    ms.author="marsma"/>

# <a name="quotas-and-limits-for-the-azure-batch-service"></a>Kontingente und Grenzwerte für den Stapel Azure-Dienst

Wie mit anderen Diensten Azure vorhanden Grenzwerte auf bestimmte Ressourcen, die dem Dienst Stapel zugeordnet sind. Viele diese Grenzwerte sind standardmäßig Kontingente von Azure-Abonnement oder Kontoebene angewendet. In diesem Artikel werden die Standardeinstellungen, und wie Sie Kontingent anfordern können erhöht.

Wenn Sie beabsichtigen, Produktionsarbeitslasten in Stapel ausführen, müssen Sie mindestens eins der Kontingente über die Standardeinstellung zu vergrößern. Wenn Sie ein Kontingent heraufstufen möchten, können Sie eine online- [Kundensupport Anforderung](#increase-a-quota) kostenlos öffnen.

>[AZURE.NOTE] Bei einem Kontingent handelt es sich um Kreditkarte maximal, Kapazität-Angebot. Wenn Sie umfangreiche Kapazität Anforderungen haben, wenden Sie sich an den Support Azure.

## <a name="subscription-quotas"></a>Abonnements von Kontingenten
**Ressource**|**Standardmäßige Grenzwert**|**Obergrenze**
---|---|---
Stapel Konten pro Region pro Abonnement | 1 | 50

## <a name="batch-account-quotas"></a>Stapel Konto von Kontingenten
[AZURE.INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="other-limits"></a>Weitere Beschränkungen
**Ressource**|**Obergrenze**
---|---
[Gleichzeitige Aufgaben](batch-parallel-node-tasks.md) pro berechnen Knoten | 4 X Anzahl Knoten Kerne
[Applikationen](batch-application-packages.md) pro Stapel-Konto        | 20
Anwendungspaketen pro Anwendung  | 40
Größe des Pakets (jeweils)       | Etwa 195GB<sup>1</sup>

<sup>1</sup> Azure Speichergrenzwert für maximale blockieren BLOB-Größe

## <a name="view-batch-quotas"></a>Anzeigen von Kontingenten Stapel

Anzeigen Ihrer Stapel Konto Kontingente im [Portal Azure][portal].

1. Wählen Sie im Portal **Stapel Konten** aus, und wählen Sie dann das Stapel-Konto, dem Sie interessiert.

2. Klicken Sie auf das Blatt-Konto im Menü Blade **Eigenschaften**

3. Das Blade Eigenschaften zeigt die **Kontingente** derzeit mit dem Konto Stapel angewendet

    ![Stapel Konto von Kontingenten][account_quotas]

## <a name="increase-a-quota"></a>Vergrößern eines Kontingents

Führen Sie die Schritte zum Anfordern eines Kontingents vergrößern mithilfe der [Azure-Portal][portal].

1. Wählen Sie die Kachel **Hilfe + Unterstützung** auf das Fragezeichen (**?**) oder des Portals Dashboards in der oberen rechten Ecke des Portals aus.

2. Wählen Sie **neu support-Anfragen** > **Grundlagen**.

3. Klicken Sie auf das Blade **Grundlagen** :

    ein. **Problemtyp** > **Kontingent**

    b. Wählen Sie Ihr Abonnement.

    c. **Speicherkontingent Typ** > **Stapel**

    d. **Unterstützung für Plan** > **Kontingent Support - enthalten**

    Klicken Sie auf **Weiter**.

4. Klicken Sie auf das **Problem** Blade:

    ein. Wählen Sie eine **schwere** entsprechend Ihrer [geschäftlichen][support_sev].

    b. **Details**Geben Sie jeder Kontingent, die Sie ändern möchten, den Stapel Kontonamen und den neuen Grenzwert.

    Klicken Sie auf **Weiter**.

5. Klicken Sie auf das Blade **Kontaktinformationen** :

    ein. Wählen Sie eine **bevorzugte wenden Sie sich an Methode**aus.

    b. Überprüfen Sie, und geben Sie die erforderlichen Kontaktdetails.

    Klicken Sie auf **Erstellen** , um die Anfrage abzusenden.

Nachdem Sie Ihre Support-Anforderung gesendet haben, wird Azure Support Sie kontaktieren. Beachten Sie, dass die Anfrage durchführen bis zu 2 Arbeitstagen ausführen kann.

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen Sie ein Stapel Azure-Konto über das Azure-portal](batch-account-create-portal.md)

* [Azure Stapel Features (Übersicht)](batch-api-basics.md)

* [Azure-Abonnement und Beschränkungen Service, Kontingente und Einschränkungen](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
