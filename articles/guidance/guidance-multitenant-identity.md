<properties
   pageTitle="Η Διαχείριση ταυτοτήτων για τις εφαρμογές του Multitenant | Microsoft Azure"
   description="Βέλτιστες πρακτικές για τον έλεγχο ταυτότητας, την εξουσιοδότηση και την διαχείριση στο multitenant εφαρμογές."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="identity-management-for-multitenant-applications-in-microsoft-azure"></a>Η Διαχείριση ταυτοτήτων για multitenant εφαρμογές στο Microsoft Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Αυτήν τη σειρά περιγράφει τις βέλτιστες πρακτικές για multitenancy, όταν χρησιμοποιείτε Azure AD για τον έλεγχο ταυτότητας και ταυτότητα διαχείρισης.

Όταν δημιουργείτε μια εφαρμογή multitenant, μία από τις προκλήσεις πρώτη Διαχείριση ταυτοτήτων χρήστη, επειδή τώρα κάθε χρήστης ανήκει σε ένα μισθωτή. Για παράδειγμα:

- Οι χρήστες, πραγματοποιήστε είσοδο με τα διαπιστευτήριά τους εταιρικός.
- Οι χρήστες θα πρέπει να έχουν πρόσβαση του οργανισμού τους δεδομένων, αλλά όχι τα δεδομένα που ανήκει σε άλλες μισθωτές.
- Μια εταιρεία να εγγραφείτε για την εφαρμογή και, στη συνέχεια, να αναθέσετε ρόλους εφαρμογής στα μέλη της.

Azure Active Directory (Azure AD) περιλαμβάνει ορισμένες δυνατότητες εξαιρετική που υποστηρίζουν όλα αυτά τα σενάρια.

Να συνοδεύει αυτήν τη σειρά άρθρων, που δημιουργήσαμε επίσης μια πλήρης, [να ολοκληρωμένες υλοποίηση] [ tailspin] multitenant μια εφαρμογή του. Τα άρθρα αντανακλούν μας μάθατε σε διαδικασία η δημιουργία της εφαρμογής. Για να ξεκινήσετε με την εφαρμογή, ανατρέξτε στο θέμα [λειτουργεί η εφαρμογή έρευνες](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md).

Ακολουθεί η λίστα με άρθρα σε αυτήν τη σειρά:

- [Εισαγωγή στη Διαχείριση ταυτοτήτων για multitenant εφαρμογές](guidance-multitenant-identity-intro.md)
- [Σχετικά με την εφαρμογή Tailspin έρευνες](guidance-multitenant-identity-tailspin.md)
- [Έλεγχος ταυτότητας με χρήση Azure AD και συνδέστε OpenID](guidance-multitenant-identity-authenticate.md)
- [Εργασία με βάσει αξιώσεων ταυτότητες](guidance-multitenant-identity-claims.md)
- [Εγγραφής και μισθωτή προσθήκης λογαριασμών](guidance-multitenant-identity-signup.md)
- [Ρόλοι της εφαρμογής](guidance-multitenant-identity-app-roles.md)
- [Βάσει ρόλων και πόρων βάσει εξουσιοδότησης](guidance-multitenant-identity-authorize.md)
- [Ασφάλιση μιας API web παρασκηνίου](guidance-multitenant-identity-web-api.md)
- [Προσωρινή αποθήκευση διακριτικά πρόσβασης OAuth2](guidance-multitenant-identity-token-cache.md)
- [Ενοποίηση με έναν πελάτη AD FS για multitenant εφαρμογές στο Azure](guidance-multitenant-identity-adfs.md)
- [Χρήση διεκδίκηση προγράμματος-πελάτη για να λάβετε διακριτικά πρόσβασης από το Azure AD](guidance-multitenant-identity-client-assertion.md)
- [Χρήση αριθμού-κλειδιού θάλαμο για την προστασία απορρήτου εφαρμογής](guidance-multitenant-identity-keyvault.md)

[tailspin]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
