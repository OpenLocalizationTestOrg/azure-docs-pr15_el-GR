<properties
    pageTitle="Azure υπηρεσίες τομέα AD: Ρυθμίσεις Update DNS για το Azure εικονικό δίκτυο | Microsoft Azure"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---update-dns-settings-for-the-azure-virtual-network"></a>Υπηρεσίες Azure τομέα AD - ρυθμίσεις Update DNS για το Azure εικονικού δικτύου

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Εργασία 4: Ενημέρωση ρυθμίσεων DNS για το Azure εικονικού δικτύου
Στον παραπάνω εργασίες ρύθμισης παραμέτρων, έχετε ενεργοποιήσει με επιτυχία υπηρεσίες τομέα AD Azure για τον κατάλογο. Η επόμενη εργασία είναι για να βεβαιωθείτε ότι οι υπολογιστές το εικονικό δίκτυο μπορούν να συνδεθούν και χρήση αυτών των υπηρεσιών. Ενημερώστε τις ρυθμίσεις διακομιστή DNS για το δίκτυό σας εικονικού ώστε να οδηγεί τις δύο διευθύνσεις IP στην οποία είναι διαθέσιμη στο δίκτυο εικονικού Azure υπηρεσίες τομέα AD.

> [AZURE.NOTE] Σημείωση προς τα κάτω στις διευθύνσεις IP για τις υπηρεσίες τομέα AD Azure εμφανίζεται στην καρτέλα **Ρύθμιση παραμέτρων** του καταλόγου σας, αφού έχετε ενεργοποιήσει τις υπηρεσίες τομέα AD Azure για τον κατάλογο.

Ακολουθήστε τα παρακάτω βήματα ρύθμισης παραμέτρων για να ενημερώσετε τις παραμέτρους του διακομιστή DNS για το εικονικό δίκτυο στο οποίο έχετε ενεργοποιήσει τις υπηρεσίες τομέα AD Azure.

1. Μεταβείτε στην **πύλη του Azure κλασική** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Επιλέξτε τον κόμβο **δίκτυα** στο αριστερό τμήμα του παραθύρου.

    ![Εικονικό δίκτυα κόμβου](./media/active-directory-domain-services-getting-started/virtual-network-select.png)

3. Στην καρτέλα **Εικονικού δίκτυα** , επιλέξτε το εικονικό δίκτυο στο οποίο έχετε ενεργοποιήσει τις υπηρεσίες τομέα AD Azure για να προβάλετε τις ιδιότητές του.

4. Κάντε κλικ στην καρτέλα **Ρύθμιση παραμέτρων** .

    ![Εικονικό δίκτυα κόμβου](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)

5. Στην ενότητα **οι διακομιστές DNS** , πληκτρολογήστε τις διευθύνσεις IP των υπηρεσιών Azure AD τομέα.

6. Βεβαιωθείτε ότι πληκτρολογήσατε τις διευθύνσεις IP που εμφανίζονταν στην ενότητα **Υπηρεσίες τομέα** στην καρτέλα **Ρύθμιση παραμέτρων** του καταλόγου σας.

7. Για να αποθηκεύσετε τις ρυθμίσεις του διακομιστή DNS για αυτό το εικονικό δίκτυο, κάντε κλικ στην επιλογή **Αποθήκευση** από το παράθυρο εργασιών στο κάτω μέρος της σελίδας.

   ![Ενημερώστε τις ρυθμίσεις διακομιστή DNS για το εικονικό δίκτυο.](./media/active-directory-domain-services-getting-started/update-dns.png)

> [AZURE.NOTE] Μετά την ενημέρωση των ρυθμίσεων διακομιστή DNS για το εικονικό δίκτυο, αυτό μπορεί να χρειαστεί κάποιος χρόνος για εικονικές μηχανές του δικτύου για να λάβετε το ενημερωμένο ρύθμισης παραμέτρων DNS. Εάν μια εικονική μηχανή είναι δυνατό να συνδεθούν με τον τομέα, μπορείτε να Εκκαθαρίστε το cache DNS (π.χ. ' ipconfig/flushdns') στον υπολογιστή εικονική. Αυτή η εντολή επιβάλει μια ανανέωση των ρυθμίσεων DNS στον υπολογιστή εικονική.


## <a name="task-5---enable-password-synchronization-to-azure-ad-domain-services"></a>Εργασία 5 - Ενεργοποίηση συγχρονισμό κωδικού πρόσβασης στις υπηρεσίες τομέα AD Azure
Η επόμενη εργασία ρύθμισης παραμέτρων είναι για να [ενεργοποιήσετε το συγχρονισμό κωδικού πρόσβασης σε υπηρεσίες τομέα AD Azure](active-directory-ds-getting-started-password-sync.md).
