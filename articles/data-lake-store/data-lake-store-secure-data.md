<properties 
   pageTitle="Sichern von Daten aus Azure Lake Datenspeicher | Microsoft Azure" 
   description="Erfahren Sie, wie Daten in Azure Lake Datenspeicher Entwurfsphase gesichert und zugreifen Steuerelement Listen" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/29/2016"
   ms.author="nitinme"/>

# <a name="securing-data-stored-in-azure-data-lake-store"></a>Sichern von Daten aus Azure Lake Datenspeicher

Schützen von Daten in Azure Lake Datenspeicher ist ein Ansatz drei Schritten.

1. Erstes erstellen Sicherheitsgruppen in Azure Active Directory (AAD). Diese Sicherheitsgruppen dienen zum rollenbasierte Access-Steuerelements (RBAC) Azure-Portal zu implementieren. Weitere Informationen finden Sie unter [Rollenbasierte Access Control in Microsoft Azure](../active-directory/role-based-access-control-configure.md).

2. Zuweisen der AAD Sicherheitsgruppen mit dem Konto Azure Lake Datenspeicher an. Diese Steuerelemente greifen mit dem Konto Lake Datenspeicher aus den Vorgängen Portal und Verwaltung aus dem Portal oder APIs.

3. Zuweisen der AAD Sicherheitsgruppen als Access Control Lists (ACLs) für das Dateisystem Lake Datenspeicher.

4. Darüber hinaus können Sie auch einen IP-Adressenbereich für Clients festlegen, die die Daten in Lake Datenspeicher zugreifen können.

Dieser Artikel enthält Anweisungen zur Verwendung das Azure-Portal für die oben genannten Aufgaben. Ausführliche Informationen zu Lake Datenspeicher wie implementiert Sicherheit auf der Ebene-Konto und Daten finden Sie unter [Sicherheit in Azure dem Datenspeicher](data-lake-store-security-overview.md). Tief greifende Informationen wie ACLs in Azure Lake Datenspeicher implementiert werden finden Sie unter [Übersicht der Access Control in dem Datenspeicher](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).
- **Ein Azure Datenspeicher dem Konto**. Informationen dazu, wie Sie einen erstellen finden Sie unter [Erste Schritte mit Azure dem Datenspeicher](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Erstellen von Sicherheitsgruppen in Azure Active Directory

Anweisungen zum AAD Sicherheitsgruppen erstellen und Hinzufügen von Benutzern zur Gruppe finden Sie unter [Verwalten von Sicherheitsgruppen in Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Zuweisen von Benutzern oder Sicherheitsgruppen zu Azure Lake Datenspeicher-Konten

Wenn Sie Benutzern oder Sicherheitsgruppen Azure Lake Datenspeicher Konten zuweisen, können Sie Zugriff auf die Verwaltungsvorgänge auf Konto mithilfe der Azure-Portal und Azure Ressourcenmanager APIs. 

1. Öffnen Sie ein Konto Azure Lake Datenspeicher. Im linken Bereich klicken Sie auf **Durchsuchen**, klicken Sie auf **Dem Datenspeicher**, und klicken Sie dann aus dem Blade Lake Datenspeicher auf den Namen des Kontos, den Sie eine Benutzer oder eine Sicherheitsgruppe Gruppe zuweisen möchten.

2. Klicken Sie in Ihrem Konto Blade Lake Datenspeicher auf **Einstellungen**. Klicken Sie auf **Benutzer**, aus dem Blade **Einstellungen** .

    ![Zuweisen von Sicherheitsgruppe mit dem-Datenspeicher Azure-Konto] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Zuweisen von Sicherheitsgruppe mit dem-Datenspeicher Azure-Konto")

3. Das **Benutzer** Blade standardmäßig **Abonnement Administratoren** Gruppe Besitzer aufgeführt. 

    ![Hinzufügen von Benutzern und Rollen] (./media/data-lake-store-secure-data/adl.add.group.roles.png "Hinzufügen von Benutzern und Rollen")
 
    Es gibt zwei Methoden zum Hinzufügen einer Gruppe und relevante Rollen zuweisen.

    * Hinzufügen ein Benutzers/einer Gruppe mit dem Konto und zuweisen eine Rolle, oder
    * Hinzufügen einer Rolle aus, und weisen Sie Benutzer und Gruppen/Rolle.

    In diesem Abschnitt betrachten wir der erste Ansatz ist eine Gruppe hinzufügen und dann den Rollen zuweisen. Sie können ähnliche Schritte aus, um Wählen Sie zuerst eine Rolle aus, und klicken Sie dann Gruppen, die diese Rolle zuweisen ausführen.
    
4. Klicken Sie auf **Hinzufügen** , um das Blade **Hinzufügen Access** öffnen, in dem **Benutzer** Blade. Klicken Sie in das Blade **Access hinzufügen** auf, **Wählen Sie eine Rolle aus**, und wählen Sie dann eine Rolle für die Benutzergruppe.

     ![Hinzufügen einer Rolle für den Benutzer] (./media/data-lake-store-secure-data/adl.add.user.1.png "Hinzufügen einer Rolle für den Benutzer")

    Der **Besitzer** und der **Teilnehmerrolle** bieten Zugriff auf eine Vielzahl von Verwaltungsfunktionen für die Daten Lake Konto ein. Für Benutzer, die mit Daten in den Daten Lake interagieren werden, können Sie diese **Rolle **hinzufügen. Der Umfang der ausführenden ist auf Management-Vorgänge, die im Zusammenhang mit dem Konto Azure Lake Datenspeicher beschränkt.

    Für Daten definieren Operationen einzelne Berechtigungen des Dateisystems Möglichkeiten, die Benutzer haben. Daher ein Benutzer eine Leserrolle Probleme kann nur anzeigen administrative Einstellungen, die dem Konto zugeordnet, aber können potenziell lesen und schreiben Daten auf Grundlage der ihnen zugewiesenen Berechtigungen des Dateisystems. Berechtigungen des Dateisystems Lake Datenspeicher werden am [Sicherheitsgruppe als ACLs im Dateisystem Azure dem Datenspeicher zuweisen](#filepermissions)beschrieben.



5. Klicken Sie in das Blade **Hinzufügen Zugriff** auf **Benutzer hinzufügen** , um das Blade **Benutzer hinzufügen** zu öffnen. Suchen Sie in diesem Blade nach der Sicherheitsgruppe, die Sie zuvor in Azure Active Directory erstellt haben. Wenn Sie viele Gruppen für die Suche verfügen, verwenden Sie im Textfeld oben zum Filtern auf den Gruppennamen. Klicken Sie auf **auswählen**.

    ![Hinzufügen einer Sicherheitsgruppe] (./media/data-lake-store-secure-data/adl.add.user.2.png "Hinzufügen einer Sicherheitsgruppe")

    Wenn Sie möchten einen Gruppe/Benutzer hinzufügen, der nicht aufgelistet ist, können Sie sie einladen, indem Sie das Symbol **einladen** und die e-Mail-Adresse für die Benutzergruppe.

6. Klicken Sie auf **OK**. Es sollte die Sicherheitsgruppe hinzugefügt, wie unten dargestellt angezeigt.

    ![Sicherheitsgruppe hinzugefügt] (./media/data-lake-store-secure-data/adl.add.user.3.png "Sicherheitsgruppe hinzugefügt")

7. Ihre Benutzer/Sicherheitsgruppe verfügt jetzt über Zugriff auf das Konto Azure Lake Datenspeicher. Wenn Sie Zugriff auf bestimmte Benutzer bereitstellen möchten, können Sie diese zur Sicherheitsgruppe hinzufügen. Wenn Sie den Zugriff für einen Benutzer widerrufen möchten, können Sie diese auf ähnliche Weise aus der Sicherheitsgruppe entfernen. Sie können auch mehrere Sicherheitsgruppen mit einer Firma zuweisen. 

## <a name="a-namefilepermissionsaassign-users-or-security-group-as-acls-to-the-azure-data-lake-store-file-system"></a><a name="filepermissions"></a>Zuweisen von Benutzern oder Sicherheitsgruppe als ACLs im Dateisystem Azure Lake Datenspeicher

Das Dateisystem Azure Daten Lake Benutzer/Sicherheitsgruppen zuweisen, legen Sie die Access-Steuerelement auf der Registerkarte Daten in Azure Lake Datenspeicher gespeichert.

1. Klicken Sie in Ihrem Konto Blade Lake Datenspeicher auf **Daten-Explorer**.

    ![Verzeichnisse erstellen, in dem Datenspeicher-Konto] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Verzeichnisse erstellen, in dem Daten-Konto")

2. Klicken Sie in das Blade **Daten-Explorer** klicken Sie auf die Datei oder den Ordner, für die Sie die ACL konfigurieren möchten, und klicken Sie dann auf **Access**. Um eine Datei ACL zuweisen möchten, müssen Sie **Access** aus dem Blade **Dateivorschau** klicken.

    ![Festlegen von ACLs auf dem Daten Dateisystem] (./media/data-lake-store-secure-data/adl.acl.1.png "Festlegen von ACLs auf dem Daten Dateisystem")

3. Das **Access** -Blade Listet die Zugriff auf standard und die bereits zugewiesen werden im Stammordner benutzerspezifische. Klicken Sie auf das Symbol **Hinzufügen** , um Stufe ACLs hinzuzufügen.

    ![Standard- und benutzerdefinierte Liste-Zugriff] (./media/data-lake-store-secure-data/adl.acl.2.png "Standard- und benutzerdefinierte Liste-Zugriff")

    * **Standard Zugriff auf** die Access UNIX-Format ist, wenn Sie weitere angeben, schreiben, ausführen (Rwx) in drei unterschiedliche Benutzerklassen: Eigentümer, Gruppen und andere.
    * **Benutzerdefinierte Access** entspricht der POSIX ACLs, die Sie zum Festlegen von Berechtigungen für bestimmte Benutzer oder Gruppen, und nicht nur der gewünschten Datei Besitzer oder Gruppe ermöglicht. 
    
    Weitere Informationen finden Sie unter [HDFS ACLs](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Weitere Informationen wie ACLs in Lake Datenspeicher implementiert werden finden Sie unter [Control Access, in dem Datenspeicher](data-lake-store-access-control.md).

4. Klicken Sie auf das Symbol **Hinzufügen** , um das Blade **Benutzerdefinierte Access hinzufügen** zu öffnen. Klicken Sie in diesem Blade auf **Benutzer oder Gruppe auswählen**, und suchen Sie auf **Benutzer oder Gruppe auswählen** Blade für die Sicherheitsgruppe, die Sie zuvor in Azure Active Directory erstellt haben. Wenn Sie viele Gruppen für die Suche verfügen, verwenden Sie im Textfeld oben zum Filtern auf den Gruppennamen. Klicken Sie auf die Gruppe, die Sie hinzufügen, und klicken Sie auf **auswählen**möchten.

    ![Hinzufügen einer Gruppe] (./media/data-lake-store-secure-data/adl.acl.3.png "Hinzufügen einer Gruppe")

5. Klicken Sie auf **Berechtigungen auswählen**, wählen Sie die Berechtigungen und ACL oder beide zugreifen, ob Sie die Berechtigungen als Standard-ACL zuweisen möchten, auf. Klicken Sie auf **OK**.

    ![Zuweisen von Berechtigungen zu einer Gruppe] (./media/data-lake-store-secure-data/adl.acl.4.png "Zuweisen von Berechtigungen zu einer Gruppe")

    Weitere Informationen zu Berechtigungen in Lake Datenspeicher und Standard/Access Zugriffssteuerungslisten finden Sie unter [Access Control in dem Datenspeicher](data-lake-store-access-control.md).


6. Das Blade **Benutzerdefinierte Access hinzufügen** klicken Sie auf **OK**. Durch die zugeordneten Berechtigungen, die neu hinzugefügte Gruppe wird jetzt in der **Access** -Blade aufgeführt.

    ![Zuweisen von Berechtigungen zu einer Gruppe] (./media/data-lake-store-secure-data/adl.acl.5.png "Zuweisen von Berechtigungen zu einer Gruppe")

    > [AZURE.IMPORTANT] In der aktuellen Version können Sie nur 9 Einträge unter **Benutzerdefinierte Access**haben. Wenn Sie mehr als 9 Benutzer hinzufügen möchten, sollten Sie Sicherheitsgruppen, erstellen Hinzufügen von Benutzern zu Sicherheitsgruppen, ermöglichen den Zugriff auf diese Sicherheitsgruppen für das Konto Lake Datenspeicher.

7. Falls erforderlich, können Sie auch die Zugriffsberechtigungen ändern, nachdem Sie die Gruppe hinzugefügt haben. Deaktivieren Sie oder aktivieren Sie das Kontrollkästchen für jeden Berechtigungstyp (Lesen, schreiben, ausführen) basierend auf sollen entfernen oder die Berechtigung zur Sicherheitsgruppe zuweisen. Klicken Sie auf **Speichern** , um die Änderungen zu speichern oder **verwerfen** , um die Änderungen rückgängig zu machen.

## <a name="set-ip-address-range-for-data-access"></a>Legen Sie IP-Adressbereich für den Zugriff auf Daten

Azure Lake Datenspeicher ermöglicht es Ihnen zu weiteren Zugriff auf Ihre Datenspeicher Netzwerk Ebene Position fixieren. Sie können Firewall aktivieren, geben Sie eine IP-Adresse oder einen IP-Adressenbereich für vertrauenswürdigen Kunden definieren. Nachdem die aktiviert ist, können nur Clients, die die IP-Adressen definierten Bereichs enthalten zum Store verbinden.

![Firewall-Einstellungen und IP-Zugriff] (./media/data-lake-store-secure-data/firewall-ip-access.png "Firewall-Einstellungen und IP-Adresse")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Entfernen Sie Sicherheitsgruppen für ein Konto Azure Lake Datenspeicher

Wenn Sie Sicherheitsgruppen aus Azure Lake Datenspeicher Konten entfernen, werden nur den Zugriff für Management Vorgänge auf Konto mithilfe der Azure-Portal und Azure Ressourcenmanager APIs ändern.

1. Klicken Sie in Ihrem Konto Blade Lake Datenspeicher auf **Einstellungen**. Klicken Sie auf **Benutzer**, aus dem Blade **Einstellungen** .

    ![Zuweisen der Sicherheitsgruppe Azure Daten dem Konto] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Zuweisen der Sicherheitsgruppe Azure Daten dem Konto")

2. Klicken Sie auf die Sicherheitsgruppe, die Sie entfernen möchten, in dem **Benutzer** Blade.

    ![Entfernen der Sicherheitsgruppe] (./media/data-lake-store-secure-data/adl.add.user.3.png "Entfernen der Sicherheitsgruppe")

3. Klicken Sie in das Blade für die Sicherheitsgruppe klicken Sie auf **Entfernen**.

    ![Sicherheitsgruppe entfernt] (./media/data-lake-store-secure-data/adl.remove.group.png "Sicherheitsgruppe entfernt")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Entfernen der Sicherheitsgruppe ACLs aus Azure Lake Datenspeicher Dateisystem

Wenn Sie Sicherheitsgruppen ACLs aus Azure Lake Datenspeicher Dateisystem entfernen möchten, ändern Sie Access mit den Daten im Datenspeicher Lake.

1. Klicken Sie in Ihrem Konto Blade Lake Datenspeicher auf **Daten-Explorer**.

    ![Verzeichnisse erstellen, in dem Daten-Konto] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Verzeichnisse erstellen, in dem Daten-Konto")

2. Klicken Sie auf die Datei oder den Ordner, für die Sie die ACL entfernen möchten, und klicken Sie dann in Ihrem Konto Blade auf das **Access** -Symbol, in das Blade **Daten-Explorer** . Um ACL für eine Datei zu entfernen, müssen Sie **Access** aus dem Blade **Dateivorschau** klicken.

    ![Festlegen von ACLs auf dem Daten Dateisystem] (./media/data-lake-store-secure-data/adl.acl.1.png "Festlegen von ACLs auf dem Daten Dateisystem")

3. Klicken Sie in der **Access** -Blade, klicken Sie im Abschnitt **Benutzerdefinierte Access** auf die Sicherheitsgruppe, die Sie entfernen möchten. Klicken Sie in das **Benutzerdefinierte Access** -Blade auf **Entfernen** , und klicken Sie dann auf **OK**.

    ![Zuweisen von Berechtigungen zu einer Gruppe] (./media/data-lake-store-secure-data/adl.remove.acl.png "Zuweisen von Berechtigungen zu einer Gruppe")


## <a name="see-also"></a>Siehe auch

- [Übersicht über Azure Lake Datenspeicher](data-lake-store-overview.md)
- [Kopieren von Daten aus Azure-Speicher Blobs in Lake Datenspeicher](data-lake-store-copy-data-azure-storage-blob.md)
- [Verwenden von Azure Daten Lake Analytics mit Lake Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight mit Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Erste Schritte mit Lake Datenspeicher mithilfe der PowerShell](data-lake-store-get-started-powershell.md)
- [Erste Schritte mit Lake Datenspeicher mit .NET SDK](data-lake-store-get-started-net-sdk.md)
- [Access-Diagnoseprotokolle für Lake Datenspeicher](data-lake-store-diagnostic-logs.md)
