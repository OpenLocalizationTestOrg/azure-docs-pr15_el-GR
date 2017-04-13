<properties
    pageTitle="Ρύθμιση συσκευής Windows 10 με το Azure AD από τις ρυθμίσεις | Microsoft Azure"
    description="Εξηγεί πώς οι χρήστες μπορούν να συμμετάσχουν σε να Azure AD μέσα στο μενού ρυθμίσεις."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>Ρύθμιση συσκευής Windows 10 με το Azure AD από τις ρυθμίσεις
Εάν χρησιμοποιείτε ήδη Windows 7 ή Windows 8 και υπολογιστή ή συσκευή έχει αναβαθμιστεί σε Windows 10, μπορείτε να συμμετάσχετε σε Azure Active Directory (Azure AD) μέσα στο μενού ρυθμίσεις.

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a>Για να συμμετάσχετε σε Azure AD από το μενού "Ρυθμίσεις"


1. Από το μενού **Έναρξη** , επιλέξτε το σύμβολο **Ρυθμίσεις** .
2. Από τις **Ρυθμίσεις**, επιλέξτε **σύστημα**->**σχετικά με**->**συμμετοχή Azure AD**.
<center>
![Συμμετοχή σε Azure AD από το μενού ρυθμίσεις](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>

3. Κάντε κλικ στην επιλογή **Continue** στο παράθυρο μηνύματος Azure AD συμμετοχή.
<center>
![Παράθυρο μηνύματος συμμετοχή Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Δώστε τα διαπιστευτήρια εισόδου σας. Αυτή η εμπειρία εισόδου θα περιλαμβάνει όλα τα βήματα που απαιτούνται για να ολοκληρωθεί ο έλεγχος ταυτότητας. Εάν είστε μέλος από μια εξωτερική μισθωτή, ο διαχειριστής θα σας δώσει με την εμπειρία Ομοσπονδία που φιλοξενούνται από την εταιρεία σας.
<center>
![Δώστε τα διαπιστευτήρια εισόδου](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Εάν η εταιρεία σας έχει ρυθμίσει τις παραμέτρους ελέγχου ταυτότητας πολλαπλών παραγόντων Azure για τη συμμετοχή, να Azure AD, δώστε το δεύτερο παραγόντων πριν να συνεχίσετε.
6. Κάντε κλικ στην επιλογή **Αποδοχή** στην οθόνη **Αποδοχή αυτής της συσκευής για διαχείριση** .
7. Θα πρέπει να βλέπετε το μήνυμα "η συσκευή σας τώρα είναι συνδεδεμένος για την εταιρεία σας στο Azure AD".


## <a name="additional-information"></a>Πρόσθετες πληροφορίες
* [Μάθετε σχετικά με τη χρήση σενάρια για το Azure AD συμμετοχή](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Σύνδεση συσκευών τομέα με Azure AD για Windows 10 εμπειρίες](active-directory-azureadjoin-devices-group-policy.md)
* [Ρύθμιση του Azure AD συμμετοχή](active-directory-azureadjoin-setup.md)
* [Πραγματοποίηση ελέγχου ταυτότητας ταυτότητες χωρίς κωδικούς πρόσβασης μέσω του Microsoft Passport](active-directory-azureadjoin-passport.md)
