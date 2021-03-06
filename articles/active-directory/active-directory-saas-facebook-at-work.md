<properties
    pageTitle="Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με το Facebook στο χώρο εργασίας | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους καθολικής σύνδεσης μεταξύ Azure Active Directory και το Facebook στο χώρο εργασίας."
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
    ms.date="04/26/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-facebook-at-work"></a>Πρόγραμμα εκμάθησης: Ενοποίηση καταλόγου Azure Active Directory με το Facebook στο χώρο εργασίας

Ο στόχος αυτού του προγράμματος εκμάθησης είναι να σας δείξουν πώς μπορείτε να ενοποιήσετε το Facebook κατά την εργασία με το Azure Active Directory (Azure AD).

Ενοποίηση του Facebook στην εργασία με το Azure AD σάς παρέχει τα ακόλουθα πλεονεκτήματα: 

- Μπορείτε να ελέγξετε σε Azure AD ποιος έχει πρόσβαση στο Facebook στο χώρο εργασίας 
- Μπορείτε αυτόματα να παροχή του λογαριασμού για τους χρήστες που έχουν πρόσβαση στο Facebook στην εργασία
- Μπορείτε να ενεργοποιήσετε τους χρήστες σας να αυτόματα να πραγματοποιήσει-σε στο Facebook στην εργασία (καθολικής σύνδεσης) με τους λογαριασμούς Azure AD
- Μπορείτε να διαχειριστείτε τους λογαριασμούς σας σε μια κεντρική θέση 

Εάν θέλετε να μάθετε περισσότερες λεπτομέρειες σχετικά με ΑΔΑ εφαρμογή ενοποίηση με το Azure AD, ανατρέξτε στο θέμα [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία 

Για να ρυθμίσετε τις παραμέτρους ενοποίησης Azure AD με αστέρια στρατηγική συνεργασίας ανά ΧΏΡΑ, χρειάζεστε τα ακόλουθα στοιχεία:

- Μια συνδρομή του Azure AD
- Facebook στην εργασία με καθολική σύνδεση ενεργοποιημένη

Για να ελέγξετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να ακολουθήσετε αυτές τις συστάσεις:

- Δεν πρέπει να χρησιμοποιείτε το περιβάλλον παραγωγής, εκτός εάν αυτό είναι απαραίτητο.
- Εάν δεν έχετε ένα περιβάλλον δοκιμαστική Azure AD, μπορείτε να αποκτήσετε μια μηνιαία δοκιμαστική [εδώ](https://azure.microsoft.com/pricing/free-trial/). 


## <a name="adding-facebook-at-work-from-the-gallery"></a>Προσθήκη του Facebook στην εργασία από τη συλλογή
Για να ρυθμίσετε την ενοποίηση του Facebook στην εργασία σε Azure AD, πρέπει να προσθέσετε Facebook στην εργασία από τη συλλογή στη λίστα των διαχειριζόμενων ΑΔΑ εφαρμογών.

**Για να προσθέσετε Facebook στην εργασία από τη συλλογή, ακολουθήστε τα παρακάτω βήματα:**

1. Στην **πύλη του Azure κλασική**, στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**. 

    ![Υπηρεσία καταλόγου Active Directory][1]

2. Από τη λίστα **καταλόγου** , επιλέξτε τον κατάλογο για την οποία θέλετε να ενεργοποιήσετε την ενοποίηση καταλόγου.

3. Για να ανοίξετε τις εφαρμογές προβολή, στην προβολή του καταλόγου, κάντε κλικ στην επιλογή **εφαρμογές** στο μενού επάνω.

    ![Εφαρμογές][2]

4. Κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω μέρος της σελίδας.
    
    ![Εφαρμογές][3]

5. Στο παράθυρο διαλόγου **Τι θέλετε να κάνετε** , κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής από τη συλλογή**.

    ![Εφαρμογές][4]

6. Στο πλαίσιο αναζήτησης, πληκτρολογήστε **Facebook στην εργασία**.

7. Στο παράθυρο αποτελεσμάτων, επιλέξτε **Facebook στην εργασία**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** , για να προσθέσετε την εφαρμογή.


### <a name="configuring-azure-ad-single-sign-on"></a>Ρύθμιση παραμέτρων Azure AD καθολικής σύνδεσης

Αυτή η ενότητα περιγράφει πώς μπορείτε να επιτρέψετε στους χρήστες να ελέγχουν την ταυτότητα στο Facebook κατά την εργασία με το λογαριασμό στο Azure Active Directory, χρησιμοποιώντας Ομοσπονδία με βάση το πρωτόκολλο SAML.

**Για να ρυθμίσετε τις παραμέτρους Azure AD καθολικής σύνδεσης με το Facebook στο χώρο εργασίας, ακολουθήστε τα παρακάτω βήματα:**

1.  Μετά την προσθήκη Facebook στην εργασία στην πύλη του Azure κλασική, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων Καθολικής σύνδεσης**.

2.  Στην οθόνη της **Διεύθυνσης URL ρύθμιση παραμέτρων εφαρμογής** , πληκτρολογήστε τη διεύθυνση URL, όπου οι χρήστες θα συνδεθείτε στο σας στο Facebook στο εργασίας της εφαρμογής. Αυτό είναι το Facebook σε διεύθυνση URL μισθωτή εργασία (παράδειγμα: https://example.facebook.com/). Αφού ολοκληρώσετε τη διαδικασία, κάντε κλικ στο κουμπί **Επόμενο**.

3.  Σε ένα παράθυρο προγράμματος περιήγησης web διαφορετικά, συνδεθείτε στο σας στο Facebook στην τοποθεσία της εταιρείας εργασίας ως διαχειριστής.

4. Ακολουθήστε τις οδηγίες στην εξής διεύθυνση URL για τη ρύθμιση παραμέτρων του Facebook στην εργασία για να χρησιμοποιήσετε Azure AD ως υπηρεσία παροχής ταυτότητας: [https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers)

5.  Όταν ολοκληρωθεί, επιστρέψτε στο τα παράθυρα του προγράμματος περιήγησης που εμφανίζει την πύλη του Azure κλασική, επιλέξτε το πλαίσιο ελέγχου για να επιβεβαιώσετε την έχετε ολοκληρώσει τη διαδικασία και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο** και **ολοκλήρωσης**.


## <a name="automatically-provisioning-users-to-facebook-at-work"></a>Αυτόματη προμήθεια στους χρήστες να Facebook στο χώρο εργασίας

Azure AD υποστηρίζει τη δυνατότητα να synchonize αυτόματα τις λεπτομέρειες του λογαριασμού που του έχουν ανατεθεί χρηστών στο Facebook στην εργασία. Αυτό αυτόματων sychronization επιτρέπει Facebook στην εργασία για να λάβετε τα δεδομένα που χρειάζεται για να εξουσιοδοτήσετε χρήστες για πρόσβαση, πριν από τις επιχειρήσετε να συνδεθείτε στο για πρώτη φορά. Το επίσης καταργήστε την διατάξεις τους χρήστες από το Facebook στο χώρο εργασίας όταν έχει ανακληθεί πρόσβαση στο Azure AD.

Αυτόματη παροχή μπορεί να ρυθμιστεί κάνοντας κλικ στην επιλογή **Ρύθμιση παραμέτρων λογαριασμού προμήθεια** στο παράθυρο πύλης κλασική Azure του.

Για περισσότερες λεπτομέρειες σχετικά με τη ρύθμιση αυτόματων προμήθειας, ανατρέξτε στο θέμα [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)


## <a name="assigning-users-to-facebook-at-work"></a>Αντιστοίχιση χρηστών στο Facebook στο χώρο εργασίας

Για την προμήθεια του φακέλου AAD από τους χρήστες να μπορούν να βλέπουν Facebook στο χώρο εργασίας στον πίνακα τους πρόσβασης, πρέπει να αντιστοιχιστούν access μέσα Azure κλασική πύλη.

**Για να εκχωρήσετε στους χρήστες στο Facebook στο χώρο εργασίας:**

1.  Στη σελίδα έναρξης για το Facebook στο χώρο εργασίας στην πύλη του Azure κλασική, κάντε κλικ στην επιλογή **Αντιστοίχιση λογαριασμών**.

2.  Στο μενού " **Εμφάνιση** ", επιλέξτε εάν θέλετε να αντιστοιχίσετε ένα χρήστη ή μια ομάδα στο Facebook στην εργασία και κάντε κλικ στο κουμπί σημάδι ελέγχου.

3.  Στη λίστα που προκύπτει, επιλέξτε τους χρήστες ή ομάδα στο οποίο θέλετε να αντιστοιχίσετε Facebook στην εργασία.

4.  Στο υποσέλιδο της σελίδας, κάντε κλικ στο κουμπί **Αντιστοίχιση** .


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Λίστα εκμάθησης σχετικά με τον τρόπο ενσωμάτωσης εφαρμογές ΑΔΑ καταλόγου Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory;](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png




