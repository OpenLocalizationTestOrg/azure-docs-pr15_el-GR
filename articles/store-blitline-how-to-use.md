<properties 
    pageTitle="Πώς να χρησιμοποιήσετε Blitline για εικόνα επεξεργασίας - Azure δυνατοτήτων οδηγού" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία Blitline σε διαδικασία εικόνες μέσα σε μια εφαρμογή του Azure." 
    services="" 
    documentationCenter=".net" 
    authors="blitline-dev" 
    manager="jason@blitline.com" 
    editor="jason@blitline.com"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/09/2014" 
    ms.author="support@blitline.com"/>
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a>Πώς μπορείτε να χρησιμοποιήσετε Blitline με Azure και αποθήκευσης Azure

Αυτός ο οδηγός θα εξηγούν τον τρόπο για να αποκτήσετε πρόσβαση σε υπηρεσίες Blitline και να υποβάλλουν εργασίες για να Blitline.

## <a name="what-is-blitline"></a>Τι είναι το Blitline;

Blitline είναι μια εικόνα που βασίζεται στο cloud, η υπηρεσία που παρέχει για μεγάλες επιχειρήσεις επεξεργασίας εικόνας στην σε κλάσμα της τιμής που θα είχε το κόστος για να δημιουργήσετε τον εαυτό σας επεξεργασίας.

Το γεγονός είναι ότι εικόνα επεξεργασία έχει γίνει ξανά και ξανά, συνήθως αναδημιουργείται από το μηδέν για κάθε τοποθεσία Web. Θα σας συνειδητοποιήσετε ότι αυτό επειδή θα σας έχετε δημιουργήσει τους ένα εκατομμύρια φορές πολύ. Μία ημέρα που θα σας αποφασίσει ότι ίσως είναι ώρα απλώς κάνουμε αυτό για όλους τους συμμετέχοντες. Γνωρίζουμε πώς μπορείτε να το κάνετε, για να το κάνετε γρήγορα και αποτελεσματικά και αποθηκεύστε όλοι εργασίας στο μεταξύ.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [http://www.blitline.com](http://www.blitline.com).

## <a name="what-blitline-is-not"></a>Τι Blitline δεν είναι...

Για να αποσαφηνίσετε τι είναι χρήσιμη για Blitline, είναι συχνά ευκολότερη για τον προσδιορισμό τι Blitline δεν κάνει πριν από την αναβάθμισή.

- Blitline δεν διαθέτει γραφικά στοιχεία HTML για να αποστείλετε εικόνες. Πρέπει να έχετε εικόνες που είναι διαθέσιμες στο κοινό ή με περιορισμένα δικαιώματα που είναι διαθέσιμες για Blitline για την επίτευξη.

- ΔΕΝ κάνετε Blitline ζωντανή επεξεργασία εικόνας, όπως Aviary.com

- Blitline δεν αποδέχεται αποστολή εικόνων, που δεν είναι δυνατό να προωθήσετε τις εικόνες σας σε Blitline απευθείας. Πρέπει να προωθήσετε τους σε χώρο αποθήκευσης Azure ή άλλα σημεία Blitline υποστηρίζει και, στη συνέχεια, πείτε Blitline τι να κάνετε λήψη τους.

- Blitline μαζικά παράλληλο και κάντε οποιαδήποτε σύγχρονη επεξεργασία. Δηλαδή πρέπει να παρέχετε μια postback_url και μπορεί να σας ενημερώσουμε πότε θα σας γίνονται επεξεργασίας.

## <a name="create-a-blitline-account"></a>Δημιουργία λογαριασμού Blitline

[AZURE.INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a>Πώς μπορείτε να δημιουργήσετε μια εργασία Blitline

Blitline χρησιμοποιεί JSON για να ορίσετε τις ενέργειες που θέλετε να κάνετε σε μια εικόνα. Αυτό JSON αποτελείται από μερικά απλά πεδία.

Το παράδειγμα απλούστερος είναι ως εξής:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Εδώ έχουμε JSON που θα διαρκέσει μια "src" εικόνα "... boys.jpeg" και, στη συνέχεια, αλλάξτε το μέγεθος για να 240 x 140 αυτήν την εικόνα.

Το Αναγνωριστικό εφαρμογής είναι κάτι που μπορείτε να βρείτε την καρτέλα **ΠΛΗΡΟΦΟΡΊΕΣ ΣΎΝΔΕΣΗΣ** ή **ΔΙΑΧΕΊΡΙΣΗ** στην Azure. Είναι το αναγνωριστικό μυστικού που σας επιτρέπει να εκτελέσετε εργασίες σε Blitline.

Η παράμετρος "Αποθήκευση" προσδιορίζει πληροφορίες σχετικά με το σημείο όπου θέλετε να τοποθετήσετε την εικόνα όταν θα σας έχει την επεξεργασία. Σε αυτήν την περίπτωση trivial, έχετε ορίσαμε μία. Εάν δεν έχει καθοριστεί αποθήκη Blitline θα αποθηκεύσετε τοπικά (και προσωρινά) σε μια θέση στο cloud μοναδικών τιμών. Θα μπορούν να λαμβάνουν αυτήν τη θέση από την JSON που επιστρέφονται από Blitline όταν κάνετε την Blitline. Απαιτείται το αναγνωριστικό "εικόνα" και επιστρέφεται για εσάς όταν για τον προσδιορισμό αυτό ιδιαίτερα αποθηκευμένο εικόνα.

Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με τις *συναρτήσεις* υποστηρίζουμε εδώ: <http://www.blitline.com/docs/functions>

Μπορείτε επίσης να βρείτε τεκμηρίωση σχετικά με τις επιλογές έργου: <http://www.blitline.com/docs/api>

Όταν έχετε JSON το μόνο που πρέπει να κάνετε είναι **ΔΗΜΟΣΊΕΥΣΗ** για να`http://api.blitline.com/job`

Θα λάβετε JSON πίσω που μοιάζει κάπως έτσι:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


Αυτό σας ενημερώνει ότι Blitline έχει λάβει την αίτησή σας, το έχει τεθεί σε ουρά επεξεργασίας και, όταν ολοκληρωθεί η εικόνα θα είναι διαθέσιμες στη: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_Εφαρμογή\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a>Πώς μπορείτε να αποθηκεύσετε μια εικόνα στο λογαριασμό σας χώρο αποθήκευσης Azure

Εάν έχετε ένα λογαριασμό αποθήκευσης Azure, μπορείτε να έχετε εύκολα Blitline push την επεξεργασία εικόνων σε σας Azure κοντέινερ. Με την προσθήκη ενός "azure_destination" μπορείτε να καθορίσετε τη θέση και τα δικαιώματα για Blitline για να προωθήσετε σε.

Ακολουθεί ένα παράδειγμα:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Συμπληρώνοντας τις τιμές ΚΕΦΑΛΑΙΟΠΟΙΗΜΕΝΩΝ με το δικό σας, μπορείτε να υποβάλετε αυτό JSON για http://api.blitline.com/job και η εικόνα "src" θα υποβάλλονται σε επεξεργασία με θάμπωμα φίλτρο και, στη συνέχεια, να προωθούνται στις Azure προορισμού.

###<a name="please-note"></a>Λάβετε υπόψη:

Τις συσχετίσεις Ασφαλείας πρέπει να περιέχει ολόκληρη συσχετισμών Ασφαλείας τη διεύθυνση url, όπως το όνομα αρχείου του αρχείου προορισμού.

Παράδειγμα:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Μπορείτε, επίσης, να διαβάσετε την πιο πρόσφατη έκδοση του έγγραφα αποθήκευσης Azure του Blitline [εδώ](http://www.blitline.com/docs/azure_storage).


## <a name="next-steps"></a>Επόμενα βήματα

Επισκεφθείτε blitline.com για να διαβάσετε σχετικά με όλα τα άλλα δυνατότητες:

* Τελικό σημείο API Blitline έγγραφα <http://www.blitline.com/docs/api>
* Συναρτήσεις Blitline API <http://www.blitline.com/docs/functions>
* Παραδείγματα Blitline API <http://www.blitline.com/docs/examples>
* Τρίτο μέρος βιβλιοθήκη Nuget <http://nuget.org/packages/Blitline.Net>
