<properties
    pageTitle="Verwalten von Berechtigungen zu Ressourcen pro Benutzer in Azure Stapel (Dienstadministrator und Mandanten) | Microsoft Azure"
    description="Als Dienstadministrator oder Mandanten Informationen Sie zum Verwalten von Berechtigungen für Ressourcen pro Benutzer."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="manage-user-permissions"></a>Verwalten von Benutzerberechtigungen

Ein Benutzer in Azure Stapel kann es sich um einen Reader, Besitzer oder Mitwirkender für jede Instanz eines Abonnements, Ressourcengruppe oder Dienst sein. Angenommen, Benutzer A möglicherweise verfügen über Leseberechtigungen Abonnement 1, aber die Berechtigungen eines Websitebesitzers auf virtuellen Computern 7.

-   Reader: Benutzer kann alles anzeigen, aber keine Änderungen vornehmen.

-   Für den Systemzustand: Benutzer kann alles außer der Zugriff auf Ressourcen verwalten.

-   Besitzer: Benutzer kann alles, einschließlich des Zugriffs auf Ressourcen verwalten.


## <a name="set-access-permissions-for-a-user"></a>Festlegen von Zugriffsberechtigungen für einen Benutzer

1.  Melden Sie sich mit einem Konto mit Berechtigungen eines Websitebesitzers, die der Ressource, die Sie verwalten möchten.

2.  Klicken Sie in das Blade für die Ressource, die **Access** -Symbol auf ![](media/azure-stack-manage-permissions/image1.png).

3.  Klicken Sie in das Blade **Benutzer** **Rollen**aus.

4.  Klicken Sie in das Blade **Rollen** auf **Hinzufügen** , um Berechtigungen für den Benutzer hinzufügen.

## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen von einem Stapel Azure-Mandanten](azure-stack-add-new-user-aad.md)
