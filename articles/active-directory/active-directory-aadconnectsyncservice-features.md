<properties
    pageTitle="Azure AD Connect συγχρονισμού υπηρεσίας δυνατότητες και ρυθμίσεις παραμέτρων | Microsoft Azure"
    description="Περιγράφει τις δυνατότητες πλευρά υπηρεσίας για την υπηρεσία συγχρονισμού Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="andkjell;markvi"/>

# <a name="azure-ad-connect-sync-service-features"></a>Δυνατότητες υπηρεσίας συγχρονισμού του Azure AD Connect

Η δυνατότητα συγχρονισμού του Azure AD Connect έχει δύο στοιχεία:

- Το στοιχείο εσωτερικής εγκατάστασης που ονομάζεται **Συγχρονισμός Azure AD Connect**, που ονομάζεται επίσης **μηχανισμού συγχρονισμού**.
- Η υπηρεσία που βρίσκονται σε Azure AD γνωστό και ως **υπηρεσία συγχρονισμού Azure AD Connect**

Αυτό το θέμα εξηγεί πώς λειτουργούν οι ακόλουθες δυνατότητες της **υπηρεσίας συγχρονισμού Azure AD Connect** και πώς μπορείτε να ρυθμίσετε τους χρησιμοποιώντας το Windows PowerShell.

Αυτές οι ρυθμίσεις ρυθμίζονται από το [Azure Active Directory Module για Windows PowerShell](http://aka.ms/aadposh). Λήψη και εγκατάσταση ξεχωριστά από το Azure AD Connect. Τα cmdlet που τεκμηριώνονται σε αυτό το θέμα έχουν εισαχθεί στο το [2016 Μαρτίου αφήστε (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Εάν δεν έχετε τα cmdlet που τεκμηριώνονται σε αυτό το θέμα ή δεν παράγουν το ίδιο αποτέλεσμα, στη συνέχεια, βεβαιωθείτε ότι μπορείτε να εκτελέσετε την πιο πρόσφατη έκδοση.

Για να δείτε τη ρύθμιση παραμέτρων στον κατάλογό σας Azure AD, εκτελέστε `Get-MsolDirSyncFeatures`.  
![Get-MsolDirSyncFeatures αποτέλεσμα](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Πολλές από αυτές τις ρυθμίσεις μπορεί να αλλάξει μόνο από Azure AD Connect.

Οι ακόλουθες ρυθμίσεις μπορούν να ρυθμιστούν από `Set-MsolDirSyncFeature`:

DirSyncFeature | Σχόλιο
--- | ---
[DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) | Επιτρέπει σε ένα χαρακτηριστικό για να είναι σε καραντίνα όταν είναι διπλότυπο άλλο αντικείμενο και όχι αποτυγχάνει ολόκληρο το αντικείμενο κατά την εξαγωγή.
[EnableSoftMatchOnUpn](#userprincipalname-soft-match) | Επιτρέπει αντικειμένων για να συμμετάσχετε σε userPrincipalName εκτός από την κύρια διεύθυνση SMTP.
[SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) | Επιτρέπει ο μηχανισμός συγχρονισμού για να ενημερώσετε το χαρακτηριστικό userPrincipalName για χρήστες (μη ομόσπονδο) διαχειριζόμενων/με άδεια χρήσης.

Αφού έχετε ενεργοποιήσει μια δυνατότητα, δεν μπορεί να απενεργοποιηθεί ξανά.

>[AZURE.NOTE] Από τις 24 Αυγούστου 2016 η δυνατότητα *υποστηρίζεται χαρακτηριστικό αναπαραγωγή* είναι ενεργοποιημένη από προεπιλογή για νέες Azure AD καταλόγων. Αυτή η δυνατότητα θα επίσης ανάπτυξης και θα ενεργοποιηθεί σε καταλόγους που δημιουργήθηκαν πριν από αυτήν την ημερομηνία. Θα λάβετε μια ειδοποίηση ηλεκτρονικού ταχυδρομείου όταν κατάλογό σας πρόκειται να λάβετε ενεργοποίηση αυτής της δυνατότητας.

Οι ακόλουθες ρυθμίσεις έχουν ρυθμιστεί με Azure AD Connect και δεν μπορεί να τροποποιηθεί από `Set-MsolDirSyncFeature`:

DirSyncFeature | Σχόλιο
--- | ---
DeviceWriteback | [Azure AD Connect: Ενεργοποίηση συσκευή αντιγραφής](active-directory-aadconnect-feature-device-writeback.md)
DirectoryExtensions | [Azure AD Connect συγχρονισμού: επεκτάσεις καταλόγου](active-directory-aadconnectsync-feature-directory-extensions.md)
PasswordSync | [Εφαρμογή συγχρονισμό κωδικού πρόσβασης με το συγχρονισμό Azure AD Connect](active-directory-aadconnectsync-implement-password-synchronization.md)
UnifiedGroupWriteback | [Προεπισκόπηση: Ομάδα αντιγραφής](active-directory-aadconnect-feature-preview.md#group-writeback)
UserWriteback | Δεν υποστηρίζονται αυτήν τη στιγμή.

## <a name="duplicate-attribute-resiliency"></a>Αναπαραγωγή ανοχή χαρακτηριστικό
Αντί να παρουσιάζει σφάλμα προμήθειας αντικείμενα με διπλότυπα UPN / proxyAddresses, το χαρακτηριστικό διπλή είναι "σε καραντίνα" και εκχωρείται ένα προσωρινό τιμή. Όταν επιλυθεί της διένεξης, το προσωρινό UPN έχει αλλάξει στην κατάλληλη τιμή αυτόματα. Αυτή η συμπεριφορά μπορεί να ενεργοποιηθεί για το UPN και proxyAddress ξεχωριστά. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Συγχρονισμός ταυτότητας και ανοχή Διπλότυπο χαρακτηριστικό](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>Ταίριασμα απαλές UserPrincipalName
Όταν αυτή η δυνατότητα είναι ενεργοποιημένη, απαλών Ταίριασμα είναι ενεργοποιημένη για UPN εκτός από την [κύρια διεύθυνση SMTP](https://support.microsoft.com/kb/2641663), η οποία είναι πάντα ενεργοποιημένη. Ταίριασμα απαλών χρησιμοποιείται ώστε να ταιριάζει με υπάρχοντες χρήστες cloud στο Azure AD με χρήστες εσωτερικής εγκατάστασης.

Εάν πρέπει να match εσωτερικής AD λογαριασμούς με υπαρχόντων λογαριασμών που δημιουργήθηκε στο cloud και δεν χρησιμοποιείτε το Exchange Online, τότε αυτή η δυνατότητα είναι χρήσιμη. Σε αυτό το σενάριο, γενικά δεν έχετε κάποιο λόγο για να ορίσετε το χαρακτηριστικό SMTP στο cloud.

Αυτή η δυνατότητα είναι στην από προεπιλογή για που δημιουργήθηκαν πρόσφατα σε καταλόγους Azure AD. Μπορείτε να δείτε εάν αυτή η δυνατότητα είναι ενεργοποιημένη για εσάς, εκτελώντας:  
```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Εάν αυτή η δυνατότητα δεν είναι ενεργοποιημένη για τον κατάλογο Azure AD, στη συνέχεια, μπορείτε να την ενεργοποιήσετε, εκτελώντας:  
```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>Συγχρονισμός userPrincipalName ενημερώσεις
Ιστορικά, ενημερώσεις για το χαρακτηριστικό UserPrincipalName με χρήση της υπηρεσίας συγχρονισμού από εσωτερικής εγκατάστασης έχει αποκλειστεί, εκτός εάν ισχύουν και οι δύο αυτές τις συνθήκες:

- Ο χρήστης θα γίνεται η Διαχείριση (μη ομόσπονδο).
- Ο χρήστης δεν έχει εκχωρηθεί μια άδεια χρήσης.

Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [ονόματα χρηστών στο Office 365, Azure, ή Intune δεν ταιριάζουν με το UPN εσωτερικής εγκατάστασης ή το Αναγνωριστικό εναλλακτική login](https://support.microsoft.com/kb/2523192).

Ενεργοποίηση αυτή η δυνατότητα επιτρέπει ο μηχανισμός συγχρονισμού για να ενημερώσετε το userPrincipalName όταν είναι τροποποιημένο εσωτερικής εγκατάστασης και χρησιμοποιείτε συγχρονισμό κωδικού πρόσβασης. Εάν χρησιμοποιείτε το ομοσπονδίας, αυτή η δυνατότητα δεν υποστηρίζεται.

Αυτή η δυνατότητα είναι στην από προεπιλογή για που δημιουργήθηκαν πρόσφατα σε καταλόγους Azure AD. Μπορείτε να δείτε εάν αυτή η δυνατότητα είναι ενεργοποιημένη για εσάς, εκτελώντας:  
```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Εάν αυτή η δυνατότητα δεν είναι ενεργοποιημένη για τον κατάλογο Azure AD, στη συνέχεια, μπορείτε να την ενεργοποιήσετε, εκτελώντας:  
```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Μετά την ενεργοποίηση αυτής της δυνατότητας, θα παραμείνει υπάρχουσες τιμές userPrincipalName ως-είναι. Στην επόμενη αλλαγή από το userPrincipalName χαρακτηριστικό εσωτερικής εγκατάστασης, ο συγχρονισμός κανονική delta σε χρήστες θα ενημερώσει το UPN.  

## <a name="see-also"></a>Δείτε επίσης

- [Συγχρονισμός Azure AD Connect](active-directory-aadconnectsync-whatis.md)
- [Ενοποίηση του ταυτοτήτων εσωτερικής εγκατάστασης με Azure Active Directory](active-directory-aadconnect.md).
