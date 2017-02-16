<properties
   pageTitle="Informationen zu, und erstellen Sie eine app BizTalk Regeln API in Ihrer App Logik | Microsoft Azure"
   description="In diesem Thema enthält die BizTalk Regeln Connector-Features und erläutert, deren Verwendung"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="04/20/2016"
   ms.author="andalmia"/>

#<a name="biztalk-rules"></a>BizTalk-Regeln

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Business Regeln schließt die Richtlinien und die Entscheidungen, die Geschäftsprozesse steuern. Diese Richtlinien können in Verfahren Handbücher, Verträge oder Vereinbarungen formell definiert werden oder möglicherweise als wissen oder Fachwissen in Mitarbeiter enthaltenen vorhanden. Diese Richtlinien sind dynamisch und Ankündigung geändert über Uhrzeit fällig Änderungen in Business-Pläne, Vorschriften oder aus anderen Gründen.

Diese Richtlinien in einer herkömmlichen Programmiersprache implementieren erfordert viel Zeit und koordinieren und ohne Programmierkenntnisse zur Erstellung und Pflege von Unternehmensrichtlinien Teilnahme wird nicht aktiviert. BizTalk Business Regeln bietet eine Möglichkeit, schnell diese Richtlinien implementieren und die restlichen Geschäftsprozess entkoppeln. Dadurch wird für die erforderlichen Änderung Unternehmensrichtlinien beeinträchtigt die restlichen Geschäftsprozess.

##<a name="why-rules"></a>Warum von Regeln

Es gibt 3 wichtigsten Gründe für die Verwendung von BizTalk Business Regeln in Geschäftsprozess aus:

* Entkoppeln Sie Geschäftslogik vom Code der Anwendung
- Zulassen Sie Business Analysten haben Sie mehr Kontrolle über Business Logik management
+ Änderungen an Geschäftslogik wechseln Sie zu der Herstellung schneller

##<a name="rules-concepts"></a>Regeln Konzepte

##<a name="vocabulary"></a>Vokabular

Ein _Vokabular_ ist eine Auflistung von Definitionen aus Anzeigenamen für die computing Objekte in der Regel Bedingung und Aktion verwendet werden. Vokabulardefinitionen, damit die Regeln einfacher zu lesen, verstehen und Freigeben von Personen in einer bestimmten Business-Domäne.

Sprachen sollen die Lücke zwischen Geschäftssemantik und Implementierung zu schließen. Beispielsweise möglicherweise eine Bindung für einen Genehmigungsstatus auf eine bestimmte Spalte in einer bestimmten Zeile in einer bestimmten Datenbank als eine SQL-Abfrage dargestellt verweisen. Statt diese Art von komplexer Darstellung in einer Regel eingefügt haben, können Sie stattdessen eine Vokabulardefinition, zugeordnet diese Daten Bindung, mit einem angezeigten Namen von "Status". erstellen. Anschließend können Sie "Status" in eine beliebige Anzahl von Regeln einschließen, und die Regel-Engine die entsprechenden Daten aus der Tabelle abrufen kann.

##<a name="rule"></a>Regel

Business Regeln sind deklarative Anweisungen, die die Durchführung von Geschäftsprozessen zu steuern. Eine Regel besteht aus einer Bedingung und Aktionen. Die Bedingung ausgewertet wird, und wenn es als WAHR ausgewertet wird, initiiert der Regel-Engine eine oder mehrere Aktionen.
Regeln in Business Framework Regeln werden mit folgendem Format definiert:

_IF_ _Bedingung_ _Klicken Sie dann_ _Aktion_

Betrachten Sie das folgende Beispiel:

*IF-Betrag ist kleiner oder gleich zu erledigenden verfügbar*  
*Durchführen von Transaktion und Drucken Bestätigung*

Mit dieser Regel bestimmt, ob eine Transaktion durch Anwenden von Geschäftslogik, in Form von einen Vergleich der beiden monetäre Werte in Form von einem Zeitraum Transaktion und vorhandenen Mitteln durchgeführt werden.
Die Regel Business können Sie erstellen, ändern und Business Regeln bereitstellen. Alternativ können Sie die vorherigen Aufgaben programmgesteuert ausführen.

###<a name="conditions"></a>Bedingungen

Eine Bedingung ist ein Wahrheitswert (boolesch) Ausdruck, der eine oder mehrere Prädikate besteht.
In diesem Beispiel wird das Prädikat kleiner als oder gleich der Betrag und vorhandenen Mitteln angewendet. Diese Bedingung wird immer als True oder False ausgewertet werden.
Prädikate kombiniert werden können, die logischen Operatoren AND, OR und nicht zu einen logischen Ausdruck zu erstellen, der potenziell ganz groß ist, aber immer entweder als True oder False ausgewertet werden.

###<a name="actions"></a>Aktionen

Aktionen sind die Bewertung der Bedingung funktionsübergreifendes folgen. Wenn eine Regel Bedingung erfüllt ist, werden die zugehörige Aktion oder Aktionen initiiert.
In diesem Beispiel werden "leiten Transaktion" und "Drucken" Aktionen, die ausgeführt werden, wenn und nur dann, wenn die Bedingung (in diesem Fall "ist Menge kleiner oder gleich zu erledigenden verfügbar") wahr ist.
Aktionen werden in Business Regeln Framework durch Ausführen festgelegter XML-Dokumenten dargestellt.

##<a name="policy"></a>Richtlinie

Eine Richtlinie ist eine logische Gruppierung von Regeln. Sie verfassen eine Richtlinie, speichern Sie es, testen Sie Sie, und, wenn Sie mit dem Ergebnis zufrieden sind, verwenden sie in einer Umgebung für die Herstellung.

###<a name="policy-composition"></a>Richtlinie Komposition

Sie können die Richtlinien im Portal Regeln verfassen. Eine Richtlinie kann eine beliebig große Regelsatzes enthalten, jedoch in der Regel beim Verfassen einer Richtlinie aus Regeln, die zu einer bestimmten Business Domäne innerhalb des Kontexts der Anwendung gehören, die die Richtlinie verwendet wird.

###<a name="policy-testing"></a>Testen der Richtlinie
Bevor sie in einer Umgebung für die Herstellung verwendet wird, können Sie einen Testlauf mit Ihrer Richtlinie effektiv ausführen. Im Portal Regeln können Sie bereitstellen Eingaben zu einer Richtlinie, führen Sie die Richtlinie, und deren Ausgabe anzeigen. Die Ausgabe enthält, Protokolle, Ausführen der Regel, die Bewertung der Bedingung, und die entsprechenden Ausgaben.

##<a name="sample-scenario---insurance-claims"></a>Beispiel für Szenario - Versicherungsansprüche
Wir übernehmen einer Stichprobe Szenario, und wir die Geschäftslogik für das gleiche Verfassen sie durchlaufen.

![ALT-text][1]

In einem Szenario mit really simple Versicherungsansprüche sendet Person seine Versicherungsansprüche (über eine beliebige Client wie Website, Telefon App usw.). An den Unternehmens anfordern Verarbeitung Einheit gesendet und auf Basis des Ergebnisses der Verarbeitung erhält in Anspruch anfordern, kann des Anspruchs entweder genehmigt, abgelehnt oder entlang zur weiteren manuellen Verarbeitung gesendet.
Anfordern Verarbeitung Einheit in diesem Szenario wäre das Schema die Geschäftslogik für das System einschließt. Aufzeichnen dieser Einheit Detail, können wir die Meldung:

![ALT-text][2]

Lassen Sie uns jetzt verwenden Sie Business Regeln zum implementieren diese Geschäftslogik.


##<a name="creation-of-rules-api-app"></a>Erstellen von Regeln Api-App


1. Melden Sie sich im Azure-Portal
2. Wählen Sie-Marketplace und Suchen nach *BizTalk Regeln* neu >
3. Wählen Sie aus der Ergebnisliste BizTalk Regeln aus. Das BizTalk Regeln Blade wird geöffnet.
4. Wählen Sie die Schaltfläche *Erstellen* ![Alt-Text][3]
1. Geben Sie in das neue Blade, das geöffnet wird, die folgenden Informationen ein:  
    1. Name – gewähren einen Namen für Ihre Regeln API-App
    1. App-Serviceplan – aktivieren, oder erstellen eine neue App-Dienst planen
    1. Preise Ebene – auswählen der Preisgestaltung Ebene Sie diese App in befinden möchten.
    1. Ressourcengruppe – auswählen oder erstellen, in denen die App in befinden soll, Ressourcengruppe
    2. Abonnement - wählen Sie das Abonnement, das Sie verwenden möchten
    1. Speicherort – wählen Sie die geografische Position, möchte Sie die App bereitgestellt werden soll.
4.  Wählen Sie auf *Erstellen*. Innerhalb weniger Minuten würden Ihre BizTalk Regeln API App erstellt werden.

##<a name="vocabulary-creation"></a>Erstellung von Vokabular
Nach dem Erstellen einer BizTalk Regeln API App, wäre im nächste Schritt Sprachen erstellen. Der Annahme ist, dass der Entwickler häufiger Rolle dieser Übung Aufgabe ausführen sollen. So sieht so erhalten Sie dies erledigt aus:


1. -API Apps - Schnellstartleiste Ihrer BizTalk Regeln API App aus dem Portal an, indem Sie auf Durchsuchen > ><Your Rules API App>. Dies erhalten zum Dashboard App-API Regeln ähnlich wie unten Sie:

   ![ALT-text][4]

2.Klicken Wählen Sie "Vokabulardefinitionen" aus. Dies würden Ihnen die Vokabular Authoring Bildschirm 3.Klicken zu beginnen, Hinzufügen von neuen Vokabulardefinitionen auswählen "hinzufügen" angezeigt.
Es gibt 2 Typen von Vokabulardefinitionen derzeit unterstützt – Literalen und XML-ein.

##<a name="literal-definition"></a>Literalen Definition
1.  Nach dem Klicken auf "Hinzufügen", öffnet ein neuer "Definition hinzufügen" Blade. Geben Sie die folgenden Werte
  1.    Namen – nur alphanumerische Zeichen werden ohne ein Sonderzeichen erwartet. Dies sollte auch Ihrer vorhandenen Vokabular Definition Liste eindeutig sein.
  2.    Beschreibung – optionales Feld.
  3.    Geben Sie Definition – es gibt 2 Typen unterstützt. Wählen Sie in diesem Beispiel literalen
  4.    Datentyp – kann diese Benutzer wählen Sie den Datentyp der Definition. Aktuell 4 Datentypen werden unterstützt: ich.  Zeichenfolge – diese Werte eingegeben werden müssen, in doppelte Anführungszeichen ("Beispiel String")  
    II. Boolesche – Dies kann entweder true oder false sein  
    III.    Zahl – kann eine Dezimalzahl  
    IV. DateTime – bedeutet dies, dass die Qualität des Datums Type ist. Daten müssen dieses Format – mm/tt/jjjj hh: mm: AM\PM eingegeben  
  5. Eingabemethoden – ist dies, in dem Sie den Wert der Definition Ihrer eingeben. Die hier eingegebene Werte müssen mit dem ausgewählten Datentyp entsprechen. Sie können entweder einen einzelnen Wert, der einen Satz von Werten getrennt durch Kommas oder einen Bereich von Werten, die mit dem Schlüsselwort *zu*eingeben. Beispielsweise können Sie einen eindeutigen Wert 1 eingeben. eine Reihe 1, 2, 3; oder einen Bereich von 1 bis 5. Zahlen Notiz, die nur für Bereich unterstützt wird.
  6. Wählen Sie *OK*aus.

![ALT-text][5]
##<a name="xml-definition"></a>XML-Definition
Wenn Vokabular Typ als XML ausgewählt ist, muss die folgenden Eingaben angegeben werden muss  
  ein.    Schema – wird Klick auf diese neue Blade ermöglichen, dass Benutzer entweder aus einer Liste mit bereits hochgeladene Schemas oder festlegen, dass eine neue Hochladen auswählen geöffnet.
b.    XPATH – neben diesem Eingabefeld ruft nur nach Ablauf ein Schema im vorherigen Schritt auswählen. Klicken auf diese wird das Schema angezeigt, das ausgewählt wurde, und ermöglicht dem Benutzer, den Knoten auswählen, für den eine Vokabulardefinition erstellt werden soll.  
  c.    Fakultät – diese Eingabe bezeichnet, welches Datenobjekt an die Regeln-Engine für die Verarbeitung "Herd" werden möchten. Dies ist eine erweiterte Eigenschaft und standardmäßig auf das übergeordnete Element des ausgewählten XPATH festgelegt ist. Fakultät wird für verketten und Websitesammlung Szenarien besonders wichtig.

![ALT-text][6]

### <a name="add-bulk"></a>Massen hinzufügen
Die oben aufgeführten Schritte haben die Oberfläche für das Erstellen von Vokabulardefinitionen erfasst. Sobald erstellt haben, werden die erstellte Vokabulardefinitionen in Listenformular angezeigt. Es gibt Anforderungen mehrere Definitionen aus dem gleichen Schema anstatt zu wiederholen die obigen Schritte jedem einzelnen generieren können. Dies ist der Massen hinzufügen Videofunktionen äußerst wird.
Auswählen "Massenhinzufügen" gelangen Sie zu einer neuen Blade. Der erste Schritt besteht im Schema auszuwählen, für die mehrere Definitionen erstellt werden. Durch die Aktivierung dieser öffnen ein neues Blades zulassen, dass entweder aus einer Liste mit bereits hochgeladene Schemas auswählen oder festlegen, dass eine neue hochladen.
Jetzt wird die XPath-Eigenschaft nicht gesperrte. Durch die Aktivierung dieser wird das Schema Viewer geöffnet, in dem mehrere Knoten ausgewählt werden können.
Die Namen für die mehrere Definitionen erstellt werden standardmäßig auf den Namen des ausgewählten Knotens verwendet. Diese können immer nach der Erstellung geändert werden.

![ALT-text][7]

##<a name="policy-creation"></a>Erstellen von Richtlinien
Nachdem der Entwickler erforderliche Sprachen erstellt hat, besteht die Anforderung, dass Business Analysten Erstellen einer über das Azure-Portal Unternehmensrichtlinien wäre.  
    1. auf die Regeln-App erstellt haben befindet sich einer Richtlinie Lens auf die der Benutzer in der Richtlinie Erstellung Seite zu wechseln, wird.  
    2. auf dieser Seite wird die Liste der Richtlinien angezeigt, die diese bestimmten Regeln App hat. Die Analysten kann eine neue Richtlinie hinzufügen, indem Sie einfach einen Richtliniennamen eingeben und die Tab-Taste zweimal drücken. Mehrere Richtlinien können in einer einzelnen Regeln API App befinden.  
    3. auswählen die erstellte Richtlinie dauert den Benutzer auf der Seite Richtlinie eine kann, in dem die Regeln angezeigt, die in der Richtlinie sind.  
    ![ALT-Text][8]
 4.  Wählen Sie "Hinzufügen", um eine neue Regel hinzufügen. Dadurch gelangen Sie zu einer neuen Blade.

##<a name="rule-creation"></a>Regel erstellen
Eine Regel ist Auflistung von Bedingung und Aktion Anweisungen. Aktionen werden ausgeführt, wenn die Bedingung als WAHR bewertet wird. Geben Sie in der Regel erstellen Blade einen eindeutigen Regelname (für diese Richtlinie) und eine Beschreibung (optional) ein.
Im Feld Bedingung (wenn) kann verwendet werden, um komplexe bedingten Anweisungen zu erstellen. Es folgen die Schlüsselwörter unterstützt:  
1.  Und – bedingte operator  
2.  Oder – bedingte operator  
3.  unterstützt\_nicht\_bestehen.  
4.  vorhanden ist  
5.  falsch  
6.  ist\_gleich\_zu  
7.  ist\_größer\_als  
8.  ist\_größer\_als\_gleich\_zu  
9.  ist\_in  
10. ist\_weniger\_als  
11. ist\_weniger\_als\_gleich\_zu  
12. ist\_nicht\_in  
13. ist\_nicht\_gleich\_zu  
14. Rest  
15. WAHR  

Im Feld Action(THEN) kann mehrere Anweisungen, die jeweils einer Zeile, die zum Erstellen von Aktionen, die ausgeführt werden sollen, enthalten. Es folgen die Schlüsselwörter unterstützt:  
1.  gleich  
2.  falsch  
3.  WAHR  
4.  Halt  
5.  Rest  
6.  NULL  
7.  Aktualisieren  

Die Felder Bedingung und Aktion bieten Intellisense, damit Sie schnell eine Regel erstellen können. Dies kann bekommen, indem Sie STRG + LEERTASTE oder Eingabe gerade begonnen ausgelöst werden. Schlüsselwörter übereinstimmenden eingegebene Zeichen werden automatisch gefiltert nach unten und dargestellt werden. Alle Schlüsselwörter und Vokabulardefinitionen zeigt das Intellisense-Fenster.
![ALT-text][9]

##<a name="explicit-forward-chaining"></a>Explizite verketten weiterleiten
BizTalk Regeln unterstützt explizite weiterleiten verketten können, wenn Benutzer den Regeln als Antwort auf bestimmte Aktionen erneut auswerten möchten, können sie dies auslösen mithilfe von bestimmter Schlüsselwörter. Im folgenden sind die Schlüsselwörter unterstützt:  
   1.   Aktualisieren <vocabulary definition> – dieses Schlüsselworts wertet alle Regeln, die die angegebenen Vokabulardefinition in seinem Zustand verwenden erneut aus.  
   2.   Halt – dieses Schlüsselworts beendet alle Regel Ausführungen

##<a name="enabledisable-rules"></a>Auf Regeln
Jede Regel in der Richtlinie kann aktiviert oder deaktiviert werden. Standardmäßig werden alle Regeln aktiviert. Deaktivierte Regeln nicht werden während der Ausführung der Richtlinie ausgeführt. Auf Regeln können dafür aus dem Blade Regel direkt – die Befehle stehen in der Befehlsleiste am oberen Rand der Blade, oder aus der Richtlinie, hat das Kontextmenü (Rechtsklick auf eine Regel) die Option auf.

##<a name="rule-priority"></a>Regelpriorität
Alle Regeln einer Richtlinie werden in Reihenfolge ausgeführt. Die Priorität der Ausführung wird durch die Reihenfolge bestimmt, in der Richtlinie auftreten. Diese Priorität kann durch einfach ziehen und Ablegen von der Regel geändert werden.

##<a name="test-policy"></a>Testen der Richtlinie
Sie können Ihre Richtlinien mithilfe des Befehls "Richtlinie testen" in der Richtlinie testen Blade testen. In diesem Blade sehen Sie eine Liste der Vokabulardefinitionen, die in der Richtlinie verwendet werden, die eine Benutzereingabe erfordern. Benutzer können die Werte für diese Eingaben, deren Testszenario für manuell hinzufügen. Alternativ können Benutzer auch auswählen zu importierenden Eingaben XMLs prüfen. Wenn Sie alle Eingaben angegeben haben, der Test ausgeführt werden kann, und die Ausgaben für jede Vokabulardefinition werden in der Ausgabespalte für einfache Vergleich angezeigt werden. Wenn Business Analysten geeignet Protokolle anzeigen möchten, klicken Sie auf "Protokolle anzeigen" zum Anzeigen der Ausführung Protokolle. Wenn die Protokolle speichern möchten, steht die Option "Ausgabe speichern" zum Speichern aller verwandte Daten für die unabhängige Analyse testen.

## <a name="using-rules-in-logic-apps"></a>Verwenden von Regeln in Logik Apps
Nachdem die Richtlinie verfasst und getestet hat, ist es jetzt zur Verwendung bereit. Sie können eine neue Logik App erstellen, indem der linken Seite der Homepage des Portals Logik Apps auswählen. Nachdem Sie Ihre App Logik erstellt wurde, starten Sie ihn, und wählen Sie *Trigger und Aktionen*. Sie können dann die Vorlage *ganz neu erstellen* auswählen. Führen Sie die Schritte zum Hinzufügen von Ihrem BizTalk Regeln API-App mit der Logik aus. Nachdem dies erfolgt ist, werden die Möglichkeit, welche Regeln API App (Aktion) zum Ziel auswählen. Zählen die Liste der Richtlinien, die ausgeführt werden sollen. Wählen Sie aus, dass eine bestimmte Richtlinie, nach denen die Eingaben für die Richtlinie erforderlich, angegeben werden muss. Benutzer können die Ausgabe unterhalb der Regeln API App für weitere entscheidungsfindung verwenden.

## <a name="using-rules-via-apis"></a>Verwenden von Regeln über APIs
Die Regeln API-App kann auch mit einem umfassenden Satz von APIs aufgerufen werden. Benutzer werden nicht bei der Verwendung von Apps Logik nur eingeschränkt und Regeln in einer beliebigen Anwendung vom restlichen Anrufe verwenden. Die genauen REST-APIs können, indem Sie auf der "Abgegrenzt API" Lens im Dashboard Regeln angezeigt werden.

![ALT-text][10]

Im folgenden sehen Sie ein Beispiel für eine dieser API in C Nutzung möglicherweise#

            // Constructing the JSON message to use in API call to Rules API App

            // xmlInstance is the XML message instance to be passed as input to our Policy
            string xmlInstance = "<ns0:Patient xmlns:ns0=\"http://tempuri.org/XMLSchema.xsd\">  <ns0:Name>Name_0</ns0:Name>  <ns0:Email>Email_0</ns0:Email>  <ns0:PatientID>PatientID_0</ns0:PatientID>  <ns0:Age>10.4</ns0:Age>  <ns0:Claim>    <ns0:ClaimDate>2012-05-31T13:20:00.000-05:00</ns0:ClaimDate>    <ns0:ClaimID>10</ns0:ClaimID>    <ns0:TreatmentID>12</ns0:TreatmentID>    <ns0:ClaimAmount>10000.0</ns0:ClaimAmount>    <ns0:ClaimStatus>ClaimStatus_0</ns0:ClaimStatus>    <ns0:ClaimStatusReason>ClaimStatusReason_0</ns0:ClaimStatusReason>  </ns0:Claim></ns0:Patient>";

            JObject input = new JObject();

            // The JSON object is to be of form {"<XMLSchemName>_<RootNodeName>":"<XML Instance String>".
            // In the below case, we are using XML Schema - "insruanceclaimsschema" and the root node is "Patient".
            // This is CASE SENSITIVE.
            input.Add("insuranceclaimschema_Patient", xmlInstance);
            string stringContent = JsonConvert.SerializeObject(input);


            // Making REST call to Rules API App
            HttpClient httpClient = new HttpClient();

            // The url is the Host URL of the Rules API App
            httpClient.BaseAddress = new Uri("https://rulesservice77492755b7b54c3f9e1df8ba0b065dc6.azurewebsites.net/");
            HttpContent httpContent = new StringContent(stringContent);
            httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json");

            // Invoking API "Execute" on policy "InsruranceClaimPolicy" and getting response JSON object. The url can be gotten from the API Definition Lens
            var postReponse = httpClient.PostAsync("api/Policies/InsuranceClaimPolicy?comp=Execute", httpContent).Result;

Beachten Sie, dass die obigen Regeln API App seine Sicherheitseinstellungen auf "Öffentliche Anon" festgelegt wurde. Dies kann mit festgelegt werden von innerhalb der App API – alle Einstellungen-Anwendungseinstellungen > -> Zugriffsebene.

![ALT-text][11]

## <a name="editing-vocabulary-and-policy"></a>Begriffe und Richtlinie bearbeiten
Einer der Hauptvorteile der Verwendung von Business ist, dass die Änderungen von Geschäftslogik zu viel schneller Herstellung abgelegt werden können. Jede Änderung an Begriffe und Richtlinien wird in der Herstellung sofort angewendet. Benutzer muss einfach navigieren Sie zu der jeweiligen Vokabulardefinition Richtlinie, und nehmen Sie die Änderung, damit sie wirksam.

<!--Image references-->
[1]: ./media/app-service-logic-use-biztalk-rules/InsuranceScenario.PNG
[2]: ./media/app-service-logic-use-biztalk-rules/InsuranceBusinessLogic.png
[3]: ./media/app-service-logic-use-biztalk-rules/CreateRuleApiApp.png
[4]: ./media/app-service-logic-use-biztalk-rules/RulesDashboard.png
[5]: ./media/app-service-logic-use-biztalk-rules/LiteralVocab.PNG
[6]: ./media/app-service-logic-use-biztalk-rules/XMLVocab.PNG
[7]: ./media/app-service-logic-use-biztalk-rules/BulkAdd.PNG
[8]: ./media/app-service-logic-use-biztalk-rules/PolicyList.PNG
[9]: ./media/app-service-logic-use-biztalk-rules/RuleCreate.PNG
[10]: ./media/app-service-logic-use-biztalk-rules/APIDef.PNG
[11]: ./media/app-service-logic-use-biztalk-rules/PublicAnon.PNG
