<properties
    pageTitle="Δημιουργία εφαρμογής web σε ένα περιβάλλον εφαρμογής υπηρεσίας"
    description="Μάθετε πώς να δημιουργείτε εφαρμογές web και εφαρμογή υπηρεσίας προγράμματος σε ένα περιβάλλον εφαρμογής υπηρεσίας"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="create-a-web-app-in-an-app-service-environment"></a>Δημιουργία εφαρμογής web σε ένα περιβάλλον εφαρμογής υπηρεσίας

## <a name="overview"></a>Επισκόπηση

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να δημιουργήσετε εφαρμογές web και προγράμματα εφαρμογής υπηρεσίας σε ένα [Περιβάλλον εφαρμογής υπηρεσίας](app-service-app-service-environment-intro.md) (ASE). 

> [AZURE.NOTE] Εάν θέλετε να μάθετε πώς μπορείτε να δημιουργήσετε μια εφαρμογή web, αλλά δεν χρειάζεται να το κάνετε σε ένα περιβάλλον εφαρμογής υπηρεσίας, ανατρέξτε στο θέμα [Δημιουργία μιας εφαρμογής web .NET](web-sites-dotnet-get-started.md) ή ένα από τα σχετικά προγράμματα εκμάθησης για άλλες γλώσσες και πλαισίων.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε δημιουργήσει ένα περιβάλλον εφαρμογής υπηρεσίας. Εάν δεν το έχετε κάνει που ακόμα, ανατρέξτε στο θέμα [Δημιουργία περιβάλλον εφαρμογής υπηρεσίας](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Δημιουργία εφαρμογής web

1. Στην [Πύλη του Azure](https://portal.azure.com/), κάντε κλικ στην επιλογή **Δημιουργία > Web + Mobile > Web App**. 

    ![][1]

2. Επιλέξτε τη συνδρομή σας.  

    Εάν έχετε πολλές συνδρομές υπόψη ότι για να δημιουργήσετε μια εφαρμογή στο περιβάλλον σας εφαρμογής υπηρεσίας, πρέπει να χρησιμοποιήσετε την ίδια συνδρομή που χρησιμοποιήσατε κατά τη δημιουργία του περιβάλλοντος. 

3. Επιλέξτε ή δημιουργήστε μια ομάδα πόρων.

    *Ομάδες πόρων* σάς επιτρέπουν να διαχειρίζεστε Σχετικοί πόροι Azure ως μια μονάδα και είναι χρήσιμες κατά τη δημιουργία κανόνων *Έλεγχος πρόσβασης βάσει ρόλων* (RBAC) για τις εφαρμογές σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση του Azure πόρους][ResourceGroups]. 

4. Επιλέξτε ή δημιουργήστε ένα πρόγραμμα εφαρμογής υπηρεσίας.

    *Προγράμματα εφαρμογής υπηρεσίας* πραγματοποιείται σύνολα των εφαρμογών web.  Κανονικά όταν επιλέγετε τις τιμές, οι τιμές που εφαρμόζονται εφαρμόζεται στο πρόγραμμα εφαρμογής υπηρεσίας και όχι τις μεμονωμένες εφαρμογές. Σε ένα ASE πληρώνετε για παρουσίες του υπολογισμού που διατίθενται για την ASE και όχι τι εμφανίζεται με τις ASP.  Για να κλιμακωθεί τον αριθμό των παρουσίες μιας εφαρμογής web που κλιμάκωσης τις παρουσίες της εφαρμογής υπηρεσίας σχέδιο και το επηρεάζει όλες τις εφαρμογές web στο συγκεκριμένο πρόγραμμα.  Ορισμένες δυνατότητες όπως η τοποθεσία υποδοχές ή VNET ενοποίηση έχει επίσης περιορισμούς ποσότητα μέσα στο πρόγραμμα.  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισκόπηση προγράμματος Azure εφαρμογής υπηρεσίας](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)

    Μπορείτε να προσδιορίσετε τα προγράμματα εφαρμογής υπηρεσίας στο ASE σας, εξετάζοντας στη θέση που εμφανίζεται κάτω από το όνομα του προγράμματος.  

    ![][5]

    Εάν θέλετε να χρησιμοποιήσετε ένα πρόγραμμα εφαρμογής υπηρεσίας που υπάρχει ήδη στο περιβάλλον σας εφαρμογής υπηρεσίας, επιλέξτε το σχέδιο. Εάν θέλετε να δημιουργήσετε ένα νέο πρόγραμμα εφαρμογής υπηρεσίας, ανατρέξτε στην παρακάτω ενότητα αυτού του προγράμματος εκμάθησης, [Δημιουργία μιας εφαρμογής υπηρεσίας σχεδίου σε ένα περιβάλλον εφαρμογής υπηρεσίας](#createplan).

5. Πληκτρολογήστε το όνομα για την εφαρμογή web της και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**. 

    Εάν σας ASE χρησιμοποιεί μια εξωτερική VIP τη διεύθυνση URL της εφαρμογής σε μια ASE είναι: [*όνομα τοποθεσίας*]. [*όνομα του περιβάλλοντός σας εφαρμογής υπηρεσίας*]. p.azurewebsites.net αντί για το [*όνομα τοποθεσίας*]. azurewebsites.net
    
    Εάν σας ASE χρησιμοποιεί μια εσωτερική VIP, στη συνέχεια, τη διεύθυνση URL της εφαρμογής σε που είναι ASE: [*όνομα τοποθεσίας*]. [*ο δευτερεύων τομέας που καθορίζεται κατά τη δημιουργία ASE*]   
    Αφού επιλέξετε κατά τη δημιουργία ASE θα εμφανιστεί ο δευτερεύων τομέας που ενημερώνετε κάτω από **το όνομά** σας ASP

## <a name="createplan"></a>Δημιουργήστε ένα πρόγραμμα εφαρμογής υπηρεσίας

Όταν δημιουργείτε ένα πρόγραμμα εφαρμογής υπηρεσίας σε ένα περιβάλλον υπηρεσίας εφαρμογή, τις επιλογές εργασίας διαφέρουν όπως υπάρχουν χωρίς κοινόχρηστο εργαζομένων σε μια ASE.  Οι εργαζόμενοι που θα πρέπει να χρησιμοποιήσετε είναι εκείνα που έχουν εκχωρηθεί για το ASE από το διαχειριστή.  Αυτό σημαίνει ότι για να δημιουργήσετε ένα νέο πρόγραμμα, πρέπει να έχετε περισσότερες εργαζομένων που διατίθενται για την ομάδα εργασίας ASE από τον συνολικό αριθμό των παρουσιών σε όλα τα σχέδια ήδη σε αυτό το σύνολο εργασίας.  Εάν δεν έχετε αρκετό εργαζομένων σε σας ομάδα εργασίας ASE για να δημιουργήσετε το πρόγραμμά σας, πρέπει να εργαστείτε με το διαχειριστή του ASE για να τους προσθέσετε.

Μια άλλη διαφορά με προγράμματα εφαρμογής υπηρεσίας που φιλοξενούνται από ένα περιβάλλον εφαρμογής υπηρεσίας είναι η έλλειψη τις τιμές επιλογής.  Όταν έχετε ένα περιβάλλον εφαρμογής υπηρεσίας πληρώνετε για τους πόρους υπολογισμού που χρησιμοποιείται από το σύστημα και δεν χρειάζεται επιπλέον χρεώσεις για τα προγράμματα του στο περιβάλλον αυτό.  Κανονικά όταν δημιουργείτε ένα πρόγραμμα εφαρμογής υπηρεσίας μπορείτε να επιλέξετε ένα σχέδιο τιμολόγησης που προσδιορίζει χρέωσής σας.  Περιβάλλον εφαρμογής υπηρεσίας είναι ουσιαστικά μια ιδιωτική θέση όπου μπορείτε να δημιουργήσετε περιεχόμενο.  Μπορείτε να πληρώσετε για το περιβάλλον και όχι για τη φιλοξενία του περιεχομένου σας.

Οι παρακάτω οδηγίες δείχνουν πώς μπορείτε να δημιουργήσετε ένα πρόγραμμα εφαρμογής υπηρεσίας ενώ δημιουργείτε μια εφαρμογή web, όπως εξηγείται στην προηγούμενη ενότητα του προγράμματος εκμάθησης.

1. Κάντε κλικ στην επιλογή **Δημιουργία νέου** στην επιλογή πρόγραμμα του περιβάλλοντος εργασίας Χρήστη και δώστε ένα όνομα για το πρόγραμμά σας όπως θα κάνατε κανονικά εκτός μιας ASE.

2. Επιλέξτε το ASE που θέλετε να χρησιμοποιήσετε από τον επιλογέα θέση.

    Επειδή ένα περιβάλλον εφαρμογής υπηρεσίας είναι ουσιαστικά μια θέση ιδιωτικό ανάπτυξης, εμφανίζεται στην περιοχή θέση. 

    ![][2]

    Μετά την επιλογή μιας ASE στην επιλογή θέση, η δημιουργία εφαρμογής υπηρεσίας πρόγραμμα περιβάλλοντος εργασίας Χρήστη ενημερώσεις.  Στη θέση τώρα εμφανίζει το όνομα του συστήματος ASE και στην περιοχή βρίσκεται σε και επιλογέα τιμολόγησης πρόγραμμα έχει αντικατασταθεί με την επιλογή χώρου συγκέντρωσης εργαζόμενου.  

    ![][3]

### <a name="selecting-a-worker-pool"></a>Επιλογή ενός χώρου συγκέντρωσης εργαζόμενου

Κανονικά στο Azure εφαρμογής υπηρεσίας και εκτός της περιβάλλον εφαρμογής υπηρεσίας, υπάρχουν 3 μεγέθη υπολογισμού που είναι διαθέσιμες με την επιλογή προγράμματος αποκλειστικό τιμή.  Με παρόμοιο τρόπο, για ένα ASE μπορείτε να ορίσετε έως 3 χώρους συγκέντρωσης των εργαζομένων και καθορίστε το μέγεθος του υπολογισμού που χρησιμοποιείται για τη συγκεκριμένη ομάδα εργασίας.  Τι αυτό σημαίνει ότι για μισθωτές του το ASE αντί για αυτό, που είναι επιλογής σε τιμολόγησης πρόγραμμα με μέγεθος υπολογισμού για το πρόγραμμα εφαρμογής υπηρεσίας, επιλέξτε αυτό που ονομάζεται μια *ομάδα εργασίας*.  

Στην επιλογή χώρος συγκέντρωσης εργαζόμενου περιβάλλοντος εργασίας Χρήστη εμφανίζει το μέγεθος υπολογισμού που χρησιμοποιείται για τη συγκεκριμένη ομάδα εργασίας κάτω από το όνομα.  Αναφέρεται η ποσότητα που είναι διαθέσιμη για να υπολογίσετε πόσες εμφανίσεις είναι διαθέσιμη για χρήση σε αυτό το σύνολο.  Το συνολικό χώρο συγκέντρωσης μπορεί να έχει στην πραγματικότητα περισσότερες εμφανίσεις από αυτόν τον αριθμό, αλλά αυτό το όρισμα τιμή αναφέρεται απλώς πόσες δεν χρησιμοποιούνται.  Εάν χρειάζεστε για να προσαρμόσετε το περιβάλλον εφαρμογής υπηρεσίας για να προσθέσετε περισσότερη υπολογιστική πόρους ανατρέξτε στο θέμα [ρύθμιση το περιβάλλον εφαρμογής υπηρεσίας](app-service-web-configure-an-app-service-environment.md).

![][4]

Σε αυτό το παράδειγμα μπορείτε να δείτε μόνο δύο χώρους συγκέντρωσης εργασίας που είναι διαθέσιμες. Αυτό συμβαίνει επειδή ο διαχειριστής ASE εκχωρηθεί μόνο κεντρικών υπολογιστών σε αυτές τις δύο εργαζόμενου χώρους συγκέντρωσης.  Η τρίτη θα εμφανίζονται όταν υπάρχουν ΣΠΣ που έχει εκχωρηθεί σε αυτήν.  

## <a name="after-web-app-creation"></a>Μετά τη δημιουργία της εφαρμογής web

Υπάρχουν μερικά θέματα για εκτέλεση εφαρμογών web και τη Διαχείριση εφαρμογών υπηρεσίας σχέδια σε μια ASE που πρέπει να ληφθούν υπόψη.  

Όπως σημειώθηκε προηγούμενα, ο κάτοχος του το ASE είναι υπεύθυνος για το μέγεθος του συστήματος και ως αποτέλεσμα είναι επίσης υπεύθυνοι για την εξασφάλιση ότι υπάρχει αρκετή χωρητικότητα για τη φιλοξενία την επιθυμητή προγράμματος εφαρμογής υπηρεσίας. Εάν υπάρχουν δεν υπάρχει διαθέσιμη εργαζομένων, δεν θα μπορείτε να δημιουργήσετε το πρόγραμμά σας εφαρμογής υπηρεσίας.  Αυτό είναι επίσης την τιμή TRUE σε κλιμάκωση προς την εφαρμογή web.  Εάν χρειάζεστε περισσότερες παρουσίες, στη συνέχεια, θα πρέπει να λάβετε το περιβάλλον εφαρμογής υπηρεσίας διαχειριστή για να προσθέσετε περισσότερες εργαζομένων.

Μετά τη δημιουργία του web app και το πρόγραμμα εφαρμογής υπηρεσίας είναι καλή ιδέα να περιορίσετε το μέγεθος.  Σε ένα ASE πάντοτε πρέπει να έχετε τουλάχιστον 2 παρουσίες του προγράμματός σας εφαρμογής υπηρεσίας για την παροχή ανοχή σφαλμάτων για τις εφαρμογές σας.  Κλιμάκωση ένα σχέδιο παροχής υπηρεσιών εφαρμογής σε μια ASE είναι το ίδιο κανονικά έως το πρόγραμμα εφαρμογής υπηρεσίας στο περιβάλλον εργασίας Χρήστη.  Για περισσότερες πληροφορίες σχετικά με την κλίμακα, [τον τρόπο για να κλιμακωθεί μια εφαρμογή web σε ένα περιβάλλον εφαρμογής υπηρεσίας](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: http://azure.microsoft.com/documentation/articles/resource-group-portal/
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/