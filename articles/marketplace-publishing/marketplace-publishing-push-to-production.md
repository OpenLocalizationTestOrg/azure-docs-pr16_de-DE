<properties
   pageTitle="Ihr Angebot zum Azure Marketplace bereitstellen | Microsoft Azure"
   description="Erfahren Sie mehr über, und zeigen Sie den Anweisungen, um Ihr Angebot – Bereitstellen von virtuellen Computerabbild, Developer Service, Datendienst usw. – zum Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/02/2016"
   ms.author="hascipio" />

# <a name="deploy-your-offer-to-the-azure-marketplace"></a>Bereitstellen von Ihr Angebot zum Azure Marketplace
Wenn Sie zufrieden sind mit Ihr Angebot (d. h., Sie haben überprüft Kundenszenarien, marketing-Inhalte usw.) und Sie sind bereit, um zu starten, anfordern **zu Herstellung und drücken Sie** auf der Registerkarte **Veröffentlichen** .  

1. Die vier Schritte unter der exemplarische Vorgehensweise Seite in der Veröffentlichung Portal sollten fertigen und Grün. Für virtuellen Computern Angebote stellen Sie sicher, dass die folgenden Richtlinien eingehalten werden.

    ![Zeichnung][img-pubportal-walkthru-checked]

2. Wählen Sie aus der Liste auf der linken Seite die Registerkarte **Veröffentlichen** aus.

    ![Zeichnung][img-pubportal-menu-publish]

3. Klicken Sie auf die Schaltfläche **Anfordern der Genehmigung an Herstellung übertragen**. Nachdem die Anforderung gesendet wurde, das Team Genehmigung führt eine Korrekturlesen, und klicken Sie dann Ihr Angebot wird zur Verfügung, in dem Azure Marketplace.

    ![Zeichnung][img-pubportal-publish-pushproduction]

>[AZURE.IMPORTANT] Beim Klicken auf die Schaltfläche Genehmigung anfordern, um an der Herstellung, übertragen werden die folgenden Schritte bei virtuellen Computern hinter der Szene ausgeführt. Sie werden möglicherweise den Fortschritt der einzelnen Schritte unter der Registerkarte Veröffentlichen in der Veröffentlichung anzeigen Portal. Überprüfen Sie diese Seite in regelmäßigen Abständen (bis sich der Status "Liste aufgeführten" zeigt) Fehler Informationen, die Korrektur von Ihrer Seite aus benötigen.

> - Auf den ersten, geht die Herstellung Anforderung an Zertifizierung Teams, die die virtuelle Festplatte zu überprüfen. Jedoch ist Sie Ihr Angebot bereits aufgeführte aktualisieren, und die Anforderung wurde eingerichtet nur marketing ändern, klicken Sie dann der Zertifizierung, Schritt ausgelassen.
> - Bei dem nächsten Schritt fort die Anfrage hier Content-Prüfung Teams, die den marketing Inhalt des Angebots zu überprüfen.
> - Wenn die obigen Schritte erfolgreich sind, wird das Angebot in Herstellung genehmigt. Zu diesem Zeitpunkt der Status machen "aufgeführt" in den Veröffentlichungsportal. Dieser Status "Liste aufgeführten" bedeutet jedoch nicht, dass der Vorgang abgeschlossen ist. Die folgenden Schritte müssen abgeschlossen sein, bevor das Angebot verfügbar, in dem Azure Marketplace ist.
> - Nachdem das Angebot produziert in den vorstehenden Schritt genehmigt wurde, die Replikation des Angebots über alle den Azure Datencentern starten. Im Allgemeinen dauert 24-48hours für die Replikation abgeschlossen, aber es kann bis zu eine Woche, abhängig von der Größe der virtuellen Festplatte dauern. Ist jedoch wenn Sie Ihr Angebot bereits aufgeführte aktualisieren und weist das richtige nur marketing ändern, klicken Sie dann die Replikation schneller.
> - Klicken Sie nach Abschluss die Replikation wird dann das Angebot der Azure Marketplace verfügbar.

> Sie können das Angebot immer löschen, während sie einen **Entwurf** Status (d. h., nie **drücken, um das Staging** oder **Pushbenachrichtigungen zu Herstellung**) wird. Klicken Sie auf die Schaltfläche **Entwurf verwerfen** am unteren Rand der Seite zum Löschen eines Entwurfs, klicken Sie auf die Registerkarte **Verlauf** .


## <a name="production-checklist-for-all-virtual-machine-offers"></a>Herstellung Checkliste für alle virtuellen Computern Angebote

- Stellen Sie sicher, dass Sie eine Microsoft Azure zertifizierten Partner sind
- Klicken Sie auf der Registerkarte SKUs sollte die Option "Ausblenden dieser SKU aus dem Marketplace, da es immer über eine Lösungsvorlage erworben werden sollte" als "Ja" gekennzeichnet werden nur, wenn die SKU Teil einer Lösungsvorlage ist. In allen anderen Fällen sollte diese Option immer als Nein markiert werden
- Denken Sie daran: Sollten nicht geändert werden die SKU Sichtbarkeit festlegen, nachdem die SKU aufgeführt wird. Dieses Feature wird nicht unterstützt.
- Stellen Sie sicher, dass die Logos an die nachfolgenden Azure Marketplace-Logo-Richtlinien halten.
- Angebot und SKU Beschreibung dürfen nicht identisch sein.
- Titel des SKU und bieten lange Zusammenfassung dürfen nicht identisch sein.
- SKU Titel und Zusammenfassung anbieten dürfen nicht identisch sein.
- SKU Titel sollten nicht für ein Angebot mit mehreren SKUs identisch sein.

**Azure Marketplace-Logo-Richtlinien**

- Das Design Azure weist eine einfache Farbpalette. Halten Sie die Anzahl der primären und sekundären Farben auf Ihr Logo niedrig.
- Designfarben des Portals Azure sind weiß und Schwarz. Daher vermeiden Sie diese Farben als die Hintergrundfarbe für Ihre Logos. Verwenden Sie einige Farbe, die Ihre Logos Azure-Portal herausragender vornehmen möchten. Wir empfehlen einfache primär Farben aus. Wenn Sie mit transparenten Hintergrund arbeiten, stellen Sie sicher, dass der Logo/Text nicht weiß oder schwarz dargestellt ist.
- Verwenden Sie einen Hintergrund mit Farbverlauf nicht auf das Logo ein.
- Platzieren von Text, auch Ihre Firma oder Marke Name, klicken Sie auf das Logo zu vermeiden.
- Das Aussehen und Verhalten Ihres Logos Farbverläufe vermeiden und sollten 'flachen'.
- Das Logo sollte nicht gestreckt werden.

**Weitere Richtlinien für das Logo Hero:**

- Das Logo Hero ist optional. Herausgeber kann nicht zu einem Hero Logo Hochladen auswählen. **Jedoch einmal hochgeladene das Symbol Hero kann nicht gelöscht werden, aus der Veröffentlichung Portal. Zu diesem Zeitpunkt muss des Partners die Azure Marketplace-Richtlinien für Hero Symbole sonst, die das Angebot nicht genehmigt werden, werden, in der Herstellung folgen.**
- Weiß Schriftfarbe werden der Anzeigename Publisher, SKU Titel und lange Sammelvorgang Angebot angezeigt. Daher sollten Sie einen hellen Farbe im Hintergrund des Symbols Hero planmäßigen an. Schwarz, weißem und mit transparentem Hintergrund ist für Hero Symbole nicht zulässig.
- Herausgeber anzeigen Name, SKU Titel, lange Sammelvorgang Angebot und die Schaltfläche erstellen werden in einem Logo Hero programmgesteuert eingebettet, sobald das Angebot aufgeführten wird. Daher sollten Sie nicht Text eingeben, während Sie das Logo Hero entwerfen. Lassen Sie einfach leeren Bereich auf der rechten Seite, da der Text (d. h. Anzeigen von Publisher benennen, SKU Titel, lange Sammelvorgang Angebot) wird über dort programmgesteuert durch us-enthalten sein. Der leere Bereich für den Text sollten 415 x 100 auf der rechten Seite (und es wird Versatz durch 370px von links).


## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Bietet zusätzliche Herstellung Checkliste für bereits aufgeführten virtuellen Computern

- Überprüfen Sie, ob ein Angebot mit demselben Namen aus Ihrem Unternehmen Angebot bereits vorhanden ist. Wenn Ja, sollten Sie eine neue Version von der SKU in das vorhandene Angebot statt Erstellen eines neuen doppelten Angebots hinzufügen.
- Datenträger sollten nicht zwischen zwei Versionen des gleichen SKU ändern.
- Der Azure Marketplace unterstützt keine Preisgestaltung Ändern der aufgeführten SKU, da er die Rechnung an den vorhandenen Kunden beeinflusst. Stellen Sie sicher, dass die Preise der aufgeführten SKU in den Einsatzbereich die SKU Regionen nicht geändert wird. Sie können jedoch neue SKUs hinzufügen oder neue Bereiche einer vorhandenen SKU hinzufügen.


## <a name="next-steps"></a>Nächste Schritte
Nachdem das Angebot online geht, testen Sie die Kundenszenarien, um zu überprüfen, dass alle die Verträge und-Funktionen in dieser Umgebung als ordnungsgemäß funktioniert getestet und in das staging-Umgebung.

## <a name="see-also"></a>Siehe auch
- [Erste Schritte: So veröffentlichen ein Angebots zu Azure Marketplace](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
