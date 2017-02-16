<properties
   pageTitle="CSV-Dateiformat für die Zusammenarbeit Preview Azure Active Directory B2B | Microsoft Azure"
   description="Azure Active Directory B2B unterstützt Ihrer Beziehungen unternehmensweit Business Partner an Ihre corporate Applikationen Selektives Zugriff auf"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-csv-file-format"></a>Azure AD B2B Zusammenarbeit Vorschau: CSV-Dateiformat

Die Vorabversion von Azure AD B2B Zusammenarbeit erfordert eine CSV-Datei angeben Partner Benutzerinformationen über das Azure AD-Portal hochgeladen werden. Die CSV-Datei sollte die folgenden erforderlichen Etiketten und optionaler Felder nach Bedarf enthalten. Ändern Sie die CSV-Beispieldatei (unten), ohne die Schreibweise für die Beschriftungen in der ersten Zeile ändern.

>[AZURE.NOTE] Die erste Zeile der Etiketten (beispielsweise E-Mail, DisplayName und usw.) ist erforderlich für die CSV-Datei erfolgreich analysiert werden soll. Die Rechtschreibung muss die unten angegebene Felder übereinstimmen. Diese Etiketten Identifizieren des Inhalts darunter. Für optionale Felder, die nicht erforderlich sind, können die Beschriftungen aus der CSV-Datei entfernt werden. Die gesamte Spalte kann leer gelassen werden.

## <a name="required-fields-br"></a>Pflichtfelder: <br/>
**E-Mail:** E-Mail-Adresse der eingeladene Benutzer aus. <br/>
**DisplayName:** Anzeigenamen für eingeladene Benutzer (normalerweise vor- und Nachnamen ein).<br/>


## <a name="optional-fields-br"></a>Optionaler Felder: <br/>

**InvitationText:** Passen Sie die Einladung e-Mail-Text nach app branding und vor der Rückzahlung Link.<br/>
**InvitedToApplications:** AppIDs corporate Applications zum Zuweisen von Benutzern. AppIDs können in PowerShell abgerufen werden durch Aufrufen`Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/>
**InvitedToGroups:** Pluszeichen für Gruppen Benutzer hinzufügen. Können in PowerShell abgerufen werden durch Aufrufen`Get-MsolGroup | fl DisplayName, ObjectId`<br/>
**InviteRedirectURL:** URL für einen Benutzer eingeladen nach einladen Annahme direkte. Dies sollte eine firmenspezifischen URL (z. B. [*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/)) sein. Dieses Feld optionale nicht angegeben ist, wird der eingeladene Benutzer zum Bereich Access-App weitergeleitet, wo sie zu Ihrer ausgewählten corporate apps navigieren können. Die App Access Systemsteuerung URL des Formulars ist `https://account.activedirectory.windowsazure.com/applications/default.aspx?tenantId=<TenantID>`.<br/>
**CcEmailAddress**: e-Mail-Adresse, um per e-Mail gesendeten Einladung zu kopieren. Wenn das Feld CcEmailAddress verwendet wird, kann diese Einladung für Benutzer-e-Mail-überprüft oder Mandanten Erstellung verwendet werden.<br/>
**Sprache:** Sprache für die Einladung e-Mail und Rückzahlung Erfahrung mit "En" (in Englisch) als Standard Wenn nicht angegeben. Die anderen 10 unterstützt Sprache Codes sind:<br/>
1. de: Deutsch<br/>
2. es: Spanisch<br/>
3. Französisch: Französisch<br/>
4. es: Italienisch<br/>
5. Ja: Japanisch<br/>
6. Ko: Koreanisch<br/>
7. pt-BR: Portugiesisch (Brasilien)<br/>
8. RU: Russisch<br/>
9. Zh-HANS: Chinesisch (vereinfacht)<br/>
10. Zh-HANT: Chinesisch (traditionell)<br/>

## <a name="sample-csv-file"></a>CSV-Beispieldatei
Hier ist eine CSV-Beispieldatei geändert werden können.

>[AZURE.NOTE] Kopieren Sie und fügen Sie diese in Editor, und speichern Sie es mit der Erweiterung 'CSV'. Klicken Sie in Excel bearbeiten. Es wird in eine Tabelle mit Beschriftungen in der ersten Zeile strukturiert werden.

> Fügen Sie weitere Optionaler Felder in Excel, indem Sie die Bezeichnung angeben, und füllen die Spalte darunter hinzu.

```
Email,DisplayName,InvitationText,InviteRedirectUrl,InvitedToApplications,InvitedToGroups,CcEmailAddress,Language
wharp@contoso.com,Walter Harp,Hi Walter! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
jsmith@contoso.com,Jeff Smith,Hi Jeff! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
bsmith@contoso.com,Ben Smith,Hi Ben! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en

```

## <a name="related-articles"></a>Verwandte Artikel
Durchsuchen von den anderen Artikeln zu Azure AD B2B für die Zusammenarbeit

- [Was ist für die Zusammenarbeit Azure AD B2B?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [So funktioniert es](active-directory-b2b-how-it-works.md)
- [Ausführliche exemplarische Vorgehensweise](active-directory-b2b-detailed-walkthrough.md)
- [Token Format für externe Benutzer](active-directory-b2b-references-external-user-token-format.md)
- [Externe Benutzer Objekt Attribut Änderungen](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Aktuelle Vorschau Einschränkungen](active-directory-b2b-current-preview-limitations.md)
- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
