<properties title="" pageTitle="Ονόματα αρχείων και θέσεις για τεχνικά άρθρα Azure" description="Εξηγεί τη δομή αρχείου για άρθρα και τις συμβάσεις ονομασίας θα πρέπει να ακολουθήσετε όταν δημιουργείτε ένα νέο άρθρο." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="tysonn" />

#<a name="file-names-and-locations-for-azure-technical-articles"></a>Ονόματα αρχείων και θέσεις για τεχνικά άρθρα Azure

Στο μας τεχνική περιεχομένου αποθετήριο, μπορούμε να χρησιμοποιήσουμε ένα φάκελο (ο φάκελος **άρθρα** ) για όλα τα άρθρα. Δεν υπάρχει καμία ιεραρχία φακέλων - όλα τα άρθρα live στη δομή της επίπεδο αρχείο. Εάν δημιουργείτε φακέλους με άρθρα σε αυτά, δεν μπορεί να δημοσιευτεί άρθρα σας.

Αντί να χρησιμοποιήσετε μια δομή αρχείου ως μια οργάνωση αρχή, χρησιμοποιούμε ένα κανόνες ονοματοθεσίας αυστηρών αρχείου που προσδιορίζει τα θέματα με σαφήνεια και που συμβάλλει στη δυνατότητα εντοπισμού στο web.

Ακολουθεί τι πρέπει να γνωρίζετε:

+ [Κανόνες]
+ [Μοτίβο]
+ [Τυπική παραδείγματα]
+ [Αγορά του περιεχομένου]
+ [Έγκριση όνομα αρχείου]

##<a name="rules"></a>Κανόνες

- Τα αρχεία μπορούν να περιέχουν ΜΌΝΟ πεζά γράμματα, αριθμούς και παύλες. 
- Δεν υπάρχουν κενά διαστήματα ή χαρακτήρες στίξης. Χρησιμοποιήστε τα ενωτικά για να διαχωρίσετε τις λέξεις και αριθμούς στο όνομα αρχείου.
- Λιγότερα από 80 χαρακτήρες - αυτό είναι ένα όριο συστήματος δημοσίευσης
- Χρήση ενέργειας ρήματα που αφορούν συγκεκριμένα, όπως ανάπτυξη, αγορά, δημιουργία, αντιμετώπιση προβλημάτων. Χωρίς λέξεις - ση.
- Χωρίς μικρές λέξεις - δεν περιλαμβάνουν μια και, το, σε ή, κ.λπ.
- Όλα τα αρχεία που πρέπει να είναι markdown και να χρησιμοποιούν την επέκταση αρχείου .md.
- Τον ορθογραφικό τις λέξεις. αποφύγετε δεν έχουν εγκριθεί ή δεν είναι απαραίτητες αρκτικόλεξα σε ονόματα αρχείων

Αρκτικόλεξα και initialisms σε ονόματα αρχείων - συγκεκριμένες οδηγίες:

- Μην χρησιμοποιείτε συντομογραφία ονόματα υπηρεσιών Azure - οι πρώτες λέξεις από το όνομα του αρχείου θα πρέπει να είναι το πρότυπο, ολογράφως Azure όνομα υπηρεσία ή τεχνολογία. 
-   Δεν επιτρέπεται διαχείριση πόρων ή arm ως αρκτικόλεξα σε οποιοδήποτε σημείο σε ένα όνομα αρχείου
- Άλλες συντομογραφίες βιομηχανικών προτύπων είναι αποδεκτό ως απαραίτητες σε ονόματα αρχείων. 

##<a name="pattern"></a>Μοτίβο

Ακολουθεί το γενικά μοτίβο:

 **Service-Platform-Language-Content-Product-Version.MD**

Χρήση των τμημάτων του μοτίβου που εφαρμογή και εξετάστε τη λίστα των άρθρων στο αποθετήριο δεδομένων για να λάβετε μια ιδέα για υπάρχοντα ονόματα. Ονόματα που δεν ξεκινούν με μια πλατφόρμα αποκλίσεις ή ένα όνομα υπηρεσίας είναι πιθανώς υποψιάζεστε ότι και Αποκλίνουσες μέσω.

##<a name="standard-examples"></a>Τυπική παραδείγματα

Ακολουθούν μερικά παραδείγματα έγκυρα ονόματα που ακολουθούν το μοτίβο. :

- cloud-υπηρεσίες-dotnet-συνεχής-delivery.md
- Mobile-υπηρεσίες-ios-get-started.md
- documentdb Διαχείριση account.md
- Mobile-Services-DotNet-backend-Get-Started-Settings-Sync.MD
- Active-Directory-Java-Authenticate-Users-Access-Control-eclipse.MD
- Virtual-machines-Install-Windows-Server-2008r2.MD
- cache-aspnet--κατάσταση-υπηρεσία παροχής περιόδου λειτουργίας
- Azure-SDK-DotNet-Release-Notes-2-8
- storsimple-Disaster-Recovery-using-Azure-Site-Recovery

##<a name="marketplace-content"></a>Αγορά του περιεχομένου

Για να διακρίνετε περιεχομένου που εστιάζει σε εισφορών συνεργάτη για να το Azure marketplace, ξεκινήστε τα ονόματα αρχείων με "αγορά". Αυτό το περιεχόμενο δεν πρέπει να είναι πολύ κοινές, όπως θα πρέπει να δημιουργηθεί μεγαλύτερο μέρος του περιεχομένου συνεργάτη στις τοποθεσίες web των συνεργατών.

- Marketplace-mongodb-Virtual-machines-Install-Windows-Server-2008r2.MD

##<a name="file-name-approval"></a>Έγκριση όνομα αρχείου

Είναι η δουλειά μας ομάδας ελκυστική αναθεωρητές αίτηση για να δείτε τα ονόματα αρχείων όταν υποβάλλετε ένα νέο αρχείο στο αποθετήριο δεδομένων για πρώτη φορά. Αναθεωρητές αίτηση ελκυστική πρέπει να εξετάσετε το όνομα του αρχείου και παροχή σχολίων μέσω στη ροή ελκυστική αίτηση σχόλιο εάν απαιτούνται αλλαγές. Το όνομα του αρχείου πρέπει να διορθωθεί πριν την αίτηση ελκυστική γίνει αποδεκτή. Οι συνεργάτες μπορούν να push εύκολα την ενημέρωση στην πρόσκληση σε ελκυστική σε εκκρεμότητα.

##<a name="folder-names-in-the-repo"></a>Ονόματα φακέλων στο το repo

Οι φάκελοι θα πρέπει να δημιουργηθούν μόνο για τις υπηρεσίες και το όνομα του αρχείου θα πρέπει να συμφωνεί με την υπηρεσία περιθωρίου. Χρησιμοποιήστε μόνο γράμματα και παύλες, και πεζά γράμματα. Λάβετε έγκριση από το διαχειριστή του αποθετηρίου πριν να δημιουργήσετε ένα νέο φάκελο που δεν είναι για μια τελική υπηρεσία.

##<a name="changing-case-in-file-names"></a>Αλλαγή πεζών-κεφαλαίων σε ονόματα αρχείων

Λειτουργικά συστήματα Windows είναι πεζών, επομένως εάν χρειάζεστε για να αλλάξετε ένα όνομα αρχείου για να διορθώσετε περίβλημα, είναι καλύτερα να κάνετε μια αλλαγή επί της ουσίας, εκτός εάν είστε σε θέση να κάνετε την αλλαγή σε Linux ή Mac. Για παράδειγμα:

  biztalk-administration-and-Development-Task-List-in-BizTalk-Services--> biztalk-services-administration-and-development-task-list

Χρησιμοποιήστε την παρακάτω εντολή για να μετονομάσετε ένα αρχείο:
```
  git mv <articles/service-folder/current-file-name.md> <articles/service-folder/new-file-name>
```

###<a name="contributors-guide-links"></a>Συνδέσεις Οδηγός των συνεργατών

- [Το άρθρο Επισκόπηση](./../README.md)
- [Ευρετήριο άρθρων με οδηγίες](./contributor-guide-index.md)


<!--Anchors-->
[Κανόνες]: #rules
[Μοτίβο]: #pattern
[Τυπική παραδείγματα]: #standard-examples
[Αγορά του περιεχομένου]: #marketplace-content
[Έγκριση όνομα αρχείου]: #file-name-approval