<properties
    pageTitle="Einheit zurücksetzen ein Ball Lernprogramm"
    description="Schritte zum klassischen eins einsatzbereit ein Ball Spiel beträchtlich erhöhen, eine Vorbedingung für alle Mobile Engagement eins-Lernprogramme ist"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a name="a-idunity-roll-a-ballacreate-unity-roll-a-ball-game"></a><a id="unity-roll-a-ball"></a>Erstellen Sie ein Ball Spiel eins einsatzbereit

Dieses Lernprogramm führt über die wichtigsten Schritte für eine leicht abgewandelte [Einheit im Rollupzeilen ein Ball Lernprogramm](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial). Dieses Beispiel Spiel besteht aus einem kugelförmigen '' Playerobjekt die vom Benutzer app gesteuert wird und die Zielsetzung Spielende 'entladbaren Objekte sammeln', indem Sie das Playerobjekt mit dieser Objekte entladbaren Kollision ist. Dies setzt voraus, grundlegende Kenntnisse von eins-Editor-Umgebung. Wenn Sie Probleme auftreten, sollten Sie zum vollständigen Lernprogramm verweisen. 

### <a name="setting-up-the-game"></a>Das Spiel einrichten
Die nachstehenden Schritte sind die [Einheit im Lernprogramm](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. Öffnen Sie **Eins-Editor** , und klicken Sie auf **neu**. 
    
    ![][51] 
    
2. Geben Sie einen **Projektnamen** & **Speicherort**, **3D** wählen Sie aus, und klicken Sie auf **Projekt erstellen**.
    
    ![][52]

3. Speichern die standardmäßigen Szene soeben erstellte Bestandteil des neuen Projekts als mit den Namen **Minispiel** innerhalb eines neuen ** \_Szenen** Ordner unter **Posten** Ordner:
    
    ![][53]

4. Erstellen Sie einer- **3D-Objekt -> Ebene** wie das Feld Bereich, und benennen Sie dieses Objekt Ebene als **Grund**

    ![][1]

5. Zurücksetzen der Transformation-Komponente für dieses Objekt **Grund** , so, dass es am Ursprung befindet. 

    ![][3]

6. Deaktivieren Sie **Raster anzeigen** **Zukunftsvisionen** Menü für das Objekt **Grund** ein.

    ![][4]

7. Aktualisieren Sie die Komponente **Maßstab** für das Objekt **Grund** sein [X = 2, Y = 1, Z = 2]. 

    ![][5]

8. Fügen Sie einer neuen- **3D-Objekt-Kugel >** des Projekts hinzu, und benennen Sie dieses Objekt Kugel als **Player**. 

    ![][6]

9. Wählen Sie das Objekt **Player** , und klicken Sie auf das Objekt Ebene ähnlich wie **Transformation zurücksetzen** . 

10. Update **Transformation-Position > -> Y-Koordinate** Komponente für den Player Y 0,5 dargestellt.  

    ![][7]

11. Erstellen Sie einen neuen Ordner namens **Material** im Projekt, in der wir die Informationen für die Medienwiedergabe Farbe erstellen. 

12. Erstellen Sie ein neues **Material** namens **Hintergrund** in diesem Ordner. 

    ![][8]

13. Aktualisieren Sie die Farbe des Materials, indem sie die Eigenschaft **Albedo** aktualisieren. Sie können die RGB-Werte [0,32,64] auswählen. 

    ![][9]

14. Ziehen Sie diese Material in der Szenenansicht zum Anwenden von Farbe auf das Objekt **Grund** ein. 

    ![][10]

17. Aktualisieren Sie schließlich die **Transformation-Drehung > Y ->** zu 60 auf das Objekt gerichtete Lichtquelle aus Gründen der Übersichtlichkeit. 

    ![][12]

### <a name="moving-the-player"></a>Verschieben des Players
Die nachstehenden Schritte sind die [Einheit im Lernprogramm](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. Fügen Sie eine **RigidBody** -Komponente auf das Objekt **Player** aus. 

    ![][13]

2. Erstellen Sie einen neuen Ordner namens **Skripts** im Projekt. 

3. Klicken Sie auf **Komponente hinzufügen -> neues Skript -> C#-Skript**. Nennen Sie es **PlayerController**, und klicken Sie auf **Erstellen und hinzufügen**. Erstellen und fügen Sie ein Skript auf das Objekt Player.  

    ![][14]

5. Wechseln Sie dieses Skript unter dem Ordner **Skripts** im Projekt 

6. Öffnen Sie das Skript zur Bearbeitung in Ihrem bevorzugten Skripteditor, aktualisieren Sie den Skriptcode mit den folgenden Code ein, und speichern sie. 

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
    
8. Beachten Sie, dass das Skript oben eine Eigenschaft **Geschwindigkeit** verwendet. Aktualisieren Sie die Eigenschaft Geschwindigkeit im Editor eins 10.  

    ![][15]

9. Drücken Sie die **Wiedergabe** in der Einheit im Editor. Steuern den Ball mithilfe der Tastatur können nun sollten und sollte drehen und navigieren. 

### <a name="moving-the-camera"></a>Verschieben der Kamera
Die folgenden Schritte aus, sind die [Einheit im Lernprogramm](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) und werden die **Kamera primär** auf das Objekt **Player** einbinden. 

1. Aktualisieren Sie die **Transform.Position** benutzerspezifisch X = 0, Y = 10.5, Z =-10.  
2. Aktualisieren Sie die **Transform.Rotation** benutzerspezifisch X = 45, Y = 0, Z = 0.  

    ![][16]

2. Fügen Sie ein neues Skript namens **CameraController** , um die **MainCamera** und verschieben Sie es unter dem Ordner Skripts. 

    ![][17]

3. Öffnen Sie das Skript zum Bearbeiten, und fügen Sie den folgenden Code darin:

        using UnityEngine;
        using System.Collections;
        
        public class CameraController : MonoBehaviour {
        
            public GameObject player;
        
            private Vector3 offset;
        
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
            
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
    
5. Ziehen Sie in Einheit Umgebung - die Player-Variable in den Slot Player für das Objekt primär Kamera, damit die beiden miteinander verbunden sind. 

    ![][18]

6. Jetzt, wenn Sie Wiedergabe in der Einheit im Editor erreicht haben und das Player Ball Objekt drehen wird dann die Kamera in der Bewegung folgen angezeigt.  

### <a name="setting-up-the-play-area"></a>Einrichten des Bereichs wiedergeben
Die [Einheit im Lernprogramm](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141)sind die folgenden Schritte aus. Wir werden die Wände umgebenden Grund, damit das Objekt Ball Player aus dem Wiedergabebereich in der Bewegung Ablegen nicht erstellen. 

1. Klicken Sie auf **Erstellen -> erstellen leeren-Spiel Objekt >** und nennen Sie es **Wände**

    ![][19]

2. Klicken Sie unter dieses Objekt Wände - erstellen Sie einer neuen- **3D-Objekt-Cube >** , und nennen Sie es mit "Westen Wand". 

    ![][20]

3. Aktualisieren Sie die **Transformation -> Position** und **Transformieren-Farben-Skala >** für dieses Objekt "Westen" Wand. 

    ![][21]

4. Duplizieren Sie die Wand "Westen" zum Erstellen einer **Ost Wand** mit aktualisierten Transformation positionieren und skalieren. 

    ![][22]

5. Doppelte Osten Wand um eine **North Wand** mit den aktualisierten Transformation Position & Skala zu erstellen. 

    ![][23]

6. Doppelte North Wand, und erstellen Sie eine **Wand Süd** mit den aktualisierten Transformation Position & skalieren. 

    ![][24]

### <a name="creating-collectible-objects"></a>Erstellen von entladbaren Objekten
Die [Einheit im Lernprogramm](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141)sind die folgenden Schritte aus. Wir werden einige attraktive aussehenden Objekte erstellen, die entladbaren Objektgruppe, die das Objekt Player Ball bilden sollen'sammeln', indem Sie mit ihnen Kollision muss. 

1. Erstellen Sie eines neuen **3D Cubeobjekt** , und nennen Sie es Abholung. 

2. Anpassen der- **Transformation-Drehung >** & des Objekts Pickup-**Transformation-Farben-Skala >** . 

    ![][25]

3. Erstellen Sie, und fügen Sie ein **Neues C#-Skript** **Rotator** auf das Objekt Pickup bezeichnet. Vergewissern Sie sich an das Skript unter dem Ordner Skripts zu setzen. 

    ![][26]

4. Öffnen Sie dieses Skript zur Bearbeitung zu, und aktualisieren sie wie folgt aussehen: 

        using UnityEngine;
        using System.Collections;
        
        public class Rotator : MonoBehaviour {
        
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }

5. Drücken Sie jetzt werden der Wiedergabemodus in der Einheit im Editor und Ihre Pickup Objekt anzeigen drehen seine Achse.

6. Erstellen Sie einen neuen Ordner namens **Prefabs** 

    ![][27]

7. Ziehen Sie das Objekt **Abholung** , und legen Sie es in den Ordner Prefabs ab.

    ![][28]

8. Erstellen eines neuen **leeren Spiel Objekt** **Abholung**bezeichnet. Zurücksetzen Sie seine Position auf Origin, und ziehen Sie dann das Pickup Objekt unter diesem Spiel Objekt.  

    ![][29]

9. Das Objekt **Abholung** duplizieren und ihn auf das Objekt **Grund** , um das Objekt **Player** aktualisieren Sie die Werte **des Transform.Position X und Z** ordnungsgemäß verteilt sind. 

    ![][30]

10. Erstellen Sie ein **Neues Material** **Abholung** aufgerufen und aktualisieren Sie, um werden rot in Farbe, durch die **Albedo Eigenschaft** ähnlich wie wir zum Aktualisieren des Objekts Grund konnten aktualisieren. 

    ![][31]

11. Wenden Sie das Material auf alle 4 pickup Objekte an.

    ![][32]

### <a name="collecting-the-pickup-objects"></a>Sammeln der Pickup Objekte
Die [Einheit im Lernprogramm](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141)sind die folgenden Schritte aus. Wir werden im Player aktualisieren, damit 'pickup Objekte sammeln', indem Sie mit ihnen Kollision kann. 

1. Öffnen Sie auf das Objekt Player zur Bearbeitung verbundene **PlayerController** Skript, und aktualisieren Sie ihn wie folgt:  

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour {
        
            public float speed;
        
            private Rigidbody rb;
        
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
        
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
        
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
        
                rb.AddForce (movement * speed);
            }
        
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }

2. Erstellen Sie eine neue **Kategorie** , aufgerufen **Wählen Sie oben** (es muss im Skript Neuigkeiten übereinstimmen)  

    ![][33]
    
    ![][34]

3. Diese **Kategorie** auf das Objekt Prefab Abholung anwenden. 

    ![][35]

4. Aktivieren Sie **IsTrigger** Kontrollkästchen für das Objekt Prefab.

    ![][36]

5. Hinzufügen einer starre Textkörper Abholung Prefab Objekt. Optimierung der Leistung aktualisieren wir die statische Collider, die wir verwendet, um eine dynamische Collider. 

    ![][37]
  
6. Aktivieren Sie schließlich die **IsKinematic** -Eigenschaft für das prefab Objekt ein. 

    ![][38]

7. Treffer **Wiedergeben** , in der Einheit im Editor, und Sie werden können für dieses **einsatzbereit einen Ball** Spiel durch Verschieben des Player-Objekts, das mit den Tasten der Tastatur Eingaben Richtung. 

### <a name="updating-the-game-for-mobile-play"></a>Aktualisieren das Spiel für mobile wiedergeben
In den Abschnitten oben geschlossenen das grundlegende Lernprogramm aus Einheit. Wir werden nun das Spiel, Mobilgerät geeignet wird ändern. Beachten Sie, dass wir Tastatureingaben für das Spiel bisher Testzwecken verwendet. Jetzt werden wir ändern, damit wir den Player steuern können, d. h. mithilfe der Bewegung des Telefons mit Accelerometer als Eingabe. 

Öffnen Sie das Skript **PlayerController** zur Bearbeitung, und aktualisieren Sie die **FixedUpdate** Methode, um die Bewegung aus der Beschleunigungsmesser verwenden, um das Objekt Player verschieben. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

In diesem Lernprogramm wird die Erstellung ein einfaches Spiels mit eins abgeschlossen und können Sie diese auf einem Gerät Ihrer Wahl zum Spielen bereitstellen. 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png  
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png  
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png

    
    
    
    
    
    
    
    
    
    
    
    
