<properties
    pageTitle="Fügen Sie Angaben zur Anmeldung und Access-Systemsteuerung Seiten Firma hinzu"
    description="Informationen Sie zum Hinzufügen von einem Unternehmen die Azure-Anmeldeseite und der Access-Seite Panel branding"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/30/2016"
    ms.author="curtand"/>

# <a name="add-company-branding-to-your-sign-in-and-access-panel-pages"></a>Fügen Sie Angaben zur Anmeldung und Access-Systemsteuerung Seiten Firma hinzu


Der Übersichtlichkeit möchten anwenden ein einheitliches Aussehen und Verhalten für alle Websites und Diensten, die sie verwalten viele Unternehmen. Azure-Active Directory bietet diese Möglichkeit, sodass Sie die Darstellung der folgenden Webseiten mit Ihrem Firmenlogo und benutzerdefinierte Farbschemas anzupassen:

- **Anmeldeseite** – Dies ist die Seite, die angezeigt wird, wenn Sie melden Sie sich bei Office 365 oder einer anderen webbasierten Anwendung, die als Ihre Identitätsanbieter Azure AD verwenden. Interaktion mit dieser Seite entweder während einer Home-Bereich Erkennung oder geben Sie Ihre Anmeldeinformationen ein. Der Start-Bereich Suche kann das System partnerverbundkontakte Benutzer in ihrer lokalen STS (z. B. AD FS) umgeleitet.

- **Seite Access Panel** - das Access-Panel ist ein Web-basierte Portal, mit dem Sie zum Anzeigen und starten die Cloud-basierten Programme, die durch denen Ihre Azure AD-Administrator Zugriff auf erteilt hat. Um die Access-Bereich zu gelangen, verwenden Sie den folgenden URL: [https://myapps.microsoft.com](https://myapps.microsoft.com).

In diesem Thema wird erläutert, wie Sie die Anmeldeseite und der Access-Seite Panel anpassen können.

> [AZURE.NOTE]
>
- Branding Unternehmen ist ein Feature, das ist nur verfügbar, wenn Sie auf die Premium oder Basisversion von Azure Active Directory aktualisiert haben oder Office 365-Benutzer sind. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).
- Azure Active Directory Premium und grundlegende Editionen sind verfügbar für Kunden in China mithilfe der weltweiten Instanz von Azure Active Directory. Azure Active Directory Premium und grundlegende Editionen werden in der Microsoft Azure-Dienst von 21Vianet in China betrieben wird derzeit nicht unterstützt. Weitere Informationen erreichen Sie uns das [Azure-Active Directory-Forum](https://feedback.azure.com/forums/169401-azure-active-directory/).



## <a name="customizing-the-sign-in-page"></a>Anpassen der Anmeldeseite

Wenn Sie browserbasierten Zugriff auf Cloud apps und Dienste benötigen, denen die Ihre Organisation abonnieren, verwenden Sie normalerweise die Anmeldeseite an.

Wenn Sie Änderungen auf der Anmeldeseite angewendet haben, können sie bis zu einer Stunde, damit die Änderungen angezeigt werden dauern.

Firmenspezifischen Anmeldeseite wird nur angezeigt, wenn Sie einen Dienst mit einem Mandanten-spezifische URL wie https://outlook.com/**Contoso**.com oder https://mail besuchen. **Contoso**. com.

Wenn Sie einen Dienst mit bestimmten URLs nicht Mandanten besuchen (z. B.: https://mail.office365.com), ein nicht-Branding-Anmeldeseite angezeigt wird. In diesem Fall wird Ihr branding, nachdem Sie Ihre Benutzer-ID eingegeben haben oder Sie eine Benutzerkachel ausgewählt haben.

> [AZURE.NOTE]
>
- Ihren Domänennamen muss als "Aktiv" angezeigt, in der **Active Directory** > **Verzeichnis** > Abschnitt**Domänen** des Portals Azure klassischen Stelle, an der Sie das branding konfiguriert haben.
- Branding-Anmeldeseite übertragen nicht auf der Anmeldeseite Consumer von Microsoft. Wenn Sie sich mit einem persönlichen Microsoft-Konto anmelden, wird möglicherweise eine firmenspezifischen Liste der Benutzer Kacheln von Azure AD gerendert, aber das branding Ihrer Organisation gilt nicht für die Microsoft-Konto-Anmeldeseite.


Wenn Sie auf dieser Seite Ihres Unternehmens Brandings, Farben und andere anpassbare Elemente anzeigen möchten, finden Sie unter die folgenden Bilder, um den Unterschied zwischen den beiden Erfahrung zu verstehen.

Die folgenden Screenshot zeigt und Beispiel für die Office 365-Anmeldeseite auf einen Desktopcomputer **vor** einer Anpassung:

![Office 365-Anmeldeseite vor der Anpassung][1]

Die folgenden Screenshot zeigt und Beispiel für die Office 365-Anmeldeseite auf eine Desktopcomputer, **nachdem** eine Anpassung:

![Office 365-Anmeldeseite nach Anpassung][2]

Der folgende Screenshot zeigt ein Beispiel für die Office 365-Anmeldeseite auf einem mobilen Gerät **vor** eine Anpassung zu:

![Office 365-Anmeldeseite vor der Anpassung][3]


Das folgende Bildschirmabbild zeigt ein Beispiel für die Office 365-Anmeldeseite auf einem mobilen Gerät **nach** einer Anpassung:

![Office 365-Anmeldeseite nach Anpassung][4]


Beim Ändern der Größe einer Browserfenster wird die große Abbildung, wie die Tabelle in früheren Versionen häufig zugeschnitten anderen Bildschirm Seitenverhältnis angepasst. Mit diesem Hintergrund sollten Sie versuchen, die wichtigsten visuellen Elemente in der Abbildung beibehalten, damit sie immer in der oberen linken Ecke (oben rechts für rechts-nach-links-Sprachen) angezeigt werden. Dies ist wichtig, da tritt auf, Ändern der Größe in der Regel in der unteren rechten Ecke rechts oben gezeigt / links oder von unten nach oben.

Die folgende Abbildung zeigt, wie die Abbildung zugeschnittene ist, wenn der Browser links Größe geändert wird:

![][6]

Hier ist, wie er angezeigt wird, nachdem der Browser Größe, klicken Sie oben geändert wurde:

![][7]

## <a name="what-elements-on-the-page-can-i-customize"></a>Welche Elemente auf der Seite können werden angepasst?

Sie können die folgenden Elemente auf der Anmeldeseite anpassen:

![][5]



| Seitenelements  | Position auf der Seite |
|:--            | ---                  |
|Banner-Logo    | In der oberen rechten Ecke der Seite angezeigt. Ersetzt das Logo der Zielwebsite, die Sie zu Monitoren (z. b Signieren sind. Office 365 oder Azure).|
|Große Abbildung / Hintergrundfarbe | Siehe am linken Seitenrand ein. Ersetzt das Bild der Zielwebsite, die zu Monitoren anmelden. Die Hintergrundfarbe möglicherweise anstelle der großen Abbildung Verbindungen mit niedriger Bandbreite oder schmale Bildschirme angezeigt.|
|Angemeldet bleiben | Klicken Sie unter dem Textfeld Kennwort angezeigt. |
|Anmeldung Seitentext | Über die Seitenfuß angezeigt, wenn Sie hilfreiche Informationen, bevor Sie eine Anmeldung mit einem geschäftlichen oder schulnotizbücher Konto vermitteln müssen. Beispielsweise, wenn Sie die Telefonnummer an den Helpdesk, oder einen rechtlichen Hinweis einbeziehen möchten.|


> [AZURE.NOTE]
Alle Elemente sind optional. Wenn Sie ein Logo Banner aber keine großen Abbildung angeben, zeigt die Anmeldeseite beispielsweise das Logo und die Abbildung der Zielwebsite (d. h., das Office 365 Kalifornien Autobahnen Bild).


Klicken Sie auf der Anmeldeseite ermöglicht das Kontrollkästchen **angemeldet bleiben** einen Benutzer angemeldet zu bleiben, wenn sie schließen und erneut öffnen Sie ihren Browser. Es hat keinen Einfluss auf Sitzung Lebensdauer. Sie können das Kontrollkästchen in der Azure-Active Directory-Anmeldeseite ausblenden.

Gibt an, ob das Kontrollkästchen angezeigt wird, hängt von der Einstellung für **KMSI ausblenden**.

![][9]


Wenn Sie das Kontrollkästchen ausblenden, konfigurieren Sie diese Einstellung, damit **ausgeblendet**. 

> [AZURE.NOTE] Einige Features von SharePoint Online und Office 2010 abhängig Benutzer können dieses Kontrollkästchen aktivieren. Wenn Sie diese Einstellung, um ausgeblendete konfiguriert haben, wird möglicherweise Ihre Benutzer zusätzliche – und unerwartet Aufforderung zum Anmelden angezeigt.




Sie können auch alle Elemente auf dieser Seite lokalisiert. Nachdem Sie einen Satz "Standard" Anpassung Elemente konfiguriert haben, können Sie mehrere Versionen für unterschiedliche Gebietsschemas konfigurieren. Sie können auch mischen und mit verschiedenen Elementen überein. Beispielsweise können Sie:

- Erstellen einer großen Abbildung, funktioniert für alle Kulturen, erstellen Sie anschließend bestimmte Versionen für Englisch und Französisch "Standard". Wenn Sie Ihr Browser auf eine dieser beiden Sprachen festlegen, wird das bestimmte Bild angezeigt, während die Abbildung Standard für alle anderen Sprachen angezeigt wird.

- Konfigurieren Sie unterschiedliche Logos für Ihre Organisation (z. B. Japanisch oder Hebräisch Versionen) ein.



## <a name="access-panel-page-customization"></a>Access Systemsteuerung Seite zur Anpassung

Die Access-Bereich Seite ist im Wesentlichen eine Portalseite für schnellen Zugriff auf die Cloud-apps, die Ihnen Zugriff auf vom Administrator erteilt wurden. Auf dieser Seite werden Sie Ihre apps als anklickbare Anwendung Kacheln angezeigt.


Das folgende Bildschirmabbild zeigt ein Beispiel für eine Access-Seite Panel nach Anpassung.

![][8]

## <a name="configure-your-directory-with-company-branding"></a>Konfigurieren von Ihrem Verzeichnis mit Unternehmen branding

Sie können einen Standardsatz von anpassbare Elemente pro Verzeichnis im klassischen Azure-Portal konfigurieren. Nachdem Sie die Standardeinstellungen gespeichert haben, kann ein Administrator lokalisierte Versionen von jedes Element, für verschiedene Sprachen hinzufügen / Gebietsschemas. Alle anpassbare Elemente sind optional.

Wenn Sie einen Standardwert Banner Logo, aber keine großen Abbildung konfiguriert haben, zeigt die Anmeldeseite des Logos angenommen, in der oberen rechten Ecke. Es wird jedoch die standardmäßige Abbildung der Website angezeigt.

Stellen Sie vor der folgenden Konfiguration:

- Eine Standard-Banner Logo und Anmeldung Seitentext nur auf Englisch verfügbar
- Eine sprachspezifische anmelden Seitentext für Deutsch

Ist Ihre bevorzugte Sprache auswählen auf Deutsch, erhalten Sie die Standardeinstellung Banner Logo jedoch der Deutsch Text an.

Während Sie eine andere Gruppe für die einzelnen Sprachen von Azure AD unterstützt technisch konfigurieren konnten, empfehlen wir, dass die Anzahl der Variationen, Wartung und Leistung Gründen, gering halten.

**Wenn Unternehmen branding bis hin zu Ihrem Verzeichnis hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei der [Azure klassischen Portal](https://manage.windowsazure.com) als Administrator des Verzeichnisses, die Sie anpassen möchten.
2. Wählen Sie das Verzeichnis aus, die, das Sie anpassen möchten.
3. Klicken Sie oben auf der Symbolleiste auf **Konfigurieren**.
4. Klicken Sie auf **Branding anpassen**.
4. Ändern Sie die Elemente, die Sie anpassen möchten. Alle Felder sind optional.
5. Klicken Sie auf **Speichern**.

Sie können in in eine Stunde für neue Änderung Anspruch vorgenommenen der Anmeldung Seite branding angezeigt werden.

**Wenn sprachspezifischen Unternehmen branding hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei der [Azure klassischen Portal](https://manage.windowsazure.com) als Administrator des Verzeichnisses, die Sie anpassen möchten.
2. Wählen Sie das Verzeichnis aus, die, das Sie anpassen möchten.
3. Klicken Sie oben auf der Symbolleiste auf **Konfigurieren**.
4. Klicken Sie auf **Branding anpassen**.
2. Klicken Sie auf **branding für eine bestimmte Sprache hinzufügen**.
3. Wählen Sie die Sprache, der Sie das Logo für anpassen möchten, und klicken Sie dann auf **Weiter**.
3. Bearbeiten Sie nur die Elemente, für die Sie die sprachspezifischen überschreibt konfigurieren möchten. Alle Felder sind optional. Wenn ein Feld leer bleibt, und klicken Sie dann der benutzerdefinierten Standardwert stattdessen angezeigt wird (oder den Microsoft-Standardwert, wenn ein benutzerdefinierter Standard konfiguriert ist).
4. Klicken Sie auf **Speichern**.

**Wenn Unternehmen branding aus Ihrem Verzeichnis entfernen möchten, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei der [Azure klassischen Portal](https://manage.windowsazure.com) als Administrator des Verzeichnisses, die Sie anpassen möchten.
2. Wählen Sie das Verzeichnis aus, die, das Sie anpassen möchten.
3. Klicken Sie oben auf der Symbolleiste auf **Konfigurieren**.
4. Klicken Sie auf **Branding anpassen**.
5. Klicken Sie auf der Seite Branding anpassen wählen Sie **Vorhandene Branding Einstellungen bearbeiten** aus, und wechseln Sie zur nächsten Seite.
3. Je nachdem, welche Elemente, die Sie entfernen möchten, führen Sie eine oder mehrere der folgenden Aktionen aus:

    ein. Wählen Sie unter **Banner Logo** **Entfernen hochgeladen Logo**ein.

    b. Wählen Sie unter **Kachel Logo** **Entfernen hochgeladen Logo**ein.

    c. Entfernen Sie den Text von allen Textfelder ein.

    d. Klicken Sie auf **Weiter**.

    e. Entfernen Sie den Text von allen Textfelder ein.

4. Klicken Sie auf **Speichern** , um die Elemente zu entfernen.
5. Falls erforderlich, klicken Sie auf **Branding anpassen** erneut und wiederholen Sie diese Schritte für alle sprachspezifischen branding, der entfernt werden muss.
    Wenn Sie auf **Branding anpassen** , und finden Sie im Formular **Anpassen Branding Standard** mit keine vorhandenen Einstellungen konfiguriert wurden alle branding Einstellungen entfernt.

## <a name="testing-and-examples"></a>Testen und Beispiele

Es empfiehlt sich, dass Sie mit einem Test-Mandanten experimentieren, bevor Änderungen vorgenommen werden in Ihrem Unternehmen.

**So überprüfen, ob Ihr branding angewendet wurde:**

1. Öffnen Sie eine Browsersitzung InPrivate oder Incognito.
2. Besuchen Sie https://outlook.com/contoso.com, "contoso.com" durch die Domäne, die von Ihnen angepasste ersetzt werden.

Dies funktioniert auch mit Domänen, die wie contoso.onmicrosoft.com aussehen.

Legt fest, wenn Sie eine effektive Anpassung erstellen können, wir die folgenden beiden fiktiven Anmeldung Seiten angepasst haben:

- [http://AKA.ms/aaddemo001](http://aka.ms/aaddemo001)
- [http://AKA.ms/aaddemo002](http://aka.ms/aaddemo002)

Klicken Sie zum Testen der sprachspezifischen Einstellungen müssen Sie die standardmäßigen spracheinstellungen in Ihrem Webbrowser auf eine andere Sprache zu ändern, wie Sie in der Anpassung festgelegt haben. Dies konfigurieren im Menü **Internetoptionen** in Internet Explorer.

## <a name="customizable-elements"></a>Anpassbare Elemente

Einige anpassbare Elemente in Azure AD haben mehrere Fälle von verwenden. Sie können Firmenlogos einmal pro Directory konfigurieren und auf, Anmeldung sowohl die Access-Systemsteuerung Seiten verwendet werden. Einige anpassbare Elemente sind nur auf der Anmeldeseite. Die folgende Tabelle enthält die Details für die verschiedenen anpassbare Elemente.

Namen | Beschreibung | Einschränkungen | Empfehlungen
    ------------- | ------------- | ------------- | -------------
Banner-Logo | Klicken Sie auf der Anmeldeseite und der Access-Bereich wird das Logo Banner angezeigt. | <p>JPG oder PNG</p><p>60 x 280 Pixel</p><p>10 KB</p> | <p>Verwenden von Ihrer Organisation vollständigen Logos (einschließlich Piktogramm und Arealstaaten dieser)</p><p>Behalten Sie klicken Sie unter 30 Pixel hoch, um die Einführung in Scrollbars auf mobilen Geräten vermeiden bei</p><p>Halten Sie es unter 4 KB</p><p>Verwenden eines transparenten PNG (nie annehmen, dass die Anmeldeseite immer einen weißen Hintergrund hat)</p>
Kachel-Logo | (zurzeit nicht verwendet in der Anmeldeseite) In der Zukunft möglicherweise dieser Text verwendet werden, die generische "Arbeit oder Schule Konto" Piktogramm an verschiedenen Stellen die Erfahrung zu ersetzen. | <p>JPG oder PNG</p><p>120 x 120 Pixel</p><p>10 KB</p> | <p>Behalten sie einfache (keine kleine Schrift), dieses Bild auf 50 % angepasst werden kann
</p> |
Bezeichnung für Benutzer anmelden Seite | (zurzeit nicht verwendet in der Anmeldeseite) In der Zukunft möglicherweise dieser Text verwendet werden, die generische "Arbeit oder Schule Konto" Zeichenfolge an verschiedenen Stellen die Erfahrung ersetzt. Sie können auf einen anderen Wert wie "" Contoso "Firma" oder "Contoso-ID" festlegen | <p>Unicode-Text, bis zu 50 Zeichen</p><p>Nur-Text (keine Links oder HTML-Tags)</p> | <p>Kurze und einfache beibehalten</p><p>Bitten Sie die Benutzer aus, wie sie normalerweise finden Sie in der Arbeit oder Schule Konto aus, das Sie diese mit bereitstellen.</p>
Anmeldung Seitentext | Diese "Textbaustein" wird unter dem Formular-Anmeldeseite angezeigt und kann verwendet werden, um zusätzliche Anweisungen oder Abrufen von Hilfe und Support, wo kommunizieren. | <p>UnicodeText, bis zu 256 Zeichen</p><p>Nur-Text (keine Links oder HTML-Tags)</p> | Klicken Sie unter 250 Zeichen (etwa 3 Textzeilen) beibehalten
Abbildung der Seite anmelden | Die Abbildung ist ein großes Bild, das auf der Anmeldeseite auf der linken Seite des Formulars-Anmeldeseite angezeigt wird. | <p>JPG oder PNG</p><p>1420 x 1.200</p><p>500 KB</p> | <p>1420 x 1.200 Pixel</p><p>Wichtig: Beibehalten kleinstmöglichen, idealerweise unter 200 KB. Wenn diese Abbildung zu groß ist, ist die Leistung von der Anmeldeseite beeinträchtigt, wenn das Bild Cache nicht zur Verfügung</p><p>Diese Abbildung ist häufig zugeschnitten, um verschiedene Bildschirm Verhältnissen aufnehmen zu können. Die visuellen Elemente in der oberen Ecke (oben rechts für Sprachen mit Schreibrichtung), linken da tritt auf, aus der unten rechts Ecke rechts oben gezeigt Ändern der Größe beibehalten / links, wie, das Browserfenster verkleinert.</p>
Hintergrundfarbe der Seite anmelden | Die Hintergrundfarbe der Anmeldeseite wird in den Bereich links des Formulars Anmeldeseite verwendet. | Muss eine RGB-Farbe in die hexadezimale Form (Beispiel: #FFFFFF) | <p>Die Hintergrundfarbe möglicherweise anstelle der großen Abbildung auf Verbindungen mit niedriger Bandbreite angezeigt werden</p><p>Wir empfehlen, die primäre Textfarbe des Logos Banner auswählen</p>


## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte mit Azure Active Directory Premium](active-directory-get-started-premium.md)
- [Zeigen Sie Ihrer Berichte Zugriff und Verwendung an](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-add-company-branding/SignInPage_beforecustomization.png
[2]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization.png
[3]: ./media/active-directory-add-company-branding/SignInPage_mobile_beforecustomization.png
[4]: ./media/active-directory-add-company-branding/SignInPage_mobile_aftercustomization.png
[5]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_elements.png
[6]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedleft.png
[7]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedtop.png
[8]: ./media/active-directory-add-company-branding/APBranding.png
[9]: ./media/active-directory-add-company-branding/hidekmsi.png
