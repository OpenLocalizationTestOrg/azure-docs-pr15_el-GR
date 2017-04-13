

<properties
    pageTitle="Συμμετοχή σε μια προσωπική συσκευή για την εταιρεία σας | Microsoft Azure"
    description="Εξηγεί πώς οι χρήστες να καταχωρήσετε τις προσωπικές συσκευές με Windows 10 με το εταιρικό δίκτυο και παρέχει τα βήματα ανάπτυξης για ένα σενάριο BYOD."
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

# <a name="join-a-personal-device-to-your-organization"></a>Συμμετοχή σε μια προσωπική συσκευή για την εταιρεία σας

## <a name="to-join-a-windows-10-device-to-your-organization"></a>Για να συμμετάσχετε σε μια συσκευή Windows 10 για την εταιρεία σας

1.  Από το μενού **Έναρξη** , επιλέξτε **Ρυθμίσεις**.
2.  Επιλέξτε **Λογαριασμοί**και, στη συνέχεια, κάντε κλικ στο **λογαριασμό σας**.
3.  Κάντε κλικ στην επιλογή **Προσθήκη εταιρικό ή σχολικό λογαριασμό**και, στη συνέχεια, πληκτρολογήστε τον εταιρικό λογαριασμό σας.
4.  Στη σελίδα εισόδου για την εταιρεία σας, πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.
5.  Θα σας ζητηθεί για έλεγχο ταυτότητας πολλών παραγόντων δύσκολο. (Αυτό πρόκληση είναι με δυνατότητα ρύθμισης παραμέτρων από ένα διαχειριστή IT.)
6.  Azure Active Directory (Azure AD) ελέγχει εάν η συσκευή απαιτεί κινητή συσκευή διαχείρισης εγγραφής.
7.  Windows καταχωρεί τη συσκευή στον κατάλογο της εταιρείας στο Azure AD και εγγράφει στη Διαχείριση κινητής συσκευής, εάν είναι απαραίτητο.
8.  Εάν είστε χρήστης διαχειριζόμενων, Windows σας μεταφέρει στην επιφάνεια εργασίας έως την αυτόματη είσοδος.
9.  Εάν είστε χρήστης ομόσπονδη, θα μεταφερθείτε σε μια οθόνη εισόδου των Windows για να εισαγάγετε τα διαπιστευτήριά σας.

## <a name="additional-information"></a>Πρόσθετες πληροφορίες
* [Windows 10 για την εταιρεία: τρόποι για να χρησιμοποιήσετε συσκευές για εργασία](active-directory-azureadjoin-windows10-devices-overview.md)
* [Επέκταση cloud δυνατότητες σε συσκευές με Windows 10 έως συμμετοχή Azure Active Directory](active-directory-azureadjoin-user-upgrade.md)
* [Πραγματοποίηση ελέγχου ταυτότητας ταυτότητες χωρίς κωδικούς πρόσβασης μέσω του Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Μάθετε σχετικά με τη χρήση σενάρια για το Azure AD συμμετοχή](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Σύνδεση συσκευών τομέα με Azure AD για Windows 10 εμπειρίες](active-directory-azureadjoin-devices-group-policy.md)
* [Ρύθμιση του Azure AD συμμετοχή](active-directory-azureadjoin-setup.md)
