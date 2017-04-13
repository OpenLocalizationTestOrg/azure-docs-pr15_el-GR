<properties
    pageTitle="Άλμπουμ ενότητας ένα πρόγραμμα εκμάθησης μπάλα"
    description="Βήματα για να δημιουργήσετε το κλασικό λήψη ενότητας ένα παιχνίδι μπάλα που είναι προαπαιτούμενα για όλα τα προγράμματα εκμάθησης Mobile δέσμευση ενότητας"
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

#<a id="unity-roll-a-ball"></a>Δημιουργία ενότητας λήψη ένα παιχνίδι μπάλα

Αυτό το πρόγραμμα εκμάθησης παρουσιάζει τα κύρια βήματα έχουν τροποποιηθεί ελαφρώς [ενότητας άλμπουμ ένα πρόγραμμα εκμάθησης μπάλα](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial). Αυτό το δείγμα παιχνίδι αποτελείται από ένα αντικείμενο σφαιρική 'πρόγραμμα αναπαραγωγής' το οποίο ελέγχεται από το χρήστη app και το στόχο της το παιχνίδι είναι η 'συλλογή' collectible αντικείμενα με το πρόγραμμα αναπαραγωγής αντικείμενο με αυτά τα αντικείμενα collectible πρόσκρουσης. Θεωρείται βασικές εξοικείωση με το περιβάλλον επεξεργασίας ενότητας. Εάν αντιμετωπίσετε προβλήματα, στη συνέχεια, θα πρέπει να ανατρέξετε το πλήρες πρόγραμμα εκμάθησης. 

### <a name="setting-up-the-game"></a>Ρύθμιση το παιχνίδι
Τα παρακάτω βήματα είναι από το [πρόγραμμα εκμάθησης ενότητας](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. Ανοίξτε **Τον επεξεργαστή ενότητας** και κάντε κλικ στην επιλογή **Δημιουργία**. 
    
    ![][51] 
    
2. Δώστε ένα **όνομα έργου** & **θέση**, επιλέξτε **3Δ** και κάντε κλικ στην επιλογή **Δημιουργία έργου**.
    
    ![][52]

3. Αποθηκεύστε τη σκηνή προεπιλεγμένη δημιουργήσατε ως μέρος του νέου έργου όπως με το όνομα **MiniGame** μέσα σε ένα νέο ** \_σκηνών** φακέλου στην περιοχή **στοιχεία** φακέλου:
    
    ![][53]

4. Δημιουργία ενός **αντικειμένου 3D -> επίπεδο** ως πεδίο αναπαραγωγή και μετονομάστε αυτό το επίπεδο αντικείμενο ως **αρχής**

    ![][1]

5. Επαναφορά στοιχείου μετασχηματισμό για αυτό το αντικείμενο **αρχής** ώστε να είναι με την προέλευση. 

    ![][3]

6. Καταργήστε την επιλογή από το **μενού αφήστε στην** **Εμφάνιση πλέγματος** για το αντικείμενο **αρχής** .

    ![][4]

7. Το στοιχείο **κλίμακα** για το αντικείμενο **αρχής** για να ενημερώσετε [X = 2, Y = 1, Z = 2]. 

    ![][5]

8. Προσθήκη ενός νέου **αντικειμένου 3D -> σφαίρα** στο έργο και μετονομάστε αυτό το αντικείμενο σφαίρα ως **πρόγραμμα αναπαραγωγής**. 

    ![][6]

9. Επιλέξτε το αντικείμενο **προγράμματος αναπαραγωγής** και κάντε κλικ στην επιλογή **Επαναφορά μετασχηματισμού** παρόμοιο με το αντικείμενο επιπέδου. 

10. Ενημέρωση **Μετασχηματισμός -> θέση -> συντεταγμένων Y** στοιχείων για το πρόγραμμα αναπαραγωγής Y με τον αριθμό 0,5.  

    ![][7]

11. Δημιουργήστε ένα νέο φάκελο που ονομάζεται **υλικά** του έργου όπου θα δημιουργήσουμε το υλικό στο χρώμα του προγράμματος αναπαραγωγής. 

12. Δημιουργήστε ένα νέο **υλικό** που ονομάζεται **φόντου** σε αυτόν το φάκελο. 

    ![][8]

13. Ενημερώστε το χρώμα του υλικού, ενημερώνοντας την ιδιότητα **Albedo** του. Μπορείτε να επιλέξετε τις τιμές RGB [0,32,64]. 

    ![][9]

14. Σύρετε το υλικό στην προβολή σκηνή για να εφαρμόσετε χρώμα στο αντικείμενο **αρχής** . 

    ![][10]

17. Τέλος ενημέρωση του **Μετασχηματισμός -> Περιστροφή -> Y** έως 60 του αντικειμένου Οδικός Light για σαφήνεια. 

    ![][12]

### <a name="moving-the-player"></a>Μετακίνηση του προγράμματος αναπαραγωγής
Τα παρακάτω βήματα είναι από το [πρόγραμμα εκμάθησης ενότητας](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. Προσθέστε ένα στοιχείο **RigidBody** στο αντικείμενο του **προγράμματος αναπαραγωγής** . 

    ![][13]

2. Δημιουργήστε ένα νέο φάκελο που ονομάζεται **δεσμών ενεργειών** του έργου. 

3. Κάντε κλικ στην επιλογή **Προσθήκη στοιχείου -> νέα δέσμη ενεργειών -> C# δέσμης ενεργειών**. Ονομάστε το **PlayerController**και κάντε κλικ στην επιλογή **Δημιουργία και προσθήκη**. Αυτό θα δημιουργήσετε και να επισυνάψετε μια δέσμη ενεργειών στο αντικείμενο του προγράμματος αναπαραγωγής.  

    ![][14]

5. Μετακίνηση αυτήν τη δέσμη ενεργειών κάτω από το φάκελο **δεσμών ενεργειών** του έργου. 

6. Ανοίξτε τη δέσμη ενεργειών για επεξεργασία στο πρόγραμμα επεξεργασίας δέσμης ενεργειών Αγαπημένα, ενημερώστε τον κώδικα δέσμης ενεργειών με τον ακόλουθο κώδικα και αποθηκεύστε το. 

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
    
8. Σημειώστε ότι η δέσμη ενεργειών παραπάνω χρησιμοποιεί μια ιδιότητα **ταχύτητας** . Στο πρόγραμμα επεξεργασίας ενότητας, ενημερώστε την ιδιότητα ταχύτητας σε 10.  

    ![][15]

9. Πατήστε **αναπαραγωγή** στο πρόγραμμα επεξεργασίας της ενότητας. Τώρα θα πρέπει να μπορείτε να ελέγχετε την μπάλα με χρήση του πληκτρολογίου και θα πρέπει να περιστρέψετε και μετακίνηση. 

### <a name="moving-the-camera"></a>Μετακίνηση της κάμερας
Τα παρακάτω βήματα είναι από το [πρόγραμμα εκμάθησης ενότητας](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) και θα συνδέουν την **Κύρια κάμερας** στο αντικείμενο του **προγράμματος αναπαραγωγής** . 

1. Ενημέρωση του **Transform.Position** να είναι X = 0, Y = 10.5, Z =-10.  
2. Ενημέρωση του **Transform.Rotation** να είναι X = 45, Y = 0, Z = 0.  

    ![][16]

2. Προσθέστε μια νέα δέσμη ενεργειών που ονομάζεται **CameraController** για να το **MainCamera** και μετακινήστε το στο φάκελο δεσμών ενεργειών. 

    ![][17]

3. Άνοιγμα της δέσμης ενεργειών για την επεξεργασία και προσθέστε τον ακόλουθο κώδικα σε αυτήν:

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
    
5. Στο περιβάλλον ενότητας - σύρετε τη μεταβλητή προγράμματος αναπαραγωγής στην υποδοχή προγράμματος αναπαραγωγής για το αντικείμενο κύριες κάμερας, έτσι ώστε τα δύο έχουν συσχετιστεί με ένα το άλλο. 

    ![][18]

6. Τώρα εάν πατήσετε αναπαραγωγή στο πρόγραμμα επεξεργασίας ενότητας και περιστρέψετε το αντικείμενο μπάλα Player, στη συνέχεια, θα δείτε την κάμερα ακολουθεί στο την κίνηση.  

### <a name="setting-up-the-play-area"></a>Ρύθμιση της περιοχής αναπαραγωγής
Τα παρακάτω βήματα είναι από το [πρόγραμμα εκμάθησης ενότητας](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141). Θα δημιουργήσουμε το τοίχοι γύρω από το μηδέν, έτσι ώστε το αντικείμενο μπάλα Player δεν απόθεσης στην περιοχή αναπαραγωγής η κίνηση. 

1. Κάντε κλικ στην επιλογή **Δημιουργία -> Δημιουργία κενού -> αντικείμενο παιχνίδι** και ονομάστε το **τοίχων**

    ![][19]

2. Σε αυτό το αντικείμενο τοίχοι - δημιουργία ενός νέου **αντικειμένου 3D -> κύβου** και ονομάστε το "Δυτική τοίχου". 

    ![][20]

3. Ενημέρωση του **μετασχηματισμού -> θέση** και **μετασχηματισμού -> κλίμακα** για αυτό το αντικείμενο Δυτική τοίχου. 

    ![][21]

4. Αναπαραγωγή του τοίχου Δυτική για να δημιουργήσετε μια **Ανατολή τοίχου** με το ενημερωμένο μετασχηματισμός θέση και την κλίμακα. 

    ![][22]

5. Αναπαραγωγή του τοίχου Ανατολή για να δημιουργήσετε έναν **τοίχο Βόρεια** με το ενημερωμένο μετασχηματισμός θέση & κλίμακα. 

    ![][23]

6. Αναπαραγωγή του τοίχου βόρεια και δημιουργήστε έναν **τοίχο Νότια** με το ενημερωμένο μετασχηματισμός θέση και την κλίμακα. 

    ![][24]

### <a name="creating-collectible-objects"></a>Δημιουργία Collectible αντικειμένων
Τα παρακάτω βήματα είναι από το [πρόγραμμα εκμάθησης ενότητας](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141). Θα δημιουργήσουμε ορισμένα ελκυστική εμφάνιση αντικείμενα που θα αποτελούν το σύνολο των collectible αντικείμενα τα οποία το αντικείμενο μπάλα προγράμματος αναπαραγωγής πρέπει να 'συλλογή' με διένεξη με τους. 

1. Δημιουργία ενός νέου **αντικειμένου 3D κύβου** και ονομάστε το απάντηση κλήσης από την. 

2. Προσαρμόστε το **μετασχηματισμό -> Περιστροφή** & **Μετασχηματισμός -> κλίμακα** του αντικειμένου συλλογής. 

    ![][25]

3. Δημιουργία και να επισυνάψετε μια **Νέα C# δέσμη ενεργειών** που ονομάζεται **"Εναλλαγή"** στο αντικείμενο συλλογής. Βεβαιωθείτε ότι για να τοποθετήσετε τη δέσμη ενεργειών κάτω από το φάκελο δεσμών ενεργειών. 

    ![][26]

4. Ανοίξτε αυτήν τη δέσμη ενεργειών για την επεξεργασία και ενημέρωση να είναι τα εξής: 

        using UnityEngine;
        using System.Collections;
        
        public class Rotator : MonoBehaviour {
        
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }

5. Τώρα πατήσετε περιστροφή στη λειτουργία αναπαραγωγής στο πρόγραμμα επεξεργασίας ενότητας και την προβολή συλλογής αντικειμένου σας προς τον άξονα.

6. Δημιουργήστε ένα νέο φάκελο που ονομάζεται **Prefabs** 

    ![][27]

7. Σύρετε το αντικείμενο **Απάντηση κλήσης από** και τοποθετήστε το στο φάκελο Prefabs.

    ![][28]

8. Δημιουργήστε ένα νέο **κενό παιχνίδι αντικείμενο** που ονομάζεται **Pickups**. Επαναφέρετε τη θέση προέλευσης και, στη συνέχεια, σύρετε το αντικείμενο συλλογής κάτω από αυτό το αντικείμενο παιχνιδιών.  

    ![][29]

9. Το αντικείμενο **Απάντηση κλήσης από την** αναπαραγωγή και ανακοίνωση στο αντικείμενο **αρχής** γύρω από το αντικείμενο του **προγράμματος αναπαραγωγής** , ενημερώνοντας τις τιμές **X και Z του Transform.Position** σωστά. 

    ![][30]

10. Δημιουργήστε ένα **νέο υλικό** που ονομάζεται **Απάντηση κλήσης από την** και ενημέρωση να είναι με κόκκινο χρώμα, ενημερώνοντας την **ιδιότητα Albedo** παρόμοιο με το τι κάναμε για την ενημέρωση του αντικειμένου αρχής. 

    ![][31]

11. Εφαρμόστε το υλικό για όλα τα 4 αντικείμενα συλλογής.

    ![][32]

### <a name="collecting-the-pickup-objects"></a>Συλλογή των αντικειμένων συλλογής
Τα παρακάτω βήματα είναι από το [πρόγραμμα εκμάθησης ενότητας](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141). Θα ενημερώσουμε το πρόγραμμα αναπαραγωγής, ώστε να είναι μπορούν να 'συλλογή' τα αντικείμενα συλλογής με διένεξη με αυτά. 

1. Άνοιγμα της δέσμης ενεργειών **PlayerController** που συνδέονται με το πρόγραμμα αναπαραγωγής αντικείμενο για επεξεργασία και ενημέρωση σε τα εξής:  

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

2. Δημιουργήστε μια νέα **ετικέτα** που ονομάζεται **Επιλέξτε προς τα επάνω** (αυτό πρέπει να συμφωνεί με τι υπάρχει στη δέσμη ενεργειών)  

    ![][33]
    
    ![][34]

3. Εφαρμογή αυτής της **ετικέτας** στο αντικείμενο απάντηση κλήσης από την Prefab. 

    ![][35]

4. Ενεργοποιήστε το πλαίσιο ελέγχου **IsTrigger** για το αντικείμενο Prefab.

    ![][36]

5. Προσθέστε ένα σταθερό σώμα Prefab απάντηση κλήσης από το αντικείμενο. Για βελτιστοποίηση των επιδόσεων θα ενημερώσουμε το στατικό collider που χρησιμοποιήσαμε σε μια δυναμική collider της. 

    ![][37]
  
6. Τέλος, επιλέξτε την ιδιότητα **IsKinematic** για το αντικείμενο prefab. 

    ![][38]

7. Επισκέψεων **αναπαραγωγή** στο πρόγραμμα επεξεργασίας ενότητας και που θα μπορούν να παιχνίδι αυτό **ρίξετε μια μπάλα** , μετακινώντας το αντικείμενο προγράμματος αναπαραγωγής χρησιμοποιώντας τα πλήκτρα του πληκτρολογίου σας για εισαγωγή δεδομένων κατεύθυνση. 

### <a name="updating-the-game-for-mobile-play"></a>Ενημέρωση το παιχνίδι για κινητές συσκευές αναπαραγωγής
Οι παραπάνω ενότητες συμπέρασμα το βασικό πρόγραμμα εκμάθησης από ενότητας. Τώρα θα σας θα τροποποιήσετε το παιχνίδι ώστε να είναι κινητή συσκευή φιλικό. Σημειώστε ότι χρησιμοποιήσαμε είσοδο πληκτρολογίου για το παιχνίδι μέχρι στιγμής για σκοπούς δοκιμής. Τώρα θα σας θα το τροποποιήσετε ώστε να σας να ελέγχετε το πρόγραμμα αναπαραγωγής με τη χρήση της κίνησης του τηλεφώνου, π.χ. χρήση επιταχυνσιόμετρο ως δεδομένα εισόδου. 

Άνοιγμα της δέσμης ενεργειών **PlayerController** για επεξεργασία και να ενημερώσετε τη μέθοδο **FixedUpdate** για να χρησιμοποιήσετε την κίνηση από το επιταχυνσιόμετρο για να μετακινήσετε το αντικείμενο προγράμματος αναπαραγωγής. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

Αυτό το πρόγραμμα εκμάθησης συμπεραίνει μια βασική παιχνιδιών δημιουργία με ενότητας και μπορείτε να το αναπτύξετε σε μια συσκευή της επιλογής σας για το παιχνίδι. 

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

    
    
    
    
    
    
    
    
    
    
    
    
