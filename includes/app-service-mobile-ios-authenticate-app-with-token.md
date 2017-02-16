
Im vorherige Beispiel Kontakte sowohl der Identitätsanbieter und den mobilen Dienst jedes Mal, wenn die app gestartet wird. Stattdessen können Sie das Autorisierungstoken Zwischenspeichern und versuchen Sie es zuerst verwenden.

* Es wird empfohlen zu verschlüsseln und Speichern von Authentifizierungstoken auf einem iOS-Client der iOS Schlüsselbund zu verwenden. Wir verwenden [SSKeychain](https://github.com/soffes/sskeychain) – einen einfachen Wrapper für den iOS Schlüsselbund. Folgen Sie den Anweisungen auf der Seite SSKeychain und zum Projekt hinzugefügt werden. Stellen Sie sicher, dass die Einstellung **Module aktivieren** aktiviert ist, in den des Projekts **Erstellen Einstellungen** (Abschnitt **Apple LLVM - Sprachen - Module**).

* Öffnen Sie **QSTodoListViewController.m** , und fügen Sie den folgenden Code hinzu:

```
        - (void) saveAuthInfo {
                [SSKeychain setPassword:self.todoService.client.currentUser.mobileServiceAuthenticationToken forService:@"AzureMobileServiceTutorial" account:self.todoService.client.currentUser.userId]
        }


        - (void)loadAuthInfo {
                NSString *userid = [[SSKeychain accountsForService:@"AzureMobileServiceTutorial"][0] valueForKey:@"acct"];
            if (userid) {
                NSLog(@"userid: %@", userid);
                self.todoService.client.currentUser = [[MSUser alloc] initWithUserId:userid];
                 self.todoService.client.currentUser.mobileServiceAuthenticationToken = [SSKeychain passwordForService:@"AzureMobileServiceTutorial" account:userid];

            }
        }
```

* In `loginAndGetData`, ändern `loginWithProvider:controller:animated:completion:`der Fertigstellung blockieren. Fügen Sie die folgende Zeile unmittelbar vor dem `[self refresh]` zum Speichern der Benutzer-ID und das token Eigenschaften:

```
                [self saveAuthInfo];
```

* Laden Sie uns den Benutzer-ID und das Token auf, wenn die app gestartet wird. In der `viewDidLoad` in **QSTodoListViewController.m**, fügen Sie dieses Recht nach`self.todoService` wird.

```
                [self loadAuthInfo];
```
