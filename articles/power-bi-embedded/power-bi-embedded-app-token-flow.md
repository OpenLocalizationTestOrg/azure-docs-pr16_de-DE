<properties
   pageTitle="Anmeldung und Autorisierung mit Power BI eingebettete"
   description="Anmeldung und Autorisierung mit Power BI eingebettete"
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

# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Anmeldung und Autorisierung mit Power BI eingebettete

Der Power BI eingebettete Dienst verwendet **Schlüssel** und **App Token** zur Authentifizierung und Autorisierung, anstelle von expliziten Endbenutzer-Authentifizierung. In diesem Modell verwaltet Ihrer Anwendung Authentifizierung und Autorisierung für Ihre Endbenutzer. Bei Bedarf werden Ihre app erstellt und sendet die App-Token, die unseren Service zum Darstellen des angeforderten Berichts zu erfahren. Dieser Entwurf erforderlich nicht Azure Active Directory für die Benutzerauthentifizierung und Autorisierung, verwenden Sie Ihre app, zwar Sie weiterhin können.

## <a name="two-ways-to-authenticate"></a>Zwei Methoden zum Authentifizieren

**Key** - können Schlüssel für alle Power BI eingebettete REST API-Anrufe. Die Tasten können, indem Sie auf **Alle Einstellungen** und dann auf **Tastenkombinationen**im **Azure-Portal** gefunden werden. Immer behandeln Sie den Key, als wäre sie ein Kennwort ein. Diese Schlüssel haben Berechtigungen, um alle REST-API aufrufen, die eine Auflistung von bestimmter Arbeitsbereich zu machen.

Wenn einen Schlüssel in einem Anruf REST verwenden möchten, fügen Sie die folgenden Autorisierung Kopfzeile hinzu:            

    Authorization: AppKey {your key}

**App-Token** - App Token für alle Einbetten von Anfragen verwendet werden. Sie sind ausgelegt clientseitige, ausgeführt werden, sodass sie zu einem einzelnen Bericht beschränkt sind und bewährte Methode, die eine Uhrzeit Ablauf eingestellt ist.

App Token sind JWT (JSON Web Token), die mit einer der Schlüssel signiert.

Ihre app Token kann die folgenden Ansprüche enthalten:

| Anfordern      | Beschreibung        |
|--------------|------------|
| **Version**      | Die Version der app-Token. 0.2.0 ist die aktuelle Version.       |
| **also**      | Der Empfänger das Token. Power BI Embedded verwendet werden: "https://analysis.windows.net/powerbi/api".  |
| **ISS**      |  Eine Zeichenfolge, die angibt, der Anwendungs, die das Token ausgestellt.    |
| **Typ**     | Die Art des Token zum app erstellt wird. Aktuell ist der einzige unterstützte Typ **einbetten**.   |
| **WCN**      | Arbeitsbereich Websitesammlung Name das Token wird für ausgegeben wird.  |
| **WID**      | Arbeitsbereich-ID das Token wird für ausgegeben wird.  |
| **Entfernen**      | Berichts-ID das Token wird für ausgegeben wird.     |
| **Benutzername** (optional) |  Mit RLS verwendet wird, ist dies eine Zeichenfolge, die den Benutzer zu identifizieren, beim Anwenden von Regeln RLS helfen können. |
| **Rollen** (optional)   |   Eine Zeichenfolge mit der Rollen beim Anwenden von Regeln Sicherheitsstufe Zeile auswählen. Wenn Sie mehr als eine Rolle weitergegeben werden, sollten sie als Array Sting übergeben werden.    |
| **EXP** (optional)    |   Gibt den Zeitpunkt an, in dem das Token abläuft. Diese sollte als Unix Zeitstempel in übergeben.   |
| **NBF** (optional)    |   Gibt den Zeitpunkt an, bei denen das Token beginnt gültig. Diese sollte als Unix Zeitstempel in übergeben.   |

Ein Beispiel für app-Token sieht wie folgt aus:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-coded.png)


Wenn decodiert, sieht er wie folgt aus:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-decoded.png)


## <a name="heres-how-the-flow-works"></a>Und so funktioniert der Fluss

1. Kopieren Sie die Tasten API an Ihrer Anwendung. Sie können die Tasten **Azure**-Portal erhalten.

    ![](media\powerbi-embedded-get-started-sample\azure-portal.png)

2. Token bestätigt einen Anspruch und ein Ablaufdatum hat.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-2.png)

3. Mit einer API Zugriffstasten ruft Token signiert.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-3.png)

4. Benutzeranforderungen einen Bericht anzeigen.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-4.png)

5.  Token wird mit einer API Zugriffstasten überprüft.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-5.png)

6.  Power BI eingebettete ein Berichts an Benutzer gesendet wird.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-6.png)

Nachdem Sie **Power BI eingebettete** ein Berichts für den Benutzer gesendet wird, kann der Benutzer den Bericht in der benutzerdefinierten app anzeigen. Wenn Sie die [Analyse von Sales Daten PBIX Beispiel](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix)importiert haben, würde das Stichprobe Web app beispielsweise wie folgt aussehen:

![](media\powerbi-embedded-get-started-sample\sample-web-app.png)

## <a name="see-also"></a>Siehe auch
- [Erste Schritte mit Microsoft Power BI eingebettete Stichprobe](power-bi-embedded-get-started-sample.md)
- [Häufige Microsoft Power BI eingebettete Szenarien](power-bi-embedded-scenarios.md)
- [Erste Schritte mit Microsoft Power BI eingebettete](power-bi-embedded-get-started.md)
