<properties
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε την πρόσληψη API σε Android"
    description="Τελευταία Android SDK - πώς μπορείτε να χρησιμοποιήσετε την πρόσληψη API σε Android"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="piyushjo;ricksal" />

#<a name="how-to-use-the-engagement-api-on-android"></a>Πώς μπορείτε να χρησιμοποιήσετε την πρόσληψη API σε Android

Αυτό το έγγραφο είναι ένα πρόσθετο για το έγγραφο [Επιλογές για προχωρημένους αναφοράς για Android SDK δέσμευση Mobile](mobile-engagement-android-advanced-reporting.md). Παρέχει σε βάθος λεπτομέρειες σχετικά με τη χρήση του API δέσμευση για να αναφέρετε τα στατιστικά στοιχεία των εφαρμογών.

Λάβετε υπόψη ότι εάν θέλετε μόνο δέσμευση για την αναφορά της εφαρμογής σας περιόδους λειτουργίας, δραστηριότητες, παρουσιάζει σφάλμα και τεχνικές πληροφορίες, στη συνέχεια, ο πιο απλός τρόπος είναι να κάνετε όλων σας `Activity` δευτερεύουσες κλάσεις μεταβιβάζονται από το αντίστοιχο `EngagementActivity` τάξης.

Εάν θέλετε να κάνετε περισσότερες πληροφορίες, για παράδειγμα, εάν πρέπει να αναφέρετε εφαρμογή συγκεκριμένα συμβάντα, σφάλματα και εργασίες, ή εάν έχετε για την αναφορά της εφαρμογής σας δραστηριότητες με διαφορετικό τρόπο από το ένα υλοποιηθεί σε το `EngagementActivity` κλάδους, στη συνέχεια, θα πρέπει να χρησιμοποιήσετε το API δέσμευση.

Το API δέσμευση παρέχεται από το `EngagementAgent` τάξης. Μια παρουσία αυτής της κλάσης μπορεί να ανακτηθεί, καλώντας την `EngagementAgent.getInstance(Context)` στατική μέθοδο (Σημειώστε ότι η `EngagementAgent` αντικείμενο που επιστρέφεται είναι singleton).

##<a name="engagement-concepts"></a>Δέσμευση έννοιες

Τα παρακάτω τμήματα περιορίστε τις κοινές [Έννοιες δέσμευση Mobile](mobile-engagement-concepts.md), για την πλατφόρμα Android.

### <a name="session-and-activity"></a>`Session`και`Activity`

Εάν ο χρήστης παραμένει αδρανής μεταξύ δύο *δραστηριότητες*περισσότερα από μερικά δευτερόλεπτα, στη συνέχεια, την ακολουθία των *δραστηριοτήτων* χωρίζεται σε δύο ξεχωριστές *περιόδους λειτουργίας*. Αυτά τα μερικά δευτερόλεπτα ονομάζονται "χρονικό όριο περιόδου λειτουργίας".

*Δραστηριότητα* είναι συνήθως σχετίζεται με μία οθόνη της εφαρμογής, δηλαδή τη *δραστηριότητα* ξεκινά όταν της οθόνης εμφανίζεται και σταματά όταν είναι κλειστό οθόνης: Αυτό συμβαίνει, όταν είναι ενσωματωμένο στο SDK δέσμευση χρησιμοποιώντας το `EngagementActivity` κλάσεις.

Αλλά *δραστηριότητες* μπορεί επίσης να ελέγχεται με μη αυτόματο τρόπο από με χρήση του API δέσμευση. Αυτό σας επιτρέπει να διαιρέσετε μια δεδομένη οθόνη σε διάφορα μέρη sub για να δείτε περισσότερες λεπτομέρειες σχετικά με τη χρήση αυτής της οθόνης (για παράδειγμα σε γνωστά πόσο συχνά και πόσο χρόνο παράθυρα διαλόγου που χρησιμοποιούνται μέσα σε αυτή την οθόνη).

##<a name="reporting-activities"></a>Αναφορά δραστηριοτήτων

> [AZURE.IMPORTANT] Δεν χρειάζεται να αναφοράς δραστηριότητες, όπως περιγράφεται σε αυτήν την ενότητα, εάν χρησιμοποιείτε το `EngagementActivity` τάξης και τις παραλλαγές, όπως εξηγείται στο θέμα το πώς μπορείτε να ενοποιήσετε δέσμευση σε Android έγγραφο.

### <a name="user-starts-a-new-activity"></a>Χρήστης ξεκινά μια νέα δραστηριότητα

            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

Πρέπει να καλέσετε `startActivity()` κάθε φορά που αλλάζει το δραστηριοτήτων χρήστη. Η πρώτη κλήση σε αυτήν τη συνάρτηση ξεκινά μια νέα περίοδο λειτουργίας χρήστη.

Το καλύτερο σημείο για να καλέσετε αυτή η συνάρτηση είναι σε κάθε δραστηριότητα `onResume` επιστροφής κλήσης.

### <a name="user-ends-his-current-activity"></a>Χρήστης τερματίζει την τρέχουσα δραστηριότητά

            EngagementAgent.getInstance(this).endActivity();

Πρέπει να καλέσετε `endActivity()` τουλάχιστον μία φορά όταν ο χρήστης ολοκληρώσει την τελευταία δραστηριότητά. Αυτό πληροφορεί το SDK δέσμευση ότι ο χρήστης είναι αυτήν τη στιγμή αδράνειας και ότι η περίοδος λειτουργίας χρήστη πρέπει να είναι κλειστά μία φορά το χρονικό όριο περιόδου λειτουργίας θα λήξει (αν καλέσετε `startActivity()` πριν λήξει το χρονικό όριο περιόδου λειτουργίας, απλώς πριν αρχίσει την περίοδο λειτουργίας).

Το καλύτερο σημείο για να καλέσετε αυτή η συνάρτηση είναι σε κάθε δραστηριότητα `onPause` επιστροφής κλήσης.

##<a name="reporting-events"></a>Δημιουργία αναφορών συμβάντα

### <a name="session-events"></a>Συμβάντα περιόδου λειτουργίας

Συμβάντα περιόδου λειτουργίας που χρησιμοποιούνται συνήθως για την αναφορά τις ενέργειες που εκτελούνται από το χρήστη κατά την περίοδο λειτουργίας.

**Παράδειγμα χωρίς επιπλέον δεδομένα:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Παράδειγμα με επιπλέον δεδομένα:**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a>Μεμονωμένη συμβάντα

Αντίθετα με την περίοδο λειτουργίας συμβάντα, μεμονωμένη συμβάντα μπορεί να προκύψει εκτός του στο περιβάλλον μιας περιόδου λειτουργίας.

**Παράδειγμα:**

Ας υποθέσουμε ότι θέλετε να αναφοράς συμβάντα που προκύπτουν όταν ενεργοποιείται μια ακουστικό εκπομπής:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

##<a name="reporting-errors"></a>Αναφορά σφαλμάτων

### <a name="session-errors"></a>Σφάλματα περιόδου λειτουργίας

Σφάλματα περιόδου λειτουργίας που χρησιμοποιούνται συνήθως για την αναφορά των σφαλμάτων που επηρεάζουν το χρήστη κατά την περίοδο λειτουργίας.

**Παράδειγμα:**

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a>Μεμονωμένη σφάλματα

Αντίθετα με σφάλματα περιόδου λειτουργίας, μεμονωμένη σφάλματα μπορεί να προκύψουν εκτός του στο περιβάλλον μιας περιόδου λειτουργίας.

**Παράδειγμα:**

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να αναφέρετε το σφάλμα κάθε φορά που η μνήμη είναι χαμηλή στο τηλέφωνο, ενώ εκτελείται το διαδικασία εφαρμογής.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

##<a name="reporting-jobs"></a>Αναφορά εργασιών

### <a name="example"></a>Παράδειγμα

Ας υποθέσουμε ότι θέλετε να αναφέρετε τη διάρκεια της διεργασίας σύνδεσης:

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a>Αναφορά σφαλμάτων κατά τη διάρκεια μιας εργασίας

Σφάλματα μπορούν να συσχετιστούν με μια εργασία που εκτελείται αντί να συσχετίζονται με την τρέχουσα περίοδο λειτουργίας του χρήστη.

**Παράδειγμα:**

Ας υποθέσουμε ότι θέλετε να καταγγείλετε σφάλμα κατά τη διάρκεια που συνδεθείτε διαδικασία:

[...] δημόσια void εισόδου (περιβάλλον περιβάλλον,...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a>Δημιουργία αναφορών συμβάντα κατά τη διάρκεια μιας εργασίας

Συμβάντα μπορούν να συσχετιστούν με μια εργασία που εκτελείται αντί να συσχετίζονται με την τρέχουσα περίοδο λειτουργίας του χρήστη.

**Παράδειγμα:**

Ας υποθέσουμε ότι έχουμε ένα κοινωνικό δίκτυο και χρησιμοποιούμε μια εργασία στην αναφορά ο συνολικός χρόνος κατά τον οποίο ο χρήστης είναι συνδεδεμένος με το διακομιστή. Ο χρήστης μπορεί να παραμείνετε συνδεδεμένοι στο παρασκήνιο, ακόμα και όταν χρησιμοποιεί μια άλλη εφαρμογή ή όταν το τηλέφωνο βρίσκεται σε αναστολή λειτουργίας, επομένως δεν υπάρχει καμία περίοδο λειτουργίας.

Ο χρήστης να λαμβάνετε μηνύματα από το φίλους, αυτό είναι ένα συμβάν εργασία.

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

##<a name="extra-parameters"></a>Επιπλέον παράμετροι

Εκτέλεση αυθαίρετου δεδομένων μπορείτε να το επισυνάψετε συμβάντα, σφάλματα, δραστηριότητες και οι εργασίες.

Αυτά τα δεδομένα να είναι δομημένα, αυτό χρησιμοποιεί κλάση πακέτου στο Android (στην πραγματικότητα, λειτουργεί όπως επιπλέον παραμέτρων σε Android στόχοι). Σημειώστε ότι ένα πακέτο μπορεί να περιέχει πίνακες ή άλλη δέσμη παρουσιών.

> [AZURE.IMPORTANT] Εάν τοποθετήσετε σε parcelable ή σειριοποιηθεί παραμέτρους, βεβαιωθείτε ότι το `toString()` έχει υλοποιηθεί μέθοδο για να επιστρέψει μια συμβολοσειρά αναγνώσιμο. Δυνατότητα σειριοποίησης κλάσεις που περιέχουν μη μεταβατικές πεδία που δεν θα κάνετε Android σφάλμα όταν θα καλείτε`bundle.putSerializable("key",value);`

> [AZURE.WARNING] Κατακερματισμένο πίνακες σε επιπλέον παράμετροι δεν υποστηρίζονται, αυτό σημαίνει ότι δεν θα να σειριοποιηθεί ως πίνακα. Θα πρέπει να τις μετατρέψετε σε τυπική πίνακες πριν τη χρησιμοποιήσετε σε επιπλέον παράμετροι.

### <a name="example"></a>Παράδειγμα

            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Όρια

#### <a name="keys"></a>Πλήκτρα

Σε κάθε αριθμό-κλειδί του `Bundle` πρέπει να συμφωνεί με την ακόλουθη κανονική έκφραση:

`^[a-zA-Z][a-zA-Z_0-9]*`

Αυτό σημαίνει ότι πλήκτρα πρέπει να ξεκινούν με γράμμα τουλάχιστον μία, ακολουθούμενο από γράμματα, ψηφία ή χαρακτήρες υπογράμμισης (\_).

#### <a name="size"></a>Μέγεθος

Πρόσθετα περιορίζονται σε **1024** χαρακτήρες ανά κλήση (κωδικοποιημένη μία φορά στο JSON από την υπηρεσία δέσμευση).

Στο προηγούμενο παράδειγμα, το JSON αποστέλλεται στο διακομιστή είναι 58 χαρακτήρες:

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Αναφορά πληροφοριών εφαρμογής

Μπορείτε να αναφέρετε με μη αυτόματο τρόπο παρακολούθησης πληροφορίες (ή οποιαδήποτε άλλη πληροφορία συγκεκριμένη εφαρμογή) χρησιμοποιώντας το `sendAppInfo()` συνάρτηση.

Σημειώστε ότι αυτές οι πληροφορίες μπορούν να αποσταλούν βαθμιαία: μόνο η πιο πρόσφατη τιμή για έναν δεδομένο αριθμό-κλειδί θα διατηρηθούν για μια δεδομένη συσκευή.

Όπως πρόσθετα του συμβάντος, την κλάση πακέτου χρησιμοποιείται για συνοπτική πληροφορίες εφαρμογής, σημειώστε ότι πίνακες ή δευτερεύουσες πακέτα θα αντιμετωπίζεται ως επίπεδο συμβολοσειρές (χρησιμοποιώντας σειριοποίησης JSON).

### <a name="example"></a>Παράδειγμα

Ακολουθεί ένα δείγμα κώδικα για την αποστολή φύλο χρήστη και ημερομηνία γέννησης:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Όρια

#### <a name="keys"></a>Πλήκτρα

Σε κάθε αριθμό-κλειδί του `Bundle` πρέπει να συμφωνεί με την ακόλουθη κανονική έκφραση:

`^[a-zA-Z][a-zA-Z_0-9]*`

Αυτό σημαίνει ότι πλήκτρα πρέπει να ξεκινούν με γράμμα τουλάχιστον μία, ακολουθούμενο από γράμματα, ψηφία ή χαρακτήρες υπογράμμισης (\_).

#### <a name="size"></a>Μέγεθος

Πληροφορίες εφαρμογής περιορίζονται σε **1024** χαρακτήρες ανά κλήση (κωδικοποιημένη μία φορά στο JSON από την υπηρεσία δέσμευση).

Στο προηγούμενο παράδειγμα, το JSON αποστέλλεται στο διακομιστή είναι 44 χαρακτήρες:

            {"expiration":"2016-12-07","status":"premium"}
