<properties
    pageTitle="Anpassen von Attribut Zuordnungen | Microsoft Azure"
    description="Erfahren Sie, welche Attribut Zuordnungen für SaaS-apps in Azure Active Directory sind, wie diese Ihre geschäftliche Anforderungen Adresse ändern zu können."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="customizing-attribute-mappings"></a>Anpassen von Zuordnungen Attribut


Microsoft Azure AD bietet Unterstützung für Benutzer von Drittanbietern SaaS Clientanwendungen wie Vertrieb, Google Apps und andere bereitgestellt. Wenn Sie die Benutzer bereitgestellt für einen dritten SaaS Anwendung aktiviert haben, steuert der Azure-Verwaltungsportal zugehörigen Attributwerte in Form einer Konfiguration als "Attribut Zuordnung" bezeichnet.

Es gibt ein vorkonfigurierter Satz von Attribut Zuordnungen zwischen Azure AD-Benutzer und jeder SaaS-app-Benutzer-Objekten. Einige apps verwalten andere Arten von Objekten, z. B. Gruppen oder Kontakte. <br> 
Sie können die standardmäßigen Attribut Zuordnungen entsprechend Ihren geschäftlichen Anforderungen anpassen. Dies bedeutet, Sie können ändern oder löschen vorhandene Attribut Zuordnungen oder erstellen Sie neue Attribut Zuordnungen.

Im Portal Azure AD-können Sie dieses Feature, indem Sie in der Symbolleiste einer Anwendung SaaS Attribute auf zugreifen.

> [AZURE.NOTE] Der Link **Attributen** ist nur verfügbar, wenn Benutzer bereitgestellt für eine Anwendung SaaS aktiviert ist. 


![Vertrieb][1] 


Wenn Sie Attribute in der Symbolleiste der Liste der aktuellen Zuordnungen klicken Sie auf, die für eine Anwendung SaaS konfiguriert sind.

Das folgende Bildschirmabbild zeigt ein Beispiel für diese:



![Vertrieb][2]  


Im oben genannten Beispiel können Sie sehen, dass das Attribut **Vorname** eines verwalteten Objekts in Vertrieb mit dem Wert **Vorname** , der die verknüpfte ausgefüllt wird Azure AD-Objekt.

Wenn Sie Ihre Attribut Zuordnungen anpassen möchten oder wenn Sie zurückkehren möchten diese angepasste wieder auf die Konfiguration, können Sie hierzu auf die zugehörige Schaltfläche in der Symbolleiste auf das Ende einer Anwendung.


![Vertrieb][3]  


Es gibt Attribut Zuordnungen, die von einer Anwendung SaaS ordnungsgemäß erforderlich sind. In der Attributetabelle haben im zugehörigen Attribut Zuordnungen **Ja** als Wert für das Attribut **erforderlich** . Wenn eine Zuordnung Attribut erforderlich ist, können nicht Sie es löschen. In diesem Fall ist **Löschen** Feature nicht verfügbar.

Wenn Sie eine vorhandene Attribut Zuordnung ändern möchten, wählen Sie nur die Zuordnung, und klicken Sie dann auf **Bearbeiten**. Dadurch wird eine Dialogseite mit dem Sie die ausgewählte Attribut Zuordnung ändern kann.


![Attribut Zuordnung bearbeiten][4]  



## <a name="understanding-attribute-mapping-types"></a>Grundlegendes zu Attribut Zuordnen von Datentypen


Klicken Sie mit dem Attribut Zuordnungen Steuerelement wie Attribute in eine dritte ausgefüllt werden SaaS Anwendung party. Es gibt vier verschiedene Zuordnungsarten unterstützt:

- **Direkte** – das Zielattribut wird mit dem Wert, der ein Attribut für das verknüpfte Objekt in Azure AD aufgefüllt.


- **Konstante** – das Zielattribut wird mit einer bestimmten Zeichenfolge aufgefüllt, die Sie angegeben haben.


- **Ausdrucks** - die Zielattribut wird basierend auf das Ergebnis eines Ausdrucks Skript ähnelt ausgefüllt. Weitere Informationen hierzu finden Sie unter [Schreiben von Ausdrücken für Attribut Zuordnungen in Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).


- **Keine** - das Zielattribut ist links unverändert. Jedoch ist das Zielattribut jemals leer, wird es mit dem Standardwert gefüllt, den Sie angeben.



Zusätzlich zu dieser vier grundlegende Attribut Zuordnung Typen unterstützen benutzerdefinierte Attribut Zuordnungen des Konzepts der Zuordnung einer **Standard** -Wert. Die standardmäßigen Wert Zuordnung ist sichergestellt, dass eine Zielattribut mit einem Wert vorhanden ist, ist es weder um ein Wert in Azure AD- noch auf das Objekt Target.

Microsoft Azure AD enthält eine sehr effiziente Implementierung eines Prozesses Synchronisierung. In einer Umgebung initialisierte werden nur Objekte mit Anforderung der Updates während einer Synchronisierungszyklus verarbeitet werden. Aktualisieren von Zuordnungen Attribut wirkt sich auf die Leistung von einem Synchronisierungszyklus. Dies ist, da alle verwalteten Objekte erneut ausgewertet werden, die ein Update auf das Attribut Zuordnungskonfiguration erforderlich. Aus diesem Grund ist es empfiehlt es sich die Anzahl der aufeinander folgenden Änderungen an Ihrem Attribut Zuordnungen mindestens beibehalten.


##<a name="related-articles"></a>Verwandte Artikel

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
- [Automatisieren von Benutzer Bereitstellung/aufheben zu SaaS-Apps](active-directory-saas-app-provisioning.md)
- [Schreiben von Ausdrücken für Zuordnungen Attribut](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Bereichsdefinition Filter für Benutzer bereitgestellt](active-directory-saas-scoping-filters.md)
- [Aktivieren die automatische Bereitstellung von Benutzern und Gruppen aus Active Directory Azure Applications mithilfe von SCIM](active-directory-scim-provisioning.md)
- [Bereitstellung von Benachrichtigungen zu berücksichtigen](active-directory-saas-account-provisioning-notifications.md)
- [Liste der Lernprogramme erfahren Sie, wie Apps SaaS integriert werden soll.](active-directory-saas-tutorial-list.md)


<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
