<properties 
    pageTitle="Übersicht über die Widevine Lizenz Vorlagen | Microsoft Azure" 
    description="Dieses Thema bietet einen Überblick über die Vorlage Lizenz Widevine, mit dem Widevine Lizenzen konfigurieren." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="widevine-license-template-overview"></a>Widevine Lizenz Vorlage (Übersicht)

##<a name="overview"></a>(Übersicht)

Azure Media Services ermöglicht jetzt konfigurieren und Widevine Lizenzen anfordern. Wenn der Endbenutzer-Player zur Wiedergabe von Widevine geschützten Inhalten versucht, wird eine Anforderung der Lizenz Übermittlungsdienst gesendet, eine Lizenz. Wenn Sie der Lizenzdienst Zustimmung, gibt er die Lizenz, die an den Client gesendet wird und entschlüsseln und den angegebenen Inhalt wiedergeben verwendet werden kann.

Widevine Lizenz Anforderung wird als JSON-Nachricht formatiert.  

Beachten Sie, dass die Möglichkeit, eine leere Nachricht ohne Werte erstellen nur "{}" und alle Standardeinstellungen eine Lizenz Vorlage erstellt werden.  

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

##<a name="json-message"></a>JSON-Nachricht

Namen | Wert | Beschreibung
---|---|---
Nutzlast |Base64-codierte Zeichenfolge |Von einem Client gesendeten Anforderung der Lizenz. 
content_id | Base64-codierte Zeichenfolge|Bezeichner zum KeyId(s) und Inhalt Schlüssel für jede content_key_specs.track_type abgeleitet werden.
Anbieter |Zeichenfolge |Zum Suchen von Inhalten Tasten und Richtlinien verwendet. Erforderlich.
policy_name | Zeichenfolge |Der Name einer zuvor registrierten Richtlinie. Optional
allowed_track_types | Aufzählung  | SD_ONLY oder SD_HD. Steuert, welche Schlüssel Inhalte sollten in eine Lizenz aufgenommen werden
content_key_specs | Array von JSON-Strukturen, finden Sie unter **Inhalt Schlüssel Spezifikationen** unten | Eine genauere Abstimmung Steuerelement auf Inhalte Pfeiltasten, um zurückzukehren. Details finden Sie unter Inhalt Schlüssel Spezifikation unter.  Nur eine der Allowed_track_types und Content_key_specs kann angegeben werden. 
use_policy_overrides_exclusively | Boolesch. WAHR oder falsch | Verwenden von Policy_overrides angegebenen Richtlinienattribute und alle zuvor gespeicherte Richtlinie auslassen.
policy_overrides | JSON-Struktur, finden Sie unter folgenden **Richtlinie überschreibt** | Richtlinieneinstellungen für diese Lizenz.  Für den Fall, dass diese Anlage eine vordefinierte Richtlinie enthält, werden diese Werte angegebenen verwendet werden. 
session_init | JSON-Struktur, finden Sie unter **Initialization Sitzung** unten | Optionale Daten an Lizenz übergeben.
parse_only | Boolesch. WAHR oder falsch | Die Anfrage Lizenz wird jedoch keine Lizenz ausgestellt wurde. Allerdings Werte Formular, das die Anfrage Lizenz in der Antwort zurückgegeben werden.  

##<a name="content-key-specs"></a>Inhalt Schlüssel Specification 

Wenn eine bereits vorhandene Richtlinie vorhanden, gibt es keinen der Werte in den Inhalt Schlüssel Spezifikationen nicht angegeben.  Die bereits vorhandene Richtlinie dieses Inhaltstyps zugeordnet wird anhand den Ausgabe Schutz wie HDCP und CGMS verwendet werden.  Wenn Sie eine bereits vorhandene Richtlinie mit Widevine License Server nicht registriert ist, kann der Inhalt Provider die Werte in die Anfrage Lizenz einfügen.   


Jede Content_key_specs muss für alle Spuren, unabhängig von der Option Use_policy_overrides_exclusively angegeben werden. 


Namen | Wert | Beschreibung
---|---|---
Content_key_specs. track_type | Zeichenfolge | Der Name eines nachverfolgen Typs. Wenn in der Besprechungsanfrage Lizenz Content_key_specs angegeben wird, stellen Sie sicher, um anzugeben, dass alle Typen explizit verfolgen. Wenn dies nicht der Fall führt bei der Wiedergabe vergangenen 10 Sekunden. 
content_key_specs  <br/> security_level | UInt32 | Client Stabilität Erfordernisse für die Wiedergabe definiert werden. <br/> 1 - Software-basierten Whitebox kryptomobilität ist erforderlich. <br/> 2 - Software kryptomobilität und eine verborgene Decoder ist erforderlich. <br/> 3 - die wichtigsten Material- und Verschlüsselung Vorgänge müssen innerhalb einer gesicherten vertrauenswürdige Ausführung Hardware-Umgebung ausgeführt werden. <br/> 4 - der kryptomobilität und Umwandlung eines Inhalt müssen innerhalb einer gesicherten vertrauenswürdige Ausführung Hardware-Umgebung ausgeführt werden.  <br/> 5 - die Verschlüsselung, Decodierung und alle Versand der Medien (komprimiert und nicht komprimiert) müssen innerhalb einer gesicherten vertrauenswürdige Ausführung Hardware-Umgebung behandelt werden.  
content_key_specs <br/> required_output_protection.hdc | Zeichenfolge:-eine der: HDCP_NONE, HDCP_V1, HDCP_V2 | Gibt an, ob HDCP erforderlich ist
content_key_specs <br/>Schlüssel | Base64 <br/>codierte Zeichenfolge|Inhalt Schlüssel für diese Spur verwendet werden soll. Wenn angegeben, ist die Track_type oder Key_id erforderlich.  Mit dieser Option können den Inhalten Anbieter zum Einfügen des Inhalten Schlüssels für diese Spur anstatt Widevine Lizenzserver generieren oder einen Schlüssel zu suchen.
content_key_specs.key_ID| Base64-codierte Zeichenfolge binäre, 16 Byte | Eindeutiger Bezeichner für den Schlüssel. 


##<a name="policy-overrides"></a>Außer Kraft setzt 

Namen | Wert | Beschreibung
---|---|---
Policy_overrides. can_play | Boolesch. WAHR oder falsch | Zeigt an, die Wiedergabe des Inhalts zulässig ist. Standardmäßig ist "false".
Policy_overrides. can_persist | Boolesch. WAHR oder falsch |Gibt an, dass die Lizenz zu permanenten Speicher für die Offlineverwendung beibehalten werden kann. Standardmäßig ist "false".
Policy_overrides. can_renew | Boolesche WAHR oder falsch |Gibt an, dass dieser Lizenz Erneuerung zulässig ist. Wenn der Wert true, kann die Dauer der Lizenz mit Heartbeat erweitert werden. Standardmäßig ist "false". 
Policy_overrides. license_duration_seconds | Int64 | Zeigt an, das Zeitfenster für diese bestimmte Lizenz. Der Wert 0 gibt an, dass es keine Beschränkung für die Dauer gibt. Standardwert ist 0. 
Policy_overrides. rental_duration_seconds | Int64 | Gibt das Zeitfenster an, während der Wiedergabe zulässig ist. Der Wert 0 gibt an, dass es keine Beschränkung für die Dauer gibt. Standardwert ist 0. 
Policy_overrides. playback_duration_seconds | Int64 | Das Anzeigefenster der Zeit, nachdem die Wiedergabe innerhalb der Lizenz Dauer beginnt. Der Wert 0 gibt an, dass es keine Beschränkung für die Dauer gibt. Standardwert ist 0. 
Policy_overrides. renewal_server_url |Zeichenfolge | Alle Heartbeat (Erneuerung) Anfragen für diese Lizenz werden an der angegebenen URL weitergeleitet werden. Dieses Feld wird nur verwendet, wenn Can_renew true ist.
Policy_overrides. renewal_delay_seconds |Int64 |Wie viele Sekunden nach License_start_time, bevor Erneuerung zum ersten Mal zu übermitteln. Dieses Feld wird nur verwendet, wenn Can_renew true ist. Standardmäßig ist 0 
Policy_overrides. renewal_retry_interval_seconds | Int64 | Gibt die Verzögerung in Sekunden zwischen nachfolgende Lizenz Erneuerung Besprechungsanfragen, im Fall eines Fehlers. Dieses Feld wird nur verwendet, wenn Can_renew true ist. 
Policy_overrides. renewal_recovery_duration_seconds | Int64 | Das Fenster an, in der Wiedergabe während Erneuerung versuchten, aber nicht erfolgreich aufgrund von Back-End-Probleme mit dem Lizenzserver ist weiterhin zulässig ist. Der Wert 0 gibt an, dass es keine Beschränkung für die Dauer gibt. Dieses Feld wird nur verwendet, wenn Can_renew true ist.
Policy_overrides. renew_with_usage | Boolesche WAHR oder falsch |Gibt an, dass die Lizenz für Erneuerung übermittelt bei Verwendung gestartet wird. Dieses Feld wird nur verwendet, wenn Can_renew true ist. 

##<a name="session-initialization"></a>Sitzung Initialisierung

Namen | Wert | Beschreibung
---|---|---
provider_session_token | Base64-codierte Zeichenfolge |Dieses Sitzungstoken wird wieder in der Lizenz übergeben, und es werden nachfolgende Erneuerung vorhanden ist.  Das Sitzungstoken wird neben Sitzungen nicht beibehalten. 
provider_client_token | Base64-codierte Zeichenfolge | Clienttoken wieder in der Lizenzantwort zu senden.  Wenn die Anfrage Lizenz ein Clienttoken enthält, wird dieser Wert ignoriert. Das Clienttoken wird über hinaus Lizenz Sitzungen bestehen.
override_provider_client_token | Boolesch. WAHR oder falsch |Wenn falsch und die Anfrage Lizenz enthält ein Clienttoken, verwenden Sie das Token aus der Anforderung, selbst wenn ein Clienttoken in dieser Struktur angegeben wurde.  Wenn der Wert true, immer verwenden Sie das angegebene in dieser Struktur Token.

##<a name="configure-your-widevine-licenses-using-net-types"></a>Konfigurieren Sie Ihre Widevine Lizenzen verwenden .NET Inhaltstypen

Media Services bietet .NET APIs, mit denen Sie Ihre Lizenzen Widevine konfigurieren können. 

###<a name="classes-as-defined-in-the-media-services-net-sdk"></a>Klassen im Media Services .NET SDK definierten

Im folgenden werden die Definitionen dieser Typen.

    public class WidevineMessage
    {
        public WidevineMessage();
    
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

###<a name="example"></a>Beispiel

Im folgenden Beispiel wird gezeigt, wie in .NET APIs verwenden, um eine einfache Widevine-Lizenz zu konfigurieren.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Siehe auch

[Verwenden allgemeine PlayReady und/oder Widevine Dynamic-Verschlüsselung](media-services-protect-with-drm.md)
