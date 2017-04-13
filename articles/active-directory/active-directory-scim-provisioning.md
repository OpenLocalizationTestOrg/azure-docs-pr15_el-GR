<properties
    pageTitle="Χρήση SCIM για να ενεργοποιήσετε την αυτόματη προμήθεια των χρηστών και ομάδων από το Azure Active Directory για τις εφαρμογές του | Microsoft Azure"
    description="Azure Active Directory μπορούν να προμηθεύσουν αυτόματα χρήστες και ομάδες σε οποιαδήποτε εφαρμογή ή ταυτότητα χώρου αποθήκευσης που είναι fronted από μια υπηρεσία Web με το περιβάλλον εργασίας που ορίζονται στην προδιαγραφή πρωτοκόλλου SCIM"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="using-scim-to-enable-automatic-provisioning-of-users-and-groups-from-azure-active-directory-to-applications"></a>Χρήση SCIM για να ενεργοποιήσετε την αυτόματη προμήθεια των χρηστών και ομάδων από το Azure Active Directory για τις εφαρμογές

##<a name="overview"></a>Επισκόπηση

Azure Active Directory μπορούν να προμηθεύσουν αυτόματα χρήστες και ομάδες σε οποιαδήποτε εφαρμογή ή ταυτότητα χώρου αποθήκευσης που είναι fronted από μια υπηρεσία Web με το περιβάλλον εργασίας που ορίζονται στην [προδιαγραφή πρωτοκόλλου SCIM 2.0](https://tools.ietf.org/html/draft-ietf-scim-api-19). Azure Active Directory να στείλετε προσκλήσεις για δημιουργία, τροποποίηση και διαγραφή που του έχουν ανατεθεί χρήστες και ομάδες σε αυτήν την υπηρεσία Web, που, στη συνέχεια, να μπορούν να μεταφράσουν αυτές τις προσκλήσεις σε λειτουργίες κατά το χώρο αποθήκευσης ταυτότητα προορισμού. 

![][1]
*Εικόνα: Προμήθεια από το Azure Active Directory στο κατάστημα ταυτότητας μέσω μιας υπηρεσίας Web*

Αυτή η δυνατότητα μπορεί να χρησιμοποιηθεί σε συνδυασμό με τη δυνατότητα "[μεταφορά τη δική σας εφαρμογή](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)" στο Azure AD για ενεργοποίηση της καθολικής σύνδεσης και αυτόματη χρήστη προμήθεια για εφαρμογές που παρέχουν ή είναι fronted από μια υπηρεσία web SCIM.

Υπάρχουν δύο περιπτώσεις χρήσης για SCIM στο Azure Active Directory:

* **Προμήθεια χρήστες και ομάδες για τις εφαρμογές που υποστηρίζουν SCIM** - εφαρμογές που υποστηρίζουν SCIM 2.0 και χρησιμοποιήστε τα διακριτικά φορέα OAuth για τον έλεγχο ταυτότητας θα λειτουργούν με το Azure AD του πλαισίου.

* **Δημιουργήστε το δικό σας παροχής λύσεων για τις εφαρμογές που υποστηρίζουν άλλες προμήθεια βάσει API** - για μη SCIM εφαρμογές, μπορείτε να δημιουργήσετε ένα τελικό σημείο SCIM για να μεταφράσετε μεταξύ Azure AD SCIM τελικού σημείου και ό, τι υποστηρίζει το API της εφαρμογής για την προμήθεια του χρήστη.  Για βοήθεια στην ανάπτυξη του ένα τελικό σημείο SCIM, παρέχουμε βιβλιοθήκες CLI μαζί με δείγματα κώδικα που σας δείχνουν πώς μπορείτε να δώσετε ένα τελικό σημείο SCIM και μετάφραση SCIM μηνύματα.  

##<a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Προμήθεια χρήστες και ομάδες για τις εφαρμογές που υποστηρίζουν SCIM

Azure Active Directory μπορούν να ρυθμιστούν για αυτόματη προμήθεια στους οποίους έχουν ανατεθεί στους χρήστες και ομάδες για τις εφαρμογές που υλοποίηση [συστήματος για το 2 (SCIM) η Διαχείριση ταυτοτήτων μεταξύ τομέων](https://tools.ietf.org/html/draft-ietf-scim-api-19) Web υπηρεσίας και αποδοχή διακριτικά φορέα OAuth για τον έλεγχο ταυτότητας. Εντός της προδιαγραφής SCIM 2.0, εφαρμογές πρέπει να πληροί αυτές τις απαιτήσεις:

* Υποστηρίζει τη δημιουργία χρηστών ή/και ομάδες, σύμφωνα με την ενότητα 3.3 του πρωτοκόλλου SCIM.  

* Υποστηρίζει την τροποποίηση χρήστες ή/και ομάδες με την ενημέρωση κώδικα αιτήσεις σύμφωνα με την ενότητα 3.5.2 του πρωτοκόλλου SCIM.  

* Υποστηρίζει την ανάκτηση ενός πόρου γνωστά σύμφωνα με την ενότητα 3.4.1 του πρωτοκόλλου SCIM.  

*  Υποστηρίζει την υποβολή ερωτημάτων χρήστες ή/και ομάδες, σύμφωνα με την ενότητα 3.4.2 του πρωτοκόλλου SCIM.  Από προεπιλογή, οι χρήστες έχουν αναζητηθούν με externalId και ομάδες είναι αναζητηθούν με εμφανιζόμενο όνομα.  

* Υποστηρίζει την υποβολή ερωτημάτων χρήστη κατά Αναγνωριστικό και με τη Διαχείριση σύμφωνα με την ενότητα 3.4.2 του πρωτοκόλλου SCIM.  

* Υποστηρίζει την υποβολή ερωτημάτων ομάδες κατά Αναγνωριστικό και από μέλος σύμφωνα με την ενότητα 3.4.2 του πρωτοκόλλου SCIM.  

* Αποδέχεται διακριτικά φορέα OAuth για την έγκριση σύμφωνα με την ενότητα 2.1 το πρωτόκολλο SCIM.

Πρέπει να ελέγξετε με την υπηρεσία παροχής εφαρμογής ή της υπηρεσίας παροχής εφαρμογή τεκμηρίωση για δηλώσεις συμβατότητας με αυτές τις απαιτήσεις.
 
###<a name="getting-started"></a>Γρήγορα αποτελέσματα

Εφαρμογές που υποστηρίζουν το προφίλ SCIM που περιγράφεται παραπάνω μπορεί να συνδεθεί με Azure Active Directory χρησιμοποιώντας τη δυνατότητα "Προσαρμογή" εφαρμογή στη συλλογή εφαρμογών Azure AD. Μετά τη σύνδεση, Azure AD εκτελείται μια διαδικασία συγχρονισμού κάθε 5 λεπτά, όπου το τελικό σημείο SCIM της εφαρμογής για χρήστες που έχουν ανατεθεί και ομάδες, τα ερωτήματα και δημιουργεί ή τροποποιεί τους σύμφωνα με τις λεπτομέρειες της ανάθεσης.

**Για να συνδεθείτε μια εφαρμογή που υποστηρίζει SCIM:**

1.  Σε ένα πρόγραμμα περιήγησης web, ξεκινήστε την πύλη διαχείρισης Azure στο https://manage.windowsazure.com.
2.  Αναζήτηση για να **υπηρεσίας καταλόγου Active Directory > καταλόγου > [σας κατάλογος] > εφαρμογές**, και επιλέξτε **Προσθήκη > Προσθήκη εφαρμογής από τη συλλογή**.
3.  Επιλέξτε την καρτέλα **προσαρμοσμένη** στα αριστερά, πληκτρολογήστε ένα όνομα για την εφαρμογή σας και κάντε κλικ στο εικονίδιο σημάδι ελέγχου για να δημιουργήσετε ένα αντικείμενο εφαρμογής.

![][2]

4.  Στην οθόνη που προκύπτει, επιλέξτε το δεύτερο κουμπί **Ρύθμιση παραμέτρων λογαριασμού προμήθειας** .
5.  Στο πεδίο **Διεύθυνση URL προμήθεια του τελικού σημείου** , πληκτρολογήστε τη διεύθυνση URL του τελικού σημείου SCIM της εφαρμογής.
6.  Εάν το τελικό σημείο SCIM απαιτεί ένα διακριτικό φορέα OAuth από έναν εκδότη εκτός από το Azure AD, στη συνέχεια, αντιγράψτε το απαιτούμενο διακριτικό φορέα OAuth στο πεδίο **Ελέγχου ταυτότητας διακριτικού (προαιρετικό)** . Είναι αυτό το πεδίο είναι κενή και, στη συνέχεια, Azure AD θα περιλαμβάνει ένα διακριτικό φορέα OAuth εκδοθεί από το Azure AD με κάθε αίτηση. Εφαρμογές που χρησιμοποιούν Azure AD, όπως μια υπηρεσία παροχής idenity να επικυρώσετε αυτό Azure AD-εκδοθεί διακριτικού.
7.  Κάντε κλικ στο κουμπί **Επόμενο**και κάντε κλικ στο κουμπί **Εκκίνηση δοκιμής** να έχουν Azure Active Directory, προσπαθήστε να συνδεθείτε με το τελικό σημείο SCIM. Εάν αποτύχει η προσπάθειες, θα εμφανιστεί διαγνωστικών πληροφοριών.  
8.  Εάν το επιχειρεί να συνδεθείτε με επιτυχία την εφαρμογή, στη συνέχεια, κλικ στο κουμπί **Επόμενο** το υπόλοιπο οθόνες και κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να κλείσετε το παράθυρο διαλόγου.
9.  Στην οθόνη που προκύπτει, επιλέξτε το τρίτο κουμπί **Εκχώρηση λογαριασμών** . Της ενότητας που προκύπτει χρήστες και ομάδες, αντιστοιχίστε τους χρήστες ή τις ομάδες που θέλετε για την παροχή των στην εφαρμογή.
10. Όταν αντιστοιχίζονται χρήστες και ομάδες, κάντε κλικ στην καρτέλα **Ρύθμιση παραμέτρων** κοντά στο επάνω μέρος της οθόνης.
11. Στην περιοχή **Λογαριασμός προμήθειας**, επιβεβαιώστε ότι η κατάσταση έχει οριστεί σε ΕΝΕΡΓΟ. 
12. Στην περιοχή **Εργαλεία**, κάντε κλικ στην επιλογή **επανεκκινήστε την προμήθεια του λογαριασμού** για να kick-start της διαδικασίας προετοιμασίας.

Σημειώστε ότι μπορεί να περάσει 5-10 λεπτά πριν θα ξεκινήσει η διαδικασία προετοιμασίας για την αποστολή προσκλήσεων για το τελικό σημείο SCIM.  Παρέχεται μια σύνοψη των προσπαθειών σύνδεσης στην καρτέλα πίνακα εργαλείων της εφαρμογής και μια αναφορά δραστηριότητας παροχής και παροχής σφάλματα μπορούν να ληφθούν από τον κατάλογο καρτέλα αναφορές.

##<a name="building-your-own-provisioning-solution-for-any-application"></a>Δημιουργία δικό προμήθεια λύσης για οποιαδήποτε εφαρμογή σας

Δημιουργώντας μια υπηρεσία web SCIM που διασυνδέσεις με Azure Active Directory, μπορείτε να ενεργοποιήσετε μεμονωμένου χρήστη καθολικής σύνδεσης και ο αυτόματος προμήθεια για σχεδόν οποιαδήποτε εφαρμογή που παρέχει το ΥΠΌΛΟΙΠΟ ή SOAP χρήστη προμήθεια API.

Δείτε τώρα πώς λειτουργεί:

1.  Azure AD παρέχει μια κοινή βιβλιοθήκη υποδομή γλώσσα με το όνομα [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Υπευθύνων ολοκλήρωσης καθολικών συστημάτων και τους προγραμματιστές να χρησιμοποιήσετε αυτήν τη βιβλιοθήκη για να δημιουργήσετε και να αναπτύξετε ένα web βάσει SCIM τελικού σημείου υπηρεσίας με δυνατότητα σύνδεσης Azure AD σε οποιαδήποτε εφαρμογή ταυτότητας store.
2.  Αντιστοιχίσεις σχετικά με την εφαρμογή της υπηρεσίας web για να αντιστοιχίσετε το σχήμα τυποποιημένη χρήστη για το πρωτόκολλο που απαιτούνται από την εφαρμογή και σχήμα χρήστη.
3.  Η διεύθυνση URL τελικού σημείου είναι καταχωρημένος στο Azure AD ως μέρος μια προσαρμοσμένη εφαρμογή στη συλλογή εφαρμογών.
4.  Χρήστες και ομάδες έχουν εκχωρηθεί σε αυτήν την εφαρμογή στο Azure AD. Κατά την ανάθεση, που τίθενται σε μια ουρά για να συγχρονιστεί με την εφαρμογή προορισμού. Η διαδικασία συγχρονισμού χειρισμός ουρά εκτελείται κάθε 5 λεπτά.

###<a name="code-samples"></a>Δείγματα κώδικα

Για να διευκολύνετε τη διαδικασία, ένα σύνολο [δείγματα κώδικα](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) που υπό την προϋπόθεση ότι δημιουργήσετε ένα τελικό σημείο της υπηρεσίας web SCIM και δείχνουν αυτόματη παροχή. Ένα δείγμα είναι μια υπηρεσία παροχής που διατηρεί ένα αρχείο με γραμμές τιμές διαχωρισμένες με κόμματα που αντιπροσωπεύουν χρήστες και ομάδες.  Το άλλο είναι μια υπηρεσία παροχής που λειτουργεί στην υπηρεσία Amazon Web υπηρεσίες ταυτότητας και διαχείρισης της Access.  

**Προαπαιτούμενα στοιχεία**

* Visual Studio 2013 ή νεότερη έκδοση
* [Azure SDK για .NET](https://azure.microsoft.com/downloads/)
* Windows μηχάνημα που υποστηρίζει το πλαίσιο διαίρεσης 4,5 ASP.NET που θα χρησιμοποιηθεί ως το τελικό σημείο SCIM. Αυτός ο υπολογιστής πρέπει να είναι προσβάσιμα από το cloud
* [Συνδρομή Azure με μια δοκιμαστική έκδοση ή με την άδεια χρήσης έκδοση του Azure AD Premium](https://azure.microsoft.com/services/active-directory/)
* Το δείγμα Amazon AWS απαιτεί βιβλιοθήκες από το [Κιτ εργαλείων AWS για το Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html). Ανατρέξτε στο αρχείο README που περιλαμβάνεται με το δείγμα για περισσότερες λεπτομέρειες

###<a name="getting-started"></a>Γρήγορα αποτελέσματα

Ο ευκολότερος τρόπος για να υλοποιήσετε ένα τελικό σημείο SCIM που μπορεί να αποδεχτεί αιτήσεις προετοιμασίας από το Azure AD είναι να δημιουργήστε και αναπτύξτε το δείγμα κώδικα που εξάγει την προμήθεια του φακέλου χρηστών σε ένα αρχείο διαχωρισμένων με κόμματα (CSV).

**Για να δημιουργήσετε ένα τελικό σημείο SCIM δείγμα:**

1.  Κάντε λήψη του πακέτου δείγματα κώδικα στο [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2.  Αποσυμπιέστε το πακέτο και τοποθετήστε το στον υπολογιστή σας με Windows σε μια θέση όπως C:\AzureAD-BYOA-Provisioning-Samples\.
3.  Σε αυτόν το φάκελο, ξεκινήστε τη λύση FileProvisioningAgent στο Visual Studio.
4.  Επιλέξτε **Εργαλεία > Διαχείριση πακέτου βιβλιοθήκη > Κονσόλα διαχείρισης πακέτου**, και να εκτελέσει τις εντολές κάτω από το στοιχείο για το έργο FileProvisioningAgent για να επιλύσετε τις αναφορές λύση:

    Εγκατάσταση του πακέτου Microsoft.SystemForCrossDomainIdentityManagement εγκατάσταση-πακέτο πακέτο εγκατάστασης Microsoft.IdentityModel.Clients.ActiveDirectory πακέτο εγκατάστασης Microsoft.Owin.Diagnostics Microsoft.Owin.Host.SystemWeb

5.  Δημιουργήστε το έργο FileProvisioningAgent.
6.  Ξεκινήστε την εφαρμογή εντολών του Windows (ως διαχειριστής) και χρησιμοποιήστε την εντολή **cd** για να αλλάξετε τον κατάλογο στο φάκελο **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** .
7.  Εκτελέστε την εντολή παρακάτω, αντικαθιστώντας < διεύθυνση ip > με το IP ή το όνομα τομέα του ηλεκτρονικού υπολογιστή Windows.

    FileAgnt.exe http://<ip-address>:9000 TargetFile.csv

8.  Στα Windows, στην περιοχή **ρυθμίσεις των Windows > Ρυθμίσεις Internet & δικτύου**, επιλέξτε το **τείχος προστασίας των Windows > ρυθμίσεις για προχωρημένους**, και δημιουργήστε έναν **Κανόνα εισερχομένων** που επιτρέπει την εισερχόμενη πρόσβαση στη θύρα 9000.
9.  Εάν η μηχανή Windows είναι πίσω από ένα δρομολογητή, το δρομολογητή θα πρέπει να ρυθμιστεί για να εκτελέσετε μετάφραση πρόσβασης δικτύου μεταξύ τη θύρα 9000 που εκτίθεται στο Internet και τη θύρα 9000 στον υπολογιστή Windows. Αυτή είναι απαραίτητη για το Azure AD για να μπορέσετε να αποκτήσετε πρόσβαση σε αυτό το τελικό σημείο στο cloud.


**Για να καταχωρήσετε το τελικό σημείο SCIM δείγμα στο Azure AD:**

1.  Σε ένα πρόγραμμα περιήγησης web, ξεκινήστε την πύλη διαχείρισης Azure στο https://manage.windowsazure.com.
2.  Αναζήτηση για να **υπηρεσίας καταλόγου Active Directory > καταλόγου > [σας κατάλογος] > εφαρμογές**, και επιλέξτε **Προσθήκη > Προσθήκη εφαρμογής από τη συλλογή**.
3.  Επιλέξτε την καρτέλα **προσαρμοσμένη** στα αριστερά, πληκτρολογήστε ένα όνομα, όπως "SCIM δοκιμής εφαρμογής" και κάντε κλικ στο εικονίδιο σημάδι ελέγχου για να δημιουργήσετε ένα αντικείμενο εφαρμογής. Σημείωση που δημιουργήθηκε το αντικείμενο της εφαρμογής είναι σκοπεύετε να αναπαραστήσετε την εφαρμογή προορισμού που θα είχε προμήθεια να και υλοποίηση της καθολικής σύνδεσης για και όχι μόνο το τελικό σημείο SCIM.

![][2]

4.  Στην οθόνη που προκύπτει, επιλέξτε το δεύτερο κουμπί **Ρύθμιση παραμέτρων λογαριασμού προμήθειας** .
5.  Στο παράθυρο διαλόγου, εισαγάγετε τις εκτίθεται στο internet διεύθυνση URL και θύρα της το τελικό σημείο SCIM. Αυτό είναι κάτι όπως http://testmachine.contoso.com:9000 ή http://<ip-address>:9000/, όπου < διεύθυνση ip > είναι στο internet που εκτίθεται IP address.  
6.  Κάντε κλικ στο κουμπί **Επόμενο**και κάντε κλικ στο κουμπί **Εκκίνηση δοκιμής** να έχουν Azure Active Directory, προσπαθήστε να συνδεθείτε με το τελικό σημείο SCIM. Εάν αποτύχει η προσπάθειες, θα εμφανιστεί διαγνωστικών πληροφοριών.  
7.  Εάν το επιχειρεί να συνδεθείτε με την υπηρεσία Web ολοκληρωθεί με επιτυχία, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο** στις υπόλοιπες οθόνες, και κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να κλείσετε το παράθυρο διαλόγου.
8.  Στην οθόνη που προκύπτει, επιλέξτε το τρίτο κουμπί **Εκχώρηση λογαριασμών** . Της ενότητας που προκύπτει χρήστες και ομάδες, αντιστοιχίστε τους χρήστες ή τις ομάδες που θέλετε για την παροχή των στην εφαρμογή.
9.  Όταν αντιστοιχίζονται χρήστες και ομάδες, κάντε κλικ στην καρτέλα **Ρύθμιση παραμέτρων** κοντά στο επάνω μέρος της οθόνης.
10. Στην περιοχή **Λογαριασμός προμήθειας**, επιβεβαιώστε ότι η κατάσταση έχει οριστεί σε ΕΝΕΡΓΟ. 
11. Στην περιοχή **Εργαλεία**, κάντε κλικ στην επιλογή **επανεκκινήστε την προμήθεια του λογαριασμού** για να kick-start της διαδικασίας προετοιμασίας.

Σημειώστε ότι μπορεί να περάσει 5-10 λεπτά πριν θα ξεκινήσει η διαδικασία προετοιμασίας για την αποστολή προσκλήσεων για το τελικό σημείο SCIM.  Παρέχεται μια σύνοψη των προσπαθειών σύνδεσης στην καρτέλα πίνακα εργαλείων της εφαρμογής και μια αναφορά δραστηριότητας παροχής και παροχής σφάλματα μπορούν να ληφθούν από τον κατάλογο καρτέλα αναφορές.

Το τελικό βήμα επαληθεύετε το δείγμα είναι να ανοίξετε το αρχείο TargetFile.csv στο φάκελο \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug στον υπολογιστή σας με Windows. Αφού εκτελέσετε τη διαδικασία προετοιμασίας, αυτό το αρχείο εμφανίζει όλες τις λεπτομέρειες που έχουν εκχωρηθεί και παρασχεθεί χρήστες και ομάδες.

###<a name="development-libraries"></a>Βιβλιοθήκες ανάπτυξης

Για να αναπτύξετε τη δική σας υπηρεσία Web που συμμορφώνεται με την προδιαγραφή SCIM, πρώτα εξοικειωθείτε με τις εξής βιβλιοθήκες που παρέχεται από τη Microsoft για να επιταχύνετε τη διαδικασία ανάπτυξης: 

**1:**  Κοινές βιβλιοθήκες υποδομή γλώσσας παρέχεται για χρήση με γλώσσες που βασίζονται σε αυτήν την υποδομή, όπως C#.  Μία από αυτές τις βιβλιοθήκες, [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), δηλώνει ένα περιβάλλον εργασίας, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, φαίνεται στην παρακάτω εικόνα.  Ένας προγραμματιστής χρησιμοποιώντας τις βιβλιοθήκες θα υλοποιήστε αυτήν τη διασύνδεση με κλάση που ενδέχεται να αναφέρεται, Γενικά, ως μια υπηρεσία παροχής.  Ο προγραμματιστής για την ανάπτυξη εύκολα μια υπηρεσία Web που συμφωνεί με την προδιαγραφή SCIM ενεργοποιήσετε τις βιβλιοθήκες, είτε φιλοξενείται μέσα από το Internet Information Services ή οποιαδήποτε εκτελέσιμο συγκρότησης κοινές υποδομή γλώσσας.  Προσκλήσεις σε αυτή την υπηρεσία Web θα μετατρέπεται σε κλήσεις σε μεθόδους της υπηρεσίας παροχής, οι οποίες μπορεί να έχει προγραμματιστεί από τον προγραμματιστή για λειτουργούν σε ορισμένες store ταυτότητα.    

![][3]

**2:** [Προγράμματα χειρισμού δρομολόγηση Express](http://expressjs.com/guide/routing.html) είναι διαθέσιμες για την ανάλυση node.js αίτηση αντικείμενα που αντιπροσωπεύει κλήσεις (όπως καθορίζεται από την προδιαγραφή SCIM), που έγιναν σε μια υπηρεσία Web node.js.     

###<a name="building-a-custom-scim-endpoint"></a>Δημιουργία ενός προσαρμοσμένου SCIM τελικού σημείου

Χρησιμοποιώντας τις βιβλιοθήκες που περιγράφεται παραπάνω, τους προγραμματιστές που χρησιμοποιούν αυτές τις βιβλιοθήκες να φιλοξενήσετε τις υπηρεσίες τους εντός οποιαδήποτε εκτελέσιμο συγκρότησης κοινές υποδομή γλώσσα ή εντός των υπηρεσιών Internet Information Services.  Ακολουθεί δείγμα κώδικα για τη φιλοξενία υπηρεσίας μέσα σε ένα εκτελέσιμο συγκρότησης, στη διεύθυνση http://localhost:9000: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  
    
    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Είναι σημαντικό να γνωρίζετε ότι αυτή η υπηρεσία πρέπει να έχει ένα HTTP διευθύνσεων και διακομιστή πιστοποιητικό ελέγχου ταυτότητας του οποίου η αρχή έκδοσης πιστοποιητικών ρίζας είναι ένα από τα εξής: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Πιστοποιητικό ελέγχου ταυτότητας διακομιστή μπορεί να συνδεθεί σε μια θύρα σε ένα κεντρικό υπολογιστή Windows χρησιμοποιώντας το βοηθητικό πρόγραμμα κελύφους δικτύου, ως εξής: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  
 
Εδώ, η τιμή που παρέχεται για το όρισμα Κατακερματισμός πιστοποιητικού είναι την αποτύπωση του πιστοποιητικού, ενώ η τιμή που παρέχεται για το όρισμα αναγνωριστικό εφαρμογής είναι ένα αυθαίρετο καθολικά μοναδικό αναγνωριστικό.  

Για τη φιλοξενία της υπηρεσίας εντός υπηρεσιών Internet Information Services, ο προγραμματιστής θα δημιουργήσετε μιας συγκρότησης βιβλιοθήκης κώδικα κοινές υποδομή γλώσσα με κλάση με το όνομα εκκίνησης σε ο προεπιλεγμένος χώρος ονομάτων της συγκρότησης.  Ακολουθεί ένα δείγμα μιας κλάσης όπως: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

###<a name="handling-endpoint-authentication"></a>Χειρισμός τελικό σημείο ελέγχου ταυτότητας

Οι αιτήσεις από το Azure Active Directory περιλαμβάνουν ένα διακριτικό φορέα OAuth 2.0.   Οποιαδήποτε υπηρεσία λαμβάνει την αίτηση πρέπει να ελέγχουν την ταυτότητα τον εκδότη ως εκ μέρους τον αναμενόμενο μισθωτή Azure Active Directory, για την πρόσβαση σε υπηρεσία Graph Web Azure Active Directory του Azure Active Directory.  Στο το διακριτικό, τον εκδότη προσδιορίζεται από μια απαίτηση iss, όπως "iss": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  Σε αυτό το παράδειγμα, η βασική διεύθυνση της τιμής διεκδίκηση, https://sts.windows.net, προσδιορίζει Azure Active Directory ως εκδότη, ενώ το τμήμα τη σχετική διεύθυνση, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, είναι ένα μοναδικό αναγνωριστικό του μισθωτή του Azure Active Directory για λογαριασμό του οποίου έχει εκδοθεί το διακριτικό.  Εάν το διακριτικό έχει εκδοθεί για την πρόσβαση σε υπηρεσία Graph Web το Azure Active Directory του, στη συνέχεια, το αναγνωριστικό της συγκεκριμένης υπηρεσίας, 00000002-0000-0000-c000-000000000000, πρέπει να έχει την τιμή του διεκδίκηση και το διακριτικό.  

Τους προγραμματιστές που χρησιμοποιούν τις βιβλιοθήκες κοινών υποδομή γλώσσας που παρέχεται από τη Microsoft για τη δημιουργία μιας υπηρεσίας SCIM μπορούν να ελέγχουν την ταυτότητα αιτήσεις από Azure Active Directory χρησιμοποιώντας το πακέτο Microsoft.Owin.Security.ActiveDirectory ακολουθώντας τα παρακάτω βήματα: 

**1:**  Σε μια υπηρεσία παροχής, υλοποίηση της ιδιότητας Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior να επιστρέψει μια μέθοδο για να ονομάζεται κάθε φορά που έχει ξεκινήσει η υπηρεσία: 

    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }

**2:**  Προσθέστε τον ακόλουθο κώδικα σε αυτή τη μέθοδο για να έχετε οποιαδήποτε αίτηση σε οποιαδήποτε από τα τελικά σημεία της υπηρεσίας έλεγχος ταυτότητας ως που φέρουν ενός διακριτικού εκδοθεί από Azure Active Directory εκ μέρους ενός καθορισμένου μισθωτή, για την πρόσβαση σε υπηρεσία Graph Web Azure Active Directory του: 

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }

##<a name="user-and-group-schema"></a>Χρήστη και ομάδα σχήματος

Azure Active Directory μπορούν να προμηθεύσουν δύο τύποι πόρων σε υπηρεσίες Web SCIM.  Αυτούς τους τύπους των πόρων είναι χρήστες και ομάδες.  

Πόροι χρήστη αναγνωρίζονται από το αναγνωριστικό του σχήματος, urn: ietf:params:scim:schemas:extension:enterprise:2.0:User, το οποίο περιλαμβάνεται σε αυτήν την προδιαγραφή πρωτόκολλο: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  Η προεπιλεγμένη αντιστοίχιση με τα χαρακτηριστικά των χρηστών στο Azure Active Directory για τα χαρακτηριστικά των πόρων urn: ietf:params:scim:schemas:extension:enterprise:2.0:User παρέχεται στον πίνακα 1, κάτω από το στοιχείο.  

Ομαδοποίηση πόρων αναγνωρίζονται από το αναγνωριστικό του σχήματος, http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Πίνακας 2, παρακάτω, εμφανίζει την προεπιλεγμένη αντιστοίχιση με τα χαρακτηριστικά των ομάδων στο Azure Active Directory για να τα χαρακτηριστικά http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group πόρων.  

###<a name="table-1-default-user-attribute-mapping"></a>Πίνακας 1: Προεπιλεγμένη αντιστοίχιση χαρακτηριστικού χρήστη

| Azure χρήστη υπηρεσίας καταλόγου Active Directory | urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| ------------- | ------------- |
| IsSoftDeleted | ενεργό |
| Εμφανιζόμενο όνομα | Εμφανιζόμενο όνομα |
| TelephoneNumber φαξ | .value phoneNumbers [τύπος eq "φαξ"] |
| givenName | name.givenName |
| Τίτλος εργασίας | Τίτλος |
| Αλληλογραφία | .value μηνύματα ηλεκτρονικού ταχυδρομείου [τύπος eq "εργασία"] |
| mailNickname | externalId |
| Διαχείριση | Διαχείριση |
| κινητές συσκευές | .value phoneNumbers [τύπος eq "κινητό"] |
| objectId | αναγνωριστικό |
| "Ταχυδρομικός κώδικας" | .postalCode διευθύνσεις [τύπος eq "εργασία"] |
| Διευθύνσεις διακομιστή μεσολάβησης | μηνύματα ηλεκτρονικού ταχυδρομείου [Πληκτρολογήστε eq "άλλα"]. Τιμή |
| φυσικά-παράδοσης-OfficeName | διευθύνσεις [Πληκτρολογήστε eq "άλλα"]. Μορφοποίηση |
| streetAddress | .streetAddress διευθύνσεις [τύπος eq "εργασία"] |
| Επώνυμο | name.familyName |
| Αριθμός τηλεφώνου | .value phoneNumbers [τύπος eq "εργασία"] |
| PrincipalName χρήστη | όνομα χρήστη |


###<a name="table-2-default-group-attribute-mapping"></a>Πίνακας 2: Προεπιλεγμένη ομαδοποίηση χαρακτηριστικό αντιστοίχισης

| Ομάδα Azure Active Directory | http://Schemas.Microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| ------------- | ------------- |
| Εμφανιζόμενο όνομα | externalId |
| Αλληλογραφία | .value μηνύματα ηλεκτρονικού ταχυδρομείου [τύπος eq "εργασία"] |
| mailNickname | Εμφανιζόμενο όνομα |
| τα μέλη | τα μέλη |
| objectId | αναγνωριστικό |
| proxyAddresses | μηνύματα ηλεκτρονικού ταχυδρομείου [Πληκτρολογήστε eq "άλλα"]. Τιμή |


##<a name="user-provisioning-and-de-provisioning"></a>Προμήθεια του χρήστη και καταργήστε την προμήθεια

Η εικόνα παρακάτω εμφανίζει τα μηνύματα που Azure Active Directory θα στείλετε σε μια υπηρεσία SCIM για τη διαχείριση του κύκλου ζωής ενός χρήστη σε μια άλλη ταυτότητα αποθηκεύουν.  Το διάγραμμα παρουσιάζει επίσης πώς υπηρεσίας SCIM υλοποιηθεί με τη χρήση των βιβλιοθηκών κοινές υποδομή γλώσσας που παρέχεται από τη Microsoft για τη δημιουργία υπηρεσιών αυτών μεταφράζεται αυτές τις προσκλήσεις σε κλήσεις προς τις μεθόδους από μια υπηρεσία παροχής.  

![][4]
*Εικόνα: Προμήθεια του χρήστη και καταργήστε την προμήθεια ακολουθίας*

**1:**  Azure Active Directory θα ερωτήματος την υπηρεσία για ένα χρήστη με μια τιμή του χαρακτηριστικού externalId που ταιριάζουν με την τιμή του χαρακτηριστικού mailNickname ενός χρήστη στην υπηρεσία καταλόγου Azure Active Directory.  Το ερώτημα θα εκφρασμένη αίτησης πρωτόκολλο μεταφοράς υπερκειμένου όπως αυτή, όπου jyoung είναι ένα δείγμα του ένα mailNickname ενός χρήστη στην υπηρεσία καταλόγου Azure Active Directory: 

    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...

Εάν η υπηρεσία δημιουργήθηκε χρησιμοποιώντας τις βιβλιοθήκες κοινών υποδομή γλώσσας που παρέχεται από τη Microsoft για την εφαρμογή υπηρεσιών SCIM, στη συνέχεια, την αίτηση θα μετατρέπεται σε μια κλήση για να τη μέθοδο του ερωτήματος από την υπηρεσία παροχής.  Ακολουθεί η υπογραφή του αυτή τη μέθοδο: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);

Ακολουθεί τον ορισμό του περιβάλλοντος εργασίας Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters: 

    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }
    
    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }

Στην περίπτωση το παραπάνω δείγμα ενός ερωτήματος για ένα χρήστη με μια δεδομένη τιμή για το χαρακτηριστικό externalId, οι τιμές των ορισμάτων που του μεταβιβάστηκε τη μέθοδο του ερωτήματος θα είναι ως εξής: 

* παράμετροι. AlternateFilters.Count: 1
* παράμετροι. AlternateFilters.ElementAt(0). AttributePath: "externalId"
* παράμετροι. AlternateFilters.ElementAt(0). ΤελεστήςΣύγκρισης: ComparisonOperator.Equals
* παράμετροι. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"
* correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. RequestId]" 

**2:**  Εάν δεν επιστρέφει την απάντηση σε ένα ερώτημα με την υπηρεσία για ένα χρήστη με μια τιμή του χαρακτηριστικού externalId που ταιριάζουν με την τιμή του χαρακτηριστικού mailNickname ενός χρήστη στην υπηρεσία καταλόγου Azure Active Directory τυχόν χρήστες, στη συνέχεια, Azure Active Directory θα ζητήσει ότι η υπηρεσία παροχή χρήστη αντιστοιχεί με εκείνη του Azure Active Directory.  Ακολουθεί ένα παράδειγμα της αίτησης: 

    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}

Οι βιβλιοθήκες κοινών υποδομή γλώσσας που παρέχεται από τη Microsoft για την εφαρμογή υπηρεσιών SCIM θα μετάφραση που ζητούν σε μια κλήση στη μέθοδο Create από την υπηρεσία παροχής που χρησιμοποιείτε.  Η μέθοδος Create περιλαμβάνει αυτής της υπογραφής: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);

Στην περίπτωση μια αίτηση για την παροχή του χρήστη, η τιμή του ορίσματος πόρων θα είναι μια παρουσία του Microsoft.SystemForCrossDomainIdentityManagement. Κλάση Core2EnterpriseUser, που ορίζονται από το στη βιβλιοθήκη Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  Εάν η αίτηση για την παροχή του χρήστη ολοκληρωθεί με επιτυχία και, στη συνέχεια, την υλοποίηση της μεθόδου αναμένεται να επιστρέψει μια παρουσία του το Microsoft.SystemForCrossDomainIdentityManagement. Κλάση Core2EnterpriseUser, με την τιμή της ιδιότητας αναγνωριστικό ορίστε το μοναδικό αναγνωριστικό του χρήστη που μόλις παρασχεθεί.  

**3:**  Για να ενημερώσετε ένα χρήστη γνωστό ότι υπάρχουν σε ένα χώρο αποθήκευσης ταυτότητας fronted από μια SCIM, θα συνεχίσετε Azure Active Directory ζητώντας την τρέχουσα κατάσταση αυτού του χρήστη από την υπηρεσία με μια αίτηση όπως το εξής: 

    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...

Σε μια υπηρεσία που δημιουργούνται χρησιμοποιώντας τις βιβλιοθήκες κοινών υποδομή γλώσσας που παρέχεται από τη Microsoft για την εφαρμογή υπηρεσιών SCIM, την αίτηση θα μετατρέπεται σε μια κλήση για να τη μέθοδο ανάκτηση από την υπηρεσία παροχής που χρησιμοποιείτε.  Ακολουθεί η υπογραφή της μεθόδου ανάκτηση: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);
    
    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }

Στην περίπτωση το παραπάνω παράδειγμα μιας αίτησης για να ανακτήσετε την τρέχουσα κατάσταση του χρήστη, οι τιμές από τις ιδιότητες του αντικειμένου που παρέχεται ως η τιμή του ορίσματος παράμετροι θα είναι ως εξής: 

* Αναγνωριστικό: "54D382A4-2050-4C03-94D1-E769F1D15682"
* SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

**4:**  Εάν ένα χαρακτηριστικό αναφορά είναι να ενημερωθούν, τότε Azure Active Directory θα ερωτήματος της υπηρεσίας για να προσδιορίσετε ή όχι τα την τρέχουσα τιμή του χαρακτηριστικού αναφορά στο χώρο αποθήκευσης ταυτότητας fronted από την υπηρεσία ήδη συμφωνεί με την τιμή του χαρακτηριστικού που στο Azure Active Directory.  Στην περίπτωση χρήστες, το χαρακτηριστικό μόνο από τις οποίες θα να αναζητηθούν την τρέχουσα τιμή με τον τρόπο αυτό είναι το χαρακτηριστικό manager.  Ακολουθεί ένα παράδειγμα μιας αίτησης για να προσδιορίσετε εάν το χαρακτηριστικό manager ενός αντικειμένου συγκεκριμένος χρήστης έχει αυτήν τη στιγμή μια συγκεκριμένη τιμή: 

    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...

Η τιμή της παραμέτρου ερωτήματος χαρακτηριστικά, αναγνωριστικό, υποδεικνύει που εάν υπάρχει ένα αντικείμενο χρήστη που ικανοποιεί την παράσταση που παρέχεται ως η τιμή της παραμέτρου ερωτήματος φίλτρο και, στη συνέχεια, την υπηρεσία Αναμένεται απάντηση με έναν πόρο urn: ietf:params:scim:schemas:core:2.0:User ή urn: ietf:params:scim:schemas:extension:enterprise:2.0:User, συμπεριλαμβανομένων των μόνο η τιμή του χαρακτηριστικού id αυτόν τον πόρο.  Φυσικά, η τιμή του χαρακτηριστικού αναγνωριστικού είναι γνωστό ότι αιτούντα — που συμπεριλαμβάνεται στην την τιμή της παραμέτρου ερωτήματος φίλτρο; ο σκοπός της που σας ρωτά για αυτό είναι στην πραγματικότητα για να ζητήσετε μια ελάχιστη αναπαράσταση ενός πόρου που πληρούν η παράσταση φίλτρου ως ένδειξη της ή όχι υπάρχει τέτοιο οποιοδήποτε αντικείμενο.   

Εάν η υπηρεσία δημιουργήθηκε χρησιμοποιώντας τις βιβλιοθήκες κοινών υποδομή γλώσσας που παρέχεται από τη Microsoft για την εφαρμογή υπηρεσιών SCIM, στη συνέχεια, την αίτηση θα μετατρέπεται σε μια κλήση για να τη μέθοδο του ερωτήματος από την υπηρεσία παροχής.  Η τιμή από τις ιδιότητες του αντικειμένου που παρέχεται ως η τιμή του ορίσματος παράμετροι θα είναι ως εξής: 

* παράμετροι. AlternateFilters.Count: 2
* παράμετροι. AlternateFilters.ElementAt(x). AttributePath: "αναγνωριστικό"
* παράμετροι. AlternateFilters.ElementAt(x). ΤελεστήςΣύγκρισης: ComparisonOperator.Equals
* παράμετροι. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
* παράμετροι. AlternateFilters.ElementAt(y). AttributePath: "Διαχείριση"
* παράμετροι. AlternateFilters.ElementAt(y). ΤελεστήςΣύγκρισης: ComparisonOperator.Equals
* παράμετροι. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
* παράμετροι. RequestedAttributePaths.ElementAt(0): "αναγνωριστικό"
* παράμετροι. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

Εδώ, η τιμή του x ευρετηρίου μπορεί να είναι 0 και την τιμή του y ευρετηρίου μπορεί να είναι 1, ή η τιμή του x μπορεί να είναι 1 και η τιμή του y μπορεί να είναι 0, ανάλογα με τη σειρά των παραστάσεων της παραμέτρου ερωτήματος φίλτρου.   

**5:**  Ακολουθεί ένα παράδειγμα μιας αίτησης από το Azure Active Directory σε μια υπηρεσία SCIM για να ενημερώσετε ένα χρήστη: 

    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}

Οι βιβλιοθήκες Microsoft κοινές υποδομή γλώσσα για την εφαρμογή υπηρεσιών SCIM θα μετάφραση στην πρόσκληση σε μια κλήση για να τη μέθοδο ενημέρωση της υπηρεσίας παροχής.  Ακολουθεί η υπογραφή του αυτή τη μέθοδο: 

    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }
    
    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }
      
      public string Value
      { get; set; }
    }



Στην περίπτωση το παραπάνω παράδειγμα μιας αίτησης για να ενημερώσετε ένα χρήστη, το αντικείμενο που παρέχεται ως η τιμή του ορίσματος της ενημέρωσης κώδικα θα έχει αυτές τις τιμές ιδιοτήτων: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
* (PatchRequest ως PatchRequest2). Operations.Count: 1
* (PatchRequest ως PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add
* (PatchRequest ως PatchRequest2). Operations.ElementAt(0). Path.AttributePath: "Διαχείριση"
* (PatchRequest ως PatchRequest2). Operations.ElementAt(0). Value.Count: 1
* (PatchRequest ως PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Αναφορά: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
* (PatchRequest ως PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Τιμή: 2819c223-7f76-453a-919d-413861904646

**6:**  Για να καταργήστε την παροχή του χρήστη από ένα κατάστημα ταυτότητας fronted από μια υπηρεσία SCIM, Azure Active Directory θα στείλετε μια αίτηση όπως το εξής: 

    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    
Εάν η υπηρεσία δημιουργήθηκε χρησιμοποιώντας τις βιβλιοθήκες κοινών υποδομή γλώσσας που παρέχεται από τη Microsoft για την εφαρμογή υπηρεσιών SCIM, την αίτηση θα μετατρέπεται σε μια κλήση για να τη μέθοδο Delete από την υπηρεσία παροχής που χρησιμοποιείτε.   Αυτή η μέθοδος έχει αυτής της υπογραφής: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
 
Το αντικείμενο που παρέχεται ως η τιμή του ορίσματος resourceIdentifier να χρησιμοποιείτε αυτές τις τιμές ιδιοτήτων για το παραπάνω παράδειγμα μιας αίτησης για να καταργήστε την παροχή του χρήστη: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

##<a name="group-provisioning-and-de-provisioning"></a>Ομάδα προμήθειας και καταργήστε την προμήθεια

Η εικόνα παρακάτω εμφανίζει τα μηνύματα που Azure Active Directory θα στείλετε σε μια υπηρεσία SCIM για τη διαχείριση του κύκλου ζωής μιας ομάδας σε μια άλλη ταυτότητα αποθηκεύουν.  Αυτά τα μηνύματα διαφέρουν από τα μηνύματα που αφορούν τους χρήστες με τρεις τρόπους: 

* Το σχήμα ενός πόρου ομάδας θα αναγνωρίζονται ως http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  
* Αιτήσεις για την ανάκτηση ομάδες θα ορίζουν ότι το χαρακτηριστικό τα μέλη να εξαιρούνται από οποιονδήποτε πόρο που παρέχεται στην απόκριση στην αίτηση.  
* Αιτήσεις για να καθορίσετε εάν ένα χαρακτηριστικό αναφορά έχει μια συγκεκριμένη τιμή θα αιτήσεις σχετικά με το χαρακτηριστικό μέλη.  

![][5]
*Εικόνα: Ομάδα προμήθεια και καταργήστε την προμήθεια ακολουθίας*

##<a name="related-articles"></a>Σχετικά άρθρα

- [Το άρθρο ευρετηρίου για Διαχείριση εφαρμογών υπηρεσίας καταλόγου Azure Active Directory](active-directory-apps-index.md)
- [Αυτοματοποίηση χρήστη προμήθεια/Deprovisioning ΑΔΑ εφαρμογές](active-directory-saas-app-provisioning.md)
- [Προσαρμογή αντιστοιχίσεις χαρακτηριστικών για την προμήθεια του χρήστη](active-directory-saas-customizing-attribute-mappings.md)
- [Σύνταξη εκφράσεων για αντιστοιχίσεις χαρακτηριστικών](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Εμβέλεια φίλτρα για την προμήθεια του χρήστη](active-directory-saas-scoping-filters.md)
- [Λογαριασμού προμήθεια ειδοποιήσεων](active-directory-saas-account-provisioning-notifications.md)
- [Λίστα προγραμμάτων εκμάθησης σχετικά με τον τρόπο για να ενσωματώσετε ΑΔΑ εφαρμογές](active-directory-saas-tutorial-list.md)


    
<!--Image references-->
[1]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
