<properties
  pageTitle="Azure IoT οικογένεια συνήθεις Ερωτήσεις | Microsoft Azure"
  description="Συνήθεις ερωτήσεις για την οικογένεια προγραμμάτων IoT"
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="09/26/2016"
  ms.author="araguila"/>
   
# <a name="frequently-asked-questions-for-iot-suite"></a>Συνήθεις ερωτήσεις για την οικογένεια προγραμμάτων IoT

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Ποια είναι η διαφορά μεταξύ η διαγραφή μιας ομάδας πόρων στην πύλη του Azure και κάνοντας κλικ στην επιλογή Διαγραφή σε μια προκαθορισμένη λύση στο azureiotsuite.com;

- Εάν διαγράψετε το προδιαμορφωμένο λύσης στο [azureiotsuite.com][lnk-azureiotsuite], μπορείτε να διαγράψετε όλους τους πόρους που έχουν παρασχεθεί κατά τη δημιουργία της λύσης προδιαμορφωμένο; Αν έχετε προσθέσει επιπλέον πόρους στην ομάδα πόρων, αυτά διαγράφονται επίσης. 

- Εάν διαγράψετε την ομάδα των πόρων στην [πύλη του Azure][lnk-azure-portal], μπορείτε να διαγράψετε μόνο πόρων σε αυτήν την ομάδα πόρων; θα πρέπει επίσης να διαγράψετε την εφαρμογή Azure Active Directory που σχετίζεται με τη λύση προδιαμορφωμένο στην [πύλη Azure κλασική][lnk-classic-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Πόσες παρουσίες IoT διανομέα να μπορούν να προμηθεύσουν σε μια συνδρομή; 

Δέκα. Μπορείτε να δημιουργήσετε μια [Azure υποστηρίζουν δελτίων] [ link-azuresupportticket] για να βελτιώσετε αυτό το όριο, αλλά από προεπιλογή, μπορείτε να μόνο παροχή του δέκα διανομείς IoT ανά συνδρομή, όπως περιγράφεται στο [Azure συνδρομή όρια][link-azuresublimits]. Ως αποτέλεσμα, επειδή κάθε προδιαμορφωμένο λύση διατάξεις διανομέα IoT, να προμήθεια μόνο έως δέκα προκαθορισμένες λύσεις σε μια δεδομένη συνδρομή. 

### <a name="how-many-documentdb-instances-can-i-provision-in-a-subscription"></a>Πόσες παρουσίες DocumentDB να μπορούν να προμηθεύσουν σε μια συνδρομή;

Πενήντα. Μπορείτε να δημιουργήσετε μια [Azure υποστηρίζουν δελτίων] [ link-azuresupportticket] για να βελτιώσετε αυτό το όριο, αλλά από προεπιλογή, που μπορούν να προμηθεύσουν μόνο πενήντα DocumentDB παρουσίες ανά συνδρομή. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Πώς πολλές δωρεάν Bing χάρτες διασυνδέσεις API να μπορούν να προμηθεύσουν σε μια συνδρομή;

Δύο. Μπορείτε να δημιουργήσετε μόνο δύο εσωτερικές συναλλαγές επιπέδου 1 χάρτες Bing για επιχειρηματικά προγράμματα σε μια συνδρομή του Azure. Η απομακρυσμένη λύση παρακολούθησης παρέχεται από προεπιλογή με το πρόγραμμα στο εσωτερικό συναλλαγές επιπέδου 1. Ως αποτέλεσμα, που μπορούν να προμηθεύσουν μόνο έως και δύο απομακρυσμένης παρακολούθησης λύσεις που περιέχονται σε μια συνδρομή χωρίς τροποποιήσεις.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Έχω μια απομακρυσμένη παρακολούθησης ανάπτυξη λύσης με ένα στατικό χάρτη, πώς μπορώ να προσθέσω ένα αλληλεπιδραστικό χάρτη Bing; 
1. Λήψη του API χάρτες Bing για μεγάλες επιχειρήσεις QueryKey από [Azure πύλη][lnk-azure-portal]: 
 1. Μεταβείτε στην ομάδα πόρων όπου το API χάρτες Bing για μεγάλες επιχειρήσεις είναι στην [πύλη του Azure][lnk-azure-portal].
 2. Κάντε κλικ στην επιλογή όλες τις ρυθμίσεις, διαχείριση, στη συνέχεια, κλειδιών. 
 3. Θα παρατηρήσετε δύο κλειδιά: κλειδιού και QueryKey. Αντιγράψτε την τιμή για το QueryKey.

     > [AZURE.NOTE] Δεν έχετε ένα API χάρτες Bing για το λογαριασμό για μεγάλες επιχειρήσεις; Δημιουργήστε μία στην [πύλη του Azure] [ lnk-azure-portal] με κάνοντας κλικ στην επιλογή + νέα, αναζήτηση για το API χάρτες Bing για μεγάλες επιχειρήσεις και ακολουθήστε τις οδηγίες για να δημιουργήσετε.

2. Αναπτυσσόμενο την πιο πρόσφατη κώδικα από την [Azure IoT-Remote-παρακολούθησης][lnk-remote-monitoring-github].

3. Εκτελέστε μια τοπική ή ανάπτυξη ακολουθώντας τις οδηγίες ανάπτυξης εντολών στο φάκελο /docs/ στο χώρο αποθήκευσης στο cloud. 

4. Αφού έχετε εκτελέσει σε τοπική ή στο cloud ανάπτυξη, αναζητήστε του ριζικού φακέλου για το *. user.config αρχείο που δημιουργήσατε κατά την ανάπτυξη. Ανοίξτε το αρχείο από ένα πρόγραμμα επεξεργασίας κειμένου. 

5. Αλλάξτε την ακόλουθη γραμμή για να συμπεριλάβετε την τιμή που αντιγράψατε από το QueryKey: 
   
  `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Μπορώ να δημιουργήσω μια προκαθορισμένη λύση εάν έχω Microsoft Azure για DreamSpark;
Προς το παρόν, δεν μπορείτε να δημιουργήσετε μια λύση προδιαμορφωμένο με ένα [Microsoft Azure για DreamSpark] [ lnk-dreamspark] λογαριασμού. Ωστόσο, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό της δοκιμαστικής έκδοσης για Azure] [ lnk-30daytrial] απλώς δύο των λεπτών που θα σας επιτρέψει μπορείτε να δημιουργήσετε μια λύση προδιαμορφωμένο.

### <a name="how-do-i-delete-an-aad-tenant"></a>Πώς μπορώ να διαγράψω ένα μισθωτή AAD;

Δείτε τον Golpe του ιστολογίου [αναλυτικές οδηγίες για τη διαγραφή ένα μισθωτή του Azure AD][lnk-delete-aad-tennant].

## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε, επίσης, να εξερευνήσετε ορισμένες από τις άλλες δυνατότητες και τις δυνατότητες των λύσεων οικογένεια IoT προ-διαμορφωμένες:

- [Επισκόπηση της λύσης πρόβλεψης συντήρησης προ-διαμορφωμένες][lnk-predictive-overview]
- [Ασφάλεια IoT από το μηδέν προς τα επάνω][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
