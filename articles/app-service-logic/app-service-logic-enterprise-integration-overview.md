<properties 
    pageTitle="Επισκόπηση της ενοποίησης για μεγάλες επιχειρήσεις | Microsoft Azure εφαρμογής υπηρεσίας | Microsoft Azure" 
    description="Χρησιμοποιήστε τις δυνατότητες της ενοποίησης για μεγάλες επιχειρήσεις για να ενεργοποιήσετε την επιχειρηματική διαδικασία ενοποίησης σενάρια και χρήση λογικής εφαρμογών" 
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
    ms.date="09/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-the-enterprise-integration-pack"></a>Επισκόπηση του πακέτου ενοποίησης για μεγάλες επιχειρήσεις

## <a name="what-is-the-enterprise-integration-pack"></a>Τι είναι το πακέτο ενοποίησης Enterprise;
Το πακέτο ενοποίησης για μεγάλες επιχειρήσεις είναι λύση βασίζεται στο cloud της Microsoft για την ενεργοποίηση απρόσκοπτα επιχειρήσεις επιχειρήσεων (B2B) επικοινωνίες. Το πακέτο χρησιμοποιεί τυπική πρωτόκολλα κλάδο όπως [AS2](./app-service-logic-enterprise-integration-as2.md), [X12](./app-service-logic-enterprise-integration-x12.md)και [EDIFACT](./app-service-logic-enterprise-integration-edifact.md) για την ανταλλαγή μηνυμάτων μεταξύ συνεργάτες. Τα μηνύματα μπορεί να είναι ασφαλείς προαιρετικά χρησιμοποιώντας κρυπτογράφηση και ψηφιακές υπογραφές. 

Το πακέτο επιτρέπει σε εταιρείες που χρησιμοποιούν διαφορετικά πρωτόκολλα μορφές και την ανταλλαγή μηνυμάτων ηλεκτρονικά, με τη μετατροπή του διαφορετικές μορφές σε μια μορφή που συστήματα τις δύο εταιρείες μπορούν να ερμηνεύσετε και να εκτελέσουν κάποια ενέργεια. 

Εάν είστε εξοικειωμένοι με το διακομιστή BizTalk ή υπηρεσίες της Microsoft Azure BizTalk, θα βρείτε εύχρηστες δυνατότητες την ενοποίηση για μεγάλες επιχειρήσεις, επειδή περισσότερες από τις έννοιες είναι παρόμοια. Μια βασική διαφορά είναι ότι ενοποίησης για μεγάλες επιχειρήσεις χρησιμοποιεί ενοποίηση λογαριασμών για να απλοποιήσετε τη χώρου αποθήκευσης και τη διαχείριση των αντικείμενα που χρησιμοποιείται σε B2B επικοινωνίες. 

Ένας, το πακέτο ενοποίησης για μεγάλες επιχειρήσεις βασίζεται σε **λογαριασμούς ενοποίησης** που αποθηκεύουν όλα τα αντικείμενα που μπορούν να χρησιμοποιηθούν για να σχεδιάσετε, ανάπτυξη και τη συντήρηση των εφαρμογών σας B2B. Ένας λογαριασμός ενοποίησης στην ουσία είναι ένα κοντέινερ που βασίζεται στο cloud, όπου μπορείτε να αποθηκεύσετε αντικείμενα όπως σχήματα, συνεργάτες, πιστοποιητικά, χάρτες και τις συμβάσεις. Αυτά τα στοιχεία, στη συνέχεια, μπορεί να χρησιμοποιηθεί σε εφαρμογές της λογικής για να δημιουργήσετε B2B ροές εργασίας. Να μπορέσετε να χρησιμοποιήσετε τα αντικείμενα σε μια εφαρμογή λογικής, χρειάζεστε για να συνδέσετε το λογαριασμό σας ενοποίηση με εφαρμογή της λογικής. Μετά τη σύνδεση σε αυτά, εφαρμογή της λογικής θα έχουν πρόσβαση σε αντικείμενα του λογαριασμού της ενοποίησης του.  

## <a name="why-should-you-use-enterprise-integration"></a>Γιατί πρέπει να χρησιμοποιήσετε για μεγάλες επιχειρήσεις ενοποίησης;
- Με ενσωμάτωση του για μεγάλες επιχειρήσεις, θα μπορείτε να αποθηκεύσετε όλα τα αντικείμενα σας σε ένα σημείο, το οποίο είναι ο λογαριασμός σας ενοποίησης. 
- Μπορείτε να αξιοποιήσετε το μηχανισμό εφαρμογές λογικής και όλες τις γραμμές σύνδεσης για να δημιουργήσετε B2B ροές εργασίας και ενοποίηση με εφαρμογές άλλων κατασκευαστών ΑΔΑ, εσωτερικής εγκατάστασης εφαρμογών, καθώς και προσαρμοσμένες εφαρμογές
- Μπορείτε επίσης να αξιοποιήσετε Azure συναρτήσεις

## <a name="how-to-get-started-with-enterprise-integration"></a>Πώς μπορείτε να ξεκινήσετε με ενσωμάτωση του enterprise;
Μπορείτε να δημιουργήσετε και να διαχειριστείτε B2B εφαρμογές χρησιμοποιώντας το πακέτο ενοποίησης Enterprise μέσω τη σχεδίαση εφαρμογών λογικής στην **πύλη του Azure**.  

Μπορείτε επίσης να χρησιμοποιήσετε [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "λογικής εφαρμογές τα θέματα του PowerShell") για τη διαχείριση των εφαρμογών σας λογική. 

Ακολουθεί μια επισκόπηση των βημάτων που χρειάζεστε για να μπορέσετε να δημιουργήσετε εφαρμογές στην πύλη του Azure: ![Επισκόπηση εικόνας](./media/app-service-logic-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Τι είναι οι ορισμένα κοινά σενάρια;

Για μεγάλες επιχειρήσεις ενοποίησης υποστηρίζει αυτά τα βιομηχανικά πρότυπα:   

- Επεξεργασία - ηλεκτρονική ανταλλαγή δεδομένων  
- EAI - ενοποίηση επιχειρηματικής εφαρμογής  

## <a name="heres-what-you-need-to-get-started"></a>Παρακάτω θα δείτε τι πρέπει να επιτύχετε γρήγορα αποτελέσματα
- Συνδρομή Azure με ένα λογαριασμό ενοποίησης
- Visual Studio 2015, για να δημιουργήσετε χάρτες και οι διατάξεις
- [Ενοποίηση εφαρμογές εταιρικής λογικής Microsoft Azure Tools for Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it"></a>Δοκιμάστε το
[Δοκιμάστε το τώρα](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) για να αναπτύξετε ένα πλήρως λειτουργικό δείγμα AS2 αποστολή και παραλαβή λογικής app που χρησιμοποιεί τις δυνατότητες B2B λογικής εφαρμογών.

## <a name="learn-more-about"></a>Μάθετε περισσότερα σχετικά με:
- [Συμφωνίες] (./app-service-logic-enterprise-integration-agreements.md "Μάθετε σχετικά με τις συμβάσεις ενοποίησης για μεγάλες επιχειρήσεις")
- [Σενάρια για επιχειρήσεις για επιχειρήσεις (B2B)] (./app-service-logic-enterprise-integration-b2b.md "Μάθετε πώς μπορείτε να δημιουργήσετε εφαρμογές λογικής με δυνατότητες B2B")  
- [Πιστοποιητικά] (./app-service-logic-enterprise-integration-certificates.md "Μάθετε σχετικά με τα πιστοποιητικά ενοποίησης για μεγάλες επιχειρήσεις")
- [Επίπεδο αρχείο κωδικοποίησης/αποκωδικοποίησης] (./app-service-logic-enterprise-integration-flatfile.md "Μάθετε πώς να κωδικοποιήσετε και αποκωδικοποιείτε περιεχόμενα επίπεδο αρχείο")  
- [Ενοποίηση λογαριασμών] (./app-service-logic-enterprise-integration-accounts.md "Μάθετε σχετικά με τους λογαριασμούς ενοποίησης")
- [Χάρτες] (./app-service-logic-enterprise-integration-maps.md "Μάθετε σχετικά με την ενοποίηση του εταιρικού χαρτών")
- [Συνεργάτες] (./app-service-logic-enterprise-integration-partners.md "Μάθετε σχετικά με τους συνεργάτες ενοποίησης για μεγάλες επιχειρήσεις")
- [Οι διατάξεις] (./app-service-logic-enterprise-integration-schemas.md "Μάθετε σχετικά με σχήματα ενοποίησης για μεγάλες επιχειρήσεις")
- [XML μήνυμα επικύρωσης] (./app-service-logic-enterprise-integration-xml.md "Μάθετε πώς μπορείτε να επικυρώσετε XML μηνύματα με λογική εφαρμογών")
- [Μετασχηματισμός XML] (./app-service-logic-enterprise-integration-transform.md "Μάθετε σχετικά με την ενοποίηση του εταιρικού χαρτών")
- [Γραμμές σύνδεσης ενοποίησης για μεγάλες επιχειρήσεις] (../connectors/apis-list.md "Μάθετε σχετικά με τις γραμμές σύνδεσης πακέτου ενοποίησης για μεγάλες επιχειρήσεις")


