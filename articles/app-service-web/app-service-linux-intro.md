<properties 
    pageTitle="Εισαγωγή στην εφαρμογή υπηρεσίας στην Linux | Microsoft Azure" 
    description="Μάθετε σχετικά με την εφαρμογή υπηρεσίας στην Linux." 
    keywords="Azure εφαρμογής υπηρεσίας, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="introduction-to-app-service-on-linux"></a>Εισαγωγή στην εφαρμογή υπηρεσίας στην Linux
Εφαρμογή υπηρεσίας στην Linux είναι αυτήν τη στιγμή σε δημόσια προεπισκόπηση και υποστηρίζει εγγενώς εφαρμογές web της εκτελείται σε Linux. 

## <a name="overview"></a>Επισκόπηση ##
Οι πελάτες να χρησιμοποιήσετε εφαρμογής υπηρεσίας στο Linux για να κεντρικού υπολογιστή web apps εγγενώς σε Linux στοίβες υποστηριζόμενων εφαρμογών. Στην παρακάτω ενότητα δυνατότητες παραθέτει τις στοίβες εφαρμογή υποστηρίζονται αυτήν τη στιγμή.

## <a name="features"></a>Δυνατότητες ##
Εφαρμογή υπηρεσίας στην Linux υποστηρίζει επί του παρόντος οι παρακάτω στοίβες εφαρμογής

- Node.js
- PHP

Οι πελάτες να αναπτύξετε τις εφαρμογές που χρησιμοποιούν

- FTP.
- Τοπική Git.
- GitHub ή BitBucket.

Για την εφαρμογή κλιμάκωση


- Οι πελάτες να κλίμακα τους εφαρμογής web προς τα επάνω ή προς τα κάτω, αλλάζοντας την κλίμακα στο τους πρόγραμμα εφαρμογής υπηρεσίας. 
- Οι πελάτες να κλιμάκωση τις αιτήσεις για ανάληψη και να εκτελέσετε την εφαρμογή σε πολλές παρουσίες μέσα στα όρια της τους SKU.

Για Kudu ορισμένες από τις βασικές λειτουργίες θα λειτουργούν

- Περιβάλλον.
- Αναπτύξεις.
- Βασική κονσόλας.

## <a name="limitations"></a>Περιορισμοί ##

Στην πύλη διαχείρισης Azure μόνο θα εμφάνιση δυνατότητες που υποστηρίζονται αυτήν τη στιγμή για εφαρμογής υπηρεσίας στο Linux και να αποκρύψετε τα υπόλοιπα. Ως ομάδα μας Ενεργοποίηση περισσότερες δυνατότητες θα σας θα διατηρήσετε αναλογιστείτε με αυτή την πύλη διαχείρισης. Ορισμένες δυνατότητες, όπως η ενοποίηση VNET και AAD / έλεγχο ταυτότητας τρίτων κατασκευαστών ή επεκτάσεις τοποθεσίας Kudu δεν εργασίας αυτήν τη στιγμή. Αλλά, όπως αυτά εργασία λαμβάνουμε θα ενημερώσουμε μας τεκμηρίωση και ιστολογίου σχετικά με τις αλλαγές.

Αυτή η προεπισκόπηση δημόσια διατίθεται προς το παρόν μόνο στις ακόλουθες περιοχές

-   Δυτική Η.Π.Α.
-   Δυτική Ευρώπη.
-   Νοτιοανατολικής Ασίας.

Εφαρμογή Web Linux υποστηρίζεται μόνο στις αφοσιωμένη προγράμματος εφαρμογής υπηρεσίας και δεν διαθέτει ένα επίπεδο δωρεάν ή κοινή χρήση. Επίσης, εφαρμογή προγράμματα υπηρεσίας για κανονική και εφαρμογές web Linux είναι αμοιβαία αποκλειόμενα, έτσι δεν μπορείτε να δημιουργήσετε μια εφαρμογή web Linux σε ένα σχέδιο μη Linux εφαρμογής υπηρεσίας.

Εφαρμογή Web Linux πρέπει να δημιουργηθούν σε μια ομάδα πόρων που δεν περιέχει μη Linux εφαρμογές web στην ίδια περιοχή.

Λόγω της έλλειψης επικαλυπτόμενο Ανακύκλωσης από τις εφαρμογές web, οι πελάτες θα πρέπει να θεωρείτε ότι έχετε επανεκκίνηση του ένα μικρό χρόνου εκτός λειτουργίας σε περίπτωση μια εφαρμογή web. 

## <a name="next-steps"></a>Επόμενα βήματα ##

Ακολουθήστε τις παρακάτω συνδέσεις για να ξεκινήσετε με εφαρμογή υπηρεσίας στην Linux. Καταχωρήστε ερωτήσεις και προβλήματα στο [φόρουμ μας](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Δημιουργία εφαρμογών Web στο εφαρμογής υπηρεσίας στο Linux](./app-service-linux-how-to-create-a-web-app.md)
* [Χρήση ΑΣ2 ρύθμισης παραμέτρων για Node.js στο Web Apps σε Linux](./app-service-linux-using-nodejs-pm2.md)
