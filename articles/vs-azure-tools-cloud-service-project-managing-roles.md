<properties
   pageTitle="Verwalten von Rollen in der Azure Cloud services-Projekten mit Visual Studio | Microsoft Azure"
   description="Informationen Sie zum Hinzufügen neuer Rollen zum Projekt Azure-Cloud-Dienst oder vorhandene Rollen aufheben, indem Sie mit Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-roles-in-the-azure-cloud-services-projects-with-visual-studio"></a>Verwalten von Rollen in der Azure Cloud Services-Projekte mit Visual Studio

Nachdem Sie Ihre Azure-Cloud-Dienstprojekt erstellt haben, können Sie neue Rollen hinzufügen oder vorhandene Rollen aus der Nachricht entfernen. Sie können auch ein vorhandenes Projekt importieren und wandeln Sie es in eine Rolle. Beispielsweise können Sie eine ASP.NET Web-Anwendung zu importieren, und legen es als Webrolle.

## <a name="adding-or-removing-roles"></a>Hinzufügen oder Entfernen von Rollen

**Hinzufügen eine Rolle**

Klicken Sie in der **Lösung Explorer**öffnen Sie das Kontextmenü für den Knoten **Rollen** in Ihrem Projekt Cloud-Dienst, und wählen Sie **Hinzufügen**. Wählen Sie eine vorhandene Webrolle oder Worker-Rolle aus der aktuellen Lösung oder Erstellen eines neuen Projekts von Web oder Arbeitskollegen Rolle können. Oder eine entsprechende Projekt, z. B. einer ASP.NET Web-Anwendung-Projekt auswählen und verbinden Sie es mit einem Projekt Rolle.

**So entfernen Sie eine Rolle association**

Öffnen Sie in den Knoten **Rollen** des Projekts Cloud-Dienst in Lösung Explorer das Kontextmenü für die Rolle zu entfernen, und wählen Sie **Entfernen**aus.

## <a name="removing-and-adding-roles-in-your-cloud-service"></a>Entfernen und Hinzufügen von Rollen in der Cloud-Dienst

Wenn Sie eine Rolle aus Ihrem Projekt Cloud-Dienst entfernen, aber später entscheiden, die Rolle wieder zum Projekt hinzufügen, werden nur die Rolle Deklaration und grundlegende Attribute, wie z. B. Endpunkte und Diagnose Informationen hinzugefügt. Keine zusätzliche Ressourcen oder Bezüge werden in der Datei ServiceDefinition.csdef oder die Datei ServiceConfiguration.cscfg hinzugefügt. Wenn Sie diese Informationen hinzufügen möchten, müssen Sie sie manuell wieder in diese Dateien hinzufügen zu können.

Angenommen, Sie möglicherweise eine Web Service Rolle entfernen und später wieder in Ihre Lösung die Funktion hinzugefügt haben. Wenn Sie dies tun, tritt ein Fehler auf. Um diesen Fehler zu vermeiden, müssen Sie das Hinzufügen der `<LocalResources>` Element in der folgenden XML wieder in der Datei ServiceDefinition.csdef angezeigt. Verwenden Sie den Namen der Rolle des Web-Dienst, die Sie als Teil der Name-Attribut für das Projekt wieder hinzugefügt der **<LocalStorage>** Element. In diesem Beispiel wird der Name der Rolle des Web-Dienst **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Nächste Schritte

Informationen Sie zum Konfigurieren von Rollen in Visual Studio durch [konfigurieren die Rollen für einen Azure-Cloud-Dienst mit Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md)lesen.
