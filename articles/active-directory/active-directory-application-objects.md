<properties
pageTitle="Εφαρμογή καταλόγου Azure Active Directory και τα αντικείμενα υπηρεσίας κεφάλαιο | Microsoft Azure"
description="Μια συζήτηση σχετικά με τη σχέση μεταξύ των εφαρμογών και υπηρεσιών κεφαλαίου αντικείμενα στο Azure Active Directory"
documentationCenter="dev-center-name"
authors="bryanla"
manager="mbaldwin"
services="active-directory"
editor=""/>

<tags
ms.service="active-directory"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="identity"
ms.date="08/10/2016"
ms.author="bryanla;mbaldwin"/>

# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>Εφαρμογή και υπηρεσία κεφαλαίου αντικείμενα στο Azure Active Directory
Όταν διαβάζετε σχετικά με μια Azure Active Directory (AD) "εφαρμογή", δεν είναι πάντα Απαλοιφή ακριβώς τι που αναφέρεται από το συντάκτη. Ο στόχος αυτού του άρθρου είναι ώστε να είναι πιο σαφείς, ορίζοντας τις έννοιες και μπετόν πτυχές της Azure AD ενσωμάτωση εφαρμογών, με ένα παράδειγμα της καταχώρησης και συγκατάθεση για μια [εφαρμογή πολλών μισθωτή](active-directory-dev-glossary.md#multi-tenant-application).

## <a name="overview"></a>Επισκόπηση
Μια εφαρμογή του Azure AD είναι μεγαλύτερο από απλώς ένα κομμάτι του λογισμικού. Είναι εννοιολογική όρου, που αναφέρονται όχι μόνο σε εφαρμογή λογισμικού, αλλά επίσης την καταχώρησή (ή: ρύθμιση παραμέτρων ταυτότητας) με το Azure AD, που σας επιτρέπει να συμμετάσχετε σε τον έλεγχο ταυτότητας και εξουσιοδότηση "συνομιλίες" κατά το χρόνο εκτέλεσης. Από τον ορισμό, μια εφαρμογή μπορεί να λειτουργήσει σε ένα ρόλο [προγράμματος-πελάτη](active-directory-dev-glossary.md#client-application) (κατανάλωση πόρου), ένα ρόλο [πόρων διακομιστή](active-directory-dev-glossary.md#resource-server) (εκθέσετε API στους υπολογιστές-πελάτες) ή ακόμα και τα δύο. Το πρωτόκολλο συνομιλίας ορίζεται από μια [εκχώρηση άδειας 2.0 OAuth ροής](active-directory-dev-glossary.md#authorization-grant), με ένα στόχο επιτρέπει στον υπολογιστή-πελάτη/πόρο για την access/προστασία δεδομένων ενός πόρου αντίστοιχα. Τώρα, ας εξετάσουμε επιπέδου βαθύτερη και δείτε πώς το μοντέλο εφαρμογής Azure AD αντιπροσωπεύει μια εφαρμογή εσωτερικά. 

## <a name="application-registration"></a>Δήλωση εφαρμογής
Όταν καταχωρείτε μια εφαρμογή στην [πύλη του Azure κλασική][AZURE-Classic-Portal], δημιουργούνται δύο αντικείμενα στο μισθωτή του Azure AD: ένα αντικείμενο εφαρμογής και ένα αντικείμενο κεφαλαίου υπηρεσίας.

#### <a name="application-object"></a>Αντικείμενο εφαρμογής
Μια εφαρμογή του Azure AD είναι *που ορίζονται από το* από τη μία και μόνο αντικείμενο εφαρμογής, το οποίο βρίσκεται στο μισθωτή Azure AD όπου έχει καταχωρηθεί η εφαρμογή, η οποία αναφέρεται ως μισθωτή "Οικία" της εφαρμογής. Το αντικείμενο της εφαρμογής παρέχει πληροφορίες σχετικά με την ταυτότητα για μια εφαρμογή και είναι το πρότυπο από την οποία το αντίστοιχο αντικειμένων κεφαλαίου υπηρεσίας είναι *προέρχεται* για χρήση κατά το χρόνο εκτέλεσης. 

Μπορείτε να θεωρήσετε την εφαρμογή, όπως η *καθολική* αναπαράσταση της εφαρμογής σας (για χρήση σε όλες μισθωτές) και το κεφάλαιο υπηρεσίας ως την *τοπική* αναπαράσταση (για χρήση σε έναν συγκεκριμένο μισθωτή). Η Azure AD Graph [οντότητα εφαρμογής] [ AAD-Graph-App-Entity] ορίζει το σχήμα για ένα αντικείμενο εφαρμογής. Ένα αντικείμενο application περιλαμβάνει, επομένως, μια σχέση 1:1 με την εφαρμογή λογισμικού και 1:*n* σχέση με την αντίστοιχη αντικειμένων κεφαλαίου *n* υπηρεσίας.

#### <a name="service-principal-object"></a>Υπηρεσία κύριου αντικειμένου
Το αντικείμενο κεφαλαίου υπηρεσία καθορίζει την πολιτική και τα δικαιώματα για μια εφαρμογή, παρέχοντας βάση για μια αρχή ασφαλείας για να αντιπροσωπεύει την εφαρμογή κατά την πρόσβαση σε πόρους κατά το χρόνο εκτέλεσης. Η Azure AD Graph [οντότητα ServicePrincipal] [ AAD-Graph-Sp-Entity] ορίζει το σχήμα για ένα αντικείμενο κεφαλαίου υπηρεσίας. 

Ένα αντικείμενο κεφαλαίου υπηρεσίας απαιτείται σε κάθε μισθωτή για τον οποίο μια παρουσία της χρήσης της εφαρμογής πρέπει να αντιστοιχεί, ενεργοποίηση ασφαλή πρόσβαση σε πόρους που ανήκουν σε λογαριασμούς χρηστών από αυτόν το μισθωτή. Μια εφαρμογή μονό μισθωτή θα έχει μόνο μία υπηρεσία κεφάλαιο (στο την αρχική μισθωτή). Πολλαπλή μισθωτή [εφαρμογής Web](active-directory-dev-glossary.md#web-client) θα έχει επίσης αρχής υπηρεσίας σε κάθε μισθωτή όπου διαχειριστής ή χρηστών από αυτόν το μισθωτή έχουν δώσει τη συγκατάθεσή, επιτρέποντας την πρόσβαση στους πόρους τους. Μετά τη συγκατάθεσή, το αντικείμενο κεφαλαίου υπηρεσία θα τη γνώμη για μελλοντική εξουσιοδότησης αιτήσεις. 

> [AZURE.NOTE] Οι αλλαγές που κάνετε στο αντικείμενό σας εφαρμογής, επίσης θα αντανακλώνται στο το αντικείμενο κεφαλαίου υπηρεσίας στο της εφαρμογής οικίας μισθωτή μόνο (ο μισθωτής όπου έχει καταχωρηθεί). Για εφαρμογές πολλών μισθωτών, αλλαγές στο αντικείμενο εφαρμογής δεν αντικατοπτρίζονται στην των μισθωτές του καταναλωτή οποιαδήποτε υπηρεσία κεφαλαίου αντικείμενα, μέχρι ο μισθωτής καταναλωτή καταργεί την πρόσβαση και να εκχωρεί πρόσβαση ξανά.

## <a name="example"></a>Παράδειγμα
Το παρακάτω διάγραμμα παρουσιάζει τη σχέση μεταξύ μιας εφαρμογής αντικείμενο της εφαρμογής και αντιστοιχεί κεφαλαίου αντικείμενα, στο περιβάλλον μιας εφαρμογής πολλών μισθωτών δείγμα που ονομάζεται **HR εφαρμογής**υπηρεσίας. Υπάρχουν τρεις Azure AD μισθωτές σε αυτό το σενάριο: 

- **Adatum** - ο μισθωτής που χρησιμοποιούνται από την εταιρεία που αναπτύχθηκε την **εφαρμογή HR**
- **Το Contoso** - ο μισθωτής που χρησιμοποιούνται από τον οργανισμό Contoso, που είναι το πρόγραμμα-πελάτη της **εφαρμογής HR**
- **Fabrikam** - ο μισθωτής που χρησιμοποιούνται από την οργάνωση Fabrikam, η οποία χρησιμοποιεί επίσης την **εφαρμογή HR**

![Σχέση μεταξύ του αντικειμένου εφαρμογής και ένα αντικείμενο κεφαλαίου υπηρεσίας](./media/active-directory-application-objects/application-objects-relationship.png)

Στο προηγούμενο διάγραμμα, βήμα 1 είναι η διαδικασία δημιουργίας της εφαρμογής και κεφαλαίου αντικειμένων υπηρεσίας στο μισθωτή αρχική της εφαρμογής.

Στο βήμα 2, όταν Contoso και Fabrikam διαχειριστές ολοκλήρωση συγκατάθεση, ένα αντικείμενο κεφαλαίου υπηρεσίας δημιουργείται στο μισθωτή Azure AD της εταιρείας τους και στους οποίους έχουν ανατεθεί τα δικαιώματα που εκχωρηθεί ο διαχειριστής. Επίσης, σημειώστε ότι η εφαρμογή HR θα μπορούσε να είναι έχει ρυθμιστεί/έχει σχεδιαστεί για να επιτρέψετε τη συγκατάθεσή από τους χρήστες για μεμονωμένες χρήση.

Στο βήμα 3, το μισθωτές καταναλωτή της HR εφαρμογής (Contoso και Fabrikam) κάθε έχουν τις δικές τους κεφαλαίου αντικειμένου της υπηρεσίας. Κάθε αναπαριστά δεχθεί τη χρήση τους από μια παρουσία της εφαρμογής κατά το χρόνο εκτέλεσης, διέπεται από τα δικαιώματα από το διαχειριστή της αντίστοιχα.

## <a name="next-steps"></a>Επόμενα βήματα
Αντικείμενο εφαρμογής μιας εφαρμογής είναι δυνατή η πρόσβαση μέσω του API Azure AD Graph, όπως αντιπροσωπεύονται από το OData [οντότητα εφαρμογής][AAD-Graph-App-Entity]

Κύριο αντικείμενο μιας εφαρμογής υπηρεσίας είναι δυνατή η πρόσβαση μέσω του API Azure AD Graph, όπως αντιπροσωπεύονται από το OData [ServicePrincipal οντότητα][AAD-Graph-Sp-Entity]



<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Classic-Portal]: https://manage.windowsazure.com