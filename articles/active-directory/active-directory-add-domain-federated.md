<properties
    pageTitle="Fügen Sie Ihren benutzerdefinierten Domänennamen und Einrichten von partnerverbundkontakte melden Sie sich für den Zugriff auf Azure Active Directory | Microsoft Azure"
    description="So Azure Active Directory Ihres Unternehmens Domänennamen hinzu, und wie eingerichtet Partnerbenutzern zwischen Azure Active Directory und Ihre Lösung lokal anmelden."
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
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="add-your-custom-domain-name-to-azure-active-directory"></a>Hinzufügen von Ihren benutzerdefinierten Domänennamen zu Azure Active Directory

Sie können einen benutzerdefinierten Domänennamen, wie etwa "contoso.com" konfigurieren, damit Benutzer auf "contoso.com" eine partnerverbundkontakte einzelne anmelden Nutzung von Ihrem Unternehmensnetzwerk haben können. Wenn bereits Active Directory Federation Services (AD FS) oder einem anderen Föderation Server in Ihrem Netzwerk Ihres Unternehmens ausführen, können Sie Azure AD, um Ihren benutzerdefinierten Domänennamen mit dem Tool Azure AD verbinden verwenden konfigurieren. Sie können auch verwenden Sie Azure AD verbinden, um eine neue AD FS-Umgebung bereitstellen und konfigurieren, dass für partnerverbundkontakte einmaliges Anmelden zu Azure AD.

Wenn Sie keinen und nicht AD FS oder einen anderen Föderation Server bereitstellen möchten, befolgen Sie diese Anweisungen: [Hinzufügen einer benutzerdefinierten Domänennamen zu Azure Active Directory](active-directory-add-domain.md).

## <a name="add-a-custom-domain-name-to-your-directory"></a>Hinzufügen eines benutzerdefinierten Domänennamens zum Verzeichnis

1. Melden Sie sich [Azure klassischen Portal](https://manage.windowsazure.com/) mit einem Benutzerkonto, das ein globaler Administrator Ihres Verzeichnisses Azure AD-ist.

2. Klicken Sie in **Active Directory**öffnen Sie Ihrem Verzeichnis, und wählen Sie die Registerkarte **Domains** .

3. Klicken Sie auf der Befehlsleiste wählen Sie **Hinzufügen**aus, und geben Sie den Namen Ihrer benutzerdefinierten Domäne, beispielsweise "contoso.com". Achten Sie darauf, dass die. com-, .NET- oder andere Erweiterung auf oberster Ebene aufnehmen möchten.

4. Aktivieren Sie das Kontrollkästchen **ich diese Domäne für einmaliges Anmelden mit meinem lokalen Active Directory konfigurieren möchten** .

5. Wählen Sie auf **Hinzufügen**.

Führen Sie das Tool Azure AD verbinden, um den DNS-Eintrag zu erhalten, den Azure AD zur Überprüfung der Domäne verwendet wird. Sehen Sie den DNS-Eintrag **Azure AD-Domäne** Schritt im Assistenten ein. Sie können sehen, was dieser Schritt im Assistenten sieht wie [in der folgenden Schritte](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation)aus. Wenn Sie das Tool Azure AD Verbinden nicht verfügen, können Sie [hier herunterladen](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Fügen Sie bei der domänenregistrierungsstelle für die Domäne den DNS-Eintrag hinzu

Im nächsten Schritt zur Verwendung von Ihren benutzerdefinierten Domänennamen mit Azure AD wird die DNS-Zonendatei für die Domäne aktualisieren. Dadurch Azure AD, um sicherzustellen, dass Ihre Organisation den benutzerdefinierten Domänennamen zugegriffen werden.

1. Melden Sie sich auf der Website für Domänennamen-Registrierungsstelle für Ihren Domänennamen ein. Wenn Sie Zugriff auf die Aktion besitzen, bitten Sie die Person oder ein Team in Ihrer Organisation, hat dieses Access Schritt 2 ausführen und, damit Sie wissen, wenn es abgeschlossen ist.

2. Aktualisieren der DNS-Zonendatei für die Domäne durch den DNS-Eintrag enthalten, das Sie von Azure AD hinzufügen. Dieser DNS-Eintrag ermöglicht Azure AD Ihre Besitzrechte für die Domäne überprüft. Der DNS-Eintrag ändern keine Verhaltensweisen wie e-Mail-Weiterleitung oder Bereitstellung auf einem Webserver.

Lesen Sie Hilfe mit diesem Schritt [Anweisungen zum Hinzufügen eines DNS-Eintrags bei beliebte DNS-Registrierungsstelle](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Überprüfen Sie den Domänennamen mit Azure AD-

Nachdem Sie den DNS-Eintrag hinzugefügt haben, sind Sie bereit sind, überprüfen Sie den Domänennamen mit Azure AD-.

Zum Überprüfen der Domäne, wählen Sie **Weiter** **Azure AD-Domäne** Schritt des Assistenten Azure AD verbinden. Azure AD sucht nach den DNS-Eintrag in der DNS-Zonendatei für die Domäne. Azure AD nur den Namen der Domäne überprüfen, nachdem die DNS-Einträge weitergegeben wurden. Weitergabe häufig dauert nur wenige Sekunden, aber es kann manchmal mindestens eine Stunde dauern. Wenn die Überprüfung erste Mal nicht funktioniert, versuchen Sie es erneut.

Fahren Sie mit der restlichen Schritte des Assistenten Azure AD verbinden. Dadurch wird die Benutzer aus dem Windows Server AD Azure AD synchronisiert. Synchronisierte Benutzer in der Domäne, die Sie für die Föderation konfiguriert werden kann eine partnerverbundkontakte einzelne anmelden Erfahrung aus Ihr Unternehmensnetzwerk zu Azure AD abgerufen.

## <a name="troubleshooting"></a>Behandlung von Problemen

Wenn Sie einen benutzerdefinierten Domänennamen nicht überprüfen können, versuchen Sie Folgendes. Wir verwenden die am häufigsten verwendeten und nach unten zu den wenigsten allgemeinen arbeiten.

1.  **Warten Sie eine Stunde**. DNS-Einträge müssen zum Verteilen, bevor Azure AD die Domäne überprüft werden kann. Dies kann eine Stunde oder länger dauern.

2.  **Sicherstellen, dass der DNS-Eintrag eingegeben wurde, und es korrekt sind**. Führen Sie diesen Schritt bei der Website, für die domänenregistrierungsstelle für die Domäne. Azure AD kann den Domänennamen nicht überprüfen, falls der DNS-Eintrag nicht in der DNS-Zonendatei vorhanden ist, oder wenn es sich nicht um eine genaue Übereinstimmung mit den DNS-Eintrag ist die Azure AD. Wenn Sie keinen Zugriff auf DNS-Einträge für die Domäne auf den Domänennamen-Registrierungsstelle aktualisieren Teilen Sie den DNS-Eintrag mit der Person oder ein Team in Ihrer Organisation, die diese zugreifen können, und bitten Sie den DNS-Eintrag hinzufügen.

3.  **Löschen Sie den Domänennamen aus einem anderen Verzeichnis in Azure Active Directory**. Ein Domänennamen kann in nur ein einzelnes Verzeichnis überprüft werden. Wenn Sie ein Domänennamen in einem anderen Verzeichnis zuvor überprüft wurde, müssen sie es gelöscht werden, bevor sie in Ihrem neuen Verzeichnis überprüft werden kann. Weitere Informationen zum Löschen von Domänennamen, lesen Sie [Verwalten benutzerdefinierten Domänennamen](active-directory-add-manage-domain-names.md)ein.

## <a name="add-more-custom-domain-names"></a>Fügen Sie weitere benutzerdefinierten Domänennamen

Wenn Ihre Organisation mehrere benutzerdefinierten Domänennamen, wie etwa 'contoso.com' und 'contosobank.com', verwendet, können Sie diese bis zu einem Maximum von 900 Domänennamen hinzufügen. Verwenden Sie die gleichen Schritte in diesem Artikel jeder mit den Domänennamen Ihrer hinzufügen.

## <a name="next-steps"></a>Nächste Schritte

-   [Verwalten von benutzerdefinierten Domänennamen](active-directory-add-manage-domain-names.md)
-   [Informationen Sie zu Domain Management Konzepte in Azure Active Directory](active-directory-add-domain-concepts.md)
-   [Zeigen Sie an, dass Ihr Unternehmen branding ist, wenn die Benutzer sich anmelden](active-directory-add-company-branding.md)
-   [Verwalten von Domänennamen in Azure Active Directory mithilfe von PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
