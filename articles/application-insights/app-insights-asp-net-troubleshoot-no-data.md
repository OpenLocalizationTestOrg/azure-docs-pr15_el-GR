<properties 
    pageTitle="Αντιμετώπιση προβλημάτων χωρίς δεδομένα - εφαρμογή ιδέες για .NET" 
    description="Δεν βλέπετε δεδομένα στο Visual Studio εφαρμογή ιδέες; Δοκιμάστε εδώ." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Αντιμετώπιση προβλημάτων χωρίς δεδομένα - εφαρμογή ιδέες για .NET

## <a name="some-of-my-telemetry-is-missing"></a>Ορισμένα από μου τηλεμετρίας λείπει

*Στην εφαρμογή ιδέες, να δείτε μόνο ένα κλάσμα τα συμβάντα που δημιουργούνται από την εφαρμογή μου.*

* Εάν βλέπετε με συνέπεια το ίδιο κλάσμα, αυτό οφείλεται πιθανώς προσαρμόσιμη [δειγματοληψία](app-insights-sampling.md). Για να επιβεβαιώσετε αυτήν, Άνοιγμα αναζήτησης (από την επισκόπηση blade) και δείτε μια παρουσία μιας πρόσκλησης σε ή άλλου συμβάντος. Στο κάτω μέρος της ενότητας ιδιότητες, κάντε κλικ στο κουμπί "…" για να δείτε λεπτομέρειες πλήρους ιδιότητα. Εάν ζητήσετε Count > 1 και, στη συνέχεια, δειγματοληψία είναι σε λειτουργία. 
* Διαφορετικά, είναι πιθανό ότι που πάτημα έναν [περιορισμό ταχύτητα δεδομένων](app-insights-pricing.md#limits-summary) για το πρόγραμμα τις πληροφορίες τιμολόγησης. Αυτά τα όρια εφαρμόζονται ανά λεπτό.

## <a name="no-data-from-my-server"></a>Δεν υπάρχουν δεδομένα από το διακομιστή

*Έχω εγκαταστήσει την εφαρμογή μου στο διακομιστή web και τώρα δεν μπορώ να δω οποιαδήποτε τηλεμετρίας από αυτό. Ότι λειτούργησε OK στον υπολογιστή μου αποκλίσεις.*

* Πιθανώς ένα ζήτημα τείχος προστασίας. [Ορισμός εξαιρέσεων τείχους προστασίας για εφαρμογή ιδέες για την αποστολή δεδομένων](app-insights-ip-addresses.md).

*Να [εγκαταστήσει Εποπτεία κατάστασης](app-insights-monitor-performance-live-website-now.md) του διακομιστή web για την παρακολούθηση της υπάρχουσας εφαρμογές. Δεν βλέπω κανένα αποτέλεσμα.*

* Ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων Εποπτεία κατάστασης](app-insights-monitor-performance-live-website-now.md#troubleshooting). 


## <a name="q01"></a>Δεν υπάρχει η επιλογή 'Προσθήκη εφαρμογής ιδέες' στο Visual Studio

*Κατά τη δημιουργία νέου έργου στο Visual Studio ή όταν να κάντε δεξί κλικ σε ένα υπάρχον έργο στο Εξερεύνηση λύσεων, δεν βλέπω τις επιλογές εφαρμογής ιδέες.*

+ Δεν όλοι οι τύποι του έργου .NET υποστηρίζονται από τα εργαλεία. Υποστηρίζονται Web και WCF έργα. Για άλλους τύπους έργου όπως εφαρμογές υπολογιστή ή υπηρεσία, μπορείτε ακόμα να [προσθέσετε μια εφαρμογή του SDK ιδέες για το έργο σας με μη αυτόματο τρόπο](app-insights-windows-desktop.md).
+ Βεβαιωθείτε ότι έχετε επιλέξει [Visual Studio 2013 ενημέρωση 3 ή νεότερη έκδοση](http://go.microsoft.com/fwlink/?LinkId=397827). Διατίθεται προ-εγκατεστημένες με εργαλεία ιδέες εφαρμογών.
+ Επιλέξτε **Εργαλεία**, **επεκτάσεις και ενημερώσεις** και ελέγξτε ότι η **Εφαρμογή ιδέες εργαλεία** έχει εγκατασταθεί και ενεργοποιηθεί. Αν Ναι, κάντε κλικ στην επιλογή **ενημερώσεις** για να δείτε εάν υπάρχει διαθέσιμη μια ενημερωμένη έκδοση.
+ Ανοίξτε το παράθυρο διαλόγου νέο έργο και επιλέξτε την εφαρμογή ASP.NET Web. Εάν βλέπετε την επιλογή εφαρμογής ιδέες εκεί, είναι εγκατεστημένα τα εργαλεία. Εάν όχι, δοκιμάστε την κατάργηση της εγκατάστασης και, στη συνέχεια, επαναλάβετε την εγκατάσταση του τα εργαλεία ιδέες εφαρμογής.


## <a name="q02"></a>Απέτυχε η προσθήκη ιδέες εφαρμογής

*Κατά τη δημιουργία νέου έργου web ή όταν προσπαθείτε να προσθέσετε ιδέες εφαρμογής σε ένα υπάρχον έργο, να δείτε ένα μήνυμα σφάλματος.*

Πιθανές αιτίες:

* Επικοινωνία με την εφαρμογή ιδέες πύλη απέτυχε. ή
* Υπάρχει κάποιο πρόβλημα με το λογαριασμό σας Azure;
* Έχετε μόνο [πρόσβαση για ανάγνωση για τη συνδρομή ή την ομάδα όπου που προσπαθήσατε να δημιουργήσετε το νέο πόρο](app-insights-resources-roles-access-control.md).

Επιδιόρθωση:

+ Ελέγξτε ότι δώσατε τα διαπιστευτήρια εισόδου για το λογαριασμό Azure προς τα δεξιά. 
+ Στο πρόγραμμα περιήγησης, ελέγξτε ότι έχετε πρόσβαση στην [πύλη του Azure](https://portal.azure.com). Ανοίξτε τις ρυθμίσεις και δείτε εάν υπάρχει τυχόν περιορισμού.
+ [Προσθήκη εφαρμογής ιδέες για το υπάρχον έργο](app-insights-asp-net.md): στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στην επιλογή το έργο σας και επιλέξτε "Προσθήκη εφαρμογής ιδέες."
+ Εάν ακόμη δεν λειτουργεί, ακολουθήστε τη [μη αυτόματη διαδικασία](app-insights-windows-services.md) για να προσθέσετε έναν πόρο στην πύλη του και, στη συνέχεια, προσθέστε το SDK στο έργο σας. 

## <a name="emptykey"></a>Λαμβάνω ένα μήνυμα σφάλματος "το κλειδί οργάνων δεν μπορεί να είναι κενό"

Φαίνεται Παρουσιάστηκε κάποιο πρόβλημα ενώ εγκαθιστάτε εφαρμογή ιδέες ή μήπως έναν προσαρμογέα καταγραφής.

Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ `ApplicationInsights.config` και επιλέξτε **Ρύθμιση παραμέτρων εφαρμογής ιδέες**. Θα εμφανιστεί ένα παράθυρο διαλόγου που σάς καλεί να εισέλθετε στο Azure και είτε δημιουργήστε έναν πόρο ιδέες εφαρμογής, ή εκ νέου χρήση μιας υπάρχουσας.


##<a name="NuGetBuild"></a>"Πακέτα NuGet λείπουν" στο διακομιστή Δόμηση

*Όλα τα στοιχεία δημιουργεί OK όταν να τον εντοπισμό σφαλμάτων στον υπολογιστή μου ανάπτυξη, αλλά εμφανίζεται ένα σφάλμα NuGet στο διακομιστή Δόμηση.*

Ανατρέξτε στο θέμα [NuGet πακέτου επαναφορά](http://docs.nuget.org/Consume/Package-Restore) και [Αυτόματη πακέτο](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Εντολή μενού που λείπουν για να ανοίξετε ιδέες εφαρμογής από το Visual Studio

*Όταν κάνω δεξιό κλικ το έργο μου Εξερεύνηση λύσεων, δεν βλέπω τις εντολές εφαρμογής ιδέες ή δεν βλέπω μια εντολή Άνοιγμα εφαρμογής ιδέες.*

Πιθανές αιτίες:

* Εάν έχετε δημιουργήσει με μη αυτόματο τρόπο τον πόρο ιδέες εφαρμογής, ή εάν το έργο είναι τύπου που δεν υποστηρίζονται από τα εργαλεία ιδέες εφαρμογής.
* Τα εργαλεία ιδέες εφαρμογής είναι απενεργοποιημένες στην του Visual Studio.
* Το Visual Studio είναι παλαιότερης έκδοσης από την ενημερωμένη έκδοση 2013 3.

Επιδιόρθωση:

* Βεβαιωθείτε ότι η έκδοση του Visual Studio είναι ενημέρωση 2013 3 ή νεότερη έκδοση.
* Επιλέξτε **Εργαλεία**, **επεκτάσεις και ενημερώσεις** και ελέγξτε ότι η **Εφαρμογή ιδέες εργαλεία** έχει εγκατασταθεί και ενεργοποιηθεί. Αν Ναι, κάντε κλικ στην επιλογή **ενημερώσεις** για να δείτε εάν υπάρχει διαθέσιμη μια ενημερωμένη έκδοση.
* Κάντε δεξί κλικ στην Εξερεύνηση λύσεων το έργο σας. Εάν βλέπετε την εντολή **Ρύθμιση παραμέτρων εφαρμογής ιδέες**, το χρησιμοποιήσετε για να συνδεθείτε το έργο σας με τον πόρο στην υπηρεσία εφαρμογής ιδέες.


Διαφορετικά, τον τύπο του έργου σας δεν υποστηρίζεται απευθείας από τα εργαλεία ιδέες εφαρμογής. Για να δείτε το τηλεμετρίας, εισέλθετε στην [πύλη του Azure](https://portal.azure.com), επιλέξτε εφαρμογή ιδέες στην αριστερή γραμμή περιήγησης και επιλέξτε την εφαρμογή σας.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>'Δεν επιτρέπεται η πρόσβαση' κατά το άνοιγμα ιδέες εφαρμογής από το Visual Studio

*Η εντολή μενού 'Άνοιγμα εφαρμογής ιδέες' εμένα χρειάζεται για την πύλη του Azure, αλλά λαμβάνω ένα μήνυμα σφάλματος "δεν επιτρέπεται η πρόσβαση".*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

Η Microsoft είσοδος που χρησιμοποιήσατε τελευταίο στο προεπιλεγμένο πρόγραμμα περιήγησης δεν έχει πρόσβαση [στον πόρο που δημιουργήθηκε κατά την προσθήκη εφαρμογής ιδέες αυτής της εφαρμογής](app-insights-asp-net.md). Υπάρχουν δύο πιθανές αιτίες: 

* Έχετε περισσότερα από ένα λογαριασμό Microsoft - μήπως μια εργασία και έναν προσωπικό λογαριασμό Microsoft; Η είσοδος που χρησιμοποιήσατε τελευταίο στο προεπιλεγμένο πρόγραμμα περιήγησης έχει για ένα λογαριασμό διαφορετικό από αυτό που έχει πρόσβαση για να [προσθέσετε την εφαρμογή ιδέες για το έργο](app-insights-asp-net.md). 

 * Επιδιόρθωση: Κάντε κλικ στο όνομά σας στο επάνω δεξιό μέρος του παραθύρου προγράμματος περιήγησης και έξοδος. Στη συνέχεια, πραγματοποιήστε είσοδο με το λογαριασμό που έχει πρόσβαση. Στη συνέχεια, στην αριστερή γραμμή περιήγησης, κάντε κλικ στην επιλογή εφαρμογή ιδέες και επιλέξτε την εφαρμογή σας.

* Πρόσθεσε κάποιος άλλος εφαρμογή ιδέες για το έργο και τους ξεχάσατε να παρέχουν [πρόσβαση στην ομάδα πόρων](app-insights-resources-roles-access-control.md) στην οποία δημιουργήθηκε. 

 * Επιδιόρθωση: Εάν χρησιμοποιήσει ήδη έναν εταιρικό λογαριασμό, μπορούν να προσθέτουν που στην ομάδα; ή να εκχωρήσετε τα μεμονωμένα πρόσβαση στην ομάδα πόρων.



## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>Περιουσιακών στοιχείων δεν ' Εύρεση' κατά το άνοιγμα ιδέες εφαρμογής από το Visual Studio

*Η εντολή μενού 'Άνοιγμα εφαρμογής ιδέες' εμένα χρειάζεται για την πύλη του Azure, αλλά λαμβάνω ένα μήνυμα σφάλματος ''περιουσιακών στοιχείων δεν Εύρεση.*

Πιθανές αιτίες:

* Έχει διαγραφεί ο πόρος εφαρμογής ιδέες για την εφαρμογή σας; ή
* Το κλειδί οργάνων έχει οριστεί ή τροποποιηθεί στο ApplicationInsights.config με την επεξεργασία απευθείας, χωρίς να ενημερώσετε το αρχείο έργου. 

Το κλειδί οργάνων σε στοιχεία ελέγχου ApplicationInsights.config όπου αποστέλλονται τα τηλεμετρίας. Μια γραμμή στο αρχείο έργου ελέγχει τον πόρο που ανοίγει όταν χρησιμοποιείτε την εντολή στο Visual Studio. 

Επιδιόρθωση:

* Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο και επιλέξτε εφαρμογή ιδέες, ρύθμιση παραμέτρων εφαρμογής ιδέες. Στο παράθυρο διαλόγου, μπορείτε είτε να επιλέξετε να στείλετε τηλεμετρίας σε έναν υπάρχοντα πόρο ή να δημιουργήσετε ένα νέο. Ή:
* Ανοίξτε τον πόρο απευθείας. Είσοδος στην [πύλη του Azure](https://portal.azure.com), κάντε κλικ στην επιλογή εφαρμογή ιδέες στην αριστερή γραμμή περιήγησης και, στη συνέχεια, επιλέξτε την εφαρμογή σας.



## <a name="where-do-i-find-my-telemetry"></a>Πού μπορώ να βρω τηλεμετρίας μου;

*Να πραγματοποιήσει είσοδο στην [πύλη του Microsoft Azure](https://portal.azure.com)και ψάχνω στο Azure κεντρικό πίνακα εργαλείων. Επομένως, πού μπορώ να βρω τα δεδομένα μου ιδέες εφαρμογή;*

* Στην αριστερή γραμμή περιήγησης, κάντε κλικ στην επιλογή εφαρμογή ιδέες, στη συνέχεια, το όνομα εφαρμογής. Εάν δεν έχετε τα έργα εκεί, πρέπει να [προσθέσετε ή να ρυθμίσετε τις παραμέτρους ιδέες εφαρμογή στο έργο σας web](app-insights-asp-net.md).

    Θα δείτε ορισμένα σύνοψης γραφήματα. Μπορείτε να κάνετε κλικ σε αυτά για να δείτε περισσότερες λεπτομέρειες.

* Στο Visual Studio, ενώ κάνετε εντοπισμό σφαλμάτων της εφαρμογής σας, κάντε κλικ στο κουμπί Εφαρμογή ιδέες.


## <a name="q03"></a>Δεν υπάρχουν δεδομένα διακομιστή (ή χωρίς δεδομένα καθόλου)

*Να εκτελέσατε εφαρμογή μου και, στη συνέχεια, να ανοίξει την υπηρεσία ιδέες εφαρμογή στο Microsoft Azure, αλλά εμφανίζονται όλα τα γραφήματα 'Μάθετε πώς μπορείτε να συλλέξετε...' ή "Χωρίς ρύθμιση παραμέτρων."* Ή, *μόνο Προβολή σελίδας και χρήστη δεδομένων, αλλά χωρίς δεδομένα διακομιστή.*

+ Εκτελέστε την εφαρμογή σας σε κατάσταση εντοπισμού σφαλμάτων στο Visual Studio (F5). Χρησιμοποιήστε την εφαρμογή ώστε να δημιουργήσετε ορισμένες τηλεμετρίας. Ελέγξτε ότι μπορείτε να δείτε τα συμβάντα που έχουν καταγραφεί στο παράθυρο εξόδου του Visual Studio. 

    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)

+ Στην πύλη του ιδέες εφαρμογή, ανοίξτε [Διαγνωστικών αναζήτησης](app-insights-diagnostic-search.md). Δεδομένα συνήθως εμφανίζεται εδώ πρώτα.
+ Κάντε κλικ στο κουμπί Ανανέωση. Το blade ανανεώνει τα ίδια περιοδικά, αλλά μπορείτε επίσης να κάνετε αυτό με μη αυτόματο τρόπο. Το χρονικό διάστημα ανανέωσης είναι περισσότερο χρόνο για μεγαλύτερα χρονικά διαστήματα.
+ Ελέγξτε τα κλειδιά οργάνων ταιριάζουν. Στη το κύριο blade για την εφαρμογή σας στην πύλη εφαρμογής ιδέες, με τα **βασικά στοιχεία για την** αναπτυσσόμενη, δείτε **το κλειδί οργάνων**. Στη συνέχεια, στο έργο σας στο Visual Studio, ανοίξτε ApplicationInsights.config και βρείτε το `<instrumentationkey>`. Βεβαιωθείτε ότι τα δύο κλειδιά είναι ίσες. Αν δεν:
 + Στην πύλη, κάντε κλικ στην επιλογή εφαρμογή ιδέες και αναζητήστε τον πόρο εφαρμογή με το σωστό κλειδί. ή
 + Στην Εξερεύνηση λύσεων Visual Studio, κάντε δεξί κλικ στο έργο και επιλέξτε εφαρμογή ιδέες, ρύθμιση παραμέτρων. Επαναφορά της εφαρμογής για να στείλετε τηλεμετρίας στον πόρο δεξιά.
 + Εάν δεν μπορείτε να βρείτε τα πλήκτρα με τα αντίστοιχα, ελέγξτε ότι χρησιμοποιείτε τα ίδια διαπιστευτήρια εισόδου στο Visual Studio ως στην πύλη του.

    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
    
+ Στο [Microsoft Azure κεντρικό πίνακα εργαλείων](https://portal.azure.com), δείτε το χάρτη εύρυθμη λειτουργία των υπηρεσιών. Εάν υπάρχουν ορισμένες ενδείξεις προειδοποίησης, περιμένετε έως ότου αυτές έχουν επιστρέφονται στο κουμπί OK και, στη συνέχεια, κλείστε και ανοίξτε ξανά σας blade εφαρμογή ιδέες εφαρμογής.
+ Επιλέξτε επίσης [το ιστολόγιο του κατάσταση](http://blogs.msdn.com/b/applicationinsights-status/).
+ Μήπως γράφετε κώδικα για την [πλευρά του διακομιστή SDK](app-insights-api-custom-events-metrics.md) που μπορεί να αλλάξει το κλειδί οργάνων στο `TelemetryClient` παρουσίες ή στο `TelemetryContext`; Ή μήπως συντάξετε ένα [φίλτρο ή δειγματοληψία ρύθμισης παραμέτρων](app-insights-api-filtering-sampling.md) που μπορεί να το φιλτράρισμα ανάληψη πάρα πολύ;
+ Εάν επεξεργαστήκατε ApplicationInsights.config, ελέγξτε προσεκτικά τις παραμέτρους των [TelemetryInitializers και TelemetryProcessors](app-insights-api-filtering-sampling.md). Ένα που ονομάζεται λάθος τύπο ή η παράμετρος μπορεί να προκαλέσει το SDK για να στείλετε χωρίς δεδομένα.

## <a name="q04"></a>Χωρίς δεδομένα στις προβολές σελίδας, προγράμματα περιήγησης, χρήση

*Μπορώ να δω δεδομένων σε γραφήματα χρόνος απόκρισης του διακομιστή και προσκλήσεις σε διακομιστή, αλλά χωρίς δεδομένα σε χρόνου φόρτωσης προβολής σελίδας ή το πρόγραμμα περιήγησης ή χρήση λεπίδες.*

Τα δεδομένα προέρχονται από δέσμες ενεργειών σε ιστοσελίδες. 

+ Αν έχετε προσθέσει ιδέες εφαρμογής σε ένα υπάρχον έργο web, [θα πρέπει να προσθέσετε με μη αυτόματο τρόπο τις δέσμες ενεργειών](app-insights-javascript.md).
+ Βεβαιωθείτε ότι ο Internet Explorer δεν εμφανίζει την τοποθεσία σας σε κατάσταση λειτουργίας συμβατότητας.
+ Χρησιμοποιήστε τη δυνατότητα εντοπισμού σφαλμάτων του προγράμματος περιήγησης (F12 σε ορισμένα προγράμματα περιήγησης, στη συνέχεια, επιλέξτε δίκτυο) για να επαληθεύσετε ότι τα δεδομένα αποστέλλεται σε `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Δεν υπάρχουν δεδομένα εξάρτηση ή εξαίρεσης

Ανατρέξτε στο θέμα [τηλεμετρίας εξάρτηση](app-insights-asp-net-dependencies.md) και [τηλεμετρίας εξαίρεση](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>Δεν υπάρχουν δεδομένα επιδόσεων

Δεδομένα επιδόσεων (CPU, rate εισόδου/ΕΞΌΔΟΥ και ούτω καθεξής) είναι διαθέσιμο για [υπηρεσίες web Java](app-insights-java-collectd.md), [εφαρμογές υπολογιστή των Windows](app-insights-windows-desktop.md), [web των υπηρεσιών IIS εφαρμογές και υπηρεσίες Εάν εγκαταστήσετε την οθόνη κατάσταση](app-insights-monitor-performance-live-website-now.md)και [Τις υπηρεσίες Cloud Azure](app-insights-azure.md). Μπορείτε να τα εντοπίσετε στην περιοχή ρυθμίσεις, οι διακομιστές.

Δεν είναι διαθέσιμη για Azure τοποθεσίες Web που διαθέτετε.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>Δεν υπάρχουν δεδομένα (διακομιστής) επειδή το να δημοσιευτεί η εφαρμογή με το διακομιστή

+ Ελέγξτε που αντιγράψατε στην πραγματικότητα όλα της Microsoft. DLL ApplicationInsights στο διακομιστή, μαζί με το Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll
+ Στο τείχος προστασίας σας, ίσως χρειαστεί να [ανοίξετε κάποιες θύρες TCP](app-insights-ip-addresses.md#data-access-api).
+ Εάν πρέπει να χρησιμοποιήσετε ένα διακομιστή μεσολάβησης για να στείλετε εκτός του εταιρικού σας δικτύου, ορισμός [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) στο Web.config
+ Windows Server 2008: Βεβαιωθείτε ότι έχετε εγκαταστήσει τις ακόλουθες ενημερώσεις: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).


## <a name="i-used-to-see-data-but-it-has-stopped"></a>Να χρησιμοποιείται για να δείτε τα δεδομένα, αλλά σταμάτησε

* Ελέγξτε την [κατάσταση ιστολογίου](http://blogs.msdn.com/b/applicationinsights-status/).
* Να πατήσετε το όριο του μηνιαίου των σημείων δεδομένων; Ανοίξτε τις ρυθμίσεις/ορίου και τιμολόγηση για να μάθετε. Εάν Ναι, μπορείτε να αναβαθμίσετε το πρόγραμμά σας ή να πληρώσετε για πρόσθετη χωρητικότητα. Δείτε την [Τιμολόγηση συνδυασμού](https://azure.microsoft.com/pricing/details/application-insights/).


## <a name="i-dont-see-all-the-data-im-expecting"></a>Δεν βλέπω όλα τα δεδομένα να αναμένεται

Εάν η εφαρμογή σας στέλνει πολλά δεδομένα και χρησιμοποιείτε το SDK ιδέες εφαρμογής για ASP.NET έκδοση 2.0.0-beta3 ή νεότερη έκδοση, η δυνατότητα [προσαρμόσιμης δειγματοληψία](app-insights-sampling.md) μπορεί να εφαρμόζει και αποστολή μόνο ποσοστό του τηλεμετρίας σας. 

Μπορείτε να το απενεργοποιήσετε, αλλά αυτό δεν συνιστάται. Δειγματοληψία έχει σχεδιαστεί ώστε να σχετικές τηλεμετρίας σωστά μεταδίδεται, για σκοπούς διαγνωστικών. 

## <a name="wrong-geographical-data-in-user-telemetry"></a>Πρόβλημα γεωγραφικά δεδομένα στο τηλεμετρίας χρήστη

Η πόλη, περιοχή και των διαστάσεων χώρα προέρχονται από διευθύνσεις IP και δεν είναι πάντα ακριβείς.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Εξαίρεση "δεν βρέθηκε η μέθοδος" σχετικά με την εκτέλεση στις υπηρεσίες Cloud Azure

Δημιουργείτε για .NET 4.6; 4.6 δεν υποστηρίζεται αυτόματα με τις υπηρεσίες Cloud Azure ρόλους. [Εγκατάσταση 4.6 σχετικά με κάθε ρόλο](../cloud-services/cloud-services-dotnet-install-dotnet.md) πριν από την εκτέλεση της εφαρμογής σας.

## <a name="still-not-working"></a>Εξακολουθεί να μην λειτουργεί...

* [Φόρουμ εφαρμογής ιδέες](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)
