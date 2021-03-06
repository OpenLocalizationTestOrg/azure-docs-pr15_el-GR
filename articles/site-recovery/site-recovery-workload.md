<properties
    pageTitle="Τι φόρτους εργασίας να προστατεύσετε με την ανάκτηση Azure τοποθεσίας;"
    description="Azure Επαναφορά τοποθεσίας προστατεύει του όγκου εργασίας και εφαρμογών με το συντονισμό την αναπαραγωγή, ανακατεύθυνσης και αποκατάστασης εικονικές μηχανές εσωτερικής εγκατάστασης και φυσική διακομιστών Azure ή μια δευτερεύουσα τοποθεσία εσωτερικής"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>

# <a name="what-workloads-can-you-protect-with-azure-site-recovery"></a>Τι φόρτους εργασίας να προστατεύσετε με την ανάκτηση Azure τοποθεσίας;


Σε αυτό το άρθρο περιγράφει φόρτους εργασίας και τις εφαρμογές που μπορείτε να αναπαραγάγετε με την υπηρεσία Azure Επαναφορά τοποθεσίας.

Δημοσίευση σχόλια ή ερωτήσεις στο κάτω μέρος αυτού του άρθρου ή στο [Φόρουμ υπηρεσίες ανάκτησης Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Επισκόπηση

Οι οργανισμοί πρέπει επιχειρήσεις συνέχειας και καταστροφή (BCDR) στρατηγική αποκατάστασης για να διατηρήσετε όγκου εργασίας και δεδομένων ασφαλείς και διαθέσιμα κατά τη διάρκεια του χρόνου εκτός λειτουργίας προγραμματισμένες και μη προγραμματισμένες και ανακτήσετε όσο το δυνατόν πιο σύντομα σε κανονική συνθήκες εργασίας.

Επαναφορά τοποθεσίας είναι μια υπηρεσία Azure συνεισφοράς σε της στρατηγικής BCDR. Χρησιμοποιώντας την αποκατάσταση τοποθεσίας, μπορείτε να αναπτύξετε εφαρμογή υπόψη αναπαραγωγής στο cloud ή σε μια δευτερεύουσα τοποθεσία. Εάν οι εφαρμογές σας είναι Windows ή Linux, εκτελείται σε φυσική διακομιστές, VMware ή Hyper-V, μπορείτε να χρησιμοποιήσετε Επαναφορά τοποθεσίας για να οργανώσετε αναπαραγωγής, εκτέλεση καταστροφής δοκιμές ανάκτησης, και εκτέλεση ανακατευθύνσεις και επιστροφή σε αυτόν.


Επαναφορά τοποθεσίας ενοποιείται με τις εφαρμογές της Microsoft, συμπεριλαμβανομένου του SharePoint, Exchange, Dynamics, SQL Server και υπηρεσίας καταλόγου Active Directory. Η Microsoft συνεργάζεται επίσης στενά με αρχικών προμηθευτές, συμπεριλαμβάνοντας Oracle, SAP, IBM και καπέλο κόκκινο χρώμα. Μπορείτε να προσαρμόσετε λύσεις αλληλεπίδραση με βάση μια εφαρμογή από την εφαρμογή.

## <a name="why-use-site-recovery-for-application-replication"></a>Γιατί να χρησιμοποιήσετε Επαναφορά τοποθεσίας για την αναπαραγωγή της εφαρμογής;

Επαναφορά τοποθεσίας συμβάλλει στην προστασία σε επίπεδο εφαρμογής και αποκατάστασης ως εξής:

- Εφαρμογή-agnostic, παρέχοντας αναπαραγωγής για οποιαδήποτε φόρτους εργασίας εκτελούνται σε έναν υπολογιστή που υποστηρίζονται.
- Κοντά σύγχρονη αναπαραγωγής, με RPOs μέχρι 30 δευτερόλεπτα για να ανταποκρίνονται στις ανάγκες των πιο κρίσιμων επιχειρηματικών εφαρμογών.
- Εφαρμογή συνεπή στιγμιότυπα, για μία ή πολλαπλών επιπέδων εφαρμογές.
- Ενοποίηση με το SQL Server AlwaysOn και συνεργασία με άλλα επίπεδο εφαρμογής τεχνολογίες αναπαραγωγής, συμπεριλαμβανομένων AD αναπαραγωγή, SQL AlwaysOn, Exchange βάσης δεδομένων διαθεσιμότητας ομάδες (DAGs) και Επιφυλακή δεδομένων Oracle.
- Ευέλικτη σχέδια ανάκτησης, που σας επιτρέπουν να ανακτήσετε μια στοίβα ολόκληρη την εφαρμογή με ένα μόνο κλικ, και συμπεριλάβετε για να συμπεριλάβετε εξωτερικές δέσμες ενεργειών και των μη αυτόματων ενεργειών στο πρόγραμμα.
- Διαχείριση του δικτύου για προχωρημένους στο Επαναφορά τοποθεσίας και Azure για να απλοποιήσετε εφαρμογή τις απαιτήσεις του δικτύου, συμπεριλαμβανομένης της δυνατότητας να δεσμεύσετε διευθύνσεις IP, ρύθμιση παραμέτρων εξισορρόπησης φόρτου και την ενοποίηση με το Azure κίνηση Manager, για χαμηλή switchovers δικτύου RTO.
-  Μια βιβλιοθήκη εμπλουτισμένου αυτοματισμού που παρέχει παραγωγής ετοιμότητα, συγκεκριμένη εφαρμογή δεσμών ενεργειών που μπορούν να έχουν ληφθεί και ενσωματωμένο με σχέδια ανάκτησης.



## <a name="workload-summary"></a>Φόρτο εργασίας σύνοψης

Επαναφορά τοποθεσίας να αναπαραγάγετε οποιαδήποτε εφαρμογή που εκτελούνται σε έναν υπολογιστή που υποστηρίζονται. Επιπλέον μας έχετε συνεργαστεί με ομάδες προϊόντων για την εκτέλεση πρόσθετο έλεγχο συγκεκριμένης εφαρμογής.

**Φόρτο εργασίας** | **Αναπαραγωγή ΣΠΣ Hyper-V σε μια δευτερεύουσα τοποθεσία** | **Αναπαραγωγή ΣΠΣ Hyper-V σε Azure** | **Αναπαραγωγή ΣΠΣ VMware σε μια δευτερεύουσα τοποθεσία** | **Αναπαραγωγή ΣΠΣ VMware σε Azure**
---|---|---|---|---
Υπηρεσία καταλόγου Active Directory, DNS | Y | Y | Y | Y
Εφαρμογές Web (IIS, SQL) | Y | Y | Y | Y
System Center Operations Manager | Y | Y | Y | Y
Του SharePoint | Y | Y | Y | Y
SAP<br/><br/>Αναπαραγωγή τοποθεσία SAP σε Azure για μη συμπλέγματος | Y (ελεγχθεί από τη Microsoft) | Y (ελεγχθεί από τη Microsoft) | Y (ελεγχθεί από τη Microsoft) | Y (ελεγχθεί από τη Microsoft)
Exchange (μη DAG) | Y | Έρχομαι σύντομα | Y | Y
Απομακρυσμένη επιφάνεια εργασίας/VDI | Y | Y | Y | Δ/Υ
Linux (λειτουργικό σύστημα και εφαρμογές) | Y (ελεγχθεί από τη Microsoft) | Y (ελεγχθεί από τη Microsoft) | Y (ελεγχθεί από τη Microsoft) | Y (ελεγχθεί από τη Microsoft)
Dynamics AX | Y | Y | Y | Y
Dynamics CRM | Y | Έρχομαι σύντομα | Y | Έρχομαι σύντομα
Oracle | Y (ελεγχθεί από τη Microsoft) | Y (ελεγχθεί από τη Microsoft) | Y (ελεγχθεί από τη Microsoft) | Y (ελεγχθεί από τη Microsoft)
Διακομιστή αρχείων των Windows | Y | Y | Y | Y


## <a name="replicate-active-directory-and-dns"></a>Αναπαραγωγή υπηρεσίας καταλόγου Active Directory και το DNS

Μια υπηρεσία καταλόγου Active Directory και την υποδομή DNS είναι απαραίτητες για τα περισσότερα εταιρικών εφαρμογών. Κατά την αποκατάσταση, θα πρέπει να προστατεύσετε και να ανακτήσετε τα στοιχεία αυτά υποδομή πριν από την ανάκτηση του όγκου εργασίας και εφαρμογών.

Μπορείτε να χρησιμοποιήσετε Επαναφορά τοποθεσίας για να δημιουργήσετε ένα πρόγραμμα αποκατάστασης ολοκλήρωσης αυτοματοποιημένη καταστροφής για την υπηρεσία καταλόγου Active Directory και το DNS. Για παράδειγμα, εάν θέλετε να αποτύχει πάνω από το SharePoint και SAP από έναν πρωτεύοντα σε μια δευτερεύουσα τοποθεσία, μπορείτε να ρυθμίσετε ένα σχέδιο αποκατάστασης που αποτυγχάνει πρώτα πάνω από την υπηρεσία καταλόγου Active Directory και, στη συνέχεια ένα πρόγραμμα επιπλέον συγκεκριμένη εφαρμογή αποκατάστασης αποτυχία επάνω από τις άλλες εφαρμογές που βασίζονται στην υπηρεσία καταλόγου Active Directory.

[Μάθετε περισσότερα](site-recovery-active-directory.md) σχετικά με την προστασία της υπηρεσίας καταλόγου Active Directory και το DNS.

## <a name="protect-sql-server"></a>Προστασία του SQL Server

SQL Server παρέχει μια βάση δεδομένων υπηρεσιών για υπηρεσίες δεδομένων για πολλές επιχειρηματικών εφαρμογών σε ένα κέντρο δεδομένων εσωτερικής εγκατάστασης.  Επαναφορά τοποθεσίας μπορεί να χρησιμοποιηθεί μαζί με τις τεχνολογίες SQL Server HA/DR, για να προστατεύσετε εφαρμογές πολλαπλών επιπέδων για μεγάλες επιχειρήσεις που χρησιμοποιούν το SQL Server. Επαναφορά τοποθεσίας παρέχει:

- Μια λύση αποκατάστασης από καταστροφή απλές και οικονομικός για τον SQL Server. Αναπαραγωγή πολλών εκδόσεων και τις εκδόσεις του SQL Server μεμονωμένη διακομιστές και συμπλεγμάτων, για να Azure ή σε μια δευτερεύουσα τοποθεσία.  
- Ενοποίηση με τις ομάδες διαθεσιμότητας AlwaysOn SQL, για να διαχειριστείτε ανακατεύθυνσης και επιστροφή σε αυτόν με σχέδια ανάκτησης Azure Επαναφορά τοποθεσίας.
- Σχέδια για ολοκληρωμένες ανάκτησης για όλα τα επίπεδα σε μια εφαρμογή, συμπεριλαμβανομένων των βάσεων δεδομένων SQL Server.
- Κλιμάκωση του SQL Server για κορύφωσης φορτώνει με Επαναφορά τοποθεσίας, από την "πλούσια" τους σε μεγαλύτερα μεγέθη εικονική μηχανή IaaS στο Azure.
- Εύκολη δοκιμές του SQL Server αποκατάσταση. Μπορείτε να εκτελέσετε ανακατευθύνσεις δοκιμή για ανάλυση δεδομένων και την εκτέλεση ελέγχων συμμόρφωσης, χωρίς να επηρεάζονται περιβάλλον παραγωγής σας.

[Μάθετε περισσότερα](site-recovery-sql.md) σχετικά με την προστασία του SQL server.

##<a name="protect-sharepoint"></a>Προστασία του SharePoint

Azure Επαναφορά τοποθεσίας συμβάλλει στην προστασία αναπτύξεις του SharePoint, ως εξής:

- Αποκλείει την ανάγκη και σχετική υποδομή κόστους για ένα σύμπλεγμα αναμονής για αποκατάσταση. Χρησιμοποιούν Επαναφορά τοποθεσίας για την αναπαραγωγή ένα ολόκληρο το σύμπλεγμα (βαθμίδες Web, εφαρμογών και βάση δεδομένων) για να Azure ή σε μια δευτερεύουσα τοποθεσία.
- Απλοποιεί την ανάπτυξη εφαρμογών και διαχείρισης. Ενημερώσεις που έχουν αναπτυχθεί σε κύρια τοποθεσία αναπαράγονται αυτόματα και, επομένως, είναι διαθέσιμες μετά την ανακατεύθυνση και αποκατάστασης μιας συστοιχίας σε μια δευτερεύουσα τοποθεσία. Μειώνει επίσης την πολυπλοκότητα διαχείρισης και το κόστος που σχετίζεται με την ενημέρωση μια συστοιχία αναμονής.
- Απλοποιεί την ανάπτυξη εφαρμογών του SharePoint και τη δοκιμή, δημιουργώντας ένα περιβάλλον ρεπλίκα στη ζήτηση αντίγραφο μοιάζει με παραγωγής για τον έλεγχο και τον εντοπισμό σφαλμάτων.
- Απλοποιεί μετάβασης στο cloud χρησιμοποιώντας Επαναφορά τοποθεσίας για τη μετεγκατάσταση αναπτύξεων του SharePoint στο Azure.

[Μάθετε περισσότερα](https://gallery.technet.microsoft.com/SharePoint-DR-Solution-f6b4aeae) σχετικά με την προστασία του SharePoint.


## <a name="protect-dynamics-ax"></a>Προστασία Dynamics AX

Azure Επαναφορά τοποθεσίας συμβάλλει στην προστασία σας λύση Dynamics AX ERP, με:

- Orchestrating αναπαραγωγή της ολόκληρο Dynamics AX το περιβάλλον σας (βαθμίδες Web και AOS, βαθμίδες βάσης δεδομένων, SharePoint) Azure ή σε μια δευτερεύουσα τοποθεσία.
- Απλοποίηση της μετεγκατάστασης του Dynamics AX αναπτύξεις στο cloud (Azure).
- Απλοποίηση ανάπτυξη εφαρμογών Dynamics AX και έλεγχος, δημιουργώντας ένα αντίγραφο μοιάζει με παραγωγής στην ' απαίτηση, για τον έλεγχο και τον εντοπισμό σφαλμάτων.

[Μάθετε περισσότερα](https://gallery.technet.microsoft.com/Dynamics-AX-DR-Solution-b2a76281) σχετικά με την προστασία δυναμικής AX.

## <a name="protect-rds"></a>Προστασία RDS

Υπηρεσίες απομακρυσμένης επιφάνειας εργασίας (RDS) επιτρέπει υποδομή εικονικού υπολογιστή (VDI), με βάση την περίοδο λειτουργίας υπολογιστών και εφαρμογές, επιτρέποντας στους χρήστες να εργαστείτε από οπουδήποτε. Με την Επαναφορά τοποθεσίας Azure μπορείτε να:

- Αναπαραγωγή διαχειριζόμενη ή μη διαχειριζόμενη συνδυασμένου εικονικών επιφανειών εργασίας σε μια δευτερεύουσα τοποθεσία, και απομακρυσμένο εφαρμογές και περιόδους λειτουργίας σε μια δευτερεύουσα τοποθεσία ή Azure.
- Εδώ θα βρείτε τι μπορείτε να αναπαραγάγετε:

**RDS** | **Αναπαραγωγή ΣΠΣ Hyper-V σε μια δευτερεύουσα τοποθεσία** | **Αναπαραγωγή ΣΠΣ Hyper-V σε Azure** | **Αναπαραγωγή ΣΠΣ VMware σε μια δευτερεύουσα τοποθεσία** | **Αναπαραγωγή ΣΠΣ VMware σε Azure** | **Αναπαραγωγή φυσικής διακομιστές σε μια δευτερεύουσα τοποθεσία** | **Αναπαραγωγή φυσικής διακομιστές σε Azure**
---|---|---|---|---|---|---
**Συνδυασμένου εικονική επιφάνεια εργασίας (μη διαχειριζόμενου)** | Ναι | Όχι | Ναι | Όχι | Ναι | Όχι
**Συνδυασμένου εικονική επιφάνεια εργασίας (διαχειριζόμενων και χωρίς UPD)** | Ναι | Όχι | Ναι | Όχι | Ναι | Όχι
**Απομακρυσμένη εφαρμογές και περιόδους λειτουργίας επιφάνειας εργασίας (χωρίς UPD)** | Ναι | Ναι | Ναι | Ναι | Ναι | Ναι


[Μάθετε περισσότερα](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) σχετικά με την προστασία RDS.


## <a name="protect-exchange"></a>Προστασία του Exchange

Επαναφορά τοποθεσίας συμβάλλει στην προστασία Exchange, ως εξής:

- Για μικρή αναπτύξεις του Exchange, όπως ένα μονό ή αυτόνομη έκδοση διακομιστές, Επαναφορά τοποθεσίας μπορεί να αναπαραγάγετε και ανακατευθύνει Azure ή μια δευτερεύουσα τοποθεσία.
- Για μεγαλύτερες αναπτύξεις, Επαναφορά τοποθεσίας ενοποιείται με το Exchange DAGS.
- Exchange DAGs είναι η προτεινόμενη λύση για το Exchange αποκατάσταση σε μια επιχείρηση.  Σχέδια ανάκτησης αποκατάστασης τοποθεσίας μπορούν να περιλαμβάνουν DAGs, για να οργανώσετε DAG ανακατεύθυνση σε τοποθεσίες.


[Μάθετε περισσότερα](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) σχετικά με την προστασία του Exchange.

## <a name="protect-sap"></a>Προστασία SAP

Χρησιμοποιήστε Επαναφορά τοποθεσίας για να προστατεύσετε την ανάπτυξη SAP, ως εξής:

- Ενεργοποίηση προστασίας της ολόκληρο ανάπτυξης SAP, κατά την αναπαραγωγή ανάπτυξης διαφορετικά επίπεδα Azure ή σε μια δευτερεύουσα τοποθεσία.
- Απλοποίηση της μετεγκατάστασης cloud, με τη χρήση Επαναφορά τοποθεσίας για τη μετεγκατάσταση του ανάπτυξη SAP στο Azure.
- Απλοποίηση της ανάπτυξης εφαρμογών SAP και δοκιμές, δημιουργώντας μια παραγωγής μοιάζει με αντιγραφή σε απαιτήσεων για τον έλεγχο και τον εντοπισμό σφαλμάτων σε εφαρμογές.

[Μάθετε περισσότερα](http://aka.ms/asr-sap) σχετικά με την προστασία SAP.

## <a name="next-steps"></a>Επόμενα βήματα

[Προετοιμασία για ανάπτυξη Επαναφορά τοποθεσίας](site-recovery-best-practices.md) 
