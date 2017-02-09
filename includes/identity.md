Verwalten von Identität ist genauso wichtig in der öffentlichen Cloud ungeändert lokal. Um dies zu ermöglichen, unterstützt Azure mehrere anderen Cloud Identität Technologien an. Sie umfassen folgende:

- Sie können in der Cloud mit Azure-virtuellen Computern erstellte Maschinen mit Windows Server Active Directory (häufig nur AD genannt) ausführen. Dieser Ansatz ist sinnvoll, wenn Sie Azure verwenden, um Ihre lokalen Datacenter in der Cloud zu erweitern.


- Azure Active Directory können Sie Ihre Benutzer einmaliges Anmelden auf [Software als Service (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) Applications gewähren. Von Microsoft Office 365 verwendet diese Technologie, beispielsweise und Azure oder andere Cloud Plattformen ausgeführt Applications können auch verwenden.


- In der Cloud oder lokalen ausgeführt Applications verwenden Azure Active Directory Access Control zur können Benutzer melden Sie sich mit Identitäten aus Facebook, Google, Microsoft und andere Identitätsanbieter.


In diesem Artikel werden alle drei der folgenden Optionen.

## <a name="table-of-contents"></a>Inhaltsverzeichnis

- [Active Directory von Windows Server auf virtuellen Computern ausgeführt](#adinvm)

- [Verwenden von Azure Active Directory](#ad)

- [Verwenden von Azure-Active Directory Access Control](#ac)


## <a name="a-nameadinvmarunning-windows-server-active-directory-in-virtual-machines"></a><a name="adinvm"></a>Ausführen von Active Directory von Windows Server auf virtuellen Computern

Mit Windows Server AD in Azure-virtuellen Computern ähnelt es lokal ausgeführt. [Abbildung 1](#fig1) zeigt ein Beispiel für typische wie folgt aussieht.

![Azure-Active Directory auf virtuellen Computern](./media/identity/identity_01_ADinVM.png)


<a name="Fig1"></a>Abbildung 1: Windows Server Active Directory in Azure-virtuellen Computern in einer Organisation lokalen Datencenter mit Azure-virtuellen Netzwerk verbunden ausgeführt werden kann.

Im hier gezeigten Beispiel ist Windows Server AD in virtuellen Computern erstellt Azure-virtuellen Computern, Ihrer Plattform IaaS Technologie ausgeführt. Diese virtuellen Computern und einige andere werden in einem virtuellen Netzwerk mit einer lokalen Datacenter verwenden Azure-virtuellen Netzwerk verbunden gruppiert. Das virtuelle Netzwerk bietet sich eine Gruppe von Cloud-virtuellen Computern, die Interaktion mit dem lokalen Netzwerk über eine Verbindung virtuelles privates Netzwerk (VPN). Diese Berechtigung ermöglicht Aktionen aussieht nur ein anderes Subnetz in der lokalen Datencenter diese Azure-virtuellen Computern. Wie in die Abbildung dargestellt, sind zwei der betreffenden virtuellen Computern Windows Server AD Domain Controller ausgeführt. Die anderen virtuellen Computern in das virtuelle Netzwerk möglicherweise Anwendungen, wie SharePoint oder auf andere Weise, wie z. B. für verwendete Test- und ausgeführt werden. Die lokale Datacenter wird auch zwei Windows Server AD Domain Controller ausgeführt werden.

Es gibt mehrere Optionen für die Domänencontroller in der Cloud verbinden, mit denen lokal ausgeführt:

- Nehmen Sie alle für einen Teil einer einzelnen Active Directory-Domäne.

- Erstellen Sie separate AD Domänen lokal und in der Cloud, die die gleichen Struktur gehören.

- Erstellen Sie separate AD Gesamtstrukturen in der Cloud und lokale und dann verbinden Sie die Gesamtstrukturen mit Cross-Gesamtstrukturvertrauensstellungen oder Windows Server Active Directory Federation Services (AD FS), die auch virtuellen-Azure ausführen können.

Jeden Wahlmöglichkeiten besteht, ein Administrator sollte stellen Sie sicher, dass Authentifizierungsanfragen lokale Benutzer zu Cloud Domänencontroller nur bei Bedarf wechseln, da Sie der Link in der Cloud langsamer als lokale Netzwerke werden soll. Ein weiterer Faktor in Verbindungslinien Cloud und lokale Domain Controller zu berücksichtigen ist von der Replikation erzeugte Verkehr. Controller in der Cloud sind in der Regel in ihren eigenen Active Directory-Standort, der einen Administrator voraus, wie oft planen kann Replikation abgeschlossen ist. Azure Gebühren für Verkehr aus einer Azure Datacenter gesendet wird zwar nicht für Bytes gesendet, in denen des Administrators Replikation Auswahlmöglichkeiten auswirken können. Es lohnt darauf hin, dass während Azure eine eigene Unterstützung (DNS = Domain Name System) zur Verfügung stellt, dieser Dienst Active Directory (z. B. Unterstützung für dynamisches DNS und SRV-Einträge) erforderlichen Features nicht angezeigt wird. Aus diesem Grund erfordert Windows Server AD auf Windows Azure ausgeführte Sie Ihre eigenen DNS-Server in der Cloud.

Mit Windows Server AD in Azure-virtuellen Computern kann in mehreren unterschiedlichen Situationen sinnvoll. Es folgen einige Beispiele:

- Wenn Sie als Erweiterung von Ihrem eigenen Datencenter Azure-virtuellen Computern verwenden, möglicherweise Ausführung Applications in der Cloud, die lokale Domänencontroller Dinge wie Anfragen der Windows-Authentifizierung oder LDAP-Abfragen verarbeitet benötigen. SharePoint, beispielsweise interagiert häufig mit Active Directory und damit es zwar möglich, eine SharePoint-Farm auf Azure mit einem lokalen Verzeichnis auszuführen, Einrichten der Domänencontroller in der Cloud wird erheblich verbessern der Leistung. (Es ist zu beachten, dass dies nicht unbedingt erforderlich, jedoch; viel Applikationen ausgeführt werden kann erfolgreich in der Cloud nur lokale Domänencontroller verwenden.)

- Nehmen Sie an, dass eine ferne Zweigstelle verfügt nicht über die Ressourcen, um eine eigene Domänencontroller ausführen. Heute, Authentifizierung der Benutzer müssen zu Domänencontroller auf der anderen Seite der Welt - Benutzernamen langsam sind. Active Directory in einem näher Microsoft Datencenter auf Windows Azure ausgeführte kann dies beschleunigen ohne mehr Server in der Zweigstelle.

- Eine Organisation, die für die Wiederherstellung Azure verwendet möglicherweise eine kleine Gruppe von aktiven virtuellen Computern in der Cloud, einschließlich eines Domänencontrollers verwalten. Sie können dann vorbereitet auf dieser Website zum Übernehmen für Fehler an anderer Stelle nach Bedarf zu erweitern.

Es gibt auch in anderer Weise. Beispielsweise können Sie nicht erforderlich, um eine Verbindung mit einer lokalen Datacenter Windows Server AD in der Cloud. Wenn Sie möchten eine SharePoint-Farm ausführen, die bestimmte Benutzer bereitgestellt, z. B. alle wem würde ausschließlich mit Identitäten cloudbasierten anmelden, wird Sie möglicherweise eine eigenständigen Gesamtstruktur auf Azure erstellen. Wie Sie diese Technologie verwenden, hängt davon ab, was Ihre Ziele sind. (Für eine ausführlichere Anleitung zum Verwenden von Windows Server AD mit Azure, [finden Sie hier](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx).)

## <a name="a-nameadausing-azure-active-directory"></a><a name="ad"></a>Verwenden von Azure Active Directory

Sobald SaaS Applikationen häufiger sind, sie auslösen eine offensichtliche Frage: welche Art von Verzeichnisdienst sollte diese Cloud-basierten Anwendungen verwendet? Microsoft Antwort auf diese Frage ist Azure Active Directory.

Es gibt zwei wichtigste Optionen zur Verwendung dieser Verzeichnisdienstes in der Cloud aus:

- Personen und Organisationen, die nur SaaS Applikationen verwenden können als ihre einzige Verzeichnisdienst auf Azure Active Directory verlassen.

- Organisationen, die den Windows Server Active Directory ausführen können Herstellen einer Verbindung Azure Active Directory mit ihren lokalen Verzeichnis und dann verwenden, um ihre Benutzer einmaliges Anmelden auf SaaS Applikationen gewähren.


[Abbildung 2](#fig2) zeigt das erste dieser beiden Optionen, wobei Azure Active Directory allen besteht, die erforderlich ist.

![Azure-Active Directory auf virtuellen Computern](./media/identity/identity_02_AD.png)

<a name="fig2"></a>Abbildung 2: Azure-Active Directory bietet eine Organisation Benutzer einmaliges Anmelden auf SaaS-Anwendungen, einschließlich Office 365.

Wie in die Abbildung dargestellt, ist Azure AD ein mit mehreren Mandanten-Dienst an. Dies bedeutet, dass es viele verschiedene Organisationen Speichern von Verzeichnisinformationen über Websitebenutzer bei jeder von ihnen gleichzeitig unterstützen. In diesem Beispiel wird ein Benutzer in der Organisation A versucht, auf eine Anwendung SaaS zuzugreifen. Dieser Anwendung möglicherweise ein Teil des Office 365, z. B. SharePoint Online oder möglicherweise etwas anderem: nicht-Microsoft-Programme können Sie auch diese Technologie. Da Azure AD SAML 2.0-Protokoll unterstützt, von einer Anwendung erforderlich ist, lediglich die Möglichkeit, die mit diesen Industriestandard interagieren. (Tatsächlich, Azure AD verwenden, in einer beliebigen Datacenter, nicht nur eine Azure Datacenter ausgeführt werden können.)

Der Vorgang beginnt, wenn der Benutzer eine Anwendung SaaS (Schritt 1) greift auf. Wenn Sie diese Anwendung verwenden zu können, muss der Benutzer ein Token ausgestellt von Azure AD präsentieren.

Dieses Token enthält Informationen, die den Benutzer angeben, und es wird von Azure AD digital signiert. Um das Token zu gelangen, authentifiziert den Benutzer selbst zu Azure AD eines Benutzernamens und Kennworts (Abschnitt 2). Klicken Sie dann Azure AD gibt das Token er muss (Schritt 3).

Dieses Token wird dann an die Anwendung SaaS (Schritt 4) gesendet prüft das Token der Signatur und deren Inhalt (Schritt 5) verwendet. Die Anwendung wird in der Regel, die Identitätsinformationen verwenden, die das Token enthält, um zu entscheiden, welche Informationen der Benutzer für den Zugriff auf und vielleicht an anderen Methoden zulässig ist.

Wenn die Anwendung mehr Informationen zu den Benutzer benötigt als was im Token enthalten ist, können sie dies direkt aus Azure Active Directory mithilfe der Azure AD Graph-API (Schritt 6) anfordern. Das Directory-Schema in der anfänglichen Azure AD-Version ist ganz einfach: Es enthält nur Benutzer und Gruppen und Beziehungen zwischen ihnen. Diese Informationen können Applikationen Verbindungen zwischen Benutzer lernen. Nehmen Sie beispielsweise an, dass eine Anwendung muss wissen, wer Vorgesetzten dieses Benutzers ist selbst entscheiden, ob er Zugriff auf einige Datenblock gewährt hat. Sie können dies durch Abfragen über die Graph-API Azure AD erfahren.

Die Graph-API verwendet eine einfache Rest Protokoll, wodurch von den meisten Clients, einschließlich von mobilen Geräten recht einfach verwendet wird. Die API unterstützt auch die Erweiterungen durch OData, hinzufügen Punkte wie einer Abfragesprache, lassen Sie Clients Zugriff auf Daten in sinnvoller Weise definiert werden. (Weitere Informationen zum OData finden Sie unter [Einführung in OData](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf).) Da die Graph-API Beziehungen zwischen Benutzer lernen verwendet werden kann, kann es Applications im Diagramm für soziale Netzwerke zu verstehen, das im Schema Azure AD-für eine bestimmte Organisation eingebettet ist (das ist, warum sie das Graph-API aufgerufen wurde). Und wenn sich Azure AD für Besprechungsanfragen Graph-API authentifizieren, verwendet eine Anwendung OAuth 2.0.

Wenn eine Organisation nicht von Windows Server Active Directory verwenden-verfügt über keine lokalen Servern oder Domänen - und Cloudanwendungen, mit denen Azure AD ausschließlich abhängig, legen nur dieses Cloud-Verzeichnis mithilfe des Unternehmens Benutzer einmaliges Anmelden für alle von ihnen fest. Noch während dieses Szenario täglich häufiger erhält, lokalen die meisten Organisationen verwenden weiterhin Domänen mit Windows Server Active Directory erstellt. Azure AD verfügt über eine nützliche Rolle hier ebenfalls ausprobieren, wie in [Abbildung 3](#fig3) dargestellt.

![Azure-Active Directory auf virtuellen Computern](./media/identity/identity_03_AD.png)
<a id="fig3"></a>Abbildung 3: eine Organisation kann Windows Server Active Directory Azure Active Directory seine Benutzer einmaliges Anmelden auf SaaS Applikationen gewähren zusammenarbeitet.

Ein Benutzer in der Organisation B möchte in diesem Szenario eine Anwendung SaaS zugreifen. Bevor sie dies erledigt haben, müssen der Organisation Directory-Administratoren eine Föderation-Beziehung mit Azure Active Directory mithilfe von AD FS, wie in der Abbildung dargestellt einrichten. Außerdem müssen diese Administratoren Synchronisierung von Daten zwischen der Organisation lokalen Windows Server Active Directory und Azure AD-konfigurieren. Dies kopiert automatisch Benutzer und Gruppeninformationen aus dem lokalen Verzeichnis in Azure AD. Beachten Sie, was auf diese Weise können: die Organisation ist, deren lokalen Verzeichnis in der Cloud erweitern. Kombinieren von Windows Server Active Directory und Azure AD-auf diese Weise bietet der Organisation einen Verzeichnisdienst, der als einzelne Einheit, trotz einer Präsenz beide lokal und in der Cloud verwaltet werden kann.

Um Azure AD verwenden, meldet sich der Benutzer zuerst dem lokalen Active Directory-Domäne wie gewohnt (Schritt 1). Wenn Anna versucht, die Anwendung SaaS (Schritt 2) zugreifen, führt der Prozess Föderation Azure AD ausgeben sich ein Token für diese Anwendung (Schritt 3). (Weitere Informationen zur Funktionsweise von Föderation finden Sie unter [anspruchsbasierte Identität für Windows: Technologien und Szenarien mit mehreren](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Wie zuvor dieses Token enthält Informationen, die den Benutzer angeben, und es wird von Azure AD digital signiert. Dieses Token wird dann an die Anwendung SaaS (Schritt 4) gesendet prüft das Token der Signatur und deren Inhalt (Schritt 5) verwendet. Und befindet sich im vorherigen Szenario der Anwendung die Graph-API verwenden kann, um weitere Informationen zu diesem Benutzer wenn SaaS erforderlichen (Schritt 6).

Heute, nicht Azure AD vollständiger Ersatz für lokale Windows Server Active Directory. Wie bereits erwähnt Cloud Verzeichnis über ein viel einfacheres Schema verfügt, und es fehlt auch Elemente, wie z. B. Gruppenrichtlinien, die Möglichkeit zum Speichern von Informationen über Autos und Unterstützung für LDAP. (Tatsächlich, ein Windows-Computer kann nicht konfiguriert werden, damit die Benutzer mit lediglich die Azure AD anmelden können: dies nicht unterstütztes Szenario.) Sie stattdessen die ursprüngliche Ziele der Azure AD einschließen ganz Enterprise Benutzer Access Applications in der Cloud können, ohne eine separate Anmeldung des und Freigeben von lokalen Verzeichnisadministratoren von manuell synchronisieren ihrer lokalen Verzeichnis mit jeder SaaS-Anwendung, die ihre Organisation verwendet. Im Laufe der Zeit erwarten Sie jedoch dieses Cloud-Verzeichnisdienst, um einen größeren Bereich von Szenarien Adresse ein.

## <a name="a-nameacausing-azure-active-directory-access-control"></a><a name="ac"></a>Verwenden von Azure-Active Directory Access Control

Identität cloudbasierte Technologies können verwendet werden, zu eine Vielzahl von Problemen zu lösen. Azure-Active Directory können beispielsweise eine Organisation Benutzer einmaliges Anmelden auf mehrere SaaS Applications, gewähren. Aber Identität Technologien in der Cloud können auch auf andere Weise verwendet werden.

Nehmen Sie an, z. B., dass die Anwendung ihren Benutzern lassen möchte melden Sie sich mit Token ausgestellt von mehreren *Identitätsanbieter (IdPs)*. Zahlreiche verschiedene Identitätsanbieter vorhanden heute, Facebook, Google, Microsoft und andere Personen, einschließlich, und Applikationen weisen häufig Benutzer melden Sie sich mit einer der folgenden Identitäten. Warum sollte eine Anwendung überhaupt eine eigene Liste mit Benutzer und Kennwörter verwalten, wenn sie stattdessen auf Identitäten verlassen kann, die bereits vorhanden sind? Akzeptieren die vorhandene Identitäten macht Ihnen das Leben einfacher beide-Benutzer, die mindestens ein weniger Benutzernamen und Ihr Kennwort vergessen Sie nicht, sowie Personen, die die Anwendung zu erstellen, die ihre eigenen Listen mit Benutzernamen und Kennwörter verwalten nicht mehr benötigen.

Aber während jeder Identitätsanbieter einige Arten von Token Probleme, diese Token werden nicht standard - jede IdP ein eigenen Format hat. Darüber hinaus nicht die Informationen in diesen Token auch standard. Mit der Herausforderung, Schreiben von Code eindeutige alle drei verschiedene Formate verarbeitet wird eine Anwendung, die möchte Token ausgestellt von, sagen, Facebook, Google und Microsoft akzeptieren konfrontiert.

Aber warum sollten Sie dies tun? Warum nicht erstellen stattdessen Vermittler, der ein einzelnes Tokens Format mit eine gemeinsame Darstellung der Identitätsinformationen generieren können? Dieser Ansatz würde machen Leben einfacher für die Entwickler, die Applikationen, erstellen, da jetzt helfen kann nur eine müssen Art von token. Azure Active Directory Access Control geschieht genau, beim der Cloud an-und für die Arbeit mit unterschiedlichen Token bereitstellen. [Abbildung 4](#fig4) zeigt, wie es funktioniert

![Azure-Active Directory auf virtuellen Computern](./media/identity/identity_04_IdentityProviders.png)
<a id="fig4"></a>Abbildung 4: Azure Active Directory Access Control vereinfacht das für Applikationen Identität Token ausgestellt von anderen Identitätsanbieter annehmen.

Der Vorgang beginnt, wenn ein Benutzer versucht, auf die Anwendung über einen Browser zugreifen. Die Anwendung leitet ihr, eine IdP seiner Wahl (und der Anwendung auch vertraut). Anna authentifiziert sich diese IdP, z. B. durch Eingabe von Benutzername und Kennwort (Schritt 1), und die IdP gibt ein Token mit Informationen über ihre (Schritt 2).

Wie in die Abbildung dargestellt, unterstützt Access Control einen Bereich von anderen cloudbasierten IdPs, einschließlich von Gmail, Yahoo, Facebook, Microsoft (vormals als Windows Live ID bezeichnet) und alle OpenID-Anbieter erstellte Konten aus. Es unterstützt auch Identitäten mit Azure Active Directory erstellt und Verbunds mit AD FS, Windows Server Active Directory. Ziel ist es, die am häufigsten verwendeten Identitäten heute, Deckblatt, ob sie von IdPs in der Cloud oder lokalen ausgestellt sind.

Sobald der Browser des Benutzers ein IdP Token aus dem ausgewählten IdP verfügt, sendet es dieses token zu Access-Steuerelement (Schritt 3). Access Control überprüft das Token, und stellen Sie sicher, dass sie wirklich von diesem IdP ausgestellt wurde, und klicken Sie dann erstellt ein neues Token nach den Regeln, die für diese Anwendung definiert wurden. Wie Azure Active Directory Access Control ist ein mit mehreren Mandanten-Dienst, die Mandanten sind jedoch Clientanwendungen anstatt Kunden Organisationen. Jede Anwendung bekomme eigene Namespace, wie in der Abbildung dargestellt, und Sie können verschiedene Regeln zur Autorisierung und weitere definieren.

Diese Regeln lassen Sie jede der Anwendung Administrator definieren, wie Token aus verschiedenen IdPs in einer Access-Steuerelements Token umgewandelt werden soll. Beispielsweise wenn IdPs verschiedene Arten für die Darstellung von Benutzernamen verwenden möchten, können Access Control Regeln aller Werte in ein anderes allgemeine Username transformieren. Access Control sendet dieses neue Token dann wieder an den Browser (Schritt 4), der sie mit der Anwendung (Schritt 5) sendet. Nachdem sie das Steuerung des Benutzerzugriffs Token aufweist, überprüft die Anwendung an, dass dieses Token wirklich von Access Control ausgestellt wurde, und klicken Sie dann mit der eingegebenen Informationen enthaltenen (Schritt 6).

Während dieser Vorgang etwas kompliziert erscheinen möglicherweise, erleichtert tatsächlich Nutzungsdauer erheblich für den Ersteller der Anwendung. Statt behandeln verschiedene Token, die andere Informationen enthält, kann die Anwendung Identitäten ausgestellt von mehrere Identitätsanbieter während immer noch empfängt nur ein einzelnes Token vertraute Angaben akzeptieren. Auch, anstatt jede Anwendung konfiguriert sein, um verschiedene IdPs als vertrauenswürdig einzustufen erforderlich ist, wird diese Trust Beziehungen stattdessen von Access Control verwaltet werden - Anwendung muss nur vertrauen.

Lohnt sich darauf hin, dass nichts über Access Control Windows verknüpft ist – es konnte genauso gut von einer Linux-Anwendung, die nur Google und Facebook Identitäten akzeptiert verwendet werden. Und obwohl Access Control der Azure-Active Directory-Familie gehört, Sie können sich vorstellen, als vollständig distinct-Dienst aus, wie im vorherigen Abschnitt beschrieben wurde. Während beide Technologien Identität konzipiert, beheben sie Probleme, etwas ganz anderes (zwar Microsoft unter dem Gesichtspunkt sind weist, dass er erwartet die beiden zu einem bestimmten Zeitpunkt integriert werden soll).

Arbeiten mit Identität ist wichtig in nahezu jeder Anwendung. Das Ziel des Access-Steuerelements ist, damit es einfacher für Entwickler Applications erstellen, die Identitäten aus unterschiedlichen Identitätsanbieter akzeptieren. Durch diesen Dienst in der Cloud einfügen, hat Microsoft es an eine beliebige Anwendung, die auf einer beliebigen Plattform ausgeführt zur Verfügung.

##<a name="about-the-author"></a>Informationen zum Autor

David Chappell ist Hauptbenutzer der Chappell & Associates [www.davidchappell.com](http://www.davidchappell.com) in San Francisco, Kalifornien.
