<properties 
    pageTitle="Δημιουργία και διαχείριση συνδέσεων υβριδική | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια σύνδεση υβριδική, διαχειριστείτε τη σύνδεση και εγκατάσταση της υβριδικής διαχείρισης σύνδεσης. MABS, WABS" 
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
    ms.date="10/18/2016" 
    ms.author="ccompy"/>


# <a name="create-and-manage-hybrid-connections"></a>Δημιουργία και διαχείριση συνδέσεων υβριδική


## <a name="overview-of-the-steps"></a>Επισκόπηση των βημάτων
1. Δημιουργήστε μια σύνδεση υβριδική, πληκτρολογώντας το **όνομα κεντρικού υπολογιστή** ή το **FQDN** του πόρου εσωτερικής εγκατάστασης στο δίκτυό σας ιδιωτικό.
2. Σύνδεση σας εφαρμογές Azure web ή Azure εφαρμογές για κινητές συσκευές με τη σύνδεση υβριδική.
3. Εγκατάσταση της υβριδικής διαχείρισης σύνδεσης σε τον πόρο στην εσωτερική εγκατάσταση και συνδεθείτε με τη συγκεκριμένη σύνδεση υβριδική. Η πύλη του Azure παρέχει μια εμπειρία μονό κλικ για να εγκαταστήσετε και να συνδέσετε.
4. Διαχείριση συνδέσεων υβριδική και τους αριθμούς-κλειδιά σύνδεσης.

Αυτό το θέμα παραθέτει τα παρακάτω βήματα. 

> [AZURE.IMPORTANT] Είναι δυνατή για να ορίσετε ένα τελικό σημείο σύνδεσης υβριδική σε μια διεύθυνση IP. Εάν χρησιμοποιείτε μια διεύθυνση IP, ενδέχεται να ή δεν μπορεί να φτάσει τον πόρο στην εσωτερική εγκατάσταση, ανάλογα με το πρόγραμμα-πελάτη. Η σύνδεση υβριδική εξαρτάται από το πρόγραμμα-πελάτη κάνοντας αναζήτηση DNS. Στις περισσότερες περιπτώσεις, ο __υπολογιστής-πελάτης__ είναι κώδικα της εφαρμογής σας. Εάν ο υπολογιστής-πελάτης δεν εκτελεί μια αναζήτηση DNS, (δεν επιχειρεί να επιλύσει τη διεύθυνση IP, σαν να ήταν ένα όνομα τομέα (x.x.x.x)), στη συνέχεια, η κίνηση δεν αποστέλλεται μέσω της σύνδεσης υβριδική.
>
> Για παράδειγμα (pseudocode), μπορείτε να ορίσετε **10.4.5.6** ως υπηρεσία παροχής φιλοξενίας εσωτερικής εγκατάστασης:
> 
> **Λειτουργεί το ακόλουθο σενάριο:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **Το ακόλουθο σενάριο δεν λειτουργεί:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`


## <a name="CreateHybridConnection"></a>Δημιουργία σύνδεσης υβριδική

Μια σύνδεση υβριδική μπορούν να δημιουργηθούν στην πύλη του Azure χρησιμοποιώντας εφαρμογές Web **ή** χρήση των υπηρεσιών BizTalk. 

**Για να δημιουργήσετε συνδέσεις υβριδική χρησιμοποιώντας εφαρμογές Web**, ανατρέξτε στο θέμα [Σύνδεση του Azure Web Apps σε έναν πόρο εσωτερικής εγκατάστασης](../app-service-web/web-sites-hybrid-connection-get-started.md). Μπορείτε επίσης να εγκαταστήσετε το υβριδικό Manager σύνδεσης (HCM) από την εφαρμογή web, η οποία είναι η προτιμώμενη μέθοδος. 

**Για να δημιουργήσετε συνδέσεις υβριδική στις υπηρεσίες BizTalk**:

1. Πραγματοποιήστε είσοδο στο [Azure κλασική πύλη](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Στο αριστερό παράθυρο περιήγησης, επιλέξτε **Υπηρεσίες BizTalk** και, στη συνέχεια, επιλέξτε την υπηρεσία BizTalk. 

    Εάν δεν έχετε μια υπάρχουσα υπηρεσία BizTalk, μπορείτε να [Δημιουργία υπηρεσίας BizTalk](biztalk-provision-services.md).
3. Επιλέξτε την καρτέλα **Υβριδική συνδέσεις** :  
![Καρτέλα υβριδική συνδέσεις][HybridConnectionTab]

4. Επιλέξτε **Δημιουργία σύνδεσης υβριδική** ή επιλέξτε το κουμπί **ΠΡΟΣΘΉΚΗ** στη γραμμή εργασιών. Εισαγάγετε τα εξής:

    Ιδιότητα | Περιγραφή
--- | ---
Όνομα | Το όνομα της υβριδικής σύνδεσης πρέπει να είναι μοναδικό και δεν μπορεί να είναι το ίδιο όνομα με την υπηρεσία BizTalk. Μπορείτε να εισαγάγετε οποιοδήποτε όνομα αλλά να είναι συγκεκριμένες με το σκοπό του. Παραδείγματα:<br/><br/>Μισθοδοσία*της*<br/>SupplyList*SharepointServer*<br/>Οι πελάτες*OracleServer*
Όνομα κεντρικού υπολογιστή | Πληκτρολογήστε το πλήρως προσδιορισμένο όνομα, μόνο το όνομα κεντρικού υπολογιστή ή τη διεύθυνση IPv4 του πόρου εσωτερικής εγκατάστασης. Παραδείγματα:<br/><br/>mySQLServer<br/>*mySQLServer*. *Τομέας*. corp.*yourCompany*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. το .com *yourCompany*<br/>10.100.10.10<br/><br/>Εάν χρησιμοποιείτε τη διεύθυνση IPv4, σημειώστε ότι το πρόγραμμα-πελάτη ή εφαρμογή κώδικα δεν μπορεί να επιλύσει τη διεύθυνση IP. Ανατρέξτε στο θέμα το σημαντικό σημείωση στο επάνω μέρος αυτού του θέματος.
Θύρα | Πληκτρολογήστε τον αριθμό θύρας του πόρου εσωτερικής εγκατάστασης. Για παράδειγμα, εάν χρησιμοποιείτε εφαρμογές Web, πληκτρολογήστε 80 ή θύρα 443. Εάν χρησιμοποιείτε το SQL Server, πληκτρολογήστε θύρα 1433.

5. Επιλέξτε το σημάδι ελέγχου για να ολοκληρώσετε τη ρύθμιση. 

#### <a name="additional"></a>Επιπλέον

- Πολλές συνδέσεις υβριδική μπορούν να δημιουργηθούν. Ανατρέξτε στο θέμα το [υπηρεσίες BizTalk: γράφημα εκδόσεις](biztalk-editions-feature-chart.md) για τον αριθμό των συνδέσεων που επιτρέπονται. 
- Κάθε σύνδεση υβριδική δημιουργείται με ένα ζεύγος συμβολοσειρές σύνδεσης: εφαρμογή κλειδιά που κλειδιά ΑΠΟΣΤΟΛΉΣ και εσωτερικής εγκατάστασης που κάνουν ΑΚΡΌΑΣΗ. Κάθε ζεύγος έχει ένα πρωτεύον και ένα δευτερεύον κλειδί. 


## <a name="LinkWebSite"></a>Σύνδεση του Azure εφαρμογής υπηρεσίας Web App ή εφαρμογή για κινητές συσκευές

Για να συνδέσετε μια εφαρμογή Web ή εφαρμογή Mobile στο Azure εφαρμογής υπηρεσίας μια υπάρχουσα σύνδεση υβριδική, επιλέξτε **Χρήση υπάρχουσας σύνδεσης υβριδική** στο το blade υβριδική συνδέσεις. Ανατρέξτε στο θέμα [πρόσβαση στην εσωτερική εγκατάσταση πόρων με χρήση της υβριδικής συνδέσεις στο Azure εφαρμογής υπηρεσίας](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="InstallHCM"></a>Εγκαταστήστε τη Διαχείριση σύνδεσης υβριδική εσωτερικής εγκατάστασης

Μετά τη δημιουργία μιας σύνδεσης υβριδική, εγκαταστήστε τη Διαχείριση σύνδεσης υβριδική στον πόρο εσωτερικής εγκατάστασης. Μπορούν να ληφθούν από τις εφαρμογές του Azure web ή από την υπηρεσία BizTalk. Υπηρεσίες BizTalk βήματα: 

1. Πραγματοποιήστε είσοδο στο [Azure κλασική πύλη](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Στο αριστερό παράθυρο περιήγησης, επιλέξτε **Υπηρεσίες BizTalk** και, στη συνέχεια, επιλέξτε την υπηρεσία BizTalk. 
3. Επιλέξτε την καρτέλα **Υβριδική συνδέσεις** :  
![Καρτέλα "Συνδέσεις" υβριδική][HybridConnectionTab]
4. Στη γραμμή εργασιών, επιλέξτε **Εσωτερικής εγκατάστασης**:  
![Εσωτερικής εγκατάστασης][HCOnPremSetup]
5. Επιλέξτε **εγκατάσταση και ρύθμιση παραμέτρων** για να εκτελέσετε ή να κάνετε λήψη στη Διαχείριση σύνδεσης υβριδική στο σύστημα εσωτερικής εγκατάστασης. 
6. Επιλέξτε το σημάδι ελέγχου για να ξεκινήσετε την εγκατάσταση. 

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Επιπλέον
- Διαχείριση συνδέσεων υβριδική μπορεί να εγκατασταθεί σε τα ακόλουθα λειτουργικά συστήματα:

    - Windows Server 2008 R2 (.NET Framework διαίρεσης 4,5 + και Windows Management Framework 4.0 + απαιτείται)
    - Windows Server 2012 (Windows Management Framework 4.0 + απαιτείται)
    - Windows Server 2012 R2


- Μετά την εγκατάσταση της υβριδικής διαχείρισης σύνδεσης, συμβαίνουν τα εξής: 

    - Η σύνδεση υβριδική φιλοξενούνται σε Azure αυτόματα έχει ρυθμιστεί ώστε να χρησιμοποιούν την κύρια συμβολοσειρά σύνδεσης εφαρμογής. 
    - Ο πόρος εσωτερικής εγκατάστασης αυτόματα έχει ρυθμιστεί ώστε να χρησιμοποιούν την κύρια συμβολοσειρά σύνδεσης εσωτερικής εγκατάστασης.

- Στη Διαχείριση σύνδεσης υβριδική πρέπει να χρησιμοποιήσετε μια συμβολοσειρά σύνδεσης έγκυρη εσωτερικής εγκατάστασης για την άδεια. Το Azure εφαρμογές Web ή εφαρμογές του Mobile πρέπει να χρησιμοποιήσετε μια εφαρμογή έγκυρη συμβολοσειρά σύνδεσης για την άδεια.
- Μπορείτε να κλιμάκωση συνδέσεις υβριδική εγκαθιστώντας μια άλλη παρουσία της διαχείρισης σύνδεσης υβριδική σε άλλο διακομιστή. Ρύθμιση παραμέτρων το ακροατήριο εσωτερικής εγκατάστασης για να χρησιμοποιήσετε την ίδια διεύθυνση ως η πρώτη ακρόαση εσωτερικής εγκατάστασης. Σε αυτήν την περίπτωση, η κίνηση είναι τυχαία κατανεμημένα (round robin) μεταξύ του ακροατών ενεργό εσωτερικής εγκατάστασης. 


## <a name="ManageHybridConnection"></a>Διαχείριση συνδέσεων υβριδική
Για να διαχειριστείτε τις συνδέσεις σας υβριδική, μπορείτε:

- Χρησιμοποιήστε την πύλη του Azure και μεταβείτε στην υπηρεσία σας BizTalk. 
- Χρησιμοποιήστε το [ΥΠΌΛΟΙΠΟ APIs](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Αντιγραφή/αναδημιουργία την υβριδική συμβολοσειρές σύνδεσης

1. Πραγματοποιήστε είσοδο στο [Azure κλασική πύλη](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Στο αριστερό παράθυρο περιήγησης, επιλέξτε **Υπηρεσίες BizTalk** και, στη συνέχεια, επιλέξτε την υπηρεσία BizTalk. 
3. Επιλέξτε την καρτέλα **Υβριδική συνδέσεις** :  
![Καρτέλα υβριδική συνδέσεις][HybridConnectionTab]
4. Επιλέξτε τη σύνδεση υβριδική. Στη γραμμή εργασιών, επιλέξτε **Διαχείριση σύνδεσης**:  
![Διαχείριση επιλογών][HCManageConnection]

    **Διαχείριση σύνδεσης** παραθέτει τις συμβολοσειρές σύνδεσης εφαρμογής και εσωτερικής εγκατάστασης. Μπορείτε να αντιγράψετε τις συμβολοσειρές σύνδεσης ή να δημιουργήσετε ξανά το κλειδί πρόσβασης που χρησιμοποιούνται στη συμβολοσειρά σύνδεσης. 

    **Εάν επιλέξετε αναδημιουργία**, το πλήκτρο πρόσβασης σε κοινή χρήση χρησιμοποιείται μέσα στη συμβολοσειρά σύνδεσης έχει αλλάξει. Κάντε τα εξής:
    - Στην κλασική Azure πύλη, επιλέξτε **Συγχρονισμός κλειδιά** στην εφαρμογή του Azure.
    - Εκτελέστε ξανά το **Εσωτερικής εγκατάστασης**. Όταν εκτελείτε εκ νέου τη διαμόρφωση της εσωτερικής εγκατάστασης, ο πόρος εσωτερικής ρυθμίζεται αυτόματα για να χρησιμοποιήσετε τη συμβολοσειρά σύνδεσης ενημερωμένη πρωτεύοντος.


#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Χρήση της πολιτικής ομάδας για να ελέγξετε τους πόρους εσωτερικής εγκατάστασης που χρησιμοποιούνται από μια σύνδεση υβριδική

1. Κάντε λήψη του τα [πρότυπα διαχείρισης υβριδική διαχείρισης σύνδεσης](http://www.microsoft.com/download/details.aspx?id=42963).
2. Εξαγωγή αρχείων.
3. Στον υπολογιστή που τροποποιεί πολιτικής ομάδας, κάντε τα εξής:  

    - Αντιγραφή του. ADMX αρχεία στο φάκελο *%WINROOT%\PolicyDefinitions* .
    - Αντιγραφή του. ADML αρχεία στο φάκελο *%WINROOT%\PolicyDefinitions\en-us* .

Μετά την αντιγραφή, μπορείτε να χρησιμοποιήσετε το πρόγραμμα επεξεργασίας πολιτικής ομάδας για να αλλάξετε την πολιτική.




## <a name="next"></a>Επόμενο

[Σύνδεση Azure Web Apps σε έναν πόρο στην εσωτερική εγκατάσταση](../app-service-web/web-sites-hybrid-connection-get-started.md)  
[Σύνδεση με τον SQL Server εσωτερικής εγκατάστασης από εφαρμογές Azure Web](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)   
[Επισκόπηση των συνδέσεων υβριδική](integration-hybrid-connection-overview.md)


## <a name="see-also"></a>Δείτε επίσης

[REST API για τη Διαχείριση BizTalk υπηρεσίες στο Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk υπηρεσίες: Γράφημα εκδόσεις](biztalk-editions-feature-chart.md)  
[Δημιουργία μιας υπηρεσίας BizTalk με Azure κλασική πύλη](biztalk-provision-services.md)  
[BizTalk υπηρεσίες: Καρτέλες πίνακα εργαλείων, εποπτεία και κλίμακας](biztalk-dashboard-monitor-scale-tabs.md)


[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
