<properties
    pageTitle="Benutzer bereitgestellt Management für Enterprise-apps in der Vorschau Azure Active Directory | Microsoft Azure"
    description="Informationen Sie zum Verwalten der Benutzer Konto für Enterprise-apps mit Azure Active Directory Preview bereitgestellt"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/12/2016"
    ms.author="asmalser"/>

#<a name="preview-managing-user-account-provisioning-for-enterprise-apps-in-the-new-azure-portal"></a>Vorschau: Verwalten von Benutzerkonto für Enterprise-apps im neuen Azure-Portal bereitgestellt

Dieser Artikel beschreibt, wie Sie mithilfe der [Azure-Portal](https://portal.azure.com) verwalten automatische Benutzerkonto erteilen und entziehen für Applikationen, die es, insbesondere unterstützen, die aus der Kategorie "bereitgestellte" im [Katalog der Azure-Active Directory-Anwendung](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)hinzugefügt wurden. Diese Verwaltungsoption im neuen Azure-Portal gibt es zurzeit in public Preview-Version, und in diesem Artikel werden die neuen Features sowie einige temporäre Einschränkungen, die während der Preview-Zeitraum angeordnet sind. [Was ist in der Vorschau?](active-directory-preview-explainer.md)

Weitere Informationen zum automatischen Benutzer Konto bereitgestellt und deren Funktionsweise finden Sie unter [automatisieren Benutzer bereitgestellt und Deprovisioning zu SaaS Applications with Azure Active Directory](active-directory-saas-app-provisioning.md).

##<a name="finding-your-apps-in-the-new-portal"></a>Suchen Ihre apps im neuen Portal

Ab September 2016 können alle Programme, die für einzelne konfiguriert wurden im Verzeichnis, von einem Directory-Administrator mit Hilfe der [Azure-Active Directory-Anwendung Katalog](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) innerhalb der [Azure klassischen Portal](https://manage.windowsazure.com)anmelden, nun angezeigt und verwaltet werden in das neue Azure-Portal.

Im Abschnitt **Enterprise Applications** des neuen Azure Portals, die über das Menü **Weitere Dienste** im linken Navigationsbereich zugegriffen werden kann, können diese Applikationen gefunden werden. Enterprise-apps sind apps, haben bereitgestellt wurde, die von Benutzern in Ihrer Organisation verwendet werden.

![Enterprise-Applikationen blade][0]

Markieren den Link **Alle Programme** , klicken Sie auf der linken Seite wird eine Liste der alle apps, die konfiguriert wurde haben, einschließlich apps, die aus dem Katalog hinzugefügt wurde. Auswählen einer app lädt das Blade Ressourcen für die app, wo Berichte angezeigt werden können, für die app und eine Vielzahl von Einstellungen verwaltet werden kann.

Benutzerkonto provisioning Einstellungen kann, indem Sie auf der linken Seite **Provisioning** auswählen verwaltet werden.

![Anwendung Ressource blade][1]


##<a name="provisioning-modes"></a>Bereitstellung von Modi

Das **Provisioning** Blade beginnt mit einem Menü **Modus** , in dem angezeigt wird, welche provisioning Modi für eine Enterprise-Anwendung unterstützt werden, und ermöglicht es ihnen, konfiguriert sein. Die verfügbaren Optionen umfassen:

* **Automatische** – diese Option wird angezeigt, wenn Azure AD automatische-API-Bereitstellung und/oder Aufheben der Bereitstellung von Benutzerkonten in dieser Anwendung unterstützt. In diesem Modus auswählen zeigt eine Oberfläche, die führt Administratoren durch Konfigurieren der Verbindung zu der Anwendung Benutzermanagement-API Azure AD, Erstellen von Zuordnungen Konto und Workflows, die definieren, wie Daten zu Benutzerkonten zwischen Azure AD übertragen werden soll, und die app und Verwalten von der Azure AD-Dienst bereitgestellt.

* **Manueller** – diese Option wird angezeigt, wenn Azure AD automatische Bereitstellung von Benutzerkonten in dieser Anwendung nicht unterstützt. Diese Option bedeutet, dass Benutzer Firmendatensätzen in der Anwendung gespeicherte mit einem externen Prozess, basierend auf den Benutzer Management und provisioning Funktionen durch die Anwendung (der SAML Just-in-Time-Bereitstellung enthalten sein kann) bereitgestellten verwaltet werden müssen.


##<a name="configuring-automatic-user-account-provisioning"></a>Konfigurieren von automatischen Benutzer Konto bereitgestellt

Auswählen die Option **Automatische** wird ein angezeigt, die in vier Abschnitte unterteilt ist:

###<a name="admin-credentials"></a>Administrator-Anmeldeberechtigungen
Dies ist die Stelle, an der die Anmeldeinformationen erforderlich für Azure AD zum Verbinden mit der Anwendung Benutzermanagement API eingegeben werden. Die Eingabe erforderlich, abhängig von der Anwendung ab. Finden Sie weitere Informationen zu den Arten von Anmeldeinformationen und die Anforderungen für bestimmte Applikationen im [Konfigurations-Lernprogramm für die bestimmte Anwendung](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning)aus.

Markieren die Schaltfläche **Verbindung testen** , können Sie die Anmeldeinformationen testen, indem Sie Azure AD Verbindungsversuch mit der app der app, die mit den angegebenen Anmeldeinformationen bereitgestellt.

###<a name="mappings"></a>Zuordnungen
Hier wird Administratoren können anzeigen und bearbeiten welche Benutzer Attribute Fluss zwischen Azure AD- und der Zielanwendung auftreten, wenn Benutzerkonten nach der Bereitstellung oder aktualisiert werden.

Es gibt ein vorkonfigurierter Satz von Zuordnungen zwischen Azure AD-Benutzer und jeder SaaS-app-Benutzer-Objekten. Einige apps verwalten andere Arten von Objekten, z. B. Gruppen oder Kontakte. Auswählen eine der folgenden Zuordnungen in die Tabelle zeigt den Zuordnung-Editor auf der rechten Seite, wo diese angezeigt und angepasst werden können.

![Anwendung Ressource blade][2]

Unterstützte Anpassungen bei der ersten Vorschau umfassen:

* Aktivieren und Deaktivieren von Zuordnungen für bestimmte Objekte, wie etwa das Objekt Azure AD-Benutzer der SaaS-app-Benutzer-Objekt.

* Bearbeiten die Attribute aus dem Azure AD-des app Benutzer Objekt Datenfluss. Weitere Informationen zur Zuordnung Attribut finden Sie unter [Grundlegendes zu Attribut Zuordnungstypen](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).

* Welche provisioning Aktionen Azure AD auf der Zielanwendung ausführen soll, also ein neues Feature in der Azure-Portal zu filtern. Anstatt Azure AD komplett-Objekte synchronisieren, können Sie die Aktionen ausgeführt werden. Angenommen, mit der nur bestimmen **Aktualisieren**, nur Updates Azure AD-vorhandene Benutzerkonten in einer Anwendung und erstellen Sie neue nicht. Wählen Sie nur **Erstellen**, wird Azure nur neue Benutzerkonten erstellt, jedoch nicht vorhandene aktualisiert. Mit diesem Feature können Administratoren zum Erstellen von anderen Zuordnungen für das Erstellen von Konten und Aktualisieren von Workflows. Die vollständige Möglichkeit zum Erstellen von mehreren Zuordnungen pro app ist für weiter unten in der Preview-Zeitraum geplant.

###<a name="settings"></a>Einstellungen
In diesem Abschnitt können Administratoren zu starten und beenden das Dienst für die ausgewählte Anwendung bereitgestellt Azure AD als auch optional den provisioning Cache löschen und starten Sie den Dienst an.

Wenn provisioning zum ersten Mal für eine Anwendung aktiviert wird, aktivieren Sie durch Ändern der **Status bereitgestellt** , **Klicken Sie auf**den Dienst. Dies bewirkt, dass das Azure AD Dienst bereitgestellt einer anfänglichen synchronisieren, ausführen, in dem sie die Benutzer im Abschnitt **Benutzer und Gruppen** zugewiesen liest, die Zielanwendung für diese Abfragen, und klicken Sie dann führt die provisioning Aktionen, die im Abschnitt Azure AD- **Zuordnungen** definiert. Während dieses Vorgangs werden provisioning Dienst zwischengespeicherte Daten über welche Benutzerkonten verwaltet wird, damit nicht verwaltete Konten innerhalb der zielanwendungen, die nie im Bereich für die Zuordnung wurden für die Vorgänge Aufheben der Bereitstellung auswirken werden nicht gespeichert. Nach der anfänglichen synchronisieren synchronisiert der Bereitstellung Dienst automatisch Benutzer- und Gruppenkonten in einem Intervall 10 Minuten.

Ändern den **Status Provisioning** zu **Deaktivieren** hält einfach den Bereitstellung Dienst aus. In diesem Zustand Azure nicht erstellen, aktualisiert, oder entfernen Sie alle Benutzer oder die Gruppierung von Objekten in der app. Ändern den Status zu auf bewirkt, dass den Dienst auswählen, wo unterbrochen.

Wenn Sie das Kontrollkästchen **Deaktivieren des aktuellen Status, und starten Sie die Synchronisierung erneut** , und speichern stoppt provisioning Dienst speichert die zwischengespeicherten Daten über welche Azure AD-Konten verwaltet, startet die Dienste neu und führt die ursprüngliche Synchronisierung erneut. Mit dieser Option können Administratoren das provisioning Bereitstellung erneut starten.

###<a name="synchronization-details"></a>Synchronisierungsdetails
Dieser Abschnitt enthält die Erweiterung-Details zu dem Vorgang des provisioning Service, einschließlich der vor- und Nachnamen, wie oft, die der Bereitstellung Dienst ausgeführt wurde vor der Anwendung, und wie viele Benutzer- und Gruppenkonten verwaltet werden.

Links zu werden, vorausgesetzt, auf den **Bericht zu Provisioning Aktivitäten**, die bietet ein Protokoll aller Benutzer und Gruppen erstellt wurden, aktualisierte und entfernte zwischen Azure AD und das Ziel Anwendung erstellt, und zum **Provisioning Fehlerbericht** mehr bietet detaillierte Fehlermeldungen für Benutzer- und Gruppenkonten, die nicht gelesen werden, erstellt, aktualisiert oder entfernt. 

[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG
