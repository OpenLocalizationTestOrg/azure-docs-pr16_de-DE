<properties 
    pageTitle="Verwenden von Eigenschaften in Azure-API Informationsverwaltungsrichtlinien" 
    description="Informationen Sie zum Verwenden von Eigenschaften in Azure-API Informationsverwaltungsrichtlinien." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Verwenden von Eigenschaften in Azure-API Informationsverwaltungsrichtlinien

Informationsverwaltungsrichtlinien-API sind eine leistungsfähige Funktion des Systems, mit denen Herausgeber das Verhalten der API durch Konfiguration ändern können. Richtlinien sind eine Zusammenstellung von Anweisungen, die sequenziell auf die Anfrage ausgeführt werden oder einer API Antwort an. Anweisungen Richtlinie können mithilfe von literalen Textwerte, Richtlinienausdrücke und Eigenschaften erstellt werden. 

Jede API Management Service-Instanz verfügt über eine Properties-Auflistung des Schlüssel/Wert-Paare, die global für die Instanz des Diensts gelten. Diese Eigenschaften können zum Verwalten von Konstanten Zeichenfolgenwerte in allen API Konfiguration und Richtlinien verwendet werden. Jede Eigenschaft weist die folgenden Attribute.


| Attribut | Typ            | Beschreibung                                                                                             |
|-----------|-----------------|---------------------------------------------------------------------------------------------------------|
| Namen      | Zeichenfolge          | Der Name der Eigenschaft. Es kann nur Buchstaben, Ziffern, Punkt, Bindestrich enthalten, und Unterstriche. |
| Wert     | Zeichenfolge          | Der Wert der Eigenschaft. Es möglicherweise nicht leer sein oder nur aus Leerzeichen bestehen.                           |
| Geheim    | Boolesch         | Bestimmt, ob der Wert ein Geheimnis und oder nicht verschlüsselt werden sollte.                                |
| Kategorien      | Matrix Zeichenfolge | Optional tags, die beim bereitgestellten zum Filtern der Eigenschaftenliste verwendet werden können.                               |

Eigenschaften werden auf der Registerkarte **Eigenschaften** im Publisher-Portal konfiguriert. Im folgenden Beispiel werden drei Eigenschaften konfiguriert.

![Eigenschaften][api-management-properties]

Immobilienwerte können Literalzeichenfolgen und [Richtlinienausdrücke](https://msdn.microsoft.com/library/azure/dn910913.aspx)enthalten. Die folgende Tabelle zeigt die vorherigen drei Beispieleigenschaften und den Attributen. Der Wert der `ExpressionProperty` ist ein Richtlinienausdruck, der eine Zeichenfolge, die das aktuelle Datum und die Uhrzeit enthält zurückgibt. Die Eigenschaft `ContosoHeaderValue` wird als Geheimnis, markiert, sodass deren Wert nicht angezeigt wird.

| Namen               | Wert                      | Geheim | Kategorien    |
|--------------------|----------------------------|--------|---------|
| ContosoHeader      | TrackingId                 | Falsch  | Contoso |
| ContosoHeaderValue | ••••••••••••••••••••••     | WAHR   | Contoso |
| ExpressionProperty | @(DateTime.Now.ToString()) | Falsch  |         |

## <a name="to-use-a-property"></a>Verwenden Sie eine Eigenschaft

Um eine Eigenschaft in einer Richtlinie verwenden möchten, platzieren Sie den Namen der Eigenschaft in einem double Paar geschweifter Klammern wie `{{ContosoHeader}}`, wie im folgenden Beispiel gezeigt.

    <set-header name="{{ContosoHeader}}" exists-action="override">
      <value>{{ContosoHeaderValue}}</value>
    </set-header>

In diesem Beispiel `ContosoHeader` dient als Name für eine Kopfzeile in einer `set-header` Richtlinie, und `ContosoHeaderValue` wird als Wert für die Kopfzeile verwendet. Wenn diese Richtlinie während einer Anforderung oder Antwort mit dem Gateway-API Management ausgewertet wird `{{ContosoHeader}}` und `{{ContosoHeaderValue}}` durch ihre jeweiligen Eigenschaftswerte ersetzt werden.

Eigenschaften können als abgeschlossen Attribut oder Elementwerte wie im vorherigen Beispiel dargestellt verwendet werden, aber sie können auch in eingefügt oder kombiniert mit Teil eines Ausdrucks literaler Text wie im folgenden Beispiel gezeigt:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Eigenschaften können auch Richtlinienausdrücke enthalten. Im folgenden Beispiel die `ExpressionProperty` verwendet wird.

    <set-header name="CustomHeader" exists-action="override">
        <value>{{ExpressionProperty}}</value>
    </set-header>

Wenn dieser Richtlinie ausgewertet wird, `{{ExpressionProperty}}` wird durch den Wert ersetzt: `@(DateTime.Now.ToString())`. Da der Wert eines Ausdrucks Richtlinie ist, der Ausdruck ausgewertet wird, und die Richtlinie, die mit der Ausführung fortgesetzt wird.

Sie können diese Out Aufrufen eines Vorgangs, die eine Richtlinie mit Eigenschaften im Gültigkeitsbereich ist in der Entwicklerportal testen. Im folgenden Beispiel wird ein Vorgang mit zwei vorherigen Beispiel aufgerufen `set-header` Richtlinien mit Eigenschaften. Beachten Sie, dass die Antwort zwei benutzerdefinierte Header enthält, die mit Richtlinien mit Eigenschaften konfiguriert wurden.

![-Entwicklerportal][api-management-send-results]

Wenn Sie die [API Inspektor Spur](api-management-howto-api-inspector.md) für einen Anruf, die die beiden vorherigen Beispielrichtlinien mit Eigenschaften enthält betrachten, sehen Sie die beiden `set-header` Richtlinien mit der Immobilienwerte eingefügt sowie die Auswertung von Ausdrücken Richtlinie für die Eigenschaft, die den Richtlinienausdruck enthalten.

![API Inspektor Spur][api-management-api-inspector-trace]

Beachten Sie, dass während der Immobilienwerte, die Richtlinienausdrücke enthalten können, Immobilienwerte, die andere Eigenschaften enthalten können. Wenn Sie Text mit einem Eigenschaftenverweis für einen Eigenschaftswert verwendet wird, z. B. `Property value text {{MyProperty}}`, dieser Eigenschaft Bezug wird nicht ersetzt werden und werden als Teil der Wert der Eigenschaft enthalten.

## <a name="to-create-a-property"></a>Zum Erstellen einer Eigenschaft

Um eine Eigenschaft zu erstellen, klicken Sie auf der Registerkarte **Eigenschaften** auf **Eigenschaft hinzufügen** .

![Eigenschaft hinzufügen][api-management-properties-add-property-menu]

**Namen** und **Wert** sind erforderlichen Werte. Aktivieren Sie dieser Eigenschaft ein Geheimnis ist, das Kontrollkästchen **Dies ist ein Geheimnis** . Geben Sie einen oder mehrere optionale Tags unterstützen Sie beim Organisieren Ihrer Eigenschaften, und klicken Sie auf **Speichern**.

![Eigenschaft hinzufügen][api-management-properties-add-property]

Wenn eine neue Eigenschaft gespeichert ist, wird das **Eigenschaft suchen** Textfeld mit dem Namen der neuen Eigenschaft aufgefüllt, und die neue Eigenschaft wird angezeigt. Wenn Sie alle Eigenschaften anzeigen möchten, deaktivieren Sie das Textfeld **Eigenschaft suchen** , und drücken Sie die EINGABETASTE.

![Eigenschaften][api-management-properties-property-saved]

Informationen zum Erstellen einer Eigenschaft mithilfe der REST-API finden Sie unter [Erstellen einer Eigenschaft mithilfe der REST-API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>Um eine Eigenschaft bearbeiten

Um eine Eigenschaft zu bearbeiten, klicken Sie neben der Eigenschaft bearbeiten auf **Bearbeiten** .

![Eigenschaft bearbeiten][api-management-properties-edit]

Nehmen Sie alle gewünschten Änderungen vor, und klicken Sie auf **Speichern**. Wenn Sie den Namen der Eigenschaft ändern, werden alle Richtlinien, die sich auf die Eigenschaft automatisch aktualisiert, um den neuen Namen zu verwenden.

![Eigenschaft bearbeiten][api-management-properties-edit-property]

Informationen zum Bearbeiten einer Eigenschaft die REST-API verwenden finden Sie unter [Bearbeiten einer Eigenschaft mithilfe der REST-API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>So löschen Sie eine Eigenschaft

Um eine Eigenschaft zu löschen, klicken Sie neben dem zu löschenden Eigenschaft auf **Löschen** .

![Löschen Sie die Eigenschaft][api-management-properties-delete]

Klicken Sie auf **Ja, löschen Sie ihn** , um zu bestätigen.

![Löschen bestätigen][api-management-delete-confirm]

>[AZURE.IMPORTANT] Wenn Sie die Eigenschaft mithilfe von Richtlinien verwiesen wird, werden Sie möglicherweise nicht erfolgreich löschen, bis Sie die Eigenschaft aus allen Richtlinien entfernen, die sie verwenden.

Informationen zum Löschen einer Eigenschaft, die die REST-API verwenden finden Sie unter [Löschen einer Eigenschaft mithilfe der REST-API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Zum Suchen und Filtern Eigenschaften

Die Registerkarte **Eigenschaften** enthält Such- und Filterfunktionen, denen Sie Ihre Eigenschaften verwalten können. Geben Sie zum Filtern der Eigenschaftenliste nach Eigenschaftsname einen Suchbegriff in das Textfeld **Eigenschaft suchen** . Wenn Sie alle Eigenschaften anzeigen möchten, deaktivieren Sie das Textfeld **Eigenschaft suchen** , und drücken Sie die EINGABETASTE.

![Suchen][api-management-properties-search]

Um der Eigenschaftenliste nach Kategorie Werte gefiltert werden, geben Sie eine oder mehrere Kategorien in das Textfeld **Filtern nach Kategorien** aus. Geben Sie löschen, um alle Eigenschaften anzeigen möchten, im Textfeld **Filtern, indem Sie Kategorien** , und drücken Sie.

![Filter][api-management-properties-filter]

## <a name="next-steps"></a>Nächste Schritte

-   Weitere Informationen zum Arbeiten mit Richtlinien
    -   [Richtlinien in API Management](api-management-howto-policies.md)
    -   [Richtlinie Bezug](https://msdn.microsoft.com/library/azure/dn894081.aspx)
    -   [Richtlinienausdrücke](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Schauen Sie sich einen Überblick video

> [AZURE.VIDEO use-properties-in-policies]

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

