<properties
    pageTitle="Häufig gestellte Fragen: Azure AD-Kennwort Management | Microsoft Azure"
    description="Häufig gestellte Fragen (FAQ) zur Verwaltung der Kennwörter in Azure AD, einschließlich Kennwort zurückgesetzt, Registrierung, Berichte und abgeschlossenen writebackvorgängen zu lokalen Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="password-management-frequently-asked-questions"></a>Verwaltung der Kennwörter häufig gestellte Fragen

> [AZURE.IMPORTANT] **Sie hier sind, weil Sie Probleme beim Anmelden haben?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).

Im folgenden werden einige häufig gestellten Fragen für alle Elemente im Zusammenhang mit der Verwaltung der Kennwörter.

Wenn Sie sich mit einer Frage finden, die Sie nicht wissen, die Antwort auf, oder sind benötigen Sie Hilfe für ein bestimmtes Problem, das Sie gegenüberliegende sind, können Sie auf unter lesen, um festzustellen, ob es bereits verdeckt werden haben.  Wenn wir noch nicht geschehen ist, das kein Problem! Können Sie einer Frage stellen, stehen Ihnen, die hier nicht in den [Azure AD-Foren](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD) enthalten ist, und wir erhalten wieder Ihnen als wir kann.

In den folgenden Abschnitten wird hier unterteilt:

- [**Fragen zur Registrierung von Kennwort zurücksetzen**](#password-reset-registration)
- [**Fragen zum Zurücksetzen von Kennwörtern**](#password-reset)
- [**Fragen zu Kennwort Management-Berichte**](#password-management-reports)
- [**Fragen zu Kennwort abgeschlossenen Writebackvorgängen**](#password-writeback)

## <a name="password-reset-registration"></a>Zurücksetzen des Kennworts Registrierung
 - **F: registrieren können meine Benutzer eigene Kennwort zurücksetzen Daten?**

 > **A:** Ja, solange das Zurücksetzen von Kennwörtern aktiviert ist, und sie lizenziert sind, diese zum Kennwort zurücksetzen Registrierung-Portal unter http://aka.ms/ssprsetup registrieren ihre Authentifizierungsinformationen für das Zurücksetzen von Kennwörtern verwendet werden gelangen. Benutzer können auch in den Bereich Access am http://myapps.microsoft.com vertraut und anschließend auf die Registerkarte Profil durch Klicken auf das Register für die Option Kennwort zurücksetzen registrieren. Weitere Informationen zum Abrufen Ihrer Benutzer für die Kennwortrücksetzung durch Lesen so erhalten Sie für das Zurücksetzen von Kennwörtern so konfiguriert, dass Benutzer konfiguriert.

 - **F: definieren kann ich für meine Benutzer Kennwort zurücksetzen Daten?**

 > **A:** Ja, Sie können mit DirSync oder PowerShell oder über das Portal [Azure-Verwaltungsportal](https://manage.windowsazure.com) oder Office-Administrator ausführen können. Erfahren Sie mehr über dieses Feature auf den Blogbeitrag verbesserte Datenschutz für Azure AD MFA und Kennwort zurücksetzen Telefonnummern und durch das Lesen erfahren Sie, wie Daten verwendet werden, indem Sie Ihr Kennwort ein Zurücksetzen.

 - **F: werden kann ich Daten für Fragen zur Sicherheit von lokal synchronisiert?**

 > **A:** Nein, dies ist heute nicht möglich, aber wir sind unter Berücksichtigung.

 - **F: registrieren können meine Benutzer Daten so, dass andere Benutzer diese Daten nicht angezeigt wird?**

 > **A:** Ja, wenn Benutzer Daten über das Kennwort zurücksetzen Registrierung Portal registrieren wird es dort gespeichert in privaten Authentifizierungsinformationen Felder, die nur sichtbar sind von globalen Administratoren und der Benutzer selbst. Erfahren Sie mehr über dieses Feature auf den Blogbeitrag verbesserte Datenschutz für Azure AD MFA und Kennwort zurücksetzen Telefonnummern und durch das Lesen erfahren Sie, wie Daten verwendet werden, indem Sie Ihr Kennwort ein Zurücksetzen.

 - **F: muss meine Benutzer registriert sein, bevor das Zurücksetzen von Kennwörtern verwendet werden können?**

 > **A:** Nein, wenn Sie genügend Authentifizierungsinformationen in ihrem Namen definieren, werden Benutzer keinen registrieren. Das Zurücksetzen von Kennwörtern funktionieren problemlos, solange Sie in den entsprechenden Feldern im Verzeichnis gespeicherte Daten ordnungsgemäß formatiert wurden. Erfahren Sie mehr über durch das Lesen zu erfahren, wie die Daten durch das Zurücksetzen von Kennwörtern verwendet werden.

 - **F: kann ich synchronisieren oder Festlegen der Authentifizierung Telefon, E-Mail-Authentifizierung oder Telefon Authentifizierung Felder für meine Benutzer?**

 > **A:** Zurzeit nicht, aber wir werden nachdenken, aktivieren diese Funktion.

 - **F: wissen woher im Portal Registrierung welche Optionen zum Anzeigen meiner Benutzer?**

 > **A:** Das Kennwort zurücksetzen Registrierung Portal zeigt nur den Optionen, die Sie für Ihre Benutzer im Abschnitt Benutzer Kennwortrichtlinien Zurücksetzen des des Verzeichnisses konfigurieren Registerkarte aktiviert haben. Dies bedeutet, dass, wenn Sie nicht, sagen Sie Fragen zur Sicherheit, aktivieren Sie dann Benutzern nicht registrieren für diese Option werden können.

 - **Wenn ein Benutzer betrachtet wird f: registriert ist?**

 > **A:** Ein Benutzer gilt registriert, wenn er oder sie mindestens besitzt N Textstellen Authentifizierung Informationen definiert, wobei N die Anzahl der Authentifizierung Methoden erforderlich ist, die Sie in der [Azure-Verwaltungsportal](https://manage.windowsazure.com)festgelegt haben. Weitere Informationen finden Sie unter Anpassen Benutzer zurücksetzen Kennwortrichtlinien.


## <a name="password-reset"></a>Das Zurücksetzen von Kennwörtern

 - **F: wie lange sollte ich warten, erhalten eine e-Mail, SMS oder Anruf über das Zurücksetzen von Kennwörtern?**

 > **A:** E-Mail, SMS-Nachrichten und Telefonanrufe sollten in einer unter Minute mit der normalen Groß-/Kleinschreibung wird 5 bis 20 Sekunden lang werden. Wenn Sie die Benachrichtigung in diesem Zeitraum nicht erhalten, überprüfen Sie Ihren Junk-e-Ordner, die die Anzahl / hergestellt wird e-Mail wird das Schema erwartet, und die Authentifizierungsdaten im Verzeichnis richtig formatiert wurde. Weitere Informationen zum Formatieren von Telefonnummern und e-Mail-Adressen für die Verwendung mit Zurücksetzen von Kennwörtern finden Sie unter erfahren Sie, wie Daten durch das Zurücksetzen von Kennwörtern verwendet werden.

 - **F: Welche Sprachen werden durch das Zurücksetzen von Kennwörtern unterstützt?**

 > **A:** Zurücksetzen des Kennworts UI, SMS-Nachrichten und VoIP-Anrufe sind lokalisiert in der gleichen 40 Sprachen, die in Office 365 unterstützt werden. Dies sind: Arabisch, Bulgarisch, vereinfachtes Chinesisch, traditionelles Chinesisch, Kroatisch, Tschechisch, Dänisch, Niederländisch, Englisch, Estnisch, Finnisch, Französisch, Deutsch, Griechisch, Hebräisch, Hindi, Ungarisch, Indonesisch, Italienisch, Japanisch, Kasachisch, Koreanisch, Lettisch, Litauisch, Malaiisch (Malaysia), Norwegisch (Bokmål), Polnisch, Portugiesisch (Brasilien), Portugiesisch (Portugal), Rumänisch, Russisch, Serbisch (Lateinisch), Slowakisch, Slowenisch, Spanisch, Schwedisch, Thai, Türkisch, Ukrainisch, und Vietnamesisch.

 - **F: welche Teile der Benutzeroberfläche Kennwort zurücksetzen Marke erhalten, wenn ich in meinem Verzeichnis branding organisationsinterne Festlegen der Registerkarte Konfigurieren?**

 > **A:** Das Kennwort zurücksetzen Portal zeigt Ihre Organisation Logo und wird auch ermöglichen es Ihnen so konfigurieren Sie den Kontakt Ihr Administrator Link verweisen auf eine benutzerdefinierte e-Mail- oder die URL. Eine e-Mail, die durch das Zurücksetzen von Kennwörtern gesendet wird wird Logo Ihrer Organisation, Farben (in diesem Fall Rot) Namen im Textkörper der e-Mail, und von Namen angepasst. Sehen Sie ein Beispiel mit allen firmenspezifischen Elementen unten an. Weitere Informationen hierzu das Zurücksetzen von Kennwörtern Anpassen von Aussehen und Verhalten.

  ![][001]

 - **F: wie kann ich meine Benutzer dazu, wo Sie ihre Kennwörter zurücksetzen informieren?**

 > **A:** Sie können die Benutzer direkt an https://passwordreset.microsoftonline.com senden, oder Sie können anweisen, klicken Sie auf der Ihr Konto-Link auf einer beliebigen Schule oder Arbeit ID Anmeldebildschirms gefunden kann nicht zugegriffen werden. Sie können gerne veröffentlichen diese Links (oder URL leitet zu erstellen) in eine beliebige Stelle, die den Benutzern einfach zugänglich ist.

 - **F: verwenden kann ich diese Seite auf einem mobilen Gerät?**

 > **A:** Ja, funktioniert diese Seite auf mobilen Geräten aus.

 - **F: unterstützen entsperren lokalen active Directory-Konten Sie beim Benutzer ihre Kennwörter zurücksetzen?**

 > **A:** Ja, wenn ein Benutzer vertretene Kennwort und Kennwort abgeschlossenen Writebackvorgängen werden zurückgesetzt wurde mit Azure AD Verbinden aller Versionen oder Versionen von Azure AD synchronisieren 1.0.0485.0222 bereitgestellte oder höher, und klicken Sie dann Konto des Benutzers automatisch aufgehoben werden sollen, wenn dieser Benutzer vertretene das Zurücksetzen von Kennwörtern.

 - **F: wie kann ich Kennwortrücksetzung direkt in des Benutzers desktop Anmeldeverhalten integrieren?**

 > **A:** Dies ist nicht möglich heute. Wenn Sie absolut benötigen diese Funktion und ein Azure AD Premium-Kunde sind, können Sie jedoch Microsoft Identität-Manager zu installieren, ohne zusätzliche Kosten und Bereitstellung der lokalen Kennwort zurücksetzen Lösung dort gefunden werden, um diese Anforderung lösen.

 - **F: festgelegt kann ich anderen sicherheitsgrupe Fragen für unterschiedliche Gebietsschemas?**

 > **A:** Nein, dies ist heute nicht möglich, aber wir sind unter Berücksichtigung.

 - **F: Konfigurieren wie viele Fragen können wir die Sicherheitsfragen Authentifizierung Option?**

 > **A:** Sie können bis zu 20 benutzerdefinierten Sicherheitsfragen im [Verwaltungsportal Azure](https://manage.windowsazure.com)konfigurieren.

 - **F: möglicherweise wie lange Fragen zur Sicherheit?**

 > **A:** Fragen zur Sicherheit können zwischen 3 und 200 Zeichen lang sein.

 - **F: möglicherweise wie lange Antworten auf Fragen zur Sicherheit?**

 > **A:** Antworten möglicherweise 3 bis 40 Zeichen lang sein.

 - **F: sind doppelte Antworten auf Fragen zur Sicherheit abgelehnt?**

 > **A:** Ja, ablehnen wir doppelte Antworten auf Fragen zur Sicherheit.

 - **F: Mai einen Benutzer registrieren mehr als eins der gleichen Sicherheitsfrage?**

 > **A:** Nein, nachdem ein Benutzer eine bestimmte Frage registriert, möglicherweise hingegen nicht für die Frage ein zweites Mal zu registrieren.

 - **F: ist es möglich, die Untergrenze für Sicherheitsfragen zur Registrierung festlegen und Zurücksetzen?**

 > **A:** Ja, kann ein Grenzwert für die Registrierung sowie eine weitere zurücksetzen festgelegt werden. möglicherweise Fragen zur Sicherheit von 3 bis 5 für die Registrierung erforderlich, und 3 bis 5 für können erforderlich sein zurücksetzen.

 - **F: Wenn ein Benutzer mehr als die maximale Anzahl von Fragen zum Zurücksetzen erforderlich registriert hat, werden wie Fragen zur Sicherheit während zurücksetzen ausgewählt?**

 > **A:** N Sicherheitsfragen werden zufällig aus der Gesamtzahl der Fragen ausgewählt, die, denen ein Benutzer, registriert hat, wobei N die minimale Anzahl von Fragen, die für das Zurücksetzen von Kennwörtern erforderlich ist. Beispielsweise, wenn ein Benutzer verfügt über 5 Sicherheitsfragen registriert, aber nur 3 zurücksetzen erforderlich sind, 3 von diesen 5 markiert zufällig und der Benutzer zum Zeitpunkt der Zurücksetzen angezeigt. Wenn der Benutzer die Antworten auf Fragen falschen erhält, wiederkehrt der Auswahlprozess um Frage hammering zu verhindern.

 - **F: verhindern ich Sie, dass Benutzer versuchen, oft in einem kurzen Zeitraum Kennwortrücksetzung?**

 > **A:** Ja, es gibt zahlreiche Sicherheitsfeatures zur Verfügung, die in das Zurücksetzen von Kennwörtern integriert. Benutzer können nur versuchen, 5 Kennwort zurücksetzen Versuche innerhalb einer Stunde, bevor er gesperrt wird für 24 Stunden. Benutzer können nur versuchen, eine Telefonnummer 5 mit innerhalb einer Stunde, bevor er gesperrt 24 Stunden wird überprüft. Benutzer können nur versuchen, eine einzelne Authentifizierungsmethode 5 mit innerhalb einer Stunde, bevor er gesperrt wird für 24 Stunden.

 - **F: für wie lange die e-Mail- und einmalige SMS-Kennung gültig sind?**

 > **A:** Die Gültigkeitsdauer der Sitzung für das Zurücksetzen von Kennwörtern ist 105 Minuten. Dies bedeutet, die vom Anfang des Kennworts Vorgang zurückgesetzt, muss der Benutzer 105 Minuten das Kennwort zurückzusetzen. Die e-Mail- und einmalige SMS-Kennung sind ungültig nach Ablauf dieses Zeitraums.


## <a name="password-management-reports"></a>Kennwort Management-Berichte

 - **F: dauert wie lange es, für die Daten auf Kennwort Management Berichte angezeigt?**

 > **A:** Daten sollte innerhalb von 5 bis 10 Minuten auf Kennwort Management Berichte angezeigt werden. Es einige Instanzen es bis zu eine Stunde dauern kann angezeigt werden.

 - **F: wie kann ich die Kennwort Management Berichte filtern?**

 > **A:** Sie können das Kennwort Management-Berichte filtern, indem Sie auf das kleine Lupensymbol extrem rechts neben den spaltenbeschriftungen, zum Anfang des Berichts (Siehe Screenshot). Wenn Sie führen aussagefähigere filtern möchten, können Sie den Bericht in excel und Erstellen einer PivotTable herunterladen.

  ![][002]

 - **F: Was ist, dass die maximale Anzahl von Ereignissen werden in den Kennwort Management Berichten gespeichert?**

 > **A:** Bis zu 1.000 Kennwort werden zurücksetzen oder das Kennwort zurücksetzen Registrierung Ereignisse in Berichten Management Kennwort gespeichert.  Wir arbeiten, um diese Nummer, um weitere Ereignisse gehören zu erweitern.

 - **F: wie weit Berichte Kennworts Management zurückkehren?**

 > **A:** Kennwort Management Berichte anzeigen Vorgänge, die innerhalb der letzten 30 Tage auftreten. Wir arbeiten gerade daran, wie anlegen mehr Zeit. Jetzt Wenn Sie diesen Daten archivieren müssen können die Berichte in regelmäßigen Abständen herunterladen und in einem anderen Speicherort zu speichern.

 - **F: Gibt es eine maximale Anzahl von Zeilen, die auf die Kennwort Management Berichte angezeigt werden können?**

 > **A:** Ja, maximal 1.000 Zeilen werden möglicherweise auf entweder die Verwaltung der Kennwörter Berichte, ob sie in der Benutzeroberfläche angezeigt wird, werden oder heruntergeladene. Wir arbeiten gerade daran, wie Sie diesen Grenzwert zu erhöhen.

 - **F: Gibt es eine API, um das Zurücksetzen von Kennwörtern oder Berichtsdaten Registrierung zugreifen?**

 > **A:** Ja, finden Sie in der folgenden Dokumentation, um zu erfahren, wie Sie das Zurücksetzen des Kennworts reporting Datenstream zugreifen können.  [Sie erhalten grundlegende Informationen zum Zugreifen auf Kennwort reporting Ereignisse programmgesteuert zurücksetzen](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).

## <a name="password-writeback"></a>Kennwort abgeschlossenen Writebackvorgängen
 - **F: wie funktioniert Kennwort abgeschlossenen Writebackvorgängen Hintergrundinformationen?**

 > **A:** Was passiert, wenn das Kennwort abgeschlossenen Writebackvorgängen, sowie aktivieren, wie das System wieder in Ihrer lokalen Umgebung Daten durchläuft eine ausführliche Erläuterung finden Sie unter [wie Kennwort abgeschlossenen Writebackvorgängen funktioniert](active-directory-passwords-learn-more.md#how-password-writeback-works) . Finden Sie unter [Kennwort abgeschlossenen Writebackvorgängen Sicherheitsmodell](active-directory-passwords-learn-more.md#password-writeback-security-model) in wie Kennwort abgeschlossenen Writebackvorgängen funktioniert, erfahren Sie, wie wir sicherstellen, dass ein Kennwort abgeschlossenen Writebackvorgängen einen sehr sicheren Dienst ist.

 - **F: bis wie lange dauert Kennwort abgeschlossenen Writebackvorgängen entwickelt?  Gibt es eine Verzögerung Synchronisierung wie mit Kennwort Hash synchronisieren?**

 > **A:** Kennwort abgeschlossenen Writebackvorgängen ist Chat. Es ist ein synchroner Verkaufspipeline, die grundlegend anders als die Synchronisierung von Kennwörtern Hash passt. Kennwort abgeschlossenen Writebackvorgängen kann Benutzer in Echtzeit Feedback zu den Erfolg Zurücksetzen von Kennwörtern erhalten oder Vorgang ändern. Die durchschnittliche Zeit für eine erfolgreiche abgeschlossenen writebackvorgängen eines Kennworts ist unter 500 ms.

 - **F: funktioniert welche Arten von Konten Kennwort abgeschlossenen Writebackvorgängen für?**

 > **A:** Kennwort abgeschlossenen Writebackvorgängen funktioniert für verbundene und Kennwort Hash synchronisieren verdächtigem Benutzer.

 - **F: Erzwingt Kennwort abgeschlossenen Writebackvorgängen meiner Domäne Kennwortrichtlinien?**

 > **A:** Ja, erzwingt Kennwort abgeschlossenen Writebackvorgängen Gültigkeitsdauer von Kennwörtern, Verlauf, Komplexität, Filter und alle anderen Einschränkung, die Sie in Ihrer lokalen Domäne auf Kennwörter direkte ablegen können.

 - **F: ist Kennwort abgeschlossenen Writebackvorgängen sicher?  Wie kann ich sein, dass ich gehackt werden, wird nicht?**

 > **A:** Ja, ist das Kennwort abgeschlossenen Writebackvorgängen extrem sicher. Weitere Informationen zum Lesen die 4 Sicherheitsebenen vom Kennwort abgeschlossenen Writebackvorgängen Dienst implementiert, schauen Sie sich das [Kennwort abgeschlossenen Writebackvorgängen Sicherheitsmodell](active-directory-passwords-learn-more.md#password-writeback-security-model) in wie Kennwort abgeschlossenen Writebackvorgängen funktioniert.




## <a name="links-to-password-reset-documentation"></a>Links zu Kennwort zurücksetzen Dokumentation
Nachstehend sind Links zu allen Seiten Dokumentation Azure AD Kennwort zurücksetzen aus:

* **Sie hier sind, weil Sie Probleme beim Anmelden haben?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).
* [**Funktionsweise**](active-directory-passwords-how-it-works.md) – erfahren Sie mehr über die sechs verschiedenen Komponenten des Dienst und was bedeutet jede
* [**Erste Schritte**](active-directory-passwords-getting-started.md) - erfahren Sie, wie Sie Benutzern gestatten, zurücksetzen und ihre Cloud oder lokalen Kennwörter ändern
* [**Anpassen**](active-directory-passwords-customize.md) – erfahren Sie, wie Sie das gewünschte Aussehen und Verhalten und Verhalten des Diensts zu den Anforderungen Ihrer Organisation anpassen
* [**Bewährte Methoden**](active-directory-passwords-best-practices.md) - erfahren, wie Sie schnell bereitstellen und Verwalten von Kennwörtern in Ihrer Organisation effektiv
* [**Erste Einsichten**](active-directory-passwords-get-insights.md) – erfahren Sie mehr über unsere integrierten reporting-Funktionen
* [**Problembehandlung**](active-directory-passwords-troubleshoot.md) – erfahren, wie Sie schnell Behandeln von Problemen mit dem Dienst
* [**Erfahren Sie mehr**](active-directory-passwords-learn-more.md) - wechseln tief in die technische Details der Funktionsweise des service


[001]: ./media/active-directory-passwords-faq/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-faq/002.jpg "Image_002.jpg"
