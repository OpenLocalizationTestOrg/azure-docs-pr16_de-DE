<properties
   pageTitle="Leitfaden zum Erstellen einer Data Service von Marketplace | Microsoft Azure"
   description="Wenn Sie ausführliche Anweisungen zum Erstellen, zertifizieren und Bereitstellen einer Data Service für kaufen auf Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="examples-of-mapping-an-existing-web-service-to-odata-through-csdls"></a>Beispiele für einen vorhandenen Webdienst mit OData bis CSDLs Zuordnung

>[AZURE.IMPORTANT] **Zu diesem Zeitpunkt sind wir nicht mehr Onboarding alle neuen Data Service Herausgeber. Neue Dataservices wird nicht für Auflistung genehmigt abrufen.** Wenn Sie eine SaaS Business-Anwendung haben Sie auf Elemente verwenden veröffentlichen möchten Sie weitere Informationen finden Sie [hier](https://appsource.microsoft.com/partners). Oder wenn Sie eine IaaS Applikationen Entwicklertools Dienst auf Azure Marketplace, die Sie veröffentlichen möchten weitere Informationen finden Sie [hier](https://azure.microsoft.com/marketplace/programs/certified/).

## <a name="example-functionimport-for-raw-data-returned-using-post"></a>Beispiel: FunctionImport für "Rohstoffe" Daten mithilfe von "Beitrag" zurückgegeben
Verwenden Sie POST unformatierten Daten erstellen eine neue untergeordnete und zurückgeben, deren Server definiert URL(location) oder um Teil der untergeordnete auf dem Server zu aktualisieren definiert URL ein.  Wo finde ich die untergeordnete Stream, d. h. sowie Ex. eine Textdatei.  Beachten Sie Beitrag in nicht Idempotent ohne einen Speicherort aus.

        <!--  No EntitySet or EntityType nodes required for Raw output-->
        <FunctionImport Name="AddUsageEvent" ReturnType="Raw(text/plain)" d:EncodeParameterValues="true" d:AllowedHttpMethods="POST" d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Add usage event (data acquisition)</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>

## <a name="example-functionimport-using-delete"></a>Beispiel: FunctionImport mithilfe von "Löschen"
Verwenden Sie löschen, um einen angegebenen URI zu entfernen.

        <EntitySet Name="DeleteUsageFileEntitySet" EntityType="MyOffer.DeleteUsageFileEntity" />
        <FunctionImport Name="DeleteUsageFile" EntitySet="DeleteUsageFileEntitySet" ReturnType="Collection(MyOffer.DeleteUsageFileEntity)"  d:AllowedHttpMethods="DELETE" d:EncodeParameterValues="true” d:BaseUri=”http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643" >
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Delete usage File</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>
        <EntityType Name="DeleteUsageFileEntity" d:Map="//boolean">
        <Property Name="boolean" Type="String" Nullable="true" d:Map="./boolean" />
        </EntityType>

## <a name="example-functionimport-using-post"></a>Beispiel: FunctionImport mithilfe von "Veröffentlichen"
Verwenden Sie POST unformatierten Daten erstellen eine neue untergeordnete und zurückgeben, deren Server definiert URL(location) oder um Teil der untergeordnete auf dem Server zu aktualisieren definiert URL ein.  Dabei ist der untergeordnete eine Struktur. Vorsicht vor Beitrag ist nicht Idempotent ohne einen Speicherort.

        <EntitySet Name="CreateANewModelEntitySet2" EntityType=" MyOffer.CreateANewModelEntity2" />
        <FunctionImport Name="CreateModel" EntitySet="CreateANewModelEntitySet2" ReturnType="Collection(MyOffer.CreateANewModelEntity2)" d:EncodeParameterValues="true" d:AllowedHttpMethods="POST" d:BaseUri=”http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Create A New Model</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>

## <a name="example-functionimport-using-put"></a>Beispiel: FunctionImport mithilfe von "Einfügen"
Verwenden Sie sich aus, um eine neue untergeordnete erstellen oder aktualisieren Sie die gesamte untergeordnete bei einer URL Server definiert.  In der untergeordnete eine Struktur ist, ist sich Idempotent, daher mehrere Vorkommen im gleichen Zustand, z. B. X = 5.  Sich sollte mit dem gesamten Inhalt der angegebenen Ressource verwendet werden.

        <EntitySet Name="UpdateAnExistingModelEntitySet" EntityType="MyOffer.UpdateAnExistingModelEntity" />
        <FunctionImport Name="UpdateModel" EntitySet="UpdateAnExistingModelEntitySet" ReturnType="Collection(MyOffer.UpdateAnExistingModelEntity)" d:EncodeParameterValues="true" d:AllowedHttpMethods="PUT" d:BaseUri=”http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Update an Existing Model</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>
        <EntityType Name="UpdateAnExistingModelEntity" d:Map="//string">
        <Property Name="string"     Type="String" Nullable="true" d:Map="./string" />
        </EntityType>


## <a name="example-functionimport-for-raw-data-returned-using-put"></a>Beispiel: FunctionImport für "Rohstoffe" Daten mithilfe von "Setzen" zurückgegeben
Verwenden Sie setzen unformatierten Daten aus, um eine neue untergeordnete erstellen oder aktualisieren Sie die gesamte untergeordnete bei einer URL Server definiert.  Wo finde ich die untergeordnete Stream, d. h. sowie Ex. eine Textdatei.  SICH ist Idempotent, daher mehrere Vorkommen im gleichen Zustand, z. B. X = 5.  Mit dem gesamten Inhalt der angegebenen Ressource sollte sich verwendet werden.

        <!--  No EntitySet or EntityType nodes required for Raw output-->
        <FunctionImport Name="CancelBuild” ReturnType="Raw(text/plain)" d:AllowedHttpMethods="PUT" d:EncodeParameterValues="true" d:BaseUri=” http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Cancel Build</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>


## <a name="example-functionimport-for-raw-data-returned-using-get"></a>Beispiel: FunctionImport für "Rohstoffe" Daten "Abrufen" mit zurückgegeben
Verwenden Sie erste unformatierten Daten um eine untergeordnete zurückzugeben, die sowie ist d. h. Text ein.

        <!--  No EntitySet or EntityType nodes required for Raw output-->
        <FunctionImport Name="GetModelUsageFile" ReturnType="Raw(text/plain)" d:EncodeParameterValues="true" d:AllowedHttpMethods="GET" d:BaseUri="https://cmla.cloudapp.net/api2/model/builder/build?buildId={buildId}&amp;apiVersion={apiVersion}">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Download A Models Usage File</d:Description>
        <d:Headers>
        <d:Header d:Name="Accept" d:Value="application/xml,application/xhtml+xml,text/html;" />
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>

## <a name="example-functionimport-for-paging-through-returned-data"></a>Beispiel: FunctionImport für "Seitennavigation" durch die zurückgegebenen Daten
Verwenden von Rest Seitennavigation implementieren, durch die Ihre Daten mit abrufen.  Standard-Seitennavigation wird auf 100 Datenzeile pro Seite festgelegt.

        <EntitySet Name=”CropEntitySet" EntityType="MyOffer.CropEntity" />
        <FunctionImport Name="GetCropReport" EntitySet="CropEntitySet” ReturnType="Collection(MyOffer.CropEntity)" d:EmitSelfLink="false" d:EncodeParameterValues="true" d:Paging="SkipTake" d:MaxPageSize="100" d:BaseUri="http://api.mydata.org/Crop? report={report}&amp;series={series}&amp;start={$skip}&amp;size=100">
        <Parameter Name="report" Type="Int32" Mode="In" Nullable="false" d:SampleValues="4"  d:enum="1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|18|19"  />
        <Parameter Name="series"    Type="String"   Mode="In" Nullable="false" d:SampleValues="FARM" />
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="text/xml;charset=UTF-8" />
        </d:Headers>
        <d:Namespaces>
        <d:Namespace d:Prefix="diffgr" d:Uri="urn:schemas-microsoft-com:xml-diffgram-v1" />
        </d:Namespaces>
        </FunctionImport>

## <a name="see-also"></a>Siehe auch
- Wenn Sie die allgemeine OData-Zuordnungsprozess und Zweck Grundlegendes interessiert sind, lesen Sie diesen Artikel [Data Service OData-Zuordnung](marketplace-publishing-data-service-creation-odata-mapping.md) zu Definitionen, Strukturen und Anweisungen zu überprüfen.
- Wenn Sie sich interessieren lernen und verstehen die bestimmten Knoten und deren Parameter, lesen Sie diesen Artikel [Data Service OData Zuordnung Knoten](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) Definitionen und erläuterungen, Beispiele, und Groß-/Kleinschreibung Kontext verwenden.
- Lesen Sie diesen Artikel [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md), um den vorgegebenen Pfad für die Veröffentlichung von einem Datendienst zu Azure Marketplace zurückzukehren.
