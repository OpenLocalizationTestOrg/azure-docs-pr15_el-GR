<properties
    pageTitle="Επισκόπηση των σχημάτων και το πακέτο ενοποίησης για μεγάλες επιχειρήσεις | Microsoft Azure"
    description="Μάθετε πώς να χρησιμοποιείτε σχήματα με τις εφαρμογές του πακέτου ενοποίησης για μεγάλες επιχειρήσεις και λογική"
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="msftman"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="deonhe"/>

# <a name="learn-about-schemas-and-the-enterprise-integration-pack"></a>Μάθετε περισσότερα σχετικά με τα σχήματα και το πακέτο ενοποίησης για μεγάλες επιχειρήσεις  

## <a name="why-use-a-schema"></a>Γιατί να χρησιμοποιήσετε ένα σχήμα;
Χρησιμοποιήστε τα σχήματα για να επιβεβαιώσετε ότι τα έγγραφα XML που λαμβάνετε είναι έγκυρες, με τα αναμενόμενα δεδομένα σε μια προκαθορισμένη μορφή. Τα σχήματα που χρησιμοποιούνται για την επικύρωση των μηνυμάτων που ανταλλάσσονται σε ένα σενάριο B2B.

## <a name="add-a-schema"></a>Προσθήκη σχήματος
Από την πύλη του Azure:  

1. Επιλέξτε **περισσότερες υπηρεσίες**.  
![Στιγμιότυπο οθόνης του Azure πύλη, με "Περισσότερες υπηρεσίες" Επισήμανση](./media/app-service-logic-enterprise-integration-overview/overview-11.png)    

2. Στο πλαίσιο αναζήτησης "Φίλτρο", εισαγάγετε **ενοποίηση**και επιλέξτε **Λογαριασμοί ενοποίησης** από τη λίστα αποτελεσμάτων.     
![Στιγμιότυπο οθόνης του πλαισίου αναζήτησης φίλτρου](./media/app-service-logic-enterprise-integration-overview/overview-21.png)  
3. Επιλέξτε το **λογαριασμό ενοποίησης** στην οποία μπορείτε να προσθέσετε το σχήμα.    
![Στιγμιότυπο οθόνης της λίστας ενοποίηση λογαριασμών](./media/app-service-logic-enterprise-integration-overview/overview-31.png)  

4. Επιλέξτε το πλακίδιο **σχήματα** .  
![Στιγμιότυπο οθόνης της IEP ενοποίησης λογαριασμό, με "Σχήματα" Επισήμανση](./media/app-service-logic-enterprise-integration-schemas/schema-11.png)  

### <a name="add-a-schema-file-less-than-2-mb"></a>Προσθέστε ένα αρχείο σχήματος μικρότερη από 2 MB  

1. Στο το blade **σχήματα** που ανοίγει (από τα προηγούμενα βήματα), επιλέξτε **Προσθήκη**.  
![Στιγμιότυπο οθόνης των σχημάτων blade, με επισημασμένο το κουμπί "Προσθήκη"](./media/app-service-logic-enterprise-integration-schemas/schema-21.png)  

2. Πληκτρολογήστε ένα όνομα για το σχήμα. Στη συνέχεια, για να αποστείλετε το αρχείο σχήματος, επιλέξτε το εικονίδιο φακέλου δίπλα στο πλαίσιο κειμένου του **σχήματος** . Μετά την ολοκλήρωση της διαδικασίας αποστολής, επιλέξτε **OK**.    
![Στιγμιότυπο οθόνης "Προσθήκη σχήματος", με "Μικρές αρχείο" Επισήμανση](./media/app-service-logic-enterprise-integration-schemas/schema-31.png)  

### <a name="add-a-schema-file-larger-than-2-mb-up-to-a-maximum-of-8-mb"></a>Προσθήκη αρχείου σχήματος μεγαλύτερο από 2 MB (έως και 8 MB)  

Η διαδικασία για αυτό εξαρτάται από το επίπεδο πρόσβασης κοντέινερ αντικειμένων blob: **δημόσια** ή **χωρίς ανώνυμης πρόσβασης**. Για να προσδιορίσετε αυτό το επίπεδο πρόσβασης, στην **Εξερεύνηση χώρου αποθήκευσης Azure**, στην περιοχή **Blob κοντέινερ**, επιλέξτε το κοντέινερ αντικειμένων blob που θέλετε. Επιλέξτε " **ασφάλεια**" και επιλέξτε την καρτέλα **Επίπεδο πρόσβασης** .

1. Εάν το επίπεδο πρόσβασης ασφαλείας blob είναι **δημόσια**, ακολουθήστε τα παρακάτω βήματα.  
  ![Στιγμιότυπο οθόνης της Azure αποθήκευσης Εξερεύνησης, με "Blob κοντέινερ", "Ασφάλεια" και "Δημόσια" Επισήμανση](./media/app-service-logic-enterprise-integration-schemas/blob-public.png)  

    μια. Αποστολή του σχήματος με το χώρο αποθήκευσης και αντιγράψτε το URI.  
    ![Στιγμιότυπο οθόνης από το λογαριασμό χώρου αποθήκευσης, με επισήμανση URI](./media/app-service-logic-enterprise-integration-schemas/schema-blob.png)  

    β. **Προσθήκη σχήματος**, επιλέξτε **μεγάλο αρχείο**και δώστε το URI στο πλαίσιο κειμένου **URI περιεχομένου** .  
    ![Στιγμιότυπο οθόνης των σχημάτων, με το κουμπί "Προσθήκη" και "Μεγάλο αρχείο" Επισήμανση](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

2. Εάν το επίπεδο πρόσβασης ασφαλείας blob είναι **χωρίς ανώνυμη πρόσβαση**, ακολουθήστε τα παρακάτω βήματα.  
  ![Στιγμιότυπο οθόνης της Azure αποθήκευσης Εξερεύνησης, με "Blob κοντέινερ", "Ασφάλεια" και "Χωρίς ανώνυμης πρόσβασης" Επισήμανση](./media/app-service-logic-enterprise-integration-schemas/blob-1.png)  

    μια. Κάντε αποστολή του σχήματος με το χώρο αποθήκευσης.  
    ![Στιγμιότυπο οθόνης από το λογαριασμό χώρου αποθήκευσης](./media/app-service-logic-enterprise-integration-schemas/blob-3.png)

    β. Δημιουργία υπογραφής κοινόχρηστη πρόσβαση για το σχήμα.  
    ![Στιγμιότυπο οθόνης του χώρου αποθήκευσης sccount, με επισήμανση στην καρτέλα υπογραφών κοινόχρηστη πρόσβαση](./media/app-service-logic-enterprise-integration-schemas/blob-2.png)

    c. **Προσθήκη σχήματος**, επιλέξτε **μεγάλο αρχείο**και δώστε την υπογραφή κοινόχρηστη πρόσβαση URI στο πλαίσιο κειμένου **URI περιεχομένου** .  
    ![Στιγμιότυπο οθόνης των σχημάτων, με το κουμπί "Προσθήκη" και "Μεγάλο αρχείο" Επισήμανση](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

3. Στο το blade **σχήματα** του λογαριασμού ενοποίησης EIP, θα πρέπει τώρα βλέπετε το σχήμα που προστέθηκε πρόσφατα.  
![Στιγμιότυπο οθόνης της EIP ενοποίησης λογαριασμό, με "Σχήματα" και το νέο σχήμα επισημασμένο](./media/app-service-logic-enterprise-integration-schemas/schema-41.png)
  

## <a name="edit-schemas"></a>Επεξεργασία σχημάτων
1. Επιλέξτε το πλακίδιο **σχήματα** .  
2. Επιλέξτε το σχήμα που θέλετε να επεξεργαστείτε από το blade **σχήματα** που ανοίγει.
3. Στην blade τα **σχήματα** , επιλέξτε **Επεξεργασία**.  
![Στιγμιότυπο οθόνης των σχημάτων blade](./media/app-service-logic-enterprise-integration-schemas/edit-12.png)    
4. Επιλέξτε το αρχείο σχήματος που θέλετε να επεξεργαστείτε, χρησιμοποιώντας το παράθυρο διαλόγου Επιλογή αρχείου που ανοίγει.
5. Επιλέξτε " **Άνοιγμα** " στην επιλογή αρχείο.  
![Στιγμιότυπο οθόνης της επιλογής αρχείου](./media/app-service-logic-enterprise-integration-schemas/edit-31.png)  
6. Λαμβάνετε μια ειδοποίηση που υποδεικνύει η αποστολή ολοκληρώθηκε με επιτυχία.  

## <a name="delete-schemas"></a>Διαγραφή σχημάτων
1. Επιλέξτε το πλακίδιο **σχήματα** .  
2. Επιλέξτε το σχήμα που θέλετε να διαγράψετε από το blade **σχήματα** που ανοίγει.  
3. Στην blade τα **σχήματα** , επιλέξτε **Διαγραφή**.
![Στιγμιότυπο οθόνης των σχημάτων blade](./media/app-service-logic-enterprise-integration-schemas/delete-12.png)  

4. Για να επιβεβαιώσετε την επιλογή σας, επιλέξτε **Ναι**.  
![Στιγμιότυπο οθόνης του μηνύματος επιβεβαίωσης "Διαγραφή σχήματος"](./media/app-service-logic-enterprise-integration-schemas/delete-21.png)  
5. Τέλος, παρατηρήστε ότι η λίστα των σχημάτων στο τα **σχήματα** blade ανανεώνει και δεν εμφανίζεται πλέον το σχήμα που διαγράψατε.  
![Στιγμιότυπο οθόνης της EIP ενοποίησης λογαριασμό, με "Σχήματα" Επισήμανση](./media/app-service-logic-enterprise-integration-schemas/delete-31.png)    

## <a name="next-steps"></a>Επόμενα βήματα

- [Μάθετε περισσότερα σχετικά με το πακέτο ενοποίησης για μεγάλες επιχειρήσεις] (./app-service-logic-enterprise-integration-overview.md "Μάθετε σχετικά με το πακέτο ενοποίησης για μεγάλες επιχειρήσεις").  
