<properties
    pageTitle="Was ist Self-Service beim Registrieren für Azure? | Microsoft Azure"
    description="Eine Übersicht über die Self-service beim Registrieren für Azure, zum Verwalten der Anmeldevorgang und wie Sie einen DNS-Domänennamen zu übernehmen."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/23/2016"
    ms.author="curtand"/>


# <a name="what-is-self-service-signup-for-azure"></a>Was ist Self-Service beim Registrieren für Azure?

In diesem Thema wird erläutert, sowie Self-service-Setup zu DNS-Domänennamen zu übernehmen.  

## <a name="why-use-self-service-signup"></a>Gründe für die Verwendung von Self-service-Anmeldung

- Aufrufen Sie Kunden Services schneller gewünschten.
- Erstellen Sie die e-Mail-basierte Angebote für einen Dienst an.
- Erstellen Sie die e-Mail-basierte beim Registrieren Zahlungen, die schnell Benutzer Identitäten mit ihrer Arbeit leicht zu merkende e-Mail-Aliase erstellen können.
- Nicht verwaltete Azure Verzeichnisse später in verwalteten Verzeichnisse vorgenommen werden können und für andere Dienste wiederverwendet werden.

## <a name="terms-and-definitions"></a>Begriffe und Definitionen

+ **Self-Service registrieren**: Dies ist die Methode, bei dem ein Benutzer für einen Clouddienst anmeldet und verfügt über eine Identität in Azure Active Directory (Azure AD) automatisch für sie erstellt basierend auf ihrer e-Mail-Domäne.
+ **Nicht verwaltete Azure Directory**: Dies ist das Verzeichnis, in dem die Identität erstellt wird. Eine nicht verwaltete Verzeichnis ist ein Verzeichnis, das kein globalen Administrator zugeordnet ist.
+ **Benutzer-e-Mail-überprüft**: Dies ist eine Art des Benutzerkontos in Azure Active Directory. Ein Benutzer mit einer Identität erstellt automatisch nach der Anmeldung für ein Angebot, Self-service wird als Benutzer e-Mails überprüft bezeichnet. Ein Benutzer e-Mails überprüft ist ein reguläre Mitglied ein Verzeichnis mit Creationmethod markiert = EmailVerified.

## <a name="user-experience"></a>Benutzerfunktionalität

Angenommen, beispielsweise einen Benutzer, dessen e-Mail-Adresse des Dan@BellowsCollege.com vertrauliche Dateien per e-Mail empfängt. Die Dateien haben durch Azure Verwaltung von Informationsrechten (RMS Azure) geschützt wurden. Peters Organisation Ausgleich-rohrverbind Kalkulation, nicht für RMS Azure registriert hat, aber noch wurde Active Directory RMS bereitgestellt. In diesem Fall kann Dan für ein kostenloses Abonnement für RMS für einzelne Benutzer anmelden, um die geschützten Dateien lesen.

Ist Dan den ersten Benutzer mit einer e-Mail-Adresse von BellowsCollege.com für dieses Angebot, Self-Service anmelden, und klicken Sie dann eine nicht verwalteten Verzeichnis für BellowsCollege.com in Azure Active Directory erstellt wird. Wenn andere Benutzer der Domäne BellowsCollege.com für dieses Angebot oder eine ähnliche Self-service-Angebot registrieren, haben sie auch Benutzerkonten erstellt im selben nicht verwalteten Verzeichnis in Azure e-Mail überprüft.

## <a name="admin-experience"></a>Admin-Benutzeroberfläche

Ein Administrator den DNS-Domänennamen eines nicht verwalteten Azure Directory Besitzer kann übernehmen, oder Verzeichnis nach Nachweis Besitz zusammenführen. In den nächsten Abschnitten erläutern die Administrator-Oberfläche ausführlicher, aber hier eine Übersicht:

- Wenn Sie über eine nicht verwaltete Azure Verzeichnis übernehmen möchten, werden Sie einfach die globalen Administrators von nicht verwalteten Verzeichnis aus. Dies ist eine interne Übernahme bezeichnet.
- Wenn Sie einem nicht verwalteten Azure Verzeichnis zusammenführen, Sie den DNS-Domänennamen, der nicht verwalteten Verzeichnis zu Ihrem verwalteten Azure Verzeichnis hinzufügen und eine Zuordnung von Benutzern für die Ressourcen erstellt können also Benutzer weiterhin ohne Unterbrechung Dienste zuzugreifen. Dies ist eine externe Übernahme bezeichnet.

## <a name="what-gets-created-in-azure-active-directory"></a>Was wird in Azure Active Directory erstellt werden?

#### <a name="directory"></a>Verzeichnis

- Für die Domäne ein Azure Active Directory-Verzeichnis wird erstellt, ein Verzeichnis pro Domäne.
- Azure AD-Verzeichnis weist keine globalen Administrator.

#### <a name="users"></a>Benutzer

- Für jeden Benutzer anmeldet, wird ein Benutzerobjekt im Verzeichnis Azure AD-erstellt.
- Jedes Benutzerobjekt wird als extern markiert.
- Jeden Benutzer erhält Zugriff auf den Dienst, die sie für signiert.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Wie kann ich ein Self-service Azure AD beanspruchen Verzeichnis für eine Domäne, die ich besitze?

Sie können eine Self-service Azure AD beanspruchen Verzeichnis durch Ausführen der Domäne Überprüfung. Domäne Überprüfung weist nach, dass Sie die Domäne besitzen, durch Erstellen von DNS-Einträge.

Es gibt zwei Möglichkeiten, eine DNS-Übernahme eines Azure AD-Directory gehen Sie wie folgt aus:

- Interner Übernahme (Administrator einer nicht verwalteten Azure Directory erkennt und möchte in verwalteten Verzeichnis umwandeln)
- externe Übernahme (Admin-versucht, deren verwalteten Azure Verzeichnis eine neue Domäne hinzufügen)

Sie möglicherweise interessieren, überprüfen, dass Sie eine Domäne besitzen, da Sie über ein Verzeichnis nicht verwalteten aufzeichnen werden, nachdem ein Benutzer Self-service-Anmeldung durchgeführt oder werden möglicherweise eine neue Domäne zu einem vorhandenen verwalteten Verzeichnis hinzufügen. Angenommen, Sie haben eine Domäne namens "contoso.com" und eine neue Domäne mit dem Namen contoso.co.uk oder contoso.uk hinzugefügt werden sollen.

## <a name="what-is-domain-takeover"></a>Was ist die Domäne Übernahme?  

In diesem Abschnitt beschrieben, wie Sie überprüfen, ob Sie eine Domäne besitzen

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Was ist die Domäne Überprüfung, und warum wird es verwendet?

Um das Ausführen von Vorgängen in einem Verzeichnis erfordert Azure AD an, dass Sie Besitzer der DNS-Domäne überprüfen.  Überprüfung der Domäne können Sie beanspruchen Verzeichnis und entweder Höherstufen Self-service-Verzeichnis zu verwalteten Verzeichnis, oder ein vorhandenes verwalteten Verzeichnis Self-service-Verzeichnis zusammenführen.

## <a name="examples-of-domain-validation"></a>Beispiele für die Domäne Überprüfung

Es gibt zwei Möglichkeiten, eine DNS-Übernahme eines Verzeichnisses gehen Sie wie folgt aus:

+ Interner Übernahme (beispielsweise ein Administrator Self-service, nicht verwalteten Verzeichnis erkennt und möchte in verwalteten Verzeichnis umwandeln)
+ externe Übernahme (beispielsweise ein Administrator versucht, eine neue Domäne zu einem verwalteten Verzeichnis hinzufügen)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>Interner Übernahme - Self-service, nicht verwalteten Verzeichnis benutzerspezifisch verwalteten Verzeichnis Höherstufen

In diesem Fall internen Übernahme ruft Verzeichnis aus einem nicht verwalteten Verzeichnis in verwalteten Verzeichnis konvertiert. Sie müssen DNS-Domain Name-Überprüfung, führen Sie die Stelle, an der Sie einen MX-Eintrag oder einen TXT-Eintrag in der DNS-Zone erstellen. Diese Aktion:

+ Überprüft, dass Sie die Domäne besitzen
+ Macht die verwaltete Verzeichnis
+ Macht Sie den globalen Administrator des Verzeichnisses

Angenommen, ein IT-Administrator aus Ausgleich-rohrverbind Kalkulation erkennt, dass Benutzer die Schule für Self-service-Angebote angemeldet haben. Als Besitzer registrierte der DNS-name BellowsCollege.com, können Sie der IT-Administrator Besitzer des DNS-Namens in Azure überprüfen und dann nicht verwalteten Verzeichnis übernehmen. Verzeichnis wird dann verwalteten Verzeichnis, und der IT-Administrator die globalen Administratorrolle für das BellowsCollege.com Verzeichnis zugeordnet ist.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Externe Übernahme - ein Self-service-Verzeichnis in einem vorhandenen verwalteten Verzeichnis zusammenführen

In einer externen Übernahme bereits verwaltetes Verzeichnis und alle Benutzer und Gruppen aus einem nicht verwalteten Verzeichnis dieser verwaltete Verzeichnis teilnehmen soll, anstatt eigene zwei Verzeichnisse zu trennen.

Als Administrator eines verwalteten Verzeichnisses Hinzufügen einer Domänennamens, und die Domäne geschieht zu einem nicht verwalteten Verzeichnis zugeordnet haben.

Angenommen, angenommen, Sie sind ein IT-Administrator und bereits verwaltetes Verzeichnis für Contoso.com, einen Domänennamen aus, die für Ihre Organisation registriert ist. Feststellen, dass Benutzer in Ihrer Organisation Self-service melden Sie sich von für ein Angebot durchgeführt haben mithilfe der e-Mail-Domänenname user@contoso.co.uk, welche ist ein anderer Domänenname wird, die im Besitz der Organisation. Diese Benutzer haben derzeit Konten in einem nicht verwalteten Verzeichnis für contoso.co.uk.

Sie möchten nicht zwei separate Verzeichnisse durchsuchen, verwalten, damit Sie nicht verwaltete Verzeichnis für contoso.co.uk in Ihrer vorhandenen IT-verwaltete Verzeichnis für contoso.com zusammenführen.

Externe Übernahme folgt demselben DNS-Überprüfung Prozess wie internen Übernahme.  Differenz gezogen: Benutzer und Dienste erneut in das verwaltete IT-Verzeichnis zugeordnet sind.

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>Was ist die Auswirkung der Durchführung einer externen Übernahme?

Mit einer externen Übernahme wird eine Zuordnung von Benutzern für die Ressourcen erstellt, damit Benutzer für den Zugriff auf Dienste ohne Unterbrechung fortgesetzt werden können. Viele Clientanwendungen, einschließlich RMS für alle Personen, für die Zuordnung von Benutzern für die Ressourcen auch helfen und Benutzer können weiterhin Zugriff auf diese Dienste ohne ändern. Wenn eine Anwendung nicht die Zuordnung von Benutzern für die Ressourcen effektiv behandelt, möglicherweise externe Übernahme explizit blockiert werden, um zu verhindern, dass Benutzer eine schlechte Erfahrung.

#### <a name="directory-takeover-support-by-service"></a>Verzeichnis Übernahme Support vom Dienst

Die folgenden Dienste wird zurzeit Übernahme unterstützt:

- RMS


Die folgenden Dienste werden bald Übernahme unterstützende werden:

- PowerBI

Im folgenden nicht zu empfehlen und zusätzlichen Administratoraktion Migrieren von Benutzerdaten nach einem externen Übernahme erforderlich.

- SharePoint-OneDrive


## <a name="how-to-perform-a-dns-domain-name-takeover"></a>Zum Ausführen einer DNS-Domäne Namen Übernahme

Sie müssen einige Optionen für eine Domäne Validierung (und eine Übernahme führen, wenn Sie möchten):

1.  Azure-Verwaltungsportal

    Eine Übernahme wird durch Hinzufügen einer Domäne Aktionen ausgelöst.  Wenn bereits ein Verzeichnis für die Domäne vorhanden ist, müssen Sie die Option, um eine externe Übernahme durchzuführen.

    Melden Sie sich mit Ihren Anmeldeinformationen Azure-Portal aus.  Navigieren Sie zu Ihrer vorhandenen Verzeichnis und anschließend auf **Domäne hinzufügen**.

2.  Office 365

    Sie können die Optionen auf der Seite [Manage Domains](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) in Office 365 für die Arbeit mit Ihren Domänen und DNS-Einträge verwenden. [Überprüfen Ihrer Domäne in Office 365](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/)finden Sie unter.

3.  Windows PowerShell

    Die folgenden Schritte sind erforderlich, die Validierung mithilfe der Windows PowerShell.

    Schritt    |   Cmdlet verwenden
    ------- | -------------
    Erstellen Sie ein Anmeldeinformationsobjekt | Get-Anmeldeinformationen
    Herstellen einer Verbindung Azure AD mit | Verbinden von MsolService
    Abrufen einer Liste der Domänen   | Get-MsolDomain
    Erstellen Sie eine Herausforderung  | Get-MsolDomainVerificationDns
    Erstellen von DNS-Eintrag   | Aktion auf Ihrem DNS-server
    Vergewissern Sie sich die Herausforderung    | Bestätigen MsolEmailVerifiedDomain

Beispiel:

1. Verbinden mit Azure AD mit den Anmeldeinformationen, die zum Reagieren auf das Self-service-Angebot verwendet wurden: Importieren-Modul MSOnline $msolcred = Get-Credential verbinden-Msolservice-$msolcred von Anmeldeinformationen

2. Erhalten Sie eine Liste der Domänen:

    Get-MsolDomain

3. Führen Sie dann das Cmdlet "Get-MsolDomainVerificationDns", um eine Herausforderung zu erstellen:

    Get-MsolDomainVerificationDns – Domänenname *Ihr_Domänenname* – Modus DnsTxtRecord

    Beispiel:

    Get-MsolDomainVerificationDns – Domänenname "contoso.com" – Modus DnsTxtRecord

4. Kopieren Sie den Wert (Anfrage), der von diesen Befehl zurückgegeben wird.

    Beispiel:

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9

5. Erstellen Sie in Ihrer öffentlichen DNS-Namespace einen DNS Txt-Eintrag mit dem Wert, den Sie im vorherigen Schritt kopiert haben.

    Der Namen für diesen Datensatz ist der Name der übergeordneten Domäne, also wenn Sie diesen Eintrag mithilfe der DNS-Rolle aus Windows Server erstellen, sollten Sie dem Eintrag Name leeren und nur einfügen den Wert in das Textfeld

6. Führen Sie das Cmdlet bestätigen-MsolDomain aus, um zu überprüfen, ob die Herausforderung:

    Bestätigen Sie MsolEmailVerifiedDomain - Domänenname *Ihr_Domänenname*

    Beispiel:

    Bestätigen Sie MsolEmailVerifiedDomain - Domänenname "contoso.com"

Eine erfolgreiche Herausforderung, die Sie auf die Eingabeaufforderung ohne einen Fehler zurück.

## <a name="how-do-i-control-self-service-settings"></a>Wie steuere ich Einstellungen Self-service?

Administratoren müssen zwei Self-service-Steuerelemente auf heute. Sie können steuern:

- Unabhängig davon, ob Benutzer im Verzeichnis per e-Mail an Besprechung teilnehmen können.
- Unabhängig davon, ob Benutzer selbst für Anwendungen und Dienste lizenzieren können.


### <a name="how-can-i-control-these-capabilities"></a>Wie kann ich diese Funktionen steuern?

Ein Administrator kann diese Funktionen verwenden diese Azure AD-Cmdlet "Set-MsolCompanySettings" Parameter konfigurieren:

+ **AllowEmailVerifiedUsers** steuert, ob ein Benutzer teilnehmen an einer nicht verwalteten Verzeichnis oder erstellen kann. Wenn Sie diesen Parameter auf $false festgelegt haben, können keine Benutzer-e-Mail-überprüft Verzeichnis teilnehmen.
+ **AllowAdHocSubscriptions** steuert die Möglichkeit für Benutzer zum Ausführen von Self-service melden Sie sich nach oben. Wenn Sie diesen Parameter auf $false festgelegt haben, können keine Benutzer Self-service-Anmeldung durchführen.


### <a name="how-do-the-controls-work-together"></a>Wie funktionieren die Steuerelemente zusammen?

Diese beiden Parameter können in Verbindung zum Definieren von eine bessere Kontrolle über Self-service Signieren von verwendet werden. Beispielsweise der folgende Befehl wird Benutzerberechtigungen zum Signieren Self-service einrichten, aber nur ausführen, wenn diese Benutzer ein Konto in Azure AD bereits (also Benutzern, die ein überprüft-e-Mail Konto erstellt werden müssten ist nicht möglich Self-service melden Sie sich von):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Das folgende Flussdiagramm erläutert die verschiedenen Kombinationen für diesen Parameter und die resultierende Umstände für das Verzeichnis und Self-service melden Sie sich nach oben.

![][1]

Weitere Informationen und Beispiele für diesen Parameter verwenden finden Sie unter [Festlegen-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx).

## <a name="see-also"></a>Siehe auch

-  [So installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)

-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)

-  [Azure-Cmdlet Bezug](https://msdn.microsoft.com/library/azure/jj554330.aspx)

-  [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
