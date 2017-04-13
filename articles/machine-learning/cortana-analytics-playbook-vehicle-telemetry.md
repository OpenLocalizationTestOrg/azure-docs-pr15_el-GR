<properties 
    pageTitle="Οχήματος τηλεμετρίας ανάλυση λύση playbook | Microsoft Azure" 
    description="Χρησιμοποιήστε τις δυνατότητες της Cortana πληροφοριών για να αποκτήσετε σε πραγματικό χρόνο και πρόβλεψης ιδεών οχήματος εύρυθμης λειτουργίας και την οι συνήθειές όσον αφορά." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Playbook λύση ανάλυση τηλεμετρίας οχήματος

Αυτό **μενού** συνδέσεις για τα κεφάλαια στο αυτό playbook. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Επισκόπηση
Υπερ-υπολογιστές έχουν μετακινηθεί από το εργαστήριο και τώρα σε στάθμευση στο γκαράζ μας! Αυτές οι αιχμής αυτοκινήτων περιέχουν άπειρα αισθητήρες, δίνοντάς τους τη δυνατότητα να παρακολουθείτε εκατομμύρια συμβάντα ανά δευτερόλεπτο. Αναμένεται ότι ως το 2020, περισσότερες από αυτές τις αυτοκίνητα θα έχουν συνδεθεί με το internet. Φανταστείτε πάτημα σε αυτό πληθώρα δεδομένων για την παροχή καλύτερα στο εκπαιδευτικό ασφάλεια, την αξιοπιστία και οδήγησης εμπειρία! Η Microsoft έχει κάνει αυτό θέλετε μια πραγματικότητα με Cortana πληροφοριών.

Ευφυΐας Cortana της Microsoft είναι ένα πλήρως διαχειριζόμενων big data και οικογένεια προηγμένη ανάλυση που σας επιτρέπει να μετατρέψετε τα δεδομένα σας σε έξυπνη ενέργεια. Θέλουμε να εξοικειωθείτε με το πρότυπο λύση Cortana πληροφοριών οχήματος Τηλεμετρίας ανάλυσης. Αυτή η λύση παρουσιάζει πώς αντιπροσωπειών αυτοκίνητο, κατασκευαστές αυτοκινήτων και ασφάλισης εταιρείες μπορούν να χρησιμοποιούν τις δυνατότητες της Cortana πληροφοριών για σε πραγματικό χρόνο και πρόβλεψης ιδέες για την υγεία οχήματος και την οι συνήθειές όσον αφορά. 

Η λύση εφαρμόζεται ως ένα [μοτίβο αρχιτεκτονική λάμδα](https://en.wikipedia.org/wiki/Lambda_architecture) που εμφανίζει το πλήρες πιθανά πλατφόρμας Cortana πληροφοριών για σε πραγματικό χρόνο και μαζική επεξεργασία. Η λύση: 

- παρέχει προσομοιωτή τηλεματικές οχήματος
- αξιοποιεί διανομείς συμβάντων για ingesting εκατομμύρια προσομοιωμένη οχήματος τηλεμετρίας συμβάντα σε Azure 
- χρησιμοποιεί ανάλυση ροής για να αποκτήσει σε πραγματικό χρόνο ιδέες σχετικά με κατάσταση οχήματος
-  εξακολουθεί να εμφανίζεται τα δεδομένα σε μακροπρόθεσμες χώρου αποθήκευσης για πιο αναλυτικών στοιχείων δέσμης. 
- αξιοποιεί μηχανικής εκμάθησης για τον εντοπισμό ανωμαλία στο σε πραγματικό χρόνο και μαζική επεξεργασία για να αποκτήσετε πρόβλεψης ιδέες.
- αξιοποιεί HDInsight για τη μετατροπή των δεδομένων σε κλίμακα και εργοστασίου δεδομένων για το χειρισμό ενορχήστρωσης, τον προγραμματισμό, διαχείριση πόρων και παρακολούθηση των της διοχέτευσης μαζική επεξεργασία 
- παρέχει αυτήν τη λύση εμπλουτισμένου πίνακα εργαλείων για δεδομένα σε πραγματικό χρόνο και πρόβλεψης ανάλυση απεικονίσεις χρησιμοποιώντας Power BI

## <a name="architecture"></a>Αρχιτεκτονική

![](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Σχήμα 1-αρχιτεκτονική λύσης ανάλυση Τηλεμετρίας οχήματος*

Αυτή η λύση περιλαμβάνει τα παρακάτω **στοιχεία πληροφοριών Cortana** και σάς δείχνουν την ενοποίηση τελικών


- **Διανομείς συμβάντων** για ingesting εκατομμύρια οχήματος τηλεμετρίας συμβάντα σε Azure.
- **Ανάλυση ροής** για να αποκτήσετε σε πραγματικό χρόνο ιδέες σχετικά με κατάσταση οχήματος και εξακολουθεί να εμφανίζεται ότι τα δεδομένα σε μακροπρόθεσμες χώρου αποθήκευσης για πιο αναλυτικών στοιχείων δέσμης.
- **Μηχανικής εκμάθησης** για τον εντοπισμό ανωμαλία σε πραγματικό χρόνο και μαζική επεξεργασία για να αποκτήσετε πρόβλεψης ιδέες.
- **HDInsight** είναι δυνατή για το μετασχηματισμό δεδομένων σε κλίμακα
- **Δεδομένα εργοστασίου** χειρίζεται ενορχήστρωσης, τον προγραμματισμό, διαχείριση πόρων και την παρακολούθηση της διοχέτευσης μαζική επεξεργασία.
- **Power BI** παρέχουν αυτήν τη λύση εμπλουτισμένου πίνακα εργαλείων για δεδομένα σε πραγματικό χρόνο και απεικονίσεων πρόβλεψης ανάλυσης.

Αυτή η λύση αποκτά πρόσβαση σε δύο διαφορετικές **προελεύσεις δεδομένων**: 

- **Προσομοιωμένη οχήματος σήματα και τα Διαγνωστικά**: προσομοιωτή τηλεματικές οχήματος εκπέμπει διαγνωστικών πληροφοριών και σήματα που αντιστοιχούν στην κατάσταση του οχήματος και το μοτίβο οδήγησης σε μια δεδομένη χρονική στιγμή. 
- **Κατάλογος οχήματος**: ένα σύνολο δεδομένων αναφοράς που περιέχει μια Κατάσταση αντιστοίχιση μοντέλο.