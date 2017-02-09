<properties 
    pageTitle="Richtlinien in Azure-API Management | Microsoft Azure" 
    description="Informationen Sie zum Erstellen, bearbeiten und Konfigurieren von Richtlinien in API Management." 
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


#<a name="policies-in-azure-api-management"></a>Richtlinien in Azure-API Management

Richtlinien sind in Azure-API Verwaltung eine leistungsfähige Funktion des Systems, mit denen Herausgeber das Verhalten der API durch Konfiguration ändern können. Richtlinien sind eine Zusammenstellung von Anweisungen, die sequenziell auf die Anfrage ausgeführt werden oder einer API Antwort an. Beliebte Anweisungen Format Konvertierung von XML-JSON enthalten, und rufen Sie Zins Begrenzung verwenden, um die Menge des eingehende Anrufe von einem Entwickler einschränken. Viele weitere Richtlinien sind sofort verfügbar ist.

Finden Sie die [Richtlinie Bezug][] für eine vollständige Liste der Richtlinie Anweisungen und deren Einstellungen aus.

Richtlinien werden innerhalb des Gateways angewendet, der sich zwischen den Consumer API und der verwalteten API befindet. Das Gateway empfängt alle Besprechungsanfragen und in der Regel weiterleitet unverändert in der zugrunde liegenden API. Jedoch eine Richtlinie Änderungen der eingehenden Anfragen und die ausgehende Antwort anwenden kann.

Richtlinienausdrücke können als Attributwerte oder Textwerte in keines der API Informationsverwaltungsrichtlinien verwendet werden, die Richtlinie nicht anders angegeben. Einige Richtlinien, wie etwa die Richtlinien [Fluss Steuerelements][] , und [Legen Sie die Variable][] basieren auf Richtlinienausdrücke. Weitere Informationen finden Sie unter [Erweiterte Richtlinien][] und [Richtlinienausdrücke][].

## <a name="scopes"> </a>Zum Konfigurieren von Richtlinien
Richtlinien können global oder in den Bereich eines [Produkts][], [API][] oder [Vorgang][]konfiguriert werden. Zum Konfigurieren einer Richtlinie, navigieren Sie zu den Richtlinien-Editor im Portal Publisher.

![Menü Richtlinien][policies-menu]

Der Richtlinien-Editor besteht aus drei Komponenten: die Richtlinie Umfang (oben), die Richtliniendefinition, wo Richtlinien (links) bearbeitet werden, und die Anweisungen Liste (nach rechts):

![Richtlinien-editor][policies-editor]

Zum beginnen eine Richtlinie konfigurieren müssen Sie zuerst den Bereich auswählen, an dem die Richtlinie angewendet werden soll. In den Screenshot unterhalb der **Starter** ist Produkt aktiviert. Beachten Sie, dass das quadratische Symbol neben dem Richtliniennamen gibt an, dass diese Ebene bereits eine Richtlinie angewendet wird.

![Bereich][policies-scope]

Da bereits eine Richtlinie angewendet wurde, wird die Konfiguration der Definition Ansicht angezeigt.

![Konfigurieren][policies-configure]

Die Richtlinie ist schreibgeschützt angezeigt zuerst. Um der Definition klicken Sie auf die Aktion **Konfigurieren einer Richtlinie** zu bearbeiten.

![Bearbeiten][policies-edit]

Die Richtliniendefinition ist ein einfaches XML-Dokument, das eine Abfolge von eingehenden und ausgehenden Aussagen beschreibt. Die XML-Daten kann direkt in das Definitionsfenster bearbeitet werden. Eine Liste von Anweisungen rechts bereitgestellt ist, und Anweisungen zum aktuellen Bereich anwendbar aktiviert und hervorgehoben werden. wie durch die **Beschränkung anrufen Zins** -Anweisung aus dem Screenshot gezeigt.

Durch Klicken auf eine aktiviert-Anweisung wird die entsprechende XML an der Position des Cursors in der Definition Ansicht hinzufügen. 

>[AZURE.NOTE] Wenn die Richtlinie, die Sie hinzufügen möchten, nicht aktiviert ist, stellen Sie sicher, dass Sie in den richtigen Bereich für diese Richtlinie haben. Jede Informationsverwaltungsrichtlinien-Anweisung ist für bestimmte Bereiche und Richtlinie Abschnitte entwickelt. So prüfen Sie die Richtlinie Abschnitte und Bereiche für eine Richtlinie, aktivieren Sie im Abschnitt **Verwendung** für diese Richtlinie in die [Richtlinie Bezug][]ein.

Eine vollständige Liste der Richtlinie Aussagen und deren Einstellungen sind in der [Richtlinie Bezug][]verfügbar.

Angenommen, um eine neue Anweisung zum Einschränken der eingehender Anfragen an angegebene IP-Adressen hinzufügen möchten, platzieren Sie den Cursor nur innerhalb des Inhalts von der `inbound` XML-Element, und klicken Sie auf die Aussage **einschränken Anrufer IP-Adressen** .

![Einschränkung Richtlinien][policies-restrict]

Dies wird einen XML-Ausschnitt zum Hinzufügen der `inbound` Element, das Anleitungen zum Konfigurieren der Anweisung bereitstellt.

    <ip-filter action="allow | forbid">
        <address>address</address>
        <address-range from="address" to="address"/>
    </ip-filter>

Einschränken von eingehenden Anfragen und annehmen, dass nur die über eine IP-Adresse 1.2.3.4 die XML-Daten wie folgt ändern:

    <ip-filter action="allow">
        <address>1.2.3.4</address>
    </ip-filter>

![Speichern][policies-save]

Führen Sie beim Konfigurieren der Anweisungen für die Richtlinie, klicken Sie auf **Speichern** und die Änderungen sofort mit dem Gateway-API Management verteilt.

##<a name="sections"> </a>Grundlegendes zu Richtlinienkonfiguration

Eine Richtlinie ist eine Reihe von Anweisungen, die für eine Anforderung und eine Antwort Reihenfolge ausgeführt. Die Konfiguration ist aufgeteilte ordnungsgemäß `inbound`, `backend`, `outbound`, und `on-error` Abschnitte wie in der folgenden Konfiguration dargestellt.

    <policies>
      <inbound>
        <!-- statements to be applied to the request go here -->
      </inbound>
      <backend>
        <!-- statements to be applied before the request is forwarded to 
             the backend service go here -->
      </backend>
      <outbound>
        <!-- statements to be applied to the response go here -->
      </outbound>
      <on-error>
        <!-- statements to be applied if there is an error condition go here -->
      </on-error>
    </policies> 

Ist es ein Fehler bei der Verarbeitung einer Anforderung, die verbleibenden Schritte der `inbound`, `backend`, oder `outbound` Abschnitte werden übersprungen und Ausführung springt zu Anweisungen in der `on-error` Abschnitt. Indem Sie ein Richtlinie Anweisungen in der `on-error` Abschnitt überprüfen Sie die Fehlermeldung mithilfe der `context.LastError` Eigenschaft prüfen und Anpassen der Fehler Antwort mit der `set-body` Richtlinie, und konfigurieren, was passiert, wenn ein Fehler auftritt. Es gibt Fehlercodes für integrierte Schritte und Fehler, die während der Verarbeitung der Richtlinie Aussagen auftreten können. Weitere Informationen finden Sie unter [Fehlerbehandlung in Informationsverwaltungsrichtlinien API](https://msdn.microsoft.com/library/azure/mt629506.aspx).

Da Richtlinien auf verschiedenen Ebenen (globaler, Produkt, api und Vorgang) angegeben werden muss, können stellt die Konfiguration eine Möglichkeit, die Reihenfolge angeben, in der die Richtliniendefinition Anweisungen in Bezug auf die übergeordnete Richtlinie ausführen. 

Richtlinie Bereiche werden in der folgenden Reihenfolge ausgewertet.

1. Bereich "Global"
2. Produkt-Bereich
3. API Umfang
4. Umfang der Vorgang

Die Aussagen auf der Grundlage der Position des ausgewertet werden die `base` Elements, sofern es vorhanden ist.

Beispielsweise, wenn Sie eine Richtlinie bei der globalen Ebene und einer Richtlinie für eine API konfiguriert haben, werden klicken Sie dann bei jeder Verwendung dieser bestimmten API beide Richtlinien angewendet. Projektmanagement-API ermöglicht deterministisch Sortierung der kombinierte Richtlinie Aussagen über das Basiselement. 

    <policies>
        <inbound>
            <cross-domain />
            <base />
            <find-and-replace from="xyz" to="abc" />
        </inbound>
    </policies>

In der obigen Beispiel Richtliniendefinition der `cross-domain` -Anweisung ausführen möchten, bevor eine höheren Richtlinien der im Gegenzug würde folgen die `find-and-replace` Richtlinie.

Wenn die gleiche Richtlinie zweimal in die Informationsverwaltungsrichtlinien-Anweisung angezeigt wird, ist die am häufigsten zuletzt ausgewertete Richtlinie angewendet. Sie können dies zum Außerkraftsetzen von Richtlinien, die mit einem höheren Bereich definiert sind. Wenn die Richtlinien im aktuellen Bereich in den Richtlinien-Editor anzeigen möchten, klicken Sie auf **neu berechnen effektive Richtlinie für den ausgewählten Bereich**.

Beachten Sie, dass die globale Richtlinie keine übergeordnete Richtlinie hat und Verwenden der `<base>` Element darin hat keine Auswirkung. 

## <a name="next-steps"></a>Nächste Schritte

Schauen Sie sich das Video auf Richtlinienausdrücke folgen.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Richtlinie Bezug]: api-management-policy-reference.md
[Produkt]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[Vorgang]: api-management-howto-add-operations.md

[Erweiterte Richtlinien]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Steuerelement Fluss]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Festlegen einer Variablen]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Richtlinienausdrücke]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
