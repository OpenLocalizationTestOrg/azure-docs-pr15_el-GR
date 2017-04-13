<properties
    pageTitle="Ορίστε τις πολιτικές λήξης κωδικού πρόσβασης στην υπηρεσία καταλόγου Azure Active Directory | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ελέγξετε τις πολιτικές λήξης και να αλλάξετε λήξη κωδικού πρόσβασης χρήστη, είτε μεμονωμένα είτε μαζικά για τους κωδικούς πρόσβασης Azure Active directory"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="set-password-expiration-policies-in-azure-active-directory"></a>Ορισμός κωδικού πρόσβασης τις πολιτικές λήξης στο Azure Active Directory

> [AZURE.IMPORTANT] **Είστε εδώ επειδή αντιμετωπίζετε προβλήματα κατά την είσοδο;** Εάν Ναι, [Μάθετε πώς μπορείτε να αλλάξετε και να επαναφέρετε το δικό σας κωδικό πρόσβασης](active-directory-passwords-update-your-own-password.md).

Ως καθολικός διαχειριστής για μια υπηρεσία cloud της Microsoft, μπορείτε να χρησιμοποιήσετε το Microsoft Azure Active Directory Module για Windows PowerShell για να ρυθμίσετε τους κωδικούς πρόσβασης χρήστη ώστε να μην λήγει. Μπορείτε επίσης να χρησιμοποιήσετε το cmdlet του Windows PowerShell για να καταργήσετε το-δεν λήγει ποτέ ρύθμισης παραμέτρων ή για να δείτε ποιος χρήστης τους κωδικούς πρόσβασης έχουν ρυθμιστεί δεν να λήξει. Σε αυτό το άρθρο παρέχει βοήθεια για τις υπηρεσίες cloud, όπως το Microsoft Intune και το Office 365, που βασίζονται στο Microsoft Azure Active Directory για υπηρεσίες ταυτότητας και καταλόγου.

  > [AZURE.NOTE] Μόνο τους κωδικούς πρόσβασης για τους λογαριασμούς χρηστών που δεν συγχρονίζονται μέσω του συγχρονισμού καταλόγου μπορεί να ρυθμιστεί ώστε να μην λήγει. Για περισσότερες πληροφορίες σχετικά με το συγχρονισμό καταλόγου, ανατρέξτε στη λίστα των θεμάτων στο [Χάρτης συγχρονισμού καταλόγου](https://msdn.microsoft.com/library/azure/hh967642.aspx).

Για να χρησιμοποιήσετε το cmdlet του Windows PowerShell, πρέπει πρώτα να εγκαταστήσετε τους.

## <a name="what-do-you-want-to-do"></a>Τι θέλετε να κάνετε;

- [Πώς μπορώ να ελέγξω την πολιτική λήξης για έναν κωδικό πρόσβασης](#how-to-check-expiration-policy-for-a-password)

- [Ορισμός κωδικού πρόσβασης ώστε να λήγει](#set-a-password-to-expire)

- [Ορισμός κωδικού πρόσβασης ώστε να μην θα λήξει](#set-a-password-to-never-expire)

## <a name="how-to-check-expiration-policy-for-a-password"></a>Πώς μπορώ να ελέγξω την πολιτική λήξης για έναν κωδικό πρόσβασης

1.  Σύνδεση με το Windows PowerShell, χρησιμοποιώντας τα διαπιστευτήρια διαχειριστή της εταιρείας σας.

2.  Κάντε ένα από τα εξής:

    - Για να δείτε εάν ένα μεμονωμένο χρήστη κωδικός πρόσβασης έχει ρυθμιστεί ώστε να μην λήγει ποτέ, εκτελέστε το ακόλουθο cmdlet χρησιμοποιώντας το κύριο όνομα χρήστη (UPN) (για παράδειγμα, aprilr@contoso.onmicrosoft.com) ή το Αναγνωριστικό χρήστη του χρήστη που θέλετε να ελέγξετε:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`

    - Για να δείτε τη ρύθμιση "Κωδικός πρόσβασης δεν λήγει ποτέ" για όλους τους χρήστες, εκτελέστε το ακόλουθο cmdlet:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

## <a name="set-a-password-to-expire"></a>Ορισμός κωδικού πρόσβασης ώστε να λήγει

1.  Σύνδεση με το Windows PowerShell, χρησιμοποιώντας τα διαπιστευτήρια διαχειριστή της εταιρείας σας.

2.  Κάντε ένα από τα εξής:

    - Για να ορίσετε τον κωδικό πρόσβασης ενός χρήστη ώστε να λήγει ο κωδικός πρόσβασης, εκτελέστε το ακόλουθο cmdlet χρησιμοποιώντας το κύριο όνομα χρήστη (UPN) ή το Αναγνωριστικό χρήστη του χρήστη:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`

    - Για να ρυθμίσετε τους κωδικούς πρόσβασης όλων των χρηστών στον οργανισμό ώστε θα λήξουν, χρησιμοποιήστε το ακόλουθο cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

## <a name="set-a-password-to-never-expire"></a>Ορισμός κωδικού πρόσβασης ώστε να μην λήγει ποτέ

1. Σύνδεση με το Windows PowerShell, χρησιμοποιώντας τα διαπιστευτήρια διαχειριστή της εταιρείας σας.

2.  Κάντε ένα από τα εξής:

    - Για να ορίσετε τον κωδικό πρόσβασης ενός χρήστη ώστε να μην λήγει ποτέ, εκτελέστε το ακόλουθο cmdlet χρησιμοποιώντας το κύριο όνομα χρήστη (UPN) ή το Αναγνωριστικό χρήστη του χρήστη:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`

    - Για να ρυθμίσετε τους κωδικούς πρόσβασης όλων των χρηστών σε μια εταιρεία να μην λήγει ποτέ, εκτελέστε το ακόλουθο cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Επόμενα βήματα

* **Είστε εδώ επειδή αντιμετωπίζετε προβλήματα κατά την είσοδο;** Εάν Ναι, [Μάθετε πώς μπορείτε να αλλάξετε και να επαναφέρετε το δικό σας κωδικό πρόσβασης](active-directory-passwords-update-your-own-password.md).
