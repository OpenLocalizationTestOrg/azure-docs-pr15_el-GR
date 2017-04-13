<properties 
pageTitle="Ανάπτυξη SAP IDES EHP7 SP3 για SAP ERP 6.0 στο Microsoft Azure | Microsoft Azure" 
description="Ανάπτυξη SAP IDES EHP7 SP3 για SAP ERP 6.0 στο Microsoft Azure" 
services="virtual-machines-windows" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
keywords=""/> 
<tags 
ms.service="virtual-machines-windows" 
ms.devlang="na" 
ms.topic="article" 
ms.tgt_pltfrm="vm-windows" 
ms.workload="infrastructure-services" 
ms.date="09/16/2016" 
ms.author="hermannd"/> 


# <a name="deploying-sap-ides-ehp7-sp3-for-sap-erp-60-on-microsoft-azure"></a>Ανάπτυξη SAP IDES EHP7 SP3 για SAP ERP 6.0 στο Microsoft Azure 

Σε αυτό το άρθρο περιγράφει τον τρόπο ανάπτυξης SAP IDES εκτελείται με τον SQL Server και λειτουργικού Συστήματος των Windows σε Windows Azure μέσω SAP Cloud συσκευής βιβλιοθήκη 3.0. Τα στιγμιότυπα οθόνης εμφανίζουν τη διαδικασία βήμα προς βήμα. Ανάπτυξη άλλες λύσεις στη λίστα λειτουργεί τον ίδιο τρόπο από μια προοπτική διαδικασία. Μία έχει απλώς για να επιλέξετε μια διαφορετική λύση.

Για να ξεκινήσετε με SAP Cloud συσκευής βιβλιοθήκης (SAP CAL) μεταβείτε [εδώ](https://cal.sap.com/). Υπάρχει ένα ιστολόγιο από SAP σχετικά με το νέο [SAP Cloud συσκευής βιβλιοθήκη 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience). 


Το παρακάτω στιγμιότυπα οθόνης εμφάνιση βήμα προς βήμα τον τρόπο ανάπτυξης IDES SAP στο Microsoft Azure. Η διαδικασία λειτουργεί με τον ίδιο τρόπο για άλλες λύσεις.


![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

Στην πρώτη εικόνα εμφανίζει όλες τις λύσεις που είναι διαθέσιμες στο Microsoft Azure. Η επισήμανση λύση IDES SAP βασίζεται στα Windows, η οποία είναι διαθέσιμη μόνο σε Azure έχει επιλέξει να ακολουθήσετε τη διαδικασία.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic2.jpg)

Πρώτα ένα νέο λογαριασμό SAP CAL έχει δημιουργηθεί. Υπάρχουν δύο επιλογές για το Azure - τυπική Azure και Azure στην Κίνα ηπειρωτική που είναι με διαχείριση από 21Vianet συνεργάτη.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic3.jpg)

Στη συνέχεια, μία έχει για να εισαγάγετε το Αναγνωριστικό Azure συνδρομή που μπορεί να βρεθεί στην πύλη του Azure - επίσης να δείτε περαιτέρω προς τα κάτω Τρόπος απόκτησης. Στη συνέχεια ένα πιστοποιητικό Azure διαχείρισης πρέπει να γίνει λήψη.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic6.jpg)

Στο το νέο Azure πύλης μία εντοπίζει το στοιχείο "Συνδρομές" στην αριστερή πλευρά. Κάντε κλικ για να εμφανίσετε όλες τις ενεργές συνδρομές για το χρήστη.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic7.jpg)

Επιλέγοντας μία από τις συνδρομές και, στη συνέχεια, επιλέγοντας "Διαχείριση πιστοποιητικών" εξηγεί εκεί που είναι μια νέα έννοια με χρήση του "αρχές υπηρεσίας" για το νέο μοντέλο Azure διαχείριση πόρων.
SAP CAL δεν έχει προσαρμοστεί ακόμα για αυτό το νέο μοντέλο και εξακολουθεί να απαιτεί το μοντέλο "κλασική" και την πρώην Azure πύλη για να εργαστείτε με τα πιστοποιητικά διαχείρισης.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic4.jpg)

Δείτε ένα μπορούν να δουν την πύλη του πρώην Azure. Η αποστολή ενός πιστοποιητικού διαχείρισης παρέχει SAP CAL τα δικαιώματα για να δημιουργήσετε εικονικές μηχανές μιας συνδρομής του πελάτη. Στην περιοχή "ΣΥΝΔΡΟΜΈΣ", καρτέλα μία να βρείτε το Αναγνωριστικό συνδρομής που έχει να εισαχθεί στην πύλη του SAP CAL.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic5.jpg)

Στην καρτέλα δεύτερη, στη συνέχεια, είναι πιθανό να αποστείλετε το πιστοποιητικό διαχείρισης που έχει ληφθεί πριν από SAP CAL.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic8.jpg)

Ένα μικρό παράθυρο διαλόγου αναδύεται για να επιλέξετε το αρχείο πιστοποιητικού που έχετε λάβει.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic9.jpg)

Εφόσον το πιστοποιητικό έχει αποσταλεί της σύνδεσης μεταξύ του SAP CAL και ο πελάτης Azure συνδρομή μπορεί να ελεγχθεί εντός SAP CAl. Ένα μικρό μήνυμα θα πρέπει να αναδύεται που σας ενημερώνει ότι η σύνδεση είναι έγκυρη.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic10.jpg)

Μετά τη ρύθμιση ενός λογαριασμού μία έχει για να επιλέξετε μια λύση που θα πρέπει να αναπτυχθούν και να δημιουργήσετε μια περίοδο λειτουργίας.
Με τη λειτουργία "Βασική", είναι πραγματικά trivial. Πληκτρολογήστε ένα όνομα παρουσία, επιλέξτε μια περιοχή Azure και ορισμός κύριας κωδικού πρόσβασης για τη λύση.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic11.jpg)

Μετά από κάποιο χρονικό διάστημα, ανάλογα με το μέγεθος και την πολυπλοκότητα της λύσης (εκτίμηση δίνεται από SAP CAL) εμφανίζεται ως "ενεργό" και είναι έτοιμη για χρήση. Είναι πολύ απλής.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic12.jpg)

Ορισμένες των λεπτομερειών της λύσης μία μπορεί να δει το είδος του ΣΠΣ είχαν αναπτυχθεί. Σε αυτήν την περίπτωση υπάρχει ένα μεμονωμένο Εικονική Azure του μεγέθους D12 που δημιουργήθηκε από SAP CAL.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic13.jpg)

Στην πύλη του Azure, μπορείτε να βρείτε την εικονική μηχανή ξεκινώντας με το ίδιο όνομα της παρουσίας που δόθηκε σε SAP CAL.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic14.jpg)

Τώρα είναι δυνατή η σύνδεση με τη λύση μέσω το κουμπί σύνδεσης στην πύλη του SAP CAL. Το μικρό παράθυρο διαλόγου περιέχει μια σύνδεση με έναν οδηγό χρήστη που περιγράφει όλες τις προεπιλεγμένες πιστοποιήσεις για να εργαστείτε με τη λύση.
[Εδώ](https://caldocs.hana.ondemand.com/caldocs/help/Getting_Started_Guide_IDES607MSSQL.pdf) θα είναι η σύνδεση στον οδηγό για τη λύση IDES.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

Μια άλλη επιλογή είναι να συνδεθείτε στη η Εικονική των Windows και να ξεκινήσετε για παράδειγμα το προκαθορισμένο Γραφικών SAP.





