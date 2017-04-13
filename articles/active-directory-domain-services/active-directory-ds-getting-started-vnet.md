<properties
    pageTitle="Azure υπηρεσίες τομέα AD: Δημιουργία ή επιλογή ένα εικονικό δίκτυο | Microsoft Azure"
    description="Γρήγορα αποτελέσματα με τις υπηρεσίες τομέα Active Directory Azure"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="create-or-select-a-virtual-network-for-azure-ad-domain-services"></a>Δημιουργία ή επιλογή ένα εικονικό δίκτυο για τις υπηρεσίες τομέα AD Azure

## <a name="guidelines-to-select-an-azure-virtual-network"></a>Οδηγίες για να επιλέξετε μια Azure εικονικού δικτύου
> [AZURE.NOTE] **Πριν να ξεκινήσετε**: αναφέρονται σε [δίκτυο ζητήματα σχετικά με τις υπηρεσίες τομέα AD Azure](active-directory-ds-networking.md).


## <a name="task-2-create-an-azure-virtual-network"></a>Εργασία 2: Δημιουργήστε μια Azure εικονικού δικτύου
Η επόμενη εργασία ρύθμισης παραμέτρων είναι να δημιουργήσετε μια Azure εικονικού δικτύου και ένα υποδίκτυο μέσα σε αυτό. Μπορείτε να ενεργοποιήσετε τις υπηρεσίες τομέα AD Azure σε αυτό το υποδίκτυο εικονικό δίκτυο σας. Εάν έχετε ήδη ένα υπάρχον εικονικό δίκτυο που προτιμάτε να χρησιμοποιήσετε, μπορείτε να παραλείψετε αυτό το βήμα.

> [AZURE.NOTE] Βεβαιωθείτε ότι το Azure εικονικό δίκτυο, μπορείτε να δημιουργήσετε ή να επιλέξετε να χρησιμοποιήσετε με τις υπηρεσίες τομέα AD Azure ανήκει σε μια περιοχή Azure που υποστηρίζεται από τις υπηρεσίες τομέα AD Azure. Δείτε τη σελίδα [υπηρεσίες Azure ανά περιοχή](https://azure.microsoft.com/regions/#services/) να γνωρίζετε τις Azure περιοχές στις οποίες Azure υπηρεσίες τομέα AD είναι διαθέσιμη.

Σημείωση προς τα κάτω στο όνομα του εικονικού δικτύου, ώστε να επιλέξετε το προς τα δεξιά εικονικού δικτύου κατά την ενεργοποίηση του υπηρεσίες τομέα AD Azure σε ένα βήμα επόμενες ρύθμισης παραμέτρων.

Ακολουθήστε τα παρακάτω βήματα ρύθμισης παραμέτρων για να δημιουργήσετε μια Azure εικονικού δικτύου στο οποίο θέλετε να ενεργοποιήσετε τις υπηρεσίες τομέα AD Azure.

1. Μεταβείτε στην **πύλη του Azure κλασική** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Επιλέξτε τον κόμβο **δίκτυα** στο αριστερό τμήμα του παραθύρου.

    ![Δίκτυα κόμβου](./media/active-directory-domain-services-getting-started/networks-node.png)

3. Στο παράθυρο εργασιών στο κάτω μέρος της σελίδας, κάντε κλικ στην επιλογή **ΔΗΜΙΟΥΡΓΊΑ** .

    ![Εικονικό δίκτυα κόμβου](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. Στον κόμβο **Υπηρεσίες δικτύου** , επιλέξτε **Εικονικού δικτύου**.

5. Κάντε κλικ στην επιλογή **Γρήγορη δημιουργία** για να δημιουργήσετε ένα εικονικό δίκτυο.

    ![Δημιουργία εικονικού δικτύου - γρήγορη](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Καθορίστε ένα **όνομα** για το εικονικό δίκτυο. Μπορείτε επίσης να ρυθμίσετε τις παραμέτρους του **χώρου διευθύνσεων** ή **Πλήθος μέγιστη Εικονική** για αυτό το δίκτυο. Μπορείτε να αφήσετε τη ρύθμιση **διακομιστή DNS** που έχει οριστεί σε "Κανένα" προς το παρόν. Μπορείτε να ενημερώσετε το διακομιστή DNS για τη ρύθμιση μετά την ενεργοποίηση Azure AD τομέα υπηρεσιών.

7. Βεβαιωθείτε ότι επιλέγετε μια υποστηριζόμενη περιοχή Azure στην αναπτυσσόμενη λίστα **θέση** . Δείτε τη σελίδα [υπηρεσίες Azure ανά περιοχή](https://azure.microsoft.com/regions/#services/) να γνωρίζετε τις Azure περιοχές στις οποίες Azure υπηρεσίες τομέα AD είναι διαθέσιμη.

8. Για να δημιουργήσετε το εικονικό δίκτυο, κάντε κλικ στο κουμπί **Δημιουργία ένα εικονικό δίκτυο** .

    ![Δημιουργήστε ένα εικονικό δίκτυο για τις υπηρεσίες τομέα AD Azure.](./media/active-directory-domain-services-getting-started/create-vnet.png)

9. Αφού δημιουργηθεί το εικονικό δίκτυο, επιλέξτε το εικονικό δίκτυο και κάντε κλικ στην καρτέλα **Ρύθμιση ΠΑΡΑΜΈΤΡΩΝ** .

    ![Δημιουργήστε ένα υποδίκτυο](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)

10. Μεταβείτε στην ενότητα **κενά διαστήματα διεύθυνση εικονικού δικτύου** . Κάντε κλικ στην επιλογή **Προσθήκη υποδικτύου** και καθορίστε ένα δευτερεύον δίκτυο με το όνομα **AaddsSubnet**. Κάντε κλικ στο κουμπί **Αποθήκευση** για να δημιουργήσετε το υποδίκτυο.

    ![Δημιουργήστε ένα υποδίκτυο για τις υπηρεσίες τομέα AD Azure.](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)


<br>

## <a name="task-3---enable-azure-ad-domain-services"></a>Εργασία 3 - ενεργοποίηση υπηρεσιών Azure AD τομέα
Η επόμενη εργασία ρύθμισης παραμέτρων είναι για να [ενεργοποιήσετε τις υπηρεσίες τομέα AD Azure](active-directory-ds-getting-started-enableaadds.md).
