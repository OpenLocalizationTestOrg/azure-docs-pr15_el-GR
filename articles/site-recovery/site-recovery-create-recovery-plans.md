<properties
    pageTitle="Δημιουργήστε σχέδια ανάκτησης | Microsoft Azure" 
    description="Δημιουργήστε σχέδια ανάκτησης Azure Επαναφορά τοποθεσίας για να ανακατευθύνει και να ανακτήσετε ομάδες εικονικές μηχανές και φυσική διακομιστές." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="create-recovery-plans"></a>Δημιουργήστε σχέδια ανάκτησης

Της υπηρεσίας ανάκτησης τοποθεσίας Azure συμβάλλει της στρατηγικής ανάκτησης (BCDR) επιχειρήσεις συνέχειας και καταστροφή από orchestrating αναπαραγωγή, ανακατεύθυνσης και αποκατάστασης εικονικές μηχανές και φυσική διακομιστών. Μηχανές μπορούν να αναπαραχθούν Azure ή σε ένα κέντρο δεδομένων δευτερεύοντα εσωτερικής εγκατάστασης. Για μια γρήγορη επισκόπηση Διαβάστε [Τι είναι η Επαναφορά τοποθεσίας Azure;](site-recovery-overview.md).


## <a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο παρέχει πληροφορίες σχετικά με τη δημιουργία και προσαρμογή σχέδια ανάκτησης. 

Σχέδια ανάκτησης αποτελείται από μία ή περισσότερες ομάδες ταξινομημένη που περιέχουν εικονικές μηχανές ή φυσική τους διακομιστές που θέλετε να γίνει ανακατεύθυνση μαζί. Σχέδια ανάκτησης κάντε τα εξής:

- Ορισμός ομάδων μηχανήματα που ανακατευθύνει και, στη συνέχεια, ξεκινήστε μαζί.
- Μοντέλο εξαρτήσεις μεταξύ των υπολογιστών, ομαδοποιώντας τις μαζί σε μια ομάδα πρόγραμμα αποκατάστασης. Για παράδειγμα, εάν θέλετε να γίνει ανακατεύθυνση και εμφανιστεί μια συγκεκριμένη εφαρμογή που θα ομαδοποιήσετε τις εικονικές μηχανές για αυτήν την εφαρμογή της ίδιας ομάδας πρόγραμμα αποκατάστασης.
- Αυτοματοποίηση και επέκταση ανακατεύθυνσης. Μπορείτε να εκτελέσετε μια δοκιμή, προγραμματισμένη, ή μη προγραμματισμένη ανακατεύθυνσης ένα πρόγραμμα αποκατάστασης. Μπορείτε να προσαρμόσετε σχέδια ανάκτησης με δέσμες ενεργειών, Azure αυτοματισμού και μη αυτόματων ενεργειών.

Σχέδια ανάκτησης εμφανίζονται σχετικά με τα **Σχέδια ανάκτησης** στην πύλη του Επαναφορά τοποθεσίας.


Δημοσίευση σχόλια ή ερωτήσεις στο κάτω μέρος αυτού του άρθρου ή στο [Φόρουμ υπηρεσίες ανάκτησης Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Πριν ξεκινήσετε

Λάβετε υπόψη τα εξής:

- Σχέδιο αποκατάστασης δεν πρέπει ο συνδυασμός ΣΠΣ με προσαρμογέων δικτύου μίας και πολλών. Αυτό συμβαίνει επειδή ανάμιξη αυτά τα ΣΠΣ δεν επιτρέπονται σε μια υπηρεσία Azure cloud.
- Μπορείτε να επεκτείνετε σχέδια ανάκτησης με δέσμες ενεργειών και μη αυτόματων ενεργειών. Λάβετε υπόψη τα εξής:
    - Γράψτε δέσμης ενεργειών με χρήση του Windows PowerShell.
    - Βεβαιωθείτε ότι οι δέσμες ενεργειών χρησιμοποιούν μπλοκ δοκιμή προϊόντων, έτσι ώστε οι εξαιρέσεις αντιμετωπίζονται ομαλά. Εάν υπάρχει μια εξαίρεση στη δέσμη ενεργειών του παύει να εκτελείται και εμφανίζει την εργασία, καθώς απέτυχε.  Εάν παρουσιαστεί σφάλμα, δεν θα εκτελεστεί οποιοδήποτε υπόλοιπο τμήμα της δέσμης ενεργειών. Εάν αυτό συμβαίνει όταν εκτελείτε μια μη προγραμματισμένη ανακατεύθυνσης, θα συνεχίσει το πρόγραμμα αποκατάστασης. Εάν αυτό συμβαίνει όταν εκτελείτε μια προγραμματισμένη ανακατεύθυνσης, το πρόγραμμα αποκατάστασης θα σταματήσει. Αν συμβεί αυτό, διορθώσετε τη δέσμη ενεργειών, βεβαιωθείτε ότι εκτελείται με τον αναμενόμενο τρόπο και, στη συνέχεια, εκτελέστε ξανά το πρόγραμμα αποκατάστασης.
    - Η εντολή Host εγγραφής δεν λειτουργεί σε μια δέσμη ενεργειών πρόγραμμα αποκατάστασης και τη δέσμη ενεργειών θα αποτύχει. Εάν θέλετε να δημιουργήσετε εξόδου, δημιουργήσετε μια δέσμη ενεργειών του διακομιστή μεσολάβησης που εκτελείται με τη σειρά του κύριου δέσμης ενεργειών και βεβαιωθείτε ότι όλα τα δεδομένα εξόδου είναι διοχετεύεται μέσω του >> εντολή.
    - Η δέσμη ενεργειών λήγει το χρονικό όριο Εάν δεν επιστρέφει μέσα σε 600 δευτερόλεπτα.
    - Εάν όλα τα στοιχεία είναι αντιγραφούν στο STDERR, η δέσμη ενεργειών ταξινομούνται ως απέτυχε. Αυτές οι πληροφορίες θα εμφανίζονται στις λεπτομέρειες εκτέλεσης δέσμης ενεργειών.
    - Εάν χρησιμοποιείτε το VMM στην ανάπτυξή σας να σημειωθεί ότι:

        - Δέσμες ενεργειών σε ένα σχέδιο αποκατάστασης εκτελούνται στο περιβάλλον του λογαριασμού της υπηρεσίας VMM. Βεβαιωθείτε ότι αυτός ο λογαριασμός έχει δικαιώματα ανάγνωσης στην Απομακρυσμένη κοινή χρήση στην οποία βρίσκεται η δέσμη ενεργειών και δοκιμάστε τη δέσμη ενεργειών για να εκτελέσετε το επίπεδο δικαιωμάτων VMM υπηρεσίας λογαριασμού.
        - Cmdlet για VMM παραδίδονται σε μια λειτουργική μονάδα Windows PowerShell. Τη λειτουργική μονάδα VMM Windows PowerShell εγκαθίσταται όταν εγκαθιστάτε την κονσόλα VMM. Η λειτουργική μονάδα VMM μπορεί να μεταφερθεί σε δέσμη ενεργειών σας χρησιμοποιώντας την ακόλουθη εντολή στη δέσμη ενεργειών: εισαγωγή-λειτουργική μονάδα-virtualmachinemanager όνομα. [Μάθετε περισσότερες λεπτομέρειες](hhttps://technet.microsoft.com/library/hh875013.aspx).
        - Βεβαιωθείτε ότι έχετε τουλάχιστον μία βιβλιοθήκη διακομιστή στην ανάπτυξή σας VMM. Από προεπιλογή τη διαδρομή κοινή χρήση βιβλιοθήκης για ένα διακομιστή VMM βρίσκεται τοπικά στο διακομιστή VMM με το όνομα του φακέλου MSCVMMLibrary.
        - Εάν σας διαδρομή κοινή χρήση βιβλιοθήκη είναι απομακρυσμένο (ή τοπικά αλλά δεν κοινόχρηστα με MSCVMMLibrary, ρύθμιση παραμέτρων κοινή χρήση ως εξής (χρησιμοποιώντας \\libserver2.contoso.com\share\ ως παράδειγμα):
            - Ανοίξτε τον Επεξεργαστή μητρώου και μεταβείτε σε HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Recovery\Registration τοποθεσίας.
            -  Επεξεργαστείτε την τιμή ScriptLibraryPath και τοποθετήστε την τιμή ως \\libserver2.contoso.com\share\. Καθορίστε το πλήρες FQDN. Παροχή δικαιωμάτων για τη θέση του κοινόχρηστου στοιχείου.
            -  Βεβαιωθείτε ότι ελέγχετε τη δέσμη ενεργειών με ένα λογαριασμό χρήστη που έχει τα ίδια δικαιώματα με το λογαριασμό της υπηρεσίας VMM, για να βεβαιωθείτε ότι αυτόνομη έλεγχο δέσμες ενεργειών που εκτελούνται με τον ίδιο τρόπο που θα στα σχέδια ανάκτησης. Στο διακομιστή VMM, ορίστε την πολιτική εκτέλεσης να παρακάμψει ως εξής:
                -  Ανοίξτε την κονσόλα του Windows PowerShell 64-bit με αναβαθμισμένα δικαιώματα.
                -  Τύπος: **Set-της πολιτικής εκτέλεσης του παράκαμψη**. [Μάθετε περισσότερες λεπτομέρειες](https://technet.microsoft.com/library/ee176961.aspx).

## <a name="create-a-recovery-plan"></a>Δημιουργία σχεδίου ανάκτησης

Ο τρόπος στο οποίο μπορείτε να δημιουργήσετε ένα σχέδιο αποκατάστασης εξαρτάται από την ανάπτυξη Επαναφορά τοποθεσίας.

- **Αναπαραγωγή ΣΠΣ VMware και φυσική διακομιστών**— μπορείτε να δημιουργήσετε ένα σχέδιο και να προσθέσετε ομάδες αναπαραγωγής που περιέχουν εικονικές μηχανές και φυσική διακομιστές σε ένα πρόγραμμα αποκατάστασης.
- **Αναπαραγωγή ΣΠΣ Hyper-V (σε σύννεφων VMM)**— Δημιουργία σχεδίου και προσθήκη προστατευμένο Hyper-V εικονικές μηχανές από ένα σύννεφο VMM σε ένα πρόγραμμα αποκατάστασης.
- **Αναπαραγωγή ΣΠΣ Hyper-V (όχι σε σύννεφων VMM)**— Δημιουργία σχεδίου και προσθέστε προστατευμένο Hyper-V εικονικές μηχανές από μια ομάδα προστασία σχεδίου αποκατάστασης.
- **Αναπαραγωγή ΣΑΝ**— Δημιουργία σχεδίου και προσθέστε μια ομάδα αναπαραγωγής που περιέχει εικονικές μηχανές στο πρόγραμμα αποκατάστασης. Επιλέξτε μια ομάδα αναπαραγωγής αντί για συγκεκριμένες εικονικές μηχανές, επειδή όλες οι εικονικές μηχανές σε μια ομάδα αναπαραγωγής πρέπει να γίνει ανακατεύθυνση μαζί (ανακατεύθυνσης πραγματοποιείται στο επίπεδο του χώρου αποθήκευσης πρώτα).


Δημιουργία σχεδίου αποκατάστασης ως εξής:

1. Κάντε κλικ στην καρτέλα **Προγράμματος αποκατάστασης** > **Δημιουργία Σχεδιασμός αποκατάστασης**.
Καθορίστε ένα όνομα για το πρόγραμμα ανάκτησης, και ένα αρχείο προέλευσης και προορισμού. Στο διακομιστή προέλευσης πρέπει να έχετε εικονικές μηχανές που έχουν ενεργοποιηθεί για ανακατεύθυνση και αποκατάστασης.

    - Εάν αναπαραγωγή από VMM να VMM επιλέξτε **Τύπο προέλευσης** > **VMM**και τους διακομιστές VMM προέλευσης και προορισμού. Κάντε κλικ στην επιλογή **Hyper-V** για να δείτε σύννεφων που έχουν ρυθμιστεί για να χρησιμοποιήσετε το Hyper-V αντιγράφου. 
    - Εάν αναπαραγωγή από VMM να VMM με τη χρήση ΣΑΝ επιλέξτε **Τύπο προέλευσης** > **VMM**και τους διακομιστές VMM προέλευσης και προορισμού. Κάντε κλικ στο κουμπί για να δείτε σύννεφων που έχουν ρυθμιστεί για την αναπαραγωγή ΣΑΝ **ΣΑΝ** .
    - Εάν αναπαραγωγή από VMM να Azure επιλέξτε **Τύπο προέλευσης** > **VMM**.  Επιλέξτε το διακομιστή VMM προέλευσης και **Azure** ως προορισμό.
    - Εάν αναπαραγωγή από μια τοποθεσία Hyper-V επιλέξτε **Τύπο προέλευσης** > **Hyper-V τοποθεσίας**. Επιλέξτε την τοποθεσία ως προέλευσης και **Azure **ως προορισμό.
    - Εάν αναπαραγωγή από VMware ή ένα διακομιστή φυσικής εσωτερικής εγκατάστασης για να Azure, επιλέξτε ένα διακομιστή ρύθμισης παραμέτρων ως προέλευσης και **Azure** ως προορισμό

2. Στην περιοχή **επιλογή εικονικές μηχανές** , επιλέξτε το εικονικές μηχανές (ή ομάδα αναπαραγωγής) που θέλετε να προσθέσετε στην προεπιλεγμένη ομάδα (ομάδα 1) στο πρόγραμμα αποκατάστασης.

## <a name="customize-recovery-plans"></a>Προσαρμογή σχέδια ανάκτησης

Αφού προσθέσετε τα προστατευμένα εικονικές μηχανές ή ομάδες αναπαραγωγής στην προεπιλεγμένη ομάδα σχέδιο αποκατάστασης και δημιουργήθηκε το πρόγραμμα που μπορείτε να την προσαρμόσετε:

- **Προσθήκη νέων ομάδων**— μπορείτε να προσθέσετε επιπλέον αποκατάστασης πρόγραμμα ομάδες. Μπορείτε να προσθέσετε ομάδες αριθμούνται με τη σειρά με την οποία τα προσθέτετε. Μπορείτε να προσθέσετε έως και επτά ομάδες. Μπορείτε να προσθέσετε περισσότερους υπολογιστές ή ομάδες αναπαραγωγής σε αυτές τις νέες ομάδες. Σημειώστε ότι μια εικονική μηχανή ή ομάδας αναπαραγωγής να συμπεριληφθεί μόνο στην ομάδα σχέδιο μία αποκατάστασης.
- **Προσθήκη μιας δέσμης ενεργειών **— μπορείτε να προσθέσετε δέσμες ενεργειών που πριν ή μετά από μια αποκατάσταση σχεδιασμός ομάδας. Όταν προσθέτετε μια δέσμη ενεργειών που προσθέτει ένα νέο σύνολο ενεργειών για την ομάδα. Για παράδειγμα ένα σύνολο προ-βήματα για την ομάδα 1 θα δημιουργηθεί με το όνομα: ομάδα 1: προ-βήματα. Όλα τα προ-βήματα θα εμφανίζονται μέσα σε αυτό το σύνολο. Σημειώστε ότι μπορείτε να προσθέσετε μόνο μια δέσμη ενεργειών στην κύρια τοποθεσία εάν έχετε ένα διακομιστή VMM αναπτυχθεί.
- **Προσθέστε μια μη αυτόματη ενέργεια**— μπορείτε να προσθέσετε μη αυτόματων ενεργειών που εκτελούνται πριν ή μετά από μια ομάδα πρόγραμμα αποκατάστασης. Όταν εκτελείται το πρόγραμμα ανάκτησης, να σταματήσει στο σημείο στο οποίο έχετε εισαγάγει η μη αυτόματη ενέργεια και ένα παράθυρο διαλόγου σάς ζητά να καθορίσετε ότι ολοκληρώθηκε η μη αυτόματη ενέργεια.

## <a name="extend-recovery-plans-with-scripts"></a>Επέκταση σχέδια ανάκτησης με δέσμες ενεργειών

Μπορείτε να προσθέσετε μια δέσμη ενεργειών για το πρόγραμμά σας αποκατάστασης:

- Εάν έχετε μια τοποθεσία προέλευσης VMM μπορείτε να δημιουργήσετε μια δέσμη ενεργειών του διακομιστή VMM και να συμπεριλάβετε στο πρόγραμμα αποκατάστασης.
- Εάν αναπαραγωγή για να Azure μπορείτε να ενοποιήσετε το Azure αυτοματισμού runbooks στο σχέδιό σας ανάκτησης

### <a name="create-a-vmm-script"></a>Δημιουργία δέσμης ενεργειών VMM


Δημιουργία της δέσμης ενεργειών ως εξής:

1. Δημιουργία νέου φακέλου σε βιβλιοθήκη κοινή χρήση, για παράδειγμα \<VMMServerName > \MSSCVMMLibrary\RPScripts. Τοποθέτηση στο αρχείο προέλευσης και προορισμού VMM διακομιστές.
2. Δημιουργία της δέσμης ενεργειών (για παράδειγμα RPScript) και επιλέξτε λειτουργεί όπως αναμένεται.
3. Τοποθετήστε τη δέσμη ενεργειών στη θέση \<VMMServerName > \MSSCVMMLibrary στους διακομιστές VMM προέλευσης και προορισμού.

### <a name="create-an-azure-automation-runbook"></a>Δημιουργήστε μια runbook Azure αυτοματισμού

Μπορείτε να επεκτείνετε το πρόγραμμά σας ανάκτησης, εκτελώντας μια runbook Azure αυτοματισμού ως μέρος του σχεδίου. [Διαβάστε περισσότερα](site-recovery-runbook-automation.md).


### <a name="add-custom-settings-to-a-recovery-plan"></a>Προσθέστε προσαρμοσμένες ρυθμίσεις σε ένα πρόγραμμα ανάκτησης

1. Ανοίξτε το πρόγραμμα ανάκτησης που θέλετε να προσαρμόσετε.
2. Κάντε κλικ στην επιλογή για να προσθέσετε μια εικονικές μηχανές ή νέα ομάδα.
3. Για να προσθέσετε μια δέσμη ενεργειών ή μη αυτόματη ενέργεια κλικ σε οποιοδήποτε στοιχείο στη λίστα **βήμα** και, στη συνέχεια, κάντε κλικ στην επιλογή **δέσμης ενεργειών** ή **Μη αυτόματη ενέργεια**. Καθορίστε εάν θα θέλετε να προσθέσετε τη δέσμη ενεργειών ή την ενέργεια πριν ή μετά από το επιλεγμένο στοιχείο. Χρησιμοποιήστε τα κουμπιά εντολών **Μετακίνηση προς τα επάνω** και **Μετακίνηση προς τα κάτω** για να μετακινήσετε τη θέση της δέσμης ενεργειών προς τα επάνω ή προς τα κάτω.
4. Αν προσθέσετε μια δέσμη ενεργειών VMM, επιλέξτε **ανακατεύθυνση VMM δέσμη ενεργειών**και στο στη **Διαδρομή δέσμης ενεργειών** , πληκτρολογήστε τη σχετική διαδρομή για κοινή χρήση. Ναι, για παράδειγμά μας όπου βρίσκεται η εντολή κοινή χρήση στο \\ <VMMServerName>\MSSCVMMLibrary\RPScripts, καθορίστε τη διαδρομή: \RPScripts\RPScript.PS1.
5. Εάν προσθέτετε έναν Azure αυτοματισμού εκτέλεση βιβλίο, καθορίστε το **Λογαριασμό αυτοματισμού Azure** στην οποία βρίσκεται το runbook, και επιλέξτε το κατάλληλο **Δέσμης ενεργειών Runbook Azure**.
5. Κάντε ανακατεύθυνσης του σχεδίου ανάκτησης για να βεβαιωθείτε ότι η δέσμη ενεργειών λειτουργεί όπως αναμένεται.


## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε να εκτελέσετε διαφορετικούς τύπους ανακατευθύνσεις πρόγραμμα ανάκτησης, συμπεριλαμβανομένων των ανακατεύθυνσης δοκιμή για να ελέγξετε το περιβάλλον και προγραμματισμένες ή μη προγραμματισμένες ανακατευθύνσεις. [Μάθετε περισσότερα](site-recovery-failover.md).


 
