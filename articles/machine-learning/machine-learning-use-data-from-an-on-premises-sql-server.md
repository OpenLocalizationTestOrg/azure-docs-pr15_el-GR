<properties
pageTitle="Χρήση δεδομένων από μια βάση δεδομένων SQL Server εσωτερικής εγκατάστασης σε μηχανικής εκμάθησης | Azure"
description="Χρήση δεδομένων από μια βάση δεδομένων SQL Server εσωτερικής εγκατάστασης για να εκτελέσετε προηγμένη ανάλυση με Azure μηχανικής εκμάθησης."
services="machine-learning"
documentationCenter=""
authors="garyericson"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/16/2016"
ms.author="garye;krishnan"/>

# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Εκτέλεση προηγμένη ανάλυση με Azure μηχανικής εκμάθησης με τη χρήση δεδομένων από μια βάση δεδομένων SQL Server εσωτερικής εγκατάστασης


Συχνά επιχειρήσεων που λειτουργούν με τα δεδομένα εσωτερικής εγκατάστασης θα θέλατε να επωφεληθείτε από την κλίμακα και ευελιξία από το cloud για τους μηχανικής εκμάθησης φόρτους εργασίας. Αλλά δεν θέλουν να διακόψει την τρέχουσα επιχειρηματικές διαδικασίες και ροές εργασίας, μετακινώντας τα δεδομένα εσωτερικής εγκατάστασης στο cloud. Azure μηχανικής εκμάθησης υποστηρίζει πλέον ανάγνωσης των δεδομένων σας από μια βάση δεδομένων SQL Server εσωτερικής εγκατάστασης και, στη συνέχεια, εκπαίδευση και βαθμολογίας μοντέλου με αυτά τα δεδομένα. Δεν χρειάζεται πλέον να αντιγράψετε και να συγχρονίσετε τα δεδομένα μεταξύ του στο cloud και διακομιστή εσωτερικής εγκατάστασης με μη αυτόματο τρόπο. Αντί για αυτό, τη λειτουργική μονάδα **Εισαγωγή δεδομένων** στο Azure μηχανικής εκμάθησης Studio μπορεί να εμφανίζει την ένδειξη απευθείας από τη βάση δεδομένων SQL Server εσωτερικής εγκατάστασης για την εκπαίδευση και βαθμολογίας εργασίες. 

Σε αυτό το άρθρο παρέχει μια επισκόπηση του τρόπου εισόδου εσωτερικής δεδομένα του SQL server σε Azure μηχανικής εκμάθησης. Προϋποθέτει ότι είστε εξοικειωμένοι με το Azure μηχανικής εκμάθησης έννοιες όπως χώρους εργασίας, λειτουργικές μονάδες, σύνολα δεδομένων, δοκιμές, *κλπ*...

> [AZURE.NOTE] Αυτή η δυνατότητα δεν είναι διαθέσιμη για τους χώρους εργασίας δωρεάν. Για περισσότερες πληροφορίες σχετικά με τις τιμές μηχανικής εκμάθησης και επίπεδα, ανατρέξτε στο θέμα [Τις τιμές του Azure μηχανικής εκμάθησης](https://azure.microsoft.com/pricing/details/machine-learning/).

<!-- --> 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="install-the-microsoft-data-management-gateway"></a>Εγκατάσταση της πύλης διαχείρισης δεδομένων της Microsoft

Για να αποκτήσετε πρόσβαση σε μια βάση δεδομένων SQL Server εσωτερικής εγκατάστασης στο Azure μηχανικής εκμάθησης, πρέπει να κάνετε λήψη και εγκατάσταση της πύλης διαχείρισης δεδομένων της Microsoft. Όταν ρυθμίζετε τις παραμέτρους της σύνδεσης πύλης στο μηχανικής εκμάθησης Studio θα έχετε την ευκαιρία να κάνετε λήψη και εγκατάσταση της πύλης χρησιμοποιώντας το **λήψης και register δεδομένων πύλη** παράθυρο διαλόγου που περιγράφεται παρακάτω.

Μπορείτε επίσης να εγκαταστήσετε την πύλη διαχείρισης δεδομένων βρίσκεστε μπροστά από το χρόνο κατά τη λήψη και την εκτέλεση του πακέτου εγκατάστασης MSI από το [Κέντρο λήψης της Microsoft](https://www.microsoft.com/download/details.aspx?id=39717). Επιλέξτε την πιο πρόσφατη έκδοση, επιλέγοντας 32-bit ή 64-bit, ανάλογα με τον υπολογιστή σας. Το MSI μπορεί επίσης να χρησιμοποιηθεί για την αναβάθμιση μιας υπάρχουσας πύλη διαχείρισης δεδομένων για την πιο πρόσφατη έκδοση, με όλες τις ρυθμίσεις διατηρούνται.

Η πύλη περιλαμβάνει τις ακόλουθες προϋποθέσεις:

- Οι υποστηριζόμενες εκδόσεις λειτουργικό σύστημα Windows είναι Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 και Windows Server 2012 R2.
- Η συνιστώμενη ρύθμιση παραμέτρων για υπολογιστή πύλης είναι τουλάχιστον 2 GHz, 4 πυρήνων, 8 GB RAM και δίσκου 80 GB.
- Εάν αδρανοποίηση τον κεντρικό υπολογιστή, η πύλη δεν θα μπορούν να ανταποκριθεί στις αιτήσεις δεδομένων. Γι ' αυτό, ρυθμίστε τις παραμέτρους ενός σχεδίου κατάλληλο power στον υπολογιστή πριν από την εγκατάσταση της πύλης. Η εγκατάσταση πύλης εμφανίζει ένα μήνυμα, εάν ο υπολογιστής έχει ρυθμιστεί για αδρανοποίηση.
- Επειδή γίνεται αντιγραφή δραστηριότητας σε συγκεκριμένη συχνότητα, τη χρήση πόρων (CPU, μνήμη) στον υπολογιστή ακολουθεί επίσης το ίδιο μοτίβο με κορύφωσης και ώρες αδράνειας. Χρήση των πόρων επίσης εξαρτάται σε μεγάλο βαθμό την ποσότητα των δεδομένων που μετακινείται. Όταν πολλές εργασίες αντίγραφο βρίσκονται σε εξέλιξη που θα παρατηρήσετε μετακινηθείτε προς τα επάνω κατά τη διάρκεια ώρες αιχμής "Χρήση πόρων". Ενώ η ελάχιστη ρύθμιση παραμέτρων που αναφέρονται παραπάνω είναι τεχνικά αρκετό, μπορείτε να έχετε μια ρύθμιση παραμέτρων με περισσότερους πόρους από την ελάχιστη ρύθμιση των παραμέτρων ανάλογα με τις συγκεκριμένες φόρτωσης για τη μεταφορά δεδομένων.

Θα πρέπει να λάβετε υπόψη τα εξής κατά την εγκατάσταση και τη χρήση μιας πύλης διαχείρισης δεδομένων:

- Μπορείτε να εγκαταστήσετε μόνο μία παρουσία της πύλης διαχείρισης δεδομένων σε έναν υπολογιστή.
- Μπορείτε να χρησιμοποιήσετε μια μοναδική πύλη για πολλές προελεύσεις δεδομένων εσωτερικής εγκατάστασης.
- Μπορείτε να συνδεθείτε πολλές πύλες σε διαφορετικούς υπολογιστές στην ίδια προέλευση δεδομένων εσωτερικής εγκατάστασης.
- Μπορείτε να ρυθμίσετε μια πύλη για μόνο ένα χώρο εργασίας κάθε φορά. Πύλες δεν μπορεί να είναι κοινόχρηστη σε χώρους εργασίας αυτήν τη στιγμή.
- Μπορείτε να ρυθμίσετε πολλές πύλες για ένα μεμονωμένο χώρο εργασίας. Για παράδειγμα, μπορεί να θέλετε να χρησιμοποιήσετε μια πύλη που είναι συνδεδεμένη με τις προελεύσεις δεδομένων έλεγχο κατά την ανάπτυξη και την πύλη παραγωγής όταν είστε έτοιμοι να operationalize.
- Η πύλη δεν χρειάζεται να είναι στον ίδιο υπολογιστή με το αρχείο προέλευσης δεδομένων, αλλά παραμονή πιο κοντά στο αρχείο προέλευσης δεδομένων μειώνει το χρόνο για την πύλη για να συνδεθείτε στην προέλευση δεδομένων. Συνιστάται να εγκαταστήσετε την πύλη σε έναν υπολογιστή που είναι διαφορετικό από αυτό που φιλοξενεί την προέλευση δεδομένων εσωτερικής εγκατάστασης, έτσι ώστε η πύλη και της προέλευσης δεδομένων δεν διαγωνιστούν για τους πόρους.
- Εάν έχετε ήδη μια πύλη εγκατεστημένο στον υπολογιστή σας σερβίρισμα του Power BI ή εργοστασίου δεδομένων Azure σενάρια, εγκαταστήσετε μια ξεχωριστή πύλη για Azure μηχανικής εκμάθησης σε άλλον υπολογιστή. 

    > [AZURE.NOTE] Δεν μπορείτε να εκτελέσετε την πύλη διαχείρισης δεδομένων και Power BI πύλης στον ίδιο υπολογιστή.

- Πρέπει να χρησιμοποιήσετε την πύλη διαχείρισης δεδομένων για Azure μηχανικής εκμάθησης, ακόμα και αν χρησιμοποιείτε Azure ExpressRoute για άλλα δεδομένα. Θα πρέπει να διαχειρίζεστε το αρχείο προέλευσης δεδομένων ως προέλευση δεδομένων εσωτερικής εγκατάστασης (δηλαδή, πίσω από ένα τείχος προστασίας) ακόμα και όταν χρησιμοποιείτε ExpressRoute, και χρησιμοποιήστε την πύλη διαχείρισης δεδομένων για να δημιουργήσετε τη σύνδεση μεταξύ μηχανικής εκμάθησης και το αρχείο προέλευσης δεδομένων. 

Μπορείτε να βρείτε λεπτομερείς πληροφορίες σχετικά με τις προϋποθέσεις εγκατάστασης, βήματα εγκατάστασης και συμβουλές αντιμετώπισης προβλημάτων στο άρθρο, [Μετακίνηση δεδομένων μεταξύ προελεύσεις εσωτερικής εγκατάστασης και cloud με την πύλη διαχείρισης δεδομένων](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway), ξεκινώντας από την ενότητα, [ζητήματα σχετικά με τη χρήση της πύλης διαχείρισης δεδομένων](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway).

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Δεδομένα εισόδου από τη βάση δεδομένων SQL Server εσωτερικής εγκατάστασης σε Azure μηχανικής εκμάθησης


Σε αυτές τις οδηγίες, που θα ρυθμίσετε πύλης διαχείρισης δεδομένων σε ένα χώρο εργασίας Azure μηχανικής εκμάθησης, ρυθμίστε τις παραμέτρους του και, στη συνέχεια, διαβάστε δεδομένων από μια βάση δεδομένων SQL Server εσωτερικής εγκατάστασης.

> [AZURE.TIP] Πριν ξεκινήσετε, απενεργοποίηση αποκλεισμού αναδυόμενων παραθύρων του προγράμματος περιήγησής σας για `studio.azureml.net`. Εάν χρησιμοποιείτε το πρόγραμμα περιήγησης Google Chrome, λήψη και εγκατάσταση ένα από τα αρκετές προσθήκες είναι διαθέσιμο στη χώρου αποθήκευσης στο Web Google Chrome [Κάντε κλικ μία φορά εφαρμογή επέκταση](https://chrome.google.com/webstore/search/clickonce?_category=extensions).

### <a name="step-1-create-a-gateway"></a>Βήμα 1: Δημιουργία μιας πύλης

Το πρώτο βήμα είναι να δημιουργήσετε και να ρυθμίσετε την πύλη για να αποκτήσετε πρόσβαση σε βάση δεδομένων SQL εσωτερικής εγκατάστασης.

1. Συνδεθείτε στο [Azure μηχανικής εκμάθησης Studio](https://studio.azureml.net/Home/) και επιλέξτε το χώρο εργασίας που θέλετε να εργαστείτε.

2. Κάντε κλικ στην επιλογή το blade **ΡΥΘΜΊΣΕΙΣ** στα αριστερά και, στη συνέχεια, κάντε κλικ στην καρτέλα **ΠΎΛΕΣ ΔΕΔΟΜΈΝΩΝ** στο επάνω μέρος.

3. Κάντε κλικ στην επιλογή **ΝΈΑ ΠΎΛΗ ΔΕΔΟΜΈΝΩΝ** στο κάτω μέρος της οθόνης.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)

4. Στο παράθυρο διαλόγου **νέα πύλη δεδομένων** , πληκτρολογήστε το **Όνομα πύλης** και προαιρετικά να προσθέσετε μια **Περιγραφή**. Κάντε κλικ στο βέλος στην κάτω δεξιά γωνία για να μεταβείτε στο επόμενο βήμα της ρύθμισης παραμέτρων.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)

5. Στο λήψης και register δεδομένων πύλης στο παράθυρο, αντιγράψτε το ΚΛΕΙΔΊ ΠΎΛΗΣ ΕΓΓΡΑΦΉΣ στο Πρόχειρο.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)

6. <span id="note-1" class="anchor"></span>Εάν δεν έχουν ακόμα έχουν ληφθεί και εγκατεστημένη η πύλη διαχείρισης δεδομένων της Microsoft, στη συνέχεια, κάντε κλικ στην επιλογή **λήψη πύλη διαχείρισης δεδομένων**. Ενέργεια αυτή σας μεταφέρει στο Κέντρο λήψης της Microsoft, όπου μπορείτε να επιλέξετε την έκδοση πύλης που χρειάζεστε, κάντε λήψη του και να το εγκαταστήσετε. Μπορείτε να βρείτε λεπτομερείς πληροφορίες σχετικά με τις προϋποθέσεις εγκατάστασης, βήματα εγκατάστασης και συμβουλές αντιμετώπισης προβλημάτων στις ενότητες αρχή του άρθρου [Μετακίνηση δεδομένων μεταξύ προελεύσεις εσωτερικής εγκατάστασης και cloud με την πύλη διαχείρισης δεδομένων](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).

7. Μετά την εγκατάσταση της πύλης, θα ανοίξει η Διαχείριση παραμέτρων πύλης διαχείρισης δεδομένων και εμφανίζεται το παράθυρο διαλόγου **καταχωρήσετε πύλη** . Επικολλήστε το **Κλειδί πύλης εγγραφής** που αντιγράψατε στο Πρόχειρο και κάντε κλικ στην επιλογή **καταχώρηση**.

8. Εάν έχετε ήδη μια πύλη που είναι εγκατεστημένη, εκτελέστε τη Διαχείριση παραμέτρων πύλης διαχείρισης δεδομένων, κάντε κλικ στην επιλογή **Αλλαγή αριθμού-κλειδιού**, επικολλήστε το  **Κλειδί πύλης εγγραφής** που αντιγράψατε στο Πρόχειρο και κάντε κλικ στο κουμπί **OK**.

9. Όταν ολοκληρωθεί η εγκατάσταση, εμφανίζεται το **καταχωρήσετε πύλη** παράθυρο διαλόγου για το Microsoft δεδομένων παραμέτρων πύλης διαχείρισης. Επικολλήστε το ΚΛΕΙΔΊ ΔΉΛΩΣΗΣ ΠΎΛΗΣ που αντιγράψατε στο Πρόχειρο παραπάνω και κάντε κλικ στην επιλογή **καταχώρηση**.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)

10. Η ρύθμιση παραμέτρων της πύλης έχει ολοκληρωθεί όταν τις παρακάτω τιμές έχουν οριστεί στην καρτέλα **αρχική** στο Microsoft δεδομένων παραμέτρων πύλης διαχείρισης:

    - **Όνομα πύλης** και το **όνομα της παρουσίας** ορίζονται για το όνομα της πύλης.

    - **Καταχώρηση** έχει οριστεί σε **καταχωρημένο**.

    - **Κατάσταση** έχει οριστεί σε **ξεκίνησε**.

    - Η γραμμή κατάστασης στο κάτω μέρος εμφανίζει **σύνδεση σε υπηρεσία Cloud της πύλης διαχείρισης δεδομένων** μαζί με ένα πράσινο σημάδι ελέγχου.

     ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

     Azure μηχανικής εκμάθησης Studio επίσης ενημερώνεται όταν η καταχώρηση είναι επιτυχής.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-registered.png)

11. Στο παράθυρο διαλόγου **λήψη και καταχώρηση της πύλης δεδομένων** , κάντε κλικ στο σημάδι ελέγχου για να ολοκληρώσετε τη ρύθμιση. Η σελίδα " **Ρυθμίσεις** " εμφανίζει την κατάσταση πύλης ως "Online". Στο δεξιό τμήμα παραθύρου θα βρείτε κατάσταση και άλλες χρήσιμες πληροφορίες.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-status.png)

12. Στο παράθυρο Διαχείριση παραμέτρων πύλης διαχείρισης δεδομένων Microsoft Μετάβαση στη σελίδα " **πιστοποιητικό** ". Το πιστοποιητικό που καθορίζονται σε αυτήν την καρτέλα χρησιμοποιείται για την Κρυπτογράφηση/αποκρυπτογράφηση διαπιστευτήρια για το χώρο αποθήκευσης δεδομένων εσωτερικής εγκατάστασης που καθορίζετε στην πύλη. Αυτό είναι το προεπιλεγμένο πιστοποιητικό που δημιουργούνται. Η Microsoft συνιστά αλλαγή αυτό το δικό σας πιστοποιητικό που δημιουργείτε αντίγραφα ασφαλείας στο σύστημα διαχείρισης πιστοποιητικού. Κάντε κλικ στην επιλογή **Αλλαγή** για να χρησιμοποιήσετε το δικό σας πιστοποιητικό αντί για αυτό.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-certificate.png)

13. (προαιρετικό) Εάν θέλετε να ενεργοποιήσετε τη λεπτομερή καταγραφή για την αντιμετώπιση προβλημάτων με την πύλη, στο το Microsoft δεδομένων παραμέτρων πύλης διαχείρισης Μετάβαση στην καρτέλα **Εργαλεία διαγνωστικών** και ελέγξτε την επιλογή **Ενεργοποίηση Λεπτομερής καταγραφή για την αντιμετώπιση προβλημάτων** . Μπορείτε να βρείτε τις πληροφορίες σύνδεσης στο πρόγραμμα προβολής συμβάντων των Windows κάτω από τις **υπηρεσίες αρχεία καταγραφής εφαρμογών και**  - &gt; κόμβου **Πύλη διαχείρισης δεδομένων** . Μπορείτε επίσης να χρησιμοποιήσετε την καρτέλα **Εργαλεία διαγνωστικού ελέγχου** για να ελέγξετε τη σύνδεση με μια προέλευση δεδομένων εσωτερικής εγκατάστασης με χρήση της πύλης.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-verbose-logging.png)

Έτσι ολοκληρώνεται η πύλη ρύθμιση διεργασίας σε Azure μηχανικής εκμάθησης.
Τώρα είστε έτοιμοι να χρησιμοποιήσετε τα δεδομένα εσωτερικής εγκατάστασης.

Μπορείτε να δημιουργήσετε και να ρυθμίσετε πολλές πύλες σε Studio για κάθε χώρο εργασίας. Για παράδειγμα, μπορεί να έχετε μια πύλη που θέλετε να συνδεθείτε με τις προελεύσεις δεδομένων έλεγχο κατά την ανάπτυξη, και μια διαφορετική πύλη για τις προελεύσεις δεδομένων παραγωγής. Azure μηχανικής εκμάθησης παρέχει την ευελιξία να ρυθμίσετε πολλές πύλες ανάλογα με το εταιρικό περιβάλλον. Προς το παρόν, δεν μπορείτε να κάνετε κοινή χρήση μιας πύλης μεταξύ των χώρων εργασίας και μπορεί να εγκατασταθεί μόνο μία πύλη σε έναν υπολογιστή. Για περισσότερες πληροφορίες σχετικά με κατά την εγκατάσταση της πύλης, ανατρέξτε στο θέμα [ζητήματα σχετικά με τη χρήση της πύλης διαχείρισης δεδομένων](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway) στο άρθρο, [Μετακίνηση δεδομένων μεταξύ προελεύσεις εσωτερικής εγκατάστασης και cloud με την πύλη διαχείρισης δεδομένων](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>Βήμα 2: Χρήση της πύλης για την ανάγνωση δεδομένων από μια προέλευση δεδομένων εσωτερικής εγκατάστασης

Αφού ρυθμίσετε την πύλη, μπορείτε να προσθέσετε μια λειτουργική μονάδα **Εισαγωγή δεδομένων** σε μια έρευνα που εισροές τα δεδομένα από τη βάση δεδομένων SQL Server εσωτερικής εγκατάστασης.

1.  Στο μηχανικής εκμάθησης Studio, επιλέξτε την καρτέλα **ΔΟΚΙΜΈΣ** , κάντε κλικ στο κουμπί **+ ΝΈΑ** στην κάτω αριστερή γωνία, και επιλέξτε **Κενό έρευνας** (ή επιλέξτε μία από πολλές δείγμα δοκιμές διαθέσιμη).

2.  Βρείτε και σύρετε τη λειτουργική μονάδα **Εισαγωγή δεδομένων** στον καμβά έρευνας.

3.  Κάτω από τον καμβά, κάντε κλικ στην επιλογή **Αποθήκευση ως** . Πληκτρολογήστε "Azure μηχανικής εκμάθησης στην εσωτερική εγκατάσταση SQL Server πρόγραμμα εκμάθησης" για το όνομα της έρευνας, επιλέξτε το χώρο εργασίας και κάντε κλικ στο **κουμπί OK** σημάδι ελέγχου.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\experiment-save-as.png)

4.  Κάντε κλικ στην επιλογή της λειτουργικής μονάδας **Εισαγωγή δεδομένων** για να την επιλέξετε και, στη συνέχεια, στο παράθυρο **ιδιοτήτων** στα δεξιά του καμβά, επιλέξτε "Βάση δεδομένων SQL εσωτερικής εγκατάστασης" στην αναπτυσσόμενη λίστα **προέλευση δεδομένων** .

5.  Επιλέξτε την **πύλη δεδομένων** εγκατάσταση και την καταχώρηση. Μπορείτε να ρυθμίσετε μια άλλη πύλη, επιλέγοντας "(Προσθήκη νέα πύλη δεδομένων...)".

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-select-on-premises-data-source.png)

6.  Πληκτρολογήστε το **όνομα του διακομιστή βάσης δεδομένων** του SQL και το **όνομα της βάσης δεδομένων**, μαζί με το SQL **ερώτημα βάσης δεδομένων** που θέλετε να εκτελεί.

7.  Κάντε κλικ στο κουμπί **Enter τιμές** στην περιοχή **όνομα χρήστη και τον κωδικό πρόσβασης** και εισαγάγετε τα διαπιστευτήριά σας βάση δεδομένων. Μπορείτε να χρησιμοποιήσετε την ενσωματωμένη ελέγχου ταυτότητας των Windows ή ανάλογα με τη ρύθμιση των παραμέτρων του SQL Server εσωτερικής εγκατάστασης έλεγχο ταυτότητας του SQL Server.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\database-credentials.png)
    
    Το μήνυμα "απαιτείται τιμές" θα αλλάξει σε "σύνολο τιμών" με ένα πράσινο σημάδι ελέγχου. Πρέπει να εισαγάγετε τα διαπιστευτήρια μία φορά, εκτός εάν οι πληροφορίες βάσης δεδομένων ή τον κωδικό πρόσβασης αλλάζει. Azure μηχανικής εκμάθησης χρησιμοποιεί το πιστοποιητικό που παρείχατε κατά την εγκατάσταση της πύλης για να κρυπτογραφήσετε το διαπιστευτηρίων στο cloud. Azure θα ποτέ αποθήκευση των διαπιστευτηρίων εσωτερικής χωρίς κρυπτογράφηση.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-properties-entered.png)

8.  Κάντε κλικ στο κουμπί **ΕΚΤΈΛΕΣΗ** για να εκτελέσετε την έρευνα.

Μόλις ολοκληρωθεί η έρευνα εκτελούνται, μπορείτε να οπτικοποιήσετε τα δεδομένα που εισάγονται από τη βάση δεδομένων στη θύρα εξόδου της λειτουργικής μονάδας **Εισαγωγή δεδομένων** και επιλέγοντας **αναπαράσταση**.

Αφού ολοκληρώσετε την ανάπτυξη του έρευνας, μπορείτε να αναπτύξετε και να operationalize το μοντέλο. Με την υπηρεσία εκτέλεσης δέσμης, τα δεδομένα από τη βάση δεδομένων SQL Server εσωτερικής εγκατάστασης έχει ρυθμιστεί στη λειτουργική μονάδα **Εισαγωγή δεδομένων** θα Διαβάστε και χρησιμοποιείται για τη βαθμολογία. Ενώ μπορείτε να χρησιμοποιήσετε την υπηρεσία απόκρισης αίτηση για βαθμολογίας δεδομένων εσωτερικής εγκατάστασης, Microsoft συνιστά τη χρήση αντί για αυτό το [πρόσθετο του Excel](machine-learning-excel-add-in-for-web-services.md) . Προς το παρόν, εγγραφή σε έναν SQL Server εσωτερικής εγκατάστασης βάση δεδομένων μέσω **Εξαγωγή δεδομένων** δεν υποστηρίζεται είτε στο δοκιμών ή υπηρεσίες web δημοσιευμένα.
