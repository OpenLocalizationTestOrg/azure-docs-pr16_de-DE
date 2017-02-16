<properties
    pageTitle="Azure-Active Directory-B2C: Kombinierte Authentifizierung | Microsoft Azure"
    description="Aktivieren der kombinierte Authentifizierung in Consumer zugänglichen Clientanwendungen durch Azure Active Directory B2C gesichert"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory B2C: Aktivieren Sie kombinierte Authentifizierung in Ihrer Consumer zugänglichen Applikationen

Azure Active Directory (Azure AD) B2C Integration direkte mit [Azure mehrstufige Authentifizierung](../multi-factor-authentication/multi-factor-authentication.md) , damit Sie eine zweite Sicherheitsebene Anmeldung und-Anmeldung Erfahrung in Clientanwendungen Consumer zugänglichen hinzufügen können. Und Sie können dies tun, ohne eine einzelne Zeile mit Code schreiben. Aktuell wird der Anruf und Text Nachricht Überprüfung unterstützt. Wenn Sie bereits Anmelde und-Anmeldung Richtlinien erstellt haben, können Sie weiterhin kombinierte Authentifizierung aktivieren.

> [AZURE.NOTE]
Mehrstufige Authentifizierung kann auch aktiviert werden, wenn Sie Anmelde und-Anmeldung Richtlinien, nicht nur durch Bearbeiten von vorhandenen Richtlinien erstellen.

Dieses Feature hilft Applikationen Szenarien wie folgt verarbeitet:

- Sie ist keine kombinierte Authentifizierung Zugriff auf eine Anwendung erforderlich, jedoch benötigen sie ein weiteres Zugriff auf. Beispielsweise Consumer kann in eine automatische Formulars für Anwendung mit einem sozialen oder lokalen Konto anmelden, aber muss die Telefonnummer überprüfen, bevor Sie den Zugriff auf die private Formulars für Anwendung im selben Verzeichnis registriert.
- Sie nicht kombinierte Authentifizierung im Allgemeinen eine Anwendung Zugriff auf erfordern, aber erforderlich darauf, um die darin enthaltenen sensiblen Teil zugreifen. Beispielsweise Consumer eine Anwendung Bank mit einem sozialen oder lokalen Konto und das Kontrollkästchen Saldo anmelden kann, jedoch muss die Telefonnummer überprüfen, bevor Sie versuchen, eine Überweisung.

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>Ändern Sie die Anmelde Richtlinie aus, um die kombinierte Authentifizierung aktivieren

1. [Gehen Sie folgendermaßen vor, um das B2C Features Blade Azure-Portal zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Anmeldung Richtlinien**.
3. Klicken Sie auf die Richtlinie Anmeldung (beispielsweise "B2C_1_SiUp"), um ihn zu öffnen.
4. Klicken Sie auf die **kombinierte Authentifizierung** , und aktivieren Sie den **Status** **auf**. Klicken Sie auf **OK**.
5. Klicken Sie auf die am oberen Rand der Blade **Speichern** .

Die Funktion "Jetzt ausführen" können auf die Richtlinie Sie um Benutzerfunktionen zu überprüfen. Überprüfen Sie Folgendes:

Ein Consumer-Konto wird in Ihrem Verzeichnis erstellt, bevor der Schritt kombinierte Authentifizierung eintritt. Während der Schritt ist der Consumer aufgefordert, den bieten vertretene Telefonnummer und bestätigen es. Wenn die Überprüfung erfolgreich ist, ist die Telefonnummer des Kontos Consumer zur späteren Verwendung beigefügt. Auch wenn der Consumer bricht ab oder löscht ab, können hingegen aufgefordert, eine Telefonnummer erneut während der nächsten Anmeldung (mit kombinierte Authentifizierung aktiviert) überprüfen.

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>Ändern Sie die Anmeldung-Richtlinie, um die kombinierte Authentifizierung aktivieren

1. [Gehen Sie folgendermaßen vor, um das B2C Features Blade Azure-Portal zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Anmeldung Richtlinien**.
3. Klicken Sie auf die Anmeldung Richtlinie (beispielsweise "B2C_1_SiIn") um ihn zu öffnen. Klicken Sie auf die am oberen Rand der Blade **Bearbeiten** .
4. Klicken Sie auf die **kombinierte Authentifizierung** , und aktivieren Sie den **Status** **auf**. Klicken Sie auf **OK**.
5. Klicken Sie auf die am oberen Rand der Blade **Speichern** .

Die Funktion "Jetzt ausführen" können auf die Richtlinie Sie um Benutzerfunktionen zu überprüfen. Überprüfen Sie Folgendes:

Wenn der Consumer anmeldet (mit einem sozialen oder lokalen Konto), wenn eine Telefonnummer überprüft mit dem Konto Consumer angeschlossen ist, hingegen wird zur Bestätigung aufgefordert. Wenn keine Telefonnummer angeschlossen ist, ist der Consumer aufgefordert, den eine bereitstellen und bestätigen es. Auf dem erfolgreiche Überprüfung ist die Telefonnummer des Kontos Consumer zur späteren Verwendung beigefügt.

## <a name="multi-factor-authentication-on-other-policies"></a>Mehrstufige Authentifizierung auf andere Richtlinien

Beschriebenen für die oben angegebenen Anmeldung und Anmeldung Richtlinien kann auch mehrstufige Authentifizierung auf Anmeldung aktivieren oder Anmeldung Richtlinien und das Kennwort zurücksetzen Richtlinien. Es wird auf Profil Bearbeiten von Richtlinien bald verfügbar sein.
