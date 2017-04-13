<properties
    pageTitle="Αλλάξτε το Αναγνωριστικό μισθωτή κλειδιού θάλαμο μετά τη μετακίνηση μιας συνδρομής | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να αλλάξετε το Αναγνωριστικό μισθωτή για ένα πλήκτρο θάλαμο μετά τη μετακίνηση μιας συνδρομής σε μια διαφορετική μισθωτή"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Αλλάξετε ένα Αναγνωριστικό μισθωτή κλειδιού θάλαμο μετά τη μετακίνηση μιας συνδρομής
### <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>Ε: μου συνδρομή μετακινήθηκε από μισθωτή A μισθωτή B. Πώς αλλαγή το Αναγνωριστικό μισθωτή για μου υπάρχοντα κλειδιού θάλαμο και ορισμός σωστή ACL για αρχές στο μισθωτή B;

Όταν δημιουργείτε ένα νέο κλειδί θάλαμο σε μια συνδρομή, συνδέεται αυτόματα με το προεπιλεγμένο Αναγνωριστικό μισθωτή Azure Active Directory για αυτήν τη συνδρομή. Όλες οι εγγραφές πολιτική πρόσβασης επίσης συνδέονται με αυτό το αναγνωριστικό μισθωτή. Όταν μετακινείτε τη συνδρομή σας στο Azure από μισθωτή A μισθωτή B, το υπάρχον κλειδιού χώροι φύλαξης είναι δυνατή η πρόσβαση από το αρχών (χρήστες και εφαρμογές) στο μισθωτή B. Για να διορθώσετε αυτό το ζήτημα, πρέπει να:

- Αλλαγή του Αναγνωριστικού μισθωτή που σχετίζονται με όλα υπάρχοντα κλειδιού χώροι φύλαξης σε αυτήν την εγγραφή στο μισθωτή B.
- Καταργήστε όλες τις υπάρχουσες εγγραφές πολιτική πρόσβασης.
- Προσθήκη νέων καταχωρήσεων πολιτική πρόσβασης που συσχετίζονται με μισθωτή B.

Για παράδειγμα, εάν έχετε κλειδιού θάλαμο 'myvault' σε μια συνδρομή που έχει μετακινηθεί από το μισθωτή A μισθωτή B, αυτό το σημείο του πώς μπορείτε να αλλάξετε το Αναγνωριστικό μισθωτή για αυτό κλειδιού θάλαμο και κατάργηση παλιών πολιτικές πρόσβασης.

<pre>
$vaultResourceId = (get-AzureRmKeyVault - VaultName myvault). ResourceId $vault = Get-AzureRmResource – ResourceId $vaultResourceId - ExpandProperties $vault. Properties.TenantId = (Get-AzureRmContext). Tenant.TenantId $vault. Properties.AccessPolicies = @() σύνολο AzureRmResource - ResourceId $vaultResourceId-ιδιότητες $vault. Ιδιότητες
</pre>

Επειδή αυτό θάλαμο ήταν στο μισθωτή A πριν από τη μετακίνηση, την αρχική τιμή του **$vault. Properties.TenantId** είναι A μισθωτή, ενώ **(Get-AzureRmContext). Tenant.TenantId** είναι μισθωτή B.

Τώρα που είναι συσχετισμένη με το Αναγνωριστικό σωστή μισθωτή σας θάλαμο και παλιές εγγραφές πολιτική πρόσβασης έχουν καταργηθεί, ορίστε νέα access καταχωρήσεις πολιτικής με το [Σύνολο AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).

## <a name="next-steps"></a>Επόμενα βήματα

Εάν έχετε ερωτήσεις σχετικά με το Azure κλειδί θάλαμο, επισκεφθείτε τα [Φόρουμ θάλαμο Azure αριθμού-κλειδιού](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
