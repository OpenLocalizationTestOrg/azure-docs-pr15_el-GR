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
Στην προηγούμενη καρτέλα εργασίες, έχετε ενεργοποιήσει τις υπηρεσίες τομέα AD Azure για το μισθωτή του Azure AD. Η επόμενη εργασία είναι η ενεργοποίηση του συγχρονισμού των κωδικών πρόσβασης στις υπηρεσίες τομέα AD Azure. Μόλις ρυθμιστεί συγχρονισμού διαπιστευτηρίων, οι χρήστες να συνδεθείτε διαχειριζόμενων τομέα χρησιμοποιώντας τα διαπιστευτήριά τους εταιρικούς.

Τα βήματα που απαιτούνται διαφέρουν ανάλογα με το εάν η εταιρεία σας έχει ένα μόνο στο cloud Azure AD μισθωτή ή έχει οριστεί για το συγχρονισμό με χρήση του Azure AD Connect στον κατάλογο εσωτερικής εγκατάστασης.

<br>

> [AZURE.SELECTOR]
- [Μόνο στο cloud Azure AD μισθωτή](active-directory-ds-getting-started-password-sync.md)
- [Συγχρονίσει Azure AD μισθωτή](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-synced-azure-ad-tenant"></a>Εργασία 5: Ενεργοποίηση του συγχρονισμού τον κωδικό πρόσβασης στις υπηρεσίες τομέα AAD για μια συγχρονισμένη Azure AD μισθωτή
A συγχρονίσει Azure AD μισθωτή έχει ρυθμιστεί για συγχρονισμό με χρήση του Azure AD Connect κατάλογο εσωτερικής εγκατάστασης της εταιρείας σας. Azure AD Connect δεν συγχρονίζεται NTLM και Kerberos διαπιστευτηρίων Κατακερματισμοί να Azure AD από προεπιλογή. Για να χρησιμοποιήσετε τις υπηρεσίες τομέα AD Azure, πρέπει να ρυθμίσετε τις παραμέτρους Azure AD Connect για να συγχρονίσετε Κατακερματισμοί διαπιστευτηρίων που απαιτούνται για τον έλεγχο ταυτότητας NTLM και Kerberos. Ακολουθήστε τα παρακάτω βήματα ενεργοποίηση του συγχρονισμού στο μισθωτή του Azure AD κατακερματισμών απαιτούμενα διαπιστευτήρια.


### <a name="install-or-update-azure-ad-connect"></a>Εγκατάσταση ή ενημέρωση Azure AD Connect
Εγκαταστήστε την πιο πρόσφατη προτεινόμενα έκδοση του Azure AD Connect σε έναν υπολογιστή συνδεδεμένο τομέα. Εάν έχετε μια υπάρχουσα εμφάνιση της ρύθμισης Azure AD Connect, πρέπει να ενημερώσετε ώστε να χρησιμοποιεί την πιο πρόσφατη έκδοση του Azure AD Connect. Για να αποφύγετε γνωστά θέματα/σφαλμάτων που που ενδέχεται να έχει ήδη επιδιορθωθεί, βεβαιωθείτε ότι χρησιμοποιείτε πάντα την πιο πρόσφατη έκδοση του Azure AD Connect.

**[Σύνδεση λήψης Azure AD](http://www.microsoft.com/download/details.aspx?id=47594)**

Συνιστάται η έκδοση: **1.1.281.0** - δημοσιευτεί σε 7 Σεπτεμβρίου 2016.

  > [AZURE.WARNING] ΠΡΈΠΕΙ να εγκαταστήσετε την πιο πρόσφατη προτεινόμενα έκδοση του Azure AD Connect για να ενεργοποιήσετε τα διαπιστευτήρια παλαιού τύπου τον κωδικό πρόσβασης (απαιτείται για τον έλεγχο ταυτότητας NTLM και Kerberos) για να συγχρονίσετε στο μισθωτή του Azure AD. Αυτή η λειτουργία δεν είναι διαθέσιμη σε παλαιότερες εκδόσεις του Azure AD Connect ή με το εργαλείο DirSync παλαιού τύπου.

Οδηγίες εγκατάστασης για το Azure AD Connect είναι διαθέσιμες στο ακόλουθο άρθρο - [Γρήγορα αποτελέσματα με το Azure AD Connect](../active-directory/active-directory-aadconnect.md)


### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>Ενεργοποίηση του συγχρονισμού NTLM και Kerberos κατακερματισμών διαπιστευτηρίων για Azure AD
Να εκτελέσετε την ακόλουθη δέσμη ενεργειών PowerShell σε κάθε δάσος AD, για να επιβάλετε το συγχρονισμό πλήρους κωδικού πρόσβασης, και ενεργοποίηση όλων των εσωτερικής χρηστών Κατακερματισμοί διαπιστευτηρίων για να συγχρονίσετε στο μισθωτή του Azure AD. Αυτή η δέσμη ενεργειών επιτρέπει την Κατακερματισμοί διαπιστευτηρίων που απαιτείται για τον έλεγχο ταυτότητας NTLM/Kerberos για να συγχρονιστεί με το μισθωτή του Azure AD.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

Ανάλογα με το μέγεθος του καταλόγου σας (αριθμός χρηστών, ομάδων κ.λπ.), διαρκεί συγχρονισμού κατακερματισμών διαπιστευτηρίων για Azure AD. Οι κωδικοί πρόσβασης θα μπορεί να χρησιμοποιηθεί τον τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure αμέσως μετά τα διαπιστευτήρια Κατακερματισμοί έχουν συγχρονιστεί σε Azure AD.


<br>

## <a name="related-content"></a>Σχετικό περιεχόμενο

- [Ενεργοποίηση του συγχρονισμού τον κωδικό πρόσβασης στις υπηρεσίες τομέα AAD για ένα μόνο στο cloud Azure AD καταλόγου](active-directory-ds-getting-started-password-sync.md)

- [Διαχείριση ενός τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure](active-directory-ds-admin-guide-administer-domain.md)

- [Συμμετοχή σε μια εικονική μηχανή των Windows σε έναν τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure](active-directory-ds-admin-guide-join-windows-vm.md)

- [Συμμετοχή σε μια εικονική μηχανή κόκκινο καπέλο Enterprise Linux σε έναν τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
