<properties 
    pageTitle="Media Services PlayReady Lizenz Vorlage (Übersicht)" 
    description="Dieses Thema bietet einen Überblick über die Vorlage Lizenz PlayReady, mit dem PlayReady Lizenzen konfigurieren." 
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

#<a name="media-services-playready-license-template-overview"></a>Media Services PlayReady Lizenz Vorlage (Übersicht)

Azure Media Services bietet nun einen Dienst für die Bereitstellung von Microsoft PlayReady Lizenzen. Wenn der Endbenutzer-Player (z. B. Silverlight) zur Wiedergabe von PlayReady geschützten Inhalten versucht, wird eine Anforderung der Lizenz Übermittlungsdienst gesendet, eine Lizenz. Wenn Sie der Lizenzdienst Zustimmung, gibt er die Lizenz, die an den Client gesendet wird und entschlüsseln und den angegebenen Inhalt wiedergeben verwendet werden kann.

Media Services bietet auch APIs, mit denen Sie Ihre Lizenzen PlayReady konfigurieren können. Lizenzen enthalten die Rechte und die Einschränkungen, die Sie für die Laufzeit PlayReady DRM erzwingen möchten, wenn ein Benutzer, geschützte Inhalte wiederzugeben versucht.
Nachfolgend finden Sie einige Beispiele für PlayReady-lizenzeinschränkungen, die Sie angeben können:

- Die DateTime, ab der die Lizenz gültig ist.
- Der DateTime-Wert, wenn die Lizenz abläuft. 
- Für die Lizenz in permanenten Speicher auf dem Client gespeichert werden. Beständige Lizenzen werden in der Regel zum offline Wiedergabe des Inhalts zulassen.
- Die minimale Sicherheitsstufe, die ein Player benötigen, um Ihre Inhalte wiedergeben. 
- Die Ausgabe Schutzebene für die Ausgabe Steuerelemente für Audio\video Inhalt. 
- Weitere Informationen finden Sie unter der Ausgabe Steuerelemente im Abschnitt (3,5) im Dokument [PlayReady Compliance-Regeln](https://www.microsoft.com/playready/licensing/compliance/) .

>[AZURE.NOTE]Derzeit können Sie nur die PlayRight der Lizenz PlayReady konfigurieren (dieses Recht ist erforderlich). Die PlayRight bietet dem Client die Möglichkeit, um die Wiedergabe des Inhalts. Die PlayRight kann auch die Einschränkungen, die speziell für Wiedergabe konfigurieren. Weitere Informationen finden Sie unter [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight).

PlayReady Lizenzen Media-Dienste verwenden um zu konfigurieren, müssen Sie die Vorlage Media Services PlayReady Lizenz konfigurieren. Die Vorlage wird in XML definiert.

Im folgenden Beispiel wird die einfachste (und am häufigsten verwendete) Vorlage, die eine einfache streaming Lizenz konfiguriert. Mit dieser Lizenz, Kunden wären Lage Wiedergabe Ihrer PlayReady geschützten Inhalten.
    
    <?xml version="1.0" encoding="utf-8"?>
    <PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" 
                                      xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <PlayRight />
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

Das XML entspricht dem PlayReady Lizenz Vorlage XML-Schema in der PlayReady Lizenz Vorlage XML-Schemaabschnitt definiert.

Media-Dienste definiert auch eine Reihe von .NET Klassen, die serialisiert und deserialisiert zu und aus dem XML-in verwendet werden kann. Beschreibung der wichtigsten Klassen finden Sie unter [Medien Services .NET Klassen](media-services-playready-license-template-overview.md#classes). die verwendeten Lizenz Vorlagen konfigurieren.

Für eine End-to-End-Beispiel, bei dem .NET Klassen zum Konfigurieren der PlayReady Lizenz Vorlage finden Sie unter [Dynamic PlayReady-Verschlüsselung verwenden und Lizenz Übermittlungsdienst](https://msdn.microsoft.com/library/azure/dn783467.aspx)verwendet.

##<a name="a-idclassesamedia-services-net-classes-that-are-used-to-configure-license-templates"></a><a id="classes"></a>Media Services .NET Klassen, die verwendet werden, um die Lizenz Vorlagen konfigurieren

Im folgenden sind die wichtigsten .NET Klassen so konfigurieren Sie Media Services PlayReady Lizenz Vorlagen verwendet werden. Diese Klassen zuordnen den Typen in [PlayReady Lizenz Vorlage XML-Schema](media-services-playready-license-template-overview.md#schema)definiert.

Die [MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) -Klasse dient zum Serialisieren und Deserialisieren an und von XML-Vorlage Lizenz Media-Dienste.

###<a name="playreadylicenseresponsetemplate"></a>PlayReadyLicenseResponseTemplate

[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx) – diese Klasse stellt die Vorlage für die Antwort an den Endbenutzer gesendet. Sie enthält ein Feld für eine benutzerdefinierte Datenzeichenfolge zwischen License Server und der Anwendung (für benutzerdefinierte app Logik hilfreich sein kann) sowie eine Liste der Vorlagen für eine oder mehrere Lizenz.

Dies ist der Klasse 'obersten Ebene' in der Vorlagenhierarchie. Was bedeutet, dass die Antwortvorlage eine Liste der Lizenz Vorlagen umfasst und die Lizenz Vorlagen (direkt oder indirekt) alle anderen Klassen, die die Vorlagendaten zusammensetzt serialisiert werden soll.


###<a name="playreadylicensetemplate"></a>PlayReadyLicenseTemplate

[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx) - Klasse stellt eine Lizenz-Vorlage zum Erstellen von PlayReady Lizenzen für den Endbenutzer zurückgegeben werden soll. Sie enthält die Daten auf den Inhalt Schlüssel in der Lizenz und einem beliebigen Rechte oder Einschränkungen um von der PlayReady DRM Runtime erzwungen werden, wenn den Inhalten Schlüssel verwenden.


###<a name="a-idplayreadyplayrightaplayreadyplayright"></a><a id="PlayReadyPlayRight"></a>PlayReadyPlayRight

[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx) – diese Klasse stellt die PlayRight einer PlayReady-Lizenz. Es erhält der Benutzer die Möglichkeit, die Wiedergabe des Inhalts unterliegen die 0 oder mehr Einschränkungen, die in der Lizenz und klicken Sie auf die PlayRight selbst (für bestimmte Wiedergabe-Richtlinie) konfiguriert. Viele die Richtlinie auf die PlayRight hat tun mit Ausgabe Einschränkungen, die die Arten von Ausgaben zu steuern, denen über der Inhalt wiedergegeben werden kann und Einschränkungen, die bei Verwendung eines angegebenen direkte vorangestellt werden müssen. Angenommen, wenn die DigitalVideoOnlyContentRestriction aktiviert ist, lässt klicken Sie dann die DRM Runtime nur das Video für digitale Ausgänge angezeigt werden (analoge video Ausgaben wird nicht zulässig sein, um den Inhalt zu übergeben).

>[AZURE.IMPORTANT]Diese Arten von Einschränkungen können sehr leistungsfähige jedoch Benutzerfunktionen können auch auswirken. Wenn die Ausgabe Schutzmechanismen auch nach konfiguriert sind, Nutzungsrechte auf einige Clients möglicherweise der Inhalt. Weitere Informationen finden Sie unter dem Dokument [PlayReady Compliance-Regeln](https://www.microsoft.com/playready/licensing/compliance/) .

Ein Beispiel für welche Schutz Silverlight unterstützt Ebenen, finden Sie unter: [ohne Silverlight-Unterstützung für die Ausgabe Schutzmechanismen](http://go.microsoft.com/fwlink/?LinkId=617318).

##<a name="a-idschemaaplayready-license-template-xml-schema"></a><a id="schema"></a>XML-Schemas PlayReady Lizenz-Vorlagen
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:ser="http://schemas.microsoft.com/2003/10/Serialization/" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:import namespace="http://schemas.microsoft.com/2003/10/Serialization/" />
      <xs:complexType name="AgcAndColorStripeRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction" />
      <xs:simpleType name="ContentType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Unspecified" />
          <xs:enumeration value="UltravioletDownload" />
          <xs:enumeration value="UltravioletStreaming" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="ContentType" nillable="true" type="tns:ContentType" />
      <xs:complexType name="ExplicitAnalogTelevisionRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="BestEffort" type="xs:boolean" />
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ExplicitAnalogTelevisionRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction" />
      <xs:complexType name="PlayReadyContentKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="PlayReadyContentKey" nillable="true" type="tns:PlayReadyContentKey" />
      <xs:complexType name="ContentEncryptionKeyFromHeader">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence />
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromHeader" nillable="true" type="tns:ContentEncryptionKeyFromHeader" />
      <xs:complexType name="ContentEncryptionKeyFromKeyIdentifier">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence>
              <xs:element minOccurs="0" name="KeyIdentifier" type="ser:guid" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromKeyIdentifier" nillable="true" type="tns:ContentEncryptionKeyFromKeyIdentifier" />
      <xs:complexType name="PlayReadyLicenseResponseTemplate">
        <xs:sequence>
          <xs:element name="LicenseTemplates" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
          <xs:element minOccurs="0" name="ResponseCustomData" nillable="true" type="xs:string">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseResponseTemplate" nillable="true" type="tns:PlayReadyLicenseResponseTemplate" />
      <xs:complexType name="ArrayOfPlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfPlayReadyLicenseTemplate" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
      <xs:complexType name="PlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AllowTestDevices" type="xs:boolean" />
          <xs:element minOccurs="0" name="BeginDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element name="ContentKey" nillable="true" type="tns:PlayReadyContentKey" />
          <xs:element minOccurs="0" name="ContentType" type="tns:ContentType">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExpirationDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="GracePeriod" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="LicenseType" type="tns:PlayReadyLicenseType" />
          <xs:element minOccurs="0" name="PlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
      <xs:simpleType name="PlayReadyLicenseType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Nonpersistent" />
          <xs:enumeration value="Persistent" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="PlayReadyLicenseType" nillable="true" type="tns:PlayReadyLicenseType" />
      <xs:complexType name="PlayReadyPlayRight">
        <xs:sequence>
          <xs:element minOccurs="0" name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AllowPassingVideoContentToUnknownOutput" type="tns:UnknownOutputPassingOption">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AnalogVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="DigitalVideoOnlyContentRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExplicitAnalogTelevisionOutputRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="FirstPlayExpiration" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComponentVideoRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComputerMonitorRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyPlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
      <xs:simpleType name="UnknownOutputPassingOption">
        <xs:restriction base="xs:string">
          <xs:enumeration value="NotAllowed" />
          <xs:enumeration value="Allowed" />
          <xs:enumeration value="AllowedWithVideoConstriction" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="UnknownOutputPassingOption" nillable="true" type="tns:UnknownOutputPassingOption" />
      <xs:complexType name="ScmsRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction" />
    </xs:schema>



##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
