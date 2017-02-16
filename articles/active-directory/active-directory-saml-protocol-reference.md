<properties
    pageTitle="Azure AD SAML-Protokoll Verweis | Microsoft Azure"
    description="Dieser Artikel enthält eine Übersicht über die einmaliges Anmelden und einzelnen Sign-Out SAML Profile in Azure Active Directory."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/23/2016"
    ms.author="priyamo"/>


# <a name="how-azure-active-directory-uses-the-saml-protocol"></a>Wie Azure Active Directory das SAML-Protokoll verwendet

Azure Active Directory (Azure AD) verwendet die SAML 2.0-Protokoll Aktivieren von Applications eine einzelne anmelden Erfahrung ihren Benutzern zur Verfügung gestellt. Die Profilen [Einmaliges Anmelden](active-directory-single-sign-on-protocol-reference.md) und [Abmelden Single](active-directory-single-sign-out-protocol-reference.md) SAML Azure AD-wird erläutert, wie SAML-Assertionen, Protokolle und Bindungen in der Identität-Anbieter-Dienst verwendet werden.

SAML-Protokoll erfordert der Identitätsanbieter (Azure AD-) und dem Dienstanbieter (die Anwendung) zum Austauschen von Informationen über sich selbst.

Wenn eine Anwendung mit Azure AD registriert ist, registriert der app-Entwickler Föderation-bezogene Informationen mit Azure AD an. Dies umfasst die **URI umleiten** und **Metadaten URI** der Anwendung.

Azure AD verwendet die **Metadaten URI** des Cloud-Dienst, um den signierenden Schlüssel und die Abmeldung URI des Cloud-Dienst abzurufen. Wenn die Anwendung einer Metadaten URI nicht unterstützt, der Entwickler muss wenden Sie sich an Microsoft Support, um die Abmeldung URI bereitstellen und Schlüssel signieren.

Azure-Active Directory macht Mandanten-spezifische und allgemeine (Mandanten unabhängig) einzelne anmelden und einzelnen Abmeldung Endpunkte verfügbar. Diese URLs darstellen adressiert Speicherorte – sie sind nicht nur ein Bezeichner –, sodass Sie bis zum Lesen der Metadaten-Endpunkt wechseln können.

 - Der Endpunkt Mandanten-spezifische befindet sich unter `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  Die <TenantDomainName> Platzhalter steht für einen registrierten Domänennamen oder TenantID GUID des einem Azure AD-Mandanten. Beträgt beispielsweise die Föderation Metadaten für den Mandanten contoso.com am: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

- Der Endpunkt Mandanten unabhängig befindet sich unter `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. In dieser Endpunktadresse angezeigt wird **häufig** , anstelle eines Mandanten Domänen- oder -ID an.

Informationen über die Föderation Metadaten Dokumente die Azure AD veröffentlicht, finden Sie unter [Föderation Metadaten](active-directory-federation-metadata.md).
