<properties
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε το API δέσμευση στο iOS"
    description="Πιο πρόσφατη iOS SDK - τον τρόπο χρήσης του API δέσμευση στο iOS"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-use-the-engagement-api-on-ios"></a>Πώς μπορείτε να χρησιμοποιήσετε το API δέσμευση στο iOS

Αυτό το έγγραφο είναι ένα πρόσθετο για το έγγραφο πώς μπορείτε να ενοποιήσετε δέσμευση σε iOS: παρέχει σε βάθος λεπτομέρειες σχετικά με τη χρήση του API δέσμευση για να αναφέρετε τα στατιστικά στοιχεία των εφαρμογών.

Λάβετε υπόψη ότι εάν θέλετε μόνο δέσμευση για την αναφορά της εφαρμογής σας περιόδους λειτουργίας, δραστηριότητες, παρουσιάζει σφάλμα και τεχνικές πληροφορίες, στη συνέχεια, ο πιο απλός τρόπος είναι να κάνετε όλων προσαρμοσμένου `UIViewController` αντικείμενα μεταβιβάζονται από το αντίστοιχο `EngagementViewController` τάξης.

Εάν θέλετε να κάνετε περισσότερες πληροφορίες, για παράδειγμα, εάν πρέπει να αναφέρετε εφαρμογή συγκεκριμένα συμβάντα, σφάλματα και εργασίες, ή εάν έχετε για την αναφορά της εφαρμογής σας δραστηριότητες με διαφορετικό τρόπο από το ένα υλοποιηθεί σε το `EngagementViewController` κλάδους, στη συνέχεια, θα πρέπει να χρησιμοποιήσετε το API δέσμευση.

Το API δέσμευση παρέχεται από το `EngagementAgent` τάξης. Μια παρουσία αυτής της κλάσης μπορεί να ανακτηθεί, καλώντας την `[EngagementAgent shared]` στατική μέθοδο (Σημειώστε ότι η `EngagementAgent` αντικείμενο που επιστρέφεται είναι singleton).

Πριν από τις κλήσεις API, το `EngagementAgent` αντικειμένου πρέπει να προετοιμαστεί καλώντας τη μέθοδο`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

##<a name="engagement-concepts"></a>Δέσμευση έννοιες

Τα παρακάτω τμήματα περιορίστε τις κοινές [Έννοιες δέσμευση Mobile](mobile-engagement-concepts.md) για την πλατφόρμα iOS.

### <a name="session-and-activity"></a>`Session`και`Activity`

*Δραστηριότητα* είναι συνήθως σχετίζεται με μία οθόνη της εφαρμογής, δηλαδή τη *δραστηριότητα* ξεκινά όταν της οθόνης εμφανίζεται και σταματά όταν είναι κλειστό οθόνης: Αυτό συμβαίνει, όταν είναι ενσωματωμένο στο SDK δέσμευση χρησιμοποιώντας το `EngagementViewController` κλάσεις.

Αλλά *δραστηριότητες* μπορεί επίσης να ελέγχεται με μη αυτόματο τρόπο από με χρήση του API δέσμευση. Αυτό σας επιτρέπει να διαιρέσετε μια δεδομένη οθόνη σε διάφορα μέρη sub για να δείτε περισσότερες λεπτομέρειες σχετικά με τη χρήση αυτής της οθόνης (για παράδειγμα σε γνωστά πόσο συχνά και πόσο χρόνο παράθυρα διαλόγου που χρησιμοποιούνται μέσα σε αυτή την οθόνη).

##<a name="reporting-activities"></a>Αναφορά δραστηριοτήτων

### <a name="user-starts-a-new-activity"></a>Χρήστης ξεκινά μια νέα δραστηριότητα

            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

Πρέπει να καλέσετε `startActivity()` κάθε φορά που αλλάζει το δραστηριοτήτων χρήστη. Η πρώτη κλήση σε αυτήν τη συνάρτηση ξεκινά μια νέα περίοδο λειτουργίας χρήστη.

### <a name="user-ends-his-current-activity"></a>Χρήστης τερματίζει την τρέχουσα δραστηριότητά

            [[EngagementAgent shared] endActivity];

> [AZURE.WARNING] Έχετε **ΠΟΤΈ** πρέπει να καλέσετε αυτήν τη συνάρτηση από τον εαυτό σας, εκτός εάν θέλετε να διαιρέσετε μια χρήση της εφαρμογής σας σε πολλές περίοδοι λειτουργίας: μια κλήση σε αυτήν τη συνάρτηση θα τερματίσετε την τρέχουσα περίοδο λειτουργίας, αμέσως, επομένως, οι επόμενες κλήσης για να `startActivity()` να ξεκινήσετε μια νέα περίοδο λειτουργίας. Αυτή η συνάρτηση ονομάζεται αυτόματα από το SDK, όταν η εφαρμογή σας είναι κλειστό.

##<a name="reporting-events"></a>Δημιουργία αναφορών συμβάντα

### <a name="session-events"></a>Συμβάντα περιόδου λειτουργίας

Συμβάντα περιόδου λειτουργίας που χρησιμοποιούνται συνήθως για την αναφορά τις ενέργειες που εκτελούνται από το χρήστη κατά την περίοδο λειτουργίας.

**Παράδειγμα χωρίς επιπλέον δεδομένα:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**Παράδειγμα με επιπλέον δεδομένα:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a>Μεμονωμένη συμβάντα

Αντίθετα με τα συμβάντα περίοδο λειτουργίας, μπορεί να χρησιμοποιηθεί μεμονωμένη συμβάντα εκτός του στο περιβάλλον μιας περιόδου λειτουργίας.

**Παράδειγμα:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

##<a name="reporting-errors"></a>Αναφορά σφαλμάτων

### <a name="session-errors"></a>Σφάλματα περιόδου λειτουργίας

Σφάλματα περιόδου λειτουργίας που χρησιμοποιούνται συνήθως για την αναφορά των σφαλμάτων που επηρεάζουν το χρήστη κατά την περίοδο λειτουργίας.

**Παράδειγμα:**

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a>Μεμονωμένη σφάλματα

Αντίθετα με σφάλματα περίοδο λειτουργίας, μπορεί να χρησιμοποιηθεί σφάλματα μεμονωμένη εκτός του στο περιβάλλον μιας περιόδου λειτουργίας.

**Παράδειγμα:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

##<a name="reporting-jobs"></a>Αναφορά εργασιών

**Παράδειγμα:**

Ας υποθέσουμε ότι θέλετε να αναφέρετε τη διάρκεια της διεργασίας σύνδεσης:

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a>Αναφορά σφαλμάτων κατά τη διάρκεια μιας εργασίας

Σφάλματα μπορούν να συσχετιστούν με μια εργασία που εκτελείται αντί να συσχετίζονται με την τρέχουσα περίοδο λειτουργίας του χρήστη.

**Παράδειγμα:**

Ας υποθέσουμε ότι θέλετε να αναφέρετε ένα σφάλμα κατά τη διαδικασία σύνδεσης:

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a>Συμβάντα κατά τη διάρκεια μιας εργασίας

Συμβάντα μπορούν να συσχετιστούν με μια εργασία που εκτελείται αντί να συσχετίζονται με την τρέχουσα περίοδο λειτουργίας του χρήστη.

**Παράδειγμα:**

Ας υποθέσουμε ότι έχουμε ένα κοινωνικό δίκτυο και χρησιμοποιούμε μια εργασία στην αναφορά ο συνολικός χρόνος κατά τον οποίο ο χρήστης είναι συνδεδεμένος με το διακομιστή. Ο χρήστης να λαμβάνετε μηνύματα από το φίλους, αυτό είναι ένα συμβάν εργασία.

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

##<a name="extra-parameters"></a>Επιπλέον παράμετροι

Εκτέλεση αυθαίρετου δεδομένων μπορείτε να το επισυνάψετε συμβάντα, σφάλματα, δραστηριότητες και οι εργασίες.

Αυτά τα δεδομένα να είναι δομημένα, που χρησιμοποιεί κλάσης NSDictionary του iOS.

Σημειώστε ότι μπορεί να περιέχει πρόσθετα `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` ή άλλες `NSDictionary` παρουσίες.

> [AZURE.NOTE] Η παράμετρος επιπλέον είναι σειριοποιημένο στο JSON. Εάν θέλετε να περάσει διαφορετικά αντικείμενα από αυτά που περιγράφεται παραπάνω, πρέπει να εφαρμόσετε την ακόλουθη μέθοδο στην τάξη σας:
>
             -(NSString*)JSONRepresentation;
>
> Η μέθοδος θα πρέπει να επιστρέψει μια αναπαράσταση JSON του αντικειμένου σας.

### <a name="example"></a>Παράδειγμα

    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Όρια

#### <a name="keys"></a>Πλήκτρα

Σε κάθε αριθμό-κλειδί του `NSDictionary` πρέπει να συμφωνεί με την ακόλουθη κανονική έκφραση:

`^[a-zA-Z][a-zA-Z_0-9]*`

Αυτό σημαίνει ότι πλήκτρα πρέπει να ξεκινούν με γράμμα τουλάχιστον μία, ακολουθούμενο από γράμματα, ψηφία ή χαρακτήρες υπογράμμισης (\_).

#### <a name="size"></a>Μέγεθος

Πρόσθετα περιορίζονται σε **1024** χαρακτήρες ανά κλήση (κωδικοποιημένη μία φορά στο JSON από τον παράγοντα δέσμευση).

Στο προηγούμενο παράδειγμα, το JSON αποστέλλεται στο διακομιστή είναι 58 χαρακτήρες:

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Αναφορά πληροφοριών εφαρμογής

Μπορείτε να αναφέρετε με μη αυτόματο τρόπο παρακολούθησης πληροφορίες (ή οποιαδήποτε άλλη πληροφορία συγκεκριμένη εφαρμογή) χρησιμοποιώντας το `sendAppInfo:` συνάρτηση.

Σημειώστε ότι αυτές οι πληροφορίες μπορούν να αποσταλούν βαθμιαία: μόνο η πιο πρόσφατη τιμή για έναν δεδομένο αριθμό-κλειδί θα διατηρηθούν για μια δεδομένη συσκευή.

Πρόσθετα του συμβάντος, όπως το `NSDictionary` κλάση χρησιμοποιείται για συνοπτική πληροφορίες εφαρμογής, σημειώστε ότι πίνακες ή δευτερεύουσες λεξικά θα αντιμετωπίζεται ως επίπεδο συμβολοσειρές (χρησιμοποιώντας σειριοποίησης JSON).

**Παράδειγμα:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Όρια

#### <a name="keys"></a>Πλήκτρα

Σε κάθε αριθμό-κλειδί του `NSDictionary` πρέπει να συμφωνεί με την ακόλουθη κανονική έκφραση:

`^[a-zA-Z][a-zA-Z_0-9]*`

Αυτό σημαίνει ότι πλήκτρα πρέπει να ξεκινούν με γράμμα τουλάχιστον μία, ακολουθούμενο από γράμματα, ψηφία ή χαρακτήρες υπογράμμισης (\_).

#### <a name="size"></a>Μέγεθος

Πληροφορίες εφαρμογής περιορίζονται σε **1024** χαρακτήρες ανά κλήση (κωδικοποιημένη μία φορά στο JSON από τον παράγοντα δέσμευση).

Στο προηγούμενο παράδειγμα, το JSON αποστέλλεται στο διακομιστή είναι 44 χαρακτήρες:

    {"birthdate":"1983-12-07","gender":"female"}
