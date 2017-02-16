<properties
    pageTitle="Attribut-basierte app provisioning mit Filter Bereichsdefinition | Microsoft Azure"
    description="Informationen Sie zum Bereiche Filter verwenden, um zu verhindern, dass Objekte in apps, die unterstützen Automatisches Benutzer bereitgestellt von tatsächlich bereitgestellt werden, wenn ein Objekt nicht Ihren geschäftlichen Anforderungen entsprechen."
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


# <a name="attribute-based-app-provisioning-with-scoping-filters"></a>Attribut-basierte app provisioning mit Bereichsdefinition Filter

Erläutert, wie Sie Bereiche Filter verwenden, um das Attribut-basierte Regeln definieren, die ermitteln können, welche Benutzer mit der Anwendung bereitgestellt werden, werden ist das Ziel in diesem Abschnitt.





## <a name="clauses-and-scope-groups"></a>Klauseln und Umfang Gruppen


![Festlegen des Gültigkeitsbereichs von Filtern][1] 




Bereiche Filter werden durch eine oder mehrere **Umfang Gruppen**, von denen jede eine oder mehrere **Klauseln**halten definiert. Zum Anzeigen der Klauseln für einen bestimmten Bereich Gruppieren, erweitern Sie ihn durch Klicken auf den Pfeil nach links neben dem Gruppennamen.

Eine **Klausel** bestimmt, welche Benutzer dürfen Bereiche Filter passieren, durch die Auswertung des Benutzers Attributen. Beispielsweise müssen Sie eine Klausel an, die erfordert, dass 'Status'-Attribut eines Benutzers gleich New York, was bedeutet, dass nur die New York Benutzer in die Anwendung bereitgestellt werden sollen.

![Bereichsdefinition Gruppennamen][2] 



Jede **Gruppe Bereich** beginnt mit: eine obligatorische- **Klausel**, wie in dem Screenshot dargestellt. Diese Klausel gibt einfach an, dass der Benutzer zuerst zur Anwendung zugewiesen werden muss, bevor sie durch Ihre Bereiche Filter ausgewertet wird. Diese Klausel kann nicht gelöscht oder geändert werden soll.

Sie können die neue Klauseln oder neue Bereichsgruppen durch Drücken die entsprechende Schaltfläche hinzufügen. Sie können jede Gruppe Bereich benennen durch seine Eigenschaft **Gruppennamen Umfang** bearbeiten.





## <a name="how-scoping-filters-are-evaluated"></a>Wie Filter Bereichsdefinition ausgewertet werden

Während der Bereitstellung testen wir jeder zugewiesenen Benutzer gegen Ihre Bereiche Filter, um festzustellen, ob dieser Benutzer Zugriff auf die Anwendung erhalten soll. Sie können sich vorstellen jeder Klausel als einen Testanruf, der in der Reihenfolge für den Benutzer aus, um zu vermeiden, erste herausgefiltert übergeben werden muss. 

Wenn Sie mehrere Umfang Gruppen definiert haben, muss jeder Benutzer mindestens eine von ihnen übergeben, um die Anwendung zugreifen. Innerhalb jeder Gruppe Bereich muss jedoch der Benutzer jede einzelne Klausel übergeben, um die Gruppe bestimmten Bereich zu übergeben. 

Kurzum, Sie können sich vorstellen Umfang Gruppen als oder würde zusammen, und Sie können sich die Klauseln darin enthaltenen als vorstellen und würde zusammen. Betrachten Sie beispielsweise den folgenden Bereiche Filter aus:


![Bereichsdefinition Gruppennamen][2]  


Nach dieser Bereiche Filter müssen Benutzer die folgenden Kriterien erfüllen, damit bereitgestellt werden:

1. Sie müssen die Anwendung zugewiesen werden.

2. Sie müssen in der Abteilung technisch funktionieren.

3. Er muss Arbeit in San Francisco oder in Kanada.


##<a name="related-articles"></a>Verwandte Artikel

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
- [Automatisieren von Benutzer bereitstellen und SaaS Applications deaktivieren](active-directory-saas-app-provisioning.md)
- [Anpassen von Attribut Zuordnungen für Benutzer bereitgestellt](active-directory-saas-customizing-attribute-mappings.md)
- [Schreiben von Ausdrücken für Zuordnungen Attribut](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Bereitstellung von Benachrichtigungen zu berücksichtigen](active-directory-saas-account-provisioning-notifications.md)
- [Aktivieren die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory Applications mithilfe von SCIM](active-directory-scim-provisioning.md)
- [Liste der Lernprogramme erfahren Sie, wie Apps SaaS integriert werden soll.](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./active-directory-saas-scoping-filters/ic782813.png
