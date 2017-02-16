<properties
    pageTitle="Azure Active Directory B2C: Benutzerdefinierte Attribute | Microsoft Azure"
    description="Verwenden von benutzerdefinierten Attributen in Azure Active Directory B2C zum Sammeln von Informationen über Ihre Nutzer"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

#  <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory B2C: Verwenden von benutzerdefinierten Attributen zum Sammeln von Informationen über Ihre Nutzer

Ihr Verzeichnis B2C Azure Active Directory (Azure AD) im Lieferumfang von eines integrierten Satz von Informationen (Attribute): Vorname, Nachname, Ort, Postleitzahl und andere Attribute. Jeder Consumer zugängliche Anwendung hat jedoch auf welche Attribute zum Sammeln von Nutzer Anforderungen an. Mit Azure AD B2C können Sie den Satz von Attributen, die für die einzelnen Konten Consumer gespeichert erweitern. Benutzerdefinierte Attribute in [Azure-Portal](https://portal.azure.com/) erstellen können und diese in Ihrer Richtlinien für die Anmeldung verwenden, wie unten dargestellt. Sie können auch lesen und Schreiben diese Attribute mithilfe der [Azure AD Graph-API](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [AZURE.NOTE]
Benutzerdefinierte Attribute verwenden [Azure AD Graph-API Directory Schema Erweiterungen](https://msdn.microsoft.com/library/azure/dn720459.aspx)an.

## <a name="create-a-custom-attribute"></a>Erstellen Sie ein benutzerdefiniertes Attribut

1. [Gehen Sie folgendermaßen vor, um das B2C Features Blade Azure-Portal zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Benutzerattribute**.
3. Klicken Sie auf **+ Add** am oberen Rand der Blade.
4. Geben Sie einen **Namen** für das benutzerdefinierte Attribut (beispielsweise "ShoeSize") und optional eine **Beschreibung**ein. Klicken Sie auf **Erstellen**.

    > [AZURE.NOTE]
    Nur die "String" **-Datentyp** ist derzeit verfügbar.

Das benutzerdefinierte Attribut ist jetzt in der Liste der **Benutzerattribute**und zur Verwendung in Ihre anmelden-Richtlinien zur Verfügung.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Verwenden Sie ein benutzerdefiniertes Attribut in Ihrer Richtlinie Anmeldung

1. [Gehen Sie folgendermaßen vor, um das B2C Features Blade Azure-Portal zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Anmeldung Richtlinien**.
3. Klicken Sie auf die Richtlinie Anmeldung (beispielsweise "B2C_1_SiUp"), um ihn zu öffnen. Klicken Sie auf die am oberen Rand der Blade **Bearbeiten** .
4. Klicken Sie auf **Anmeldung Attribute** , und wählen Sie das benutzerdefinierte Attribut (beispielsweise "ShoeSize"). Klicken Sie auf **OK**.
5. Klicken Sie auf **Anwendung Ansprüche** , und wählen Sie das benutzerdefinierte Attribut aus. Klicken Sie auf **OK**.
6. Klicken Sie auf die am oberen Rand der Blade **Speichern** .

Die Funktion "Jetzt ausführen" können auf die Richtlinie Sie um Benutzerfunktionen zu überprüfen. Sie sollten jetzt finden Sie unter "ShoeSize" in der Liste von Attributen, die bei der Anmeldung Consumer gesammelten und diese in das Token gesendet an Ihrer Anwendung wieder angezeigt werden.

## <a name="notes"></a>Notizen

- Zusammen mit der Anmeldung Richtlinien können benutzerdefinierte Attribute auch in Anmeldung oder Anmeldung Richtlinien und Bearbeiten von Richtlinien Profil verwendet werden.
- Es gibt eine Einschränkung des benutzerdefinierten Attributen aus. Es wird nur das erste Mal erstellt, die, das es verwendet wird, klicken Sie in eine beliebige Richtlinie und nicht, wenn Sie es in der Liste der **Benutzerattribute**hinzufügen.
