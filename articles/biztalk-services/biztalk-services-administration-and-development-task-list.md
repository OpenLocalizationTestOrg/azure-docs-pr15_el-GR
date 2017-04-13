<properties
    pageTitle="Λίστα στις υπηρεσίες BizTalk εργασιών διαχείρισης και την ανάπτυξη | Microsoft Azure"
    description="Σχεδιασμός και εργασία Βοήθεια για την ανάπτυξη των υπηρεσιών Azure BizTalk Services."
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="administration-and-development-task-list-in-biztalk-services"></a>Διαχείριση και στη λίστα εργασιών ανάπτυξης στις υπηρεσίες BizTalk  

## <a name="getting-started"></a>Γρήγορα αποτελέσματα
Όταν εργάζεστε με το Microsoft Azure BizTalk Services, υπάρχουν αρκετές εσωτερικής εγκατάστασης και στοιχεία που βασίζεται στο cloud για να λάβετε υπόψη σας. Για να ξεκινήσετε, λάβετε υπόψη τα παρακάτω ροής διεργασίας:  

|Βήμα|Ποιος είναι υπεύθυνος|Εργασία|Σχετικές συνδέσεις|
|----|----|----|----|
|1.|Διαχειριστής|Δημιουργία της εγγραφής Azure Microsoft χρησιμοποιώντας ένα λογαριασμό Microsoft ή έναν εταιρικό λογαριασμό|[Azure κλασική πύλη](http://go.microsoft.com/fwlink/p/?LinkID=213885)|
|2.|Διαχειριστής|Δημιουργία ή παροχή υπηρεσίας BizTalk.|[Δημιουργία μιας υπηρεσίας BizTalk με Azure κλασική πύλη](http://go.microsoft.com/fwlink/p/?LinkID=302280)|
|3.|Διαχειριστής|Καταχώρηση που ή ανάπτυξη υπηρεσιών BizTalk της εταιρείας σας|[Καταχώρηση και ενημέρωση μιας ανάπτυξης BizTalk υπηρεσίας στην πύλη υπηρεσιών BizTalk](https://msdn.microsoft.com/library/azure/hh689837.aspx)|
|4.|Διαχειριστής|Εφαρμογή εάν η εφαρμογή χρησιμοποιεί BizTalk προσαρμογέα υπηρεσίας για να συνδεθείτε σε ένα σύστημα γραμμής εταιρικά (LOB) εσωτερικής εγκατάστασης ή χρησιμοποιεί ουρά ή το θέμα προορισμού.  Δημιουργήστε το Namespace Bus Azure υπηρεσίας. Δώστε αυτό χώρο ονομάτων, όνομα εκδότη Bus υπηρεσίας και τις τιμές υπηρεσία Bus κλειδί στον προγραμματιστή.|[Τον τρόπο: δημιουργία ή τροποποίηση μιας υπηρεσίας Bus υπηρεσίας Namespace](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) και [λάβετε όνομα εκδότη και τιμές κλειδιού εκδότη](biztalk-issuer-name-issuer-key.md)|
|5.|Για προγραμματιστές|Εγκατάσταση του SDK και δημιουργήστε το έργο BizTalk υπηρεσίας στο Visual Studio.|[Εγκατάσταση των υπηρεσιών Azure BizTalk SDK](https://msdn.microsoft.com/library/azure/hh689760.aspx) και [Δημιουργία εμπλουτισμένων τελικά σημεία ανταλλαγής μηνυμάτων σε Azure](https://msdn.microsoft.com/library/azure/hh689766.aspx)|
|6.|Για προγραμματιστές|Ανάπτυξη της υπηρεσίας BizTalk έργου στην υπηρεσία σας BizTalk φιλοξενούνται σε Azure.|[Ανάπτυξη και η ανανέωση του έργου υπηρεσίες BizTalk](https://msdn.microsoft.com/library/azure/hh689881.aspx)|
|7.|Διαχειριστής|Ισχύουν εάν χρησιμοποιείτε Επεξεργασία.  Μπορείτε να προσθέσετε συνεργάτες και να δημιουργήσετε συμφωνίες στην πύλη υπηρεσίες του Microsoft Azure BizTalk. Όταν δημιουργείτε μια συμφωνία, μπορείτε να προσθέσετε το bridge ή/και μετασχηματισμούς που δημιουργήθηκε από τον προγραμματιστή για τις ρυθμίσεις της άδειας.|[Ρύθμιση παραμέτρων Επεξεργασία, AS2 και EDIFACT στην πύλη υπηρεσιών BizTalk](https://msdn.microsoft.com/library/azure/hh689853.aspx)|
|8.|Διαχειριστής|Με την πύλη Azure κλασική, παρακολουθεί την εύρυθμη λειτουργία της υπηρεσίας σας BizTalk, συμπεριλαμβανομένων των μετρικών απόδοσης.|[BizTalk υπηρεσίες: Καρτέλες πίνακα εργαλείων, εποπτεία και κλίμακας](http://go.microsoft.com/fwlink/p/?LinkID=302281)|
|9.|Διαχειριστής|Με την πύλη Microsoft Azure BizTalk υπηρεσίες, να διαχειριστούν τα αντικείμενα που χρησιμοποιούνται από τις υπηρεσίες BizTalk και παρακολούθηση μηνυμάτων κατά την επεξεργασία τους γίνεται με τα αρχεία γέφυρα.|[Με την πύλη υπηρεσιών BizTalk](https://msdn.microsoft.com/library/azure/dn874043.aspx)|
|10.|Διαχειριστής|Δημιουργήστε ένα σχέδιο δημιουργίας αντιγράφων ασφαλείας για να δημιουργήσετε αντίγραφα ασφαλείας της υπηρεσίας BizTalk.|[Αδιάκοπη παροχή υπηρεσιών και αποκατάσταση στις υπηρεσίες BizTalk](https://msdn.microsoft.com/library/azure/dn509557.aspx) |  
## <a name="next-steps"></a>Επόμενα βήματα
[Προγράμματα εκμάθησης και δείγματα](https://msdn.microsoft.com/library/azure/hh689895.aspx)

[Δημιουργήστε το έργο στο Visual Studio](https://msdn.microsoft.com/library/azure/hh689811.aspx)

[Εγκατάσταση των υπηρεσιών Azure BizTalk SDK](https://msdn.microsoft.com/library/azure/hh689760.aspx)

## <a name="concepts"></a>Έννοιες
[Δημιουργήστε το έργο στο Visual Studio](https://msdn.microsoft.com/library/azure/hh689811.aspx)  
[Επεξεργασία, AS2 και EDIFACT μηνυμάτων (Business για επιχειρήσεις)](https://msdn.microsoft.com/library/azure/hh689898.aspx)  
## <a name="other-resources"></a>Άλλοι πόροι  
[Προσθήκη προέλευσης, προορισμού και γέφυρα μηνυμάτων τελικά σημεία](https://msdn.microsoft.com/library/azure/hh689877.aspx)  
[Μάθετε και να δημιουργήσετε χάρτες μήνυμα και μετασχηματισμούς](https://msdn.microsoft.com/library/azure/hh689905.aspx)  
[Με την υπηρεσία προσαρμογέα BizTalk (BAS)](https://msdn.microsoft.com/library/azure/hh689889.aspx)  
[Υπηρεσίες Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=303664)
