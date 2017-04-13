<properties
   pageTitle="Αντιμετώπιση προβλημάτων με την ανίχνευση συμβάντων | Microsoft Azure"
   description="Τα πιο συνηθισμένα ζητήματα κατά την ανάπτυξη υπηρεσίες στο Microsoft Azure Service ύφασμα."
   services="service-fabric"
   documentationCenter=".net"
   authors="mattrowmsft"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/31/2016"
   ms.author="mattrow"/>


# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Αντιμετώπιση κοινών ζητημάτων κατά την ανάπτυξη υπηρεσίες σε ύφασμα υπηρεσίας Azure

Όταν χρησιμοποιείτε τις υπηρεσίες στον υπολογιστή σας για προγραμματιστές, είναι εύκολο για χρήση [του Visual Studio εργαλεία εντοπισμού σφαλμάτων](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Για απομακρυσμένο συμπλεγμάτων, [αναφορών εύρυθμης λειτουργίας](service-fabric-view-entities-aggregated-health.md) είναι πάντα ένα καλό μέρος για να ξεκινήσετε. Οι πιο εύκολος τρόποι για να αποκτήσετε πρόσβαση σε αυτές τις αναφορές είναι μέσω του PowerShell ή [SFX](service-fabric-visualizing-your-cluster.md). Σε αυτό το άρθρο προϋποθέτει ότι τον εντοπισμό σφαλμάτων σε ένα σύμπλεγμα απομακρυσμένο και έχετε κατανοήσει βασικές πώς μπορείτε να χρησιμοποιήσετε ένα από αυτά τα εργαλεία.

##<a name="application-crash"></a>Σφάλμα εφαρμογής
Το "διαμερίσματα είναι κάτω από το στόχου ρεπλίκα ή παρουσία καταμέτρηση" αναφορά είναι μια καλή ένδειξη που παρουσιάζει πρόβλημα της υπηρεσίας. Για να μάθετε πού παρουσιάζει πρόβλημα υπηρεσία σας μεταφέρει λίγο περισσότερες έρευνα. Όταν της υπηρεσίας εκτελείται σε κλίμακα, βέλτιστη φίλο σας θα είναι ένα σύνολο ανιχνεύσεις thought well-out.  Προτείνουμε να δοκιμάσετε [Διαγνωστικά του Azure](service-fabric-diagnostics-how-to-setup-wad.md) για τη συλλογή αυτές τις ανιχνεύσεις και τη χρήση μιας λύσης όπως [Ελαστικά αναζήτησης](service-fabric-diagnostic-how-to-use-elasticsearch.md) για την προβολή και αναζήτηση τα ίχνη.

![Εύρυθμη λειτουργία των διαμερισμάτων SFX](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

###<a name="during-service-or-actor-initialization"></a>Κατά την προετοιμασία υπηρεσία ή παράγοντα
Τυχόν εξαιρέσεις πριν από τον τύπο της υπηρεσίας είναι προετοιμασία θα προκαλέσει τη διαδικασία σφάλμα. Για αυτούς τους τύπους παρουσιάσει σφάλμα, το αρχείο καταγραφής συμβάντων της εφαρμογής θα εμφανιστεί το σφάλμα από την υπηρεσία.
Αυτά είναι τα πιο συνηθισμένα εξαιρέσεις για να δείτε πριν από την υπηρεσία έχει προετοιμαστεί.

***System.IO.FileNotFoundException***

Αυτό το σφάλμα είναι συχνά λόγω συγκρότησης εξαρτήσεις που λείπουν. Ελέγξτε την ιδιότητα CopyLocal στο Visual Studio ή στο καθολικό cache συγκροτήσεων για τον κόμβο.

***System.Runtime.InteropServices.COMException***
 *στο System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)*
 
 Αυτό υποδεικνύει ότι το όνομα του τύπου καταχωρημένες υπηρεσίας δεν ταιριάζουν με το δηλωτικό υπηρεσίας.

[Διαγνωστικά Azure](service-fabric-diagnostics-how-to-setup-wad.md) μπορεί να ρυθμιστεί για να αποστείλετε το αρχείο καταγραφής συμβάντων της εφαρμογής για όλους τους κόμβους σας αυτόματα.

###<a name="runasync-or-onactivateasync"></a>RunAsync() ή OnActivateAsync()
Εάν το σφάλμα συμβαίνει στη διάρκεια της προετοιμασίας ή εκτέλεση του τύπου καταχωρημένες υπηρεσίας ή παράγοντα, η εξαίρεση θα αποκλείονται από ύφασμα υπηρεσίας Azure. Μπορείτε να προβάλετε αυτά από τις υπηρεσίες παροχής Προέλευση_συμβάντος που περιγράφονται στην ενότητα "Επόμενα βήματα".

## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε περισσότερα σχετικά με την υπάρχουσα διαγνωστικά που παρέχεται από ύφασμα υπηρεσίας:

* [Αξιόπιστη Διαγνωστικά ηθοποιών](service-fabric-reliable-actors-diagnostics.md)
* [Αξιόπιστη Διαγνωστικά υπηρεσιών](service-fabric-reliable-services-diagnostics.md)
