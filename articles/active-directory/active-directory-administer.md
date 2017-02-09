<properties
    pageTitle="Verwalten Ihrer Azure AD-Verzeichnis | Microsoft Azure"
    description="Erläutert, einem Azure AD-Mandanten ist, und zum Verwalten von Azure über Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    writer="markvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="markvi"/>

# <a name="administer-your-azure-ad-directory"></a>Verwalten Sie Ihrer Azure AD-Verzeichnis

## <a name="what-is-an-azure-ad-tenant"></a>Was ist eine Azure AD-Mandanten?

Die physische Arbeitsplatz kann der Mandanten Word als Gruppe oder Unternehmen, die von einem Gebäude belegt definiert werden. Beispielsweise kann Ihre Organisation Office Leerzeichen in einem Gebäude besitzen. Dieser Baustein möglicherweise in einer Straße mit anderen Organisationen. Ihre Organisation wird einen dieser Baustein-Mandanten angesehen. Dieser Baustein ist eine Anlage Ihrer Organisation und bietet Sicherheit und stellt sicher, dass Sie Business sicheres durchführen zu können. Es ist auch von den anderen Unternehmen in Ihrer Straße getrennt. Dies stellt sicher, dass es sich bei Ihrer Organisation und die Anlagen dort von anderen Organisationen isoliert sind.

Am Arbeitsplatz Cloud-fähige kann ein Mandanten als Client oder Organisation, die besitzt und verwaltet die Cloud-Dienst eine bestimmte Instanz definiert werden. Mit der Identitätsplattform von Microsoft Azure bereitgestellt wird ein Mandanten einfach eine dedizierte Instanz von Azure Active Directory (Azure AD), die Ihre Organisation erhält und zugegriffen werden, wenn es für ein Microsoft-Cloud-Dienst wie Azure oder Office 365 anmeldet.

Jedes Azure AD-Verzeichnis ist distinct und getrennt von anderen Azure AD-Verzeichnisse durchsuchen. Wie eine corporate Bürogebäude eine sichere Wirtschaftsguts für nur Ihre Organisation spezifisch ist, wurde ein Verzeichnis Azure AD-auch konzipiert eine sichere Anlage zur Verwendung von nur in Ihrer Organisation. Die Azure AD-Architektur isoliert Kundeninformationen Daten und Identität von gemeinsame Integration. Dies bedeutet, dass Benutzer und Administratoren von einem Azure AD-Verzeichnis versehentlich oder absichtlich Daten in ein anderes Verzeichnis zugreifen können.

![Verwalten von Azure Active Directory][1]

## <a name="how-can-i-get-an-azure-ad-directory"></a>Wie erhalte ich ein Verzeichnis Azure AD-?

Azure AD bietet das Herzstück Verzeichnis und Identität Verwaltungsfunktionen hinter die meisten Microsoft Cloud-Diensten, einschließlich:

- Azure
- Microsoft Office 365
- Microsoft Dynamics CRM Online
- Microsoft Intune

Ein Azure AD-Verzeichnis erhalten, wenn Sie nach jedem der folgenden Microsoft-Cloud-Dienste registrieren. Sie können weitere Verzeichnisse erstellen, je nach Bedarf. Sie möglicherweise beispielsweise Verwalten Ihrer erste Verzeichnis wie ein Verzeichnis Herstellung und erstellen Sie dann auf ein anderes Verzeichnis zum Testen oder für das staging.

> [AZURE.NOTE]
> Nachdem Sie sich für den ersten Dienst haben registriert, wird empfohlen, dass Sie das gleiche Administratorkonto Ihrer Organisation zugeordnet wird verwenden, wenn Sie für andere Microsoft-Cloud-Dienste registrieren.

Für ein Microsoft-Cloud-Dienst registrieren zum ersten Mal werden Sie aufgefordert, Details zu Ihrer Organisation und Ihrer Organisation Internet Domain Name Registration wird bereit. Diese Informationen werden dann zum Erstellen eines neuen verwendet Azure AD-Directory-Instanz für Ihre Organisation. Dasselbe Verzeichnis wird verwendet, um die Zeichen in Versuche authentifizieren, wenn Sie mehrere Microsoft Cloud-Dienste abonnieren.

Zusätzliche Dienste nutzen vollständig alle vorhandenen Benutzer Konten, Richtlinien, Einstellungen oder lokalen Verzeichnisintegration, die Sie zur Verbesserung der Effizienz zwischen Identität Infrastruktur lokalen und Azure AD-Ihres Unternehmens konfigurieren.

Beispielsweise, wenn Sie ursprünglich für ein Abonnement Microsoft Intune registriert und die erforderlichen Schritte zum Ihrem lokalen Active Directory mit Ihrem Verzeichnis Azure AD-Weitere Integration von durch die Bereitstellung von Verzeichnissynchronisation und/oder einzelnen anmelden Servern abgeschlossen, können Sie für einen weiteren Microsoft-Cloud-Dienst wie Office 365 anmelden die auch dieselben Directory Integration Vorteile nutzen können, die Sie jetzt mit Microsoft Intune verwenden.

Weitere Informationen zur Integration von Ihrem lokalen Verzeichnis mit Azure AD finden Sie unter [Verzeichnisintegration](active-directory-aadconnect.md).

### <a name="associate-an-azure-ad-directory-with-a-new-azure-subscription"></a>Zuordnen einer Azure AD-Verzeichnis mit einem neuen Azure-Abonnement

Sie können ein neues Azure-Abonnement mit demselben Verzeichnis zuordnen, die für ein Office 365 oder Microsoft Intune Abonnement anmelden authentifiziert. Melden Sie bei der mit Ihrem Konto geschäftlichen oder schulnotizbücher Azure-Verwaltungsportal an. Verwaltungsportal gibt eine Meldung, dass es zum Suchen von Abonnements für das Konto nicht möglich war. SELECT **Melden Sie sich oben für Azure**und Ihrem Verzeichnis verfügbar für die Verwaltung innerhalb des Portals. Weitere Informationen finden Sie unter [Verwalten von Verzeichnis für Ihr Office 365-Abonnement in Azure](active-directory-how-subscriptions-associated-directory.md#manage-the-directory-for-your-office-365-subscription-in-azure).

Ein Video zu Allgemeine Fragen zur Verwendung von Azure AD-finden Sie unter [Azure Active Directory - gemeinsamer Anmeldung, Anmeldung und die Verwendung von Fragen](http://channel9.msdn.com/Series/Windows-Azure-Active-Directory/WAADCommonSignupsigninquestions).

### <a name="create-an-azure-ad-directory-by-signing-up-for-a-microsoft-cloud-service-as-an-organization"></a>Erstellen Sie ein Verzeichnis mit Azure AD-durch Anmelden für ein Microsoft-Cloud-Dienst als einer Organisation

Wenn Sie noch nicht über ein Abonnement für ein Microsoft-Cloud-Dienst besitzen, verwenden Sie eine der nachstehenden Links zu registrieren. Der Vorgang, für den ersten Dienst anmelden erstellt automatisch ein Azure AD-Verzeichnis.

- [Microsoft Azure](https://account.windowsazure.com/organization)
- [Office 365](http://products.office.com/business/compare-office-365-for-business-plans/)
- [Microsoft Intune](https://account.manage.microsoft.com/Signup/MainSignUp.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&ali=1)

### <a name="manage-an-azure-provisioned-default-directory"></a>Verwalten einer Azure bereitgestellt Standard-Verzeichnis

Heute, wird ein Verzeichnis automatisch erstellt, wenn für Azure registrieren und Ihr Abonnement dieses Verzeichnis zugeordnet ist. Aber wenn Sie ursprünglich für Azure vor Oktober 2013 haben angemeldet, ein Verzeichnis wurde nicht automatisch erstellt. In diesem Fall Azure "aufgefüllt" für Ihr Konto möglicherweise durch ein Standardverzeichnis für ihn bereitgestellt. Ihr Abonnement wurde, klicken Sie dann die standardmäßigen Verzeichnis zugeordnet.

Abgleichen der Verzeichnisse wurde in Oktober 2013 als Teil einer generellen in das Datenmodell Sicherheit für Azure fertig. Hilft organisationsidentität Features für alle Azure anbieten, und damit ist sichergestellt, dass alle Azure Ressourcen im Kontext eines Benutzers im Verzeichnis zugegriffen werden. Sie können keine Azure ohne ein Verzeichnis zu verwenden. Um zu erreichen, dass jeder Benutzer, der vor dem 7: Juli 2013 registriert wurde, aber nicht über ein Verzeichnis verfügen hatte eine erstellt haben. Wenn Sie bereits ein Verzeichnis erstellt haben, wurde Ihr Abonnement dieses Verzeichnis zugeordnet.

Es gibt keine Kosten für die Verwendung von Azure AD aus. Das Verzeichnis ist ein kostenloses Ressourcen. Es gibt eine weitere Azure Active Directory Premium Ebene, die separat lizenziert ist, und bietet zusätzliche Funktionen, wie Unternehmen branding und Self-service das Zurücksetzen von Kennwörtern an.

Zum Ändern des Anzeigenamens Ihres Verzeichnisses klicken Sie auf das Verzeichnis im Portal aus, und klicken Sie auf **Konfigurieren**. Wie weiter unten in diesem Thema erläutert wird, können Sie ein neues Verzeichnis hinzufügen oder löschen ein Verzeichnis aus, die Sie nicht mehr benötigen. Wenn Sie Ihr Abonnement mit einem anderen Verzeichnis zuordnen möchten, klicken Sie auf die Erweiterung **Einstellungen** im linken Navigationsbereich, und klicken Sie am unteren Rand der Seite **Abonnements** , klicken Sie auf **Verzeichnis bearbeiten**. Sie können auch eine benutzerdefinierte Domäne mit einem DNS-Namen, die Sie sich registriert haben anstelle der standardmäßigen erstellen *. onmicrosoft.com-Domäne, die möglicherweise besser mit einem Dienst wie SharePoint Online.

## <a name="how-can-i-manage-directory-data"></a>Wie kann ich Directory Daten verwalten

Als Administrator von einem oder mehreren Microsoft Cloud-Dienstabonnements können entweder die Azure-Verwaltungsportal, das Intune Microsoft-Konto-Portal oder im Office 365 Admin Center Sie zum Verwalten Ihrer Organisationen Directory Daten. Sie können auch herunterladen und Ausführen von [Microsoft Azure Active Directory-Modul für Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) -Cmdlets Sie in Azure AD gespeicherte Daten verwalten können.

Von jedem der folgenden communityportalen (oder Cmdlets) können Sie folgende Aktionen ausführen:

- Erstellen und Verwalten von Benutzer- und Gruppenkonten
- Verwalten Sie verwandte Cloud-Dienste, die, denen Ihre Organisation abonniert,
- Einrichten von lokalen Integration in Ihrem Verzeichnisdienst

Verwaltungsportal Azure, Office 365 Admin Center, Microsoft Intune Kontoportal und Azure AD-Cmdlets alle lesen, und schreiben eine einzelne freigegebene Instanz des Azure AD, die mit dem Directory Ihres Unternehmens, verknüpft ist, wie in der folgenden Abbildung gezeigt. Auf diese Weise Verhalten sich communityportalen (oder Cmdlets) wie eine Front-End-Benutzeroberfläche, die extrahieren und/oder Ihre Verzeichnisdaten ändern.

![][2]

Diese Konto-Portals und der zugehörigen Azure AD PowerShell-Cmdlets verwendet, um Benutzer und Gruppen verwalten sind auf der Azure AD-Plattform integriert.

Wenn Sie eine Änderung mit Organisationen Daten verhindert des Portals (oder Cmdlets) während im Kontext eines der folgenden Dienste angemeldet haben, werden die Änderung auch in der anderen Portals das nächste Mal angezeigt, das Sie unter Kontext diesen Dienst anmelden, da diese Daten über die Microsoft-Cloud-Dienste freigegeben werden, die Sie abonniert haben.
Beispielsweise, wenn Sie Office 365 Admin Center um zu blockierenden ein Benutzers aus der Anmeldung verwendet haben, blockiert diese Aktion den Benutzer bei allen anderen Diensten, die Ihre Organisation derzeit abonniert Anmeldung verhindern. Wenn Sie wurden, ziehen Sie die gleichen Benutzerkonto im Kontext des Portals Intune Microsoft-Konto sehen Sie sich, dass der Benutzer gesperrt ist.

## <a name="how-can-i-add-and-manage-multiple-directories"></a>Wie kann ich hinzufügen und Verwalten von mehreren Verzeichnissen?

Sie können einem Azure AD-Verzeichnis im Azure-Verwaltungsportal hinzufügen. Wählen Sie die **Active Directory** -Erweiterung auf der linken Seite aus, und klicken Sie auf **Hinzufügen**.

Sie können jedes Verzeichnis als vollständig unabhängige Ressource verwalten: jedes Verzeichnis ist ein Peer, voll ausgestatteten und logisch unabhängig von anderen Verzeichnisse durchsuchen, die Sie verwalten; Es gibt keine hierarchische Beziehung zwischen Verzeichnissen aus. Diese Unabhängigkeit zwischen Verzeichnissen beinhaltet Ressource Unabhängigkeit, administrative Unabhängigkeit und Synchronisierung Unabhängigkeit.

- **Ressource Unabhängigkeit**. Wenn Sie erstellen oder einer Ressource in einem Verzeichnis löschen, hat es keine Auswirkung auf eine beliebige Ressource in einem anderen Verzeichnis, mit Ausnahme von externen Benutzern, die nachfolgend beschriebenen teilweise. Wenn Sie eine benutzerdefinierte Domäne "contoso.com" mit einem Verzeichnis verwenden, können sie mit einem beliebigen anderen Verzeichnis verwendet werden.
- **Administrative Unabhängigkeit**.  Wenn ein Benutzer ohne Administratorrechte des Verzeichnisses 'Contoso' erstellt Testverzeichnis 'Test' anschließend:
    - Das Directory-Synchronisierungstool, für die Synchronisierung von Daten mit einer einzelnen Active Directory-Struktur.
    - Die Administratoren von Verzeichnis 'Contoso' haben keine direkten Administratorberechtigungen in Verzeichnis 'Test' aus, es sei denn, ein Administrator von 'Test' speziell ihnen diese Berechtigungen erteilt. Administratoren von 'Contoso' können Zugriff auf Verzeichnis 'Test' gemäß ihren Steuerelement das Benutzerkonto, die "Test." erstellt steuern.

    Und wenn Sie ändern (hinzufügen oder entfernen) einer Administratorrolle für einen Benutzer im Verzeichnis ein, die Änderung hat keine Auswirkung auf eine beliebige Administratorrolle möglicherweise diesen Benutzer in ein anderes Verzeichnis.


- **Synchronisierung Unabhängigkeit**. Sie können jede Azure AD unabhängig voneinander zum Abrufen von Daten aus einer einzelnen Instanz des entweder synchronisiert konfigurieren:
    - Das Directory-Synchronisierungstool zum Synchronisieren von Daten mit einer einzelnen Active Directory-Struktur
    - Azure Active Directory Connector für Forefront Identität Manager zum Synchronisieren von Daten mit einem oder mehreren lokalen Gesamtstrukturen und/oder nicht AD-Datenquellen.

Beachten Sie auch, dass Ihre Verzeichnisse im Gegensatz zu anderen Azure Ressourcen, nicht untergeordneten Ressourcen eines Azure-Abonnements sind. Also wenn Sie Abbrechen oder das Azure-Abonnement zulassen abläuft, können Sie weiterhin Ihre Directory Daten mithilfe von Azure AD-PowerShell, die Azure Graph-API oder andere Schnittstellen wie Office 365 Admin Center zugreifen. Sie können auch ein anderes Abonnement Verzeichnis zuordnen.

## <a name="how-can-i-delete-an-azure-ad-directory"></a>Wie kann ich ein Verzeichnis Azure AD-löschen?
Ein globaler Administrator kann eine Azure AD-Verzeichnis aus dem Portal gelöscht werden. Wenn ein Verzeichnis gelöscht wird, werden alle Ressourcen, die im Verzeichnis enthaltenen ebenfalls gelöscht. Daher sollten Sie sicher, dass das Verzeichnis nicht benötigen, bevor Sie es löschen.

> [AZURE.NOTE]
> Wenn der Benutzer mit einer geschäftlichen oder schulnotizbücher Konto angemeldet ist, muss der Benutzer nicht versuchen, vertretene Privat-Ordner löschen. Wenn der Benutzer als angemeldet ist beispielsweise joe@contoso.onmicrosoft.com, diesen Benutzer das Verzeichnis mit contoso.onmicrosoft.com als deren Standarddomäne muss kann nicht gelöscht werden.

### <a name="conditions-that-must-be-met-to-delete-an-azure-ad-directory"></a>Bedingung, die erfüllt sein müssen, um ein Verzeichnis Azure AD-löschen

Azure AD erfordert, dass bestimmte Bedingungen erfüllt sind, um ein Verzeichnis zu löschen. Dies reduziert Risiko, das Löschen eines Verzeichnisses Benutzer oder Anwendungen, wie etwa die Möglichkeit von Benutzern, melden Sie sich bei Office 365 oder Zugriff auf Ressourcen in Azure beeinträchtigen würden. Beispielsweise wenn ein Verzeichnis für ein Abonnement unbeabsichtigt gelöscht geworden ist, konnte dann Benutzer nicht die Azure Ressourcen für diese Abonnement zugreifen.

Die folgenden Bedingungen aktiviert sind:

- Der einzige Benutzer im Verzeichnis ist globaler Administrator, wer das Verzeichnis zu löschen. Alle anderen Benutzer müssen gelöscht werden, bevor Verzeichnis gelöscht werden kann. Wenn Benutzer aus lokalen synchronisiert werden, klicken Sie dann synchronisieren ausgeschaltet werden müssen, und Benutzer im Verzeichnis Cloud mithilfe der Verwaltungsportal oder Azure-Modul für Windows PowerShell gelöscht werden müssen. Es gibt keine Anforderung So löschen Sie Gruppen oder Kontakte, z. B. Kontakte aus der Office 365-Verwaltungskonsole hinzugefügt.
- Es können keine Programme im Verzeichnis vorhanden sein. Bevor Verzeichnis gelöscht werden kann, müssen alle Anwendungen gelöscht werden.
- Es kann keine Abonnements für alle Microsoft Online Services, wie etwa Microsoft Azure, Office 365 oder Azure AD Premium mit dem Verzeichnis verknüpft. Beispielsweise, wenn ein Standardverzeichnis in Azure für Sie erstellt wurde, Löschen nicht dieses Verzeichnis, wenn Ihr Abonnement Azure immer noch auf dieses Verzeichnis für die Authentifizierung greift. Auf ähnliche Weise können Sie ein Verzeichnis löschen, wenn ein anderer Benutzer ein Abonnement verbunden ist. Wenn Sie Ihr Abonnement mit einem anderen Verzeichnis zuordnen möchten, melden Sie sich bei der Azure-Verwaltungsportal, und klicken Sie im linken Navigationsbereich auf **Einstellungen** . Klicken Sie dann auf das Ende der Seite **Abonnements** auf **Verzeichnis bearbeiten**. Weitere Informationen zur Azure-Abonnements finden Sie unter [wie Azure-Abonnements Azure AD zugeordnet sind](active-directory-how-subscriptions-associated-directory.md).

> [AZURE.NOTE]
> Wenn der Benutzer mit einem geschäftlichen oder schulnotizbücher-Konto angemeldet ist, muss der Benutzer nicht versuchen, vertretene Privat-Ordner löschen. Wenn der Benutzer als angemeldet ist beispielsweise joe@contoso.onmicrosoft.com, diesen Benutzer das Verzeichnis mit contoso.onmicrosoft.com als deren Standarddomäne muss kann nicht gelöscht werden.

- Keine kombinierte Authentifizierung Anbieter können mit dem Verzeichnis verknüpft werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Azure AD-Forum](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD)
- [Forum Azure kombinierte Authentifizierung](https://social.msdn.microsoft.com/Forums/home?forum=windowsazureactiveauthentication)
- [Stackoverflow](http://stackoverflow.com/questions/tagged/azure)
- [Registrieren Sie sich bei Azure als einer Organisation](sign-up-organization.md)
- [Verwalten von Azure Active Directory mithilfe von Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx)
- [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md)

<!--Image references-->
[1]: ./media/active-directory-administer/aad_portals.png
[2]: ./media/active-directory-administer/azure_tenants.png
