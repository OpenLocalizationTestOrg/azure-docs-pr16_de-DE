<properties
   pageTitle="Zeile Sicherheitsstufe mit Power BI eingebettete"
   description="Details zu Zeile Sicherheitsstufe mit Power BI eingebettete"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="row-level-security-with-power-bi-embedded"></a>Zeile, die mit Power BI eingebettete Sicherheit auf Datensatzebene

Zeile Ebene Sicherheit (RLS) kann zum Einschränken des Benutzerzugriffs mit bestimmten Daten in einem Bericht oder ein Dataset, gleicht für mehrere andere Benutzer mit den gleichen Bericht, während alle anderen Anzeige-Daten verwendet werden. Power BI eingebettete unterstützt jetzt Datasets mit RLS konfiguriert.

![](media\power-bi-embedded-rls\pbi-embedded-rls-flow-1.png)

Um RLS nutzen zu können, ist es wichtig, dass Sie die drei wichtigsten Konzepte kennen. Benutzer, Rollen und Regeln. Sehen wir uns näher an jedem:

**Benutzer** – Hierbei handelt es sich um den tatsächlichen Endbenutzer Berichte anzeigen. In Power BI eingebettete werden Benutzer durch die Username-Eigenschaft in einer App Token identifiziert.

**Rollen** – Benutzer, die Rollen gehören. Eine Rolle ist ein Container für Regeln und kann werden mit Namen wie "Sales Manager" oder "Vertriebsmitarbeiter". In Power BI eingebettete werden Benutzer von der Rollen-Eigenschaft in einer App Token identifiziert.

**Regeln** – Rollen haben Regeln, und diese Regeln sind die ist-Filter, die die Daten angewendet werden soll. Dies ist ganz einfach als "Land = USA" oder etwas dynamischeren.

### <a name="example"></a>Beispiel

Im weiteren Verlauf dieses Artikels können wir erläutern die notwendigen Beispiel RLS authoring, und klicken Sie dann Verarbeitung, die innerhalb einer eingebetteten Anwendungs. Beispiel wird die Beispieldatei [Retail Analyse](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX verwendet.

![](media\power-bi-embedded-rls\pbi-embedded-rls-scenario-2.png)

Unsere Retail Analysis-Beispiel zeigt die Umsätze für alle Speicher in einer bestimmten Retail Kette. Ohne RLS, unabhängig davon, welche Region Manager anmeldet, und klicken Sie im Bericht Ansichten, wird derselben Daten angezeigt. Geschäftsleitung hat festgelegt, jeder Region-Manager sollte nur die Umsätze für die, die sie verwalten Speicher angezeigt werden, und dazu wir RLS verwenden können.

RLS in Power BI-Desktop erstellt wurde. Wenn das Dataset und der Bericht geöffnet werden, können wir zu Diagrammansicht auf das Schema finden Sie unter wechseln:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-3.png)

Hier sind ein paar Punkte, die mit diesem Schema zu beachten:

-   Alle Measures, wie **Total Sales**, werden in der Faktentabelle **Sales** gespeichert.
-   Es gibt vier zusätzliche verwandte Dimensionstabellen: **Element**, **Uhrzeit**, **Store**und **Region**.
-   Die Pfeile an den Beziehungslinien anzugeben, wie Filter aus einer Tabelle in eine andere versendet werden können. Angenommen, wenn ein Filter auf **Zeit [Datum]**platziert wurde, würde im aktuellen Schema er nur nach unten Werte in der Tabelle " **Sales** " filtern. Keine anderen Tabellen sind von diesem Filter betroffen, da alle die Pfeile an den Beziehungslinien zeigen Sie in der Tabelle sales und nicht abwesend.
-   Der Tabelle " **Region** " gibt an, wer der Manager für jede ist:

    ![](media\power-bi-embedded-rls\pbi-embedded-rls-district-table-4.png)

Anhand dieses Schemas aus, wenn wir eines Filters auf die Spalte " **Region-Manager** " in der Tabelle "Region anwenden", und wenn, die der Benutzer den Bericht anzeigen Treffer filtern, dieses Filters werden außerdem filtern, nach unten **Store** und **Sales** Tabellen nur Daten für diese bestimmten Region-Manager angezeigt werden.

So sieht wie:

1.  Klicken Sie auf der Registerkarte Modellierung auf **Rollen verwalten**.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-modeling-tab-5.png)

2.  Erstellen einer neuen Rolle namens **-Manager**.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-6.png)

3.  Geben Sie den folgenden DAX-Ausdruck in der Tabelle **Region** : **[Region Manager] = USERNAME()**  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-7.png)

4.  Klicken Sie auf **Ansicht als Rollen**, und geben Sie vor, um sicherzustellen, dass die Regeln, klicken Sie auf der Registerkarte **Modeling** arbeiten:  
![](media\power-bi-embedded-rls\pbi-embedded-rls-view-as-roles-8.png)

    Die Berichte werden nun Daten angezeigt, als wäre Sie als **Andrew Ma**signiert wurden.

Anwenden des Filters, wird verarbeitet haben wir hier gefiltert werden alle Datensätze in der **Region**, **Store**und **Umsatz** -Tabellen. Allerdings werden aufgrund der Filters Richtung auf die Beziehungen zwischen **Sales** und die **Uhrzeit**, Tabellen **Sales** und **Element**, und **Element** und **Zeit** nicht unten gefiltert.

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-9.png)

Für diese Anforderung ok sein können, jedoch wenn wir nicht möchten, dass Elemente angezeigt, für die Benutzer Verkäufe verfügt nicht-Manager, wir konnte bidirektionaler kreuzfiltern für die Beziehung aktivieren und Datenfluss der Sicherheitsfilters zweiseitiger. Dies kann erfolgen, indem Sie die Beziehung zwischen **Sales** und **Element**, wie folgt:

![](media\power-bi-embedded-rls\pbi-embedded-rls-edit-relationship-10.png)

Nun können Filter auch aus der Tabelle "Sales" in der Tabelle **Artikel** Datenfluss:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-11.png)

**Notiz** Wenn Sie DirectQuery-Modus für Ihre Daten verwenden, müssen Sie aktivieren bidirektionaler-Cross filtern, indem Sie diese beiden Optionen auswählen:

1.  **Datei** -> **Optionen und Einstellungen** -> **Features Preview** -> **cross zweiseitiger für DirectQuery Filtern aktivieren**.
2.  **Datei** -> **Optionen und Einstellungen** -> **DirectQuery** -> **Zulassen der uneingeschränkter Measure im DirectQuery-Modus**.


Weitere Informationen zum bidirektionaler kreuzfiltern herunterladen Sie das [bidirektionale kreuzfiltern in SQL Server Analysis Services 2016 und Power BI-Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) -whitepaper

Dies schließt die gesamte Arbeit, die in Power BI-Desktop ausgeführt werden muss, aber vorhanden ist, dass eine weitere Aufgabe erfüllen, die ausgeführt werden, um die RLS machen muss Regel, dass wir Arbeit im Power BI eingebettete definiert. Benutzer authentifiziert und von der Anwendung berechtigt sind und App Token werden verwendet, um diesem Benutzer den Zugriff auf einen bestimmten Power BI eingebetteten Bericht erteilen. Power BI eingebettete bestimmte Informationen keinen auf, wer Ihre Benutzer ist. Für RLS entwickelt müssen Sie einige zusätzlichen Kontext als Teil Ihrer app Token übergeben:
-   **Benutzername** (optional) – zusammen mit RLS Dies ist eine Zeichenfolge, die zum Identifizieren den Benutzer beim Anwenden von Regeln RLS verwendet werden kann. Finden Sie unter Verwenden von Sicherheit auf Datensatzebene mit Power BI eingebettete Zeile
-   **Rollen** – eine Zeichenfolge, die Rollen Auswahl beim Anwenden von Regeln Sicherheitsstufe Zeile enthält. Wenn mehr als eine Rolle weitergegeben werden, sollten sie als Zeichenfolgenarray übergeben werden.

Wenn die Username-Eigenschaft vorhanden ist, müssen Sie mindestens einen Wert auch in Rollen übergeben.

Das vollständige app Token sieht ungefähr wie folgt aus:

![](media\power-bi-embedded-rls\pbi-embedded-rls-app-token-string-12.png)

Nun für alle Teile nicht trennen zwar wenn Benutzer bei der Anmeldung in unserer Anwendung zum Anzeigen dieses Berichts, diese nur können Daten sehen, die sie angezeigt, dürfen wie unsere Zeile Sicherheitsstufe definiert.

![](media\power-bi-embedded-rls\pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Siehe auch
[Sicherheit (RLS) mit Power Zeile auf](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)
