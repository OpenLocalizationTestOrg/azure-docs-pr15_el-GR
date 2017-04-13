<properties 
   pageTitle="Ρύθμιση παραμέτρων σύνδεσης VPN μεταξύ δύο δικτύων εικονικού | Microsoft Azure" 
   description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους συνδέσεις VPN και η επίλυση ονομάτων τομέα μεταξύ δύο Azure εικονικού δικτύων, και πώς μπορείτε να ρυθμίσετε τις παραμέτρους HBase παν-αναπαραγωγής." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-a-vpn-connection-between-two-azure-virtual-networks"></a>Ρύθμιση παραμέτρων σύνδεσης VPN μεταξύ δύο Azure virtual δικτύων  

> [AZURE.SELECTOR]
- [Ρύθμιση παραμέτρων συνδεσιμότητας VPN](hdinsight-hbase-geo-replication-configure-vnets.md)
- [Ρύθμιση παραμέτρων του DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [Ρύθμιση παραμέτρων HBase αναπαραγωγής](hdinsight-hbase-geo-replication.md) 

Συνδεσιμότητα τοποθεσίας σε τοποθεσία του Azure εικονικού δικτύου χρησιμοποιεί μια πύλη VPN για να παρέχουν μια ασφαλής διοχέτευση χρησιμοποιώντας ασφαλείας IP/IKE. Το VNets μπορεί να είναι στις διάφορες συνδρομές και διαφορετικές περιοχές. Μπορείτε να συνδυάσετε ακόμα VNet σε VNet επικοινωνία με ρυθμίσεις παραμέτρων πολλών τοποθεσιών. Υπάρχουν πολλοί λόγοι για VNet να VNet συνδεσιμότητας:

- Περιοχή διασταύρωσης παν-πλεονασμού και παν παρουσίας 
- Τοπικές εφαρμογές πολλαπλών επιπέδων με ισχυρό απομόνωσης όριο 
- Cross συνδρομή, επικοινωνίας μεταξύ εταιρείας στο Azure

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων ενός VNet VNet σύνδεση](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). 

Για να δείτε το βίντεο:

> [AZURE.VIDEO configure-the-vpn-connectivity-between-two-azure-virtual-networks]

Αυτό το πρόγραμμα εκμάθησης είναι μέρος [σειράς] [ hdinsight-hbase-replication] σχετικά με τη δημιουργία HBase παν-αναπαραγωγής. 

- Ρύθμιση παραμέτρων μια σύνδεση VPN μεταξύ δύο εικονικού δικτύων (αυτό το πρόγραμμα εκμάθησης)
- [Ρύθμιση παραμέτρων DNS για το εικονικό δίκτυα][hdinsight-hbase-geo-replication-dns]
- [Ρύθμιση παραμέτρων HBase παν αναπαραγωγής][hdinsight-hbase-geo-replication]

Το παρακάτω διάγραμμα παρουσιάζει τα δύο εικονικού δίκτυα που θα δημιουργήσετε με αυτό το πρόγραμμα εκμάθησης:

![Διάγραμμα δικτύου εικονικού αναπαραγωγής HDInsight HBase][img-vnet-diagram]
 

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Μια σταθμούς εργασίας με το Azure PowerShell**.

    Πριν από την εκτέλεση δεσμών ενεργειών του PowerShell, βεβαιωθείτε ότι είστε συνδεδεμένοι στη συνδρομή σας Azure χρησιμοποιώντας το ακόλουθο cmdlet:

        Add-AzureAccount

    Εάν έχετε πολλές συνδρομές Azure, χρησιμοποιήστε το ακόλουθο cmdlet για να ορίσετε την τρέχουσα συνδρομή σας:

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


>[AZURE.NOTE] Τα ονόματα υπηρεσιών Azure και εικονική μηχανή πρέπει να είναι μοναδικό. Το όνομα που χρησιμοποιείται σε αυτό το πρόγραμμα εκμάθησης είναι Contoso-[όνομα Azure Service/Εικονική]-[ΕΕ / η.π.α.]. Για παράδειγμα, Contoso-VNet-ΕΕ είναι το Azure εικονικού δικτύου στο κέντρο δεδομένων Βόρειας Ευρώπης; Contoso-DNS-ΗΠΑ είναι ο διακομιστής DNS Εικονική στο κέντρο δεδομένων η.π.α. Ανατολικής. Πρέπει να καταλήξετε με τα δικά σας ονόματα.
 

##<a name="create-two-azure-vnets"></a>Δημιουργήστε δύο VNets Azure



**Για να δημιουργήσετε ένα εικονικό δίκτυο που ονομάζεται Contoso-VNet-ΕΕ στην Ευρώπη Βόρεια**

1.  Είσοδος στην [πύλη του Azure κλασική][azure-portal].
2.  Κάντε κλικ στην επιλογή **ΔΗΜΙΟΥΡΓΊΑ**, **ΥΠΗΡΕΣΊΕΣ ΔΙΚΤΎΟΥ**, **ΕΙΚΟΝΙΚΉ ΔΙΚΤΎΟΥ**, **ΔΗΜΙΟΥΡΓΊΑ ΠΡΟΣΑΡΜΟΣΜΈΝΟΥ**.
3.  Εισαγάγετε:

    - **ΌΝΟΜΑ**: Contoso-VNet-ΕΕ
    - **ΘΈΣΗ**: Βόρεια Ευρώπη

        Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί Βόρειας Ευρώπη και Ανατολικής ΗΠΑ κέντρα δεδομένων. Μπορείτε να επιλέξετε το δικό σας κέντρα δεδομένων.
4.  Εισαγάγετε:

    - **ΔΙΑΚΟΜΙΣΤΉ DNS**: (αφήστε την κενή) 
    
        Θα χρειαστεί το δικό σας διακομιστή DNS για την επίλυση ονομάτων μέσα σε εικονικό δίκτυα. Για περισσότερες πληροφορίες σχετικά με πότε στην ανάλυση που παρέχονται από Azure όνομα χρήση και πότε πρέπει να χρησιμοποιείτε το δικό σας διακομιστή DNS, ανατρέξτε στο θέμα [Επίλυση ονομάτων (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). Για οδηγίες σχετικά με τη ρύθμιση παραμέτρων επίλυση ονομάτων μεταξύ VNets, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων DNS μεταξύ δύο δικτύων Azure εικονικού][hdinsight-hbase-dns].
  
    - **Ρύθμιση παραμέτρων ενός VPN σημείου σε τοποθεσία**: (μη επιλεγμένο)

        Σημείο σε τοποθεσία δεν ισχύει για αυτό το σενάριο.

    - **Ρύθμιση παραμέτρων μιας τοποθεσίας σε τοποθεσία VPN**: (μη επιλεγμένο)
    
        Θα μπορείτε να ρυθμίσετε τη σύνδεση VPN τοποθεσίας σε τοποθεσία με το Azure εικονικού δικτύου στο κέντρο δεδομένων η.π.α. Ανατολικής.
5.  Εισαγάγετε:

    -   **ΔΙΕΎΘΥΝΣΗ IP ΈΝΑΡΞΗΣ ΧΏΡΟΥ**: 10.1.0.0
    -   **ΔΙΕΎΘΥΝΣΗ ΧΏΡΟΥ CIDR**: / 16
    -   **ΈΝΑΡΞΗ υποδικτύου-1 IP**: 10.1.0.0
    -   **CIDR υποδικτύου-1**: / 24

    Το χώρο διευθύνσεων δεν επικαλύπτονται με το εικονικό δίκτυο ΗΠΑ.  

**Για να δημιουργήσετε ένα εικονικό δίκτυο που ονομάζεται Contoso-VNet-ΕΕ στην Ευρώπη Δυτική**

- Επαναλάβετε τη διαδικασία τελευταία με τις εξής τιμές:

    - **ΌΝΟΜΑ**: Contoso-VNet-η.π.α.
    - **ΘΈΣΗ**: Ανατολικής η.π.α.
     
    - **ΔΙΑΚΟΜΙΣΤΉ DNS**: (αφήστε την κενή)
    - **Ρύθμιση παραμέτρων ενός VPN σημείου σε τοποθεσία**: (μη επιλεγμένο)
    - **Ρύθμιση παραμέτρων μιας τοποθεσίας σε τοποθεσία VPN**: (μη επιλεγμένο)
     
    - **ΔΙΕΎΘΥΝΣΗ IP ΈΝΑΡΞΗΣ ΧΏΡΟΥ**: 10.2.0.0
    - **ΔΙΕΎΘΥΝΣΗ ΧΏΡΟΥ CIDR**: / 16
    - **ΈΝΑΡΞΗ υποδικτύου-1 IP**: 10.2.0.0
    - **CIDR υποδικτύου-1**: / 24

















##<a name="configure-a-vpn-connection-between-the-two-vnets"></a>Ρύθμιση παραμέτρων σύνδεσης VPN μεταξύ του δύο VNets

###<a name="create-local-networks"></a>Δημιουργία τοπικού δίκτυα

Όταν δημιουργείτε μια VNet VNet ρύθμιση παραμέτρων, πρέπει να ρυθμίσετε τις παραμέτρους κάθε VNet για τον προσδιορισμό των υπόλοιπων ως τοποθεσία τοπικού δικτύου. Σε αυτήν την ενότητα, θα ρυθμίσετε τις παραμέτρους κάθε VNet ως ένα τοπικό δίκτυο. Τα τοπικά δίκτυα κοινή χρήση τα ίδια διαστήματα διευθύνσεων IP με το αντίστοιχο VNet.

![Ρύθμιση παραμέτρων Azure VPN τοποθεσίας σε τοποθεσία ρύθμιση παραμέτρων - azure τοπικά δίκτυα][img-vnet-lnet-diagram]


**Για να δημιουργήσετε ένα τοπικό δίκτυο που ονομάζεται Contoso-LNet-ΕΕ που ταιριάζουν με το χώρο διευθύνσεων δικτύου Contoso-VNet-ΕΕ**

1. Από την πύλη κλασική Azure, κάντε κλικ στην επιλογή **ΔΗΜΙΟΥΡΓΊΑ**, **ΥΠΗΡΕΣΊΕΣ ΔΙΚΤΎΟΥ**, **ΕΙΚΟΝΙΚΟΎ ΔΙΚΤΎΟΥ**, **Προσθήκη ΤΟΠΙΚΟΎ ΔΙΚΤΎΟΥ**.
3. Εισαγάγετε:

    - **ΌΝΟΜΑ**: Contoso-LNet-ΕΕ
    - **ΔΙΕΎΘΥΝΣΗ IP του VPN ΣΥΣΚΕΥΉΣ**: 192.168.0.1 (αυτή η διεύθυνση θα ενημερωθεί αργότερα)

        Συνήθως, θα χρησιμοποιήσετε την πραγματική εξωτερική διεύθυνση IP για μια συσκευή VPN. Για VNet να VNet ρυθμίσεις παραμέτρων, θα χρησιμοποιήσετε τη διεύθυνση IP της πύλης VPN. Δεδομένου ότι δεν έχετε δημιουργήσει πυλών VPN για τα δύο VNets ακόμη, καταχωρήστε μια διεύθυνση IP arbitary και επιστρέψει για να διορθώσετε το πρόβλημα.
4.  Εισαγάγετε:

    - **ΔΙΕΎΘΥΝΣΗ IP ΈΝΑΡΞΗΣ ΧΏΡΟΥ:** 10.1.0.0
    - **CIDR ΧΏΡΟΥ ΔΙΕΥΘΎΝΣΕΩΝ:** /16
    
    Αυτό πρέπει να αντιστοιχεί ακριβώς την περιοχή που καθορίσατε προηγουμένως για Contoso-VNet-ΕΕ.

**Για να δημιουργήσετε ένα τοπικό δίκτυο που ονομάζεται Contoso-LNet-ΗΠΑ που ταιριάζουν με το χώρο διευθύνσεων δικτύου Contoso-VNet-η.π.α.**

- Επαναλάβετε τη διαδικασία τελευταία με τις ακόλουθες παραμέτρους:

    - **ΌΝΟΜΑ**: Contoso-LNet-η.π.α.
    - **ΔΙΕΎΘΥΝΣΗ IP του VPN ΣΥΣΚΕΥΉΣ**: 192.168.0.1 (αυτή η διεύθυνση θα ενημερωθεί αργότερα)
     
    - **ΔΙΕΎΘΥΝΣΗ IP ΈΝΑΡΞΗΣ ΧΏΡΟΥ**: 10.2.0.0
    - **ΔΙΕΎΘΥΝΣΗ ΧΏΡΟΥ CIDR**: / 16


###<a name="create-vpn-gateways"></a>Δημιουργία πύλες VPN

Υπάρχουν δύο μέρη σε αυτήν τη ρύθμιση. Πρώτα να ρυθμίσετε μια σύνδεση τοποθεσίας σε τοποθεσία VNet σε τοπικό δίκτυο και, στη συνέχεια, μπορείτε να δημιουργήσετε μια δυναμική δρομολόγηση VPN. VNet να VNet απαιτεί Azure VPN πύλες με δυναμική δρομολόγηση VPN. Azure στατική δρομολόγηση VPN δεν υποστηρίζονται.

**Για να ρυθμίσετε τις παραμέτρους της σύνδεσης τοποθεσίας σε τοποθεσία Contoso-VNet-ΕΕ Contoso-LNet-US**

1.  Από την πύλη κλασική Azure, κάντε κλικ στην επιλογή **ΔΊΚΤΥΑ** στο αριστερό τμήμα του παραθύρου,
2.  Κάντε κλικ στην επιλογή **Contoso-VNet-ΕΕ**.
3.  Κάντε κλικ στην καρτέλα **CONFIGUE** .
4.  Επιλέξτε **σύνδεση σε τοπικό δίκτυο**.
5.  Σε **ΤΟΠΙΚΌ ΔΊΚΤΥΟ**, επιλέξτε **Contoso-LNet-ΗΠΑ**.
6.  Κάντε κλικ στην επιλογή **Προσθήκη πύλης υποδικτύου** στην ενότητα εικονικού δικτύου διεύθυνση κενά διαστήματα.
7.  Κάντε κλικ στην επιλογή **ΑΠΟΘΉΚΕΥΣΗ**.
8.  Κάντε κλικ στο **κουμπί OK** για να επιβεβαιώσετε.


**Για να δημιουργήσετε μια πύλη VPN για Contoso-VNet-ΕΕ**

1.  Από την πύλη κλασική Azure, κάντε κλικ στην καρτέλα του **πίνακα ΕΡΓΑΛΕΊΩΝ** .
4.  Κάντε κλικ στην επιλογή **ΔΗΜΙΟΥΡΓΊΑ ΠΎΛΗΣ** στο κάτω μέρος της σελίδας και, στη συνέχεια, κάντε κλικ στην επιλογή **Δυναμική δρομολόγηση**.
5.  Κάντε κλικ στο κουμπί **Ναι** για επιβεβαίωση. Παρατηρήστε το γραφικό πύλης στη σελίδα αλλάζει σε κίτρινο και εμφανίζεται η ένδειξη τη δημιουργία πύλης. Συνήθως, διαρκεί περίπου 15 λεπτά για την πύλη για να δημιουργήσετε.

    Όταν η κατάσταση πύλης μετατραπεί σε σύνδεση, η διεύθυνση IP για κάθε πύλη θα είναι ορατές στον πίνακα εργαλείων. Σημειώστε τη διεύθυνση IP που αντιστοιχεί σε κάθε VNet, φροντίζοντας να μην τα συνδυάσετε προς τα επάνω. Αυτές είναι οι διευθύνσεις IP που θα χρησιμοποιηθεί κατά την επεξεργασία διευθύνσεις IP σας κράτησης θέσης για τη συσκευή VPN σε τοπική δίκτυα.

6.  Δημιουργήστε ένα αντίγραφο της **ΔΙΕΎΘΥΝΣΗΣ IP ΠΎΛΗΣ**. Θα χρησιμοποιείτε για να ρυθμίσετε τις παραμέτρους της διεύθυνσης IP πύλης VPN για Contoso-VNet-ΕΕ στην επόμενη ενότητα.

**Για να δημιουργήσετε μια πύλη VPN για Contoso-VNet-ΕΕ**

- Επαναλάβετε τα δύο τελευταία διαδικασία για να ρυθμίσετε τη συνδεσιμότητα-τοποθεσίας Contoso-VNet-ΗΠΑ να Contoso-LNet-ΕΕ και τη δημιουργία μιας πύλης VPN για Contoso-Vnet-η.π.α.. Όταν ολοκληρώσετε, θα έχετε τη διεύθυνση IP της πύλης VPN για Contoso-VNet-ΗΠΑ.


### <a name="set-the-vpn-device-ip-addresses-for-local-networks"></a>Ρυθμίστε τη συσκευή VPN διευθύνσεις IP για το τοπικό δίκτυα
Στην τελευταία ενότητα, μπορείτε να δημιουργήσετε μια πύλη VPN για κάθε ένα από τα VNets. Που έχετε στη διάθεσή σας τις διευθύνσεις IP των πυλών VPN. Τώρα μπορείτε να μεταβείτε ξανά για να ρυθμίσετε τις παραμέτρους διευθύνσεις IP του τοπικού δικτύου VPN συσκευή.

**Για να ρυθμίσετε τις παραμέτρους της διεύθυνσης IP του VPN τη συσκευή για Contoso-LNet-ΕΕ** 

1.  Από την πύλη κλασική Azure, κάντε κλικ στην επιλογή **ΔΊΚΤΥΑ** στο αριστερό τμήμα του παραθύρου.
2.  Κάντε κλικ στην επιλογή **ΤΟΠΙΚΆ ΔΊΚΤΥΑ** από την αρχή.
3.  Κάντε κλικ στην επιλογή **Contoso-LNet-ΕΕ**και, στη συνέχεια, κάντε κλικ στην επιλογή **ΕΠΕΞΕΡΓΑΣΊΑ** κάτω.
4.  Ενημερώστε **τη ΔΙΕΎΘΥΝΣΗ IP ΣΥΣΚΕΥΉ VPN**.  Αυτή είναι η διεύθυνση που λαμβάνετε από την καρτέλα πίνακα ΕΡΓΑΛΕΊΩΝ της Contoso-VNET-ΕΕ.
5.  Κάντε κλικ στο δεξιό κουμπί.
6.  Κάντε κλικ στο κουμπί ελέγχου.

**Για να ρυθμίσετε τις παραμέτρους της διεύθυνσης IP του VPN τη συσκευή για Contoso-LNet-η.π.α.** 

- Επαναλάβετε την τελευταία διαδικασία για να ρυθμίσετε τις παραμέτρους της διεύθυνσης IP του VPN τη συσκευή για Contoso-LNet-η.π.α..

###<a name="set-vnet-gateway-keys"></a>Ορισμός VNet πλήκτρα πύλης

Οι πύλες Vnet Χρησιμοποιήστε ένα κοινόχρηστο κλειδί για τον έλεγχο ταυτότητας συνδέσεις μεταξύ των εικονικού δικτύων. Το κλειδί δεν μπορεί να ρυθμιστεί από την πύλη κλασική Azure. Πρέπει να χρησιμοποιήσετε PowerShell ή .NET SDK.

**Για να ορίσετε τα πλήκτρα**

1. Από σταθμούς εργασίας σας, ανοίξτε το **Windows PowerShell ISE** ή την κονσόλα του Windows PowerShell.
2. Ενημερώστε τις παραμέτρους σε αυτήν τη δέσμη ενεργειών παρακολούθηση και εκτελέστε το:

        Add-AuzreAccount
        Select-AzureSubscription -[AzureSubscriptionName]
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-EU -LocalNetworkSiteName Contoso-LNet-US -SharedKey A1b2C3D4
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-US -LocalNetworkSiteName Contoso-LNet-EU -SharedKey A1b2C3D4 


##<a name="check-the-vpn-connection"></a>Ελέγξτε τη σύνδεση VPN 

Χωρίς οποιαδήποτε ΣΠΣ αναπτυχθεί σε το VNets, μπορείτε να χρησιμοποιήσετε το διάγραμμα οπτική εικονικού δικτύου σελίδας πίνακα εργαλείων VNet στην πύλη κλασική Azure για να ελέγξετε την κατάσταση σύνδεσης:

![HDInsight HBase αναπαραγωγής εικονικού δικτύου κατάσταση σύνδεσης VPN][img-vpn-status]
  



##<a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης μάθατε πώς να ρυθμίσετε μια σύνδεση VPN μεταξύ δύο Azure εικονικού δικτύων. Καλύπτει τα δύο άρθρα στη σειρά:

- [Ρύθμιση παραμέτρων DNS μεταξύ δύο Azure εικονικού δικτύων][hdinsight-hbase-geo-replication-dns]
- [Ρύθμιση παραμέτρων HBase παν αναπαραγωγής][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com

[powershell-install]: ../install-configure-powershell.md



[hdinsight-hbase-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-dns]: hdinsight-hbase-geo-replication-configure-dns.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-diagram.png
[img-vnet-lnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-lnet-diagram.png
[img-vpn-status]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-status.png 