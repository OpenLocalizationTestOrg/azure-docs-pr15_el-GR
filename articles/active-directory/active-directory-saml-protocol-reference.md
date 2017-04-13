<properties
    pageTitle="Αναφορά πρωτόκολλο Azure AD SAML | Microsoft Azure"
    description="Σε αυτό το άρθρο παρέχει μια επισκόπηση των προφίλ Καθολικής σύνδεσης και μία SAML Sign-Out στο Azure Active Directory."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/23/2016"
    ms.author="priyamo"/>


# <a name="how-azure-active-directory-uses-the-saml-protocol"></a>Πώς χρησιμοποιεί το πρωτόκολλο SAML το Azure Active Directory

Πρωτόκολλο χρησιμοποιεί το SAML 2.0 Azure υπηρεσίας καταλόγου Active Directory (Azure AD) για να ενεργοποιήσετε τις εφαρμογές για την παροχή μια μεμονωμένη εμπειρία καθολικής σύνδεσης για τους χρήστες. Τα προφίλ [Καθολικής σύνδεσης](active-directory-single-sign-on-protocol-reference.md) και [μίας Sign-Out](active-directory-single-sign-out-protocol-reference.md) SAML του Azure AD εξηγούν πώς χρησιμοποιούνται στην υπηρεσία παροχής ταυτότητας SAML διεκδικήσεων, πρωτόκολλα και συνδέσεις.

Πρωτόκολλο SAML απαιτεί την υπηρεσία παροχής ταυτότητας (Azure AD) και της υπηρεσίας παροχής (η εφαρμογή) για να ανταλλάσσετε πληροφορίες σχετικά με τον εαυτό τους.

Όταν μια εφαρμογή έχει καταχωρηθεί με το Azure AD, τον προγραμματιστή της εφαρμογής καταγράφει πληροφορίες σχετικά με την Ομοσπονδία Azure AD. Αυτό περιλαμβάνει την **Ανακατεύθυνση URI** και **URI μετα-δεδομένων** της εφαρμογής.

Azure AD χρησιμοποιεί το **URI μετα-δεδομένων** της υπηρεσίας cloud για να ανακτήσετε τον αριθμό-κλειδί υπογραφής και το URI της υπηρεσίας cloud αποσυνδεθείτε. Εάν η εφαρμογή δεν υποστηρίζει ένα URI μετα-δεδομένων, ο προγραμματιστής πρέπει να επικοινωνήσετε με υποστήριξης της Microsoft για την παροχή του URI αποσυνδεθείτε και κλειδί υπογραφής.

Azure Active Directory εκθέτει συγκεκριμένο μισθωτή και κοινές (μισθωτή ανεξάρτητο) μόνο καθολικής σύνδεσης και μία sign-out τελικά σημεία. Αυτές οι διευθύνσεις URL αντιπροσωπεύουν μπορούν να χρησιμοποιηθούν θέσεις--δεν είναι απλώς μια αναγνωριστικά--, ώστε να μπορείτε να μεταβείτε στο τελικό σημείο για να διαβάσετε τα μετα-δεδομένα.

 - Το τελικό σημείο συγκεκριμένο μισθωτή βρίσκεται στην `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  Το <TenantDomainName> αντιπροσωπεύει ένα όνομα τομέα καταχωρημένες ή TenantID GUID από ένα μισθωτή του Azure AD. Για παράδειγμα, τα μετα-δεδομένα ομοσπονδίας του μισθωτή contoso.com είναι στο: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

- Το τελικό σημείο μισθωτή ανεξάρτητο βρίσκεται στην `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. Σε αυτήν τη διεύθυνση τελικού σημείου, **κοινά** εμφανίζεται, αντί για ένα όνομα τομέα μισθωτή ή αναγνωριστικό.

Για πληροφορίες σχετικά με τα έγγραφα Ομοσπονδία μετα-δεδομένων που δημοσιεύει Azure AD, ανατρέξτε στο θέμα [Ομοσπονδία μετα-δεδομένων](active-directory-federation-metadata.md).
