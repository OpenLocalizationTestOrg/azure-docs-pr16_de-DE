<properties title="" pageTitle="Dateinamen und Speicherorte für Azure technische Artikel" description="Erläutert die Dateistruktur für die Artikel und die Benennungskonventionen, die sollten Sie beim Erstellen eines neuen Artikels befolgen." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="tysonn" />

#<a name="file-names-and-locations-for-azure-technical-articles"></a>Dateinamen und Speicherorte für Azure technische Artikel

In unseren technischen Content Repository verwenden wir einen einzelnen Ordner (der Ordner **Artikel** ) für alle Artikel aus. Es gibt keine Ordnerhierarchie: alle Artikel in die Struktur Flatfile Leben. Wenn Sie mit dem Artikel in diese Ordner erstellen, können der Artikel veröffentlicht werden.

Statt eine Dateistruktur als ein zum Organisieren von Prinzip, verwenden wir eine Datei strict Benennungskonvention, klar identifiziert Themen und die Auffindbarkeit im Web einen Beitrag.

Hier ist, was Sie wissen müssen:

+ [Regeln]
+ [Muster]
+ [Standard-Beispiele]
+ [Marketplace-Inhalt]
+ [Datei Namen Genehmigung]

##<a name="rules"></a>Regeln

- Dateinamen können nur Kleinbuchstaben, Zahlen und Bindestriche enthalten. 
- Keine Leerzeichen oder Interpunktionszeichen. Verwenden Sie die Bindestriche zum Trennen von Wörtern und Zahlen im Dateinamen.
- Nicht mehr als 80 Zeichen – Dies ist für die Veröffentlichung System maximal
- Verwenden Verben, die bestimmte, beispielsweise sind entwickeln, kaufen, erstellen, Problembehandlung bei. Keine Wörter - Wert.
- Keine kurze Wörter - setzen kein und, die in oder, usw..
- Alle Dateien müssen werden in Abzug und die Erweiterung .md verwenden.
- Rechtschreibprüfung Wörter ab; Vermeiden Sie nicht genehmigte oder nicht mehr benötigte Akronyme in Dateinamen

Akronyme und Initialisms in Dateinamen - spezifische Richtlinien:

- Nicht gekürzt Azure Service Namen – die ersten Wörter des Dateinamens sollten die Standard ausgeschrieben Azure Service oder Technologie-Name. 
-   Lassen Sie keine RM- oder Cloud als Akronyme an einer beliebigen Stelle in einem Dateinamen
- Andere Industriestandard Abkürzungen sind je nach Bedarf in Dateinamen zulässig. 

##<a name="pattern"></a>Muster

So sieht das allgemeine Muster aus:

 **Service-Platform-Language-Content-Product-Version.MD**

Verwenden Sie die Teile des Musters, die gelten, und überprüfen die Liste der Artikel im Repository können Sie eine Vorstellung davon vorhandenen Namen zu gelangen. Namen, die nicht mit einem Entwickler Plattform beginnen oder Diensten sind wahrscheinlich verdächtige und überfälliger durch.

##<a name="standard-examples"></a>Standard-Beispiele

Hier sind einige Beispiele für gültige Namen, die dem Muster folgen. :

- Cloud-Services-Dotnet-fortlaufender-delivery.md
- Mobile-Services-Ios-Get-started.md
- Documentdb verwalten account.md
- Mobile-Services-dotnet-Backend-Get-Started-Settings-Sync.MD
- Active-Directory-Java-Authenticate-Users-Access-Control-Eclipse.MD
- Virtual-Machines-Install-Windows-Server-2008r2.MD
- Cache-Aspnet-Session-State-provider
- Azure-SDK-dotnet-Release-Notes-2-8
- storsimple-Disaster-Recovery-Using-Azure-Site-Recovery

##<a name="marketplace-content"></a>Marketplace-Inhalt

Um Inhalte zu unterscheiden, die Schwerpunkt auf Partner Beiträge zu Azure Marketplace, starten Sie die Dateinamen mit "Store". Dieses Inhaltstyps muss nicht zu häufig, werden die meisten Partner für Inhalte auf Websites Partner erstellt werden soll.

- Marketplace-MongoDB-Virtual-Machines-Install-Windows-Server-2008r2.MD

##<a name="file-name-approval"></a>Datei Namen Genehmigung

Es ist den Auftrag unsere Gruppe von Abruf Anforderung Bearbeitern Dateinamen überprüfen, wenn eine neue Datei zum ersten Mal zum Repository gesendet wird. Abruf Anforderung Bearbeitern sollte überprüfen den Dateinamen und Feedback über den Abruf Anforderung Kommentar Stream, wenn Änderungen erforderlich sind. Der Dateiname muss korrigiert werden, bevor die Abruf Anforderung akzeptiert wird. Mitwirkenden können einfach die Aktualisierung auf die Anforderung ausstehend Abruf drücken.

##<a name="folder-names-in-the-repo"></a>Ordnernamen in der repo

Ordner nur für Dienste erstellt werden soll, und der Dateinamen sollte der Dienst Slug entsprechen. Verwenden Sie nur Buchstaben und Bindestriche und verwenden Sie alle Kleinbuchstaben. Beziehen Sie Genehmigung aus der Repository-Administrator, bevor Sie einen neuen Ordner erstellen, der für einen freigegebenen Dienst nicht geeignet ist.

##<a name="changing-case-in-file-names"></a>Ändern der Schreibweise in Dateinamen

Windows-Betriebssysteme Groß-und Kleinschreibung, damit Sie einen Dateinamen ein, beheben zur Groß-und Kleinschreibung ändern müssen, besser, grundlegend Änderung, ist es sei denn, Sie nehmen Sie die Änderung auf einem Linux oder Mac können Beispiel:

  Biztalk-administration-and-Development-Task-List-in-BizTalk-Services--> biztalk-services-administration-and-development-task-list

Verwenden Sie zum Umbenennen einer Datei mit dem folgenden Befehl ein:
```
  git mv <articles/service-folder/current-file-name.md> <articles/service-folder/new-file-name>
```

###<a name="contributors-guide-links"></a>Mitwirkenden Leitfaden für Links

- [Artikel (Übersicht)](./../README.md)
- [Index der Artikel mit Hinweisen](./contributor-guide-index.md)


<!--Anchors-->
[Regeln]: #rules
[Muster]: #pattern
[Standard-Beispiele]: #standard-examples
[Marketplace-Inhalt]: #marketplace-content
[Datei Namen Genehmigung]: #file-name-approval
