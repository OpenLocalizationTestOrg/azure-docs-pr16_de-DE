<properties
    pageTitle="Bereitstellen von großen Bereitstellungen"
    description="Informationen Sie zum Bereitstellen von großer Bereitstellungen mit dem Azure-Toolkit für "Ellipse"."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->

# <a name="deploying-large-deployments"></a>Bereitstellen von großen Bereitstellungen #

Ist die Bereitstellung zu groß im Standardordner Approot enthalten sein soll, können eine lokale Speicherressource als Bereitstellungsstammordner für Ihre JDK und Anwendungsserver.

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>Verwenden Sie eine lokale Speicherressource als Stammordner der Bereitstellung für große Bereitstellungen ##

1. Erstellen Sie eine neue lokale Speicherressource ein. Der Namen der Ressource spielt keine Rolle. Speicherressourcen werden auf der Rollenebene definiert. Am schnellsten können Sie im Konfigurationsdialogfeld lokaler Speicher zugreifen, aus dem Sie eine neue lokale Speicherressource erstellen konnte, wird anhand der folgenden Schritte: mit der rechten Maustaste in der Rolle in der **Projekt-Explorer** -Ansicht (erweitern Ihre Azure Projektknoten aus, wenn die Rolle nicht angezeigt wird), klicken Sie auf **Azure**, und klicken Sie dann auf **Lokale Speicher**. Klicken Sie in das Dialogfeld **Lokaler Speicher** auf **Hinzufügen** , um eine neue lokale Speicherressource erstellen.
1. Legen Sie die gewünschte Größe auf mindestens 2048 MB (nichts möglicherweise weniger derselben Datei Größe Probleme verursachen, als würde der Begegnung in der Approot).
1. Stellen Sie sicher, dass **Bereinigen des Inhalts, wenn die Instanz der Rolle wiederverwendet wird** aktiviert ist; Dadurch wird verhindern, dass die Bereitstellung des Start-Logik in Konflikte mit bereits vorhandenen Dateien in der Ressource ausgeführt wird, wenn die Instanz der Rolle wieder verwendet wird.
1. Stellen Sie sicher, dass der **Umgebungsvariable speichern Pfad der Ressource nach der Bereitstellung von** Wert auf die Zeichenfolge **DEPLOYROOT**festgelegt ist. Der lokale Speicher Ressource Dialogfeld sieht ähnlich wie der folgende aus.
    ![][ic667943]

Alternativ, wenn Sie den *Namen* der lokalen Ressource **DEPLOYROOT** verwenden und Änderung nicht-Umgebung, die automatisch generierte Variable Name (auf **DEPLOYROOT_PATH** in diesem Fall festgelegt wird), die für eine Anwendung auch funktioniert.

Weitere Informationen zum Erstellen einer lokalen Speicherressource kann am [lokalen Speichereigenschaften][]gefunden werden.

## <a name="see-also"></a>Siehe auch ##

[Azure-Toolkit für "Ellipse"][]

[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"][]

[Installieren des Azure-Toolkits für "Ellipse"][] 

Weitere Informationen zur Verwendung von Azure mit Java finden Sie im [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java-Entwicklercenter]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Toolkit für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699529
[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installieren des Azure-Toolkits für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkId=699546
[Speichereigenschaften der lokalen]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png
