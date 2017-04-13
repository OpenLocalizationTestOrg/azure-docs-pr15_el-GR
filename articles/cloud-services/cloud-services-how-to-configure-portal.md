<properties 
    pageTitle="Πώς μπορείτε να ρυθμίσετε μια υπηρεσία cloud (πύλη) | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις υπηρεσίες cloud στο Azure. Μάθετε πώς μπορείτε να ενημερώσετε τις παραμέτρους της υπηρεσίας cloud και ρύθμιση παραμέτρων απομακρυσμένης πρόσβασης σε παρουσίες των ρόλων. Αυτά τα παραδείγματα χρησιμοποιούν την πύλη του Azure." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="how-to-configure-cloud-services"></a>Πώς μπορείτε να ρυθμίσετε τις υπηρεσίες Cloud

> [AZURE.SELECTOR]
- [Πύλη του Azure](cloud-services-how-to-configure-portal.md)
- [Azure κλασική πύλη](cloud-services-how-to-configure.md)

Μπορείτε να ρυθμίσετε τις πιο συχνά χρησιμοποιούμενες ρυθμίσεις για μια υπηρεσία cloud στην πύλη του Azure. Εναλλακτικά, εάν θέλετε να ενημερώσετε το αρχείο ρύθμισης παραμέτρων απευθείας, κάντε λήψη ενός αρχείου ρύθμισης παραμέτρων υπηρεσίας για να ενημερώσετε, και, στη συνέχεια, αποστείλετε το ενημερωμένο αρχείο και να ενημερώσετε την υπηρεσία cloud με τις αλλαγές ρύθμισης παραμέτρων. Σε κάθε περίπτωση, τις ενημερώσεις ρύθμισης παραμέτρων προωθούνται όλες τις εμφανίσεις του ρόλου.

Μπορείτε επίσης να διαχειριστείτε τις παρουσίες ρόλους υπηρεσία cloud ή απομακρυσμένης επιφάνειας εργασίας του σε αυτά.

Azure μόνο να εξασφαλίσετε διαθεσιμότητα υπηρεσίας τοις εκατό 99.95 κατά τη διάρκεια των ενημερώσεων ρύθμισης παραμέτρων Εάν έχετε τουλάχιστον δύο παρουσίες ρόλο για κάθε ρόλο. Που ενεργοποιεί μία εικονική μηχανή για επεξεργασία αιτήσεων προγράμματος-πελάτη, ενώ το άλλο ενημερώνεται. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τις συμβάσεις επιπέδου υπηρεσιών](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Αλλάξτε μια υπηρεσία στο cloud

Αφού ανοίξετε την [πύλη του Azure](https://portal.azure.com/), μεταβείτε στην υπηρεσία cloud. Από εδώ μπορείτε να διαχειριστείτε πολλές πτυχές της. 

![Σελίδα "Ρυθμίσεις"](./media/cloud-services-how-to-configure-portal/cloud-service.png)

Οι συνδέσεις **Ρυθμίσεις** ή **όλες τις ρυθμίσεις** θα ανοίξει το blade **Ρυθμίσεις** όπου μπορείτε να αλλάξετε τις **Ιδιότητες**, αλλάξτε τις **ρυθμίσεις παραμέτρων**, τα **πιστοποιητικά**, το πρόγραμμα εγκατάστασης **ειδοποίησης κανόνες**, διαχείριση και διαχειριστείτε τους **χρήστες** που έχουν πρόσβαση σε αυτήν την υπηρεσία cloud προς τα επάνω.

![Blade ρυθμίσεις υπηρεσίας Azure cloud](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

>[AZURE.NOTE]
>Το λειτουργικό σύστημα για την υπηρεσία cloud δεν μπορεί να αλλάξει με την **πύλη του Azure**, μπορείτε να αλλάξετε μόνο αυτήν τη ρύθμιση, μέσω της [Azure κλασική πύλη](http://manage.windowsazure.com/). Αυτό είναι λεπτομερές [εδώ](cloud-services-how-to-configure.md#update-a-cloud-service-configuration-file).

## <a name="monitoring"></a>Παρακολούθηση

Μπορείτε να προσθέσετε ειδοποιήσεις για την υπηρεσία cloud. Κάντε κλικ στην επιλογή **Ρυθμίσεις** > **Κανόνες ειδοποίηση** > **Προσθήκη ειδοποίησης**. 

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Από εδώ, μπορείτε να ρυθμίσετε μια ειδοποίηση. Με το **Mertic** αναπτυσσόμενο πλαίσιο, μπορείτε να ρυθμίσετε μια ειδοποίηση για τους ακόλουθους τύπους δεδομένων.

- Ανάγνωση δίσκου
- Εγγραφή δίσκου
- Δικτύου στο
- Δίκτυο ανάληψη
- Ποσοστό της CPU 

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Ρύθμιση παραμέτρων παρακολούθησης από το ένα μετρικό πλακίδιο

Αντί να χρησιμοποιήσετε τις **Ρυθμίσεις** > **Ειδοποίησης κανόνες**, μπορείτε να κάνετε κλικ σε ένα από τα πλακίδια μετρικό στην ενότητα **παρακολούθησης** της blade την **υπηρεσία στο Cloud** .

![Υπηρεσία cloud παρακολούθησης](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Από εδώ μπορείτε να προσαρμόσετε το γράφημα που χρησιμοποιείται με το πλακίδιο ή Προσθήκη κανόνα ειδοποίησης.


## <a name="reboot-reimage-or-remote-desktop"></a>Επανεκκινήστε τον υπολογιστή, reimage ή απομακρυσμένης επιφάνειας εργασίας

Προς το παρόν, δεν μπορείτε να ρυθμίσετε απομακρυσμένης επιφάνειας εργασίας με την **πύλη του Azure**. Ωστόσο, μπορείτε να ρυθμίσετε το μέσω του [Azure κλασική πύλη](cloud-services-role-enable-remote-desktop.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), ή μέσω του [Visual Studio](../vs-azure-tools-remote-desktop-roles.md). 

Πρώτα, κάντε κλικ στην παρουσία υπηρεσία cloud.

![Η παρουσία της υπηρεσίας cloud](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Από το blade που ανοίγει να ξεκινήσετε μια σύνδεση απομακρυσμένης επιφάνειας εργασίας, από απόσταση επανεκκινήστε την παρουσία ή απομακρυσμένα reimage uou (ξεκινά με μια εικόνα Φρέσκο) την παρουσία.

![Κουμπιά παρουσία υπηρεσίας cloud](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)



## <a name="reconfigure-your-cscfg"></a>Ρυθμίστε ξανά τις παραμέτρους του .cscfg

Ίσως χρειαστεί να ρυθμίσει ξανά τις παραμέτρους μπορείτε υπηρεσία στο cloud μέσω του αρχείου [ρύθμισης παραμέτρων υπηρεσίας (cscfg)](cloud-services-model-and-package.md#cscfg) . Πρέπει πρώτα να κάνετε λήψη του αρχείου .cscfg, τροποποιήστε το και, στη συνέχεια, στείλτε το.

1. Επιλέξτε το εικονίδιο **Ρυθμίσεις** ή τη σύνδεση **όλες τις ρυθμίσεις** για να ανοίξετε το blade **Ρυθμίσεις** .

    ![Σελίδα "Ρυθμίσεις"](./media/cloud-services-how-to-configure-portal/cloud-service.png)

2. Κάντε κλικ στο στοιχείο **ρύθμισης παραμέτρων** .

    ![Ρύθμιση παραμέτρων Blade](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)

3. Κάντε κλικ στο κουμπί **λήψη** .

    ![Λήψη](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)

4. Αφού ενημερώσετε το αρχείο ρύθμισης παραμέτρων υπηρεσίας, αποστολή και εφαρμόστε τις ενημερώσεις ρύθμισης παραμέτρων:

    ![Αποστολή](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png) 
    
5. Επιλέξτε το αρχείο .cscfg και κάντε κλικ στο **κουμπί OK**.

            
## <a name="next-steps"></a>Επόμενα βήματα

* Μάθετε πώς να [αναπτύξετε μια υπηρεσία στο cloud](cloud-services-how-to-create-deploy-portal.md).
* Ρύθμιση παραμέτρων ένα [προσαρμοσμένο όνομα τομέα](cloud-services-custom-domain-name-portal.md).
* [Διαχείριση της υπηρεσίας cloud](cloud-services-how-to-manage-portal.md).
* Ρύθμιση παραμέτρων [πιστοποιητικά ssl](cloud-services-configure-ssl-certificate-portal.md).