<properties
pageTitle="MailChimp | Microsoft Azure"
description="Erstellen Sie Logik apps mit Azure-App-Dienst an. MailChimp ist ein SaaS-Dienst, der Unternehmen verwalten und automatisieren Marketingaktivitäten e-Mail ermöglicht, einschließlich Senden von marketing-e-Mails, automatisierte Nachrichten und den massensendungen zu ermitteln."
services="logic-apps"
documentationCenter=".net,nodejs,java"  
authors="msftman"
manager="erikre"
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-mailchimp-connector"></a>Erste Schritte mit der MailChimp Verbinder

MailChimp ist ein SaaS-Dienst, der Unternehmen verwalten und automatisieren Marketingaktivitäten e-Mail ermöglicht, einschließlich Senden von marketing-e-Mails, automatisierte Nachrichten und den massensendungen zu ermitteln.


>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus.

Sie können beginnen, indem Sie eine app Logik jetzt erstellen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Der Verbinder MailChimp kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen die Daten in JSON und XML-Formaten.

 Der Verbinder MailChimp weist die folgenden Aktion(en) und/oder Triggern zur Verfügung:

### <a name="mailchimp-actions"></a>MailChimp Aktionen
Sie können folgende Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|[newcampaign](connectors-create-api-mailchimp.md#newcampaign)|Erstellen einer neuen Campaign basierend auf einen Typ für eine Marketingkampagne, Liste der Empfänger und für eine Marketingkampagne Einstellungen (Betreffzeile, Titel, From_name und Reply_to)|
|[NewList](connectors-create-api-mailchimp.md#newlist)|Erstellen einer neuen Liste in Ihr Konto MailChimp|
|[gestellten](connectors-create-api-mailchimp.md#addmember)|Hinzufügen oder Aktualisieren eines Mitglieds|
|[RemoveMember](connectors-create-api-mailchimp.md#removemember)|Löschen Sie ein Mitglied aus einer Liste aus.|
|[updatemember](connectors-create-api-mailchimp.md#updatemember)|Aktualisieren von Informationen für ein Mitglied bestimmten Liste|
### <a name="mailchimp-triggers"></a>MailChimp Trigger
Sie können diese Ereignisse überwachen:

|Auslösen | Beschreibung|
|--- | ---|
|Wenn ein Element einer Liste hinzugefügt wurde|Einen Workflow ausgelöst wird, wenn Sie ein neues Mitglied zu einer Liste hinzugefügt wurde|
|Bei der Erstellung einer neuen Liste|Einen Workflow ausgelöst wird, wenn eine neue Liste erstellt wurde|


## <a name="create-a-connection-to-mailchimp"></a>Herstellen einer Verbindung mit MailChimp
Um Logik apps mit MailChimp zu erstellen, müssen Sie zunächst eine **Verbindung** erstellen anschließend geben Sie die Details für die folgenden Eigenschaften:

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Geben Sie MailChimp-Anmeldeinformationen|

>[AZURE.INCLUDE [Steps to create a connection to MailChimp](../../includes/connectors-create-api-mailchimp.md)]

>[AZURE.TIP] Sie können diese Verbindung in andere apps Logik verwenden.

## <a name="reference-for-mailchimp"></a>Referenz für MailChimp
Version gilt: 1.0

## <a name="newcampaign"></a>newcampaign
Neue Campaign: Erstellen einer neuen Campaign basierend auf einen Typ für eine Marketingkampagne, Liste der Empfänger und für eine Marketingkampagne Einstellungen (Betreffzeile, Titel, From_name und Reply_to)

```POST: /campaigns```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|newCampaignRequest| |Ja|Textkörper|keine|JSON-Objekt, das im Text mit den neuen Campaign Anforderungsparametern senden|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="newlist"></a>NewList
Neue Liste: Erstellen einer neuen Liste in Ihr Konto MailChimp

```POST: /lists```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|newListRequest| |Ja|Textkörper|keine|JSON-Objekt, das im Text mit den neuen Campaign Anforderungsparametern senden|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="addmember"></a>gestellten
Hinzufügen von Mitglied zur Liste: Hinzufügen oder aktualisieren ein Mitglieds

```POST: /lists/{list_id}/members```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|Zeichenfolge|Ja|Pfad|keine|Die eindeutige Id für die Liste|
|newMemberInList| |Ja|Textkörper|keine|JSON-Objekt, das im Text mit den neuen Mitglieds Informationen senden|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="removemember"></a>RemoveMember
Mitglied aus der Liste zu entfernen: Löschen Sie ein Mitglied aus einer Liste.

```DELETE: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|Zeichenfolge|Ja|Pfad|keine|Die eindeutige Id für die Liste|
|member_email|Zeichenfolge|Ja|Pfad|keine|Die e-Mail-Adresse des zu löschenden Elements|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="updatemember"></a>updatemember
Aktualisieren Sie Mitglied Informationen: Informationen zu einer bestimmten Liste Mitglied aktualisieren

```PATCH: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|Zeichenfolge|Ja|Pfad|keine|Die eindeutige Id für die Liste|
|member_email|Zeichenfolge|Ja|Pfad|keine|Der eindeutige e-Mail-Adresse des Mitglieds aktualisieren|
|updateMemberInListRequest| |Ja|Textkörper|keine|JSON-Objekt, in den Textkörper mit den aktualisierten Mitglied Informationen zu senden.|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="onmembersubscribed"></a>OnMemberSubscribed
Wenn ein Mitglied zu einer Liste hinzugefügt wurde: einen Workflow ausgelöst wird, wenn Sie ein neues Mitglied zu einer Liste hinzugefügt wurde

```GET: /trigger/lists/{list_id}/members```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|Zeichenfolge|Ja|Pfad|keine|Die eindeutige Id für die Liste|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|202|Akzeptiert|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="oncreatelist"></a>OnCreateList
Beim Erstellen einer neuen Liste: Auslösen ein Workflows Wenn eine neue Liste erstellt wird

```GET: /trigger/lists```

Es sind keine Parameter für diesen Anruf
#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|202|Akzeptiert|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="object-definitions"></a>Objektdefinitionen

### <a name="newcampaignrequest"></a>NewCampaignRequest


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Typ|Zeichenfolge|Ja |
|Empfänger|nicht definiert|Ja |
|Einstellungen|nicht definiert|Ja |
|variate_settings|nicht definiert|Nein |
|Verlauf|nicht definiert|Nein |
|rss_opts|nicht definiert|Nein |
|social_card|nicht definiert|Nein |



### <a name="recipient"></a>Empfänger


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|list_id|Zeichenfolge|Ja |
|segment_opts|nicht definiert|Nein |



### <a name="settings"></a>Einstellungen


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|subject_line|Zeichenfolge|Ja |
|Titel|Zeichenfolge|Nein |
|from_name|Zeichenfolge|Ja |
|reply_to|Zeichenfolge|Ja |
|use_conversation|Boolesch|Nein |
|to_name|Zeichenfolge|Nein |
|folder_id|ganze Zahl|Nein |
|authentifizieren|Boolesch|Nein |
|auto_footer|Boolesch|Nein |
|inline_css|Boolesch|Nein |
|auto_tweet|Boolesch|Nein |
|auto_fb_post|Matrix|Nein |
|fb_comments|Boolesch|Nein |



### <a name="variatesettings"></a>Variate_Settings


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|winner_criteria|Zeichenfolge|Nein |
|Wartezeit|ganze Zahl|Nein |
|test_size|ganze Zahl|Nein |
|subject_lines|Matrix|Nein |
|send_times|Matrix|Nein |
|from_names|Matrix|Nein |
|reply_to_addresses|Matrix|Nein |



### <a name="tracking"></a>Verlauf


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Öffnet|Boolesch|Nein |
|html_clicks|Boolesch|Nein |
|text_clicks|Boolesch|Nein |
|goal_tracking|Boolesch|Nein |
|ecomm360|Boolesch|Nein |
|google_analytics|Zeichenfolge|Nein |
|clicktale|Zeichenfolge|Nein |
|Vertrieb|nicht definiert|Nein |
|highrise|nicht definiert|Nein |
|Kapsel|nicht definiert|Nein |



### <a name="rssopts"></a>RSS_Opts


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|feed_url|Zeichenfolge|Nein |
|Häufigkeit|Zeichenfolge|Nein |
|constrain_rss_img|Zeichenfolge|Nein |
|Zeitplan|nicht definiert|Nein |



### <a name="socialcard"></a>Social_Card


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|image_url|Zeichenfolge|Nein |
|Beschreibung|Zeichenfolge|Nein |
|Titel|Zeichenfolge|Nein |



### <a name="segmentopts"></a>Segment_Opts


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|saved_segment_id|ganze Zahl|Nein |
|Vergleich|Zeichenfolge|Nein |



### <a name="salesforce"></a>Vertrieb


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|für eine Marketingkampagne|Boolesch|Nein |
|Notizen|Boolesch|Nein |



### <a name="highrise"></a>Highrise


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|für eine Marketingkampagne|Boolesch|Nein |
|Notizen|Boolesch|Nein |



### <a name="capsule"></a>Kapsel


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Notizen|Boolesch|Nein |



### <a name="schedule"></a>Zeitplan


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Stunde|ganze Zahl|Nein |
|daily_send|nicht definiert|Nein |
|weekly_send_day|Zeichenfolge|Nein |
|monthly_send_date|Zahl|Nein |



### <a name="dailysend"></a>Daily_Send


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Sonntag|Boolesch|Nein |
|Montag|Boolesch|Nein |
|Dienstag|Boolesch|Nein |
|Mittwoch|Boolesch|Nein |
|Donnerstag|Boolesch|Nein |
|Freitag|Boolesch|Nein |
|Samstag|Boolesch|Nein |



### <a name="campaignresponsemodel"></a>CampaignResponseModel


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|Typ|Zeichenfolge|Nein |
|create_time|Zeichenfolge|Nein |
|archive_url|Zeichenfolge|Nein |
|Status|Zeichenfolge|Nein |
|emails_sent|ganze Zahl|Nein |
|send_time|Zeichenfolge|Nein |
|content_type|Zeichenfolge|Nein |
|Empfänger|Matrix|Nein |
|Einstellungen|nicht definiert|Nein |
|variate_settings|nicht definiert|Nein |
|Verlauf|nicht definiert|Nein |
|rss_opts|nicht definiert|Nein |
|ab_split_opts|nicht definiert|Nein |
|social_card|nicht definiert|Nein |
|report_summary|nicht definiert|Nein |
|delivery_status|nicht definiert|Nein |
|_links|Matrix|Nein |



### <a name="absplitopts"></a>AB_Split_Opts


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|split_test|Zeichenfolge|Nein |
|pick_winner|Zeichenfolge|Nein |
|wait_units|Zeichenfolge|Nein |
|Wartezeit|ganze Zahl|Nein |
|split_size|ganze Zahl|Nein |
|from_name_a|Zeichenfolge|Nein |
|from_name_b|Zeichenfolge|Nein |
|reply_email_a|Zeichenfolge|Nein |
|reply_email_b|Zeichenfolge|Nein |
|subject_a|Zeichenfolge|Nein |
|subject_b|Zeichenfolge|Nein |
|send_time_a|Zeichenfolge|Nein |
|send_time_b|Zeichenfolge|Nein |
|send_time_winner|Zeichenfolge|Nein |



### <a name="reportsummary"></a>Report_Summary


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Öffnet|ganze Zahl|Nein |
|unique_opens|ganze Zahl|Nein |
|open_rate|Zahl|Nein |
|klickt|ganze Zahl|Nein |
|subscriber_clicks|Zahl|Nein |
|click_rate|Zahl|Nein |



### <a name="deliverystatus"></a>Delivery_Status


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|aktiviert|Boolesch|Nein |
|can_cancel|Boolesch|Nein |
|Status|Zeichenfolge|Nein |
|emails_sent|ganze Zahl|Nein |
|emails_canceled|ganze Zahl|Nein |



### <a name="link"></a>Link


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|rel|Zeichenfolge|Nein |
|href|Zeichenfolge|Nein |
|Methode|Zeichenfolge|Nein |
|targetSchema|Zeichenfolge|Nein |
|Schema|Zeichenfolge|Nein |



### <a name="newlistrequest"></a>NewListRequest


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Namen|Zeichenfolge|Ja |
|Wenden Sie sich an|nicht definiert|Ja |
|permission_reminder|Zeichenfolge|Ja |
|use_archive_bar|Boolesch|Nein |
|campaign_defaults|nicht definiert|Ja |
|notify_on_subscribe|Zeichenfolge|Nein |
|notify_on_unsubscribe|Zeichenfolge|Nein |
|email_type_option|Boolesch|Ja |
|Sichtbarkeit|Zeichenfolge|Nein |



### <a name="contact"></a>Kontakt


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Unternehmen|Zeichenfolge|Ja |
|1|Zeichenfolge|Ja |
|Adresse2|Zeichenfolge|Nein |
|Ort|Zeichenfolge|Ja |
|Bundesstaat|Zeichenfolge|Ja |
|ZIP|Zeichenfolge|Ja |
|Land|Zeichenfolge|Ja |
|Mobiltelefon|Zeichenfolge|Ja |



### <a name="campaigndefaults"></a>Campaign_Defaults


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|from_name|Zeichenfolge|Ja |
|from_email|Zeichenfolge|Ja |
|Betreff|Zeichenfolge|Nein |
|Sprache|Zeichenfolge|Ja |



### <a name="createnewlistresponsemodel"></a>CreateNewListResponseModel


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Ja |
|Namen|Zeichenfolge|Ja |
|Wenden Sie sich an|nicht definiert|Ja |
|permission_reminder|Zeichenfolge|Ja |
|use_archive_bar|Boolesch|Nein |
|campaign_defaults|nicht definiert|Ja |
|notify_on_subscribe|Zeichenfolge|Nein |
|notify_on_unsubscribe|Zeichenfolge|Nein |
|date_created|Zeichenfolge|Nein |
|list_rating|ganze Zahl|Nein |
|email_type_option|Boolesch|Ja |
|subscribe_url_short|Zeichenfolge|Nein |
|subscribe_url_long|Zeichenfolge|Nein |
|beamer_address|Zeichenfolge|Nein |
|Sichtbarkeit|Zeichenfolge|Nein |
|Module|Matrix|Nein |
|Stats|nicht definiert|Nein |
|_links|Matrix|Nein |



### <a name="stats"></a>Stats


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|member_count|ganze Zahl|Nein |
|unsubscribe_count|ganze Zahl|Nein |
|cleaned_count|ganze Zahl|Nein |
|member_count_since_send|ganze Zahl|Nein |
|unsubscribe_count_since_send|ganze Zahl|Nein |
|cleaned_count_since_send|ganze Zahl|Nein |
|campaign_count|ganze Zahl|Nein |
|campaign_last_sent|ganze Zahl|Nein |
|merge_field_count|ganze Zahl|Nein |
|avg_sub_rate|Zahl|Nein |
|avg_unsub_rate|Zahl|Nein |
|target_sub_rate|Zahl|Nein |
|open_rate|Zahl|Nein |
|click_rate|Zahl|Nein |
|last_sub_date|Zeichenfolge|Nein |
|last_unsub_date|Zeichenfolge|Nein |



### <a name="getlistsresponsemodel"></a>GetListsResponseModel


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Listen|Matrix|Nein |
|total_items|ganze Zahl|Nein |



### <a name="newmemberinlistrequest"></a>NewMemberInListRequest


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|email_type|Zeichenfolge|Nein |
|Status|Zeichenfolge|Ja |
|merge_fields|nicht definiert|Nein |
|Interessen|Zeichenfolge|Nein |
|Sprache|Zeichenfolge|Nein |
|VIP|Boolesch|Nein |
|Speicherort|nicht definiert|Nein |
|email_address|Zeichenfolge|Ja |



### <a name="firstandlastname"></a>FirstAndLastName


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|FNAME|Zeichenfolge|Nein |
|LNAME|Zeichenfolge|Nein |



### <a name="location"></a>Speicherort


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Breite|Zahl|Nein |
|Länge|Zahl|Nein |



### <a name="memberresponsemodel"></a>MemberResponseModel


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|email_address|Zeichenfolge|Nein |
|unique_email_id|Zeichenfolge|Nein |
|email_type|Zeichenfolge|Nein |
|Status|Zeichenfolge|Nein |
|merge_fields|nicht definiert|Nein |
|Interessen|Zeichenfolge|Nein |
|Stats|nicht definiert|Nein |
|ip_signup|Zeichenfolge|Nein |
|timestamp_signup|Zeichenfolge|Nein |
|ip_opt|Zeichenfolge|Nein |
|timestamp_opt|Zeichenfolge|Nein |
|member_rating|ganze Zahl|Nein |
|last_changed|Zeichenfolge|Nein |
|Sprache|Zeichenfolge|Nein |
|VIP|Boolesch|Nein |
|email_client|Zeichenfolge|Nein |
|Speicherort|nicht definiert|Nein |
|last_note|nicht definiert|Nein |
|list_id|Zeichenfolge|Nein |
|_links|Matrix|Nein |



### <a name="lastnote"></a>Last_Note


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|note_id|ganze Zahl|Nein |
|created_at|Zeichenfolge|Nein |
|created_by|Zeichenfolge|Nein |
|Notiz|Zeichenfolge|Nein |



### <a name="getallmembersresponsemodel"></a>GetAllMembersResponseModel


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Mitglieder|Matrix|Nein |
|list_id|Zeichenfolge|Nein |
|total_items|ganze Zahl|Nein |



### <a name="object"></a>Objekt






### <a name="updatememberinlistrequest"></a>UpdateMemberInListRequest


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|email_address|Zeichenfolge|Nein |
|email_type|Zeichenfolge|Nein |
|Status|Zeichenfolge|Ja |
|merge_fields|nicht definiert|Nein |
|Interessen|Zeichenfolge|Nein |
|Sprache|Zeichenfolge|Nein |
|VIP|Boolesch|Nein |
|Speicherort|nicht definiert|Nein |



### <a name="getmembersresponsemodel"></a>GetMembersResponseModel


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Mitglieder|Matrix|Nein |
|list_id|Zeichenfolge|Nein |
|total_items|ganze Zahl|Nein |



### <a name="adduserresponsemodel"></a>AddUserResponseModel


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Ja |
|email_address|Zeichenfolge|Ja |
|unique_email_id|Zeichenfolge|Nein |
|email_type|Zeichenfolge|Nein |
|Status|Zeichenfolge|Nein |
|merge_fields|nicht definiert|Ja |
|Interessen|Zeichenfolge|Nein |
|Stats|nicht definiert|Nein |
|ip_signup|Zeichenfolge|Nein |
|timestamp_signup|Zeichenfolge|Nein |
|ip_opt|Zeichenfolge|Nein |
|timestamp_opt|Zeichenfolge|Nein |
|member_rating|ganze Zahl|Nein |
|last_changed|Zeichenfolge|Nein |
|Sprache|Zeichenfolge|Nein |
|VIP|Boolesch|Nein |
|email_client|Zeichenfolge|Nein |
|Speicherort|nicht definiert|Nein |
|last_note|nicht definiert|Nein |
|list_id|Zeichenfolge|Nein |
|_links|Matrix|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)
