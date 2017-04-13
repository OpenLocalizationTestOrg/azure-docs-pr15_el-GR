<properties
    pageTitle="Azure υπηρεσίες τομέα AD: Ενεργοποίηση συγχρονισμό κωδικού πρόσβασης | Microsoft Azure"
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
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Ενεργοποίηση του συγχρονισμού τον κωδικό πρόσβασης στις υπηρεσίες τομέα AD Azure
Στην προηγούμενη καρτέλα εργασίες, έχετε ενεργοποιήσει τις υπηρεσίες τομέα AD Azure για το μισθωτή του Azure AD. Η επόμενη εργασία είναι η ενεργοποίηση Κατακερματισμοί διαπιστευτηρίων που απαιτούνται για έλεγχο ταυτότητας NTLM και Kerberos για συγχρονισμό με τις υπηρεσίες τομέα AD Azure. Μόλις ρυθμιστεί ο συγχρονισμός διαπιστευτηρίων, οι χρήστες να συνδεθείτε διαχειριζόμενων τομέα χρησιμοποιώντας τα διαπιστευτήριά τους εταιρικούς.

Τα βήματα που απαιτούνται διαφέρουν ανάλογα με το εάν η εταιρεία σας έχει ένα μόνο στο cloud Azure AD μισθωτή ή έχει οριστεί για το συγχρονισμό με χρήση του Azure AD Connect στον κατάλογο εσωτερικής εγκατάστασης.

<br>

> [AZURE.SELECTOR]
- [Μόνο στο cloud Azure AD μισθωτή](active-directory-ds-getting-started-password-sync.md)
- [Συγχρονίσει Azure AD μισθωτή](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Εργασία 5: Ενεργοποίηση του συγχρονισμού τον κωδικό πρόσβασης στις υπηρεσίες τομέα AAD για ένα μόνο στο cloud Azure AD μισθωτή
Azure υπηρεσίες τομέα AD πρέπει διαπιστευτηρίων Κατακερματισμοί σε μορφή που είναι κατάλληλη για έλεγχο ταυτότητας NTLM και Kerberos, για τον έλεγχο ταυτότητας χρηστών στον τομέα διαχειριζόμενων. Εκτός εάν ενεργοποιήσετε AAD υπηρεσίες τομέα για το μισθωτή, Azure AD δεν δημιουργούν ή αποθήκευση διαπιστευτηρίων Κατακερματισμοί με τη μορφή που απαιτούνται για το NTLM ή Kerberos. Για λόγους ασφαλείας εμφανή, Azure AD επίσης δεν αποθηκεύει τα διαπιστευτήρια σε μορφή απλού κειμένου. Επομένως, Azure AD δεν έχει έναν τρόπο για να δημιουργήσετε αυτές τις Κατακερματισμοί διαπιστευτηρίων NTLM ή Kerberos που βασίζονται σε υπάρχοντα διαπιστευτήρια των χρηστών.

> [AZURE.NOTE] Εάν η εταιρεία σας έχει ένα μόνο στο cloud Azure AD μισθωτή, οι χρήστες που πρέπει να χρησιμοποιήσετε τις υπηρεσίες τομέα AD Azure πρέπει να αλλάξετε τους κωδικούς πρόσβασής τους.

Αυτή η διαδικασία αλλαγής κωδικού πρόσβασης έχει ως αποτέλεσμα των διαπιστευτηρίων Κατακερματισμοί που απαιτούνται από υπηρεσίες τομέα AD Azure για έλεγχο ταυτότητας Kerberos και NTLM που θα δημιουργηθούν στον Azure AD. Μπορείτε είτε να λήξει τους κωδικούς πρόσβασης για όλους τους χρήστες στο μισθωτή που πρέπει να χρησιμοποιήσετε τις υπηρεσίες τομέα AD Azure ή να ζητήσετε από αυτούς τους χρήστες για να αλλάξετε τους κωδικούς πρόσβασής τους.


### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>Ενεργοποίηση NTLM και Kerberos γενιάς κατακερματισμός διαπιστευτηρίων για ένα μόνο στο cloud Azure AD μισθωτή
Ακολουθούν οδηγίες που πρέπει να παρέχουν στους τελικούς χρήστες, ώστε να μπορούν να αλλάξουν τους κωδικούς πρόσβασής τους:

1. Μεταβείτε στη σελίδα πίνακα Azure AD πρόσβασης για την εταιρεία σας στο [http://myapps.microsoft.com](http://myapps.microsoft.com).

2. Επιλέξτε την καρτέλα **προφίλ** σε αυτήν τη σελίδα.

3. Κάντε κλικ στο πλακίδιο **Αλλαγή κωδικού πρόσβασης** σε αυτήν τη σελίδα.

    ![Δημιουργήστε ένα εικονικό δίκτυο για τις υπηρεσίες τομέα AD Azure.](./media/active-directory-domain-services-getting-started/user-change-password.png)

    > [AZURE.NOTE] Εάν δεν βλέπετε την επιλογή **Αλλαγή κωδικού πρόσβασης** στη σελίδα του πίνακα της Access, βεβαιωθείτε ότι η εταιρεία σας έχει ρυθμίσει [Διαχείριση κωδικών πρόσβασης στο Azure AD](../active-directory/active-directory-passwords-getting-started.md).

4. Στη σελίδα **Αλλαγή κωδικού πρόσβασης** , πληκτρολογήστε τον υπάρχοντα κωδικό πρόσβασης (παλιά) και, στη συνέχεια, πληκτρολογήστε έναν νέο κωδικό πρόσβασης και επιβεβαιώστε τον. Κάντε κλικ στην επιλογή **Υποβολή**.

    ![Δημιουργήστε ένα εικονικό δίκτυο για τις υπηρεσίες τομέα AD Azure.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Αφού έχετε αλλάξει τον κωδικό πρόσβασής σας, το νέο κωδικό πρόσβασης θα μπορεί να χρησιμοποιηθεί στις υπηρεσίες τομέα AD Azure λίγο. Μετά από μερικά λεπτά (συνήθως, περίπου 20 λεπτά), μπορείτε να εισέλθετε σε υπολογιστές που είναι συνδεδεμένοι με τον τομέα διαχειριζόμενων χρησιμοποιώντας τον κωδικό πρόσβασης που μόλις έχει τροποποιηθεί.

<br>

## <a name="related-content"></a>Σχετικό περιεχόμενο

- [Πώς μπορείτε να ενημερώσετε το δικό σας κωδικό πρόσβασης](../active-directory/active-directory-passwords-update-your-own-password.md)

- [Γρήγορα αποτελέσματα με τη Διαχείριση κωδικού πρόσβασης στην Azure AD](../active-directory/active-directory-passwords-getting-started.md).

- [Ενεργοποίηση του συγχρονισμού τον κωδικό πρόσβασης στις υπηρεσίες τομέα AAD για μια συγχρονισμένη Azure AD μισθωτή](active-directory-ds-getting-started-password-sync-synced-tenant.md)

- [Διαχείριση ενός τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure](active-directory-ds-admin-guide-administer-domain.md)

- [Συμμετοχή σε μια εικονική μηχανή των Windows σε έναν τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure](active-directory-ds-admin-guide-join-windows-vm.md)

- [Συμμετοχή σε μια εικονική μηχανή κόκκινο καπέλο Enterprise Linux σε έναν τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
