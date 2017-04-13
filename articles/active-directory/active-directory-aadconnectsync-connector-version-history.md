<properties
   pageTitle="Ιστορικό κυκλοφορίας έκδοση σύνδεσης | Microsoft Azure"
   description="Αυτό το θέμα παραθέτει όλες τις εκδόσεις των συνδέσεων για Διαχείριση ταυτοτήτων Forefront (FIM) και Διαχείριση ταυτοτήτων Microsoft (MIM)"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="billmath"/>

# <a name="connector-version-release-history"></a>Γραμμή σύνδεσης έκδοση κυκλοφορίας ιστορικού
Τις συνδέσεις για Διαχείριση ταυτοτήτων Forefront (FIM) και Διαχείριση ταυτοτήτων Microsoft (MIM) ενημερώνονται συχνά.

>[AZURE.NOTE]
Αυτό το θέμα είναι μόνο σε FIM και MIM. Αυτές οι γραμμές σύνδεσης δεν υποστηρίζονται στο Azure AD Connect.

Αυτό το θέμα παραθέτει όλες τις εκδόσεις των συνδέσεων που έχουν κυκλοφορήσει.

Σχετικές συνδέσεις:

- [Λήψη πιο πρόσφατη γραμμών σύνδεσης](http://go.microsoft.com/fwlink/?LinkId=717495)
- Τεκμηρίωση αναφοράς [Γενικής χρήσης σύνδεσης LDAP](active-directory-aadconnectsync-connector-genericldap.md)
- Τεκμηρίωση αναφοράς [Γενικής χρήσης σύνδεσης SQL](active-directory-aadconnectsync-connector-genericsql.md)
- [Γραμμή σύνδεσης υπηρεσιών Web](http://go.microsoft.com/fwlink/?LinkID=226245) τεκμηρίωση αναφοράς
- Τεκμηρίωση αναφοράς [PowerShell σύνδεσης](active-directory-aadconnectsync-connector-powershell.md)
- Τεκμηρίωση αναφοράς [Lotus Domino σύνδεσης](active-directory-aadconnectsync-connector-domino.md)

## <a name="111170"></a>1.1.117.0
Κυκλοφορήσει: 2016 Μάρτιος

**Νέα γραμμή σύνδεσης**  
Αρχική έκδοση της [Γενικής χρήσης σύνδεσης SQL](active-directory-aadconnectsync-connector-genericsql.md).

**Νέες δυνατότητες:**

- Γραμμή σύνδεσης LDAP γενικής χρήσης:
    - Υποστήριξη που προστέθηκε για εισαγωγή delta με Isode.
- Γραμμή σύνδεσης υπηρεσιών Web:
    - Ενημέρωση της δραστηριότητας csEntryChangeResult και setImportErrorCode δραστηριότητας για να επιτρέψετε σε επίπεδο σφαλμάτων αντικειμένων πρέπει να επιστραφεί ξανά το μηχανισμό συγχρονισμού.
    - Ενημέρωση τα πρότυπα SAP6 και SAP6User για να χρησιμοποιήσετε τις νέες λειτουργίες σφάλμα επιπέδου αντικειμένου.
- Lotus Domino σύνδεσης:
    - Για εξαγωγή, χρειάζεστε ένα certifier ανά βιβλίο διευθύνσεων. Τώρα, μπορείτε να χρησιμοποιήσετε τον ίδιο κωδικό πρόσβασης για όλους certifiers για να διευκολύνετε τη διαχείριση.

**Σταθερή θέματα:**

- Γραμμή σύνδεσης LDAP γενικής χρήσης:
    - Για IBM Tivoli DS, ορισμένες ιδιότητες αναφοράς δεν εντοπίστηκαν σωστά.
    - Για άνοιγμα LDAP κατά την εισαγωγή ενός delta, τέλος στην αρχή και στο τέλος της συμβολοσειρές έχουν περικοπεί.
    - Για Novell και NetIQ, εξαγωγή που μετακινείται ένα αντικείμενο μεταξύ OU/κοντέινερ και την ίδια στιγμή μετονομαστεί το αντικείμενο απέτυχε.
- Γραμμή σύνδεσης υπηρεσιών Web:
    - Εάν η υπηρεσία web είχε πολλές τελικών σημείων ελέγχου για την ίδια σύνδεση, στη συνέχεια, η γραμμή σύνδεσης έχει αλλάξει δεν σωστά Ανακαλύψτε αυτών των τελικών σημείων ελέγχου.
- Lotus Domino σύνδεσης:
    - Εξαγωγή του χαρακτηριστικού ονοματεπώνυμο σε αλληλογραφίας στη βάση δεδομένων δεν λειτούργησε.
    - Εξαγωγή που προσθέσατε και Κατάργηση μέλους από ομάδα εξαγωγή μόνο τα μέλη που προσθέσατε.
    - Εάν ένα έγγραφο σημειώσεων είναι μη έγκυρη (η τιμή false isValid χαρακτηριστικό), στη συνέχεια, η γραμμή σύνδεσης αποτυγχάνει.

## <a name="older-releases"></a>Παλαιότερες εκδόσεις
Πριν από την Μαρτίου 2016, οι γραμμές σύνδεσης έχουν κυκλοφορήσει ως θέματα υποστήριξης.

**Γενικό LDAP**

- [KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597, Σεπτεμβρίου 2015
- [KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, Μαρτίου 2015
- [KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, Ιανουαρίου 2015
- [KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, Σεπτεμβρίου 2014
- [KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, Μαρτίου 2014

**WebServices**

- [KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, Σεπτεμβρίου 2014

**PowerShell**

- [KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, Σεπτεμβρίου 2014

**Lotus Domino**

- [KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597, Σεπτεμβρίου 2015
- [KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, Μαρτίου 2015
- [KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, Αυγούστου 2014
- [KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, Φεβρουαρίου 2014  
- [KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, Οκτωβρίου 2013
- [KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, Αυγούστου 2013

## <a name="next-steps"></a>Επόμενα βήματα
Μάθετε περισσότερα σχετικά με τη ρύθμιση παραμέτρων [Azure AD Connect συγχρονισμός](active-directory-aadconnectsync-whatis.md) .

Μάθετε περισσότερα σχετικά με την [ενσωμάτωση του ταυτοτήτων εσωτερικής εγκατάστασης με Azure Active Directory](active-directory-aadconnect.md).
