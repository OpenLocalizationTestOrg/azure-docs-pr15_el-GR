<properties
   pageTitle="Πώς μπορείτε να εργαστείτε με αρχεία προέλευσης δεδομένων 'big data' | Microsoft Azure"
   description="Άρθρο διαδικασιών με την επισήμανση μοτίβων για τη χρήση του καταλόγου δεδομένων του Azure με προελεύσεις δεδομένων 'big data', συμπεριλαμβανομένων χώρο αποθήκευσης Blob του Azure, λίμνης δεδομένων Azure και Hadoop HDFS."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Πώς μπορείτε να εργαστείτε με προελεύσεις δεδομένων μεγάλο στον κατάλογο δεδομένων Azure

## <a name="introduction"></a>Εισαγωγή
**Κατάλογος δεδομένων Microsoft Azure** είναι μια υπηρεσία cloud πλήρως διαχειριζόμενων που χρησιμοποιείται ως ενός συστήματος ταξινόμησης και το σύστημα του εντοπισμού για προελεύσεις δεδομένων για μεγάλες επιχειρήσεις. Με άλλα λόγια, **Καταλόγου Azure δεδομένων** είναι όλα σχετικά με άτομα βοήθεια στην ανακαλύψετε, να κατανοήσετε και να χρησιμοποιήσετε προελεύσεις δεδομένων και παροχή βοήθειας εταιρείες για να λάβετε περισσότερες τιμή από τις υπάρχουσες προελεύσεις δεδομένων, συμπεριλαμβανομένων μεγάλο δεδομένων.

**Κατάλογος δεδομένων Azure** υποστηρίζει την καταχώρηση του Azure ιστολογίου αποθήκευσης αντικειμένων blob και σε καταλόγους, καθώς και τα αρχεία Hadoop HDFS και καταλόγων. Η ημι-δομημένα φύση αυτών των προελεύσεων δεδομένων παρέχει μεγάλη ευελιξία, αλλά αυτό σημαίνει επίσης ότι οι χρήστες πρέπει να εξετάσετε τον τρόπο οργάνωσης των προελεύσεων δεδομένων για να λάβετε την τιμή περισσότερες από την εγγραφή σας με **Τον κατάλογο δεδομένων Azure**.

## <a name="directories-as-logical-data-sets"></a>Σε καταλόγους ως λογική σύνολα δεδομένων

Ένα κοινό μοτίβο για την οργάνωση των προελεύσεων δεδομένων μεγάλο είναι να χειριστείτε σε καταλόγους ως λογική σύνολα δεδομένων. Ανώτατου επιπέδου σε καταλόγους που χρησιμοποιούνται για τον ορισμό ενός συνόλου δεδομένων, ενώ οι υποφάκελοι καθορίσετε τα διαμερίσματα και τα αρχεία που περιέχουν αποθηκεύουν τα ίδια τα δεδομένα.

Παράδειγμα αυτό το μοτίβο μπορεί να είναι:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

Σε αυτό το παράδειγμα, vehicle_maintenance_events και location_tracking_events αντιπροσωπεύουν λογική σύνολα δεδομένων. Κάθε έναν από αυτούς τους φακέλους περιέχει αρχεία δεδομένων, το οποίο είναι οργανωμένο κατά έτος και μήνα σε υποφακέλους. Κάθε έναν από αυτούς τους φακέλους πιθανόν περιέχουν εκατοντάδες ή χιλιάδες αρχεία.

Σε αυτό το μοτίβο, την καταχώρηση μεμονωμένων αρχείων με **Τον κατάλογο δεδομένων Azure** μάλλον δεν έχει νόημα. Αντί για αυτό, η καταχώρηση καταλόγους που αντιπροσωπεύει τα σύνολα δεδομένων που θα είναι κατανοητά για τους χρήστες που εργάζονται με τα δεδομένα.

## <a name="reference-data-files"></a>Αρχεία δεδομένων αναφοράς

Συμπληρωματική μοτίβο είναι να αποθηκεύσετε σύνολα δεδομένων αναφοράς ως μεμονωμένα αρχεία. Αυτά τα σύνολα δεδομένων μπορεί να θεωρηθεί ως η 'μικρή' πλευρά μεγάλο δεδομένων και συχνά είναι παρόμοιες με τις διαστάσεις σε ένα μοντέλο ανάλυσης δεδομένων. Αρχεία δεδομένων αναφοράς περιέχει τις εγγραφές που χρησιμοποιούνται για την παροχή περιβάλλοντος μαζικής τα αρχεία δεδομένων που είναι αποθηκευμένα σε κάποιο άλλο σημείο στο χώρο αποθήκευσης μεγάλο δεδομένων.

Παράδειγμα αυτό το μοτίβο μπορεί να είναι:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Όταν μια υπεύθυνος δημιουργίας αναλυτής ή τα δεδομένα εργάζεστε με τα δεδομένα που περιέχει το μεγαλύτερο δομές καταλόγου, τα δεδομένα σε αυτά τα αρχεία αναφορά μπορεί να χρησιμοποιηθεί για την παροχή πιο λεπτομερείς πληροφορίες για οντοτήτων που αναφέρονται μόνο με όνομα ή το Αναγνωριστικό στο μεγαλύτερο σύνολο δεδομένων.

Σε αυτό το μοτίβο, αυτό θα έχει νόημα να καταχωρήσετε τα αρχεία δεδομένων μεμονωμένα αναφοράς με **Τον κατάλογο δεδομένων Azure**. Κάθε αρχείο αντιπροσωπεύει ένα σύνολο δεδομένων και μπορεί να περιέχει σχόλια και να εντόπισε μεμονωμένα κάθε μία.

## <a name="alternate-patterns"></a>Εναλλακτική μοτίβα

Τα μοτίβα που περιγράφεται παραπάνω είναι δύο μόνο τους πιθανούς τρόπους που μπορείτε να οργανώσετε μια χώρου αποθήκευσης μεγάλο δεδομένων, αλλά κάθε εφαρμογή θα είναι διαφορετικές. Ανεξάρτητα από τις προελεύσεις δεδομένων είναι δομής, κατά την καταχώρηση προελεύσεων δεδομένων μεγάλο με **Κατάλογο δεδομένων Azure**, εστίαση σχετικά με την καταχώρηση των αρχείων και καταλόγων που αντιπροσωπεύουν τα σύνολα δεδομένων που θα είναι της τιμής σε άλλα άτομα εντός της εταιρείας σας. Εγγραφή για όλα τα αρχεία και καταλόγους να δευτερεύουσας αλληλογραφίας του καταλόγου, καθιστώντας πιο δύσκολο για τους χρήστες για να βρείτε ό, τι χρειάζονται.

## <a name="summary"></a>Σύνοψη
Καταχώρηση προελεύσεων δεδομένων με **Azure κατάλογο δεδομένων** κάνουν ευκολότερη για τον εντοπισμό και την κατανόηση. Κατά την καταχώρηση και επισήμανση των αρχείων δεδομένων μεγάλο και καταλόγων που αντιπροσωπεύουν λογική σύνολα δεδομένων, μπορείτε να βοηθήσετε τους χρήστες να βρείτε και να χρησιμοποιήσετε τις προελεύσεις δεδομένων μεγάλο που χρειάζονται.