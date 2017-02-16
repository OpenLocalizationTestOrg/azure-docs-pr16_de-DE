<properties
    pageTitle="Azure Active Directory-Gerät Registrierung Übersicht | Microsoft Azure"
    description="ist die Grundlage für bedingten Zugriff Gerät-basierten Szenarien. Wenn ein Gerät registriert ist, Vorschriften Azure Active Directory-Gerät Registration wird das Gerät mit einer Identität, die das Gerät authentifiziert, wenn der Benutzer anmeldet verwendet wird."
    services="active-directory"
    keywords="Gerät Registrierung, aktivieren Gerät Registrierung, Gerät Registrierung und MDM"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="Markvi"/>

# <a name="get-started-with-azure-active-directory-device-registration"></a>Erste Schritte mit Azure Active Directory-Gerät Registrierung

Azure Active Directory-Gerät-Registrierung ist die Grundlage für die bedingte Access-basierten Gerät Szenarien. Wenn ein Gerät registriert ist, stellt Azure Active Directory-Gerät Registration wird das Gerät mit einer Identität, die verwendet wird, um das Gerät authentifiziert, wenn der Benutzer anmeldet. Das Gerät authentifizierten und die Attribute des Geräts, können dann bedingte Richtlinien für Applikationen erzwingen, die in der Cloud und lokal gehostet werden verwendet werden.

In Kombination mit einem mobilen Gerät management(MDM) Lösung wie Microsoft Intune werden die Gerätattribute in Active Directory Azure mit weiteren Informationen über das Gerät aktualisiert. So können Sie bedingte Access Regeln erstellen, die Access-Geräten entsprechen Ihren Standards für Sicherheit und Kompatibilität erzwingen. Weitere Informationen zum Registrieren von Geräten in Microsoft Intune finden Sie unter [Registrieren Geräten für die Verwaltung von Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).

## <a name="scenarios-enabled-by-azure-active-directory-device-registration"></a>Aktivierte Azure Active Directory-Gerät Registrierung Szenarien

Azure Active Directory-Gerät-Registrierung umfasst Unterstützung für iOS, Android und Windows Geräte. Die einzelnen Szenarien, die Registrierung Azure AD-Gerät nutzen möglicherweise weitere spezifische Anforderungen und Plattform unterstützt. Diese Szenarios werden wie folgt aus:

- **Bedingte Zugriff auf Anwendungen, die lokale gehosteten sind**: können registrierte Geräte mit Access-Richtlinien für Applikationen, die so konfiguriert sind, dass AD FS mit Windows Server 2012 R2 verwenden. Weitere Informationen zum Einrichten von bedingten Zugriff für lokale finden Sie unter [Einrichten von lokalen bedingten Zugriff Azure Active Directory-Gerät Registrierung verwenden](active-directory-conditional-access-on-premises-setup.md).

- **Bedingte Zugriff für Office 365-Clientanwendungen mit einem Microsoft-Intune** : Bereitstellung von IT-Administratoren können bedingte Gerät-Richtlinien zum Sichern Ihres Unternehmens Ressourcen, während gleichzeitig Information Worker auf kompatible Geräte Zugriff auf die Dienste zulässt. Weitere Informationen finden Sie unter [Bedingte Gerät-Richtlinien für Office 365-Dienste](active-directory-conditional-access-device-policies.md).

##<a name="setting-up-azure-active-directory-device-registration"></a>Einrichten von Azure Active Directory-Gerät Registrierung

Sie müssen Azure AD-Gerät Registrierung Azure-Portal zu aktivieren, damit mobile Geräte Dienst ermitteln können, indem bekannten DNS-Einträge gesucht. Sie müssen Ihr Unternehmen DNS so konfigurieren, dass Windows 10, Windows 8.1, Windows 7, Android und iOS-Geräte ermitteln und den Dienst verwenden können.
Sie können angezeigt und registrierten Geräte mithilfe der Administratorportal in Azure Active Directory aktivieren/deaktivieren.

>[AZURE.NOTE]
 Neueste Anweisungen zum Einrichten von automatischen Gerät Registrierung finden Sie unter, [zum Einrichten der automatischen Registrierung von Windows-Domäne beigetreten Geräte mit Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

### <a name="enable-azure-active-directory-device-registration-service"></a>Azure-Active Directory-Gerät Registration Service aktivieren

1. Melden Sie sich beim Microsoft Azure-Portal als Administrator an.
2. Wählen Sie im linken Bereich aus **Active Directory**.
3. Wählen Sie auf der Registerkarte **Verzeichnis** aus Ihrem Verzeichnis.
4. Wählen Sie die Registerkarte **Konfigurieren** .
5. Führen Sie einen Bildlauf zum Abschnitt **Geräte**bezeichnet.
6. Wählen Sie **Alle** für **Benutzer möglicherweise Arbeitsplatz Verknüpfung Geräte**.
7. Wählen Sie die maximale Anzahl von Geräten, die Sie pro Benutzer autorisieren möchten.

>[AZURE.NOTE]
>Registrierung mit Microsoft Intune oder Verwaltung mobiler Geräte für Office 365 erfordert Arbeitsplatz teilnehmen. Wenn Sie eine der folgenden Dienste konfiguriert haben, alle aktiviert ist, und die Schaltfläche ist deaktiviert.

Standardmäßig ist die Authentifizierung mit zwei Faktoren für den Dienst nicht aktiviert. Authentifizierung mit zwei Faktoren wird jedoch empfohlen, wenn Sie ein Gerät zu registrieren.

- Vor dem Anfordern einer zweifaktorielle Varianzanalyse Authentifizierung für diesen Dienst, müssen Sie konfigurieren ein Anbieters Authentifizierung mit zwei Faktoren in Azure Active Directory und konfigurieren die Benutzerkonten für kombinierte Authentifizierung finden Sie finden Sie unter [Hinzufügen von mehrstufige Authentifizierung zu Azure Active Directory](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)

- Wenn Sie mit Windows Server 2012 R2 AD FS verwenden, müssen Sie ein Modul Authentifizierung mit zwei Faktoren in AD FS konfigurieren, finden Sie unter [Verwenden von kombinierte Authentifizierung mit Active Directory Federation Services](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="configure-azure-active-directory-device-registration-discovery"></a>Konfigurieren der Suche Azure Active Directory-Gerät Registrierung
Windows 7 und Windows 8.1-Geräte werden das Gerät Registration-Service durch Kombinieren der aktuelle Kontoname mit bekannten Gerät Registrierung Servernamen ermitteln.

Sie müssen einen DNS-CNAME-Eintrag erstellen, der auf dem Dienst Azure Active Directory-Gerät Registrierung zugeordnet A-Eintrag verweist. Der CNAME-Eintrag muss die bekannten Präfix Enterpriseregistration gefolgt von Benutzerkonten in Ihrer Organisation verwendeten Benutzerprinzipalnamen-Suffix verwenden. Wenn Ihre Organisation mehrere UPN-Suffixe verwendet, müssen mehrere CNAME-Einträge in DNS erstellt werden.

Beispielsweise, wenn Sie zwei Benutzerprinzipalnamen-Suffixe in Ihrer Organisation mit dem Namen verwenden @contoso.com und @region.contoso.com, erstellen Sie die folgenden DNS-Einträge.

| Eintrag                                     | Typ  | Adresse                            |
|-------------------------------------------|-------|------------------------------------|
| enterpriseregistration.contoso.com        | CNAME | enterpriseregistration.Windows.NET |
| enterpriseregistration.Region.contoso.com | CNAME | enterpriseregistration.Windows.NET |

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a>Anzeigen und Verwalten von Geräteobjekte in Azure Active Directory
1. Im Portal Azure-Administrator können Sie anzeigen, blockieren und Aufheben der Blockierung Geräte. Ein Gerät, das gesperrt ist haben nicht mehr Zugriff auf Anwendungen, die konfiguriert werden, um nur registrierte Geräten ermöglichen.
2. Melden Sie sich an den Microsoft Azure-Portal als Administrator.
3. Wählen Sie im linken Bereich aus **Active Directory**.
4. Wählen Sie Ihr Verzeichnis aus.
5. Wählen Sie die Registerkarte **Benutzer** . Wählen Sie dann auf Benutzer aktivierten Geräte anzeigen
6. Wählen Sie die Registerkarte **Geräte** .
7. Wählen Sie **Registriert Geräte** aus dem Dropdownmenü aus.
8. Im folgenden finden Sie anzeigen können, blockieren oder Aufheben der Blockierung der Benutzer registrierte Geräte.

## <a name="additional-topics"></a>Weitere Themen

Sie können Ihre Windows 7 und Windows 8.1 Domäne beigetreten Geräte mit Azure AD-Gerät Registrierung registrieren. In den folgenden Themen finden weitere Informationen zu den erforderlichen Komponenten und die erforderlichen Schritte zum Konfigurieren der Registrierung Gerät auf Windows 7 und Windows 8.1-Geräten.

- [Automatische Gerät Registrierung mit für Domänenverbund Geräte von Windows Azure-Active Directory](active-directory-conditional-access-automatic-device-registration.md)
- [Konfigurieren der automatischen Gerät Registrierung für Windows 7-Domäne beitreten-Geräte](active-directory-conditional-access-automatic-device-registration-windows7.md)
- [Konfigurieren der automatischen Gerät Registrierung für Windows 8.1-Domäne beitreten Geräte](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Automatische Gerät Registrierung mit Azure Active Directory für Windows 10 Domänenverbund Geräte](active-directory-azureadjoin-devices-group-policy.md)
