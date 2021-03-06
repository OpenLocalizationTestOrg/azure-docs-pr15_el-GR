<properties
    pageTitle="Διαχείριση της πρόσβασης στο αρχείο καταγραφής ανάλυσης | Microsoft Azure"
    description="Διαχείριση της πρόσβασης στο αρχείο καταγραφής αναλυτικών στοιχείων χρησιμοποιώντας μια ποικιλία εργασίες διαχείρισης χρηστών, λογαριασμούς, OMS χώρων εργασίας και λογαριασμοί Azure."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="banders"/>

# <a name="manage-access-to-log-analytics"></a>Διαχείριση της πρόσβασης στο αρχείο καταγραφής ανάλυσης

Για να διαχειριστείτε την πρόσβαση στο αρχείο καταγραφής ανάλυσης, θα χρησιμοποιήσετε μια ποικιλία εργασίες διαχείρισης χρηστών, λογαριασμούς, OMS χώρων εργασίας και λογαριασμοί Azure. Για να δημιουργήσετε ένα νέο χώρο εργασίας στην οικογένεια προγραμμάτων διαχείρισης λειτουργίες (OMS), μπορείτε να επιλέξετε ένα όνομα χώρου εργασίας, συσχετίσετε με το λογαριασμό σας και να επιλέξετε μια γεωγραφική θέση. Ένας χώρος εργασίας είναι ουσιαστικά ένα κοντέινερ που περιλαμβάνει τις πληροφορίες λογαριασμού και πληροφορίες απλό ρύθμισης παραμέτρων για το λογαριασμό. Εσείς ή άλλα μέλη της εταιρείας σας, μπορεί να χρησιμοποιήστε πολλούς χώρους εργασίας OMS για τη Διαχείριση διαφορετικά σύνολα δεδομένα που συλλέγονται από όλα ή τμήματα της υποδομής ΠΛΗΡΟΦΟΡΙΚΉΣ.

Στο άρθρο [Γρήγορα αποτελέσματα με το αρχείο καταγραφής ανάλυσης](log-analytics-get-started.md) που δείχνει τον τρόπο για να μεταβείτε γρήγορα προς τα επάνω και εκτελείται και τα υπόλοιπα σε αυτό το άρθρο περιγράφει με περισσότερες λεπτομέρειες ορισμένες από τις ενέργειες που θα πρέπει να διαχειριστείτε την πρόσβαση σε OMS.

Παρόλο που δεν μπορεί να χρειαστεί να εκτελέσετε κάθε εργασία διαχείρισης την πρώτη, παρουσιάζεται η όλες τις εργασίες που χρησιμοποιείται συχνά που μπορείτε να χρησιμοποιήσετε στις ακόλουθες ενότητες:

- Καθορισμός του αριθμού των χώρων εργασίας που χρειάζεστε
- Διαχείριση λογαριασμών και τους χρήστες
- Προσθήκη μιας ομάδας σε έναν υπάρχοντα χώρο εργασίας
- Σύνδεση έναν υπάρχοντα χώρο εργασίας σε μια συνδρομή του Azure
- Αναβάθμιση ενός χώρου εργασίας σε ένα πρόγραμμα επί πληρωμή δεδομένων
- Αλλαγή ενός τύπου δεδομένων πρόγραμμα
- Προσθήκη μιας εταιρείας Azure Active Directory σε έναν υπάρχοντα χώρο εργασίας
- Κλείσιμο του χώρου εργασίας σας OMS

## <a name="determine-the-number-of-workspaces-you-need"></a>Καθορισμός του αριθμού των χώρων εργασίας που χρειάζεστε

Ένα χώρο εργασίας είναι ένας πόρος Azure και είναι ένα κοντέινερ όπου δεδομένων είναι που συλλέγονται, αθροίζονται, ανάλυση και παρουσιάζονται στην πύλη του OMS.

Μπορείτε να δημιουργήσετε πολλούς χώρους εργασίας OMS καταγραφής ανάλυσης και για τους χρήστες να έχουν πρόσβαση σε έναν ή περισσότερους χώρους εργασίας. Σε γενικές γραμμές που θέλετε να ελαχιστοποιήσετε τον αριθμό των χώρων εργασίας όπως αυτό θα σας επιτρέψει να ερωτήματος και ο συσχετισμός κατά μήκος τα δεδομένα. Αυτή η ενότητα περιγράφει πότε μπορεί να είναι χρήσιμο να δημιουργήσετε περισσότερα από ένα χώρο εργασίας.

Σήμερα, παρέχει ένα χώρο εργασίας καταγραφής αναλυτικών στοιχείων:

- Μια γεωγραφική θέση για την αποθήκευση δεδομένων
- Υποδιαίρεση για τις χρεώσεις
- Απομόνωσης δεδομένων

Βάσει των παραπάνω χαρακτηριστικών, ενδέχεται να θέλετε να δημιουργήσετε πολλούς χώρους εργασίας εάν:

- Να είστε Καθολικός εταιρεία και χρειάζεστε δεδομένα που είναι αποθηκευμένα σε συγκεκριμένες περιοχές για λόγους ακεραιότητα ή συμμόρφωσης δεδομένων.
- Χρησιμοποιείτε το Azure και θέλετε να αποφύγετε χρεώσεις μεταφορά δεδομένων εξερχομένων, ζητώντας από ένα χώρο εργασίας του αρχείου καταγραφής ανάλυσης στην ίδια περιοχή ως το Azure πόρους που διαχειρίζεται.
- Θέλετε να εισαγάγετε χρεώσεις σε διαφορετικά τμήματα ή ομάδες επιχειρήσεις με βάση τη χρήση τους. Όταν δημιουργείτε ένα χώρο εργασίας για κάθε τμήμα ή ομάδα εργασία, το Azure δήλωση χρέωσης και η χρήση εμφανίζει τις χρεώσεις για κάθε χώρο εργασίας ξεχωριστά.
- Είστε μια διαχειριζόμενη υπηρεσία παροχής και πρέπει να διατηρήσετε τα δεδομένα ανάλυσης καταγραφής για κάθε πελάτη διαχειρίζεστε απομόνωσης από τα δεδομένα του πελάτη άλλων.
- Μπορείτε να διαχειριστείτε πολλούς πελάτες και θέλετε κάθε πελατών ή τμήμα ή ομάδα εργασία για να δείτε τα δικά τους δεδομένα, αλλά όχι τα δεδομένα για άλλους πελάτες ή τμήματα ή ομάδες επιχειρήσεις.

Όταν χρησιμοποιείτε παράγοντες για τη συλλογή δεδομένων, μπορείτε να ρυθμίσετε κάθε παράγοντα για την αναφορά στο απαιτούμενο χώρο εργασίας.

Εάν χρησιμοποιείτε τις λειτουργίες του κέντρου συστήματος διαχείρισης, κάθε ομάδα διαχείρισης Operations Manager μπορεί να συνδεθεί με μόνο ένα χώρο εργασίας. Μπορείτε να εγκαταστήσετε τον αντιπρόσωπο της Microsoft παρακολούθησης σε υπολογιστές που ελέγχονται από Operations Manager και να ρυθμίσετε την αναφορά παράγοντας για Operations Manager και σε διαφορετικό χώρο εργασίας του αρχείου καταγραφής ανάλυσης.

### <a name="workspace-information"></a>Πληροφορίες χώρου εργασίας

Στην πύλη του OMS, μπορείτε να προβάλετε τις πληροφορίες του χώρου εργασίας σας και να επιλέξετε αν θέλετε να λάβετε πληροφορίες από τη Microsoft.

#### <a name="view-workspace-information"></a>Προβολή πληροφοριών χώρου εργασίας

1. Στο OMS, κάντε κλικ στο πλακίδιο **Ρυθμίσεις** .
2. Κάντε κλικ στην καρτέλα **Λογαριασμοί** .
3. Κάντε κλικ στην καρτέλα **Πληροφορίες χώρου εργασίας** .  
  ![Πληροφορίες χώρου εργασίας](./media/log-analytics-manage-access/workspace-information.png)

## <a name="manage-accounts-and-users"></a>Διαχείριση λογαριασμών και τους χρήστες

Κάθε χώρο εργασίας μπορεί να έχει πολλούς λογαριασμούς χρηστών που σχετίζονται με το και κάθε λογαριασμό χρήστη (λογαριασμός Microsoft ή εταιρικός λογαριασμός) μπορούν να έχουν πρόσβαση σε πολλούς χώρους εργασίας OMS.

Από προεπιλογή, το λογαριασμό Microsoft ή εταιρικό λογαριασμό που χρησιμοποιήσατε για να δημιουργήσετε το χώρο εργασίας γίνεται ο διαχειριστής του χώρου εργασίας. Ο διαχειριστής μπορεί να προσκαλέσετε επιπλέον λογαριασμούς Microsoft ή επιλέξτε τους χρήστες από το Azure Active Directory.

Εκχωρεί πρόσβαση ατόμων στο χώρο εργασίας OMS ελέγχεται σε 2 θέσεις:

- Στο Azure, μπορείτε να χρησιμοποιήσετε τον έλεγχο πρόσβασης βάσει ρόλων για την παροχή πρόσβασης για τη συνδρομή Azure και το Azure τους συναφείς πόρους. Χρησιμοποιείται επίσης για πρόσβαση PowerShell και REST API.
- Στην πύλη του OMS, πρόσβαση μόνο στην πύλη OMS - δεν τη συσχετισμένη Azure συνδρομή.

Εάν που επιτρέπουν στους χρήστες πρόσβαση στην πύλη του OMS αλλά όχι να το Azure συνδρομή στην οποία είναι συνδεδεμένη με, στη συνέχεια, τα πλακίδια λύση αυτοματισμού, δημιουργία αντιγράφων ασφαλείας και Επαναφορά τοποθεσίας δεν εμφανίζονται δεδομένα στους χρήστες όταν τους είσοδος στην πύλη OMS.

Για να επιτρέψετε σε όλους τους χρήστες για να δείτε τα δεδομένα σε αυτές τις λύσεις, βεβαιωθείτε ότι έχουν τουλάχιστον πρόσβαση για το λογαριασμό αυτοματισμού, θάλαμο δημιουργίας αντιγράφων ασφαλείας και Επαναφορά τοποθεσίας θάλαμο που συνδέεται με το χώρο εργασίας OMS στο **πρόγραμμα ανάγνωσης** .   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Διαχείριση της πρόσβασης καταγραφής ανάλυση με την πύλη του Azure

Εάν εκχωρείτε πρόσβαση σε χρήστες στο χώρο εργασίας καταγραφής αναλυτικών στοιχείων χρήσης Azure δικαιώματα, στην πύλη του Azure για παράδειγμα, στη συνέχεια, τους ίδιους χρήστες έχουν πρόσβαση την πύλη του αρχείου καταγραφής ανάλυσης. Εάν οι χρήστες είναι στην πύλη του Azure, τους να μεταβείτε στην πύλη του OMS, κάνοντας κλικ στην εργασία **OMS πύλη** κατά την προβολή τον πόρο χώρου εργασίας καταγραφής αναλυτικών στοιχείων.

Ορισμένα σημεία που πρέπει να λάβετε υπόψη σχετικά με την πύλη του Azure:

- Αυτό δεν είναι *Έλεγχος πρόσβασης βάσει ρόλων*. Εάν έχετε δικαιώματα πρόσβασης *ανάγνωσης* στην πύλη του Azure για το χώρο εργασίας ανάλυση καταγραφής, στη συνέχεια, μπορείτε να κάνετε αλλαγές χρησιμοποιώντας την πύλη OMS. Η πύλη OMS έχει μια έννοια της διαχειριστή, συμβολής και χρήστη μόνο για ανάγνωση. Εάν ο λογαριασμός που είναι συνδεδεμένοι στο με είναι στο Azure Active Directory συνδεδεμένο με το χώρο εργασίας που θα είναι ο διαχειριστής στην πύλη του OMS, διαφορετικά θα ένα συνεργάτη.

- Όταν εισέρχεστε σε πύλη OMS χρησιμοποιώντας http://mms.microsoft.com, στη συνέχεια, από προεπιλογή, μπορείτε να δείτε τη λίστα **Επιλέξτε ένα χώρο εργασίας** . Περιέχει μόνο τους χώρους εργασίας που έχουν προστεθεί, χρησιμοποιώντας την πύλη OMS. Για να δείτε τους χώρους εργασίας που έχετε πρόσβαση με Azure συνδρομές και, στη συνέχεια, πρέπει να καθορίσετε ένα μισθωτή ως τμήμα της διεύθυνσης URL. Για παράδειγμα:

  `mms.microsoft.com/?tenant=contoso.com`Το αναγνωριστικό μισθωτή είναι συχνά αυτό το τελευταίο τμήμα της διεύθυνσης ηλεκτρονικού ταχυδρομείου που θα πραγματοποιήσετε είσοδο με.

- Εάν ο λογαριασμός θα πραγματοποιήσετε είσοδο με είναι ένα λογαριασμό στο μισθωτή Azure Active Directory, που είναι συνήθως πεζών και κεφαλαίων γραμμάτων, εκτός εάν είστε σχετικά με την είσοδο στο ως μια υπηρεσία παροχής Κρυπτογράφησης, στη συνέχεια, θα *διαχειριστή* στην πύλη του OMS. Εάν ο λογαριασμός σας δεν είναι μισθωτή Azure Active Directory, στη συνέχεια, θα ενός *χρήστη* στην πύλη του OMS.

- Εάν θέλετε να μεταβείτε απευθείας σε μια πύλη ότι έχετε πρόσβαση με τη χρήση του Azure δικαιώματα, στη συνέχεια, πρέπει να καθορίσετε τον πόρο ως τμήμα της διεύθυνσης URL. Είναι δυνατή η λήψη αυτής της διεύθυνσης URL με χρήση του PowerShell.

  Για παράδειγμα, `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  Η διεύθυνση URL θα μοιάζει κάπως:`https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`


### <a name="managing-users-in-the-oms-portal"></a>Διαχείριση χρηστών στην πύλη του OMS

Διαχείριση χρηστών και ομάδων στην καρτέλα **Διαχείριση χρηστών** στην καρτέλα **Λογαριασμοί** στη σελίδα ρυθμίσεις. Εκεί, μπορείτε να εκτελέσετε τις εργασίες στις ακόλουθες ενότητες.  

![Διαχείριση χρηστών](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### <a name="add-a-user-to-an-existing-workspace"></a>Προσθήκη ενός χρήστη σε έναν υπάρχοντα χώρο εργασίας

Χρησιμοποιήστε τα ακόλουθα βήματα για να προσθέσετε ένα χρήστη ή μια ομάδα σε ένα χώρο εργασίας OMS. Ο χρήστης ή η ομάδα θα μπορούν να προβάλλουν και να λειτουργεί σε όλες τις ειδοποιήσεις που σχετίζονται με αυτόν το χώρο εργασίας.

>[AZURE.NOTE] Εάν θέλετε να προσθέσετε ένα χρήστη ή μια ομάδα από τον εταιρικό λογαριασμό του Azure Active Directory, πρέπει πρώτα να βεβαιωθείτε ότι έχετε που σχετίζονται λογαριασμό OMS με τον τομέα της υπηρεσίας καταλόγου Active Directory. Ανατρέξτε στο θέμα [Προσθήκη ενός Azure Active Directory την εταιρεία σε έναν υπάρχοντα χώρο εργασίας](#add-an-azure-active-directory-organization-to-an-existing-workspace).

1. Στο OMS, κάντε κλικ στο πλακίδιο **Ρυθμίσεις** .
2. Κάντε κλικ στην καρτέλα **Λογαριασμοί** και, στη συνέχεια, κάντε κλικ στην καρτέλα **Διαχείριση χρηστών** .
3. Στην ενότητα **Διαχείριση χρηστών** , επιλέξτε τον τύπο του λογαριασμού για να προσθέσετε: **Εταιρικό λογαριασμό**, το **Λογαριασμό Microsoft**, **Υποστήριξη της Microsoft**.
    - Εάν επιλέξετε το λογαριασμό της Microsoft, πληκτρολογήστε τη διεύθυνση ηλεκτρονικού ταχυδρομείου του χρήστη που σχετίζεται με το λογαριασμό Microsoft.
    - Εάν επιλέξετε το εταιρικό λογαριασμό σας, μπορείτε να εισαγάγετε μέρος του χρήστη ή της ομάδας όνομα ή ψευδώνυμο ηλεκτρονικού ταχυδρομείου και θα εμφανιστεί μια λίστα των χρηστών και ομάδων. Επιλέξτε ένα χρήστη ή μια ομάδα.
    - Χρήση της υπηρεσίας υποστήριξης της Microsoft για να δώσετε μια υπηρεσία υποστήριξης της Microsoft αποσυμπίληση προσωρινή πρόσβαση στο χώρο εργασίας σας για να σας βοηθήσει με την αντιμετώπιση προβλημάτων.

    >[AZURE.NOTE] Για καλύτερα αποτελέσματα επιδόσεις, περιορίστε τον αριθμό των ομάδων υπηρεσίας καταλόγου Active Directory που σχετίζεται με ένα λογαριασμό OMS σε τρεις — μία για διαχειριστές, μία για συνεργάτες και μία για τους χρήστες μόνο για ανάγνωση. Χρήση περισσότερων ομάδων ενδέχεται να επηρεάσουν την απόδοση του αρχείου καταγραφής αναλυτικών στοιχείων.

5. Επιλέξτε τον τύπο του χρήστη ή μια ομάδα για να προσθέσετε: **διαχειριστή**, **συνεργάτη**ή **Χρήστη μόνο για ανάγνωση** .  
6. Κάντε κλικ στην επιλογή **Προσθήκη**.

  Εάν πρόκειται να προσθέσετε ένα λογαριασμό Microsoft, το μήνυμα ηλεκτρονικού ταχυδρομείου που παρέχεται αποστέλλεται μια πρόσκληση για να συμμετάσχετε στο χώρο εργασίας. Αφού ο χρήστης ακολουθεί τις οδηγίες που εμφανίζονται στην πρόσκληση για να συμμετάσχετε σε OMS, ο χρήστης μπορεί να προβάλει τις ειδοποιήσεις και τις πληροφορίες λογαριασμού για αυτόν το λογαριασμό OMS και θα μπορούν να δουν τις πληροφορίες χρήστη στην καρτέλα **Λογαριασμοί** της σελίδας **Ρυθμίσεις** .
  Εάν προσθέτετε έναν εταιρικό λογαριασμό, ο χρήστης θα μπορούν να έχουν πρόσβαση καταγραφής ανάλυσης αμέσως.  
  ![μήνυμα ηλεκτρονικού ταχυδρομείου πρόσκλησης](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

#### <a name="edit-an-existing-user-type"></a>Επεξεργασία υπάρχοντος τύπου χρήστη

Μπορείτε να αλλάξετε το ρόλο του λογαριασμού για ένα χρήστη που έχει συσχετιστεί με το λογαριασμό σας OMS. Έχετε τις εξής επιλογές ρόλου:

 - *Διαχειριστής*: μπορεί να Διαχείριση χρηστών, προβολή και εκτέλεση ενεργειών σε όλες τις ειδοποιήσεις, προσθήκη και Κατάργηση διακομιστών

 - *Συνεργάτης*: μπορεί να προβάλετε και εκτέλεση ενεργειών σε όλες τις ειδοποιήσεις, και προσθήκη ή Κατάργηση διακομιστών

 - *Μόνο για ανάγνωση χρήστη*: οι χρήστες που έχει επισημανθεί ως μόνο για ανάγνωση, δεν θα μπορείτε να:
   1. Προσθήκη/κατάργηση λύσεις. Είναι ορατή στη συλλογή λύσεων.
   2. Τα πλακίδια τροποποίηση/Προσθαφαίρεση στον **Πίνακα εργαλείων μου**.
   3. Προβάλετε τις σελίδες **ρυθμίσεων** . Οι σελίδες είναι κρυφές.
   4. Σε την αναζήτηση προβολής, PowerBI ρύθμισης παραμέτρων, αποθηκευμένες αναζητήσεις και ειδοποιήσεις εργασίες είναι κρυφές.


#### <a name="to-edit-an-account"></a>Για να επεξεργαστείτε ένα λογαριασμό

1. Στο OMS, κάντε κλικ στο πλακίδιο **Ρυθμίσεις** .
2. Κάντε κλικ στην καρτέλα **Λογαριασμοί** και, στη συνέχεια, κάντε κλικ στην καρτέλα **Διαχείριση χρηστών** .
3. Επιλέξτε το ρόλο για το χρήστη που θέλετε να αλλάξετε.
2. Στο παράθυρο διαλόγου επιβεβαίωσης, κάντε κλικ στο κουμπί **Ναι**.

### <a name="remove-a-user-from-a-oms-workspace"></a>Κατάργηση χρήστη από ένα χώρο εργασίας OMS

Χρησιμοποιήστε τα παρακάτω βήματα για να καταργήσετε ένα χρήστη από ένα χώρο εργασίας OMS. Σημειώστε ότι αυτό δεν Κλείσιμο του χώρου εργασίας του χρήστη. Αντί για αυτό, καταργεί τη συσχέτιση μεταξύ αυτόν το χρήστη και το χώρο εργασίας. Εάν ένας χρήστης σχετίζεται με πολλούς χώρους εργασίας, αυτός ο χρήστης εξακολουθεί να θα μπορείτε να εισέλθετε στο OMS και να δείτε τους χώρους εργασίας.

1. Στο OMS, κάντε κλικ στο πλακίδιο **Ρυθμίσεις** .
2. Κάντε κλικ στην καρτέλα **Λογαριασμοί** και, στη συνέχεια, κάντε κλικ στην καρτέλα **Διαχείριση χρηστών** .
3. Κάντε κλικ στην επιλογή **Κατάργηση** δίπλα στο όνομα του χρήστη που θέλετε να καταργήσετε.
4. Στο παράθυρο διαλόγου επιβεβαίωσης, κάντε κλικ στο κουμπί **Ναι**.


### <a name="add-a-group-to-an-existing-workspace"></a>Προσθήκη μιας ομάδας σε έναν υπάρχοντα χώρο εργασίας

1.  Ακολουθήστε τα βήματα 1 -4 σε "Για να προσθέσετε ένα χρήστη σε έναν υπάρχοντα χώρο εργασίας", παραπάνω.
2.  Στην περιοχή **Επιλέξτε χρήστη/ομάδας**, επιλέξτε **την ομάδα**.
    ![Προσθήκη μιας ομάδας σε έναν υπάρχοντα χώρο εργασίας](./media/log-analytics-manage-access/add-group.png)
3.  Εισαγάγετε τη διεύθυνση ηλεκτρονικού ταχυδρομείου ή εμφανιζόμενο όνομα για την ομάδα που θέλετε να προσθέσετε.
4.  Επιλέξτε την ομάδα από τη λίστα αποτελεσμάτων και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Σύνδεση έναν υπάρχοντα χώρο εργασίας σε μια συνδρομή του Azure

Είναι δυνατή η δημιουργία χώρου εργασίας από την τοποθεσία Web [microsoft.com/oms](https://microsoft.com/oms) .  Ωστόσο, ορισμένες υπάρχουν όρια για αυτούς τους χώρους εργασίας, τα περισσότερα με δυνατότητα σημείωσης που έχει τεθεί όριο 500MB/ημέρα αποστολών δεδομένα, εάν χρησιμοποιείτε ένα δωρεάν λογαριασμό. Για να κάνετε αλλαγές σε αυτόν το χώρο εργασίας θα πρέπει να *δημιουργήσετε τη σύνδεση του υπάρχοντος χώρου εργασίας σας σε μια συνδρομή του Azure*.

>[AZURE.IMPORTANT] Για να συνδέσετε ένα χώρο εργασίας, το Azure λογαριασμό πρέπει να έχετε ήδη πρόσβαση στο χώρο εργασίας που θέλετε να συνδέσετε.  Με άλλα λόγια, το λογαριασμό που χρησιμοποιείτε για πρόσβαση στην πύλη Azure πρέπει να είναι **το ίδιο** με το λογαριασμό που χρησιμοποιείτε για πρόσβαση του χώρου εργασίας σας OMS. Εάν δεν συμβαίνει αυτό, ανατρέξτε στο θέμα [Προσθήκη ενός χρήστη σε έναν υπάρχοντα χώρο εργασίας](#add-a-user-to-an-existing-workspace).

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Για να συνδέσετε ένα χώρο εργασίας με μια συνδρομή στην πύλη του OMS του Azure

Για να συνδέσετε ένα χώρο εργασίας σε μια συνδρομή του Azure στην πύλη του OMS, ο χρήστης πραγματοποιήσει στο πρέπει να έχετε ήδη ένα λογαριασμό επί πληρωμή Azure. Στο χώρο εργασίας που χρησιμοποιείτε ενεργά λαμβάνει που συνδέονται με το λογαριασμό Azure.

1. Στο OMS, κάντε κλικ στο πλακίδιο **Ρυθμίσεις** .
2. Κάντε κλικ στην καρτέλα **Λογαριασμοί** και, στη συνέχεια, κάντε κλικ στην καρτέλα **συνδρομή Azure και το πρόγραμμα δεδομένων** .
3. Κάντε κλικ στο πρόγραμμα δεδομένων που θέλετε να χρησιμοποιήσετε.
4. Κάντε κλικ στην επιλογή **Αποθήκευση**.  
  ![προγράμματα συνδρομής και δεδομένων](./media/log-analytics-manage-access/subscription-tab.png)

Το νέο πρόγραμμα δεδομένων εμφανίζεται στην κορδέλα του OMS πύλης στο επάνω μέρος της ιστοσελίδας σας.

![Κορδέλα OMS](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Για να συνδέσετε ένα χώρο εργασίας με μια συνδρομή του Azure στην πύλη του Azure

1.  Πραγματοποιήστε είσοδο στην [πύλη του Azure](http://portal.azure.com).
2.  Αναζήτηση για το **Αρχείο καταγραφής ανάλυσης (OMS)** και, στη συνέχεια, επιλέξτε την.
3.  Θα δείτε τη λίστα των υπάρχοντα χώρους εργασίας. Κάντε κλικ στην επιλογή **Προσθήκη**.  
    ![λίστα των χώρων εργασίας](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4.  Στην περιοχή **OMS χώρου εργασίας**, κάντε κλικ στο κουμπί **ή τη σύνδεση του υπάρχοντος**.  
    ![Σύνδεση υπάρχοντος](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5.  Κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων απαιτούμενες ρυθμίσεις**.  
    ![ρύθμιση παραμέτρων απαιτείται](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6.  Θα δείτε τη λίστα των χώρων εργασίας που δεν έχουν συνδεθεί ακόμα στο λογαριασμό σας Azure. Επιλέξτε ένα χώρο εργασίας.  
    ![Επιλέξτε τους χώρους εργασίας](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7.  Εάν είναι απαραίτητο, μπορείτε να αλλάξετε τιμές για τα ακόλουθα στοιχεία:
    - Συνδρομή
    - Ομάδα πόρων
    - Θέση
    - Πληροφορίες για την τιμολόγηση επιπέδων  
        ![Αλλαγή τιμών](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8.  Κάντε κλικ στην επιλογή **Δημιουργία**. Στο χώρο εργασίας είναι τώρα συνδεδεμένη Azure λογαριασμό σας.

>[AZURE.NOTE] Εάν δεν βλέπετε το χώρο εργασίας που θέλετε να δημιουργήσετε τη σύνδεση, στη συνέχεια, τη συνδρομή σας στο Azure δεν έχουν πρόσβαση στο χώρο εργασίας OMS που έχετε δημιουργήσει με χρήση της τοποθεσίας Web OMS.  Θα πρέπει να εκχωρήσετε πρόσβαση σε αυτόν το λογαριασμό από μέσα του χώρου εργασίας σας OMS χρήση της τοποθεσίας Web OMS. Για να το κάνετε, ανατρέξτε στο θέμα [Προσθήκη ενός χρήστη σε έναν υπάρχοντα χώρο εργασίας](#add-a-user-to-an-existing-workspace).



## <a name="upgrade-a-workspace-to-a-paid-data-plan"></a>Αναβάθμιση ενός χώρου εργασίας σε ένα πρόγραμμα επί πληρωμή δεδομένων

Υπάρχουν τρεις δεδομένων χώρου εργασίας Σχεδιασμός τύποι για OMS: **δωρεάν** **Βασική**και **Premium**.  Εάν είστε σε ένα πρόγραμμα *δωρεάν* , ενδέχεται να έχετε πατήσετε σας καπέλο δεδομένων 500 MB.  Θα πρέπει να κάνετε αναβάθμιση του χώρου εργασίας σας σε ένα ***πρόγραμμα διανεμητική*** προκειμένου να συλλέγετε δεδομένα υπερβαίνουν αυτό το όριο. Οποιαδήποτε στιγμή, μπορείτε να μετατρέψετε τον τύπο του σχεδίου σας.  Για περισσότερες πληροφορίες σχετικά με τις τιμές OMS, δείτε [Τις πληροφορίες τιμολόγησης λεπτομέρειες](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

>[AZURE.IMPORTANT] Προγράμματα του χώρου εργασίας μπορεί να αλλάξει μόνο εάν είναι *συνδεδεμένο* σε μια συνδρομή του Azure.  Εάν έχετε δημιουργήσει του χώρου εργασίας σας στο Azure ή εάν έχετε *ήδη* συνδεθεί του χώρου εργασίας σας, μπορείτε να αγνοήσετε αυτό το μήνυμα.  Εάν έχετε δημιουργήσει του χώρου εργασίας σας με την [τοποθεσία Web OMS](http://www.microsoft.com/oms), θα πρέπει να ακολουθήσετε τα βήματα στο θέμα [σύνδεση σε μια συνδρομή του Azure έναν υπάρχοντα χώρο εργασίας](#link-an-existing-workspace-to-an-azure-subscription).

### <a name="using-entitlements-from-the-oms-add-on-for-system-center"></a>Χρήση δικαιωμάτων από το πρόσθετο OMS για το κέντρο του συστήματος

Το πρόσθετο OMS για το κέντρο του συστήματος παρέχει δικαίωμα για το πρόγραμμα Premium της ανάλυσης καταγραφής OMS, περιγράφεται στην [OMS τις τιμές](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

Όταν αγοράζετε το πρόσθετο OMS για το κέντρο του συστήματος, το πρόσθετο OMS προστίθεται ως δικαίωμα σε συμφωνία με το κέντρο του συστήματος. Οποιαδήποτε Azure συνδρομή που δημιουργείται στην παρούσα συμφωνία μπορεί να κάνει χρήση του δικαιώματος. Αυτό σας επιτρέπει, για παράδειγμα, για να έχετε πολλούς χώρους εργασίας OMS που χρησιμοποιούν το δικαίωμα από το πρόσθετο OMS.

Για να βεβαιωθείτε ότι η χρήση της ενός χώρου εργασίας OMS εφαρμόζεται σε σας δικαιώματα από το πρόσθετο OMS, θα πρέπει να:

1. Σύνδεση του χώρου εργασίας σας OMS με μια συνδρομή Azure που είναι μέρος της συμφωνίας για μεγάλες επιχειρήσεις που περιλαμβάνει τόσο η αγορά πρόσθετων OMS και η χρήση Azure συνδρομή
2. Επιλέξτε το πρόγραμμα Premium για το χώρο εργασίας

Όταν εξετάζετε χρήση σας στην πύλη του Azure ή OMS, δεν θα βλέπετε τα δικαιώματα πρόσθετο OMS. Ωστόσο, μπορείτε να δείτε τα δικαιώματα στην πύλη για μεγάλες επιχειρήσεις.  

Εάν χρειάζεστε για να αλλάξετε τη συνδρομή Azure που συνδέεται με το χώρο εργασίας OMS, μπορείτε να χρησιμοποιήσετε το cmdlet του PowerShell Azure [Μετακίνηση AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) .

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Χρήση του Azure δέσμευσης από μια σύμβαση Enterprise

Εάν επιλέξετε να χρησιμοποιήσετε μεμονωμένη τις τιμές για τα στοιχεία OMS, θα μπορείτε να πληρώσετε ξεχωριστά για κάθε στοιχείο της OMS και η χρήση της θα εμφανίζεται στο τιμολόγιο του Azure.

Εάν έχετε μια Azure υποβολή νομισματικά στην εγγραφής enterprise στο οποίο είναι συνδεδεμένες όλες τις συνδρομές σας Azure, οποιαδήποτε χρήση της καταγραφής ανάλυσης θα χρέωσης αυτόματα σε σχέση με οποιαδήποτε υπόλοιπα υποβολή νομισματικά.

Εάν θέλετε να αλλάξετε τη συνδρομή Azure ότι είναι συνδεδεμένο στο χώρο εργασίας OMS για να μπορείτε να χρησιμοποιήσετε το cmdlet Azure PowerShell [Μετακίνηση AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) .  



### <a name="to-change-a-workspace-to-a-paid-data-plan"></a>Για να αλλάξετε ένα χώρο εργασίας σε ένα πρόγραμμα επί πληρωμή δεδομένων

1.  Πραγματοποιήστε είσοδο στην [πύλη του Azure](http://portal.azure.com).
2.  Αναζήτηση για το **Αρχείο καταγραφής ανάλυσης (OMS)** και, στη συνέχεια, επιλέξτε την.
3.  Θα δείτε τη λίστα των υπάρχοντα χώρους εργασίας. Επιλέξτε ένα χώρο εργασίας.  
    ![λίστα των χώρων εργασίας](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4.  Στην περιοχή **Ρυθμίσεις**, κάντε κλικ στο **επίπεδο Τιμολόγηση**.  
    ![πληροφορίες για την τιμολόγηση επιπέδων](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5.  Στην περιοχή **Τιμολόγηση επίπεδο**, επιλέξτε ένα πρόγραμμα δεδομένων και, στη συνέχεια, κάντε κλικ στην **επιλογή**.  
    ![Επιλέξτε πρόγραμμα](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6.  Όταν ανανεώνετε προβολή σας στην πύλη του Azure, θα δείτε **επίπεδο Τιμολόγηση** ενημέρωση για το πρόγραμμα που επιλέξατε.  
    ![Ενημερώστε τις πληροφορίες τιμολόγησης επιπέδων](./media/log-analytics-manage-access/manage-access-change-plan04.png)

Τώρα μπορείτε να συλλέξετε δεδομένα πέρα από το καπέλο "δωρεάν" δεδομένων.


## <a name="add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Προσθήκη μιας εταιρείας Azure Active Directory σε έναν υπάρχοντα χώρο εργασίας

Μπορείτε να συσχετίσετε το χώρο εργασίας ανάλυσης καταγραφής (OMS) με έναν τομέα Azure Active Directory. Αυτό σας επιτρέπει να προσθέσετε χρήστες από την υπηρεσία καταλόγου Active Directory απευθείας στο χώρο εργασίας σας OMS χωρίς να απαιτείται ένα νέο λογαριασμό Microsoft.

Κατά τη δημιουργία του χώρου εργασίας από την πύλη του Azure ή σύνδεση του χώρου εργασίας σας σε μια συνδρομή του Azure σας Azure Active Directory θα είναι συνδεδεμένο με τον εταιρικό λογαριασμό σας.

Όταν δημιουργείτε το χώρο εργασίας από την πύλη OMS θα σας ζητηθεί να συνδεθείτε σε μια συνδρομή στο Azure και έναν εταιρικό λογαριασμό.

### <a name="to-add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Για να προσθέσετε μια εταιρεία Azure Active Directory σε έναν υπάρχοντα χώρο εργασίας

1. Στη σελίδα "Ρυθμίσεις" στο OMS, κάντε κλικ στην επιλογή **Λογαριασμοί** και, στη συνέχεια, κάντε κλικ στην καρτέλα **Πληροφορίες χώρου εργασίας** .  
2. Διαβάστε τις πληροφορίες σχετικά με την εταιρικοί λογαριασμοί και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη εταιρείας**.  
    ![Προσθήκη εταιρείας](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Εισαγάγετε τις πληροφορίες ταυτότητας για το διαχειριστή του τομέα σας Azure Active Directory. Μετά από αυτό, θα δείτε μια βεβαίωση που δηλώνει ότι του χώρου εργασίας σας είναι συνδεδεμένο με τον τομέα σας Azure Active Directory.
    ![Επιβεβαίωση συνδεδεμένων χώρου εργασίας](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

>[AZURE.NOTE] Όταν ο λογαριασμός σας είναι συνδεδεμένη με έναν εταιρικό λογαριασμό, σύνδεση δεν είναι δυνατό να καταργηθεί ή έχουν αλλάξει.

## <a name="close-your-oms-workspace"></a>Κλείσιμο του χώρου εργασίας σας OMS

Κατά το κλείσιμο ενός χώρου εργασίας OMS, διαγράφονται όλα τα δεδομένα που σχετίζονται με το χώρο εργασίας από την υπηρεσία OMS εντός 30 ημερών από το κλείσιμο του χώρου εργασίας.

Εάν είστε διαχειριστής και υπάρχουν πολλοί χρήστες που σχετίζεται με το χώρο εργασίας, η συσχέτιση ανάμεσα σε αυτούς τους χρήστες και το χώρο εργασίας έχει διακοπεί. Εάν οι χρήστες συσχετίζονται με άλλους χώρους εργασίας, στη συνέχεια, τους να συνεχίσετε να χρησιμοποιείτε OMS με αυτές τις άλλες χώρους εργασίας. Ωστόσο, εάν δεν συσχετίζονται με άλλους χώρους εργασίας που θα χρειάζονται για να δημιουργήσετε ένα νέο χώρο εργασίας για να χρησιμοποιήσετε OMS.

### <a name="to-close-an-oms-workspace"></a>Για να κλείσετε ένα χώρο εργασίας OMS

1. Στο OMS, κάντε κλικ στο πλακίδιο **Ρυθμίσεις** .
2. Κάντε κλικ στην καρτέλα **Λογαριασμοί** και, στη συνέχεια, κάντε κλικ στην καρτέλα **Πληροφορίες χώρου εργασίας** .
3. Κάντε κλικ στο κουμπί **Κλείσιμο χώρου εργασίας**.
4. Επιλέξτε έναν από τους λόγους για το κλείσιμο του χώρου εργασίας σας ή πληκτρολογήστε ένα διαφορετικό λόγο στο πλαίσιο κειμένου.
5. Κάντε κλικ στο κουμπί **Κλείσιμο χώρου εργασίας**.

## <a name="next-steps"></a>Επόμενα βήματα

- Ανατρέξτε στο θέμα [σύνδεση Windows υπολογιστών ανάλυση καταγραφής](log-analytics-windows-agents.md) για να προσθέσετε παράγοντες και συγκέντρωση δεδομένων.
- [Προσθήκη αρχείου καταγραφής ανάλυσης λύσεις από τη συλλογή λύσεων](log-analytics-add-solutions.md) για να προσθέσετε λειτουργικότητα και συγκέντρωση δεδομένων.
- [Ρύθμιση παραμέτρων διακομιστή μεσολάβησης και το τείχος προστασίας στην ανάλυση καταγραφής](log-analytics-proxy-firewall.md) εάν η εταιρεία σας χρησιμοποιεί ένα διακομιστή μεσολάβησης ή το τείχος προστασίας ώστε να παράγοντες να επικοινωνήσετε με την υπηρεσία του αρχείου καταγραφής ανάλυσης.
