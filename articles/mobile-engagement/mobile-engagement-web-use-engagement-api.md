<properties
    pageTitle="Azure κινητή δέσμευση Web SDK APIs | Microsoft Azure"
    description="Τις πιο πρόσφατες ενημερώσεις και διαδικασίες για το SDK Web για δέσμευση Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="piyushjo" />

# <a name="use-the-azure-mobile-engagement-api-in-a-web-application"></a>Χρήση του API Azure Mobile δέσμευση σε μια εφαρμογή web

Αυτό το έγγραφο είναι μια προσθήκη στο έγγραφο που σας ενημερώνει πώς μπορείτε να [ενοποιήσετε δέσμευση Mobile σε μια εφαρμογή web](mobile-engagement-web-integrate-engagement.md). Παρέχει αναλυτικά λεπτομέρειες σχετικά με τη χρήση του API Azure Mobile δέσμευση για να αναφέρετε τα στατιστικά στοιχεία των εφαρμογών.

Το API δέσμευση Mobile παρέχεται από το `engagement.agent` αντικειμένου. Η προεπιλογή είναι ψευδώνυμο Azure Mobile δέσμευση Web SDK `engagement`. Μπορείτε να επανακαθορίσετε αυτό το ψευδώνυμο από τη ρύθμιση παραμέτρων SDK.

## <a name="mobile-engagement-concepts"></a>Δέσμευση Mobile έννοιες

Τα παρακάτω τμήματα Περιορίστε κοινές [έννοιες δέσμευση Mobile](mobile-engagement-concepts.md) για την πλατφόρμα στο web.

### <a name="session-and-activity"></a>`Session`και`Activity`

Εάν ο χρήστης παραμένει αδρανής για περισσότερα από μερικά δευτερόλεπτα μεταξύ των δύο δραστηριότητες, ο χρήστης ακολουθία δραστηριοτήτων χωρίζεται σε δύο ξεχωριστές περιόδους λειτουργίας. Αυτά τα μερικά δευτερόλεπτα ονομάζονται το χρονικό όριο περιόδου λειτουργίας.

Εάν η εφαρμογή web δεν δηλώσετε στο τέλος της δραστηριότητες χρηστών μόνη (καλώντας την `engagement.agent.endActivity` συνάρτηση), ο διακομιστής δέσμευση Mobile λήγει αυτόματα την περίοδο λειτουργίας χρήστη μέσα σε τρία λεπτά μετά από τη σελίδα της εφαρμογής είναι κλειστό. Αυτή η διαδικασία ονομάζεται το χρονικό όριο περιόδου λειτουργίας διακομιστή.

### `Crash`

Αυτοματοποιημένη αναφορές μη καταγεγραμμένο εξαιρέσεων JavaScript δεν δημιουργούνται από προεπιλογή. Ωστόσο, μπορείτε να αναφέρετε παρουσιάσει σφάλμα με μη αυτόματο τρόπο, χρησιμοποιώντας το `sendCrash` συνάρτηση (ανατρέξτε στην ενότητα στην αναφορά παρουσιάζει σφάλμα).

## <a name="reporting-activities"></a>Αναφορά δραστηριοτήτων

Δημιουργία αναφορών σχετικά με τη δραστηριότητα χρήστη περιλαμβάνει όταν ένας χρήστης ξεκινά μια νέα δραστηριότητα και, όταν ο χρήστης λήξει η τρέχουσα δραστηριότητα.

### <a name="user-starts-a-new-activity"></a>Χρήστης ξεκινά μια νέα δραστηριότητα

    engagement.agent.startActivity("MyUserActivity");

Πρέπει να καλέσετε `startActivity()` αλλάζει κάθε φορά δραστηριότητας χρήστη. Η πρώτη κλήση σε αυτήν τη συνάρτηση ξεκινά μια νέα περίοδο λειτουργίας χρήστη.

### <a name="user-ends-the-current-activity"></a>Χρήστη λήξει η τρέχουσα δραστηριότητα

    engagement.agent.endActivity();

Πρέπει να καλέσετε `endActivity()` τουλάχιστον μία φορά όταν ο χρήστης ολοκληρωθεί η τελευταία δραστηριότητές τους. Αυτό ενημερώνει το SDK Mobile δέσμευση Web ότι ο χρήστης είναι αυτήν τη στιγμή αδράνειας και ότι η περίοδος λειτουργίας χρήστη πρέπει να είναι κλειστά αφού λήξει το χρονικό όριο περιόδου λειτουργίας. Αν καλέσετε `startActivity()` πριν λήξει το χρονικό όριο περιόδου λειτουργίας, απλώς πριν αρχίσει την περίοδο λειτουργίας.

Επειδή δεν υπάρχει καμία αξιόπιστη κλήση για κατά το κλείσιμο του παραθύρου περιήγησης, είναι συχνά δύσκολη ή αδύνατη για να ενημερωθείτε στο τέλος της δραστηριότητες χρηστών μέσα σε ένα περιβάλλον web. Που είναι γιατί ο διακομιστής δέσμευση Mobile λήγει αυτόματα την περίοδο λειτουργίας χρήστη μέσα σε τρία λεπτά μετά από τη σελίδα της εφαρμογής είναι κλειστό.

## <a name="reporting-events"></a>Δημιουργία αναφορών συμβάντα

Δημιουργία αναφορών σχετικά με τα συμβάντα καλύπτει τα συμβάντα περιόδου λειτουργίας και συμβάντα μεμονωμένη.

### <a name="session-events"></a>Συμβάντα περιόδου λειτουργίας

Συμβάντα περιόδου λειτουργίας συνήθως χρησιμοποιούνται για την αναφορά τις ενέργειες που εκτελούνται από το χρήστη κατά την περίοδο λειτουργίας του χρήστη.

**Παράδειγμα χωρίς επιπλέον δεδομένα:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Παράδειγμα με επιπλέον δεδομένα:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Μεμονωμένη συμβάντα

Σε αντίθεση με τα συμβάντα περιόδου λειτουργίας, μπορεί να προκύψουν συμβάντα μεμονωμένη έξω από το περιβάλλον μιας περιόδου λειτουργίας.

Για αυτό, χρησιμοποιήστε ``engagement.agent.sendEvent`` αντί για ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Αναφορά σφαλμάτων

Δημιουργία αναφορών σχετικά με τα σφάλματα καλύπτει σφάλματα περιόδου λειτουργίας και μεμονωμένη σφάλματα.

### <a name="session-errors"></a>Σφάλματα περιόδου λειτουργίας

Σφάλματα περιόδου λειτουργίας συνήθως χρησιμοποιούνται για την αναφορά των σφαλμάτων που έχουν επιπτώσεις στο χρήστη κατά την περίοδο λειτουργίας του χρήστη.

**Παράδειγμα χωρίς επιπλέον δεδομένα:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Παράδειγμα με επιπλέον δεδομένα:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Μεμονωμένη σφάλματα

Σε αντίθεση με την περίοδο λειτουργίας σφάλματα, μεμονωμένη σφάλματα μπορεί να προκύψουν έξω από το περιβάλλον μιας περιόδου λειτουργίας.

Για αυτό, χρησιμοποιήστε `engagement.agent.sendError` αντί για `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Αναφορά εργασιών

Δημιουργία αναφορών σε εργασίες εξώφυλλα αναφοράς σφαλμάτων και συμβάντων που προκύπτουν κατά τη διάρκεια μιας εργασίας και δημιουργία αναφορών παρουσιάσει σφάλμα.

**Παράδειγμα:**

Εάν θέλετε να παρακολουθείτε μια αίτηση AJAX, πρέπει να χρησιμοποιήσετε τα εξής:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a>Αναφορά σφαλμάτων κατά τη διάρκεια μιας εργασίας

Σφάλματα μπορούν να συσχετιστούν με μια εργασία που εκτελείται αντί για την τρέχουσα περίοδο λειτουργίας του χρήστη.

**Παράδειγμα:**

Εάν θέλετε να αναφέρετε ένα σφάλμα εάν αποτύχει μια αίτηση AJAX:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a>Δημιουργία αναφορών συμβάντα κατά τη διάρκεια μιας εργασίας

Συμβάντα μπορούν να συσχετιστούν με μια εργασία που εκτελείται αντί για την τρέχουσα περίοδο λειτουργίας του χρήστη, ευχαριστώντας το `engagement.agent.sendJobEvent` συνάρτηση.

Αυτή η συνάρτηση λειτουργεί ακριβώς όπως `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Δημιουργία αναφορών παρουσιάζει σφάλμα

Χρησιμοποιήστε το `sendCrash` συνάρτηση αναφορά παρουσιάζει σφάλμα με μη αυτόματο τρόπο.

Το `crashid` όρισμα είναι μια συμβολοσειρά που προσδιορίζει τον τύπο της σφάλμα.
Το `crash` όρισμα είναι συνήθως η παρακολούθηση στοίβας της το σφάλμα ως συμβολοσειρά.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>Επιπλέον παράμετροι

Μπορείτε να επισυνάψετε αυθαίρετο δεδομένων σε ένα συμβάν, σφάλματος, δραστηριότητας ή εργασία.

Τα δεδομένα μπορεί να είναι οποιοδήποτε αντικείμενο JSON (αλλά όχι έναν πίνακα ή στοιχειώδη τύπο).

**Παράδειγμα:**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Όρια

Είναι τα όρια που ισχύουν για επιπλέον παραμέτρους στις περιοχές των κανονικών παραστάσεων για αριθμούς-κλειδιά, τύποι τιμή και το μέγεθος.

#### <a name="keys"></a>Πλήκτρα

Κάθε κλειδί στο αντικείμενο πρέπει να συμφωνεί με την ακόλουθη κανονική έκφραση:

    ^[a-zA-Z][a-zA-Z_0-9]*

Αυτό σημαίνει ότι πλήκτρα πρέπει να ξεκινούν με γράμμα τουλάχιστον μία, ακολουθούμενο από γράμματα, ψηφία ή χαρακτήρες υπογράμμισης (\_).

#### <a name="values"></a>Τιμές

Τιμές περιορίζονται σε συμβολοσειρά, αριθμός και δυαδική τύπους.

#### <a name="size"></a>Μέγεθος

Πρόσθετα περιορίζονται 1.024 χαρακτήρες ανά κλήση (μετά το SDK Mobile δέσμευση Web κωδικοποιεί το στο JSON).

## <a name="reporting-application-information"></a>Αναφορά πληροφοριών εφαρμογής

Να αναφέρετε με μη αυτόματο τρόπο την παρακολούθηση πληροφοριών (ή οποιαδήποτε άλλη πληροφορία συγκεκριμένη εφαρμογή), χρησιμοποιώντας το `sendAppInfo()` συνάρτηση.

Σημειώστε ότι αυτές οι πληροφορίες μπορούν να αποσταλούν σταδιακά. Μόνο η τελευταία τιμή για έναν συγκεκριμένο αριθμό-κλειδί θα διατηρηθούν για μια συγκεκριμένη συσκευή.

Όπως πρόσθετα του συμβάντος, μπορείτε να χρησιμοποιήσετε οποιοδήποτε αντικείμενο JSON για συνοπτική πληροφορίες εφαρμογής. Σημειώστε ότι πίνακες ή δευτερεύουσες αντικείμενα αντιμετωπίζονται ως επίπεδο συμβολοσειρές (χρησιμοποιώντας σειριοποίησης JSON).

**Παράδειγμα:**

Ακολουθεί ένα δείγμα κώδικα για την αποστολή του χρήστη φύλο και ημερομηνία γέννησης:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Όρια

Είναι τα όρια που ισχύουν για τις πληροφορίες της εφαρμογής στις περιοχές των κανονικών παραστάσεων για αριθμούς-κλειδιά και μέγεθος.

#### <a name="keys"></a>Πλήκτρα

Κάθε κλειδί στο αντικείμενο πρέπει να συμφωνεί με την ακόλουθη κανονική έκφραση:

    ^[a-zA-Z][a-zA-Z_0-9]*

Αυτό σημαίνει ότι πλήκτρα πρέπει να ξεκινούν με γράμμα τουλάχιστον μία, ακολουθούμενο από γράμματα, ψηφία ή χαρακτήρες υπογράμμισης (\_).

#### <a name="size"></a>Μέγεθος

Πληροφορίες εφαρμογής περιορίζεται σε 1.024 χαρακτήρες ανά κλήση (μετά το SDK Mobile δέσμευση Web κωδικοποιεί το στο JSON).

Στο προηγούμενο παράδειγμα, το JSON αποστέλλεται στο διακομιστή είναι 44 χαρακτήρες:

    {"birthdate":"1983-12-07","gender":"female"}
