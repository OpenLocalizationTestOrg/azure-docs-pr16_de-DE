<properties
    pageTitle="Hinzufügen von Ihren benutzerdefinierten Domänennamen zu Azure Active Directory | Microsoft Azure"
    description="Wie Azure Active Directory Ihres Unternehmens Domänennamen hinzugefügt, und wie Sie den Namen der Domäne überprüfen."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/30/2016"
    ms.author="curtand;jeffsta"/>

# <a name="add-a-custom-domain-name-to-azure-active-directory"></a>Hinzufügen von benutzerdefinierten Domänennamen zu Azure Active Directory

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-domains-add-azure-portal.md)
- [Azure klassischen-portal](active-directory-add-domain.md)

Sie besitzen ein oder mehrere Domänennamen, die Ihre Organisation verwendet, um eine Geschäftsbeziehung unterhalten, und Ihre Benutzer melden Sie sich mit dem Unternehmensnetzwerk mit Ihrem Domänennamen Ihres Unternehmens. Jetzt, da Sie Azure Active Directory (Azure AD) verwenden, können Sie Ihren Domänennamen corporate Azure AD ebenfalls hinzufügen. Dies können Sie Benutzernamen im Verzeichnis zuweisen, die an die Benutzer, vertraut, z. B. sind ‘alice@contoso.com.’ der Vorgang ist einfach:

1. Hinzufügen des benutzerdefinierten Domänennamens zum Verzeichnis
2. Hinzufügen eines DNS-Eintrags für den Namen der Domäne bei der domänenregistrierungsstelle
3. Überprüfen des benutzerdefinierten Domänennamens in Azure Active Directory

> [AZURE.NOTE] Wenn Sie beabsichtigen, konfigurieren Ihren benutzerdefinierten Domänennamen mit Active Directory Federation Services (AD FS) oder einem anderen Security token Service (STS) in Ihrem Netzwerk Ihres Unternehmens verwendet werden, folgen Sie den Anweisungen in [Hinzufügen und konfigurieren eine Domäne für die Föderation mit Azure Active Directory](active-directory-add-domain-federated.md). Dies ist nützlich, wenn Sie, zum Synchronisieren von Benutzern aus Ihrem Firmenverzeichnis zu Azure AD planen und [Kennwort Hash synchronisieren](active-directory-aadconnectsync-implement-password-synchronization.md) nicht Ihren Anforderungen erfüllt.

## <a name="add-a-custom-domain-name-to-your-directory"></a>Hinzufügen eines benutzerdefinierten Domänennamens zum Verzeichnis

1. Melden Sie sich [Azure klassischen Portal](https://manage.windowsazure.com/) mit einem Benutzerkonto, das ein globaler Administrator Ihres Verzeichnisses Azure AD-ist.

2. Öffnen Sie in **Active Directory**Ihr Verzeichnis, und wählen Sie die Registerkarte **Domains** .

3. Wählen Sie auf der Befehlsleiste **Hinzufügen**aus. Geben Sie den Namen Ihrer benutzerdefinierten Domäne, beispielsweise "contoso.com". Achten Sie darauf, gehören die. com-, .NET- oder andere Erweiterung auf oberster Ebene, und lassen Sie das Kontrollkästchen für "einmaliges Anmelden" (Verbund) deaktiviert.

4. Wählen Sie auf **Hinzufügen**.

5. Klicken Sie auf der zweiten Seite des Assistenten Domäne hinzufügen erhalten Sie den DNS-Eintrag, den Azure AD verwendet werden, um sicherzustellen, dass Ihre Organisation den benutzerdefinierten Domänennamen zugegriffen.

Jetzt, da Sie den Namen der Domäne hinzugefügt haben, muss im Besitz der Organisation den Namen der Domäne Azure AD überprüfen. Bevor diese Überprüfung Azure AD ausgeführt werden kann, müssen Sie einen DNS-Eintrag in der DNS-Zonendatei für den Namen der Domäne hinzufügen. Diese Aufgabe wird der Website für Domänennamen-Registrierungsstelle für den Namen der Domäne durchgeführt.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Fügen Sie bei der domänenregistrierungsstelle für die Domäne den DNS-Eintrag hinzu

Im nächsten Schritt zur Verwendung von Ihren benutzerdefinierten Domänennamen mit Azure AD wird die DNS-Zonendatei für die Domäne aktualisieren. Dadurch Azure AD, um sicherzustellen, dass Ihre Organisation den benutzerdefinierten Domänennamen zugegriffen werden.

1.  Melden Sie sich an den Domänennamen-Registrierungsstelle für die Domäne aus. Wenn Sie Zugriff auf den DNS-Eintrag aktualisieren besitzen, bitten Sie die Person oder ein Team, hat dieses Access Schritt 2 ausführen und, damit Sie wissen, wenn es abgeschlossen ist.

2.  Aktualisieren Sie die DNS-Zonendatei für die Domäne, indem der DNS-Eintrag enthalten, das Sie von Azure AD hinzufügen. Dieser DNS-Eintrag ermöglicht Azure AD Ihre Besitzrechte für die Domäne überprüft. Der DNS-Eintrag ändern keine Verhaltensweisen wie e-Mail-Weiterleitung oder Bereitstellung auf einem Webserver.

Hilfe zu dieser den DNS-Eintrag hinzufügen finden Sie unter [Anweisungen zum Hinzufügen eines DNS-Eintrags bei beliebte DNS-Registrierungsstelle](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Überprüfen Sie den Domänennamen mit Azure AD-

Nachdem Sie den DNS-Eintrag hinzugefügt haben, sind Sie bereit sind, überprüfen Sie den Domänennamen mit Azure AD-.

Wenn Sie immer noch müssen den Assistenten für **Domänen hinzufügen** öffnen, und wählen auf der dritten Seite des Assistenten **Überprüfen** . Wenn Sie **Überprüfen**ausgewählt haben, sieht Azure AD für den DNS-Eintrag in der DNS-Zonendatei für die Domäne aus. Azure AD kann nur, nachdem Sie die DNS-Einträge weitergegeben wurden, den Namen der Domäne überprüfen. Diese Verteilung häufig dauert nur wenige Sekunden, aber können manchmal nehmen eine Stunde oder mehr. Wenn die Überprüfung erste Mal nicht funktioniert, versuchen Sie es erneut.

Wenn Sie der Assistenten für **Domänen hinzufügen** nicht immer noch geöffnet ist, können Sie die Domäne in der [Azure klassischen Portal](https://manage.windowsazure.com/)überprüfen:

1.  Melden Sie sich mit einem Benutzerkonto, das ein globaler Administrator Ihres Verzeichnisses Azure AD-ist.

2.  Öffnen Sie Ihr Verzeichnis, und wählen Sie die Registerkarte **Domains** .

3.  Wählen Sie den Domänennamen ein, den Sie überprüfen, und wählen auf der Befehlsleiste **Sie prüfen** möchten.

4. Wählen Sie im Dialogfeld für die vollständige Überprüfung **Überprüfen** aus.

Jetzt können Sie [die Namen der Benutzer, die Ihren benutzerdefinierten Domänennamen enthalten, zuweisen](active-directory-add-domain-add-users.md).

## <a name="troubleshooting"></a>Behandlung von Problemen

Wenn Sie einen benutzerdefinierten Domänennamen nicht überprüfen können, versuchen Sie Folgendes. Wir verwenden die am häufigsten verwendeten und nach unten zu den wenigsten allgemeinen arbeiten.

1.  **Warten Sie eine Stunde**. DNS-Einträge müssen zum Verteilen, bevor Azure AD die Domäne überprüft werden kann. Dies kann eine Stunde oder länger dauern.

2.  **Sicherstellen, dass der DNS-Eintrag eingegeben wurde, und es korrekt sind**. Führen Sie diesen Schritt bei der Website, für die domänenregistrierungsstelle für die Domäne. Azure AD kann den Domänennamen nicht überprüfen, falls der DNS-Eintrag nicht in der DNS-Zonendatei vorhanden ist, oder wenn es sich nicht um eine genaue Übereinstimmung mit den DNS-Eintrag ist die Azure AD. Wenn Sie keinen Zugriff auf DNS-Einträge für die Domäne auf den Domänennamen-Registrierungsstelle aktualisieren Freigeben des DNS-Eintrags für die Person oder ein Team in Ihrer Organisation, die diese zugreifen können, und bitten Sie den DNS-Eintrag hinzufügen.

3.  **Löschen Sie den Domänennamen aus einem anderen Verzeichnis in Azure Active Directory**. Ein Domänennamen kann in nur ein einzelnes Verzeichnis überprüft werden. Wenn Sie ein Domänennamen in einem anderen Verzeichnis zuvor überprüft wurde, müssen sie es gelöscht werden, bevor sie in Ihrem neuen Verzeichnis überprüft werden kann. Weitere Informationen zum Löschen von Domänennamen, lesen Sie [Verwalten benutzerdefinierten Domänennamen](active-directory-add-manage-domain-names.md)ein.


## <a name="add-more-custom-domain-names"></a>Fügen Sie weitere benutzerdefinierten Domänennamen

Wenn Ihre Organisation mehrere benutzerdefinierten Domänennamen, wie etwa 'contoso.com' und 'contosobank.com', verwendet, können Sie diese bis zu einem Maximum von 900 Domänennamen hinzufügen. Verwenden Sie die gleichen Schritte in diesem Artikel jeder mit den Domänennamen Ihrer hinzufügen.

## <a name="next-steps"></a>Nächste Schritte

-   [Zuweisen von Benutzernamen, die Ihren benutzerdefinierten Domänennamen enthalten.](active-directory-add-domain-add-users.md)
-   [Verwalten der benutzerdefinierten Domänennamen](active-directory-add-manage-domain-names.md)
-   [Informationen Sie zu Domain Management Konzepte in Azure Active Directory](active-directory-add-domain-concepts.md)
-   [Zeigen Sie an, dass Ihr Unternehmen branding ist, wenn die Benutzer sich anmelden](active-directory-add-company-branding.md)
-   [Verwalten von Domänennamen in Azure Active Directory mithilfe von PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
