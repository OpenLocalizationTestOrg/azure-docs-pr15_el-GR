<properties 
    pageTitle="Μάθετε περισσότερα σχετικά με περιορισμού στις υπηρεσίες BizTalk | Microsoft Azure" 
    description="Μάθετε περισσότερα σχετικά με ορίων περιορισμού και που προκύπτουν κατά το χρόνο εκτέλεσης συμπεριφορές για τις υπηρεσίες BizTalk. Περιορισμού βασίζεται σε χρήση μνήμης και τον αριθμό των μηνυμάτων. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>





# <a name="biztalk-services-throttling"></a>Υπηρεσίες BizTalk: περιορισμού

Azure υπηρεσίες BizTalk υλοποιεί υπηρεσίας περιορισμού που βασίζεται σε δύο συνθήκες: χρήση μνήμης και τον αριθμό των μηνυμάτων ταυτόχρονη επεξεργασία. Αυτό το θέμα παραθέτει τα όρια επιτάχυνσης και περιγράφει τη συμπεριφορά χρόνου εκτέλεσης όταν προκύπτει μια συνθήκη επιτάχυνσης.

## <a name="throttling-thresholds"></a>Ορίων περιορισμού

Ο παρακάτω πίνακας παραθέτει επιτάχυνσης προέλευσης και όρια:

||Περιγραφή|Χαμηλή όριο|Όριο υψηλής|
|---|---|---|---|
|Μνήμη|% του συνολικό σύστημα μνήμης διαθέσιμη/PageFileBytes. <p><p>Συνολικός διαθέσιμος PageFileBytes περίπου είναι 2 επί τη μνήμη RAM του συστήματος.|60%|70%|
|Επεξεργασία μηνυμάτων|Αριθμός των μηνυμάτων ταυτόχρονη επεξεργασία|40 * αριθμός πυρήνων|100 * αριθμός πυρήνων|

Όταν φτάσει ένα όριο υψηλής, υπηρεσίες BizTalk Azure αρχίζει να επιτάχυνσης. Περιορισμού σταματά όταν φτάσει το όριο χαμηλή. Για παράδειγμα, την υπηρεσία χρησιμοποιεί μνήμη συστήματος 65%. Σε αυτήν την περίπτωση, η υπηρεσία δεν επιτάχυνσης. Υπηρεσία σας ξεκινά με 70% μνήμη του συστήματος. Σε αυτήν την περίπτωση, η υπηρεσία throttles και συνεχίζει να επιτάχυνσης μέχρι την υπηρεσία χρησιμοποιεί μνήμη συστήματος 60% (χαμηλή όριο).

Azure BizTalk υπηρεσιών παρακολουθεί την κατάσταση επιτάχυνσης (κανονική κατάσταση έναντι επιτάχυνσης κατάσταση) και της επιτάχυνσης διάρκειας.


## <a name="runtime-behavior"></a>Συμπεριφορά χρόνου εκτέλεσης

Όταν υπηρεσίες BizTalk Azure μεταβαίνει σε κατάσταση επιτάχυνσης, συμβαίνουν τα εξής:

- Περιορισμού είναι ανά ρόλο παρουσία. Για παράδειγμα:<br/>
RoleInstanceA περιορισμού. RoleInstanceB δεν περιορισμού. Σε αυτήν την περίπτωση, μηνύματα στο RoleInstanceB υποβάλλονται σε επεξεργασία με τον αναμενόμενο τρόπο. Μηνύματα σε RoleInstanceA απορρίπτονται και αποτύχει με το ακόλουθο σφάλμα:<br/><br/>
**Ο διακομιστής είναι απασχολημένος. Προσπαθήστε ξανά.**<br/><br/>
- Οποιαδήποτε προελεύσεις ελκυστική μην ψηφοφορία ή λήψη ενός μηνύματος. Για παράδειγμα:<br/>
Μια διαδικασία χρησιμοποιεί τα μηνύματα από μια εξωτερική προέλευση FTP. Λαμβάνει την ρόλο παρουσία κάνοντας τα ελκυστική σε κατάσταση επιτάχυνσης. Σε αυτήν την περίπτωση, της διοχέτευσης διακόπτει τη λήψη πρόσθετα μηνύματα μέχρι να σταματήσει την παρουσία του ρόλου περιορισμού.
- Απόκριση αποστέλλεται στον υπολογιστή-πελάτη, ώστε το πρόγραμμα-πελάτη μπορούν να υποβάλετε ξανά το μήνυμα.
- Θα πρέπει να περιμένετε μέχρι το περιορισμού έχει επιλυθεί. Συγκεκριμένα, πρέπει να περιμένετε μέχρι να φτάσει το όριο χαμηλή.

## <a name="important-notes"></a>Σημαντικές σημειώσεις
- Περιορισμού δεν μπορεί να απενεργοποιηθεί.
- Δεν είναι δυνατή η τροποποίηση ορίων περιορισμού.
- Περιορισμού έχει υλοποιηθεί σε ολόκληρο το σύστημα.
- Το διακομιστή βάσης δεδομένων SQL Azure έχει επίσης ενσωματωμένες περιορισμού.

## <a name="additional-azure-biztalk-services-topics"></a>Πρόσθετα θέματα υπηρεσιών BizTalk Azure

-  [Κατά την εγκατάσταση των υπηρεσιών Azure BizTalk SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Προγράμματα εκμάθησης για: Υπηρεσίες Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Πώς μπορώ να ξεκινήσετε με το Azure BizTalk υπηρεσίες SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Υπηρεσίες Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Δείτε επίσης
- [Υπηρεσίες BizTalk: Προγραμματιστής, Basic, τυπική και γράφημα εκδόσεις Premium](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk υπηρεσίες: Κλασική πύλη χρησιμοποιώντας Azure προμήθεια](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [Υπηρεσίες BizTalk: Προμήθεια του γραφήματος κατάστασης](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk υπηρεσίες: Καρτέλες πίνακα εργαλείων, εποπτεία και κλίμακας](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk υπηρεσίες: Δημιουργία αντιγράφων ασφαλείας και επαναφορά](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk υπηρεσίες: Όνομα εκδότη και το κλειδί εκδότη](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
 