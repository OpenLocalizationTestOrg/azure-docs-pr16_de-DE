<properties
    pageTitle="Übersicht über die Azure MFA | Microsoft Azure"
    description="Was ist die kombinierte Authentifizierung Azure, warum verwenden MFA, Weitere Informationen zu den Client kombinierte Authentifizierung und der unterschiedlichen Methoden und Versionen zur Verfügung. "
    keywords="Einführung in MFA Mfa Übersicht, was Mfa ist"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="what-is-azure-multi-factor-authentication"></a>Was ist die kombinierte Authentifizierung Azure?
Überprüfung in zwei Schritten ist eine Methode der Authentifizierung, die mehr als eine Überprüfungsmethode erfordert und Benutzer melden Sie sich-ins und Transaktionen kritische zweite Sicherheitsebene hinzugefügt. Funktionsweise durch zwei oder mehrere der folgenden Methoden Überprüfung anfordern:

- Etwas wissen (in der Regel ein Kennwort)
- Etwas, das Sie (ein vertrauenswürdiges Gerät, das nicht einfach besitzen, wie ein Telefon dupliziert ist)
- Etwas werden (Biometrik)

<center>![Username and Password](./media/multi-factor-authentication/pword.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Certificates](./media/multi-factor-authentication/phone.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smart Phone](./media/multi-factor-authentication/hware.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smart Card](./media/multi-factor-authentication/smart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Virtual Smart Card](./media/multi-factor-authentication/vsmart.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Username and Password](./media/multi-factor-authentication/cert.png)</center>

Azure mehrstufige Authentifizierung (MFA) ist die Lösung von Microsoft in zwei Schritten Überprüfung. Azure MFA hilft Schutz Zugriff auf Daten und Applikationen während der Besprechung Benutzer Demand für ein einfacher Vorgang Anmeldung. Es stellt strenge Authentifizierung über einen Zellbereich Überprüfung Methoden, einschließlich Anruf, Textnachricht oder Überprüfung mobile-app.

>[AZURE.VIDEO multi-factor-authentication-overview]

## <a name="why-use-azure-multi-factor-authentication"></a>Gründe für das Verwenden von Azure kombinierte Authentifizierung

Heute mehr denn je, sind zunehmend Personen verbunden. Mit Smartphones, Tablets, Laptops und PCs haben Personen mehrere Optionen auf, wie sie überlegen eine Verbindung herstellen und zu einem beliebigen Zeitpunkt bleiben. Personen können zugreifen ihren Konten und Applikationen überall, d. h., sie können mehr Arbeit erledigen erhalten und ihre Kunden dienen besser.

Azure kombinierte Authentifizierung ist eine benutzerfreundliche, skalierbare und zuverlässigen Lösung, die eine zweite Methode der Authentifizierung bereitstellt, so dass der Benutzer immer geschützt sind.

![Einfach zu verwenden](./media/multi-factor-authentication/simple.png)| ![Skalierbare](./media/multi-factor-authentication/scalable.png)| ![Immer geschützt](./media/multi-factor-authentication/protected.png)|![Zuverlässigen](./media/multi-factor-authentication/reliable.png)
:-------------: | :-------------: | :-------------: | :-------------: |
**Einfach zu verwenden**|**Skalierbare**|**Immer geschützt**|**Zuverlässigen**

- **Einfache Anwendung** - Azure kombinierte Authentifizierung ist Einsteiger-bis zum Einrichten und verwenden. Der zusätzliche Schutz, der Lieferumfang Azure kombinierte Authentifizierung kann Benutzer eigene Geräte verwalten. Optimale aller in vielen Fällen können sie mit nur wenigen Mausklicks eingerichtet werden.
- **Skalierbar** - Azure kombinierte Authentifizierung mit der Cloud verwendet und lässt sich mit Ihrem lokalen Active Directory und benutzerdefinierte apps. Dieser Schutz wird auch für Ihre umfangreicher, wichtiger Szenarios erweitert.
- **Immer geschützt** - Azure kombinierte Authentifizierung bietet leistungsfähige Authentifizierung mithilfe von der höchsten Branchenstandards.
- **Zuverlässig** - Verfügbarkeit von 99,9 % Azure kombinierte Authentifizierung garantiert. Der Dienst gilt als nicht verfügbar, wenn es erhalten oder Überprüfung die Anfragen für die Überprüfung in zwei Schritten nicht möglich ist.

>[AZURE.VIDEO windows-azure-multi-factor-authentication]


## <a name="how-azure-multi-factor-authentication-works"></a>Funktionsweise von Azure kombinierte Authentifizierung

Die Sicherheit in zwei Schritten Überprüfung liegt in deren Ebenen. Mehreren Überprüfung Methoden beeinträchtigen stellt eine signifikante Herausforderung Angreifern. Auch wenn ein Angreifer erfahren Sie, Ihr Kennwort ein, ist es ohne auch Besitz eines vertrauenswürdigen Gerät nicht verwendbar. Wenn Sie das Gerät vergessen haben, kann die Person, die es findet es verwenden, wenn er oder sie auch Ihr Kennwort bekannt ist.

>[AZURE.VIDEO multi-factor-authentication-deep-dive-securing-access-on-premises]

## <a name="methods-available-for-multi-factor-authentication"></a>Verfügbaren Methoden für die kombinierte Authentifizierung
Wenn sich der Benutzer anmeldet, wird eine zusätzliche Überprüfung Anforderung an den Benutzer gesendet. Es folgen eine Liste der Methoden, die für diese zweite Überprüfung verwendet werden können.

Überprüfungsmethode | Beschreibung
------------- | ------------- |
Anruf | Ein Anruf wird an eines Benutzers Telefonnummer bitten, stellen Sie sicher, dass sie bei der Anmeldung sind platziert. Drücken Sie die #-Taste, bis zum Abschluss der Überprüfung aus. Diese Option ist konfigurierbare und geändert werden kann, um einen Code, den Sie angeben.
Textnachricht | Textnachricht wird an eines Benutzers Smartphone mit einem 6 zweistellig Code gesendet. Geben Sie diesen Code in bis zum Abschluss der Überprüfung aus.
Benachrichtigung für Mobile-app | Anforderung einer Überprüfung wird an eines Benutzers Smartphone fragt sie abgeschlossen, indem Sie in der mobilen app **Überprüfen** auswählen die Überprüfung gesendet. In diesem Fall app Benachrichtigung ist die primäre Überprüfungsmethode. Wenn sie diese Benachrichtigung bei sie keine Anmeldung sind, können sie es als Betrug melden.
Überprüfungscode mit mobile-app | Die mobile-app auf dem Gerät eines Benutzers generiert einen Überprüfungscode mit. Dies geschieht, wenn Sie einen Überprüfungscode als Ihre primäre Überprüfungsmethode ausgewählt.

Die mobile-app Überprüfung Methoden passt mit Drittanbieter-Authentifizierung apps für Smartphones Azure kombinierte Authentifizierung. Wir empfehlen jedoch die Microsoft Authenticator-app für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073)verfügbar ist.

## <a name="available-versions-of-azure-multi-factor-authentication"></a>Verfügbarer Versionen Azure kombinierte Authentifizierung
Azure kombinierte Authentifizierung ist in drei unterschiedliche Versionen zur Verfügung. 

Version | Beschreibung
------------- | ------------- |
Mehrstufige Authentifizierung für Office 365 | Diese Version funktioniert ausschließlich mit Office 365-Anwendungen und vom Office 365-Portal verwaltet wird. Administratoren können also jetzt Sichern ihrer Office 365-Ressourcen mit zwei Überprüfung unterstützt. Diese Version im Lieferumfang von Office 365-Abonnement.
Mehrstufige Authentifizierung für Azure-Administratoren | Die gleiche Teilmenge der Funktionen zur Überprüfung in zwei Schritten für Office 365 ist alle Azure Administratoren kostenlos verfügbar. Jeder Administratorkonto eines Azure-Abonnements kann dieses Feature für zusätzlichen Schutz aktivieren. Ein Administrator, der zum Erstellen eines virtuellen Computers oder der Website, verwalten Speicher oder verwenden Sie eine beliebige andere Azure Service Azure-Portal zugreifen möchte kann MFA seinem Administratorkonto hinzufügen.
Azure kombinierte Authentifizierung | Azure kombinierte Authentifizierung bietet den meisten Satz von Funktionen. Es bietet zusätzliche Optionen über das [Azure klassischen Portal](http://manage.windowsazure.com), Erweiterte Berichte und Unterstützung für eine Reihe von lokalen und cloud Applications. Azure kombinierte Authentifizierung ist im Lieferumfang von Azure Active Directory Premium und Mobilität Enterprise-Suite aus, und in der Cloud oder lokal bereitgestellt werden kann.

## <a name="feature-comparison-of-versions"></a>Vergleich der Features von Versionen
Die folgende Tabelle enthält eine Liste der Features, die in den verschiedenen Versionen von Azure kombinierte Authentifizierung verfügbar sind.

>[AZURE.NOTE] In dieser Vergleichstabelle werden die Features, die jedes Abonnement gehören. Wenn Sie Azure AD Premium oder Enterprise Mobilität Suite verfügen, möglicherweise einige Features nicht zur Verfügung, je nachdem, ob Sie [in der Cloud oder MFA lokalen MFA](multi-factor-authentication-get-started.md)verwenden.

Feature | Mehrstufige Authentifizierung für Office 365 (enthalten in Office 365-SKUs)|Mehrstufige Authentifizierung für Azure-Administratoren (mit Azure-Abonnement enthalten) | Azure kombinierte Authentifizierung (in Azure AD Premium und Enterprise Mobilität Suite enthalten)
------------- | :-------------: |:-------------: |:-------------: |
Administratoren können Konten mit MFA schützen.| ● | ● (nur für Azure Administrator Konten verfügbar)|●
Mobile-app als ein zweites Argument|● | ● | ●
Anruf als ein zweites Argument|● | ● | ●
SMS als ein zweites Argument|● | ● | ●
App Kennwörter für Clients, die nicht MFA unterstützen|● | ● | ●
Administrator Kontrolle über Authentifizierungsmethoden| ● | ● | ●
PIN-Modus| | | ●
Betrug Benachrichtigung| | | ●
MFA-Berichte| | | ●
Einmalige umgehen| | | ●
Benutzerdefinierte Grüße für Telefonanrufe| | | ●
Anpassung der Anrufer-ID für Telefonanrufe| | | ●
Ereignis Bestätigung| | | ●
Vertrauenswürdigen IP-Adressen| | | ●
Denken Sie daran MFA für vertrauenswürdige Geräte |● | ● | ●
MFA SDK | | | ● erfordert mehrstufige Authentifizierung Anbieter und vollständige Azure-Abonnement
MFA für lokale Applikationen mit MFA-server| | | ●

## <a name="how-to-get-azure-multi-factor-authentication"></a>So erhalten Sie Azure kombinierte Authentifizierung

Wenn Sie die vollständige Funktionalität von Azure kombinierte Authentifizierung erhalten möchten, gibt es mehrere Optionen zur Verfügung:

1.  Azure kombinierte Authentifizierung Lizenzen erwerben und Ihre Benutzer zuweisen.
2.  Erwerben von Lizenzen, die Azure kombinierte Authentifizierung gebündelten darin enthaltenen wie Azure Active Directory Premium, Enterprise Mobilität Suite oder Enterprise-Cloud-Suite haben und sie Ihre Benutzer zuzuweisen.
3.  Erstellen einer Azure mehrstufige Authentifizierungsanbieter innerhalb eines Azure-Abonnements. Wenn eine Azure mehrstufige Authentifizierungsanbieter verwenden, gibt es zwei Verwendung Modelle verfügbar, die über Ihre Azure-Abonnement in Rechnung gestellt werden:  
    - **Pro Benutzer**. Für Unternehmen, die in zwei Schritten Überprüfung für einer festen Anzahl von Mitarbeitern, die regelmäßig Authentifizierung benötigen, aktivieren möchten.  
    - **Pro Authentifizierung**. Für Unternehmen, die in zwei Schritten Überprüfung für eine große Gruppe von externen Benutzern, die selten Authentifizierung benötigen, aktivieren möchten.  

Azure kombinierte Authentifizierung bietet auswählbar Überprüfung Methoden für Cloud und Server. Dies bedeutet, dass Sie auswählen können, welche Methoden für Ihre Benutzer verfügbar sind. Dieses Feature ist derzeit in public Preview-Version für die Cloud Version kombinierte Authentifizierung. Weitere Informationen finden Sie unter [auswählbar Überprüfung Methoden](multi-factor-authentication-whats-next.md#selectable-verification-methods).

Preise Details, finden Sie unter [Azure MFA Preise.](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)

## <a name="next-steps"></a>Nächste Schritte

Um mit Azure kombinierte Authentifizierung anzufangen, besteht Ihre erste Schritt [entscheiden, ob in der Cloud oder lokalen MFA](multi-factor-authentication-get-started.md)
