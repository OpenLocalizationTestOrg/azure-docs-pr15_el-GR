<properties 
    pageTitle="Ενεργοποίηση της απομακρυσμένης επιφάνειας εργασίας για τις υπηρεσίες cloud (Node.js)" 
    description="Μάθετε πώς μπορείτε να ενεργοποιήσετε την πρόσβαση απομακρυσμένης επιφάνειας εργασίας για τις εικονικές μηχανές που φιλοξενεί την εφαρμογή του Azure Node.js." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="enabling-remote-desktop-in-azure"></a>Ενεργοποίηση της απομακρυσμένης επιφάνειας εργασίας στο Azure

Σύνδεση απομακρυσμένης επιφάνειας εργασίας σάς επιτρέπει να αποκτήσετε πρόσβαση στην επιφάνεια εργασίας από μια παρουσία ρόλων εκτελείται στο Azure. Μπορείτε να χρησιμοποιήσετε μια σύνδεση απομακρυσμένης επιφάνειας εργασίας για να ρυθμίσετε την εικονική μηχανή ή αντιμετώπιση προβλημάτων με την εφαρμογή σας.

> [AZURE.NOTE] Σε αυτό το άρθρο ισχύει για εφαρμογές Node.js φιλοξενούνται ως υπηρεσία Cloud Azure.


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- Εγκατάσταση και ρύθμιση παραμέτρων [Azure Powershell](../powershell-install-configure.md).
- Ανάπτυξη εφαρμογής Node.js σε μια υπηρεσία Azure Cloud. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία και ανάπτυξη μιας εφαρμογής Node.js σε μια υπηρεσία Cloud Azure](cloud-services-nodejs-develop-deploy-app.md).


## <a name="step-1-use-azure-powershell-to-configure-the-service-for-remote-desktop-access"></a>Βήμα 1: Χρήση του PowerShell Azure για ρύθμιση παραμέτρων της υπηρεσίας για πρόσβαση απομακρυσμένης επιφάνειας εργασίας

Για να χρησιμοποιήσετε απομακρυσμένης επιφάνειας εργασίας, πρέπει να ενημερώσετε τον ορισμό Azure υπηρεσίας και ρύθμισης παραμέτρων με ένα όνομα χρήστη, τον κωδικό πρόσβασης και το πιστοποιητικό. 

Ακολουθήστε τα παρακάτω βήματα από έναν υπολογιστή που περιέχει τα αρχεία προέλευσης για την εφαρμογή σας.

1. Εκτέλεση **του Windows PowerShell** ως διαχειριστής. (Από το **Μενού Έναρξη** ή η **Οθόνη έναρξης**, αναζήτησης για το **Windows PowerShell**.)

2.  Μεταβείτε στον κατάλογο που περιέχει τον ορισμό υπηρεσίας (.csdef) και αρχεία ρύθμισης παραμέτρων (.cscfg) υπηρεσίας.

3. Εισαγάγετε το παρακάτω cmdlet του PowerShell:

        Enable-AzureServiceProjectRemoteDesktop

4. Στη γραμμή εντολών, πληκτρολογήστε ένα όνομα χρήστη και τον κωδικό πρόσβασης.

    ![Ενεργοποίηση azureserviceprojectremotedesktop][enable-rdp]

3.  Εισαγάγετε το παρακάτω cmdlet του PowerShell για να δημοσιεύσετε τις αλλαγές:

        Publish-AzureServiceProject

    ![δημοσίευση azureserviceproject][publish-project]

## <a name="step-2-connect-to-the-role-instance"></a>Βήμα 2: Σύνδεση με την παρουσία του ρόλου

Μετά τη δημοσίευση τον ορισμό της υπηρεσίας ενημέρωσης, μπορείτε να συνδεθείτε στην παρουσία ρόλο.

1.  Στην [πύλη του Azure κλασική], επιλέξτε **Τις υπηρεσίες Cloud** και, στη συνέχεια, επιλέξτε την υπηρεσία.

    ![Azure κλασική πύλη][cloud-services]

2.  Κάντε κλικ στην επιλογή **παρουσίες**και, στη συνέχεια, κάντε κλικ στην επιλογή **παραγωγής** ή **ενδιάμεσου σταδίου** για να δείτε τις παρουσίες της υπηρεσίας. Επιλέξτε μια παρουσία και, στη συνέχεια, κάντε κλικ στην επιλογή **σύνδεση** στο κάτω μέρος της σελίδας.

    ![Στη σελίδα παρουσίες][3]

2.  Όταν κάνετε κλικ στο κουμπί " **σύνδεση**", το πρόγραμμα περιήγησης web σάς ζητά να αποθηκεύσετε ένα αρχείο .rdp. Άνοιγμα αυτού του αρχείου. (Για παράδειγμα, εάν χρησιμοποιείτε τον Internet Explorer, κάντε κλικ στην επιλογή **Άνοιγμα**.)

    ![να γίνεται ερώτηση για να ανοίξετε ή να αποθηκεύσετε το αρχείο .rdp][4]

3.  Όταν ανοίξει το αρχείο, εμφανίζεται το ακόλουθο μήνυμα ασφαλείας:

    ![Ερώτημα ασφαλείας των Windows][5]

4.  Κάντε κλικ στην επιλογή **σύνδεση**, και θα εμφανιστεί ένα μήνυμα ασφαλείας για την εισαγωγή διαπιστευτηρίων για την πρόσβαση της παρουσίας. Πληκτρολογήστε τον κωδικό πρόσβασης που δημιουργήσατε στο [βήμα 1] [βήμα 1: ρύθμιση παραμέτρων της υπηρεσίας για πρόσβαση απομακρυσμένης επιφάνειας εργασίας με χρήση του PowerShell Azure], και, στη συνέχεια, κάντε κλικ στο **κουμπί OK**.

    ![όνομα χρήστη/κωδικού πρόσβασης][6]

Όταν γίνεται η σύνδεση, σύνδεση απομακρυσμένης επιφάνειας εργασίας εμφανίζει την επιφάνεια εργασίας της παρουσίας στο Azure. 

![Απομακρυσμένη περίοδο λειτουργίας επιφάνειας εργασίας][7]

## <a name="step-3-configure-the-service-to-disable-remote-desktop-access"></a>Βήμα 3: Ρύθμιση παραμέτρων της υπηρεσίας για να απενεργοποιήσετε την πρόσβαση απομακρυσμένης επιφάνειας εργασίας 

Όταν δεν χρειάζεστε πλέον συνδέσεις απομακρυσμένης επιφάνειας εργασίας για να τις παρουσίες ρόλος στο cloud, απενεργοποίηση πρόσβαση απομακρυσμένης επιφάνειας εργασίας με χρήση [Του PowerShell Azure].

1.  Εισαγάγετε το παρακάτω cmdlet του PowerShell:

        Disable-AzureServiceProjectRemoteDesktop

2.  Εισαγάγετε το παρακάτω cmdlet του PowerShell για να δημοσιεύσετε τις αλλαγές:

        Publish-AzureServiceProject

## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Απομακρυσμένη πρόσβαση σε παρουσίες ρόλος στο Azure] 
- [Χρήση απομακρυσμένης επιφάνειας εργασίας με τους ρόλους Azure]
- [Κέντρο για προγραμματιστές του node.js](/develop/nodejs/)

  [Azure PowerShell]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[Azure κλασική πύλη]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
[3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
[4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
[5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
[6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
[7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png
  
[Απομακρυσμένη πρόσβαση σε παρουσίες ρόλος στο Azure]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
[Χρήση απομακρυσμένης επιφάνειας εργασίας με τους ρόλους Azure]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx
 