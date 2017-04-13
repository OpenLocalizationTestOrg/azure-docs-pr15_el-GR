<properties 
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε την πρόσληψη API σε Windows Phone Silverlight" 
    description="Πώς μπορείτε να χρησιμοποιήσετε την πρόσληψη API σε Windows Phone Silverlight"    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" /> 

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="how-to-use-the-engagement-api-on-windows-phone-silverlight"></a>Πώς μπορείτε να χρησιμοποιήσετε την πρόσληψη API σε Windows Phone Silverlight

Αυτό το έγγραφο είναι ένα πρόσθετο για το [πώς μπορείτε να ενοποιήσετε δέσμευση Mobile στην εφαρμογή Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)έγγραφο. Παρέχει σε βάθος λεπτομέρειες σχετικά με τη χρήση του API δέσμευση για να αναφέρετε τα στατιστικά στοιχεία των εφαρμογών.

Εάν θέλετε μόνο δέσμευση για την αναφορά της εφαρμογής σας περιόδους λειτουργίας, δραστηριότητες, παρουσιάζει σφάλμα και τεχνικές πληροφορίες και, στη συνέχεια, ο πιο απλός τρόπος είναι να κάνετε όλων σας `PhoneApplicationPage` δευτερεύουσες κλάσεις μεταβιβάζονται από το `EngagementPage` τάξης.

Εάν θέλετε να κάνετε περισσότερες πληροφορίες, για παράδειγμα, εάν πρέπει να αναφέρετε εφαρμογή συγκεκριμένα συμβάντα, σφάλματα και εργασίες, ή εάν έχετε για την αναφορά της εφαρμογής σας δραστηριότητες με διαφορετικό τρόπο από το ένα υλοποιηθεί σε το `EngagementPage` κλάδους, στη συνέχεια, θα πρέπει να χρησιμοποιήσετε το API δέσμευση.

Το API δέσμευση παρέχεται από το `EngagementAgent` τάξης. Μπορείτε να αποκτήσετε πρόσβαση σε αυτές τις μεθόδους μέσω `EngagementAgent.Instance`.

Ακόμα και αν δεν έχει προετοιμαστεί τη λειτουργική μονάδα παράγοντας, κάθε κλήσης για το API είναι αναβάλει και θα εκτελεστεί ξανά, όταν ο παράγοντας είναι διαθέσιμη.

##<a name="engagement-concepts"></a>Δέσμευση έννοιες

Τα παρακάτω τμήματα περιορίστε τις έννοιες δέσμευση Mobile για την πλατφόρμα Windows Phone.

### <a name="session-and-activity"></a>`Session`και`Activity`

*Δραστηριότητα* είναι συνήθως σχετίζεται με μία σελίδα της εφαρμογής, αυτό σημαίνει ότι τη *δραστηριότητα* ξεκινά όταν η σελίδα εμφανίζεται και σταματά όταν είναι κλειστό στη σελίδα: Αυτό συμβαίνει, όταν είναι ενσωματωμένο στο SDK δέσμευση χρησιμοποιώντας το `EngagementPage` τάξης.

Αλλά *δραστηριότητες* μπορεί επίσης να ελέγχεται με μη αυτόματο τρόπο από με χρήση του API δέσμευση. Αυτό σας επιτρέπει να διαιρέσετε μια συγκεκριμένη σελίδα σε διάφορα μέρη sub για να δείτε περισσότερες λεπτομέρειες σχετικά με τη χρήση αυτής της σελίδας (για παράδειγμα σε γνωστά πόσο συχνά και πόσο χρόνο παράθυρα διαλόγου που χρησιμοποιούνται μέσα σε αυτήν τη σελίδα).

##<a name="reporting-activities"></a>Αναφορά δραστηριοτήτων

### <a name="user-starts-a-new-activity"></a>Χρήστης ξεκινά μια νέα δραστηριότητα

#### <a name="reference"></a>Αναφορά

            void StartActivity(string name, Dictionary<object, object> extras = null)

Πρέπει να καλέσετε `StartActivity()` κάθε φορά που αλλάζει το δραστηριοτήτων χρήστη. Η πρώτη κλήση σε αυτήν τη συνάρτηση ξεκινά μια νέα περίοδο λειτουργίας χρήστη.

> [AZURE.IMPORTANT] Το SDK καλέστε αυτόματα τη μέθοδο EndActivity όταν η εφαρμογή είναι κλειστό. Επομένως, συνιστάται ΙΔΙΑΊΤΕΡΑ να καλέσετε τη μέθοδο StartActivity κάθε φορά που τη δραστηριότητα από την αλλαγή χρηστών και ποτέ κλήση της μεθόδου EndActivity, επειδή το καλέσετε αυτή τη μέθοδο επιβάλει την τρέχουσα περίοδο λειτουργίας θα τερματιστεί.

#### <a name="example"></a>Παράδειγμα

            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Χρήστης τερματίζει την τρέχουσα δραστηριότητά

#### <a name="reference"></a>Αναφορά

            void EndActivity()

Πρέπει να καλέσετε `EndActivity()` τουλάχιστον μία φορά όταν ο χρήστης ολοκληρώσει την τελευταία δραστηριότητά. Αυτό πληροφορεί το SDK δέσμευση ότι ο χρήστης είναι αυτήν τη στιγμή αδράνειας και ότι η περίοδος λειτουργίας χρήστη πρέπει να είναι κλειστά μία φορά το χρονικό όριο περιόδου λειτουργίας θα λήξει (αν καλέσετε `StartActivity()` πριν λήξει το χρονικό όριο περιόδου λειτουργίας, απλώς συνεχίζεται η περίοδος λειτουργίας).

#### <a name="example"></a>Παράδειγμα

            EngagementAgent.Instance.EndActivity();

##<a name="reporting-jobs"></a>Αναφορά εργασιών

### <a name="start-a-job"></a>Έναρξη μιας εργασίας

#### <a name="reference"></a>Αναφορά

            void StartJob(string name, Dictionary<object, object> extras = null)

Μπορείτε να χρησιμοποιήσετε το έργο για την παρακολούθηση ορισμένες εργασίες σε μια χρονική περίοδο.

#### <a name="example"></a>Παράδειγμα

            // An upload begins...
            
            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");
            
            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Τερματισμός μιας εργασίας

#### <a name="reference"></a>Αναφορά

            void EndJob(string name)

Μόλις τερματίστηκε εργασίας παρακολουθείται από μια εργασία, θα πρέπει να καλέσετε τη μέθοδο EndJob για αυτή την εργασία, παρέχοντας το όνομα της εργασίας.

#### <a name="example"></a>Παράδειγμα

            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends
            
            EngagementAgent.Instance.EndJob("uploadData");

##<a name="reporting-events"></a>Δημιουργία αναφορών συμβάντα

Υπάρχει τρεις τύπους συμβάντων:

-   Μεμονωμένη συμβάντα
-   Συμβάντα περιόδου λειτουργίας
-   Εργασία συμβάντα

### <a name="standalone-events"></a>Μεμονωμένη συμβάντα

#### <a name="reference"></a>Αναφορά

            void SendEvent(string name, Dictionary<object, object> extras = null)

Μεμονωμένη συμβάντα μπορεί να προκύψει εκτός του στο περιβάλλον μιας περιόδου λειτουργίας.

#### <a name="example"></a>Παράδειγμα

            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Συμβάντα περιόδου λειτουργίας

#### <a name="reference"></a>Αναφορά

            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Συμβάντα περιόδου λειτουργίας που χρησιμοποιούνται συνήθως για την αναφορά τις ενέργειες που εκτελούνται από το χρήστη κατά την περίοδο λειτουργίας.

#### <a name="example"></a>Παράδειγμα

**Χωρίς δεδομένα:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");
            
            // or
            
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**Με τα δεδομένα:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Εργασία συμβάντα

#### <a name="reference"></a>Αναφορά

            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Εργασία συμβάντα που χρησιμοποιούνται συνήθως για την αναφορά τις ενέργειες που εκτελούνται από το χρήστη κατά τη διάρκεια μιας εργασίας.

#### <a name="example"></a>Παράδειγμα

            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

##<a name="reporting-errors"></a>Αναφορά σφαλμάτων

Υπάρχει τρεις τύπους σφαλμάτων:

-   Μεμονωμένη σφάλματα
-   Σφάλματα περιόδου λειτουργίας
-   Σφάλματα εργασίας

### <a name="standalone-errors"></a>Μεμονωμένη σφάλματα

#### <a name="reference"></a>Αναφορά

            void SendError(string name, Dictionary<object, object> extras = null)

Αντίθετα με σφάλματα περιόδου λειτουργίας, μεμονωμένη σφάλματα μπορεί να προκύψουν εκτός του στο περιβάλλον μιας περιόδου λειτουργίας.

#### <a name="example"></a>Παράδειγμα

            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Σφάλματα περιόδου λειτουργίας

#### <a name="reference"></a>Αναφορά

            void SendSessionError(string name, Dictionary<object, object> extras = null)

Σφάλματα περιόδου λειτουργίας που χρησιμοποιούνται συνήθως για την αναφορά των σφαλμάτων που επηρεάζουν το χρήστη κατά την περίοδο λειτουργίας.

#### <a name="example"></a>Παράδειγμα

            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Σφάλματα εργασίας

#### <a name="reference"></a>Αναφορά

            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Σφάλματα μπορούν να συσχετιστούν με μια εργασία που εκτελείται αντί να συσχετίζονται με την τρέχουσα περίοδο λειτουργίας του χρήστη.

#### <a name="example"></a>Παράδειγμα

            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

##<a name="reporting-crashes"></a>Δημιουργία αναφορών παρουσιάζει σφάλμα

Ο παράγοντας παρέχει δύο μεθόδους για να αντιμετωπίσετε παρουσιάσει σφάλμα.

### <a name="send-an-exception"></a>Αποστολή μιας εξαίρεσης

#### <a name="reference"></a>Αναφορά

            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Παράδειγμα

Μπορείτε να στείλετε μια εξαίρεση ανά πάσα στιγμή, καλώντας:

            EngagementAgent.Instance.SendCrash(aCatchedException);

Μπορείτε επίσης να χρησιμοποιήσετε μια προαιρετική παράμετρος για να τερματίσετε την περίοδο λειτουργίας δέσμευση την ίδια στιγμή από την αποστολή το σφάλμα. Για να το κάνετε, καλέστε:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Εάν κάνετε αυτό, η περίοδος λειτουργίας και οι εργασίες θα κλείσει μόνο μετά την αποστολή το σφάλμα.

### <a name="send-an-unhandled-exception"></a>Στείλτε μια ανεπίλυτη εξαίρεση

#### <a name="reference"></a>Αναφορά

            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

Δέσμευση παρέχει επίσης μια μέθοδο για να στείλετε ανεπίλυτη εξαιρέσεις. Αυτό είναι ιδιαίτερα χρήσιμο όταν χρησιμοποιείται μέσα το πρόγραμμα χειρισμού συμβάντων UnhandledException silverlight.

Αυτό θα μέθοδο **ΠΆΝΤΑ** τερματιστεί η περίοδος λειτουργίας δέσμευση και οι εργασίες μετά την κλήση.

#### <a name="example"></a>Παράδειγμα

Μπορείτε να το χρησιμοποιήσετε για να υλοποιήσετε το δικό σας πρόγραμμα χειρισμού UnhandledException (ειδικά εάν έχετε απενεργοποιήσει την αυτόματη σφάλμα δυνατότητα της δέσμευσης αναφοράς). Για παράδειγμα, το `Application_UnhandledException` μέθοδο το `App.xaml.cs` αρχείο:

            // In your App.xaml.cs file
            
            // Code to execute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code
            
              EngagementAgent.Instance.SendCrash(e);
            }

##<a name="onactivated"></a>OnActivated

### <a name="reference"></a>Αναφορά

            void OnActivated(ActivatedEventArgs e)

Όταν ο χρήστης μεταβαίνει προς τα εμπρός, μακριά από μια εφαρμογή, μετά το συμβάν Deactivated ενεργοποιείται, το λειτουργικό σύστημα θα προσπαθήσει να τοποθετήσετε την εφαρμογή σε αδρανείς κατάσταση. Στη συνέχεια, η εφαρμογή είναι Tombstoning. Σε αυτήν τη διαδικασία τερματίζεται μια εφαρμογή, αλλά διατηρείται ορισμένα δεδομένα σχετικά με την κατάσταση της αίτησης και τις μεμονωμένες σελίδες μέσα από την εφαρμογή.

Πρέπει να εισαγάγετε `EngagementAgent.Instance.OnActivated(e)` στο το `Application_Activated` μέθοδο από το αρχείο App.xaml.cs για να επαναφέρετε τον παράγοντα δέσμευση όταν η εφαρμογή έχει Tombstoned.

### <a name="example"></a>Παράδειγμα

            // Inside your App.xaml.cs file
            
            // Code to execute when the application is activated (brought to foreground)
            // This code will not execute when the application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

##<a name="device-id"></a>Αναγνωριστικό συσκευής

            String GetDeviceId()

Μπορείτε να λάβετε το αναγνωριστικό συσκευής δέσμευση καλώντας αυτήν τη μέθοδο.

##<a name="extras-parameters"></a>Παράμετροι πρόσθετα

Μπορείτε να το επισυνάψετε αυθαίρετο δεδομένων σε ένα συμβάν, ένα μήνυμα σφάλματος, μια δραστηριότητα ή μια εργασία. Αυτά τα δεδομένα να είναι δομημένα χρησιμοποιώντας ένα λεξικό. Κλειδιά και τιμές μπορεί να είναι οποιουδήποτε τύπου.

Πρόσθετα δεδομένα είναι σειριοποίηση, ώστε να εάν θέλετε να εισαγάγετε το δικό σας τύπο σε πρόσθετα έχετε για να προσθέσετε ένα συμβόλαιο δεδομένων για αυτόν τον τύπο.

### <a name="example"></a>Παράδειγμα

Μπορούμε να δημιουργήσουμε μια νέα κλάση "Άτομο".

            using System.Runtime.Serialization;
            
            namespace Engagement.Agent
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }
            
                // Properties
            
                [DataMember]
                public int Age
                {
                  get;
                  set;
                }
            
                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Στη συνέχεια, θα προσθέσουμε μια `Person` παρουσία για ένα επιπλέον.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);
            
            EngagementAgent.Instance.SendEvent("Event", extras);

> [AZURE.WARNING] Εάν τοποθετήσετε άλλοι τύποι αντικειμένων, βεβαιωθείτε ότι έχει υλοποιηθεί τη μέθοδο ToString() τους για να επιστρέψει μια συμβολοσειρά human ευανάγνωστη.

### <a name="limits"></a>Όρια

#### <a name="keys"></a>Πλήκτρα

Κάθε κλειδί στο αντικείμενο πρέπει να συμφωνεί με την ακόλουθη κανονική έκφραση:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Αυτό σημαίνει ότι πλήκτρα πρέπει να ξεκινούν με γράμμα τουλάχιστον μία, ακολουθούμενο από γράμματα, ψηφία ή χαρακτήρες υπογράμμισης (\_).

#### <a name="size"></a>Μέγεθος

Πρόσθετα περιορίζονται σε **1024** χαρακτήρες ανά κλήση.

##<a name="reporting-application-information"></a>Αναφορά πληροφοριών εφαρμογής

### <a name="reference"></a>Αναφορά

            void SendAppInfo(Dictionary<object, object> appInfos)

Μπορείτε να αναφέρετε με μη αυτόματο τρόπο παρακολούθησης πληροφορίες (ή οποιαδήποτε άλλη πληροφορία συγκεκριμένη εφαρμογή) χρησιμοποιώντας το SendAppInfo() συνάρτηση.

Σημειώστε ότι αυτές οι πληροφορίες μπορούν να αποσταλούν βαθμιαία: μόνο η πιο πρόσφατη τιμή για έναν δεδομένο αριθμό-κλειδί θα διατηρηθούν για μια δεδομένη συσκευή. Όπως πρόσθετα του συμβάντος, χρησιμοποιήσει ένα λεξικό\<αντικείμενο, αντικείμενο\> για να επισυνάψετε πληροφορίες.

### <a name="example"></a>Παράδειγμα

            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };
            
            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Όρια

#### <a name="keys"></a>Πλήκτρα

Κάθε κλειδί στο αντικείμενο πρέπει να συμφωνεί με την ακόλουθη κανονική έκφραση:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Αυτό σημαίνει ότι πλήκτρα πρέπει να ξεκινούν με γράμμα τουλάχιστον μία, ακολουθούμενο από γράμματα, ψηφία ή χαρακτήρες υπογράμμισης (\_).

#### <a name="size"></a>Μέγεθος

Πληροφορίες εφαρμογής περιορίζονται σε **1024** χαρακτήρες ανά κλήση.

Στο προηγούμενο παράδειγμα, το JSON αποστέλλεται στο διακομιστή είναι 44 χαρακτήρες:

            {"subscription":"2013-12-07","premium":"true"}
 
##<a name="logging"></a>Καταγραφή
###<a name="enable-logging"></a>Ενεργοποίηση καταγραφής

Το SDK μπορεί να ρυθμιστεί για την παραγωγή δοκιμής αρχείων καταγραφής στην κονσόλα IDE.
Αυτά τα αρχεία καταγραφής δεν είναι ενεργοποιημένες από προεπιλογή. Για να προσαρμόσετε έτσι, ενημερώστε την ιδιότητα `EngagementAgent.Instance.TestLogEnabled` σε μία από την τιμή που είναι διαθέσιμα από το `EngagementTestLogLevel` απαρίθμηση, για παράδειγμα:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
